---
title: Erste Schritte in .NET Framework – Neue Datenbank – EF Core
author: rowanmiller
ms.date: 08/06/2018
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: 66d9011a5978fc3d8253a963bdb9df27848e9ff9
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997586"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a><span data-ttu-id="8e651-102">Erste Schritte mit EF Core in .NET Framework mit einer neuen Datenbank</span><span class="sxs-lookup"><span data-stu-id="8e651-102">Getting started with EF Core on .NET Framework with a New Database</span></span>

<span data-ttu-id="8e651-103">In diesem Tutorial erstellen Sie eine Konsolenanwendung, die einen grundlegenden Datenzugriff auf eine Microsoft SQL Server-Datenbank mithilfe von Entity Framework durchführt.</span><span class="sxs-lookup"><span data-stu-id="8e651-103">In this tutorial, you build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="8e651-104">Sie verwenden Migrationen, um die Datenbank aus einem Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="8e651-104">You use migrations to create the database from a model.</span></span>

<span data-ttu-id="8e651-105">[Sehen Sie sich das Beispiel aus diesem Artikel auf GitHub an](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).</span><span class="sxs-lookup"><span data-stu-id="8e651-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8e651-106">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="8e651-106">Prerequisites</span></span>

* [<span data-ttu-id="8e651-107">Visual Studio 2017 Version 15.7 (oder höher)</span><span class="sxs-lookup"><span data-stu-id="8e651-107">Visual Studio 2017 version 15.7 or later</span></span>](https://www.visualstudio.com/downloads/)

## <a name="create-a-new-project"></a><span data-ttu-id="8e651-108">Erstellt ein neues Projekt</span><span class="sxs-lookup"><span data-stu-id="8e651-108">Create a new project</span></span>

* <span data-ttu-id="8e651-109">Öffnen Sie Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="8e651-109">Open Visual Studio 2017</span></span>

* <span data-ttu-id="8e651-110">Klicken Sie auf **Datei > Neu > Projekt...**.</span><span class="sxs-lookup"><span data-stu-id="8e651-110">**File > New > Project...**</span></span>

* <span data-ttu-id="8e651-111">Klicken Sie im Menü links auf **Installiert > Visual C# > Windows Desktop**.</span><span class="sxs-lookup"><span data-stu-id="8e651-111">From the left menu select **Installed > Visual C# > Windows Desktop**</span></span>

* <span data-ttu-id="8e651-112">Wählen Sie die Projektvorlage **Konsolen-App (.NET Framework)** aus.</span><span class="sxs-lookup"><span data-stu-id="8e651-112">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="8e651-113">Stellen Sie sicher, dass das Projekt **.NET Framework 4.6.1** (oder höher) anzielt.</span><span class="sxs-lookup"><span data-stu-id="8e651-113">Make sure that the project targets **.NET Framework 4.6.1** or later</span></span>

* <span data-ttu-id="8e651-114">Benennen Sie das Projekt *ConsoleApp.NewDb*, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="8e651-114">Name the project *ConsoleApp.NewDb* and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="8e651-115">Installieren von Entity Framework</span><span class="sxs-lookup"><span data-stu-id="8e651-115">Install Entity Framework</span></span>

<span data-ttu-id="8e651-116">Installieren Sie zur Verwendung von EF Core das Paket für den (oder die) gewünschten Datenbankanbieter.</span><span class="sxs-lookup"><span data-stu-id="8e651-116">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="8e651-117">In diesem Tutorial wird SQL Server verwendet.</span><span class="sxs-lookup"><span data-stu-id="8e651-117">This tutorial uses SQL Server.</span></span> <span data-ttu-id="8e651-118">Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="8e651-118">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="8e651-119">Wählen Sie „Tools“ > „NuGet-Paket-Manager“ > „Paket-Manager-Konsole“ aus.</span><span class="sxs-lookup"><span data-stu-id="8e651-119">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="8e651-120">Ausführen von `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="8e651-120">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="8e651-121">Später in diesem Tutorial verwenden Sie Entity Framework Tools zum Verwalten der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="8e651-121">Later in this tutorial you use some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="8e651-122">Installieren Sie deshalb auch das Toolpaket.</span><span class="sxs-lookup"><span data-stu-id="8e651-122">So install the tools package as well.</span></span>

* <span data-ttu-id="8e651-123">Ausführen von `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="8e651-123">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-the-model"></a><span data-ttu-id="8e651-124">Erstellen des Modells</span><span class="sxs-lookup"><span data-stu-id="8e651-124">Create the model</span></span>

<span data-ttu-id="8e651-125">Jetzt ist es Zeit, einen Kontext und Entitätsklassen für das Modell zu definieren.</span><span class="sxs-lookup"><span data-stu-id="8e651-125">Now it's time to define a context and entity classes that make up the model.</span></span>

* <span data-ttu-id="8e651-126">Klicken Sie auf **Projekt > Klasse hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="8e651-126">**Project > Add Class...**</span></span>

* <span data-ttu-id="8e651-127">Geben Sie als Name *Model.cs* ein, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="8e651-127">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="8e651-128">Ersetzen Sie den Inhalt der Datei durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="8e651-128">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] 

