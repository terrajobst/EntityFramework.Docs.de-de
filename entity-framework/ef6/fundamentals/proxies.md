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
# <a name="working-with-proxies"></a><span data-ttu-id="a82bd-102">Arbeiten mit Proxys</span><span class="sxs-lookup"><span data-stu-id="a82bd-102">Working with proxies</span></span>
<span data-ttu-id="a82bd-103">Beim Erstellen von Instanzen von poco-Entitäts Typen erstellt Entity Framework häufig Instanzen eines dynamisch generierten abgeleiteten Typs, der als Proxy für die Entität fungiert.</span><span class="sxs-lookup"><span data-stu-id="a82bd-103">When creating instances of POCO entity types, Entity Framework often creates instances of a dynamically generated derived type that acts as a proxy for the entity.</span></span> <span data-ttu-id="a82bd-104">Dieser Proxy überschreibt einige virtuelle Eigenschaften der Entität, um beim Zugriff auf die Eigenschaft automatisch Hooks zum Ausführen von Aktionen einzufügen.</span><span class="sxs-lookup"><span data-stu-id="a82bd-104">This proxy overrides some virtual properties of the entity to insert hooks for performing actions automatically when the property is accessed.</span></span> <span data-ttu-id="a82bd-105">Dieser Mechanismus wird z. b. verwendet, um Lazy Loading von Beziehungen zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="a82bd-105">For example, this mechanism is used to support lazy loading of relationships.</span></span> <span data-ttu-id="a82bd-106">Die in diesem Thema dargestellten Techniken gelten jeweils für Modelle, die mit Code First und dem EF-Designer erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="a82bd-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="disabling-proxy-creation"></a><span data-ttu-id="a82bd-107">Proxy Erstellung wird deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="a82bd-107">Disabling proxy creation</span></span>  

<span data-ttu-id="a82bd-108">Manchmal ist es hilfreich, die Erstellung von Proxy Instanzen durch Entity Framework zu verhindern.</span><span class="sxs-lookup"><span data-stu-id="a82bd-108">Sometimes it is useful to prevent Entity Framework from creating proxy instances.</span></span> <span data-ttu-id="a82bd-109">Beispielsweise ist das Serialisieren von nicht-Proxy Instanzen erheblich einfacher als das Serialisieren von Proxy Instanzen.</span><span class="sxs-lookup"><span data-stu-id="a82bd-109">For example, serializing non-proxy instances is considerably easier than serializing proxy instances.</span></span> <span data-ttu-id="a82bd-110">Die Proxy Erstellung kann deaktiviert werden, indem das Flag "proxyroationaktivierte" gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="a82bd-110">Proxy creation can be turned off by clearing the ProxyCreationEnabled flag.</span></span> <span data-ttu-id="a82bd-111">Ein Ort, an dem Sie dies vornehmen können, ist der Konstruktor Ihres Kontexts.</span><span class="sxs-lookup"><span data-stu-id="a82bd-111">One place you could do this is in the constructor of your context.</span></span> <span data-ttu-id="a82bd-112">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a82bd-112">For example:</span></span>  

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

<span data-ttu-id="a82bd-113">Beachten Sie, dass EF keine Proxys für Typen erstellt, bei denen es für den Proxy nichts gibt.</span><span class="sxs-lookup"><span data-stu-id="a82bd-113">Note that the EF will not create proxies for types where there is nothing for the proxy to do.</span></span> <span data-ttu-id="a82bd-114">Dies bedeutet, dass Sie Proxys auch vermeiden können, wenn Sie über Typen verfügen, die versiegelt sind und/oder keine virtuellen Eigenschaften haben.</span><span class="sxs-lookup"><span data-stu-id="a82bd-114">This means that you can also avoid proxies by having types that are sealed and/or have no virtual properties.</span></span>  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a><span data-ttu-id="a82bd-115">Explizites Erstellen einer Instanz eines Proxys</span><span class="sxs-lookup"><span data-stu-id="a82bd-115">Explicitly creating an instance of a proxy</span></span>  

