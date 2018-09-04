---
title: Erste Schritte in UWP – Neue Datenbank – EF Core
author: rowanmiller
ms.date: 08/08/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: c243ef2a1940af9bf4f4b32f17acfcce7f972862
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996909"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="efc10-102">Erste Schritte mit EF Core in UWP (Universelle Windows-Plattform) mit einer neuen Datenbank</span><span class="sxs-lookup"><span data-stu-id="efc10-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

<span data-ttu-id="efc10-103">In diesem Tutorial erstellen Sie eine UWP-Anwendung, die grundlegenden Datenzugriff auf eine lokale SQLite-Datenbank mithilfe von Entity Framework Core ausführt.</span><span class="sxs-lookup"><span data-stu-id="efc10-103">In this tutorial, you build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework Core.</span></span>

<span data-ttu-id="efc10-104">[Sehen Sie sich das Beispiel aus diesem Artikel auf GitHub an](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span><span class="sxs-lookup"><span data-stu-id="efc10-104">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="efc10-105">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="efc10-105">Prerequisites</span></span>

* <span data-ttu-id="efc10-106">[Windows 10 Fall Creators Update (10.0, Build 16299) oder höher](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span><span class="sxs-lookup"><span data-stu-id="efc10-106">[Windows 10 Fall Creators Update (10.0; Build 16299) or later](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).</span></span>

* <span data-ttu-id="efc10-107">[Visual Studio 2017](https://www.visualstudio.com/downloads/), Version 15.7 oder höher, mit der Workload für die **UWP-Entwicklung**.</span><span class="sxs-lookup"><span data-stu-id="efc10-107">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with the **Universal Windows Platform Development** workload.</span></span>

* <span data-ttu-id="efc10-108">[.NET Core 2.1 SDK oder höher](https://www.microsoft.com/net/core).</span><span class="sxs-lookup"><span data-stu-id="efc10-108">[.NET Core 2.1 SDK or later](https://www.microsoft.com/net/core) or later.</span></span>

## <a name="create-a-model-project"></a><span data-ttu-id="efc10-109">Erstellen eines Modellprojekts</span><span class="sxs-lookup"><span data-stu-id="efc10-109">Create a model project</span></span>

> [!IMPORTANT]
> <span data-ttu-id="efc10-110">Aufgrund von Einschränkungen in der Interaktion zwischen .NET Core-Tools mit UWP-Projekten muss das Modell in einem Nicht-UWP-Projekt platziert werden, um Migrationsbefehle in der **Paket-Manager-Konsole** (PMC) ausführen zu können.</span><span class="sxs-lookup"><span data-stu-id="efc10-110">Due to limitations in the way .NET Core tools interact with UWP projects the model needs to be placed in a non-UWP project to be able to run migrations commands in the **Package Manager Console** (PMC)</span></span>

* <span data-ttu-id="efc10-111">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="efc10-111">Open Visual Studio</span></span>

* <span data-ttu-id="efc10-112">Klicken Sie auf **Datei > Neu > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="efc10-112">**File > New > Project**</span></span>

* <span data-ttu-id="efc10-113">Wählen Sie im Menü auf der linken Seite **Installiert > Visual C# > .NET Standard** aus.</span><span class="sxs-lookup"><span data-stu-id="efc10-113">From the left menu select **Installed > Visual C# > .NET Standard**.</span></span>

* <span data-ttu-id="efc10-114">Wählen Sie die Vorlage **Klassenbibliothek (.NET Standard)** aus.</span><span class="sxs-lookup"><span data-stu-id="efc10-114">Select the **Class Library (.NET Standard)** template.</span></span>

* <span data-ttu-id="efc10-115">Nennen Sie das Projekt *Blogging.Model*.</span><span class="sxs-lookup"><span data-stu-id="efc10-115">Name the project *Blogging.Model*.</span></span>

* <span data-ttu-id="efc10-116">Nennen Sie die Projektmappe *Blogging*.</span><span class="sxs-lookup"><span data-stu-id="efc10-116">Name the solution *Blogging*.</span></span>

* <span data-ttu-id="efc10-117">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="efc10-117">Click **OK**.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="efc10-118">Installieren von Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="efc10-118">Install Entity Framework Core</span></span>

<span data-ttu-id="efc10-119">Installieren Sie zur Verwendung von EF Core das Paket für den (oder die) gewünschten Datenbankanbieter.</span><span class="sxs-lookup"><span data-stu-id="efc10-119">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="efc10-120">In diesem Tutorial wird SQLite verwendet.</span><span class="sxs-lookup"><span data-stu-id="efc10-120">This tutorial uses SQLite.</span></span> <span data-ttu-id="efc10-121">Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="efc10-121">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="efc10-122">**Tools > NuGet-Paket-Manager > Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="efc10-122">**Tools > NuGet Package Manager > Package Manager Console**.</span></span>

* <span data-ttu-id="efc10-123">Ausführen von `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span><span class="sxs-lookup"><span data-stu-id="efc10-123">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

<span data-ttu-id="efc10-124">Später in diesem Tutorial verwenden Sie Entity Framework Core-Tools zum Verwalten der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="efc10-124">Later in this tutorial you will be using some Entity Framework Core tools to maintain the database.</span></span> <span data-ttu-id="efc10-125">Installieren Sie deshalb auch das Toolpaket.</span><span class="sxs-lookup"><span data-stu-id="efc10-125">So install the tools package as well.</span></span>

* <span data-ttu-id="efc10-126">Ausführen von `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="efc10-126">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="efc10-127">Erstellen des Modells</span><span class="sxs-lookup"><span data-stu-id="efc10-127">Create the model</span></span>

<span data-ttu-id="efc10-128">Jetzt ist es Zeit, einen Kontext und Entitätsklassen für das Modell zu definieren.</span><span class="sxs-lookup"><span data-stu-id="efc10-128">Now it's time to define a context and entity classes that make up the model.</span></span>

* <span data-ttu-id="efc10-129">Löschen Sie *Class1.cs*.</span><span class="sxs-lookup"><span data-stu-id="efc10-129">Delete *Class1.cs*.</span></span>

* <span data-ttu-id="efc10-130">Erstellen Sie *Model.cs* mit dem folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="efc10-130">Create *Model.cs* with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a><span data-ttu-id="efc10-131">Erstellen eines neuen UWP-Projekts</span><span class="sxs-lookup"><span data-stu-id="efc10-131">Create a new UWP project</span></span>

* <span data-ttu-id="efc10-132">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Projektmappe, und wählen Sie dann **Hinzufügen > Neues Projekt** aus.</span><span class="sxs-lookup"><span data-stu-id="efc10-132">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="efc10-133">Wählen Sie im Menü links **Installiert > Visual C# > Universelles Windows** aus.</span><span class="sxs-lookup"><span data-stu-id="efc10-133">From the left menu select **Installed > Visual C# > Windows Universal**.</span></span>

* <span data-ttu-id="efc10-134">Wählen Sie die Projektvorlage **Leere App (universelles Windows)** aus.</span><span class="sxs-lookup"><span data-stu-id="efc10-134">Select the **Blank App (Universal Windows)** project template.</span></span>

* <span data-ttu-id="efc10-135">Nennen Sie das Projekt *Blogging.UWP*, und klicken Sie auf **OK**</span><span class="sxs-lookup"><span data-stu-id="efc10-135">Name the project *Blogging.UWP*, and click **OK**</span></span>

* <span data-ttu-id="efc10-136">Legen Sie die Ziel- und Mindestversionen auf mindestens **Windows 10 Fall Creators Update (10.0, Build 16299.0)** fest.</span><span class="sxs-lookup"><span data-stu-id="efc10-136">Set the target and minimum versions to at least **Windows 10 Fall Creators Update (10.0; build 16299.0)**.</span></span>

## <a name="create-the-initial-migration"></a><span data-ttu-id="efc10-137">Erstellen der ursprünglichen Migration</span><span class="sxs-lookup"><span data-stu-id="efc10-137">Create the initial migration</span></span>

<span data-ttu-id="efc10-138">Da Sie nun über ein Modell verfügen, richten Sie die App so ein, dass sie beim ersten Start zunächst eine Datenbank erstellt.</span><span class="sxs-lookup"><span data-stu-id="efc10-138">Now that you have a model, set up the app to create a database the first time it runs.</span></span> <span data-ttu-id="efc10-139">In diesem Abschnitt erstellen Sie die ursprüngliche Migration.</span><span class="sxs-lookup"><span data-stu-id="efc10-139">In this section, you create the initial migration.</span></span> <span data-ttu-id="efc10-140">Im folgenden Abschnitt fügen Sie Code hinzu, der diese Migration anwendet, wenn die App gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="efc10-140">In the following section, you add code that applies this migration when the app starts.</span></span>

<span data-ttu-id="efc10-141">Migrationstools erfordern ein Nicht-UWP-Startprojekt. Erstellen Sie dieses also zuerst.</span><span class="sxs-lookup"><span data-stu-id="efc10-141">Migrations tools require a non-UWP startup project, so create that first.</span></span>

* <span data-ttu-id="efc10-142">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Projektmappe, und wählen Sie dann **Hinzufügen > Neues Projekt** aus.</span><span class="sxs-lookup"><span data-stu-id="efc10-142">In **Solution Explorer**, right-click the solution, and then choose **Add > New Project**.</span></span>

* <span data-ttu-id="efc10-143">Klicken Sie im Menü links auf **Installiert > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="efc10-143">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>

* <span data-ttu-id="efc10-144">Wählen Sie die Projektvorlage **Konsolen-App (.NET Core)** aus.</span><span class="sxs-lookup"><span data-stu-id="efc10-144">Select the **Console App (.NET Core)** project template.</span></span>

* <span data-ttu-id="efc10-145">Nennen Sie das Projekt *Blogging.Migrations.Startup*, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="efc10-145">Name the project *Blogging.Migrations.Startup*, and click **OK**.</span></span>

* <span data-ttu-id="efc10-146">Fügen Sie dem *Blogging.Model*-Projekt einen Projektverweis aus dem *Blogging.Migrations.Startup*-Projekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="efc10-146">Add a project reference from the *Blogging.Migrations.Startup* project to the *Blogging.Model* project.</span></span>

<span data-ttu-id="efc10-147">Jetzt können Sie Ihre ursprüngliche Migration erstellen.</span><span class="sxs-lookup"><span data-stu-id="efc10-147">Now you can create your initial migration.</span></span>

* <span data-ttu-id="efc10-148">Wählen Sie **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="efc10-148">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="efc10-149">Wählen Sie das *Blogging.Model*-Projekt als **Standardprojekt** aus.</span><span class="sxs-lookup"><span data-stu-id="efc10-149">Select the *Blogging.Model* project as the **Default project**.</span></span>

* <span data-ttu-id="efc10-150">Legen Sie im **Projektmappen-Explorer** das Projekt *Blogging.Migrations.Startup* als Startprojekt fest.</span><span class="sxs-lookup"><span data-stu-id="efc10-150">In **Solution Explorer**, set the *Blogging.Migrations.Startup* project as the startup project.</span></span>

* <span data-ttu-id="efc10-151">Führen Sie `Add-Migration InitialCreate` aus.</span><span class="sxs-lookup"><span data-stu-id="efc10-151">Run `Add-Migration InitialCreate`.</span></span>

  <span data-ttu-id="efc10-152">Dieser Befehl richtet über Gerüstbau eine Migration zum Erstellen des anfänglichen Tabellensatzes für Ihr Modell ein.</span><span class="sxs-lookup"><span data-stu-id="efc10-152">This command scaffolds a migration that creates the initial set of tables for your model.</span></span>

## <a name="create-the-database-on-app-startup"></a><span data-ttu-id="efc10-153">Erstellen der Datenbank beim Start der App</span><span class="sxs-lookup"><span data-stu-id="efc10-153">Create the database on app startup</span></span>

<span data-ttu-id="efc10-154">Da die Datenbank auf dem Gerät erstellt werden soll, auf dem die App ausgeführt wird, fügen Sie Code hinzu, mit dem beim Anwendungsstart ausstehende Migrationen auf die lokale Datenbank angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="efc10-154">Since you want the database to be created on the device that the app runs on, add code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="efc10-155">Bei der ersten App-Ausführung wird hierbei die lokale Datenbank erstellt.</span><span class="sxs-lookup"><span data-stu-id="efc10-155">The first time that the app runs, this will take care of creating the local database.</span></span>

* <span data-ttu-id="efc10-156">Fügen Sie dem *Blogging.Model*-Projekt einen Projektverweis aus dem *Blogging.UWP*-Projekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="efc10-156">Add a project reference from the *Blogging.UWP* project to the *Blogging.Model* project.</span></span>

* <span data-ttu-id="efc10-157">Öffnen Sie *App.Xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="efc10-157">Open *App.xaml.cs*.</span></span>

* <span data-ttu-id="efc10-158">Fügen Sie den hervorgehobenen Code hinzu, um ausstehende Migrationen anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="efc10-158">Add the highlighted code to apply any pending migrations.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> <span data-ttu-id="efc10-159">Wenn Sie Änderungen an Ihrem Modell vornehmen, können Sie mit dem Befehl `Add-Migration` per Gerüstbau eine neue Migration einrichten, um die entsprechenden Änderungen auf die Datenbank anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="efc10-159">If you change your model, use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="efc10-160">Alle ausstehenden Migrationen werden beim Start der Anwendung auf die lokale Datenbank auf jedem Gerät angewendet.</span><span class="sxs-lookup"><span data-stu-id="efc10-160">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="efc10-161">EF verwendet eine `__EFMigrationsHistory`-Tabelle in der Datenbank, um nachzuverfolgen, welche Migrationen bereits auf die Datenbank angewendet wurden.</span><span class="sxs-lookup"><span data-stu-id="efc10-161">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="efc10-162">Verwenden des Modells</span><span class="sxs-lookup"><span data-stu-id="efc10-162">Use the model</span></span>

<span data-ttu-id="efc10-163">Jetzt können Sie das Modell zum Zugreifen auf Daten verwenden.</span><span class="sxs-lookup"><span data-stu-id="efc10-163">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="efc10-164">Öffnen Sie *MainPage.xaml*.</span><span class="sxs-lookup"><span data-stu-id="efc10-164">Open *MainPage.xaml*.</span></span>

* <span data-ttu-id="efc10-165">Fügen Sie den nachstehend markierten Handler zum Laden der Seite und den Benutzerschnittstelleninhalt ein.</span><span class="sxs-lookup"><span data-stu-id="efc10-165">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="efc10-166">Fügen Sie jetzt Code zum Verknüpfen der Benutzeroberfläche mit der Datenbank hinzu.</span><span class="sxs-lookup"><span data-stu-id="efc10-166">Now add code to wire up the UI with the database</span></span>

* <span data-ttu-id="efc10-167">Öffnen Sie *MainPage.xaml.cs*.</span><span class="sxs-lookup"><span data-stu-id="efc10-167">Open *MainPage.xaml.cs*.</span></span>

* <span data-ttu-id="efc10-168">Fügen Sie den hervorgehobenen Code aus der folgenden Liste hinzu:</span><span class="sxs-lookup"><span data-stu-id="efc10-168">Add the highlighted code from the following listing:</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

<span data-ttu-id="efc10-169">Sie können die Anwendung jetzt ausführen, um sie in Aktion zu sehen.</span><span class="sxs-lookup"><span data-stu-id="efc10-169">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="efc10-170">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das *Blogging.UWP*-Projekt, und wählen Sie dann **Bereitstellen** aus.</span><span class="sxs-lookup"><span data-stu-id="efc10-170">In **Solution Explorer**, right-click the *Blogging.UWP* project and then select **Deploy**.</span></span>

* <span data-ttu-id="efc10-171">Legen Sie *Blogging.UWP* als Startprojekt fest.</span><span class="sxs-lookup"><span data-stu-id="efc10-171">Set *Blogging.UWP* as the startup project.</span></span>

* <span data-ttu-id="efc10-172">Klicken Sie auf **Debuggen > Ohne Debuggen starten**.</span><span class="sxs-lookup"><span data-stu-id="efc10-172">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="efc10-173">Die App wird erstellt und ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="efc10-173">The app builds and runs.</span></span>

* <span data-ttu-id="efc10-174">Geben Sie eine URL ein, und klicken Sie auf die Schaltfläche **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="efc10-174">Enter a URL and click the **Add** button</span></span>

  ![Bild](_static/create.png)

  ![Bild](_static/list.png)

  <span data-ttu-id="efc10-177">Voilá!</span><span class="sxs-lookup"><span data-stu-id="efc10-177">Tada!</span></span> <span data-ttu-id="efc10-178">Sie verfügen jetzt über eine einfache UWP-App, die Entity Framework Core ausführt.</span><span class="sxs-lookup"><span data-stu-id="efc10-178">You now have a simple UWP app running Entity Framework Core.</span></span>

## <a name="next-steps"></a><span data-ttu-id="efc10-179">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="efc10-179">Next steps</span></span>

<span data-ttu-id="efc10-180">Informationen zur Kompatibilität und Leistung, die Sie bei der Verwendung von EF Core mit UWP kennen sollten, finden Sie unter [.NET-Implementierungen, die von EF Core unterstützt werden](../../platforms/index.md#universal-windows-platform).</span><span class="sxs-lookup"><span data-stu-id="efc10-180">For compatibility and performance information that you should know when using EF Core with UWP, see [.NET implementations supported by EF Core](../../platforms/index.md#universal-windows-platform).</span></span>

<span data-ttu-id="efc10-181">Sehen Sie sich auch die weiteren Artikel in dieser Dokumentation an, um mehr über die Features von Entity Framework Core zu erfahren.</span><span class="sxs-lookup"><span data-stu-id="efc10-181">Check out other articles in this documentation to learn more about Entity Framework Core features.</span></span>
