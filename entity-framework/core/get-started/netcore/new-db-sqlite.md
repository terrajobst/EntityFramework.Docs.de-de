---
title: Erste Schritte in .NET Core – Neue Datenbank – EF Core
author: rick-anderson
ms.author: riande
description: Erste Schritte mit .NET Core unter Verwendung von Entity Framework Core
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: e6996630e399659807d23304993c8e19c11ca6f5
ms.sourcegitcommit: 83c1e2fc034e5eb1fec1ebabc8d629ffcc7c0632
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2019
ms.locfileid: "67351341"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="2fc36-103">Erste Schritte mit EF Core in einer .NET Core-Konsolen-App mit einer neuen Datenbank</span><span class="sxs-lookup"><span data-stu-id="2fc36-103">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="2fc36-104">In diesem Tutorial erstellen Sie eine .NET Core-Konsolen-App, die Datenzugriff auf eine SQLite-Datenbank mithilfe von Entity Framework Core ausführt.</span><span class="sxs-lookup"><span data-stu-id="2fc36-104">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="2fc36-105">Sie verwenden [Migrationen](xref:core/managing-schemas/migrations/index), um die Datenbank aus dem Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="2fc36-105">You use [migrations](xref:core/managing-schemas/migrations/index) to create the database from the model.</span></span> <span data-ttu-id="2fc36-106">Siehe [ASP.NET Core – Neue Datenbank](xref:core/get-started/aspnetcore/new-db) für eine Visual Studio-Version mit Verwendung von ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="2fc36-106">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

