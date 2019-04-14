---
title: Tabelle teilen – EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
ms.assetid: 0EC2CCE1-BD55-45D8-9EA9-20634987F094
uid: core/modeling/table-splitting
ms.openlocfilehash: 4a0bfaf017106a0bfdff084b1c472bdc17459a89
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562586"
---
# <a name="table-splitting"></a>Tabellenaufteilung

>[!NOTE]
> Dieses Feature ist neu in EF Core 2.0.

EF Core kann zwei oder mehr Entitäten auf eine einzelne Zeile zugeordnet. Dies wird als bezeichnet _Tabelle aufteilen_ oder _Tabelle Freigabe_.

## <a name="configuration"></a>Konfiguration

Um verwenden die tabellenaufteilung, die die Entitätstypen derselben Tabelle zugeordnet werden müssen, haben Sie primären Schlüssel, der den gleichen Spalten zugeordnet und mindestens eine Beziehung zwischen den primären Schlüssel von einem Entitätstyp und einem anderen in derselben Tabelle konfiguriert.

Ein häufiges Szenario für die tabellenaufteilung wird nur eine Teilmenge der Spalten in der Tabelle für mehr Leistung oder Kapselung verwendet werden.

In diesem Beispiel `Order` stellt eine Teilmenge der `DetailedOrder`.

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

Zusätzlich zu die erforderliche Konfiguration rufen wir `HasBaseType((string)null)` Zuordnung vermeiden `DetailedOrder` in der gleichen Hierarchie wie `Order`.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

Finden Sie unter den [vollständigen Beispielprojekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) für weiteren Kontext.

## <a name="usage"></a>Verwendung

Speichern und Abfragen von Entitäten, die über tabellenaufteilung erfolgt auf dieselbe Weise wie andere Entitäten, der einzige Unterschied besteht darin, dass alle Entitäten, die Freigabe einer Zeile für den Einfügevorgang nachverfolgt werden müssen.

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a>Parallelitätstoken

Wenn keines der Entitätstypen, die gemeinsame Nutzung einer Tabelle hat ein parallelitätstoken muss es enthalten in alle anderen Entitätstypen, um einen Tokenwert veraltete Parallelität zu vermeiden, wenn nur eine der Entitäten zugeordnet, die der gleichen Tabelle aktualisiert wird.

Um zu vermeiden, eine Offenlegung für den konsumierenden Code kann die erstellen Sie einen in die Schatten-Status.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]