---
title: "SQLite-Datenbank-Anbieter - Einschränkungen - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
ms.technology: entity-framework-core
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 08a4b8c26a3678491d412b333a7415cb45d4231f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="sqlite-ef-core-database-provider-limitations"></a><span data-ttu-id="37710-102">SQLite EF Core Datenbank Anbietereinschränkungen</span><span class="sxs-lookup"><span data-stu-id="37710-102">SQLite EF Core Database Provider Limitations</span></span>

<span data-ttu-id="37710-103">Der SQLite-Anbieter hat einige Migrationen Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="37710-103">The SQLite provider has a number of migrations limitations.</span></span> <span data-ttu-id="37710-104">Die meisten dieser Einschränkungen sind Beschränkungen in der zugrunde liegende Datenbankmodul SQLite zurückzuführen und sind nicht spezifisch für EF.</span><span class="sxs-lookup"><span data-stu-id="37710-104">Most of these limitations are a result of limitations in the underlying SQLite database engine and are not specific to EF.</span></span>

## <a name="modeling-limitations"></a><span data-ttu-id="37710-105">Modellieren von Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="37710-105">Modeling limitations</span></span>

<span data-ttu-id="37710-106">Die allgemeine relationale Bibliothek (shared von Entity Framework, relationale Datenbank-Anbieter) definiert APIs für die Modellierung Konzepte, die für die meisten relationale Datenbankmodule gelten.</span><span class="sxs-lookup"><span data-stu-id="37710-106">The common relational library (shared by Entity Framework relational database providers) defines APIs for modelling concepts that are common to most relational database engines.</span></span> <span data-ttu-id="37710-107">Eine Reihe von dieser Konzepte werden von der SQLite-Anbieter nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="37710-107">A couple of these concepts are not supported by the SQLite provider.</span></span>

* <span data-ttu-id="37710-108">Schemata</span><span class="sxs-lookup"><span data-stu-id="37710-108">Schemas</span></span>
* <span data-ttu-id="37710-109">Sequenzen</span><span class="sxs-lookup"><span data-stu-id="37710-109">Sequences</span></span>

## <a name="migrations-limitations"></a><span data-ttu-id="37710-110">Migrationen von Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="37710-110">Migrations limitations</span></span>

