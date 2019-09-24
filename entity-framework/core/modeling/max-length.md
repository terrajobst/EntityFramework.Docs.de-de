---
title: Maximale Länge-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: b6f0594fed0c491b4f79dcda5273cdebe9ecf35f
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197224"
---
# <a name="maximum-length"></a>Maximale Länge

Das Konfigurieren einer maximalen Länge stellt einen Hinweis für den Datenspeicher für den entsprechenden Datentyp bereit, der für eine bestimmte Eigenschaft verwendet werden soll. Die maximale Länge gilt nur für Array Datentypen, `string` z `byte[]`. b. und.

> [!NOTE]  
> Entity Framework führt keine Überprüfung der maximalen Länge durch, bevor Daten an den Anbieter übergeben werden. Der Anbieter oder Datenspeicher muss ggf. überprüft werden. Wenn Sie z. b. auf SQL Server abzielen, führt das Überschreiten der maximalen Länge zu einer Ausnahme, da der Datentyp der zugrunde liegenden Spalte nicht zulässt, dass überschüssige Daten gespeichert werden.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention wird es dem Datenbankanbieter überlassen, einen geeigneten Datentyp für Eigenschaften auszuwählen. Bei Eigenschaften mit einer Länge wählt der Datenbankanbieter im Allgemeinen einen Datentyp aus, der die längste Daten Länge zulässt. Microsoft SQL Server werden z. b. `nvarchar(max)` für `string` Eigenschaften verwenden ( `nvarchar(450)` oder, wenn die Spalte als Schlüssel verwendet wird).

## <a name="data-annotations"></a>Datenanmerkungen

Mit den Daten Anmerkungen können Sie eine maximale Länge für eine Eigenschaft konfigurieren. In diesem Beispiel führt die Ziel SQL Server dies dazu, dass `nvarchar(500)` der Datentyp verwendet wird.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?highlight=14)]

## <a name="fluent-api"></a>Fluent-API

Sie können die fließende API verwenden, um eine maximale Länge für eine Eigenschaft zu konfigurieren. In diesem Beispiel führt die Ziel SQL Server dies dazu, dass `nvarchar(500)` der Datentyp verwendet wird.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?highlight=11-13)]
