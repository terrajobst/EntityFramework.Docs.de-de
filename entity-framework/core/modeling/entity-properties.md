---
title: Entitäts Eigenschaften-EF Core
description: Konfigurieren und Zuordnen von Entitäts Eigenschaften mithilfe von Entity Framework Core
author: roji
ms.date: 12/10/2019
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/entity-properties
ms.openlocfilehash: b67603fbffd1f1c8506bc21f8972c851eb8eef29
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414566"
---
# <a name="entity-properties"></a>Entitätseigenschaften

Jeder Entitätstyp in Ihrem Modell verfügt über eine Reihe von Eigenschaften, die EF Core aus der Datenbank lesen und schreiben. Wenn Sie eine relationale Datenbank verwenden, werden die Entitäts Eigenschaften Tabellen Spalten zugeordnet.

## <a name="included-and-excluded-properties"></a>Enthaltene und ausgeschlossene Eigenschaften

Gemäß der Konvention werden alle öffentlichen Eigenschaften mit einem Getter und einem Setter in das Modell eingeschlossen.

Bestimmte Eigenschaften können wie folgt ausgeschlossen werden:

### <a name="data-annotations"></a>[Datenanmerkungen](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?name=IgnoreProperty&highlight=6)]

### <a name="fluent-api"></a>[Fließende API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?name=IgnoreProperty&highlight=3,4)]

***

## <a name="column-names"></a>Spaltennamen

Gemäß der Konvention werden bei Verwendung einer relationalen Datenbank Entitäts Eigenschaften Tabellen Spalten mit dem gleichen Namen wie die Eigenschaft zugeordnet.

Wenn Sie Ihre Spalten lieber mit unterschiedlichen Namen konfigurieren möchten, können Sie dies wie folgt tun:

### <a name="data-annotations"></a>[Datenanmerkungen](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnName.cs?Name=ColumnName&highlight=3)]

### <a name="fluent-api"></a>[Fließende API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnName.cs?Name=ColumnName&highlight=3-5)]

***

## <a name="column-data-types"></a>Spaltendatentypen

