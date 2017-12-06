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
<a name="custom-migrations-history-table"></a>Benutzerdefinierte Migrationen Verlaufstabelle
===============================
Standardmäßig EF Core der nachverfolgt welche Migrationen auf die Datenbank angewendet wurden, indem Sie diese in einer Tabelle mit dem Namen aufzeichnen `__EFMigrationsHistory`. Aus verschiedenen Gründen können Sie diese Tabelle, um Ihre Bedürfnisse anpassen möchten.

> [!IMPORTANT]
> Wenn Sie die Verlaufstabelle Migrationen anpassen *nach* anwenden Migrationen an, Sie sind verantwortlich für die vorhandene Tabelle in der Datenbank zu aktualisieren.

<a name="schema-and-table-name"></a>Schema und Tabellennamen
----------------------
Sie können ändern, das Schema und die Tabelle mit den `MigrationsHistoryTable()` Methode im `OnConfiguring()` (oder `ConfigureServices()` auf ASP.NET Core). Hier ist ein Beispiel für die Verwendung des SQL Server EF Core-Anbieters.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

<a name="other-changes"></a>Andere Änderungen
-------------
Um zusätzliche Aspekte der Tabelle zu konfigurieren, außer Kraft setzen, und Ersetzen Sie die anbieterspezifische `IHistoryRepository` Dienst. Hier ist ein Beispiel einer Änderung der Spaltenname MigrationId *Id* auf SQL Server.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> `SqlServerHistoryRepository`befindet sich innerhalb eines internen Namespace und in zukünftigen Versionen möglicherweise geändert.

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
