---
title: Erste Schritte in ASP.NET Core – Neue Datenbank – EF Core
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: c6a86dd943dc7fe6f600455fe6743ea01a062aab
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996063"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="1c43a-102">Erste Schritte mit EF Core in ASP.NET Core mit einer neuen Datenbank</span><span class="sxs-lookup"><span data-stu-id="1c43a-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="1c43a-103">In diesem Tutorial entwickeln Sie eine ASP.NET Core MVC-Anwendung, die einen grundlegenden Datenzugriff mit Entity Framework Core durchführt.</span><span class="sxs-lookup"><span data-stu-id="1c43a-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="1c43a-104">Sie verwenden Migrationen, um die Datenbank aus Ihrem EF Core-Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="1c43a-104">You use migrations to create the database from your EF Core model.</span></span>

<span data-ttu-id="1c43a-105">[Sehen Sie sich das Beispiel aus diesem Artikel auf GitHub an](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).</span><span class="sxs-lookup"><span data-stu-id="1c43a-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1c43a-106">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="1c43a-106">Prerequisites</span></span>

<span data-ttu-id="1c43a-107">Installieren Sie die folgenden Softwarekomponenten:</span><span class="sxs-lookup"><span data-stu-id="1c43a-107">Install the following software:</span></span>

* <span data-ttu-id="1c43a-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) mit diesen Workloads:</span><span class="sxs-lookup"><span data-stu-id="1c43a-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="1c43a-109">**ASP.NET- und Webentwicklung** (unter **Web & Cloud**)</span><span class="sxs-lookup"><span data-stu-id="1c43a-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="1c43a-110">**Plattformübergreifende .NET Core-Entwicklung** (unter **Andere Toolsets**)</span><span class="sxs-lookup"><span data-stu-id="1c43a-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="1c43a-111">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core)</span><span class="sxs-lookup"><span data-stu-id="1c43a-111">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="create-a-new-project-in-visual-studio-2017"></a><span data-ttu-id="1c43a-112">Erstellen eines neuen Projekts in Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="1c43a-112">Create a new project in Visual Studio 2017</span></span>

* <span data-ttu-id="1c43a-113">Öffnen Sie Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="1c43a-113">Open Visual Studio 2017</span></span>
* <span data-ttu-id="1c43a-114">Klicken Sie auf **Datei > Neu > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="1c43a-114">**File > New > Project**</span></span>
* <span data-ttu-id="1c43a-115">Klicken Sie im Menü links auf **Installiert > Visual C# > .NET Core**.</span><span class="sxs-lookup"><span data-stu-id="1c43a-115">From the left menu select **Installed > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="1c43a-116">Wählen Sie **ASP.NET Core-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="1c43a-116">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="1c43a-117">Geben Sie als Name **EFGetStarted.AspNetCore.NewDb** ein, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="1c43a-117">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="1c43a-118">Gehen Sie im Dialogfeld **ASP.NET Core-Webanwendung** folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="1c43a-118">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="1c43a-119">Stellen Sie sicher, dass die Optionen **.NET Core** und **ASP.NET Core 2.1** in den Dropdownlisten ausgewählt sind.</span><span class="sxs-lookup"><span data-stu-id="1c43a-119">Ensure the options **.NET Core** and **ASP.NET Core 2.1** are selected in the drop down lists</span></span>
  * <span data-ttu-id="1c43a-120">Wählen Sie die Projektvorlage **Webanwendung (Model-View-Controller)** aus.</span><span class="sxs-lookup"><span data-stu-id="1c43a-120">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="1c43a-121">Stellen Sie sicher, dass **Authentifizierung** auf **Keine Authentifizierung** festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="1c43a-121">Ensure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="1c43a-122">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="1c43a-122">Click **OK**</span></span>

<span data-ttu-id="1c43a-123">Warnung: Wenn Sie für **Authentifizierung** anstelle von **Keine Authentifizierung** die Option **Einzelne Benutzerkonten** verwenden, wird Ihrem Projekt in `Models\IdentityModel.cs` ein Entity Framework Core-Modell hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="1c43a-123">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to your project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="1c43a-124">Anhand der Techniken, die Sie in diesem Tutorial kennenlernen, können Sie ein zweites Modell hinzufügen oder das vorhandene Modell um Ihre Entitätsklassen erweitern.</span><span class="sxs-lookup"><span data-stu-id="1c43a-124">Using the techniques you learn in this tutorial, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="1c43a-125">Installieren von Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="1c43a-125">Install Entity Framework Core</span></span>

