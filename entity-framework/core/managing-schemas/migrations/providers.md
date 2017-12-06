---
title: Migrationen mit mehreren Anbietern - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 6b278a5ae270b6a84269dffd72eeca609b168cdd
ms.sourcegitcommit: 3b6159db8a6c0653f13c7b528367b4e69ac3d51e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2017
---
<a name="migrations-with-multiple-providers"></a>Migrationen mit mehreren Anbietern
==================================
Die [EF Core Tools] [ 1] nur das Gerüst für Migrationen für den aktiven Anbieter erstellen. In einigen Fällen sollten Sie jedoch für die Verwendung von mehr als einen Anbieter (z. B. Microsoft SQL Server und SQLite) mit Ihrem DbContext. Es gibt zwei Möglichkeiten, dies mit Migrationen behandeln. Verwalten von zwei Sätze können Sie Migrationen – eine für jeden Anbieter-- oder Merge diese in einem einzelnen festlegen, die auf beiden arbeiten kann.

<a name="two-migration-sets"></a>Zwei Sätze von migration
------------------
In der ersten Methode generieren Sie zwei Migrationen für jedes Modell ändern.

Eine Möglichkeit hierfür dies darin, jeden Satz Migration [in einer separaten Assembly] [ 2] und manuell die aktiven Anbieter (und Migrationen Assembly) zwischen dem Hinzufügen von zwei Migrationen wechseln.

Ein anderer Ansatz vereinfacht die Verwendung mit den Tools einfacher ist, zum Erstellen eines neuen Typs von Ihrem ' DbContext ' abgeleitet und überschreibt den aktiven Anbieter. Dieser Typ dient zur Entwurfszeit Zeit beim Hinzufügen oder Migrationen anwenden.

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> Da jede Migration eigene DbContext-Typen verwendet werden, erforderlich nicht bei diesem Ansatz unter Verwendung einer separaten Migrationen-Assembly.

Wenn neue Migration hinzufügen möchten, geben Sie die Kontexttypen.

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> Sie müssen das Ausgabeverzeichnis für nachfolgende Migrationen angeben, da sie als gleichgeordnete Elemente, das letzte Lesezeichen erstellt werden.

<a name="one-migration-set"></a>Eine Migration festlegen
-----------------
Wenn Sie nicht, müssen zwei Sätze von Migrationen möchten, können Sie diese manuell in einem Einzelsatz kombinieren, die auf beide Anbieter angewendet werden können.

Anmerkungen können zusammen verwendet werden, da ein Anbieter alle Anmerkungen ignoriert, die es nicht verstanden hat. Beispielsweise kann eine Primärschlüsselspalte, die mit Microsoft SQL Server und SQLite funktioniert wie folgt aussehen.

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

Wenn Vorgänge nur auf einen Anbieter angewendet werden können (oder sie unterschiedlich zwischen Anbietern sind), verwenden Sie die `ActiveProvider` Eigenschaft mitteilen, welche Anbieter aktiv ist.

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
