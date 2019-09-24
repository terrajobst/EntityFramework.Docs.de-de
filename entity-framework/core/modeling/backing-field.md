---
title: Unterstützungs Felder-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: c3ca8bb97992c192672e8c2f2040b0de029df68d
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197480"
---
# <a name="backing-fields"></a>Unterstützungsfelder

> [!NOTE]  
> Diese Funktion ist neu in EF Core 1,1.

Mit Unterstützungs Feldern kann EF anstelle einer Eigenschaft ein Feld lesen und/oder in ein Feld schreiben. Dies kann hilfreich sein, wenn die Kapselung in der-Klasse verwendet wird, um die Verwendung von zu beschränken und/oder die Semantik für den Zugriff auf die Daten durch den Anwendungscode zu verbessern. der Wert sollte jedoch aus der Datenbank gelesen und/oder in diese geschrieben werden, ohne dass diese Einschränkungen verwendet werden. Steigerung.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention werden die folgenden Felder als Sicherungs Felder für eine bestimmte Eigenschaft (aufgelistet in der Rangfolge) erkannt. Felder werden nur für Eigenschaften erkannt, die im Modell enthalten sind. Weitere Informationen zu den Eigenschaften, die im Modell enthalten sind, finden Sie unter [einschließen & Ausschließen von Eigenschaften](included-properties.md).

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

Wenn ein dahinter liegendes Feld konfiguriert ist, schreibt EF direkt in dieses Feld, wenn Entitäts Instanzen aus der Datenbank materialisiert werden (anstatt den Eigenschaften Setter zu verwenden). Wenn EF den Wert zu einem anderen Zeitpunkt lesen oder schreiben muss, wird die-Eigenschaft, sofern möglich, verwendet. Wenn EF z. b. den Wert für eine Eigenschaft aktualisieren muss, wird der Eigenschaften Setter verwendet, wenn ein solcher definiert ist. Wenn die Eigenschaft schreibgeschützt ist, wird in das Feld geschrieben.

## <a name="data-annotations"></a>Datenanmerkungen

Unterstützungs Felder können nicht mit Daten Anmerkungen konfiguriert werden.

## <a name="fluent-api"></a>Fluent-API

Sie können die fließende API verwenden, um ein Unterstützungs Feld für eine Eigenschaft zu konfigurieren.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a>Steuern, wann das Feld verwendet wird

Sie können konfigurieren, wann EF das Feld oder die Eigenschaft verwendet. Die unterstützten Optionen finden Sie in der [propertyaccessmode-Enumeration](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) .

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a>Felder ohne Eigenschaft

Sie können auch eine konzeptionelle Eigenschaft in Ihrem Modell erstellen, die nicht über eine entsprechende CLR-Eigenschaft in der Entitäts Klasse verfügt, sondern stattdessen ein Feld zum Speichern der Daten in der Entität verwendet. Dies unterscheidet sich von den [Schatten Eigenschaften](shadow-properties.md), bei denen die Daten in der Änderungs Nachverfolgung gespeichert werden. Dies wird normalerweise verwendet, wenn die Entitäts Klasse Methoden verwendet, um Werte zu erhalten bzw. festzulegen.

Sie können EF den Namen des Felds in der `Property(...)` API übergeben. Wenn keine Eigenschaft mit dem angegebenen Namen vorhanden ist, sucht EF nach einem Feld.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

Sie können auch festlegen, dass der Eigenschaft ein anderer Name als der Feldname zugewiesen wird. Dieser Name wird dann verwendet, wenn das Modell erstellt wird. insbesondere wird er für den Spaltennamen verwendet, der in der Datenbank zugeordnet ist.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldConceptualProperty.cs#Sample)]

Wenn in der Entitäts Klasse keine Eigenschaft vorhanden ist, können Sie die `EF.Property(...)` -Methode in einer LINQ-Abfrage verwenden, um auf die Eigenschaft zu verweisen, die konzeptionell Teil des Modells ist.

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
