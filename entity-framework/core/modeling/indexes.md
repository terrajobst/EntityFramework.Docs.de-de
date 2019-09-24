---
title: Indizes-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: b6f11401b69bd8e8795f6b22e5392ba16fc9ba2e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197243"
---
# <a name="indexes"></a><span data-ttu-id="92aa4-102">Indizes</span><span class="sxs-lookup"><span data-stu-id="92aa4-102">Indexes</span></span>

<span data-ttu-id="92aa4-103">Indizes sind ein gängiges Konzept in vielen Daten speichern.</span><span class="sxs-lookup"><span data-stu-id="92aa4-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="92aa4-104">Während Ihre Implementierung im Datenspeicher variieren kann, werden Sie verwendet, um Suchvorgänge auf Grundlage einer Spalte (oder einer Gruppe von Spalten) effizienter zu gestalten.</span><span class="sxs-lookup"><span data-stu-id="92aa4-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="92aa4-105">Konventionen</span><span class="sxs-lookup"><span data-stu-id="92aa4-105">Conventions</span></span>

<span data-ttu-id="92aa4-106">Gemäß der Konvention wird ein Index in jeder Eigenschaft (oder einem Satz von Eigenschaften) erstellt, die als Fremdschlüssel verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="92aa4-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="92aa4-107">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="92aa4-107">Data Annotations</span></span>

<span data-ttu-id="92aa4-108">Indizes können nicht mithilfe von Daten Anmerkungen erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="92aa4-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="92aa4-109">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="92aa4-109">Fluent API</span></span>

<span data-ttu-id="92aa4-110">Sie können die fließende API verwenden, um einen Index für eine einzelne Eigenschaft anzugeben.</span><span class="sxs-lookup"><span data-stu-id="92aa4-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="92aa4-111">Standardmäßig sind Indizes nicht eindeutig.</span><span class="sxs-lookup"><span data-stu-id="92aa4-111">By default, indexes are non-unique.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Index.cs?highlight=7,8)] -->
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

<span data-ttu-id="92aa4-112">Sie können auch angeben, dass ein Index eindeutig sein soll. Dies bedeutet, dass keine zwei Entitäten für die angegebene Eigenschaft (n) die gleichen Werte aufweisen dürfen.</span><span class="sxs-lookup"><span data-stu-id="92aa4-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

<span data-ttu-id="92aa4-113">Sie können einen Index auch über mehr als eine Spalte angeben.</span><span class="sxs-lookup"><span data-stu-id="92aa4-113">You can also specify an index over more than one column.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/IndexComposite.cs?highlight=7,8)] -->
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
> <span data-ttu-id="92aa4-114">Pro eindeutigem Satz von Eigenschaften gibt es nur einen Index.</span><span class="sxs-lookup"><span data-stu-id="92aa4-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="92aa4-115">Wenn Sie die fließende API verwenden, um einen Index für eine Gruppe von Eigenschaften zu konfigurieren, für die bereits ein Index definiert wurde (entweder durch Konvention oder vorherige Konfiguration), ändern Sie die Definition des Indexes.</span><span class="sxs-lookup"><span data-stu-id="92aa4-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="92aa4-116">Dies ist hilfreich, wenn Sie einen von der Konvention erstellten Index weiter konfigurieren möchten.</span><span class="sxs-lookup"><span data-stu-id="92aa4-116">This is useful if you want to further configure an index that was created by convention.</span></span>
