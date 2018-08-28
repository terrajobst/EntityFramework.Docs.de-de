---
title: Lokale Daten – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 2eda668b-1e5d-487d-9a8c-0e3beef03fcb
ms.openlocfilehash: dac1a1de20398501c706b118443743d47970df17
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994273"
---
# <a name="local-data"></a><span data-ttu-id="bdda1-102">Lokale Daten</span><span class="sxs-lookup"><span data-stu-id="bdda1-102">Local Data</span></span>
<span data-ttu-id="bdda1-103">Ausführen einer LINQ-Abfrage direkt für ein "DbSet" wird immer eine Abfrage an die Datenbank senden können, aber Sie die Daten, die derzeit im Arbeitsspeicher mithilfe der DbSet.Local-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="bdda1-103">Running a LINQ query directly against a DbSet will always send a query to the database, but you can access the data that is currently in-memory using the DbSet.Local property.</span></span> <span data-ttu-id="bdda1-104">Sie können auch die zusätzliche Daten, die von die EF nachverfolgt zugreifen, zu Ihrer Entitäten mit dem die "DbContext.Entry" und DbContext.ChangeTracker.Entries-Methode.</span><span class="sxs-lookup"><span data-stu-id="bdda1-104">You can also access the extra information EF is tracking about your entities using the DbContext.Entry and DbContext.ChangeTracker.Entries methods.</span></span> <span data-ttu-id="bdda1-105">Die in diesem Thema dargestellten Techniken gelten jeweils für Modelle, die mit Code First und dem EF-Designer erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="bdda1-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="using-local-to-look-at-local-data"></a><span data-ttu-id="bdda1-106">Verwenden Sie lokale betrachten Sie die lokalen Daten</span><span class="sxs-lookup"><span data-stu-id="bdda1-106">Using Local to look at local data</span></span>  

<span data-ttu-id="bdda1-107">Die lokale Eigenschaft von "DbSet" bietet einfachen Zugriff auf die Entitäten der Gruppe, die derzeit vom Kontext nachverfolgt wird und nicht als gelöscht gekennzeichnet wurden.</span><span class="sxs-lookup"><span data-stu-id="bdda1-107">The Local property of DbSet provides simple access to the entities of the set that are currently being tracked by the context and have not been marked as Deleted.</span></span> <span data-ttu-id="bdda1-108">Zugriff auf die lokale Eigenschaft bewirkt nie, dass eine Abfrage an die Datenbank gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="bdda1-108">Accessing the Local property never causes a query to be sent to the database.</span></span> <span data-ttu-id="bdda1-109">Dies bedeutet, dass sie in der Regel verwendet wird, nachdem bereits eine Abfrage ausgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="bdda1-109">This means that it is usually used after a query has already been performed.</span></span> <span data-ttu-id="bdda1-110">Die Load-Erweiterungsmethode kann zum Ausführen einer Abfrage so, dass der Kontext verfolgt, die Ergebnisse werden nach verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="bdda1-110">The Load extension method can be used to execute a query so that the context tracks the results.</span></span> <span data-ttu-id="bdda1-111">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="bdda1-111">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load all blogs from the database into the context
    context.Blogs.Load();

    // Add a new blog to the context
    context.Blogs.Add(new Blog { Name = "My New Blog" });

    // Mark one of the existing blogs as Deleted
    context.Blogs.Remove(context.Blogs.Find(1));

    // Loop over the blogs in the context.
    Console.WriteLine("In Local: ");
    foreach (var blog in context.Blogs.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            blog.BlogId,  
            blog.Name,
            context.Entry(blog).State);
    }

    // Perform a query against the database.
    Console.WriteLine("\nIn DbSet query: ");
    foreach (var blog in context.Blogs)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            blog.BlogId,  
            blog.Name,
            context.Entry(blog).State);
    }
}
```  

<span data-ttu-id="bdda1-112">Wenn wir zwei Blogs in der Datenbank - "ADO.NET-Blog" mit einem BlogId 1- und "Visual Studio-Blog" mit einem BlogId 2 mussten konnten wir erwarten, dass die folgende Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="bdda1-112">If we had two blogs in the database - 'ADO.NET Blog' with a BlogId of 1 and 'The Visual Studio Blog' with a BlogId of 2 - we could expect the following output:</span></span>  

```  
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

