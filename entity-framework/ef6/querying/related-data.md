---
title: Laden verwandter Entitäten EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
ms.openlocfilehash: c359d8d32a88049213fd5e98e99fe49d7e3121a3
ms.sourcegitcommit: d01fc19aa42ca34c3bebccbc96ee26d06fcecaa2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/16/2019
ms.locfileid: "71005475"
---
# <a name="loading-related-entities"></a><span data-ttu-id="572a1-102">Laden verwandter Entitäten</span><span class="sxs-lookup"><span data-stu-id="572a1-102">Loading Related Entities</span></span>

<span data-ttu-id="572a1-103">Entity Framework unterstützt drei Möglichkeiten, verwandte Daten Eager Loading, Lazy Loading und expliziten Laden zu laden.</span><span class="sxs-lookup"><span data-stu-id="572a1-103">Entity Framework supports three ways to load related data - eager loading, lazy loading and explicit loading.</span></span> <span data-ttu-id="572a1-104">Die in diesem Thema dargestellten Techniken gelten jeweils für Modelle, die mit Code First und dem EF-Designer erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="572a1-104">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>

## <a name="eagerly-loading"></a><span data-ttu-id="572a1-105">Eifrig geladen</span><span class="sxs-lookup"><span data-stu-id="572a1-105">Eagerly Loading</span></span>

<span data-ttu-id="572a1-106">Das unverzügliches Laden ist der Prozess, bei dem eine Abfrage für einen Entitätstyp auch Verwandte Entitäten als Teil der Abfrage lädt.</span><span class="sxs-lookup"><span data-stu-id="572a1-106">Eager loading is the process whereby a query for one type of entity also loads related entities as part of the query.</span></span> <span data-ttu-id="572a1-107">Das unverzügliches Laden wird durch die Verwendung der Include-Methode erreicht.</span><span class="sxs-lookup"><span data-stu-id="572a1-107">Eager loading is achieved by use of the Include method.</span></span> <span data-ttu-id="572a1-108">Die nachfolgenden Abfragen laden z. b. Blogs und alle Beiträge im Zusammenhang mit den einzelnen Blogs.</span><span class="sxs-lookup"><span data-stu-id="572a1-108">For example, the queries below will load blogs and all the posts related to each blog.</span></span>

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs and related posts.
    var blogs1 = context.Blogs
                        .Include(b => b.Posts)
                        .ToList();

    // Load one blog and its related posts.
    var blog1 = context.Blogs
                       .Where(b => b.Name == "ADO.NET Blog")
                       .Include(b => b.Posts)
                       .FirstOrDefault();

    // Load all blogs and related posts
    // using a string to specify the relationship.
    var blogs2 = context.Blogs
                        .Include("Posts")
                        .ToList();

    // Load one blog and its related posts
    // using a string to specify the relationship.
    var blog2 = context.Blogs
                       .Where(b => b.Name == "ADO.NET Blog")
                       .Include("Posts")
                       .FirstOrDefault();
}
```

> [!NOTE]
> <span data-ttu-id="572a1-109">Include ist eine Erweiterungsmethode im System. Data. Entity-Namespace. Stellen Sie daher sicher, dass Sie diesen Namespace verwenden.</span><span class="sxs-lookup"><span data-stu-id="572a1-109">Include is an extension method in the System.Data.Entity namespace so make sure you are using that namespace.</span></span>

### <a name="eagerly-loading-multiple-levels"></a><span data-ttu-id="572a1-110">Das eifrig Laden mehrerer Ebenen</span><span class="sxs-lookup"><span data-stu-id="572a1-110">Eagerly loading multiple levels</span></span>

<span data-ttu-id="572a1-111">Es ist auch möglich, mehrere Ebenen verwandter Entitäten zu laden.</span><span class="sxs-lookup"><span data-stu-id="572a1-111">It is also possible to eagerly load multiple levels of related entities.</span></span> <span data-ttu-id="572a1-112">Die folgenden Abfragen zeigen Beispiele dazu, wie Sie dies für die Sammlungs-und Verweis Navigations Eigenschaften durchführen können.</span><span class="sxs-lookup"><span data-stu-id="572a1-112">The queries below show examples of how to do this for both collection and reference navigation properties.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs, all related posts, and all related comments.
    var blogs1 = context.Blogs
                        .Include(b => b.Posts.Select(p => p.Comments))
                        .ToList();

    // Load all users, their related profiles, and related avatar.
    var users1 = context.Users
                        .Include(u => u.Profile.Avatar)
                        .ToList();

    // Load all blogs, all related posts, and all related comments  
    // using a string to specify the relationships.
    var blogs2 = context.Blogs
                        .Include("Posts.Comments")
                        .ToList();

    // Load all users, their related profiles, and related avatar  
    // using a string to specify the relationships.
    var users2 = context.Users
                        .Include("Profile.Avatar")
                        .ToList();
}
```  

