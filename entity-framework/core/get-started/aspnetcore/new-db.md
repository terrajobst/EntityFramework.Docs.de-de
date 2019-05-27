---
title: Erste Schritte in ASP.NET Core – Neue Datenbank – EF Core
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: fbc1a00d6d6d0624bcbbfa1e51f4e21a915baaaa
ms.sourcegitcommit: f277883a5ed28eba57d14aaaf17405bc1ae9cf94
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/18/2019
ms.locfileid: "65874566"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a><span data-ttu-id="e3f7b-102">Erste Schritte mit EF Core in ASP.NET Core mit einer neuen Datenbank</span><span class="sxs-lookup"><span data-stu-id="e3f7b-102">Getting Started with EF Core on ASP.NET Core with a New database</span></span>

<span data-ttu-id="e3f7b-103">In diesem Tutorial entwickeln Sie eine ASP.NET Core MVC-Anwendung, die einen grundlegenden Datenzugriff mit Entity Framework Core durchführt.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="e3f7b-104">Dieses Tutorial verwendet Migrationen, um die Datenbank aus dem Datenmodell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-104">The tutorial uses migrations to create the database from the data model.</span></span>

<span data-ttu-id="e3f7b-105">Sie können das Tutorial mit Visual Studio 2017 für Windows oder mithilfe von .NET Core-CLI unter Windows, MacOS oder Linux nachvollziehen.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-105">You can follow the tutorial by using Visual Studio 2017 on Windows, or by using the .NET Core CLI on Windows, macOS, or Linux.</span></span>

