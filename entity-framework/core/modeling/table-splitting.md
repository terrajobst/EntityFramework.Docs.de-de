---
title: Tabellen Aufteilung-EF Core
description: Konfigurieren der Tabellen Aufteilung mithilfe von Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
uid: core/modeling/table-splitting
ms.openlocfilehash: 0e48c516de43cdc2b54c56f1a96f5e01f9fbbbc4
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824560"
---
# <a name="table-splitting"></a><span data-ttu-id="9fd43-103">Tabellenaufteilung</span><span class="sxs-lookup"><span data-stu-id="9fd43-103">Table Splitting</span></span>

>[!NOTE]
> <span data-ttu-id="9fd43-104">Diese Funktion ist neu in EF Core 2,0.</span><span class="sxs-lookup"><span data-stu-id="9fd43-104">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="9fd43-105">EF Core ermöglicht das Zuordnen von zwei oder mehr Entitäten zu einer einzelnen Zeile.</span><span class="sxs-lookup"><span data-stu-id="9fd43-105">EF Core allows to map two or more entities to a single row.</span></span> <span data-ttu-id="9fd43-106">Dies wird als _Tabellen Aufteilung_ oder _Tabellen Freigabe_bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="9fd43-106">This is called _table splitting_ or _table sharing_.</span></span>

## <a name="configuration"></a><span data-ttu-id="9fd43-107">-Konfiguration</span><span class="sxs-lookup"><span data-stu-id="9fd43-107">Configuration</span></span>

<span data-ttu-id="9fd43-108">Zur Verwendung der Tabellen Aufteilung müssen die Entitäts Typen derselben Tabelle zugeordnet werden, die Primärschlüssel müssen denselben Spalten zugeordnet werden, und mindestens eine Beziehung muss zwischen dem Primärschlüssel eines Entitäts Typs und einem anderen in derselben Tabelle konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="9fd43-108">To use table splitting the entity types need to be mapped to the same table, have the primary keys mapped to the same columns and at least one relationship configured between the primary key of one entity type and another in the same table.</span></span>

<span data-ttu-id="9fd43-109">Ein häufiges Szenario für die Tabellen Aufteilung ist die Verwendung einer Teilmenge der Spalten in der Tabelle, um eine höhere Leistung oder Kapselung zu erzielen.</span><span class="sxs-lookup"><span data-stu-id="9fd43-109">A common scenario for table splitting is using only a subset of the columns in the table for greater performance or encapsulation.</span></span>

<span data-ttu-id="9fd43-110">In diesem Beispiel `Order` eine Teilmenge `DetailedOrder`darstellt.</span><span class="sxs-lookup"><span data-stu-id="9fd43-110">In this example `Order` represents a subset of `DetailedOrder`.</span></span>

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

<span data-ttu-id="9fd43-111">Zusätzlich zur erforderlichen Konfiguration werden `Property(o => o.Status).HasColumnName("Status")` aufgerufen, um `DetailedOrder.Status` derselben Spalte wie `Order.Status`zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="9fd43-111">In addition to the required configuration we call `Property(o => o.Status).HasColumnName("Status")` to map `DetailedOrder.Status` to the same column as `Order.Status`.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

> [!TIP]
> <span data-ttu-id="9fd43-112">Weitere Informationen finden Sie im [vollständigen Beispiel Projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) .</span><span class="sxs-lookup"><span data-stu-id="9fd43-112">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) for more context.</span></span>

## <a name="usage"></a><span data-ttu-id="9fd43-113">Verwendungs-</span><span class="sxs-lookup"><span data-stu-id="9fd43-113">Usage</span></span>

<span data-ttu-id="9fd43-114">Das Speichern und Abfragen von Entitäten mithilfe von Tabellen Aufteilung erfolgt auf die gleiche Weise wie andere Entitäten.</span><span class="sxs-lookup"><span data-stu-id="9fd43-114">Saving and querying entities using table splitting is done in the same way as other entities.</span></span> <span data-ttu-id="9fd43-115">Und ab EF Core 3,0 kann der abhängige Entitäts Verweis `null`werden.</span><span class="sxs-lookup"><span data-stu-id="9fd43-115">And starting with EF Core 3.0 the dependent entity reference can be `null`.</span></span> <span data-ttu-id="9fd43-116">Wenn alle von der abhängigen Entität verwendeten Spalten `NULL` die Datenbank ist, wird keine Instanz für Sie erstellt, wenn Sie abgefragt wird.</span><span class="sxs-lookup"><span data-stu-id="9fd43-116">If all of the columns used by the dependent entity are `NULL` is the database then no instance for it will be created when queried.</span></span> <span data-ttu-id="9fd43-117">Dies würde auch vorkommen, dass alle Eigenschaften optional sind und auf `null`festgelegt werden, was nicht erwartet wird.</span><span class="sxs-lookup"><span data-stu-id="9fd43-117">This would also happen all of the properties are optional and set to `null`, which might not be expected.</span></span>

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a><span data-ttu-id="9fd43-118">Parallelitäts Token</span><span class="sxs-lookup"><span data-stu-id="9fd43-118">Concurrency tokens</span></span>

<span data-ttu-id="9fd43-119">Wenn einer der Entitäts Typen, die eine Tabelle gemeinsam nutzen, über ein Parallelitäts Token verfügt, muss er in allen anderen Entitäts Typen enthalten sein, um einen veralteten Parallelitäts Token-Wert zu vermeiden, wenn nur eine der Entitäten aktualisiert wird, die derselben Tabelle zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="9fd43-119">If any of the entity types sharing a table has a concurrency token then it must be included in all other entity types to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="9fd43-120">Um die Bereitstellung für den verwendeten Code zu vermeiden, ist es möglich, einen im Schatten Zustand zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="9fd43-120">To avoid exposing it to the consuming code it's possible the create one in shadow-state.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
