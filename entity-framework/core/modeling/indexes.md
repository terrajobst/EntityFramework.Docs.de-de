---
title: Indizes – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: 87fe893243377e3ab83d419ae9bedf813ca50c3f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995479"
---
# <a name="indexes"></a><span data-ttu-id="cae34-102">Indizes</span><span class="sxs-lookup"><span data-stu-id="cae34-102">Indexes</span></span>

<span data-ttu-id="cae34-103">Indizes sind ein gängiges Konzept für viele Datenspeicher.</span><span class="sxs-lookup"><span data-stu-id="cae34-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="cae34-104">Obwohl deren Implementierung im Datenspeicher variieren kann, müssen sie dienen zum Suchen auf Grundlage einer Spalte (oder eine Gruppe von Spalten) stärker effizient.</span><span class="sxs-lookup"><span data-stu-id="cae34-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="cae34-105">Konventionen</span><span class="sxs-lookup"><span data-stu-id="cae34-105">Conventions</span></span>

<span data-ttu-id="cae34-106">Gemäß der Konvention ist ein Index erstellt, in jeder Eigenschaft (oder einen Satz von Eigenschaften) an, die als Fremdschlüssel verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="cae34-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="cae34-107">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="cae34-107">Data Annotations</span></span>

<span data-ttu-id="cae34-108">Indizes können nicht mithilfe von datenanmerkungen erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="cae34-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="cae34-109">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="cae34-109">Fluent API</span></span>

<span data-ttu-id="cae34-110">Sie können die Fluent-API verwenden, um einen Index für eine einzelne Eigenschaft angeben.</span><span class="sxs-lookup"><span data-stu-id="cae34-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="cae34-111">Standardmäßig sind die Indizes nicht eindeutig.</span><span class="sxs-lookup"><span data-stu-id="cae34-111">By default, indexes are non-unique.</span></span>

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

<span data-ttu-id="cae34-112">Sie können auch angeben, dass ein Index muss eindeutig sein, was bedeutet, dass keine zwei Entitäten, die gleichen Werte für die angegebenen Eigenschaften verfügen können.</span><span class="sxs-lookup"><span data-stu-id="cae34-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

<span data-ttu-id="cae34-113">Sie können auch einen Index über mehr als eine Spalte angeben.</span><span class="sxs-lookup"><span data-stu-id="cae34-113">You can also specify an index over more than one column.</span></span>

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
> <span data-ttu-id="cae34-114">Es gibt nur einen Index pro Satz von Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="cae34-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="cae34-115">Wenn Sie die Fluent-API verwenden, so konfigurieren Sie einen Index für eine Gruppe von Eigenschaften, die bereits einen Index definiert wird, entweder durch Konventionen oder vorherigen Konfiguration werden dann Sie die Definition des Index ändern.</span><span class="sxs-lookup"><span data-stu-id="cae34-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="cae34-116">Dies ist nützlich, wenn Sie möchten, um einen Index zu konfigurieren, der gemäß der Konvention erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="cae34-116">This is useful if you want to further configure an index that was created by convention.</span></span>
