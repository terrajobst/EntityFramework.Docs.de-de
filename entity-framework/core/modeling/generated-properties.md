---
title: Generierten Werte - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: eb082011-11a1-41b4-a108-15daafa03e80
ms.technology: entity-framework-core
uid: core/modeling/generated-properties
ms.openlocfilehash: 892494461bcf49ee10d05c972da0ba19ca003c35
ms.sourcegitcommit: 4b7d3d3e258b0d9cb778bb45a9f4a33c0792e38e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="generated-values"></a>Generierten Werte

## <a name="value-generation-patterns"></a>Wert Generation Muster

Es gibt drei Wert Generation Muster, die für Eigenschaften verwendet werden können.

### <a name="no-value-generation"></a>Kein Wert generiert

Kein Wert generiert bedeutet, dass Sie einen gültigen Wert in der Datenbank gespeichert werden sollen immer geliefert werden. Neue Entitäten muss dieser gültige Wert zugewiesen werden, bevor sie an den Kontext hinzugefügt werden.

### <a name="value-generated-on-add"></a>Auf generierten Wert hinzufügen

Auf generierten Wert hinzufügen bedeutet, der ein Wert für neue Entitäten generiert wird.

Abhängig von der Datenbankanbieter verwendet wird, können Werte sein Clientseite von EF oder in der Datenbank generiert. Wenn der Wert von der Datenbank generiert wird, möglicherweise EF einen temporären Wert zuweisen, wenn Sie den Kontext die Entität hinzufügen. Diese temporäre Wert dann ersetzt werden durch den Wert der Datenbank generierte während `SaveChanges()`.

Wenn Sie den Kontext eine Entität, dessen Wert der Eigenschaft zugewiesen hinzufügen, versucht EF, legen Sie diesen Wert, sondern einen neuer Typ generiert. Eine Eigenschaft gilt ein Wert zugewiesen, wenn er nicht den CLR-Standardwert zugewiesen ist (`null` für `string`, `0` für `int`, `Guid.Empty` für `Guid`usw..). Weitere Informationen finden Sie unter [expliziten Werte für die generierten Eigenschaften](..\saving\explicit-values-generated-properties.md).

> [!WARNING]  
> Wie der Wert für die hinzugefügten Entitäten generiert wird, hängen für den Datenbankanbieter verwendet wird. Datenbankanbieter möglicherweise automatisch Wert Generation für bestimmte Eigenschaftentypen einrichten, aber andere müssen Sie möglicherweise manuell einrichten, wie der Wert generiert wird.
>
> Beispielsweise bei Verwendung von SQL Server, Werte automatisch generiert werden für `GUID` Eigenschaften (mithilfe des SQL Server sequenziellen GUID-Algorithmus). Jedoch, wenn Sie, dass angeben eine `DateTime` Eigenschaft generiert wird auf Hinzufügen, dann müssen Sie richten Sie eine Möglichkeit für die Werte generiert werden soll. Eine Möglichkeit hierzu ist, so konfigurieren Sie den Standardwert `GETDATE()`, finden Sie unter [Standardwerte](relational/default-values.md).

### <a name="value-generated-on-add-or-update"></a>Generierter Wert hinzufügen oder aktualisieren

Generierter Wert hinzufügen oder Update bedeutet, dass ein neuer Wert generiert wird, jedes Mal, wenn der Datensatz (INSERT- oder Update) gespeichert wird.

Wie `value generated on add`, wenn Sie einen Wert für die Eigenschaft auf eine neu hinzugefügte Instanz einer Entität angeben, dass der Wert eingefügt werden soll, anstatt einen Wert generiert wird. Es ist auch möglich, einen expliziten Wert festzulegen, bei der Aktualisierung. Weitere Informationen finden Sie unter [expliziten Werte für die generierten Eigenschaften](..\saving\explicit-values-generated-properties.md).

> [!WARNING]  
> Wie der Wert für die hinzugefügten und aktualisierten Entitäten generiert wird, hängen für den Datenbankanbieter verwendet wird. Datenbankanbieter möglicherweise automatisch Wert Generation für bestimmte Eigenschaftentypen einrichten, während andere Benutzer müssen Sie manuell einrichten, wie der Wert generiert wird.
>
> Bei Verwendung von SQL Server, beispielsweise `byte[]` auf generiert festgelegt hinzuzufügen oder zu aktualisieren und markiert als parallelitätstoken, wird Setup mit der `rowversion` -Datentyp - so, dass die Werte in der Datenbank generiert werden sollen. Jedoch, wenn Sie, dass angeben eine `DateTime` Eigenschaft generiert wird auf hinzuzufügen oder zu aktualisieren, dann müssen Sie richten Sie eine Möglichkeit für die Werte generiert werden soll. Eine Möglichkeit hierzu ist, so konfigurieren Sie den Standardwert `GETDATE()` (siehe [Standardwerte](relational/default-values.md)) zum Generieren von Werten für neue Zeilen. Klicken Sie dann können einen Trigger auf Datenbankebene Sie zum Generieren von Werten während eines Updates (z. B. das folgende Beispieltrigger).
>
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>Konventionen

Gemäß der Konvention nicht zusammengesetzte Primärschlüssel vom Typ short Int, long oder Guid werden Setup hinzufügen auf generierten Werte aufweisen. Alle anderen Eigenschaften werden Setup mit kein Wert generiert werden.

## <a name="data-annotations"></a>Datenanmerkungen

### <a name="no-value-generation-data-annotations"></a>Kein Wert generiert (Datenanmerkungen)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>Auf generierten Wert hinzufügen (Datenanmerkungen)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> Auf diese Weise können nur auf EF wissen, dass die Werte für hinzugefügte Elemente generiert werden, es kann nicht garantiert, dass EF setup wird für den eigentlichen Mechanismus, um Werte zu generieren. Finden Sie unter [hinzufügen generierter auf](#value-generated-on-add) Abschnitt, um weitere Details.

### <a name="value-generated-on-add-or-update-data-annotations"></a>Auf generierten Wert hinzufügen oder aktualisieren (Datenanmerkungen)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Auf diese Weise können nur auf EF wissen, dass die Werte für hinzugefügte oder aktualisierte Entitäten generiert werden, es kann nicht garantiert, dass EF setup wird für den eigentlichen Mechanismus, um Werte zu generieren. Finden Sie unter [auf generierten Wert hinzufügen oder aktualisieren](#value-generated-on-add-or-update) Abschnitt, um weitere Details.

## <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, so ändern Sie das Value-Generation-Muster für eine bestimmte Eigenschaft.

### <a name="no-value-generation-fluent-api"></a>Kein Wert generiert (Fluent-API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>Auf generierten Wert hinzufügen (Fluent-API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()` können nur EF wissen, dass die Werte für hinzugefügte Elemente generiert werden, es kann nicht garantiert, dass EF setup wird für den eigentlichen Mechanismus, um Werte zu generieren.  Finden Sie unter [hinzufügen generierter auf](#value-generated-on-add) Abschnitt, um weitere Details.

### <a name="value-generated-on-add-or-update-fluent-api"></a>Auf generierten Wert hinzufügen oder aktualisieren (Fluent-API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Auf diese Weise können nur auf EF wissen, dass die Werte für hinzugefügte oder aktualisierte Entitäten generiert werden, es kann nicht garantiert, dass EF setup wird für den eigentlichen Mechanismus, um Werte zu generieren. Finden Sie unter [auf generierten Wert hinzufügen oder aktualisieren](#value-generated-on-add-or-update) Abschnitt, um weitere Details.
