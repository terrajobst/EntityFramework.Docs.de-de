---
title: Generierte Werte-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: eb082011-11a1-41b4-a108-15daafa03e80
uid: core/modeling/generated-properties
ms.openlocfilehash: 6b38fd2e540ec29674f1116e7c204052d06ca1bc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197427"
---
# <a name="generated-values"></a>Generierte Werte

## <a name="value-generation-patterns"></a>Muster Generierungs Muster

Es gibt drei Muster Generierungs Muster, die für Eigenschaften verwendet werden können:
* Keine Wert Generierung
* Beim Hinzufügen generierter Wert
* Beim Hinzufügen oder aktualisieren generierter Wert

### <a name="no-value-generation"></a>Keine Wert Generierung

Keine Wert Generierung bedeutet, dass Sie immer einen gültigen Wert angeben, der in der Datenbank gespeichert werden soll. Dieser gültige Wert muss neuen Entitäten zugewiesen werden, bevor Sie dem Kontext hinzugefügt werden.

### <a name="value-generated-on-add"></a>Beim Hinzufügen generierter Wert

Beim Hinzufügen generierter Wert bedeutet, dass für neue Entitäten ein Wert generiert wird.

Abhängig vom verwendeten Datenbankanbieter können die Werte Client seitig von EF oder in der Datenbank generiert werden. Wenn der Wert von der Datenbank generiert wird, kann EF einen temporären Wert zuweisen, wenn Sie die Entität zum Kontext hinzufügen. Dieser temporäre Wert wird dann durch den von der Datenbank generierten Wert `SaveChanges()`ersetzt.

Wenn Sie dem Kontext eine Entität hinzufügen, die über einen Wert verfügt, der der Eigenschaft zugewiesen ist, versucht EF, diesen Wert einzufügen, anstatt einen neuen zu erstellen. Einer Eigenschaft wird ein Wert zugewiesen, wenn`null` ihr kein CLR-Standardwert ( `0` für `Guid.Empty` `string` `int` `Guid`, für, usw.) zugewiesen ist. Weitere Informationen finden Sie unter [explizite Werte für generierte Eigenschaften](../saving/explicit-values-generated-properties.md).

> [!WARNING]  
> Die Art, wie der Wert für hinzugefügte Entitäten generiert wird, hängt vom verwendeten Datenbankanbieter ab. Datenbankanbieter können die Wert Generierung für einige Eigenschafts Typen automatisch einrichten, bei anderen müssen Sie jedoch möglicherweise manuell einrichten, wie der Wert generiert wird.
>
> Wenn Sie z. b. SQL Server verwenden, werden Werte automatisch für `GUID` Eigenschaften generiert (mithilfe des SQL Server sequenziellen GUID-Algorithmus). Wenn Sie jedoch angeben, dass eine `DateTime` Eigenschaft beim Hinzufügen generiert wird, müssen Sie eine Möglichkeit einrichten, damit die Werte generiert werden. Eine Möglichkeit, dies zu erreichen, besteht darin, einen Standardwert `GETDATE()`von zu konfigurieren, siehe [Standardwerte](relational/default-values.md).

### <a name="value-generated-on-add-or-update"></a>Beim Hinzufügen oder aktualisieren generierter Wert

Der beim Hinzufügen oder aktualisieren generierte Wert bedeutet, dass bei jedem Speichern des Datensatzes (einfügen oder aktualisieren) ein neuer Wert generiert wird.

`value generated on add`Wenn Sie z. b. einen Wert für die-Eigenschaft in einer neu hinzugefügten Instanz einer Entität angeben, wird dieser Wert anstelle eines generierten Werts eingefügt. Es ist auch möglich, bei der Aktualisierung einen expliziten Wert festzulegen. Weitere Informationen finden Sie unter [explizite Werte für generierte Eigenschaften](../saving/explicit-values-generated-properties.md).

