---
title: 'Grundlegende Abfragen: EF Core'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ab6e35f1-397f-41c0-9ef4-85aec5466377
ms.technology: entity-framework-core
uid: core/querying/basic
ms.openlocfilehash: eceac81546b23157611edd530b8b71f71e970c1f
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "42447691"
---
# <a name="basic-queries"></a><span data-ttu-id="04e52-102">Standardabfragen</span><span class="sxs-lookup"><span data-stu-id="04e52-102">Basic Queries</span></span>

<span data-ttu-id="04e52-103">Erfahren Sie, wie Sie mit LINQ (Language Integrate Query) Entitäten aus der Datenbank laden.</span><span class="sxs-lookup"><span data-stu-id="04e52-103">Learn how to load entities from the database using Language Integrated Query (LINQ).</span></span>

> [!TIP]  
> <span data-ttu-id="04e52-104">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="04e52-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="101-linq-samples"></a><span data-ttu-id="04e52-105">101 LINQ-Beispiele</span><span class="sxs-lookup"><span data-stu-id="04e52-105">101 LINQ samples</span></span>

<span data-ttu-id="04e52-106">Auf dieser Seite werden einige Beispiele für das Ausführen gängiger Aufgaben mit Entity Framework Core dargestellt.</span><span class="sxs-lookup"><span data-stu-id="04e52-106">This page shows a few examples to achieve common tasks with Entity Framework Core.</span></span> <span data-ttu-id="04e52-107">Eine umfangreiche Anzahl von Beispielen, die zeigen, was mit LINQ ermöglicht wird, finden Sie unter [101 LINQ Samples (101 LINQ-Beispiele)](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).</span><span class="sxs-lookup"><span data-stu-id="04e52-107">For an extensive set of samples showing what is possible with LINQ, see [101 LINQ Samples](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).</span></span>

## <a name="loading-all-data"></a><span data-ttu-id="04e52-108">Laden aller Daten</span><span class="sxs-lookup"><span data-stu-id="04e52-108">Loading all data</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.ToList();
}
```

## <a name="loading-a-single-entity"></a><span data-ttu-id="04e52-109">Laden einer einzelnen Entität</span><span class="sxs-lookup"><span data-stu-id="04e52-109">Loading a single entity</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Single(b => b.BlogId == 1);
}
```

## <a name="filtering"></a><span data-ttu-id="04e52-110">Filtern</span><span class="sxs-lookup"><span data-stu-id="04e52-110">Filtering</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .Where(b => b.Url.Contains("dotnet"))
        .ToList();
}
```
