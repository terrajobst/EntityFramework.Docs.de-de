---
title: Arbeiten mit Status von Entitäten – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: acb27f46-3f3a-4179-874a-d6bea5d7120c
ms.openlocfilehash: ef0e8d5a5a9d66adab7046088c49d8cd472edc8a
ms.sourcegitcommit: e5f9ca4aa41e64141fa63a1e5fcf4d4775d67d24
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2018
ms.locfileid: "52899651"
---
# <a name="working-with-entity-states"></a>Arbeiten mit Status von Entitäten
In diesem Thema wird behandelt, das Hinzufügen und Anfügen von Entitäten an einen Kontext und wie Entity Framework diese während SaveChanges verarbeitet.
Entitätsframework übernimmt Überwachung, die der Zustand der Entitäten, während sie mit einem Kontext verbunden sind, aber in nicht verbundenen oder N-Tier-Szenarien Sie EF wissen, welcher Zustand Ihrer Entitäten können auf sein soll.
Die in diesem Thema dargestellten Techniken gelten jeweils für Modelle, die mit Code First und dem EF-Designer erstellt wurden.  

## <a name="entity-states-and-savechanges"></a>Status von Entitäten und "SaveChanges"

Eine Entität kann einen der fünf Status sein, wie durch die "EntityState"-Enumeration definiert. Folgende Status sind möglich:  

- Hinzugefügt: die Entität vom Kontext nachverfolgt wird, aber noch nicht in der Datenbank vorhanden  
- Unchanged: die Entität vom Kontext nachverfolgt wird, und in der Datenbank vorhanden ist und die Eigenschaftswerte nicht aus den Werten in der Datenbank geändert haben  
- Geänderte: die Entität vom Kontext nachverfolgt wird, und in der Datenbank vorhanden ist, und einige oder alle der Eigenschaftswerte geändert wurden  
- Gelöschte: die Entität vom Kontext nachverfolgt wird, und in der Datenbank vorhanden, aber wurde zum Löschen aus der Datenbank das nächste Mal, das aufgerufen wird, "SaveChanges" markiert  
- Getrennt: die Entität wird nicht vom Kontext nachverfolgt wird  

"SaveChanges" führt verschiedene Dinge für Entitäten in unterschiedlichen Zuständen:  

- Unveränderte Entitäten werden nicht von "SaveChanges" berührt werden. Updates werden nicht in der Datenbank für Entitäten in den Status "Unchanged" gesendet.  
- Hinzugefügt von Entitäten, die in die Datenbank eingefügt werden, und klicken Sie dann werden nicht verändert, wenn "SaveChanges" zurückgegeben.  
- Geänderte Entitäten werden in der Datenbank aktualisiert und dann werden nicht verändert, wenn "SaveChanges" zurückgegeben.  
- Gelöschte Entitäten aus der Datenbank gelöscht werden, und klicken Sie dann aus dem Kontext getrennt sind.  

Die folgenden Beispiele zeigen die Möglichkeiten, die in denen der Zustand einer Entität oder ein Diagramm für die Entität geändert werden kann.  

## <a name="adding-a-new-entity-to-the-context"></a>Hinzufügen einer neuen Entität in den Kontext  

Eine neue Entität kann in den Kontext hinzugefügt werden, indem Sie die Add-Methode für "DbSet" aufrufen.
Dies versetzt die Entität auf, in dem Status "Added", Sie dies bedeutet, dass es in der Datenbank das nächste Mal eingefügt wird, das "SaveChanges" aufgerufen wird.
Zum Beispiel:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Blogs.Add(blog);
    context.SaveChanges();
}
```  

Eine weitere Möglichkeit zum Hinzufügen einer neuen Entität in den Kontext ist, dessen Zustand Added zu ändern. Zum Beispiel:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Entry(blog).State = EntityState.Added;
    context.SaveChanges();
}
```  

Schließlich können Sie eine neue Entität in den Kontext hinzufügen, indem Sie binden es in einer anderen Entität, die bereits nachverfolgt wird.
Dies kann durch die auflistungsnavigationseigenschaft einer anderen Entität die neue Entität hinzugefügt oder durch Festlegen einer Verweiseigenschaft für die Navigation von einer anderen Entität, zeigen Sie auf die neue Entität sein. Zum Beispiel:  

``` csharp
using (var context = new BloggingContext())
{
    // Add a new User by setting a reference from a tracked Blog
    var blog = context.Blogs.Find(1);
    blog.Owner = new User { UserName = "johndoe1987" };

    // Add a new Post by adding to the collection of a tracked Blog
    blog.Posts.Add(new Post { Name = "How to Add Entities" });

    context.SaveChanges();
}
```  

