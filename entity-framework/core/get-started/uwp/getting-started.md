---
title: "Erste Schritte in UWP – Neue Datenbank – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.topic: get-started-article
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
ms.technology: entity-framework-core
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: f743ff5392d1f30283a13d2e7fb8029be88387aa
ms.sourcegitcommit: 96324e58c02b97277395ed43173bf13ac80d2012
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2017
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a><span data-ttu-id="3f421-102">Erste Schritte mit EF Core in UWP (Universelle Windows-Plattform) mit einer neuen Datenbank</span><span class="sxs-lookup"><span data-stu-id="3f421-102">Getting Started with EF Core on Universal Windows Platform (UWP) with a New Database</span></span>

> [!NOTE]
> <span data-ttu-id="3f421-103">In diesem Tutorial wird EF Core 2.0.1 verwendet (zusammen mit ASP.NET Core und .NET Core SDK 2.0.3 veröffentlicht).</span><span class="sxs-lookup"><span data-stu-id="3f421-103">This tutorial uses EF Core 2.0.1 (released alongside ASP.NET Core and .NET Core SDK 2.0.3).</span></span> <span data-ttu-id="3f421-104">In EF Core 2.0.0 fehlen einige wichtige Bugfixes, die für eine optimale UWP-Nutzung nötig sind.</span><span class="sxs-lookup"><span data-stu-id="3f421-104">EF Core 2.0.0 lacks some crucial bug fixes required for a good UWP experience.</span></span>

<span data-ttu-id="3f421-105">In dieser exemplarischen Vorgehensweise entwickeln Sie eine UWP-Anwendung, die einen grundlegenden Datenzugriff auf eine lokale SQLite-Datenbank mithilfe von Entity Framework durchführt.</span><span class="sxs-lookup"><span data-stu-id="3f421-105">In this walkthrough, you will build a Universal Windows Platform (UWP) application that performs basic data access against a local SQLite database using Entity Framework.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3f421-106">**Vermeiden Sie die Verwendung anonymer Typen in LINQ-Abfragen in UWP**.</span><span class="sxs-lookup"><span data-stu-id="3f421-106">**Consider avoiding anonymous types in LINQ queries on UWP**.</span></span> <span data-ttu-id="3f421-107">Das Bereitstellen einer UWP-Anwendung im App Store erfordert eine Kompilierung Ihrer Anwendung mit .NET Native.</span><span class="sxs-lookup"><span data-stu-id="3f421-107">Deploying a UWP application to the app store requires your application to be compiled with .NET Native.</span></span> <span data-ttu-id="3f421-108">Abfragen mit anonymen Typen zeigen in .NET Native eine unzureichende Leistung.</span><span class="sxs-lookup"><span data-stu-id="3f421-108">Queries with anonymous types have worse performance on .NET Native.</span></span>

> [!TIP]
> <span data-ttu-id="3f421-109">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="3f421-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f421-110">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="3f421-110">Prerequisites</span></span>

<span data-ttu-id="3f421-111">Für diese exemplarische Vorgehensweise wird Folgendes benötigt:</span><span class="sxs-lookup"><span data-stu-id="3f421-111">The following items are required to complete this walkthrough:</span></span>

* <span data-ttu-id="3f421-112">[Windows 10 Fall Creators Update](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0)</span><span class="sxs-lookup"><span data-stu-id="3f421-112">[Windows 10 Fall Creators Update](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0)</span></span>

* <span data-ttu-id="3f421-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) oder höher</span><span class="sxs-lookup"><span data-stu-id="3f421-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>

