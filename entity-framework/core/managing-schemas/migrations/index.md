---
title: Migrationen – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: dd164125c053497af94773011127853ad10d27a6
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754508"
---
<a name="migrations"></a><span data-ttu-id="9f419-102">Migrationen</span><span class="sxs-lookup"><span data-stu-id="9f419-102">Migrations</span></span>
==========
<span data-ttu-id="9f419-103">Migrationen stellen eine Möglichkeit dar, Schemaänderungen schrittweise auf die Datenbank anzuwenden, um diese mit Ihrem EF Core-Modell zu synchronisieren und gleichzeitig vorhandene Daten in der Datenbank zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="9f419-103">Migrations provide a way to incrementally apply schema changes to the database to keep it in sync with your EF Core model while preserving existing data in the database.</span></span>

<a name="creating-the-database"></a><span data-ttu-id="9f419-104">Erstellen der Datenbank</span><span class="sxs-lookup"><span data-stu-id="9f419-104">Creating the database</span></span>
---------------------
<span data-ttu-id="9f419-105">Nachdem Sie [Ihr Ausgangsmodell definiert][1] haben, können Sie nun die Datenbank erstellen.</span><span class="sxs-lookup"><span data-stu-id="9f419-105">After you've [defined your initial model][1], it's time to create the database.</span></span> <span data-ttu-id="9f419-106">Fügen Sie hierfür eine erste Migration hinzu.</span><span class="sxs-lookup"><span data-stu-id="9f419-106">To do this, add an initial migration.</span></span>
<span data-ttu-id="9f419-107">Installieren Sie [EF Core Tools][2], und führen Sie den entsprechenden Befehl aus.</span><span class="sxs-lookup"><span data-stu-id="9f419-107">Install the [EF Core Tools][2] and run the appropriate command.</span></span>

``` powershell
Add-Migration InitialCreate
```
``` Console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="9f419-108">Drei Dateien werden Ihrem Projekt im Verzeichnis **Migrationen** hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="9f419-108">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="9f419-109">**00000000000000_InitialCreate.cs** – Die wichtigste Migrationsdatei.</span><span class="sxs-lookup"><span data-stu-id="9f419-109">**00000000000000_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="9f419-110">Enthält die Vorgänge, die zum Durchführen der Migration (in `Up()`) und Rückgängigmachen (in `Down()`) erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="9f419-110">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="9f419-111">**00000000000000_InitialCreate.Designer.cs** – Die Metadatendatei für die Migration.</span><span class="sxs-lookup"><span data-stu-id="9f419-111">**00000000000000_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="9f419-112">Enthält Informationen, die vom EF verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="9f419-112">Contains information used by EF.</span></span>
* <span data-ttu-id="9f419-113">**MyContextModelSnapshot.cs** – Eine Momentaufnahme des aktuellen Modells.</span><span class="sxs-lookup"><span data-stu-id="9f419-113">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="9f419-114">Diese wird verwendet, um festzustellen, was sich beim Hinzufügen der nächsten Migration geändert hat.</span><span class="sxs-lookup"><span data-stu-id="9f419-114">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="9f419-115">Mit dem Zeitstempel im Dateinamen können Migrationen chronologisch angeordnet werden, sodass Sie den Fortschritt der Änderungen mitverfolgen können.</span><span class="sxs-lookup"><span data-stu-id="9f419-115">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="9f419-116">Es steht Ihnen frei, Migrationsdateien zu verschieben und ihren Namespace zu ändern.</span><span class="sxs-lookup"><span data-stu-id="9f419-116">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="9f419-117">Neue Migrationen werden als gleichgeordnete Elemente der letzten Migration erstellt.</span><span class="sxs-lookup"><span data-stu-id="9f419-117">New migrations are created as siblings of the last migration.</span></span>

<span data-ttu-id="9f419-118">Als Nächstes führen Sie für die Datenbank die Migration durch, um das Schema zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="9f419-118">Next, apply the migration to the database to create the schema.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="adding-another-migration"></a><span data-ttu-id="9f419-119">Hinzufügen einer anderen Migration</span><span class="sxs-lookup"><span data-stu-id="9f419-119">Adding another migration</span></span>
------------------------
<span data-ttu-id="9f419-120">Nachdem Sie Änderungen an Ihrem EF Core-Modell vorgenommen haben, ist das Datenbankschema nicht mehr synchron. Fügen Sie eine andere Migration hinzu, um es auf dem neuesten Stand zu bringen.</span><span class="sxs-lookup"><span data-stu-id="9f419-120">After making changes to your EF Core model, the database schema will be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="9f419-121">Der Migrationsname kann wie eine Commitnachricht in einem Versionskontrollsystem verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="9f419-121">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="9f419-122">Wenn zum Beispiel Änderungen vorgenommen wurden, um Kundenbewertungen von Produkten zu speichern, wäre z.B. *AddProductReviews* auswählbar.</span><span class="sxs-lookup"><span data-stu-id="9f419-122">For example, if I made changes to save customer reviews of products, I might choose something like *AddProductReviews*.</span></span>

``` powershell
Add-Migration AddProductReviews
```
``` Console
dotnet ef migrations add AddProductReviews
```

<span data-ttu-id="9f419-123">Sobald das Gerüst für die Migration erstellt wurde, sollten Sie sie auf ihre Genauigkeit überprüfen und alle zusätzlichen Vorgänge hinzufügen, die für die ordnungsgemäße Durchführung erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="9f419-123">Once the migration is scaffolded, you should review it for accuracy and add any additional operations required to apply it correctly.</span></span> <span data-ttu-id="9f419-124">Beispielsweise könnte Ihre Migration die folgenden Vorgänge umfassen:</span><span class="sxs-lookup"><span data-stu-id="9f419-124">For example, your migration might contain the following operations:</span></span>

``` csharp
migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");

migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);
```

<span data-ttu-id="9f419-125">Durch diese Vorgänge wird zwar Kompatibilität mit dem Datenbankschema hergestellt, die bestehenden Kundennamen werden jedoch nicht gespeichert.</span><span class="sxs-lookup"><span data-stu-id="9f419-125">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="9f419-126">Um dies zu korrigieren, schreiben Sie es wie folgt neu:</span><span class="sxs-lookup"><span data-stu-id="9f419-126">To make it better, rewrite it as follows.</span></span>

``` csharp
migrationBuilder.AddColumn<string>(
    name: "Name",
    table: "Customer",
    nullable: true);

migrationBuilder.Sql(
@"
    UPDATE Customer
    SET Name = FirstName + ' ' + LastName;
");

migrationBuilder.DropColumn(
    name: "FirstName",
    table: "Customer");

migrationBuilder.DropColumn(
    name: "LastName",
    table: "Customer");
```

> [!TIP]
> <span data-ttu-id="9f419-127">Beim Hinzufügen einer neuen Migration wird eine Warnung ausgegeben, wenn ein Gerüst für einen Vorgang erstellt wurde, der zu Datenverlusten führen kann (z.B. beim Löschen einer Spalte).</span><span class="sxs-lookup"><span data-stu-id="9f419-127">Adding a new migration warns when an operation is scaffolded that may result in data loss (like dropping a column).</span></span> <span data-ttu-id="9f419-128">Achten Sie besonders darauf, diese Migrationen auf ihre Genauigkeit hin zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="9f419-128">Be sure to especially review these migrations for accuracy.</span></span>

<span data-ttu-id="9f419-129">Führen Sie für die Datenbank die Migration mit dem entsprechenden Befehl durch.</span><span class="sxs-lookup"><span data-stu-id="9f419-129">Apply the migration to the database using the appropriate command.</span></span>

``` powershell
Update-Database
```
``` Console
dotnet ef database update
```

<a name="removing-a-migration"></a><span data-ttu-id="9f419-130">Entfernen einer Migration</span><span class="sxs-lookup"><span data-stu-id="9f419-130">Removing a migration</span></span>
--------------------
<span data-ttu-id="9f419-131">Es kann vorkommen, dass Sie eine Migration hinzufügen und feststellen, dass Sie zusätzliche Änderungen an Ihrem EF Core-Modell vornehmen müssen, bevor Sie diese durchführen.</span><span class="sxs-lookup"><span data-stu-id="9f419-131">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span>
<span data-ttu-id="9f419-132">Um die letzte Migration zu entfernen, verwenden Sie diesen Befehl.</span><span class="sxs-lookup"><span data-stu-id="9f419-132">To remove the last migration, use this command.</span></span>

``` powershell
Remove-Migration
```
``` Console
dotnet ef migrations remove
```

<span data-ttu-id="9f419-133">Nachdem Sie diese entfernt haben, können Sie die zusätzlichen Modelländerungen vornehmen und es erneut hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="9f419-133">After removing it, you can make the additional model changes and add it again.</span></span>

<a name="reverting-a-migration"></a><span data-ttu-id="9f419-134">Rückgängigmachen einer Migration</span><span class="sxs-lookup"><span data-stu-id="9f419-134">Reverting a migration</span></span>
---------------------
<span data-ttu-id="9f419-135">Wenn Sie bereits eine Migration (oder mehrere Migrationen) für die Datenbank durchgeführt haben, diese jedoch rückgängig machen müssen, können Sie mit demselben Befehl Migrationen durchführen, aber den Namen der Migration angeben, für die Sie ein Rollback ausführen möchten.</span><span class="sxs-lookup"><span data-stu-id="9f419-135">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

``` powershell
Update-Database LastGoodMigration
```
``` Console
dotnet ef database update LastGoodMigration
```

<a name="empty-migrations"></a><span data-ttu-id="9f419-136">Leere Migrationen</span><span class="sxs-lookup"><span data-stu-id="9f419-136">Empty migrations</span></span>
----------------
<span data-ttu-id="9f419-137">Manchmal ist es sinnvoll, eine Migration hinzuzufügen, ohne Modelländerungen vorzunehmen.</span><span class="sxs-lookup"><span data-stu-id="9f419-137">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="9f419-138">In diesem Fall wird durch das Hinzufügen einer neuen Migration eine leere Migration erstellt.</span><span class="sxs-lookup"><span data-stu-id="9f419-138">In this case, adding a new migration creates an empty one.</span></span> <span data-ttu-id="9f419-139">Sie können diese Migration anpassen, um Vorgänge durchzuführen, die sich nicht direkt auf das EF Core-Modell beziehen.</span><span class="sxs-lookup"><span data-stu-id="9f419-139">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span>
<span data-ttu-id="9f419-140">Einige Dinge, die Sie vielleicht auf diese Weise handhaben wollen, sind Folgende:</span><span class="sxs-lookup"><span data-stu-id="9f419-140">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="9f419-141">Volltextsuche</span><span class="sxs-lookup"><span data-stu-id="9f419-141">Full-Text Search</span></span>
* <span data-ttu-id="9f419-142">Funktionen</span><span class="sxs-lookup"><span data-stu-id="9f419-142">Functions</span></span>
* <span data-ttu-id="9f419-143">Gespeicherte Prozeduren</span><span class="sxs-lookup"><span data-stu-id="9f419-143">Stored procedures</span></span>
* <span data-ttu-id="9f419-144">Trigger</span><span class="sxs-lookup"><span data-stu-id="9f419-144">Triggers</span></span>
* <span data-ttu-id="9f419-145">Ansichten</span><span class="sxs-lookup"><span data-stu-id="9f419-145">Views</span></span>
* <span data-ttu-id="9f419-146">usw.</span><span class="sxs-lookup"><span data-stu-id="9f419-146">etc.</span></span>

<a name="generating-a-sql-script"></a><span data-ttu-id="9f419-147">Generieren eines SQL-Skripts</span><span class="sxs-lookup"><span data-stu-id="9f419-147">Generating a SQL script</span></span>
-----------------------
<span data-ttu-id="9f419-148">Wenn Sie Ihre Migrationen debuggen oder in einer Produktionsdatenbank bereitstellen, ist es sinnvoll, ein SQL-Skript zu generieren.</span><span class="sxs-lookup"><span data-stu-id="9f419-148">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="9f419-149">Das Skript kann dann weiter auf Genauigkeit überprüft und auf die Anforderungen einer Produktionsdatenbank abgestimmt werden.</span><span class="sxs-lookup"><span data-stu-id="9f419-149">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="9f419-150">Das Skript kann auch in Verbindung mit einer Bereitstellungstechnologie verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="9f419-150">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="9f419-151">Der grundlegende Befehl lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="9f419-151">The basic command is as follows.</span></span>

``` powershell
Script-Migration
```
``` Console
dotnet ef migrations script
```

<span data-ttu-id="9f419-152">Es gibt mehrere Optionen für diesen Befehl.</span><span class="sxs-lookup"><span data-stu-id="9f419-152">There are several options to this command.</span></span>

<span data-ttu-id="9f419-153">Die **from**-Migration sollte die letzte Migration sein, die vor der Skriptausführung für die Datenbank durchgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="9f419-153">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="9f419-154">Wenn keine Migrationen durchgeführt wurden, geben Sie `0` an (dies ist die Standardeinstellung).</span><span class="sxs-lookup"><span data-stu-id="9f419-154">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="9f419-155">Die **to**-Migration ist die letzte Migration, die nach der Skriptausführung für die Datenbank durchgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="9f419-155">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="9f419-156">Dies ist standardmäßig die letzte Migration in Ihrem Projekt.</span><span class="sxs-lookup"><span data-stu-id="9f419-156">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="9f419-157">Optional kann ein **idempotent**-Skript generiert werden.</span><span class="sxs-lookup"><span data-stu-id="9f419-157">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="9f419-158">Dieses Skript wendet nur Migrationen an, wenn sie nicht bereits für die Datenbank durchgeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="9f419-158">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="9f419-159">Dies ist hilfreich, wenn Sie nicht genau wissen, welche Migration für die Datenbank zuletzt durchgeführt wurde oder wenn Bereitstellungen in mehreren Datenbanken, die sich jeweils in unterschiedlichen Migrationen befinden, erfolgen.</span><span class="sxs-lookup"><span data-stu-id="9f419-159">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

<a name="applying-migrations-at-runtime"></a><span data-ttu-id="9f419-160">Durchführen von Migrationen zur Runtime</span><span class="sxs-lookup"><span data-stu-id="9f419-160">Applying migrations at runtime</span></span>
------------------------------
<span data-ttu-id="9f419-161">Einige Apps sollten beim Start oder bei der ersten Ausführung Migrationen zur Runtime durchführen.</span><span class="sxs-lookup"><span data-stu-id="9f419-161">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="9f419-162">Dies ist mit der Methode `Migrate()` möglich.</span><span class="sxs-lookup"><span data-stu-id="9f419-162">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="9f419-163">Achten Sie darauf, dass diese Vorgehensweise nicht für alle geeignet ist.</span><span class="sxs-lookup"><span data-stu-id="9f419-163">Caution, this approach isn't for everyone.</span></span> <span data-ttu-id="9f419-164">Diese eignet sich zwar ideal für Apps mit einer lokalen Datenbank, doch die meisten Anwendungen erfordern zuverlässigere Bereitstellungsstrategien wie etwa durch das Generieren von SQL-Skripts.</span><span class="sxs-lookup"><span data-stu-id="9f419-164">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
> <span data-ttu-id="9f419-165">Rufen Sie `EnsureCreated()` nicht vor `Migrate()` auf.</span><span class="sxs-lookup"><span data-stu-id="9f419-165">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="9f419-166">`EnsureCreated()` umgeht Migrationen zum Erstellen von Schemas, was dazu führt, dass `Migrate()` fehlerhaft ist.</span><span class="sxs-lookup"><span data-stu-id="9f419-166">`EnsureCreated()` bypasses Migrations to create the schema, which causes `Migrate()` to fail.</span></span>

> [!NOTE]
> <span data-ttu-id="9f419-167">Diese Methode basiert auf dem `IMigrator`-Dienst, der für erweiterte Szenarien verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="9f419-167">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="9f419-168">Verwenden Sie `DbContext.GetService<IMigrator>()`, um darauf zugreifen.</span><span class="sxs-lookup"><span data-stu-id="9f419-168">Use `DbContext.GetService<IMigrator>()` to access it.</span></span>


  [1]: ../../modeling/index.md
  [2]: ../../miscellaneous/cli/index.md
