---
title: Code First mit einer vorhandenen Datenbank - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
ms.openlocfilehash: f05420beb3dff2d632151fcbf48986b0d9cd18ff
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490609"
---
# <a name="code-first-to-an-existing-database"></a><span data-ttu-id="76591-102">Code First mit einer vorhandenen Datenbank</span><span class="sxs-lookup"><span data-stu-id="76591-102">Code First to an Existing Database</span></span>
<span data-ttu-id="76591-103">Dieses video und schrittweise exemplarische Vorgehensweise bieten eine Einführung in die Code First-Entwicklung für eine vorhandene Datenbank.</span><span class="sxs-lookup"><span data-stu-id="76591-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting an existing database.</span></span> <span data-ttu-id="76591-104">Code können Sie Ihr Modell mithilfe von C definieren zuerst\# oder VB.Net-Klassen.</span><span class="sxs-lookup"><span data-stu-id="76591-104">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="76591-105">Optional zusätzliche Konfiguration kann mithilfe von Attributen auf Klassen und Eigenschaften oder mithilfe einer fluent-API ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="76591-105">Optionally additional configuration can be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="76591-106">Video ansehen</span><span class="sxs-lookup"><span data-stu-id="76591-106">Watch the video</span></span>
<span data-ttu-id="76591-107">Dieses Video ist [jetzt verfügbar auf Channel 9](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span><span class="sxs-lookup"><span data-stu-id="76591-107">This video is [now available on Channel 9](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="76591-108">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="76591-108">Pre-Requisites</span></span>

<span data-ttu-id="76591-109">Sie müssen **Visual Studio 2012** oder **Visual Studio 2013** zum Durchführen dieser exemplarischen Vorgehensweise installiert.</span><span class="sxs-lookup"><span data-stu-id="76591-109">You will need to have **Visual Studio 2012** or **Visual Studio 2013** installed to complete this walkthrough.</span></span>

<span data-ttu-id="76591-110">Sie benötigen außerdem die Version **6.1** (oder höher) von der **Entity Framework-Tools für Visual Studio** installiert.</span><span class="sxs-lookup"><span data-stu-id="76591-110">You will also need version **6.1** (or later) of the **Entity Framework Tools for Visual Studio** installed.</span></span> <span data-ttu-id="76591-111">Finden Sie unter [Entity Framework erhalten](~/ef6/fundamentals/install.md) Informationen zum Installieren der neuesten Version von Entity Framework-Tools.</span><span class="sxs-lookup"><span data-stu-id="76591-111">See [Get Entity Framework](~/ef6/fundamentals/install.md) for information on installing the latest version of the Entity Framework Tools.</span></span>

## <a name="1-create-an-existing-database"></a><span data-ttu-id="76591-112">1. Erstellen Sie eine vorhandene Datenbank</span><span class="sxs-lookup"><span data-stu-id="76591-112">1. Create an Existing Database</span></span>

<span data-ttu-id="76591-113">In der Regel müssen, wenn Sie eine vorhandene Datenbank verwenden möchten, die sie bereits erstellt wird, aber in dieser exemplarischen Vorgehensweise wir zum Erstellen einer Datenbank auf.</span><span class="sxs-lookup"><span data-stu-id="76591-113">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="76591-114">Wir jetzt, und Erstellen der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="76591-114">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="76591-115">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="76591-115">Open Visual Studio</span></span>
-   <span data-ttu-id="76591-116">**Ansicht – Profiler -&gt; Server-Explorer**</span><span class="sxs-lookup"><span data-stu-id="76591-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="76591-117">Klicken Sie mit der rechten Maustaste auf **Datenverbindungen -&gt; Verbindung hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="76591-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="76591-118">Wenn Sie mit einer Datenbank aus verbunden haben **Server-Explorer** , bevor Sie auswählen müssen, **Microsoft SQL Server** als Datenquelle</span><span class="sxs-lookup"><span data-stu-id="76591-118">If you haven’t connected to a database from **Server Explorer** before you’ll need to select **Microsoft SQL Server** as the data source</span></span>

    ![Auswählen einer Datenquelle](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="76591-120">Verbinden mit der LocalDB-Instanz, und geben Sie **Blogging** als Datenbankname</span><span class="sxs-lookup"><span data-stu-id="76591-120">Connect to your LocalDB instance, and enter **Blogging** as the database name</span></span>

    ![LocalDB-Verbindung](~/ef6/media/localdbconnection.png)

-   <span data-ttu-id="76591-122">Wählen Sie **OK** und Sie werden gefragt, ob Sie eine neue Datenbank, die auf erstellen möchten **Ja**</span><span class="sxs-lookup"><span data-stu-id="76591-122">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Datenbank-Dialogfeld "erstellen"](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="76591-124">Die neue Datenbank wird jetzt im Server-Explorer angezeigt. mit der rechten Maustaste darauf und wählen Sie **neue Abfrage**</span><span class="sxs-lookup"><span data-stu-id="76591-124">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="76591-125">Kopieren Sie die folgende SQL-Anweisung in die neue Abfrage, und klicken Sie dann mit der rechten Maustaste auf die Abfrage, und wählen **ausführen**</span><span class="sxs-lookup"><span data-stu-id="76591-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
CREATE TABLE [dbo].[Blogs] (
    [BlogId] INT IDENTITY (1, 1) NOT NULL,
    [Name] NVARCHAR (200) NULL,
    [Url]  NVARCHAR (200) NULL,
    CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
);

CREATE TABLE [dbo].[Posts] (
    [PostId] INT IDENTITY (1, 1) NOT NULL,
    [Title] NVARCHAR (200) NULL,
    [Content] NTEXT NULL,
    [BlogId] INT NOT NULL,
    CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
    CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
);

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('The Visual Studio Blog', 'http://blogs.msdn.com/visualstudio/')

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('.NET Framework Blog', 'http://blogs.msdn.com/dotnet/')
```

## <a name="2-create-the-application"></a><span data-ttu-id="76591-126">2. Erstellen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="76591-126">2. Create the Application</span></span>

<span data-ttu-id="76591-127">Einfachheit halber werden wir eine einfache Konsolenanwendung zu erstellen, die Code First verwendet, auf Daten zugreift:</span><span class="sxs-lookup"><span data-stu-id="76591-127">To keep things simple we’re going to build a basic console application that uses Code First to perform data access:</span></span>

-   <span data-ttu-id="76591-128">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="76591-128">Open Visual Studio</span></span>
-   <span data-ttu-id="76591-129">**Datei -&gt; neu –&gt; Projekt...**</span><span class="sxs-lookup"><span data-stu-id="76591-129">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="76591-130">Wählen Sie **Windows** im linken Menü und **-Konsolenanwendung**</span><span class="sxs-lookup"><span data-stu-id="76591-130">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="76591-131">Geben Sie **CodeFirstExistingDatabaseSample** als Name</span><span class="sxs-lookup"><span data-stu-id="76591-131">Enter **CodeFirstExistingDatabaseSample** as the name</span></span>
-   <span data-ttu-id="76591-132">Wählen Sie **OK** aus.</span><span class="sxs-lookup"><span data-stu-id="76591-132">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="76591-133">3. Reverse Engineering-Modells</span><span class="sxs-lookup"><span data-stu-id="76591-133">3. Reverse Engineer Model</span></span>

<span data-ttu-id="76591-134">Wir werden zu der die Entity Framework-Tools für Visual Studio verwenden, um uns die anfänglichen Code in der Datenbank zuordnen zu generieren.</span><span class="sxs-lookup"><span data-stu-id="76591-134">We’re going to make use of the Entity Framework Tools for Visual Studio to help us generate some initial code to map to the database.</span></span> <span data-ttu-id="76591-135">Diese Tools sind nur Code generiert, die Sie auch per hand eingeben könnten, falls gewünscht.</span><span class="sxs-lookup"><span data-stu-id="76591-135">These tools are just generating code that you could also type by hand if you prefer.</span></span>

-   <span data-ttu-id="76591-136">**Projekt -&gt; neues Element hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="76591-136">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="76591-137">Wählen Sie **Daten** im linken Menü und dann **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="76591-137">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="76591-138">Geben Sie **BloggingContext** als ein, und klicken Sie auf **OK**</span><span class="sxs-lookup"><span data-stu-id="76591-138">Enter **BloggingContext** as the name and click **OK**</span></span>
-   <span data-ttu-id="76591-139">Dadurch wird die **Entity Data Model-Assistenten**</span><span class="sxs-lookup"><span data-stu-id="76591-139">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="76591-140">Wählen Sie **Code First aus Datenbank** , und klicken Sie auf **weiter**</span><span class="sxs-lookup"><span data-stu-id="76591-140">Select **Code First from Database** and click **Next**</span></span>

    ![Assistent für eine CFE](~/ef6/media/wizardonecfe.png)

-   <span data-ttu-id="76591-142">Wählen Sie die Verbindung mit der Datenbank, die Sie im ersten Abschnitt erstellt haben, und klicken Sie auf **weiter**</span><span class="sxs-lookup"><span data-stu-id="76591-142">Select the connection to the database you created in the first section and click **Next**</span></span>

    ![Assistent zwei CFE](~/ef6/media/wizardtwocfe.png)

-   <span data-ttu-id="76591-144">Klicken Sie auf das Kontrollkästchen neben **Tabellen** importieren Sie alle Tabellen aus, und klicken Sie auf **Fertig stellen**</span><span class="sxs-lookup"><span data-stu-id="76591-144">Click the checkbox next to **Tables** to import all tables and click **Finish**</span></span>

    ![Drei CFE Assistenten](~/ef6/media/wizardthreecfe.png)

<span data-ttu-id="76591-146">Sobald der reverse Engineering-Prozess auf eine Anzahl von Elementen abgeschlossen ist wird wurde das Projekt, wir sehen Sie sich, was hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="76591-146">Once the reverse engineer process completes a number of items will have been added to the project, let's take a look at what's been added.</span></span>

### <a name="configuration-file"></a><span data-ttu-id="76591-147">Konfigurationsdatei</span><span class="sxs-lookup"><span data-stu-id="76591-147">Configuration file</span></span>

<span data-ttu-id="76591-148">Eine App.config-Datei wurde dem Projekt diese Datei die Verbindungszeichenfolge für die vorhandene Datenbank enthält.</span><span class="sxs-lookup"><span data-stu-id="76591-148">An App.config file has been added to the project, this file contains the connection string to the existing database.</span></span>

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

<span data-ttu-id="76591-149">*Sehen Sie einige andere Einstellungen in der Konfigurationsdatei zu, Hierbei handelt es sich um EF-Standardeinstellungen, die Code First mitzuteilen, wohin Datenbanken erstellt werden soll. Da wir zu einer vorhandenen Datenbank werden diese Einstellungen ignoriert in unserer Anwendung zugeordnet sind.*</span><span class="sxs-lookup"><span data-stu-id="76591-149">*You’ll notice some other settings in the configuration file too, these are default EF settings that tell Code First where to create databases. Since we are mapping to an existing database these setting will be ignored in our application.*</span></span>

### <a name="derived-context"></a><span data-ttu-id="76591-150">Abgeleiteten Kontext</span><span class="sxs-lookup"><span data-stu-id="76591-150">Derived Context</span></span>

<span data-ttu-id="76591-151">Ein **BloggingContext** Klasse dem Projekt hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="76591-151">A **BloggingContext** class has been added to the project.</span></span> <span data-ttu-id="76591-152">Der Kontext stellt eine Sitzung mit der Datenbank, die es uns ermöglicht, Abfragen und Speichern von Daten dar.</span><span class="sxs-lookup"><span data-stu-id="76591-152">The context represents a session with the database, allowing us to query and save data.</span></span>
<span data-ttu-id="76591-153">Stellt der Kontext einer **"DbSet"&lt;TEntity&gt;**  für jeden Typ in unserem Modell.</span><span class="sxs-lookup"><span data-stu-id="76591-153">The context exposes a **DbSet&lt;TEntity&gt;** for each type in our model.</span></span> <span data-ttu-id="76591-154">Sie werden bemerken, dass der Standardkonstruktor einer Basiskonstruktor mit Ruft die **Name =** Syntax.</span><span class="sxs-lookup"><span data-stu-id="76591-154">You’ll also notice that the default constructor calls a base constructor using the **name=** syntax.</span></span> <span data-ttu-id="76591-155">Dies weist Code First, dass die Verbindungszeichenfolge für diesen Kontext verwenden, die aus der Konfigurationsdatei geladen werden soll.</span><span class="sxs-lookup"><span data-stu-id="76591-155">This tells Code First that the connection string to use for this context should be loaded from the configuration file.</span></span>

``` csharp
public partial class BloggingContext : DbContext
    {
        public BloggingContext()
            : base("name=BloggingContext")
        {
        }

        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
        }
    }
```

<span data-ttu-id="76591-156">*Verwenden Sie immer die **Name =** Syntax, wenn Sie eine Verbindungszeichenfolge in der Config-Datei verwenden. Dadurch wird sichergestellt, dass wenn die Verbindungszeichenfolge nicht vorhanden ist. Klicken Sie dann auf Entity Framework ausgelöst wird anstatt eine neue Datenbank gemäß der Konvention zu erstellen.*</span><span class="sxs-lookup"><span data-stu-id="76591-156">*You should always use the **name=** syntax when you are using a connection string in the config file. This ensures that if the connection string is not present then Entity Framework will throw rather than creating a new database by convention.*</span></span>

### <a name="model-classes"></a><span data-ttu-id="76591-157">ViewModel-Klassen</span><span class="sxs-lookup"><span data-stu-id="76591-157">Model classes</span></span>

<span data-ttu-id="76591-158">Zum Schluss eine **Blog** und **Post** Klasse wurden ebenfalls dem Projekt hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="76591-158">Finally, a **Blog** and **Post** class have also been added to the project.</span></span> <span data-ttu-id="76591-159">Hierbei handelt es sich um Domänenklassen, die unser Modell bilden.</span><span class="sxs-lookup"><span data-stu-id="76591-159">These are the domain classes that make up our model.</span></span> <span data-ttu-id="76591-160">Sehen Sie Datenanmerkungen auf Klassen angewendet, an der Konfiguration, würde der Code First-Konventionen nicht die Struktur der vorhandenen Datenbank ausgerichtet.</span><span class="sxs-lookup"><span data-stu-id="76591-160">You'll see Data Annotations applied to the classes to specify configuration where the Code First conventions would not align with the structure of the existing database.</span></span> <span data-ttu-id="76591-161">Beispielsweise sehen Sie die **StringLength** Anmerkung zu **Blog.Name** und **Blog.Url** , da sie eine Höchstlänge von haben **200** in der Datenbank (der Code First standardmäßig wird die maximale Länge vom Datenbankanbieter - unterstützt verwendet **nvarchar(max)** in SQL Server).</span><span class="sxs-lookup"><span data-stu-id="76591-161">For example, you'll see the **StringLength** annotation on **Blog.Name** and **Blog.Url** since they have a maximum length of **200** in the database (the Code First default is to use the maximun length supported by the database provider - **nvarchar(max)** in SQL Server).</span></span>

``` csharp
public partial class Blog
{
    public Blog()
    {
        Posts = new HashSet<Post>();
    }

    public int BlogId { get; set; }

    [StringLength(200)]
    public string Name { get; set; }

    [StringLength(200)]
    public string Url { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}
```

## <a name="4-reading--writing-data"></a><span data-ttu-id="76591-162">4. Lesen und Schreiben von Daten</span><span class="sxs-lookup"><span data-stu-id="76591-162">4. Reading & Writing Data</span></span>

<span data-ttu-id="76591-163">Nun, da es sich um ein Modell verfügen, ist es Zeit, die sie verwenden, um einige Daten zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="76591-163">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="76591-164">Implementieren der **Main** -Methode in der **"Program.cs"** wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="76591-164">Implement the **Main** method in **Program.cs** as shown below.</span></span> <span data-ttu-id="76591-165">Dieser Code erstellt eine neue Instanz unseres Kontexts und anschließend verwendet, um eine neue einfügen **Blog**.</span><span class="sxs-lookup"><span data-stu-id="76591-165">This code creates a new instance of our context and then uses it to insert a new **Blog**.</span></span> <span data-ttu-id="76591-166">Anschließend er eine LINQ-Abfrage, verwendet um alle abzurufen **Blogs** aus der Datenbank, die alphabetisch nach **Titel**.</span><span class="sxs-lookup"><span data-stu-id="76591-166">Then it uses a LINQ query to retrieve all **Blogs** from the database ordered alphabetically by **Title**.</span></span>

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

<span data-ttu-id="76591-167">Sie können jetzt die Anwendung auszuführen, und probieren Sie es aus.</span><span class="sxs-lookup"><span data-stu-id="76591-167">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a><span data-ttu-id="76591-168">Was geschieht, wenn meine Datenbank geändert wird?</span><span class="sxs-lookup"><span data-stu-id="76591-168">What if My Database Changes?</span></span>

<span data-ttu-id="76591-169">Der Code First, Datenbank-Assistent dient zum Starten zeigen einige Klassen zu generieren, die Sie anschließend anpassen und ändern können.</span><span class="sxs-lookup"><span data-stu-id="76591-169">The Code First to Database wizard is designed to generate a starting point set of classes that you can then tweak and modify.</span></span> <span data-ttu-id="76591-170">Wenn am Datenbankschema Änderungen können Sie entweder manuell bearbeiten die Klassen oder führen Sie einen anderen reverse Engineering, um die Klassen zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="76591-170">If your database schema changes you can either manually edit the classes or perform another reverse engineer to overwrite the classes.</span></span>

## <a name="using-code-first-migrations-to-an-existing-database"></a><span data-ttu-id="76591-171">Mithilfe von Code First-Migrationen zu einer vorhandenen Datenbank</span><span class="sxs-lookup"><span data-stu-id="76591-171">Using Code First Migrations to an Existing Database</span></span>

<span data-ttu-id="76591-172">Wenn Sie Code First-Migrationen mit einer vorhandenen Datenbank verwenden möchten, finden Sie unter [Code First-Migrationen zu einer vorhandenen Datenbank](~/ef6/modeling/code-first/migrations/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="76591-172">If you want to use Code First Migrations with an existing database, see [Code First Migrations to an existing database](~/ef6/modeling/code-first/migrations/existing-database.md).</span></span>

## <a name="summary"></a><span data-ttu-id="76591-173">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="76591-173">Summary</span></span>

<span data-ttu-id="76591-174">In dieser exemplarischen Vorgehensweise haben Sie Code First-Entwicklung mithilfe einer vorhandenen Datenbank.</span><span class="sxs-lookup"><span data-stu-id="76591-174">In this walkthrough we looked at Code First development using an existing database.</span></span> <span data-ttu-id="76591-175">Verwendet die Entity Framework-Tools für Visual Studio für das reverse Engineering einer Reihe von Klassen, die in der Datenbank zugeordnet und kann zum Speichern und Abrufen von Daten verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="76591-175">We used the Entity Framework Tools for Visual Studio to reverse engineer a set of classes that mapped to the database and could be used to store and retrieve data.</span></span>
