---
title: Testen mit InMemory - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 8aaea52f22954ef6a2b7d9b9c5627597c61ac644
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562545"
---
# <a name="testing-with-inmemory"></a>Testen mit InMemory

Der InMemory-Anbieter ist nützlich, wenn Sie Komponenten verwenden, indem Sie die Verbindung zur echten Datenbank, ohne den Aufwand für die tatsächliche Datenbankvorgänge Tools, testen möchten.

> [!TIP]  
> Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) finden Sie auf GitHub.

## <a name="inmemory-is-not-a-relational-database"></a>InMemory ist nicht mit einer relationalen Datenbank

EF Core-Datenbankanbieter keine relationalen Datenbanken sein. InMemory fungiert als ein allgemeiner-Datenbank zu Testzwecken und dient nicht zum imitieren von einer relationalen Datenbank.

Beispiele hierfür sind:

* InMemory können Sie zum Speichern von Daten, die Einschränkungen der referenziellen Integrität in einer relationalen Datenbank verletzen würde.
* Wenn Sie DefaultValueSql(string) für eine Eigenschaft im Modell verwenden, dies ist eine relationale Datenbank-API und hat keine Auswirkungen, bei der Ausführung für InMemory.
* [Parallelität über Zeitstempel/Zeilenversion](xref:core/modeling/concurrency#timestamprow-version) (`[Timestamp]` oder `IsRowVersion`) wird nicht unterstützt. Keine [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) wird ausgelöst, wenn ein Update erfolgt mit einem alten Concurrency-Token.

> [!TIP]  
> Für viele Zwecke testen werden diese Unterschiede nicht wichtig. Wenn Sie mit dem Sie etwas zu testen, die mehr wie eine "true" relationale Datenbank verhält sich möchten, klicken Sie dann sollten Sie jedoch [SQLite-in-Memory-Modus](sqlite.md).

## <a name="example-testing-scenario"></a>Beispiel-Szenario

Betrachten Sie den folgenden Dienst, mit dem Anwendungscode zum Durchführen bestimmter Vorgänge im Zusammenhang mit Blogs zu können. Intern verwendet eine `DbContext` , die eine Verbindung mit SQL Server-Datenbank her. Es wäre nützlich, um diesem Kontext für die Verbindung eine InMemory-Datenbank, damit wir können effiziente Tests für diesen Dienst schreiben, ohne den Code ändern zu müssen, oder einen Großteil der Arbeit beim Erstellen eines Tests Austauschen des Kontexts double.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Bereiten Sie den Kontext

### <a name="avoid-configuring-two-database-providers"></a>Vermeiden Sie die Konfiguration von zwei Datenbankanbieter

In den Tests wird den Kontext zum Verwenden des InMemory-Anbieters extern konfigurieren. Wenn Sie durch das Überschreiben ein Datenbankanbieters konfigurieren `OnConfiguring` im Kontext Ihrer müssen Sie fügen der bedingten Code, um sicherzustellen, dass Sie nur den Datenbankanbieter konfigurieren, wenn eine nicht bereits konfiguriert wurde.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> Wenn Sie ASP.NET Core verwenden, sollten dann nicht dieser Code erforderlich, da Ihr Datenbankanbieter außerhalb des Kontexts (in "Startup.cs") bereits konfiguriert ist.

### <a name="add-a-constructor-for-testing"></a>Fügen Sie einen Konstruktor für das Testen

Die einfachste Möglichkeit zum Aktivieren von Tests mit einer anderen Datenbank ist so ändern Sie den Kontext, um einen Konstruktor verfügbar machen, die akzeptiert eine `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` Gibt dem Kontext alle zugehörigen Einstellungen, z. B. welche Datenbank für die Verbindung. Dies ist das gleiche Objekt, das durch Ausführen der OnConfiguring-Methode im Kontext Ihrer basiert.

## <a name="writing-tests"></a>Schreiben von tests

Die Taste, um Tests mit dieser Anbieter ist die Möglichkeit, teilen Sie den Kontext den InMemory-Anbieter, und Steuern des Gültigkeitsbereichs der in-Memory-Datenbank. Möchten Sie in der Regel eine fehlerfreie Datenbank, für jede Testmethode.

Hier ist ein Beispiel für eine Testklasse, die der InMemory-Datenbank verwendet. Jede Testmethode gibt einen eindeutigen Datenbanknamen an, was bedeutet, dass jede Methode ihre eigene InMemory-Datenbank verfügt.

>[!TIP]
> Verwenden der `.UseInMemoryDatabase()` Erweiterungsmethode Verweis das NuGet-Paket ["Microsoft.entityframeworkcore.InMemory"](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