> [!NOTE]
> <span data-ttu-id="572a1-113">Es ist derzeit nicht möglich, zu filtern, welche verknüpften Entitäten geladen werden.</span><span class="sxs-lookup"><span data-stu-id="572a1-113">It is not currently possible to filter which related entities are loaded.</span></span> <span data-ttu-id="572a1-114">Include führt immer alle zugehörigen Entitäten ein.</span><span class="sxs-lookup"><span data-stu-id="572a1-114">Include will always bring in all related entities.</span></span>  

## <a name="lazy-loading"></a><span data-ttu-id="572a1-115">Lazy Loading</span><span class="sxs-lookup"><span data-stu-id="572a1-115">Lazy Loading</span></span>

<span data-ttu-id="572a1-116">Lazy Load ist der Prozess, bei dem eine Entität oder eine Auflistung von Entitäten automatisch aus der Datenbank geladen wird, wenn zum ersten Mal auf eine Eigenschaft zugegriffen wird, die auf Entitäten/Entitäten verweist.</span><span class="sxs-lookup"><span data-stu-id="572a1-116">Lazy loading is the process whereby an entity or collection of entities is automatically loaded from the database the first time that a property referring to the entity/entities is accessed.</span></span> <span data-ttu-id="572a1-117">Wenn poco-Entitäts Typen verwendet werden, wird Lazy Loading durch Erstellen von Instanzen abgeleiteter Proxy Typen und anschließendes Überschreiben von virtuellen Eigenschaften zum Hinzufügen des Lade Hooks erreicht.</span><span class="sxs-lookup"><span data-stu-id="572a1-117">When using POCO entity types, lazy loading is achieved by creating instances of derived proxy types and then overriding virtual properties to add the loading hook.</span></span> <span data-ttu-id="572a1-118">Wenn Sie z. b. die unten definierte Blog-Entitäts Klasse verwenden, werden die zugehörigen Beiträge beim ersten Zugriff auf die "Posts"-Navigations Eigenschaft geladen:</span><span class="sxs-lookup"><span data-stu-id="572a1-118">For example, when using the Blog entity class defined below, the related Posts will be loaded the first time the Posts navigation property is accessed:</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }
    public string Tags { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}
```

### <a name="turn-lazy-loading-off-for-serialization"></a><span data-ttu-id="572a1-119">Lazy Loading für Serialisierung deaktivieren</span><span class="sxs-lookup"><span data-stu-id="572a1-119">Turn lazy loading off for serialization</span></span>

<span data-ttu-id="572a1-120">Lazy Load und Serialisierung sind nicht gut gemischt, und wenn Sie nicht vorsichtig sind, können Sie die Abfrage für Ihre gesamte Datenbank durchzuführen, da Lazy Loading aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="572a1-120">Lazy loading and serialization don’t mix well, and if you aren’t careful you can end up querying for your entire database just because lazy loading is enabled.</span></span> <span data-ttu-id="572a1-121">Die meisten serialisierungsinitialisierer funktionieren, indem Sie auf jede Eigenschaft einer Instanz eines Typs zugreifen.</span><span class="sxs-lookup"><span data-stu-id="572a1-121">Most serializers work by accessing each property on an instance of a type.</span></span> <span data-ttu-id="572a1-122">Eigenschaften Zugriffs Trigger Lazy Loading, sodass weitere Entitäten serialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="572a1-122">Property access triggers lazy loading, so more entities get serialized.</span></span> <span data-ttu-id="572a1-123">Auf diesen Entitäten wird zugegriffen, und es werden sogar weitere Entitäten geladen.</span><span class="sxs-lookup"><span data-stu-id="572a1-123">On those entities properties are accessed, and even more entities are loaded.</span></span> <span data-ttu-id="572a1-124">Es empfiehlt sich, Lazy Loading zu deaktivieren, bevor Sie eine Entität serialisieren.</span><span class="sxs-lookup"><span data-stu-id="572a1-124">It’s a good practice to turn lazy loading off before you serialize an entity.</span></span> <span data-ttu-id="572a1-125">In den folgenden Abschnitten wird gezeigt, wie dies geschieht.</span><span class="sxs-lookup"><span data-stu-id="572a1-125">The following sections show how to do this.</span></span>

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a><span data-ttu-id="572a1-126">Ausschalten Lazy Loading für bestimmte Navigations Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="572a1-126">Turning off lazy loading for specific navigation properties</span></span>

<span data-ttu-id="572a1-127">Das Lazy Load der Posts-Auflistung kann deaktiviert werden, indem die Posts-Eigenschaft nicht virtuell ist:</span><span class="sxs-lookup"><span data-stu-id="572a1-127">Lazy loading of the Posts collection can be turned off by making the Posts property non-virtual:</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }
    public string Tags { get; set; }

    public ICollection<Post> Posts { get; set; }
}
```

