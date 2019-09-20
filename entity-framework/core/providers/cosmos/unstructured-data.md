---
title: 'Azure Cosmos DB Anbieter: Arbeiten mit unstrukturierten Daten EF Core'
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: b47d41b6-984f-419a-ab10-2ed3b95e3919
uid: core/providers/cosmos/unstructured-data
ms.openlocfilehash: 86bb0f7915c8a2561e7d5cd5dffc27474218a112
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150770"
---
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a><span data-ttu-id="b588b-102">Arbeiten mit unstrukturierten Daten in EF Core Azure Cosmos DB-Anbieter</span><span class="sxs-lookup"><span data-stu-id="b588b-102">Working with Unstructured Data in EF Core Azure Cosmos DB Provider</span></span>

<span data-ttu-id="b588b-103">EF Core wurde entwickelt, um die Arbeit mit Daten zu erleichtern, die einem im Modell definierten Schema folgen.</span><span class="sxs-lookup"><span data-stu-id="b588b-103">EF Core was designed to make it easy to work with data that follows a schema defined in the model.</span></span> <span data-ttu-id="b588b-104">Eine der Stärken von Azure Cosmos DB ist jedoch die Flexibilität in der Form der gespeicherten Daten.</span><span class="sxs-lookup"><span data-stu-id="b588b-104">However one of the strengths of Azure Cosmos DB is the flexibility in the shape of the data stored.</span></span>

## <a name="accessing-the-raw-json"></a><span data-ttu-id="b588b-105">Zugreifen auf die unformatierte JSON</span><span class="sxs-lookup"><span data-stu-id="b588b-105">Accessing the raw JSON</span></span>

<span data-ttu-id="b588b-106">Es ist möglich, auf die Eigenschaften zuzugreifen, die nicht durch EF Core über eine spezielle Eigenschaft im [schattenzustand](../../modeling/shadow-properties.md) mit `"__jObject"` dem Namen verfolgt `JObject` werden, die ein-Objekt enthält, das die aus dem Speicher empfangenen Daten und die zu speichernden Daten darstellt:</span><span class="sxs-lookup"><span data-stu-id="b588b-106">It is possible to access the properties that are not tracked by EF Core through a special property in [shadow-state](../../modeling/shadow-properties.md) named `"__jObject"` that contains a `JObject` representing the data recieved from the store and data that will be stored:</span></span>

[!code-csharp[Unmapped](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=21-23&name=Unmapped)]

``` json
{
    "Id": 1,
    "Discriminator": "Order",
    "TrackingNumber": null,
    "id": "Order|1",
    "Address": {
        "ShipsToCity": "London",
        "Discriminator": "StreetAddress",
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
> <span data-ttu-id="b588b-107">Die `"__jObject"` -Eigenschaft ist Teil der EF Core-Infrastruktur und sollte nur als letzter Ausweg verwendet werden, da Sie in zukünftigen Versionen wahrscheinlich ein anderes Verhalten hat.</span><span class="sxs-lookup"><span data-stu-id="b588b-107">The `"__jObject"` property is part of the EF Core infrastructure and should only be used as a last resort as it is likely to have different behavior in future releases.</span></span>

> [!NOTE]
> <span data-ttu-id="b588b-108">Durch Änderungen an der-Entität werden die in `"__jObject"` `SaveChanges`gespeicherten Werte überschrieben.</span><span class="sxs-lookup"><span data-stu-id="b588b-108">Changes to the entity will override the values stored in `"__jObject"` during `SaveChanges`.</span></span>

## <a name="using-cosmosclient"></a><span data-ttu-id="b588b-109">Verwenden von cosmosclient</span><span class="sxs-lookup"><span data-stu-id="b588b-109">Using CosmosClient</span></span>

<span data-ttu-id="b588b-110">Um vollständig von EF Core zu entkoppeln `CosmosClient` , erhalten Sie das Objekt, das [Teil des Azure Cosmos DB SDK](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started) ist, von `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="b588b-110">To decouple completely from EF Core get the `CosmosClient` object that is [part of the Azure Cosmos DB SDK](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started) from `DbContext`:</span></span>

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a><span data-ttu-id="b588b-111">Fehlende Eigenschaftswerte</span><span class="sxs-lookup"><span data-stu-id="b588b-111">Missing property values</span></span>

<span data-ttu-id="b588b-112">Im vorherigen Beispiel haben wir die `"TrackingNumber"` Eigenschaft aus der Reihenfolge entfernt.</span><span class="sxs-lookup"><span data-stu-id="b588b-112">In the previous example we removed the `"TrackingNumber"` property from the order.</span></span> <span data-ttu-id="b588b-113">Aufgrund der Funktionsweise der Indizierung in Cosmos DB können Abfragen, die an anderer Stelle als in der Projektion auf die fehlende Eigenschaft verweisen, unerwartete Ergebnisse zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="b588b-113">Because of how indexing works in Cosmos DB, queries that reference the missing property somewhere else than in the projection could return unexpected results.</span></span> <span data-ttu-id="b588b-114">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b588b-114">For example:</span></span>

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

<span data-ttu-id="b588b-115">Die sortierte Abfrage gibt tatsächlich keine Ergebnisse zurück.</span><span class="sxs-lookup"><span data-stu-id="b588b-115">The sorted query actually returns no results.</span></span> <span data-ttu-id="b588b-116">Dies bedeutet, dass Sie die von EF Core zugeordneten Eigenschaften immer auffüllen müssen, wenn Sie direkt mit dem Speicher arbeiten.</span><span class="sxs-lookup"><span data-stu-id="b588b-116">This means that one should take care to always populate properties mapped by EF Core when working with the store directly.</span></span>

> [!NOTE]
> <span data-ttu-id="b588b-117">Dieses Verhalten kann sich in zukünftigen Versionen von Cosmos ändern.</span><span class="sxs-lookup"><span data-stu-id="b588b-117">This behavior might change in future versions of Cosmos.</span></span> <span data-ttu-id="b588b-118">Wenn die Indizierungs Richtlinie beispielsweise den zusammengesetzten Index {ID/? definiert</span><span class="sxs-lookup"><span data-stu-id="b588b-118">For instance, currently if the indexing policy defines the composite index {Id/?</span></span> <span data-ttu-id="b588b-119">ASC, TrackingNumber/?</span><span class="sxs-lookup"><span data-stu-id="b588b-119">ASC, TrackingNumber/?</span></span> <span data-ttu-id="b588b-120">ASC)}. eine Abfrage, die "Order by c.ID ASC, c. Diskriminator ASC" hat, __würde__ Elemente zurückgeben, `"TrackingNumber"` für die die Eigenschaft fehlt.</span><span class="sxs-lookup"><span data-stu-id="b588b-120">ASC)}, then a query the has 'ORDER BY c.Id ASC, c.Discriminator ASC' __would__ return items that are missing the `"TrackingNumber"` property.</span></span>