<span data-ttu-id="e3f7b-106">Beispiel aus diesem Artikel auf GitHub anzeigen:</span><span class="sxs-lookup"><span data-stu-id="e3f7b-106">View this article's sample on GitHub:</span></span>
* [<span data-ttu-id="e3f7b-107">Visual Studio 2017 mit SQLServer</span><span class="sxs-lookup"><span data-stu-id="e3f7b-107">Visual Studio 2017 with SQL Server</span></span>](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb)
* <span data-ttu-id="e3f7b-108">[.NET Core-CLI mit SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).</span><span class="sxs-lookup"><span data-stu-id="e3f7b-108">[.NET Core CLI with SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e3f7b-109">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="e3f7b-109">Prerequisites</span></span>

<span data-ttu-id="e3f7b-110">Installieren Sie die folgenden Softwarekomponenten:</span><span class="sxs-lookup"><span data-stu-id="e3f7b-110">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e3f7b-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e3f7b-111">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e3f7b-112">[Visual Studio 2017, Version 15.7 oder höher](https://www.visualstudio.com/downloads/), mit folgenden Workloads:</span><span class="sxs-lookup"><span data-stu-id="e3f7b-112">[Visual Studio 2017 version 15.7 or later](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="e3f7b-113">**ASP.NET- und Webentwicklung** (unter **Web & Cloud**)</span><span class="sxs-lookup"><span data-stu-id="e3f7b-113">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="e3f7b-114">**Plattformübergreifende .NET Core-Entwicklung** (unter **Andere Toolsets**)</span><span class="sxs-lookup"><span data-stu-id="e3f7b-114">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="e3f7b-115">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core)</span><span class="sxs-lookup"><span data-stu-id="e3f7b-115">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e3f7b-116">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="e3f7b-116">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="e3f7b-117">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core)</span><span class="sxs-lookup"><span data-stu-id="e3f7b-117">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

---

## <a name="create-a-new-project"></a><span data-ttu-id="e3f7b-118">Erstellt ein neues Projekt</span><span class="sxs-lookup"><span data-stu-id="e3f7b-118">Create a new project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e3f7b-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e3f7b-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e3f7b-120">Öffnen Sie Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-120">Open Visual Studio 2017</span></span>
* <span data-ttu-id="e3f7b-121">Klicken Sie auf **Datei > Neu > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-121">**File > New > Project**</span></span>
* <span data-ttu-id="e3f7b-122">Wählen Sie im Menü links **Installiert > Visual C# > .NET Core** aus.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-122">From the left menu, select **Installed > Visual C# > .NET Core**.</span></span>
* <span data-ttu-id="e3f7b-123">Wählen Sie **ASP.NET Core-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-123">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="e3f7b-124">Geben Sie als Name **EFGetStarted.AspNetCore.NewDb** ein, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-124">Enter **EFGetStarted.AspNetCore.NewDb** for the name and click **OK**.</span></span>
* <span data-ttu-id="e3f7b-125">Gehen Sie im Dialogfeld **ASP.NET Core-Webanwendung** folgendermaßen vor:</span><span class="sxs-lookup"><span data-stu-id="e3f7b-125">In the **New ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="e3f7b-126">Achten Sie darauf, dass **.NET Core** und **ASP.NET Core 2.1** in den Dropdownlisten ausgewählt sind.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-126">Make sure that **.NET Core** and **ASP.NET Core 2.1** are selected in the drop-down lists</span></span>
  * <span data-ttu-id="e3f7b-127">Wählen Sie die Projektvorlage **Webanwendung (Model-View-Controller)** aus.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-127">Select the **Web Application (Model-View-Controller)** project template</span></span>
  * <span data-ttu-id="e3f7b-128">Vergewissern Sie sich, dass **Authentifizierung** auf **Keine Authentifizierung** festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-128">Make sure that **Authentication** is set to **No Authentication**</span></span>
  * <span data-ttu-id="e3f7b-129">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-129">Click **OK**</span></span>

<span data-ttu-id="e3f7b-130">Warnung: Wenn Sie für **Authentifizierung** anstelle von **Keine Authentifizierung** die Option **Einzelne Benutzerkonten** verwenden, wird dem Projekt in `Models\IdentityModel.cs` ein Entity Framework Core-Modell hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-130">Warning: If you use **Individual User Accounts** instead of **None** for **Authentication** then an Entity Framework Core model will be added to the project in `Models\IdentityModel.cs`.</span></span> <span data-ttu-id="e3f7b-131">Anhand der Techniken, die Sie in diesem Tutorial kennenlernen, können Sie ein zweites Modell hinzufügen oder das vorhandene Modell um Ihre Entitätsklassen erweitern.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-131">Using the techniques you learn in this tutorial, you can choose to add a second model, or extend this existing model to contain your entity classes.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e3f7b-132">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="e3f7b-132">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="e3f7b-133">Führen Sie den folgenden Befehl aus, um ein MVC-Projekt zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="e3f7b-133">Run the following command to create an MVC project:</span></span>

   ```cli
   dotnet new mvc -n EFGetStarted.AspNetCore.NewDb
   ```
* <span data-ttu-id="e3f7b-134">Wechseln Sie in das Projektverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-134">Change to the project directory.</span></span> <span data-ttu-id="e3f7b-135">Die nächsten Befehle, die Sie eingeben, müssen für das neue Projekt ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-135">The next commands you enter need to run against the new project.</span></span>

   ```cli
   cd EFGetStarted.AspNetCore.NewDb
   ```
---

## <a name="install-entity-framework-core"></a><span data-ttu-id="e3f7b-136">Installieren von Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e3f7b-136">Install Entity Framework Core</span></span>

<span data-ttu-id="e3f7b-137">Installieren Sie das Paket für den (oder die) gewünschten EF Core-Datenbankanbieter, um EF Core zu installieren.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-137">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="e3f7b-138">Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="e3f7b-138">For a list of available providers, see [Database Providers](../../providers/index.md).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e3f7b-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e3f7b-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e3f7b-140">Für dieses Tutorial müssen Sie kein Anbieterpaket installieren, da das Tutorial SQL Server verwendet.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-140">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="e3f7b-141">Das SQL Server-Anbieterpaket ist im Metapaket [Microsoft.AspnetCore.App](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1) enthalten.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-141">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e3f7b-142">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="e3f7b-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="e3f7b-143">In diesem Tutorial wird SQLite verwendet, da es auf allen Plattformen ausgeführt werden kann, die .NET Core unterstützt.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-143">This tutorial uses SQLite because it runs on all platforms that .NET Core supports.</span></span>

* <span data-ttu-id="e3f7b-144">Führen Sie den folgenden Befehl aus, um den SQLite-Anbieter zu installieren:</span><span class="sxs-lookup"><span data-stu-id="e3f7b-144">Run the following command to install the SQLite provider:</span></span>

   ```cli
   dotnet add package Microsoft.EntityFrameworkCore.Sqlite
   ```

---

## <a name="create-the-model"></a><span data-ttu-id="e3f7b-145">Erstellen des Modells</span><span class="sxs-lookup"><span data-stu-id="e3f7b-145">Create the model</span></span>

<span data-ttu-id="e3f7b-146">Definieren Sie eine Kontextklasse und Entitätsklassen für das Modell.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-146">Define a context class and entity classes that make up the model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e3f7b-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e3f7b-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e3f7b-148">Klicken Sie mit der rechten Maustaste auf den Ordner **Modelle**, und wählen Sie **Hinzufügen > Klasse** aus.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-148">Right-click on the **Models** folder and select **Add > Class**.</span></span>
* <span data-ttu-id="e3f7b-149">Geben Sie als Name **Model.cs** ein, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-149">Enter **Model.cs** as the name and click **OK**.</span></span>
* <span data-ttu-id="e3f7b-150">Ersetzen Sie den Inhalt der Datei durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="e3f7b-150">Replace the contents of the file with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e3f7b-151">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="e3f7b-151">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="e3f7b-152">Erstellen Sie mit dem folgenden Code im Ordner **Models** (Modelle) eine **Model.cs**-Datei:</span><span class="sxs-lookup"><span data-stu-id="e3f7b-152">In the **Models** folder create **Model.cs** with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Models/Model.cs)]

---

<span data-ttu-id="e3f7b-153">Bei einer Produktionsanwendung würde normalerweise jede Klasse in einer separaten Datei platziert.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-153">A production app would typically put each class in a separate file.</span></span> <span data-ttu-id="e3f7b-154">Der Einfachheit halber werden in diesem Tutorial die Klassen in einer Datei zusammengefasst.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-154">For the sake of simplicity, this tutorial puts these classes in one file.</span></span>

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="e3f7b-155">Registrieren des Kontexts durch Dependency Injection</span><span class="sxs-lookup"><span data-stu-id="e3f7b-155">Register the context with dependency injection</span></span>

<span data-ttu-id="e3f7b-156">Registrieren Sie eine `BloggingContext`-Klasse als Dienst in `Startup.cs`, um diese für MVC-Controller verfügbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-156">To make `BloggingContext` available to MVC controllers, register it as a service in `Startup.cs`.</span></span>

<span data-ttu-id="e3f7b-157">Dienste (z.B. `BloggingContext`) werden mit [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) beim Starten einer Anwendung registriert, sodass sie Komponenten automatisch bereitgestellt werden können, die Dienste (z.B. MVC-Controller) über Konstruktorparameter und -eigenschaften nutzen.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-157">Services (such as `BloggingContext`) are registered with [dependency injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) during application startup so that they can be provided automatically to components that consume services (such as MVC controllers) via constructor parameters and properties.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e3f7b-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e3f7b-158">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e3f7b-159">Fügen Sie in **Startup.cs** die folgenden `using`-Anweisungen hinzu:</span><span class="sxs-lookup"><span data-stu-id="e3f7b-159">In **Startup.cs** add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

* <span data-ttu-id="e3f7b-160">Fügen Sie der `ConfigureServices`-Methode den folgenden hervorgehobenen Code zu:</span><span class="sxs-lookup"><span data-stu-id="e3f7b-160">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=12-14)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e3f7b-161">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="e3f7b-161">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="e3f7b-162">Fügen Sie in **Startup.cs** die folgenden `using`-Anweisungen hinzu:</span><span class="sxs-lookup"><span data-stu-id="e3f7b-162">In **Startup.cs** add the following `using` statements:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs#AddedUsings)]

* <span data-ttu-id="e3f7b-163">Fügen Sie der `ConfigureServices`-Methode den folgenden hervorgehobenen Code zu:</span><span class="sxs-lookup"><span data-stu-id="e3f7b-163">Add the following highlighted code to the `ConfigureServices` method:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs?name=ConfigureServices&highlight=12-14)]

---

<span data-ttu-id="e3f7b-164">In einer Produktionsanwendung wird die Verbindungszeichenfolge im Allgemeinen in eine Konfigurationsdatei oder Umgebungsvariable eingefügt.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-164">A production app would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="e3f7b-165">Der Einfachheit halber wird sie in diesem Tutorial im Code definiert.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-165">For the sake of simplicity, this tutorial defines it in code.</span></span> <span data-ttu-id="e3f7b-166">Weitere Informationen finden Sie in [Verbindungszeichenfolgen](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="e3f7b-166">See [Connection Strings](../../miscellaneous/connection-strings.md) for more information.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="e3f7b-167">Erstellen der Datenbank</span><span class="sxs-lookup"><span data-stu-id="e3f7b-167">Create the database</span></span>

<span data-ttu-id="e3f7b-168">Die folgenden Schritte verwenden [Migrationen](xref:core/managing-schemas/migrations/index), um eine Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-168">The following steps use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e3f7b-169">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e3f7b-169">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e3f7b-170">Wählen Sie **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-170">**Tools > NuGet Package Manager > Package Manager Console**</span></span>
* <span data-ttu-id="e3f7b-171">Führen Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="e3f7b-171">Run the following commands:</span></span>

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

  <span data-ttu-id="e3f7b-172">Wenn Sie den Fehler `The term 'add-migration' is not recognized as the name of a cmdlet` erhalten, schließen Sie Visual Studio, und öffnen Sie es erneut.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-172">If you get an error stating `The term 'add-migration' is not recognized as the name of a cmdlet`, close and reopen Visual Studio.</span></span>

  <span data-ttu-id="e3f7b-173">Der Befehl `Add-Migration` richtet per Gerüstbau eine Migration ein und erstellt den anfänglichen Tabellensatz für das Modell.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-173">The `Add-Migration` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="e3f7b-174">Der Befehl `Update-Database` erstellt die Datenbank und wendet die neue Migration auf sie an.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-174">The `Update-Database` command creates the database and applies the new migration to it.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e3f7b-175">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="e3f7b-175">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="e3f7b-176">Führen Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="e3f7b-176">Run the following commands:</span></span>

  ```cli
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  <span data-ttu-id="e3f7b-177">Der Befehl `migrations` richtet per Gerüstbau eine Migration ein und erstellt den anfänglichen Tabellensatz für das Modell.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-177">The `migrations` command scaffolds a migration to create the initial set of tables for the model.</span></span> <span data-ttu-id="e3f7b-178">Der Befehl `database update` erstellt die Datenbank und wendet die neue Migration auf sie an.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-178">The `database update` command creates the database and applies the new migration to it.</span></span>

---

## <a name="create-a-controller"></a><span data-ttu-id="e3f7b-179">Erstellen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="e3f7b-179">Create a controller</span></span>

<span data-ttu-id="e3f7b-180">Erstellen Sie ein Gerüst für einen Controller und Ansichten für die `Blog`-Entität.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-180">Scaffold a controller and views for the `Blog` entity.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e3f7b-181">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e3f7b-181">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e3f7b-182">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Controller**, und wählen Sie **Hinzufügen > Controller** aus.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-182">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add > Controller**.</span></span>
* <span data-ttu-id="e3f7b-183">Klicken Sie auf **MVC-Controller mit Ansichten unter Verwendung von Entity Framework** und danach auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-183">Select **MVC Controller with views, using Entity Framework** and click **Add**.</span></span>
* <span data-ttu-id="e3f7b-184">Legen Sie **Modellklasse** auf **Blog** und **Datenkontextklasse** auf **BloggingContext** fest.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-184">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**.</span></span>
* <span data-ttu-id="e3f7b-185">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-185">Click **Add**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e3f7b-186">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="e3f7b-186">.NET Core CLI</span></span>](#tab/netcore-cli)

* <span data-ttu-id="e3f7b-187">Führen Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="e3f7b-187">Run the following commands:</span></span>

  ```cli
  dotnet tool install -g dotnet-aspnet-codegenerator
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet restore
  dotnet aspnet-codegenerator controller -name BlogsController -m Blog -dc BloggingContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  <span data-ttu-id="e3f7b-188">Die Befehle `tool install` und `add package` installieren die Tools, die für den Gerüstbau für Controller und Ansichten verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-188">The `tool install` and `add package` commands install the tooling that can scaffold controllers and views.</span></span> <span data-ttu-id="e3f7b-189">Mit dem Befehl `restore` wird sichergestellt, dass alle Pakete des Projekts heruntergeladen werden, und der Befehl `aspnet-codegenerator` erledigt den Gerüstbau.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-189">The `restore` command ensures that all of the project's packages are downloaded, and the `aspnet-codegenerator` command does the scaffolding.</span></span>
---

<span data-ttu-id="e3f7b-190">Die Gerüstbau-Engine erstellt die folgenden Dateien:</span><span class="sxs-lookup"><span data-stu-id="e3f7b-190">The scaffolding engine creates the following files:</span></span>

* <span data-ttu-id="e3f7b-191">Einen Controller (*Controllers/BlogsController.cs*)</span><span class="sxs-lookup"><span data-stu-id="e3f7b-191">A controller (*Controllers/BlogsController.cs*)</span></span>
* <span data-ttu-id="e3f7b-192">Razor-Ansichten für die Seiten „Erstellen“, „Löschen“, „Details“ „Bearbeiten“ und „Index“ (_Views/Blogs/\*.cshtml_)</span><span class="sxs-lookup"><span data-stu-id="e3f7b-192">Razor views for Create, Delete, Details, Edit, and Index pages (_Views/Blogs/\*.cshtml_)</span></span>

## <a name="run-the-application"></a><span data-ttu-id="e3f7b-193">Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="e3f7b-193">Run the application</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e3f7b-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e3f7b-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e3f7b-195">**Debuggen** > **Starten ohne Debuggen**</span><span class="sxs-lookup"><span data-stu-id="e3f7b-195">**Debug** > **Start Without Debugging**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e3f7b-196">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="e3f7b-196">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet run
```
---

* <span data-ttu-id="e3f7b-197">Navigieren Sie zu `/Blogs`.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-197">Navigate to `/Blogs`</span></span>

* <span data-ttu-id="e3f7b-198">Verwenden Sie den Link **Neu erstellen**, um einige Blogeinträge zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-198">Use the **Create New** link to create some blog entries.</span></span>

  ![Seite „Create“](_static/create.png)

* <span data-ttu-id="e3f7b-200">Testen Sie die Links **Details**, **Bearbeiten** und **Löschen**.</span><span class="sxs-lookup"><span data-stu-id="e3f7b-200">Test the **Details**, **Edit**, and **Delete** links.</span></span>

  ![Indexseite](_static/index-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="e3f7b-202">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="e3f7b-202">Additional Resources</span></span>

* [<span data-ttu-id="e3f7b-203">Tutorial: Erste Schritte mit EF Core in .NET Core mit einer neuen Datenbank mithilfe von SQLite</span><span class="sxs-lookup"><span data-stu-id="e3f7b-203">Tutorial: Get started with EF Core on .NET Core with a new database using SQLite</span></span>](xref:core/get-started/netcore/new-db-sqlite)
* <span data-ttu-id="e3f7b-204">[Erste Schritte mit Razor-Seiten in ASP.NET Core](/aspnet/core/tutorials/razor-pages/razor-pages-start) oder [Erste Schritte mit ASP.NET Core MVC](/aspnet/core/tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="e3f7b-204">[Get started with Razor Pages in ASP.NET Core](/aspnet/core/tutorials/razor-pages/razor-pages-start) or [Get started with ASP.NET Core MVC ](/aspnet/core/tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="e3f7b-205">[Tutorial: Razor Pages mit Entity Framework Core in ASP.NET Core](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro) oder [Erste Schritte mit EF Core in einer ASP.NET Core MVC-Web-App](/aspnet/core/data/ef-mvc/intro)</span><span class="sxs-lookup"><span data-stu-id="e3f7b-205">[Tutorial: Razor Pages with Entity Framework Core in ASP.NET Core](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro) or [Get started with EF Core in an ASP.NET MVC web app](/aspnet/core/data/ef-mvc/intro)</span></span>
