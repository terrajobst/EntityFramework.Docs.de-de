---
title: Datenseeding - EF Core
author: AndriySvyryd
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/data-seeding
ms.openlocfilehash: 7028e1923152b27f56721dab75aae8b9c2f5ad75
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2018
ms.locfileid: "34163199"
---
# <a name="data-seeding"></a>Datenseeding

> [!NOTE]  
> Dieses Feature ist neu in EF Core 2.1.

Datenseeding kann anfänglichen Daten zum Auffüllen einer Datenbank bereitstellen. Im Gegensatz zu ist in EF6, in der EF-Core seeding Daten einen Entitätstyp als Teil der Modellkonfiguration zugeordnet. Klicken Sie dann EF Core [Migrationen](xref:core/managing-schemas/migrations/index) können automatisch berechnen, was einfügen, aktualisieren oder löschen Vorgänge müssen beim Aktualisieren der Datenbank auf eine neue Version des Modells angewendet werden.

Beispielsweise können dies so konfigurieren Sie die Startdaten für eine `Blog` in `OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Zum Hinzufügen von Entitäten, die eine Beziehung die foreign Key-Werte aufweisen muss angegeben werden. Häufig sind die Fremdschlüsseleigenschaften Schatten aufweist, damit Sie den Wert festlegen, eine anonyme Klasse können verwendet werden sollten:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

Sobald Entitäten hinzugefügt wurden, wird empfohlen, [Migrationen](xref:core/managing-schemas/migrations/index) , Änderungen zu übernehmen. 

Alternativ können Sie `context.Database.EnsureCreated()` zum Erstellen einer neuen Datenbank mit den Ausgangswert-Daten, z. B. für eine Testdatenbank oder bei Verwendung des in-Memory-Anbieters. Beachten Sie, dass bei die Datenbank bereits vorhanden ist, `EnsureCreated()` weder aktualisiert das Schema noch die Seed-Daten in der Datenbank.
