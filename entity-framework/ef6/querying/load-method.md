---
title: Die Load-Methode – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
ms.openlocfilehash: bcea8ab2477f44281cd5de824457a72a84ccc766
ms.sourcegitcommit: 4a795285004612ac03ab26532ac09ca333cb4c8f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50123816"
---
# <a name="the-load-method"></a>Load-Methode
Es gibt mehrere Szenarien, in denen Sie Entitäten aus der Datenbank in den Kontext zu laden, ohne direkt mit diesen Entitäten übernehmen möchten. Ein gutes Beispiel hierfür ist Laden von Entitäten für die Datenbindung unter [lokale Daten](~/ef6/querying/local-data.md). Eine allgemeine Möglichkeit hierzu ist eine LINQ-Abfrage schreiben, und rufen Sie dann darauf, nur für die erstellte Liste verwirft sofort "ToList". Die Load-Erweiterungsmethode funktioniert genauso wie "ToList", außer dass es die Erstellung der Liste der vollständig vermeidet.  

Die in diesem Thema dargestellten Techniken gelten jeweils für Modelle, die mit Code First und dem EF-Designer erstellt wurden.  

Hier sind zwei Beispiele für die Verwendung der Last. Die erste stammt aus einer Windows Forms-datenbindungsanwendung, in der Last verwendet für Entitäten, die vor der Bindung an eine lokale Sammlung, Abfragen, wie in beschrieben [lokale Daten](~/ef6/querying/local-data.md):  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

Im zweiten Beispiel wird mithilfe von Load Laden Sie eine gefilterte Auflistung von verknüpften Entitäten in beschriebenen [Laden von verknüpften Entitäten](~/ef6/querying/related-data.md):  

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
