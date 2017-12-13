---
title: "Erste Schritte in ASP.NET Core – Vorhandene Datenbank – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: afd99d68d2ba25ce58a21dc48d2c7ce27f208807
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="d6822-102">Erste Schritte mit EF Core in ASP.NET Core mit einer vorhandenen Datenbank</span><span class="sxs-lookup"><span data-stu-id="d6822-102">Getting Started with EF Core on ASP.NET Core with an Existing Database</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="d6822-103">`project.json` oder Visual Studio 2015 werden vom [.NET Core SDK](https://www.microsoft.com/net/download/core) nicht mehr unterstützt.</span><span class="sxs-lookup"><span data-stu-id="d6822-103">The [.NET Core SDK](https://www.microsoft.com/net/download/core) no longer supports `project.json` or Visual Studio 2015.</span></span> <span data-ttu-id="d6822-104">Allen .NET Core-Entwicklern wird empfohlen, eine [Migration von „project.json“ zu „csproj“](https://docs.microsoft.com/dotnet/articles/core/migration/) und [Visual Studio 2017](https://www.visualstudio.com/downloads/) durchzuführen.</span><span class="sxs-lookup"><span data-stu-id="d6822-104">Everyone doing .NET Core development is encouraged to [migrate from project.json to csproj](https://docs.microsoft.com/dotnet/articles/core/migration/) and [Visual Studio 2017](https://www.visualstudio.com/downloads/).</span></span>

<span data-ttu-id="d6822-105">In dieser exemplarischen Vorgehensweise entwickeln Sie eine ASP.NET Core MVC-Anwendung, die einen grundlegenden Datenzugriff mit Entity Framework durchführt.</span><span class="sxs-lookup"><span data-stu-id="d6822-105">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework.</span></span> <span data-ttu-id="d6822-106">Sie erstellen per Reverse Engineering ein Entity Framework-Modell, das auf einer vorhandenen Datenbank basiert.</span><span class="sxs-lookup"><span data-stu-id="d6822-106">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="d6822-107">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="d6822-107">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d6822-108">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="d6822-108">Prerequisites</span></span>

<span data-ttu-id="d6822-109">Zum Ausführen dieser exemplarischen Vorgehensweise müssen folgende Voraussetzungen erfüllt sein:</span><span class="sxs-lookup"><span data-stu-id="d6822-109">The following prerequisites are needed to complete this walkthrough:</span></span>

* <span data-ttu-id="d6822-110">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) mit diesen Workloads:</span><span class="sxs-lookup"><span data-stu-id="d6822-110">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="d6822-111">**ASP.NET- und Webentwicklung** (unter **Web & Cloud**)</span><span class="sxs-lookup"><span data-stu-id="d6822-111">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="d6822-112">**Plattformübergreifende .NET Core-Entwicklung** (unter **Andere Toolsets**)</span><span class="sxs-lookup"><span data-stu-id="d6822-112">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="d6822-113">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="d6822-113">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>
* [<span data-ttu-id="d6822-114">Blogging-Datenbank</span><span class="sxs-lookup"><span data-stu-id="d6822-114">Blogging database</span></span>](#blogging-database)

### <a name="blogging-database"></a><span data-ttu-id="d6822-115">Blogging-Datenbank</span><span class="sxs-lookup"><span data-stu-id="d6822-115">Blogging database</span></span>

<span data-ttu-id="d6822-116">In diesem Tutorial wird als vorhandene Datenbank eine **Blogging**-Datenbank in Ihrer LocalDb-Instanz verwendet.</span><span class="sxs-lookup"><span data-stu-id="d6822-116">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="d6822-117">Wenn Sie die **Blogging**-Datenbank bereits im Rahmen eines anderen Tutorials erstellt haben, können Sie diese Schritte überspringen.</span><span class="sxs-lookup"><span data-stu-id="d6822-117">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="d6822-118">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d6822-118">Open Visual Studio</span></span>
* <span data-ttu-id="d6822-119">Klicken Sie auf **Tools > Verbindung mit Datenbank herstellen**.</span><span class="sxs-lookup"><span data-stu-id="d6822-119">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="d6822-120">Wählen Sie **Microsoft SQL Server** aus, und klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="d6822-120">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="d6822-121">Geben Sie **(localdb)\mssqllocaldb** als **Servername** ein.</span><span class="sxs-lookup"><span data-stu-id="d6822-121">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="d6822-122">Geben Sie als **Datenbankname** den Wert **master** ein, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="d6822-122">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="d6822-123">Die master-Datenbank wird jetzt unter **Datenverbindungen** im **Server-Explorer** angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d6822-123">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="d6822-124">Klicken Sie im **Server-Explorer** mit der rechten Maustaste auf die Datenbank, und wählen Sie **Neue Abfrage** aus.</span><span class="sxs-lookup"><span data-stu-id="d6822-124">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="d6822-125">Kopieren Sie das nachstehende Skript in den Abfrage-Editor.</span><span class="sxs-lookup"><span data-stu-id="d6822-125">Copy the script, listed below, into the query editor</span></span>
* <span data-ttu-id="d6822-126">Klicken Sie mit der rechten Maustaste auf den Abfrage-Editor, und wählen Sie **Ausführen** aus.</span><span class="sxs-lookup"><span data-stu-id="d6822-126">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="d6822-127">Erstellen eines neuen Projekts</span><span class="sxs-lookup"><span data-stu-id="d6822-127">Create a new project</span></span>

* <span data-ttu-id="d6822-128">Öffnen Sie Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="d6822-128">Open Visual Studio 2017</span></span>
* <span data-ttu-id="d6822-129">Klicken Sie auf **Datei > Neu > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="d6822-129">**File -> New -> Project...**</span></span>
* <span data-ttu-id="d6822-130">Wählen Sie im Menü links **Installiert > Vorlagen > Visual C# > Web** aus.</span><span class="sxs-lookup"><span data-stu-id="d6822-130">From the left menu select **Installed -> Templates -> Visual C# -> Web**</span></span>
* <span data-ttu-id="d6822-131">Wählen Sie die Projektvorlage **ASP.NET Core-Webanwendung (.NET Core)** aus.</span><span class="sxs-lookup"><span data-stu-id="d6822-131">Select the **ASP.NET Core Web Application (.NET Core)** project template</span></span>
* <span data-ttu-id="d6822-132">Geben Sie als Name **EFGetStarted.AspNetCore.ExistingDb** ein, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="d6822-132">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name and click **OK**</span></span>
* <span data-ttu-id="d6822-133">Warten Sie, bis das Dialogfeld **Neue ASP.NET Core-Webanwendung** angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="d6822-133">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="d6822-134">Wählen Sie unter **ASP.NET Core Templates 2.0** die Vorlage **Webanwendung (Model-View-Controller)** aus.</span><span class="sxs-lookup"><span data-stu-id="d6822-134">Under **ASP.NET Core Templates 2.0** select the **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="d6822-135">Stellen Sie sicher, dass **Authentifizierung** auf **Keine Authentifizierung** festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="d6822-135">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="d6822-136">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="d6822-136">Click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="d6822-137">Installieren von Entity Framework</span><span class="sxs-lookup"><span data-stu-id="d6822-137">Install Entity Framework</span></span>

<span data-ttu-id="d6822-138">Installieren Sie zur Verwendung von EF Core das Paket für den (oder die) gewünschten Datenbankanbieter.</span><span class="sxs-lookup"><span data-stu-id="d6822-138">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="d6822-139">In dieser exemplarischen Vorgehensweise wird SQL Server verwendet.</span><span class="sxs-lookup"><span data-stu-id="d6822-139">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="d6822-140">Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="d6822-140">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="d6822-141">Wählen Sie **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="d6822-141">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="d6822-142">Führen Sie `Install-Package Microsoft.EntityFrameworkCore.SqlServer` aus.</span><span class="sxs-lookup"><span data-stu-id="d6822-142">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="d6822-143">Wir werden einige Entity Framework-Tools verwenden, um ein Modell aus der Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d6822-143">We will be using some Entity Framework Tools to create a model from the database.</span></span> <span data-ttu-id="d6822-144">Deshalb installieren wir auch das Toolpaket:</span><span class="sxs-lookup"><span data-stu-id="d6822-144">So we will install the tools package as well:</span></span>

* <span data-ttu-id="d6822-145">Führen Sie `Install-Package Microsoft.EntityFrameworkCore.Tools` aus.</span><span class="sxs-lookup"><span data-stu-id="d6822-145">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="d6822-146">Später verwenden wir einige ASP.NET Core-Gerüsttools, um Controller und Ansichten zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d6822-146">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="d6822-147">Deshalb installieren wird auch dieses Entwurfspaket:</span><span class="sxs-lookup"><span data-stu-id="d6822-147">So we will install this design package as well:</span></span>

* <span data-ttu-id="d6822-148">Führen Sie `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design` aus.</span><span class="sxs-lookup"><span data-stu-id="d6822-148">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="d6822-149">Zurückentwickeln (Reverse Engineering) Ihres Modells</span><span class="sxs-lookup"><span data-stu-id="d6822-149">Reverse engineer your model</span></span>

<span data-ttu-id="d6822-150">Jetzt ist es Zeit, basierend auf Ihrer vorhandenen Datenbank das EF-Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d6822-150">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="d6822-151">Wählen Sie **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="d6822-151">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="d6822-152">Führen Sie den folgenden Befehl aus, um ein Modell aus der vorhandenen Datenbank zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="d6822-152">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="d6822-153">Wenn Sie den Fehler `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet` erhalten, schließen Sie Visual Studio, und öffnen Sie es erneut.</span><span class="sxs-lookup"><span data-stu-id="d6822-153">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="d6822-154">Indem Sie das `-Tables`-Argument zum obigen Befehl hinzufügen, können Sie angeben, für welche Tabellen Sie Entitäten generieren möchten.</span><span class="sxs-lookup"><span data-stu-id="d6822-154">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="d6822-155">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d6822-155">E.g.</span></span> <span data-ttu-id="d6822-156">`-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="d6822-156">`-Tables Blog,Post`.</span></span>

<span data-ttu-id="d6822-157">Der Reverse Engineering-Prozess hat Entitätsklassen (`Blog.cs` & `Post.cs`) und einen abgeleiteten Kontext (`BloggingContext.cs`) basierend auf dem Schema der vorhandenen Datenbank erstellt.</span><span class="sxs-lookup"><span data-stu-id="d6822-157">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="d6822-158">Die Entitätsklassen sind einfache C#-Objekte zum Repräsentieren der Daten, die Sie abfragen und speichern werden.</span><span class="sxs-lookup"><span data-stu-id="d6822-158">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 <span data-ttu-id="d6822-159">Der Kontext repräsentiert eine Sitzung mit der Datenbank und ermöglicht Ihnen das Abfragen und Speichern von Instanzen der Entitätsklassen.</span><span class="sxs-lookup"><span data-stu-id="d6822-159">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

<!-- Static code listing, rather than a linked file, because the walkthrough modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
    public virtual DbSet<Blog> Blog { get; set; }
    public virtual DbSet<Post> Post { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        if (!optionsBuilder.IsConfigured)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }
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
}
```

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="d6822-160">Registrieren Ihres Kontexts durch Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="d6822-160">Register your context with dependency injection</span></span>

<span data-ttu-id="d6822-161">Die Abhängigkeitsinjektion ist ein zentrales Konzept von ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d6822-161">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="d6822-162">Dienste (z.B. `BloggingContext`) werden per Abhängigkeitsinjektion während des Anwendungsstarts registriert.</span><span class="sxs-lookup"><span data-stu-id="d6822-162">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="d6822-163">Komponenten, die diese Dienste benötigen (z.B. Ihre MVC-Controller), werden die Dienste anschließend über Konstruktorparameter oder Eigenschaften bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="d6822-163">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="d6822-164">Weitere Informationen zur Abhängigkeitsinjektion finden Sie im Artikel [Abhängigkeitsinjektion](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) auf der ASP.NET-Website.</span><span class="sxs-lookup"><span data-stu-id="d6822-164">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="remove-inline-context-configuration"></a><span data-ttu-id="d6822-165">Entfernen der Inlinekontextkonfiguration</span><span class="sxs-lookup"><span data-stu-id="d6822-165">Remove inline context configuration</span></span>

<span data-ttu-id="d6822-166">In ASP.NET Core wird die Konfiguration im Allgemeinen in **Startup.cs** durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="d6822-166">In ASP.NET Core, configuration is generally performed in **Startup.cs**.</span></span> <span data-ttu-id="d6822-167">Um diesem Muster zu entsprechen, verschieben wir die Konfiguration des Datenbankanbieters in **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="d6822-167">To conform to this pattern, we will move configuration of the database provider to **Startup.cs**.</span></span>

* <span data-ttu-id="d6822-168">Öffnen Sie `Models\BloggingContext.cs`.</span><span class="sxs-lookup"><span data-stu-id="d6822-168">Open `Models\BloggingContext.cs`</span></span>
* <span data-ttu-id="d6822-169">Löschen Sie die `OnConfiguring(...)`-Methode.</span><span class="sxs-lookup"><span data-stu-id="d6822-169">Delete the `OnConfiguring(...)` method</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* <span data-ttu-id="d6822-170">Fügen Sie den folgenden Konstruktor hinzu, der eine Übergabe der Konfiguration in den Kontext per Abhängigkeitsinjektion zulässt.</span><span class="sxs-lookup"><span data-stu-id="d6822-170">Add the following constructor, which will allow configuration to be passed into the context by dependency injection</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="d6822-171">Registrieren und Konfigurieren Ihres Kontexts in „Startup.cs“</span><span class="sxs-lookup"><span data-stu-id="d6822-171">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="d6822-172">Damit unsere MVC-Controller `BloggingContext` nutzen können, werden wir sie als Dienst registrieren.</span><span class="sxs-lookup"><span data-stu-id="d6822-172">In order for our MVC controllers to make use of `BloggingContext` we are going to register it as a service.</span></span>

* <span data-ttu-id="d6822-173">Öffnen Sie **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="d6822-173">Open **Startup.cs**</span></span>
* <span data-ttu-id="d6822-174">Fügen Sie am Anfang der Datei die folgenden `using`-Anweisungen ein.</span><span class="sxs-lookup"><span data-stu-id="d6822-174">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="d6822-175">Jetzt können wir die `AddDbContext(...)`-Methode verwenden, um sie als Dienst zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="d6822-175">Now we can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="d6822-176">Suchen Sie nach der `ConfigureServices(...)`-Methode.</span><span class="sxs-lookup"><span data-stu-id="d6822-176">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="d6822-177">Fügen Sie den folgenden Code hinzu, um den Kontext als Dienst zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="d6822-177">Add the following code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> <span data-ttu-id="d6822-178">In einer echten Anwendung würden Sie die Verbindungszeichenfolge typischerweise in einer Konfigurationsdatei platzieren.</span><span class="sxs-lookup"><span data-stu-id="d6822-178">In a real application you would typically put the connection string in a configuration file.</span></span> <span data-ttu-id="d6822-179">Der Einfachheit halber definieren wir sie hier im Code.</span><span class="sxs-lookup"><span data-stu-id="d6822-179">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="d6822-180">Weitere Informationen finden Sie in [Verbindungszeichenfolgen](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="d6822-180">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="d6822-181">Erstellen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="d6822-181">Create a controller</span></span>

<span data-ttu-id="d6822-182">Als Nächstes aktivieren wir den Gerüstbau in unserem Projekt.</span><span class="sxs-lookup"><span data-stu-id="d6822-182">Next, we'll enable scaffolding in our project.</span></span>

* <span data-ttu-id="d6822-183">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Controller**, und wählen Sie **Hinzufügen > Controller** aus.</span><span class="sxs-lookup"><span data-stu-id="d6822-183">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="d6822-184">Wählen Sie **Vollständige Abhängigkeiten** aus, und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="d6822-184">Select **Full Dependencies** and click **Add**</span></span>
* <span data-ttu-id="d6822-185">Sie können die Anweisungen in der jetzt geöffneten Datei `ScaffoldingReadMe.txt` ignorieren.</span><span class="sxs-lookup"><span data-stu-id="d6822-185">You can ignore the instructions in the `ScaffoldingReadMe.txt` file that opens</span></span>

<span data-ttu-id="d6822-186">Nach der Aktivierung des Gerüstbaus können Sie einen Controller für die `Blog`-Entität einrichten.</span><span class="sxs-lookup"><span data-stu-id="d6822-186">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="d6822-187">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Controller**, und wählen Sie **Hinzufügen > Controller** aus.</span><span class="sxs-lookup"><span data-stu-id="d6822-187">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="d6822-188">Wählen Sie **MVC-Controller mit Ansichten unter Verwendung von Entity Framework** aus, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="d6822-188">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="d6822-189">Legen Sie **Modellklasse** auf **Blog** und **Datenkontextklasse** auf **BloggingContext** fest.</span><span class="sxs-lookup"><span data-stu-id="d6822-189">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="d6822-190">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="d6822-190">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="d6822-191">Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="d6822-191">Run the application</span></span>

<span data-ttu-id="d6822-192">Sie können die Anwendung jetzt ausführen, um sie in Aktion zu sehen.</span><span class="sxs-lookup"><span data-stu-id="d6822-192">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="d6822-193">Klicken Sie auf **Debuggen > Ohne Debuggen starten**.</span><span class="sxs-lookup"><span data-stu-id="d6822-193">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="d6822-194">Die Anwendung wird erstellt und in einem Webbrowser geöffnet.</span><span class="sxs-lookup"><span data-stu-id="d6822-194">The application will build and open in a web browser</span></span>
* <span data-ttu-id="d6822-195">Navigieren Sie zu `/Blogs`.</span><span class="sxs-lookup"><span data-stu-id="d6822-195">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="d6822-196">Klicken Sie auf **Neu erstellen**.</span><span class="sxs-lookup"><span data-stu-id="d6822-196">Click **Create New**</span></span>
* <span data-ttu-id="d6822-197">Geben Sie eine **URL** für den neuen Blog ein, und klicken Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="d6822-197">Enter a **Url** for the new blog and click **Create**</span></span>

![image](_static/create.png)

![image](_static/index-existing-db.png)
