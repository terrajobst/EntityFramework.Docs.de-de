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
# <a name="custom-migrations-history-table"></a>Benutzerdefinierte Migrations Verlaufs Tabelle

Standardmäßig wird von EF Core nachverfolgt, welche Migrationen auf die Datenbank angewendet wurden, indem Sie in einer Tabelle mit dem Namen `__EFMigrationsHistory`aufgezeichnet wurden. Aus verschiedenen Gründen kann es sinnvoll sein, diese Tabelle anzupassen, damit Sie Ihren Anforderungen besser entspricht.

> [!IMPORTANT]
> Wenn Sie die Migrations Verlaufs Tabelle *nach* dem Anwenden von Migrationen anpassen, sind Sie dafür verantwortlich, die vorhandene Tabelle in der Datenbank zu aktualisieren.

## <a name="schema-and-table-name"></a>Schema-und Tabellenname

Sie können das Schema und den Tabellennamen mithilfe der `MigrationsHistoryTable()`-Methode in `OnConfiguring()` (oder `ConfigureServices()` auf ASP.net Core) ändern. Im folgenden finden Sie ein Beispiel für die Verwendung des SQL Server EF Core Anbieters.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options.UseSqlServer(
        connectionString,
        x => x.MigrationsHistoryTable("__MyMigrationsHistory", "mySchema"));
```

## <a name="other-changes"></a>Weitere Änderungen

Um zusätzliche Aspekte der Tabelle zu konfigurieren, überschreiben Sie den anbieterspezifischen `IHistoryRepository` Dienst, und ersetzen Sie ihn. Im folgenden finden Sie ein Beispiel für das Ändern des Namens der migrationid-Spalte in " *ID* " in SQL Server.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IHistoryRepository, MyHistoryRepository>();
```

> [!WARNING]
> `SqlServerHistoryRepository` befindet sich innerhalb eines internen Namespace und kann sich in zukünftigen Versionen ändern.

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
