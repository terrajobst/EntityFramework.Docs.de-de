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
# <a name="loading-related-data"></a>Laden von verknüpften Daten

Entity Framework Core können Sie die Navigationseigenschaften zum Laden verknüpfter Entitäten im Modell zu verwenden. Es gibt drei allgemeine O/RM-Muster, die zum Laden von verknüpften Daten verwendet.
* **Unverzüglichem Laden** bedeutet, dass die verknüpften Daten aus der Datenbank als Teil der ursprünglichen Abfrage geladen werden.
* **Explizites Laden** bedeutet, dass die verwandten Daten explizit aus der Datenbank zu einem späteren Zeitpunkt geladen werden.
* **Verzögertes Laden** bedeutet, dass die verwandten Daten transparent aus der Datenbank geladen werden, wenn die Navigationseigenschaft zugegriffen wird.

> [!TIP]  
> Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) finden Sie auf GitHub.

## <a name="eager-loading"></a>Eager Loading

Sie können die `Include` Methode, um verwandte Daten angeben, die in den Ergebnissen enthalten sein. Im folgenden Beispiel müssen die Blogs, die in den Ergebnissen zurückgegeben werden ihre `Posts` Eigenschaft, die mit den zugehörigen Beiträgen aufgefüllt.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Entity Framework Core wird automatisch ein Fixup-Navigationseigenschaften für alle anderen Entitäten, die zuvor in die Kontextinstanz geladen wurden. Damit auch, wenn Sie die Daten für eine Navigationseigenschaft explizit nicht einschließen, die Eigenschaft kann noch immer aufgefüllt werden, wenn einige oder alle verknüpften Entitäten wurden zuvor geladen.


Sie können aufeinander bezogene Daten in mehreren Beziehungen in einer einzelnen Abfrage einschließen.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>Einschließlich mehrere Ebenen

Drilldown kann über Beziehungen zu mehrere Stufen der verknüpften Daten mithilfe der `ThenInclude` Methode. Im folgende Beispiel lädt alle Blogs, Verwandte Beiträge und der Autor jedes Post.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> Aktuelle Versionen von Visual Studio bieten eine falsche Code Abschluss Optionen und können dazu führen, dass das richtige Ausdrücke mit Syntaxfehler gekennzeichnet wird, bei Verwendung der `ThenInclude` Methode auf, nachdem eine auflistungsnavigationseigenschaft. Dies ist ein Symptom für ein IntelliSense-Fehler überwachten https://github.com/dotnet/roslyn/issues/8237. Sie können ruhig, um diese unbegründete Syntaxfehler zu ignorieren, solange der Code korrekt ist und erfolgreich kompiliert werden. 

Sie können mehrere Aufrufe verkettet `ThenInclude` zu fortfahren, einschließlich der weiteren Ebenen verknüpfter Daten.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

Sie können diese Option, um verwandte Daten aus mehreren Ebenen und mehrere Stämme in derselben Abfrage umfassen alle kombinieren.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

Möglicherweise sollen mehrere verknüpfte Entitäten für eine der Entitäten, die gerade enthalten ist. Beispielsweise beim Abfragen von `Blog`e, Sie enthalten `Posts` , und klicken Sie dann beide einschließen möchten die `Author` und `Tags` von der `Posts`. Zu diesem Zweck müssen Sie angeben, angefangen beim Stamm Pfad enthalten. Beispielsweise `Blog -> Posts -> Author` und `Blog -> Posts -> Tags`. Dies bedeutet nicht, dass erhalten Sie redundante Joins, in den meisten Fällen, die EF zu konsolidieren, werden, die Joins beim Generieren von SQL.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="include-on-derived-types"></a>Abgeleitete Typen enthalten

Zählen Sie aufeinander bezogene Daten in Navigationen, die nur für einen abgeleiteten Typ verwenden definiert `Include` und `ThenInclude`. 

Betrachten Sie das folgende Modell:

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

Inhalt der `School` Navigation für alle Personen, die Studenten kann vorzeitig über eine Reihe von Mustern geladen werden:

- Verwenden von cast
  ```Csharp
  context.People.Include(person => ((Student)person).School).ToList()
  ```

- Mithilfe von `as` Operator
  ```Csharp
  context.People.Include(person => (person as Student).School).ToList()
  ```

- Mithilfe der Überladung der `Include` , akzeptiert Parameter vom Typ `string`
  ```Csharp
  context.People.Include("Student").ToList()
  ```

### <a name="ignored-includes"></a>Ignoriert enthält

Wenn Sie die Abfrage ändern, sodass sie nicht mehr Instanzen des Entitätstyps, die zurückgibt mit der Abfrage wurde gestartet, werden die Operatoren Include ignoriert.

Im folgenden Beispiel werden die Include-Operatoren basierend auf den `Blog`, aber dann die `Select` Operator wird verwendet, so ändern Sie die Abfrage aus, um einen anonymen Typ zurückgegeben. Die Include-Operatoren haben in diesem Fall keine Auswirkungen.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

