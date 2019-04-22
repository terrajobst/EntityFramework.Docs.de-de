---
title: Laden zugehöriger Daten – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: e98e2e601203db7ea3d3344ddc7b7e0aff7f2143
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562519"
---
# <a name="loading-related-data"></a><span data-ttu-id="daf06-102">Laden zugehöriger Daten</span><span class="sxs-lookup"><span data-stu-id="daf06-102">Loading Related Data</span></span>

<span data-ttu-id="daf06-103">Mit Entity Framework Core können Sie die Navigationseigenschaften in Ihrem Modell zum Laden zugehöriger Entitäten verwenden.</span><span class="sxs-lookup"><span data-stu-id="daf06-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="daf06-104">Es gibt drei allgemeine O/RM-Muster für das Laden zugehöriger Daten.</span><span class="sxs-lookup"><span data-stu-id="daf06-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="daf06-105">**Eager Loading** bedeutet, dass die zugehörigen Daten als Teil der ersten Abfrage aus der Datenbank geladen werden.</span><span class="sxs-lookup"><span data-stu-id="daf06-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="daf06-106">**Explizites Laden** bedeutet, dass die zugehörigen Daten zu einem späteren Zeitpunkt explizit aus der Datenbank geladen werden.</span><span class="sxs-lookup"><span data-stu-id="daf06-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="daf06-107">**Lazy Loading** bedeutet, dass die zugehörigen Daten bei Zugriff auf die Navigationseigenschaft transparent aus der Datenbank geladen werden.</span><span class="sxs-lookup"><span data-stu-id="daf06-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]  
> <span data-ttu-id="daf06-108">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="daf06-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="daf06-109">Eager Loading</span><span class="sxs-lookup"><span data-stu-id="daf06-109">Eager loading</span></span>

<span data-ttu-id="daf06-110">Sie können mit der `Include`-Methode zugehörige Daten angeben, die in den Abfrageergebnissen enthalten sein sollen.</span><span class="sxs-lookup"><span data-stu-id="daf06-110">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="daf06-111">Im folgenden Beispiel weisen die in den Ergebnissen zurückgegebenen Blogs ihre `Posts`-Eigenschaft auf, die mit den zugehörigen Beiträgen aufgefüllt wurden.</span><span class="sxs-lookup"><span data-stu-id="daf06-111">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="daf06-112">Entity Framework Core korrigiert automatisch Navigationseigenschaften zur Korrektur für alle anderen Entitäten, die zuvor in die Kontextinstanz geladen wurden.</span><span class="sxs-lookup"><span data-stu-id="daf06-112">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="daf06-113">Auch wenn Sie die Daten für eine Navigationseigenschaft nicht explizit eingeschlossen haben, kann die Eigenschaft folglich immer noch aufgefüllt werden, wenn einige oder alle zugehörigen Entitäten zuvor geladen wurden.</span><span class="sxs-lookup"><span data-stu-id="daf06-113">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="daf06-114">Sie können zugehörigen Daten aus mehreren Beziehungen in einer einzelnen Abfrage einschließen.</span><span class="sxs-lookup"><span data-stu-id="daf06-114">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="daf06-115">Einschließen mehrerer Ebenen</span><span class="sxs-lookup"><span data-stu-id="daf06-115">Including multiple levels</span></span>

