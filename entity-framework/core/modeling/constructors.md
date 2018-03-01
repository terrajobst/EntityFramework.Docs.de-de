---
title: "Entitätstypen mit Konstruktoren - EF Core"
author: ajcvickers
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
ms.technology: entity-framework-core
uid: core/modeling/constructors
ms.openlocfilehash: 2632488569c538a11c7a31a9a866d2fadb29eeb5
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="entity-types-with-constructors"></a><span data-ttu-id="33765-102">Entitätstypen mit Konstruktoren</span><span class="sxs-lookup"><span data-stu-id="33765-102">Entity types with constructors</span></span>

> [!NOTE]  
> <span data-ttu-id="33765-103">Dieses Feature ist neu in EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="33765-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="33765-104">Beginnend mit EF Core 2.1, ist es jetzt möglich, definieren Sie einen Konstruktor mit Parametern und EF Core Aufruf dieses Konstruktors beim Erstellen einer Instanz der Entität verfügen.</span><span class="sxs-lookup"><span data-stu-id="33765-104">Starting with EF Core 2.1, it is now possible to define a constructor with parameters and have EF Core call this constructor when creating an instance of the entity.</span></span> <span data-ttu-id="33765-105">Die Parameter des Konstruktors an zugeordneten Eigenschaften gebunden werden können, oder auf verschiedene Arten von Diensten zu ermöglichen like Verhaltensweisen verzögertes Laden.</span><span class="sxs-lookup"><span data-stu-id="33765-105">The constructor parameters can be bound to mapped properties, or to various kinds of services to facilitate behaviors like lazy-loading.</span></span>

> [!NOTE]  
> <span data-ttu-id="33765-106">Ab EF Core 2.1 ist alle Konstruktor-Bindung gemäß der Konvention.</span><span class="sxs-lookup"><span data-stu-id="33765-106">As of EF Core 2.1, all constructor binding is by convention.</span></span> <span data-ttu-id="33765-107">Konfiguration von bestimmten Konstruktoren zu verwenden, ist für eine zukünftige Version geplant.</span><span class="sxs-lookup"><span data-stu-id="33765-107">Configuration of specific constructors to use is planned for a future release.</span></span>

## <a name="binding-to-mapped-properties"></a><span data-ttu-id="33765-108">Binden an die zugeordnete Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="33765-108">Binding to mapped properties</span></span>

<span data-ttu-id="33765-109">Betrachten Sie ein typisches Modell für den Blogbeitrag/aus:</span><span class="sxs-lookup"><span data-stu-id="33765-109">Consider a typical Blog/Post model:</span></span>

```Csharp
public class Blog
{
    public int Id { get; set; }

    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public int Id { get; set; }
    
    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```

<span data-ttu-id="33765-110">EF Core Instanzen dieser Typen erstellt werden, werden z. B. für die Ergebnisse einer Abfrage zuerst parameterlose Standardkonstruktor aufrufen und legen Sie dann jede Eigenschaft mit dem Wert aus der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="33765-110">When EF Core creates instances of these types, such as for the results of a query, it will first call the default parameterless constructor and then set each property to the value from the database.</span></span> <span data-ttu-id="33765-111">Allerdings findet EF Kern ein parametrisierten Konstruktors mit Parameternamen und Typen, die übereinstimmen zugeordneten Eigenschaften, und es wird stattdessen rufen Sie die parametrisierten Konstruktor mit Werten für diese Eigenschaften und jede Eigenschaft nicht explizit festgelegt.</span><span class="sxs-lookup"><span data-stu-id="33765-111">However, if EF Core finds a parameterized constructor with parameter names and types that match those of mapped properties, then it will instead call the parameterized constructor with values for those properties and will not set each property explicitly.</span></span> <span data-ttu-id="33765-112">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="33765-112">For example:</span></span>

```Csharp
public class Blog
{
    public Blog(int id, string name, string author)
    {
        Id = id;
        Name = name;
        Author = author;
    }

    public int Id { get; set; }

    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public Post(int id, string title, DateTime postedOn)
    {
        Id = id;
        Title = title;
        PostedOn = postedOn;
    }

    public int Id { get; set; }

    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```
