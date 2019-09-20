---
title: Tabellen Aufteilung-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
ms.assetid: 0EC2CCE1-BD55-45D8-9EA9-20634987F094
uid: core/modeling/table-splitting
ms.openlocfilehash: 684fcfbb66debfd1b89e23c8aaf0a32909378c6b
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149192"
---
# <a name="table-splitting"></a>Tabellen Aufteilung

>[!NOTE]
> Diese Funktion ist neu in EF Core 2,0.

EF Core ermöglicht das Zuordnen von zwei oder mehr Entitäten zu einer einzelnen Zeile. Dies wird als _Tabellen Aufteilung_ oder _Tabellen Freigabe_bezeichnet.

## <a name="configuration"></a>Konfiguration

Zur Verwendung der Tabellen Aufteilung müssen die Entitäts Typen derselben Tabelle zugeordnet werden, die Primärschlüssel müssen denselben Spalten zugeordnet werden, und mindestens eine Beziehung muss zwischen dem Primärschlüssel eines Entitäts Typs und einem anderen in derselben Tabelle konfiguriert werden.

Ein häufiges Szenario für die Tabellen Aufteilung ist die Verwendung einer Teilmenge der Spalten in der Tabelle, um eine höhere Leistung oder Kapselung zu erzielen.

In diesem Beispiel `Order` stellt eine Teilmenge von `DetailedOrder`dar.

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

Zusätzlich zur erforderlichen Konfiguration wird aufgerufen `Property(o => o.Status).HasColumnName("Status")` `DetailedOrder.Status` , um derselben Spalte wie `Order.Status`zuzuordnen.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

> [!TIP]
> Weitere Informationen finden Sie im [vollständigen Beispiel Projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) .

## <a name="usage"></a>Verwendung

Das Speichern und Abfragen von Entitäten mithilfe von Tabellen Aufteilung erfolgt auf die gleiche Weise wie andere Entitäten. Und beginnend mit EF Core 3,0 kann der abhängige Entitäts Verweis `null`sein. Wenn alle von der abhängigen Entität verwendeten Spalten die Datenbank `NULL` sind, wird bei der Abfrage keine Instanz für Sie erstellt. Dies würde auch vorkommen, dass alle Eigenschaften optional sind und auf `null`festgelegt werden, was nicht erwartet wird.

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a>Parallelitäts Token

Wenn einer der Entitäts Typen, die eine Tabelle gemeinsam nutzen, über ein Parallelitäts Token verfügt, muss er in allen anderen Entitäts Typen enthalten sein, um einen veralteten Parallelitäts Token-Wert zu vermeiden, wenn nur eine der Entitäten aktualisiert wird, die derselben Tabelle zugeordnet ist.

Um die Bereitstellung für den verwendeten Code zu vermeiden, ist es möglich, einen im Schatten Zustand zu erstellen.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]