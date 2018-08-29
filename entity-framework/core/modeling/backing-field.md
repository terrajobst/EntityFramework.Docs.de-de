---
title: Unterstützungsfelder – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 79221b6f7968675ff10f80d5df181b674b6a20c9
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994092"
---
# <a name="backing-fields"></a>Dahinter liegenden Felder

> [!NOTE]  
> Dieses Feature ist neu in EF Core 1.1.

Dahinter liegenden Felder können EF lesen bzw. Schreiben auf ein Feld anstelle einer Eigenschaft an. Dies kann nützlich sein, wenn Kapselung in der Klasse beschränken die Verwendung von und/oder verbessern die Semantik für den Zugriff auf die Daten vom Anwendungscode verwendet wird, aber der Wert sollte lesen bzw. in die Datenbank geschrieben werden, ohne diese Einschränkungen werden / Erweiterungen.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention werden die folgenden Felder als unterstützungsfelder für eine bestimmte Eigenschaft (in ihrer Rangfolge aufgeführt) ermittelt. Felder werden nur für Eigenschaften ermittelt, die im Modell enthalten sind. Weitere Informationen, die für die Eigenschaften im Modell enthalten sind, finden Sie unter [einschließen und Ausschließen von Eigenschaften](included-properties.md).

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

Wenn ein dahinter liegendes Feld konfiguriert ist, wird EF direkt in das Feld schreiben, bei der Umsetzung von Instanzen der Entität aus der Datenbank (anstatt den Eigenschaftensetter). Wenn EF muss zum Lesen oder schreiben den Wert zu anderen Zeiten wird, wird nach Möglichkeit die Eigenschaft verwendet. Z. B. wenn EF den Wert für eine Eigenschaft zu aktualisieren muss, wird es Setter für die Eigenschaft verwenden, wenn eine definiert ist. Wenn die Eigenschaft schreibgeschützt ist, wird es in das Feld schreiben.

## <a name="data-annotations"></a>Datenanmerkungen

Unterstützungsfelder kann nicht mit datenanmerkungen konfiguriert werden.

## <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, zum Konfigurieren eines unterstützungsfelds für eine Eigenschaft.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a>Steuern, wenn das Feld verwendet wird

Sie können konfigurieren, wenn EF Felds oder der Eigenschaft verwendet. Finden Sie unter den [PropertyAccessMode Enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) für die unterstützten Optionen.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a>Felder ohne eine Eigenschaft

Sie können auch eine grundlegende Eigenschaft in Ihrem Modell erstellen, die verfügt nicht über einen entsprechenden CLR-Eigenschaft in der Entitätsklasse, sondern ein Feld zum Speichern der Daten in der Entität verwendet. Dies unterscheidet sich von [Schatteneigenschaften](shadow-properties.md), in denen die Daten in der änderungsprotokollierung durchlief gespeichert werden. Dies wird normalerweise verwendet, wenn die Entity-Klasse Methoden verwendet, um Get/Set-Werte.

Sie erhalten EF den Namen des Felds in die `Property(...)` API. Wenn keine Eigenschaft mit dem angegebenen Namen vorhanden ist, sieht dann EF für ein Feld.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

Sie können auch auswählen, um der Eigenschaft einen anderen Namen, als der Name des Felds zu gewähren. Dieser Name wird verwendet, wenn das Modell zu erstellen, sondern wird insbesondere verwendet werden, für den Namen der Spalte, der die in der Datenbank zugeordnet ist.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

Wenn keine Eigenschaft in der Entitätsklasse vorhanden ist, können Sie mithilfe der `EF.Property(...)` -Methode in einer LINQ-Abfrage zum Verweisen auf die Eigenschaft, die konzeptionell Teil des Modells ist.

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
