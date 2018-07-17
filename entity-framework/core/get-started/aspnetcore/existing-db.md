---
title: Erste Schritte in ASP.NET Core – Vorhandene Datenbank – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: e28149346ccd7531449ea696505588317471e6dd
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949152"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="19bd1-102">Erste Schritte mit EF Core in ASP.NET Core mit einer vorhandenen Datenbank</span><span class="sxs-lookup"><span data-stu-id="19bd1-102">Getting Started with EF Core on ASP.NET Core with an Existing Database</span></span>

<span data-ttu-id="19bd1-103">In dieser exemplarischen Vorgehensweise entwickeln Sie eine ASP.NET Core MVC-Anwendung, die einen grundlegenden Datenzugriff mit Entity Framework durchführt.</span><span class="sxs-lookup"><span data-stu-id="19bd1-103">In this walkthrough, you will build an ASP.NET Core MVC application that performs basic data access using Entity Framework.</span></span> <span data-ttu-id="19bd1-104">Sie erstellen per Reverse Engineering ein Entity Framework-Modell, das auf einer vorhandenen Datenbank basiert.</span><span class="sxs-lookup"><span data-stu-id="19bd1-104">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="19bd1-105">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="19bd1-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="19bd1-106">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="19bd1-106">Prerequisites</span></span>

