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
# <a name="no-tracking-queries"></a>Abfragen ohne Nachverfolgung
In einigen Fällen empfiehlt es sich um zurückkehren, Entitäten aus einer Abfrage jedoch nicht diese Entitäten vom Kontext nachverfolgt werden. Dies kann eine bessere Leistung bei Abfragen für eine große Anzahl von Entitäten in schreibgeschützten Szenarien führen. Die in diesem Thema dargestellten Techniken gelten jeweils für Modelle, die mit Code First und dem EF-Designer erstellt wurden.  

Eine neue Erweiterungsmethode kann "asnotracking" Abfrage auf diese Weise ausgeführt werden. Zum Beispiel:  

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
