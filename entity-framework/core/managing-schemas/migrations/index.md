---
title: Migrationen – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/05/2018
uid: core/managing-schemas/migrations/index
ms.openlocfilehash: e9c4013d17a2d41772822f77b3ceba15702ffc48
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812058"
---
# <a name="migrations"></a><span data-ttu-id="5cbce-102">Migrationen</span><span class="sxs-lookup"><span data-stu-id="5cbce-102">Migrations</span></span>

<span data-ttu-id="5cbce-103">Ein Datenmodell ändert sich im Lauf der Entwicklung und ist nicht mehr synchron mit der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="5cbce-103">A data model changes during development and gets out of sync with the database.</span></span> <span data-ttu-id="5cbce-104">Sie können die Datenbank verwerfen und EF eine neue erstellen lassen, die dem Modell entspricht, aber dieses Vorgehen führt zum Verlust von Daten.</span><span class="sxs-lookup"><span data-stu-id="5cbce-104">You can drop the database and let EF create a new one that matches the model, but this procedure results in the loss of data.</span></span> <span data-ttu-id="5cbce-105">Das Migrations-Feature in EF Core bietet einen Weg, das Datenbankschema inkrementell zu aktualisieren, um es mit dem Datenmodell der Anwendung synchron zu halten und zugleich die vorhandenen Daten in der Datenbank beizubehalten.</span><span class="sxs-lookup"><span data-stu-id="5cbce-105">The migrations feature in EF Core provides a way to incrementally update the database schema to keep it in sync with the application's data model while preserving existing data in the database.</span></span>

<span data-ttu-id="5cbce-106">Migrations enthält Befehlszeilentools und APIs, die Sie bei den folgenden Aufgaben unterstützen:</span><span class="sxs-lookup"><span data-stu-id="5cbce-106">Migrations includes command-line tools and APIs that help with the following tasks:</span></span>

