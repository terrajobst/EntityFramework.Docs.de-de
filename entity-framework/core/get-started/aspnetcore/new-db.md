---
title: "Erste Schritte in ASP.NET Core – Neue Datenbank – EF Core"
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 7e7ecaff29e9830bf3bcf742e6a5d54e1ced24de
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2017
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="3f979-102">Erste Schritte mit EF Core in ASP.NET Core mit einer neuen Datenbank</span><span class="sxs-lookup"><span data-stu-id="3f979-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="3f979-103">In dieser exemplarischen Vorgehensweise entwickeln Sie eine ASP.NET Core MVC-Anwendung, die einen grundlegenden Datenzugriff mit Entity Framework Core durchführt.</span><span class="sxs-lookup"><span data-stu-id="3f979-103">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="3f979-104">Sie verwenden Migrationen, um die Datenbank aus Ihrem EF Core-Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="3f979-104">You will use migrations to create the database from your EF Core model.</span></span> <span data-ttu-id="3f979-105">Weitere Entity Framework Core-Tutorials finden Sie unter [Zusätzliche Ressourcen](#additional-resources).</span><span class="sxs-lookup"><span data-stu-id="3f979-105">See [Additional Resources](#additional-resources) for more Entity Framework Core tutorials.</span></span>

<span data-ttu-id="3f979-106">Für dieses Tutorial gelten die folgenden Voraussetzungen:</span><span class="sxs-lookup"><span data-stu-id="3f979-106">This tutorial requires:</span></span>
* <span data-ttu-id="3f979-107">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) mit diesen Workloads:</span><span class="sxs-lookup"><span data-stu-id="3f979-107">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="3f979-108">**ASP.NET- und Webentwicklung** (unter **Web & Cloud**)</span><span class="sxs-lookup"><span data-stu-id="3f979-108">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="3f979-109">**Plattformübergreifende .NET Core-Entwicklung** (unter **Andere Toolsets**)</span><span class="sxs-lookup"><span data-stu-id="3f979-109">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="3f979-110">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="3f979-110">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>

> [!TIP]  
> <span data-ttu-id="3f979-111">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="3f979-111">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb) on GitHub.</span></span>

## <a name="create-a-new-project-in-visual-studio-2017"></a><span data-ttu-id="3f979-112">Erstellen eines neuen Projekts in Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="3f979-112">Create a new project in Visual Studio 2017</span></span>

* <span data-ttu-id="3f979-113">Klicken Sie auf **Datei > Neu > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="3f979-113">**File > New > Project**</span></span>
* <span data-ttu-id="3f979-114">Wählen Sie im Menü links **Installiert > Vorlagen > Visual C# > .NET Core** aus.</span><span class="sxs-lookup"><span data-stu-id="3f979-114">From the left menu select **Installed > Templates > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="3f979-115">Wählen Sie **ASP.NET Core-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="3f979-115">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="3f979-116">Geben Sie als Name **EFGetStarted.AspNetCore.NewDb** ein, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="3f979-116">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="3f979-117">Gehen Sie im Dialogfeld **ASP.NET Core-Webanwendung** folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="3f979-117">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="3f979-118">Stellen Sie sicher, dass die Optionen **.NET Core** und **ASP.NET Core 2.0** in den Dropdownlisten ausgewählt sind.</span><span class="sxs-lookup"><span data-stu-id="3f979-118">Ensure the options **.NET Core** and **ASP.NET Core 2.0** are selected in the drop down lists</span></span>
  * <span data-ttu-id="3f979-119">Wählen Sie die Projektvorlage **Webanwendung (Model-View-Controller)** aus.</span><span class="sxs-lookup"><span data-stu-id="3f979-119">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="3f979-120">Stellen Sie sicher, dass **Authentifizierung** auf **Keine Authentifizierung** festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="3f979-120">Ensure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="3f979-121">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="3f979-121">Click **OK**</span></span>

<span data-ttu-id="3f979-122">Warnung: Wenn Sie für **Authentifizierung** anstelle von **Keine Authentifizierung** die Option **Einzelne Benutzerkonten** verwenden, wird Ihrem Projekt in `Models\IdentityModel.cs` ein Entity Framework Core-Modell hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="3f979-122">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to your project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="3f979-123">Anhand der Techniken, die Sie in dieser exemplarischen Vorgehensweise kennenlernen, können Sie ein zweites Model hinzufügen oder das vorhandene Modell um Ihre Entitätsklassen erweitern.</span><span class="sxs-lookup"><span data-stu-id="3f979-123">Using the techniques you will learn in this walkthrough, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="3f979-124">Installieren von Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="3f979-124">Install Entity Framework Core</span></span>

