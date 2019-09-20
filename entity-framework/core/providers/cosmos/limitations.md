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
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a>Einschränkungen für EF Core Azure Cosmos DB-Anbieter

Der Cosmos-Anbieter hat eine Reihe von Einschränkungen. Viele dieser Einschränkungen sind das Ergebnis von Einschränkungen in der zugrunde liegenden Cosmos-Datenbank-Engine und nicht spezifisch für EF. Die meisten [sind jedoch noch nicht implementiert](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).

## <a name="temporary-limitations"></a>Temporäre Einschränkungen

- Auch wenn nur ein Entitätstyp ohne Vererbung einem Container zugeordnet ist, verfügt er immer noch über eine diskriminatoreigenschaft.
- Entitäts Typen mit Partitions Schlüsseln funktionieren in einigen Szenarios nicht ordnungsgemäß.
- `Include`Aufrufe werden nicht unterstützt.
- `Join`Aufrufe werden nicht unterstützt.

## <a name="azure-cosmos-db-sdk-limitations"></a>Azure Cosmos DB SDK-Einschränkungen

- Es werden nur Async-Methoden bereitgestellt.

> [!WARNING]
> Da es keine Synchronisierungs Versionen der Low-Level-Methoden gibt EF Core die von abhängig sind, wird die entsprechende Funktionalität `.Wait()` derzeit durch Aufrufen `Task`von für das zurückgegebene implementiert. Dies bedeutet, dass die Verwendung `SaveChanges`von Methoden `ToList` wie oder anstelle der asynchronen Gegenstücke zu einem Deadlock in der Anwendung führen kann.

## <a name="azure-cosmos-db-limitations"></a>Einschränkungen für Azure Cosmos DB

Im Vergleich zu einer relationalen Datenbank finden Sie die vollständige Übersicht über [Azure Cosmos DB unterstützten Features](https://docs.microsoft.com/en-us/azure/cosmos-db/modeling-data).

- Vom Client initiierte Transaktionen werden nicht unterstützt.
- Einige Partitions übergreifende Abfragen werden abhängig von den beteiligten Operatoren entweder nicht unterstützt oder sind viel langsamer.