---
title: Grundlegende Save - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: deead323301dc4a0ee0748b4536ddff4596b99e6
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="basic-save"></a>Grundlegende speichern

Informationen Sie zum Hinzufügen, ändern und Entfernen von Daten mit Ihrem Kontext und Entität Klassen.

> [!TIP]  
> Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) finden Sie auf GitHub.

## <a name="adding-data"></a>Hinzufügen von Daten

Verwenden der *DbSet.Add* Methode, um neue Instanzen der Entitätsklassen hinzuzufügen. Die Daten werden in der Datenbank eingefügt werden, wenn Sie aufrufen *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> Die Add-, Anfügen und Update-Methoden, die die gesamte auf das vollständige Diagramm von Entitäten Arbeit an sie übergeben, wie beschrieben in der [verknüpften Daten](related-data.md) Abschnitt. Alternativ kann die EntityEntry.State-Eigenschaft zum Festlegen des Status von nur einer einzigen Entität verwendet werden. Beispielsweise `context.Entry(blog).State = EntityState.Modified`.

## <a name="updating-data"></a>Aktualisieren von Daten

EF erkennt automatisch Änderungen an einer vorhandenen Entität, die vom Kontext nachverfolgt wird. Dies schließt Entitäten, die Sie laden/aus der Datenbank Abfragen und Entitäten, die zuvor hinzugefügt und in der Datenbank gespeichert wurden.

Ändern Sie die Werte für Eigenschaften ein, und rufen dann *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a>Löschen von Daten

Verwenden der *DbSet.Remove* Methode, um Instanzen der Entitätsklassen zu löschen.

Wenn die Entität bereits in der Datenbank vorhanden ist, wird sie während der gelöscht werden *SaveChanges*. Wenn die Entität nicht noch in der Datenbank gespeichert wurde (d. h. es wird nachverfolgt, wie hinzugefügt) und wird aus dem Kontext entfernt und nicht mehr eingefügt, wenn *SaveChanges* aufgerufen wird.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a>Mehrere Vorgänge in einer einzelnen SaveChanges

Sie können mehrere Hinzufügen/Aktualisieren/Entfernen-Vorgänge in einem einzigen Aufruf kombinieren *SaveChanges*.

> [!NOTE]  
> Für die meisten Datenbankanbieter *SaveChanges* ist transaktional. Dies bedeutet, dass alle Vorgänge, die entweder erfolgreich oder nicht und die Vorgänge werden nie Links teilweise angewendet.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
