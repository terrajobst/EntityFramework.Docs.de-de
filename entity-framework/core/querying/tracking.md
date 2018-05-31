---
title: 'Abfragen mit Nachverfolgung im Vergleich zu Abfragen ohne Nachverfolgung: EF Core'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
ms.technology: entity-framework-core
uid: core/querying/tracking
ms.openlocfilehash: 9a22c893f3b1e9991560e25e0252287a2844b39e
ms.sourcegitcommit: 3b6159db8a6c0653f13c7b528367b4e69ac3d51e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2017
ms.locfileid: "26053960"
---
# <a name="tracking-vs-no-tracking-queries"></a><span data-ttu-id="1dd9d-102">Abfragen mit Nachverfolgung im Vergleich zu Abfragen ohne Nachverfolgung</span><span class="sxs-lookup"><span data-stu-id="1dd9d-102">Tracking vs. No-Tracking Queries</span></span>

<span data-ttu-id="1dd9d-103">Das Nachverfolgungsverhalten steuert, ob Entity Framework Core Informationen über eine Entitätsinstanz in der Änderungsprotokollierung speichert.</span><span class="sxs-lookup"><span data-stu-id="1dd9d-103">Tracking behavior controls whether or not Entity Framework Core will keep information about an entity instance in its change tracker.</span></span> <span data-ttu-id="1dd9d-104">Wenn eine Entität nachverfolgt wird, werden alle Änderungen, die in der Entität erkannt werden, während `SaveChanges()` in der Datenbank beibehalten.</span><span class="sxs-lookup"><span data-stu-id="1dd9d-104">If an entity is tracked, any changes detected in the entity will be persisted to the database during `SaveChanges()`.</span></span> <span data-ttu-id="1dd9d-105">Entity Framework Core korrigiert zusätzlich Navigationseigenschaften zwischen Entitäten, die von einer Abfrage mit Nachverfolgung abgerufen werden, und Entitäten, die zuvor in die DbContext-Instanz geladen wurden.</span><span class="sxs-lookup"><span data-stu-id="1dd9d-105">Entity Framework Core will also fix-up navigation properties between entities that are obtained from a tracking query and entities that were previously loaded into the DbContext instance.</span></span>

> [!TIP]  
> <span data-ttu-id="1dd9d-106">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="1dd9d-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="tracking-queries"></a><span data-ttu-id="1dd9d-107">Abfragen mit Nachverfolgung</span><span class="sxs-lookup"><span data-stu-id="1dd9d-107">Tracking queries</span></span>

<span data-ttu-id="1dd9d-108">Abfragen, die Entitätstypen zurückgeben, verfügen standardmäßig über Nachverfolgung.</span><span class="sxs-lookup"><span data-stu-id="1dd9d-108">By default, queries that return entity types are tracking.</span></span> <span data-ttu-id="1dd9d-109">Das bedeutet, Sie können Änderungen an diesen Entitätsinstanzen vornehmen, und diese Änderungen werden von `SaveChanges()` beibehalten.</span><span class="sxs-lookup"><span data-stu-id="1dd9d-109">This means you can make changes to those entity instances and have those changes persisted by `SaveChanges()`.</span></span>

<span data-ttu-id="1dd9d-110">Im folgenden Beispiel wird die Änderung an der Bewertung des Blogs erkannt und während `SaveChanges()` in der Datenbank beibehalten.</span><span class="sxs-lookup"><span data-stu-id="1dd9d-110">In the following example, the change to the blogs rating will be detected and persisted to the database during `SaveChanges()`.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a><span data-ttu-id="1dd9d-111">Abfragen ohne Nachverfolgung</span><span class="sxs-lookup"><span data-stu-id="1dd9d-111">No-tracking queries</span></span>

<span data-ttu-id="1dd9d-112">Abfragen ohne Nachverfolgung sind nützlich, wenn die Ergebnisse in einem schreibgeschützten Szenario verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="1dd9d-112">No tracking queries are useful when the results are used in a read-only scenario.</span></span> <span data-ttu-id="1dd9d-113">Sie werden schneller ausgeführt, da keine Informationen für die Änderungsnachverfolgung eingerichtet werden müssen.</span><span class="sxs-lookup"><span data-stu-id="1dd9d-113">They are quicker to execute because there is no need to setup change tracking information.</span></span>

