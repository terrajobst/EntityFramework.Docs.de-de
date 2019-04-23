---
title: 'Maximale Länge: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: 3220518cb0a409b6e802d2f3a98acdb949ffbf56
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929848"
---
# <a name="maximum-length"></a>Maximale Länge

Konfigurieren eine maximale Länge enthält einen Hinweis für den Datenspeicher zu den entsprechenden Datentyp, der für eine bestimmte Eigenschaft verwendet. Maximale Länge gilt nur für Arraytypen, z. B. `string` und `byte[]`.

> [!NOTE]  
> Entitätsframework führt keine Validierung einer maximalen Länge vor der Übergabe von Daten an den Anbieter aus. Es ist Aufgabe der Anbieter oder einem Datenspeicher gespeichert, um bei Bedarf zu überprüfen. Z. B. bei der Zielgruppenadressierung von SQL Server, die maximale Paketlänge führt zu einer Ausnahme mit dem Datentyp der zugrunde liegenden Spalte nicht überschüssige Daten gespeichert werden können.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention wird es den Datenbankanbieter, wählen Sie einen entsprechenden Datentyp für Eigenschaften überlassen. Für Eigenschaften, die eine Länge aufweisen, wird der Anbieter in der Regel einen Datentyp auswählen, der die längste Länge der Daten ermöglicht. Microsoft SQL Server verwendet z. B. `nvarchar(max)` für `string` Eigenschaften (oder `nvarchar(450)` , wenn die Spalte als Schlüssel verwendet wird).

## <a name="data-annotations"></a>Datenanmerkungen

Sie können die Datenanmerkungen verwenden, so konfigurieren Sie eine maximale Länge für eine Eigenschaft. In diesem Beispiel als Ziel in SQL Server führt dies zu den `nvarchar(500)` -Datentyp verwendet wird.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=14)]

## <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, so konfigurieren Sie eine maximale Länge für eine Eigenschaft. In diesem Beispiel als Ziel in SQL Server führt dies zu den `nvarchar(500)` -Datentyp verwendet wird.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/MaxLength.cs?highlight=11-13)]
