---
title: 'Azure Cosmos DB Anbieter: Arbeiten mit unstrukturierten Daten EF Core'
description: Arbeiten mit Azure Cosmos DB unstrukturierten Daten mithilfe Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/unstructured-data
ms.openlocfilehash: 69f979d46174ff56310b334f28438ac271f45155
ms.sourcegitcommit: b3cf5d2e3cb170b9916795d1d8c88678269639b1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2020
ms.locfileid: "76888095"
---
# <a name="working-with-unstructured-data-in-ef-core-azure-cosmos-db-provider"></a>Arbeiten mit unstrukturierten Daten in EF Core Azure Cosmos DB-Anbieter

EF Core wurde entwickelt, um die Arbeit mit Daten zu erleichtern, die einem im Modell definierten Schema folgen. Eine der Stärken von Azure Cosmos DB ist jedoch die Flexibilität in der Form der gespeicherten Daten.

## <a name="accessing-the-raw-json"></a>Zugreifen auf die unformatierte JSON

Es ist möglich, auf die Eigenschaften zuzugreifen, die nicht durch EF Core über eine spezielle Eigenschaft in einem [Schatten Zustand](../../modeling/shadow-properties.md) mit dem Namen `"__jObject"` verfolgt werden, der eine `JObject` enthält, die die aus dem Speicher empfangenen Daten und die zu speichernden Daten darstellt:

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
> Die `"__jObject"`-Eigenschaft ist Teil der EF Core-Infrastruktur und sollte nur als letzter Ausweg verwendet werden, da Sie in zukünftigen Versionen wahrscheinlich ein anderes Verhalten hat.

> [!NOTE]
> Änderungen an der Entität überschreiben die Werte, die während `SaveChanges`in `"__jObject"` gespeichert werden.

## <a name="using-cosmosclient"></a>Verwenden von cosmosclient

Um vollständig von EF Core zu entkoppeln, erhalten Sie das [cosmosclient](/dotnet/api/Microsoft.Azure.Cosmos.CosmosClient) -Objekt, das [Teil des Azure Cosmos DB SDK](/azure/cosmos-db/sql-api-get-started) ist, von `DbContext`:

[!code-csharp[CosmosClient](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?highlight=3&name=CosmosClient)]

## <a name="missing-property-values"></a>Fehlende Eigenschaftswerte

Im vorherigen Beispiel haben wir die `"TrackingNumber"`-Eigenschaft aus der Reihenfolge entfernt. Aufgrund der Funktionsweise der Indizierung in Cosmos DB können Abfragen, die an anderer Stelle als in der Projektion auf die fehlende Eigenschaft verweisen, unerwartete Ergebnisse zurückgeben. Beispiel:

[!code-csharp[MissingProperties](../../../../samples/core/Cosmos/UnstructuredData/Sample.cs?name=MissingProperties)]

Die sortierte Abfrage gibt tatsächlich keine Ergebnisse zurück. Dies bedeutet, dass Sie die von EF Core zugeordneten Eigenschaften immer auffüllen müssen, wenn Sie direkt mit dem Speicher arbeiten.

> [!NOTE]
> Dieses Verhalten kann sich in zukünftigen Versionen von Cosmos ändern. Wenn die Indizierungs Richtlinie beispielsweise den zusammengesetzten Index {ID/? definiert ASC, TrackingNumber/? ASC)}, dann __würde__ eine Abfrage, die ' Order by c.ID ASC, c. Diskriminator ASC ' aufweist, Elemente zurückgeben, die die `"TrackingNumber"`-Eigenschaft fehlen.
