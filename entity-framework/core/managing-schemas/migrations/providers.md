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
<a name="migrations-with-multiple-providers"></a><span data-ttu-id="de5e2-102">Migrationen mit mehreren Anbietern</span><span class="sxs-lookup"><span data-stu-id="de5e2-102">Migrations with Multiple Providers</span></span>
==================================
<span data-ttu-id="de5e2-103">Die [EF Core Tools] [ 1] Gerüst nur Migrationen für den aktiven Anbieter.</span><span class="sxs-lookup"><span data-stu-id="de5e2-103">The [EF Core Tools][1] only scaffold migrations for the active provider.</span></span> <span data-ttu-id="de5e2-104">In einigen Fällen sollten Sie jedoch mehrere Anbieter (z. B. Microsoft SQL Server und SQLite) mit Ihr "DbContext" verwenden.</span><span class="sxs-lookup"><span data-stu-id="de5e2-104">Sometimes, however, you may want to use more than one provider (for example Microsoft SQL Server and SQLite) with your DbContext.</span></span> <span data-ttu-id="de5e2-105">Es gibt zwei Möglichkeiten, dies mit Migrationen behandeln.</span><span class="sxs-lookup"><span data-stu-id="de5e2-105">There are two ways to handle this with Migrations.</span></span> <span data-ttu-id="de5e2-106">Sie können zwei Gruppen verwalten für Migrationen – eine für jeden Anbieter-- oder zusammenführen, die sie in einem einzelnen festgelegt, kann die auf beide.</span><span class="sxs-lookup"><span data-stu-id="de5e2-106">You can maintain two sets of migrations--one for each provider--or merge them into a single set that can work on both.</span></span>

<a name="two-migration-sets"></a><span data-ttu-id="de5e2-107">Zwei Sätze von migration</span><span class="sxs-lookup"><span data-stu-id="de5e2-107">Two migration sets</span></span>
------------------
<span data-ttu-id="de5e2-108">In der ersten Methode generieren Sie zwei livemigrationen für jede modelländerung.</span><span class="sxs-lookup"><span data-stu-id="de5e2-108">In the first approach, you generate two migrations for each model change.</span></span>

<span data-ttu-id="de5e2-109">Eine Möglichkeit, Sie ist jede Migration Satz einzufügenden [in einer separaten Assembly] [ 2] und manuelles wechseln von der aktiven Anbieter (und Migrationen Assembly) zwischen die beiden Migrationen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="de5e2-109">One way to do this is to put each migration set [in a separate assembly][2] and manually switch the active provider (and migrations assembly) between adding the two migrations.</span></span>

<span data-ttu-id="de5e2-110">Ein anderer Ansatz, der erleichtert die Arbeit mit den Tools vereinfachen das Erstellen eines neuen Typs, das Ihr "DbContext" abgeleitet und überschreibt die aktiven Anbieter.</span><span class="sxs-lookup"><span data-stu-id="de5e2-110">Another approach that makes working with the tools easier is to create a new type that derives from your DbContext and overrides the active provider.</span></span> <span data-ttu-id="de5e2-111">Dieser Typ wird verwendet, zur Entwurfszeit Zeit beim Hinzufügen oder Durchführen von Migrationen.</span><span class="sxs-lookup"><span data-stu-id="de5e2-111">This type is used at design time when adding or applying migrations.</span></span>

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> <span data-ttu-id="de5e2-112">Da jede Migration ihre eigenen "DbContext"-Typen verwendet werden, erforderlich nicht bei diesem Ansatz mit einer Assembly separate Migrationen.</span><span class="sxs-lookup"><span data-stu-id="de5e2-112">Since each migration set uses its own DbContext types, this approach doesn't require using a separate migrations assembly.</span></span>

<span data-ttu-id="de5e2-113">Wenn Sie neue Migration hinzufügen, wird Geben Sie die Kontexttypen an.</span><span class="sxs-lookup"><span data-stu-id="de5e2-113">When adding new migration, specify the context types.</span></span>

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> <span data-ttu-id="de5e2-114">Sie müssen nicht das Ausgabeverzeichnis für nachfolgende Migrationen angeben, da sie als gleichgeordnete Elemente bis zum letzten erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="de5e2-114">You don't need to specify the output directory for subsequent migrations since they are created as siblings to the last one.</span></span>

<a name="one-migration-set"></a><span data-ttu-id="de5e2-115">Eine Migration Satz</span><span class="sxs-lookup"><span data-stu-id="de5e2-115">One migration set</span></span>
-----------------
<span data-ttu-id="de5e2-116">Wenn Sie nicht, müssen zwei Sätze von Migrationen möchten, können Sie sie manuell in einem einzigen Satz kombinieren, die auf beide Anbieter angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="de5e2-116">If you don't like having two sets of migrations, you can manually combine them into a single set that can be applied to both providers.</span></span>

<span data-ttu-id="de5e2-117">Anmerkungen können gleichzeitig verwendet werden, da es sich bei ein Anbieter alle Anmerkungen ignoriert, die sie nicht versteht.</span><span class="sxs-lookup"><span data-stu-id="de5e2-117">Annotations can coexist since a provider ignores any annotations that it doesn't understand.</span></span> <span data-ttu-id="de5e2-118">Beispielsweise könnte eine Primärschlüsselspalte, die mit Microsoft SQL Server- und SQLite funktioniert wie folgt aussehen.</span><span class="sxs-lookup"><span data-stu-id="de5e2-118">For example, a primary key column that works with both Microsoft SQL Server and SQLite might look like this.</span></span>

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

<span data-ttu-id="de5e2-119">Wenn Vorgänge nur auf einem Anbieter angewendet werden können (oder sie unterschiedlich zwischen Anbietern sind), verwenden Sie die `ActiveProvider` Eigenschaft mitteilen, welche Anbieter aktiv ist.</span><span class="sxs-lookup"><span data-stu-id="de5e2-119">If operations can only be applied on one provider (or they're differently between providers), use the `ActiveProvider` property to tell which provider is active.</span></span>

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
