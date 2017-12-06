---
title: "Datentypen – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
ms.technology: entity-framework-core
uid: core/modeling/relational/data-types
ms.openlocfilehash: fd4668a3f9554eb9d3b1161d5dddce2fcdcac712
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2017
---
# <a name="data-types"></a>Datentypen

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt ist im Allgemeinen gilt für relationale Datenbanken. Die Erweiterungsmethoden, die hier gezeigten werden verfügbar, wenn Sie einen relationale Datenbank-Anbieter installieren (aufgrund der freigegebenen *Microsoft.EntityFrameworkCore.Relational* Paket).

Datentyp bezieht sich auf den bestimmten Datenbanktyp der Spalte, die eine Eigenschaft zugeordnet ist.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention wählt der Datenbankanbieter einen Datentyp, der basierend auf den CLR-Typ der Eigenschaft. Außerdem akzeptiert es berücksichtigt andere Metadaten, wie z. B. die konfigurierte [Maximallänge](../max-length.md), ob die Eigenschaft Teil einer primary key-, usw. ist.

SQL Server verwendet z. B. `datetime2(7)` für `DateTime` Eigenschaften und `nvarchar(max)` für `string` Eigenschaften (oder `nvarchar(450)` für `string` Eigenschaften, die als Schlüssel verwendet werden).

## <a name="data-annotations"></a>Datenanmerkungen

Sie können Datenanmerkungen verwenden, einen genauer Datentyp für eine Spalte angeben.

Der folgende Code z. B. konfiguriert `Url` als nicht-Unicode-Zeichenfolge mit maximal `200` und `Rating` als Dezimalzahl mit einer Genauigkeit von `5` und Skalieren von `2`.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a>Fluent-API

Die Fluent-API können auch die gleichen Datentypen für die Spalten angeben.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
