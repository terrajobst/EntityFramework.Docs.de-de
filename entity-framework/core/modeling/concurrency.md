---
title: Parallelitäts Token-EF Core
author: rowanmiller
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: db768c1de99000be91d33764ccd3c3924237f8bb
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197451"
---
# <a name="concurrency-tokens"></a>Parallelitätstoken

> [!NOTE]
> Auf dieser Seite wird das Konfigurieren von Parallelitäts Token dokumentiert. Eine ausführliche Erläuterung der Funktionsweise der Parallelitäts Steuerung in EF Core und Beispielen zur Behandlung von Parallelitäts Konflikten in der Anwendung finden Sie unter Behandeln von Parallelitäts [Konflikten](../saving/concurrency.md) .

Als Parallelitäts Token konfigurierte Eigenschaften werden verwendet, um die Steuerung der vollständigen Parallelität zu implementieren.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention werden Eigenschaften nie als Parallelitäts Token konfiguriert.

## <a name="data-annotations"></a>Datenanmerkungen

Mit den Daten Anmerkungen können Sie eine Eigenschaft als Parallelitäts Token konfigurieren.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>Fluent-API

Sie können die fließende API verwenden, um eine Eigenschaft als Parallelitäts Token zu konfigurieren.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Zeitstempel/Zeilen Version

Ein Zeitstempel ist eine Eigenschaft, bei der jedes Mal, wenn eine Zeile eingefügt oder aktualisiert wird, ein neuer Wert von der Datenbank generiert wird. Die-Eigenschaft wird auch als Parallelitäts Token behandelt. Dadurch wird sichergestellt, dass Sie eine Ausnahme erhalten, wenn eine Zeile, die Sie aktualisieren möchten, geändert wurde, weil Sie die Daten abgefragt haben.

Wie dies erreicht wird, ist der Datenbankanbieter, der verwendet wird. Für SQL Server wird Zeitstempel in der Regel für die Eigenschaft " *Byte []* " verwendet, die als *rowversion* -Spalte in der Datenbank eingerichtet wird.

### <a name="conventions"></a>Konventionen

Gemäß der Konvention werden Eigenschaften nie als Zeitstempel konfiguriert.

### <a name="data-annotations"></a>Datenanmerkungen

Mithilfe von Daten Anmerkungen können Sie eine Eigenschaft als Zeitstempel konfigurieren.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>Fluent-API

Sie können die fließende API verwenden, um eine Eigenschaft als Zeitstempel zu konfigurieren.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Timestamp.cs#ConfigureTimestampFluent)]
