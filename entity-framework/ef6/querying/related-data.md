---
title: Laden von verbundenen Entitäten – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
ms.openlocfilehash: 1d59e6c079e306158ed918cde16e69c9cb084711
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489049"
---
# <a name="loading-related-entities"></a><span data-ttu-id="a2228-102">Laden verwandter Entitäten</span><span class="sxs-lookup"><span data-stu-id="a2228-102">Loading Related Entities</span></span>
<span data-ttu-id="a2228-103">Entitätsframework unterstützt drei Möglichkeiten zum Laden verwandter Daten: Eager laden, lazy Loading und explizites Laden.</span><span class="sxs-lookup"><span data-stu-id="a2228-103">Entity Framework supports three ways to load related data - eager loading, lazy loading and explicit loading.</span></span> <span data-ttu-id="a2228-104">Die in diesem Thema dargestellten Techniken gelten jeweils für Modelle, die mit Code First und dem EF-Designer erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="a2228-104">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="eagerly-loading"></a><span data-ttu-id="a2228-105">Eager Loading geladen.</span><span class="sxs-lookup"><span data-stu-id="a2228-105">Eagerly Loading</span></span>  

<span data-ttu-id="a2228-106">Eager Loading ist der Prozess, bei dem eine Abfrage für einen Entitätstyp auch verknüpfte Entitäten als Teil der Abfrage wird geladen.</span><span class="sxs-lookup"><span data-stu-id="a2228-106">Eager loading is the process whereby a query for one type of entity also loads related entities as part of the query.</span></span> <span data-ttu-id="a2228-107">Eager Loading erfolgt mithilfe der Include-Methode.</span><span class="sxs-lookup"><span data-stu-id="a2228-107">Eager loading is achieved by use of the Include method.</span></span> <span data-ttu-id="a2228-108">Die folgenden Abfragen werden z. B. Blogs und alle Beiträge, die im Zusammenhang mit jedem Blog geladen.</span><span class="sxs-lookup"><span data-stu-id="a2228-108">For example, the queries below will load blogs and all the posts related to each blog.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs and related posts
    var blogs1 = context.Blogs
                          .Include(b => b.Posts)
                          .ToList();

    // Load one blogs and its related posts
    var blog1 = context.Blogs
                        .Where(b => b.Name == "ADO.NET Blog")
                        .Include(b => b.Posts)
                        .FirstOrDefault();

    // Load all blogs and related posts  
    // using a string to specify the relationship
    var blogs2 = context.Blogs
                          .Include("Posts")
                          .ToList();

    // Load one blog and its related posts  
    // using a string to specify the relationship
    var blog2 = context.Blogs
                        .Where(b => b.Name == "ADO.NET Blog")
                        .Include("Posts")
                        .FirstOrDefault();
}
```  

<span data-ttu-id="a2228-109">Beachten Sie, dass das Einschließen einer Erweiterungsmethode im System.Data.Entity-Namespace ist also stellen Sie sicher, dass Sie diesen Namespace verwenden.</span><span class="sxs-lookup"><span data-stu-id="a2228-109">Note that Include is an extension method in the System.Data.Entity namespace so make sure you are using that namespace.</span></span>  

### <a name="eagerly-loading-multiple-levels"></a><span data-ttu-id="a2228-110">Eager Loading Laden von mehreren Ebenen</span><span class="sxs-lookup"><span data-stu-id="a2228-110">Eagerly loading multiple levels</span></span>  

<span data-ttu-id="a2228-111">Es ist auch möglich, mehrere Ebenen von verknüpften Entitäten vorzeitig geladen werden.</span><span class="sxs-lookup"><span data-stu-id="a2228-111">It is also possible to eagerly load multiple levels of related entities.</span></span> <span data-ttu-id="a2228-112">Die folgenden Abfragen sind Beispiele für für sowohl die Datensammlung als auch die verweisnavigationseigenschaften dazu.</span><span class="sxs-lookup"><span data-stu-id="a2228-112">The queries below show examples of how to do this for both collection and reference navigation properties.</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs, all related posts, and all related comments
    var blogs1 = context.Blogs
                       .Include(b => b.Posts.Select(p => p.Comments))
                       .ToList();

    // Load all users their related profiles, and related avatar
    var users1 = context.Users
                        .Include(u => u.Profile.Avatar)
                        .ToList();

    // Load all blogs, all related posts, and all related comments  
    // using a string to specify the relationships
    var blogs2 = context.Blogs
                       .Include("Posts.Comments")
                       .ToList();

    // Load all users their related profiles, and related avatar  
    // using a string to specify the relationships
    var users2 = context.Users
                        .Include("Profile.Avatar")
                        .ToList();
}
```  

