---
title: Die Load-Methode-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
ms.openlocfilehash: bcea8ab2477f44281cd5de824457a72a84ccc766
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414494"
---
# <a name="the-load-method"></a>Load-Methode
Es gibt mehrere Szenarien, in denen Sie möglicherweise Entitäten aus der Datenbank in den Kontext laden möchten, ohne sofort etwas mit diesen Entitäten zu tun. Ein gutes Beispiel hierfür ist das Laden von Entitäten für die Datenbindung, wie unter [lokale Daten](~/ef6/querying/local-data.md)beschrieben. Eine gängige Methode hierfür ist das Schreiben einer LINQ-Abfrage und das anschließende aufzurufen der Liste, um die erstellte Liste sofort zu verwerfen. Die Load-Erweiterungsmethode funktioniert genauso wie die-Liste, mit der Ausnahme, dass Sie die vollständige Erstellung der Liste vermeidet.  

Die in diesem Thema dargestellten Techniken gelten jeweils für Modelle, die mit Code First und dem EF-Designer erstellt wurden.  

Im folgenden finden Sie zwei Beispiele für die Verwendung von Load. Der erste wird aus einer Windows Forms Daten Bindungs Anwendung übernommen, bei der Load verwendet wird, um Entitäten vor der Bindung an die lokale Auflistung abzufragen, wie unter [lokale Daten](~/ef6/querying/local-data.md)beschrieben:  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

Das zweite Beispiel zeigt die Verwendung von Load, um eine gefilterte Auflistung verwandter Entitäten zu laden, wie unter Laden von verknüpften [Entitäten](~/ef6/querying/related-data.md)beschrieben:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework"))
        .Load();
}
```  