<span data-ttu-id="572a1-128">Das Laden der Posts-Auflistung kann weiterhin mithilfe von Eager Loading (siehe das explizite *Laden* oben) oder der Load-Methode erzielt werden (siehe *Explizites Laden* unten).</span><span class="sxs-lookup"><span data-stu-id="572a1-128">Loading of the Posts collection can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>

### <a name="turn-off-lazy-loading-for-all-entities"></a><span data-ttu-id="572a1-129">Lazy Loading für alle Entitäten deaktivieren</span><span class="sxs-lookup"><span data-stu-id="572a1-129">Turn off lazy loading for all entities</span></span>

<span data-ttu-id="572a1-130">Lazy Load kann für alle Entitäten im Kontext deaktiviert werden, indem ein Flag für die Konfigurations Eigenschaft festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="572a1-130">Lazy loading can be turned off for all entities in the context by setting a flag on the Configuration property.</span></span> <span data-ttu-id="572a1-131">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="572a1-131">For example:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```

<span data-ttu-id="572a1-132">Das Laden von verknüpften Entitäten kann weiterhin mithilfe von Eager Loading (siehe das explizite *Laden* oben) oder der Load-Methode erzielt werden (siehe *Explizites Laden* unten).</span><span class="sxs-lookup"><span data-stu-id="572a1-132">Loading of related entities can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>

## <a name="explicitly-loading"></a><span data-ttu-id="572a1-133">Explizites Laden</span><span class="sxs-lookup"><span data-stu-id="572a1-133">Explicitly Loading</span></span>

<span data-ttu-id="572a1-134">Auch wenn Lazy Loading deaktiviert ist, ist es weiterhin möglich, verknüpfte Entitäten verzögert zu laden, aber es muss ein expliziter-Vorgang ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="572a1-134">Even with lazy loading disabled it is still possible to lazily load related entities, but it must be done with an explicit call.</span></span> <span data-ttu-id="572a1-135">Zu diesem Zweck verwenden Sie die Load-Methode für den Eintrag der zugehörigen Entität.</span><span class="sxs-lookup"><span data-stu-id="572a1-135">To do so you use the Load method on the related entity’s entry.</span></span> <span data-ttu-id="572a1-136">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="572a1-136">For example:</span></span>

``` csharp
using (var context = new BloggingContext())
{
    var post = context.Posts.Find(2);

    // Load the blog related to a given post.
    context.Entry(post).Reference(p => p.Blog).Load();

    // Load the blog related to a given post using a string.
    context.Entry(post).Reference("Blog").Load();

    var blog = context.Blogs.Find(1);

    // Load the posts related to a given blog.
    context.Entry(blog).Collection(p => p.Posts).Load();

    // Load the posts related to a given blog
    // using a string to specify the relationship.
    context.Entry(blog).Collection("Posts").Load();
}
```

> [!NOTE]
> <span data-ttu-id="572a1-137">Die Reference-Methode sollte verwendet werden, wenn eine Entität über eine Navigations Eigenschaft zu einer anderen einzelnen Entität verfügt.</span><span class="sxs-lookup"><span data-stu-id="572a1-137">The Reference method should be used when an entity has a navigation property to another single entity.</span></span> <span data-ttu-id="572a1-138">Dagegen sollte die-Auflistungs Methode verwendet werden, wenn eine Entität über eine Navigations Eigenschaft für eine Auflistung von anderen Entitäten verfügt.</span><span class="sxs-lookup"><span data-stu-id="572a1-138">On the other hand, the Collection method should be used when an entity has a navigation property to a collection of other entities.</span></span>

### <a name="applying-filters-when-explicitly-loading-related-entities"></a><span data-ttu-id="572a1-139">Anwenden von Filtern beim expliziten Laden verwandter Entitäten</span><span class="sxs-lookup"><span data-stu-id="572a1-139">Applying filters when explicitly loading related entities</span></span>

<span data-ttu-id="572a1-140">Die Query-Methode ermöglicht den Zugriff auf die zugrunde liegende Abfrage, die Entity Framework beim Laden von verknüpften Entitäten verwenden wird.</span><span class="sxs-lookup"><span data-stu-id="572a1-140">The Query method provides access to the underlying query that Entity Framework will use when loading related entities.</span></span> <span data-ttu-id="572a1-141">Anschließend können Sie mithilfe von LINQ Filter auf die Abfrage anwenden, bevor Sie Sie ausführen, indem Sie eine LINQ-Erweiterungsmethode wie z. b. ToList, Load usw. verwenden. Die Query-Methode kann sowohl mit Verweis-als auch Auflistungs Navigations Eigenschaften verwendet werden, ist jedoch besonders nützlich für Auflistungen, in denen Sie zum Laden von nur einem Teil der Auflistung verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="572a1-141">You can then use LINQ to apply filters to the query before executing it with a call to a LINQ extension method such as ToList, Load, etc. The Query method can be used with both reference and collection navigation properties but is most useful for collections where it can be used to load only part of the collection.</span></span> <span data-ttu-id="572a1-142">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="572a1-142">For example:</span></span>

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog.
    context.Entry(blog)
           .Collection(b => b.Posts)
           .Query()
           .Where(p => p.Tags.Contains("entity-framework"))
           .Load();

    // Load the posts with the 'entity-framework' tag related to a given blog
    // using a string to specify the relationship.
    context.Entry(blog)
           .Collection("Posts")
           .Query()
           .Where(p => p.Tags.Contains("entity-framework"))
           .Load();
}
```