<span data-ttu-id="a82bd-116">Eine Proxy Instanz wird nicht erstellt, wenn Sie mithilfe des new-Operators eine Instanz einer Entität erstellen.</span><span class="sxs-lookup"><span data-stu-id="a82bd-116">A proxy instance will not be created if you create an instance of an entity using the new operator.</span></span> <span data-ttu-id="a82bd-117">Dies ist möglicherweise kein Problem, aber wenn Sie eine Proxy Instanz erstellen müssen (z. b., damit Lazy Loading oder Proxy Änderungs Nachverfolgung funktioniert), können Sie dies mithilfe der Create-Methode von dbset tun.</span><span class="sxs-lookup"><span data-stu-id="a82bd-117">This may not be a problem, but if you need to create a proxy instance (for example, so that lazy loading or proxy change tracking will work) then you can do so using the Create method of DbSet.</span></span> <span data-ttu-id="a82bd-118">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a82bd-118">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

<span data-ttu-id="a82bd-119">Die generische Version von CREATE kann verwendet werden, wenn Sie eine Instanz eines abgeleiteten Entitäts Typs erstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="a82bd-119">The generic version of Create can be used if you want to create an instance of a derived entity type.</span></span> <span data-ttu-id="a82bd-120">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a82bd-120">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

<span data-ttu-id="a82bd-121">Beachten Sie, dass die erstellte Entität durch die Create-Methode nicht dem Kontext hinzugefügt oder angefügt wird.</span><span class="sxs-lookup"><span data-stu-id="a82bd-121">Note that the Create method does not add or attach the created entity to the context.</span></span>  

<span data-ttu-id="a82bd-122">Beachten Sie, dass die Create-Methode nur eine Instanz des Entitäts Typs selbst erstellt, wenn das Erstellen eines Proxytyps für die Entität keinen Wert hätte, weil Sie nichts Unternehmen würde.</span><span class="sxs-lookup"><span data-stu-id="a82bd-122">Note that the Create method will just create an instance of the entity type itself if creating a proxy type for the entity would have no value because it would not do anything.</span></span> <span data-ttu-id="a82bd-123">Wenn der Entitätstyp z. b. versiegelt ist und/oder keine virtuellen Eigenschaften aufweist, erstellt Create lediglich eine Instanz des Entitäts Typs.</span><span class="sxs-lookup"><span data-stu-id="a82bd-123">For example, if the entity type is sealed and/or has no virtual properties then Create will just create an instance of the entity type.</span></span>  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a><span data-ttu-id="a82bd-124">Der tatsächliche Entitätstyp wird von einem Proxytyp erhalten.</span><span class="sxs-lookup"><span data-stu-id="a82bd-124">Getting the actual entity type from a proxy type</span></span>  

<span data-ttu-id="a82bd-125">Proxy Typen haben Namen, die in etwa wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="a82bd-125">Proxy types have names that look something like this:</span></span>  

<span data-ttu-id="a82bd-126">System. Data. Entity. dynamicproxies. Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span><span class="sxs-lookup"><span data-stu-id="a82bd-126">System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span></span>  

<span data-ttu-id="a82bd-127">Sie können den Entitätstyp für diesen Proxytyp mithilfe der getObjectType-Methode von ObjectContext suchen.</span><span class="sxs-lookup"><span data-stu-id="a82bd-127">You can find the entity type for this proxy type using the GetObjectType method from ObjectContext.</span></span> <span data-ttu-id="a82bd-128">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a82bd-128">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

<span data-ttu-id="a82bd-129">Beachten Sie Folgendes: Wenn der an getObjectType weiter gegebene Typ eine Instanz eines Entitäts Typs ist, der kein Proxytyp ist, wird der Entitätstyp weiterhin zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="a82bd-129">Note that if the type passed to GetObjectType is an instance of an entity type that is not a proxy type then the type of entity is still returned.</span></span> <span data-ttu-id="a82bd-130">Dies bedeutet, dass Sie immer diese Methode verwenden können, um den eigentlichen Entitätstyp ohne andere zu überprüfen, um festzustellen, ob es sich bei dem Typ um einen Proxytyp handelt.</span><span class="sxs-lookup"><span data-stu-id="a82bd-130">This means you can always use this method to get the actual entity type without any other checking to see if the type is a proxy type or not.</span></span>  
