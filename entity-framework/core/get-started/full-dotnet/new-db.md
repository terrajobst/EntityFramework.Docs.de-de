---
title: Erste Schritte in .NET Framework – Neue Datenbank – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: bd7054c6834ae11bfdc352d63654e4304771e432
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/26/2018
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a><span data-ttu-id="25b66-102">Erste Schritte mit EF Core in .NET Framework mit einer neuen Datenbank</span><span class="sxs-lookup"><span data-stu-id="25b66-102">Getting started with EF Core on .NET Framework with a New Database</span></span>

<span data-ttu-id="25b66-103">In dieser exemplarischen Vorgehensweise entwickeln Sie eine Konsolenanwendung, die einen grundlegenden Datenzugriff auf eine Microsoft SQL Server-Datenbank mithilfe von Entity Framework durchführt.</span><span class="sxs-lookup"><span data-stu-id="25b66-103">In this walkthrough, you will build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="25b66-104">Sie verwenden Migrationen, um die Datenbank aus Ihrem Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="25b66-104">You will use migrations to create the database from your model.</span></span>

> [!TIP]  
> <span data-ttu-id="25b66-105">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="25b66-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="25b66-106">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="25b66-106">Prerequisites</span></span>

<span data-ttu-id="25b66-107">Zum Ausführen dieser exemplarischen Vorgehensweise müssen folgende Voraussetzungen erfüllt sein:</span><span class="sxs-lookup"><span data-stu-id="25b66-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* [<span data-ttu-id="25b66-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="25b66-108">Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/)

