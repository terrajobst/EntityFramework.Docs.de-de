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
# <a name="table-splitting"></a><span data-ttu-id="7a979-102">Tabellen Aufteilung</span><span class="sxs-lookup"><span data-stu-id="7a979-102">Table Splitting</span></span>

>[!NOTE]
> <span data-ttu-id="7a979-103">Diese Funktion ist neu in EF Core 2,0.</span><span class="sxs-lookup"><span data-stu-id="7a979-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="7a979-104">EF Core ermöglicht das Zuordnen von zwei oder mehr Entitäten zu einer einzelnen Zeile.</span><span class="sxs-lookup"><span data-stu-id="7a979-104">EF Core allows to map two or more entities to a single row.</span></span> <span data-ttu-id="7a979-105">Dies wird als _Tabellen Aufteilung_ oder _Tabellen Freigabe_bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="7a979-105">This is called _table splitting_ or _table sharing_.</span></span>

## <a name="configuration"></a><span data-ttu-id="7a979-106">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="7a979-106">Configuration</span></span>

<span data-ttu-id="7a979-107">Zur Verwendung der Tabellen Aufteilung müssen die Entitäts Typen derselben Tabelle zugeordnet werden, die Primärschlüssel müssen denselben Spalten zugeordnet werden, und mindestens eine Beziehung muss zwischen dem Primärschlüssel eines Entitäts Typs und einem anderen in derselben Tabelle konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="7a979-107">To use table splitting the entity types need to be mapped to the same table, have the primary keys mapped to the same columns and at least one relationship configured between the primary key of one entity type and another in the same table.</span></span>

<span data-ttu-id="7a979-108">Ein häufiges Szenario für die Tabellen Aufteilung ist die Verwendung einer Teilmenge der Spalten in der Tabelle, um eine höhere Leistung oder Kapselung zu erzielen.</span><span class="sxs-lookup"><span data-stu-id="7a979-108">A common scenario for table splitting is using only a subset of the columns in the table for greater performance or encapsulation.</span></span>

<span data-ttu-id="7a979-109">In diesem Beispiel `Order` stellt eine Teilmenge von `DetailedOrder`dar.</span><span class="sxs-lookup"><span data-stu-id="7a979-109">In this example `Order` represents a subset of `DetailedOrder`.</span></span>

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

<span data-ttu-id="7a979-110">Zusätzlich zur erforderlichen Konfiguration wird aufgerufen `Property(o => o.Status).HasColumnName("Status")` `DetailedOrder.Status` , um derselben Spalte wie `Order.Status`zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="7a979-110">In addition to the required configuration we call `Property(o => o.Status).HasColumnName("Status")` to map `DetailedOrder.Status` to the same column as `Order.Status`.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

> [!TIP]
> <span data-ttu-id="7a979-111">Weitere Informationen finden Sie im [vollständigen Beispiel Projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) .</span><span class="sxs-lookup"><span data-stu-id="7a979-111">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) for more context.</span></span>

## <a name="usage"></a><span data-ttu-id="7a979-112">Verwendung</span><span class="sxs-lookup"><span data-stu-id="7a979-112">Usage</span></span>

<span data-ttu-id="7a979-113">Das Speichern und Abfragen von Entitäten mithilfe von Tabellen Aufteilung erfolgt auf die gleiche Weise wie andere Entitäten.</span><span class="sxs-lookup"><span data-stu-id="7a979-113">Saving and querying entities using table splitting is done in the same way as other entities.</span></span> <span data-ttu-id="7a979-114">Und beginnend mit EF Core 3,0 kann der abhängige Entitäts Verweis `null`sein.</span><span class="sxs-lookup"><span data-stu-id="7a979-114">And starting with EF Core 3.0 the dependent entity reference can be `null`.</span></span> <span data-ttu-id="7a979-115">Wenn alle von der abhängigen Entität verwendeten Spalten die Datenbank `NULL` sind, wird bei der Abfrage keine Instanz für Sie erstellt.</span><span class="sxs-lookup"><span data-stu-id="7a979-115">If all of the columns used by the dependent entity are `NULL` is the database then no instance for it will be created when queried.</span></span> <span data-ttu-id="7a979-116">Dies würde auch vorkommen, dass alle Eigenschaften optional sind und auf `null`festgelegt werden, was nicht erwartet wird.</span><span class="sxs-lookup"><span data-stu-id="7a979-116">This would also happen all of the properties are optional and set to `null`, which might not be expected.</span></span>

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a><span data-ttu-id="7a979-117">Parallelitäts Token</span><span class="sxs-lookup"><span data-stu-id="7a979-117">Concurrency tokens</span></span>

<span data-ttu-id="7a979-118">Wenn einer der Entitäts Typen, die eine Tabelle gemeinsam nutzen, über ein Parallelitäts Token verfügt, muss er in allen anderen Entitäts Typen enthalten sein, um einen veralteten Parallelitäts Token-Wert zu vermeiden, wenn nur eine der Entitäten aktualisiert wird, die derselben Tabelle zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="7a979-118">If any of the entity types sharing a table has a concurrency token then it must be included in all other entity types to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="7a979-119">Um die Bereitstellung für den verwendeten Code zu vermeiden, ist es möglich, einen im Schatten Zustand zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="7a979-119">To avoid exposing it to the consuming code it's possible the create one in shadow-state.</span></span>

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]