<span data-ttu-id="bdda1-113">Dies verdeutlicht die drei Punkte:</span><span class="sxs-lookup"><span data-stu-id="bdda1-113">This illustrates three points:</span></span>  

- <span data-ttu-id="bdda1-114">Neue Blog "Meine neuen Blog" ist in der lokalen Auflistung enthalten, auch wenn sie noch nicht mit der Datenbank gespeichert wurde.</span><span class="sxs-lookup"><span data-stu-id="bdda1-114">The new blog 'My New Blog' is included in the Local collection even though it has not yet been saved to the database.</span></span> <span data-ttu-id="bdda1-115">Dieser Blog verfügt über einen Primärschlüssel 0 (null) aus, da die Datenbank eine tatsächliche Taste für die Entität noch nicht generiert wurde.</span><span class="sxs-lookup"><span data-stu-id="bdda1-115">This blog has a primary key of zero because the database has not yet generated a real key for the entity.</span></span>  
- <span data-ttu-id="bdda1-116">"ADO.NET Blog" ist nicht in der lokalen Auflistung enthalten, auch wenn es immer noch vom Kontext nachverfolgt wird.</span><span class="sxs-lookup"><span data-stu-id="bdda1-116">The 'ADO.NET Blog' is not included in the local collection even though it is still being tracked by the context.</span></span> <span data-ttu-id="bdda1-117">Dies ist, da wir es aus und kennzeichnen es als gelöscht "DbSet" entfernt.</span><span class="sxs-lookup"><span data-stu-id="bdda1-117">This is because we removed it from the DbSet thereby marking it as deleted.</span></span>  
- <span data-ttu-id="bdda1-118">Wenn "DbSet" verwendet wird, um eine Abfrage auszuführen im Blog zur Löschung (ADO.NET-Blog) gekennzeichnet in den Ergebnissen enthalten ist, und die neue Blog (Blog zur meine neue), das noch nicht mit der Datenbank gespeichert wurde, ist nicht in den Ergebnissen enthalten.</span><span class="sxs-lookup"><span data-stu-id="bdda1-118">When DbSet is used to perform a query the blog marked for deletion (ADO.NET Blog) is included in the results and the new blog (My New Blog) that has not yet been saved to the database is not included in the results.</span></span> <span data-ttu-id="bdda1-119">Dies ist daran, dass "DbSet" eine Abfrage für die Datenbank führt und die Ergebnisse immer widerspiegeln, was in der Datenbank ist.</span><span class="sxs-lookup"><span data-stu-id="bdda1-119">This is because DbSet is performing a query against the database and the results returned always reflect what is in the database.</span></span>  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a><span data-ttu-id="bdda1-120">Verwenden Sie lokale hinzufügen und Entfernen von Entitäten aus dem Kontext</span><span class="sxs-lookup"><span data-stu-id="bdda1-120">Using Local to add and remove entities from the context</span></span>  

