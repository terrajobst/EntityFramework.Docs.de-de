---
title: Indizes – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 605b30ce710d9034deab97f695496ec66a576565
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993216"
---
# <a name="indexes"></a>Indizes

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken. Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).

Das gleiche Konzept als Index in der Kern von Entity Framework ist ein Index in einer relationalen Datenbank zugeordnet.

## <a name="conventions"></a>Konventionen

Indizes werden gemäß der Konvention benannt `IX_<type name>_<property name>`. Bei zusammengesetzten Indizes `<property name>` wird eine Unterstrich getrennt Liste von Eigenschaftsnamen.

## <a name="data-annotations"></a>Datenanmerkungen

Indizes können nicht mithilfe von Datenanmerkungen konfiguriert werden.

## <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, so konfigurieren Sie den Namen eines Indexes.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

Sie können auch einen Filter angeben.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

Wenn durch die Verwendung des SQL Server-Ressourcenanbieters EF Filtern eine 'IS NOT NULL"für alle auf NULL festlegbare Spalten, die Teil eines eindeutigen Indexes sind. Diese Konvention überschreiben, Sie angeben können, ein `null` Wert.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
