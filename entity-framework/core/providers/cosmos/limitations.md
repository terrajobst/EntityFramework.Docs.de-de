---
title: Azure Cosmos DB Anbieter-Einschränkungen-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 9d02a2cd-484e-4687-b8a8-3748ba46dbc9
uid: core/providers/cosmos/limitations
ms.openlocfilehash: 8dcc82a68c89e21ad1902a0bbbce8ebbc3535801
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150764"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a><span data-ttu-id="da4a4-102">Einschränkungen für EF Core Azure Cosmos DB-Anbieter</span><span class="sxs-lookup"><span data-stu-id="da4a4-102">EF Core Azure Cosmos DB Provider Limitations</span></span>

<span data-ttu-id="da4a4-103">Der Cosmos-Anbieter hat eine Reihe von Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="da4a4-103">The Cosmos provider has a number of limitations.</span></span> <span data-ttu-id="da4a4-104">Viele dieser Einschränkungen sind das Ergebnis von Einschränkungen in der zugrunde liegenden Cosmos-Datenbank-Engine und nicht spezifisch für EF.</span><span class="sxs-lookup"><span data-stu-id="da4a4-104">Many of these limitations are a result of limitations in the underlying Cosmos database engine and are not specific to EF.</span></span> <span data-ttu-id="da4a4-105">Die meisten [sind jedoch noch nicht implementiert](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="da4a4-105">But most simply [haven't been implemented yet](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).</span></span>

## <a name="temporary-limitations"></a><span data-ttu-id="da4a4-106">Temporäre Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="da4a4-106">Temporary limitations</span></span>

- <span data-ttu-id="da4a4-107">Auch wenn nur ein Entitätstyp ohne Vererbung einem Container zugeordnet ist, verfügt er immer noch über eine diskriminatoreigenschaft.</span><span class="sxs-lookup"><span data-stu-id="da4a4-107">Even if there is only one entity type without inheritance mapped to a container it still has a discriminator property.</span></span>
- <span data-ttu-id="da4a4-108">Entitäts Typen mit Partitions Schlüsseln funktionieren in einigen Szenarios nicht ordnungsgemäß.</span><span class="sxs-lookup"><span data-stu-id="da4a4-108">Entity types with partition keys don't work correctly in some scenarios</span></span>
- <span data-ttu-id="da4a4-109">`Include`Aufrufe werden nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="da4a4-109">`Include` calls are not supported</span></span>
- <span data-ttu-id="da4a4-110">`Join`Aufrufe werden nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="da4a4-110">`Join` calls are not supported</span></span>

## <a name="azure-cosmos-db-sdk-limitations"></a><span data-ttu-id="da4a4-111">Azure Cosmos DB SDK-Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="da4a4-111">Azure Cosmos DB SDK limitations</span></span>

- <span data-ttu-id="da4a4-112">Es werden nur Async-Methoden bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="da4a4-112">Only async methods are provided</span></span>

> [!WARNING]
> <span data-ttu-id="da4a4-113">Da es keine Synchronisierungs Versionen der Low-Level-Methoden gibt EF Core die von abhängig sind, wird die entsprechende Funktionalität `.Wait()` derzeit durch Aufrufen `Task`von für das zurückgegebene implementiert.</span><span class="sxs-lookup"><span data-stu-id="da4a4-113">Since there are no sync versions of the low level methods EF Core relies on, the corresponding functionality is currently implemented by calling `.Wait()` on the returned `Task`.</span></span> <span data-ttu-id="da4a4-114">Dies bedeutet, dass die Verwendung `SaveChanges`von Methoden `ToList` wie oder anstelle der asynchronen Gegenstücke zu einem Deadlock in der Anwendung führen kann.</span><span class="sxs-lookup"><span data-stu-id="da4a4-114">This means that using methods like `SaveChanges`, or `ToList` instead of their async counterparts could lead to a deadlock in your application</span></span>

## <a name="azure-cosmos-db-limitations"></a><span data-ttu-id="da4a4-115">Einschränkungen für Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="da4a4-115">Azure Cosmos DB limitations</span></span>

<span data-ttu-id="da4a4-116">Im Vergleich zu einer relationalen Datenbank finden Sie die vollständige Übersicht über [Azure Cosmos DB unterstützten Features](https://docs.microsoft.com/en-us/azure/cosmos-db/modeling-data).</span><span class="sxs-lookup"><span data-stu-id="da4a4-116">You can see the full overview of [Azure Cosmos DB supported features](https://docs.microsoft.com/en-us/azure/cosmos-db/modeling-data), these are the most notable differences compared to a relational database:</span></span>

- <span data-ttu-id="da4a4-117">Vom Client initiierte Transaktionen werden nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="da4a4-117">Client-initiated transactions are not supported</span></span>
- <span data-ttu-id="da4a4-118">Einige Partitions übergreifende Abfragen werden abhängig von den beteiligten Operatoren entweder nicht unterstützt oder sind viel langsamer.</span><span class="sxs-lookup"><span data-stu-id="da4a4-118">Some cross-partition queries are either not supported or much slower depending on the operators involved</span></span>