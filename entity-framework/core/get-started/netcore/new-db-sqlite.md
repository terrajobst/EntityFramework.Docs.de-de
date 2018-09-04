---
title: Erste Schritte in .NET Core – Neue Datenbank – EF Core
author: rick-anderson
ms.author: riande
description: Erste Schritte mit .NET Core unter Verwendung von Entity Framework Core
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 51f5752eebce5603c663072f7b36dfecd4ddf227
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993691"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="43712-103">Erste Schritte mit EF Core in einer .NET Core-Konsolen-App mit einer neuen Datenbank</span><span class="sxs-lookup"><span data-stu-id="43712-103">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="43712-104">In diesem Tutorial erstellen Sie eine .NET Core-Konsolen-App, die Datenzugriff auf eine SQLite-Datenbank mithilfe von Entity Framework Core ausführt.</span><span class="sxs-lookup"><span data-stu-id="43712-104">In this tutorial, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="43712-105">Sie verwenden Migrationen, um die Datenbank aus dem Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="43712-105">You use migrations to create the database from the model.</span></span> <span data-ttu-id="43712-106">Siehe [ASP.NET Core – Neue Datenbank](xref:core/get-started/aspnetcore/new-db) für eine Visual Studio-Version mit Verwendung von ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="43712-106">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

<span data-ttu-id="43712-107">Beispiel aus diesem Artikel auf GitHub anzeigen](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span><span class="sxs-lookup"><span data-stu-id="43712-107">View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="43712-108">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="43712-108">Prerequisites</span></span>

* [<span data-ttu-id="43712-109">.NET Core 2.1 SDK</span><span class="sxs-lookup"><span data-stu-id="43712-109">The .NET Core 2.1 SDK</span></span>](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a><span data-ttu-id="43712-110">Erstellt ein neues Projekt</span><span class="sxs-lookup"><span data-stu-id="43712-110">Create a new project</span></span>

* <span data-ttu-id="43712-111">Erstellen Sie ein neues Konsolenprojekt:</span><span class="sxs-lookup"><span data-stu-id="43712-111">Create a new console project:</span></span>

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  cd ConsoleApp.SQLite/
  ```

## <a name="install-entity-framework-core"></a><span data-ttu-id="43712-112">Installieren von Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="43712-112">Install Entity Framework Core</span></span>

<span data-ttu-id="43712-113">Installieren Sie zur Verwendung von EF Core das Paket für den (oder die) gewünschten Datenbankanbieter.</span><span class="sxs-lookup"><span data-stu-id="43712-113">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="43712-114">In dieser exemplarischen Vorgehensweise wird SQLite verwendet.</span><span class="sxs-lookup"><span data-stu-id="43712-114">This walkthrough uses SQLite.</span></span> <span data-ttu-id="43712-115">Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="43712-115">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="43712-116">Installieren Sie „Microsoft.EntityFrameworkCore.Sqlite“ und „Microsoft.EntityFrameworkCore.Design“.</span><span class="sxs-lookup"><span data-stu-id="43712-116">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* <span data-ttu-id="43712-117">Führen Sie `dotnet restore` aus, um die neuen Pakete zu installieren.</span><span class="sxs-lookup"><span data-stu-id="43712-117">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="43712-118">Erstellen des Modells</span><span class="sxs-lookup"><span data-stu-id="43712-118">Create the model</span></span>

<span data-ttu-id="43712-119">Definieren Sie einen Kontext und Entitätsklassen für Ihr Modell.</span><span class="sxs-lookup"><span data-stu-id="43712-119">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="43712-120">Erstellen Sie eine neue Datei *Model.cs* mit den folgenden Inhalten.</span><span class="sxs-lookup"><span data-stu-id="43712-120">Create a new *Model.cs* file with the following contents.</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="43712-121">Tipp: In einer echten Anwendung platzieren Sie jede Anwendung in einer separaten Datei und fügen die Verbindungszeichenfolge in eine Konfigurationsdatei oder Umgebungsvariable ein.</span><span class="sxs-lookup"><span data-stu-id="43712-121">Tip: In a real application, you put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="43712-122">Der Einfachheit halber sind in diesem Tutorial alle Elemente in einer Datei enthalten.</span><span class="sxs-lookup"><span data-stu-id="43712-122">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="43712-123">Erstellen der Datenbank</span><span class="sxs-lookup"><span data-stu-id="43712-123">Create the database</span></span>

<span data-ttu-id="43712-124">Da Sie jetzt über ein Modell verfügen, können Sie mithilfe von [Migrationen](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) eine Datenbank erstellen.</span><span class="sxs-lookup"><span data-stu-id="43712-124">Once you have a model, you use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="43712-125">Führen Sie `dotnet ef migrations add InitialCreate` aus, um per Gerüstbau eine Migration einzurichten und den anfänglichen Tabellensatz für das Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="43712-125">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="43712-126">Führen Sie `dotnet ef database update` aus, um die neue Migration auf die Datenbank anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="43712-126">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="43712-127">Mit diesem Befehl wird die Datenbank erstellt, bevor Migrationen angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="43712-127">This command creates the database before applying migrations.</span></span>

<span data-ttu-id="43712-128">Die SQLite-Datenbank *blogging.db*\* ist das Projektverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="43712-128">The *blogging.db*\* SQLite DB is in the project directory.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="43712-129">Verwenden des Modells</span><span class="sxs-lookup"><span data-stu-id="43712-129">Use the model</span></span>

* <span data-ttu-id="43712-130">Öffnen Sie die Datei *Program.cs*, und ersetzen Sie den Inhalt durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="43712-130">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="43712-131">Testen der App:</span><span class="sxs-lookup"><span data-stu-id="43712-131">Test the app:</span></span>

  `dotnet run`

  <span data-ttu-id="43712-132">Ein Blog wird in der Datenbank gespeichert, und die Details zu allen Blogs werden an die Konsole ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="43712-132">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="43712-133">Ändern des Modells:</span><span class="sxs-lookup"><span data-stu-id="43712-133">Changing the model:</span></span>

- <span data-ttu-id="43712-134">Wenn Sie Änderungen am Modell vornehmen, können Sie mithilfe des `dotnet ef migrations add`-Befehls per Gerüstbau eine neue [Migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) erstellen.</span><span class="sxs-lookup"><span data-stu-id="43712-134">If you make changes to the model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations).</span></span> <span data-ttu-id="43712-135">Nachdem Sie den Gerüstcode überprüft (und alle erforderlichen Änderungen vorgenommen) haben, können Sie mit dem Befehl `dotnet ef database update` Schemaänderungen auf die Datenbank anwenden.</span><span class="sxs-lookup"><span data-stu-id="43712-135">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the schema changes to the database.</span></span>
- <span data-ttu-id="43712-136">EF Core verwendet eine `__EFMigrationsHistory`-Tabelle in der Datenbank, um nachzuverfolgen, welche Migrationen bereits auf die Datenbank angewendet wurden.</span><span class="sxs-lookup"><span data-stu-id="43712-136">EF Core uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="43712-137">Die SQLite-Datenbankengine unterstützt bestimmte Schemaänderungen nicht, die von den meisten anderen relationalen Datenbanken unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="43712-137">The SQLite database engine doesn't support certain schema changes that are supported by most other relational databases.</span></span> <span data-ttu-id="43712-138">Beispielsweise wird der `DropColumn`-Vorgang nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="43712-138">For example, the `DropColumn` operation is not supported.</span></span> <span data-ttu-id="43712-139">EF Core-Migrationen generieren Code für diese Vorgänge.</span><span class="sxs-lookup"><span data-stu-id="43712-139">EF Core Migrations will generate code for these operations.</span></span> <span data-ttu-id="43712-140">Wenn Sie aber versuchen, diese auf eine Datenbank anzuwenden oder ein Skript zu generieren, löst EF Core Ausnahmen aus.</span><span class="sxs-lookup"><span data-stu-id="43712-140">But if you try to apply them to a database or generate a script, EF Core throws exceptions.</span></span> <span data-ttu-id="43712-141">Siehe [SQLite-Einschränkungen](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="43712-141">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="43712-142">Für eine Neuentwicklung können Sie erwägen, bei Modelländerungen die Datenbank zu verwerfen und eine neue Datenbank zu erstellen, anstatt Migrationen zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="43712-142">For new development, consider dropping the database and creating a new one rather than using migrations when the model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="43712-143">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="43712-143">Additional Resources</span></span>

* [<span data-ttu-id="43712-144">Einführung in ASP.NET Core MVC unter Mac oder Linux</span><span class="sxs-lookup"><span data-stu-id="43712-144">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="43712-145">Einführung in ASP.NET Core MVC mit Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43712-145">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="43712-146">Erste Schritte mit ASP.NET Core und Entity Framework Core mithilfe von Visual Studio</span><span class="sxs-lookup"><span data-stu-id="43712-146">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
