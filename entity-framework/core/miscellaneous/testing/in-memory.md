---
title: Testen mit InMemory - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: c5c48c575e9fd693d1f28d1a6d10eb83ebbc9d70
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2017
---
# <a name="testing-with-inmemory"></a>Testen mit InMemory

Der Anbieter InMemory ist nützlich, wenn Sie Komponenten mithilfe von etwas, das Herstellen einer Verbindung mit der real-Datenbank, ohne den Aufwand für die tatsächliche Datenbankvorgänge entspricht in etwa testen möchten.

> [!TIP]  
> Sie können anzeigen, dass dieser Artikel [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) auf GitHub.

## <a name="inmemory-is-not-a-relational-database"></a>InMemory ist nicht mit einer relationalen Datenbank

EF Datenbank kernanbieter keine relationalen Datenbanken werden. InMemory dient als allgemeines Datenbank zum Testen und dient nicht zum Simulieren einer relationalen Datenbank.

Beispiele hierfür sind:
* InMemory können Sie zum Speichern von Daten, die Einschränkungen der referenziellen Integrität in einer relationalen Datenbank verletzen würde.

* Wenn Sie DefaultValueSql(string) für eine Eigenschaft im Modell verwenden, dies ist eine relationale Datenbank-API und hat keine Auswirkungen, bei der Ausführung für InMemory.

> [!TIP]  
> Für viele Zwecke testen werden diese Unterschiede spielt keine Rolle. Allerdings gegebenenfalls für die Tests anhand etwas mehr Verhalten hat wie ein "true" relationale Datenbank sollten Sie die Verwendung [SQLite InMemory-Modus](sqlite.md).

## <a name="example-testing-scenario"></a>Beispiel-Testszenario

Betrachten Sie den folgenden Dienst, der Anwendungscode im Zusammenhang mit Blogs Eingriffe ermöglicht. Er verwendet intern eine `DbContext` , die eine Verbindung mit einer SQL Server-Datenbank her. Wäre es nützlich, um diesem Kontext die Verbindung mit einer InMemory-Datenbank, damit wir effiziente Tests für diesen Dienst schreiben, ohne den Code ändern können, oder viele Vorgänge zum Erstellen eines Tests führen Austauschen des Kontexts double.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Bereiten Sie den Kontext

### <a name="avoid-configuring-two-database-providers"></a>Vermeiden Sie die Konfiguration von zwei Datenbankanbieter

In den Tests wirst du extern konfigurieren Sie den Kontext des InMemory-Anbieters verwenden. Wenn Sie einen Datenbankanbieter durch Außerkraftsetzen konfigurieren `OnConfiguring` in den Kontext, dann müssen Sie fügen Sie bedingte Code zum sicherstellen, dass Sie nur den Datenbankanbieter konfigurieren, wenn Sie noch nicht konfiguriert wurde.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> Bei Verwendung von ASP.NET Core sollte dann nicht mit diesem Code erforderlich, da der Datenbankanbieter bereits außerhalb des Kontexts (in "Startup.cs) konfiguriert ist.

### <a name="add-a-constructor-for-testing"></a>Fügen Sie einen Konstruktor zu Testzwecken

Die einfachste Methode zum Aktivieren von Tests mit einer anderen Datenbank so ändern Sie den Kontext, um einen Konstruktor verfügbar machen, das akzeptiert, wird eine `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>`Gibt dem Kontext alle zugehörigen Einstellungen, z. B. welche Datenbank für die Verbindung. Dies ist das gleiche Objekt, das erstellt wird, durch die OnConfiguring-Methode in Ihrem Kontext ausgeführt wird.

## <a name="writing-tests"></a>Schreiben von tests

Der Schlüssel für das Testen von mit diesem Anbieter ist die Fähigkeit, teilen Sie den Kontext zum Verwenden des InMemory-Anbieters und Steuern des Bereichs der Datenbank im Arbeitsspeicher. Sie möchten in der Regel eine fehlerfreie Datenbank für jede Testmethode.

Hier ist ein Beispiel für eine Testklasse, die den InMemory-Datenbank verwendet. Jede Testmethode gibt einen eindeutigen Datenbanknamen an, was bedeutet, dass jede Methode ihre eigene InMemory-Datenbank hat.

>[!TIP]
> Verwenden der `.UseInMemoryDatabase()` Erweiterungsmethode, Verweis das NuGet-Paket `Microsoft.EntityFrameworkCore.InMemory`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
