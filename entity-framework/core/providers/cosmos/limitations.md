---
title: Azure Cosmos DB Anbieter-Einschränkungen-EF Core
description: Die Einschränkungen des Entity Framework Core Azure Cosmos DB Anbieters
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/limitations
ms.openlocfilehash: 2631526b152d6ddcacf25173c8d51e4e3cb24500
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414560"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a>Einschränkungen für EF Core Azure Cosmos DB-Anbieter

Der Cosmos-Anbieter hat eine Reihe von Einschränkungen. Viele dieser Einschränkungen sind das Ergebnis von Einschränkungen in der zugrunde liegenden Cosmos-Datenbank-Engine und nicht spezifisch für EF. Die meisten [sind jedoch noch nicht implementiert](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).

## <a name="temporary-limitations"></a>Temporäre Einschränkungen

- Auch wenn nur ein Entitätstyp ohne Vererbung einem Container zugeordnet ist, verfügt er immer noch über eine diskriminatoreigenschaft.
- Entitäts Typen mit Partitions Schlüsseln funktionieren in einigen Szenarios nicht ordnungsgemäß.
- `Include`-Aufrufe werden nicht unterstützt.
- `Join`-Aufrufe werden nicht unterstützt.

## <a name="azure-cosmos-db-sdk-limitations"></a>Azure Cosmos DB SDK-Einschränkungen

- Es werden nur Async-Methoden bereitgestellt.

> [!WARNING]
> Da es keine Synchronisierungs Versionen der Low-Level-Methoden gibt EF Core die von abhängig sind, wird die entsprechende Funktionalität aktuell implementiert, indem `.Wait()` auf dem zurückgegebenen `Task`aufgerufen wird. Dies bedeutet, dass die Verwendung von Methoden wie `SaveChanges`oder `ToList` anstelle der asynchronen Gegenstücke zu einem Deadlock in der Anwendung führen kann.

## <a name="azure-cosmos-db-limitations"></a>Einschränkungen für Azure Cosmos DB

Im Vergleich zu einer relationalen Datenbank finden Sie die vollständige Übersicht über [Azure Cosmos DB unterstützten Features](/azure/cosmos-db/modeling-data).

- Vom Client initiierte Transaktionen werden nicht unterstützt.
- Einige Partitions übergreifende Abfragen werden abhängig von den beteiligten Operatoren entweder nicht unterstützt oder sind viel langsamer.