<span data-ttu-id="bdda1-121">Gibt zurück, die lokale Eigenschaft auf "DbSet" eine [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) mit Ereignissen verknüpft, mit dem Inhalt der Kontext synchron bleibt.</span><span class="sxs-lookup"><span data-stu-id="bdda1-121">The Local property on DbSet returns an [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) with events hooked up such that it stays in sync with the contents of the context.</span></span> <span data-ttu-id="bdda1-122">Dies bedeutet, dass die Entitäten können hinzugefügt oder entfernt aus der lokalen Auflistung oder "DbSet".</span><span class="sxs-lookup"><span data-stu-id="bdda1-122">This means that entities can be added or removed from either the Local collection or the DbSet.</span></span> <span data-ttu-id="bdda1-123">Dies bedeutet auch, dass in der lokalen Auflistung mit diesen Entitäten aktualisiert führt Abfragen, die neue Entitäten in den Kontext zu bieten.</span><span class="sxs-lookup"><span data-stu-id="bdda1-123">It also means that queries that bring new entities into the context will result in the Local collection being updated with those entities.</span></span> <span data-ttu-id="bdda1-124">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="bdda1-124">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load some posts from the database into the context
    context.Posts.Where(p => p.Tags.Contains("entity-framework").Load();  

    // Get the local collection and make some changes to it
    var localPosts = context.Posts.Local;
    localPosts.Add(new Post { Name = "What's New in EF" });
    localPosts.Remove(context.Posts.Find(1));  

    // Loop over the posts in the context.
    Console.WriteLine("In Local after entity-framework query: ");
    foreach (var post in context.Posts.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            post.Id,  
            post.Title,
            context.Entry(post).State);
    }

    var post1 = context.Posts.Find(1);
    Console.WriteLine(
        "State of post 1: {0} is {1}",
        post1.Name,  
        context.Entry(post1).State);  

    // Query some more posts from the database
    context.Posts.Where(p => p.Tags.Contains("asp.net").Load();  

    // Loop over the posts in the context again.
    Console.WriteLine("\nIn Local after asp.net query: ");
    foreach (var post in context.Posts.Local)
    {
        Console.WriteLine(
            "Found {0}: {1} with state {2}",
            post.Id,  
            post.Title,
            context.Entry(post).State);
    }
}
```  

<span data-ttu-id="bdda1-125">Angenommen, Sie hatten wir ein paar Beiträge markiert mit 'Entity Framework' und 'asp.net"die Ausgabe kann etwa wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="bdda1-125">Assuming we had a few posts tagged with 'entity-framework' and 'asp.net' the output may look something like this:</span></span>  

```  
In Local after entity-framework query:
Found 3: EF Designer Basics with state Unchanged
Found 5: EF Code First Basics with state Unchanged
Found 0: What's New in EF with state Added
State of post 1: EF Beginners Guide is Deleted