Wenn Sie eine relationale Datenbank verwenden, wählt der Datenbankanbieter einen Datentyp aus, der auf dem .NET-Typ der Eigenschaft basiert. Es berücksichtigt auch andere Metadaten, z. b. die konfigurierte [Maximale Länge](#maximum-length), ob die Eigenschaft Teil eines Primärschlüssels ist usw.

SQL Server ordnet z. b. `DateTime` Eigenschaften `datetime2(7)` Spalten zu und `string` Eigenschaften `nvarchar(max)` Spalten (oder `nvarchar(450)` für Eigenschaften, die als Schlüssel verwendet werden).

Sie können auch die Spalten so konfigurieren, dass Sie einen exakten Datentyp für eine Spalte angeben. Der folgende Code konfiguriert z. b. `Url` als nicht-Unicode-Zeichenfolge mit maximaler Länge von `200` und `Rating` als Dezimal Länge mit der Genauigkeit `5` und der `2`Skala:

### <a name="data-annotations"></a>[Datenanmerkungen](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnDataType.cs?name=ColumnDataType&highlight=4,6)]

### <a name="fluent-api"></a>[Fließende API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnDataType.cs?name=ColumnDataType&highlight=5-6)]

***

### <a name="maximum-length"></a>Maximale Länge

Das Konfigurieren einer maximalen Länge gibt dem Datenbankanbieter einen Hinweis zum entsprechenden Spaltendatentyp, der für eine bestimmte Eigenschaft ausgewählt werden soll. Die maximale Länge gilt nur für Array Datentypen, z. b. `string` und `byte[]`.

> [!NOTE]
> Entity Framework führt keine Überprüfung der maximalen Länge durch, bevor Daten an den Anbieter übergeben werden. Der Anbieter oder Datenspeicher muss ggf. überprüft werden. Wenn Sie z. b. auf SQL Server abzielen, führt das Überschreiten der maximalen Länge zu einer Ausnahme, da der Datentyp der zugrunde liegenden Spalte nicht zulässt, dass überschüssige Daten gespeichert werden.

Im folgenden Beispiel bewirkt das Konfigurieren einer maximalen Länge von 500, dass eine Spalte vom Typ "`nvarchar(500)`" in SQL Server erstellt wird:

#### <a name="data-annotations"></a>[Datenanmerkungen](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?name=MaxLength&highlight=4)]

#### <a name="fluent-api"></a>[Fließende API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?name=MaxLength&highlight=3-5)]

***

## <a name="required-and-optional-properties"></a>Erforderliche und optionale Eigenschaften

Eine Eigenschaft wird als optional eingestuft, wenn Sie `null`enthalten soll. Wenn `null` kein gültiger Wert ist, der einer Eigenschaft zugewiesen werden soll, wird er als erforderliche Eigenschaft betrachtet. Bei der Zuordnung zu einem relationalen Datenbankschema werden erforderliche Eigenschaften als Spalten erstellt, die keine NULL-Werte zulassen, und optionale Eigenschaften werden als Spalten erstellt, die NULL-Werte zulassen.

### <a name="conventions"></a>Konventionen

Gemäß der Konvention wird eine Eigenschaft, deren .NET-Typ NULL enthalten kann, als optional konfiguriert, wohingegen Eigenschaften, deren .NET-Typ keinen NULL-Wert enthalten darf, als erforderlich konfiguriert werden. Beispielsweise werden alle Eigenschaften mit .net-Werttypen (`int`, `decimal`, `bool`usw.) als erforderlich konfiguriert, und alle Eigenschaften mit .net-Werttypen, die NULL-Werte zulassen (`int?`, `decimal?`, `bool?`usw.), werden als optional konfiguriert.

C#in 8 wurde ein neues Feature namens " [Werte zulässt Reference Types](/dotnet/csharp/tutorials/nullable-reference-types)" eingeführt, mit dem Verweis Typen mit Anmerkungen versehen werden können. Dies gibt an, ob es zulässig ist, dass NULL-Werte enthalten sind. Diese Funktion ist standardmäßig deaktiviert. Wenn Sie aktiviert ist, ändert Sie das Verhalten der EF Core auf folgende Weise:

* Wenn NULL-Werte zulassen (Standardeinstellung), werden alle Eigenschaften mit .net-Verweis Typen gemäß der Konvention (z. b. `string`) als optional konfiguriert.
* Wenn Verweis Typen, die NULL-Werte zulassen, aktiviert sind, werden die C# Eigenschaften basierend auf der NULL-Zulässigkeit ihres .net-Typs konfiguriert: `string?` als optional konfiguriert werden, während `string` als erforderlich konfiguriert wird.

Das folgende Beispiel zeigt einen Entitätstyp mit erforderlichen und optionalen Eigenschaften, wobei die Verweis Funktion NULL-Werte ist deaktiviert (Standard) und aktiviert ist:

#### <a name="without-nullable-reference-types-default"></a>[Ohne Verweis Typen, die NULL-Werte zulassen (Standard)](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

#### <a name="with-nullable-reference-types"></a>[Mit auf NULL festleg baren Verweis Typen](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

Die Verwendung von Verweis Typen, die NULL-Werte zulassen, wird empfohlen, C# da Sie die im Code ausgedrückte NULL-Zulässigkeit in EF Core Modell und in der Datenbank überträgt und die Verwendung der flüssigen API oder Daten Anmerkungen zum doppelten Ausdrücken des gleichen Konzepts überflüssig macht.

> [!NOTE]
> Vorsicht beim Aktivieren von Verweis Typen, die NULL-Werte zulassen, für ein vorhandenes Projekt: Verweistyp Eigenschaften, die zuvor als optional konfiguriert wurden, werden nun als erforderlich konfiguriert, es sei denn, Sie sind explizit mit Anmerkungen versehen, die NULL-Werte zulassen. Wenn Sie ein relationales Datenbankschema verwalten, kann dies dazu führen, dass Migrationen generiert werden, die die NULL-Zulässigkeit der Daten Bank Spalte ändern.

Weitere Informationen zu Verweis Typen, die NULL-Werte zulassen, und deren Verwendung mit EF Core finden Sie auf [der dedizierten Dokumentationsseite für dieses Feature](xref:core/miscellaneous/nullable-reference-types).

### <a name="explicit-configuration"></a>Explizite Konfiguration

Eine Eigenschaft, die gemäß der Konvention optional ist, kann so konfiguriert werden, dass Sie wie folgt erforderlich ist:

#### <a name="data-annotations"></a>[Datenanmerkungen](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?name=Required&highlight=4)]

#### <a name="fluent-api"></a>[Fließende API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?name=Required&highlight=3-5)]

***
