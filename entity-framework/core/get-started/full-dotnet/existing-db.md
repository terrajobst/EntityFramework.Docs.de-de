---
title: Erste Schritte in .NET Framework – Vorhandene Datenbank – EF Core
author: rowanmiller
ms.date: 08/06/2018
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: 1b90c491a3b2025da750a3266ff45d9d92bb1d0d
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688614"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a><span data-ttu-id="99dee-102">Erste Schritte mit EF Core in .NET Framework mit einer vorhandenen Datenbank</span><span class="sxs-lookup"><span data-stu-id="99dee-102">Getting started with EF Core on .NET Framework with an Existing Database</span></span>

<span data-ttu-id="99dee-103">In diesem Tutorial erstellen Sie eine Konsolenanwendung, die einen grundlegenden Datenzugriff auf eine Microsoft SQL Server-Datenbank mithilfe von Entity Framework durchführt.</span><span class="sxs-lookup"><span data-stu-id="99dee-103">In this tutorial, you build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="99dee-104">Sie erstellen ein Entity Framework-Modell, indem Sie ein Reverse Engineering für eine vorhandene Datenbank durchführen.</span><span class="sxs-lookup"><span data-stu-id="99dee-104">You create an Entity Framework model by reverse engineering an existing database.</span></span>

<span data-ttu-id="99dee-105">[Sehen Sie sich das Beispiel aus diesem Artikel auf GitHub an](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).</span><span class="sxs-lookup"><span data-stu-id="99dee-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="99dee-106">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="99dee-106">Prerequisites</span></span>

