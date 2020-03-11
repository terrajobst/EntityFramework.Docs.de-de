---
title: Testen mit SQLite-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: f7f847d8c766c0d4d7577ea6760ee72a17f84933
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414632"
---
# <a name="testing-with-sqlite"></a>Testen mit SQLite

SQLite verfügt über einen in-Memory-Modus, der Ihnen die Verwendung von SQLite zum Schreiben von Tests für eine relationale Datenbank ohne den Aufwand tatsächlicher Daten Bank Vorgänge ermöglicht.

> [!TIP]  
> Sie können das [Beispiel](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) dieses Artikels auf GitHub anzeigen.

## <a name="example-testing-scenario"></a>Beispiel für ein Testszenario

Beachten Sie den folgenden Dienst, der es Anwendungscode ermöglicht, einige mit Blogs verbundene Vorgänge auszuführen. Intern wird eine `DbContext` verwendet, die eine Verbindung mit einer SQL Server Datenbank herstellt. Es wäre hilfreich, diesen Kontext auszutauschen, um eine Verbindung mit einer in-Memory-SQLite-Datenbank herzustellen, damit wir effiziente Tests für diesen Dienst schreiben können, ohne den Code ändern zu müssen, oder viel Arbeit erledigen, um einen doppelten Test des Kontexts zu erstellen.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Vorbereiten Ihres Kontexts

### <a name="avoid-configuring-two-database-providers"></a>Vermeiden der Konfiguration von zwei Datenbankanbietern

In den Tests werden Sie den Kontext extern so konfigurieren, dass der inMemory-Anbieter verwendet wird. Wenn Sie einen Datenbankanbieter konfigurieren, indem Sie `OnConfiguring` in ihrem Kontext überschreiben, müssen Sie einigen bedingten Code hinzufügen, um sicherzustellen, dass Sie den Datenbankanbieter nur dann konfigurieren, wenn noch keiner konfiguriert wurde.

> [!TIP]  
> Wenn Sie ASP.net Core verwenden, sollte dieser Code nicht benötigt werden, da der Datenbankanbieter außerhalb des Kontexts konfiguriert ist (in Startup.cs).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>Hinzufügen eines Konstruktors zum Testen

Die einfachste Möglichkeit, Tests für eine andere Datenbank zu aktivieren, besteht darin, den Kontext so zu ändern, dass er einen Konstruktor verfügbar macht, der eine `DbContextOptions<TContext>`akzeptiert.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` weist den Kontext alle seine Einstellungen zu, z. b. die Datenbank, mit der eine Verbindung hergestellt werden soll. Dabei handelt es sich um das gleiche Objekt, das erstellt wird, indem die on-Konfigurations Methode in ihrem Kontext ausgeführt wird.

## <a name="writing-tests"></a>Schreiben von Tests

Der Schlüssel zum Testen mit diesem Anbieter ist die Möglichkeit, den Kontext anzuweisen, SQLite zu verwenden, und den Bereich des in-Memory Database zu steuern. Der Bereich der Datenbank wird gesteuert, indem die Verbindung geöffnet und geschlossen wird. Die Datenbank bezieht sich auf die Dauer, in der die Verbindung geöffnet ist. In der Regel benötigen Sie eine saubere Datenbank für jede Testmethode.

>[!TIP]
> Wenn Sie `SqliteConnection()` und die `.UseSqlite()`-Erweiterungsmethode verwenden möchten, verweisen Sie auf das nuget-Paket [Microsoft. entityframeworkcore. sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