<span data-ttu-id="daf06-116">Sie können mit der `ThenInclude`-Methode einen Drilldown für Beziehungen ausführen, um mehrere Ebenen zugehöriger Daten einzuschließen.</span><span class="sxs-lookup"><span data-stu-id="daf06-116">You can drill down through relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="daf06-117">Im folgenden Beispiel werden sämtliche Blogs, die zugehörigen Beiträge und die Autoren der einzelnen Beiträge geladen.</span><span class="sxs-lookup"><span data-stu-id="daf06-117">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> <span data-ttu-id="daf06-118">Aktuelle Versionen von Visual Studio bieten falsche Codevervollständigungsoptionen an. Diese können verursachen, dass bei Verwendung der `ThenInclude`-Methode gemäß der Navigationseigenschaft einer Sammlung korrekte Ausdrücke mit Syntaxfehler gekennzeichnet werden.</span><span class="sxs-lookup"><span data-stu-id="daf06-118">Current versions of Visual Studio offer incorrect code completion options and can cause correct expressions to be flagged with syntax errors when using the `ThenInclude` method after a collection navigation property.</span></span> <span data-ttu-id="daf06-119">Dies ist ein Symptom für einen unter https://github.com/dotnet/roslyn/issues/8237 nachverfolgten IntelliSense-Fehler.</span><span class="sxs-lookup"><span data-stu-id="daf06-119">This is a symptom of an IntelliSense bug tracked at https://github.com/dotnet/roslyn/issues/8237.</span></span> <span data-ttu-id="daf06-120">Diese unbegründeten Syntaxfehler können ohne Bedenken ignoriert werden, solange der Code korrekt ist und erfolgreich kompiliert werden kann.</span><span class="sxs-lookup"><span data-stu-id="daf06-120">It is safe to ignore these spurious syntax errors as long as the code is correct and can be compiled successfully.</span></span> 

<span data-ttu-id="daf06-121">Sie können mehrere Aufrufe mit `ThenInclude` verbinden, um mit dem Einschließen weiterer Ebenen zugehöriger Daten fortzufahren.</span><span class="sxs-lookup"><span data-stu-id="daf06-121">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="daf06-122">Sie können all dies kombinieren, um zugehörige Daten aus mehreren Ebenen und mehreren Stämmen in derselben Abfrage einzuschließen.</span><span class="sxs-lookup"><span data-stu-id="daf06-122">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="daf06-123">Für eine der Entitäten, die eingeschlossen wird, sollten Sie mehrere zugehörige Entitäten einschließen.</span><span class="sxs-lookup"><span data-stu-id="daf06-123">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="daf06-124">Beispiel: Beim Abfragen von `Blogs` schließen Sie `Posts` ein und möchten anschließend `Author` und `Tags` von `Posts` einschließen.</span><span class="sxs-lookup"><span data-stu-id="daf06-124">For example, when querying `Blogs`, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="daf06-125">Hierzu müssen Sie die einzelnen Includepfade beginnend beim Stamm angeben.</span><span class="sxs-lookup"><span data-stu-id="daf06-125">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="daf06-126">Beispiel: `Blog -> Posts -> Author` und `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="daf06-126">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="daf06-127">Dies bedeutet nicht, dass Sie redundante Verknüpfungen erhalten. In den meisten Fällen konsolidiert EF die Verknüpfungen beim Generieren von SQL.</span><span class="sxs-lookup"><span data-stu-id="daf06-127">This does not mean you will get redundant joins; in most cases, EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a><span data-ttu-id="daf06-128">Einschließen in abgeleiteten Typen</span><span class="sxs-lookup"><span data-stu-id="daf06-128">Include on derived types</span></span>

<span data-ttu-id="daf06-129">Sie können zugehörige Daten aus Navigationen einschließen, die nur in einem abgeleiteten Typ mit `Include` und `ThenInclude` definiert werden.</span><span class="sxs-lookup"><span data-stu-id="daf06-129">You can include related data from navigations defined only on a derived type using `Include` and `ThenInclude`.</span></span> 

<span data-ttu-id="daf06-130">Betrachten Sie das folgende Modell:</span><span class="sxs-lookup"><span data-stu-id="daf06-130">Given the following model:</span></span>

```csharp
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

<span data-ttu-id="daf06-131">Inhalte der `School`-Navigation für alle Personen, die Studenten sind, können mit einer Reihe von Mustern vorzeitig geladen werden:</span><span class="sxs-lookup"><span data-stu-id="daf06-131">Contents of `School` navigation of all People who are Students can be eagerly loaded using a number of patterns:</span></span>

