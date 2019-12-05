---
title: Indizes (relationale Datenbank)-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/modeling/relational/indexes
ms.openlocfilehash: e14615275f85ee9b6b32d080905465d33963feca
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824574"
---
# <a name="indexes-relational-database"></a>Indizes (relationale Datenbank)

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken. Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).

Ein Index in einer relationalen Datenbank wird dem gleichen Konzept zugeordnet wie ein Index im Kern der Entity Framework.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention werden Indizes `IX_<type name>_<property name>`benannt. Bei zusammengesetzten Indizes `<property name>` zu einer durch Trennzeichen getrennten Liste mit Eigenschaftsnamen.

## <a name="data-annotations"></a>Datenanmerkungen

Indizes können nicht mithilfe von Daten Anmerkungen konfiguriert werden.

## <a name="fluent-api"></a>Fluent-API

Sie können die fließende API verwenden, um den Namen eines Indexes zu konfigurieren.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexName.cs?name=Model&highlight=9)]

Sie können auch einen Filter angeben.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexFilter.cs?name=Model&highlight=9)]

Bei Verwendung des SQL Server Anbieters fügt EF einen `'IS NOT NULL'` Filter für alle Spalten hinzu, die NULL-Werte zulassen, die Teil eines eindeutigen Indexes sind. Um diese Konvention zu überschreiben, können Sie einen `null` Wert angeben.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a>Einschließen von Spalten in SQL Server Indizes

Sie können [Indizes mit inklusivspalten](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) konfigurieren, um die Abfrageleistung erheblich zu verbessern, wenn alle Spalten in der Abfrage als Schlüssel-oder nicht Schlüssel Spalten in den Index aufgenommen werden.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexInclude.cs?name=Model)]
