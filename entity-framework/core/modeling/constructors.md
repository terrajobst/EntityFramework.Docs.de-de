---
title: Entitätstypen mit Konstruktoren – EF Core
author: ajcvickers
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
uid: core/modeling/constructors
ms.openlocfilehash: 1b36197465fb9a6571a306d36eb1e9d885a5399e
ms.sourcegitcommit: 0cef7d448e1e47bdb333002e2254ed42d57b45b6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/29/2018
ms.locfileid: "43152464"
---
# <a name="entity-types-with-constructors"></a><span data-ttu-id="baa3c-102">Entitätstypen mit Konstruktoren</span><span class="sxs-lookup"><span data-stu-id="baa3c-102">Entity types with constructors</span></span>

> [!NOTE]  
> <span data-ttu-id="baa3c-103">Dieses Feature ist neu in EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="baa3c-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="baa3c-104">Ab EF Core 2.1, ist es jetzt möglich, definieren Sie einen Konstruktor mit Parametern und dass EF Core, die dieser Konstruktor aufgerufen, wenn Sie eine Instanz der Entität zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="baa3c-104">Starting with EF Core 2.1, it is now possible to define a constructor with parameters and have EF Core call this constructor when creating an instance of the entity.</span></span> <span data-ttu-id="baa3c-105">Parameter des Konstruktors an zugeordneten Eigenschaften gebunden werden können, oder auf verschiedene Arten von Diensten zu ermöglichen wie Verhalten lazy Loading.</span><span class="sxs-lookup"><span data-stu-id="baa3c-105">The constructor parameters can be bound to mapped properties, or to various kinds of services to facilitate behaviors like lazy-loading.</span></span>

> [!NOTE]  
> <span data-ttu-id="baa3c-106">Ab EF Core 2.1 ist alle Konstruktor-Bindung gemäß der Konvention.</span><span class="sxs-lookup"><span data-stu-id="baa3c-106">As of EF Core 2.1, all constructor binding is by convention.</span></span> <span data-ttu-id="baa3c-107">Konfiguration von bestimmten Konstruktoren verwenden, ist für eine künftige Version geplant.</span><span class="sxs-lookup"><span data-stu-id="baa3c-107">Configuration of specific constructors to use is planned for a future release.</span></span>

## <a name="binding-to-mapped-properties"></a><span data-ttu-id="baa3c-108">Binden an die zugeordnete Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="baa3c-108">Binding to mapped properties</span></span>

<span data-ttu-id="baa3c-109">Betrachten Sie ein typisches Modell für die Blog-Beitrag:</span><span class="sxs-lookup"><span data-stu-id="baa3c-109">Consider a typical Blog/Post model:</span></span>

``` csharp
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

<span data-ttu-id="baa3c-110">Wenn Instanzen dieser Typen von EF Core erstellt wird, wie z. B. für die Ergebnisse einer Abfrage, sie rufen Sie zuerst dem parameterlosen Standardkonstruktor und klicken Sie dann auf den Wert jeder Eigenschaft festgelegt, aus der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="baa3c-110">When EF Core creates instances of these types, such as for the results of a query, it will first call the default parameterless constructor and then set each property to the value from the database.</span></span> <span data-ttu-id="baa3c-111">Jedoch wenn EF Core mit einen parametrisierten Konstruktor gefunden Parameternamen und -Typen, die übereinstimmen zugeordnet Eigenschaften, und es den parametrisierten Konstruktor mit den Werten für diese Eigenschaften aufzurufen, und jede Eigenschaft nicht explizit festgelegt.</span><span class="sxs-lookup"><span data-stu-id="baa3c-111">However, if EF Core finds a parameterized constructor with parameter names and types that match those of mapped properties, then it will instead call the parameterized constructor with values for those properties and will not set each property explicitly.</span></span> <span data-ttu-id="baa3c-112">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="baa3c-112">For example:</span></span>

``` csharp
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
<span data-ttu-id="baa3c-113">Einige Punkte zu beachten:</span><span class="sxs-lookup"><span data-stu-id="baa3c-113">Some things to note:</span></span>
* <span data-ttu-id="baa3c-114">Nicht alle Eigenschaften müssen Konstruktorparameter.</span><span class="sxs-lookup"><span data-stu-id="baa3c-114">Not all properties need to have constructor parameters.</span></span> <span data-ttu-id="baa3c-115">Beispielsweise wird die Post.Content-Eigenschaft von einzelnen Konstruktorparameter nicht festgelegt, damit EF Core er nach dem Aufruf des Konstruktors, auf die übliche Weise festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="baa3c-115">For example, the Post.Content property is not set by any constructor parameter, so EF Core will set it after calling the constructor in the normal way.</span></span>
* <span data-ttu-id="baa3c-116">Der Parametertypen und muss mit dem Unterschied, dass Eigenschaften Pascal-Schreibweise verwendet werden, während die Parameter in Kamel-Schreibweise werden Eigenschaftstypen und -Namen übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="baa3c-116">The parameter types and names must match property types and names, except that properties can be Pascal-cased while the parameters are camel-cased.</span></span>
* <span data-ttu-id="baa3c-117">EF Core kann nicht festgelegt Navigationseigenschaften (z. B. Blog oder Ihre Beiträge, die oben genannten) über einen Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="baa3c-117">EF Core cannot set navigation properties (such as Blog or Posts above) using a constructor.</span></span>
* <span data-ttu-id="baa3c-118">Der Konstruktor kann als öffentlich, privat, oder eine beliebige andere zugriffsmöglichkeiten aufweisen.</span><span class="sxs-lookup"><span data-stu-id="baa3c-118">The constructor can be public, private, or have any other accessibility.</span></span>

