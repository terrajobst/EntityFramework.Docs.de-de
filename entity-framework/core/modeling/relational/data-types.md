---
title: Datentypen-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: 26664ebe18abcdeaa2b9c8dc23a6410204f53c8e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197178"
---
# <a name="data-types"></a>Datentypen

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken. Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).

Datentyp bezieht sich auf den datenbankspezifischen Typ der Spalte, der eine Eigenschaft zugeordnet ist.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention wählt der Datenbankanbieter einen Datentyp aus, der auf dem .NET-Typ der Eigenschaft basiert. Es berücksichtigt auch andere Metadaten, z. b. die konfigurierte [Maximale Länge](../max-length.md), ob die Eigenschaft Teil eines Primärschlüssels ist usw.

`datetime2(7)` SQL Server z. b. für `DateTime` Eigenschaften und `nvarchar(max)` für `string` Eigenschaften (oder `nvarchar(450)` für `string` Eigenschaften, die als Schlüssel verwendet werden).

## <a name="data-annotations"></a>Datenanmerkungen

Mithilfe von Daten Anmerkungen können Sie einen exakten Datentyp für eine Spalte angeben.

`Url` Der folgende Code konfiguriert z. b. als nicht-Unicode-Zeichenfolge mit einer maximalen Länge von `200` und `Rating` als Dezimal `5` Länge mit einer `2`Genauigkeit von und einer Skala von.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a>Fluent-API

Sie können auch die fließende API verwenden, um dieselben Datentypen für die Spalten anzugeben.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DataType.cs?name=Model&highlight=9-10)]
