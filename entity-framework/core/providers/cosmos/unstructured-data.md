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
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a>Arbeiten mit unstrukturierten Daten in EF Core Azure Cosmos DB-Anbieter

EF Core wurde entwickelt, um die Arbeit mit Daten zu erleichtern, die einem im Modell definierten Schema folgen. Eine der Stärken von Azure Cosmos DB ist jedoch die Flexibilität in der Form der gespeicherten Daten.

## <a name="accessing-the-raw-json"></a>Zugreifen auf die unformatierte JSON

Es ist möglich, auf die Eigenschaften zuzugreifen, die nicht durch EF Core über eine spezielle Eigenschaft im [schattenzustand](../../modeling/shadow-properties.md) mit `"__jObject"` dem Namen verfolgt `JObject` werden, die ein-Objekt enthält, das die aus dem Speicher empfangenen Daten und die zu speichernden Daten darstellt:

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
> Die `"__jObject"` -Eigenschaft ist Teil der EF Core-Infrastruktur und sollte nur als letzter Ausweg verwendet werden, da Sie in zukünftigen Versionen wahrscheinlich ein anderes Verhalten hat.

> [!NOTE]
> Durch Änderungen an der-Entität werden die in `"__jObject"` `SaveChanges`gespeicherten Werte überschrieben.

## <a name="using-cosmosclient"></a>Verwenden von cosmosclient

Um vollständig von EF Core zu entkoppeln `CosmosClient` , erhalten Sie das Objekt, das [Teil des Azure Cosmos DB SDK](https://docs.microsoft.com/en-us/azure/cosmos-db/sql-api-get-started) ist, von `DbContext`:

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a>Fehlende Eigenschaftswerte

Im vorherigen Beispiel haben wir die `"TrackingNumber"` Eigenschaft aus der Reihenfolge entfernt. Aufgrund der Funktionsweise der Indizierung in Cosmos DB können Abfragen, die an anderer Stelle als in der Projektion auf die fehlende Eigenschaft verweisen, unerwartete Ergebnisse zurückgeben. Beispiel:

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

Die sortierte Abfrage gibt tatsächlich keine Ergebnisse zurück. Dies bedeutet, dass Sie die von EF Core zugeordneten Eigenschaften immer auffüllen müssen, wenn Sie direkt mit dem Speicher arbeiten.

> [!NOTE]
> Dieses Verhalten kann sich in zukünftigen Versionen von Cosmos ändern. Wenn die Indizierungs Richtlinie beispielsweise den zusammengesetzten Index {ID/? definiert ASC, TrackingNumber/? ASC)}. eine Abfrage, die "Order by c.ID ASC, c. Diskriminator ASC" hat, __würde__ Elemente zurückgeben, `"TrackingNumber"` für die die Eigenschaft fehlt.