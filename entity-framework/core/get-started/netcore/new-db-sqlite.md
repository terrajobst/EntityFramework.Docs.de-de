---
title: Erste Schritte in .NET Core – Neue Datenbank – EF Core
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: Erste Schritte mit .NET Core unter Verwendung von Entity Framework Core
keywords: .NET Core, Entity Framework Core, VS-Code, Visual Studio-Code, Mac, Linux
ms.date: 06/05/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: e4eafed037325237345efbc3d7d42b32270a54e3
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2018
ms.locfileid: "37911501"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a><span data-ttu-id="f1913-104">Erste Schritte mit EF Core in einer .NET Core-Konsolen-App mit einer neuen Datenbank</span><span class="sxs-lookup"><span data-stu-id="f1913-104">Getting Started with EF Core on .NET Core Console App with a New database</span></span>

<span data-ttu-id="f1913-105">In dieser exemplarischen Vorgehensweise entwickeln Sie eine .NET Core-Konsolen-App, die einen Datenzugriff auf eine SQLite-Datenbank mithilfe von Entity Framework Core durchführt.</span><span class="sxs-lookup"><span data-stu-id="f1913-105">In this walkthrough, you create a .NET Core console app that performs data access against a SQLite database using Entity Framework Core.</span></span> <span data-ttu-id="f1913-106">Sie verwenden Migrationen, um die Datenbank aus dem Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="f1913-106">You use migrations to create the database from the model.</span></span> <span data-ttu-id="f1913-107">Siehe [ASP.NET Core – Neue Datenbank](xref:core/get-started/aspnetcore/new-db) für eine Visual Studio-Version mit Verwendung von ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="f1913-107">See [ASP.NET Core - New database](xref:core/get-started/aspnetcore/new-db) for a Visual Studio version using ASP.NET Core MVC.</span></span>

> [!TIP]  
> <span data-ttu-id="f1913-108">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="f1913-108">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f1913-109">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="f1913-109">Prerequisites</span></span>

