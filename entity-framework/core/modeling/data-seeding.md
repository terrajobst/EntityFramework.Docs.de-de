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
# <a name="data-seeding"></a><span data-ttu-id="190f5-102">Datenseeding</span><span class="sxs-lookup"><span data-stu-id="190f5-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="190f5-103">Dieses Feature ist neu in EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="190f5-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="190f5-104">Datenseeding kann anfänglichen Daten zum Auffüllen einer Datenbank bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="190f5-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="190f5-105">Im Gegensatz zu ist in EF6, in der EF-Core seeding Daten einen Entitätstyp als Teil der Modellkonfiguration zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="190f5-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="190f5-106">Klicken Sie dann EF Core [Migrationen](xref:core/managing-schemas/migrations/index) können automatisch berechnen, was einfügen, aktualisieren oder löschen Vorgänge müssen beim Aktualisieren der Datenbank auf eine neue Version des Modells angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="190f5-106">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="190f5-107">Beispielsweise können dies so konfigurieren Sie die Startdaten für eine `Blog` in `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="190f5-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="190f5-108">Zum Hinzufügen von Entitäten, die eine Beziehung die foreign Key-Werte aufweisen muss angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="190f5-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="190f5-109">Häufig sind die Fremdschlüsseleigenschaften Schatten aufweist, damit Sie den Wert festlegen, eine anonyme Klasse können verwendet werden sollten:</span><span class="sxs-lookup"><span data-stu-id="190f5-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="190f5-110">Sobald Entitäten hinzugefügt wurden, wird empfohlen, [Migrationen](xref:core/managing-schemas/migrations/index) , Änderungen zu übernehmen.</span><span class="sxs-lookup"><span data-stu-id="190f5-110">Once entities have been added, it is recommended to use [migrations](xref:core/managing-schemas/migrations/index) to apply changes.</span></span> 

<span data-ttu-id="190f5-111">Alternativ können Sie `context.Database.EnsureCreated()` zum Erstellen einer neuen Datenbank mit den Ausgangswert-Daten, z. B. für eine Testdatenbank oder bei Verwendung des in-Memory-Anbieters.</span><span class="sxs-lookup"><span data-stu-id="190f5-111">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider.</span></span> <span data-ttu-id="190f5-112">Beachten Sie, dass bei die Datenbank bereits vorhanden ist, `EnsureCreated()` weder aktualisiert das Schema noch die Seed-Daten in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="190f5-112">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor the seed data in the database.</span></span>
