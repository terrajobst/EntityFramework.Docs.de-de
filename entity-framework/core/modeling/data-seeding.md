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
# <a name="data-seeding"></a>Data Seeding

> [!NOTE]  
> Dieses Feature ist neu in EF Core 2.1.

Das Datenseeding ermöglicht, ursprüngliche Daten zum Befüllen einer Datenbank zur Vefügung zu stellen. In EF Core findet das Datenseeding, anders als in EF6, mittels Entitätstypen statt, die Teil der Modellkonfiguration sind. EF Core [Migrationen](xref:core/managing-schemas/migrations/index) bestimmen dann automatisch, welche INSERT-, UPDATE- oder DELETE-Operationen beim Upgrade der Datenbank auf eine neue Version des Datenmodells angewendet werden müssen.

Seedingdaten für einen `Blog` in `OnModelCreating` können beispielsweise so konfiguriert werden:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Um Entitäten hinzuzufügen, die eine Beziehung die Fremdschlüsselwerte müssen angegeben werden. Häufig sind Fremdschlüsseleigenschaften im Volumeschattenkopie-Zustand, also um eine anonyme Klasse legen Sie die Werte können verwendet werden soll:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

Sobald Entitäten hinzugefügt wurden, ist es empfehlenswert, [Migrationen](xref:core/managing-schemas/migrations/index) für Änderungen zu verwenden. 

Alternativ können Sie `context.Database.EnsureCreated()` zum Erstellen einer neuen Datenbank, die die Seed-Daten, z. B. für eine Testdatenbank oder bei Verwendung von in-Memory-Anbieter enthält. Beachten Sie, dass bei die Datenbank bereits vorhanden ist, `EnsureCreated()` weder aktualisiert das Schema und die Seed-Daten in der Datenbank.