### <a name="read-only-properties"></a><span data-ttu-id="baa3c-119">Schreibgeschützte Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="baa3c-119">Read-only properties</span></span>

<span data-ttu-id="baa3c-120">Nach Eigenschaften über den Konstruktor festgelegt werden können einige Schreibschutz sinnvoll.</span><span class="sxs-lookup"><span data-stu-id="baa3c-120">Once properties are being set via the constructor it can make sense to make some of them read-only.</span></span> <span data-ttu-id="baa3c-121">EF Core unterstützt, aber es gibt einige Dinge achten:</span><span class="sxs-lookup"><span data-stu-id="baa3c-121">EF Core supports this, but there are some things to look out for:</span></span>
* <span data-ttu-id="baa3c-122">Eigenschaften ohne Setter werden gemäß der Konvention nicht zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="baa3c-122">Properties without setters are not mapped by convention.</span></span> <span data-ttu-id="baa3c-123">(Dies ist zum Zuordnen von Eigenschaften, die nicht, wie z. B. berechnete Eigenschaften zugeordnet werden soll.)</span><span class="sxs-lookup"><span data-stu-id="baa3c-123">(Doing so tends to map properties that should not be mapped, such as computed properties.)</span></span>
* <span data-ttu-id="baa3c-124">Die Verwendung von automatisch generierten Schlüsselwerte erfordert eine wichtige Eigenschaft, die Lese-/ Schreibzugriff, da der Schlüsselwert durch den Generator für Zugriffsschlüssel festgelegt werden, wenn neue Entitäten einfügen muss.</span><span class="sxs-lookup"><span data-stu-id="baa3c-124">Using automatically generated key values requires a key property that is read-write, since the key value needs to be set by the key generator when inserting new entities.</span></span>