<span data-ttu-id="33765-113">Einige Aspekte zu beachten:</span><span class="sxs-lookup"><span data-stu-id="33765-113">Some things to note:</span></span>
* <span data-ttu-id="33765-114">Nicht alle Eigenschaften erforderlich Konstruktorparameter sind.</span><span class="sxs-lookup"><span data-stu-id="33765-114">Not all properties need to have constructor parameters.</span></span> <span data-ttu-id="33765-115">Beispielsweise ist die Post.Content-Eigenschaft nicht durch einen Konstruktorparameter festgelegt, damit EF Core er nach dem Aufruf des Konstruktors, auf die übliche Weise festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="33765-115">For example, the Post.Content property is not set by any constructor parameter, so EF Core will set it after calling the constructor in the normal way.</span></span>
* <span data-ttu-id="33765-116">Der Parametertypen und muss mit dem Unterschied, dass Eigenschaften Pascal-Schreibweise angegeben werden, während die Parameter in Kamel-Schreibweise werden Eigenschaftentypen und Namen übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="33765-116">The parameter types and names must match property types and names, except that properties can be Pascal-cased while the parameters are camel-cased.</span></span>
* <span data-ttu-id="33765-117">EF Core Navigationseigenschaften (z. B. Blog oder Ihre Beiträge, die oben genannten) festlegen kann nicht mit einem Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="33765-117">EF Core cannot set navigation properties (such as Blog or Posts above) using a constructor.</span></span>
* <span data-ttu-id="33765-118">Der Konstruktor kann öffentliche, private, oder eine beliebige andere zugreifbarkeit besitzen.</span><span class="sxs-lookup"><span data-stu-id="33765-118">The constructor can be public, private, or have any other accessibility.</span></span>

### <a name="read-only-properties"></a><span data-ttu-id="33765-119">Schreibschutzeigenschaften</span><span class="sxs-lookup"><span data-stu-id="33765-119">Read-only properties</span></span>

<span data-ttu-id="33765-120">Nach Eigenschaften über den Konstruktor festgelegt werden, kann es sinnvoll, von denen einige schreibgeschützt machen.</span><span class="sxs-lookup"><span data-stu-id="33765-120">Once properties are being set via the constructor it can make sense to make some of them read-only.</span></span> <span data-ttu-id="33765-121">EF Core unterstützt, aber es gibt einige Dinge, um nach zu suchen:</span><span class="sxs-lookup"><span data-stu-id="33765-121">EF Core supports this, but there are some things to look out for:</span></span>
* <span data-ttu-id="33765-122">Eigenschaften ohne Getter werden gemäß der Konvention nicht zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="33765-122">Properties without getters are not mapped by convention.</span></span> <span data-ttu-id="33765-123">(Auf diese Weise tendenziell Eigenschaften zugeordnet werden, die nicht, z. B. berechnete Eigenschaften zugeordnet werden soll.)</span><span class="sxs-lookup"><span data-stu-id="33765-123">(Doing so tends to map properties that should not be mapped, such as computed properties.)</span></span>
* <span data-ttu-id="33765-124">Die Verwendung von automatisch generierten Schlüsselwerte erfordert eine Schlüsseleigenschaft, die Lese-/ Schreibzugriff, da der Schlüsselwert muss von der Schlüssel-Generator festgelegt werden, wenn Sie neue Entitäten einfügen.</span><span class="sxs-lookup"><span data-stu-id="33765-124">Using automatically generated key values requires a key property that is read-write, since the key value needs to be set by the key generator when inserting new entities.</span></span>

<span data-ttu-id="33765-125">Eine einfache Möglichkeit, vermeiden Sie die folgenden Schritte ist die Verwendung von privaten Settern für Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="33765-125">An easy way to avoid these things is to use private setters.</span></span> <span data-ttu-id="33765-126">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="33765-126">For example:</span></span>
```Csharp
public class Blog
{
    public Blog(int id, string name, string author)
    {
        Id = id;
        Name = name;
        Author = author;
    }

    public int Id { get; private set; }

    public string Name { get; private set; }
    public string Author { get; private set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public Post(int id, string title, DateTime postedOn)
    {
        Id = id;
        Title = title;
        PostedOn = postedOn;
    }

    public int Id { get; private set; }

    public string Title { get; private set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; private set; }

    public Blog Blog { get; set; }
}

```
<span data-ttu-id="33765-127">EF Core sieht eine Eigenschaft mit einem privaten Setter mit Lese-/ Schreibzugriff, d. h., die alle Eigenschaften wie zuvor zugeordnet sind, und der Schlüssel kann immer noch vom Speicher generierte werden.</span><span class="sxs-lookup"><span data-stu-id="33765-127">EF Core sees a property with a private setter as read-write, which means that all properties are mapped as before and the key can still be store-generated.</span></span>

