---
title: Schlüssel (primär) - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 51d163b867085f42f415dbd7afa9e311ab1781a0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929835"
---
# <a name="keys-primary"></a>Schlüssel (primär)

Ein Schlüssel dient als primärer Eindeutiger Bezeichner für jede Entitätsinstanz. Bei Verwendung eine relationale Datenbank zugeordnet, das Konzept von einem *Primärschlüssel*. Sie können auch einen eindeutigen Bezeichner, die nicht den Primärschlüssel konfigurieren (finden Sie unter [Alternativschlüssel](alternate-keys.md) Informationen). 

Eine der folgenden Methoden kann verwendet werden, um einen Primärschlüssel Einrichtung/erstellen.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention, eine Eigenschaft mit dem Namen `Id` oder `<type name>Id` wird als Schlüssel für eine Entität konfiguriert werden.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyTypeNameId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string CarId { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="data-annotations"></a>Datenanmerkungen

Sie können Datenanmerkungen verwenden, so konfigurieren Sie eine einzelne Eigenschaft als Schlüssel einer Entität.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, so konfigurieren Sie eine einzelne Eigenschaft als Schlüssel einer Entität.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/KeySingle.cs?highlight=11,12)]

Sie können auch die Fluent-API verwenden, so konfigurieren Sie mehrere Eigenschaften als Schlüssel einer Entität (als einen zusammengesetzten Schlüssel bezeichnet). Zusammengesetzte Schlüssel können nur mithilfe der Fluent-API konfiguriert werden – Konventionen werden einen zusammengesetzten Schlüssel niemals eingerichtet und Datenanmerkungen Konfiguration kann nicht verwendet.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/KeyComposite.cs?highlight=11,12)]