* <span data-ttu-id="3f421-114">[Visual Studio 2017](https://www.visualstudio.com/downloads/), Version 15.4 oder höher, mit der Workload für die **UWP-Entwicklung**.</span><span class="sxs-lookup"><span data-stu-id="3f421-114">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.4 or later with the **Universal Windows Platform Development** workload.</span></span>

## <a name="create-a-new-model-project"></a><span data-ttu-id="3f421-115">Erstellen eines neuen Modellprojekts</span><span class="sxs-lookup"><span data-stu-id="3f421-115">Create a new model project</span></span>

> [!WARNING]
> <span data-ttu-id="3f421-116">Aufgrund von Einschränkungen in der Interaktion zwischen .NET Core-Tools mit UWP-Projekten muss das Modell in einem Nicht-UWP-Projekt platziert werden, um Migrationsbefehle in der Paket-Manager-Konsole ausführen zu können.</span><span class="sxs-lookup"><span data-stu-id="3f421-116">Due to limitations in the way .NET Core tools interact with UWP projects the model needs to be placed in a non-UWP project to be able to run migrations commands in the Package Manager Console</span></span>

* <span data-ttu-id="3f421-117">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3f421-117">Open Visual Studio</span></span>

* <span data-ttu-id="3f421-118">Klicken Sie auf „Datei“ > „Neu“ > „Projekt“.</span><span class="sxs-lookup"><span data-stu-id="3f421-118">File > New > Project...</span></span>

* <span data-ttu-id="3f421-119">Wählen Sie im Menü links „Vorlagen“ > „Visual C#“ aus.</span><span class="sxs-lookup"><span data-stu-id="3f421-119">From the left menu select Templates > Visual C#</span></span>

* <span data-ttu-id="3f421-120">Wählen Sie die Vorlage **Klassenbibliothek (.NET Standard)** aus.</span><span class="sxs-lookup"><span data-stu-id="3f421-120">Select the **Class Library (.NET Standard)** project template</span></span>

* <span data-ttu-id="3f421-121">Benennen Sie das Projekt, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="3f421-121">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="3f421-122">Installieren von Entity Framework</span><span class="sxs-lookup"><span data-stu-id="3f421-122">Install Entity Framework</span></span>

<span data-ttu-id="3f421-123">Installieren Sie zur Verwendung von EF Core das Paket für den (oder die) gewünschten Datenbankanbieter.</span><span class="sxs-lookup"><span data-stu-id="3f421-123">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="3f421-124">In dieser exemplarischen Vorgehensweise wird SQLite verwendet.</span><span class="sxs-lookup"><span data-stu-id="3f421-124">This walkthrough uses SQLite.</span></span> <span data-ttu-id="3f421-125">Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="3f421-125">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="3f421-126">Wählen Sie „Tools“ > „NuGet-Paket-Manager“ > „Paket-Manager-Konsole“ aus.</span><span class="sxs-lookup"><span data-stu-id="3f421-126">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="3f421-127">Führen Sie `Install-Package Microsoft.EntityFrameworkCore.Sqlite` aus.</span><span class="sxs-lookup"><span data-stu-id="3f421-127">Run `Install-Package Microsoft.EntityFrameworkCore.Sqlite`</span></span>

<span data-ttu-id="3f421-128">Später in dieser exemplarischen Vorgehensweise werden einige Entity Framework-Tools zum Verwalten der Datenbank eingesetzt.</span><span class="sxs-lookup"><span data-stu-id="3f421-128">Later in this walkthrough we will also be using some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="3f421-129">Deshalb installieren wir auch das Toolpaket.</span><span class="sxs-lookup"><span data-stu-id="3f421-129">So we will install the tools package as well.</span></span>

* <span data-ttu-id="3f421-130">Führen Sie `Install-Package Microsoft.EntityFrameworkCore.Tools` aus.</span><span class="sxs-lookup"><span data-stu-id="3f421-130">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

* <span data-ttu-id="3f421-131">Bearbeiten Sie die CSPROJ-Datei, und ersetzen Sie `<TargetFramework>netstandard2.0</TargetFramework>` durch `<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`.</span><span class="sxs-lookup"><span data-stu-id="3f421-131">Edit the .csproj file and replace `<TargetFramework>netstandard2.0</TargetFramework>` with `<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`</span></span>

## <a name="create-your-model"></a><span data-ttu-id="3f421-132">Erstellen Ihres Modells</span><span class="sxs-lookup"><span data-stu-id="3f421-132">Create your model</span></span>

<span data-ttu-id="3f421-133">Jetzt ist es Zeit, einen Kontext und Entitätsklassen für Ihr Modell zu definieren.</span><span class="sxs-lookup"><span data-stu-id="3f421-133">Now it's time to define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="3f421-134">Klicken Sie auf „Projekt“ > „Klasse hinzufügen“.</span><span class="sxs-lookup"><span data-stu-id="3f421-134">Project > Add Class...</span></span>

* <span data-ttu-id="3f421-135">Geben Sie als Name *Model.cs* ein, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="3f421-135">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="3f421-136">Ersetzen Sie den Inhalt der Datei durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="3f421-136">Replace the contents of the file with the following code</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a><span data-ttu-id="3f421-137">Erstellen eines neuen UWP-Projekts</span><span class="sxs-lookup"><span data-stu-id="3f421-137">Create a new UWP project</span></span>

* <span data-ttu-id="3f421-138">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3f421-138">Open Visual Studio</span></span>

* <span data-ttu-id="3f421-139">Klicken Sie auf „Datei“ > „Neu“ > „Projekt“.</span><span class="sxs-lookup"><span data-stu-id="3f421-139">File > New > Project...</span></span>

* <span data-ttu-id="3f421-140">Wählen Sie im Menü links „Vorlagen“ > „Visual C#“ > „Windows Universal“ aus.</span><span class="sxs-lookup"><span data-stu-id="3f421-140">From the left menu select Templates > Visual C# > Windows Universal</span></span>

* <span data-ttu-id="3f421-141">Wählen Sie die Projektvorlage **Leere App (universelles Windows)** aus.</span><span class="sxs-lookup"><span data-stu-id="3f421-141">Select the **Blank App (Universal Windows)** project template</span></span>

* <span data-ttu-id="3f421-142">Benennen Sie das Projekt, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="3f421-142">Give the project a name and click **OK**</span></span>

* <span data-ttu-id="3f421-143">Legen Sie die Ziel- und Mindestversionen mindestens auf `Windows 10 Fall Creators Update (10.0; build 16299.0)` fest.</span><span class="sxs-lookup"><span data-stu-id="3f421-143">Set the target and minimum versions to at least `Windows 10 Fall Creators Update (10.0; build 16299.0)`</span></span>

## <a name="create-your-database"></a><span data-ttu-id="3f421-144">Erstellen Ihrer Datenbank</span><span class="sxs-lookup"><span data-stu-id="3f421-144">Create your database</span></span>

<span data-ttu-id="3f421-145">Nachdem Sie jetzt über ein Modell verfügen, können Sie mithilfe von Migrationen eine Datenbank erstellen.</span><span class="sxs-lookup"><span data-stu-id="3f421-145">Now that you have a model, you can use migrations to create a database for you.</span></span>

* <span data-ttu-id="3f421-146">Wählen Sie „Tools“ > „NuGet-Paket-Manager“ > „Paket-Manager-Konsole“ aus.</span><span class="sxs-lookup"><span data-stu-id="3f421-146">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="3f421-147">Wählen Sie das Modellprojekt als Standardprojekt aus, und legen Sie es als Startprojekt fest.</span><span class="sxs-lookup"><span data-stu-id="3f421-147">Select the model project as the Default project and set it as the startup project</span></span>

* <span data-ttu-id="3f421-148">Führen Sie `Add-Migration MyFirstMigration` aus, um per Gerüstbau eine Migration zum Erstellen des anfänglichen Tabellensatzes für Ihr Modell einzurichten.</span><span class="sxs-lookup"><span data-stu-id="3f421-148">Run `Add-Migration MyFirstMigration` to scaffold a migration to create the initial set of tables for your model.</span></span>

<span data-ttu-id="3f421-149">Weil die Datenbank auf dem Gerät erstellt werden soll, auf der die App ausgeführt wird, fügen wir Code hinzu, mit dem beim Anwendungsstart ausstehende Migrationen auf die lokale Datenbank angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="3f421-149">Since we want the database to be created on the device that the app runs on, we will add some code to apply any pending migrations to the local database on application startup.</span></span> <span data-ttu-id="3f421-150">Bei der ersten App-Ausführung wird hierbei die lokale Datenbank für uns erstellt.</span><span class="sxs-lookup"><span data-stu-id="3f421-150">The first time that the app runs, this will take care of creating the local database for us.</span></span>

* <span data-ttu-id="3f421-151">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **App.xaml**, und wählen Sie **Code anzeigen** aus.</span><span class="sxs-lookup"><span data-stu-id="3f421-151">Right-click on **App.xaml** in **Solution Explorer** and select **View Code**</span></span>

* <span data-ttu-id="3f421-152">Fügen Sie den hervorgehobenen using-Code am Anfang der Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="3f421-152">Add the highlighted using to the start of the file</span></span>

* <span data-ttu-id="3f421-153">Fügen Sie den hervorgehobenen Code hinzu, um ausstehende Migrationen anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="3f421-153">Add the highlighted code to apply any pending migrations</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/App.xaml.cs?highlight=1,25-28)]