EF Core protokolliert standardmäßig eine Warnung bei enthalten Operatoren werden ignoriert. Finden Sie unter [Protokollierung](../miscellaneous/logging.md) für Weitere Informationen zur Anzeige von Protokollausgaben. Sie können das Verhalten ändern, wenn ein Operator Include ignoriert wird, entweder auslösen oder Unternehmen Sie nichts. Dies erfolgt, wenn Sie die Optionen für den Kontext - in der Regel in einrichten `DbContext.OnConfiguring`, oder im `Startup.cs` bei Verwendung von ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a>Explizites Laden

> [!NOTE]  
> Diese Funktion wurde in der EF Core 1.1 eingeführt.

Sie können explizit laden, eine Navigationseigenschaft über die `DbContext.Entry(...)` API.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

Sie können auch explizit eine Navigationseigenschaft laden, durch Ausführen einer separaten Abfrage, die die verknüpften Entitäten zurückgibt. Wenn die änderungsnachverfolgung aktiviert ist, beim Laden einer Entität EF Core automatisch legen Sie die Navigationseigenschaften der neu geladenen workflowprozessinstanz zum Verweisen auf alle Entitäten, die bereits geladen, und legen Sie die Navigationseigenschaften der bereits geladene Entitäten zum Verweisen auf die Entität neu geladen.

### <a name="querying-related-entities"></a>Abfragen von verknüpften Entitäten

Außerdem erhalten Sie eine LINQ-Abfrage, die den Inhalt einer Navigationseigenschaft darstellt.

Dadurch können Sie Aktionen wie die aggregate-Operator über die verknüpften Entitäten ausführen, ohne sie in den Arbeitsspeicher laden ausgeführt werden.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

Sie können auch filtern, welche verknüpften Entitäten in den Arbeitsspeicher geladen werden.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>Lazy Loading

> [!NOTE]  
> Diese Funktion wurde in der EF Core 2.1 eingeführt.

Die einfachste Methode zum Verwenden der verzögerten Laden wird durch die Installation der [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) Paket, und aktivieren es mit einem Aufruf von `UseLazyLoadingProxies`. Zum Beispiel:
```Csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseLazyLoadingProxies()
        .UseSqlServer(myConnectionString);
```
Oder bei der Verwendung von AddDbContext:
```Csharp
    .AddDbContext<BloggingContext>(
        b => b.UseLazyLoadingProxies()
              .UseSqlServer(myConnectionString));
```
EF Core wird verzögerte Laden von für eine Navigationseigenschaft, die überschrieben werden können –, dann aktivieren, muss er `virtual` und für eine Klasse, die vom geerbt werden kann. In den folgenden Entitäten, z. B. die `Post.Blog` und `Blog.Posts` Navigationseigenschaften lazy geladen werden.
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
### <a name="lazy-loading-without-proxies"></a>Lazy-laden, ohne Proxys

Lazy Automatisches Laden von Proxys arbeiten möchten, indem Sie Räumen der `ILazyLoader` service in einer Entität, wie in beschrieben [Entitätstypkonstruktoren](../modeling/constructors.md). Zum Beispiel:
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
Dadurch erfordert nicht das Entitätstypen werden von der geerbt oder Navigationseigenschaften virtuell sein, und erstellte mit Instanzen der Entität `new` verzögerte-einmal laden an einen Kontext angefügt. Allerdings muss einen Verweis auf die `ILazyLoader` Dienst, der Entitätstypen der EF-Core-Assembly verbindet. Dieser Kern EF zu vermeiden, kann die `ILazyLoader.Load` Methode, um die Form eines Delegaten eingefügt werden. Zum Beispiel:
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
Der Code oben verwendet eine `Load` Extension-Methode zum Erstellen von Delegaten ein wenig cleaner verwenden:
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
> Der-Parameter des Konstruktors für den Delegaten verzögertes Laden muss "LazyLoader" aufgerufen werden. Konfiguration, um einen anderen Namen, den diese geplant wurde für eine künftige Version verwenden.

## <a name="related-data-and-serialization"></a>Daten und Serialisierung

Da EF Core wird automatisch ein Fixup-Navigationseigenschaften, Sie Zyklen in Ihre Objektdiagramm ergeben können. Z. B. beim Laden eines Blogs und bezieht sich Beiträge in einem Blog Objekt führt, die eine Auflistung von Beiträgen verweist. Jedes dieser Beiträge weist einen Verweis zurück auf den Blog.

Einige serialisierungsframeworks lassen sich nicht auf solche Zyklen aus. Json.NET wird z. B. die folgende Ausnahme ausgelöst, wenn eine Schleife festgestellt wird.

> Newtonsoft.Json.JsonSerializationException: Self-Service verweisen auf Schleife, die mit Typ 'MyApplication.Models.Blog' für Eigenschaft "Blog" erkannt.

Wenn Sie ASP.NET Core verwenden, können Sie Json.NET um Zyklen zu ignorieren, die im Objektdiagramm gefunden konfigurieren. Dies erfolgt in der `ConfigureServices(...)` Methode in `Startup.cs`.

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
