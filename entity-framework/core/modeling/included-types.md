---
title: Einschließlich & Arten - EF Core ausschließen
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
ms.technology: entity-framework-core
uid: core/modeling/included-types
ms.openlocfilehash: a8d7293a144968d2506bdcc76e55a1a0b1e3fd4b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052600"
---
# <a name="including--excluding-types"></a><span data-ttu-id="3e585-102">Einschließlich & Typen ausschließen</span><span class="sxs-lookup"><span data-stu-id="3e585-102">Including & Excluding Types</span></span>

<span data-ttu-id="3e585-103">Einschließlich eines Typs in das Modell bedeutet, denen EF über Metadaten verfügt, die geben und versucht, lesen und Schreiben von Instanzen von/zu der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="3e585-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="3e585-104">Konventionen</span><span class="sxs-lookup"><span data-stu-id="3e585-104">Conventions</span></span>

<span data-ttu-id="3e585-105">Gemäß der Konvention werden die Typen, die in verfügbar gemacht werden `DbSet` Eigenschaften auf den Kontext in Ihrem Modell enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="3e585-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="3e585-106">Typen, die in erwähnt werden außerdem die `OnModelCreating` -Methode sind ebenfalls enthalten.</span><span class="sxs-lookup"><span data-stu-id="3e585-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="3e585-107">Schließlich sind alle Typen, die durch Untersuchen der Eigenschaften von ermittelten Typen rekursiv gefunden werden ebenfalls im Modell enthalten.</span><span class="sxs-lookup"><span data-stu-id="3e585-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="3e585-108">**Im folgenden Codebeispiel werden z. B. alle drei Typen ermittelt:**</span><span class="sxs-lookup"><span data-stu-id="3e585-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="3e585-109">`Blog`Da es in verfügbar gemacht wurde eine `DbSet` Eigenschaft im Kontext</span><span class="sxs-lookup"><span data-stu-id="3e585-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="3e585-110">`Post`Da es sich bei der Erkennung über die `Blog.Posts` Navigationseigenschaft</span><span class="sxs-lookup"><span data-stu-id="3e585-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="3e585-111">`AuditEntry`Da es in erwähnte`OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="3e585-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/IncludedTypes.cs?highlight=3,7,16)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<AuditEntry>();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}

public class AuditEntry
{
    public int AuditEntryId { get; set; }
    public string Username { get; set; }
    public string Action { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="3e585-112">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="3e585-112">Data Annotations</span></span>

<span data-ttu-id="3e585-113">Sie können Datenanmerkungen verwenden, um einen Typ aus dem Modell auszuschließen.</span><span class="sxs-lookup"><span data-stu-id="3e585-113">You can use Data Annotations to exclude a type from the model.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/IgnoreType.cs?highlight=9)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogMetadata Metadata { get; set; }
}

[NotMapped]
public class BlogMetadata
{
    public DateTime LoadedFromDatabase { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="3e585-114">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="3e585-114">Fluent API</span></span>

<span data-ttu-id="3e585-115">Sie können die Fluent-API verwenden, um einen Typ aus dem Modell auszuschließen.</span><span class="sxs-lookup"><span data-stu-id="3e585-115">You can use the Fluent API to exclude a type from the model.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IgnoreType.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Ignore<BlogMetadata>();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogMetadata Metadata { get; set; }
}

public class BlogMetadata
{
    public DateTime LoadedFromDatabase { get; set; }
}
```
