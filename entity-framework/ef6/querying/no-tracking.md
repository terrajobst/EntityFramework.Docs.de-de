---
title: Abfragen ohne Nachverfolgung EF6
author: divega
ms.date: 10/23/2016
ms.assetid: f80ac260-c2dc-484d-94a3-3424fd862f8b
ms.openlocfilehash: 44d58e14a2550bd08a8edd68b467237f6f5b5978
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414476"
---
# <a name="no-tracking-queries"></a>Abfragen ohne Nachverfolgung
In manchen Fällen möchten Sie Entitäten aus einer Abfrage zurückerhalten, aber diese Entitäten können nicht vom Kontext nachverfolgt werden. Dies kann zu einer besseren Leistung führen, wenn eine große Anzahl von Entitäten in schreibgeschützten Szenarien abgefragt wird. Die in diesem Thema dargestellten Techniken gelten jeweils für Modelle, die mit Code First und dem EF-Designer erstellt wurden.  

Mit der neuen Erweiterungsmethode asnotracking können alle Abfragen auf diese Weise ausgeführt werden. Beispiel:  

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
