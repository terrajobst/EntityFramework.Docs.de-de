---
title: Erste Schritte mit EF Core und einer vorhandenen Datenbank in ASP.NET Core
author: rowanmiller
description: Erste Schritte mit EF Core und einer vorhandenen Datenbank in ASP.NET Core
ms.date: 08/02/2018
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: 6b0ed0a9222644bee31d23234aa27b2084137f4a
ms.sourcegitcommit: 755a15a789631cc4ea581e2262a2dcc49c219eef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2019
ms.locfileid: "68497510"
---
# <a name="get-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a><span data-ttu-id="a2ac4-103">Lernen Sie in ASP.NET Core die ersten Schritte mit EF Core und einer bereits vorhandenen Datenbank kennen.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-103">Get started with EF Core on ASP.NET Core with an existing database</span></span>

<span data-ttu-id="a2ac4-104">In diesem Tutorial entwickeln Sie eine ASP.NET Core MVC-Anwendung, die einen grundlegenden Datenzugriff mit Entity Framework Core durchführt.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-104">In this tutorial, you build an ASP.NET Core MVC application that performs basic data access using Entity Framework Core.</span></span> <span data-ttu-id="a2ac4-105">Sie erstellen ein Entity Framework-Modell, indem Sie ein Reverse Engineering für eine vorhandene Datenbank durchführen.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-105">You reverse engineer an existing database to create an Entity Framework model.</span></span>

