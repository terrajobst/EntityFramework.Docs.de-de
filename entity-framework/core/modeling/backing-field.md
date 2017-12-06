---
title: Sichern Felder - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
ms.technology: entity-framework-core
uid: core/modeling/backing-field
ms.openlocfilehash: e95417b3368d09a64f9ec02ae40c38dc770c2e6b
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2017
---
# <a name="backing-fields"></a>Dahinter liegenden Felder

> [!NOTE]  
> Dieses Feature ist neu in EF Core 1.1.

Dahinter liegenden Felder können EF zum Lesen und/oder Schreiben in ein Feld, sondern eine Eigenschaft an. Dies kann nützlich sein, wenn für die Kapselung in der Klasse zum Einschränken der Verwendung von und/oder verbessern die Semantik für den Zugriff auf die Daten vom Anwendungscode verwendet wird, aber der Wert sollte Lese- oder in die Datenbank geschrieben werden, ohne diese Einschränkungen werden / Verbesserungen.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention werden die folgenden Felder als Felder für eine bestimmte Eigenschaft (in ihrer Rangfolge aufgeführt) sichern ermittelt. Felder werden nur für Eigenschaften ermittelt, die im Modell enthalten sind. Weitere Informationen, die für die Eigenschaften im Modell enthalten sind, finden Sie unter [einschließlich & Eigenschaften außer](included-properties.md).

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

Wenn ein dahinter liegendes Feld konfiguriert ist, schreibt EF direkt auf dieses Feld beim Materialisieren von Instanzen der Entität aus der Datenbank (anstatt den Eigenschaftensetter). Wenn EF muss zum Lesen oder schreiben den Wert zu anderen Zeiten auf, wird die Eigenschaft nach Möglichkeit verwendet. Z. B. EF muss den Wert für eine Eigenschaft aktualisieren, wird es der Setter für eine Eigenschaft verwendet, sofern eine definiert ist. Wenn die Eigenschaft schreibgeschützt ist, wird es in das Feld geschrieben.

## <a name="data-annotations"></a>Datenanmerkungen

Sichern die Felder kann nicht mit datenanmerkungen konfiguriert werden.

## <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, so konfigurieren Sie eine dahinter liegende Feld für eine Eigenschaft.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a>Steuern, wenn das Feld verwendet wird

Sie können konfigurieren, wenn EF Felds oder der Eigenschaft verwendet. Finden Sie unter der [PropertyAccessMode Enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) Informationen zu den unterstützten Optionen.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a>Felder ohne eine Eigenschaft

Sie können auch eine konzeptuelle Eigenschaft im Modell erstellen, die verfügt nicht über einen entsprechenden CLR-Eigenschaft in die Entitätsklasse, sondern ein Feld zum Speichern der Daten in der Entität verwendet. Dies unterscheidet sich von [Shadowing von Eigenschaften](shadow-properties.md), wo die Daten in das System zur änderungsnachverfolgung gespeichert werden. Dies wird normalerweise verwendet, wenn die Entitätsklasse Methoden verwendet, um Get/Set-Werte.

Der Name des Felds in EF geben die `Property(...)` API. Wenn keine Eigenschaft mit dem angegebenen Namen vorhanden ist, sieht dann EF für ein Feld.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

Sie könne auch der Eigenschaft geben Sie einen anderen Namen, als der Name des Felds. Dieser Name wird beim Erstellen des Modells verwendet, sondern wird vor allem Kontrollvorgänge verwendet werden, für den Namen der Spalte, der in der Datenbank zugeordnet ist.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

Wenn keine Eigenschaft in der Entitätsklasse vorhanden ist, können Sie die `EF.Property(...)` Methode in einer LINQ-Abfrage zum Verweisen auf die Eigenschaft, die Teil des Modells prinzipiell sind.

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
