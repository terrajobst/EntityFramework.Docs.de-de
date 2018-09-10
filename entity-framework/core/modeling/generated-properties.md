---
title: 'Generierte Werte sind möglich: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: eb082011-11a1-41b4-a108-15daafa03e80
uid: core/modeling/generated-properties
ms.openlocfilehash: 9ecfa924a0614f327f0bd202cb7dda95bea810af
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250698"
---
# <a name="generated-values"></a>Generierte Werte

## <a name="value-generation-patterns"></a>Wert-Generation-Muster

Es gibt drei Wert Generation Muster, die für Eigenschaften verwendet werden können:
* Kein Wert generiert
* Generiert am Wert hinzufügen
* Generierter Wert auf Hinzufügen oder aktualisieren

### <a name="no-value-generation"></a>Kein Wert generiert

Kein Wert generiert bedeutet, dass Sie einen gültigen Wert in der Datenbank gespeichert werden immer bereitgestellt werden. Neue Entitäten muss diese gültige Wert zugewiesen werden, bevor sie den Kontext hinzugefügt werden.

### <a name="value-generated-on-add"></a>Generiert am Wert hinzufügen

Mehrwert generiert bedeutet, die ein Wert für die neue Entitäten generiert wird.

-Werte können je nach den Datenbankanbieter, der verwendet wird, werden die Clientseite von EF oder in der Datenbank generiert. Wenn der Wert von der Datenbank generiert wird, kann EF einen temporären Wert zuweisen, wenn Sie die Entität dem Kontext hinzufügen. Diese temporäre Wert dann ersetzt werden durch den Wert generierten Datenbank während der `SaveChanges()`.

Wenn Sie eine Entität in den Kontext, die ein Wert der Eigenschaft zugewiesen wird hinzufügen, versucht EF, fügen Sie diesen Wert an, anstatt einen neuen zu generieren. Eine Eigenschaft gilt, um einen Wert zugewiesen wird, wenn sie nicht den CLR-Standardwert zugeordnet ist (`null` für `string`, `0` für `int`, `Guid.Empty` für `Guid`usw..). Weitere Informationen finden Sie unter [explizite Werte für generierte Eigenschaften](../saving/explicit-values-generated-properties.md).

> [!WARNING]  
> Wie der Wert für die hinzugefügte Entitäten generiert wird, hängt, wenn den Datenbankanbieter, der verwendet wird. Datenbankanbieter können automatisch Generation der Wert für einige Eigenschaftentypen einrichten, aber andere müssen Sie möglicherweise manuell einrichten, wie der Wert generiert wird.
>
> Z. B. Wenn Sie SQL Server verwenden möchten, Werte automatisch generiert werden für `GUID` Eigenschaften (mit der SQL Server-Algorithmus der sequenzielle GUID). Aber wenn Sie, dass angeben eine `DateTime` -Eigenschaft generiert wird hinzufügen, dann müssen Sie richten Sie eine Möglichkeit für die Werte generiert werden soll. Eine Möglichkeit dazu besteht darin konfigurieren Sie den Standardwert `GETDATE()`, finden Sie unter [Standardwerte](relational/default-values.md).

### <a name="value-generated-on-add-or-update"></a>Generierter Wert auf Hinzufügen oder aktualisieren

Generierter Wert hinzufügen oder Update bedeutet, dass ein neuer Wert generiert wird, jedes Mal, wenn der Eintrag (Insert oder Update) gespeichert ist.

Wie `value generated on add`, wenn Sie einen Wert für die Eigenschaft auf eine neu hinzugefügte Instanz einer Entität angeben, dass der Wert eingefügt werden soll, anstatt einen Wert, der generiert wird. Es ist auch möglich, einen expliziten Wert festzulegen, bei der Aktualisierung. Weitere Informationen finden Sie unter [explizite Werte für generierte Eigenschaften](../saving/explicit-values-generated-properties.md).

