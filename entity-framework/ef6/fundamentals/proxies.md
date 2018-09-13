---
title: Arbeiten mit Proxys - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 869ee4dc-06f1-471d-8e0e-0a1a2bc59c30
ms.openlocfilehash: 8f7d2e8b41ece28efe8d1df3b0679e6e4510d64a
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489816"
---
# <a name="working-with-proxies"></a><span data-ttu-id="7bb8b-102">Arbeiten mit Proxys in</span><span class="sxs-lookup"><span data-stu-id="7bb8b-102">Working with proxies</span></span>
<span data-ttu-id="7bb8b-103">Wenn Sie Instanzen von Typen, POCO-Entität zu erstellen, erstellt Entity Framework häufig Instanzen eines dynamisch generierten abgeleiteten Typs, das fungiert als Proxy für die Entität.</span><span class="sxs-lookup"><span data-stu-id="7bb8b-103">When creating instances of POCO entity types, Entity Framework often creates instances of a dynamically generated derived type that acts as a proxy for the entity.</span></span> <span data-ttu-id="7bb8b-104">Dieser Proxy überschreibt einige virtuelle Eigenschaften der Entität zum Einfügen von Hooks für Aktionen automatisch ausgeführt, wenn die Eigenschaft zugegriffen wird.</span><span class="sxs-lookup"><span data-stu-id="7bb8b-104">This proxy overrides some virtual properties of the entity to insert hooks for performing actions automatically when the property is accessed.</span></span> <span data-ttu-id="7bb8b-105">Beispielsweise wird dieser Mechanismus verwendet, lazy Loading von Beziehungen unterstützt.</span><span class="sxs-lookup"><span data-stu-id="7bb8b-105">For example, this mechanism is used to support lazy loading of relationships.</span></span> <span data-ttu-id="7bb8b-106">Die in diesem Thema dargestellten Techniken gelten jeweils für Modelle, die mit Code First und dem EF-Designer erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="7bb8b-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="disabling-proxy-creation"></a><span data-ttu-id="7bb8b-107">Deaktivieren die Erstellung von Proxys</span><span class="sxs-lookup"><span data-stu-id="7bb8b-107">Disabling proxy creation</span></span>  

<span data-ttu-id="7bb8b-108">Manchmal ist es hilfreich, um zu verhindern, dass Entity Framework-Proxyinstanzen erstellen.</span><span class="sxs-lookup"><span data-stu-id="7bb8b-108">Sometimes it is useful to prevent Entity Framework from creating proxy instances.</span></span> <span data-ttu-id="7bb8b-109">Beispielsweise ist das Serialisieren von nicht-Proxyinstanzen erheblich einfacher als das Serialisieren von Proxyinstanzen.</span><span class="sxs-lookup"><span data-stu-id="7bb8b-109">For example, serializing non-proxy instances is considerably easier than serializing proxy instances.</span></span> <span data-ttu-id="7bb8b-110">Erstellung von Proxys kann deaktiviert werden, indem Sie das Flag ProxyCreationEnabled deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="7bb8b-110">Proxy creation can be turned off by clearing the ProxyCreationEnabled flag.</span></span> <span data-ttu-id="7bb8b-111">Zentral dies möglich wäre, wird im Konstruktor Ihres Kontexts.</span><span class="sxs-lookup"><span data-stu-id="7bb8b-111">One place you could do this is in the constructor of your context.</span></span> <span data-ttu-id="7bb8b-112">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="7bb8b-112">For example:</span></span>  

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

<span data-ttu-id="7bb8b-113">Beachten Sie, dass EF keine Proxys für Typen erstellt werden, wenn vorhanden "nothing" für den Proxy ist zu tun.</span><span class="sxs-lookup"><span data-stu-id="7bb8b-113">Note that the EF will not create proxies for types where there is nothing for the proxy to do.</span></span> <span data-ttu-id="7bb8b-114">Dies bedeutet, dass Sie auch Proxys vermeiden können, indem Sie die Typen, die sind versiegelt und/oder verfügen über keine virtuellen Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="7bb8b-114">This means that you can also avoid proxies by having types that are sealed and/or have no virtual properties.</span></span>  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a><span data-ttu-id="7bb8b-115">Erstellen explizit eine Instanz eines Proxys</span><span class="sxs-lookup"><span data-stu-id="7bb8b-115">Explicitly creating an instance of a proxy</span></span>  

