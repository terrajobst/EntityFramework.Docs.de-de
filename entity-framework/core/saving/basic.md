---
title: Grundlegende Save - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: e99d755b8f7c42b15a73cfdb6a2f8999a405a739
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="basic-save"></a>Grundlegende speichern

Informationen Sie zum Hinzufügen, ändern und Entfernen von Daten mit Ihrem Kontext und Entität Klassen.

> [!TIP]  
> Sie können anzeigen, dass dieser Artikel [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) auf GitHub.

## <a name="adding-data"></a>Hinzufügen von Daten

Verwenden der *DbSet.Add* Methode, um neue Instanzen der Entitätsklassen hinzuzufügen. Die Daten werden in der Datenbank eingefügt werden, wenn Sie aufrufen *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

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