<span data-ttu-id="33765-128">Eine Alternative zur Verwendung von privaten Setter ist wirklich schreibgeschützte Eigenschaften machen und mehr explizite Zuordnung in der OnModelCreating hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="33765-128">An alternative to using private setters is to make properties really read-only and add more explicit mapping in OnModelCreating.</span></span> <span data-ttu-id="33765-129">Einige Eigenschaften können ebenfalls vollständig entfernt und durch nur Felder ersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="33765-129">Likewise, some properties can be removed completely and replaced with only fields.</span></span> <span data-ttu-id="33765-130">Betrachten Sie beispielsweise die folgenden Entitätstypen:</span><span class="sxs-lookup"><span data-stu-id="33765-130">For example, consider these entity types:</span></span>

```Csharp
public class Blog
{
    private int _id;

    public Blog(string name, string author)
    {
        Name = name;
        Author = author;
    }

    public string Name { get; }
    public string Author { get; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    private int _id;

    public Post(string title, DateTime postedOn)
    {
        Title = title;
        PostedOn = postedOn;
    }

    public string Title { get; }
    public string Content { get; set; }
    public DateTime PostedOn { get; }

    public Blog Blog { get; set; }
}
```
<span data-ttu-id="33765-131">Und diese Konfiguration in der OnModelCreating:</span><span class="sxs-lookup"><span data-stu-id="33765-131">And this configuration in OnModelCreating:</span></span>
```Csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>(
        b =>
        {
            b.HasKey("_id");
            b.Property(e => e.Author);
            b.Property(e => e.Name);
        });

    modelBuilder.Entity<Post>(
        b =>
        {
            b.HasKey("_id");
            b.Property(e => e.Title);
            b.Property(e => e.PostedOn);
        });
}
```
<span data-ttu-id="33765-132">Beachten Folgendes:</span><span class="sxs-lookup"><span data-stu-id="33765-132">Things to note:</span></span>
* <span data-ttu-id="33765-133">Der Schlüssel "Property" ist jetzt ein Feld.</span><span class="sxs-lookup"><span data-stu-id="33765-133">The key "property" is now a field.</span></span> <span data-ttu-id="33765-134">Es ist kein `readonly` Feld, damit vom Speicher generierte Schlüssel verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="33765-134">It is not a `readonly` field so that store-generated keys can be used.</span></span> 
* <span data-ttu-id="33765-135">Die anderen Eigenschaften sind schreibgeschützte Eigenschaften, die nur im Konstruktor festgelegt.</span><span class="sxs-lookup"><span data-stu-id="33765-135">The other properties are read-only properties set only in the constructor.</span></span>
* <span data-ttu-id="33765-136">Wenn der primäre Schlüssel-Wert ist immer nur von EF festgelegt oder aus der Datenbank gelesen, dann besteht keine Notwendigkeit zum Einschließen in den Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="33765-136">If the primary key value is only ever set by EF or read from the database, then there is no need to include it in the constructor.</span></span> <span data-ttu-id="33765-137">Dadurch bleibt den Schlüssel "Property" als einen einfachen Feldverweis und verdeutlichen, dass es nicht explizit soll beim neuen Blogs oder Beiträge zu erstellen festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="33765-137">This leaves the key "property" as a simple field and makes it clear that it should not be set explicitly when creating new blogs or posts.</span></span>

> [!NOTE]  
> <span data-ttu-id="33765-138">Dieser Code führt zu compilerwarnung "169" gibt an, dass das Feld nie verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="33765-138">This code will result in compiler warning '169' indicating that the field is never used.</span></span> <span data-ttu-id="33765-139">Dies kann ignoriert werden, da in der Realität EF Core automatisierbar extralinguistic Feld verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="33765-139">This can be ignored since in reality EF Core is using the field in an extralinguistic manner.</span></span>

## <a name="injecting-services"></a><span data-ttu-id="33765-140">Räumen Dienste</span><span class="sxs-lookup"><span data-stu-id="33765-140">Injecting services</span></span>

