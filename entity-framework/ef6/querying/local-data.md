---
title: Lokale Daten – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2eda668b-1e5d-487d-9a8c-0e3beef03fcb
ms.openlocfilehash: 400b9e1337edac1b9fa4f0ec9e1384ca58aa2fbc
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490453"
---
# <a name="local-data"></a>Lokale Daten
Ausführen einer LINQ-Abfrage direkt für ein "DbSet" wird immer eine Abfrage an die Datenbank senden können, aber Sie die Daten, die derzeit im Arbeitsspeicher mithilfe der DbSet.Local-Eigenschaft. Sie können auch die zusätzliche Daten, die von die EF nachverfolgt zugreifen, zu Ihrer Entitäten mit dem die "DbContext.Entry" und DbContext.ChangeTracker.Entries-Methode. Die in diesem Thema dargestellten Techniken gelten jeweils für Modelle, die mit Code First und dem EF-Designer erstellt wurden.  

## <a name="using-local-to-look-at-local-data"></a>Verwenden Sie lokale betrachten Sie die lokalen Daten  

Die lokale Eigenschaft von "DbSet" bietet einfachen Zugriff auf die Entitäten der Gruppe, die derzeit vom Kontext nachverfolgt wird und nicht als gelöscht gekennzeichnet wurden. Zugriff auf die lokale Eigenschaft bewirkt nie, dass eine Abfrage an die Datenbank gesendet werden. Dies bedeutet, dass sie in der Regel verwendet wird, nachdem bereits eine Abfrage ausgeführt wurde. Die Load-Erweiterungsmethode kann zum Ausführen einer Abfrage so, dass der Kontext verfolgt, die Ergebnisse werden nach verwendet werden. Zum Beispiel:  

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

Wenn wir zwei Blogs in der Datenbank - "ADO.NET-Blog" mit einem BlogId 1- und "Visual Studio-Blog" mit einem BlogId 2 mussten konnten wir erwarten, dass die folgende Ausgabe:  

```  
In Local:
Found 0: My New Blog with state Added
Found 2: The Visual Studio Blog with state Unchanged

In DbSet query:
Found 1: ADO.NET Blog with state Deleted
Found 2: The Visual Studio Blog with state Unchanged
```  

Dies verdeutlicht die drei Punkte:  

- Neue Blog "Meine neuen Blog" ist in der lokalen Auflistung enthalten, auch wenn sie noch nicht mit der Datenbank gespeichert wurde. Dieser Blog verfügt über einen Primärschlüssel 0 (null) aus, da die Datenbank eine tatsächliche Taste für die Entität noch nicht generiert wurde.  
- "ADO.NET Blog" ist nicht in der lokalen Auflistung enthalten, auch wenn es immer noch vom Kontext nachverfolgt wird. Dies ist, da wir es aus und kennzeichnen es als gelöscht "DbSet" entfernt.  
- Wenn "DbSet" verwendet wird, um eine Abfrage auszuführen im Blog zur Löschung (ADO.NET-Blog) gekennzeichnet in den Ergebnissen enthalten ist, und die neue Blog (Blog zur meine neue), das noch nicht mit der Datenbank gespeichert wurde, ist nicht in den Ergebnissen enthalten. Dies ist daran, dass "DbSet" eine Abfrage für die Datenbank führt und die Ergebnisse immer widerspiegeln, was in der Datenbank ist.  

## <a name="using-local-to-add-and-remove-entities-from-the-context"></a>Verwenden Sie lokale hinzufügen und Entfernen von Entitäten aus dem Kontext  

