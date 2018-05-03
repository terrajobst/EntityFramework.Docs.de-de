---
title: Laden von verknüpften Daten - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: 5f1fb9376300739ab0e306d9d60e7ec71aa2d2e7
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/26/2018
---
# <a name="loading-related-data"></a><span data-ttu-id="65a13-102">Laden von verknüpften Daten</span><span class="sxs-lookup"><span data-stu-id="65a13-102">Loading Related Data</span></span>

<span data-ttu-id="65a13-103">Entity Framework Core können Sie die Navigationseigenschaften zum Laden verknüpfter Entitäten im Modell zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="65a13-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="65a13-104">Es gibt drei allgemeine O/RM-Muster, die zum Laden von verknüpften Daten verwendet.</span><span class="sxs-lookup"><span data-stu-id="65a13-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="65a13-105">**Unverzüglichem Laden** bedeutet, dass die verknüpften Daten aus der Datenbank als Teil der ursprünglichen Abfrage geladen werden.</span><span class="sxs-lookup"><span data-stu-id="65a13-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="65a13-106">**Explizites Laden** bedeutet, dass die verwandten Daten explizit aus der Datenbank zu einem späteren Zeitpunkt geladen werden.</span><span class="sxs-lookup"><span data-stu-id="65a13-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="65a13-107">**Verzögertes Laden** bedeutet, dass die verwandten Daten transparent aus der Datenbank geladen werden, wenn die Navigationseigenschaft zugegriffen wird.</span><span class="sxs-lookup"><span data-stu-id="65a13-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="65a13-108">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="65a13-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="65a13-109">Eager Loading</span><span class="sxs-lookup"><span data-stu-id="65a13-109">Eager loading</span></span>

<span data-ttu-id="65a13-110">Sie können die `Include` Methode, um verwandte Daten angeben, die in den Ergebnissen enthalten sein.</span><span class="sxs-lookup"><span data-stu-id="65a13-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="65a13-111">Im folgenden Beispiel müssen die Blogs, die in den Ergebnissen zurückgegeben werden ihre `Posts` Eigenschaft, die mit den zugehörigen Beiträgen aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="65a13-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="65a13-112">Entity Framework Core wird automatisch ein Fixup-Navigationseigenschaften für alle anderen Entitäten, die zuvor in die Kontextinstanz geladen wurden.</span><span class="sxs-lookup"><span data-stu-id="65a13-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="65a13-113">Damit auch, wenn Sie die Daten für eine Navigationseigenschaft explizit nicht einschließen, die Eigenschaft kann noch immer aufgefüllt werden, wenn einige oder alle verknüpften Entitäten wurden zuvor geladen.</span><span class="sxs-lookup"><span data-stu-id="65a13-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="65a13-114">Sie können aufeinander bezogene Daten in mehreren Beziehungen in einer einzelnen Abfrage einschließen.</span><span class="sxs-lookup"><span data-stu-id="65a13-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="65a13-115">Einschließlich mehrere Ebenen</span><span class="sxs-lookup"><span data-stu-id="65a13-115">Including multiple levels</span></span>

<span data-ttu-id="65a13-116">Drilldown kann über Beziehungen zu mehrere Stufen der verknüpften Daten mithilfe der `ThenInclude` Methode.</span><span class="sxs-lookup"><span data-stu-id="65a13-116">You can drill down thru relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="65a13-117">Im folgende Beispiel lädt alle Blogs, Verwandte Beiträge und der Autor jedes Post.</span><span class="sxs-lookup"><span data-stu-id="65a13-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> <span data-ttu-id="65a13-118">Aktuelle Versionen von Visual Studio bieten eine falsche Code Abschluss Optionen und können dazu führen, dass das richtige Ausdrücke mit Syntaxfehler gekennzeichnet wird, bei Verwendung der `ThenInclude` Methode auf, nachdem eine auflistungsnavigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="65a13-118">Current versions of Visual Studio offer incorrect code completion options and can cause correct expressions to be flagged with syntax errors when using the `ThenInclude` method after a collection navigation property.</span></span> <span data-ttu-id="65a13-119">Dies ist ein Symptom für ein IntelliSense-Fehler überwachten https://github.com/dotnet/roslyn/issues/8237.</span><span class="sxs-lookup"><span data-stu-id="65a13-119">This is a symptom of an IntelliSense bug tracked at https://github.com/dotnet/roslyn/issues/8237.</span></span> <span data-ttu-id="65a13-120">Sie können ruhig, um diese unbegründete Syntaxfehler zu ignorieren, solange der Code korrekt ist und erfolgreich kompiliert werden.</span><span class="sxs-lookup"><span data-stu-id="65a13-120">It is safe to ignore these spurious syntax errors as long as the code is correct and can be compiled successfully.</span></span> 

