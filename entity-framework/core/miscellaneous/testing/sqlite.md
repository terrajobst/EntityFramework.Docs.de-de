---
title: "Testen mit SQLite – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: 8fcb4b1a37da6f52219ebe3c672160627fe28fab
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="testing-with-sqlite"></a>Testen mit SQLite

SQLite ist einen in-Memory-Modus, der Ihnen ermöglicht, SQLite verwenden, um Tests für eine relationale Datenbank, ohne den Aufwand für die tatsächliche Datenbankvorgänge zu schreiben.

> [!TIP]  
> Sie können anzeigen, dass dieser Artikel [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) auf GitHub

## <a name="example-testing-scenario"></a>Beispiel-Testszenario

Betrachten Sie den folgenden Dienst, der Anwendungscode im Zusammenhang mit Blogs Eingriffe ermöglicht. Er verwendet intern eine `DbContext` , die eine Verbindung mit einer SQL Server-Datenbank her. Es wäre hilfreich zum Austauschen von diesem Kontext für die Verbindung mit einer in-Memory-SQLite-Datenbank, damit wir effiziente Tests für diesen Dienst schreiben, ohne den Code ändern, oder führen Sie einen Großteil der Arbeit, die zum Erstellen eines Tests können doppelte des Kontexts.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Bereiten Sie den Kontext

### <a name="avoid-configuring-two-database-providers"></a>Vermeiden Sie die Konfiguration von zwei Datenbankanbieter

In den Tests wirst du extern konfigurieren Sie den Kontext des InMemory-Anbieters verwenden. Wenn Sie einen Datenbankanbieter durch Außerkraftsetzen konfigurieren `OnConfiguring` in den Kontext, dann müssen Sie fügen Sie bedingte Code zum sicherstellen, dass Sie nur den Datenbankanbieter konfigurieren, wenn Sie noch nicht konfiguriert wurde.

> [!TIP]  
> Bei Verwendung von ASP.NET Core sollte dann nicht mit diesem Code erforderlich, da der Datenbankanbieter außerhalb des Kontexts (in "Startup.cs) konfiguriert ist.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>Fügen Sie einen Konstruktor zu Testzwecken

Die einfachste Methode zum Aktivieren von Tests mit einer anderen Datenbank so ändern Sie den Kontext, um einen Konstruktor verfügbar machen, das akzeptiert, wird eine `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>`Gibt dem Kontext alle zugehörigen Einstellungen, z. B. welche Datenbank für die Verbindung. Dies ist das gleiche Objekt, das erstellt wird, durch die OnConfiguring-Methode in Ihrem Kontext ausgeführt wird.

## <a name="writing-tests"></a>Schreiben von tests

Der Schlüssel für das Testen von mit diesem Anbieter ist die Fähigkeit, teilen Sie den Kontext SQLite verwenden und Steuern des Gültigkeitsbereichs der Datenbank im Arbeitsspeicher. Der Bereich der Datenbank wird gesteuert, durch Öffnen und schließen die Verbindung. Die Datenbank ist auf die Dauer beschränkt, die die Verbindung geöffnet ist. Sie möchten in der Regel eine fehlerfreie Datenbank für jede Testmethode.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
