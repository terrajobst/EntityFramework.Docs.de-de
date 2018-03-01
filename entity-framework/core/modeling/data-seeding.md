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
# <a name="data-seeding"></a><span data-ttu-id="ce161-102">Datenseeding</span><span class="sxs-lookup"><span data-stu-id="ce161-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="ce161-103">Dieses Feature ist neu in EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="ce161-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="ce161-104">Datenseeding kann anfänglichen Daten zum Auffüllen einer Datenbank bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="ce161-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="ce161-105">Im Gegensatz zu ist in EF6, in der EF-Core seeding Daten einen Entitätstyp als Teil der Modellkonfiguration zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="ce161-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="ce161-106">EF Core Migrationen können dann automatisch berechnen, was einfügen, aktualisieren oder löschen Vorgänge müssen beim Aktualisieren der Datenbank auf eine neue Version des Modells angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="ce161-106">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="ce161-107">Beispielsweise können dies so konfigurieren Sie die Startdaten für eine `Blog` in `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="ce161-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="ce161-108">Zum Hinzufügen von Entitäten, die eine Beziehung die foreign Key-Werte aufweisen muss angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="ce161-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="ce161-109">Häufig sind die Fremdschlüsseleigenschaften Schatten aufweist, damit Sie den Wert festlegen, eine anonyme Klasse können verwendet werden sollten:</span><span class="sxs-lookup"><span data-stu-id="ce161-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]
