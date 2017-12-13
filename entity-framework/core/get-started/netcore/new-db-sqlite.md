---
title: "Erste Schritte in .NET Core – Neue Datenbank – EF Core"
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: Erste Schritte mit .NET Core unter Verwendung von Entity Framework Core
keywords: .NET Core, Entity Framework Core, VS-Code, Visual Studio-Code, Mac, Linux
ms.date: 04/05/2017
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 22fc0446dee71dd0d2402b47d76cc8b7307fbe5f
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="2fd10-104">Erste Schritte mit EF Core in einer .NET Core-Konsolen-App mit einer neuen Datenbank</span><span class="sxs-lookup"><span data-stu-id="2fd10-104">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="2fd10-105">In dieser exemplarischen Vorgehensweise entwickeln Sie eine .NET Core-Konsolen-App, die einen grundlegenden Datenzugriff auf eine SQLite-Datenbank mithilfe von Entity Framework Core durchführt.</span><span class="sxs-lookup"><span data-stu-id="2fd10-105">In this walkthrough, you will create a .NET Core console app that performs basic data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="2fd10-106">Sie verwenden Migrationen, um die Datenbank aus Ihrem Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="2fd10-106">You will use migrations to create the database from your model.</span></span> <span data-ttu-id="2fd10-107">Siehe [ASP.NET Core – Neue Datenbank](xref:core/get-started/aspnetcore/new-db) für eine Visual Studio-Version mit Verwendung von ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="2fd10-107">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