> [!TIP]  
> <span data-ttu-id="3f421-154">Wenn Sie später Änderungen an Ihrem Modell durchführen, können Sie mit dem Befehl `Add-Migration` per Gerüstbau eine neue Migration einrichten, um die entsprechenden Schemaänderungen auf die Datenbank anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="3f421-154">If you make future changes to your model, you can use the `Add-Migration` command to scaffold a new migration to apply the corresponding changes to the database.</span></span> <span data-ttu-id="3f421-155">Alle ausstehenden Migrationen werden beim Start der Anwendung auf die lokale Datenbank auf jedem Gerät angewendet.</span><span class="sxs-lookup"><span data-stu-id="3f421-155">Any pending migrations will be applied to the local database on each device when the application starts.</span></span>
>
><span data-ttu-id="3f421-156">EF verwendet eine `__EFMigrationsHistory`-Tabelle in der Datenbank, um nachzuverfolgen, welche Migrationen bereits auf die Datenbank angewendet wurden.</span><span class="sxs-lookup"><span data-stu-id="3f421-156">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="3f421-157">Verwenden Ihres Modells</span><span class="sxs-lookup"><span data-stu-id="3f421-157">Use your model</span></span>

<span data-ttu-id="3f421-158">Jetzt können Sie Ihr Modell zum Zugreifen auf Daten verwenden.</span><span class="sxs-lookup"><span data-stu-id="3f421-158">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="3f421-159">Öffnen Sie *MainPage.xaml*.</span><span class="sxs-lookup"><span data-stu-id="3f421-159">Open *MainPage.xaml*</span></span>