In Local after asp.net query:
Found 3: EF Designer Basics with state Unchanged
Found 5: EF Code First Basics with state Unchanged
Found 0: What's New in EF with state Added
Found 4: ASP.NET Beginners Guide with state Unchanged
```  

<span data-ttu-id="bdda1-126">Dies verdeutlicht die drei Punkte:</span><span class="sxs-lookup"><span data-stu-id="bdda1-126">This illustrates three points:</span></span>  

- <span data-ttu-id="bdda1-127">Die neue Bereitstellung "EF Neuigkeiten in", die hinzugefügt wurde der lokalen Auflistung durch den Kontext im Zustand Added nachverfolgt wird.</span><span class="sxs-lookup"><span data-stu-id="bdda1-127">The new post 'What's New in EF' that was added to the Local collection becomes tracked by the context in the Added state.</span></span> <span data-ttu-id="bdda1-128">Es wird daher in der Datenbank eingefügt werden, wenn SaveChanges aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="bdda1-128">It will therefore be inserted into the database when SaveChanges is called.</span></span>  
- <span data-ttu-id="bdda1-129">Der Beitrag, der aus der lokalen Auflistung (EF Beginners Guide) entfernt wurde, ist nun markiert, in den Kontext als gelöscht.</span><span class="sxs-lookup"><span data-stu-id="bdda1-129">The post that was removed from the Local collection (EF Beginners Guide) is now marked as deleted in the context.</span></span> <span data-ttu-id="bdda1-130">Es wird daher aus der Datenbank gelöscht werden, wenn SaveChanges aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="bdda1-130">It will therefore be deleted from the database when SaveChanges is called.</span></span>  
- <span data-ttu-id="bdda1-131">Die zusätzliche Post (Handbuch für Einsteiger ASP.NET), die in den Kontext mit der zweiten Abfrage geladen wird automatisch der lokalen Auflistung hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="bdda1-131">The additional post (ASP.NET Beginners Guide) loaded into the context with the second query is automatically added to the Local collection.</span></span>  

<span data-ttu-id="bdda1-132">Eine letzte wichtige lokale beachtende liegt, die es handelt sich um eine ObservableCollection-Leistung nicht für eine große Anzahl von Entitäten ist.</span><span class="sxs-lookup"><span data-stu-id="bdda1-132">One final thing to note about Local is that because it is an ObservableCollection performance is not great for large numbers of entities.</span></span> <span data-ttu-id="bdda1-133">Aus diesem Grund Proxyproblemen mit Tausenden von Entitäten im Kontext Ihrer es ratsam, verwenden Sie lokale möglicherweise nicht.</span><span class="sxs-lookup"><span data-stu-id="bdda1-133">Therefore if you are dealing with thousands of entities in your context it may not be advisable to use Local.</span></span>  

## <a name="using-local-for-wpf-data-binding"></a><span data-ttu-id="bdda1-134">Verwenden Sie lokale für WPF-Datenbindung</span><span class="sxs-lookup"><span data-stu-id="bdda1-134">Using Local for WPF data binding</span></span>  

<span data-ttu-id="bdda1-135">Die lokale Eigenschaft auf "DbSet" kann direkt für die Datenbindung in einer WPF-Anwendung verwendet werden, da es sich um eine Instanz der ObservableCollection handelt.</span><span class="sxs-lookup"><span data-stu-id="bdda1-135">The Local property on DbSet can be used directly for data binding in a WPF application because it is an instance of ObservableCollection.</span></span> <span data-ttu-id="bdda1-136">Wie in den vorherigen Abschnitten beschrieben, die Dies bedeutet, es automatisch synchron werden, mit dem Inhalt des Kontexts gehalten wird und der Inhalt des Kontexts automatisch werden, gehalten Sie synchron werden, damit.</span><span class="sxs-lookup"><span data-stu-id="bdda1-136">As described in the previous sections this means that it will automatically stay in sync with the contents of the context and the contents of the context will automatically stay in sync with it.</span></span> <span data-ttu-id="bdda1-137">Beachten Sie, dass Sie benötigen, um der lokalen Auflistung mit Daten für die es gibt nichts zu binden, da lokale nie eine Datenbankabfrage löst vorab zu füllen.</span><span class="sxs-lookup"><span data-stu-id="bdda1-137">Note that you do need to pre-populate the Local collection with data for there to be anything to bind to since Local never causes a database query.</span></span>  

<span data-ttu-id="bdda1-138">Dies ist keiner geeigneten Stelle für eine vollständige WPF-Bindung Datenbeispiel, jedoch werden die Schlüsselelemente:</span><span class="sxs-lookup"><span data-stu-id="bdda1-138">This is not an appropriate place for a full WPF data binding sample but the key elements are:</span></span>  

- <span data-ttu-id="bdda1-139">Einrichten der Bindungsquelle</span><span class="sxs-lookup"><span data-stu-id="bdda1-139">Setup a binding source</span></span>  
- <span data-ttu-id="bdda1-140">Binden Sie es auf die lokale Eigenschaft Ihrer Gruppe</span><span class="sxs-lookup"><span data-stu-id="bdda1-140">Bind it to the Local property of your set</span></span>  
- <span data-ttu-id="bdda1-141">Füllen Sie die lokalen mithilfe einer Abfrage mit der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="bdda1-141">Populate Local using a query to the database.</span></span>  

## <a name="wpf-binding-to-navigation-properties"></a><span data-ttu-id="bdda1-142">WPF-Datenbindung zu Navigationseigenschaften</span><span class="sxs-lookup"><span data-stu-id="bdda1-142">WPF binding to navigation properties</span></span>  

<span data-ttu-id="bdda1-143">Beim Ausführen unter Umständen Master/Detail-Daten, die Bindung, die Sie die Detailansicht an eine Navigationseigenschaft einer Ihrer Entitäten binden möchten.</span><span class="sxs-lookup"><span data-stu-id="bdda1-143">If you are doing master/detail data binding you may want to bind the detail view to a navigation property of one of your entities.</span></span> <span data-ttu-id="bdda1-144">Damit dies funktioniert eine einfache Möglichkeit ist die Verwendung eine ObservableCollection für die Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="bdda1-144">An easy way to make this work is to use an ObservableCollection for the navigation property.</span></span> <span data-ttu-id="bdda1-145">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="bdda1-145">For example:</span></span>  

``` csharp
public class Blog
{
    private readonly ObservableCollection<Post> _posts =
        new ObservableCollection<Post>();

    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual ObservableCollection<Post> Posts
    {
        get { return _posts; }
    }
}
```  

## <a name="using-local-to-clean-up-entities-in-savechanges"></a><span data-ttu-id="bdda1-146">Verwenden Sie lokale zum Bereinigen von Entitäten in "SaveChanges"</span><span class="sxs-lookup"><span data-stu-id="bdda1-146">Using Local to clean up entities in SaveChanges</span></span>  

<span data-ttu-id="bdda1-147">In den meisten Fällen werden Entitäten, die von einer Navigationseigenschaft entfernt nicht automatisch markiert werden in den Kontext als gelöscht.</span><span class="sxs-lookup"><span data-stu-id="bdda1-147">In most cases entities removed from a navigation property will not be automatically marked as deleted in the context.</span></span> <span data-ttu-id="bdda1-148">Z. B. Wenn Sie eine Post-Objekt aus der Auflistung Blog.Posts entfernen, und klicken Sie dann die Sendung werden nicht automatisch gelöscht werden, wenn SaveChanges aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="bdda1-148">For example, if you remove a Post object from the Blog.Posts collection then that post will not be automatically deleted when SaveChanges is called.</span></span> <span data-ttu-id="bdda1-149">Bei Bedarf gelöscht werden müssen Sie Sie dann diese verbleibende Entitäten suchen und markieren Sie sie als vor dem Aufruf von "SaveChanges" oder als Teil einer überschriebenen "SaveChanges" gelöscht.</span><span class="sxs-lookup"><span data-stu-id="bdda1-149">If you need it to be deleted then you may need to find these dangling entities and mark them as deleted before calling SaveChanges or as part of an overridden SaveChanges.</span></span> <span data-ttu-id="bdda1-150">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="bdda1-150">For example:</span></span>  

``` csharp
public override int SaveChanges()
{
    foreach (var post in this.Posts.Local.ToList())
    {
        if (post.Blog == null)
        {
            this.Posts.Remove(post);
        }
    }

    return base.SaveChanges();
}
```  

<span data-ttu-id="bdda1-151">Der obige Code verwendet die lokale Auflistung finden Sie alle Beiträge und markiert alle, die keinen Blog-Verweis verfügen, als gelöscht.</span><span class="sxs-lookup"><span data-stu-id="bdda1-151">The code above uses the Local collection to find all posts and marks any that do not have a blog reference as deleted.</span></span> <span data-ttu-id="bdda1-152">Die ToList-Aufrufs ist erforderlich, da andernfalls die Auflistung von entfernen geändert wird aufgerufen werden, während es aufgelistet wird.</span><span class="sxs-lookup"><span data-stu-id="bdda1-152">The ToList call is required because otherwise the collection will be modified by the Remove call while it is being enumerated.</span></span> <span data-ttu-id="bdda1-153">In den meisten anderen Fällen können Sie direkt für die lokale Eigenschaft Abfragen, ohne zuerst mithilfe von "ToList".</span><span class="sxs-lookup"><span data-stu-id="bdda1-153">In most other situations you can query directly against the Local property without using ToList first.</span></span>  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a><span data-ttu-id="bdda1-154">Mit der lokalen und ToBindingList für Windows Forms-Datenbindung</span><span class="sxs-lookup"><span data-stu-id="bdda1-154">Using Local and ToBindingList for Windows Forms data binding</span></span>  

<span data-ttu-id="bdda1-155">Windows Forms unterstützt keine uneingeschränkt Datenbindung ObservableCollection direkt verwenden.</span><span class="sxs-lookup"><span data-stu-id="bdda1-155">Windows Forms does not support full fidelity data binding using ObservableCollection directly.</span></span> <span data-ttu-id="bdda1-156">Allerdings immer noch können die "DbSet" Local-Eigenschaft für die Datenbindung Sie die Vorteile, die für die in den vorherigen Abschnitten beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="bdda1-156">However, you can still use the DbSet Local property for data binding to get all the benefits described in the previous sections.</span></span> <span data-ttu-id="bdda1-157">Erfolgt dies durch die Erweiterungsmethode ToBindingList erstellt eine [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) Implementierung, die von der lokalen ObservableCollection unterstützt.</span><span class="sxs-lookup"><span data-stu-id="bdda1-157">This is achieved through the ToBindingList extension method which creates an [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) implementation backed by the Local ObservableCollection.</span></span>  

<span data-ttu-id="bdda1-158">Dies ist keiner geeigneten Stelle für eine vollständige Windows Forms-Bindung Datenbeispiel, jedoch werden die Schlüsselelemente:</span><span class="sxs-lookup"><span data-stu-id="bdda1-158">This is not an appropriate place for a full Windows Forms data binding sample but the key elements are:</span></span>  

- <span data-ttu-id="bdda1-159">Einrichten der Bindungsquelle ein Objekt</span><span class="sxs-lookup"><span data-stu-id="bdda1-159">Setup an object binding source</span></span>  
- <span data-ttu-id="bdda1-160">Binden Sie es auf die lokale Eigenschaft aus, der den Satz mit Local.ToBindingList()</span><span class="sxs-lookup"><span data-stu-id="bdda1-160">Bind it to the Local property of your set using Local.ToBindingList()</span></span>  
- <span data-ttu-id="bdda1-161">Füllen Sie lokal mit einer Abfrage mit der Datenbank</span><span class="sxs-lookup"><span data-stu-id="bdda1-161">Populate Local using a query to the database</span></span>  

## <a name="getting-detailed-information-about-tracked-entities"></a><span data-ttu-id="bdda1-162">Abrufen von ausführliche Informationen zu nachverfolgten Entitäten</span><span class="sxs-lookup"><span data-stu-id="bdda1-162">Getting detailed information about tracked entities</span></span>  

<span data-ttu-id="bdda1-163">Viele der Beispiele in dieser Reihe verwenden die Entry-Methode, um eine "dbentityentry"-Instanz für eine Entität zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="bdda1-163">Many of the examples in this series use the Entry method to return a DbEntityEntry instance for an entity.</span></span> <span data-ttu-id="bdda1-164">Dieser eintragsobjekt fungiert dann als Ausgangspunkt für das Sammeln von Informationen über die Entität wie z. B. ihres aktuellen Zustands sowie zum Ausführen von Vorgängen für die Entität wie z. B. das explizite Laden von einer verknüpften Entität.</span><span class="sxs-lookup"><span data-stu-id="bdda1-164">This entry object then acts as the starting point for gathering information about the entity such as its current state, as well as for performing operations on the entity such as explicitly loading a related entity.</span></span>  

<span data-ttu-id="bdda1-165">Die Einträge-Methoden zurückgeben "dbentityentry"-Objekte für viele oder alle Entitäten, die vom Kontext nachverfolgt wird.</span><span class="sxs-lookup"><span data-stu-id="bdda1-165">The Entries methods return DbEntityEntry objects for many or all entities being tracked by the context.</span></span> <span data-ttu-id="bdda1-166">Dadurch können Sie zum Sammeln von Informationen oder Ausführen von Vorgängen für viele Entitäten statt nur einen einzelnen Eintrag.</span><span class="sxs-lookup"><span data-stu-id="bdda1-166">This allows you to gather information or perform operations on many entities rather than just a single entry.</span></span> <span data-ttu-id="bdda1-167">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="bdda1-167">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Load some entities into the context
    context.Blogs.Load();
    context.Authors.Load();
    context.Readers.Load();

    // Make some changes
    context.Blogs.Find(1).Title = "The New ADO.NET Blog";
    context.Blogs.Remove(context.Blogs.Find(2));
    context.Authors.Add(new Author { Name = "Jane Doe" });
    context.Readers.Find(1).Username = "johndoe1987";

    // Look at the state of all entities in the context
    Console.WriteLine("All tracked entities: ");
    foreach (var entry in context.ChangeTracker.Entries())
    {
        Console.WriteLine(
            "Found entity of type {0} with state {1}",
            ObjectContext.GetObjectType(entry.Entity.GetType()).Name,
            entry.State);
    }

    // Find modified entities of any type
    Console.WriteLine("\nAll modified entities: ");
    foreach (var entry in context.ChangeTracker.Entries()
                              .Where(e => e.State == EntityState.Modified))
    {
        Console.WriteLine(
            "Found entity of type {0} with state {1}",
            ObjectContext.GetObjectType(entry.Entity.GetType()).Name,
            entry.State);
    }

    // Get some information about just the tracked blogs
    Console.WriteLine("\nTracked blogs: ");
    foreach (var entry in context.ChangeTracker.Entries<Blog>())
    {
        Console.WriteLine(
            "Found Blog {0}: {1} with original Name {2}",
            entry.Entity.BlogId,  
            entry.Entity.Name,
            entry.Property(p => p.Name).OriginalValue);
    }

    // Find all people (author or reader)
    Console.WriteLine("\nPeople: ");
    foreach (var entry in context.ChangeTracker.Entries<IPerson>())
    {
        Console.WriteLine("Found Person {0}", entry.Entity.Name);
    }
}
```  

