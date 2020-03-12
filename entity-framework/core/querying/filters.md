---
title: 'Globale Abfragefilter: EF Core'
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: 9262ff7970b0502945480c673315071cbc3f44b9
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413762"
---
# <a name="global-query-filters"></a>Globale Abfragefilter

> [!NOTE]
> Dieses Feature wurde in EF Core 2.0 eingeführt.

Globale Abfragefilter sind LINQ-Abfrageprädikate (ein boolescher Ausdruck, der in der Regel an den LINQ-Abfrageoperator *Where* übergeben wird), die auf Entitätstypen im Metadatenmodell (normalerweise in *OnModelCreating*) angewendet werden. Solche Filter werden automatisch auf alle diese Entitätstypen betreffenden LINQ-Abfragen (z.B. indirekt referenzierte Entitätstypen) angewendet, beispielsweise durch include-Verweise oder direkte Verweise auf Navigationseigenschaften. Zu den häufigsten Anwendungsfällen dieses Features zählen Folgende:

* **Vorläufiges Löschen:** Ein Entitätstyp definiert eine *IsDeleted*-Eigenschaft.
* **Mehrinstanzenfähigkeit:** Ein Entitätstyp definiert eine *TenantId*-Eigenschaft.

## <a name="example"></a>Beispiel

Im folgenden Beispiel wird in einem einfachen Blogmodell dargestellt, wie globale Abfragefilter zum Implementieren des Abfrageverhaltens für das vorläufige Löschen und die Mehrinstanzenfähigkeit verwendet werden.

> [!TIP]
> Das in diesem Artikel verwendete [Beispiel](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) finden Sie auf GitHub.

Definieren Sie zunächst die Entitäten:

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

Beachten Sie die Deklaration eines _tenantId_-Felds in der Entität _Blog_. Dies wird dazu verwendet, jede Bloginstanz einem bestimmten Mandanten zuzuordnen. Außerdem wird eine _IsDeleted_-Eigenschaft auf dem Entitätstyp _Post_ definiert. Damit wird nachverfolgt, ob eine _Post_-Instanz „vorläufig gelöscht“ wurde. Das heißt, die Instanz wird als gelöscht gekennzeichnet, ohne dass zugrunde liegende Daten physisch entfernt werden.

Konfigurieren Sie als nächstes die Abfragefilter in _OnModelCreating_ mithilfe der `HasQueryFilter`-API.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

Die Prädikatausdrücke, die an _HasQueryFilter_ weitergegeben werden, werden nun automatisch auf alle LINQ-Abfragen dieser Typen angewendet.

> [!TIP]
> Beachten Sie die Verwendung eines DbContext-Instanzfelds: `_tenantId` wird zum Festlegen des aktuellen Mandanten verwendet. Filter auf Modellebene verwenden den Wert der korrekten Kontextinstanz, d.h. der Instanz, die die Abfrage ausführt.

> [!NOTE]
> Es ist derzeit nicht möglich, mehrere Abfragefilter für dieselbe Entität zu definieren, nur der letzte wird angewendet. Mithilfe des logischen _AND_-Operators ([`&&` in C#](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)) können jedoch einen einzelnen Filter mit mehreren Bedingungen definieren.

## <a name="disabling-filters"></a>Deaktivieren von Filtern

Filter können für einzelne LINQ-Abfragen mit dem `IgnoreQueryFilters()`-Operator deaktiviert werden.

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>Einschränkungen

Globale Abfragefilter unterliegen den folgenden Einschränkungen:

* Filter können nur für den Stammentitätstyp einer Vererbungshierarchie definiert werden.
