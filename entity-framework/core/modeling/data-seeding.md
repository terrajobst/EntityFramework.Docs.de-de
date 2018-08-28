---
title: Seeding von Daten – EF Core
author: AndriySvyryd
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 48ba2389de4b57dbe4c2b2124911c71440d45556
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994478"
---
# <a name="data-seeding"></a>Datenseeding

> [!NOTE]  
> Dieses Feature ist neu in EF Core 2.1.

Seeding von Daten ermöglicht, zu der ursprünglichen Daten zum Auffüllen einer Datenbank. Im Gegensatz zu ist in EF6 in EF Core seeding von Daten mit einem Entitätstyp als Teil der Modellkonfiguration verknüpft. Klicken Sie dann EF Core [Migrationen](xref:core/managing-schemas/migrations/index) können automatisch berechnen, was einfügen, aktualisieren oder Löschen beim Aktualisieren der Datenbank auf eine neue Version des Modells angewendet werden müssen.

Beispielsweise können diese so konfigurieren Sie die Seed-Daten für eine `Blog` in `OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Um Entitäten hinzuzufügen, die eine Beziehung die Fremdschlüsselwerte müssen angegeben werden. Häufig sind Fremdschlüsseleigenschaften im Volumeschattenkopie-Zustand, also um eine anonyme Klasse legen Sie die Werte können verwendet werden soll:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

Wenn Entitäten hinzugefügt wurden, es wird empfohlen, verwenden Sie [Migrationen](xref:core/managing-schemas/migrations/index) um Änderungen zu übernehmen. 

Alternativ können Sie `context.Database.EnsureCreated()` zum Erstellen einer neuen Datenbank, die die Seed-Daten, z. B. für eine Testdatenbank oder bei Verwendung von in-Memory-Anbieter enthält. Beachten Sie, dass bei die Datenbank bereits vorhanden ist, `EnsureCreated()` weder aktualisiert das Schema und die Seed-Daten in der Datenbank.
