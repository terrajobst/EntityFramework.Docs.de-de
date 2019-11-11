---
title: Abfragetags – EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 73C7A627-C8E9-452D-9CD5-AFCC8FEFE395
uid: core/querying/tags
ms.openlocfilehash: e8415b237df45ce652dcd152013f4f12a992aed7
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654830"
---
# <a name="query-tags"></a><span data-ttu-id="96e7f-102">Abfragetags</span><span class="sxs-lookup"><span data-stu-id="96e7f-102">Query tags</span></span>

> [!NOTE]
> <span data-ttu-id="96e7f-103">Dieses Feature ist neu in EF Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="96e7f-103">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="96e7f-104">Diese Funktion ermöglicht das Korrelieren zwischen LINQ-Abfragen im Code und generierten SQL-Abfragen, die in Protokollen erfasst werden.</span><span class="sxs-lookup"><span data-stu-id="96e7f-104">This feature helps correlate LINQ queries in code with generated SQL queries captured in logs.</span></span>
<span data-ttu-id="96e7f-105">Sie können eine LINQ-Abfrage mithilfe der neuen `TagWith()`-Methode kommentieren:</span><span class="sxs-lookup"><span data-stu-id="96e7f-105">You annotate a LINQ query using the new `TagWith()` method:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith("This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="96e7f-106">Diese LINQ-Abfrage wird in die folgende SQL-Anweisung übersetzt:</span><span class="sxs-lookup"><span data-stu-id="96e7f-106">This LINQ query is translated to the following SQL statement:</span></span>

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="96e7f-107">`TagWith()` kann mehrmals in der gleichen Abfrage aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="96e7f-107">It's possible to call `TagWith()` many times on the same query.</span></span>
<span data-ttu-id="96e7f-108">Abfragetags sind kumulativ.</span><span class="sxs-lookup"><span data-stu-id="96e7f-108">Query tags are cumulative.</span></span>
<span data-ttu-id="96e7f-109">Betrachten Sie beispielsweise die folgenden Methoden:</span><span class="sxs-lookup"><span data-stu-id="96e7f-109">For example, given the following methods:</span></span>

``` csharp
IQueryable<Friend> GetNearestFriends(Point myLocation) =>
    from f in context.Friends.TagWith("GetNearestFriends")
    orderby f.Location.Distance(myLocation) descending
    select f;

IQueryable<T> Limit<T>(IQueryable<T> source, int limit) =>
    source.TagWith("Limit").Take(limit);
```

<span data-ttu-id="96e7f-110">Diese Abfrage</span><span class="sxs-lookup"><span data-stu-id="96e7f-110">The following query:</span></span>

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).ToList();
```

<span data-ttu-id="96e7f-111">wird übersetzt in</span><span class="sxs-lookup"><span data-stu-id="96e7f-111">Translates to:</span></span>

``` sql
-- GetNearestFriends

-- Limit

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="96e7f-112">Es ist auch möglich, Multi-Linienzeichenfolgen als Abfragetags zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="96e7f-112">It's also possible to use multi-line strings as query tags.</span></span>
<span data-ttu-id="96e7f-113">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="96e7f-113">For example:</span></span>

``` csharp
var results = Limit(GetNearestFriends(myLocation), 25).TagWith(
@"This is a multi-line
string").ToList();
```

<span data-ttu-id="96e7f-114">Dies generiert diese SQL-Anweisung:</span><span class="sxs-lookup"><span data-stu-id="96e7f-114">Produces the following SQL:</span></span>

``` sql
-- GetNearestFriends

-- Limit

-- This is a multi-line
-- string

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

## <a name="known-limitations"></a><span data-ttu-id="96e7f-115">Bekannte Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="96e7f-115">Known limitations</span></span>

<span data-ttu-id="96e7f-116">**Abfragetags sind nicht parametrisierbar:** EF Core behandelt Abfragetags in LINQ-Abfragen immer als Zeichenfolgenliterale, die in der generierten SQL-Anweisung enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="96e7f-116">**Query tags aren't parameterizable:** EF Core always treats query tags in the LINQ query as string literals that are included in the generated SQL.</span></span>
<span data-ttu-id="96e7f-117">Kompilierte Abfragen, die Abfragetags als Parameter verwenden, sind nicht zulässig.</span><span class="sxs-lookup"><span data-stu-id="96e7f-117">Compiled queries that take query tags as parameters aren't allowed.</span></span>