Beachten Sie, die für alle diese Beispiele aus, wenn die Entität, die hinzuzufügenden Verweise auf andere Entitäten verfügt, die nicht noch klicken Sie dann diese neuen Entitäten nachverfolgt wird auch zum Kontext hinzugefügt werden und wird eingefügt werden in die Datenbank das nächste Mal, das "SaveChanges" aufgerufen wird.  

## <a name="attaching-an-existing-entity-to-the-context"></a>Eine vorhandene Entität anzufügen an den Kontext.  

Wenn man eine Entität, die Sie bereits wissen, in der Datenbank, aber die nicht derzeit vom Kontext nachverfolgt wird und Sie, den Kontext erkennen können, der die Entität, die mithilfe der Attach-Methode für "DbSet" Nachverfolgen vorhanden ist. Die Entität werden im Zustand Unchanged im Kontext. Zum Beispiel:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);

    // Do some more work...  

    context.SaveChanges();
}
```  

Beachten Sie, dass keine Änderungen in der Datenbank vorgenommen werden, werden wenn SaveChanges aufgerufen wird, ohne andere Änderungen an den angefügten Entität. Dies ist, da die Entität im Zustand Unchanged ist.  

Eine weitere Möglichkeit zum Anfügen einer vorhandenen Entität an den Kontext ist so ändern Sie den Zustand auf Unchanged. Zum Beispiel:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

Beachten Sie, dass für diesen beiden Beispielen verfügt die Entität angefügt werden Verweise auf andere Entitäten, die noch nicht verfolgt werden dann diese neuen Entitäten wird auch an der der Kontext, in dem Status "Unchanged".  

## <a name="attaching-an-existing-but-modified-entity-to-the-context"></a>Anfügen einer vorhandenen geänderte Entität an den Kontext  

Wenn man ist eine Entität, die bereits vorhanden, in der Datenbank jedoch, welche Änderungen möglicherweise vorgenommen wurden und Sie, dass den Kontext feststellen können, fügen Sie die Entität aus, und legen seinen Status auf "geändert".
Zum Beispiel:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Modified;

    // Do some more work...  

    context.SaveChanges();
}
```  

Wenn Sie den Status in "geändert" ändern, alle Eigenschaften der Entität als geändert markiert, und alle Eigenschaftswerte werden an die Datenbank gesendet werden, wenn SaveChanges aufgerufen wird.  

Beachten Sie, dass wenn die Entität, die angefügt werden Verweise auf andere Entitäten verfügt, die noch nicht nachverfolgt werden, klicken Sie dann diese neuen Entitäten wird an der der Kontext, in dem Status "Unchanged" – sie werden nicht automatisch durchgeführt "geändert".
Wenn Sie über mehrere Entitäten verfügen, die müssen "geändert" markiert werden, sollten Sie den Status für jede dieser Entitäten einzeln festlegen.  

## <a name="changing-the-state-of-a-tracked-entity"></a>Ändern des Status einer verfolgten Entität  

Sie können den Status einer Entität ändern, die bereits nachverfolgt wird durch Festlegen der State-Eigenschaft auf den Eintrag. Zum Beispiel:  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

Beachten Sie, dass aufrufen hinzufügen oder Anfügen einer Entität, das bereits nachverfolgt wird auch verwendet werden kann, den Entitätszustand ändern. Beispielsweise wird durch Aufrufen von Anfügen für eine Entität, die derzeit im Zustand Added wird seinen Zustand auf Unchanged geändert.  

## <a name="insert-or-update-pattern"></a>Einfügen oder Aktualisieren der Muster  

Ein häufiges Muster für einige Anwendungen ist eine Entität als neue (wodurch das Einfügen einer Datenbank) hinzufügen oder Anfügen einer Entität wie vorhandene und markieren Sie ihn als geändert (was in einem Datenbankupdate) je nach dem Wert des Primärschlüssels.
Beispielsweise ist bei Verwendung der Datenbank generierte Primärschlüsseln ganzer Zahlen als es üblich, eine Entität mit einem NULL-Schlüssel als neu und einer Entität mit einem NULL-Schlüssel wie vorhandene behandelt werden sollen.
Dieses Muster kann erreicht werden, durch Festlegen den Entitätszustand basierend auf dem Wert des Primärschlüssels zu überprüfen. Zum Beispiel:  

``` csharp
public void InsertOrUpdate(Blog blog)
{
    using (var context = new BloggingContext())
    {
        context.Entry(blog).State = blog.BlogId == 0 ?
                                   EntityState.Added :
                                   EntityState.Modified;

        context.SaveChanges();
    }
}
```  

Beachten Sie, dass wenn Sie den Status in "geändert" ändern, werden alle Eigenschaften der Entität als geändert markiert, und alle Eigenschaftswerte werden an die Datenbank gesendet werden, wenn SaveChanges aufgerufen wird.  