<span data-ttu-id="bdda1-168">Sie werden bemerken, werden wir eine Klasse erstellen und eines Smartcardlesers eingeführt, in dem Beispiel – beide Klassen implementieren die IPerson-Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="bdda1-168">You'll notice we are introducing a Author and Reader class into the example - both of these classes implement the IPerson interface.</span></span>  

``` csharp
public class Author : IPerson
{
    public int AuthorId { get; set; }
    public string Name { get; set; }
    public string Biography { get; set; }
}

public class Reader : IPerson
{
    public int ReaderId { get; set; }
    public string Name { get; set; }
    public string Username { get; set; }
}

public interface IPerson
{
    string Name { get; }
}
```  

<span data-ttu-id="bdda1-169">Nehmen wir an, dass wir die folgenden Daten in der Datenbank zu erhalten:</span><span class="sxs-lookup"><span data-stu-id="bdda1-169">Let's assume we have the following data in the database:</span></span>

<span data-ttu-id="bdda1-170">Blog mit BlogId = 1 "und" Name = "ADO.NET Blog"</span><span class="sxs-lookup"><span data-stu-id="bdda1-170">Blog with BlogId = 1 and Name = 'ADO.NET Blog'</span></span>  
<span data-ttu-id="bdda1-171">Blog mit BlogId = 2 "und" Name = "Im Visual Studio-Blog"</span><span class="sxs-lookup"><span data-stu-id="bdda1-171">Blog with BlogId = 2 and Name = 'The Visual Studio Blog'</span></span>  
<span data-ttu-id="bdda1-172">Blog mit BlogId = 3 "und" Name = ".NET Framework-Blog"</span><span class="sxs-lookup"><span data-stu-id="bdda1-172">Blog with BlogId = 3 and Name = '.NET Framework Blog'</span></span>  
<span data-ttu-id="bdda1-173">Erstellen mit AutorID = 1 "und" Name = "Joe Bloggs"</span><span class="sxs-lookup"><span data-stu-id="bdda1-173">Author with AuthorId = 1 and Name = 'Joe Bloggs'</span></span>  
<span data-ttu-id="bdda1-174">Reader mit ReaderId = 1 "und" Name = "John Doe"</span><span class="sxs-lookup"><span data-stu-id="bdda1-174">Reader with ReaderId = 1 and Name = 'John Doe'</span></span>  