<span data-ttu-id="3f979-125">Installieren Sie das Paket für den (oder die) gewünschten EF Core-Datenbankanbieter.</span><span class="sxs-lookup"><span data-stu-id="3f979-125">Install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="3f979-126">In dieser exemplarischen Vorgehensweise wird SQL Server verwendet.</span><span class="sxs-lookup"><span data-stu-id="3f979-126">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="3f979-127">Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="3f979-127">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="3f979-128">Wählen Sie **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="3f979-128">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="3f979-129">Führen Sie `Install-Package Microsoft.EntityFrameworkCore.SqlServer` aus.</span><span class="sxs-lookup"><span data-stu-id="3f979-129">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="3f979-130">Wir werden einige Entity Framework Core-Tools verwenden, um eine Datenbank aus Ihrem EF Core-Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="3f979-130">We will be using some Entity Framework Core Tools to create a database from your EF Core model.</span></span> <span data-ttu-id="3f979-131">Deshalb installieren wir auch das Toolpaket:</span><span class="sxs-lookup"><span data-stu-id="3f979-131">So we will install the tools package as well:</span></span>

* <span data-ttu-id="3f979-132">Führen Sie `Install-Package Microsoft.EntityFrameworkCore.Tools` aus.</span><span class="sxs-lookup"><span data-stu-id="3f979-132">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="3f979-133">Später verwenden wir einige ASP.NET Core-Gerüsttools, um Controller und Ansichten zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="3f979-133">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="3f979-134">Deshalb installieren wird auch dieses Entwurfspaket:</span><span class="sxs-lookup"><span data-stu-id="3f979-134">So we will install this design package as well:</span></span>

* <span data-ttu-id="3f979-135">Führen Sie `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design` aus.</span><span class="sxs-lookup"><span data-stu-id="3f979-135">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="3f979-136">Erstellen des Modells</span><span class="sxs-lookup"><span data-stu-id="3f979-136">Create the model</span></span>

<span data-ttu-id="3f979-137">Definieren Sie einen Kontext und Entitätsklassen für das Modell:</span><span class="sxs-lookup"><span data-stu-id="3f979-137">Define a context and entity classes that make up the model:</span></span>

* <span data-ttu-id="3f979-138">Klicken Sie mit der rechten Maustaste auf den Ordner **Modelle**, und wählen Sie **Hinzufügen > Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="3f979-138">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="3f979-139">Geben Sie als Name **Model.cs** ein, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="3f979-139">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="3f979-140">Ersetzen Sie den Inhalt der Datei durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="3f979-140">Replace the contents of the file with the following code:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

<span data-ttu-id="3f979-141">Hinweis: In einer echten Anwendung würden Sie jede Klasse Ihres Modells typischerweise in einer separaten Datei platzieren.</span><span class="sxs-lookup"><span data-stu-id="3f979-141">Note: In a real app you would typically put each class from your model in a separate file.</span></span> <span data-ttu-id="3f979-142">Der Einfachheit halber werden in diesem Tutorial alle Klassen in einer Datei zusammengefasst.</span><span class="sxs-lookup"><span data-stu-id="3f979-142">For the sake of simplicity, we are putting all the classes in one file for this tutorial.</span></span>

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="3f979-143">Registrieren Ihres Kontexts durch Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="3f979-143">Register your context with dependency injection</span></span>

<span data-ttu-id="3f979-144">Dienste (z.B. `BloggingContext`) werden per [Abhängigkeitsinjektion](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) während des Anwendungsstarts registriert.</span><span class="sxs-lookup"><span data-stu-id="3f979-144">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup.</span></span> <span data-ttu-id="3f979-145">Komponenten, die diese Dienste benötigen (z.B. Ihre MVC-Controller), werden die Dienste anschließend über Konstruktorparameter oder Eigenschaften bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="3f979-145">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span>

<span data-ttu-id="3f979-146">Damit unsere MVC-Controller `BloggingContext` nutzen können, werden wir sie als Dienst registrieren.</span><span class="sxs-lookup"><span data-stu-id="3f979-146">In order for our MVC controllers to make use of `BloggingContext` we will register it as a service.</span></span>

* <span data-ttu-id="3f979-147">Öffnen Sie **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="3f979-147">Open **Startup.cs**</span></span>
* <span data-ttu-id="3f979-148">Fügen Sie die folgenden `using` -Anweisungen hinzu:</span><span class="sxs-lookup"><span data-stu-id="3f979-148">Add the following `using` statements:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

<span data-ttu-id="3f979-149">Fügen Sie die `AddDbContext`-Methode hinzu, um sie als Dienst zu registrieren:</span><span class="sxs-lookup"><span data-stu-id="3f979-149">Add the `AddDbContext` method to register it as a service:</span></span>

* <span data-ttu-id="3f979-150">Fügen Sie der `ConfigureServices`-Methode den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="3f979-150">Add the following code to the `ConfigureServices` method:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