<span data-ttu-id="65a13-121">Sie können mehrere Aufrufe verkettet `ThenInclude` zu fortfahren, einschließlich der weiteren Ebenen verknüpfter Daten.</span><span class="sxs-lookup"><span data-stu-id="65a13-121">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="65a13-122">Sie können diese Option, um verwandte Daten aus mehreren Ebenen und mehrere Stämme in derselben Abfrage umfassen alle kombinieren.</span><span class="sxs-lookup"><span data-stu-id="65a13-122">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="65a13-123">Möglicherweise sollen mehrere verknüpfte Entitäten für eine der Entitäten, die gerade enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="65a13-123">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="65a13-124">Beispielsweise beim Abfragen von `Blog`e, Sie enthalten `Posts` , und klicken Sie dann beide einschließen möchten die `Author` und `Tags` von der `Posts`.</span><span class="sxs-lookup"><span data-stu-id="65a13-124">For example, when querying `Blog`s, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="65a13-125">Zu diesem Zweck müssen Sie angeben, angefangen beim Stamm Pfad enthalten.</span><span class="sxs-lookup"><span data-stu-id="65a13-125">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="65a13-126">Beispielsweise `Blog -> Posts -> Author` und `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="65a13-126">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="65a13-127">Dies bedeutet nicht, dass erhalten Sie redundante Joins, in den meisten Fällen, die EF zu konsolidieren, werden, die Joins beim Generieren von SQL.</span><span class="sxs-lookup"><span data-stu-id="65a13-127">This does not mean you will get redundant joins, in most cases EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a><span data-ttu-id="65a13-128">Abgeleitete Typen enthalten</span><span class="sxs-lookup"><span data-stu-id="65a13-128">Include on derived types</span></span>

<span data-ttu-id="65a13-129">Zählen Sie aufeinander bezogene Daten in Navigationen, die nur für einen abgeleiteten Typ verwenden definiert `Include` und `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="65a13-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span> 

<span data-ttu-id="65a13-130">Betrachten Sie das folgende Modell:</span><span class="sxs-lookup"><span data-stu-id="65a13-130">Given the following model:</span></span>

```Csharp
    public class SchoolContext : DbContext
    {
        public DbSet<Person> People { get; set; }
        public DbSet<School> Schools { get; set; }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<School>().HasMany(s => s.Students).WithOne(s => s.School);
        }
    }

    public class Person
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }

    public class Student : Person
    {
        public School School { get; set; }
    }

    public class School
    {
        public int Id { get; set; }
        public string Name { get; set; }

        public List<Student> Students { get; set; }
    }
```

<span data-ttu-id="65a13-131">Inhalt der `School` Navigation für alle Personen, die Studenten kann vorzeitig über eine Reihe von Mustern geladen werden:</span><span class="sxs-lookup"><span data-stu-id="65a13-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

- <span data-ttu-id="65a13-132">Verwenden von cast</span><span class="sxs-lookup"><span data-stu-id="65a13-132">using cast</span></span>
  ```Csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- <span data-ttu-id="65a13-133">Mithilfe von `as` Operator</span><span class="sxs-lookup"><span data-stu-id="65a13-133">using `as` operator</span></span>
  ```Csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- <span data-ttu-id="65a13-134">Mithilfe der Überladung der `Include` , akzeptiert Parameter vom Typ `string`</span><span class="sxs-lookup"><span data-stu-id="65a13-134">using overload of `Include` that takes parameter of type `string`</span></span>
  ```Csharp
  context.People.Include("Student").ToList()
  ```

### <a name="ignored-includes"></a><span data-ttu-id="65a13-135">Ignoriert enthält</span><span class="sxs-lookup"><span data-stu-id="65a13-135">Ignored includes</span></span>

