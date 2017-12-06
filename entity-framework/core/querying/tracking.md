---
title: "Nachverfolgen von Visual Studio. No-Überwachungsabfragen - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e17e060c-929f-4180-8883-40c438fbcc01
ms.technology: entity-framework-core
uid: core/querying/tracking
ms.openlocfilehash: 9a22c893f3b1e9991560e25e0252287a2844b39e
ms.sourcegitcommit: 3b6159db8a6c0653f13c7b528367b4e69ac3d51e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2017
---
# <a name="tracking-vs-no-tracking-queries"></a>Nachverfolgen von Visual Studio. No-Überwachungsabfragen

Nachverfolgen von gesteuert, und zwar unabhängig davon, ob Entity Framework Core Informationen zu einer Entitätsinstanz in seiner System zur änderungsnachverfolgung beibehalten werden sollen. Wenn eine Entität nachverfolgt wird, werden Änderungen erkannt, die in der Entität an die Datenbank während der beibehalten werden `SaveChanges()`. Entity Framework Core wird auch Fixup-Navigationseigenschaften zwischen Entitäten, die aus einer Überwachungsabfrage abgerufen werden und Entitäten, die zuvor in der DbContext-Instanz geladen wurden.

> [!TIP]  
> Sie können anzeigen, dass dieser Artikel [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) auf GitHub.

## <a name="tracking-queries"></a>Abfragen zur änderungsnachverfolgung

Standardmäßig werden Abfragen, die Entitätstypen zurückgeben nachverfolgen. Dies bedeutet, Sie können Änderungen vornehmen, um die Instanzen der Entität und wurden diese Änderungen persistent gespeichert, vom `SaveChanges()`.

Im folgenden Beispiel die Änderung auf die Bewertung der Blogs erkannt und in der Datenbank während der beibehaltenen `SaveChanges()`.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.SingleOrDefault(b => b.BlogId == 1);
    blog.Rating = 5;
    context.SaveChanges();
}
```

## <a name="no-tracking-queries"></a>No-Überwachungsabfragen

Keine Überwachungsabfragen sind nützlich, wenn die Ergebnisse in einem Szenario ohne Schreibzugriff verwendet werden. Sie sind schneller ausgeführt werden, da besteht keine Notwendigkeit für Setup änderungsnachverfolgungsinformationen.

Sie können eine einzelne Abfrage auf ohne-Überwachung werden austauschen:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=4)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .AsNoTracking()
        .ToList();
}
```

Sie können auch die standardüberwachung Verhalten auf Instanzebene Kontext ändern:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=3)] -->
``` csharp
using (var context = new BloggingContext())
{
    context.ChangeTracker.QueryTrackingBehavior = QueryTrackingBehavior.NoTracking;

    var blogs = context.Blogs.ToList();
}
```

> [!NOTE]  
> Keine Überwachungsabfragen ausführen weiterhin identitätsauflösung innerhalb der Abfrage angefordert. Wenn das Resultset auf die gleiche Entität mehrmals enthält, wird dieselbe Instanz der Entitätsklasse für jedes Vorkommen im Resultset zurückgegeben. Schwache Verweise werden jedoch verwendet, zum Nachverfolgen Entitäten, die bereits zurückgegeben wurden. Wenn ein vorheriges Ergebnis mit derselben Identität den Gültigkeitsbereich verlässt, und die Garbagecollection ausführt, können Sie eine neue Entitätsinstanz abrufen. Weitere Informationen finden Sie unter [Funktionsweise der Abfrage](overview.md).

## <a name="tracking-and-projections"></a>Nachverfolgen und für Projektionen

Auch wenn der Ergebnistyp der Abfrage ein Entitätstyp ist, wenn das Ergebnis Entitätstypen enthält werden sie trotzdem standardmäßig nachverfolgt werden. In der folgenden Abfrage, womit einen anonymen Typ und die Instanzen von `Blog` im Ergebnis Satz nachverfolgt werden.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs?highlight=7)] -->
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

Wenn das Resultset keine Entitätstypen enthält, wird keine Überwachung ausgeführt. In der folgenden Abfrage, womit einen anonymen Typ mit einigen der Werte aus der Entität (jedoch keine Instanzen von den tatsächlichen Entitätstyp), besteht keine Überwachung ausgeführt.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/Tracking/Sample.cs)] -->
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