<span data-ttu-id="3f979-151">Hinweis: In einer echten Anwendung wird die Verbindungszeichenfolge im Allgemeinen in eine Konfigurationsdatei eingefügt.</span><span class="sxs-lookup"><span data-stu-id="3f979-151">Note: A real app would gennerally put the connection string in a configuration file.</span></span> <span data-ttu-id="3f979-152">Der Einfachheit halber definieren wir sie hier im Code.</span><span class="sxs-lookup"><span data-stu-id="3f979-152">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="3f979-153">Weitere Informationen finden Sie in [Verbindungszeichenfolgen](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="3f979-153">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-your-database"></a><span data-ttu-id="3f979-154">Erstellen Ihrer Datenbank</span><span class="sxs-lookup"><span data-stu-id="3f979-154">Create your database</span></span>

<span data-ttu-id="3f979-155">Da Sie jetzt über ein Modell verfügen, können Sie mithilfe von [Migrationen](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) eine Datenbank erstellen.</span><span class="sxs-lookup"><span data-stu-id="3f979-155">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="3f979-156">Öffnen Sie die Paket-Manager-Konsole:</span><span class="sxs-lookup"><span data-stu-id="3f979-156">Open the PMC:</span></span>

  <span data-ttu-id="3f979-157">Wählen Sie **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="3f979-157">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="3f979-158">Führen Sie `Add-Migration InitialCreate` aus, um per Gerüstbau eine Migration zum Erstellen des anfänglichen Tabellensatzes für Ihr Modell einzurichten.</span><span class="sxs-lookup"><span data-stu-id="3f979-158">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for your model.</span></span> <span data-ttu-id="3f979-159">Wenn Sie den Fehler `The term 'add-migration' is not recognized as the name of a cmdlet` erhalten, schließen Sie Visual Studio, und öffnen Sie es erneut.</span><span class="sxs-lookup"><span data-stu-id="3f979-159">If you receive an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>
* <span data-ttu-id="3f979-160">Führen Sie `Update-Database` aus, um die neue Migration auf die Datenbank anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="3f979-160">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="3f979-161">Mit diesem Befehl wird die Datenbank erstellt, bevor Migrationen angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="3f979-161">This command creates the database before applying migrations.</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="3f979-162">Erstellen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="3f979-162">Create a controller</span></span>

<span data-ttu-id="3f979-163">Aktivieren Sie den Gerüstbau im Projekt:</span><span class="sxs-lookup"><span data-stu-id="3f979-163">Enable scaffolding in the project:</span></span>

* <span data-ttu-id="3f979-164">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Controller**, und wählen Sie **Hinzufügen > Controller** aus.</span><span class="sxs-lookup"><span data-stu-id="3f979-164">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="3f979-165">Wählen Sie **Mindestens erforderliche Abhängigkeiten** aus, und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="3f979-165">Select **Minimal Dependencies** and click **Add**.</span></span>
* <span data-ttu-id="3f979-166">Sie können die Datei *ScaffoldingReadMe.txt* ignorieren oder löschen.</span><span class="sxs-lookup"><span data-stu-id="3f979-166">You can ignore or delete the *ScaffoldingReadMe.txt* file.</span></span>

<span data-ttu-id="3f979-167">Nach der Aktivierung des Gerüstbaus können Sie einen Controller für die `Blog`-Entität einrichten.</span><span class="sxs-lookup"><span data-stu-id="3f979-167">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="3f979-168">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Controller**, und wählen Sie **Hinzufügen > Controller** aus.</span><span class="sxs-lookup"><span data-stu-id="3f979-168">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="3f979-169">Wählen Sie **MVC-Controller mit Ansichten unter Verwendung von Entity Framework** aus, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="3f979-169">Select **MVC Controller with views, using Entity Framework** and click **Ok**.</span></span>
* <span data-ttu-id="3f979-170">Legen Sie **Modellklasse** auf **Blog** und **Datenkontextklasse** auf **BloggingContext** fest.</span><span class="sxs-lookup"><span data-stu-id="3f979-170">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="3f979-171">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="3f979-171">Click **Add**.</span></span>


## <a name="run-the-application"></a><span data-ttu-id="3f979-172">Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="3f979-172">Run the application</span></span>

<span data-ttu-id="3f979-173">Drücken Sie F5, um die App auszuführen und zu testen.</span><span class="sxs-lookup"><span data-stu-id="3f979-173">Press F5 to run and test the app.</span></span>

* <span data-ttu-id="3f979-174">Navigieren Sie zu `/Blogs`.</span><span class="sxs-lookup"><span data-stu-id="3f979-174">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="3f979-175">Verwenden Sie den Link zum Erstellen, um einige Blogeinträge zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="3f979-175">Use the create link to create some blog entries.</span></span> <span data-ttu-id="3f979-176">Testen Sie die Links zum Anzeigen von Details und zum Löschen.</span><span class="sxs-lookup"><span data-stu-id="3f979-176">Test the details and delete links.</span></span>

![image](_static/create.png)

![image](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="3f979-179">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="3f979-179">Additional Resources</span></span>

* <span data-ttu-id="3f979-180">[EF – Neue Datenbank mit SQLite](xref:core/get-started/netcore/new-db-sqlite): Ein EF-Tutorial für beliebige Plattformkonsolen.</span><span class="sxs-lookup"><span data-stu-id="3f979-180">[EF - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="3f979-181">Einführung in ASP.NET Core MVC unter Mac oder Linux</span><span class="sxs-lookup"><span data-stu-id="3f979-181">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="3f979-182">Einführung in ASP.NET Core MVC mit Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3f979-182">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="3f979-183">Erste Schritte mit ASP.NET Core und Entity Framework Core mithilfe von Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3f979-183">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
