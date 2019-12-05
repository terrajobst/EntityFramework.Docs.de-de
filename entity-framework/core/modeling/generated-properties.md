---
title: Generierte Werte-EF Core
description: Konfigurieren der Wert Generierung für Eigenschaften bei Verwendung von Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/generated-properties
ms.openlocfilehash: 7fa3eae5e2edb7b4c40ed4f99ce4a29f367e622a
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824699"
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

Abhängig vom verwendeten Datenbankanbieter können die Werte Client seitig von EF oder in der Datenbank generiert werden. Wenn der Wert von der Datenbank generiert wird, kann EF einen temporären Wert zuweisen, wenn Sie die Entität zum Kontext hinzufügen. Dieser temporäre Wert wird dann während `SaveChanges()`durch den von der Datenbank generierten Wert ersetzt.

Wenn Sie dem Kontext eine Entität hinzufügen, die über einen Wert verfügt, der der Eigenschaft zugewiesen ist, versucht EF, diesen Wert einzufügen, anstatt einen neuen zu erstellen. Einer Eigenschaft wird ein Wert zugewiesen, wenn ihr kein CLR-Standardwert (`null` für `string``0` für `int`, `Guid.Empty` für `Guid`usw. zugewiesen ist. Weitere Informationen finden Sie unter [explizite Werte für generierte Eigenschaften](../saving/explicit-values-generated-properties.md).

> [!WARNING]  
> Die Art, wie der Wert für hinzugefügte Entitäten generiert wird, hängt vom verwendeten Datenbankanbieter ab. Datenbankanbieter können die Wert Generierung für einige Eigenschafts Typen automatisch einrichten, bei anderen müssen Sie jedoch möglicherweise manuell einrichten, wie der Wert generiert wird.
>
> Wenn Sie z. b. SQL Server verwenden, werden Werte automatisch für `GUID` Eigenschaften generiert (mit dem SQL Server sequenziellen GUID-Algorithmus). Wenn Sie jedoch angeben, dass beim Hinzufügen eine `DateTime`-Eigenschaft generiert wird, müssen Sie eine Methode einrichten, um die Werte zu generieren. Eine Möglichkeit, dies zu erreichen, besteht darin, einen Standardwert `GETDATE()`zu konfigurieren. Weitere Informationen finden Sie unter [Standardwerte](relational/default-values.md).

### <a name="value-generated-on-add-or-update"></a>Beim Hinzufügen oder aktualisieren generierter Wert

Der beim Hinzufügen oder aktualisieren generierte Wert bedeutet, dass bei jedem Speichern des Datensatzes (einfügen oder aktualisieren) ein neuer Wert generiert wird.

Wenn Sie in einer neu hinzugefügten Instanz einer Entität einen Wert für die-Eigenschaft angeben, wird der Wert wie `value generated on add`anstelle eines generierten Werts eingefügt. Es ist auch möglich, bei der Aktualisierung einen expliziten Wert festzulegen. Weitere Informationen finden Sie unter [explizite Werte für generierte Eigenschaften](../saving/explicit-values-generated-properties.md).

> [!WARNING]
> Die Art und Weise, wie der Wert für hinzugefügte und aktualisierte Entitäten generiert wird, hängt vom verwendeten Datenbankanbieter ab. Datenbankanbieter können die Wert Generierung für einige Eigenschafts Typen automatisch einrichten, während andere die Generierung des Werts manuell einrichten müssen.
>
> Wenn Sie z. b. SQL Server verwenden, werden `byte[]` Eigenschaften, die beim Hinzufügen oder aktualisieren als generiert und als Parallelitäts Token gekennzeichnet sind, mit dem `rowversion`-Datentyp eingerichtet, sodass Werte in der Datenbank generiert werden. Wenn Sie jedoch angeben, dass eine `DateTime`-Eigenschaft bei Add oder Update generiert wird, müssen Sie eine Methode einrichten, um die Werte zu generieren. Eine Möglichkeit hierfür besteht darin, einen Standardwert `GETDATE()` (siehe [Standardwerte](relational/default-values.md)) zu konfigurieren, um Werte für neue Zeilen zu generieren. Sie können dann mithilfe eines Daten Bank Auslösers Werte während der Aktualisierung generieren (z. b. im folgenden Beispiel-auslösen).
>
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>Konventionen

Standardmäßig werden nicht zusammengesetzte Primärschlüssel vom Typ Short, int, Long oder GUID so eingerichtet, dass beim Hinzufügen Werte generiert werden. Alle anderen Eigenschaften werden ohne Wert Generierung eingerichtet.

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
> `ValueGeneratedOnAdd()` lässt EF lediglich wissen, dass Werte für hinzugefügte Entitäten generiert werden. Dies garantiert nicht, dass EF den eigentlichen Mechanismus zum Generieren von Werten einrichten wird.  Weitere Informationen finden [Sie unter im Abschnitt "hinzufügen" generierter Wert](#value-generated-on-add) .

### <a name="value-generated-on-add-or-update-fluent-api"></a>Beim Hinzufügen oder aktualisieren generierter Wert (fließende API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Dies ermöglicht EF nur zu wissen, dass Werte für hinzugefügte oder aktualisierte Entitäten generiert werden. es wird nicht garantiert, dass EF den eigentlichen Mechanismus zum Generieren von Werten eingibt. Weitere Informationen finden [Sie im Abschnitt hinzufügen oder aktualisieren generierter Wert](#value-generated-on-add-or-update) .