<span data-ttu-id="a2ac4-106">[Sehen Sie sich das Beispiel aus diesem Artikel auf GitHub an](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span><span class="sxs-lookup"><span data-stu-id="a2ac4-106">[View this article's sample on GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a2ac4-107">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="a2ac4-107">Prerequisites</span></span>

<span data-ttu-id="a2ac4-108">Installieren Sie die folgenden Softwarekomponenten:</span><span class="sxs-lookup"><span data-stu-id="a2ac4-108">Install the following software:</span></span>

* <span data-ttu-id="a2ac4-109">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) mit diesen Workloads:</span><span class="sxs-lookup"><span data-stu-id="a2ac4-109">[Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) with these workloads:</span></span>
  * <span data-ttu-id="a2ac4-110">**ASP.NET- und Webentwicklung** (unter **Web & Cloud**)</span><span class="sxs-lookup"><span data-stu-id="a2ac4-110">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
  * <span data-ttu-id="a2ac4-111">**Plattformübergreifende .NET Core-Entwicklung** (unter **Andere Toolsets**)</span><span class="sxs-lookup"><span data-stu-id="a2ac4-111">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>
* <span data-ttu-id="a2ac4-112">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core)</span><span class="sxs-lookup"><span data-stu-id="a2ac4-112">[.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span>

## <a name="create-blogging-database"></a><span data-ttu-id="a2ac4-113">Erstellen einer Blogging-Datenbank</span><span class="sxs-lookup"><span data-stu-id="a2ac4-113">Create Blogging database</span></span>

<span data-ttu-id="a2ac4-114">In diesem Tutorial wird als vorhandene Datenbank eine **Blogging**-Datenbank in Ihrer LocalDb-Instanz verwendet.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-114">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span> <span data-ttu-id="a2ac4-115">Wenn Sie die **Blogging**-Datenbank bereits im Rahmen eines anderen Tutorials erstellt haben, können Sie diese Schritte überspringen.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-115">If you have already created the **Blogging** database as part of another tutorial, skip these steps.</span></span>

* <span data-ttu-id="a2ac4-116">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-116">Open Visual Studio</span></span>
* <span data-ttu-id="a2ac4-117">Klicken Sie auf **Tools > Verbindung mit Datenbank herstellen**.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-117">**Tools -> Connect to Database...**</span></span>
* <span data-ttu-id="a2ac4-118">Wählen Sie **Microsoft SQL Server** aus, und klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-118">Select **Microsoft SQL Server** and click **Continue**</span></span>
* <span data-ttu-id="a2ac4-119">Geben Sie **(localdb)\mssqllocaldb** als **Servername** ein.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-119">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>
* <span data-ttu-id="a2ac4-120">Geben Sie als **Datenbankname** den Wert **master** ein, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-120">Enter **master** as the **Database Name** and click **OK**</span></span>
* <span data-ttu-id="a2ac4-121">Die master-Datenbank wird jetzt unter **Datenverbindungen** im **Server-Explorer** angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-121">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>
* <span data-ttu-id="a2ac4-122">Klicken Sie im **Server-Explorer** mit der rechten Maustaste auf die Datenbank, und wählen Sie **Neue Abfrage** aus.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-122">Right-click on the database in **Server Explorer** and select **New Query**</span></span>
* <span data-ttu-id="a2ac4-123">Kopieren Sie das nachstehende Skript in den Abfrage-Editor.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-123">Copy the script listed below into the query editor</span></span>
* <span data-ttu-id="a2ac4-124">Klicken Sie mit der rechten Maustaste auf den Abfrage-Editor, und wählen Sie **Ausführen** aus.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-124">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="a2ac4-125">Erstellt ein neues Projekt</span><span class="sxs-lookup"><span data-stu-id="a2ac4-125">Create a new project</span></span>

* <span data-ttu-id="a2ac4-126">Öffnen Sie Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-126">Open Visual Studio 2017</span></span>
* <span data-ttu-id="a2ac4-127">Klicken Sie auf **Datei > Neu > Projekt...** .</span><span class="sxs-lookup"><span data-stu-id="a2ac4-127">**File > New > Project...**</span></span>
* <span data-ttu-id="a2ac4-128">Klicken Sie im linken Bereich auf **Installiert > Visual C# > Web**.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-128">From the left menu select **Installed > Visual C# > Web**</span></span>
* <span data-ttu-id="a2ac4-129">Wählen Sie die Projektvorlage **ASP.NET Core-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-129">Select the **ASP.NET Core Web Application** project template</span></span>
* <span data-ttu-id="a2ac4-130">Geben Sie **EFGetStarted.AspNetCore.ExistingDb** als Namen ein (es muss genau der Namespace sein, der später im Code verwendet wird), und klicken Sie auf **OK**</span><span class="sxs-lookup"><span data-stu-id="a2ac4-130">Enter **EFGetStarted.AspNetCore.ExistingDb** as the name (it has to match exactly the namespace later used in the code) and click **OK**</span></span> 
* <span data-ttu-id="a2ac4-131">Warten Sie, bis das Dialogfeld **Neue ASP.NET Core-Webanwendung** angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-131">Wait for the **New ASP.NET Core Web Application** dialog to appear</span></span>
* <span data-ttu-id="a2ac4-132">Stellen Sie sicher, dass das Dropdownmenü für das Zielframework auf **.NET Core** und für die Version auf **ASP.NET Core 2.1** festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-132">Make sure that the target framework dropdown is set to **.NET Core**, and the version dropdown is set to **ASP.NET Core 2.1**</span></span>
* <span data-ttu-id="a2ac4-133">Wählen Sie die Vorlage **Webanwendung (Model-View-Controller)** aus.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-133">Select the **Web Application (Model-View-Controller)** template</span></span>
* <span data-ttu-id="a2ac4-134">Stellen Sie sicher, dass **Authentifizierung** auf **Keine Authentifizierung** festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-134">Ensure that **Authentication** is set to **No Authentication**</span></span>
* <span data-ttu-id="a2ac4-135">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-135">Click **OK**</span></span>

## <a name="install-entity-framework-core"></a><span data-ttu-id="a2ac4-136">Installieren von Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="a2ac4-136">Install Entity Framework Core</span></span>

<span data-ttu-id="a2ac4-137">Installieren Sie das Paket für den (oder die) gewünschten EF Core-Datenbankanbieter, um EF Core zu installieren.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-137">To install EF Core, you install the package for the EF Core database provider(s) you want to target.</span></span> <span data-ttu-id="a2ac4-138">Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="a2ac4-138">For a list of available providers see [Database Providers](../../providers/index.md).</span></span> 

<span data-ttu-id="a2ac4-139">Für dieses Tutorial müssen Sie kein Anbieterpaket installieren, da das Tutorial SQL Server verwendet.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-139">For this tutorial, you don't have to install a provider package because the tutorial uses SQL Server.</span></span> <span data-ttu-id="a2ac4-140">Das SQL Server-Anbieterpaket ist im Metapaket [Microsoft.AspnetCore.App](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1) enthalten.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-140">The SQL Server provider package is included in the [Microsoft.AspnetCore.App metapackage](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="a2ac4-141">Zurückentwickeln (Reverse Engineering) Ihres Modells</span><span class="sxs-lookup"><span data-stu-id="a2ac4-141">Reverse engineer your model</span></span>

<span data-ttu-id="a2ac4-142">Jetzt ist es Zeit, basierend auf Ihrer vorhandenen Datenbank das EF-Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-142">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="a2ac4-143">Wählen Sie **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-143">**Tools –> NuGet Package Manager –> Package Manager Console**</span></span>
* <span data-ttu-id="a2ac4-144">Führen Sie den folgenden Befehl aus, um ein Modell aus der vorhandenen Datenbank zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="a2ac4-144">Run the following command to create a model from the existing database:</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="a2ac4-145">Wenn Sie den Fehler `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet` erhalten, schließen Sie Visual Studio, und öffnen Sie es erneut.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-145">If you receive an error stating `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet`, then close and reopen Visual Studio.</span></span>

> [!TIP]  
> <span data-ttu-id="a2ac4-146">Indem Sie das `-Tables`-Argument zum obigen Befehl hinzufügen, können Sie angeben, für welche Tabellen Sie Entitäten generieren möchten.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-146">You can specify which tables you want to generate entities for by adding the `-Tables` argument to the command above.</span></span> <span data-ttu-id="a2ac4-147">Beispielsweise `-Tables Blog,Post`.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-147">For example, `-Tables Blog,Post`.</span></span>

<span data-ttu-id="a2ac4-148">Der Reverse Engineering-Prozess hat Entitätsklassen (`Blog.cs` & `Post.cs`) und einen abgeleiteten Kontext (`BloggingContext.cs`) basierend auf dem Schema der vorhandenen Datenbank erstellt.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-148">The reverse engineer process created entity classes (`Blog.cs` & `Post.cs`) and a derived context (`BloggingContext.cs`) based on the schema of the existing database.</span></span>

 <span data-ttu-id="a2ac4-149">Die Entitätsklassen sind einfache C#-Objekte zum Repräsentieren der Daten, die Sie abfragen und speichern werden.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-149">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span> <span data-ttu-id="a2ac4-150">Hier sind die Entitätsklassen `Blog` und `Post`:</span><span class="sxs-lookup"><span data-stu-id="a2ac4-150">Here are the `Blog` and `Post` entity classes:</span></span>

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Post.cs)]

