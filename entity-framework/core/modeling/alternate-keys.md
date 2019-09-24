---
title: Alternative Schlüssel-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: 87df5d174a1db12fb3ab763ac76c3b863a83087e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197474"
---
# <a name="alternate-keys"></a><span data-ttu-id="ea6f8-102">Alternativschlüssel</span><span class="sxs-lookup"><span data-stu-id="ea6f8-102">Alternate Keys</span></span>

<span data-ttu-id="ea6f8-103">Ein alternativer Schlüssel dient als alternativer eindeutiger Bezeichner für jede Entitäts Instanz zusätzlich zum Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="ea6f8-103">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key.</span></span> <span data-ttu-id="ea6f8-104">Alternative Schlüssel können als Ziel einer Beziehung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="ea6f8-104">Alternate keys can be used as the target of a relationship.</span></span> <span data-ttu-id="ea6f8-105">Bei Verwendung einer relationalen Datenbank wird diese dem Konzept eines eindeutigen Indexes/einer eindeutigen Einschränkung in den alternativen Schlüssel Spalten und einer oder mehrerer Foreign Key-Einschränkungen zugeordnet, die auf die Spalte (n) verweisen.</span><span class="sxs-lookup"><span data-stu-id="ea6f8-105">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]  
> <span data-ttu-id="ea6f8-106">Wenn Sie nur die Eindeutigkeit einer Spalte erzwingen möchten, benötigen Sie einen eindeutigen Index anstelle eines alternativen Schlüssels. Weitere Informationen finden Sie unter [Indizes](indexes.md).</span><span class="sxs-lookup"><span data-stu-id="ea6f8-106">If you just want to enforce uniqueness of a column then you want a unique index rather than an alternate key, see [Indexes](indexes.md).</span></span> <span data-ttu-id="ea6f8-107">In EF bieten alternative Schlüssel mehr Funktionalität als eindeutige Indizes, da Sie als Ziel eines fremd Schlüssels verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="ea6f8-107">In EF, alternate keys provide greater functionality than unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="ea6f8-108">Alternative Schlüssel werden in der Regel bei Bedarf für Sie eingeführt, und Sie müssen Sie nicht manuell konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="ea6f8-108">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="ea6f8-109">Weitere Informationen finden Sie unter [Konventionen](#conventions) .</span><span class="sxs-lookup"><span data-stu-id="ea6f8-109">See [Conventions](#conventions) for more details.</span></span>

## <a name="conventions"></a><span data-ttu-id="ea6f8-110">Konventionen</span><span class="sxs-lookup"><span data-stu-id="ea6f8-110">Conventions</span></span>

<span data-ttu-id="ea6f8-111">Gemäß der Konvention wird ein alternativer Schlüssel für Sie eingeführt, wenn Sie eine Eigenschaft, die nicht der Primärschlüssel ist, als Ziel einer Beziehung identifizieren.</span><span class="sxs-lookup"><span data-stu-id="ea6f8-111">By convention, an alternate key is introduced for you when you identify a property, that is not the primary key, as the target of a relationship.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/AlternateKey.cs?highlight=12)] -->
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

## <a name="data-annotations"></a><span data-ttu-id="ea6f8-112">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="ea6f8-112">Data Annotations</span></span>

<span data-ttu-id="ea6f8-113">Alternative Schlüssel können nicht mithilfe von Daten Anmerkungen konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="ea6f8-113">Alternate keys can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="ea6f8-114">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="ea6f8-114">Fluent API</span></span>

<span data-ttu-id="ea6f8-115">Sie können die fließende API verwenden, um eine einzelne Eigenschaft als alternativen Schlüssel zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="ea6f8-115">You can use the Fluent API to configure a single property to be an alternate key.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?highlight=7,8)] -->
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

<span data-ttu-id="ea6f8-116">Sie können auch die fließende API verwenden, um mehrere Eigenschaften als alternativen Schlüssel (als zusammengesetzten alternativen Schlüssel bezeichnet) zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="ea6f8-116">You can also use the Fluent API to configure multiple properties to be an alternate key (known as a composite alternate key).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?highlight=7,8)] -->
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
