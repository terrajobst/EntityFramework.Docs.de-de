---
title: Erste Schritte in ASP.NET Core – Vorhandene Datenbank – EF Core
author: rowanmiller
ms.date: 08/02/2018
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: 79a73e38fdc9c4268c21de66571d6272f33e9457
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997035"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="eb864-102">Erste Schritte mit EF Core in ASP.NET Core mit einer vorhandenen Datenbank</span><span class="sxs-lookup"><span data-stu-id="eb864-102">Getting Started with EF Core on ASP.NET Core with an Existing Database</span></span>

<span data-ttu-id="eb864-103">In diesem Tutorial entwickeln Sie eine ASP.NET Core MVC-Anwendung, die einen grundlegenden Datenzugriff mit Entity Framework Core durchführt.</span><span class="sxs-lookup"><span data-stu-id="eb864-103">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="eb864-104">Sie erstellen ein Entity Framework-Modell, indem Sie ein Reverse Engineering für eine vorhandene Datenbank durchführen.</span><span class="sxs-lookup"><span data-stu-id="eb864-104">You reverse engineer an existing database to create an Entity Framework model.</span></span>

<span data-ttu-id="eb864-105">[Sehen Sie sich das Beispiel aus diesem Artikel auf GitHub an](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span><span class="sxs-lookup"><span data-stu-id="eb864-105">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eb864-106">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="eb864-106">Prerequisites</span></span>

<span data-ttu-id="eb864-107">Installieren Sie die folgenden Softwarekomponenten:</span><span class="sxs-lookup"><span data-stu-id="eb864-107">Install the following software:</span></span>

* <span data-ttu-id="eb864-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) mit diesen Workloads:</span><span class="sxs-lookup"><span data-stu-id="eb864-108">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="eb864-109">**ASP.NET- und Webentwicklung** (unter **Web & Cloud**)</span><span class="sxs-lookup"><span data-stu-id="eb864-109">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="eb864-110">**Plattformübergreifende .NET Core-Entwicklung** (unter **Andere Toolsets**)</span><span class="sxs-lookup"><span data-stu-id="eb864-110">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="eb864-111">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core)</span><span class="sxs-lookup"><span data-stu-id="eb864-111">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="create-blogging-database"></a><span data-ttu-id="eb864-112">Erstellen einer Blogging-Datenbank</span><span class="sxs-lookup"><span data-stu-id="eb864-112">Create Blogging database</span></span>

<span data-ttu-id="eb864-113">In diesem Tutorial wird als vorhandene Datenbank eine **Blogging**-Datenbank in Ihrer LocalDb-Instanz verwendet.</span><span class="sxs-lookup"><span data-stu-id="eb864-113">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span> <span data-ttu-id="eb864-114">Wenn Sie die **Blogging**-Datenbank bereits im Rahmen eines anderen Tutorials erstellt haben, können Sie diese Schritte überspringen.</span><span class="sxs-lookup"><span data-stu-id="eb864-114">If you have already created the **Blogging** database as part of another tutorial, skip these steps.</span></span>

* <span data-ttu-id="eb864-115">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="eb864-115">Open Visual Studio</span></span>
* <span data-ttu-id="eb864-116">Klicken Sie auf **Tools > Verbindung mit Datenbank herstellen**.</span><span class="sxs-lookup"><span data-stu-id="eb864-116">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="eb864-117">Wählen Sie **Microsoft SQL Server** aus, und klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="eb864-117">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="eb864-118">Geben Sie **(localdb)\mssqllocaldb** als **Servername** ein.</span><span class="sxs-lookup"><span data-stu-id="eb864-118">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="eb864-119">Geben Sie als **Datenbankname** den Wert **master** ein, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="eb864-119">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="eb864-120">Die master-Datenbank wird jetzt unter **Datenverbindungen** im **Server-Explorer** angezeigt.</span><span class="sxs-lookup"><span data-stu-id="eb864-120">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="eb864-121">Klicken Sie im **Server-Explorer** mit der rechten Maustaste auf die Datenbank, und wählen Sie **Neue Abfrage** aus.</span><span class="sxs-lookup"><span data-stu-id="eb864-121">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="eb864-122">Kopieren Sie das nachstehende Skript in den Abfrage-Editor.</span><span class="sxs-lookup"><span data-stu-id="eb864-122">Copy the script listed below into the query editor</span></span>
* <span data-ttu-id="eb864-123">Klicken Sie mit der rechten Maustaste auf den Abfrage-Editor, und wählen Sie **Ausführen** aus.</span><span class="sxs-lookup"><span data-stu-id="eb864-123">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="eb864-124">Erstellt ein neues Projekt</span><span class="sxs-lookup"><span data-stu-id="eb864-124">Create a new project</span></span>

* <span data-ttu-id="eb864-125">Öffnen Sie Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="eb864-125">Open Visual Studio 2017</span></span>
* <span data-ttu-id="eb864-126">Klicken Sie auf **Datei > Neu > Projekt...**.</span><span class="sxs-lookup"><span data-stu-id="eb864-126">**File > New > Project...**</span></span>
* <span data-ttu-id="eb864-127">Klicken Sie im linken Bereich auf **Installiert > Visual C# > Web**.</span><span class="sxs-lookup"><span data-stu-id="eb864-127">From the left menu select **Installed > Visual C# > Web**</span></span>
* <span data-ttu-id="eb864-128">Wählen Sie die Projektvorlage **ASP.NET Core-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="eb864-128">Select the **ASP.NET Core Web Application** project template</span></span>
* <span data-ttu-id="eb864-129">Geben Sie als Name **EFGetStarted.AspNetCore.ExistingDb** ein, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="eb864-129">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name and click **OK**</span></span>
* <span data-ttu-id="eb864-130">Warten Sie, bis das Dialogfeld **Neue ASP.NET Core-Webanwendung** angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="eb864-130">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="eb864-131">Stellen Sie sicher, dass das Dropdownmenü für das Zielframework auf **.NET Core** und für die Version auf **ASP.NET Core 2.1** festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="eb864-131">Make sure that the target framework dropdown is set to **.NET Core**, and the version dropdown is set to **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="eb864-132">Wählen Sie die Vorlage **Webanwendung (Model-View-Controller)** aus.</span><span class="sxs-lookup"><span data-stu-id="eb864-132">Select the **Web Application (Model-View-Controller)** template</span></span>
* <span data-ttu-id="eb864-133">Stellen Sie sicher, dass **Authentifizierung** auf **Keine Authentifizierung** festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="eb864-133">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="eb864-134">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="eb864-134">Click **OK**</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="eb864-135">Installieren von Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="eb864-135">Install Entity Framework Core</span></span>

<span data-ttu-id="eb864-136">Installieren Sie das Paket für den (oder die) gewünschten EF Core-Datenbankanbieter, um EF Core zu installieren.</span><span class="sxs-lookup"><span data-stu-id="eb864-136">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="eb864-137">Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="eb864-137">For a list of available providers see [Database Providers](../../providers/index.md).</span></span> 

<span data-ttu-id="eb864-138">Für dieses Tutorial müssen Sie kein Anbieterpaket installieren, da das Tutorial SQL Server verwendet.</span><span class="sxs-lookup"><span data-stu-id="eb864-138">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="eb864-139">Das SQL Server-Anbieterpaket ist im Metapaket [Microsoft.AspnetCore.App](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1) enthalten.</span><span class="sxs-lookup"><span data-stu-id="eb864-139">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="eb864-140">Zurückentwickeln (Reverse Engineering) Ihres Modells</span><span class="sxs-lookup"><span data-stu-id="eb864-140">Reverse engineer your model</span></span>

<span data-ttu-id="eb864-141">Jetzt ist es Zeit, basierend auf Ihrer vorhandenen Datenbank das EF-Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="eb864-141">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="eb864-142">Wählen Sie **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="eb864-142">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="eb864-143">Führen Sie den folgenden Befehl aus, um ein Modell aus der vorhandenen Datenbank zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="eb864-143">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="eb864-144">Wenn Sie den Fehler `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet` erhalten, schließen Sie Visual Studio, und öffnen Sie es erneut.</span><span class="sxs-lookup"><span data-stu-id="eb864-144">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="eb864-145">Indem Sie das `-Tables`-Argument zum obigen Befehl hinzufügen, können Sie angeben, für welche Tabellen Sie Entitäten generieren möchten.</span><span class="sxs-lookup"><span data-stu-id="eb864-145">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="eb864-146">Beispielsweise `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="eb864-146">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="eb864-147">Der Reverse Engineering-Prozess hat Entitätsklassen (`Blog.cs` & `Post.cs`) und einen abgeleiteten Kontext (`BloggingContext.cs`) basierend auf dem Schema der vorhandenen Datenbank erstellt.</span><span class="sxs-lookup"><span data-stu-id="eb864-147">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="eb864-148">Die Entitätsklassen sind einfache C#-Objekte zum Repräsentieren der Daten, die Sie abfragen und speichern werden.</span><span class="sxs-lookup"><span data-stu-id="eb864-148">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span> <span data-ttu-id="eb864-149">Hier sind die Entitätsklassen `Blog` und `Post`:</span><span class="sxs-lookup"><span data-stu-id="eb864-149">Here are the `Blog` and `Post` entity classes:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Post.cs)]

> [!TIP]  
> <span data-ttu-id="eb864-150">Sie können Navigationseigenschaften auf `virtual` festlegen (Blog.Post und Post.Blog), um Lazy Loading zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="eb864-150">To enable lazy loading, you can make navigation properties `virtual` (Blog.Post and Post.Blog).</span></span>

 <span data-ttu-id="eb864-151">Der Kontext repräsentiert eine Sitzung mit der Datenbank und ermöglicht Ihnen das Abfragen und Speichern von Instanzen der Entitätsklassen.</span><span class="sxs-lookup"><span data-stu-id="eb864-151">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

<!-- Static code listing, rather than a linked file, because the tutorial modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
    public BloggingContext()
    {
    }

    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    {
    }

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

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="eb864-152">Registrieren Ihres Kontexts durch Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="eb864-152">Register your context with dependency injection</span></span>

<span data-ttu-id="eb864-153">Die Abhängigkeitsinjektion ist ein zentrales Konzept von ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="eb864-153">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="eb864-154">Dienste (z.B. `BloggingContext`) werden per Abhängigkeitsinjektion während des Anwendungsstarts registriert.</span><span class="sxs-lookup"><span data-stu-id="eb864-154">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="eb864-155">Komponenten, die diese Dienste benötigen (z.B. Ihre MVC-Controller), werden die Dienste anschließend über Konstruktorparameter oder Eigenschaften bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="eb864-155">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="eb864-156">Weitere Informationen zur Abhängigkeitsinjektion finden Sie im Artikel [Abhängigkeitsinjektion](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) auf der ASP.NET-Website.</span><span class="sxs-lookup"><span data-stu-id="eb864-156">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="eb864-157">Registrieren und Konfigurieren Ihres Kontexts in „Startup.cs“</span><span class="sxs-lookup"><span data-stu-id="eb864-157">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="eb864-158">Um eine `BloggingContext`-Klasse für MVC-Controller verfügbar zu machen, registrieren Sie sie als Dienst.</span><span class="sxs-lookup"><span data-stu-id="eb864-158">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

* <span data-ttu-id="eb864-159">Öffnen Sie **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="eb864-159">Open **Startup.cs**</span></span>
* <span data-ttu-id="eb864-160">Fügen Sie am Anfang der Datei die folgenden `using`-Anweisungen ein.</span><span class="sxs-lookup"><span data-stu-id="eb864-160">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="eb864-161">Jetzt können Sie die `AddDbContext(...)`-Methode verwenden, um sie als Dienst zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="eb864-161">Now you can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="eb864-162">Suchen Sie nach der `ConfigureServices(...)`-Methode.</span><span class="sxs-lookup"><span data-stu-id="eb864-162">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="eb864-163">Fügen Sie den folgenden hervorgehobenen Code hinzu, um den Kontext als Dienst zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="eb864-163">Add the following highlighted code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=14-15)]

> [!TIP]  
> <span data-ttu-id="eb864-164">In einer echten Anwendung würden Sie die Verbindungszeichenfolge typischerweise in einer Konfigurationsdatei oder Umgebungsvariable platzieren.</span><span class="sxs-lookup"><span data-stu-id="eb864-164">In a real application you would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="eb864-165">Der Einfachheit halber definieren Sie diese in diesem Tutorial im Code.</span><span class="sxs-lookup"><span data-stu-id="eb864-165">For the sake of simplicity, this tutorial has you define it in code.</span></span> <span data-ttu-id="eb864-166">Weitere Informationen finden Sie in [Verbindungszeichenfolgen](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="eb864-166">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller-and-views"></a><span data-ttu-id="eb864-167">Erstellen von einem Controller und Ansichten</span><span class="sxs-lookup"><span data-stu-id="eb864-167">Create a controller and views</span></span>

* <span data-ttu-id="eb864-168">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Controller**, und wählen Sie **Hinzufügen > Controller** aus.</span><span class="sxs-lookup"><span data-stu-id="eb864-168">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="eb864-169">Wählen Sie **MVC-Controller mit Ansichten unter Verwendung von Entity Framework** aus, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="eb864-169">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="eb864-170">Legen Sie **Modellklasse** auf **Blog** und **Datenkontextklasse** auf **BloggingContext** fest.</span><span class="sxs-lookup"><span data-stu-id="eb864-170">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="eb864-171">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="eb864-171">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="eb864-172">Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="eb864-172">Run the application</span></span>

<span data-ttu-id="eb864-173">Sie können die Anwendung jetzt ausführen, um sie in Aktion zu sehen.</span><span class="sxs-lookup"><span data-stu-id="eb864-173">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="eb864-174">Klicken Sie auf **Debuggen > Ohne Debuggen starten**.</span><span class="sxs-lookup"><span data-stu-id="eb864-174">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="eb864-175">Die Anwendung wird erstellt und in einem Webbrowser geöffnet.</span><span class="sxs-lookup"><span data-stu-id="eb864-175">The application builds and opens in a web browser</span></span>
* <span data-ttu-id="eb864-176">Navigieren Sie zu `/Blogs`.</span><span class="sxs-lookup"><span data-stu-id="eb864-176">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="eb864-177">Klicken Sie auf **Neu erstellen**.</span><span class="sxs-lookup"><span data-stu-id="eb864-177">Click **Create New**</span></span>
* <span data-ttu-id="eb864-178">Geben Sie eine **URL** für den neuen Blog ein, und klicken Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="eb864-178">Enter a **Url** for the new blog and click **Create**</span></span>

![Bild](_static/create.png)

![Bild](_static/index-existing-db.png)
