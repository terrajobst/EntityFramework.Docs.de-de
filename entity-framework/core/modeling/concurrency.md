---
title: "Parallelitätstoken - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc8b1cb0-befe-4b67-8004-26e6c5f69385
ms.technology: entity-framework-core
uid: core/modeling/concurrency
ms.openlocfilehash: 6574a9098d38c4aa525ffb4896adb01082420b5f
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2017
---
# <a name="concurrency-tokens"></a>Parallelitätstoken

Wenn eine Eigenschaft als ein parallelitätstoken konfiguriert ist prüft EF dann, dass kein anderer Benutzer beim Speichern der Änderungen an diesen Datensatz der Wert in der Datenbank nicht geändert wurde. EF ein Muster für die vollständige Parallelität verwendet, d. h. sie wird wird davon ausgegangen, dass der Wert nicht geändert hat und versuchen Sie es, speichern Sie die Daten auszulösen, wenn es feststellt, dass der Wert geändert wurde.

Wir möchten z. B. konfigurieren `LastName` auf `Person` ein parallelitätstoken ist. Dies bedeutet, dass, wenn ein Benutzer versucht, speichern einige Änderungen an einer `Person`, jedoch einem anderen Benutzer geändert wurde die `LastName` und dann eine Ausnahme ausgelöst wird. Dies kann wünschenswert sein, damit Ihre Anwendung kann der Benutzer aufgefordert, stellen Sie sicher, dass dieser Datensatz weiterhin dieselbe tatsächlichen Person darstellt, bevor Sie ihre Änderungen speichern.

> [!NOTE]
> Auf dieser Seite dokumentiert, wie Sie parallelitätstoken zu konfigurieren. Finden Sie unter [Behandeln von Parallelität](../saving/concurrency.md) für Beispiele für vollständige Parallelität in Ihrer Anwendung verwenden.

## <a name="how-concurrency-tokens-work-in-ef"></a>Funktionsweise von parallelitätstoken in EF

Datenspeicher können parallelitätstoken erzwingen, indem Sie überprüfen, dass ein Datensatz, aktualisieren oder löschen noch den gleichen Wert für die parallelitätstoken, der zugewiesen wurde verfügt, wenn der Kontext ursprünglich die Daten aus der Datenbank geladen.

Z. B. relationale Datenbanken erreichen, indem z. B. im parallelitätstoken in der `WHERE` Klausel beliebiger `UPDATE` oder `DELETE` Befehle und überprüfen die Anzahl der Zeilen, die betroffen sind. Wenn immer noch im parallelitätstoken übereinstimmt, wird eine Zeile aktualisiert werden. Wenn der Wert in der Datenbank geändert wurde, werden keine Zeilen aktualisiert.

```sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

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