> [!WARNING]
> Die Art und Weise, wie der Wert für hinzugefügte und aktualisierte Entitäten generiert wird, hängt vom verwendeten Datenbankanbieter ab. Datenbankanbieter können die Wert Generierung für einige Eigenschafts Typen automatisch einrichten, während andere die Generierung des Werts manuell einrichten müssen.
> 
> Wenn Sie z. b. SQL Server `byte[]` verwenden, werden Eigenschaften, die beim Hinzufügen oder aktualisieren als generiert und als Parallelitäts Token gekennzeichnet sind, mit `rowversion` dem-Datentyp eingerichtet, sodass Werte in der Datenbank generiert werden. Wenn Sie jedoch angeben, dass eine `DateTime` Eigenschaft beim Hinzufügen oder aktualisieren generiert wird, müssen Sie eine Möglichkeit einrichten, damit die Werte generiert werden. Eine Möglichkeit hierfür besteht darin, den Standardwert `GETDATE()` (siehe [Standardwerte](relational/default-values.md)) zu konfigurieren, um Werte für neue Zeilen zu generieren. Sie können dann mithilfe eines Daten Bank Auslösers Werte während der Aktualisierung generieren (z. b. im folgenden Beispiel-auslösen).
> 
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>Konventionen

Gemäß der Konvention werden nicht zusammengesetzte Primärschlüssel vom Typ Short, int, Long oder GUID eingerichtet, sodass beim Hinzufügen Werte generiert werden. Alle anderen Eigenschaften werden ohne Wert Generierung eingerichtet.

## <a name="data-annotations"></a>Datenanmerkungen

### <a name="no-value-generation-data-annotations"></a>Keine Wert Generierung (Daten Anmerkungen)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>Beim Hinzufügen generierter Wert (Daten Anmerkungen)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> Dies ermöglicht EF nur zu wissen, dass Werte für hinzugefügte Entitäten generiert werden, und es wird nicht garantiert, dass EF den eigentlichen Mechanismus zum Generieren von Werten eingibt. Weitere Informationen finden [Sie unter im Abschnitt "hinzufügen" generierter Wert](#value-generated-on-add) .

### <a name="value-generated-on-add-or-update-data-annotations"></a>Beim Hinzufügen oder aktualisieren generierter Wert (Daten Anmerkungen)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Dies ermöglicht EF nur zu wissen, dass Werte für hinzugefügte oder aktualisierte Entitäten generiert werden. es wird nicht garantiert, dass EF den eigentlichen Mechanismus zum Generieren von Werten eingibt. Weitere Informationen finden [Sie im Abschnitt hinzufügen oder aktualisieren generierter Wert](#value-generated-on-add-or-update) .

## <a name="fluent-api"></a>Fluent-API

Sie können die fließende API verwenden, um das Muster für die Generierung von Werten für eine bestimmte Eigenschaft zu ändern.

### <a name="no-value-generation-fluent-api"></a>Keine Wert Generierung (fließende API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>Beim Hinzufügen generierter Wert (fließende API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()`Sie können EF nur wissen, dass Werte für hinzugefügte Entitäten generiert werden, und es wird nicht garantiert, dass EF den eigentlichen Mechanismus zum Generieren von Werten eingibt.  Weitere Informationen finden [Sie unter im Abschnitt "hinzufügen" generierter Wert](#value-generated-on-add) .

### <a name="value-generated-on-add-or-update-fluent-api"></a>Beim Hinzufügen oder aktualisieren generierter Wert (fließende API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Dies ermöglicht EF nur zu wissen, dass Werte für hinzugefügte oder aktualisierte Entitäten generiert werden. es wird nicht garantiert, dass EF den eigentlichen Mechanismus zum Generieren von Werten eingibt. Weitere Informationen finden [Sie im Abschnitt hinzufügen oder aktualisieren generierter Wert](#value-generated-on-add-or-update) .
