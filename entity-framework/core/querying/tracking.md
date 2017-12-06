---
title: "Nachverfolgen von Visual Studio. No-Überwachungsabfragen - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
ms.technology: entity-framework-core
uid: core/querying/tracking
ms.openlocfilehash: 9a22c893f3b1e9991560e25e0252287a2844b39e
ms.sourcegitcommit: 3b6159db8a6c0653f13c7b528367b4e69ac3d51e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2017
---
# <a name="tracking-vs-no-tracking-queries"></a><span data-ttu-id="becc3-102">Nachverfolgen von Visual Studio. No-Überwachungsabfragen</span><span class="sxs-lookup"><span data-stu-id="becc3-102">Tracking vs. No-Tracking Queries</span></span>

<span data-ttu-id="becc3-103">Nachverfolgen von gesteuert, und zwar unabhängig davon, ob Entity Framework Core Informationen zu einer Entitätsinstanz in seiner System zur änderungsnachverfolgung beibehalten werden sollen.</span><span class="sxs-lookup"><span data-stu-id="becc3-103">Tracking behavior controls whether or not Entity Framework Core will keep information about an entity instance in its change tracker.</span></span> <span data-ttu-id="becc3-104">Wenn eine Entität nachverfolgt wird, werden Änderungen erkannt, die in der Entität an die Datenbank während der beibehalten werden `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="becc3-104">If an entity is tracked, any changes detected in the entity will be persisted to the database during `SaveChanges()`.</span></span> <span data-ttu-id="becc3-105">Entity Framework Core wird auch Fixup-Navigationseigenschaften zwischen Entitäten, die aus einer Überwachungsabfrage abgerufen werden und Entitäten, die zuvor in der DbContext-Instanz geladen wurden.</span><span class="sxs-lookup"><span data-stu-id="becc3-105">Entity Framework Core will also fix-up navigation properties between entities that are obtained from a tracking query and entities that were previously loaded into the DbContext instance.</span></span>

> [!TIP]  
> <span data-ttu-id="becc3-106">Sie können anzeigen, dass dieser Artikel [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="becc3-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="tracking-queries"></a><span data-ttu-id="becc3-107">Abfragen zur änderungsnachverfolgung</span><span class="sxs-lookup"><span data-stu-id="becc3-107">Tracking queries</span></span>

<span data-ttu-id="becc3-108">Standardmäßig werden Abfragen, die Entitätstypen zurückgeben nachverfolgen.</span><span class="sxs-lookup"><span data-stu-id="becc3-108">By default, queries that return entity types are tracking.</span></span> <span data-ttu-id="becc3-109">Dies bedeutet, Sie können Änderungen vornehmen, um die Instanzen der Entität und wurden diese Änderungen persistent gespeichert, vom `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="becc3-109">This means you can make changes to those entity instances and have those changes persisted by `SaveChanges()`.</span></span>

<span data-ttu-id="becc3-110">Im folgenden Beispiel die Änderung auf die Bewertung der Blogs erkannt und in der Datenbank während der beibehaltenen `SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="becc3-110">In the following example, the change to the blogs rating will be detected and persisted to the database during `SaveChanges()`.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a><span data-ttu-id="becc3-111">No-Überwachungsabfragen</span><span class="sxs-lookup"><span data-stu-id="becc3-111">No-tracking queries</span></span>

<span data-ttu-id="becc3-112">Keine Überwachungsabfragen sind nützlich, wenn die Ergebnisse in einem Szenario ohne Schreibzugriff verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="becc3-112">No tracking queries are useful when the results are used in a read-only scenario.</span></span> <span data-ttu-id="becc3-113">Sie sind schneller ausgeführt werden, da besteht keine Notwendigkeit für Setup änderungsnachverfolgungsinformationen.</span><span class="sxs-lookup"><span data-stu-id="becc3-113">They are quicker to execute because there is no need to setup change tracking information.</span></span>

<span data-ttu-id="becc3-114">Sie können eine einzelne Abfrage auf ohne-Überwachung werden austauschen:</span><span class="sxs-lookup"><span data-stu-id="becc3-114">You can swap an individual query to be no-tracking:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

<span data-ttu-id="becc3-115">Sie können auch die standardüberwachung Verhalten auf Instanzebene Kontext ändern:</span><span class="sxs-lookup"><span data-stu-id="becc3-115">You can also change the default tracking behavior at the context instance level:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> <span data-ttu-id="becc3-116">Keine Überwachungsabfragen ausführen weiterhin identitätsauflösung innerhalb der Abfrage angefordert.</span><span class="sxs-lookup"><span data-stu-id="becc3-116">No tracking queries still perform identity resolution within the excuting query.</span></span> <span data-ttu-id="becc3-117">Wenn das Resultset auf die gleiche Entität mehrmals enthält, wird dieselbe Instanz der Entitätsklasse für jedes Vorkommen im Resultset zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="becc3-117">If the result set contains the same entity multiple times, the same instance of the entity class will be returned for each occurrence in the result set.</span></span> <span data-ttu-id="becc3-118">Schwache Verweise werden jedoch verwendet, zum Nachverfolgen Entitäten, die bereits zurückgegeben wurden.</span><span class="sxs-lookup"><span data-stu-id="becc3-118">However, weak references are used to keep track of entities that have already been returned.</span></span> <span data-ttu-id="becc3-119">Wenn ein vorheriges Ergebnis mit derselben Identität den Gültigkeitsbereich verlässt, und die Garbagecollection ausführt, können Sie eine neue Entitätsinstanz abrufen.</span><span class="sxs-lookup"><span data-stu-id="becc3-119">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span> <span data-ttu-id="becc3-120">Weitere Informationen finden Sie unter [Funktionsweise der Abfrage](overview.md).</span><span class="sxs-lookup"><span data-stu-id="becc3-120">For more information, see [How Query Works](overview.md).</span></span>

## <a name="tracking-and-projections"></a><span data-ttu-id="becc3-121">Nachverfolgen und für Projektionen</span><span class="sxs-lookup"><span data-stu-id="becc3-121">Tracking and projections</span></span>

<span data-ttu-id="becc3-122">Auch wenn der Ergebnistyp der Abfrage ein Entitätstyp ist, wenn das Ergebnis Entitätstypen enthält werden sie trotzdem standardmäßig nachverfolgt werden.</span><span class="sxs-lookup"><span data-stu-id="becc3-122">Even if the result type of the query isn't an entity type, if the result contains entity types they will still be tracked by default.</span></span> <span data-ttu-id="becc3-123">In der folgenden Abfrage, womit einen anonymen Typ und die Instanzen von `Blog` im Ergebnis Satz nachverfolgt werden.</span><span class="sxs-lookup"><span data-stu-id="becc3-123">In the following query, which returns an anonymous type, the instances of `Blog` in the result set will be tracked.</span></span>

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

<span data-ttu-id="becc3-124">Wenn das Resultset keine Entitätstypen enthält, wird keine Überwachung ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="becc3-124">If the result set does not contain any entity types, then no tracking is performed.</span></span> <span data-ttu-id="becc3-125">In der folgenden Abfrage, womit einen anonymen Typ mit einigen der Werte aus der Entität (jedoch keine Instanzen von den tatsächlichen Entitätstyp), besteht keine Überwachung ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="becc3-125">In the following query, which returns an anonymous type with some of the values from the entity (but no instances of the actual entity type), there is no tracking performed.</span></span>

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
