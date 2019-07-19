---
title: 'Globale Abfragefilter: EF Core'
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: e1cb9f5afc54aaa12e5880ace606277b00911c06
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306471"
---
# <a name="global-query-filters"></a><span data-ttu-id="88fd5-102">Globale Abfragefilter</span><span class="sxs-lookup"><span data-stu-id="88fd5-102">Global Query Filters</span></span>

> [!NOTE]
> <span data-ttu-id="88fd5-103">Dieses Feature wurde in EF Core 2.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="88fd5-103">This feature was introduced in EF Core 2.0.</span></span>

<span data-ttu-id="88fd5-104">Globale Abfragefilter sind LINQ-Abfrageprädikate (ein boolescher Ausdruck, der in der Regel an den LINQ-Abfrageoperator *Where* übergeben wird), die auf Entitätstypen im Metadatenmodell (normalerweise in *OnModelCreating*) angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="88fd5-104">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="88fd5-105">Solche Filter werden automatisch auf alle diese Entitätstypen betreffenden LINQ-Abfragen (z.B. indirekt referenzierte Entitätstypen) angewendet, beispielsweise durch include-Verweise oder direkte Verweise auf Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="88fd5-105">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="88fd5-106">Zu den häufigsten Anwendungsfällen dieses Features zählen Folgende:</span><span class="sxs-lookup"><span data-stu-id="88fd5-106">Some common applications of this feature are:</span></span>

* <span data-ttu-id="88fd5-107">**Vorläufiges Löschen:** Ein Entitätstyp definiert eine *IsDeleted*-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="88fd5-107">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="88fd5-108">**Mehrinstanzenfähigkeit:** Ein Entitätstyp definiert eine *TenantId*-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="88fd5-108">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="88fd5-109">Beispiel</span><span class="sxs-lookup"><span data-stu-id="88fd5-109">Example</span></span>

<span data-ttu-id="88fd5-110">Im folgenden Beispiel wird in einem einfachen Blogmodell dargestellt, wie globale Abfragefilter zum Implementieren des Abfrageverhaltens für das vorläufige Löschen und die Mehrinstanzenfähigkeit verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="88fd5-110">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="88fd5-111">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="88fd5-111">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="88fd5-112">Definieren Sie zunächst die Entitäten:</span><span class="sxs-lookup"><span data-stu-id="88fd5-112">First, define the entities:</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="88fd5-113">Beachten Sie die Deklaration eines _tenantId_-Felds in der Entität _Blog_.</span><span class="sxs-lookup"><span data-stu-id="88fd5-113">Note the declaration of a _tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="88fd5-114">Dies wird dazu verwendet, jede Bloginstanz einem bestimmten Mandanten zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="88fd5-114">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="88fd5-115">Außerdem wird eine _IsDeleted_-Eigenschaft auf dem Entitätstyp _Post_ definiert.</span><span class="sxs-lookup"><span data-stu-id="88fd5-115">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="88fd5-116">Damit wird nachverfolgt, ob eine _Post_-Instanz „vorläufig gelöscht“ wurde.</span><span class="sxs-lookup"><span data-stu-id="88fd5-116">This is used to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="88fd5-117">Das heißt, die Instanz wird als gelöscht gekennzeichnet, ohne dass zugrunde liegende Daten physisch entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="88fd5-117">That is, the instance is marked as deleted without physically removing the underlying data.</span></span>

<span data-ttu-id="88fd5-118">Konfigurieren Sie als nächstes die Abfragefilter in _OnModelCreating_ mithilfe der `HasQueryFilter`-API.</span><span class="sxs-lookup"><span data-stu-id="88fd5-118">Next, configure the query filters in _OnModelCreating_ using the `HasQueryFilter` API.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="88fd5-119">Die Prädikatausdrücke, die an _HasQueryFilter_ weitergegeben werden, werden nun automatisch auf alle LINQ-Abfragen dieser Typen angewendet.</span><span class="sxs-lookup"><span data-stu-id="88fd5-119">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="88fd5-120">Beachten Sie die Verwendung eines DbContext-Instanzfelds: `_tenantId` wird zum Festlegen des aktuellen Mandanten verwendet.</span><span class="sxs-lookup"><span data-stu-id="88fd5-120">Note the use of a DbContext instance level field: `_tenantId` used to set the current tenant.</span></span> <span data-ttu-id="88fd5-121">Filter auf Modellebene verwenden den Wert der korrekten Kontextinstanz, d.h. der Instanz, die die Abfrage ausführt.</span><span class="sxs-lookup"><span data-stu-id="88fd5-121">Model-level filters will use the value from the correct context instance (that is, the instance that is executing the query).</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="88fd5-122">Deaktivieren von Filtern</span><span class="sxs-lookup"><span data-stu-id="88fd5-122">Disabling Filters</span></span>

<span data-ttu-id="88fd5-123">Filter können für einzelne LINQ-Abfragen mit dem `IgnoreQueryFilters()`-Operator deaktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="88fd5-123">Filters may be disabled for individual LINQ queries by using the `IgnoreQueryFilters()` operator.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="88fd5-124">Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="88fd5-124">Limitations</span></span>

<span data-ttu-id="88fd5-125">Globale Abfragefilter unterliegen den folgenden Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="88fd5-125">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="88fd5-126">Filter können keine Verweise auf Navigationseigenschaften enthalten.</span><span class="sxs-lookup"><span data-stu-id="88fd5-126">Filters cannot contain references to navigation properties.</span></span>
* <span data-ttu-id="88fd5-127">Filter können nur für den Stammentitätstyp einer Vererbungshierarchie definiert werden.</span><span class="sxs-lookup"><span data-stu-id="88fd5-127">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
