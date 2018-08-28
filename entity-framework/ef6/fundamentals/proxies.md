---
title: Arbeiten mit Proxys - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 869ee4dc-06f1-471d-8e0e-0a1a2bc59c30
ms.openlocfilehash: 7b82dd370e67d1622fc00ff5e5275721d0fc4fe1
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997202"
---
# <a name="working-with-proxies"></a>Arbeiten mit Proxys in
Wenn Sie Instanzen von Typen, POCO-Entität zu erstellen, erstellt Entity Framework häufig Instanzen eines dynamisch generierten abgeleiteten Typs, das fungiert als Proxy für die Entität. Dieser Proxy überschreibt einige virtuelle Eigenschaften der Entität zum Einfügen von Hooks für Aktionen automatisch ausgeführt, wenn die Eigenschaft zugegriffen wird. Beispielsweise wird dieser Mechanismus verwendet, lazy Loading von Beziehungen unterstützt. Die in diesem Thema dargestellten Techniken gelten jeweils für Modelle, die mit Code First und dem EF-Designer erstellt wurden.  

## <a name="disabling-proxy-creation"></a>Deaktivieren die Erstellung von Proxys  

Manchmal ist es hilfreich, um zu verhindern, dass Entity Framework-Proxyinstanzen erstellen. Beispielsweise ist das Serialisieren von nicht-Proxyinstanzen erheblich einfacher als das Serialisieren von Proxyinstanzen. Erstellung von Proxys kann deaktiviert werden, indem Sie das Flag ProxyCreationEnabled deaktivieren. Zentral dies möglich wäre, wird im Konstruktor Ihres Kontexts. Zum Beispiel:  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.ProxyCreationEnabled = false;
    }  

    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

Beachten Sie, dass EF keine Proxys für Typen erstellt werden, wenn vorhanden "nothing" für den Proxy ist zu tun. Dies bedeutet, dass Sie auch Proxys vermeiden können, indem Sie die Typen, die sind versiegelt und/oder verfügen über keine virtuellen Eigenschaften.  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a>Erstellen explizit eine Instanz eines Proxys  

Eine Proxyinstanz wird nicht erstellt werden, wenn Sie eine Instanz einer Entität mit dem neuen Operator erstellen. Dies eventuell nicht möglich, ein Problem, aber wenn müssen Sie eine Proxyinstanz erstellen (z. B. so, dass lazy Loading oder einem Proxyserver änderungsnachverfolgung funktionieren) klicken Sie dann erreichen Sie dies mit der Create-Methode von "DbSet". Zum Beispiel:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

Die generische Version erstellen kann verwendet werden, wenn Sie eine Instanz eines abgeleiteten Entitätstyps erstellen möchten. Zum Beispiel:  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

Beachten Sie, dass die Create-Methode nicht hinzufügen oder die erstellte Entität in den Kontext angefügt.  

Beachten Sie, dass die Create-Methode erstellen Sie einen Proxytyp für die Entität nur eine Instanz des Entitätstyps selbst erstellen müssten kein Wert aus, da es keine Wirkung würden. Z. B. wenn der Typ versiegelt ist, und/oder verfügt über keine virtuellen Eigenschaften erstellt erstellen einfach eine Instanz des Entitätstyps dann.  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a>Abrufen des tatsächlichen Entitätstyps von einem Proxytyp  

Proxytypen haben Namen, die etwa wie folgt aussehen:  

System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6  

Sie finden den Entitätstyp mithilfe der GetObjectType hat-Methode von ObjectContext für diesen Proxy. Zum Beispiel:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

Beachten Sie, dass wenn der Typ an GetObjectType hat übergeben einer Instanz eines Entitätstyps, die nicht auf einen Proxytyp klicken Sie dann den Typ der Entität ist, wird weiterhin zurückgegeben. Dies bedeutet, dass Sie diese Methode immer verwenden können, um der tatsächliche Typ zu erhalten, ohne eine andere Überprüfung um festzustellen, ob der Typ einen Proxytyp oder nicht ist.  
