---
title: Tabellen Aufteilung-EF Core
description: Konfigurieren der Tabellen Aufteilung mithilfe von Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 01/03/2020
uid: core/modeling/table-splitting
ms.openlocfilehash: c38d3ee0efa82f84a1051017ae40c9f3fdd57f1f
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781169"
---
# <a name="table-splitting"></a>Tabellenaufteilung

EF Core ermöglicht das Zuordnen von zwei oder mehr Entitäten zu einer einzelnen Zeile. Dies wird als _Tabellen Aufteilung_ oder _Tabellen Freigabe_bezeichnet.

## <a name="configuration"></a>-Konfiguration

Zur Verwendung der Tabellen Aufteilung müssen die Entitäts Typen derselben Tabelle zugeordnet werden, die Primärschlüssel müssen denselben Spalten zugeordnet werden, und mindestens eine Beziehung muss zwischen dem Primärschlüssel eines Entitäts Typs und einem anderen in derselben Tabelle konfiguriert werden.

Ein häufiges Szenario für die Tabellen Aufteilung ist die Verwendung einer Teilmenge der Spalten in der Tabelle, um eine höhere Leistung oder Kapselung zu erzielen.

In diesem Beispiel `Order` eine Teilmenge `DetailedOrder`darstellt.

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

Zusätzlich zur erforderlichen Konfiguration werden `Property(o => o.Status).HasColumnName("Status")` aufgerufen, um `DetailedOrder.Status` derselben Spalte wie `Order.Status`zuzuordnen.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting)]

> [!TIP]
> Weitere Informationen finden Sie im [vollständigen Beispiel Projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) .

## <a name="usage"></a>Verwendungs-

Das Speichern und Abfragen von Entitäten mithilfe von Tabellen Aufteilung erfolgt auf dieselbe Weise wie andere Entitäten:

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="optional-dependent-entity"></a>Optionale abhängige Entität

> [!NOTE]
> Diese Funktion wurde in EF Core 3,0 eingeführt.

Wenn alle Spalten, die von einer abhängigen Entität verwendet werden, in der Datenbank `NULL` werden, wird keine Instanz für Sie erstellt, wenn Sie abgefragt wird. Dadurch kann eine optionale abhängige Entität modelliert werden, bei der die Beziehungs Eigenschaft für den Prinzipal NULL ist. Beachten Sie, dass auch alle Eigenschaften der abhängigen Eigenschaften optional sind und auf `null`festgelegt werden, was nicht erwartet wird.

## <a name="concurrency-tokens"></a>Parallelitäts Token

Wenn einer der Entitäts Typen, die eine Tabelle gemeinsam nutzen, über ein Parallelitäts Token verfügt, muss dieser auch in allen anderen Entitäts Typen enthalten sein. Dies ist erforderlich, um einen veralteten Parallelitäts Token-Wert zu vermeiden, wenn nur eine Entität aktualisiert wird, die derselben Tabelle zugeordnet ist.

Um zu vermeiden, dass das Parallelitäts Token für den verarbeitenden Code verfügbar gemacht wird, ist es möglich, eine als [Schatten Eigenschaft](xref:core/modeling/shadow-properties)zu erstellen:

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
