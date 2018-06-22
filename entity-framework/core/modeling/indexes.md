---
title: Indizes - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
ms.technology: entity-framework-core
uid: core/modeling/indexes
ms.openlocfilehash: f57b545d53613cec6887734bf434958ee8fff4d8
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
ms.locfileid: "26054884"
---
# <a name="indexes"></a><span data-ttu-id="436a3-102">Indizes</span><span class="sxs-lookup"><span data-stu-id="436a3-102">Indexes</span></span>

<span data-ttu-id="436a3-103">Indizes sind ein Konzept gemein über viele Datenspeichern.</span><span class="sxs-lookup"><span data-stu-id="436a3-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="436a3-104">Während ihre Implementierung in den Datenspeicher variieren kann, werden sie verwendet, um Suchvorgänge basierend auf einer Spalte (oder eine Gruppe von Spalten) machen effizient.</span><span class="sxs-lookup"><span data-stu-id="436a3-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="436a3-105">Konventionen</span><span class="sxs-lookup"><span data-stu-id="436a3-105">Conventions</span></span>

<span data-ttu-id="436a3-106">Gemäß der Konvention ist ein Index erstellt, in jeder Eigenschaft (oder einen Satz von Eigenschaften) an, die als Fremdschlüssel verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="436a3-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="436a3-107">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="436a3-107">Data Annotations</span></span>

<span data-ttu-id="436a3-108">Indizes können nicht mithilfe von datenanmerkungen erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="436a3-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="436a3-109">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="436a3-109">Fluent API</span></span>

<span data-ttu-id="436a3-110">Die Fluent-API können Sie einen Index für eine einzelne Eigenschaft angeben.</span><span class="sxs-lookup"><span data-stu-id="436a3-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="436a3-111">Standardmäßig werden Indizes nicht eindeutig.</span><span class="sxs-lookup"><span data-stu-id="436a3-111">By default, indexes are non-unique.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Index.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="436a3-112">Sie können auch angeben, dass ein Index eindeutig ist, was bedeutet, dass keine zwei Entitäten die gleichen Werte für den angegebenen Eigenschaft(en) enthalten kann.</span><span class="sxs-lookup"><span data-stu-id="436a3-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

<span data-ttu-id="436a3-113">Sie können auch einen Index über mehr als eine Spalte angeben.</span><span class="sxs-lookup"><span data-stu-id="436a3-113">You can also specify an index over more than one column.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .HasIndex(p => new { p.FirstName, p.LastName });
    }
}

public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
}
```

> [!TIP]  
> <span data-ttu-id="436a3-114">Es ist nur eine Index pro Satz von Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="436a3-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="436a3-115">Wenn Sie die Fluent-API verwenden, so konfigurieren Sie einen Index für eine Gruppe von Eigenschaften, die bereits einen Index definiert, indem entweder Konvention oder vorherige Konfiguration werden dann Sie die Definition dieses Indexes ändern.</span><span class="sxs-lookup"><span data-stu-id="436a3-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="436a3-116">Dies ist nützlich, wenn Sie möchten, um einen Index zu konfigurieren, der gemäß der Konvention erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="436a3-116">This is useful if you want to further configure an index that was created by convention.</span></span>