<span data-ttu-id="baa3c-125">Eine einfache Möglichkeit, diese Probleme vermeiden werden private Setter verwendet.</span><span class="sxs-lookup"><span data-stu-id="baa3c-125">An easy way to avoid these things is to use private setters.</span></span> <span data-ttu-id="baa3c-126">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="baa3c-126">For example:</span></span>
``` csharp
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
<span data-ttu-id="baa3c-127">EF Core sieht eine Eigenschaft mit einem privaten Setter als Lese-/ Schreibzugriff, d. h., die alle Eigenschaften wie zuvor zugeordnet sind, und der Schlüssel kann immer noch vom Speicher generierte werden.</span><span class="sxs-lookup"><span data-stu-id="baa3c-127">EF Core sees a property with a private setter as read-write, which means that all properties are mapped as before and the key can still be store-generated.</span></span>

<span data-ttu-id="baa3c-128">Eine Alternative zur Verwendung von private Setter, werden Eigenschaften wirklich schreibgeschützt machen, und fügen weitere explizite Zuordnung in "onmodelcreating" ab.</span><span class="sxs-lookup"><span data-stu-id="baa3c-128">An alternative to using private setters is to make properties really read-only and add more explicit mapping in OnModelCreating.</span></span> <span data-ttu-id="baa3c-129">Einige Eigenschaften können auch vollständig entfernt und durch nur Felder ersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="baa3c-129">Likewise, some properties can be removed completely and replaced with only fields.</span></span> <span data-ttu-id="baa3c-130">Betrachten Sie beispielsweise die folgenden Entitätstypen:</span><span class="sxs-lookup"><span data-stu-id="baa3c-130">For example, consider these entity types:</span></span>

``` csharp
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
<span data-ttu-id="baa3c-131">Und diese Konfiguration in "onmodelcreating":</span><span class="sxs-lookup"><span data-stu-id="baa3c-131">And this configuration in OnModelCreating:</span></span>
``` csharp
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
<span data-ttu-id="baa3c-132">Beachten Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="baa3c-132">Things to note:</span></span>
* <span data-ttu-id="baa3c-133">Der Schlüssel "Property" ist jetzt ein Feld.</span><span class="sxs-lookup"><span data-stu-id="baa3c-133">The key "property" is now a field.</span></span> <span data-ttu-id="baa3c-134">Es ist keiner `readonly` Feld, damit vom Speicher generierte Schlüssel verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="baa3c-134">It is not a `readonly` field so that store-generated keys can be used.</span></span>
* <span data-ttu-id="baa3c-135">Die anderen Eigenschaften sind schreibgeschützte Eigenschaften, die nur im Konstruktor festgelegt.</span><span class="sxs-lookup"><span data-stu-id="baa3c-135">The other properties are read-only properties set only in the constructor.</span></span>
* <span data-ttu-id="baa3c-136">Wenn der primäre Schlüsselwert ist immer nur, indem EF festlegen oder aus der Datenbank gelesen, dann besteht keine Notwendigkeit zum Einschließen in den Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="baa3c-136">If the primary key value is only ever set by EF or read from the database, then there is no need to include it in the constructor.</span></span> <span data-ttu-id="baa3c-137">Dies bewirkt, dass den Schlüssel "Property" als ein einfaches Feld und macht deutlich, dass sie nicht explizit festgelegt werden soll, wenn neue Blogs und Beiträgen zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="baa3c-137">This leaves the key "property" as a simple field and makes it clear that it should not be set explicitly when creating new blogs or posts.</span></span>

> [!NOTE]  
> <span data-ttu-id="baa3c-138">Dieser Code führt zu compilerwarnung "169" gibt an, dass das Feld nie verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="baa3c-138">This code will result in compiler warning '169' indicating that the field is never used.</span></span> <span data-ttu-id="baa3c-139">Dies kann ignoriert werden, da in der Praxis EF Core das Feld in einer Weise extralinguistic verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="baa3c-139">This can be ignored since in reality EF Core is using the field in an extralinguistic manner.</span></span>

## <a name="injecting-services"></a><span data-ttu-id="baa3c-140">Einfügen von Diensten</span><span class="sxs-lookup"><span data-stu-id="baa3c-140">Injecting services</span></span>

<span data-ttu-id="baa3c-141">EF Core kann auch "Dienste" in einen Entitätstyp-Konstruktor einfügen.</span><span class="sxs-lookup"><span data-stu-id="baa3c-141">EF Core can also inject "services" into an entity type's constructor.</span></span> <span data-ttu-id="baa3c-142">Beispielsweise kann die folgenden eingefügt werden:</span><span class="sxs-lookup"><span data-stu-id="baa3c-142">For example, the following can be injected:</span></span>
* <span data-ttu-id="baa3c-143">`DbContext` – die aktuelle Context-Instanz, die auch als Ihr abgeleiteter Typ von "DbContext" eingegeben werden kann</span><span class="sxs-lookup"><span data-stu-id="baa3c-143">`DbContext` - the current context instance, which can also be typed as your derived DbContext type</span></span>
* <span data-ttu-id="baa3c-144">`ILazyLoader` -der-Lazy Load-Dienst – finden Sie unter der [lazy Loading-Dokumentation](../querying/related-data.md) Weitere Details</span><span class="sxs-lookup"><span data-stu-id="baa3c-144">`ILazyLoader` - the lazy-loading service--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="baa3c-145">`Action<object, string>` -einen lazy Loading-Delegaten – finden Sie unter der [lazy Loading Dokumentation](../querying/related-data.md) Weitere Details</span><span class="sxs-lookup"><span data-stu-id="baa3c-145">`Action<object, string>` - a lazy-loading delegate--see the [lazy-loading documentation](../querying/related-data.md) for more details</span></span>
* <span data-ttu-id="baa3c-146">`IEntityType` -die EF Core-Metadaten, die dieser Entitätstyp zugeordnet</span><span class="sxs-lookup"><span data-stu-id="baa3c-146">`IEntityType` - the EF Core metadata associated with this entity type</span></span>

> [!NOTE]  
> <span data-ttu-id="baa3c-147">Ab EF Core 2.1 können nur Dienste, die von EF Core bekannte eingefügt werden.</span><span class="sxs-lookup"><span data-stu-id="baa3c-147">As of EF Core 2.1, only services known by EF Core can be injected.</span></span> <span data-ttu-id="baa3c-148">Unterstützung für das Einschleusen von Anwendungsdiensten ist für eine spätere Version in Betracht gezogen.</span><span class="sxs-lookup"><span data-stu-id="baa3c-148">Support for injecting application services is being considered for a future release.</span></span>

<span data-ttu-id="baa3c-149">Beispielsweise kann ein eingefügter "DbContext" verwendet werden, selektiv Zugriff auf die Datenbank Informationen zu verknüpften Entitäten zu erhalten, ohne Sie zu laden, alle.</span><span class="sxs-lookup"><span data-stu-id="baa3c-149">For example, an injected DbContext can be used to selectively access the database to obtain information about related entities without loading them all.</span></span> <span data-ttu-id="baa3c-150">Im folgenden Beispiel wird dies verwendet, um die Anzahl an Beiträgen, die in einem Blog zu erhalten, ohne Sie zu laden die Beiträge:</span><span class="sxs-lookup"><span data-stu-id="baa3c-150">In the example below this is used to obtain the number of posts in a blog without loading the posts:</span></span>

``` csharp
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
<span data-ttu-id="baa3c-151">Einige Punkte zu beachten Sie dazu zu erhalten:</span><span class="sxs-lookup"><span data-stu-id="baa3c-151">A few things to notice about this:</span></span>
* <span data-ttu-id="baa3c-152">Der Konstruktor ist privat, da immer nur durch von EF Core Aufruf ist und eine andere öffentlicher Konstruktor zur allgemeinen Verwendung vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="baa3c-152">The constructor is private, since it is only ever called by EF Core, and there is another public constructor for general use.</span></span>
* <span data-ttu-id="baa3c-153">Der Code, der mit dem eingefügten Dienst (d. h. der Kontext) ist dafür defensive wird `null` um Fälle zu behandeln, in denen EF Core ist nicht das Erstellen die Instanz.</span><span class="sxs-lookup"><span data-stu-id="baa3c-153">The code using the injected service (that is, the context) is defensive against it being `null` to handle cases where EF Core is not creating the instance.</span></span>
* <span data-ttu-id="baa3c-154">Da Service in einer Lese-/Schreibzugriff-Eigenschaft gespeichert wird werden sie zurückgesetzt werden, wenn die Entität an eine neue Kontextinstanz angefügt ist.</span><span class="sxs-lookup"><span data-stu-id="baa3c-154">Because service is stored in a read/write property it will be reset when the entity is attached to a new context instance.</span></span>

> [!WARNING]  
> <span data-ttu-id="baa3c-155">Einfügen von "DbContext" wie folgt, wird häufig als Antimuster betrachtet, da die Entitätstypen direkt mit EF Core verkoppelt.</span><span class="sxs-lookup"><span data-stu-id="baa3c-155">Injecting the DbContext like this is often considered an anti-pattern since it couples your entity types directly to EF Core.</span></span> <span data-ttu-id="baa3c-156">Vor der Verwendung von Dienst-Injection wie folgt alle Optionen sorgfältig prüfen.</span><span class="sxs-lookup"><span data-stu-id="baa3c-156">Carefully consider all options before using service injection like this.</span></span>
