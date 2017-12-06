---
title: Globale Abfragefilter - EF Core
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 4e3c3c99d155f69e00fed99c415f519808ea1a68
ms.sourcegitcommit: 6e379265e4f087fb7cf180c824722c81750554dc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="global-query-filters"></a><span data-ttu-id="d9548-102">Globale Abfragefilter</span><span class="sxs-lookup"><span data-stu-id="d9548-102">Global Query Filters</span></span>

<span data-ttu-id="d9548-103">Globale Abfragefilter werden LINQ-Abfrage-Prädikate (ein boolescher Ausdruck in der Regel übergeben wird, in der LINQ *, in denen* -Abfrage-Operator) auf Entitätstypen im Metadatenmodell angewendet (in der Regel in *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="d9548-103">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="d9548-104">Solche Filter werden automatisch an alle LINQ-Abfragen im Zusammenhang mit diesen Entitätstypen, einschließlich von Entitätstypen indirekt, z. B. durch die Verwendung von Include verwiesen wird oder direkte Navigation Eigenschaftenverweise angewendet.</span><span class="sxs-lookup"><span data-stu-id="d9548-104">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="d9548-105">Einige allgemeine Anwendungen diese Funktion sind:</span><span class="sxs-lookup"><span data-stu-id="d9548-105">Some common applications of this feature are:</span></span>

* <span data-ttu-id="d9548-106">**Vorläufigen löschen** -ein Entitätstyp definiert ein *IsDeleted* Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="d9548-106">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="d9548-107">**Mehrinstanzenfähigkeit** -ein Entitätstyp definiert eine *"tenantid"* Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="d9548-107">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="d9548-108">Beispiel</span><span class="sxs-lookup"><span data-stu-id="d9548-108">Example</span></span>

<span data-ttu-id="d9548-109">Im folgende Beispiel wird gezeigt, wie mit globalen Abfragefilter weiche löschen und mehrinstanzenfähigkeit Abfrage Verhalten in einem einfachen Blogging-Modell implementiert wird.</span><span class="sxs-lookup"><span data-stu-id="d9548-109">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="d9548-110">Sie können anzeigen, dass dieser Artikel [Beispiel](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="d9548-110">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="d9548-111">Zuerst definieren Sie die Entitäten an:</span><span class="sxs-lookup"><span data-stu-id="d9548-111">First, define the entities:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="d9548-112">Beachten Sie die Deklaration einer __"tenantid"_ Feld der _Blog_ Entität.</span><span class="sxs-lookup"><span data-stu-id="d9548-112">Note the declaration of a __tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="d9548-113">Dies wird verwendet, einen bestimmten Mandanten jede Blog-Instanz zugeordnet werden soll.</span><span class="sxs-lookup"><span data-stu-id="d9548-113">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="d9548-114">Wird auch definiert ein _IsDeleted_ Eigenschaft auf die _Post_ Entitätstyp.</span><span class="sxs-lookup"><span data-stu-id="d9548-114">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="d9548-115">Wird verwendet, diese Option, um nachverfolgen, ob ein _Post_ "vorläufig gelöschte" ist die Instanz wurde.</span><span class="sxs-lookup"><span data-stu-id="d9548-115">This is used this to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="d9548-116">D. h. Die Instanz wird als gelöschte Withouth physisch entfernt die zugrunde liegenden Daten gekennzeichnet.</span><span class="sxs-lookup"><span data-stu-id="d9548-116">I.e. The instance is marked as deleted withouth physically removing the underlying data.</span></span>

<span data-ttu-id="d9548-117">Als Nächstes konfigurieren Sie die Abfragefilter in _OnModelCreating_ mithilfe der ```HasQueryFilter``` API.</span><span class="sxs-lookup"><span data-stu-id="d9548-117">Next, configure the query filters in _OnModelCreating_ using the ```HasQueryFilter``` API.</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="d9548-118">Die Prädikatausdrücke übergeben, um die _HasQueryFilter_ Aufrufe jetzt automatisch gelten für alle LINQ-Abfragen für diese Typen.</span><span class="sxs-lookup"><span data-stu-id="d9548-118">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="d9548-119">Beachten Sie die Verwendung einer Ebene DbContext Instanzenfelds: ```_tenantId``` verwendet, um den aktuellen Mandanten festzulegen.</span><span class="sxs-lookup"><span data-stu-id="d9548-119">Note the use of a DbContext instance level field: ```_tenantId``` used to set the current tenant.</span></span> <span data-ttu-id="d9548-120">Ebene des Modells Filter werden den Wert aus der Instanz des richtigen Kontexts verwendet.</span><span class="sxs-lookup"><span data-stu-id="d9548-120">Model-level filters will use the value from the correct context instance.</span></span> <span data-ttu-id="d9548-121">D. h. Die Instanz, die die Abfrage ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="d9548-121">I.e. The instance that is executing the query.</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="d9548-122">Deaktivieren der Filter</span><span class="sxs-lookup"><span data-stu-id="d9548-122">Disabling Filters</span></span>

<span data-ttu-id="d9548-123">Filter können für einzelne LINQ-Abfragen deaktiviert werden, mithilfe der ```IgnoreQueryFilters()``` Operator.</span><span class="sxs-lookup"><span data-stu-id="d9548-123">Filters may be disabled for individual LINQ queries by using the ```IgnoreQueryFilters()``` operator.</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="d9548-124">Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="d9548-124">Limitations</span></span>

<span data-ttu-id="d9548-125">Globale Abfragefilter weisen die folgenden Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="d9548-125">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="d9548-126">Filter sind keine Verweise auf Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="d9548-126">Filters cannot contain references to navigation properties.</span></span>
* <span data-ttu-id="d9548-127">Filter können nur für den Stamm einer Vererbungshierarchie Entitätstyp definiert werden.</span><span class="sxs-lookup"><span data-stu-id="d9548-127">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