<span data-ttu-id="2fc36-107">[Sehen Sie sich das Beispiel aus diesem Artikel auf GitHub an](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span><span class="sxs-lookup"><span data-stu-id="2fc36-107">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2fc36-108">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="2fc36-108">Prerequisites</span></span>

* [<span data-ttu-id="2fc36-109">.NET Core 2.1 SDK</span><span class="sxs-lookup"><span data-stu-id="2fc36-109">The .NET Core 2.1 SDK</span></span>](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a><span data-ttu-id="2fc36-110">Erstellt ein neues Projekt</span><span class="sxs-lookup"><span data-stu-id="2fc36-110">Create a new project</span></span>

* <span data-ttu-id="2fc36-111">Erstellen Sie ein neues Konsolenprojekt:</span><span class="sxs-lookup"><span data-stu-id="2fc36-111">Create a new console project:</span></span>

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  ```
## <a name="change-the-current-directory"></a><span data-ttu-id="2fc36-112">Ändern des aktuellen Verzeichnisses</span><span class="sxs-lookup"><span data-stu-id="2fc36-112">Change the current directory</span></span>

<span data-ttu-id="2fc36-113">In den nachfolgenden Schritten werden `dotnet`-Befehle für die Anwendung ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="2fc36-113">In subsequent steps, we need to issue `dotnet` commands against the application.</span></span>

* <span data-ttu-id="2fc36-114">So ändern Sie das aktuelle Verzeichnis in das Verzeichnis der Anwendung</span><span class="sxs-lookup"><span data-stu-id="2fc36-114">We change the current directory to the application's directory like this:</span></span>

  ``` Console
  cd ConsoleApp.SQLite/
  ```
## <a name="install-entity-framework-core"></a><span data-ttu-id="2fc36-115">Installieren von Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="2fc36-115">Install Entity Framework Core</span></span>

<span data-ttu-id="2fc36-116">Installieren Sie zur Verwendung von EF Core das Paket für den (oder die) gewünschten Datenbankanbieter.</span><span class="sxs-lookup"><span data-stu-id="2fc36-116">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="2fc36-117">In dieser exemplarischen Vorgehensweise wird SQLite verwendet.</span><span class="sxs-lookup"><span data-stu-id="2fc36-117">This walkthrough uses SQLite.</span></span> <span data-ttu-id="2fc36-118">Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="2fc36-118">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="2fc36-119">Installieren Sie „Microsoft.EntityFrameworkCore.Sqlite“ und „Microsoft.EntityFrameworkCore.Design“.</span><span class="sxs-lookup"><span data-stu-id="2fc36-119">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* <span data-ttu-id="2fc36-120">Führen Sie `dotnet restore` aus, um die neuen Pakete zu installieren.</span><span class="sxs-lookup"><span data-stu-id="2fc36-120">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="2fc36-121">Erstellen des Modells</span><span class="sxs-lookup"><span data-stu-id="2fc36-121">Create the model</span></span>

<span data-ttu-id="2fc36-122">Definieren Sie einen Kontext und Entitätsklassen für Ihr Modell.</span><span class="sxs-lookup"><span data-stu-id="2fc36-122">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="2fc36-123">Erstellen Sie eine neue Datei *Model.cs* mit den folgenden Inhalten.</span><span class="sxs-lookup"><span data-stu-id="2fc36-123">Create a new *Model.cs* file with the following contents.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="2fc36-124">Tipp: In einer echten Anwendung platzieren Sie jede Anwendung in einer separaten Datei und fügen die Verbindungszeichenfolge in eine Konfigurationsdatei oder Umgebungsvariable ein.</span><span class="sxs-lookup"><span data-stu-id="2fc36-124">Tip: In a real application, you put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="2fc36-125">Der Einfachheit halber sind in diesem Tutorial alle Elemente in einer Datei enthalten.</span><span class="sxs-lookup"><span data-stu-id="2fc36-125">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="2fc36-126">Erstellen der Datenbank</span><span class="sxs-lookup"><span data-stu-id="2fc36-126">Create the database</span></span>

<span data-ttu-id="2fc36-127">Da Sie jetzt über ein Modell verfügen, können Sie mithilfe von [Migrationen](xref:core/managing-schemas/migrations/index) eine Datenbank erstellen.</span><span class="sxs-lookup"><span data-stu-id="2fc36-127">Once you have a model, you use [migrations](xref:core/managing-schemas/migrations/index) to create a database.</span></span>

* <span data-ttu-id="2fc36-128">Führen Sie `dotnet ef migrations add InitialCreate` aus, um per Gerüstbau eine Migration einzurichten und den anfänglichen Tabellensatz für das Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="2fc36-128">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="2fc36-129">Führen Sie `dotnet ef database update` aus, um die neue Migration auf die Datenbank anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="2fc36-129">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="2fc36-130">Mit diesem Befehl wird die Datenbank erstellt, bevor Migrationen angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="2fc36-130">This command creates the database before applying migrations.</span></span>

<span data-ttu-id="2fc36-131">Die SQLite-Datenbank *blogging.db* ist das Projektverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="2fc36-131">The *blogging.db* SQLite DB is in the project directory.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="2fc36-132">Verwenden des Modells</span><span class="sxs-lookup"><span data-stu-id="2fc36-132">Use the model</span></span>

* <span data-ttu-id="2fc36-133">Öffnen Sie die Datei *Program.cs*, und ersetzen Sie den Inhalt durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="2fc36-133">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="2fc36-134">Testen Sie die App über die Konsole.</span><span class="sxs-lookup"><span data-stu-id="2fc36-134">Test the app from the console.</span></span> <span data-ttu-id="2fc36-135">Weitere Informationen zum Ausführen der App aus Visual Studio finden Sie im [Visual Studio-Hinweis](#vs).</span><span class="sxs-lookup"><span data-stu-id="2fc36-135">See the [Visual Studio note](#vs) to run the app from Visual Studio.</span></span>

  `dotnet run`

  <span data-ttu-id="2fc36-136">Ein Blog wird in der Datenbank gespeichert, und die Details zu allen Blogs werden an die Konsole ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="2fc36-136">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="2fc36-137">Ändern des Modells:</span><span class="sxs-lookup"><span data-stu-id="2fc36-137">Changing the model:</span></span>

- <span data-ttu-id="2fc36-138">Wenn Sie Änderungen am Modell vornehmen, können Sie mithilfe des `dotnet ef migrations add`-Befehls per Gerüstbau eine neue [Migration](xref:core/managing-schemas/migrations/index) erstellen.</span><span class="sxs-lookup"><span data-stu-id="2fc36-138">If you make changes to the model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](xref:core/managing-schemas/migrations/index).</span></span> <span data-ttu-id="2fc36-139">Nachdem Sie den Gerüstcode überprüft (und alle erforderlichen Änderungen vorgenommen) haben, können Sie mit dem Befehl `dotnet ef database update` Schemaänderungen auf die Datenbank anwenden.</span><span class="sxs-lookup"><span data-stu-id="2fc36-139">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the schema changes to the database.</span></span>
- <span data-ttu-id="2fc36-140">EF Core verwendet eine `__EFMigrationsHistory`-Tabelle in der Datenbank, um nachzuverfolgen, welche Migrationen bereits auf die Datenbank angewendet wurden.</span><span class="sxs-lookup"><span data-stu-id="2fc36-140">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="2fc36-141">Die SQLite-Datenbankengine unterstützt bestimmte Schemaänderungen nicht, die von den meisten anderen relationalen Datenbanken unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="2fc36-141">The SQLite database engine doesn't support certain schema changes that are supported by most other relational databases.</span></span> <span data-ttu-id="2fc36-142">Beispielsweise wird der `DropColumn`-Vorgang nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="2fc36-142">For example, the `DropColumn` operation is not supported.</span></span> <span data-ttu-id="2fc36-143">EF Core-Migrationen generieren Code für diese Vorgänge.</span><span class="sxs-lookup"><span data-stu-id="2fc36-143">EF Core Migrations will generate code for these operations.</span></span> <span data-ttu-id="2fc36-144">Wenn Sie aber versuchen, diese auf eine Datenbank anzuwenden oder ein Skript zu generieren, löst EF Core Ausnahmen aus.</span><span class="sxs-lookup"><span data-stu-id="2fc36-144">But if you try to apply them to a database or generate a script, EF Core throws exceptions.</span></span> <span data-ttu-id="2fc36-145">Siehe [SQLite-Einschränkungen](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="2fc36-145">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="2fc36-146">Für eine Neuentwicklung können Sie erwägen, bei Modelländerungen die Datenbank zu verwerfen und eine neue Datenbank zu erstellen, anstatt Migrationen zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="2fc36-146">For new development, consider dropping the database and creating a new one rather than using migrations when the model changes.</span></span>

<a name="vs"></a>
### <a name="run-from-visual-studio"></a><span data-ttu-id="2fc36-147">Ausführen aus Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2fc36-147">Run from Visual Studio</span></span>

<span data-ttu-id="2fc36-148">Um dieses Beispiel von Visual Studio aus auszuführen, müssen Sie das Arbeitsverzeichnis manuell als Stammverzeichnis für das Projekt festlegen.</span><span class="sxs-lookup"><span data-stu-id="2fc36-148">To run this sample from Visual Studio, you must set the working directory manually to be the root of the project.</span></span> <span data-ttu-id="2fc36-149">Wenn Sie das Arbeitsverzeichnis nicht festlegen, wird der Ausnahmefehler `Microsoft.Data.Sqlite.SqliteException` ausgegeben: `SQLite Error 1: 'no such table: Blogs'`.</span><span class="sxs-lookup"><span data-stu-id="2fc36-149">If  you don't set the working directory, the following `Microsoft.Data.Sqlite.SqliteException` is thrown: `SQLite Error 1: 'no such table: Blogs'`.</span></span>

<span data-ttu-id="2fc36-150">So legen Sie das Arbeitsverzeichnis fest</span><span class="sxs-lookup"><span data-stu-id="2fc36-150">To set the working directory:</span></span>

* <span data-ttu-id="2fc36-151">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie dann **Eigenschaften** aus.</span><span class="sxs-lookup"><span data-stu-id="2fc36-151">In **Solution Explorer**, right click the project and then select **Properties**.</span></span>
* <span data-ttu-id="2fc36-152">Wählen Sie im linken Bereich die Registerkarte **Debuggen** aus.</span><span class="sxs-lookup"><span data-stu-id="2fc36-152">Select the **Debug** tab in the left pane.</span></span>
* <span data-ttu-id="2fc36-153">Legen Sie **Arbeitsverzeichnis** auf das Projektverzeichnis fest.</span><span class="sxs-lookup"><span data-stu-id="2fc36-153">Set **Working directory** to the project directory.</span></span>
* <span data-ttu-id="2fc36-154">Speichern Sie die Änderungen.</span><span class="sxs-lookup"><span data-stu-id="2fc36-154">Save the changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2fc36-155">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="2fc36-155">Additional Resources</span></span>

* [<span data-ttu-id="2fc36-156">Tutorial: Erste Schritte mit EF Core in ASP.NET Core mit einer neuen Datenbank mithilfe von SQLite</span><span class="sxs-lookup"><span data-stu-id="2fc36-156">Tutorial: Get started with EF Core on ASP.NET Core with a new database using SQLite</span></span>](xref:core/get-started/aspnetcore/new-db)
* [<span data-ttu-id="2fc36-157">Tutorial: Erste Schritte mit Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2fc36-157">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="2fc36-158">Tutorial: Razor Pages mit Entity Framework Core in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2fc36-158">Tutorial: Razor Pages with Entity Framework Core in ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
