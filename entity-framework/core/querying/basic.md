---
title: 'Grundlegende Abfragen: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ab6e35f1-397f-41c0-9ef4-85aec5466377
uid: core/querying/basic
ms.openlocfilehash: 49daa0d37175244617993cc6e911cbd59d27079f
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921740"
---
# <a name="basic-queries"></a><span data-ttu-id="a119f-102">Standardabfragen</span><span class="sxs-lookup"><span data-stu-id="a119f-102">Basic Queries</span></span>

<span data-ttu-id="a119f-103">Erfahren Sie, wie Sie mit LINQ (Language Integrate Query) Entitäten aus der Datenbank laden.</span><span class="sxs-lookup"><span data-stu-id="a119f-103">Learn how to load entities from the database using Language Integrated Query (LINQ).</span></span>

> [!TIP]  
> <span data-ttu-id="a119f-104">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="a119f-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="101-linq-samples"></a><span data-ttu-id="a119f-105">101 LINQ-Beispiele</span><span class="sxs-lookup"><span data-stu-id="a119f-105">101 LINQ samples</span></span>

<span data-ttu-id="a119f-106">Auf dieser Seite werden einige Beispiele für das Ausführen gängiger Aufgaben mit Entity Framework Core dargestellt.</span><span class="sxs-lookup"><span data-stu-id="a119f-106">This page shows a few examples to achieve common tasks with Entity Framework Core.</span></span> <span data-ttu-id="a119f-107">Eine umfangreiche Anzahl von Beispielen, die zeigen, was mit LINQ ermöglicht wird, finden Sie unter [101 LINQ Samples (101 LINQ-Beispiele)](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).</span><span class="sxs-lookup"><span data-stu-id="a119f-107">For an extensive set of samples showing what is possible with LINQ, see [101 LINQ Samples](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).</span></span>

## <a name="loading-all-data"></a><span data-ttu-id="a119f-108">Laden aller Daten</span><span class="sxs-lookup"><span data-stu-id="a119f-108">Loading all data</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.ToList();
}
```

## <a name="loading-a-single-entity"></a><span data-ttu-id="a119f-109">Laden einer einzelnen Entität</span><span class="sxs-lookup"><span data-stu-id="a119f-109">Loading a single entity</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Single(b => b.BlogId == 1);
}
```

## <a name="filtering"></a><span data-ttu-id="a119f-110">Filtern</span><span class="sxs-lookup"><span data-stu-id="a119f-110">Filtering</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .Where(b => b.Url.Contains("dotnet"))
        .ToList();
}
```
