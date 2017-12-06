---
title: Benutzerdefinierte Migrationen Verlaufstabelle - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: cb9892241f3d7f1fae6293bd60a8a5c3e7120969
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
<a name="custom-migrations-history-table"></a><span data-ttu-id="5d5b3-102">Benutzerdefinierte Migrationen Verlaufstabelle</span><span class="sxs-lookup"><span data-stu-id="5d5b3-102">Custom Migrations History Table</span></span>
===============================
<span data-ttu-id="5d5b3-103">Standardmäßig EF Core der nachverfolgt welche Migrationen auf die Datenbank angewendet wurden, indem Sie diese in einer Tabelle mit dem Namen aufzeichnen `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="5d5b3-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="5d5b3-104">Aus verschiedenen Gründen können Sie diese Tabelle, um Ihre Bedürfnisse anpassen möchten.</span><span class="sxs-lookup"><span data-stu-id="5d5b3-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5d5b3-105">Wenn Sie die Verlaufstabelle Migrationen anpassen *nach* anwenden Migrationen an, Sie sind verantwortlich für die vorhandene Tabelle in der Datenbank zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="5d5b3-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

<a name="schema-and-table-name"></a><span data-ttu-id="5d5b3-106">Schema und Tabellennamen</span><span class="sxs-lookup"><span data-stu-id="5d5b3-106">Schema and table name</span></span>
----------------------
<span data-ttu-id="5d5b3-107">Sie können ändern, das Schema und die Tabelle mit den `MigrationsHistoryTable()` Methode im `OnConfiguring()` (oder `ConfigureServices()` auf ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="5d5b3-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="5d5b3-108">Hier ist ein Beispiel für die Verwendung des SQL Server EF Core-Anbieters.</span><span class="sxs-lookup"><span data-stu-id="5d5b3-108">Here is an example using the SQL Server EF Core provider.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a><span data-ttu-id="5d5b3-109">Andere Änderungen</span><span class="sxs-lookup"><span data-stu-id="5d5b3-109">Other changes</span></span>
-------------
<span data-ttu-id="5d5b3-110">Um zusätzliche Aspekte der Tabelle zu konfigurieren, außer Kraft setzen, und Ersetzen Sie die anbieterspezifische `IHistoryRepository` Dienst.</span><span class="sxs-lookup"><span data-stu-id="5d5b3-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="5d5b3-111">Hier ist ein Beispiel einer Änderung der Spaltenname MigrationId *Id* auf SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5d5b3-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> <span data-ttu-id="5d5b3-112">`SqlServerHistoryRepository`befindet sich innerhalb eines internen Namespace und in zukünftigen Versionen möglicherweise geändert.</span><span class="sxs-lookup"><span data-stu-id="5d5b3-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

``` csharp
class MyHistoryRepository : SqlServerHistoryRepository
{
    public MyHistoryRepository(HistoryRepositoryDependencies dependencies)
        : base(dependencies)
    {
    }

    protected override void ConfigureTable(EntityTypeBuilder<HistoryRow> history)
    {
        base.ConfigureTable(history);

        history.Property(h => h.MigrationId).HasColumnName("Id");
    }
}
```