<span data-ttu-id="65a13-136">Wenn Sie die Abfrage ändern, sodass sie nicht mehr Instanzen des Entitätstyps, die zurückgibt mit der Abfrage wurde gestartet, werden die Operatoren Include ignoriert.</span><span class="sxs-lookup"><span data-stu-id="65a13-136">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="65a13-137">Im folgenden Beispiel werden die Include-Operatoren basierend auf den `Blog`, aber dann die `Select` Operator wird verwendet, so ändern Sie die Abfrage aus, um einen anonymen Typ zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="65a13-137">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="65a13-138">Die Include-Operatoren haben in diesem Fall keine Auswirkungen.</span><span class="sxs-lookup"><span data-stu-id="65a13-138">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="65a13-139">EF Core protokolliert standardmäßig eine Warnung bei enthalten Operatoren werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="65a13-139">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="65a13-140">Finden Sie unter [Protokollierung](../miscellaneous/logging.md) für Weitere Informationen zur Anzeige von Protokollausgaben.</span><span class="sxs-lookup"><span data-stu-id="65a13-140">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="65a13-141">Sie können das Verhalten ändern, wenn ein Operator Include ignoriert wird, entweder auslösen oder Unternehmen Sie nichts.</span><span class="sxs-lookup"><span data-stu-id="65a13-141">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="65a13-142">Dies erfolgt, wenn Sie die Optionen für den Kontext - in der Regel in einrichten `DbContext.OnConfiguring`, oder im `Startup.cs` bei Verwendung von ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="65a13-142">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="65a13-143">Explizites Laden</span><span class="sxs-lookup"><span data-stu-id="65a13-143">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="65a13-144">Diese Funktion wurde in der EF Core 1.1 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="65a13-144">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="65a13-145">Sie können explizit laden, eine Navigationseigenschaft über die `DbContext.Entry(...)` API.</span><span class="sxs-lookup"><span data-stu-id="65a13-145">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="65a13-146">Sie können auch explizit eine Navigationseigenschaft laden, durch Ausführen einer separaten Abfrage, die die verknüpften Entitäten zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="65a13-146">You can also explicitly load a navigation property by executing a seperate query that returns the related entities.</span></span> <span data-ttu-id="65a13-147">Wenn die änderungsnachverfolgung aktiviert ist, beim Laden einer Entität EF Core automatisch legen Sie die Navigationseigenschaften der neu geladenen workflowprozessinstanz zum Verweisen auf alle Entitäten, die bereits geladen, und legen Sie die Navigationseigenschaften der bereits geladene Entitäten zum Verweisen auf die Entität neu geladen.</span><span class="sxs-lookup"><span data-stu-id="65a13-147">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="65a13-148">Abfragen von verknüpften Entitäten</span><span class="sxs-lookup"><span data-stu-id="65a13-148">Querying related entities</span></span>

<span data-ttu-id="65a13-149">Außerdem erhalten Sie eine LINQ-Abfrage, die den Inhalt einer Navigationseigenschaft darstellt.</span><span class="sxs-lookup"><span data-stu-id="65a13-149">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="65a13-150">Dadurch können Sie Aktionen wie die aggregate-Operator über die verknüpften Entitäten ausführen, ohne sie in den Arbeitsspeicher laden ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="65a13-150">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="65a13-151">Sie können auch filtern, welche verknüpften Entitäten in den Arbeitsspeicher geladen werden.</span><span class="sxs-lookup"><span data-stu-id="65a13-151">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="65a13-152">Lazy Loading</span><span class="sxs-lookup"><span data-stu-id="65a13-152">Lazy loading</span></span>

> [!NOTE]  
> <span data-ttu-id="65a13-153">Diese Funktion wurde in der EF Core 2.1 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="65a13-153">This feature was introduced in EF Core 2.1.</span></span>

<span data-ttu-id="65a13-154">Die einfachste Methode zum Verwenden der verzögerten Laden wird durch die Installation der [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) Paket, und aktivieren es mit einem Aufruf von `UseLazyLoadingProxies`.</span><span class="sxs-lookup"><span data-stu-id="65a13-154">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="65a13-155">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="65a13-155">For example:</span></span>
```Csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
<span data-ttu-id="65a13-156">Oder bei der Verwendung von AddDbContext:</span><span class="sxs-lookup"><span data-stu-id="65a13-156">Or when using AddDbContext:</span></span>
```Csharp
    .AddDbContext<BloggingContext>(
        b => b.UseLazyLoadingProxies()
              .UseSqlServer(myConnectionString));
