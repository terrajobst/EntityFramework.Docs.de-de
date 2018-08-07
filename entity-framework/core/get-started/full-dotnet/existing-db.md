---
title: Erste Schritte in .NET Framework – Vorhandene Datenbank – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: 39e77ab8c124df67458cc5fa6db2882b65943ebe
ms.sourcegitcommit: 4467032fd6ca223e5965b59912d74cf88a1dd77f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2018
ms.locfileid: "39388467"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a><span data-ttu-id="aaa08-102">Erste Schritte mit EF Core in .NET Framework mit einer vorhandenen Datenbank</span><span class="sxs-lookup"><span data-stu-id="aaa08-102">Getting started with EF Core on .NET Framework with an Existing Database</span></span>

<span data-ttu-id="aaa08-103">In dieser exemplarischen Vorgehensweise entwickeln Sie eine Konsolenanwendung, die einen grundlegenden Datenzugriff auf eine Microsoft SQL Server-Datenbank mithilfe von Entity Framework durchführt.</span><span class="sxs-lookup"><span data-stu-id="aaa08-103">In this walkthrough, you will build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="aaa08-104">Sie erstellen per Reverse Engineering ein Entity Framework-Modell, das auf einer vorhandenen Datenbank basiert.</span><span class="sxs-lookup"><span data-stu-id="aaa08-104">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="aaa08-105">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="aaa08-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aaa08-106">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="aaa08-106">Prerequisites</span></span>

<span data-ttu-id="aaa08-107">Zum Ausführen dieser exemplarischen Vorgehensweise müssen folgende Voraussetzungen erfüllt sein:</span><span class="sxs-lookup"><span data-stu-id="aaa08-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* <span data-ttu-id="aaa08-108">[Visual Studio 2017](https://www.visualstudio.com/downloads/) – mindestens Version 15.3</span><span class="sxs-lookup"><span data-stu-id="aaa08-108">[Visual Studio 2017](https://www.visualstudio.com/downloads/) - at least version 15.3</span></span>

