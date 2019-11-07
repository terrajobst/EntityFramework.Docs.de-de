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
# <a name="custom-migrations-history-table"></a><span data-ttu-id="4100f-102">Benutzerdefinierte Migrations Verlaufs Tabelle</span><span class="sxs-lookup"><span data-stu-id="4100f-102">Custom Migrations History Table</span></span>

<span data-ttu-id="4100f-103">Standardmäßig wird von EF Core nachverfolgt, welche Migrationen auf die Datenbank angewendet wurden, indem Sie in einer Tabelle mit dem Namen `__EFMigrationsHistory`aufgezeichnet wurden.</span><span class="sxs-lookup"><span data-stu-id="4100f-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="4100f-104">Aus verschiedenen Gründen kann es sinnvoll sein, diese Tabelle anzupassen, damit Sie Ihren Anforderungen besser entspricht.</span><span class="sxs-lookup"><span data-stu-id="4100f-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4100f-105">Wenn Sie die Migrations Verlaufs Tabelle *nach* dem Anwenden von Migrationen anpassen, sind Sie dafür verantwortlich, die vorhandene Tabelle in der Datenbank zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="4100f-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

## <a name="schema-and-table-name"></a><span data-ttu-id="4100f-106">Schema-und Tabellenname</span><span class="sxs-lookup"><span data-stu-id="4100f-106">Schema and table name</span></span>

<span data-ttu-id="4100f-107">Sie können das Schema und den Tabellennamen mithilfe der `MigrationsHistoryTable()`-Methode in `OnConfiguring()` (oder `ConfigureServices()` auf ASP.net Core) ändern.</span><span class="sxs-lookup"><span data-stu-id="4100f-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="4100f-108">Im folgenden finden Sie ein Beispiel für die Verwendung des SQL Server EF Core Anbieters.</span><span class="sxs-lookup"><span data-stu-id="4100f-108">Here is an example using the SQL Server EF Core provider.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MigrationTableNameContext.cs#TableNameContext)]

## <a name="other-changes"></a><span data-ttu-id="4100f-109">Weitere Änderungen</span><span class="sxs-lookup"><span data-stu-id="4100f-109">Other changes</span></span>

<span data-ttu-id="4100f-110">Um zusätzliche Aspekte der Tabelle zu konfigurieren, überschreiben Sie den anbieterspezifischen `IHistoryRepository` Dienst, und ersetzen Sie ihn.</span><span class="sxs-lookup"><span data-stu-id="4100f-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="4100f-111">Im folgenden finden Sie ein Beispiel für das Ändern des Namens der migrationid-Spalte in " *ID* " in SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4100f-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepositoryContext)]

> [!WARNING]
> <span data-ttu-id="4100f-112">`SqlServerHistoryRepository` befindet sich innerhalb eines internen Namespace und kann sich in zukünftigen Versionen ändern.</span><span class="sxs-lookup"><span data-stu-id="4100f-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepository)]
