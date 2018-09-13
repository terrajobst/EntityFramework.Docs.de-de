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