* [<span data-ttu-id="25b66-109">Neueste Version des NuGet-Paket-Managers</span><span class="sxs-lookup"><span data-stu-id="25b66-109">Latest version of NuGet Package Manager</span></span>](https://dist.nuget.org/index.html)

* [<span data-ttu-id="25b66-110">Neueste Version von Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="25b66-110">Latest version of Windows PowerShell</span></span>](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

## <a name="create-a-new-project"></a><span data-ttu-id="25b66-111">Erstellt ein neues Projekt</span><span class="sxs-lookup"><span data-stu-id="25b66-111">Create a new project</span></span>

* <span data-ttu-id="25b66-112">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="25b66-112">Open Visual Studio</span></span>

* <span data-ttu-id="25b66-113">Klicken Sie auf „Datei“ > „Neu“ > „Projekt“.</span><span class="sxs-lookup"><span data-stu-id="25b66-113">File > New > Project...</span></span>

* <span data-ttu-id="25b66-114">Wählen Sie im Menü links „Vorlagen“ > „Visual C#“ > „Klassischer Windows-Desktop“ aus.</span><span class="sxs-lookup"><span data-stu-id="25b66-114">From the left menu select Templates > Visual C# > Windows Classic Desktop</span></span>

* <span data-ttu-id="25b66-115">Wählen Sie die Projektvorlage **Konsolen-App (.NET Framework)** aus.</span><span class="sxs-lookup"><span data-stu-id="25b66-115">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="25b66-116">Stellen Sie sicher, dass Sie als Ziel **.NET Framework 4.5.1** oder höher verwenden.</span><span class="sxs-lookup"><span data-stu-id="25b66-116">Ensure you are targeting **.NET Framework 4.5.1** or later</span></span>

* <span data-ttu-id="25b66-117">Benennen Sie das Projekt, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="25b66-117">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="25b66-118">Installieren von Entity Framework</span><span class="sxs-lookup"><span data-stu-id="25b66-118">Install Entity Framework</span></span>

<span data-ttu-id="25b66-119">Installieren Sie zur Verwendung von EF Core das Paket für den (oder die) gewünschten Datenbankanbieter.</span><span class="sxs-lookup"><span data-stu-id="25b66-119">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="25b66-120">In dieser exemplarischen Vorgehensweise wird SQL Server verwendet.</span><span class="sxs-lookup"><span data-stu-id="25b66-120">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="25b66-121">Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="25b66-121">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="25b66-122">Wählen Sie „Tools“ > „NuGet-Paket-Manager“ > „Paket-Manager-Konsole“ aus.</span><span class="sxs-lookup"><span data-stu-id="25b66-122">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="25b66-123">Ausführen von `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="25b66-123">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="25b66-124">Später in dieser exemplarischen Vorgehensweise werden einige Entity Framework-Tools zum Verwalten der Datenbank eingesetzt.</span><span class="sxs-lookup"><span data-stu-id="25b66-124">Later in this walkthrough we will also be using some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="25b66-125">Deshalb installieren wir auch das Toolpaket.</span><span class="sxs-lookup"><span data-stu-id="25b66-125">So we will install the tools package as well.</span></span>

* <span data-ttu-id="25b66-126">Ausführen von `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="25b66-126">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-your-model"></a><span data-ttu-id="25b66-127">Erstellen Ihres Modells</span><span class="sxs-lookup"><span data-stu-id="25b66-127">Create your model</span></span>

<span data-ttu-id="25b66-128">Jetzt ist es Zeit, einen Kontext und Entitätsklassen für Ihr Modell zu definieren.</span><span class="sxs-lookup"><span data-stu-id="25b66-128">Now it's time to define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="25b66-129">Klicken Sie auf „Projekt“ > „Klasse hinzufügen“.</span><span class="sxs-lookup"><span data-stu-id="25b66-129">Project > Add Class...</span></span>

* <span data-ttu-id="25b66-130">Geben Sie als Name *Model.cs* ein, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="25b66-130">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="25b66-131">Ersetzen Sie den Inhalt der Datei durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="25b66-131">Replace the contents of the file with the following code</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }

        public List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
}
```

> [!TIP]  
> <span data-ttu-id="25b66-132">In einer echten Anwendung würden Sie jede Anwendung in einer separaten Datei platzieren und die Verbindungszeichenfolge in die Datei `App.Config` einfügen und sie mithilfe von `ConfigurationManager` auslesen.</span><span class="sxs-lookup"><span data-stu-id="25b66-132">In a real application you would put each class in a separate file and put the connection string in the `App.Config` file and read it out using `ConfigurationManager`.</span></span> <span data-ttu-id="25b66-133">Der Einfachheit halber werden in diesem Tutorial alle Elemente in einer einzigen Codedatei zusammengefasst.</span><span class="sxs-lookup"><span data-stu-id="25b66-133">For the sake of simplicity, we are putting everything in a single code file for this tutorial.</span></span>

## <a name="create-your-database"></a><span data-ttu-id="25b66-134">Erstellen Ihrer Datenbank</span><span class="sxs-lookup"><span data-stu-id="25b66-134">Create your database</span></span>

<span data-ttu-id="25b66-135">Nachdem Sie jetzt über ein Modell verfügen, können Sie mithilfe von Migrationen eine Datenbank erstellen.</span><span class="sxs-lookup"><span data-stu-id="25b66-135">Now that you have a model, you can use migrations to create a database for you.</span></span>

* <span data-ttu-id="25b66-136">Wählen Sie „Tools“ > „NuGet-Paket-Manager“ > „Paket-Manager-Konsole“ aus.</span><span class="sxs-lookup"><span data-stu-id="25b66-136">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="25b66-137">Führen Sie `Add-Migration MyFirstMigration` aus, um per Gerüstbau eine Migration zum Erstellen des anfänglichen Tabellensatzes für Ihr Modell einzurichten.</span><span class="sxs-lookup"><span data-stu-id="25b66-137">Run `Add-Migration MyFirstMigration` to scaffold a migration to create the initial set of tables for your model.</span></span>

* <span data-ttu-id="25b66-138">Führen Sie `Update-Database` aus, um die neue Migration auf die Datenbank anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="25b66-138">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="25b66-139">Ihre Datenbank ist noch nicht vorhanden, deshalb wird sie vor dem Anwenden der Migration erstellt.</span><span class="sxs-lookup"><span data-stu-id="25b66-139">Because your database doesn't exist yet, it will be created for you before the migration is applied.</span></span>

> [!TIP]  
> <span data-ttu-id="25b66-140">Wenn Sie später Änderungen an Ihrem Modell durchführen, können Sie mit dem Befehl `Add-Migration` per Gerüstbau eine neue Migration einrichten, um die entsprechenden Schemaänderungen an der Datenbank vorzunehmen.</span><span class="sxs-lookup"><span data-stu-id="25b66-140">If you make future changes to your model, you can use the `Add-Migration` command to scaffold a new migration to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="25b66-141">Nachdem Sie den Gerüstcode überprüft (und alle erforderlichen Änderungen vorgenommen haben) haben, können Sie mit dem Befehl `Update-Database` Änderungen auf die Datenbank anwenden.</span><span class="sxs-lookup"><span data-stu-id="25b66-141">Once you have checked the scaffolded code (and made any required changes), you can use the `Update-Database` command to apply the changes to the database.</span></span>
>
><span data-ttu-id="25b66-142">EF verwendet eine `__EFMigrationsHistory`-Tabelle in der Datenbank, um nachzuverfolgen, welche Migrationen bereits auf die Datenbank angewendet wurden.</span><span class="sxs-lookup"><span data-stu-id="25b66-142">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="25b66-143">Verwenden Ihres Modells</span><span class="sxs-lookup"><span data-stu-id="25b66-143">Use your model</span></span>

<span data-ttu-id="25b66-144">Jetzt können Sie Ihr Modell zum Zugreifen auf Daten verwenden.</span><span class="sxs-lookup"><span data-stu-id="25b66-144">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="25b66-145">Öffnen Sie *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="25b66-145">Open *Program.cs*</span></span>

* <span data-ttu-id="25b66-146">Ersetzen Sie den Inhalt der Datei durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="25b66-146">Replace the contents of the file with the following code</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blogs)
                {
                    Console.WriteLine(" - {0}", blog.Url);
                }
            }
        }
    }
}
```

* <span data-ttu-id="25b66-147">Klicken Sie auf „Debuggen“ > „Ohne Debuggen starten“.</span><span class="sxs-lookup"><span data-stu-id="25b66-147">Debug > Start Without Debugging</span></span>

<span data-ttu-id="25b66-148">Sie werden sehen, dass ein Blog in der Datenbank gespeichert wird und dann die Details zu allen Blogs an die Konsole ausgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="25b66-148">You will see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

![Bild](_static/output-new-db.png)
