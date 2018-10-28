---
title: Erste Schritte in UWP – Neue Datenbank – EF Core
author: rowanmiller
ms.date: 10/13/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: 456967a0dc981053919064a19cc9c98bf7309865
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022375"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="cbe6e-102">Erste Schritte mit EF Core in UWP (Universelle Windows-Plattform) mit einer neuen Datenbank</span><span class="sxs-lookup"><span data-stu-id="cbe6e-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

<span data-ttu-id="cbe6e-103">In diesem Tutorial erstellen Sie eine UWP-Anwendung, die grundlegenden Datenzugriff auf eine lokale SQLite-Datenbank mithilfe von Entity Framework Core ausführt.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-103">In this tutorial, you build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="cbe6e-104">[Sehen Sie sich das Beispiel aus diesem Artikel auf GitHub an](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span><span class="sxs-lookup"><span data-stu-id="cbe6e-104">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cbe6e-105">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="cbe6e-105">Prerequisites</span></span>

* <span data-ttu-id="cbe6e-106">[Windows 10 Fall Creators Update (10.0, Build 16299) oder höher](https://support.microsoft.com/help/4027667/windows-update-windows-10).</span><span class="sxs-lookup"><span data-stu-id="cbe6e-106">[Windows 10 Fall Creators Update (10.0; Build 16299) or later](https://support.microsoft.com/help/4027667/windows-update-windows-10).</span></span>

* <span data-ttu-id="cbe6e-107">[Visual Studio 2017](https://www.visualstudio.com/downloads/), Version 15.7 oder höher, mit der Workload für die **UWP-Entwicklung**.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-107">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with the **Universal Windows Platform Development** workload.</span></span>

* <span data-ttu-id="cbe6e-108">[.NET Core 2.1 SDK oder höher](https://www.microsoft.com/net/core).</span><span class="sxs-lookup"><span data-stu-id="cbe6e-108">[.NET Core 2.1 SDK or later](https://www.microsoft.com/net/core) or later.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cbe6e-109">Dieses Tutorial verwendet [Migrations](xref:core/managing-schemas/migrations/index)befehle aus Entity Framework Core, um das Datenbankschema zu erstellen und zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-109">This tutorial uses Entity Framework Core [migrations](xref:core/managing-schemas/migrations/index) commands to create and update the schema of the database.</span></span>
> <span data-ttu-id="cbe6e-110">Diese Befehle funktionieren nicht direkt in UWP-Projekten.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-110">These commands don't work directly with UWP projects.</span></span>
> <span data-ttu-id="cbe6e-111">Daher befindet sich das Datenmodell der Anwendung in einem freigegebenen Bibliotheksprojekt, und eine separate .NET Core-Konsolenanwendung wird verwendet, um die Befehle auszuführen.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-111">For this reason, the application's data model is placed in a shared library project, and a separate .NET Core console application is used to run the commands.</span></span>

## <a name="create-a-library-project-to-hold-the-data-model"></a><span data-ttu-id="cbe6e-112">Erstellen eines Bibliotheksprojekts für das Datenmodell</span><span class="sxs-lookup"><span data-stu-id="cbe6e-112">Create a library project to hold the data model</span></span>

* <span data-ttu-id="cbe6e-113">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-113">Open Visual Studio</span></span>

* <span data-ttu-id="cbe6e-114">Klicken Sie auf **Datei > Neu > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-114">**File > New > Project**</span></span>

* <span data-ttu-id="cbe6e-115">Wählen Sie im Menü auf der linken Seite **Installiert > Visual C# > .NET Standard** aus.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-115">From the left menu select **Installed > Visual C# > .NET Standard**.</span></span>

* <span data-ttu-id="cbe6e-116">Wählen Sie die Vorlage **Klassenbibliothek (.NET Standard)** aus.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-116">Select the **Class Library (.NET Standard)** template.</span></span>

* <span data-ttu-id="cbe6e-117">Nennen Sie das Projekt *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-117">Name the project *Blogging.Model*.</span></span>

* <span data-ttu-id="cbe6e-118">Nennen Sie die Projektmappe *Blogging*.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-118">Name the solution *Blogging*.</span></span>

* <span data-ttu-id="cbe6e-119">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-119">Click **OK**.</span></span>

## <a name="install-entity-framework-core-runtime-in-the-data-model-project"></a><span data-ttu-id="cbe6e-120">Installieren von Entity Framework Core-Runtime im Datenmodell-Projekt</span><span class="sxs-lookup"><span data-stu-id="cbe6e-120">Install Entity Framework Core runtime in the data model project</span></span>

<span data-ttu-id="cbe6e-121">Installieren Sie zur Verwendung von EF Core das Paket für den (oder die) gewünschten Datenbankanbieter.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-121">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="cbe6e-122">In diesem Tutorial wird SQLite verwendet.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-122">This tutorial uses SQLite.</span></span> <span data-ttu-id="cbe6e-123">Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="cbe6e-123">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="cbe6e-124">**Tools > NuGet-Paket-Manager > Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-124">**Tools > NuGet Package Manager > Package Manager Console**.</span></span>

* <span data-ttu-id="cbe6e-125">Stellen Sie sicher, dass das Bibliotheksprojekt *Blogging.Model* in der Paket-Manager-Konsole als **Default Project** (Standardprojekt) ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-125">Make sure that the library project *Blogging.Model* is selected as the **Default Project** in the Package Manager Console.</span></span>

* <span data-ttu-id="cbe6e-126">Ausführen von `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span><span class="sxs-lookup"><span data-stu-id="cbe6e-126">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="cbe6e-127">Erstellen des Datenmodells</span><span class="sxs-lookup"><span data-stu-id="cbe6e-127">Create the data model</span></span>

<span data-ttu-id="cbe6e-128">Jetzt ist es Zeit, den *DbContext* und Entitätsklassen für das Modell zu definieren.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-128">Now it's time to define the *DbContext* and entity classes that make up the model.</span></span>

* <span data-ttu-id="cbe6e-129">Löschen Sie *Class1.cs*.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-129">Delete *Class1.cs*.</span></span>

* <span data-ttu-id="cbe6e-130">Erstellen Sie *Model.cs* mit dem folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="cbe6e-130">Create *Model.cs* with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-console-project-to-run-migrations-commands"></a><span data-ttu-id="cbe6e-131">Erstellen eines neuen Konsolenprojekts zum Ausführen von Migrationsbefehlen</span><span class="sxs-lookup"><span data-stu-id="cbe6e-131">Create a new console project to run migrations commands</span></span>

* <span data-ttu-id="cbe6e-132">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Projektmappe, und wählen Sie dann **Hinzufügen > Neues Projekt** aus.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-132">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="cbe6e-133">Klicken Sie im Menü links auf **Installiert > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-133">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>

* <span data-ttu-id="cbe6e-134">Wählen Sie die Projektvorlage **Konsolen-App (.NET Core)** aus.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-134">Select the **Console App (.NET Core)** project template.</span></span>

* <span data-ttu-id="cbe6e-135">Nennen Sie das Projekt *Blogging.Migrations.Startup*, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-135">Name the project *Blogging.Migrations.Startup*, and click **OK**.</span></span>

* <span data-ttu-id="cbe6e-136">Fügen Sie dem *Blogging.Model*-Projekt einen Projektverweis aus dem *Blogging.Migrations.Startup*-Projekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-136">Add a project reference from the *Blogging.Migrations.Startup* project to the *Blogging.Model* project.</span></span>

## <a name="install-entity-framework-core-tools-in-the-migrations-startup-project"></a><span data-ttu-id="cbe6e-137">Installieren von Entity Framework Core-Tools im Migrations-Startprojekt</span><span class="sxs-lookup"><span data-stu-id="cbe6e-137">Install Entity Framework Core tools in the migrations startup project</span></span>

<span data-ttu-id="cbe6e-138">Um EF Core-Migrationsbefehle in der Paket-Manager-Konsole zu aktivieren, installieren Sie das EF Core-Toolpaket in der Konsolenanwendung.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-138">To enable the EF Core migration commands in the Package Manager Console, install the EF Core tools package in the console application.</span></span>

* <span data-ttu-id="cbe6e-139">Wählen Sie **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-139">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="cbe6e-140">Ausführen von `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`</span><span class="sxs-lookup"><span data-stu-id="cbe6e-140">Run `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`</span></span>

## <a name="create-the-initial-migration"></a><span data-ttu-id="cbe6e-141">Erstellen der ursprünglichen Migration</span><span class="sxs-lookup"><span data-stu-id="cbe6e-141">Create the initial migration</span></span>

 <span data-ttu-id="cbe6e-142">Erstellen Sie die ursprüngliche Migration, indem Sie die Konsolenanwendung als Startprojekt angeben.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-142">Create the initial migration, specifying the console application as the startup project.</span></span>

* <span data-ttu-id="cbe6e-143">Ausführen von `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`</span><span class="sxs-lookup"><span data-stu-id="cbe6e-143">Run `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`</span></span>

<span data-ttu-id="cbe6e-144">Dieser Befehl richtet über Gerüstbau eine Migration zum Erstellen des anfänglichen Datenbanktabellensatzes für Ihr Datenmodell ein.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-144">This command scaffolds a migration that creates the initial set of database tables for your data model.</span></span>

## <a name="create-the-uwp-project"></a><span data-ttu-id="cbe6e-145">Erstellen eines UWP-Projekts</span><span class="sxs-lookup"><span data-stu-id="cbe6e-145">Create the UWP project</span></span>

* <span data-ttu-id="cbe6e-146">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Projektmappe, und wählen Sie dann **Hinzufügen > Neues Projekt** aus.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-146">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="cbe6e-147">Wählen Sie im Menü links **Installiert > Visual C# > Universelles Windows** aus.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-147">From the left menu select **Installed > Visual C# > Windows Universal**.</span></span>

* <span data-ttu-id="cbe6e-148">Wählen Sie die Projektvorlage **Leere App (universelles Windows)** aus.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-148">Select the **Blank App (Universal Windows)** project template.</span></span>

* <span data-ttu-id="cbe6e-149">Nennen Sie das Projekt *Blogging.UWP*, und klicken Sie auf **OK**</span><span class="sxs-lookup"><span data-stu-id="cbe6e-149">Name the project *Blogging.UWP*, and click **OK**</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cbe6e-150">Legen Sie die Ziel- und Mindestversionen auf mindestens **Windows 10 Fall Creators Update (10.0, Build 16299.0)** fest.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-150">Set the target and minimum versions to at least **Windows 10 Fall Creators Update (10.0; build 16299.0)**.</span></span>
> <span data-ttu-id="cbe6e-151">.NET Standard 2.0 ist für Entity Framework Core erforderlich, wird aber von älteren Windows 10-Versionen nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-151">Previous  versions of Windows 10 do not support .NET Standard 2.0, which is required by Entity Framework Core.</span></span>

## <a name="add-code-to-create-the-database-on-application-startup"></a><span data-ttu-id="cbe6e-152">Hinzufügen eines Codes zum Erstellen der Datenbank beim Anwendungsstart</span><span class="sxs-lookup"><span data-stu-id="cbe6e-152">Add code to create the database on application startup</span></span>

<span data-ttu-id="cbe6e-153">Da die Datenbank auf dem Gerät erstellt werden soll, auf dem die App ausgeführt wird, fügen Sie Code hinzu, mit dem beim Anwendungsstart ausstehende Migrationen auf die lokale Datenbank angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-153">Since you want the database to be created on the device that the app runs on, add code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="cbe6e-154">Bei der ersten App-Ausführung wird hierbei die lokale Datenbank erstellt.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-154">The first time that the app runs, this will take care of creating the local database.</span></span>

* <span data-ttu-id="cbe6e-155">Fügen Sie dem *Blogging.Model*-Projekt einen Projektverweis aus dem *Blogging.UWP*-Projekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-155">Add a project reference from the *Blogging.UWP* project to the *Blogging.Model* project.</span></span>

* <span data-ttu-id="cbe6e-156">Öffnen Sie *App.Xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-156">Open *App.xaml.cs*.</span></span>

* <span data-ttu-id="cbe6e-157">Fügen Sie den hervorgehobenen Code hinzu, um ausstehende Migrationen anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-157">Add the highlighted code to apply any pending migrations.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> <span data-ttu-id="cbe6e-158">Wenn Sie Änderungen an Ihrem Modell vornehmen, können Sie mit dem Befehl `Add-Migration` per Gerüstbau eine neue Migration einrichten, um die entsprechenden Änderungen auf die Datenbank anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-158">If you change your model, use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="cbe6e-159">Alle ausstehenden Migrationen werden beim Start der Anwendung auf die lokale Datenbank auf jedem Gerät angewendet.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-159">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="cbe6e-160">EF Core verwendet eine `__EFMigrationsHistory`-Tabelle in der Datenbank, um nachzuverfolgen, welche Migrationen bereits auf die Datenbank angewendet wurden.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-160">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-data-model"></a><span data-ttu-id="cbe6e-161">Verwenden des Datenmodells</span><span class="sxs-lookup"><span data-stu-id="cbe6e-161">Use the data model</span></span>

<span data-ttu-id="cbe6e-162">Jetzt können Sie EF Core zum Zugreifen auf Daten verwenden.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-162">You can now use EF Core to perform data access.</span></span>

* <span data-ttu-id="cbe6e-163">Öffnen Sie *MainPage.xaml*.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-163">Open *MainPage.xaml*.</span></span>

* <span data-ttu-id="cbe6e-164">Fügen Sie den nachstehend markierten Handler zum Laden der Seite und den Benutzerschnittstelleninhalt ein.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-164">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="cbe6e-165">Fügen Sie jetzt Code zum Verknüpfen der Benutzeroberfläche mit der Datenbank hinzu.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-165">Now add code to wire up the UI with the database</span></span>

* <span data-ttu-id="cbe6e-166">Öffnen Sie *MainPage.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-166">Open *MainPage.xaml.cs*.</span></span>

* <span data-ttu-id="cbe6e-167">Fügen Sie den hervorgehobenen Code aus der folgenden Liste hinzu:</span><span class="sxs-lookup"><span data-stu-id="cbe6e-167">Add the highlighted code from the following listing:</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

<span data-ttu-id="cbe6e-168">Sie können die Anwendung jetzt ausführen, um sie in Aktion zu sehen.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-168">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="cbe6e-169">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das *Blogging.UWP*-Projekt, und wählen Sie dann **Bereitstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-169">In **Solution Explorer**, right-click the *Blogging.UWP* project and then select **Deploy**.</span></span>

* <span data-ttu-id="cbe6e-170">Legen Sie *Blogging.UWP* als Startprojekt fest.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-170">Set *Blogging.UWP* as the startup project.</span></span>

* <span data-ttu-id="cbe6e-171">Klicken Sie auf **Debuggen > Ohne Debuggen starten**.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-171">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="cbe6e-172">Die App wird erstellt und ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-172">The app builds and runs.</span></span>

* <span data-ttu-id="cbe6e-173">Geben Sie eine URL ein, und klicken Sie auf die Schaltfläche **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-173">Enter a URL and click the **Add** button</span></span>

  ![Bild](_static/create.png)

  ![Bild](_static/list.png)

  <span data-ttu-id="cbe6e-176">Voilá!</span><span class="sxs-lookup"><span data-stu-id="cbe6e-176">Tada!</span></span> <span data-ttu-id="cbe6e-177">Sie verfügen jetzt über eine einfache UWP-App, die Entity Framework Core ausführt.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-177">You now have a simple UWP app running Entity Framework Core.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cbe6e-178">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="cbe6e-178">Next steps</span></span>

<span data-ttu-id="cbe6e-179">Informationen zur Kompatibilität und Leistung, die Sie bei der Verwendung von EF Core mit UWP kennen sollten, finden Sie unter [.NET-Implementierungen, die von EF Core unterstützt werden](../../platforms/index.md#universal-windows-platform).</span><span class="sxs-lookup"><span data-stu-id="cbe6e-179">For compatibility and performance information that you should know when using EF Core with UWP, see [.NET implementations supported by EF Core](../../platforms/index.md#universal-windows-platform).</span></span>

<span data-ttu-id="cbe6e-180">Sehen Sie sich auch die weiteren Artikel in dieser Dokumentation an, um mehr über die Features von Entity Framework Core zu erfahren.</span><span class="sxs-lookup"><span data-stu-id="cbe6e-180">Check out other articles in this documentation to learn more about Entity Framework Core features.</span></span>
