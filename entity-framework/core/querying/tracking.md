---
title: 'Abfragen mit Nachverfolgung im Vergleich zu Abfragen ohne Nachverfolgung: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
uid: core/querying/tracking
ms.openlocfilehash: d93be5c2b727d8fbaddd103f8f367c699ae80a7c
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921653"
---
# <a name="tracking-vs-no-tracking-queries"></a>Abfragen mit Nachverfolgung im Vergleich zu Abfragen ohne Nachverfolgung

Das Nachverfolgungsverhalten steuert, ob Entity Framework Core Informationen über eine Entitätsinstanz in der Änderungsprotokollierung speichert. Wenn eine Entität nachverfolgt wird, werden alle Änderungen, die in der Entität erkannt werden, während `SaveChanges()` in der Datenbank beibehalten. Entity Framework Core korrigiert zusätzlich Navigationseigenschaften zwischen Entitäten, die von einer Abfrage mit Nachverfolgung abgerufen werden, und Entitäten, die zuvor in die DbContext-Instanz geladen wurden.

> [!TIP]  
> Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) finden Sie auf GitHub.

## <a name="tracking-queries"></a>Abfragen mit Nachverfolgung

Abfragen, die Entitätstypen zurückgeben, verfügen standardmäßig über Nachverfolgung. Das bedeutet, Sie können Änderungen an diesen Entitätsinstanzen vornehmen, und diese Änderungen werden von `SaveChanges()` beibehalten.

Im folgenden Beispiel wird die Änderung an der Bewertung des Blogs erkannt und während `SaveChanges()` in der Datenbank beibehalten.

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a>Abfragen ohne Nachverfolgung

Abfragen ohne Nachverfolgung sind nützlich, wenn die Ergebnisse in einem schreibgeschützten Szenario verwendet werden. Sie werden schneller ausgeführt, da keine Informationen für die Änderungsnachverfolgung eingerichtet werden müssen.

Sie können eine einzelne Abfrage ändern, sodass sie keine Nachverfolgung ausführt:

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

Sie können das Standardnachverfolgungsverhalten auch auf der Ebene der Kontextinstanz ändern:

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> Abfragen ohne Nachverfolgung führen weiterhin die Identitätsauflösung in der ausführenden Abfrage aus. Wenn die Ergebnisse mehrmals dieselbe Entität enthalten, wird die gleiche Instanz der Entitätsklasse für jedes Vorkommen in den Ergebnissen zurückgegeben. Allerdings werden schwache Verweise verwendet, um bereits zurückgegebene Entitäten nachzuverfolgen. Wenn ein vorheriges Ergebnis mit derselben Identität den gültigen Bereich verlässt und die Garbage Collection ausgeführt wird, können Sie eine neue Entitätsinstanz abrufen. Weitere Informationen finden Sie unter [Funktionsweise von Abfragen](overview.md).

## <a name="tracking-and-projections"></a>Nachverfolgung und Projektionen

Selbst wenn der Ergebnistyp der Abfrage kein Entitätstyp ist, wird die Nachverfolgung standardmäßig ausgeführt, sofern das Ergebnis Entitätstypen enthält. In der folgenden Abfrage, die einen anonymen Typ zurückgibt, werden die Instanzen von `Blog` im Ergebnis nachverfolgt.

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs?highlight=7)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Blog = b,
                Posts = b.Posts.Count()
            });
}
```

Wenn die Ergebnisse keine Entitätstypen enthalten, wird keine Nachverfolgung ausgeführt. In der folgenden Abfrage, die einen anonymen Typ mit einigen Werten der Entität zurückgibt (aber keine Instanzen des aktuellen Entitätstyps), wird keine Nachverfolgung ausgeführt.

<!-- [!code-csharp[Main](samples/core/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Select(b =>
            new
            {
                Id = b.BlogId,
                Url = b.Url
            });
}
```
