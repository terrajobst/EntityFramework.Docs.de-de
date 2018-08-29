---
title: Datentypen – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: 9060f66c752be01090ce40be6bf3a32f348ce571
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993520"
---
# <a name="data-types"></a>Datentypen

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken. Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).

Datentyp bezieht sich auf die Datenbank bestimmte Typ der Spalte, die eine Eigenschaft zugeordnet ist.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention wählt der Datenbankanbieter einen Datentyp, der basierend auf dem CLR-Typ der Eigenschaft. Es berücksichtigt auch andere Metadaten, wie z. B. die konfigurierte [Maximallänge](../max-length.md), ob die Eigenschaft Teil eines Primärschlüssels usw. ist.

SQL Server verwendet z. B. `datetime2(7)` für `DateTime` Eigenschaften und `nvarchar(max)` für `string` Eigenschaften (oder `nvarchar(450)` für `string` Eigenschaften, die als Schlüssel verwendet werden).

## <a name="data-annotations"></a>Datenanmerkungen

Sie können Datenanmerkungen verwenden, um ein genauer Datentyp für eine Spalte anzugeben.

Der folgende Code z. B. konfiguriert `Url` als nicht-Unicode-Zeichenfolge mit maximal `200` und `Rating` als Dezimalzahl mit einer Genauigkeit von `5` und Skalieren von `2`.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a>Fluent-API

Sie können auch die Fluent-API verwenden, um die gleichen Datentypen für die Spalten anzugeben.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