<span data-ttu-id="7bb8b-116">Eine Proxyinstanz wird nicht erstellt werden, wenn Sie eine Instanz einer Entität mit dem neuen Operator erstellen.</span><span class="sxs-lookup"><span data-stu-id="7bb8b-116">A proxy instance will not be created if you create an instance of an entity using the new operator.</span></span> <span data-ttu-id="7bb8b-117">Dies eventuell nicht möglich, ein Problem, aber wenn müssen Sie eine Proxyinstanz erstellen (z. B. so, dass lazy Loading oder einem Proxyserver änderungsnachverfolgung funktionieren) klicken Sie dann erreichen Sie dies mit der Create-Methode von "DbSet".</span><span class="sxs-lookup"><span data-stu-id="7bb8b-117">This may not be a problem, but if you need to create a proxy instance (for example, so that lazy loading or proxy change tracking will work) then you can do so using the Create method of DbSet.</span></span> <span data-ttu-id="7bb8b-118">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="7bb8b-118">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

<span data-ttu-id="7bb8b-119">Die generische Version erstellen kann verwendet werden, wenn Sie eine Instanz eines abgeleiteten Entitätstyps erstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="7bb8b-119">The generic version of Create can be used if you want to create an instance of a derived entity type.</span></span> <span data-ttu-id="7bb8b-120">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="7bb8b-120">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

<span data-ttu-id="7bb8b-121">Beachten Sie, dass die Create-Methode nicht hinzufügen oder die erstellte Entität in den Kontext angefügt.</span><span class="sxs-lookup"><span data-stu-id="7bb8b-121">Note that the Create method does not add or attach the created entity to the context.</span></span>  

<span data-ttu-id="7bb8b-122">Beachten Sie, dass die Create-Methode erstellen Sie einen Proxytyp für die Entität nur eine Instanz des Entitätstyps selbst erstellen müssten kein Wert aus, da es keine Wirkung würden.</span><span class="sxs-lookup"><span data-stu-id="7bb8b-122">Note that the Create method will just create an instance of the entity type itself if creating a proxy type for the entity would have no value because it would not do anything.</span></span> <span data-ttu-id="7bb8b-123">Z. B. wenn der Typ versiegelt ist, und/oder verfügt über keine virtuellen Eigenschaften erstellt erstellen einfach eine Instanz des Entitätstyps dann.</span><span class="sxs-lookup"><span data-stu-id="7bb8b-123">For example, if the entity type is sealed and/or has no virtual properties then Create will just create an instance of the entity type.</span></span>  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a><span data-ttu-id="7bb8b-124">Abrufen des tatsächlichen Entitätstyps von einem Proxytyp</span><span class="sxs-lookup"><span data-stu-id="7bb8b-124">Getting the actual entity type from a proxy type</span></span>  

<span data-ttu-id="7bb8b-125">Proxytypen haben Namen, die etwa wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="7bb8b-125">Proxy types have names that look something like this:</span></span>  

<span data-ttu-id="7bb8b-126">System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span><span class="sxs-lookup"><span data-stu-id="7bb8b-126">System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span></span>  

<span data-ttu-id="7bb8b-127">Sie finden den Entitätstyp mithilfe der GetObjectType hat-Methode von ObjectContext für diesen Proxy.</span><span class="sxs-lookup"><span data-stu-id="7bb8b-127">You can find the entity type for this proxy type using the GetObjectType method from ObjectContext.</span></span> <span data-ttu-id="7bb8b-128">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="7bb8b-128">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

<span data-ttu-id="7bb8b-129">Beachten Sie, dass wenn der Typ an GetObjectType hat übergeben einer Instanz eines Entitätstyps, die nicht auf einen Proxytyp klicken Sie dann den Typ der Entität ist, wird weiterhin zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="7bb8b-129">Note that if the type passed to GetObjectType is an instance of an entity type that is not a proxy type then the type of entity is still returned.</span></span> <span data-ttu-id="7bb8b-130">Dies bedeutet, dass Sie diese Methode immer verwenden können, um der tatsächliche Typ zu erhalten, ohne eine andere Überprüfung um festzustellen, ob der Typ einen Proxytyp oder nicht ist.</span><span class="sxs-lookup"><span data-stu-id="7bb8b-130">This means you can always use this method to get the actual entity type without any other checking to see if the type is a proxy type or not.</span></span>  
