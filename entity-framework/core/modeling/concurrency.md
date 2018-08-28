---
title: Parallelitätstoken – EF Core
author: rowanmiller
ms.date: 03/03/2018
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
uid: core/modeling/concurrency
ms.openlocfilehash: 0051d416544a11385f99d36e45843c5b20725af7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994225"
---
# <a name="concurrency-tokens"></a>Parallelitätstoken

> [!NOTE]
> Diese Seite dokumentiert parallelitätstoken zu konfigurieren. Finden Sie unter [Behandlung von Nebenläufigkeitskonflikten](../saving/concurrency.md) für eine ausführliche Erläuterung der Funktionsweise der parallelitätssteuerung in EF Core und Beispiel Behandlung von nebenläufigkeitskonflikten in Ihrer Anwendung.

Eigenschaften, die als parallelitätstoken konfiguriert werden verwendet, um die Steuerung für optimistische Parallelität implementieren.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention werden Eigenschaften nicht als parallelitätstoken konfiguriert.

## <a name="data-annotations"></a>Datenanmerkungen

Sie können die Datenanmerkungen verwenden, um eine Eigenschaft als ein parallelitätstoken zu konfigurieren.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Concurrency.cs#ConfigureConcurrencyAnnotations)]

## <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, um eine Eigenschaft als ein parallelitätstoken zu konfigurieren.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Concurrency.cs#ConfigureConcurrencyFluent)]

## <a name="timestamprow-version"></a>Zeitstempel/Zeilenversion

Ein Zeitstempel ist eine Eigenschaft, wird ein neuer Wert von der Datenbank generiert jedes Mal, wenn eine Zeile eingefügt oder aktualisiert wird. Die Eigenschaft wird auch als ein parallelitätstoken behandelt. Dadurch wird sichergestellt, dass Sie eine Ausnahme angezeigt werden, wenn anderen Person eine Zeile, die Sie versuchen geändert hat, die aktualisiert werden, weil Sie für die Daten abgefragt.

Wie dies erreicht wird, obliegt den Datenbankanbieter, der verwendet wird. Für SQL Server Zeitstempel wird in der Regel verwendet, auf eine *Byte []* setup-Eigenschaft, die wird als eine *ROWVERSION* Spalte in der Datenbank.

### <a name="conventions"></a>Konventionen

Gemäß der Konvention werden Eigenschaften nicht als Zeitstempel konfiguriert.

### <a name="data-annotations"></a>Datenanmerkungen

Sie können Datenanmerkungen verwenden, so konfigurieren Sie eine Eigenschaft als einen Zeitstempel.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Timestamp.cs#ConfigureTimestampAnnotations)]

### <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, so konfigurieren Sie eine Eigenschaft als einen Zeitstempel.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Timestamp.cs#ConfigureTimestampFluent)]
