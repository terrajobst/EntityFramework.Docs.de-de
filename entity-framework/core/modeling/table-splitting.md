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
# <a name="table-splitting"></a><span data-ttu-id="bf7a0-102">Tabellenaufteilung</span><span class="sxs-lookup"><span data-stu-id="bf7a0-102">Table Splitting</span></span>

>[!NOTE]
> <span data-ttu-id="bf7a0-103">Dieses Feature ist neu in EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="bf7a0-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="bf7a0-104">EF Core kann zwei oder mehr Entitäten auf eine einzelne Zeile zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="bf7a0-104">EF Core allows to map two or more entities to a single row.</span></span> <span data-ttu-id="bf7a0-105">Dies wird als bezeichnet _Tabelle aufteilen_ oder _Tabelle Freigabe_.</span><span class="sxs-lookup"><span data-stu-id="bf7a0-105">This is called _table splitting_ or _table sharing_.</span></span>

## <a name="configuration"></a><span data-ttu-id="bf7a0-106">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="bf7a0-106">Configuration</span></span>

<span data-ttu-id="bf7a0-107">Um verwenden die tabellenaufteilung, die die Entitätstypen derselben Tabelle zugeordnet werden müssen, haben Sie primären Schlüssel, der den gleichen Spalten zugeordnet und mindestens eine Beziehung zwischen den primären Schlüssel von einem Entitätstyp und einem anderen in derselben Tabelle konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="bf7a0-107">To use table splitting the entity types need to be mapped to the same table, have the primary keys mapped to the same columns and at least one relationship configured between the primary key of one entity type and another in the same table.</span></span>

<span data-ttu-id="bf7a0-108">Ein häufiges Szenario für die tabellenaufteilung wird nur eine Teilmenge der Spalten in der Tabelle für mehr Leistung oder Kapselung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="bf7a0-108">A common scenario for table splitting is using only a subset of the columns in the table for greater performance or encapsulation.</span></span>

<span data-ttu-id="bf7a0-109">In diesem Beispiel `Order` stellt eine Teilmenge der `DetailedOrder`.</span><span class="sxs-lookup"><span data-stu-id="bf7a0-109">In this example `Order` represents a subset of `DetailedOrder`.</span></span>

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

<span data-ttu-id="bf7a0-110">Zusätzlich zu die erforderliche Konfiguration rufen wir `HasBaseType((string)null)` Zuordnung vermeiden `DetailedOrder` in der gleichen Hierarchie wie `Order`.</span><span class="sxs-lookup"><span data-stu-id="bf7a0-110">In addition to the required configuration we call `HasBaseType((string)null)` to avoid mapping `DetailedOrder` in the same hierarchy as `Order`.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

<span data-ttu-id="bf7a0-111">Finden Sie unter den [vollständigen Beispielprojekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) für weiteren Kontext.</span><span class="sxs-lookup"><span data-stu-id="bf7a0-111">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) for more context.</span></span>

## <a name="usage"></a><span data-ttu-id="bf7a0-112">Verwendung</span><span class="sxs-lookup"><span data-stu-id="bf7a0-112">Usage</span></span>

<span data-ttu-id="bf7a0-113">Speichern und Abfragen von Entitäten, die über tabellenaufteilung erfolgt auf dieselbe Weise wie andere Entitäten, der einzige Unterschied besteht darin, dass alle Entitäten, die Freigabe einer Zeile für den Einfügevorgang nachverfolgt werden müssen.</span><span class="sxs-lookup"><span data-stu-id="bf7a0-113">Saving and querying entities using table splitting is done in the same way as other entities, the only difference is that all entities sharing a row must be tracked for the insert.</span></span>

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a><span data-ttu-id="bf7a0-114">Parallelitätstoken</span><span class="sxs-lookup"><span data-stu-id="bf7a0-114">Concurrency tokens</span></span>

<span data-ttu-id="bf7a0-115">Wenn keines der Entitätstypen, die gemeinsame Nutzung einer Tabelle hat ein parallelitätstoken muss es enthalten in alle anderen Entitätstypen, um einen Tokenwert veraltete Parallelität zu vermeiden, wenn nur eine der Entitäten zugeordnet, die der gleichen Tabelle aktualisiert wird.</span><span class="sxs-lookup"><span data-stu-id="bf7a0-115">If any of the entity types sharing a table has a concurrency token then it must be included in all other entity types to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="bf7a0-116">Um zu vermeiden, eine Offenlegung für den konsumierenden Code kann die erstellen Sie einen in die Schatten-Status.</span><span class="sxs-lookup"><span data-stu-id="bf7a0-116">To avoid exposing it to the consuming code it's possible the create one in shadow-state.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]