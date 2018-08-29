---
title: Laden von verbundenen Entitäten – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: c8417e18-a2ee-499c-9ce9-2a48cc5b468a
ms.openlocfilehash: 091493f60c732573ca63adb8c65bc28606453d34
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995924"
---
# <a name="loading-related-entities"></a>Laden verwandter Entitäten
Entitätsframework unterstützt drei Möglichkeiten zum Laden verwandter Daten: Eager laden, lazy Loading und explizites Laden. Die in diesem Thema dargestellten Techniken gelten jeweils für Modelle, die mit Code First und dem EF-Designer erstellt wurden.  

## <a name="eagerly-loading"></a>Eager Loading geladen.  

Eager Loading ist der Prozess, bei dem eine Abfrage für einen Entitätstyp auch verknüpfte Entitäten als Teil der Abfrage wird geladen. Eager Loading erfolgt mithilfe der Include-Methode. Die folgenden Abfragen werden z. B. Blogs und alle Beiträge, die im Zusammenhang mit jedem Blog geladen.  

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

Beachten Sie, dass das Einschließen einer Erweiterungsmethode im System.Data.Entity-Namespace ist also stellen Sie sicher, dass Sie diesen Namespace verwenden.  

### <a name="eagerly-loading-multiple-levels"></a>Eager Loading Laden von mehreren Ebenen  

Es ist auch möglich, mehrere Ebenen von verknüpften Entitäten vorzeitig geladen werden. Die folgenden Abfragen sind Beispiele für für sowohl die Datensammlung als auch die verweisnavigationseigenschaften dazu.  

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

Beachten Sie, dass es nicht Filter derzeit möglich ist, welche verknüpften Entitäten geladen werden. Einschließen wird immer in alle verknüpften Entitäten anzuzeigen.  

## <a name="lazy-loading"></a>Lazy Loading  

Mit Lazy Loading steht der Prozess, bei dem eine Entität oder eine Auflistung von Entitäten automatisch aus der Datenbank erstmalig geladen wird, die eine Eigenschaft verweisen auf die Entität/Entitäten zugegriffen wird. Wenn Sie POCO-Entität-Typen zu verwenden, erfolgt die Lazy Load durch Erstellen von Instanzen der abgeleiteten Proxytypen, und klicken Sie dann überschreiben virtuelle Eigenschaften zum Hinzufügen des Hook laden. Beispielsweise werden bei Verwendung die Blog-Entity-Klasse, die nachstehend definiert Beiträge erstmalig geladen werden, die die Beiträge Navigationseigenschaft zugegriffen wird:  

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

### <a name="turn-lazy-loading-off-for-serialization"></a>Aktivieren von lazy loading-für die Serialisierung  

Lazy Loading und Serialisierung nicht auch kombinieren, und wenn Sie nicht vorsichtig sind kommen Sie können Abfragen für Ihre gesamte Datenbank, nur weil lazy Loading aktiviert ist. Die meisten Serialisierungsprogramme arbeiten, indem Sie den Zugriff auf jede Eigenschaft in einer Instanz eines Typs. Eigenschaftenzugriff löst das verzögerte Laden, damit weitere Entitäten serialisiert werden. Für diese Entitäten Eigenschaften zugegriffen werden, und sogar noch mehr Entitäten geladen. Es hat sich bewährt, deaktivieren lazy loading-deaktivieren, bevor Sie eine Entität zu serialisieren. Die folgenden Abschnitte zeigen, wie Sie dies tun.  

### <a name="turning-off-lazy-loading-for-specific-navigation-properties"></a>Lazy loading für Navigationseigenschaften für bestimmte deaktivieren  

Lazy Loading für die Sammlung von Beiträgen kann durch Festlegen der Beiträge Eigenschaft nicht virtuell deaktiviert werden:  

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

Laden die Beiträge Auflistung kann weiterhin erreicht werden über eager Loading (finden Sie unter *Eager Loading geladen* oben) oder die Load-Methode (finden Sie unter *explizites Laden* unten).  

### <a name="turn-off-lazy-loading-for-all-entities"></a>Deaktivieren Sie lazy loading für alle Entitäten  

Lazy Loading kann für alle Entitäten im Kontext deaktiviert werden, indem ein Kennzeichen festlege, für die Konfigurationseigenschaft. Zum Beispiel:  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.LazyLoadingEnabled = false;
    }
}
```  

Laden von verknüpften Entitäten kann weiterhin über eager Loading erreicht werden (finden Sie unter *Eager Loading geladen* oben) oder die Load-Methode (finden Sie unter *explizites Laden* unten).  

## <a name="explicitly-loading"></a>Explizites Laden  

Selbst bei lazy Loading deaktiviert ist es weiterhin möglich, verknüpfte Entitäten verzögert zu laden, aber ein expliziter Aufruf abgeschlossen werden muss. Zu diesem Zweck verwenden Sie die Load-Methode, auf die verknüpfte Entität Eintrag. Zum Beispiel:  

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

Beachten Sie, dass die Reference-Methode verwendet werden soll, wenn eine Entität über eine Navigationseigenschaft für eine andere einzelne Entität verfügt. Andererseits, sollte die Methode verwendet werden, wenn eine Entität über eine Navigationseigenschaft für eine Auflistung von anderen Entitäten verfügt.  

### <a name="applying-filters-when-explicitly-loading-related-entities"></a>Anwenden von Filtern beim verknüpfte Entitäten explizit zu laden  

Die Query-Methode ermöglicht den Zugriff auf die zugrunde liegende Abfrage, die Entity Framework beim Laden von verknüpften Entitäten verwenden. Sie können dann LINQ verwenden, zum Anwenden von Filtern für die Abfrage vor der Ausführung durch einen Aufruf einer Erweiterungsmethode für LINQ wie "ToList", Auslastung usw. Die Query-Methode mit sowohl Referenz-als auch Auflistung Navigationseigenschaften verwendet werden kann, aber es eignet sich am besten für Sammlungen, in dem er dient auch zum Laden nur einen Teil der Auflistung. Zum Beispiel:  

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

Bei Verwendung die Query-Methode empfiehlt es sich in der Regel um lazy loading für die Navigationseigenschaft zu deaktivieren. Dies ist, da andernfalls die gesamte Auflistung automatisch durch den Mechanismus für die Lazy Load geladen werden kann entweder vor oder nach die gefilterte Abfrage ausgeführt wurde.  

Beachten Sie, dass während die Beziehung als eine Zeichenfolge anstatt in einem Lambda-Ausdruck angegeben werden kann, das zurückgegebene IQueryable-Objekt nicht generischen, wenn eine Zeichenfolge verwendet wird und damit die Cast-Methode in der Regel benötigt wird, damit etwas Nützliches mit ihm durchgeführt werden kann.  

## <a name="using-query-to-count-related-entities-without-loading-them"></a>Verwenden die Abfrage um verknüpfte Entitäten zu zählen, ohne zu laden  

Manchmal ist es hilfreich zu wissen, wie viele Entitäten mit einer anderen Entität in der Datenbank verknüpft sind, ohne dass tatsächlich die Kosten für alle diese Entitäten werden geladen. Die Query-Methode, mit der Anzahl der LINQ-Methode kann dazu verwendet werden. Zum Beispiel:  

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
