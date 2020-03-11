---
title: Tabelle "benutzerdefinierte Migrations Verlauf"-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: 0007da7ce3d78fd8f17007ac79a395bb2e6efd86
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414302"
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
