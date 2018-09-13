---
title: Benutzerdefinierte Migrations History Table - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
ms.openlocfilehash: 1a253972a8f4e410421ec8a77c079e588d368819
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45488815"
---
<a name="custom-migrations-history-table"></a><span data-ttu-id="45435-102">Benutzerdefinierte Migrations History Table</span><span class="sxs-lookup"><span data-stu-id="45435-102">Custom Migrations History Table</span></span>
===============================
<span data-ttu-id="45435-103">In der Standardeinstellung EF Core verfolgt Überblick darüber, welche Migrationen auf die Datenbank angewendet wurden, indem Sie diese in einer Tabelle mit dem Namen aufzeichnen `__EFMigrationsHistory`.</span><span class="sxs-lookup"><span data-stu-id="45435-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="45435-104">Aus verschiedenen Gründen empfiehlt es sich um diese Tabelle, um Ihre Bedürfnisse anzupassen.</span><span class="sxs-lookup"><span data-stu-id="45435-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="45435-105">Wenn Sie anpassen, dass die Migrationen Verlaufstabelle *nach* Migrationen angewendet werden, Sie sind verantwortlich für die vorhandene Tabelle in der Datenbank zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="45435-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

<a name="schema-and-table-name"></a><span data-ttu-id="45435-106">Schema und Tabellenname</span><span class="sxs-lookup"><span data-stu-id="45435-106">Schema and table name</span></span>
----------------------
<span data-ttu-id="45435-107">Sie können ändern, das Schema und Tabellennamen mit dem `MigrationsHistoryTable()` -Methode in der `OnConfiguring()` (oder `ConfigureServices()` in ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="45435-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="45435-108">Hier ist ein Beispiel zur Verwendung des SQL Server-EF Core-Anbieters.</span><span class="sxs-lookup"><span data-stu-id="45435-108">Here is an example using the SQL Server EF Core provider.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a><span data-ttu-id="45435-109">Andere Änderungen</span><span class="sxs-lookup"><span data-stu-id="45435-109">Other changes</span></span>
-------------
<span data-ttu-id="45435-110">Um zusätzliche Aspekte von in der Tabelle zu konfigurieren, außer Kraft setzen, und Ersetzen Sie die anbieterspezifische `IHistoryRepository` Service.</span><span class="sxs-lookup"><span data-stu-id="45435-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="45435-111">Hier ist ein Beispiel zum Ändern der Spaltenname MigrationId *Id* auf SQL Server.</span><span class="sxs-lookup"><span data-stu-id="45435-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> <span data-ttu-id="45435-112">`SqlServerHistoryRepository` befindet sich in einem internen Namespace und in zukünftigen Versionen möglicherweise geändert.</span><span class="sxs-lookup"><span data-stu-id="45435-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

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
