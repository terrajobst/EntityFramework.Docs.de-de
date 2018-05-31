---
title: 'Grundlegende Abfragen: EF Core'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ab6e35f1-397f-41c0-9ef4-85aec5466377
ms.technology: entity-framework-core
uid: core/querying/basic
ms.openlocfilehash: 5070faf2aeeffad680e24e7de5a0ffa03a8f0064
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052550"
---
# <a name="basic-queries"></a>Standardabfragen

Erfahren Sie, wie Sie mit LINQ (Language Integrate Query) Entitäten aus der Datenbank laden.

> [!TIP]  
> Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) finden Sie auf GitHub.

## <a name="101-linq-samples"></a>101 LINQ-Beispiele

Auf dieser Seite werden einige Beispiele für das Ausführen gängiger Aufgaben mit Entity Framework Core dargestellt. Eine umfangreiche Anzahl von Beispielen, die zeigen, was mit LINQ ermöglicht wird, finden Sie unter [101 LINQ Samples (101 LINQ-Beispiele)](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).

## <a name="loading-all-data"></a>Laden aller Daten

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.ToList();
}
```

## <a name="loading-a-single-entity"></a>Laden einer einzelnen Entität

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Single(b => b.BlogId == 1);
}
```

## <a name="filtering"></a>Filtern

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .Where(b => b.Url.Contains("dotnet"))
        .ToList();
}
```