<span data-ttu-id="1c43a-126">Installieren Sie das Paket für den (oder die) gewünschten EF Core-Datenbankanbieter, um EF Core zu installieren.</span><span class="sxs-lookup"><span data-stu-id="1c43a-126">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="1c43a-127">Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="1c43a-127">For a list of available providers see [Database Providers](../../providers/index.md).</span></span> 

<span data-ttu-id="1c43a-128">Für dieses Tutorial müssen Sie kein Anbieterpaket installieren, da das Tutorial SQL Server verwendet.</span><span class="sxs-lookup"><span data-stu-id="1c43a-128">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="1c43a-129">Das SQL Server-Anbieterpaket ist im Metapaket [Microsoft.AspnetCore.App](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1) enthalten.</span><span class="sxs-lookup"><span data-stu-id="1c43a-129">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

## <a name="create-the-model"></a><span data-ttu-id="1c43a-130">Erstellen des Modells</span><span class="sxs-lookup"><span data-stu-id="1c43a-130">Create the model</span></span>

<span data-ttu-id="1c43a-131">Definieren Sie eine Kontextklasse und Entitätsklassen für das Modell:</span><span class="sxs-lookup"><span data-stu-id="1c43a-131">Define a context class and entity classes that make up the model:</span></span>

* <span data-ttu-id="1c43a-132">Klicken Sie mit der rechten Maustaste auf den Ordner **Modelle**, und wählen Sie **Hinzufügen > Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="1c43a-132">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="1c43a-133">Geben Sie als Name **Model.cs** ein, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="1c43a-133">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="1c43a-134">Ersetzen Sie den Inhalt der Datei durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="1c43a-134">Replace the contents of the file with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

<span data-ttu-id="1c43a-135">In einer echten Anwendung würden Sie jede Klasse Ihres Modells typischerweise in einer separaten Datei platzieren.</span><span class="sxs-lookup"><span data-stu-id="1c43a-135">In a real app you would typically put each class from your model in a separate file.</span></span> <span data-ttu-id="1c43a-136">Der Einfachheit halber werden in diesem Tutorial alle Klassen in einer Datei zusammengefasst.</span><span class="sxs-lookup"><span data-stu-id="1c43a-136">For the sake of simplicity, this tutorial puts all the classes in one file.</span></span>

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="1c43a-137">Registrieren Ihres Kontexts durch Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="1c43a-137">Register your context with dependency injection</span></span>

<span data-ttu-id="1c43a-138">Dienste (z.B. `BloggingContext`) werden per [Abhängigkeitsinjektion](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) während des Anwendungsstarts registriert.</span><span class="sxs-lookup"><span data-stu-id="1c43a-138">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup.</span></span> <span data-ttu-id="1c43a-139">Komponenten, die diese Dienste benötigen (z.B. Ihre MVC-Controller), werden die Dienste anschließend über Konstruktorparameter oder Eigenschaften bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="1c43a-139">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span>

<span data-ttu-id="1c43a-140">Um eine `BloggingContext`-Klasse für MVC-Controller verfügbar zu machen, registrieren Sie sie als Dienst.</span><span class="sxs-lookup"><span data-stu-id="1c43a-140">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

* <span data-ttu-id="1c43a-141">Öffnen Sie **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="1c43a-141">Open **Startup.cs**</span></span>
* <span data-ttu-id="1c43a-142">Fügen Sie die folgenden `using` -Anweisungen hinzu:</span><span class="sxs-lookup"><span data-stu-id="1c43a-142">Add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

<span data-ttu-id="1c43a-143">Rufen Sie die `AddDbContext`-Methode auf, um den Kontext als Dienst zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="1c43a-143">Call the `AddDbContext` method to register the context as a service.</span></span>

* <span data-ttu-id="1c43a-144">Fügen Sie der `ConfigureServices`-Methode den folgenden hervorgehobenen Code zu:</span><span class="sxs-lookup"><span data-stu-id="1c43a-144">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=13-14)]