> [!TIP]  
> <span data-ttu-id="a2ac4-151">Sie können Navigationseigenschaften auf `virtual` festlegen (Blog.Post und Post.Blog), um Lazy Loading zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-151">To enable lazy loading, you can make navigation properties `virtual` (Blog.Post and Post.Blog).</span></span>

 <span data-ttu-id="a2ac4-152">Der Kontext repräsentiert eine Sitzung mit der Datenbank und ermöglicht Ihnen das Abfragen und Speichern von Instanzen der Entitätsklassen.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-152">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

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

## <a name="register-your-context-with-dependency-injection"></a><span data-ttu-id="a2ac4-153">Registrieren Ihres Kontexts durch Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="a2ac4-153">Register your context with dependency injection</span></span>

<span data-ttu-id="a2ac4-154">Die Abhängigkeitsinjektion ist ein zentrales Konzept von ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-154">The concept of dependency injection is central to ASP.NET Core.</span></span> <span data-ttu-id="a2ac4-155">Dienste (z.B. `BloggingContext`) werden per Abhängigkeitsinjektion während des Anwendungsstarts registriert.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-155">Services (such as `BloggingContext`) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="a2ac4-156">Komponenten, die diese Dienste benötigen (z.B. Ihre MVC-Controller), werden die Dienste anschließend über Konstruktorparameter oder Eigenschaften bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-156">Components that require these services (such as your MVC controllers) are then provided these services via constructor parameters or properties.</span></span> <span data-ttu-id="a2ac4-157">Weitere Informationen zur Abhängigkeitsinjektion finden Sie im Artikel [Abhängigkeitsinjektion](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) auf der ASP.NET-Website.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-157">For more information on dependency injection see the [Dependency Injection](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) article on the ASP.NET site.</span></span>

### <a name="register-and-configure-your-context-in-startupcs"></a><span data-ttu-id="a2ac4-158">Registrieren und Konfigurieren Ihres Kontexts in „Startup.cs“</span><span class="sxs-lookup"><span data-stu-id="a2ac4-158">Register and configure your context in Startup.cs</span></span>

<span data-ttu-id="a2ac4-159">Um eine `BloggingContext`-Klasse für MVC-Controller verfügbar zu machen, registrieren Sie sie als Dienst.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-159">To make `BloggingContext` available to MVC controllers, register it as a service.</span></span>

* <span data-ttu-id="a2ac4-160">Öffnen Sie **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-160">Open **Startup.cs**</span></span>
* <span data-ttu-id="a2ac4-161">Fügen Sie am Anfang der Datei die folgenden `using`-Anweisungen ein.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-161">Add the following `using` statements at the start of the file</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

<span data-ttu-id="a2ac4-162">Jetzt können Sie die `AddDbContext(...)`-Methode verwenden, um sie als Dienst zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-162">Now you can use the `AddDbContext(...)` method to register it as a service.</span></span>
* <span data-ttu-id="a2ac4-163">Suchen Sie nach der `ConfigureServices(...)`-Methode.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-163">Locate the `ConfigureServices(...)` method</span></span>
* <span data-ttu-id="a2ac4-164">Fügen Sie den folgenden hervorgehobenen Code hinzu, um den Kontext als Dienst zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-164">Add the following highlighted code to register the context as a service</span></span>

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=14-15)]