<span data-ttu-id="19bd1-107">Zum Ausführen dieser exemplarischen Vorgehensweise müssen folgende Voraussetzungen erfüllt sein:</span><span class="sxs-lookup"><span data-stu-id="19bd1-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* <span data-ttu-id="19bd1-108">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) mit diesen Workloads:</span><span class="sxs-lookup"><span data-stu-id="19bd1-108">[Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="19bd1-109">**ASP.NET- und Webentwicklung** (unter **Web & Cloud**)</span><span class="sxs-lookup"><span data-stu-id="19bd1-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="19bd1-110">**Plattformübergreifende .NET Core-Entwicklung** (unter **Andere Toolsets**)</span><span class="sxs-lookup"><span data-stu-id="19bd1-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="19bd1-111">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="19bd1-111">[.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).</span></span>
* [<span data-ttu-id="19bd1-112">Blogging-Datenbank</span><span class="sxs-lookup"><span data-stu-id="19bd1-112">Blogging database</span></span>](#blogging-database)

### <a name="blogging-database"></a><span data-ttu-id="19bd1-113">Blogging-Datenbank</span><span class="sxs-lookup"><span data-stu-id="19bd1-113">Blogging database</span></span>

<span data-ttu-id="19bd1-114">In diesem Tutorial wird als vorhandene Datenbank eine **Blogging**-Datenbank in Ihrer LocalDb-Instanz verwendet.</span><span class="sxs-lookup"><span data-stu-id="19bd1-114">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="19bd1-115">Wenn Sie die **Blogging**-Datenbank bereits im Rahmen eines anderen Tutorials erstellt haben, können Sie diese Schritte überspringen.</span><span class="sxs-lookup"><span data-stu-id="19bd1-115">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="19bd1-116">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="19bd1-116">Open Visual Studio</span></span>
* <span data-ttu-id="19bd1-117">Klicken Sie auf **Tools > Verbindung mit Datenbank herstellen**.</span><span class="sxs-lookup"><span data-stu-id="19bd1-117">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="19bd1-118">Wählen Sie **Microsoft SQL Server** aus, und klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="19bd1-118">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="19bd1-119">Geben Sie **(localdb)\mssqllocaldb** als **Servername** ein.</span><span class="sxs-lookup"><span data-stu-id="19bd1-119">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="19bd1-120">Geben Sie als **Datenbankname** den Wert **master** ein, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="19bd1-120">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="19bd1-121">Die master-Datenbank wird jetzt unter **Datenverbindungen** im **Server-Explorer** angezeigt.</span><span class="sxs-lookup"><span data-stu-id="19bd1-121">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="19bd1-122">Klicken Sie im **Server-Explorer** mit der rechten Maustaste auf die Datenbank, und wählen Sie **Neue Abfrage** aus.</span><span class="sxs-lookup"><span data-stu-id="19bd1-122">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="19bd1-123">Kopieren Sie das nachstehende Skript in den Abfrage-Editor.</span><span class="sxs-lookup"><span data-stu-id="19bd1-123">Copy the script, listed below, into the query editor</span></span>
* <span data-ttu-id="19bd1-124">Klicken Sie mit der rechten Maustaste auf den Abfrage-Editor, und wählen Sie **Ausführen** aus.</span><span class="sxs-lookup"><span data-stu-id="19bd1-124">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="19bd1-125">Erstellt ein neues Projekt</span><span class="sxs-lookup"><span data-stu-id="19bd1-125">Create a new project</span></span>

* <span data-ttu-id="19bd1-126">Öffnen Sie Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="19bd1-126">Open Visual Studio 2017</span></span>
* <span data-ttu-id="19bd1-127">Klicken Sie auf **Datei > Neu > Projekt**.</span><span class="sxs-lookup"><span data-stu-id="19bd1-127">**File -> New -> Project...**</span></span>
* <span data-ttu-id="19bd1-128">Wählen Sie im Menü links **Installiert > Vorlagen > Visual C# > Web** aus.</span><span class="sxs-lookup"><span data-stu-id="19bd1-128">From the left menu select **Installed -> Templates -> Visual C# -> Web**</span></span>
* <span data-ttu-id="19bd1-129">Wählen Sie die Projektvorlage **ASP.NET Core-Webanwendung (.NET Core)** aus.</span><span class="sxs-lookup"><span data-stu-id="19bd1-129">Select the **ASP.NET Core Web Application (.NET Core)** project template</span></span>
* <span data-ttu-id="19bd1-130">Geben Sie als Name **EFGetStarted.AspNetCore.ExistingDb** ein, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="19bd1-130">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name and click **OK**</span></span>
* <span data-ttu-id="19bd1-131">Warten Sie, bis das Dialogfeld **Neue ASP.NET Core-Webanwendung** angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="19bd1-131">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="19bd1-132">Wählen Sie unter **ASP.NET Core Templates 2.0** die Vorlage **Webanwendung (Model-View-Controller)** aus.</span><span class="sxs-lookup"><span data-stu-id="19bd1-132">Under **ASP.NET Core Templates 2.0** select the **Web Application (Model-View-Controller)**</span></span>
* <span data-ttu-id="19bd1-133">Stellen Sie sicher, dass **Authentifizierung** auf **Keine Authentifizierung** festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="19bd1-133">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="19bd1-134">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="19bd1-134">Click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="19bd1-135">Installieren von Entity Framework</span><span class="sxs-lookup"><span data-stu-id="19bd1-135">Install Entity Framework</span></span>

<span data-ttu-id="19bd1-136">Installieren Sie zur Verwendung von EF Core das Paket für den (oder die) gewünschten Datenbankanbieter.</span><span class="sxs-lookup"><span data-stu-id="19bd1-136">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="19bd1-137">In dieser exemplarischen Vorgehensweise wird SQL Server verwendet.</span><span class="sxs-lookup"><span data-stu-id="19bd1-137">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="19bd1-138">Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="19bd1-138">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="19bd1-139">Wählen Sie **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="19bd1-139">**Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="19bd1-140">Ausführen von `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="19bd1-140">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="19bd1-141">Wir werden einige Entity Framework-Tools verwenden, um ein Modell aus der Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="19bd1-141">We will be using some Entity Framework Tools to create a model from the database.</span></span> <span data-ttu-id="19bd1-142">Deshalb installieren wir auch das Toolpaket:</span><span class="sxs-lookup"><span data-stu-id="19bd1-142">So we will install the tools package as well:</span></span>

* <span data-ttu-id="19bd1-143">Ausführen von `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="19bd1-143">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

<span data-ttu-id="19bd1-144">Später verwenden wir einige ASP.NET Core-Gerüsttools, um Controller und Ansichten zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="19bd1-144">We will be using some ASP.NET Core Scaffolding tools to create controllers and views later on.</span></span> <span data-ttu-id="19bd1-145">Deshalb installieren wird auch dieses Entwurfspaket:</span><span class="sxs-lookup"><span data-stu-id="19bd1-145">So we will install this design package as well:</span></span>

* <span data-ttu-id="19bd1-146">Ausführen von `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span><span class="sxs-lookup"><span data-stu-id="19bd1-146">Run `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="19bd1-147">Zurückentwickeln (Reverse Engineering) Ihres Modells</span><span class="sxs-lookup"><span data-stu-id="19bd1-147">Reverse engineer your model</span></span>

<span data-ttu-id="19bd1-148">Jetzt ist es Zeit, basierend auf Ihrer vorhandenen Datenbank das EF-Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="19bd1-148">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="19bd1-149">Wählen Sie **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="19bd1-149">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="19bd1-150">Führen Sie den folgenden Befehl aus, um ein Modell aus der vorhandenen Datenbank zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="19bd1-150">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="19bd1-151">Wenn Sie den Fehler `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet` erhalten, schließen Sie Visual Studio, und öffnen Sie es erneut.</span><span class="sxs-lookup"><span data-stu-id="19bd1-151">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="19bd1-152">Indem Sie das `-Tables`-Argument zum obigen Befehl hinzufügen, können Sie angeben, für welche Tabellen Sie Entitäten generieren möchten.</span><span class="sxs-lookup"><span data-stu-id="19bd1-152">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="19bd1-153">Beispielsweise `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="19bd1-153">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="19bd1-154">Der Reverse Engineering-Prozess hat Entitätsklassen (`Blog.cs` & `Post.cs`) und einen abgeleiteten Kontext (`BloggingContext.cs`) basierend auf dem Schema der vorhandenen Datenbank erstellt.</span><span class="sxs-lookup"><span data-stu-id="19bd1-154">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="19bd1-155">Die Entitätsklassen sind einfache C#-Objekte zum Repräsentieren der Daten, die Sie abfragen und speichern werden.</span><span class="sxs-lookup"><span data-stu-id="19bd1-155">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 <span data-ttu-id="19bd1-156">Der Kontext repräsentiert eine Sitzung mit der Datenbank und ermöglicht Ihnen das Abfragen und Speichern von Instanzen der Entitätsklassen.</span><span class="sxs-lookup"><span data-stu-id="19bd1-156">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

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

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="19bd1-157">Registrieren Ihres Kontexts durch Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="19bd1-157">Register your context with dependency injection</span></span>

<span data-ttu-id="19bd1-158">Die Abhängigkeitsinjektion ist ein zentrales Konzept von ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="19bd1-158">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="19bd1-159">Dienste (z.B. `BloggingContext`) werden per Abhängigkeitsinjektion während des Anwendungsstarts registriert.</span><span class="sxs-lookup"><span data-stu-id="19bd1-159">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="19bd1-160">Komponenten, die diese Dienste benötigen (z.B. Ihre MVC-Controller), werden die Dienste anschließend über Konstruktorparameter oder Eigenschaften bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="19bd1-160">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="19bd1-161">Weitere Informationen zur Abhängigkeitsinjektion finden Sie im Artikel [Abhängigkeitsinjektion](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) auf der ASP.NET-Website.</span><span class="sxs-lookup"><span data-stu-id="19bd1-161">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="remove-inline-context-configuration"></a><span data-ttu-id="19bd1-162">Entfernen der Inlinekontextkonfiguration</span><span class="sxs-lookup"><span data-stu-id="19bd1-162">Remove inline context configuration</span></span>

<span data-ttu-id="19bd1-163">In ASP.NET Core wird die Konfiguration im Allgemeinen in **Startup.cs** durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="19bd1-163">In ASP.NET Core, configuration is generally performed in **Startup.cs**.</span></span> <span data-ttu-id="19bd1-164">Um diesem Muster zu entsprechen, verschieben wir die Konfiguration des Datenbankanbieters in **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="19bd1-164">To conform to this pattern, we will move configuration of the database provider to **Startup.cs**.</span></span>

* <span data-ttu-id="19bd1-165">Öffnen Sie `Models\BloggingContext.cs`.</span><span class="sxs-lookup"><span data-stu-id="19bd1-165">Open `Models\BloggingContext.cs`</span></span>
* <span data-ttu-id="19bd1-166">Löschen Sie die `OnConfiguring(...)`-Methode.</span><span class="sxs-lookup"><span data-stu-id="19bd1-166">Delete the `OnConfiguring(...)` method</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* <span data-ttu-id="19bd1-167">Fügen Sie den folgenden Konstruktor hinzu, der eine Übergabe der Konfiguration in den Kontext per Abhängigkeitsinjektion zulässt.</span><span class="sxs-lookup"><span data-stu-id="19bd1-167">Add the following constructor, which will allow configuration to be passed into the context by dependency injection</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="19bd1-168">Registrieren und Konfigurieren Ihres Kontexts in „Startup.cs“</span><span class="sxs-lookup"><span data-stu-id="19bd1-168">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="19bd1-169">Damit unsere MVC-Controller `BloggingContext` nutzen können, werden wir sie als Dienst registrieren.</span><span class="sxs-lookup"><span data-stu-id="19bd1-169">In order for our MVC controllers to make use of `BloggingContext` we are going to register it as a service.</span></span>

* <span data-ttu-id="19bd1-170">Öffnen Sie **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="19bd1-170">Open **Startup.cs**</span></span>
* <span data-ttu-id="19bd1-171">Fügen Sie am Anfang der Datei die folgenden `using`-Anweisungen ein.</span><span class="sxs-lookup"><span data-stu-id="19bd1-171">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="19bd1-172">Jetzt können wir die `AddDbContext(...)`-Methode verwenden, um sie als Dienst zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="19bd1-172">Now we can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="19bd1-173">Suchen Sie nach der `ConfigureServices(...)`-Methode.</span><span class="sxs-lookup"><span data-stu-id="19bd1-173">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="19bd1-174">Fügen Sie den folgenden Code hinzu, um den Kontext als Dienst zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="19bd1-174">Add the following code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> <span data-ttu-id="19bd1-175">In einer echten Anwendung würden Sie die Verbindungszeichenfolge typischerweise in einer Konfigurationsdatei platzieren.</span><span class="sxs-lookup"><span data-stu-id="19bd1-175">In a real application you would typically put the connection string in a configuration file.</span></span> <span data-ttu-id="19bd1-176">Der Einfachheit halber definieren wir sie hier im Code.</span><span class="sxs-lookup"><span data-stu-id="19bd1-176">For the sake of simplicity, we are defining it in code.</span></span> <span data-ttu-id="19bd1-177">Weitere Informationen finden Sie in [Verbindungszeichenfolgen](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="19bd1-177">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller"></a><span data-ttu-id="19bd1-178">Erstellen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="19bd1-178">Create a controller</span></span>

<span data-ttu-id="19bd1-179">Als Nächstes aktivieren wir den Gerüstbau in unserem Projekt.</span><span class="sxs-lookup"><span data-stu-id="19bd1-179">Next, we'll enable scaffolding in our project.</span></span>

* <span data-ttu-id="19bd1-180">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Controller**, und wählen Sie **Hinzufügen > Controller** aus.</span><span class="sxs-lookup"><span data-stu-id="19bd1-180">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="19bd1-181">Wählen Sie **Vollständige Abhängigkeiten** aus, und klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="19bd1-181">Select **Full Dependencies** and click **Add**</span></span>
* <span data-ttu-id="19bd1-182">Sie können die Anweisungen in der jetzt geöffneten Datei `ScaffoldingReadMe.txt` ignorieren.</span><span class="sxs-lookup"><span data-stu-id="19bd1-182">You can ignore the instructions in the `ScaffoldingReadMe.txt` file that opens</span></span>

<span data-ttu-id="19bd1-183">Nach der Aktivierung des Gerüstbaus können Sie einen Controller für die `Blog`-Entität einrichten.</span><span class="sxs-lookup"><span data-stu-id="19bd1-183">Now that scaffolding is enabled, we can scaffold a controller for the `Blog` entity.</span></span>

* <span data-ttu-id="19bd1-184">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Controller**, und wählen Sie **Hinzufügen > Controller** aus.</span><span class="sxs-lookup"><span data-stu-id="19bd1-184">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="19bd1-185">Wählen Sie **MVC-Controller mit Ansichten unter Verwendung von Entity Framework** aus, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="19bd1-185">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="19bd1-186">Legen Sie **Modellklasse** auf **Blog** und **Datenkontextklasse** auf **BloggingContext** fest.</span><span class="sxs-lookup"><span data-stu-id="19bd1-186">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="19bd1-187">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="19bd1-187">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="19bd1-188">Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="19bd1-188">Run the application</span></span>

<span data-ttu-id="19bd1-189">Sie können die Anwendung jetzt ausführen, um sie in Aktion zu sehen.</span><span class="sxs-lookup"><span data-stu-id="19bd1-189">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="19bd1-190">Klicken Sie auf **Debuggen > Ohne Debuggen starten**.</span><span class="sxs-lookup"><span data-stu-id="19bd1-190">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="19bd1-191">Die Anwendung wird erstellt und in einem Webbrowser geöffnet.</span><span class="sxs-lookup"><span data-stu-id="19bd1-191">The application will build and open in a web browser</span></span>
* <span data-ttu-id="19bd1-192">Navigieren Sie zu `/Blogs`.</span><span class="sxs-lookup"><span data-stu-id="19bd1-192">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="19bd1-193">Klicken Sie auf **Neu erstellen**.</span><span class="sxs-lookup"><span data-stu-id="19bd1-193">Click **Create New**</span></span>
* <span data-ttu-id="19bd1-194">Geben Sie eine **URL** für den neuen Blog ein, und klicken Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="19bd1-194">Enter a **Url** for the new blog and click **Create**</span></span>

![Bild](_static/create.png)

![Bild](_static/index-existing-db.png)