* [<span data-ttu-id="99dee-107">Visual Studio 2017 Version 15.7 (oder höher)</span><span class="sxs-lookup"><span data-stu-id="99dee-107">Visual Studio 2017 version 15.7 or later</span></span>](https://www.visualstudio.com/downloads/)

## <a name="create-blogging-database"></a><span data-ttu-id="99dee-108">Erstellen einer Blogging-Datenbank</span><span class="sxs-lookup"><span data-stu-id="99dee-108">Create Blogging database</span></span>

<span data-ttu-id="99dee-109">In diesem Tutorial wird als vorhandene Datenbank eine **Blogging**-Datenbank in der LocalDb-Instanz verwendet.</span><span class="sxs-lookup"><span data-stu-id="99dee-109">This tutorial uses a **Blogging** database on the LocalDb instance as the existing database.</span></span> <span data-ttu-id="99dee-110">Wenn Sie die **Blogging**-Datenbank bereits im Rahmen eines anderen Tutorials erstellt haben, können Sie diese Schritte überspringen.</span><span class="sxs-lookup"><span data-stu-id="99dee-110">If you have already created the **Blogging** database as part of another tutorial, skip these steps.</span></span>

* <span data-ttu-id="99dee-111">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="99dee-111">Open Visual Studio</span></span>

* <span data-ttu-id="99dee-112">Klicken Sie auf **Extras > Verbindung mit Datenbank herstellen**.</span><span class="sxs-lookup"><span data-stu-id="99dee-112">**Tools > Connect to Database...**</span></span>

* <span data-ttu-id="99dee-113">Wählen Sie **Microsoft SQL Server** aus, und klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="99dee-113">Select **Microsoft SQL Server** and click **Continue**</span></span>

* <span data-ttu-id="99dee-114">Geben Sie **(localdb)\mssqllocaldb** als **Servername** ein.</span><span class="sxs-lookup"><span data-stu-id="99dee-114">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>

* <span data-ttu-id="99dee-115">Geben Sie als **Datenbankname** den Wert **master** ein, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="99dee-115">Enter **master** as the **Database Name** and click **OK**</span></span>

* <span data-ttu-id="99dee-116">Die master-Datenbank wird jetzt unter **Datenverbindungen** im **Server-Explorer** angezeigt.</span><span class="sxs-lookup"><span data-stu-id="99dee-116">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>

* <span data-ttu-id="99dee-117">Klicken Sie im **Server-Explorer** mit der rechten Maustaste auf die Datenbank, und wählen Sie **Neue Abfrage** aus.</span><span class="sxs-lookup"><span data-stu-id="99dee-117">Right-click on the database in **Server Explorer** and select **New Query**</span></span>

* <span data-ttu-id="99dee-118">Kopieren Sie das nachstehende Skript in den Abfrage-Editor.</span><span class="sxs-lookup"><span data-stu-id="99dee-118">Copy the script listed below into the query editor</span></span>

* <span data-ttu-id="99dee-119">Klicken Sie mit der rechten Maustaste auf den Abfrage-Editor, und wählen Sie **Ausführen** aus.</span><span class="sxs-lookup"><span data-stu-id="99dee-119">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="99dee-120">Erstellt ein neues Projekt</span><span class="sxs-lookup"><span data-stu-id="99dee-120">Create a new project</span></span>

* <span data-ttu-id="99dee-121">Öffnen Sie Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="99dee-121">Open Visual Studio 2017</span></span>

* <span data-ttu-id="99dee-122">Klicken Sie auf **Datei > Neu > Projekt...**.</span><span class="sxs-lookup"><span data-stu-id="99dee-122">**File > New > Project...**</span></span>

* <span data-ttu-id="99dee-123">Klicken Sie im Menü links auf **Installiert > Visual C# > Windows Desktop**.</span><span class="sxs-lookup"><span data-stu-id="99dee-123">From the left menu select **Installed > Visual C# > Windows Desktop**</span></span>

* <span data-ttu-id="99dee-124">Wählen Sie die Projektvorlage **Konsolen-App (.NET Framework)** aus.</span><span class="sxs-lookup"><span data-stu-id="99dee-124">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="99dee-125">Stellen Sie sicher, dass das Projekt **.NET Framework 4.6.1** (oder höher) anzielt.</span><span class="sxs-lookup"><span data-stu-id="99dee-125">Make sure that the project targets **.NET Framework 4.6.1** or later</span></span>

* <span data-ttu-id="99dee-126">Benennen Sie das Projekt *ConsoleApp.ExistingDb*, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="99dee-126">Name the project *ConsoleApp.ExistingDb* and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="99dee-127">Installieren von Entity Framework</span><span class="sxs-lookup"><span data-stu-id="99dee-127">Install Entity Framework</span></span>

<span data-ttu-id="99dee-128">Installieren Sie zur Verwendung von EF Core das Paket für den (oder die) gewünschten Datenbankanbieter.</span><span class="sxs-lookup"><span data-stu-id="99dee-128">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="99dee-129">In diesem Tutorial wird SQL Server verwendet.</span><span class="sxs-lookup"><span data-stu-id="99dee-129">This tutorial uses SQL Server.</span></span> <span data-ttu-id="99dee-130">Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="99dee-130">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="99dee-131">Wählen Sie **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="99dee-131">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="99dee-132">Ausführen von `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="99dee-132">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="99dee-133">Im nächsten Schritt verwenden Sie einige Entity Framework Tools, um ein Reverse Engineering für die Datenbank durchzuführen.</span><span class="sxs-lookup"><span data-stu-id="99dee-133">In the next step, you use some Entity Framework Tools to reverse engineer the database.</span></span> <span data-ttu-id="99dee-134">Installieren Sie deshalb auch das Toolpaket.</span><span class="sxs-lookup"><span data-stu-id="99dee-134">So install the tools package as well.</span></span>

* <span data-ttu-id="99dee-135">Ausführen von `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="99dee-135">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="reverse-engineer-the-model"></a><span data-ttu-id="99dee-136">Reverse Engineering des Modells</span><span class="sxs-lookup"><span data-stu-id="99dee-136">Reverse engineer the model</span></span>

<span data-ttu-id="99dee-137">Jetzt ist es Zeit, das EF-Modell basierend auf einer vorhandenen Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="99dee-137">Now it's time to create the EF model based on an existing database.</span></span>

* <span data-ttu-id="99dee-138">Wählen Sie **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="99dee-138">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>

* <span data-ttu-id="99dee-139">Führen Sie den folgenden Befehl aus, um ein Modell aus der vorhandenen Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="99dee-139">Run the following command to create a model from the existing database</span></span>

  ``` powershell
  Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
  ```

> [!TIP]  
> <span data-ttu-id="99dee-140">Indem Sie das `-Tables`-Argument zum obigen Befehl hinzufügen, können Sie angeben, für welche Tabellen Sie Entitäten generieren möchten.</span><span class="sxs-lookup"><span data-stu-id="99dee-140">You can specify the tables to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="99dee-141">Beispielsweise `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="99dee-141">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="99dee-142">Der Reverse Engineering-Prozess hat Entitätsklassen (`Blog` und `Post`) und einen abgeleiteten Kontext (`BloggingContext`) basierend auf dem Schema der vorhandenen Datenbank erstellt.</span><span class="sxs-lookup"><span data-stu-id="99dee-142">The reverse engineer process created entity classes (`Blog` and `Post`) and a derived context (`BloggingContext`) based on the schema of the existing database.</span></span>

<span data-ttu-id="99dee-143">Die Entitätsklassen sind einfache C#-Objekte zum Repräsentieren der Daten, die Sie abfragen und speichern werden.</span><span class="sxs-lookup"><span data-stu-id="99dee-143">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span> <span data-ttu-id="99dee-144">Hier sind die Entitätsklassen `Blog` und `Post`:</span><span class="sxs-lookup"><span data-stu-id="99dee-144">Here are the `Blog` and `Post` entity classes:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Post.cs)]

> [!TIP]  
> <span data-ttu-id="99dee-145">Sie können Navigationseigenschaften auf `virtual` festlegen (Blog.Post und Post.Blog), um Lazy Loading zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="99dee-145">To enable lazy loading, you can make navigation properties `virtual` (Blog.Post and Post.Blog).</span></span>

<span data-ttu-id="99dee-146">Der Kontext stellt eine Sitzung mit der Datenbank dar.</span><span class="sxs-lookup"><span data-stu-id="99dee-146">The context represents a session with the database.</span></span> <span data-ttu-id="99dee-147">Sie verfügt über Methoden, die Sie zum Abfragen und Speichern von Instanzen der Entitätsklassen verwenden können.</span><span class="sxs-lookup"><span data-stu-id="99dee-147">It has methods that you can use to query and save instances of the entity classes.</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)]

## <a name="use-the-model"></a><span data-ttu-id="99dee-148">Verwenden des Modells</span><span class="sxs-lookup"><span data-stu-id="99dee-148">Use the model</span></span>

<span data-ttu-id="99dee-149">Jetzt können Sie das Modell zum Zugreifen auf Daten verwenden.</span><span class="sxs-lookup"><span data-stu-id="99dee-149">You can now use the model to perform data access.</span></span>

* <span data-ttu-id="99dee-150">Öffnen Sie *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="99dee-150">Open *Program.cs*</span></span>

* <span data-ttu-id="99dee-151">Ersetzen Sie den Inhalt der Datei durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="99dee-151">Replace the contents of the file with the following code</span></span>

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] 

* <span data-ttu-id="99dee-152">Klicken Sie auf „Debuggen“ > „Ohne Debuggen starten“.</span><span class="sxs-lookup"><span data-stu-id="99dee-152">Debug > Start Without Debugging</span></span>

  <span data-ttu-id="99dee-153">Sie werden sehen, dass ein Blog in der Datenbank gespeichert wird und dann die Details zu allen Blogs an die Konsole ausgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="99dee-153">You see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

  ![Bild](_static/output-existing-db.png)

## <a name="next-steps"></a><span data-ttu-id="99dee-155">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="99dee-155">Next steps</span></span>

<span data-ttu-id="99dee-156">Weitere Informationen über das Erstellen des Gerüsts für einen Kontext und Entitätsklassen finden Sie in den folgenden Artikeln:</span><span class="sxs-lookup"><span data-stu-id="99dee-156">For more information about how to scaffold a context and entity classes, see the following articles:</span></span>
* [<span data-ttu-id="99dee-157">Reverse Engineering</span><span class="sxs-lookup"><span data-stu-id="99dee-157">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
* [<span data-ttu-id="99dee-158">Entity Framework Core Tools-Referenz – .NET-CLI</span><span class="sxs-lookup"><span data-stu-id="99dee-158">Entity Framework Core tools reference - .NET CLI</span></span>](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)
* [<span data-ttu-id="99dee-159">Entity Framework Core Tools-Referenz – Paket-Manager-Konsole</span><span class="sxs-lookup"><span data-stu-id="99dee-159">Entity Framework Core tools reference - Package Manager Console</span></span>](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext)