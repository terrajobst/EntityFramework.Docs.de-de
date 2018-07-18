---
title: Unformatierte SQL-Abfragen – EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 9e1ee76e-2499-408c-81e8-9b6c5d1945a0
caps.latest.revision: 3
ms.openlocfilehash: 1d968604cfa500784c4699b0923512572a06d773
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121132"
---
# <a name="raw-sql-queries"></a>Unformatierte SQL-Abfragen
Entitätsframework können Sie Abfragen mithilfe von LINQ mit Ihrer Entitätsklassen. Allerdings gibt es möglicherweise Fälle, in denen Sie Abfragen mithilfe roher SQL direkt in der Datenbank ausführen möchten. Dazu gehört das Aufrufen von gespeicherter Prozeduren, die für die Code First-Modelle hilfreich sein können, die derzeit keine Zuordnung zu gespeicherten Prozeduren unterstützen. Die in diesem Thema dargestellten Techniken gelten jeweils für Modelle, die mit Code First und dem EF-Designer erstellt wurden.  

## <a name="writing-sql-queries-for-entities"></a>Schreiben von SQL-Abfragen für Entitäten  

Die SqlQuery-Methode auf "DbSet" ermöglicht eine unformatierte SQL-Abfrage geschrieben werden, die Instanzen der Entität zurückgegeben wird. Die zurückgegebenen Objekte werden vom Kontext nachverfolgt werden, wie dies der Fall wäre, wenn sie von einer LINQ-Abfrage zurückgegeben wurden. Zum Beispiel:  

``` csharp  
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("SELECT * FROM dbo.Blogs").ToList();
}
```  

Beachten Sie, dass genau wie LINQ-Abfragen, die Abfrage nicht ausgeführt wird, bis die Ergebnisse aufgelistet werden, in dem Beispiel oben ist dies durch den Aufruf von "ToList".  

Vorsichtig sollte vorgenommen werden, wenn unformatierte SQL-Abfragen aus zwei Gründen geschrieben werden. Zunächst sollte die Abfrage geschrieben werden, um sicherzustellen, dass nur Entitäten zurückgegeben, die tatsächlich vom angeforderten Typ sind. Beispielsweise ist bei Verwendung von Features wie z. B. Vererbung es einfach eine Abfrage schreiben, die Entitäten erstellen, die von den falschen CLR-Typ sind.  

Zweitens machen einige Arten von unformatierte SQL-Abfrage potenziellen Sicherheitsrisiken, insbesondere SQL Injection-Angriffe. Stellen Sie sicher, dass Sie Parameter in der Abfrage in die richtige Methode, die gegen solche Angriffe verwenden.  

### <a name="loading-entities-from-stored-procedures"></a>Laden von Entitäten von gespeicherten Prozeduren  

Sie können die DbSet.SqlQuery verwenden, um Entitäten aus den Ergebnissen einer gespeicherten Prozedur zu laden. Der folgende Code ruft z. B. das "Dbo". GetBlogs Prozedur in der Datenbank:  

``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("dbo.GetBlogs").ToList();
}
```  

Sie können auch Parameter an eine gespeicherte Prozedur, die mit der folgenden Syntax übergeben:  

``` csharp
using (var context = new BloggingContext())
{
    var blogId = 1;

    var blogs = context.Blogs.SqlQuery("dbo.GetBlogById @p0", blogId).Single();
}
```  

## <a name="writing-sql-queries-for-non-entity-types"></a>Schreiben von SQL-Abfragen für nicht-Entitätstypen  

Eine SQL-Abfrage, die Instanzen eines beliebigen Typs, einschließlich primitiver Typen zurückzugeben, kann mithilfe der SqlQuery-Methode für die Datenbank-Klasse erstellt werden. Zum Beispiel:  

``` csharp
using (var context = new BloggingContext())
{
    var blogNames = context.Database.SqlQuery<string>(
                       "SELECT Name FROM dbo.Blogs").ToList();
}
```  

Aus SqlQuery für Datenbank zurückgegebenen Ergebnisse werden nie vom Kontext nachverfolgt werden, auch wenn die Objekte Instanzen eines Entitätstyps sind.  

## <a name="sending-raw-commands-to-the-database"></a>Raw-Befehle an die Datenbank gesendet.  

Nicht-Abfragebefehle können mit der Datenbank mithilfe der ExecuteSqlCommand-Methode für die Datenbank gesendet werden. Zum Beispiel:  

``` csharp
using (var context = new BloggingContext())
{
    context.Database.ExecuteSqlCommand(
        "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
}
```  

Beachten Sie, dass alle Änderungen an Daten in der Datenbank mithilfe von ExecuteSqlCommand für den Kontext nicht transparent bis Entitäten geladen oder aus der Datenbank geladen werden.  

### <a name="output-parameters"></a>Ausgabeparameter  

Wenn Output-Parameter verwendet werden, werden ihre Werte nicht verfügbar, bis die Ergebnisse vollständig gelesen wurden. Dies liegt an der zugrunde liegende Verhalten von DbDataReader, finden Sie unter [Abrufen von Daten mit einem DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) Weitere Details.  
