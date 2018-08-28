---
title: Benutzerdefinierte Migrations History Table - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.openlocfilehash: 7ee76cadd6fac4ec403918e88460e43067ae5815
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995695"
---
<a name="custom-migrations-history-table"></a>Benutzerdefinierte Migrations History Table
===============================
In der Standardeinstellung EF Core verfolgt Überblick darüber, welche Migrationen auf die Datenbank angewendet wurden, indem Sie diese in einer Tabelle mit dem Namen aufzeichnen `__EFMigrationsHistory`. Aus verschiedenen Gründen empfiehlt es sich um diese Tabelle, um Ihre Bedürfnisse anzupassen.

> [!IMPORTANT]
> Wenn Sie anpassen, dass die Migrationen Verlaufstabelle *nach* Migrationen angewendet werden, Sie sind verantwortlich für die vorhandene Tabelle in der Datenbank zu aktualisieren.

<a name="schema-and-table-name"></a>Schema und Tabellenname
----------------------
Sie können ändern, das Schema und Tabellennamen mit dem `MigrationsHistoryTable()` -Methode in der `OnConfiguring()` (oder `ConfigureServices()` in ASP.NET Core). Hier ist ein Beispiel zur Verwendung des SQL Server-EF Core-Anbieters.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a>Andere Änderungen
-------------
Um zusätzliche Aspekte von in der Tabelle zu konfigurieren, außer Kraft setzen, und Ersetzen Sie die anbieterspezifische `IHistoryRepository` Service. Hier ist ein Beispiel zum Ändern der Spaltenname MigrationId *Id* auf SQL Server.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> `SqlServerHistoryRepository` befindet sich in einem internen Namespace und in zukünftigen Versionen möglicherweise geändert.

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
