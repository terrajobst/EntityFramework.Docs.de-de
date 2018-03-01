---
title: Datenseeding - EF Core
author: AndriySvyryd
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/data-seeding
ms.openlocfilehash: 693ffe44e247a79e01ac7c98a36472bf2c68d37f
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="data-seeding"></a>Datenseeding

> [!NOTE]  
> Dieses Feature ist neu in EF Core 2.1.

Datenseeding kann anfänglichen Daten zum Auffüllen einer Datenbank bereitstellen. Im Gegensatz zu ist in EF6, in der EF-Core seeding Daten einen Entitätstyp als Teil der Modellkonfiguration zugeordnet. EF Core Migrationen können dann automatisch berechnen, was einfügen, aktualisieren oder löschen Vorgänge müssen beim Aktualisieren der Datenbank auf eine neue Version des Modells angewendet werden.

Beispielsweise können dies so konfigurieren Sie die Startdaten für eine `Blog` in `OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Zum Hinzufügen von Entitäten, die eine Beziehung die foreign Key-Werte aufweisen muss angegeben werden. Häufig sind die Fremdschlüsseleigenschaften Schatten aufweist, damit Sie den Wert festlegen, eine anonyme Klasse können verwendet werden sollten:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]