<span data-ttu-id="1dd9d-114">Sie können eine einzelne Abfrage ändern, sodass sie keine Nachverfolgung ausführt:</span><span class="sxs-lookup"><span data-stu-id="1dd9d-114">You can swap an individual query to be no-tracking:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

<span data-ttu-id="1dd9d-115">Sie können das Standardnachverfolgungsverhalten auch auf der Ebene der Kontextinstanz ändern:</span><span class="sxs-lookup"><span data-stu-id="1dd9d-115">You can also change the default tracking behavior at the context instance level:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> <span data-ttu-id="1dd9d-116">Abfragen ohne Nachverfolgung führen weiterhin die Identitätsauflösung in der ausführenden Abfrage aus.</span><span class="sxs-lookup"><span data-stu-id="1dd9d-116">No tracking queries still perform identity resolution within the excuting query.</span></span> <span data-ttu-id="1dd9d-117">Wenn die Ergebnisse mehrmals dieselbe Entität enthalten, wird die gleiche Instanz der Entitätsklasse für jedes Vorkommen in den Ergebnissen zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="1dd9d-117">If the result set contains the same entity multiple times, the same instance of the entity class will be returned for each occurrence in the result set.</span></span> <span data-ttu-id="1dd9d-118">Allerdings werden schwache Verweise verwendet, um bereits zurückgegebene Entitäten nachzuverfolgen.</span><span class="sxs-lookup"><span data-stu-id="1dd9d-118">However, weak references are used to keep track of entities that have already been returned.</span></span> <span data-ttu-id="1dd9d-119">Wenn ein vorheriges Ergebnis mit derselben Identität den gültigen Bereich verlässt und die Garbage Collection ausgeführt wird, können Sie eine neue Entitätsinstanz abrufen.</span><span class="sxs-lookup"><span data-stu-id="1dd9d-119">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span> <span data-ttu-id="1dd9d-120">Weitere Informationen finden Sie unter [Funktionsweise von Abfragen](overview.md).</span><span class="sxs-lookup"><span data-stu-id="1dd9d-120">For more information, see [How Query Works](overview.md).</span></span>

## <a name="tracking-and-projections"></a><span data-ttu-id="1dd9d-121">Nachverfolgung und Projektionen</span><span class="sxs-lookup"><span data-stu-id="1dd9d-121">Tracking and projections</span></span>

<span data-ttu-id="1dd9d-122">Selbst wenn der Ergebnistyp der Abfrage kein Entitätstyp ist, wird die Nachverfolgung standardmäßig ausgeführt, sofern das Ergebnis Entitätstypen enthält.</span><span class="sxs-lookup"><span data-stu-id="1dd9d-122">Even if the result type of the query isn't an entity type, if the result contains entity types they will still be tracked by default.</span></span> <span data-ttu-id="1dd9d-123">In der folgenden Abfrage, die einen anonymen Typ zurückgibt, werden die Instanzen von `Blog` im Ergebnis nachverfolgt.</span><span class="sxs-lookup"><span data-stu-id="1dd9d-123">In the following query, which returns an anonymous type, the instances of `Blog` in the result set will be tracked.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=7)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Blog = b,
                Posts = b.Posts.Count()
            });
}
```

<span data-ttu-id="1dd9d-124">Wenn die Ergebnisse keine Entitätstypen enthalten, wird keine Nachverfolgung ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="1dd9d-124">If the result set does not contain any entity types, then no tracking is performed.</span></span> <span data-ttu-id="1dd9d-125">In der folgenden Abfrage, die einen anonymen Typ mit einigen Werten der Entität zurückgibt (aber keine Instanzen des aktuellen Entitätstyps), wird keine Nachverfolgung ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="1dd9d-125">In the following query, which returns an anonymous type with some of the values from the entity (but no instances of the actual entity type), there is no tracking performed.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Id = b.BlogId,
                Url = b.Url
            });
}
```