> [!WARNING]
> Wie der Wert für hinzugefügter und aktualisierter Entitäten generiert wird, hängt, wenn den Datenbankanbieter, der verwendet wird. Datenbankanbieter können automatisch Generation der Wert für einige Eigenschaftstypen einrichten, während andere Benutzer müssen Sie manuell einrichten, wie der Wert generiert wird.
> 
> Bei Verwendung von SQL Server, beispielsweise `byte[]` Eigenschaften, wie auf generiert festgelegt werden, hinzufügen oder aktualisieren und als parallelitätstoken, wird Setup mit der `rowversion` -Datentyp - so, dass Werte in der Datenbank generiert werden. Aber wenn Sie, dass angeben eine `DateTime` -Eigenschaft generiert wird auf hinzuzufügen oder zu aktualisieren, müssen Sie einrichten, eine Möglichkeit für die Werte generiert werden soll. Eine Möglichkeit dazu besteht darin konfigurieren Sie den Standardwert `GETDATE()` (finden Sie unter [Standardwerte](relational/default-values.md)) zum Generieren von Werten für neue Zeilen. Sie können dann einen Trigger auf Datenbankebene verwenden, zum Generieren von Werten während eines Updates (z. B. die folgenden Beispiele für Trigger).
> 
> [!code-sql[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.sql)]

## <a name="conventions"></a>Konventionen

Gemäß der Konvention nicht zusammengesetzte Primärschlüssel vom Typ short, Int, long oder Guid wird eingerichtet werden, dass Werte generiert, auf Hinzufügen. Alle anderen Eigenschaften werden Setup kein Wert generiert.

## <a name="data-annotations"></a>Datenanmerkungen

### <a name="no-value-generation-data-annotations"></a>Kein Wert generiert (Datenanmerkungen)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-data-annotations"></a>Mehrwert generiert (Datenanmerkungen)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> Dadurch können nur EF darüber informiert, dass die Werte für hinzugefügte Entitäten generiert werden, dies garantiert nicht, dass EF den eigentlichen Mechanismus zum Generieren von Werten einrichten werden. Finden Sie unter [Mehrwert generiert](#value-generated-on-add) finden Sie weitere Details.

### <a name="value-generated-on-add-or-update-data-annotations"></a>Generiert am Wert hinzufügen oder aktualisieren (Datenanmerkungen)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Dadurch können nur EF darüber informiert, dass die Werte für hinzugefügte oder aktualisierte Entitäten generiert werden, dies garantiert nicht, dass EF den eigentlichen Mechanismus zum Generieren von Werten einrichten werden. Finden Sie unter [auf generierter Wert hinzufügen oder Aktualisieren von](#value-generated-on-add-or-update) finden Sie weitere Details.

## <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, um das Value-Generation-Muster für eine bestimmte Eigenschaft zu ändern.

### <a name="no-value-generation-fluent-api"></a>Kein Wert generiert (Fluent-API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedNever.cs#Sample)]

### <a name="value-generated-on-add-fluent-api"></a>Mehrwert generiert (Fluent-API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAdd.cs#Sample)]

> [!WARNING]  
> `ValueGeneratedOnAdd()` können nur wissen, dass Werte für hinzugefügte Entitäten generiert werden, dies garantiert nicht, dass EF den eigentlichen Mechanismus zum Generieren von Werten einrichten, wird EF.  Finden Sie unter [Mehrwert generiert](#value-generated-on-add) finden Sie weitere Details.

### <a name="value-generated-on-add-or-update-fluent-api"></a>Generiert am Wert hinzufügen oder aktualisieren (Fluent-API)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/ValueGeneratedOnAddOrUpdate.cs#Sample)]

> [!WARNING]  
> Dadurch können nur EF darüber informiert, dass die Werte für hinzugefügte oder aktualisierte Entitäten generiert werden, dies garantiert nicht, dass EF den eigentlichen Mechanismus zum Generieren von Werten einrichten werden. Finden Sie unter [auf generierter Wert hinzufügen oder Aktualisieren von](#value-generated-on-add-or-update) finden Sie weitere Details.