<span data-ttu-id="33765-141">EF Core kann auch "Dienste" in einen Entitätstyp Konstruktor einfügen.</span><span class="sxs-lookup"><span data-stu-id="33765-141">EF Core can also inject "services" into an entity type's constructor.</span></span> <span data-ttu-id="33765-142">Beispielsweise können die folgenden eingegeben werden:</span><span class="sxs-lookup"><span data-stu-id="33765-142">For example, the following can be injected:</span></span>
* <span data-ttu-id="33765-143">`DbContext` -der aktuelle Kontextinstanz, die auch als Ihre abgeleiteten DbContext-Typ eingegeben werden kann</span><span class="sxs-lookup"><span data-stu-id="33765-143">`DbContext` - the current context instance, which can also be typed as your derived DbContext type</span></span>
* <span data-ttu-id="33765-144">`ILazyLoader` -der verzögerten Laden-Dienst finden Sie unter der [verzögertes Laden Dokumentation](../querying/related-data.md) Weitere Details</span><span class="sxs-lookup"><span data-stu-id="33765-144">`ILazyLoader` - the lazy-loading service--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="33765-145">`Action<object, string>` -ein verzögertes Laden Delegat--finden Sie unter der [verzögertes Laden Dokumentation](../querying/related-data.md) Weitere Details</span><span class="sxs-lookup"><span data-stu-id="33765-145">`Action<object, string>` - a lazy-loading delegate--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="33765-146">`IEntityType` -Dieses Entitätstyps zugeordneten EF-Core-Metadaten</span><span class="sxs-lookup"><span data-stu-id="33765-146">`IEntityType` - the EF Core metadata associated with this entity type</span></span>

> [!NOTE]  
> <span data-ttu-id="33765-147">Zum Zeitpunkt der EF Core 2.1 können nur Dienste bekannt EF Core eingegeben werden.</span><span class="sxs-lookup"><span data-stu-id="33765-147">As of EF Core 2.1, only services known by EF Core can be injected.</span></span> <span data-ttu-id="33765-148">Unterstützung für Anwendungsdienste Räumen ist für eine künftige Version berücksichtigt wird.</span><span class="sxs-lookup"><span data-stu-id="33765-148">Support for injecting application services is being considered for a future release.</span></span>

<span data-ttu-id="33765-149">Beispielsweise kann ein eingefügter DbContext verwendet werden, selektiv Zugriff auf die Datenbank zum Abrufen von Informationen zu verknüpften Entitäten ohne Laden sie alle.</span><span class="sxs-lookup"><span data-stu-id="33765-149">For example, an injected DbContext can be used to selectively access the database to obtain information about related entities without loading them all.</span></span> <span data-ttu-id="33765-150">Im folgenden Beispiel wird dies verwendet, um die Anzahl der Einträge in einem Blog zu erhalten, ohne die Beiträgen zu laden:</span><span class="sxs-lookup"><span data-stu-id="33765-150">In the example below this is used to obtain the number of posts in a blog without loading the posts:</span></span>

```Csharp
public class Blog
{
    public Blog()
    {
    }

    private Blog(BloggingContext context)
    {
        Context = context;
    }

    private BloggingContext Context { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; set; }

    public int PostsCount
        => Posts?.Count
           ?? Context?.Set<Post>().Count(p => Id == EF.Property<int?>(p, "BlogId"))
           ?? 0;
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```
<span data-ttu-id="33765-151">Einige Punkte, die zu diesem beachten:</span><span class="sxs-lookup"><span data-stu-id="33765-151">A few things to notice about this:</span></span>
* <span data-ttu-id="33765-152">Der Konstruktor ist privat, da er ist nur einmal aufgerufen wird, indem EF Core und eine andere öffentliche Konstruktor für den allgemeinen Gebrauch vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="33765-152">The constructor is private, since it is only ever call by EF Core, and there is another public constructor for general use.</span></span>
* <span data-ttu-id="33765-153">Den Code mithilfe des eingefügten-Diensts (d. h. der Kontext) ist gegenüber diesem defensiven wird `null` Fälle gehandhabt werden, in denen EF Core ist die Instanz nicht erstellen.</span><span class="sxs-lookup"><span data-stu-id="33765-153">The code using the injected service (i.e. the context) is defensive against it being `null` to handle cases where EF Core is not creating the instance.</span></span>
* <span data-ttu-id="33765-154">Da Service in eine Lese-Schreib-Eigenschaft gespeichert ist wird sie zurückgesetzt werden, wenn die Entität an eine neue Kontextinstanz angefügt ist.</span><span class="sxs-lookup"><span data-stu-id="33765-154">Because service is stored in a read/write property it will be reset when the entity is attached to a new context instance.</span></span>

> [!WARNING]  
> <span data-ttu-id="33765-155">Räumen ' DbContext ' wie folgt häufig einer Antimuster gilt seit Ihrer Entitätstypen direkt mit EF Core verkoppelt.</span><span class="sxs-lookup"><span data-stu-id="33765-155">Injecting the DbContext like this is often considered an anti-pattern since it couples your entity types directly to EF Core.</span></span> <span data-ttu-id="33765-156">Bedenken Sie sorgfältig alle Optionen vor der Verwendung von Dienst-Injection wie folgt.</span><span class="sxs-lookup"><span data-stu-id="33765-156">Carefully consider all options before using service injection like this.</span></span>
