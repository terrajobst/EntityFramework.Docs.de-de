---
title: Abfragen ohne nachverfolgung – EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: f80ac260-c2dc-484d-94a3-3424fd862f8b
caps.latest.revision: 3
ms.openlocfilehash: 8310f2dab9e7ed9197a8c3e875e47e4f7b72d279
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121129"
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
