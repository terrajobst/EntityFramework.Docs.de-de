---
title: Automatisches Erkennen von Änderungen – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a8d1488d-9a54-4623-a76b-e81329ff2756
ms.openlocfilehash: 9af85fd7ca48a14432a1f33c59079fc438ef8810
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490986"
---
# <a name="automatic-detect-changes"></a>Automatisches Erkennen von Änderungen
Bei Verwendung von POCO-Entitäten, die meisten erfolgt die Bestimmung der wie eine Entität geändert wurde (und aus diesem Grund müssen die Updates an die Datenbank gesendet werden), durch den Algorithmus für die Änderungen zu erkennen. Erkennen von Änderungen funktioniert durch das Erkennen von den Unterschieden zwischen der aktuellen Eigenschaftswerte der Entität und die ursprünglichen Eigenschaftswerte, die in einer Momentaufnahme gespeichert werden, wenn die Entität abgefragt oder angefügt wurde. Die in diesem Thema dargestellten Techniken gelten jeweils für Modelle, die mit Code First und dem EF-Designer erstellt wurden.  

Standardmäßig führt Entity Framework erkennt Änderungen automatisch, wenn die folgenden Methoden aufgerufen werden:  

- DbSet.Find  
- DbSet.Local  
- "DbSet.Add"  
- "DbSet.AddRange"
- DbSet.Remove  
- DbSet.RemoveRange
- DbSet.Attach  
- DbContext.SaveChanges  
- DbContext.GetValidationErrors  
- DbContext.Entry  
- DbChangeTracker.Entries  

## <a name="disabling-automatic-detection-of-changes"></a>Deaktivieren die automatischen Erkennung von Änderungen  

Wenn Sie eine Vielzahl von Entities im Kontext Ihrer verfolgen und Sie eine dieser Methoden oft in einer Schleife aufrufen, können Sie erhebliche Leistungssteigerungen durch Deaktivieren der Erkennung von Änderungen für die Dauer der Schleife abrufen. Zum Beispiel:  

``` csharp
using (var context = new BloggingContext())
{
    try
    {
        context.Configuration.AutoDetectChangesEnabled = false;

        // Make many calls in a loop
        foreach (var blog in aLotOfBlogs)
        {
            context.Blogs.Add(blog);
        }
    }
    finally
    {
        context.Configuration.AutoDetectChangesEnabled = true;
    }
}
```  

Vergessen Sie nicht die Erkennung von Änderungen nach der Schleife erneut zu aktivieren – wir haben einen Try/finally verwendet, um sicherzustellen, ist immer wieder aktiviert, auch wenn Code in der Schleife eine Ausnahme auslöst.  

Eine Alternative zu deaktivieren und erneut aktiviert ist, die automatischen Erkennung von Änderungen auf alle Uhrzeiten und entweder Aufrufkontext deaktiviert zu lassen. ChangeTracker.DetectChanges explizit, oder verwenden ändern Änderungsnachverfolgungsproxys sorgfältig. Beide Optionen sind erweiterte und kann es zu schwer erkennbaren Fehlern in Ihrer Anwendung verwenden sie daher mit Vorsicht.  

Bei Bedarf hinzufügen oder Entfernen von zahlreichen Objekten aus einem Kontext, erwägen Sie die Verwendung von "DbSet.AddRange" und DbSet.RemoveRange an. Diese Methoden erkennen Änderungen automatisch nur einmal, wenn die Add- oder Remove-Vorgänge abgeschlossen sind. 
