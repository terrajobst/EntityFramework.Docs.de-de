---
title: Parallelitäts Token-EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: 8a5f3aa09c2a83d5be0998a11ef2ee8100437514
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781143"
---
# <a name="concurrency-tokens"></a>Parallelitätstoken

> [!NOTE]
> Auf dieser Seite wird das Konfigurieren von Parallelitäts Token dokumentiert. Eine ausführliche Erläuterung der Funktionsweise der Parallelitäts Steuerung in EF Core und Beispielen zur Behandlung von Parallelitäts Konflikten in der Anwendung finden Sie unter Behandeln von Parallelitäts [Konflikten](../saving/concurrency.md) .

Als Parallelitäts Token konfigurierte Eigenschaften werden verwendet, um die Steuerung der vollständigen Parallelität zu implementieren.

## <a name="configuration"></a>-Konfiguration

### <a name="data-annotationstabdata-annotations"></a>[Datenanmerkungen](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Concurrency.cs?name=Concurrency&highlight=5)]

### <a name="fluent-apitabfluent-api"></a>[Fließende API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Concurrency.cs?name=Concurrency&highlight=5)]

***

## <a name="timestamprowversion"></a>Timestamp/rowversion

Ein Timestamp/rowversion-Objekt ist eine Eigenschaft, für die ein neuer Wert automatisch von der Datenbank generiert wird, wenn eine Zeile eingefügt oder aktualisiert wird. Die-Eigenschaft wird auch als Parallelitäts Token behandelt, um sicherzustellen, dass Sie eine Ausnahme erhalten, wenn sich eine Zeile, die Sie aktualisieren, geändert hat, seit Sie Sie abgefragt haben. Die genauen Details hängen vom verwendeten Datenbankanbieter ab. für SQL Server wird normalerweise eine *Byte []* -Eigenschaft verwendet, die als *rowversion* -Spalte in der Datenbank eingerichtet wird.

Sie können eine Eigenschaft wie folgt als timestamp/rowversion konfigurieren:

### <a name="data-annotationstabdata-annotations"></a>[Datenanmerkungen](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Timestamp.cs?name=Timestamp&highlight=7)]

### <a name="fluent-apitabfluent-api"></a>[Fließende API](#tab/fluent-api)

[! Code-CSharp [Main] (.. /.. /.. /Samples/Core/Modeling/FluentAPI/Timestamp.cs? Name = Zeitstempel & Hervorhebung = 9, 17]

***
