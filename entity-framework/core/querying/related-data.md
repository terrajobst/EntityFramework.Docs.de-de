---
title: "Laden von verknüpften Daten - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: cd26bd2e6f85083f73d97b1356d0ba38f53e0b8f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="loading-related-data"></a>Laden von verknüpften Daten

Entity Framework Core können Sie die Navigationseigenschaften zum Laden verknüpfter Entitäten im Modell zu verwenden. Es gibt drei allgemeine O/RM-Muster, die zum Laden von verknüpften Daten verwendet.
* **Unverzüglichem Laden** bedeutet, dass die verknüpften Daten aus der Datenbank als Teil der ursprünglichen Abfrage geladen werden.
* **Explizites Laden** bedeutet, dass die verwandten Daten explizit aus der Datenbank zu einem späteren Zeitpunkt geladen werden.
* **Verzögertes Laden** bedeutet, dass die verwandten Daten transparent aus der Datenbank geladen werden, wenn die Navigationseigenschaft zugegriffen wird. Verzögertes Laden ist noch nicht mit EF Core möglich.

> [!TIP]  
> Sie können anzeigen, dass dieser Artikel [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) auf GitHub.

## <a name="eager-loading"></a>Eager Loading

Sie können die `Include` Methode, um verwandte Daten angeben, die in den Ergebnissen enthalten sein. Im folgenden Beispiel müssen die Blogs, die in den Ergebnissen zurückgegeben werden ihre `Posts` Eigenschaft, die mit den zugehörigen Beiträgen aufgefüllt.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Entity Framework Core wird automatisch ein Fixup-Navigationseigenschaften für alle anderen Entitäten, die zuvor in die Kontextinstanz geladen wurden. Damit auch, wenn Sie die Daten für eine Navigationseigenschaft explizit nicht einschließen, die Eigenschaft kann noch immer aufgefüllt werden, wenn einige oder alle verknüpften Entitäten wurden zuvor geladen.


Sie können aufeinander bezogene Daten in mehreren Beziehungen in einer einzelnen Abfrage einschließen.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>Einschließlich mehrere Ebenen

Drilldown kann über Beziehungen zu mehrere Stufen der verknüpften Daten mithilfe der `ThenInclude` Methode. Im folgende Beispiel lädt alle Blogs, Verwandte Beiträge und der Autor jedes Post.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

Sie können mehrere Aufrufe verkettet `ThenInclude` zu fortfahren, einschließlich der weiteren Ebenen verknüpfter Daten.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

Sie können diese Option, um verwandte Daten aus mehreren Ebenen und mehrere Stämme in derselben Abfrage umfassen alle kombinieren.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

Möglicherweise sollen mehrere verknüpfte Entitäten für eine der Entitäten, die gerade enthalten ist. Beispielsweise beim Abfragen von `Blog`e, Sie enthalten `Posts` , und klicken Sie dann beide einschließen möchten die `Author` und `Tags` von der `Posts`. Zu diesem Zweck müssen Sie angeben, angefangen beim Stamm Pfad enthalten. Beispielsweise `Blog -> Posts -> Author` und `Blog -> Posts -> Tags`. Dies bedeutet nicht, dass erhalten Sie redundante Joins, in den meisten Fällen, die EF zu konsolidieren, werden, die Joins beim Generieren von SQL.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="ignored-includes"></a>Ignoriert enthält

Wenn Sie die Abfrage ändern, sodass sie nicht mehr Instanzen des Entitätstyps, die zurückgibt mit der Abfrage wurde gestartet, werden die Operatoren Include ignoriert.

Im folgenden Beispiel werden die Include-Operatoren basierend auf den `Blog`, aber dann die `Select` Operator wird verwendet, so ändern Sie die Abfrage aus, um einen anonymen Typ zurückgegeben. Die Include-Operatoren haben in diesem Fall keine Auswirkungen.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

EF Core protokolliert standardmäßig eine Warnung bei enthalten Operatoren werden ignoriert. Finden Sie unter [Protokollierung](../miscellaneous/logging.md) für Weitere Informationen zur Anzeige von Protokollausgaben. Sie können das Verhalten ändern, wenn ein Operator Include ignoriert wird, entweder auslösen oder Unternehmen Sie nichts. Dies erfolgt, wenn Sie die Optionen für den Kontext - in der Regel in einrichten `DbContext.OnConfiguring`, oder im `Startup.cs` bei Verwendung von ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a>Explizites Laden

> [!NOTE]  
> Diese Funktion wurde in der EF Core 1.1 eingeführt.

Sie können explizit laden, eine Navigationseigenschaft über die `DbContext.Entry(...)` API.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

Sie können auch explizit eine Navigationseigenschaft laden, durch Ausführen einer separaten Abfrage, die die verknüpften Entitäten zurückgibt. Wenn die änderungsnachverfolgung aktiviert ist, beim Laden einer Entität EF Core automatisch legen Sie die Navigationseigenschaften der neu geladenen workflowprozessinstanz zum Verweisen auf alle Entitäten, die bereits geladen, und legen Sie die Navigationseigenschaften der bereits geladene Entitäten zum Verweisen auf die Entität neu geladen.

### <a name="querying-related-entities"></a>Abfragen von verknüpften Entitäten

Außerdem erhalten Sie eine LINQ-Abfrage, die den Inhalt einer Navigationseigenschaft darstellt.

Dadurch können Sie Aktionen wie die aggregate-Operator über die verknüpften Entitäten ausführen, ohne sie in den Arbeitsspeicher laden ausgeführt werden.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

Sie können auch filtern, welche verknüpften Entitäten in den Arbeitsspeicher geladen werden.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>Lazy Loading

Verzögertes Laden wird noch von EF Core nicht unterstützt. Sehen Sie die [verzögertes Laden Element auf unseren nachholbedarf](https://github.com/aspnet/EntityFramework/issues/3797) dieses Feature zu verfolgen.

## <a name="related-data-and-serialization"></a>Daten und Serialisierung

Da EF Core wird automatisch ein Fixup-Navigationseigenschaften, Sie Zyklen in Ihre Objektdiagramm ergeben können. Z. B. beim Laden eines Blogs und bezieht sich Beiträge in einem Blog Objekt führt, die eine Auflistung von Beiträgen verweist. Jedes dieser Beiträge weist einen Verweis zurück auf den Blog.

Einige serialisierungsframeworks lassen sich nicht auf solche Zyklen aus. Json.NET wird z. B. die folgende Ausnahme ausgelöst, ist ein Zyklus auftritt.

> Newtonsoft.Json.JsonSerializationException: Self-Service verweisen auf Schleife, die mit Typ 'MyApplication.Models.Blog' für Eigenschaft "Blog" erkannt.

Wenn Sie ASP.NET Core verwenden, können Sie Json.NET um Zyklen zu ignorieren, die im Objektdiagramm gefunden konfigurieren. Dies erfolgt in der `ConfigureServices(...)` Methode in `Startup.cs`.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```
