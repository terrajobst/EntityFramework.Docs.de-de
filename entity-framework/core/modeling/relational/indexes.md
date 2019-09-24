---
title: Indizes-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 7dcf27dedbde45302a462a4c41a811b9868e40bb
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197015"
---
# <a name="indexes"></a>Indizes

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken. Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).

Ein Index in einer relationalen Datenbank wird dem gleichen Konzept zugeordnet wie ein Index im Kern der Entity Framework.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention werden Indizes benannt `IX_<type name>_<property name>`. Bei zusammengesetzten `<property name>` Indizes wird eine durch Trennzeichen getrennte Liste mit Eigenschaftsnamen.

## <a name="data-annotations"></a>Datenanmerkungen

Indizes können nicht mithilfe von Daten Anmerkungen konfiguriert werden.

## <a name="fluent-api"></a>Fluent-API

Sie können die fließende API verwenden, um den Namen eines Indexes zu konfigurieren.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexName.cs?name=Model&highlight=9)]

Sie können auch einen Filter angeben.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexFilter.cs?name=Model&highlight=9)]

Bei Verwendung des SQL Server-Anbieters fügt EF einen ' is not NULL '-Filter für alle Spalten hinzu, die NULL-Werte zulassen, die Teil eines eindeutigen Indexes sind. Um diese Konvention zu überschreiben, können `null` Sie einen Wert angeben.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a>Einschließen von Spalten in SQL Server Indizes

Sie können [Indizes mit inklusivspalten](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) konfigurieren, um die Abfrageleistung erheblich zu verbessern, wenn alle Spalten in der Abfrage als Schlüssel-oder nicht Schlüssel Spalten in den Index aufgenommen werden.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/ForSqlServerHasIndex.cs?name=Model)]