<span data-ttu-id="a2228-113">Beachten Sie, dass es nicht Filter derzeit möglich ist, welche verknüpften Entitäten geladen werden.</span><span class="sxs-lookup"><span data-stu-id="a2228-113">Note that it is not currently possible to filter which related entities are loaded.</span></span> <span data-ttu-id="a2228-114">Einschließen wird immer in alle verknüpften Entitäten anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="a2228-114">Include will always bring in all related entities.</span></span>  

## <a name="lazy-loading"></a><span data-ttu-id="a2228-115">Lazy Loading</span><span class="sxs-lookup"><span data-stu-id="a2228-115">Lazy Loading</span></span>  

<span data-ttu-id="a2228-116">Mit Lazy Loading steht der Prozess, bei dem eine Entität oder eine Auflistung von Entitäten automatisch aus der Datenbank erstmalig geladen wird, die eine Eigenschaft verweisen auf die Entität/Entitäten zugegriffen wird.</span><span class="sxs-lookup"><span data-stu-id="a2228-116">Lazy loading is the process whereby an entity or collection of entities is automatically loaded from the database the first time that a property referring to the entity/entities is accessed.</span></span> <span data-ttu-id="a2228-117">Wenn Sie POCO-Entität-Typen zu verwenden, erfolgt die Lazy Load durch Erstellen von Instanzen der abgeleiteten Proxytypen, und klicken Sie dann überschreiben virtuelle Eigenschaften zum Hinzufügen des Hook laden.</span><span class="sxs-lookup"><span data-stu-id="a2228-117">When using POCO entity types, lazy loading is achieved by creating instances of derived proxy types and then overriding virtual properties to add the loading hook.</span></span> <span data-ttu-id="a2228-118">Beispielsweise werden bei Verwendung die Blog-Entity-Klasse, die nachstehend definiert Beiträge erstmalig geladen werden, die die Beiträge Navigationseigenschaft zugegriffen wird:</span><span class="sxs-lookup"><span data-stu-id="a2228-118">For example, when using the Blog entity class defined below, the related Posts will be loaded the first time the Posts navigation property is accessed:</span></span>  

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

### <a name="turn-lazy-loading-off-for-serialization"></a><span data-ttu-id="a2228-119">Aktivieren von lazy loading-für die Serialisierung</span><span class="sxs-lookup"><span data-stu-id="a2228-119">Turn lazy loading off for serialization</span></span>  

<span data-ttu-id="a2228-120">Lazy Loading und Serialisierung nicht auch kombinieren, und wenn Sie nicht vorsichtig sind kommen Sie können Abfragen für Ihre gesamte Datenbank, nur weil lazy Loading aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="a2228-120">Lazy loading and serialization don’t mix well, and if you aren’t careful you can end up querying for your entire database just because lazy loading is enabled.</span></span> <span data-ttu-id="a2228-121">Die meisten Serialisierungsprogramme arbeiten, indem Sie den Zugriff auf jede Eigenschaft in einer Instanz eines Typs.</span><span class="sxs-lookup"><span data-stu-id="a2228-121">Most serializers work by accessing each property on an instance of a type.</span></span> <span data-ttu-id="a2228-122">Eigenschaftenzugriff löst das verzögerte Laden, damit weitere Entitäten serialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="a2228-122">Property access triggers lazy loading, so more entities get serialized.</span></span> <span data-ttu-id="a2228-123">Für diese Entitäten Eigenschaften zugegriffen werden, und sogar noch mehr Entitäten geladen.</span><span class="sxs-lookup"><span data-stu-id="a2228-123">On those entities properties are accessed, and even more entities are loaded.</span></span> <span data-ttu-id="a2228-124">Es hat sich bewährt, deaktivieren lazy loading-deaktivieren, bevor Sie eine Entität zu serialisieren.</span><span class="sxs-lookup"><span data-stu-id="a2228-124">It’s a good practice to turn lazy loading off before you serialize an entity.</span></span> <span data-ttu-id="a2228-125">Die folgenden Abschnitte zeigen, wie Sie dies tun.</span><span class="sxs-lookup"><span data-stu-id="a2228-125">The following sections show how to do this.</span></span>  

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a><span data-ttu-id="a2228-126">Lazy loading für Navigationseigenschaften für bestimmte deaktivieren</span><span class="sxs-lookup"><span data-stu-id="a2228-126">Turning off lazy loading for specific navigation properties</span></span>  