<span data-ttu-id="37710-111">Die SQLite-Datenbank-Engine unterstützt nicht mehrere Schemavorgänge, die von der Mehrheit der andere relationalen Datenbanken unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="37710-111">The SQLite database engine does not support a number of schema operations that are supported by the majority of other relational databases.</span></span> <span data-ttu-id="37710-112">Wenn Sie versuchen, einen nicht unterstützten Vorgänge anwenden auf eine SQLite-Datenbank wird eine `NotSupportedException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="37710-112">If you attempt to apply one of the unsupported operations to a SQLite database then a `NotSupportedException` will be thrown.</span></span>

| <span data-ttu-id="37710-113">Vorgang</span><span class="sxs-lookup"><span data-stu-id="37710-113">Operation</span></span>            | <span data-ttu-id="37710-114">Unterstützt?</span><span class="sxs-lookup"><span data-stu-id="37710-114">Supported?</span></span> |
| -------------------- | ---------- |
| <span data-ttu-id="37710-115">AddColumn</span><span class="sxs-lookup"><span data-stu-id="37710-115">AddColumn</span></span>            | <span data-ttu-id="37710-116">✔</span><span class="sxs-lookup"><span data-stu-id="37710-116">✔</span></span>          |
| <span data-ttu-id="37710-117">AddForeignKey</span><span class="sxs-lookup"><span data-stu-id="37710-117">AddForeignKey</span></span>        | <span data-ttu-id="37710-118">✗</span><span class="sxs-lookup"><span data-stu-id="37710-118">✗</span></span>          |
| <span data-ttu-id="37710-119">AddPrimaryKey</span><span class="sxs-lookup"><span data-stu-id="37710-119">AddPrimaryKey</span></span>        | <span data-ttu-id="37710-120">✗</span><span class="sxs-lookup"><span data-stu-id="37710-120">✗</span></span>          |
| <span data-ttu-id="37710-121">AddUniqueConstraint</span><span class="sxs-lookup"><span data-stu-id="37710-121">AddUniqueConstraint</span></span>  | <span data-ttu-id="37710-122">✗</span><span class="sxs-lookup"><span data-stu-id="37710-122">✗</span></span>          |
| <span data-ttu-id="37710-123">AlterColumn</span><span class="sxs-lookup"><span data-stu-id="37710-123">AlterColumn</span></span>          | <span data-ttu-id="37710-124">✗</span><span class="sxs-lookup"><span data-stu-id="37710-124">✗</span></span>          |
| <span data-ttu-id="37710-125">CreateIndex</span><span class="sxs-lookup"><span data-stu-id="37710-125">CreateIndex</span></span>          | <span data-ttu-id="37710-126">✔</span><span class="sxs-lookup"><span data-stu-id="37710-126">✔</span></span>          |
| <span data-ttu-id="37710-127">CreateTable</span><span class="sxs-lookup"><span data-stu-id="37710-127">CreateTable</span></span>          | <span data-ttu-id="37710-128">✔</span><span class="sxs-lookup"><span data-stu-id="37710-128">✔</span></span>          |
| <span data-ttu-id="37710-129">DropColumn</span><span class="sxs-lookup"><span data-stu-id="37710-129">DropColumn</span></span>           | <span data-ttu-id="37710-130">✗</span><span class="sxs-lookup"><span data-stu-id="37710-130">✗</span></span>          |
| <span data-ttu-id="37710-131">DropForeignKey</span><span class="sxs-lookup"><span data-stu-id="37710-131">DropForeignKey</span></span>       | <span data-ttu-id="37710-132">✗</span><span class="sxs-lookup"><span data-stu-id="37710-132">✗</span></span>          |
| <span data-ttu-id="37710-133">DropIndex</span><span class="sxs-lookup"><span data-stu-id="37710-133">DropIndex</span></span>            | <span data-ttu-id="37710-134">✔</span><span class="sxs-lookup"><span data-stu-id="37710-134">✔</span></span>          |
| <span data-ttu-id="37710-135">DropPrimaryKey</span><span class="sxs-lookup"><span data-stu-id="37710-135">DropPrimaryKey</span></span>       | <span data-ttu-id="37710-136">✗</span><span class="sxs-lookup"><span data-stu-id="37710-136">✗</span></span>          |
| <span data-ttu-id="37710-137">DropTable</span><span class="sxs-lookup"><span data-stu-id="37710-137">DropTable</span></span>            | <span data-ttu-id="37710-138">✔</span><span class="sxs-lookup"><span data-stu-id="37710-138">✔</span></span>          |
| <span data-ttu-id="37710-139">DropUniqueConstraint</span><span class="sxs-lookup"><span data-stu-id="37710-139">DropUniqueConstraint</span></span> | <span data-ttu-id="37710-140">✗</span><span class="sxs-lookup"><span data-stu-id="37710-140">✗</span></span>          |
| <span data-ttu-id="37710-141">"RenameColumn"</span><span class="sxs-lookup"><span data-stu-id="37710-141">RenameColumn</span></span>         | <span data-ttu-id="37710-142">✗</span><span class="sxs-lookup"><span data-stu-id="37710-142">✗</span></span>          |
| <span data-ttu-id="37710-143">RenameIndex</span><span class="sxs-lookup"><span data-stu-id="37710-143">RenameIndex</span></span>          | <span data-ttu-id="37710-144">✗</span><span class="sxs-lookup"><span data-stu-id="37710-144">✗</span></span>          |
| <span data-ttu-id="37710-145">RenameTable</span><span class="sxs-lookup"><span data-stu-id="37710-145">RenameTable</span></span>          | <span data-ttu-id="37710-146">✔</span><span class="sxs-lookup"><span data-stu-id="37710-146">✔</span></span>          |

## <a name="migrations-limitations-workaround"></a><span data-ttu-id="37710-147">Migrationen Einschränkungen umgehen</span><span class="sxs-lookup"><span data-stu-id="37710-147">Migrations limitations workaround</span></span>

<span data-ttu-id="37710-148">Sie können einige umgehen dieser Einschränkungen durch Schreiben von Code manuell in Ihrer Migrationen, führen Sie eine Tabelle neu erstellen.</span><span class="sxs-lookup"><span data-stu-id="37710-148">You can workaround some of these limitations by manually writing code in your migrations to perform a table rebuild.</span></span> <span data-ttu-id="37710-149">Eine Neuerstellung der Tabelle umfasst das Umbenennen der vorhandenen Tabelle, eine neue Tabelle erstellen, Kopieren von Daten in die neue Tabelle und löschen die alte Tabelle.</span><span class="sxs-lookup"><span data-stu-id="37710-149">A table rebuild involves renaming the existing table, creating a new table, copying data to the new table, and dropping the old table.</span></span> <span data-ttu-id="37710-150">Sie benötigen, verwenden Sie die `Sql(string)` Methode, um einige der folgenden Schritte ausführen.</span><span class="sxs-lookup"><span data-stu-id="37710-150">You will need to use the `Sql(string)` method to perform some of these steps.</span></span>

<span data-ttu-id="37710-151">Finden Sie unter [machen andere Arten von Tabelle Schemaänderungen](http://sqlite.org/lang_altertable.html#otheralter) in der Dokumentation SQLite Weitere Details.</span><span class="sxs-lookup"><span data-stu-id="37710-151">See [Making Other Kinds Of Table Schema Changes](http://sqlite.org/lang_altertable.html#otheralter) in the SQLite documentation for more details.</span></span>

<span data-ttu-id="37710-152">In der Zukunft EF einige dieser Vorgänge unterstützen möglicherweise mit der Tabelle Rebuild Ansatz im Hintergrund.</span><span class="sxs-lookup"><span data-stu-id="37710-152">In the future, EF may support some of these operations by using the table rebuild approach under the covers.</span></span> <span data-ttu-id="37710-153">Sie können [verfolgen Sie diese Funktion auf unserer GitHub-Projekt](https://github.com/aspnet/EntityFramework/issues/329).</span><span class="sxs-lookup"><span data-stu-id="37710-153">You can [track this feature on our GitHub project](https://github.com/aspnet/EntityFramework/issues/329).</span></span>
