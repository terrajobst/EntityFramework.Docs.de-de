---
title: Tabellen Zuordnung-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
uid: core/modeling/relational/tables
ms.openlocfilehash: 62dce317b901bc862b3c7d20ed1d176805bb24dd
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196966"
---
# <a name="table-mapping"></a><span data-ttu-id="3f6cc-102">Tabellenzuordnung</span><span class="sxs-lookup"><span data-stu-id="3f6cc-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="3f6cc-103">Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="3f6cc-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="3f6cc-104">Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="3f6cc-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="3f6cc-105">Die Tabellen Zuordnung identifiziert, welche Tabellendaten in der Datenbank abgefragt und in der Datenbank gespeichert werden sollen.</span><span class="sxs-lookup"><span data-stu-id="3f6cc-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="3f6cc-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="3f6cc-106">Conventions</span></span>

<span data-ttu-id="3f6cc-107">Gemäß der Konvention ist jede Entität so eingerichtet, dass sie einer Tabelle mit dem gleichen Namen wie die `DbSet<TEntity>`-Eigenschaft zugeordnet ist, die die Entität für den abgeleiteten Kontext verfügbar macht.</span><span class="sxs-lookup"><span data-stu-id="3f6cc-107">By convention, each entity will be set up to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="3f6cc-108">Wenn für `DbSet<TEntity>` die angegebene Entität kein enthalten ist, wird der Klassenname verwendet.</span><span class="sxs-lookup"><span data-stu-id="3f6cc-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="3f6cc-109">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="3f6cc-109">Data Annotations</span></span>

<span data-ttu-id="3f6cc-110">Sie können Daten Anmerkungen verwenden, um die Tabelle zu konfigurieren, der ein Typ zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="3f6cc-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

``` csharp
using System.ComponentModel.DataAnnotations.Schema;
```
``` csharp
[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="3f6cc-111">Sie können auch ein Schema angeben, zu dem die Tabelle gehört.</span><span class="sxs-lookup"><span data-stu-id="3f6cc-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="3f6cc-112">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="3f6cc-112">Fluent API</span></span>

<span data-ttu-id="3f6cc-113">Sie können die fließende API verwenden, um die Tabelle zu konfigurieren, der ein Typ zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="3f6cc-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
```
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .ToTable("blogs");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="3f6cc-114">Sie können auch ein Schema angeben, zu dem die Tabelle gehört.</span><span class="sxs-lookup"><span data-stu-id="3f6cc-114">You can also specify a schema that the table belongs to.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
