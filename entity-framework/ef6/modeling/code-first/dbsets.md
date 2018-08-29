---
title: Definieren von "dbsets" – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 4528a509-ace7-4dfb-8065-1b833f5e03a0
ms.openlocfilehash: cc45ed1ceb20bc90090adb3e93c10651c69c9a6a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993856"
---
# <a name="defining-dbsets"></a><span data-ttu-id="32da5-102">Definieren von "dbsets"</span><span class="sxs-lookup"><span data-stu-id="32da5-102">Defining DbSets</span></span>
<span data-ttu-id="32da5-103">Bei der Entwicklung mit Code First-Workflow definieren Sie einen abgeleiteten "DbContext", das von der Sitzung mit der Datenbank, und stellt einen "DbSet" für jeden Typ im Modell.</span><span class="sxs-lookup"><span data-stu-id="32da5-103">When developing with the Code First workflow you define a derived DbContext that represents your session with the database and exposes a DbSet for each type in your model.</span></span> <span data-ttu-id="32da5-104">Dieses Thema behandelt die verschiedenen Möglichkeiten, die Sie die Eigenschaften "DbSet" definieren können.</span><span class="sxs-lookup"><span data-stu-id="32da5-104">This topic covers the various ways you can define the DbSet properties.</span></span>  

## <a name="dbcontext-with-dbset-properties"></a><span data-ttu-id="32da5-105">"DbContext" mit "DbSet"-Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="32da5-105">DbContext with DbSet properties</span></span>  

<span data-ttu-id="32da5-106">Meistens in Code First-Beispielen gezeigt ist, um einen "DbContext" mit öffentlichen automatische "DbSet"-Eigenschaften für die Entitätstypen des Modells zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="32da5-106">The common case shown in Code First examples is to have a DbContext with public automatic DbSet properties for the entity types of your model.</span></span> <span data-ttu-id="32da5-107">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="32da5-107">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="32da5-108">Bei Verwendung in Code First-Modus wird dieses Blogs und Beiträgen als Entitätstypen sowie die Konfiguration anderer Endpunkttypen, die von diesen erreichbar konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="32da5-108">When used in Code First mode, this will configure Blogs and Posts as entity types, as well as configuring other types reachable from these.</span></span> <span data-ttu-id="32da5-109">Darüber hinaus ruft "DbContext" automatisch den Setter für jede dieser Eigenschaften, die eine Instanz der entsprechenden "DbSet" festgelegt.</span><span class="sxs-lookup"><span data-stu-id="32da5-109">In addition DbContext will automatically call the setter for each of these properties to set an instance of the appropriate DbSet.</span></span>  

## <a name="dbcontext-with-idbset-properties"></a><span data-ttu-id="32da5-110">"DbContext" mit IDbSet-Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="32da5-110">DbContext with IDbSet properties</span></span>  

<span data-ttu-id="32da5-111">Es gibt Situationen, z. B. beim Erstellen oder mocks fakes, in denen es sinnvoller, Ihre Eigenschaften festlegen, die mit einer Schnittstelle deklariert ist.</span><span class="sxs-lookup"><span data-stu-id="32da5-111">There are situations, such as when creating mocks or fakes, where it is more useful to declare your set properties using an interface.</span></span> <span data-ttu-id="32da5-112">In solchen Fällen die IDbSet kann Schnittstelle anstelle von "DbSet" verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="32da5-112">In such cases the IDbSet interface can be used in place of DbSet.</span></span> <span data-ttu-id="32da5-113">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="32da5-113">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="32da5-114">Dieser Kontext funktioniert in genau die gleiche Weise wie der Kontext, der die "DbSet"-Klasse für die festgelegte Eigenschaften verwendet.</span><span class="sxs-lookup"><span data-stu-id="32da5-114">This context works in exactly the same way as the context that uses the DbSet class for its set properties.</span></span>  

## <a name="dbcontext-with-read-only-set-properties"></a><span data-ttu-id="32da5-115">"DbContext" mit schreibgeschützten Eigenschaften festlegen.</span><span class="sxs-lookup"><span data-stu-id="32da5-115">DbContext with read-only set properties</span></span>  

<span data-ttu-id="32da5-116">Wenn Sie nicht möchten, um öffentliche Setter für die Eigenschaften "DbSet" oder IDbSet verfügbar zu machen, Sie stattdessen schreibgeschützte Eigenschaften erstellen und die Instanzen selbst erstellen.</span><span class="sxs-lookup"><span data-stu-id="32da5-116">If you do not wish to expose public setters for your DbSet or IDbSet properties you can instead create read-only properties and create the set instances yourself.</span></span> <span data-ttu-id="32da5-117">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="32da5-117">For example:</span></span>  

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

<span data-ttu-id="32da5-118">Beachten Sie, dass "DbContext" speichert die Instanz von "DbSet" von der Set-Methode zurückgegeben wird, sodass jede dieser Eigenschaften wird die gleiche Instanz zurückgeben, jedes Mal, wenn sie aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="32da5-118">Note that DbContext caches the instance of DbSet returned from the Set method so that each of these properties will return the same instance every time it is called.</span></span>  

<span data-ttu-id="32da5-119">Ermittlung von Entitätstypen für Code First funktioniert hier genauso, wie er für Eigenschaften mit öffentlichen Getter und Setter ist.</span><span class="sxs-lookup"><span data-stu-id="32da5-119">Discovery of entity types for Code First works in the same way here as it does for properties with public getters and setters.</span></span>  
