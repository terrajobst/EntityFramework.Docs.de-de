---
title: Abfragen ohne nachverfolgung – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: f80ac260-c2dc-484d-94a3-3424fd862f8b
ms.openlocfilehash: dba4127ade9481b40d4fd3c4323532ddfedf6980
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994239"
---
# <a name="no-tracking-queries"></a><span data-ttu-id="a4668-102">Abfragen ohne Nachverfolgung</span><span class="sxs-lookup"><span data-stu-id="a4668-102">No-Tracking Queries</span></span>
<span data-ttu-id="a4668-103">In einigen Fällen empfiehlt es sich um zurückkehren, Entitäten aus einer Abfrage jedoch nicht diese Entitäten vom Kontext nachverfolgt werden.</span><span class="sxs-lookup"><span data-stu-id="a4668-103">Sometimes you may want to get entities back from a query but not have those entities be tracked by the context.</span></span> <span data-ttu-id="a4668-104">Dies kann eine bessere Leistung bei Abfragen für eine große Anzahl von Entitäten in schreibgeschützten Szenarien führen.</span><span class="sxs-lookup"><span data-stu-id="a4668-104">This may result in better performance when querying for large numbers of entities in read-only scenarios.</span></span> <span data-ttu-id="a4668-105">Die in diesem Thema dargestellten Techniken gelten jeweils für Modelle, die mit Code First und dem EF-Designer erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="a4668-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="a4668-106">Eine neue Erweiterungsmethode kann "asnotracking" Abfrage auf diese Weise ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="a4668-106">A new extension method AsNoTracking allows any query to be run in this way.</span></span> <span data-ttu-id="a4668-107">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a4668-107">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Query for all blogs without tracking them
    var blogs1 = context.Blogs.AsNoTracking();

    // Query for some blogs without tracking them
    var blogs2 = context.Blogs
                        .Where(b => b.Name.Contains(".NET"))
                        .AsNoTracking()
                        .ToList();
}
```  