<span data-ttu-id="bdda1-175">Die Ausgabe der Ausführung des Codes wäre:</span><span class="sxs-lookup"><span data-stu-id="bdda1-175">The output from running the code would be:</span></span>  

```  
All tracked entities:
Found entity of type Blog with state Modified
Found entity of type Blog with state Deleted
Found entity of type Blog with state Unchanged
Found entity of type Author with state Unchanged
Found entity of type Author with state Added
Found entity of type Reader with state Modified

All modified entities:
Found entity of type Blog with state Modified
Found entity of type Reader with state Modified

Tracked blogs:
Found Blog 1: The New ADO.NET Blog with original Name ADO.NET Blog
Found Blog 2: The Visual Studio Blog with original Name The Visual Studio Blog
Found Blog 3: .NET Framework Blog with original Name .NET Framework Blog

People:
Found Person John Doe
Found Person Joe Bloggs
Found Person Jane Doe
```  

<span data-ttu-id="bdda1-176">Diese Beispiele veranschaulichen einige Punkte:</span><span class="sxs-lookup"><span data-stu-id="bdda1-176">These examples illustrate several points:</span></span>  

- <span data-ttu-id="bdda1-177">Die Einträge-Methoden geben alle Zustände, einschließlich gelöschter Einträge für Entitäten zurück.</span><span class="sxs-lookup"><span data-stu-id="bdda1-177">The Entries methods return entries for entities in all states, including Deleted.</span></span> <span data-ttu-id="bdda1-178">Vergleichen Sie diese Option, um lokale die schließt gelöschte Entitäten.</span><span class="sxs-lookup"><span data-stu-id="bdda1-178">Compare this to Local which excludes Deleted entities.</span></span>  
- <span data-ttu-id="bdda1-179">Einträge für alle Entitätstypen werden zurückgegeben, wenn die Einträge nicht generische Methode verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="bdda1-179">Entries for all entity types are returned when the non-generic Entries method is used.</span></span> <span data-ttu-id="bdda1-180">Wenn die Einträge der generischen Methode verwendet wird werden Einträge nur für Entitäten zurückgegeben, die Instanzen des generischen Typs.</span><span class="sxs-lookup"><span data-stu-id="bdda1-180">When the generic entries method is used entries are only returned for entities that are instances of the generic type.</span></span> <span data-ttu-id="bdda1-181">Dies wurde weiter oben verwendet, um Einträge für alle Blogs zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="bdda1-181">This was used above to get entries for all blogs.</span></span> <span data-ttu-id="bdda1-182">Es wurde auch verwendet, um Einträge für alle Entitäten zu erhalten, die IPerson zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="bdda1-182">It was also used to get entries for all entities that implement IPerson.</span></span> <span data-ttu-id="bdda1-183">Dies zeigt, dass der generische Typ nicht unbedingt einen tatsächlichen Entitätstyp sein.</span><span class="sxs-lookup"><span data-stu-id="bdda1-183">This demonstrates that the generic type does not have to be an actual entity type.</span></span>  
- <span data-ttu-id="bdda1-184">LINQ to Objects kann verwendet werden, um die zurückgegebenen Ergebnisse zu filtern.</span><span class="sxs-lookup"><span data-stu-id="bdda1-184">LINQ to Objects can be used to filter the results returned.</span></span> <span data-ttu-id="bdda1-185">Dies wurde oben verwendet, um Entitäten eines beliebigen Typs zu finden, solange sie geändert werden.</span><span class="sxs-lookup"><span data-stu-id="bdda1-185">This was used above to find entities of any type as long as they are modified.</span></span>  

<span data-ttu-id="bdda1-186">Beachten Sie, dass "dbentityentry"-Instanzen immer eine nicht-Null-Entität enthalten.</span><span class="sxs-lookup"><span data-stu-id="bdda1-186">Note that DbEntityEntry instances always contain a non-null Entity.</span></span> <span data-ttu-id="bdda1-187">Beziehung und Stub-Einträge werden nicht als "dbentityentry"-Instanzen dargestellt, daher keine Notwendigkeit besteht, für diese zu filtern.</span><span class="sxs-lookup"><span data-stu-id="bdda1-187">Relationship entries and stub entries are not represented as DbEntityEntry instances so there is no need to filter for these.</span></span>
