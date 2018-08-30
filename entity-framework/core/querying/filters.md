---
title: 'Globale Abfragefilter: EF Core'
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 6b7a4069917c93015a218c131ff0d0a3920fb69d
ms.sourcegitcommit: 4467032fd6ca223e5965b59912d74cf88a1dd77f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2018
ms.locfileid: "42447648"
---
# <a name="global-query-filters"></a><span data-ttu-id="cf9e8-102">Globale Abfragefilter</span><span class="sxs-lookup"><span data-stu-id="cf9e8-102">Global Query Filters</span></span>

<span data-ttu-id="cf9e8-103">Globale Abfragefilter sind LINQ-Abfrageprädikate (ein boolescher Ausdruck, der in der Regel an den LINQ-Abfrageoperator *Where* übergeben wird), die auf Entitätstypen im Metadatenmodell (normalerweise in *OnModelCreating*) angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="cf9e8-103">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="cf9e8-104">Solche Filter werden automatisch auf alle diese Entitätstypen betreffenden LINQ-Abfragen (z.B. indirekt referenzierte Entitätstypen) angewendet, beispielsweise durch include-Verweise oder direkte Verweise auf Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="cf9e8-104">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="cf9e8-105">Zu den häufigsten Anwendungsfällen dieses Features zählen Folgende:</span><span class="sxs-lookup"><span data-stu-id="cf9e8-105">Some common applications of this feature are:</span></span>

* <span data-ttu-id="cf9e8-106">**Vorläufiges Löschen:** Ein Entitätstyp definiert eine *IsDeleted*-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="cf9e8-106">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="cf9e8-107">**Mehrinstanzenfähigkeit:** Ein Entitätstyp definiert eine *TenantId*-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="cf9e8-107">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="cf9e8-108">Beispiel</span><span class="sxs-lookup"><span data-stu-id="cf9e8-108">Example</span></span>

<span data-ttu-id="cf9e8-109">Im folgenden Beispiel wird in einem einfachen Blogmodell dargestellt, wie globale Abfragefilter zum Implementieren des Abfrageverhaltens für das vorläufige Löschen und die Mehrinstanzenfähigkeit verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="cf9e8-109">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="cf9e8-110">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/QueryFilters) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="cf9e8-110">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="cf9e8-111">Definieren Sie zunächst die Entitäten:</span><span class="sxs-lookup"><span data-stu-id="cf9e8-111">First, define the entities:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="cf9e8-112">Beachten Sie die Deklaration eines __tenantId_-Felds in der Entität _Blog_.</span><span class="sxs-lookup"><span data-stu-id="cf9e8-112">Note the declaration of a __tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="cf9e8-113">Dies wird dazu verwendet, jede Bloginstanz einem bestimmten Mandanten zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="cf9e8-113">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="cf9e8-114">Außerdem wird eine _IsDeleted_-Eigenschaft auf dem Entitätstyp _Post_ definiert.</span><span class="sxs-lookup"><span data-stu-id="cf9e8-114">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="cf9e8-115">Damit wird nachverfolgt, ob eine _Post_-Instanz „vorläufig gelöscht“ wurde.</span><span class="sxs-lookup"><span data-stu-id="cf9e8-115">This is used to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="cf9e8-116">Das heißt, die Instanz wird als gelöscht gekennzeichnet, ohne dass zugrunde liegende Daten physisch entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="cf9e8-116">That is, the instance is marked as deleted without physically removing the underlying data.</span></span>

<span data-ttu-id="cf9e8-117">Konfigurieren Sie als nächstes die Abfragefilter in _OnModelCreating_ mithilfe der ```HasQueryFilter```-API.</span><span class="sxs-lookup"><span data-stu-id="cf9e8-117">Next, configure the query filters in _OnModelCreating_ using the ```HasQueryFilter``` API.</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="cf9e8-118">Die Prädikatausdrücke, die an _HasQueryFilter_ weitergegeben werden, werden nun automatisch auf alle LINQ-Abfragen dieser Typen angewendet.</span><span class="sxs-lookup"><span data-stu-id="cf9e8-118">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="cf9e8-119">Beachten Sie die Verwendung eines DbContext-Instanzfelds: ```_tenantId``` wird zum Festlegen des aktuellen Mandanten verwendet.</span><span class="sxs-lookup"><span data-stu-id="cf9e8-119">Note the use of a DbContext instance level field: ```_tenantId``` used to set the current tenant.</span></span> <span data-ttu-id="cf9e8-120">Filter auf Modellebene verwenden den Wert der korrekten Kontextinstanz, d.h. der Instanz, die die Abfrage ausführt.</span><span class="sxs-lookup"><span data-stu-id="cf9e8-120">Model-level filters will use the value from the correct context instance (that is, the instance that is executing the query).</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="cf9e8-121">Deaktivieren von Filtern</span><span class="sxs-lookup"><span data-stu-id="cf9e8-121">Disabling Filters</span></span>

<span data-ttu-id="cf9e8-122">Filter können für einzelne LINQ-Abfragen mit dem ```IgnoreQueryFilters()```-Operator deaktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="cf9e8-122">Filters may be disabled for individual LINQ queries by using the ```IgnoreQueryFilters()``` operator.</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="cf9e8-123">Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="cf9e8-123">Limitations</span></span>

<span data-ttu-id="cf9e8-124">Globale Abfragefilter unterliegen den folgenden Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="cf9e8-124">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="cf9e8-125">Filter können keine Verweise auf Navigationseigenschaften enthalten.</span><span class="sxs-lookup"><span data-stu-id="cf9e8-125">Filters cannot contain references to navigation properties.</span></span>
* <span data-ttu-id="cf9e8-126">Filter können nur für den Stammentitätstyp einer Vererbungshierarchie definiert werden.</span><span class="sxs-lookup"><span data-stu-id="cf9e8-126">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
