---
title: Tabelle "benutzerdefinierte Migrations Verlauf"-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: 0db393ff3101564f8d8081d0a57b264c2c459df7
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812069"
---
# <a name="custom-migrations-history-table"></a><span data-ttu-id="730b2-102">Benutzerdefinierte Migrations Verlaufs Tabelle</span><span class="sxs-lookup"><span data-stu-id="730b2-102">Custom Migrations History Table</span></span>

<span data-ttu-id="730b2-103">Standardmäßig wird von EF Core nachverfolgt, welche Migrationen auf die Datenbank angewendet wurden, indem Sie in einer Tabelle mit dem Namen `__EFMigrationsHistory`aufgezeichnet wurden.</span><span class="sxs-lookup"><span data-stu-id="730b2-103">By default, EF Core keeps track of which migrations have been applied to the database by recording them in a table named `__EFMigrationsHistory`.</span></span> <span data-ttu-id="730b2-104">Aus verschiedenen Gründen kann es sinnvoll sein, diese Tabelle anzupassen, damit Sie Ihren Anforderungen besser entspricht.</span><span class="sxs-lookup"><span data-stu-id="730b2-104">For various reasons, you may want to customize this table to better suit your needs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="730b2-105">Wenn Sie die Migrations Verlaufs Tabelle *nach* dem Anwenden von Migrationen anpassen, sind Sie dafür verantwortlich, die vorhandene Tabelle in der Datenbank zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="730b2-105">If you customize the Migrations history table *after* applying migrations, you are responsible for updating the existing table in the database.</span></span>

## <a name="schema-and-table-name"></a><span data-ttu-id="730b2-106">Schema-und Tabellenname</span><span class="sxs-lookup"><span data-stu-id="730b2-106">Schema and table name</span></span>

<span data-ttu-id="730b2-107">Sie können das Schema und den Tabellennamen mithilfe der `MigrationsHistoryTable()`-Methode in `OnConfiguring()` (oder `ConfigureServices()` auf ASP.net Core) ändern.</span><span class="sxs-lookup"><span data-stu-id="730b2-107">You can change the schema and table name using the `MigrationsHistoryTable()` method in `OnConfiguring()` (or `ConfigureServices()` on ASP.NET Core).</span></span> <span data-ttu-id="730b2-108">Im folgenden finden Sie ein Beispiel für die Verwendung des SQL Server EF Core Anbieters.</span><span class="sxs-lookup"><span data-stu-id="730b2-108">Here is an example using the SQL Server EF Core provider.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

## <a name="other-changes"></a><span data-ttu-id="730b2-109">Weitere Änderungen</span><span class="sxs-lookup"><span data-stu-id="730b2-109">Other changes</span></span>

<span data-ttu-id="730b2-110">Um zusätzliche Aspekte der Tabelle zu konfigurieren, überschreiben Sie den anbieterspezifischen `IHistoryRepository` Dienst, und ersetzen Sie ihn.</span><span class="sxs-lookup"><span data-stu-id="730b2-110">To configure additional aspects of the table, override and replace the provider-specific `IHistoryRepository` service.</span></span> <span data-ttu-id="730b2-111">Im folgenden finden Sie ein Beispiel für das Ändern des Namens der migrationid-Spalte in " *ID* " in SQL Server.</span><span class="sxs-lookup"><span data-stu-id="730b2-111">Here is an example of changing the MigrationId column name to *Id* on SQL Server.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> <span data-ttu-id="730b2-112">`SqlServerHistoryRepository` befindet sich innerhalb eines internen Namespace und kann sich in zukünftigen Versionen ändern.</span><span class="sxs-lookup"><span data-stu-id="730b2-112">`SqlServerHistoryRepository` is inside an internal namespace and may change in future releases.</span></span>

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
