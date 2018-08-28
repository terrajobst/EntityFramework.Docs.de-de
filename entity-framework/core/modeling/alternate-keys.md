---
title: Alternative Schlüssel – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: b26d8bc1630af9e811d9c4e7da850a618bc8042e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996970"
---
# <a name="alternate-keys"></a><span data-ttu-id="488b2-102">Alternative Schlüssel</span><span class="sxs-lookup"><span data-stu-id="488b2-102">Alternate Keys</span></span>

<span data-ttu-id="488b2-103">Ein alternativer Schlüssel dient als eines alternativen eindeutigen Bezeichners für jede Entitätsinstanz zusätzlich zu den primären Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="488b2-103">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key.</span></span> <span data-ttu-id="488b2-104">Alternative Schlüssel können als Ziel einer Beziehung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="488b2-104">Alternate keys can be used as the target of a relationship.</span></span> <span data-ttu-id="488b2-105">Bei Verwendung eine relationale Datenbank wird hier das Konzept der eine eindeutige Index/Einschränkung für die alternative Schlüsselspalte(n) und eine oder mehrere foreign Key-Einschränkungen, die auf Spalten verweisen zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="488b2-105">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]  
> <span data-ttu-id="488b2-106">Wenn Sie nur die Eindeutigkeit einer Spalte zu erzwingen, sollten Sie einen eindeutigen Index anstelle eines alternativen Schlüssels möchten, finden Sie unter [Indizes](indexes.md).</span><span class="sxs-lookup"><span data-stu-id="488b2-106">If you just want to enforce uniqueness of a column then you want a unique index rather than an alternate key, see [Indexes](indexes.md).</span></span> <span data-ttu-id="488b2-107">In EF bieten alternative Schlüssel mehr Funktionen als eindeutige Indizes, da sie als Ziel eines Fremdschlüssels verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="488b2-107">In EF, alternate keys provide greater functionality than unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="488b2-108">Alternative Schlüssel werden in der Regel für die Sie bei Bedarf eingeführt, und Sie müssen nicht manuell konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="488b2-108">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="488b2-109">Finden Sie unter [Konventionen](#conventions) Weitere Details.</span><span class="sxs-lookup"><span data-stu-id="488b2-109">See [Conventions](#conventions) for more details.</span></span>

## <a name="conventions"></a><span data-ttu-id="488b2-110">Konventionen</span><span class="sxs-lookup"><span data-stu-id="488b2-110">Conventions</span></span>

<span data-ttu-id="488b2-111">Gemäß der Konvention wird als alternativer Schlüssel für Sie eingeführt, wenn Sie eine Eigenschaft identifizieren, die den Primärschlüssel, nicht als Ziel einer Beziehung ist.</span><span class="sxs-lookup"><span data-stu-id="488b2-111">By convention, an alternate key is introduced for you when you identify a property, that is not the primary key, as the target of a relationship.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/AlternateKey.cs?highlight=12)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .HasForeignKey(p => p.BlogUrl)
            .HasPrincipalKey(b => b.Url);
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

    public string BlogUrl { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="488b2-112">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="488b2-112">Data Annotations</span></span>

<span data-ttu-id="488b2-113">Alternative Schlüssel können nicht mithilfe von Datenanmerkungen konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="488b2-113">Alternate keys can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="488b2-114">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="488b2-114">Fluent API</span></span>

<span data-ttu-id="488b2-115">Sie können die Fluent-API verwenden, so konfigurieren Sie eine einzelne Eigenschaft, um einen alternativen Schlüssel sein.</span><span class="sxs-lookup"><span data-stu-id="488b2-115">You can use the Fluent API to configure a single property to be an alternate key.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeySingle.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => c.LicensePlate);
    }
}

class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```

<span data-ttu-id="488b2-116">Sie können auch die Fluent-API verwenden, so konfigurieren Sie mehrere Eigenschaften um einen alternativen Schlüssel (bekannt als einen zusammengesetzten Schlüssel für die alternative) sein.</span><span class="sxs-lookup"><span data-stu-id="488b2-116">You can also use the Fluent API to configure multiple properties to be an alternate key (known as a composite alternate key).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeyComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => new { c.State, c.LicensePlate });
    }
}

class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```
