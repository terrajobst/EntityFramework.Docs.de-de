---
title: "Parallelitätstoken - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: f3cf28d5c54e63aa76058e9fe1d9f3de5b37d579
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2018
---
# <a name="concurrency-tokens"></a>Parallelitätstoken

> [!NOTE]
> Auf dieser Seite dokumentiert, wie Sie parallelitätstoken zu konfigurieren. Finden Sie unter [Behandlung von Parallelitätskonflikten](../saving/concurrency.md) für eine ausführliche Erklärung der Funktionsweise der parallelitätssteuerung auf EF-Kern- und Beispiele zum Parallelitätskonflikte in Ihrer Anwendung zu behandeln.

Eigenschaften, die als parallelitätstoken verwendet werden, um die Steuerung durch vollständige Parallelität implementieren konfiguriert werden.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention werden die Eigenschaften nie als parallelitätstoken konfiguriert.

## <a name="data-annotations"></a>Datenanmerkungen

Sie können die Datenanmerkungen so konfigurieren Sie eine Eigenschaft als ein parallelitätstoken verwenden.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, so konfigurieren Sie eine Eigenschaft als ein parallelitätstoken.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Timestamp/Zeilenversion

Ein Zeitstempel ist eine Eigenschaft, wird ein neuer Wert von der Datenbank generiert jedes Mal, wenn eine Zeile eingefügt oder aktualisiert wird. Die Eigenschaft wird auch als ein parallelitätstoken behandelt. Dadurch wird sichergestellt, dass eine Ausnahme erhalten Sie, wenn andere Benutzer eine Zeile geändert hat, die aktualisiert werden, weil Sie für die Daten abgefragt werden soll.

Wird dies erreicht ist, bis zu der Datenbankanbieter verwendet wird. Für SQL Server wird in der Regel Zeitstempel auf verwendet eine *Byte []* setup-Eigenschaft, die als eine *ROWVERSION* Spalte in der Datenbank.

### <a name="conventions"></a>Konventionen

Gemäß der Konvention werden die Eigenschaften nie als Zeitstempel konfiguriert.

### <a name="data-annotations"></a>Datenanmerkungen

Datenanmerkungen können Sie eine Eigenschaft als Zeitstempel konfigurieren.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, so konfigurieren Sie eine Eigenschaft als einen Zeitstempel.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
