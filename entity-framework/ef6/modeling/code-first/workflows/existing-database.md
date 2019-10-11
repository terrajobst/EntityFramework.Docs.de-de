---
title: Code First zu einer vorhandenen Datenbank EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
ms.openlocfilehash: 61980bbd1f236f496a9d4fd92aa52264f1454615
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182620"
---
# <a name="code-first-to-an-existing-database"></a><span data-ttu-id="51f03-102">Code First an eine vorhandene Datenbank</span><span class="sxs-lookup"><span data-stu-id="51f03-102">Code First to an Existing Database</span></span>
<span data-ttu-id="51f03-103">Dieses Video und die schrittweise exemplarische Vorgehensweise bieten eine Einführung in Code First Entwicklung für eine vorhandene Datenbank.</span><span class="sxs-lookup"><span data-stu-id="51f03-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting an existing database.</span></span> <span data-ttu-id="51f03-104">Mit Code First können Sie das Modell mithilfe der Klassen C @ no__t-0 oder VB.net definieren.</span><span class="sxs-lookup"><span data-stu-id="51f03-104">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="51f03-105">Optional kann eine zusätzliche Konfiguration mithilfe von Attributen in ihren Klassen und Eigenschaften oder mithilfe einer fließenden API ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="51f03-105">Optionally additional configuration can be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="51f03-106">Video ansehen</span><span class="sxs-lookup"><span data-stu-id="51f03-106">Watch the video</span></span>
<span data-ttu-id="51f03-107">Dieses Video ist [nun auf Channel 9 verfügbar](https://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span><span class="sxs-lookup"><span data-stu-id="51f03-107">This video is [now available on Channel 9](https://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="51f03-108">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="51f03-108">Pre-Requisites</span></span>

<span data-ttu-id="51f03-109">Sie müssen **Visual Studio 2012** oder **Visual Studio 2013** installiert haben, um diese exemplarische Vorgehensweise abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="51f03-109">You will need to have **Visual Studio 2012** or **Visual Studio 2013** installed to complete this walkthrough.</span></span>

<span data-ttu-id="51f03-110">Sie benötigen auch Version **6,1** (oder höher) der **Entity Framework Tools für Visual Studio** .</span><span class="sxs-lookup"><span data-stu-id="51f03-110">You will also need version **6.1** (or later) of the **Entity Framework Tools for Visual Studio** installed.</span></span> <span data-ttu-id="51f03-111">Weitere Informationen zum Installieren der neuesten Version des Entity Framework Tools finden [Sie unter Get Entity Framework](~/ef6/fundamentals/install.md) .</span><span class="sxs-lookup"><span data-stu-id="51f03-111">See [Get Entity Framework](~/ef6/fundamentals/install.md) for information on installing the latest version of the Entity Framework Tools.</span></span>

## <a name="1-create-an-existing-database"></a><span data-ttu-id="51f03-112">1. Erstellen einer vorhandenen Datenbank</span><span class="sxs-lookup"><span data-stu-id="51f03-112">1. Create an Existing Database</span></span>

<span data-ttu-id="51f03-113">Wenn Sie eine vorhandene Datenbank als Ziel haben, wird Sie in der Regel bereits erstellt, aber in dieser exemplarischen Vorgehensweise müssen wir eine Datenbank erstellen, auf die zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="51f03-113">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="51f03-114">Nun generieren wir die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="51f03-114">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="51f03-115">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="51f03-115">Open Visual Studio</span></span>
-   <span data-ttu-id="51f03-116">**View-&gt; Server-Explorer**</span><span class="sxs-lookup"><span data-stu-id="51f03-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="51f03-117">Klicken Sie mit der rechten Maustaste auf **Datenverbindungen-&gt; Verbindung hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="51f03-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="51f03-118">Wenn Sie über **Server-Explorer** keine Verbindung mit einer Datenbank hergestellt haben, müssen Sie **Microsoft SQL Server** als Datenquelle auswählen.</span><span class="sxs-lookup"><span data-stu-id="51f03-118">If you haven’t connected to a database from **Server Explorer** before you’ll need to select **Microsoft SQL Server** as the data source</span></span>

    ![Datenquelle auswählen](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="51f03-120">Stellen Sie eine Verbindung mit ihrer localdb-Instanz her, und geben Sie als Datenbanknamen **Blog**</span><span class="sxs-lookup"><span data-stu-id="51f03-120">Connect to your LocalDB instance, and enter **Blogging** as the database name</span></span>

    ![Localdb-Verbindung](~/ef6/media/localdbconnection.png)

-   <span data-ttu-id="51f03-122">Wählen Sie **OK** aus, und Sie werden gefragt, ob Sie eine neue Datenbank erstellen möchten, und wählen Sie **Ja** aus.</span><span class="sxs-lookup"><span data-stu-id="51f03-122">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Dialog Feld Create Database](~/ef6/media/createdatabasedialog.png)

-   <span data-ttu-id="51f03-124">Die neue Datenbank wird nun in Server-Explorer angezeigt, klicken Sie mit der rechten Maustaste darauf, und wählen Sie **neue Abfrage** aus.</span><span class="sxs-lookup"><span data-stu-id="51f03-124">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="51f03-125">Kopieren Sie den folgenden SQL-Befehl in die neue Abfrage, klicken Sie mit der rechten Maustaste auf die Abfrage, und wählen Sie **Ausführen** .</span><span class="sxs-lookup"><span data-stu-id="51f03-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

## <a name="2-create-the-application"></a><span data-ttu-id="51f03-126">2. Erstellen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="51f03-126">2. Create the Application</span></span>

<span data-ttu-id="51f03-127">Um dies zu gewährleisten, erstellen wir eine einfache Konsolenanwendung, die Code First für den Datenzugriff verwendet:</span><span class="sxs-lookup"><span data-stu-id="51f03-127">To keep things simple we’re going to build a basic console application that uses Code First to perform data access:</span></span>

-   <span data-ttu-id="51f03-128">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="51f03-128">Open Visual Studio</span></span>
-   <span data-ttu-id="51f03-129">**Datei-&gt; New-&gt;-Projekt...**</span><span class="sxs-lookup"><span data-stu-id="51f03-129">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="51f03-130">Wählen Sie im Menü auf der linken Seite und **Konsolenanwendung** **Windows** aus.</span><span class="sxs-lookup"><span data-stu-id="51f03-130">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="51f03-131">Geben Sie **codefirstexistingdatabasesample** als Name ein.</span><span class="sxs-lookup"><span data-stu-id="51f03-131">Enter **CodeFirstExistingDatabaseSample** as the name</span></span>
-   <span data-ttu-id="51f03-132">Wählen Sie **OK** aus.</span><span class="sxs-lookup"><span data-stu-id="51f03-132">Select **OK**</span></span>

 

## <a name="3-reverse-engineer-model"></a><span data-ttu-id="51f03-133">3. Reverse Engineering-Modell</span><span class="sxs-lookup"><span data-stu-id="51f03-133">3. Reverse Engineer Model</span></span>

<span data-ttu-id="51f03-134">Wir verwenden die Entity Framework Tools für Visual Studio, um uns dabei zu unterstützen, Code zu generieren, der der Datenbank zugeordnet werden soll.</span><span class="sxs-lookup"><span data-stu-id="51f03-134">We’re going to make use of the Entity Framework Tools for Visual Studio to help us generate some initial code to map to the database.</span></span> <span data-ttu-id="51f03-135">Diese Tools erzeugen lediglich Code, den Sie bei Bedarf auch per Hand eingeben können.</span><span class="sxs-lookup"><span data-stu-id="51f03-135">These tools are just generating code that you could also type by hand if you prefer.</span></span>

-   <span data-ttu-id="51f03-136">**Project-&gt; neues Element hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="51f03-136">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="51f03-137">Wählen Sie im linken Menü **Daten** aus, und klicken Sie dann auf **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="51f03-137">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="51f03-138">Geben Sie **bloggingcontext** als Name ein, und klicken Sie auf **OK** .</span><span class="sxs-lookup"><span data-stu-id="51f03-138">Enter **BloggingContext** as the name and click **OK**</span></span>
-   <span data-ttu-id="51f03-139">Dadurch wird der **Entity Data Model-Assistent** gestartet.</span><span class="sxs-lookup"><span data-stu-id="51f03-139">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="51f03-140">Wählen Sie **aus Datenbank Code First aus** , und klicken Sie auf **weiter**</span><span class="sxs-lookup"><span data-stu-id="51f03-140">Select **Code First from Database** and click **Next**</span></span>

    ![Assistent ein CFE](~/ef6/media/wizardonecfe.png)

-   <span data-ttu-id="51f03-142">Wählen Sie die Verbindung mit der Datenbank, die Sie im ersten Abschnitt erstellt haben, und klicken Sie auf **weiter** .</span><span class="sxs-lookup"><span data-stu-id="51f03-142">Select the connection to the database you created in the first section and click **Next**</span></span>

    ![Assistent zwei CFE](~/ef6/media/wizardtwocfe.png)

-   <span data-ttu-id="51f03-144">Aktivieren Sie das Kontrollkästchen neben **Tabellen** , um alle Tabellen zu importieren, **und klicken Sie**</span><span class="sxs-lookup"><span data-stu-id="51f03-144">Click the checkbox next to **Tables** to import all tables and click **Finish**</span></span>

    ![Assistent drei CFE](~/ef6/media/wizardthreecfe.png)

<span data-ttu-id="51f03-146">Nachdem der Reverse-Engineering-Prozess abgeschlossen wurde, wird dem Projekt eine Reihe von Elementen hinzugefügt. wir sehen uns nun an, was hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="51f03-146">Once the reverse engineer process completes a number of items will have been added to the project, let's take a look at what's been added.</span></span>

### <a name="configuration-file"></a><span data-ttu-id="51f03-147">Konfigurationsdatei</span><span class="sxs-lookup"><span data-stu-id="51f03-147">Configuration file</span></span>

<span data-ttu-id="51f03-148">Eine APP. config-Datei wurde dem Projekt hinzugefügt. diese Datei enthält die Verbindungs Zeichenfolge für die vorhandene Datenbank.</span><span class="sxs-lookup"><span data-stu-id="51f03-148">An App.config file has been added to the project, this file contains the connection string to the existing database.</span></span>

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

<span data-ttu-id="51f03-149">*Sie sehen auch einige andere Einstellungen in der Konfigurationsdatei. Dies sind die standardmäßigen EF-Einstellungen, die Code First Speicherort der Datenbanken angeben. Da wir eine Zuordnung zu einer vorhandenen Datenbank durchführt, werden diese Einstellungen in unserer Anwendung ignoriert.*</span><span class="sxs-lookup"><span data-stu-id="51f03-149">*You’ll notice some other settings in the configuration file too, these are default EF settings that tell Code First where to create databases. Since we are mapping to an existing database these setting will be ignored in our application.*</span></span>

### <a name="derived-context"></a><span data-ttu-id="51f03-150">Abgeleiteter Kontext</span><span class="sxs-lookup"><span data-stu-id="51f03-150">Derived Context</span></span>

<span data-ttu-id="51f03-151">Dem Projekt wurde eine **bloggingcontext** -Klasse hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="51f03-151">A **BloggingContext** class has been added to the project.</span></span> <span data-ttu-id="51f03-152">Der Kontext stellt eine Sitzung mit der Datenbank dar und ermöglicht das Abfragen und Speichern von Daten.</span><span class="sxs-lookup"><span data-stu-id="51f03-152">The context represents a session with the database, allowing us to query and save data.</span></span>
<span data-ttu-id="51f03-153">Der Kontext macht ein **dbset @ no__t-1tentity @ no__t-2** für jeden Typ in unserem Modell verfügbar.</span><span class="sxs-lookup"><span data-stu-id="51f03-153">The context exposes a **DbSet&lt;TEntity&gt;** for each type in our model.</span></span> <span data-ttu-id="51f03-154">Sie werden auch bemerken, dass der Standardkonstruktor einen Basiskonstruktor mit der Syntax **Name =** aufruft.</span><span class="sxs-lookup"><span data-stu-id="51f03-154">You’ll also notice that the default constructor calls a base constructor using the **name=** syntax.</span></span> <span data-ttu-id="51f03-155">Dadurch wird Code First, dass die für diesen Kontext zu verwendende Verbindungs Zeichenfolge aus der Konfigurationsdatei geladen werden soll.</span><span class="sxs-lookup"><span data-stu-id="51f03-155">This tells Code First that the connection string to use for this context should be loaded from the configuration file.</span></span>

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

<span data-ttu-id="51f03-156">*Sie sollten immer die Syntax **Name =** verwenden, wenn Sie eine Verbindungs Zeichenfolge in der Konfigurationsdatei verwenden. Dadurch wird sichergestellt, dass, wenn die Verbindungs Zeichenfolge nicht vorhanden ist, Entity Framework ausgelöst wird, anstatt eine neue Datenbank gemäß der Konvention zu erstellen.*</span><span class="sxs-lookup"><span data-stu-id="51f03-156">*You should always use the **name=** syntax when you are using a connection string in the config file. This ensures that if the connection string is not present then Entity Framework will throw rather than creating a new database by convention.*</span></span>

### <a name="model-classes"></a><span data-ttu-id="51f03-157">Modellklassen</span><span class="sxs-lookup"><span data-stu-id="51f03-157">Model classes</span></span>

<span data-ttu-id="51f03-158">Schließlich wurden dem Projekt auch ein **Blog** und eine **Post** -Klasse hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="51f03-158">Finally, a **Blog** and **Post** class have also been added to the project.</span></span> <span data-ttu-id="51f03-159">Dabei handelt es sich um die Domänen Klassen, aus denen unser Modell besteht.</span><span class="sxs-lookup"><span data-stu-id="51f03-159">These are the domain classes that make up our model.</span></span> <span data-ttu-id="51f03-160">Sie sehen Daten Anmerkungen, die auf die Klassen angewendet werden, um die Konfiguration anzugeben, bei der die Code First Konventionen nicht mit der Struktur der vorhandenen Datenbank übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="51f03-160">You'll see Data Annotations applied to the classes to specify configuration where the Code First conventions would not align with the structure of the existing database.</span></span> <span data-ttu-id="51f03-161">Beispielsweise wird die **StringLength** -Anmerkung auf **Blog.Name** und " **Blog. URL** " angezeigt, da Sie eine maximale Länge von **200** in der Datenbank aufweisen (der Code First Standard ist die Verwendung der maximalen Länge, die vom Datenbankanbieter unterstützt wird. **nvarchar (max)** in SQL Server).</span><span class="sxs-lookup"><span data-stu-id="51f03-161">For example, you'll see the **StringLength** annotation on **Blog.Name** and **Blog.Url** since they have a maximum length of **200** in the database (the Code First default is to use the maximun length supported by the database provider - **nvarchar(max)** in SQL Server).</span></span>

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

## <a name="4-reading--writing-data"></a><span data-ttu-id="51f03-162">4. Lesen & Schreiben von Daten</span><span class="sxs-lookup"><span data-stu-id="51f03-162">4. Reading & Writing Data</span></span>

<span data-ttu-id="51f03-163">Nachdem wir nun über ein Modell verfügen, ist es an der Zeit, es für den Zugriff auf einige Daten zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="51f03-163">Now that we have a model it’s time to use it to access some data.</span></span> <span data-ttu-id="51f03-164">Implementieren Sie die **Main** -Methode in **Program.cs** , wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="51f03-164">Implement the **Main** method in **Program.cs** as shown below.</span></span> <span data-ttu-id="51f03-165">Mit diesem Code wird eine neue Instanz des Kontexts erstellt und anschließend verwendet, um einen neuen **Blog**einzufügen.</span><span class="sxs-lookup"><span data-stu-id="51f03-165">This code creates a new instance of our context and then uses it to insert a new **Blog**.</span></span> <span data-ttu-id="51f03-166">Anschließend wird eine LINQ-Abfrage verwendet, um alle **Blogs** aus der Datenbank alphabetisch nach **Titel**abzurufen.</span><span class="sxs-lookup"><span data-stu-id="51f03-166">Then it uses a LINQ query to retrieve all **Blogs** from the database ordered alphabetically by **Title**.</span></span>

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

<span data-ttu-id="51f03-167">Sie können die Anwendung jetzt ausführen und testen.</span><span class="sxs-lookup"><span data-stu-id="51f03-167">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a><span data-ttu-id="51f03-168">Was geschieht, wenn meine Datenbank geändert wird?</span><span class="sxs-lookup"><span data-stu-id="51f03-168">What if My Database Changes?</span></span>

<span data-ttu-id="51f03-169">Der Assistent für die Code First-Datenbank ist darauf ausgelegt, einen Ausgangspunkt Satz von Klassen zu generieren, die Sie dann optimieren und ändern können.</span><span class="sxs-lookup"><span data-stu-id="51f03-169">The Code First to Database wizard is designed to generate a starting point set of classes that you can then tweak and modify.</span></span> <span data-ttu-id="51f03-170">Wenn das Datenbankschema geändert wird, können Sie entweder die Klassen manuell bearbeiten oder einen weiteren umgekehrten Ingenieur ausführen, um die Klassen zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="51f03-170">If your database schema changes you can either manually edit the classes or perform another reverse engineer to overwrite the classes.</span></span>

## <a name="using-code-first-migrations-to-an-existing-database"></a><span data-ttu-id="51f03-171">Verwenden von Code First-Migrationen für eine vorhandene Datenbank</span><span class="sxs-lookup"><span data-stu-id="51f03-171">Using Code First Migrations to an Existing Database</span></span>

<span data-ttu-id="51f03-172">Wenn Sie Code First-Migrationen mit einer vorhandenen Datenbank verwenden möchten, lesen Sie [Code First-Migrationen zu einer vorhandenen Datenbank](~/ef6/modeling/code-first/migrations/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="51f03-172">If you want to use Code First Migrations with an existing database, see [Code First Migrations to an existing database](~/ef6/modeling/code-first/migrations/existing-database.md).</span></span>

## <a name="summary"></a><span data-ttu-id="51f03-173">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="51f03-173">Summary</span></span>

<span data-ttu-id="51f03-174">In dieser exemplarischen Vorgehensweise haben wir Code First Entwicklung mit einer vorhandenen Datenbank untersucht.</span><span class="sxs-lookup"><span data-stu-id="51f03-174">In this walkthrough we looked at Code First development using an existing database.</span></span> <span data-ttu-id="51f03-175">Wir haben die Entity Framework Tools für Visual Studio verwendet, um einen Satz von Klassen umzukehren, die der Datenbank zugeordnet sind und zum Speichern und Abrufen von Daten verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="51f03-175">We used the Entity Framework Tools for Visual Studio to reverse engineer a set of classes that mapped to the database and could be used to store and retrieve data.</span></span>