> [!TIP]  
> <span data-ttu-id="a2ac4-165">In einer echten Anwendung würden Sie die Verbindungszeichenfolge typischerweise in einer Konfigurationsdatei oder Umgebungsvariable platzieren.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-165">In a real application you would typically put the connection string in a configuration file or environment variable.</span></span> <span data-ttu-id="a2ac4-166">Der Einfachheit halber definieren Sie diese in diesem Tutorial im Code.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-166">For the sake of simplicity, this tutorial has you define it in code.</span></span> <span data-ttu-id="a2ac4-167">Weitere Informationen finden Sie in [Verbindungszeichenfolgen](../../miscellaneous/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="a2ac4-167">For more information, see [Connection Strings](../../miscellaneous/connection-strings.md).</span></span>

## <a name="create-a-controller-and-views"></a><span data-ttu-id="a2ac4-168">Erstellen von einem Controller und Ansichten</span><span class="sxs-lookup"><span data-stu-id="a2ac4-168">Create a controller and views</span></span>

* <span data-ttu-id="a2ac4-169">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Controller**, und wählen Sie **Hinzufügen > Controller** aus.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-169">Right-click on the **Controllers** folder in **Solution Explorer** and select **Add -> Controller...**</span></span>
* <span data-ttu-id="a2ac4-170">Wählen Sie **MVC-Controller mit Ansichten unter Verwendung von Entity Framework** aus, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-170">Select **MVC Controller with views, using Entity Framework** and click **Ok**</span></span>
* <span data-ttu-id="a2ac4-171">Legen Sie **Modellklasse** auf **Blog** und **Datenkontextklasse** auf **BloggingContext** fest.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-171">Set **Model class** to **Blog** and **Data context class** to **BloggingContext**</span></span>
* <span data-ttu-id="a2ac4-172">Klicken Sie auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-172">Click **Add**</span></span>

## <a name="run-the-application"></a><span data-ttu-id="a2ac4-173">Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="a2ac4-173">Run the application</span></span>

<span data-ttu-id="a2ac4-174">Sie können die Anwendung jetzt ausführen, um sie in Aktion zu sehen.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-174">You can now run the application to see it in action.</span></span>

* <span data-ttu-id="a2ac4-175">Klicken Sie auf **Debuggen > Ohne Debuggen starten**.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-175">**Debug -> Start Without Debugging**</span></span>
* <span data-ttu-id="a2ac4-176">Die Anwendung wird erstellt und in einem Webbrowser geöffnet.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-176">The application builds and opens in a web browser</span></span>
* <span data-ttu-id="a2ac4-177">Navigieren Sie zu `/Blogs`.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-177">Navigate to `/Blogs`</span></span>
* <span data-ttu-id="a2ac4-178">Klicken Sie auf **Neu erstellen**.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-178">Click **Create New**</span></span>
* <span data-ttu-id="a2ac4-179">Geben Sie eine **URL** für den neuen Blog ein, und klicken Sie auf **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="a2ac4-179">Enter a **Url** for the new blog and click **Create**</span></span>

  ![Seite „Create“](_static/create.png)

  ![Indexseite](_static/index-existing-db.png)

## <a name="next-steps"></a><span data-ttu-id="a2ac4-182">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="a2ac4-182">Next steps</span></span>

<span data-ttu-id="a2ac4-183">Weitere Informationen über das Erstellen des Gerüsts für einen Kontext und Entitätsklassen finden Sie in den folgenden Artikeln:</span><span class="sxs-lookup"><span data-stu-id="a2ac4-183">For more information about how to scaffold a context and entity classes, see the following articles:</span></span>
* [<span data-ttu-id="a2ac4-184">Reverse Engineering</span><span class="sxs-lookup"><span data-stu-id="a2ac4-184">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
* [<span data-ttu-id="a2ac4-185">Entity Framework Core Tools-Referenz – .NET-CLI</span><span class="sxs-lookup"><span data-stu-id="a2ac4-185">Entity Framework Core tools reference - .NET CLI</span></span>](xref:core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)
* [<span data-ttu-id="a2ac4-186">Entity Framework Core Tools-Referenz – Paket-Manager-Konsole</span><span class="sxs-lookup"><span data-stu-id="a2ac4-186">Entity Framework Core tools reference - Package Manager Console</span></span>](xref:core/miscellaneous/cli/powershell#scaffold-dbcontext)
