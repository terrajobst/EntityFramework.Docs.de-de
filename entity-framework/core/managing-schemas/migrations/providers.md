---
title: Migrationen mit mehreren Anbietern – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.openlocfilehash: 7ae695037992323337a780cda29d8c8ed8a13458
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997972"
---
<a name="migrations-with-multiple-providers"></a>Migrationen mit mehreren Anbietern
==================================
Die [EF Core Tools] [ 1] Gerüst nur Migrationen für den aktiven Anbieter. In einigen Fällen sollten Sie jedoch mehrere Anbieter (z. B. Microsoft SQL Server und SQLite) mit Ihr "DbContext" verwenden. Es gibt zwei Möglichkeiten, dies mit Migrationen behandeln. Sie können zwei Gruppen verwalten für Migrationen – eine für jeden Anbieter-- oder zusammenführen, die sie in einem einzelnen festgelegt, kann die auf beide.

<a name="two-migration-sets"></a>Zwei Sätze von migration
------------------
In der ersten Methode generieren Sie zwei livemigrationen für jede modelländerung.

Eine Möglichkeit, Sie ist jede Migration Satz einzufügenden [in einer separaten Assembly] [ 2] und manuelles wechseln von der aktiven Anbieter (und Migrationen Assembly) zwischen die beiden Migrationen hinzufügen.

Ein anderer Ansatz, der erleichtert die Arbeit mit den Tools vereinfachen das Erstellen eines neuen Typs, das Ihr "DbContext" abgeleitet und überschreibt die aktiven Anbieter. Dieser Typ wird verwendet, zur Entwurfszeit Zeit beim Hinzufügen oder Durchführen von Migrationen.

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> Da jede Migration ihre eigenen "DbContext"-Typen verwendet werden, erforderlich nicht bei diesem Ansatz mit einer Assembly separate Migrationen.

Wenn Sie neue Migration hinzufügen, wird Geben Sie die Kontexttypen an.

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> Sie müssen nicht das Ausgabeverzeichnis für nachfolgende Migrationen angeben, da sie als gleichgeordnete Elemente bis zum letzten erstellt werden.

<a name="one-migration-set"></a>Eine Migration Satz
-----------------
Wenn Sie nicht, müssen zwei Sätze von Migrationen möchten, können Sie sie manuell in einem einzigen Satz kombinieren, die auf beide Anbieter angewendet werden können.

Anmerkungen können gleichzeitig verwendet werden, da es sich bei ein Anbieter alle Anmerkungen ignoriert, die sie nicht versteht. Beispielsweise könnte eine Primärschlüsselspalte, die mit Microsoft SQL Server- und SQLite funktioniert wie folgt aussehen.

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

Wenn Vorgänge nur auf einem Anbieter angewendet werden können (oder sie unterschiedlich zwischen Anbietern sind), verwenden Sie die `ActiveProvider` Eigenschaft mitteilen, welche Anbieter aktiv ist.

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