<span data-ttu-id="a2228-127">Lazy Loading für die Sammlung von Beiträgen kann durch Festlegen der Beiträge Eigenschaft nicht virtuell deaktiviert werden:</span><span class="sxs-lookup"><span data-stu-id="a2228-127">Lazy loading of the Posts collection can be turned off by making the Posts property non-virtual:</span></span>  

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

<span data-ttu-id="a2228-128">Laden die Beiträge Auflistung kann weiterhin erreicht werden über eager Loading (finden Sie unter *Eager Loading geladen* oben) oder die Load-Methode (finden Sie unter *explizites Laden* unten).</span><span class="sxs-lookup"><span data-stu-id="a2228-128">Loading of the Posts collection can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>  

### <a name="turn-off-lazy-loading-for-all-entities"></a><span data-ttu-id="a2228-129">Deaktivieren Sie lazy loading für alle Entitäten</span><span class="sxs-lookup"><span data-stu-id="a2228-129">Turn off lazy loading for all entities</span></span>  

<span data-ttu-id="a2228-130">Lazy Loading kann für alle Entitäten im Kontext deaktiviert werden, indem ein Kennzeichen festlege, für die Konfigurationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="a2228-130">Lazy loading can be turned off for all entities in the context by setting a flag on the Configuration property.</span></span> <span data-ttu-id="a2228-131">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a2228-131">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```  

<span data-ttu-id="a2228-132">Laden von verknüpften Entitäten kann weiterhin über eager Loading erreicht werden (finden Sie unter *Eager Loading geladen* oben) oder die Load-Methode (finden Sie unter *explizites Laden* unten).</span><span class="sxs-lookup"><span data-stu-id="a2228-132">Loading of related entities can still be achieved using eager loading (see *Eagerly Loading* above) or the Load method (see *Explicitly Loading* below).</span></span>  

## <a name="explicitly-loading"></a><span data-ttu-id="a2228-133">Explizites Laden</span><span class="sxs-lookup"><span data-stu-id="a2228-133">Explicitly Loading</span></span>  

<span data-ttu-id="a2228-134">Selbst bei lazy Loading deaktiviert ist es weiterhin möglich, verknüpfte Entitäten verzögert zu laden, aber ein expliziter Aufruf abgeschlossen werden muss.</span><span class="sxs-lookup"><span data-stu-id="a2228-134">Even with lazy loading disabled it is still possible to lazily load related entities, but it must be done with an explicit call.</span></span> <span data-ttu-id="a2228-135">Zu diesem Zweck verwenden Sie die Load-Methode, auf die verknüpfte Entität Eintrag.</span><span class="sxs-lookup"><span data-stu-id="a2228-135">To do so you use the Load method on the related entity’s entry.</span></span> <span data-ttu-id="a2228-136">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a2228-136">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var post = context.Posts.Find(2);

    // Load the blog related to a given post
    context.Entry(post).Reference(p => p.Blog).Load();

    // Load the blog related to a given post using a string  
    context.Entry(post).Reference("Blog").Load();

    var blog = context.Blogs.Find(1);

    // Load the posts related to a given blog
    context.Entry(blog).Collection(p => p.Posts).Load();

    // Load the posts related to a given blog  
    // using a string to specify the relationship
    context.Entry(blog).Collection("Posts").Load();
}
```  

<span data-ttu-id="a2228-137">Beachten Sie, dass die Reference-Methode verwendet werden soll, wenn eine Entität über eine Navigationseigenschaft für eine andere einzelne Entität verfügt.</span><span class="sxs-lookup"><span data-stu-id="a2228-137">Note that the Reference method should be used when an entity has a navigation property to another single entity.</span></span> <span data-ttu-id="a2228-138">Andererseits, sollte die Methode verwendet werden, wenn eine Entität über eine Navigationseigenschaft für eine Auflistung von anderen Entitäten verfügt.</span><span class="sxs-lookup"><span data-stu-id="a2228-138">On the other hand, the Collection method should be used when an entity has a navigation property to a collection of other entities.</span></span>  

### <a name="applying-filters-when-explicitly-loading-related-entities"></a><span data-ttu-id="a2228-139">Anwenden von Filtern beim verknüpfte Entitäten explizit zu laden</span><span class="sxs-lookup"><span data-stu-id="a2228-139">Applying filters when explicitly loading related entities</span></span>  

<span data-ttu-id="a2228-140">Die Query-Methode ermöglicht den Zugriff auf die zugrunde liegende Abfrage, die Entity Framework beim Laden von verknüpften Entitäten verwenden.</span><span class="sxs-lookup"><span data-stu-id="a2228-140">The Query method provides access to the underlying query that Entity Framework will use when loading related entities.</span></span> <span data-ttu-id="a2228-141">Sie können dann LINQ verwenden, zum Anwenden von Filtern für die Abfrage vor der Ausführung durch einen Aufruf einer Erweiterungsmethode für LINQ wie "ToList", Auslastung usw. Die Query-Methode mit sowohl Referenz-als auch Auflistung Navigationseigenschaften verwendet werden kann, aber es eignet sich am besten für Sammlungen, in dem er dient auch zum Laden nur einen Teil der Auflistung.</span><span class="sxs-lookup"><span data-stu-id="a2228-141">You can then use LINQ to apply filters to the query before executing it with a call to a LINQ extension method such as ToList, Load, etc. The Query method can be used with both reference and collection navigation properties but is most useful for collections where it can be used to load only part of the collection.</span></span> <span data-ttu-id="a2228-142">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a2228-142">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();

    // Load the posts with the 'entity-framework' tag related to a given blog  
    // using a string to specify the relationship  
    context.Entry(blog)
        .Collection("Posts")
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();
}
```  

