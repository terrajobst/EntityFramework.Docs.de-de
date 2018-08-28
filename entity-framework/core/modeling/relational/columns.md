---
title: Spaltenzuordnung – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: bca9ca22d211aa58a3bba00f6e4d54b8fe4a0df8
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996203"
---
# <a name="column-mapping"></a><span data-ttu-id="13a65-102">Spaltenzuordnung</span><span class="sxs-lookup"><span data-stu-id="13a65-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="13a65-103">Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="13a65-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="13a65-104">Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="13a65-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="13a65-105">Die spaltenzuordnung gibt die Spaltendaten aus abgefragt und in der Datenbank gespeichert werden sollten.</span><span class="sxs-lookup"><span data-stu-id="13a65-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="13a65-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="13a65-106">Conventions</span></span>

<span data-ttu-id="13a65-107">Gemäß der Konvention wird jede Eigenschaft einer Spalte mit dem gleichen Namen wie die Eigenschaft zuordnen eingerichtet werden.</span><span class="sxs-lookup"><span data-stu-id="13a65-107">By convention, each property will be set up to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="13a65-108">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="13a65-108">Data Annotations</span></span>

<span data-ttu-id="13a65-109">Sie können Datenanmerkungen verwenden, um die Spalte zu konfigurieren, die eine Eigenschaft zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="13a65-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=3)] -->
``` csharp
public class Blog
{
    [Column("blog_id")]
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="13a65-110">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="13a65-110">Fluent API</span></span>

<span data-ttu-id="13a65-111">Sie können die Fluent-API verwenden, um die Spalte zu konfigurieren, die eine Eigenschaft zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="13a65-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/Column.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.BlogId)
            .HasColumnName("blog_id");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