<span data-ttu-id="f1913-110">[Das .NET Core SDK](https://www.microsoft.com/net/core) 2.1</span><span class="sxs-lookup"><span data-stu-id="f1913-110">[The .NET Core SDK](https://www.microsoft.com/net/core) 2.1</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="f1913-111">Erstellt ein neues Projekt</span><span class="sxs-lookup"><span data-stu-id="f1913-111">Create a new project</span></span>

* <span data-ttu-id="f1913-112">Erstellen Sie ein neues Konsolenprojekt:</span><span class="sxs-lookup"><span data-stu-id="f1913-112">Create a new console project:</span></span>

``` Console
dotnet new console -o ConsoleApp.SQLite
cd ConsoleApp.SQLite/
```

## <a name="install-entity-framework-core"></a><span data-ttu-id="f1913-113">Installieren von Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="f1913-113">Install Entity Framework Core</span></span>

<span data-ttu-id="f1913-114">Installieren Sie zur Verwendung von EF Core das Paket für den (oder die) gewünschten Datenbankanbieter.</span><span class="sxs-lookup"><span data-stu-id="f1913-114">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="f1913-115">In dieser exemplarischen Vorgehensweise wird SQLite verwendet.</span><span class="sxs-lookup"><span data-stu-id="f1913-115">This walkthrough uses SQLite.</span></span> <span data-ttu-id="f1913-116">Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="f1913-116">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="f1913-117">Installieren Sie „Microsoft.EntityFrameworkCore.Sqlite“ und „Microsoft.EntityFrameworkCore.Design“.</span><span class="sxs-lookup"><span data-stu-id="f1913-117">Install Microsoft.EntityFrameworkCore.Sqlite and Microsoft.EntityFrameworkCore.Design</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* <span data-ttu-id="f1913-118">Führen Sie `dotnet restore` aus, um die neuen Pakete zu installieren.</span><span class="sxs-lookup"><span data-stu-id="f1913-118">Run `dotnet restore` to install the new packages.</span></span>

## <a name="create-the-model"></a><span data-ttu-id="f1913-119">Erstellen des Modells</span><span class="sxs-lookup"><span data-stu-id="f1913-119">Create the model</span></span>

<span data-ttu-id="f1913-120">Definieren Sie einen Kontext und Entitätsklassen für Ihr Modell.</span><span class="sxs-lookup"><span data-stu-id="f1913-120">Define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="f1913-121">Erstellen Sie eine neue Datei *Model.cs* mit den folgenden Inhalten.</span><span class="sxs-lookup"><span data-stu-id="f1913-121">Create a new *Model.cs* file with the following contents.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

<span data-ttu-id="f1913-122">Tipp: In einer echten Anwendung fügen Sie jede Klasse in eine separaten Datei und die Verbindungszeichenfolge in eine Konfigurationsdatei ein.</span><span class="sxs-lookup"><span data-stu-id="f1913-122">Tip: In a real application, you put each class in a separate file and put the connection string in a configuration file.</span></span> <span data-ttu-id="f1913-123">Der Einfachheit halber sind in diesem Tutorial alle Elemente in einer Datei enthalten.</span><span class="sxs-lookup"><span data-stu-id="f1913-123">To keep the tutorial simple, everything is contained in one file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="f1913-124">Erstellen der Datenbank</span><span class="sxs-lookup"><span data-stu-id="f1913-124">Create the database</span></span>

<span data-ttu-id="f1913-125">Da Sie jetzt über ein Modell verfügen, können Sie mithilfe von [Migrationen](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) eine Datenbank erstellen.</span><span class="sxs-lookup"><span data-stu-id="f1913-125">Once you have a model, you use [migrations](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) to create a database.</span></span>

* <span data-ttu-id="f1913-126">Führen Sie `dotnet ef migrations add InitialCreate` aus, um per Gerüstbau eine Migration einzurichten und den anfänglichen Tabellensatz für das Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="f1913-126">Run `dotnet ef migrations add InitialCreate` to scaffold a migration and create the initial set of tables for the model.</span></span>
* <span data-ttu-id="f1913-127">Führen Sie `dotnet ef database update` aus, um die neue Migration auf die Datenbank anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="f1913-127">Run `dotnet ef database update` to apply the new migration to the database.</span></span> <span data-ttu-id="f1913-128">Mit diesem Befehl wird die Datenbank erstellt, bevor Migrationen angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="f1913-128">This command creates the database before applying migrations.</span></span>

<span data-ttu-id="f1913-129">Die SQLite-Datenbank *blogging.db*\* ist das Projektverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="f1913-129">The *blogging.db*\* SQLite DB is in the project directory.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="f1913-130">Verwenden Ihres Modells</span><span class="sxs-lookup"><span data-stu-id="f1913-130">Use your model</span></span>

* <span data-ttu-id="f1913-131">Öffnen Sie die Datei *Program.cs*, und ersetzen Sie den Inhalt durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="f1913-131">Open *Program.cs* and replace the contents with the following code:</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* <span data-ttu-id="f1913-132">Testen der App:</span><span class="sxs-lookup"><span data-stu-id="f1913-132">Test the app:</span></span>

  `dotnet run`

  <span data-ttu-id="f1913-133">Ein Blog wird in der Datenbank gespeichert, und die Details zu allen Blogs werden an die Konsole ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="f1913-133">One blog is saved to the database and the details of all blogs are displayed in the console.</span></span>

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a><span data-ttu-id="f1913-134">Ändern des Modells:</span><span class="sxs-lookup"><span data-stu-id="f1913-134">Changing the model:</span></span>

- <span data-ttu-id="f1913-135">Wenn Sie Änderungen an Ihrem Modell durchführen, können Sie mit dem Befehl `dotnet ef migrations add` per Gerüstbau eine neue [Migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) einrichten, um die entsprechenden Schemaänderungen an der Datenbank vorzunehmen.</span><span class="sxs-lookup"><span data-stu-id="f1913-135">If you make changes to your model, you can use the `dotnet ef migrations add` command to scaffold a new [migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations)  to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="f1913-136">Nachdem Sie den Gerüstcode überprüft (und alle erforderlichen Änderungen vorgenommen) haben, können Sie mit dem Befehl `dotnet ef database update` die Änderungen auf die Datenbank anwenden.</span><span class="sxs-lookup"><span data-stu-id="f1913-136">Once you have checked the scaffolded code (and made any required changes), you can use the `dotnet ef database update` command to apply the changes to the database.</span></span>
- <span data-ttu-id="f1913-137">EF verwendet eine `__EFMigrationsHistory`-Tabelle in der Datenbank, um nachzuverfolgen, welche Migrationen bereits auf die Datenbank angewendet wurden.</span><span class="sxs-lookup"><span data-stu-id="f1913-137">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>
- <span data-ttu-id="f1913-138">SQLite unterstützt aufgrund von Einschränkungen in SQLite nicht alle Migrationen (Schemaänderungen).</span><span class="sxs-lookup"><span data-stu-id="f1913-138">SQLite does not support all migrations (schema changes) due to limitations in SQLite.</span></span> <span data-ttu-id="f1913-139">Siehe [SQLite-Einschränkungen](../../providers/sqlite/limitations.md).</span><span class="sxs-lookup"><span data-stu-id="f1913-139">See [SQLite Limitations](../../providers/sqlite/limitations.md).</span></span> <span data-ttu-id="f1913-140">Für eine Neuentwicklung können Sie erwägen, bei Modelländerungen die Datenbank zu verwerfen und eine neue Datenbank zu erstellen, statt Migrationen zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="f1913-140">For new development, consider dropping the database and creating a new one rather than using migrations when your model changes.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f1913-141">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="f1913-141">Additional Resources</span></span>

* <span data-ttu-id="f1913-142">[.NET Core – Neue Datenbank mit SQLite](xref:core/get-started/netcore/new-db-sqlite): Ein EF-Tutorial für beliebige Plattformkonsolen.</span><span class="sxs-lookup"><span data-stu-id="f1913-142">[.NET Core - New database with SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
* [<span data-ttu-id="f1913-143">Einführung in ASP.NET Core MVC unter Mac oder Linux</span><span class="sxs-lookup"><span data-stu-id="f1913-143">Introduction to ASP.NET Core MVC on Mac or Linux</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [<span data-ttu-id="f1913-144">Einführung in ASP.NET Core MVC mit Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f1913-144">Introduction to ASP.NET Core MVC with Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [<span data-ttu-id="f1913-145">Erste Schritte mit ASP.NET Core und Entity Framework Core mithilfe von Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f1913-145">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
