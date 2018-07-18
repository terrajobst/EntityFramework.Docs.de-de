---
title: Die Load-Methode – EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
caps.latest.revision: 3
ms.openlocfilehash: 83af79220b52de6e3063868fd9bdac56867d49cb
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121144"
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
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();
}
```  