> [!NOTE]  
> <span data-ttu-id="2fd10-108">`project.json` oder Visual Studio 2015 werden vom [.NET Core SDK](https://www.microsoft.com/net/download/core) nicht mehr unterstützt.</span><span class="sxs-lookup"><span data-stu-id="2fd10-108">The [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or Visual Studio 2015.</span></span> <span data-ttu-id="2fd10-109">Es wird empfohlen, [eine Migration von „project.json“ zu „csproj“ durchzuführen](https://docs.microsoft.com/dotnet/articles/core/migration/).</span><span class="sxs-lookup"><span data-stu-id="2fd10-109">We recommend you [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/).</span></span> <span data-ttu-id="2fd10-110">Wenn Sie Visual Studio verwenden, wird eine Migration zu [Visual Studio 2017](https://www.visualstudio.com/downloads/) empfohlen.</span><span class="sxs-lookup"><span data-stu-id="2fd10-110">If you are using Visual Studio, we recommend you migrate to [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

> [!TIP]  
> <span data-ttu-id="2fd10-111">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="2fd10-111">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2fd10-112">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="2fd10-112">Prerequisites</span></span>

<span data-ttu-id="2fd10-113">Zum Ausführen dieser exemplarischen Vorgehensweise müssen folgende Voraussetzungen erfüllt sein:</span><span class="sxs-lookup"><span data-stu-id="2fd10-113">The following prerequisites are needed to complete this walkthrough:</span></span>
* <span data-ttu-id="2fd10-114">Ein Betriebssystem mit Unterstützung von .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2fd10-114">An operating system that supports .NET Core.</span></span>
* <span data-ttu-id="2fd10-115">[.NET Core SDK 2.0](https://www.microsoft.com/net/core) (wenngleich zum Verwenden einer Anwendung mit einer vorherigen Version die Anweisungen nur leicht geändert werden müssen).</span><span class="sxs-lookup"><span data-stu-id="2fd10-115">[The .NET Core SDK](https://www.microsoft.com/net/core) 2.0 (although the instructions can be used to create an application with a previous version with very few modifications).</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="2fd10-116">Erstellen eines neuen Projekts</span><span class="sxs-lookup"><span data-stu-id="2fd10-116">Create a new project</span></span>

* <span data-ttu-id="2fd10-117">Erstellen Sie einen neuen Ordner `ConsoleApp.SQLite` für Ihr Projekt, und verwenden Sie den Befehl `dotnet`, um ihn mit einer .NET Core-App aufzufüllen.</span><span class="sxs-lookup"><span data-stu-id="2fd10-117">Create a new `ConsoleApp.SQLite` folder for your project and use the `dotnet` command to populate it with a .NET Core app.</span></span>

``` Console
mkdir ConsoleApp.SQLite
cd ConsoleApp.SQLite/
dotnet new console
```

## <a name="install-entity-framework-core"></a><span data-ttu-id="2fd10-118">Installieren von Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="2fd10-118">Install Entity Framework Core</span></span>

<span data-ttu-id="2fd10-119">Installieren Sie zur Verwendung von EF Core das Paket für den (oder die) gewünschten Datenbankanbieter.</span><span class="sxs-lookup"><span data-stu-id="2fd10-119">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="2fd10-120">In dieser exemplarischen Vorgehensweise wird SQLite verwendet.</span><span class="sxs-lookup"><span data-stu-id="2fd10-120">This walkthrough uses SQLite.</span></span> <span data-ttu-id="2fd10-121">Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="2fd10-121">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="2fd10-122">Installieren Sie „Microsoft.EntityFrameworkCore.Sqlite“ und „Microsoft.EntityFrameworkCore.Design“.</span><span class="sxs-lookup"><span data-stu-id="2fd10-122">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* <span data-ttu-id="2fd10-123">Führen Sie eine manuelle Bearbeitung von `ConsoleApp.SQLite.csproj` durch, um „DotNetCliToolReference“ zu „Microsoft.EntityFrameworkCore.Tools.DotNet“ hinzuzufügen:</span><span class="sxs-lookup"><span data-stu-id="2fd10-123">Manually edit `ConsoleApp.SQLite.csproj` to add a DotNetCliToolReference to Microsoft.EntityFrameworkCore.Tools.DotNet:</span></span>

  ``` xml
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  </ItemGroup>
  ```

 <span data-ttu-id="2fd10-124">Hinweis: In einer zukünftigen Version von `dotnet` wird „DotNetCliToolReferences“ über `dotnet add tool` unterstützt.</span><span class="sxs-lookup"><span data-stu-id="2fd10-124">Note: A future version of `dotnet` will support DotNetCliToolReferences via `dotnet add tool`</span></span>

<span data-ttu-id="2fd10-125">`ConsoleApp.SQLite.csproj` sollte jetzt Folgendes enthalten:</span><span class="sxs-lookup"><span data-stu-id="2fd10-125">`ConsoleApp.SQLite.csproj` should now contain the following:</span></span>

[!code[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/ConsoleApp.SQLite.csproj)]

 <span data-ttu-id="2fd10-126">Hinweis: Die oben verwendeten Versionsnummer waren zum Zeitpunkt der Veröffentlichung korrekt.</span><span class="sxs-lookup"><span data-stu-id="2fd10-126">Note: The version numbers used above were correct at the time of publishing.</span></span>

*  <span data-ttu-id="2fd10-127">Führen Sie `dotnet restore` aus, um die neuen Pakete zu installieren.</span><span class="sxs-lookup"><span data-stu-id="2fd10-127">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="2fd10-128">Erstellen des Modells</span><span class="sxs-lookup"><span data-stu-id="2fd10-128">Create the model</span></span>

<span data-ttu-id="2fd10-129">Definieren Sie einen Kontext und Entitätsklassen für Ihr Modell.</span><span class="sxs-lookup"><span data-stu-id="2fd10-129">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="2fd10-130">Erstellen Sie eine neue Datei *Model.cs* mit den folgenden Inhalten.</span><span class="sxs-lookup"><span data-stu-id="2fd10-130">Create a new *Model.cs* file with the following contents.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="2fd10-131">Tipp: In einer echten Anwendung würden Sie jede Anwendung in einer separaten Datei platzieren und die Verbindungszeichenfolge eine Konfigurationsdatei einfügen.</span><span class="sxs-lookup"><span data-stu-id="2fd10-131">Tip: In a real application you would put each class in a separate file and put the connection string in a configuration file.</span></span> <span data-ttu-id="2fd10-132">Der Einfachheit halber werden in diesem Tutorial alle Elemente in einer Datei zusammengefasst.</span><span class="sxs-lookup"><span data-stu-id="2fd10-132">To keep the tutorial simple, we are putting everything in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="2fd10-133">Erstellen der Datenbank</span><span class="sxs-lookup"><span data-stu-id="2fd10-133">Create the database</span></span>

<span data-ttu-id="2fd10-134">Da Sie jetzt über ein Modell verfügen, können Sie mithilfe von [Migrationen](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) eine Datenbank erstellen.</span><span class="sxs-lookup"><span data-stu-id="2fd10-134">Once you have a model, you can use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="2fd10-135">Führen Sie `dotnet ef migrations add InitialCreate` aus, um per Gerüstbau eine Migration einzurichten und den anfänglichen Tabellensatz für das Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="2fd10-135">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="2fd10-136">Führen Sie `dotnet ef database update` aus, um die neue Migration auf die Datenbank anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="2fd10-136">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="2fd10-137">Mit diesem Befehl wird die Datenbank erstellt, bevor Migrationen angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="2fd10-137">This command creates the database before applying migrations.</span></span>

> [!NOTE]  
> <span data-ttu-id="2fd10-138">Wenn Sie mit SQLite relative Pfade verwenden, sind diese Pfade relativ zur Hauptassembly der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="2fd10-138">When using relative paths with SQLite, the path will be relative to the application's main assembly.</span></span> <span data-ttu-id="2fd10-139">In diesem Beispiel ist `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll` die Hauptbinärdatei, deshalb befindet sich die SQLite-Datenbank in `bin/Debug/netcoreapp2.0/blogging.db`.</span><span class="sxs-lookup"><span data-stu-id="2fd10-139">In this sample, the main binary is `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`, so the SQLite database will be in `bin/Debug/netcoreapp2.0/blogging.db`.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="2fd10-140">Verwenden Ihres Modells</span><span class="sxs-lookup"><span data-stu-id="2fd10-140">Use your model</span></span>

* <span data-ttu-id="2fd10-141">Öffnen Sie die Datei *Program.cs*, und ersetzen Sie den Inhalt durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="2fd10-141">Open *Program.cs* and replace the contents with the following code:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="2fd10-142">Testen der App:</span><span class="sxs-lookup"><span data-stu-id="2fd10-142">Test the app:</span></span>

 `dotnet run`

 <span data-ttu-id="2fd10-143">Ein Blog wird in der Datenbank gespeichert, und die Details zu allen Blogs werden an die Konsole ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="2fd10-143">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="2fd10-144">Ändern des Modells:</span><span class="sxs-lookup"><span data-stu-id="2fd10-144">Changing the model:</span></span>

- <span data-ttu-id="2fd10-145">Wenn Sie Änderungen an Ihrem Modell durchführen, können Sie mit dem Befehl `dotnet ef migrations add` per Gerüstbau eine neue [Migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) einrichten, um die entsprechenden Schemaänderungen an der Datenbank vorzunehmen.</span><span class="sxs-lookup"><span data-stu-id="2fd10-145">If you make changes to your model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)  to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="2fd10-146">Nachdem Sie den Gerüstcode überprüft (und alle erforderlichen Änderungen vorgenommen) haben, können Sie mit dem Befehl `dotnet ef database update` die Änderungen auf die Datenbank anwenden.</span><span class="sxs-lookup"><span data-stu-id="2fd10-146">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the changes to the database.</span></span>
- <span data-ttu-id="2fd10-147">EF verwendet eine `__EFMigrationsHistory`-Tabelle in der Datenbank, um nachzuverfolgen, welche Migrationen bereits auf die Datenbank angewendet wurden.</span><span class="sxs-lookup"><span data-stu-id="2fd10-147">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="2fd10-148">SQLite unterstützt aufgrund von Einschränkungen in SQLite nicht alle Migrationen (Schemaänderungen).</span><span class="sxs-lookup"><span data-stu-id="2fd10-148">SQLite does not support all migrations (schema changes) due to limitations in SQLite.</span></span> <span data-ttu-id="2fd10-149">Siehe [SQLite-Einschränkungen](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="2fd10-149">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="2fd10-150">Für eine Neuentwicklung können Sie erwägen, bei Modelländerungen die Datenbank zu verwerfen und eine neue Datenbank zu erstellen, statt Migrationen zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="2fd10-150">For new development, consider dropping the database and creating a new one rather than using migrations when your model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2fd10-151">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="2fd10-151">Additional Resources</span></span>

* <span data-ttu-id="2fd10-152">[.NET Core – Neue Datenbank mit SQLite](xref:core/get-started/netcore/new-db-sqlite): Ein EF-Tutorial für beliebige Plattformkonsolen.</span><span class="sxs-lookup"><span data-stu-id="2fd10-152">[.NET Core - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="2fd10-153">Einführung in ASP.NET Core MVC unter Mac oder Linux</span><span class="sxs-lookup"><span data-stu-id="2fd10-153">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="2fd10-154">Einführung in ASP.NET Core MVC mit Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2fd10-154">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="2fd10-155">Erste Schritte mit ASP.NET Core und Entity Framework Core mithilfe von Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2fd10-155">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