- <span data-ttu-id="daf06-132">Mit cast</span><span class="sxs-lookup"><span data-stu-id="daf06-132">using cast</span></span>
  ```csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- <span data-ttu-id="daf06-133">Mit dem Operator `as`</span><span class="sxs-lookup"><span data-stu-id="daf06-133">using `as` operator</span></span>
  ```csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- <span data-ttu-id="daf06-134">Mit einer Überladung von `Include`, die Parameter vom Typ `string` akzeptiert</span><span class="sxs-lookup"><span data-stu-id="daf06-134">using overload of `Include` that takes parameter of type `string`</span></span>
  ```csharp
  context.People.Include("School").ToList()
  ```

### <a name="ignored-includes"></a><span data-ttu-id="daf06-135">Ignorierte Include-Operatoren</span><span class="sxs-lookup"><span data-stu-id="daf06-135">Ignored includes</span></span>

<span data-ttu-id="daf06-136">Wenn Sie die Abfrage so ändern, dass keine Instanzen des Entitätstyps mehr zurückgegeben werden, mit dem die Abfrage gestartet wurde, werden die Include-Operatoren ignoriert.</span><span class="sxs-lookup"><span data-stu-id="daf06-136">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="daf06-137">Im folgenden Beispiel basieren die Include-Operatoren auf dem Operator `Blog`. Die Abfrage wird jedoch dann mit dem Operator `Select` geändert, damit ein anonymer Typ zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="daf06-137">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="daf06-138">In diesem Fall haben die Include-Operatoren keine Auswirkungen.</span><span class="sxs-lookup"><span data-stu-id="daf06-138">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="daf06-139">EF Core protokolliert standardmäßig eine Warnung, wenn Include-Operatoren ignoriert werden.</span><span class="sxs-lookup"><span data-stu-id="daf06-139">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="daf06-140">Weitere Informationen zum Anzeigen von Protokollierungsausgaben finden Sie unter [Protokollierung](../miscellaneous/logging.md).</span><span class="sxs-lookup"><span data-stu-id="daf06-140">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="daf06-141">Sie können das Verhalten, wenn ein Include-Operator ignoriert wird, auslösen oder nichts unternehmen.</span><span class="sxs-lookup"><span data-stu-id="daf06-141">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="daf06-142">Dies geschieht, wenn die Optionen für Ihren Kontext – normalerweise in `DbContext.OnConfiguring` oder `Startup.cs` – bei Verwendung von ASP.NET Core eingerichtet werden.</span><span class="sxs-lookup"><span data-stu-id="daf06-142">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="daf06-143">Explizites Laden</span><span class="sxs-lookup"><span data-stu-id="daf06-143">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="daf06-144">Dieses Feature wurde in EF Core 1.1 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="daf06-144">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="daf06-145">Sie können eine Navigationseigenschaft über die API `DbContext.Entry(...)` explizit laden.</span><span class="sxs-lookup"><span data-stu-id="daf06-145">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="daf06-146">Sie können eine Navigationseigenschaft auch explizit laden, indem Sie eine separate Abfrage ausführen, die die zugehörigen Entitäten zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="daf06-146">You can also explicitly load a navigation property by executing a separate query that returns the related entities.</span></span> <span data-ttu-id="daf06-147">Wenn die Änderungsnachverfolgung aktiviert ist, legt EF Core beim Laden einer Entität die Navigationseigenschaften der neu geladenen Entität automatisch so fest, dass diese auf bereits geladene Entitäten verweisen, und die Navigationseigenschaften bereits geladener Entitäten so, dass diese auf die neu geladene Entität verweisen.</span><span class="sxs-lookup"><span data-stu-id="daf06-147">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="daf06-148">Abfragen zugehöriger Entitäten</span><span class="sxs-lookup"><span data-stu-id="daf06-148">Querying related entities</span></span>