<span data-ttu-id="1c43a-145">Hinweis: In einer echten Anwendung wird die Verbindungszeichenfolge im Allgemeinen in eine Konfigurationsdatei oder Umgebungsvariable eingefügt.</span><span class="sxs-lookup"><span data-stu-id="1c43a-145">Note: A real app would generally put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="1c43a-146">Der Einfachheit halber wird sie in diesem Tutorial im Code definiert.</span><span class="sxs-lookup"><span data-stu-id="1c43a-146">For the sake of simplicity, this tutorial defines it in code.</span></span> <span data-ttu-id="1c43a-147">Weitere Informationen finden Sie in [Verbindungszeichenfolgen](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="1c43a-147">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="1c43a-148">Erstellen der Datenbank</span><span class="sxs-lookup"><span data-stu-id="1c43a-148">Create the database</span></span>

<span data-ttu-id="1c43a-149">Da Sie jetzt über ein Modell verfügen, können Sie mithilfe von [Migrationen](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) eine Datenbank erstellen.</span><span class="sxs-lookup"><span data-stu-id="1c43a-149">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="1c43a-150">Wählen Sie **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="1c43a-150">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="1c43a-151">Führen Sie `Add-Migration InitialCreate` aus, um per Gerüstbau eine Migration zum Erstellen des anfänglichen Tabellensatzes für Ihr Modell einzurichten.</span><span class="sxs-lookup"><span data-stu-id="1c43a-151">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for your model.</span></span> <span data-ttu-id="1c43a-152">Wenn Sie den Fehler `The term 'add-migration' is not recognized as the name of a cmdlet` erhalten, schließen Sie Visual Studio, und öffnen Sie es erneut.</span><span class="sxs-lookup"><span data-stu-id="1c43a-152">If you receive an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>
* <span data-ttu-id="1c43a-153">Führen Sie `Update-Database` aus, um die neue Migration auf die Datenbank anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="1c43a-153">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="1c43a-154">Mit diesem Befehl wird die Datenbank erstellt, bevor Migrationen angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="1c43a-154">This command creates the database before applying migrations.</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="1c43a-155">Erstellen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="1c43a-155">Create a controller</span></span>

<span data-ttu-id="1c43a-156">Erstellen Sie ein Gerüst für einen Controller und Ansichten für die `Blog`-Entität.</span><span class="sxs-lookup"><span data-stu-id="1c43a-156">Scaffold a controller and views for the `Blog` entity.</span></span>

* <span data-ttu-id="1c43a-157">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Controller**, und wählen Sie **Hinzufügen > Controller** aus.</span><span class="sxs-lookup"><span data-stu-id="1c43a-157">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="1c43a-158">Klicken Sie auf **MVC-Controller mit Ansichten unter Verwendung von Entity Framework** und danach auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="1c43a-158">Select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
* <span data-ttu-id="1c43a-159">Legen Sie **Modellklasse** auf **Blog** und **Datenkontextklasse** auf **BloggingContext** fest.</span><span class="sxs-lookup"><span data-stu-id="1c43a-159">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="1c43a-160">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="1c43a-160">Click **Add**.</span></span>


## <a name="run-the-application"></a><span data-ttu-id="1c43a-161">Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="1c43a-161">Run the application</span></span>

<span data-ttu-id="1c43a-162">Drücken Sie F5, um die App auszuführen und zu testen.</span><span class="sxs-lookup"><span data-stu-id="1c43a-162">Press F5 to run and test the app.</span></span>

* <span data-ttu-id="1c43a-163">Navigieren Sie zu `/Blogs`.</span><span class="sxs-lookup"><span data-stu-id="1c43a-163">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="1c43a-164">Verwenden Sie den Link zum Erstellen, um einige Blogeinträge zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="1c43a-164">Use the create link to create some blog entries.</span></span> <span data-ttu-id="1c43a-165">Testen Sie die Links zum Anzeigen von Details und zum Löschen.</span><span class="sxs-lookup"><span data-stu-id="1c43a-165">Test the details and delete links.</span></span>

![Bild](_static/create.png)

![Bild](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="1c43a-168">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="1c43a-168">Additional Resources</span></span>

* <span data-ttu-id="1c43a-169">[EF – Neue Datenbank mit SQLite](xref:core/get-started/netcore/new-db-sqlite): Ein EF-Tutorial für beliebige Plattformkonsolen.</span><span class="sxs-lookup"><span data-stu-id="1c43a-169">[EF - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="1c43a-170">Einführung in ASP.NET Core MVC unter Mac oder Linux</span><span class="sxs-lookup"><span data-stu-id="1c43a-170">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="1c43a-171">Einführung in ASP.NET Core MVC mit Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c43a-171">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="1c43a-172">Erste Schritte mit ASP.NET Core und Entity Framework Core mithilfe von Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c43a-172">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
