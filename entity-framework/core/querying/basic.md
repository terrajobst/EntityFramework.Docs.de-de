---
title: Standardabfragen - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ab6e35f1-397f-41c0-9ef4-85aec5466377
ms.technology: entity-framework-core
uid: core/querying/basic
ms.openlocfilehash: 5070faf2aeeffad680e24e7de5a0ffa03a8f0064
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="basic-queries"></a><span data-ttu-id="aba7b-102">Standardabfragen</span><span class="sxs-lookup"><span data-stu-id="aba7b-102">Basic Queries</span></span>

<span data-ttu-id="aba7b-103">Informationen Sie zum Laden von Entitäten aus der Datenbank mithilfe von Language integrieren Query (LINQ).</span><span class="sxs-lookup"><span data-stu-id="aba7b-103">Learn how to load entities from the database using Language Integrate Query (LINQ).</span></span>

> [!TIP]  
> <span data-ttu-id="aba7b-104">Sie können anzeigen, dass dieser Artikel [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="aba7b-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="101-linq-samples"></a><span data-ttu-id="aba7b-105">101 LINQ-Beispiele</span><span class="sxs-lookup"><span data-stu-id="aba7b-105">101 LINQ samples</span></span>

<span data-ttu-id="aba7b-106">Auf dieser Seite werden einige Beispiele für häufige Aufgaben mit Entity Framework Core zu erzielen.</span><span class="sxs-lookup"><span data-stu-id="aba7b-106">This page shows a few examples to achieve common tasks with Entity Framework Core.</span></span> <span data-ttu-id="aba7b-107">Ein umfangreicher Satz von Beispielen zeigt, was mit LINQ möglich ist, finden Sie unter [101 LINQ-Beispiele](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).</span><span class="sxs-lookup"><span data-stu-id="aba7b-107">For an extensive set of samples showing what is possible with LINQ, see [101 LINQ Samples](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).</span></span>

## <a name="loading-all-data"></a><span data-ttu-id="aba7b-108">Laden alle Daten</span><span class="sxs-lookup"><span data-stu-id="aba7b-108">Loading all data</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.ToList();
}
```

## <a name="loading-a-single-entity"></a><span data-ttu-id="aba7b-109">Laden eine einzelne Entität</span><span class="sxs-lookup"><span data-stu-id="aba7b-109">Loading a single entity</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Single(b => b.BlogId == 1);
}
```

## <a name="filtering"></a><span data-ttu-id="aba7b-110">Filtern</span><span class="sxs-lookup"><span data-stu-id="aba7b-110">Filtering</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .Where(b => b.Url.Contains("dotnet"))
        .ToList();
}
```
