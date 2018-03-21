---
title: Migrationen mit mehreren Anbietern - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/8/2017
ms.technology: entity-framework-core
ms.openlocfilehash: d950e74ed4cef7d4274aabcf3eda7b0b735574c6
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2018
---
<a name="migrations-with-multiple-providers"></a><span data-ttu-id="fc430-102">Migrationen mit mehreren Anbietern</span><span class="sxs-lookup"><span data-stu-id="fc430-102">Migrations with Multiple Providers</span></span>
==================================
<span data-ttu-id="fc430-103">Die [EF Core Tools] [ 1] nur das Gerüst für Migrationen für den aktiven Anbieter erstellen.</span><span class="sxs-lookup"><span data-stu-id="fc430-103">The [EF Core Tools][1] only scaffold migrations for the active provider.</span></span> <span data-ttu-id="fc430-104">In einigen Fällen sollten Sie jedoch für die Verwendung von mehr als einen Anbieter (z. B. Microsoft SQL Server und SQLite) mit Ihrem DbContext.</span><span class="sxs-lookup"><span data-stu-id="fc430-104">Sometimes, however, you may want to use more than one provider (for example Microsoft SQL Server and SQLite) with your DbContext.</span></span> <span data-ttu-id="fc430-105">Es gibt zwei Möglichkeiten, dies mit Migrationen behandeln.</span><span class="sxs-lookup"><span data-stu-id="fc430-105">There are two ways to handle this with Migrations.</span></span> <span data-ttu-id="fc430-106">Verwalten von zwei Sätze können Sie Migrationen – eine für jeden Anbieter-- oder Merge diese in einem einzelnen festlegen, die auf beiden arbeiten kann.</span><span class="sxs-lookup"><span data-stu-id="fc430-106">You can maintain two sets of migrations--one for each provider--or merge them into a single set that can work on both.</span></span>

<a name="two-migration-sets"></a><span data-ttu-id="fc430-107">Zwei Sätze von migration</span><span class="sxs-lookup"><span data-stu-id="fc430-107">Two migration sets</span></span>
------------------
<span data-ttu-id="fc430-108">In der ersten Methode generieren Sie zwei Migrationen für jedes Modell ändern.</span><span class="sxs-lookup"><span data-stu-id="fc430-108">In the first approach, you generate two migrations for each model change.</span></span>

<span data-ttu-id="fc430-109">Eine Möglichkeit hierfür dies darin, jeden Satz Migration [in einer separaten Assembly] [ 2] und manuell die aktiven Anbieter (und Migrationen Assembly) zwischen dem Hinzufügen von zwei Migrationen wechseln.</span><span class="sxs-lookup"><span data-stu-id="fc430-109">One way to do this is to put each migration set [in a separate assembly][2] and manually switch the active provider (and migrations assembly) between adding the two migrations.</span></span>

<span data-ttu-id="fc430-110">Ein anderer Ansatz, die vereinfacht die Verwendung mit den Tools vereinfachen das Erstellen eines neuen Typs, das von Ihrem ' DbContext ' abgeleitet und überschreibt den aktiven Anbieter.</span><span class="sxs-lookup"><span data-stu-id="fc430-110">Another approach that makes working with the tools easier is to create a new type that derives from your DbContext and overrides the active provider.</span></span> <span data-ttu-id="fc430-111">Dieser Typ dient zur Entwurfszeit Zeit beim Hinzufügen oder Migrationen anwenden.</span><span class="sxs-lookup"><span data-stu-id="fc430-111">This type is used at design time when adding or applying migrations.</span></span>

``` csharp
class MySqliteDbContext : MyDbContext
{
    protected override void OnConfiguring(DbContextOptionsBuilder options)
        => options.UseSqlite("Data Source=my.db");
}
```

> [!NOTE]
> <span data-ttu-id="fc430-112">Da jede Migration eigene DbContext-Typen verwendet werden, erforderlich nicht bei diesem Ansatz unter Verwendung einer separaten Migrationen-Assembly.</span><span class="sxs-lookup"><span data-stu-id="fc430-112">Since each migration set uses its own DbContext types, this approach doesn't require using a separate migrations assembly.</span></span>

<span data-ttu-id="fc430-113">Wenn neue Migration hinzufügen möchten, geben Sie die Kontexttypen.</span><span class="sxs-lookup"><span data-stu-id="fc430-113">When adding new migration, specify the context types.</span></span>

``` powershell
Add-Migration InitialCreate -Context MyDbContext -OutputDir Migrations\SqlServerMigrations
Add-Migration InitialCreate -Context MySqliteDbContext -OutputDir Migrations\SqliteMigrations
```
``` Console
dotnet ef migrations add InitialCreate --context MyDbContext --output-dir Migrations/SqlServerMigrations
dotnet ef migrations add InitialCreate --context MySqliteDbContext --output-dir Migrations/SqliteMigrations
```

> [!TIP]
> <span data-ttu-id="fc430-114">Sie müssen das Ausgabeverzeichnis für nachfolgende Migrationen angeben, da sie als gleichgeordnete Elemente, das letzte Lesezeichen erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="fc430-114">You don't need to specify the output directory for subsequent migrations since they are created as siblings to the last one.</span></span>

<a name="one-migration-set"></a><span data-ttu-id="fc430-115">Eine Migration festlegen</span><span class="sxs-lookup"><span data-stu-id="fc430-115">One migration set</span></span>
-----------------
<span data-ttu-id="fc430-116">Wenn Sie nicht, müssen zwei Sätze von Migrationen möchten, können Sie diese manuell in einem Einzelsatz kombinieren, die auf beide Anbieter angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="fc430-116">If you don't like having two sets of migrations, you can manually combine them into a single set that can be applied to both providers.</span></span>

<span data-ttu-id="fc430-117">Anmerkungen können zusammen verwendet werden, da ein Anbieter alle Anmerkungen ignoriert, die es nicht verstanden hat.</span><span class="sxs-lookup"><span data-stu-id="fc430-117">Annotations can coexist since a provider ignores any annotations that it doesn't understand.</span></span> <span data-ttu-id="fc430-118">Beispielsweise kann eine Primärschlüsselspalte, die mit Microsoft SQL Server und SQLite funktioniert wie folgt aussehen.</span><span class="sxs-lookup"><span data-stu-id="fc430-118">For example, a primary key column that works with both Microsoft SQL Server and SQLite might look like this.</span></span>

``` csharp
Id = table.Column<int>(nullable: false)
    .Annotation("SqlServer:ValueGenerationStrategy",
        SqlServerValueGenerationStrategy.IdentityColumn)
    .Annotation("Sqlite:Autoincrement", true),
```

<span data-ttu-id="fc430-119">Wenn Vorgänge nur auf einen Anbieter angewendet werden können (oder sie unterschiedlich zwischen Anbietern sind), verwenden Sie die `ActiveProvider` Eigenschaft mitteilen, welche Anbieter aktiv ist.</span><span class="sxs-lookup"><span data-stu-id="fc430-119">If operations can only be applied on one provider (or they're differently between providers), use the `ActiveProvider` property to tell which provider is active.</span></span>

``` csharp
if (migrationBuilder.ActiveProvider == "Microsoft.EntityFrameworkCore.SqlServer")
{
    migrationBuilder.CreateSequence(
        name: "EntityFrameworkHiLoSequence");
}
```


  [1]: ../../miscellaneous/cli/index.md
  [2]: projects.md
