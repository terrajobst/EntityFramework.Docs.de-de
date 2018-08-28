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
# <a name="data-seeding"></a><span data-ttu-id="cc1ad-102">Datenseeding</span><span class="sxs-lookup"><span data-stu-id="cc1ad-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="cc1ad-103">Dieses Feature ist neu in EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="cc1ad-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="cc1ad-104">Seeding von Daten ermöglicht, zu der ursprünglichen Daten zum Auffüllen einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="cc1ad-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="cc1ad-105">Im Gegensatz zu ist in EF6 in EF Core seeding von Daten mit einem Entitätstyp als Teil der Modellkonfiguration verknüpft.</span><span class="sxs-lookup"><span data-stu-id="cc1ad-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="cc1ad-106">Klicken Sie dann EF Core [Migrationen](xref:core/managing-schemas/migrations/index) können automatisch berechnen, was einfügen, aktualisieren oder Löschen beim Aktualisieren der Datenbank auf eine neue Version des Modells angewendet werden müssen.</span><span class="sxs-lookup"><span data-stu-id="cc1ad-106">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="cc1ad-107">Beispielsweise können diese so konfigurieren Sie die Seed-Daten für eine `Blog` in `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="cc1ad-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="cc1ad-108">Um Entitäten hinzuzufügen, die eine Beziehung die Fremdschlüsselwerte müssen angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="cc1ad-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="cc1ad-109">Häufig sind Fremdschlüsseleigenschaften im Volumeschattenkopie-Zustand, also um eine anonyme Klasse legen Sie die Werte können verwendet werden soll:</span><span class="sxs-lookup"><span data-stu-id="cc1ad-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="cc1ad-110">Wenn Entitäten hinzugefügt wurden, es wird empfohlen, verwenden Sie [Migrationen](xref:core/managing-schemas/migrations/index) um Änderungen zu übernehmen.</span><span class="sxs-lookup"><span data-stu-id="cc1ad-110">Once entities have been added, it is recommended to use [migrations](xref:core/managing-schemas/migrations/index) to apply changes.</span></span> 

<span data-ttu-id="cc1ad-111">Alternativ können Sie `context.Database.EnsureCreated()` zum Erstellen einer neuen Datenbank, die die Seed-Daten, z. B. für eine Testdatenbank oder bei Verwendung von in-Memory-Anbieter enthält.</span><span class="sxs-lookup"><span data-stu-id="cc1ad-111">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider.</span></span> <span data-ttu-id="cc1ad-112">Beachten Sie, dass bei die Datenbank bereits vorhanden ist, `EnsureCreated()` weder aktualisiert das Schema und die Seed-Daten in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="cc1ad-112">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor the seed data in the database.</span></span>
