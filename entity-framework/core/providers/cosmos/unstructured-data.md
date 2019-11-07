---
title: 'Azure Cosmos DB Anbieter: Arbeiten mit unstrukturierten Daten EF Core'
description: Arbeiten mit Azure Cosmos DB unstrukturierten Daten mithilfe Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/unstructured-data
ms.openlocfilehash: 0bfccbfd3af6e209967004752b5a3947d644544b
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655514"
---
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="183a8-103">Arbeiten mit unstrukturierten Daten in EF Core Azure Cosmos DB-Anbieter</span><span class="sxs-lookup"><span data-stu-id="183a8-103">Working with Unstructured Data in EF Core Azure Cosmos DB Provider</span></span>

<span data-ttu-id="183a8-104">EF Core wurde entwickelt, um die Arbeit mit Daten zu erleichtern, die einem im Modell definierten Schema folgen.</span><span class="sxs-lookup"><span data-stu-id="183a8-104">EF Core was designed to make it easy to work with data that follows a schema defined in the model.</span></span> <span data-ttu-id="183a8-105">Eine der Stärken von Azure Cosmos DB ist jedoch die Flexibilität in der Form der gespeicherten Daten.</span><span class="sxs-lookup"><span data-stu-id="183a8-105">However one of the strengths of Azure Cosmos DB is the flexibility in the shape of the data stored.</span></span>

## <a name="accessing-the-raw-json"></a><span data-ttu-id="183a8-106">Zugreifen auf die unformatierte JSON</span><span class="sxs-lookup"><span data-stu-id="183a8-106">Accessing the raw JSON</span></span>

<span data-ttu-id="183a8-107">Es ist möglich, auf die Eigenschaften zuzugreifen, die nicht durch EF Core über eine spezielle Eigenschaft in einem [Schatten Zustand](../../modeling/shadow-properties.md) mit dem Namen `"__jObject"` verfolgt werden, der eine `JObject` enthält, die die aus dem Speicher empfangenen Daten und die zu speichernden Daten darstellt:</span><span class="sxs-lookup"><span data-stu-id="183a8-107">It is possible to access the properties that are not tracked by EF Core through a special property in [shadow-state](../../modeling/shadow-properties.md) named `"__jObject"` that contains a `JObject` representing the data recieved from the store and data that will be stored:</span></span>

[!code-csharp[Unmapped](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=23,24&name=Unmapped)]

``` json
{
    "Id": 1,
    "PartitionKey": "1",
    "TrackingNumber": null,
    "id": "1",
    "Address": {
        "ShipsToCity": "London",
        "ShipsToStreet": "221 B Baker St"
    },
    "_rid": "eLMaAK8TzkIBAAAAAAAAAA==",
    "_self": "dbs/eLMaAA==/colls/eLMaAK8TzkI=/docs/eLMaAK8TzkIBAAAAAAAAAA==/",
    "_etag": "\"00000000-0000-0000-683e-0a12bf8d01d5\"",
    "_attachments": "attachments/",
    "BillingAddress": "Clarence House",
    "_ts": 1568164374
}
```

> [!WARNING]
> <span data-ttu-id="183a8-108">Die `"__jObject"`-Eigenschaft ist Teil der EF Core-Infrastruktur und sollte nur als letzter Ausweg verwendet werden, da Sie in zukünftigen Versionen wahrscheinlich ein anderes Verhalten hat.</span><span class="sxs-lookup"><span data-stu-id="183a8-108">The `"__jObject"` property is part of the EF Core infrastructure and should only be used as a last resort as it is likely to have different behavior in future releases.</span></span>

> [!NOTE]
> <span data-ttu-id="183a8-109">Änderungen an der Entität überschreiben die Werte, die während `SaveChanges`in `"__jObject"` gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="183a8-109">Changes to the entity will override the values stored in `"__jObject"` during `SaveChanges`.</span></span>

## <a name="using-cosmosclient"></a><span data-ttu-id="183a8-110">Verwenden von cosmosclient</span><span class="sxs-lookup"><span data-stu-id="183a8-110">Using CosmosClient</span></span>

<span data-ttu-id="183a8-111">Um vollständig von EF Core zu entkoppeln, erhalten Sie das [cosmosclient](/dotnet/api/Microsoft.Azure.Cosmos.CosmosClient) -Objekt, das [Teil des Azure Cosmos DB SDK](/azure/cosmos-db/sql-api-get-started) ist, von `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="183a8-111">To decouple completely from EF Core get the [CosmosClient](/dotnet/api/Microsoft.Azure.Cosmos.CosmosClient) object that is [part of the Azure Cosmos DB SDK](/azure/cosmos-db/sql-api-get-started) from `DbContext`:</span></span>

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a><span data-ttu-id="183a8-112">Fehlende Eigenschaftswerte</span><span class="sxs-lookup"><span data-stu-id="183a8-112">Missing property values</span></span>

<span data-ttu-id="183a8-113">Im vorherigen Beispiel haben wir die `"TrackingNumber"`-Eigenschaft aus der Reihenfolge entfernt.</span><span class="sxs-lookup"><span data-stu-id="183a8-113">In the previous example we removed the `"TrackingNumber"` property from the order.</span></span> <span data-ttu-id="183a8-114">Aufgrund der Funktionsweise der Indizierung in Cosmos DB können Abfragen, die an anderer Stelle als in der Projektion auf die fehlende Eigenschaft verweisen, unerwartete Ergebnisse zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="183a8-114">Because of how indexing works in Cosmos DB, queries that reference the missing property somewhere else than in the projection could return unexpected results.</span></span> <span data-ttu-id="183a8-115">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="183a8-115">For example:</span></span>

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

<span data-ttu-id="183a8-116">Die sortierte Abfrage gibt tatsächlich keine Ergebnisse zurück.</span><span class="sxs-lookup"><span data-stu-id="183a8-116">The sorted query actually returns no results.</span></span> <span data-ttu-id="183a8-117">Dies bedeutet, dass Sie die von EF Core zugeordneten Eigenschaften immer auffüllen müssen, wenn Sie direkt mit dem Speicher arbeiten.</span><span class="sxs-lookup"><span data-stu-id="183a8-117">This means that one should take care to always populate properties mapped by EF Core when working with the store directly.</span></span>

> [!NOTE]
> <span data-ttu-id="183a8-118">Dieses Verhalten kann sich in zukünftigen Versionen von Cosmos ändern.</span><span class="sxs-lookup"><span data-stu-id="183a8-118">This behavior might change in future versions of Cosmos.</span></span> <span data-ttu-id="183a8-119">Wenn die Indizierungs Richtlinie beispielsweise den zusammengesetzten Index {ID/? definiert</span><span class="sxs-lookup"><span data-stu-id="183a8-119">For instance, currently if the indexing policy defines the composite index {Id/?</span></span> <span data-ttu-id="183a8-120">ASC, TrackingNumber/?</span><span class="sxs-lookup"><span data-stu-id="183a8-120">ASC, TrackingNumber/?</span></span> <span data-ttu-id="183a8-121">ASC)}. eine Abfrage, die "Order by c.ID ASC, c. Diskriminator ASC" hat, __würde__ Elemente zurückgeben, die die `"TrackingNumber"`-Eigenschaft fehlen.</span><span class="sxs-lookup"><span data-stu-id="183a8-121">ASC)}, then a query the has 'ORDER BY c.Id ASC, c.Discriminator ASC' __would__ return items that are missing the `"TrackingNumber"` property.</span></span>