> [!TIP]  
> <span data-ttu-id="8e651-129">In einer echten Anwendung würden Sie jede Anwendung in einer separaten Datei platzieren und die Verbindungszeichenfolge in eine Konfigurationsdatei oder Umgebungsvariable einfügen.</span><span class="sxs-lookup"><span data-stu-id="8e651-129">In a real application you would put each class in a separate file and put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="8e651-130">Der Einfachheit halber sind in diesem Tutorial alle Elemente in einer einzigen Codedatei zusammengefasst.</span><span class="sxs-lookup"><span data-stu-id="8e651-130">For the sake of simplicity, everything is in a single code file for this tutorial.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="8e651-131">Erstellen der Datenbank</span><span class="sxs-lookup"><span data-stu-id="8e651-131">Create the database</span></span>

<span data-ttu-id="8e651-132">Nachdem Sie jetzt über ein Modell verfügen, können Sie mithilfe von Migrationen eine Datenbank erstellen.</span><span class="sxs-lookup"><span data-stu-id="8e651-132">Now that you have a model, you can use migrations to create a database.</span></span>

* <span data-ttu-id="8e651-133">Wählen Sie **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="8e651-133">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="8e651-134">Führen Sie `Add-Migration InitialCreate` aus, um per Gerüstbau eine Migration einzurichten und den anfänglichen Tabellensatz für das Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="8e651-134">Run `Add-Migration InitialCreate` to scaffold a migration to create the initial set of tables for the model.</span></span>

* <span data-ttu-id="8e651-135">Führen Sie `Update-Database` aus, um die neue Migration auf die Datenbank anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="8e651-135">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="8e651-136">Da die Datenbank noch nicht vorhanden ist, wird sie vor dem Anwenden der Migration erstellt.</span><span class="sxs-lookup"><span data-stu-id="8e651-136">Because the database doesn't exist yet, it will be created before the migration is applied.</span></span>

> [!TIP]  
> <span data-ttu-id="8e651-137">Wenn Sie Änderungen am Modell durchführen, können Sie mit dem Befehl `Add-Migration` per Gerüstbau eine neue Migration einrichten, um die entsprechenden Schemaänderungen an der Datenbank vorzunehmen.</span><span class="sxs-lookup"><span data-stu-id="8e651-137">If you make changes to the model, you can use the `Add-Migration` command to scaffold a new migration to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="8e651-138">Nachdem Sie den Gerüstcode überprüft (und alle erforderlichen Änderungen vorgenommen haben) haben, können Sie mit dem Befehl `Update-Database` Änderungen auf die Datenbank anwenden.</span><span class="sxs-lookup"><span data-stu-id="8e651-138">Once you have checked the scaffolded code (and made any required changes), you can use the `Update-Database` command to apply the changes to the database.</span></span>
>
> <span data-ttu-id="8e651-139">EF verwendet eine `__EFMigrationsHistory`-Tabelle in der Datenbank, um nachzuverfolgen, welche Migrationen bereits auf die Datenbank angewendet wurden.</span><span class="sxs-lookup"><span data-stu-id="8e651-139">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="8e651-140">Verwenden des Modells</span><span class="sxs-lookup"><span data-stu-id="8e651-140">Use the model</span></span>

<span data-ttu-id="8e651-141">Jetzt können Sie das Modell zum Zugreifen auf Daten verwenden.</span><span class="sxs-lookup"><span data-stu-id="8e651-141">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="8e651-142">Öffnen Sie *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="8e651-142">Open *Program.cs*</span></span>

* <span data-ttu-id="8e651-143">Ersetzen Sie den Inhalt der Datei durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="8e651-143">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)]

* <span data-ttu-id="8e651-144">Klicken Sie auf **Debuggen > Ohne Debuggen starten**.</span><span class="sxs-lookup"><span data-stu-id="8e651-144">**Debug > Start Without Debugging**</span></span>

  <span data-ttu-id="8e651-145">Sie werden sehen, dass ein Blog in der Datenbank gespeichert wird und dann die Details zu allen Blogs an die Konsole ausgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="8e651-145">You see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

  ![Bild](_static/output-new-db.png)

## <a name="additional-resources"></a><span data-ttu-id="8e651-147">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="8e651-147">Additional Resources</span></span>

* [<span data-ttu-id="8e651-148">EF Core in .NET Framework mit einer vorhandenen Datenbank</span><span class="sxs-lookup"><span data-stu-id="8e651-148">EF Core on .NET Framework with an existing database</span></span>](xref:core/get-started/full-dotnet/existing-db)
* <span data-ttu-id="8e651-149">[EF Core in .NET Core mit einer neuen Datenbank - SQLite](xref:core/get-started/netcore/new-db-sqlite) – Ein EF-Tutorial für beliebige Plattformkonsolen</span><span class="sxs-lookup"><span data-stu-id="8e651-149">[EF Core on .NET Core with a new database - SQLite](xref:core/get-started/netcore/new-db-sqlite) -  a cross-platform console EF tutorial.</span></span>
