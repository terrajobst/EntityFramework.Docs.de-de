---
title: "Explizite Werte festlegen für die generierten Eigenschaften – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3f1993c2-cdf5-425b-bac2-a2665a20322b
ms.technology: entity-framework-core
uid: core/saving/explicit-values-generated-properties
ms.openlocfilehash: f34e92d9a3b10b6ff904257ccd047a8acdaad231
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="setting-explicit-values-for-generated-properties"></a>Explizite Werte festlegen für die generierten Eigenschaften

Eine generierte Eigenschaft ist eine Eigenschaft, deren Wert generiert (entweder durch EF oder die Datenbank) Wenn die Entität hinzugefügt und/oder aktualisiert. Finden Sie unter [Eigenschaften generiert](../modeling/generated-properties.md) für Weitere Informationen.

Möglicherweise gibt es Situationen, in denen einen expliziten Wert für eine generierte Eigenschaft, anstatt ein generiertes festgelegt werden sollen.

> [!TIP]  
> Sie können anzeigen, dass dieser Artikel [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/ExplicitValuesGenerateProperties/) auf GitHub.

## <a name="the-model"></a>Das Modell

In diesem Artikel verwendete Modell enthält ein einziges `Employee` Entität.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Employee.cs#Sample)]

## <a name="saving-an-explicit-value-during-add"></a>Speichern einen expliziten Wert während hinzufügen

Die `Employee.EmploymentStarted` Eigenschaft ist so konfiguriert, dass die Werte, die von der Datenbank für die neue Entitäten (mit einem Standardwert) generiert haben.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#EmploymentStarted)]

Der folgende Code fügt zwei Mitarbeiter in der Datenbank.
* Für die erste Seite kein Wert zugewiesen wurde, um `Employee.EmploymentStarted` -Eigenschaft, sodass es weiterhin ist festgelegt auf den CLR-Standardwert für `DateTime`.
* Für die zweite haben wir einen expliziten Wert von setzen `1-Jan-2000`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmploymentStarted)]

Ausgabe zeigt, dass die Datenbank einen Wert für den ersten Mitarbeiter generiert und unsere explizite Wert für die zweite verwendet wurde.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/1/2000 12:00:00 AM
```

### <a name="explicit-values-into-sql-server-identity-columns"></a>Explizite Werte in Identitätsspalten für SQL Server

Gemäß der Konvention die `Employee.EmployeeId` Eigenschaft ist ein speichergenerierte `IDENTITY` Spalte.

In den meisten Fällen funktioniert die oben gezeigte Ansatz für Schlüsseleigenschaften. Allerdings zum Einfügen von expliziter Werten in einer SQL Server `IDENTITY` Spalte müssen Sie manuell aktivieren `IDENTITY_INSERT` vor dem Aufruf `SaveChanges()`.

> [!NOTE]  
> Wir haben eine [Funktionsanfrage](https://github.com/aspnet/EntityFramework/issues/703) auf unseren nachholbedarf automatisch in SQL Server-Anbieter dazu.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmployeeId)]

Ausgabe zeigt an, dass die angegebenen Ids in der Datenbank gespeichert wurden.

``` Console
100: John Doe
101: Jane Doe
```

## <a name="setting-an-explicit-value-during-update"></a>Einen expliziten Wert festlegen, während der Aktualisierung

Die `Employee.LastPayRaise` Eigenschaft ist so konfiguriert, dass die Werte, die von der Datenbank während eines Updates generiert haben.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#LastPayRaise)]

> [!NOTE]  
> Standardmäßig wird EF Core eine Ausnahme ausgelöst, wenn Sie versuchen, einen expliziten Wert für eine Eigenschaft zu speichern, die so konfiguriert ist, dass während des Updates generiert werden. Um dies zu vermeiden, müssen Sie Dropdown-Liste auf der unteren Ebene Metadaten-API und Festlegen der `AfterSaveBehavior` (siehe oben).

> [!NOTE]  
> **Änderungen in EF Core 2.0:** In früheren Versionen wurde das Verhalten After speichern gesteuert, über die `IsReadOnlyAfterSave` Flag. Dieses Flag veraltet und durch ersetzt wurde `AfterSaveBehavior`.

Es gibt auch ein Trigger in der Datenbank zum Generieren von Werten für die `LastPayRaise` Spalte während der `UPDATE` Vorgänge.

[!code-sql[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/employee_UPDATE.sql)]

Im folgende Code wird das Gehalt von zwei Mitarbeiter in der Datenbank erhöht.
* Für die erste Seite kein Wert zugewiesen wurde, um `Employee.LastPayRaise` -Eigenschaft, sodass es weiterhin festgelegt ist auf Null.
* Für die zweite haben wir einen expliziten Wert für eine Woche vor (zurück, bis des Lohns lösen) festgelegt.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#LastPayRaise)]

Ausgabe zeigt, dass die Datenbank einen Wert für den ersten Mitarbeiter generiert und unsere explizite Wert für die zweite verwendet wurde.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/19/2017 12:00:00 AM
```