```
<span data-ttu-id="65a13-157">EF Core wird verzögerte Laden von für eine Navigationseigenschaft, die überschrieben werden können –, dann aktivieren, muss er `virtual` und für eine Klasse, die vom geerbt werden kann.</span><span class="sxs-lookup"><span data-stu-id="65a13-157">EF Core will then enable lazy-loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="65a13-158">In den folgenden Entitäten, z. B. die `Post.Blog` und `Blog.Posts` Navigationseigenschaften lazy geladen werden.</span><span class="sxs-lookup"><span data-stu-id="65a13-158">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>
```Csharp
public class Blog
{
    public int Id { get; set; }
    public string Name { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public virtual Blog Blog { get; set; }
}
```
### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="65a13-159">Lazy-laden, ohne Proxys</span><span class="sxs-lookup"><span data-stu-id="65a13-159">Lazy-loading without proxies</span></span>

<span data-ttu-id="65a13-160">Lazy Automatisches Laden von Proxys arbeiten möchten, indem Sie Räumen der `ILazyLoader` service in einer Entität, wie in beschrieben [Entitätstypkonstruktoren](../modeling/constructors.md).</span><span class="sxs-lookup"><span data-stu-id="65a13-160">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="65a13-161">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="65a13-161">For example:</span></span>
```Csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader?.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(ILazyLoader lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private ILazyLoader LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader?.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="65a13-162">Dadurch erfordert nicht das Entitätstypen werden von der geerbt oder Navigationseigenschaften virtuell sein, und erstellte mit Instanzen der Entität `new` verzögerte-einmal laden an einen Kontext angefügt.</span><span class="sxs-lookup"><span data-stu-id="65a13-162">This doesn't require entity types to be inherited from or navigation properties to be virtual and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="65a13-163">Allerdings muss einen Verweis auf die `ILazyLoader` Dienst, der Entitätstypen der EF-Core-Assembly verbindet.</span><span class="sxs-lookup"><span data-stu-id="65a13-163">However, it requires a reference to the `ILazyLoader` service, which couples entity types to the EF Core assembly.</span></span> <span data-ttu-id="65a13-164">Dieser Kern EF zu vermeiden, kann die `ILazyLoader.Load` Methode, um die Form eines Delegaten eingefügt werden.</span><span class="sxs-lookup"><span data-stu-id="65a13-164">To avoid this EF Core allows the `ILazyLoader.Load` method to be injected as a delegate.</span></span> <span data-ttu-id="65a13-165">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="65a13-165">For example:</span></span>
```Csharp
public class Blog
{
    private ICollection<Post> _posts;

    public Blog()
    {
    }

    private Blog(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }

    public ICollection<Post> Posts
    {
        get => LazyLoader?.Load(this, ref _posts);
        set => _posts = value;
    }
}

public class Post
{
    private Blog _blog;

    public Post()
    {
    }

    private Post(Action<object, string> lazyLoader)
    {
        LazyLoader = lazyLoader;
    }

    private Action<object, string> LazyLoader { get; set; }

    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog
    {
        get => LazyLoader?.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="65a13-166">Der Code oben verwendet eine `Load` Extension-Methode zum Erstellen von Delegaten ein wenig cleaner verwenden:</span><span class="sxs-lookup"><span data-stu-id="65a13-166">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>
```Csharp
public static class PocoLoadingExtensions
{
    public static TRelated Load<TRelated>(
        this Action<object, string> loader,
        object entity,
        ref TRelated navigationField,
        [CallerMemberName] string navigationName = null)
        where TRelated : class
    {
        loader?.Invoke(entity, navigationName);

        return navigationField;
    }
}
```
> [!NOTE]  
> <span data-ttu-id="65a13-167">Der-Parameter des Konstruktors für den Delegaten verzögertes Laden muss "LazyLoader" aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="65a13-167">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="65a13-168">Konfiguration, um einen anderen Namen, den diese geplant wurde für eine künftige Version verwenden.</span><span class="sxs-lookup"><span data-stu-id="65a13-168">Configuration to use a different name this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="65a13-169">Daten und Serialisierung</span><span class="sxs-lookup"><span data-stu-id="65a13-169">Related data and serialization</span></span>

<span data-ttu-id="65a13-170">Da EF Core wird automatisch ein Fixup-Navigationseigenschaften, Sie Zyklen in Ihre Objektdiagramm ergeben können.</span><span class="sxs-lookup"><span data-stu-id="65a13-170">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="65a13-171">Z. B. beim Laden eines Blogs und bezieht sich Beiträge in einem Blog Objekt führt, die eine Auflistung von Beiträgen verweist.</span><span class="sxs-lookup"><span data-stu-id="65a13-171">For example, Loading a blog and it's related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="65a13-172">Jedes dieser Beiträge weist einen Verweis zurück auf den Blog.</span><span class="sxs-lookup"><span data-stu-id="65a13-172">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="65a13-173">Einige serialisierungsframeworks lassen sich nicht auf solche Zyklen aus.</span><span class="sxs-lookup"><span data-stu-id="65a13-173">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="65a13-174">Json.NET wird z. B. die folgende Ausnahme ausgelöst, wenn eine Schleife festgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="65a13-174">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="65a13-175">Newtonsoft.Json.JsonSerializationException: Self-Service verweisen auf Schleife, die mit Typ 'MyApplication.Models.Blog' für Eigenschaft "Blog" erkannt.</span><span class="sxs-lookup"><span data-stu-id="65a13-175">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="65a13-176">Wenn Sie ASP.NET Core verwenden, können Sie Json.NET um Zyklen zu ignorieren, die im Objektdiagramm gefunden konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="65a13-176">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="65a13-177">Dies erfolgt in der `ConfigureServices(...)` Methode in `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="65a13-177">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```
