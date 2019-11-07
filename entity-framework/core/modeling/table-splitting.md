---
title: Tabellen Aufteilung-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
ms.assetid: 0EC2CCE1-BD55-45D8-9EA9-20634987F094
uid: core/modeling/table-splitting
ms.openlocfilehash: a3a2e5842a6c6b4b490084d205a0d44bb46c17ee
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656042"
---
# <a name="table-splitting"></a>Tabellenaufteilung

>[!NOTE]
> Diese Funktion ist neu in EF Core 2,0.

EF Core ermöglicht das Zuordnen von zwei oder mehr Entitäten zu einer einzelnen Zeile. Dies wird als _Tabellen Aufteilung_ oder _Tabellen Freigabe_bezeichnet.

## <a name="configuration"></a>Konfiguration

Zur Verwendung der Tabellen Aufteilung müssen die Entitäts Typen derselben Tabelle zugeordnet werden, die Primärschlüssel müssen denselben Spalten zugeordnet werden, und mindestens eine Beziehung muss zwischen dem Primärschlüssel eines Entitäts Typs und einem anderen in derselben Tabelle konfiguriert werden.

Ein häufiges Szenario für die Tabellen Aufteilung ist die Verwendung einer Teilmenge der Spalten in der Tabelle, um eine höhere Leistung oder Kapselung zu erzielen.

In diesem Beispiel `Order` eine Teilmenge `DetailedOrder`darstellt.

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

Zusätzlich zur erforderlichen Konfiguration werden `Property(o => o.Status).HasColumnName("Status")` aufgerufen, um `DetailedOrder.Status` derselben Spalte wie `Order.Status`zuzuordnen.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

> [!TIP]
> Weitere Informationen finden Sie im [vollständigen Beispiel Projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) .

## <a name="usage"></a>Verwendung

Das Speichern und Abfragen von Entitäten mithilfe von Tabellen Aufteilung erfolgt auf die gleiche Weise wie andere Entitäten. Und ab EF Core 3,0 kann der abhängige Entitäts Verweis `null`werden. Wenn alle von der abhängigen Entität verwendeten Spalten `NULL` die Datenbank ist, wird keine Instanz für Sie erstellt, wenn Sie abgefragt wird. Dies würde auch vorkommen, dass alle Eigenschaften optional sind und auf `null`festgelegt werden, was nicht erwartet wird.

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a>Parallelitäts Token

Wenn einer der Entitäts Typen, die eine Tabelle gemeinsam nutzen, über ein Parallelitäts Token verfügt, muss er in allen anderen Entitäts Typen enthalten sein, um einen veralteten Parallelitäts Token-Wert zu vermeiden, wenn nur eine der Entitäten aktualisiert wird, die derselben Tabelle zugeordnet ist.

Um die Bereitstellung für den verwendeten Code zu vermeiden, ist es möglich, einen im Schatten Zustand zu erstellen.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
