---
title: Definieren von dbsets-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 4528a509-ace7-4dfb-8065-1b833f5e03a0
ms.openlocfilehash: 045b22d2b9d26804948689dd7c9dd694baadda7e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415778"
---
# <a name="defining-dbsets"></a><span data-ttu-id="cc13b-102">Definieren von dbsets</span><span class="sxs-lookup"><span data-stu-id="cc13b-102">Defining DbSets</span></span>
<span data-ttu-id="cc13b-103">Wenn Sie mit dem Code First-Workflow entwickeln, definieren Sie einen abgeleiteten dbcontext, der Ihre Sitzung mit der Datenbank repräsentiert und ein dbset für jeden Typ im Modell verfügbar macht.</span><span class="sxs-lookup"><span data-stu-id="cc13b-103">When developing with the Code First workflow you define a derived DbContext that represents your session with the database and exposes a DbSet for each type in your model.</span></span> <span data-ttu-id="cc13b-104">In diesem Thema werden die verschiedenen Möglichkeiten beschrieben, wie Sie die dbset-Eigenschaften definieren können.</span><span class="sxs-lookup"><span data-stu-id="cc13b-104">This topic covers the various ways you can define the DbSet properties.</span></span>  

## <a name="dbcontext-with-dbset-properties"></a><span data-ttu-id="cc13b-105">Dbcontext mit dbset-Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="cc13b-105">DbContext with DbSet properties</span></span>  

<span data-ttu-id="cc13b-106">Der gängige Fall, der in Code First Beispielen gezeigt wird, besteht darin, einen dbcontext mit öffentlichen automatischen dbset-Eigenschaften für die Entitäts Typen des Modells zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="cc13b-106">The common case shown in Code First examples is to have a DbContext with public automatic DbSet properties for the entity types of your model.</span></span> <span data-ttu-id="cc13b-107">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cc13b-107">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="cc13b-108">Bei der Verwendung im Code First Modus werden Blogs und Beiträge als Entitäts Typen konfiguriert und andere Typen konfiguriert, die von diesen verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="cc13b-108">When used in Code First mode, this will configure Blogs and Posts as entity types, as well as configuring other types reachable from these.</span></span> <span data-ttu-id="cc13b-109">Außerdem ruft dbcontext automatisch den Setter für jede dieser Eigenschaften auf, um eine Instanz des entsprechenden dbsets festzulegen.</span><span class="sxs-lookup"><span data-stu-id="cc13b-109">In addition DbContext will automatically call the setter for each of these properties to set an instance of the appropriate DbSet.</span></span>  

## <a name="dbcontext-with-idbset-properties"></a><span data-ttu-id="cc13b-110">Dbcontext mit idbset-Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="cc13b-110">DbContext with IDbSet properties</span></span>  

<span data-ttu-id="cc13b-111">Es gibt Situationen, z. b. beim Erstellen von Pseudo-oder Fakes, bei denen es nützlicher ist, die festgelegten Eigenschaften mithilfe einer Schnittstelle zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="cc13b-111">There are situations, such as when creating mocks or fakes, where it is more useful to declare your set properties using an interface.</span></span> <span data-ttu-id="cc13b-112">In solchen Fällen kann die idbset-Schnittstelle anstelle von dbset verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="cc13b-112">In such cases the IDbSet interface can be used in place of DbSet.</span></span> <span data-ttu-id="cc13b-113">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cc13b-113">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="cc13b-114">Dieser Kontext funktioniert auf genau dieselbe Weise wie der Kontext, der die dbset-Klasse für seine Set-Eigenschaften verwendet.</span><span class="sxs-lookup"><span data-stu-id="cc13b-114">This context works in exactly the same way as the context that uses the DbSet class for its set properties.</span></span>  

## <a name="dbcontext-with-read-only-set-properties"></a><span data-ttu-id="cc13b-115">Dbcontext mit schreibgeschützten Set-Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="cc13b-115">DbContext with read-only set properties</span></span>  

<span data-ttu-id="cc13b-116">Wenn Sie keine öffentlichen Setter für die dbset-oder idbset-Eigenschaften verfügbar machen möchten, können Sie stattdessen schreibgeschützte Eigenschaften erstellen und die Set-Instanzen selbst erstellen.</span><span class="sxs-lookup"><span data-stu-id="cc13b-116">If you do not wish to expose public setters for your DbSet or IDbSet properties you can instead create read-only properties and create the set instances yourself.</span></span> <span data-ttu-id="cc13b-117">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cc13b-117">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs
    {
        get { return Set<Blog>(); }
    }

    public DbSet<Post> Posts
    {
        get { return Set<Post>(); }
    }
}
```  

<span data-ttu-id="cc13b-118">Beachten Sie, dass dbcontext die Instanz von dbset zwischenspeichert, die von der Set-Methode zurückgegeben wird, sodass jede dieser Eigenschaften bei jedem Aufruf dieselbe Instanz zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="cc13b-118">Note that DbContext caches the instance of DbSet returned from the Set method so that each of these properties will return the same instance every time it is called.</span></span>  

<span data-ttu-id="cc13b-119">Die Ermittlung von Entitäts Typen für Code First funktioniert auf dieselbe Weise wie bei Eigenschaften mit öffentlichen Getter und Settern.</span><span class="sxs-lookup"><span data-stu-id="cc13b-119">Discovery of entity types for Code First works in the same way here as it does for properties with public getters and setters.</span></span>  