* [<span data-ttu-id="aaa08-109">Neueste Version des NuGet-Paket-Managers</span><span class="sxs-lookup"><span data-stu-id="aaa08-109">Latest version of NuGet Package Manager</span></span>](https://dist.nuget.org/index.html)

* [<span data-ttu-id="aaa08-110">Neueste Version von Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="aaa08-110">Latest version of Windows PowerShell</span></span>](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

* [<span data-ttu-id="aaa08-111">Blogging-Datenbank</span><span class="sxs-lookup"><span data-stu-id="aaa08-111">Blogging database</span></span>](#blogging-database)

### <a name="blogging-database"></a><span data-ttu-id="aaa08-112">Blogging-Datenbank</span><span class="sxs-lookup"><span data-stu-id="aaa08-112">Blogging database</span></span>

<span data-ttu-id="aaa08-113">In diesem Tutorial wird als vorhandene Datenbank eine **Blogging**-Datenbank in Ihrer LocalDb-Instanz verwendet.</span><span class="sxs-lookup"><span data-stu-id="aaa08-113">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="aaa08-114">Wenn Sie die **Blogging**-Datenbank bereits im Rahmen eines anderen Tutorials erstellt haben, können Sie diese Schritte überspringen.</span><span class="sxs-lookup"><span data-stu-id="aaa08-114">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="aaa08-115">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="aaa08-115">Open Visual Studio</span></span>

* <span data-ttu-id="aaa08-116">Klicken Sie auf „Tools“ > „Verbindung mit Datenbank herstellen“.</span><span class="sxs-lookup"><span data-stu-id="aaa08-116">Tools > Connect to Database...</span></span>

* <span data-ttu-id="aaa08-117">Wählen Sie **Microsoft SQL Server** aus, und klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="aaa08-117">Select **Microsoft SQL Server** and click **Continue**</span></span>

* <span data-ttu-id="aaa08-118">Geben Sie **(localdb)\mssqllocaldb** als **Servername** ein.</span><span class="sxs-lookup"><span data-stu-id="aaa08-118">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>

* <span data-ttu-id="aaa08-119">Geben Sie als **Datenbankname** den Wert **master** ein, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="aaa08-119">Enter **master** as the **Database Name** and click **OK**</span></span>

* <span data-ttu-id="aaa08-120">Die master-Datenbank wird jetzt unter **Datenverbindungen** im **Server-Explorer** angezeigt.</span><span class="sxs-lookup"><span data-stu-id="aaa08-120">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>

* <span data-ttu-id="aaa08-121">Klicken Sie im **Server-Explorer** mit der rechten Maustaste auf die Datenbank, und wählen Sie **Neue Abfrage** aus.</span><span class="sxs-lookup"><span data-stu-id="aaa08-121">Right-click on the database in **Server Explorer** and select **New Query**</span></span>

* <span data-ttu-id="aaa08-122">Kopieren Sie das nachstehende Skript in den Abfrage-Editor.</span><span class="sxs-lookup"><span data-stu-id="aaa08-122">Copy the script, listed below, into the query editor</span></span>

* <span data-ttu-id="aaa08-123">Klicken Sie mit der rechten Maustaste auf den Abfrage-Editor, und wählen Sie **Ausführen** aus.</span><span class="sxs-lookup"><span data-stu-id="aaa08-123">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="aaa08-124">Erstellt ein neues Projekt</span><span class="sxs-lookup"><span data-stu-id="aaa08-124">Create a new project</span></span>

* <span data-ttu-id="aaa08-125">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="aaa08-125">Open Visual Studio</span></span>

* <span data-ttu-id="aaa08-126">Klicken Sie auf „Datei“ > „Neu“ > „Projekt“.</span><span class="sxs-lookup"><span data-stu-id="aaa08-126">File > New > Project...</span></span>

* <span data-ttu-id="aaa08-127">Wählen Sie im Menü links „Vorlagen“ > „Visual C#“ > „Windows“ aus.</span><span class="sxs-lookup"><span data-stu-id="aaa08-127">From the left menu select Templates > Visual C# > Windows</span></span>

* <span data-ttu-id="aaa08-128">Wählen Sie die Projektvorlage **Konsolenanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="aaa08-128">Select the **Console Application** project template</span></span>

* <span data-ttu-id="aaa08-129">Stellen Sie sicher, dass Sie **.NET Framework 4.6.1** oder höher als Ziel verwenden.</span><span class="sxs-lookup"><span data-stu-id="aaa08-129">Ensure you are targeting **.NET Framework 4.6.1** or later</span></span>

* <span data-ttu-id="aaa08-130">Benennen Sie das Projekt, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="aaa08-130">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="aaa08-131">Installieren von Entity Framework</span><span class="sxs-lookup"><span data-stu-id="aaa08-131">Install Entity Framework</span></span>

<span data-ttu-id="aaa08-132">Installieren Sie zur Verwendung von EF Core das Paket für den (oder die) gewünschten Datenbankanbieter.</span><span class="sxs-lookup"><span data-stu-id="aaa08-132">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="aaa08-133">In dieser exemplarischen Vorgehensweise wird SQL Server verwendet.</span><span class="sxs-lookup"><span data-stu-id="aaa08-133">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="aaa08-134">Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="aaa08-134">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="aaa08-135">Wählen Sie „Tools“ > „NuGet-Paket-Manager“ > „Paket-Manager-Konsole“ aus.</span><span class="sxs-lookup"><span data-stu-id="aaa08-135">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="aaa08-136">Ausführen von `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="aaa08-136">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="aaa08-137">Um das Reverse Engineering aus einer vorhandenen Datenbank zu aktivieren, müssen einige weitere Pakete installiert werden.</span><span class="sxs-lookup"><span data-stu-id="aaa08-137">To enable reverse engineering from an existing database we need to install a couple of other packages too.</span></span>

* <span data-ttu-id="aaa08-138">Ausführen von `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="aaa08-138">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="aaa08-139">Zurückentwickeln (Reverse Engineering) Ihres Modells</span><span class="sxs-lookup"><span data-stu-id="aaa08-139">Reverse engineer your model</span></span>

<span data-ttu-id="aaa08-140">Jetzt ist es Zeit, basierend auf Ihrer vorhandenen Datenbank das EF-Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="aaa08-140">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="aaa08-141">Wählen Sie „Tools“ > „NuGet-Paket-Manager“ > „Paket-Manager-Konsole“ aus.</span><span class="sxs-lookup"><span data-stu-id="aaa08-141">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="aaa08-142">Führen Sie den folgenden Befehl aus, um ein Modell aus der vorhandenen Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="aaa08-142">Run the following command to create a model from the existing database</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="aaa08-143">Der Reverse Engineering-Prozess hat Entitätsklassen und einen abgeleiteten Kontext basierend auf dem Schema der vorhandenen Datenbank erstellt.</span><span class="sxs-lookup"><span data-stu-id="aaa08-143">The reverse engineer process created entity classes and a derived context based on the schema of the existing database.</span></span> <span data-ttu-id="aaa08-144">Die Entitätsklassen sind einfache C#-Objekte zum Repräsentieren der Daten, die Sie abfragen und speichern werden.</span><span class="sxs-lookup"><span data-stu-id="aaa08-144">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)] -->
``` csharp
using System;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class Blog
    {
        public Blog()
        {
            Post = new HashSet<Post>();
        }

        public int BlogId { get; set; }
        public string Url { get; set; }

        public virtual ICollection<Post> Post { get; set; }
    }
}
```

<span data-ttu-id="aaa08-145">Der Kontext repräsentiert eine Sitzung mit der Datenbank und ermöglicht Ihnen das Abfragen und Speichern von Instanzen der Entitätsklassen.</span><span class="sxs-lookup"><span data-stu-id="aaa08-145">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Metadata;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class BloggingContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Blog>(entity =>
            {
                entity.Property(e => e.Url).IsRequired();
            });

            modelBuilder.Entity<Post>(entity =>
            {
                entity.HasOne(d => d.Blog)
                    .WithMany(p => p.Post)
                    .HasForeignKey(d => d.BlogId);
            });
        }

        public virtual DbSet<Blog> Blog { get; set; }
        public virtual DbSet<Post> Post { get; set; }
    }
}
```

## <a name="use-your-model"></a><span data-ttu-id="aaa08-146">Verwenden Ihres Modells</span><span class="sxs-lookup"><span data-stu-id="aaa08-146">Use your model</span></span>

<span data-ttu-id="aaa08-147">Jetzt können Sie Ihr Modell zum Zugreifen auf Daten verwenden.</span><span class="sxs-lookup"><span data-stu-id="aaa08-147">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="aaa08-148">Öffnen Sie *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="aaa08-148">Open *Program.cs*</span></span>

* <span data-ttu-id="aaa08-149">Ersetzen Sie den Inhalt der Datei durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="aaa08-149">Replace the contents of the file with the following code</span></span>

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blog.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blog)
                {
                    Console.WriteLine(" - {0}", blog.Url);
                }
            }
        }
    }
}
```

* <span data-ttu-id="aaa08-150">Klicken Sie auf „Debuggen“ > „Ohne Debuggen starten“.</span><span class="sxs-lookup"><span data-stu-id="aaa08-150">Debug > Start Without Debugging</span></span>

<span data-ttu-id="aaa08-151">Sie werden sehen, dass ein Blog in der Datenbank gespeichert wird und dann die Details zu allen Blogs an die Konsole ausgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="aaa08-151">You will see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

![Bild](_static/output-existing-db.png)