* <span data-ttu-id="3f421-160">Fügen Sie den nachstehend markierten Handler zum Laden der Seite und den Benutzerschnittstelleninhalt ein.</span><span class="sxs-lookup"><span data-stu-id="3f421-160">Add the page load handler and UI content highlighted below</span></span>

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml?highlight=9,11-23)]

<span data-ttu-id="3f421-161">Jetzt fügen wir Code zum Verknüpfen der Benutzerschnittstelle mit der Datenbank ein.</span><span class="sxs-lookup"><span data-stu-id="3f421-161">Now we'll add code to wire up the UI with the database</span></span>

* <span data-ttu-id="3f421-162">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **MainPage.xaml**, und wählen Sie **Code anzeigen** aus.</span><span class="sxs-lookup"><span data-stu-id="3f421-162">Right-click **MainPage.xaml** in **Solution Explorer** and select **View Code**</span></span>

* <span data-ttu-id="3f421-163">Fügen Sie den hervorgehobenen Code aus dem folgenden Listing hinzu.</span><span class="sxs-lookup"><span data-stu-id="3f421-163">Add the highlighted code from the following listing</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml.cs?highlight=30-48)]

<span data-ttu-id="3f421-164">Sie können die Anwendung jetzt ausführen, um sie in Aktion zu sehen.</span><span class="sxs-lookup"><span data-stu-id="3f421-164">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="3f421-165">Klicken Sie auf „Debuggen“ > „Ohne Debuggen starten“.</span><span class="sxs-lookup"><span data-stu-id="3f421-165">Debug > Start Without Debugging</span></span>

* <span data-ttu-id="3f421-166">Die Anwendung wird erstellt und gestartet.</span><span class="sxs-lookup"><span data-stu-id="3f421-166">The application will build and launch</span></span>

* <span data-ttu-id="3f421-167">Geben Sie eine URL ein, und klicken Sie auf die Schaltfläche **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="3f421-167">Enter a URL and click the **Add** button</span></span>

![image](_static/create.png)

![image](_static/list.png)

## <a name="next-steps"></a><span data-ttu-id="3f421-170">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="3f421-170">Next steps</span></span>

> [!TIP]
> <span data-ttu-id="3f421-171">Die Leistung von `SaveChanges()` kann durch das Implementieren von [`INotifyPropertyChanged`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [`INotifyPropertyChanging`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx), [`INotifyCollectionChanged`](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) in Ihren Entitätstypen und durch Verwendung von`ChangeTrackingStrategy.ChangingAndChangedNotifications` verbessert werden.</span><span class="sxs-lookup"><span data-stu-id="3f421-171">`SaveChanges()` performance can be improved by implementing [`INotifyPropertyChanged`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [`INotifyPropertyChanging`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx), [`INotifyCollectionChanged`](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) in your entity types and using `ChangeTrackingStrategy.ChangingAndChangedNotifications`.</span></span>

<span data-ttu-id="3f421-172">Voilá!</span><span class="sxs-lookup"><span data-stu-id="3f421-172">Tada!</span></span> <span data-ttu-id="3f421-173">Sie verfügen jetzt über eine einfache UWP-App mit Ausführung von Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3f421-173">You now have a simple UWP app running Entity Framework.</span></span>

<span data-ttu-id="3f421-174">Sehen Sie sich auch die weiteren Artikel in dieser Dokumentation an, um mehr über die Features von Entity Framework zu erfahren.</span><span class="sxs-lookup"><span data-stu-id="3f421-174">Check out other articles in this documentation to learn more about Entity Framework's features.</span></span>
