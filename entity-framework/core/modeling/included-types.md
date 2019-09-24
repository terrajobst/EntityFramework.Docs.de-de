---
title: Einschließen & Ausschließen von Typen EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: ca83b1c432bdf4853dba81e12ec4a739bc8218dc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197378"
---
# <a name="including--excluding-types"></a><span data-ttu-id="55190-102">Einschließen und Ausschließen von Typen</span><span class="sxs-lookup"><span data-stu-id="55190-102">Including & Excluding Types</span></span>

<span data-ttu-id="55190-103">Das Einschließen eines Typs in das Modell bedeutet, dass EF über Metadaten zu diesem Typ verfügt und versucht, Instanzen aus der Datenbank zu lesen und zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="55190-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="55190-104">Konventionen</span><span class="sxs-lookup"><span data-stu-id="55190-104">Conventions</span></span>

<span data-ttu-id="55190-105">Gemäß der Konvention sind Typen, die in `DbSet` Eigenschaften in ihrem Kontext verfügbar gemacht werden, in Ihrem Modell enthalten.</span><span class="sxs-lookup"><span data-stu-id="55190-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="55190-106">Außerdem werden Typen, die in der `OnModelCreating` -Methode erwähnt werden, ebenfalls eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="55190-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="55190-107">Schließlich sind alle Typen, die gefunden werden, indem die Navigations Eigenschaften von ermittelten Typen rekursiv untersucht werden, auch im Modell enthalten.</span><span class="sxs-lookup"><span data-stu-id="55190-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="55190-108">**Im folgenden Code werden z. b. alle drei Typen ermittelt:**</span><span class="sxs-lookup"><span data-stu-id="55190-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="55190-109">`Blog`Da es in einer `DbSet` Eigenschaft im Kontext verfügbar gemacht wird.</span><span class="sxs-lookup"><span data-stu-id="55190-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="55190-110">`Post`Da es über die `Blog.Posts` Navigations Eigenschaft erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="55190-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="55190-111">`AuditEntry`Da es in`OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="55190-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/IncludedTypes.cs?highlight=3,7,16)] -->
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

## <a name="data-annotations"></a><span data-ttu-id="55190-112">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="55190-112">Data Annotations</span></span>

<span data-ttu-id="55190-113">Mithilfe von Daten Anmerkungen können Sie einen Typ aus dem Modell ausschließen.</span><span class="sxs-lookup"><span data-stu-id="55190-113">You can use Data Annotations to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a><span data-ttu-id="55190-114">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="55190-114">Fluent API</span></span>

<span data-ttu-id="55190-115">Mit der fließend-API können Sie einen Typ aus dem Modell ausschließen.</span><span class="sxs-lookup"><span data-stu-id="55190-115">You can use the Fluent API to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
