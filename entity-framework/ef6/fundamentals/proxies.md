---
title: Arbeiten mit Proxys EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 869ee4dc-06f1-471d-8e0e-0a1a2bc59c30
ms.openlocfilehash: 8f7d2e8b41ece28efe8d1df3b0679e6e4510d64a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415988"
---
# <a name="working-with-proxies"></a>Arbeiten mit Proxys
Beim Erstellen von Instanzen von poco-Entitäts Typen erstellt Entity Framework häufig Instanzen eines dynamisch generierten abgeleiteten Typs, der als Proxy für die Entität fungiert. Dieser Proxy überschreibt einige virtuelle Eigenschaften der Entität, um beim Zugriff auf die Eigenschaft automatisch Hooks zum Ausführen von Aktionen einzufügen. Dieser Mechanismus wird z. b. verwendet, um Lazy Loading von Beziehungen zu unterstützen. Die in diesem Thema dargestellten Techniken gelten jeweils für Modelle, die mit Code First und dem EF-Designer erstellt wurden.  

## <a name="disabling-proxy-creation"></a>Proxy Erstellung wird deaktiviert.  

Manchmal ist es hilfreich, die Erstellung von Proxy Instanzen durch Entity Framework zu verhindern. Beispielsweise ist das Serialisieren von nicht-Proxy Instanzen erheblich einfacher als das Serialisieren von Proxy Instanzen. Die Proxy Erstellung kann deaktiviert werden, indem das Flag "proxyroationaktivierte" gelöscht wird. Ein Ort, an dem Sie dies vornehmen können, ist der Konstruktor Ihres Kontexts. Beispiel:  

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

Beachten Sie, dass EF keine Proxys für Typen erstellt, bei denen es für den Proxy nichts gibt. Dies bedeutet, dass Sie Proxys auch vermeiden können, wenn Sie über Typen verfügen, die versiegelt sind und/oder keine virtuellen Eigenschaften haben.  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a>Explizites Erstellen einer Instanz eines Proxys  

Eine Proxy Instanz wird nicht erstellt, wenn Sie mithilfe des new-Operators eine Instanz einer Entität erstellen. Dies ist möglicherweise kein Problem, aber wenn Sie eine Proxy Instanz erstellen müssen (z. b., damit Lazy Loading oder Proxy Änderungs Nachverfolgung funktioniert), können Sie dies mithilfe der Create-Methode von dbset tun. Beispiel:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

Die generische Version von CREATE kann verwendet werden, wenn Sie eine Instanz eines abgeleiteten Entitäts Typs erstellen möchten. Beispiel:  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

Beachten Sie, dass die erstellte Entität durch die Create-Methode nicht dem Kontext hinzugefügt oder angefügt wird.  

Beachten Sie, dass die Create-Methode nur eine Instanz des Entitäts Typs selbst erstellt, wenn das Erstellen eines Proxytyps für die Entität keinen Wert hätte, weil Sie nichts Unternehmen würde. Wenn der Entitätstyp z. b. versiegelt ist und/oder keine virtuellen Eigenschaften aufweist, erstellt Create lediglich eine Instanz des Entitäts Typs.  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a>Der tatsächliche Entitätstyp wird von einem Proxytyp erhalten.  

Proxy Typen haben Namen, die in etwa wie folgt aussehen:  

System. Data. Entity. dynamicproxies. Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6  

Sie können den Entitätstyp für diesen Proxytyp mithilfe der getObjectType-Methode von ObjectContext suchen. Beispiel:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

Beachten Sie Folgendes: Wenn der an getObjectType weiter gegebene Typ eine Instanz eines Entitäts Typs ist, der kein Proxytyp ist, wird der Entitätstyp weiterhin zurückgegeben. Dies bedeutet, dass Sie immer diese Methode verwenden können, um den eigentlichen Entitätstyp ohne andere zu überprüfen, um festzustellen, ob es sich bei dem Typ um einen Proxytyp handelt.  