<span data-ttu-id="daf06-149">Sie können auch eine LINQ-Abfrage abrufen, welche die Inhalte einer Navigationseigenschaft darstellt.</span><span class="sxs-lookup"><span data-stu-id="daf06-149">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="daf06-150">Dadurch können Sie Aktionen, wie z.B. das Ausführen eines Aggregatoperators, über die zugehörigen Entitäten ausführen, ohne diese in den Speicher laden zu müssen.</span><span class="sxs-lookup"><span data-stu-id="daf06-150">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="daf06-151">Darüber hinaus können Sie filtern, welche zugehörigen Entitäten in den Speicher geladen werden.</span><span class="sxs-lookup"><span data-stu-id="daf06-151">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="daf06-152">Lazy Loading</span><span class="sxs-lookup"><span data-stu-id="daf06-152">Lazy loading</span></span>

> [!NOTE]  
> <span data-ttu-id="daf06-153">Dieses Feature wurde in EF Core 2.1 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="daf06-153">This feature was introduced in EF Core 2.1.</span></span>

<span data-ttu-id="daf06-154">Die einfachste Möglichkeit für die Verwendung von Lazy Loading besteht in der Installation des Pakets [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) und der Aktivierung dieses Pakets durch Aufrufen von `UseLazyLoadingProxies`.</span><span class="sxs-lookup"><span data-stu-id="daf06-154">The simplest way to use lazy-loading is by installing the [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) package and enabling it with a call to `UseLazyLoadingProxies`.</span></span> <span data-ttu-id="daf06-155">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="daf06-155">For example:</span></span>
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
<span data-ttu-id="daf06-156">Oder bei Verwendung von AddDbContext:</span><span class="sxs-lookup"><span data-stu-id="daf06-156">Or when using AddDbContext:</span></span>
```csharp
.AddDbContext<BloggingContext>(
    b => b.UseLazyLoadingProxies()
          .UseSqlServer(myConnectionString));
```
<span data-ttu-id="daf06-157">EF Core aktiviert Lazy Loading anschließend für beliebige überschreibbare Navigationseigenschaften – diese müssen `virtual` und in einer Klasse enthalten sein, aus der sie geerbt werden können.</span><span class="sxs-lookup"><span data-stu-id="daf06-157">EF Core will then enable lazy loading for any navigation property that can be overridden--that is, it must be `virtual` and on a class that can be inherited from.</span></span> <span data-ttu-id="daf06-158">In den folgenden Entitäten wird beispielsweise Lazy Loading für die Navigationseigenschaften `Post.Blog` und `Blog.Posts` durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="daf06-158">For example, in the following entities, the `Post.Blog` and `Blog.Posts` navigation properties will be lazy-loaded.</span></span>
```csharp
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
### <a name="lazy-loading-without-proxies"></a><span data-ttu-id="daf06-159">Lazy Loading ohne Proxys</span><span class="sxs-lookup"><span data-stu-id="daf06-159">Lazy loading without proxies</span></span>

<span data-ttu-id="daf06-160">Lazy Loading-Proxys funktionieren, indem der Dienst `ILazyLoader` in eine Entität eingefügt wird, wie unter [Entitätstypenkonstruktoren](../modeling/constructors.md) beschrieben.</span><span class="sxs-lookup"><span data-stu-id="daf06-160">Lazy-loading proxies work by injecting the `ILazyLoader` service into an entity, as described in [Entity Type Constructors](../modeling/constructors.md).</span></span> <span data-ttu-id="daf06-161">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="daf06-161">For example:</span></span>
```csharp
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
        get => LazyLoader.Load(this, ref _posts);
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
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="daf06-162">Es ist nicht erforderlich, dass aus Entitätstypen vererbt werden kann, oder dass Navigationseigenschaften „virtual“ sind. Mit `new` erstellte Entitätsinstanzen können Lazy Loading ausführen, sobald sie einem Kontext angefügt wurden.</span><span class="sxs-lookup"><span data-stu-id="daf06-162">This doesn't require entity types to be inherited from or navigation properties to be virtual, and allows entity instances created with `new` to lazy-load once attached to a context.</span></span> <span data-ttu-id="daf06-163">Erforderlich ist ein Verweis auf den `ILazyLoader`-Dienst, der im [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/)-Paket definiert ist.</span><span class="sxs-lookup"><span data-stu-id="daf06-163">However, it requires a reference to the `ILazyLoader` service, which is defined in the [Microsoft.EntityFrameworkCore.Abstractions](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Abstractions/) package.</span></span> <span data-ttu-id="daf06-164">Dieses Paket enthält einen minimalen Satz von Typen, sodass es kaum Auswirkungen davon gibt.</span><span class="sxs-lookup"><span data-stu-id="daf06-164">This package contains a minimal set of types so that there is very little impact in depending on it.</span></span> <span data-ttu-id="daf06-165">Um jedoch die Abhängigkeit von EF Core-Paketen in den Entitätstypen vollständig zu vermeiden, ist es möglich, die Methode `ILazyLoader.Load` als Delegat einzufügen.</span><span class="sxs-lookup"><span data-stu-id="daf06-165">However, to completely avoid depending on any EF Core packages in the entity types, it is possible to inject the `ILazyLoader.Load` method as a delegate.</span></span> <span data-ttu-id="daf06-166">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="daf06-166">For example:</span></span>
```csharp
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
        get => LazyLoader.Load(this, ref _posts);
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
        get => LazyLoader.Load(this, ref _blog);
        set => _blog = value;
    }
}
```
<span data-ttu-id="daf06-167">Der oben stehende Code verwendet eine `Load`-Erweiterungsmethode, um die Verwendung des Delegaten sauberer zu gestalten:</span><span class="sxs-lookup"><span data-stu-id="daf06-167">The code above uses a `Load` extension method to make using the delegate a bit cleaner:</span></span>
```csharp
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
> <span data-ttu-id="daf06-168">Der Konstruktorparameter für den Lazy Loading-Delegaten muss als „lazyLoader“ bezeichnet werden.</span><span class="sxs-lookup"><span data-stu-id="daf06-168">The constructor parameter for the lazy-loading delegate must be called "lazyLoader".</span></span> <span data-ttu-id="daf06-169">Eine Konfiguration für die Verwendung eines anderen Namens ist für ein zukünftiges Release geplant.</span><span class="sxs-lookup"><span data-stu-id="daf06-169">Configuration to use a different name than this is planned for a future release.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="daf06-170">Zugehörige Daten und Serialisierung</span><span class="sxs-lookup"><span data-stu-id="daf06-170">Related data and serialization</span></span>

<span data-ttu-id="daf06-171">Da EF Core Navigationseigenschaften automatisch korrigiert, können sich in Ihrem Objektgraph Zyklen ergeben.</span><span class="sxs-lookup"><span data-stu-id="daf06-171">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="daf06-172">Das Laden eines Blogs und der zugehörigen Beiträge führt beispielsweise zu einem Blogobjekt, das auf eine Sammlung von Beiträgen verweist.</span><span class="sxs-lookup"><span data-stu-id="daf06-172">For example, loading a blog and its related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="daf06-173">Die einzelnen Beiträge verweisen wieder auf den Blog.</span><span class="sxs-lookup"><span data-stu-id="daf06-173">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="daf06-174">Einige Serialisierungsframeworks lassen solche Zyklen nicht zu.</span><span class="sxs-lookup"><span data-stu-id="daf06-174">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="daf06-175">Json.NET löst beispielsweise folgende Ausnahme aus, wenn ein Zyklus erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="daf06-175">For example, Json.NET will throw the following exception if a cycle is encountered.</span></span>

> <span data-ttu-id="daf06-176">Newtonsoft.Json.JsonSerializationException: Für die Eigenschaft „Blog“ mit dem Typ „MyApplication.Models.Blog“ wurde eine auf sich selbst verweisende Schleife erkannt.</span><span class="sxs-lookup"><span data-stu-id="daf06-176">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="daf06-177">Wenn Sie ASP.NET Core verwenden, können Sie Json.NET so konfigurieren, dass im Objektgraph gefundene Zyklen ignoriert werden.</span><span class="sxs-lookup"><span data-stu-id="daf06-177">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="daf06-178">Dies erfolgt in der Methode `ConfigureServices(...)` in `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="daf06-178">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

```csharp
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
