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
# <a name="data-seeding"></a><span data-ttu-id="be9ed-102">Data Seeding</span><span class="sxs-lookup"><span data-stu-id="be9ed-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="be9ed-103">Dieses Feature ist neu in EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="be9ed-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="be9ed-104">Das Datenseeding ermöglicht, ursprüngliche Daten zum Befüllen einer Datenbank zur Vefügung zu stellen.</span><span class="sxs-lookup"><span data-stu-id="be9ed-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="be9ed-105">In EF Core findet das Datenseeding, anders als in EF6, mittels Entitätstypen statt, die Teil der Modellkonfiguration sind.</span><span class="sxs-lookup"><span data-stu-id="be9ed-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="be9ed-106">EF Core [Migrationen](xref:core/managing-schemas/migrations/index) bestimmen dann automatisch, welche INSERT-, UPDATE- oder DELETE-Operationen beim Upgrade der Datenbank auf eine neue Version des Datenmodells angewendet werden müssen.</span><span class="sxs-lookup"><span data-stu-id="be9ed-106">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="be9ed-107">Seedingdaten für einen `Blog` in `OnModelCreating` können beispielsweise so konfiguriert werden:</span><span class="sxs-lookup"><span data-stu-id="be9ed-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="be9ed-108">Um Entitäten hinzuzufügen, die eine Beziehung die Fremdschlüsselwerte müssen angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="be9ed-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="be9ed-109">Häufig sind Fremdschlüsseleigenschaften im Volumeschattenkopie-Zustand, also um eine anonyme Klasse legen Sie die Werte können verwendet werden soll:</span><span class="sxs-lookup"><span data-stu-id="be9ed-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="be9ed-110">Sobald Entitäten hinzugefügt wurden, ist es empfehlenswert, [Migrationen](xref:core/managing-schemas/migrations/index) für Änderungen zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="be9ed-110">Once entities have been added, it is recommended to use [migrations](xref:core/managing-schemas/migrations/index) to apply changes.</span></span> 

<span data-ttu-id="be9ed-111">Alternativ können Sie `context.Database.EnsureCreated()` zum Erstellen einer neuen Datenbank, die die Seed-Daten, z. B. für eine Testdatenbank oder bei Verwendung von in-Memory-Anbieter enthält.</span><span class="sxs-lookup"><span data-stu-id="be9ed-111">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider.</span></span> <span data-ttu-id="be9ed-112">Beachten Sie, dass bei die Datenbank bereits vorhanden ist, `EnsureCreated()` weder aktualisiert das Schema und die Seed-Daten in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="be9ed-112">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor the seed data in the database.</span></span>
