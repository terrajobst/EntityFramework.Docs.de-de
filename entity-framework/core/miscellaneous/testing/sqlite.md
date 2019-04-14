---
title: Testen mit SQLite - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: e8ff204a09d50064b4f0d4376f02b05c8681ac25
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562532"
---
# <a name="testing-with-sqlite"></a>Testen mit SQLite

SQLite ist einen in-Memory-Modus, der Ihnen ermöglicht, SQLite verwenden, um Tests für eine relationale Datenbank, ohne den Aufwand für die tatsächliche Datenbankvorgänge zu schreiben.

> [!TIP]  
> Sie finden dieses Artikels [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) auf GitHub

## <a name="example-testing-scenario"></a>Beispiel-Szenario

Betrachten Sie den folgenden Dienst, mit dem Anwendungscode zum Durchführen bestimmter Vorgänge im Zusammenhang mit Blogs zu können. Intern verwendet eine `DbContext` , die eine Verbindung mit SQL Server-Datenbank her. Es wäre nützlich, um diesem Kontext für die Verbindung eine SQLite-Datenbank im Arbeitsspeicher, damit wir können effiziente Tests für diesen Dienst schreiben, ohne den Code ändern zu müssen, oder einen Großteil der Arbeit beim Erstellen eines Tests Austauschen des Kontexts double.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Bereiten Sie den Kontext

### <a name="avoid-configuring-two-database-providers"></a>Vermeiden Sie die Konfiguration von zwei Datenbankanbieter

In den Tests wird den Kontext zum Verwenden des InMemory-Anbieters extern konfigurieren. Wenn Sie durch das Überschreiben ein Datenbankanbieters konfigurieren `OnConfiguring` im Kontext Ihrer müssen Sie fügen der bedingten Code, um sicherzustellen, dass Sie nur den Datenbankanbieter konfigurieren, wenn eine nicht bereits konfiguriert wurde.

> [!TIP]  
> Wenn Sie ASP.NET Core verwenden, sollte dann nicht dieser Code erforderlich, da Ihr Datenbankanbieter außerhalb des Kontexts (in "Startup.cs") konfiguriert ist.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>Fügen Sie einen Konstruktor für das Testen

Die einfachste Möglichkeit zum Aktivieren von Tests mit einer anderen Datenbank ist so ändern Sie den Kontext, um einen Konstruktor verfügbar machen, die akzeptiert eine `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` Gibt dem Kontext alle zugehörigen Einstellungen, z. B. welche Datenbank für die Verbindung. Dies ist das gleiche Objekt, das durch Ausführen der OnConfiguring-Methode im Kontext Ihrer basiert.

## <a name="writing-tests"></a>Schreiben von tests

Die Taste, um Tests mit dieser Anbieter ist die Möglichkeit, teilen Sie den Kontext, SQLite, und Steuern des Gültigkeitsbereichs der in-Memory-Datenbank. Der Bereich der Datenbank wird gesteuert, durch Öffnen und schließen die Verbindung. Die Datenbank ist für die Dauer beschränkt, die die Verbindung geöffnet ist. Möchten Sie in der Regel eine fehlerfreie Datenbank, für jede Testmethode.

>[!TIP]
> Verwendung von `SqliteConnection()` und `.UseSqlite()` Erweiterungsmethode Verweis das NuGet-Paket ["Microsoft.entityframeworkcore.SQLite"](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
