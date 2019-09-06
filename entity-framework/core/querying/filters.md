---
title: 'Globale Abfragefilter: EF Core'
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: c9bbb8a5889834ea078ddb7e432863b3d0cf2ffe
ms.sourcegitcommit: 0cc9578fd49802789a00c0044b4e57325476ca2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70271456"
---
# <a name="global-query-filters"></a><span data-ttu-id="8ae89-102">Globale Abfragefilter</span><span class="sxs-lookup"><span data-stu-id="8ae89-102">Global Query Filters</span></span>

> [!NOTE]
> <span data-ttu-id="8ae89-103">Dieses Feature wurde in EF Core 2.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="8ae89-103">This feature was introduced in EF Core 2.0.</span></span>

<span data-ttu-id="8ae89-104">Globale Abfragefilter sind LINQ-Abfrageprädikate (ein boolescher Ausdruck, der in der Regel an den LINQ-Abfrageoperator *Where* übergeben wird), die auf Entitätstypen im Metadatenmodell (normalerweise in *OnModelCreating*) angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="8ae89-104">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="8ae89-105">Solche Filter werden automatisch auf alle diese Entitätstypen betreffenden LINQ-Abfragen (z.B. indirekt referenzierte Entitätstypen) angewendet, beispielsweise durch include-Verweise oder direkte Verweise auf Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="8ae89-105">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="8ae89-106">Zu den häufigsten Anwendungsfällen dieses Features zählen Folgende:</span><span class="sxs-lookup"><span data-stu-id="8ae89-106">Some common applications of this feature are:</span></span>

* <span data-ttu-id="8ae89-107">**Vorläufiges Löschen:** Ein Entitätstyp definiert eine *IsDeleted*-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="8ae89-107">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="8ae89-108">**Mehrinstanzenfähigkeit:** Ein Entitätstyp definiert eine *TenantId*-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="8ae89-108">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="8ae89-109">Beispiel</span><span class="sxs-lookup"><span data-stu-id="8ae89-109">Example</span></span>

<span data-ttu-id="8ae89-110">Im folgenden Beispiel wird in einem einfachen Blogmodell dargestellt, wie globale Abfragefilter zum Implementieren des Abfrageverhaltens für das vorläufige Löschen und die Mehrinstanzenfähigkeit verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="8ae89-110">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="8ae89-111">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="8ae89-111">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="8ae89-112">Definieren Sie zunächst die Entitäten:</span><span class="sxs-lookup"><span data-stu-id="8ae89-112">First, define the entities:</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="8ae89-113">Beachten Sie die Deklaration eines _tenantId_-Felds in der Entität _Blog_.</span><span class="sxs-lookup"><span data-stu-id="8ae89-113">Note the declaration of a _tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="8ae89-114">Dies wird dazu verwendet, jede Bloginstanz einem bestimmten Mandanten zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="8ae89-114">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="8ae89-115">Außerdem wird eine _IsDeleted_-Eigenschaft auf dem Entitätstyp _Post_ definiert.</span><span class="sxs-lookup"><span data-stu-id="8ae89-115">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="8ae89-116">Damit wird nachverfolgt, ob eine _Post_-Instanz „vorläufig gelöscht“ wurde.</span><span class="sxs-lookup"><span data-stu-id="8ae89-116">This is used to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="8ae89-117">Das heißt, die Instanz wird als gelöscht gekennzeichnet, ohne dass zugrunde liegende Daten physisch entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="8ae89-117">That is, the instance is marked as deleted without physically removing the underlying data.</span></span>

<span data-ttu-id="8ae89-118">Konfigurieren Sie als nächstes die Abfragefilter in _OnModelCreating_ mithilfe der `HasQueryFilter`-API.</span><span class="sxs-lookup"><span data-stu-id="8ae89-118">Next, configure the query filters in _OnModelCreating_ using the `HasQueryFilter` API.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="8ae89-119">Die Prädikatausdrücke, die an _HasQueryFilter_ weitergegeben werden, werden nun automatisch auf alle LINQ-Abfragen dieser Typen angewendet.</span><span class="sxs-lookup"><span data-stu-id="8ae89-119">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="8ae89-120">Beachten Sie die Verwendung eines DbContext-Instanzfelds: `_tenantId` wird zum Festlegen des aktuellen Mandanten verwendet.</span><span class="sxs-lookup"><span data-stu-id="8ae89-120">Note the use of a DbContext instance level field: `_tenantId` used to set the current tenant.</span></span> <span data-ttu-id="8ae89-121">Filter auf Modellebene verwenden den Wert der korrekten Kontextinstanz, d.h. der Instanz, die die Abfrage ausführt.</span><span class="sxs-lookup"><span data-stu-id="8ae89-121">Model-level filters will use the value from the correct context instance (that is, the instance that is executing the query).</span></span>

> [!NOTE]
> <span data-ttu-id="8ae89-122">Es ist derzeit nicht möglich, mehrere Abfragefilter für dieselbe Entität zu definieren, nur der letzte wird angewendet.</span><span class="sxs-lookup"><span data-stu-id="8ae89-122">It is currently not possible to define multiple query filters on the same entity - only the last one will be applied.</span></span> <span data-ttu-id="8ae89-123">Mithilfe des logischen _AND_-Operators ([`&&` in C#](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)) können jedoch einen einzelnen Filter mit mehreren Bedingungen definieren.</span><span class="sxs-lookup"><span data-stu-id="8ae89-123">However, you can define a single filter with multiple conditions using the logical _AND_ operator ([`&&` in C#](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="8ae89-124">Deaktivieren von Filtern</span><span class="sxs-lookup"><span data-stu-id="8ae89-124">Disabling Filters</span></span>

<span data-ttu-id="8ae89-125">Filter können für einzelne LINQ-Abfragen mit dem `IgnoreQueryFilters()`-Operator deaktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="8ae89-125">Filters may be disabled for individual LINQ queries by using the `IgnoreQueryFilters()` operator.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="8ae89-126">Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="8ae89-126">Limitations</span></span>

<span data-ttu-id="8ae89-127">Globale Abfragefilter unterliegen den folgenden Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="8ae89-127">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="8ae89-128">Filter können keine Verweise auf Navigationseigenschaften enthalten.</span><span class="sxs-lookup"><span data-stu-id="8ae89-128">Filters cannot contain references to navigation properties.</span></span>
* <span data-ttu-id="8ae89-129">Filter können nur für den Stammentitätstyp einer Vererbungshierarchie definiert werden.</span><span class="sxs-lookup"><span data-stu-id="8ae89-129">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