Gibt zurück, die lokale Eigenschaft auf "DbSet" eine [ObservableCollection](https://msdn.microsoft.com/library/ms668604.aspx) mit Ereignissen verknüpft, mit dem Inhalt der Kontext synchron bleibt. Dies bedeutet, dass die Entitäten können hinzugefügt oder entfernt aus der lokalen Auflistung oder "DbSet". Dies bedeutet auch, dass in der lokalen Auflistung mit diesen Entitäten aktualisiert führt Abfragen, die neue Entitäten in den Kontext zu bieten. Zum Beispiel:  

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

Angenommen, Sie hatten wir ein paar Beiträge markiert mit 'Entity Framework' und 'asp.net"die Ausgabe kann etwa wie folgt aussehen:  

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

Dies verdeutlicht die drei Punkte:  

- Die neue Bereitstellung "EF Neuigkeiten in", die hinzugefügt wurde der lokalen Auflistung durch den Kontext im Zustand Added nachverfolgt wird. Es wird daher in der Datenbank eingefügt werden, wenn SaveChanges aufgerufen wird.  
- Der Beitrag, der aus der lokalen Auflistung (EF Beginners Guide) entfernt wurde, ist nun markiert, in den Kontext als gelöscht. Es wird daher aus der Datenbank gelöscht werden, wenn SaveChanges aufgerufen wird.  
- Die zusätzliche Post (Handbuch für Einsteiger ASP.NET), die in den Kontext mit der zweiten Abfrage geladen wird automatisch der lokalen Auflistung hinzugefügt werden.  

Eine letzte wichtige lokale beachtende liegt, die es handelt sich um eine ObservableCollection-Leistung nicht für eine große Anzahl von Entitäten ist. Aus diesem Grund Proxyproblemen mit Tausenden von Entitäten im Kontext Ihrer es ratsam, verwenden Sie lokale möglicherweise nicht.  

## <a name="using-local-for-wpf-data-binding"></a>Verwenden Sie lokale für WPF-Datenbindung  

Die lokale Eigenschaft auf "DbSet" kann direkt für die Datenbindung in einer WPF-Anwendung verwendet werden, da es sich um eine Instanz der ObservableCollection handelt. Wie in den vorherigen Abschnitten beschrieben, die Dies bedeutet, es automatisch synchron werden, mit dem Inhalt des Kontexts gehalten wird und der Inhalt des Kontexts automatisch werden, gehalten Sie synchron werden, damit. Beachten Sie, dass Sie benötigen, um der lokalen Auflistung mit Daten für die es gibt nichts zu binden, da lokale nie eine Datenbankabfrage löst vorab zu füllen.  

Dies ist keiner geeigneten Stelle für eine vollständige WPF-Bindung Datenbeispiel, jedoch werden die Schlüsselelemente:  

- Einrichten der Bindungsquelle  
- Binden Sie es auf die lokale Eigenschaft Ihrer Gruppe  
- Füllen Sie die lokalen mithilfe einer Abfrage mit der Datenbank.  

## <a name="wpf-binding-to-navigation-properties"></a>WPF-Datenbindung zu Navigationseigenschaften  

Beim Ausführen unter Umständen Master/Detail-Daten, die Bindung, die Sie die Detailansicht an eine Navigationseigenschaft einer Ihrer Entitäten binden möchten. Damit dies funktioniert eine einfache Möglichkeit ist die Verwendung eine ObservableCollection für die Navigationseigenschaft. Zum Beispiel:  

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

## <a name="using-local-to-clean-up-entities-in-savechanges"></a>Verwenden Sie lokale zum Bereinigen von Entitäten in "SaveChanges"  

In den meisten Fällen werden Entitäten, die von einer Navigationseigenschaft entfernt nicht automatisch markiert werden in den Kontext als gelöscht. Z. B. Wenn Sie eine Post-Objekt aus der Auflistung Blog.Posts entfernen, und klicken Sie dann die Sendung werden nicht automatisch gelöscht werden, wenn SaveChanges aufgerufen wird. Bei Bedarf gelöscht werden müssen Sie Sie dann diese verbleibende Entitäten suchen und markieren Sie sie als vor dem Aufruf von "SaveChanges" oder als Teil einer überschriebenen "SaveChanges" gelöscht. Zum Beispiel:  

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

Der obige Code verwendet die lokale Auflistung finden Sie alle Beiträge und markiert alle, die keinen Blog-Verweis verfügen, als gelöscht. Die ToList-Aufrufs ist erforderlich, da andernfalls die Auflistung von entfernen geändert wird aufgerufen werden, während es aufgelistet wird. In den meisten anderen Fällen können Sie direkt für die lokale Eigenschaft Abfragen, ohne zuerst mithilfe von "ToList".  

## <a name="using-local-and-tobindinglist-for-windows-forms-data-binding"></a>Mit der lokalen und ToBindingList für Windows Forms-Datenbindung  

Windows Forms unterstützt keine uneingeschränkt Datenbindung ObservableCollection direkt verwenden. Allerdings immer noch können die "DbSet" Local-Eigenschaft für die Datenbindung Sie die Vorteile, die für die in den vorherigen Abschnitten beschrieben werden. Erfolgt dies durch die Erweiterungsmethode ToBindingList erstellt eine [IBindingList](https://msdn.microsoft.com/library/system.componentmodel.ibindinglist.aspx) Implementierung, die von der lokalen ObservableCollection unterstützt.  

Dies ist keiner geeigneten Stelle für eine vollständige Windows Forms-Bindung Datenbeispiel, jedoch werden die Schlüsselelemente:  

- Einrichten der Bindungsquelle ein Objekt  
- Binden Sie es auf die lokale Eigenschaft aus, der den Satz mit Local.ToBindingList()  
- Füllen Sie lokal mit einer Abfrage mit der Datenbank  

## <a name="getting-detailed-information-about-tracked-entities"></a>Abrufen von ausführliche Informationen zu nachverfolgten Entitäten  

Viele der Beispiele in dieser Reihe verwenden die Entry-Methode, um eine "dbentityentry"-Instanz für eine Entität zurückzugeben. Dieser eintragsobjekt fungiert dann als Ausgangspunkt für das Sammeln von Informationen über die Entität wie z. B. ihres aktuellen Zustands sowie zum Ausführen von Vorgängen für die Entität wie z. B. das explizite Laden von einer verknüpften Entität.  

Die Einträge-Methoden zurückgeben "dbentityentry"-Objekte für viele oder alle Entitäten, die vom Kontext nachverfolgt wird. Dadurch können Sie zum Sammeln von Informationen oder Ausführen von Vorgängen für viele Entitäten statt nur einen einzelnen Eintrag. Zum Beispiel:  

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

Sie werden bemerken, werden wir eine Klasse erstellen und eines Smartcardlesers eingeführt, in dem Beispiel – beide Klassen implementieren die IPerson-Schnittstelle.  

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

Nehmen wir an, dass wir die folgenden Daten in der Datenbank zu erhalten:

Blog mit BlogId = 1 "und" Name = "ADO.NET Blog"  
Blog mit BlogId = 2 "und" Name = "Im Visual Studio-Blog"  
Blog mit BlogId = 3 "und" Name = ".NET Framework-Blog"  
Erstellen mit AutorID = 1 "und" Name = "Joe Bloggs"  
Reader mit ReaderId = 1 "und" Name = "John Doe"  

Die Ausgabe der Ausführung des Codes wäre:  

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

Diese Beispiele veranschaulichen einige Punkte:  

- Die Einträge-Methoden geben alle Zustände, einschließlich gelöschter Einträge für Entitäten zurück. Vergleichen Sie diese Option, um lokale die schließt gelöschte Entitäten.  
- Einträge für alle Entitätstypen werden zurückgegeben, wenn die Einträge nicht generische Methode verwendet wird. Wenn die Einträge der generischen Methode verwendet wird werden Einträge nur für Entitäten zurückgegeben, die Instanzen des generischen Typs. Dies wurde weiter oben verwendet, um Einträge für alle Blogs zu erhalten. Es wurde auch verwendet, um Einträge für alle Entitäten zu erhalten, die IPerson zu implementieren. Dies zeigt, dass der generische Typ nicht unbedingt einen tatsächlichen Entitätstyp sein.  
- LINQ to Objects kann verwendet werden, um die zurückgegebenen Ergebnisse zu filtern. Dies wurde oben verwendet, um Entitäten eines beliebigen Typs zu finden, solange sie geändert werden.  

Beachten Sie, dass "dbentityentry"-Instanzen immer eine nicht-Null-Entität enthalten. Beziehung und Stub-Einträge werden nicht als "dbentityentry"-Instanzen dargestellt, daher keine Notwendigkeit besteht, für diese zu filtern.
