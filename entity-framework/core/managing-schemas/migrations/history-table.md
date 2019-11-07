---
title: Tabelle "benutzerdefinierte Migrations Verlauf"-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: 0007da7ce3d78fd8f17007ac79a395bb2e6efd86
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655705"
---
# <a name="custom-migrations-history-table"></a>Benutzerdefinierte Migrations Verlaufs Tabelle

Standardmäßig wird von EF Core nachverfolgt, welche Migrationen auf die Datenbank angewendet wurden, indem Sie in einer Tabelle mit dem Namen `__EFMigrationsHistory`aufgezeichnet wurden. Aus verschiedenen Gründen kann es sinnvoll sein, diese Tabelle anzupassen, damit Sie Ihren Anforderungen besser entspricht.

> [!IMPORTANT]
> Wenn Sie die Migrations Verlaufs Tabelle *nach* dem Anwenden von Migrationen anpassen, sind Sie dafür verantwortlich, die vorhandene Tabelle in der Datenbank zu aktualisieren.

## <a name="schema-and-table-name"></a>Schema-und Tabellenname

Sie können das Schema und den Tabellennamen mithilfe der `MigrationsHistoryTable()`-Methode in `OnConfiguring()` (oder `ConfigureServices()` auf ASP.net Core) ändern. Im folgenden finden Sie ein Beispiel für die Verwendung des SQL Server EF Core Anbieters.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MigrationTableNameContext.cs#TableNameContext)]

## <a name="other-changes"></a>Weitere Änderungen

Um zusätzliche Aspekte der Tabelle zu konfigurieren, überschreiben Sie den anbieterspezifischen `IHistoryRepository` Dienst, und ersetzen Sie ihn. Im folgenden finden Sie ein Beispiel für das Ändern des Namens der migrationid-Spalte in " *ID* " in SQL Server.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepositoryContext)]

> [!WARNING]
> `SqlServerHistoryRepository` befindet sich innerhalb eines internen Namespace und kann sich in zukünftigen Versionen ändern.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepository)]