<span data-ttu-id="a2228-143">Bei Verwendung die Query-Methode empfiehlt es sich in der Regel um lazy loading für die Navigationseigenschaft zu deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="a2228-143">When using the Query method it is usually best to turn off lazy loading for the navigation property.</span></span> <span data-ttu-id="a2228-144">Dies ist, da andernfalls die gesamte Auflistung automatisch durch den Mechanismus für die Lazy Load geladen werden kann entweder vor oder nach die gefilterte Abfrage ausgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="a2228-144">This is because otherwise the entire collection may get loaded automatically by the lazy loading mechanism either before or after the filtered query has been executed.</span></span>  

<span data-ttu-id="a2228-145">Beachten Sie, dass während die Beziehung als eine Zeichenfolge anstatt in einem Lambda-Ausdruck angegeben werden kann, das zurückgegebene IQueryable-Objekt nicht generischen, wenn eine Zeichenfolge verwendet wird und damit die Cast-Methode in der Regel benötigt wird, damit etwas Nützliches mit ihm durchgeführt werden kann.</span><span class="sxs-lookup"><span data-stu-id="a2228-145">Note that while the relationship can be specified as a string instead of a lambda expression, the returned IQueryable is not generic when a string is used and so the Cast method is usually needed before anything useful can be done with it.</span></span>  

## <a name="using-query-to-count-related-entities-without-loading-them"></a><span data-ttu-id="a2228-146">Verwenden die Abfrage um verknüpfte Entitäten zu zählen, ohne zu laden</span><span class="sxs-lookup"><span data-stu-id="a2228-146">Using Query to count related entities without loading them</span></span>  

<span data-ttu-id="a2228-147">Manchmal ist es hilfreich zu wissen, wie viele Entitäten mit einer anderen Entität in der Datenbank verknüpft sind, ohne dass tatsächlich die Kosten für alle diese Entitäten werden geladen.</span><span class="sxs-lookup"><span data-stu-id="a2228-147">Sometimes it is useful to know how many entities are related to another entity in the database without actually incurring the cost of loading all those entities.</span></span> <span data-ttu-id="a2228-148">Die Query-Methode, mit der Anzahl der LINQ-Methode kann dazu verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="a2228-148">The Query method with the LINQ Count method can be used to do this.</span></span> <span data-ttu-id="a2228-149">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a2228-149">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Count how many posts the blog has  
    var postCount = context.Entry(blog)
                          .Collection(b => b.Posts)
                          .Query()
                          .Count();
}
```  