* <span data-ttu-id="5cbce-107">[Erstellen einer Migration](#create-a-migration).</span><span class="sxs-lookup"><span data-stu-id="5cbce-107">[Create a migration](#create-a-migration).</span></span> <span data-ttu-id="5cbce-108">Generieren Sie Code, mit dem die Datenbank aktualisiert werden kann, um sie mit einer Sammlung von Modelländerungen synchron zu halten.</span><span class="sxs-lookup"><span data-stu-id="5cbce-108">Generate code that can update the database to sync it with a set of model changes.</span></span>
* <span data-ttu-id="5cbce-109">[Aktualisieren der Datenbank](#update-the-database).</span><span class="sxs-lookup"><span data-stu-id="5cbce-109">[Update the database](#update-the-database).</span></span> <span data-ttu-id="5cbce-110">Wenden Sie ausstehende Migrationen an, um das Datenbankschema zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="5cbce-110">Apply pending migrations to update the database schema.</span></span>
* <span data-ttu-id="5cbce-111">[Anpassen des Migrationscodes](#customize-migration-code).</span><span class="sxs-lookup"><span data-stu-id="5cbce-111">[Customize migration code](#customize-migration-code).</span></span> <span data-ttu-id="5cbce-112">Manchmal muss der generierte Code geändert oder ergänzt werden.</span><span class="sxs-lookup"><span data-stu-id="5cbce-112">Sometimes the generated code needs to be modified or supplemented.</span></span>
* <span data-ttu-id="5cbce-113">[Entfernen einer Migration](#remove-a-migration).</span><span class="sxs-lookup"><span data-stu-id="5cbce-113">[Remove a migration](#remove-a-migration).</span></span> <span data-ttu-id="5cbce-114">Löschen Sie den generierten Code.</span><span class="sxs-lookup"><span data-stu-id="5cbce-114">Delete the generated code.</span></span>
* <span data-ttu-id="5cbce-115">[Rückgängigmachen einer Migration](#revert-a-migration).</span><span class="sxs-lookup"><span data-stu-id="5cbce-115">[Revert a migration](#revert-a-migration).</span></span> <span data-ttu-id="5cbce-116">Machen Sie die Datenbankänderungen rückgängig.</span><span class="sxs-lookup"><span data-stu-id="5cbce-116">Undo the database changes.</span></span>
* <span data-ttu-id="5cbce-117">[Generieren von SQL-Skripts](#generate-sql-scripts).</span><span class="sxs-lookup"><span data-stu-id="5cbce-117">[Generate SQL scripts](#generate-sql-scripts).</span></span> <span data-ttu-id="5cbce-118">Für das Aktualisieren einer Produktionsdatenbank oder zur Problembehandlung von Migrationscode benötigen Sie möglicherweise ein Skript.</span><span class="sxs-lookup"><span data-stu-id="5cbce-118">You might need a script to update a production database or to troubleshoot migration code.</span></span>
* <span data-ttu-id="5cbce-119">[Anwenden von Migrationen zur Laufzeit](#apply-migrations-at-runtime).</span><span class="sxs-lookup"><span data-stu-id="5cbce-119">[Apply migrations at runtime](#apply-migrations-at-runtime).</span></span> <span data-ttu-id="5cbce-120">Wenn Aktualisierungen zur Entwurfszeit und das Ausführen von Skripts nicht die besten Optionen sind, rufen Sie die Methode `Migrate()` auf.</span><span class="sxs-lookup"><span data-stu-id="5cbce-120">When design-time updates and running scripts aren't the best options, call the `Migrate()` method.</span></span>

> [!TIP]
> <span data-ttu-id="5cbce-121">Wenn sich `DbContext` in einer anderen Assembly als das Startprojekt befindet, können Sie die Ziel- und Startprojekte explizit in den Tools für die [Paket-Manager-Konsole](xref:core/miscellaneous/cli/powershell#target-and-startup-project) oder die [.NET Core-CLI](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project) angeben.</span><span class="sxs-lookup"><span data-stu-id="5cbce-121">If the `DbContext` is in a different assembly than the startup project, you can explicitly specify the target and startup projects in either the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell#target-and-startup-project) or the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet#target-project-and-startup-project).</span></span>

## <a name="install-the-tools"></a><span data-ttu-id="5cbce-122">Installieren der Tools</span><span class="sxs-lookup"><span data-stu-id="5cbce-122">Install the tools</span></span>

<span data-ttu-id="5cbce-123">Installieren Sie die [Befehlszeilentools](xref:core/miscellaneous/cli/index):</span><span class="sxs-lookup"><span data-stu-id="5cbce-123">Install the [command-line tools](xref:core/miscellaneous/cli/index):</span></span>

* <span data-ttu-id="5cbce-124">Für Visual Studio empfehlen wir die [Paket-Manager-Konsolentools](xref:core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="5cbce-124">For Visual Studio, we recommend the [Package Manager Console tools](xref:core/miscellaneous/cli/powershell).</span></span>
* <span data-ttu-id="5cbce-125">Wählen Sie für andere Entwicklungsumgebungen die [.NET Core-CLI-Tools aus](xref:core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="5cbce-125">For other development environments, choose the [.NET Core CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span>

## <a name="create-a-migration"></a><span data-ttu-id="5cbce-126">Erstellen einer Migration</span><span class="sxs-lookup"><span data-stu-id="5cbce-126">Create a migration</span></span>

<span data-ttu-id="5cbce-127">Nach dem [Definieren des Ausgangsmodells](xref:core/modeling/index) ist der Zeitpunkt gekommen, die Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="5cbce-127">After you've [defined your initial model](xref:core/modeling/index), it's time to create the database.</span></span> <span data-ttu-id="5cbce-128">Um eine anfängliche Migration hinzuzufügen, führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="5cbce-128">To add an initial migration, run the following command.</span></span>

``` powershell
Add-Migration InitialCreate
```

``` Console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="5cbce-129">Drei Dateien werden Ihrem Projekt im Verzeichnis **Migrationen** hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="5cbce-129">Three files are added to your project under the **Migrations** directory:</span></span>

* <span data-ttu-id="5cbce-130">**XXXXXXXXXXXXXX_InitialCreate.cs:** Die wichtigste Migrationsdatei.</span><span class="sxs-lookup"><span data-stu-id="5cbce-130">**XXXXXXXXXXXXXX_InitialCreate.cs**--The main migrations file.</span></span> <span data-ttu-id="5cbce-131">Enthält die Vorgänge, die zum Durchführen der Migration (in `Up()`) und Rückgängigmachen (in `Down()`) erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="5cbce-131">Contains the operations necessary to apply the migration (in `Up()`) and to revert it (in `Down()`).</span></span>
* <span data-ttu-id="5cbce-132">**XXXXXXXXXXXXXX_InitialCreate.Designer.cs:** Die Metadatendatei für die Migration.</span><span class="sxs-lookup"><span data-stu-id="5cbce-132">**XXXXXXXXXXXXXX_InitialCreate.Designer.cs**--The migrations metadata file.</span></span> <span data-ttu-id="5cbce-133">Enthält Informationen, die vom EF verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="5cbce-133">Contains information used by EF.</span></span>
* <span data-ttu-id="5cbce-134">**MyContextModelSnapshot.cs** – Eine Momentaufnahme des aktuellen Modells.</span><span class="sxs-lookup"><span data-stu-id="5cbce-134">**MyContextModelSnapshot.cs**--A snapshot of your current model.</span></span> <span data-ttu-id="5cbce-135">Diese wird verwendet, um festzustellen, was sich beim Hinzufügen der nächsten Migration geändert hat.</span><span class="sxs-lookup"><span data-stu-id="5cbce-135">Used to determine what changed when adding the next migration.</span></span>

<span data-ttu-id="5cbce-136">Mit dem Zeitstempel im Dateinamen können Migrationen chronologisch angeordnet werden, sodass Sie den Fortschritt der Änderungen mitverfolgen können.</span><span class="sxs-lookup"><span data-stu-id="5cbce-136">The timestamp in the filename helps keep them ordered chronologically so you can see the progression of changes.</span></span>

> [!TIP]
> <span data-ttu-id="5cbce-137">Es steht Ihnen frei, Migrationsdateien zu verschieben und ihren Namespace zu ändern.</span><span class="sxs-lookup"><span data-stu-id="5cbce-137">You are free to move Migrations files and change their namespace.</span></span> <span data-ttu-id="5cbce-138">Neue Migrationen werden als gleichgeordnete Elemente der letzten Migration erstellt.</span><span class="sxs-lookup"><span data-stu-id="5cbce-138">New migrations are created as siblings of the last migration.</span></span>

## <a name="update-the-database"></a><span data-ttu-id="5cbce-139">Aktualisieren der Datenbank</span><span class="sxs-lookup"><span data-stu-id="5cbce-139">Update the database</span></span>

<span data-ttu-id="5cbce-140">Als Nächstes führen Sie für die Datenbank die Migration durch, um das Schema zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="5cbce-140">Next, apply the migration to the database to create the schema.</span></span>

``` powershell
Update-Database
```

``` Console
dotnet ef database update
```

## <a name="customize-migration-code"></a><span data-ttu-id="5cbce-141">Anpassen des Migrationscodes</span><span class="sxs-lookup"><span data-stu-id="5cbce-141">Customize migration code</span></span>

<span data-ttu-id="5cbce-142">Nachdem Sie Änderungen an Ihrem EF Core-Modell vorgenommen haben, ist das Datenbankschema möglicherweise nicht mehr synchron. Fügen Sie eine andere Migration hinzu, um es auf dem neuesten Stand zu bringen.</span><span class="sxs-lookup"><span data-stu-id="5cbce-142">After making changes to your EF Core model, the database schema might be out of sync. To bring it up to date, add another migration.</span></span> <span data-ttu-id="5cbce-143">Der Migrationsname kann wie eine Commitnachricht in einem Versionskontrollsystem verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="5cbce-143">The migration name can be used like a commit message in a version control system.</span></span> <span data-ttu-id="5cbce-144">Beispielsweise können Sie einen Namen wie *AddProductReviews* wählen, wenn die Änderung eine neue Entitätsklasse für Rezensionen ist.</span><span class="sxs-lookup"><span data-stu-id="5cbce-144">For example, you might choose a name like *AddProductReviews* if the change is a new entity class for reviews.</span></span>

``` powershell
Add-Migration AddProductReviews
```

``` Console
dotnet ef migrations add AddProductReviews
```

<span data-ttu-id="5cbce-145">Nachdem das Gerüst für die Migration erstellt (Code für sie generiert) wurde, prüfen Sie den Code auf Genauigkeit und fügen Sie alle für seine korrekte Anwendung erforderlichen Änderungen hinzu, bzw. entfernen oder bearbeiten Sie sie.</span><span class="sxs-lookup"><span data-stu-id="5cbce-145">Once the migration is scaffolded (code generated for it), review the code for accuracy and add, remove or modify any operations required to apply it correctly.</span></span>

<span data-ttu-id="5cbce-146">Beispielsweise kann eine Migration die folgenden Vorgänge beinhalten:</span><span class="sxs-lookup"><span data-stu-id="5cbce-146">For example, a migration might contain the following operations:</span></span>

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

<span data-ttu-id="5cbce-147">Durch diese Vorgänge wird zwar Kompatibilität mit dem Datenbankschema hergestellt, die bestehenden Kundennamen werden jedoch nicht gespeichert.</span><span class="sxs-lookup"><span data-stu-id="5cbce-147">While these operations make the database schema compatible, they don't preserve the existing customer names.</span></span> <span data-ttu-id="5cbce-148">Um dies zu korrigieren, schreiben Sie es wie folgt neu:</span><span class="sxs-lookup"><span data-stu-id="5cbce-148">To make it better, rewrite it as follows.</span></span>

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
> <span data-ttu-id="5cbce-149">Der Einrüstvorgang der Migration warnt, wenn bei einem Vorgang Datenverlust eintreten kann (etwa beim Verwerfen einer Spalte).</span><span class="sxs-lookup"><span data-stu-id="5cbce-149">The migration scaffolding process warns when an operation might result in data loss (like dropping a column).</span></span> <span data-ttu-id="5cbce-150">Wenn diese Warnung angezeigt wird, überprüfen Sie den Migrationscode besonders gründlich auf Genauigkeit.</span><span class="sxs-lookup"><span data-stu-id="5cbce-150">If you see that warning, be especially sure to review the migrations code for accuracy.</span></span>

<span data-ttu-id="5cbce-151">Führen Sie für die Datenbank die Migration mit dem entsprechenden Befehl durch.</span><span class="sxs-lookup"><span data-stu-id="5cbce-151">Apply the migration to the database using the appropriate command.</span></span>

``` powershell
Update-Database
```

``` Console
dotnet ef database update
```

### <a name="empty-migrations"></a><span data-ttu-id="5cbce-152">Leere Migrationen</span><span class="sxs-lookup"><span data-stu-id="5cbce-152">Empty migrations</span></span>

<span data-ttu-id="5cbce-153">Manchmal ist es sinnvoll, eine Migration hinzuzufügen, ohne Modelländerungen vorzunehmen.</span><span class="sxs-lookup"><span data-stu-id="5cbce-153">Sometimes it's useful to add a migration without making any model changes.</span></span> <span data-ttu-id="5cbce-154">In diesem Fall werden durch das Hinzufügen einer neuen Migration Codedateien mit leeren Klassen erstellt.</span><span class="sxs-lookup"><span data-stu-id="5cbce-154">In this case, adding a new migration creates code files with empty classes.</span></span> <span data-ttu-id="5cbce-155">Sie können diese Migration anpassen, um Vorgänge durchzuführen, die sich nicht direkt auf das EF Core-Modell beziehen.</span><span class="sxs-lookup"><span data-stu-id="5cbce-155">You can customize this migration to perform operations that don't directly relate to the EF Core model.</span></span> <span data-ttu-id="5cbce-156">Einige Dinge, die Sie vielleicht auf diese Weise handhaben wollen, sind Folgende:</span><span class="sxs-lookup"><span data-stu-id="5cbce-156">Some things you might want to manage this way are:</span></span>

* <span data-ttu-id="5cbce-157">Volltextsuche</span><span class="sxs-lookup"><span data-stu-id="5cbce-157">Full-Text Search</span></span>
* <span data-ttu-id="5cbce-158">Funktionen</span><span class="sxs-lookup"><span data-stu-id="5cbce-158">Functions</span></span>
* <span data-ttu-id="5cbce-159">Gespeicherte Prozeduren</span><span class="sxs-lookup"><span data-stu-id="5cbce-159">Stored procedures</span></span>
* <span data-ttu-id="5cbce-160">Trigger</span><span class="sxs-lookup"><span data-stu-id="5cbce-160">Triggers</span></span>
* <span data-ttu-id="5cbce-161">Ansichten</span><span class="sxs-lookup"><span data-stu-id="5cbce-161">Views</span></span>

## <a name="remove-a-migration"></a><span data-ttu-id="5cbce-162">Entfernen einer Migration</span><span class="sxs-lookup"><span data-stu-id="5cbce-162">Remove a migration</span></span>

<span data-ttu-id="5cbce-163">Es kann vorkommen, dass Sie eine Migration hinzufügen und feststellen, dass Sie zusätzliche Änderungen an Ihrem EF Core-Modell vornehmen müssen, bevor Sie diese durchführen.</span><span class="sxs-lookup"><span data-stu-id="5cbce-163">Sometimes you add a migration and realize you need to make additional changes to your EF Core model before applying it.</span></span> <span data-ttu-id="5cbce-164">Um die letzte Migration zu entfernen, verwenden Sie diesen Befehl.</span><span class="sxs-lookup"><span data-stu-id="5cbce-164">To remove the last migration, use this command.</span></span>

``` powershell
Remove-Migration
```

``` Console
dotnet ef migrations remove
```

<span data-ttu-id="5cbce-165">Nachdem Sie die Migration entfernt haben, können Sie die zusätzlichen Modelländerungen vornehmen und sie erneut hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="5cbce-165">After removing the migration, you can make the additional model changes and add it again.</span></span>

## <a name="revert-a-migration"></a><span data-ttu-id="5cbce-166">Rückgängigmachen einer Migration</span><span class="sxs-lookup"><span data-stu-id="5cbce-166">Revert a migration</span></span>

<span data-ttu-id="5cbce-167">Wenn Sie bereits eine Migration (oder mehrere Migrationen) für die Datenbank durchgeführt haben, diese jedoch rückgängig machen müssen, können Sie mit demselben Befehl Migrationen durchführen, aber den Namen der Migration angeben, für die Sie ein Rollback ausführen möchten.</span><span class="sxs-lookup"><span data-stu-id="5cbce-167">If you already applied a migration (or several migrations) to the database but need to revert it, you can use the same command to apply migrations, but specify the name of the migration you want to roll back to.</span></span>

``` powershell
Update-Database LastGoodMigration
```

``` Console
dotnet ef database update LastGoodMigration
```

## <a name="generate-sql-scripts"></a><span data-ttu-id="5cbce-168">Generieren von SQL-Skripts</span><span class="sxs-lookup"><span data-stu-id="5cbce-168">Generate SQL scripts</span></span>

<span data-ttu-id="5cbce-169">Wenn Sie Ihre Migrationen debuggen oder in einer Produktionsdatenbank bereitstellen, ist es sinnvoll, ein SQL-Skript zu generieren.</span><span class="sxs-lookup"><span data-stu-id="5cbce-169">When debugging your migrations or deploying them to a production database, it's useful to generate a SQL script.</span></span> <span data-ttu-id="5cbce-170">Das Skript kann dann weiter auf Genauigkeit überprüft und auf die Anforderungen einer Produktionsdatenbank abgestimmt werden.</span><span class="sxs-lookup"><span data-stu-id="5cbce-170">The script can then be further reviewed for accuracy and tuned to fit the needs of a production database.</span></span> <span data-ttu-id="5cbce-171">Das Skript kann auch in Verbindung mit einer Bereitstellungstechnologie verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="5cbce-171">The script can also be used in conjunction with a deployment technology.</span></span> <span data-ttu-id="5cbce-172">Der grundlegende Befehl lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="5cbce-172">The basic command is as follows.</span></span>

``` powershell
Script-Migration
```

``` Console
dotnet ef migrations script
```

<span data-ttu-id="5cbce-173">Es gibt mehrere Optionen für diesen Befehl.</span><span class="sxs-lookup"><span data-stu-id="5cbce-173">There are several options to this command.</span></span>

<span data-ttu-id="5cbce-174">Die **from**-Migration sollte die letzte Migration sein, die vor der Skriptausführung für die Datenbank durchgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="5cbce-174">The **from** migration should be the last migration applied to the database before running the script.</span></span> <span data-ttu-id="5cbce-175">Wenn keine Migrationen durchgeführt wurden, geben Sie `0` an (dies ist die Standardeinstellung).</span><span class="sxs-lookup"><span data-stu-id="5cbce-175">If no migrations have been applied, specify `0` (this is the default).</span></span>

<span data-ttu-id="5cbce-176">Die **to**-Migration ist die letzte Migration, die nach der Skriptausführung für die Datenbank durchgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="5cbce-176">The **to** migration is the last migration that will be applied to the database after running the script.</span></span> <span data-ttu-id="5cbce-177">Dies ist standardmäßig die letzte Migration in Ihrem Projekt.</span><span class="sxs-lookup"><span data-stu-id="5cbce-177">This defaults to the last migration in your project.</span></span>

<span data-ttu-id="5cbce-178">Optional kann ein **idempotent**-Skript generiert werden.</span><span class="sxs-lookup"><span data-stu-id="5cbce-178">An **idempotent** script can optionally be generated.</span></span> <span data-ttu-id="5cbce-179">Dieses Skript wendet nur Migrationen an, wenn sie nicht bereits für die Datenbank durchgeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="5cbce-179">This script only applies migrations if they haven't already been applied to the database.</span></span> <span data-ttu-id="5cbce-180">Dies ist hilfreich, wenn Sie nicht genau wissen, welche Migration für die Datenbank zuletzt durchgeführt wurde oder wenn Bereitstellungen in mehreren Datenbanken, die sich jeweils in unterschiedlichen Migrationen befinden, erfolgen.</span><span class="sxs-lookup"><span data-stu-id="5cbce-180">This is useful if you don't exactly know what the last migration applied to the database was or if you are deploying to multiple databases that may each be at a different migration.</span></span>

## <a name="apply-migrations-at-runtime"></a><span data-ttu-id="5cbce-181">Anwenden von Migrationen zur Laufzeit</span><span class="sxs-lookup"><span data-stu-id="5cbce-181">Apply migrations at runtime</span></span>

<span data-ttu-id="5cbce-182">Einige Apps sollten beim Start oder bei der ersten Ausführung Migrationen zur Runtime durchführen.</span><span class="sxs-lookup"><span data-stu-id="5cbce-182">Some apps may want to apply migrations at runtime during startup or first run.</span></span> <span data-ttu-id="5cbce-183">Dies ist mit der Methode `Migrate()` möglich.</span><span class="sxs-lookup"><span data-stu-id="5cbce-183">Do this using the `Migrate()` method.</span></span>

<span data-ttu-id="5cbce-184">Diese Methode basiert auf dem `IMigrator`-Dienst, der für erweiterte Szenarien verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="5cbce-184">This method builds on top of the `IMigrator` service, which can be used for more advanced scenarios.</span></span> <span data-ttu-id="5cbce-185">Verwenden Sie `myDbContext.GetInfrastructure().GetService<IMigrator>()`, um darauf zugreifen.</span><span class="sxs-lookup"><span data-stu-id="5cbce-185">Use `myDbContext.GetInfrastructure().GetService<IMigrator>()` to access it.</span></span>

``` csharp
myDbContext.Database.Migrate();
```

> [!WARNING]
>
> * <span data-ttu-id="5cbce-186">Diese Vorgehensweise ist nicht für alle geeignet.</span><span class="sxs-lookup"><span data-stu-id="5cbce-186">This approach isn't for everyone.</span></span> <span data-ttu-id="5cbce-187">Diese eignet sich zwar ideal für Apps mit einer lokalen Datenbank, doch die meisten Anwendungen erfordern zuverlässigere Bereitstellungsstrategien wie etwa durch das Generieren von SQL-Skripts.</span><span class="sxs-lookup"><span data-stu-id="5cbce-187">While it's great for apps with a local database, most applications will require more robust deployment strategy like generating SQL scripts.</span></span>
> * <span data-ttu-id="5cbce-188">Rufen Sie `EnsureCreated()` nicht vor `Migrate()` auf.</span><span class="sxs-lookup"><span data-stu-id="5cbce-188">Don't call `EnsureCreated()` before `Migrate()`.</span></span> <span data-ttu-id="5cbce-189">`EnsureCreated()` umgeht Migrationen zum Erstellen von Schemas, was dazu führt, dass `Migrate()` fehlerhaft ist.</span><span class="sxs-lookup"><span data-stu-id="5cbce-189">`EnsureCreated()` bypasses Migrations to create the schema, which causes `Migrate()` to fail.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5cbce-190">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="5cbce-190">Next steps</span></span>

<span data-ttu-id="5cbce-191">Weitere Informationen finden Sie unter <xref:core/miscellaneous/cli/index>.</span><span class="sxs-lookup"><span data-stu-id="5cbce-191">For more information, see <xref:core/miscellaneous/cli/index>.</span></span>