<span data-ttu-id="572a1-143">Wenn Sie die Abfrage Methode verwenden, empfiehlt es sich in der Regel, Lazy Loading für die Navigations Eigenschaft zu deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="572a1-143">When using the Query method it is usually best to turn off lazy loading for the navigation property.</span></span> <span data-ttu-id="572a1-144">Der Grund hierfür ist, dass die gesamte Auflistung möglicherweise automatisch vom Lazy Loading Mechanismus geladen wird, entweder vor oder nach der Ausführung der gefilterten Abfrage.</span><span class="sxs-lookup"><span data-stu-id="572a1-144">This is because otherwise the entire collection may get loaded automatically by the lazy loading mechanism either before or after the filtered query has been executed.</span></span>

> [!NOTE]
> <span data-ttu-id="572a1-145">Obwohl die Beziehung anstelle eines Lambda-Ausdrucks als Zeichenfolge angegeben werden kann, ist das zurückgegebene iquerable-Element nicht generisch, wenn eine Zeichenfolge verwendet wird, sodass die Cast-Methode normalerweise benötigt wird, bevor etwas Nützliches möglich ist.</span><span class="sxs-lookup"><span data-stu-id="572a1-145">While the relationship can be specified as a string instead of a lambda expression, the returned IQueryable is not generic when a string is used and so the Cast method is usually needed before anything useful can be done with it.</span></span>

## <a name="using-query-to-count-related-entities-without-loading-them"></a><span data-ttu-id="572a1-146">Verwenden von Abfragen zum zählen verwandter Entitäten, ohne Sie zu laden</span><span class="sxs-lookup"><span data-stu-id="572a1-146">Using Query to count related entities without loading them</span></span>

<span data-ttu-id="572a1-147">Manchmal ist es hilfreich zu wissen, wie viele Entitäten mit einer anderen Entität in der Datenbank verknüpft sind, ohne dass tatsächlich die Kosten für das Laden all dieser Entitäten anfallen.</span><span class="sxs-lookup"><span data-stu-id="572a1-147">Sometimes it is useful to know how many entities are related to another entity in the database without actually incurring the cost of loading all those entities.</span></span> <span data-ttu-id="572a1-148">Hierfür kann die Query-Methode mit der LINQ Count-Methode verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="572a1-148">The Query method with the LINQ Count method can be used to do this.</span></span> <span data-ttu-id="572a1-149">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="572a1-149">For example:</span></span>

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Count how many posts the blog has.
    var postCount = context.Entry(blog)
                           .Collection(b => b.Posts)
                           .Query()
                           .Count();
}
```
