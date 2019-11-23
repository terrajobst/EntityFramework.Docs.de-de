---
title: Code First einer neuen Datenbank EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: d540fc6e84049f345ae22998f94c309e0be73fc3
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182572"
---
# <a name="code-first-to-a-new-database"></a><span data-ttu-id="ae55e-102">Code First einer neuen Datenbank</span><span class="sxs-lookup"><span data-stu-id="ae55e-102">Code First to a New Database</span></span>
<span data-ttu-id="ae55e-103">Dieses Video und die schrittweise exemplarische Vorgehensweise bieten eine Einführung in Code First Entwicklung für eine neue Datenbank.</span><span class="sxs-lookup"><span data-stu-id="ae55e-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="ae55e-104">Dieses Szenario umfasst das Ziel einer Datenbank, die nicht vorhanden ist und Code First erstellt wird, oder eine leere Datenbank, der Code First neue Tabellen hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="ae55e-104">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables to.</span></span> <span data-ttu-id="ae55e-105">Mit Code First können Sie das Modell mithilfe von C\#-oder VB.NET-Klassen definieren.</span><span class="sxs-lookup"><span data-stu-id="ae55e-105">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="ae55e-106">Die zusätzliche Konfiguration kann optional mithilfe von Attributen in ihren Klassen und Eigenschaften oder mithilfe einer fließenden API ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="ae55e-106">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="ae55e-107">Video ansehen</span><span class="sxs-lookup"><span data-stu-id="ae55e-107">Watch the video</span></span>
<span data-ttu-id="ae55e-108">Dieses Video bietet eine Einführung in Code First Entwicklung für eine neue Datenbank.</span><span class="sxs-lookup"><span data-stu-id="ae55e-108">This video provides an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="ae55e-109">Dieses Szenario umfasst das Ziel einer Datenbank, die nicht vorhanden ist und Code First erstellt wird, oder eine leere Datenbank, der Code First neue Tabellen hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="ae55e-109">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables to.</span></span> <span data-ttu-id="ae55e-110">Mit Code First können Sie das Modell mithilfe C# von-oder VB.NET-Klassen definieren.</span><span class="sxs-lookup"><span data-stu-id="ae55e-110">Code First allows you to define your model using C# or VB.Net classes.</span></span> <span data-ttu-id="ae55e-111">Die zusätzliche Konfiguration kann optional mithilfe von Attributen in ihren Klassen und Eigenschaften oder mithilfe einer fließenden API ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="ae55e-111">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

<span data-ttu-id="ae55e-112">**Präsentation:** [Rowan Miller](https://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="ae55e-112">**Presented By**: [Rowan Miller](https://romiller.com/)</span></span>

<span data-ttu-id="ae55e-113">**Video**: [WMV](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (zip)](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span><span class="sxs-lookup"><span data-stu-id="ae55e-113">**Video**: [WMV](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="ae55e-114">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="ae55e-114">Pre-Requisites</span></span>

<span data-ttu-id="ae55e-115">Sie müssen mindestens Visual Studio 2010 oder Visual Studio 2012 installiert haben, um diese exemplarische Vorgehensweise abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="ae55e-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="ae55e-116">Wenn Sie Visual Studio 2010 verwenden, müssen Sie auch [nuget](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installieren.</span><span class="sxs-lookup"><span data-stu-id="ae55e-116">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="ae55e-117">1. Erstellen Sie die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="ae55e-117">1. Create the Application</span></span>

<span data-ttu-id="ae55e-118">Um die Dinge einfach zu halten, erstellen wir eine einfache Konsolenanwendung, die Code First verwendet, um Daten Zugriffe auszuführen.</span><span class="sxs-lookup"><span data-stu-id="ae55e-118">To keep things simple we’re going to build a basic console application that uses Code First to perform data access.</span></span>

-   <span data-ttu-id="ae55e-119">Öffnen Sie Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ae55e-119">Open Visual Studio</span></span>
-   <span data-ttu-id="ae55e-120">**Datei&gt; Projekt für neue&gt;...**</span><span class="sxs-lookup"><span data-stu-id="ae55e-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="ae55e-121">Wählen Sie im Menü auf der linken Seite und **Konsolenanwendung** **Windows** aus.</span><span class="sxs-lookup"><span data-stu-id="ae55e-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="ae55e-122">Geben Sie **codefirstnewdatabasesample** als Name ein.</span><span class="sxs-lookup"><span data-stu-id="ae55e-122">Enter **CodeFirstNewDatabaseSample** as the name</span></span>
-   <span data-ttu-id="ae55e-123">Wählen Sie **OK** aus.</span><span class="sxs-lookup"><span data-stu-id="ae55e-123">Select **OK**</span></span>

## <a name="2-create-the-model"></a><span data-ttu-id="ae55e-124">2. Erstellen des Modells</span><span class="sxs-lookup"><span data-stu-id="ae55e-124">2. Create the Model</span></span>

<span data-ttu-id="ae55e-125">Definieren wir ein sehr einfaches Modell mithilfe von Klassen.</span><span class="sxs-lookup"><span data-stu-id="ae55e-125">Let’s define a very simple model using classes.</span></span> <span data-ttu-id="ae55e-126">Wir definieren Sie lediglich in der Program.cs-Datei, aber in einer realen Anwendung würden Sie Ihre Klassen in separate Dateien und potenziell ein separates Projekt aufteilen.</span><span class="sxs-lookup"><span data-stu-id="ae55e-126">We’re just defining them in the Program.cs file but in a real world application you would split your classes out into separate files and potentially a separate project.</span></span>

<span data-ttu-id="ae55e-127">Fügen Sie unter der Programm Klassendefinition in Program.cs die beiden folgenden Klassen hinzu.</span><span class="sxs-lookup"><span data-stu-id="ae55e-127">Below the Program class definition in Program.cs add the following two classes.</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; }
    public virtual Blog Blog { get; set; }
}
```

<span data-ttu-id="ae55e-128">Sie werden feststellen, dass die beiden Navigations Eigenschaften (Blog. Posts und Post. Blog) virtuell sind.</span><span class="sxs-lookup"><span data-stu-id="ae55e-128">You’ll notice that we’re making the two navigation properties (Blog.Posts and Post.Blog) virtual.</span></span> <span data-ttu-id="ae55e-129">Dies ermöglicht die Lazy Load-Funktion von Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ae55e-129">This enables the Lazy Loading feature of Entity Framework.</span></span> <span data-ttu-id="ae55e-130">Lazy Load bedeutet, dass der Inhalt dieser Eigenschaften automatisch aus der Datenbank geladen wird, wenn Sie versuchen, darauf zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="ae55e-130">Lazy Loading means that the contents of these properties will be automatically loaded from the database when you try to access them.</span></span>

## <a name="3-create-a-context"></a><span data-ttu-id="ae55e-131">3. Erstellen eines Kontexts</span><span class="sxs-lookup"><span data-stu-id="ae55e-131">3. Create a Context</span></span>

<span data-ttu-id="ae55e-132">Nun ist es an der Zeit, einen abgeleiteten Kontext zu definieren, der eine Sitzung mit der Datenbank darstellt, sodass wir Daten Abfragen und speichern können.</span><span class="sxs-lookup"><span data-stu-id="ae55e-132">Now it’s time to define a derived context, which represents a session with the database, allowing us to query and save data.</span></span> <span data-ttu-id="ae55e-133">Wir definieren einen Kontext, der von System. Data. Entity. dbcontext abgeleitet ist und für jede Klasse in unserem Modell eine typisierte dbset-&lt;TEntity-&gt; verfügbar macht.</span><span class="sxs-lookup"><span data-stu-id="ae55e-133">We define a context that derives from System.Data.Entity.DbContext and exposes a typed DbSet&lt;TEntity&gt; for each class in our model.</span></span>

<span data-ttu-id="ae55e-134">Wir beginnen jetzt mit der Verwendung von Typen aus der Entity Framework daher müssen wir das nuget-Paket "EntityFramework" hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="ae55e-134">We’re now starting to use types from the Entity Framework so we need to add the EntityFramework NuGet package.</span></span>

-   <span data-ttu-id="ae55e-135">**Project –&gt; nuget-Pakete verwalten...**</span><span class="sxs-lookup"><span data-stu-id="ae55e-135">**Project –&gt; Manage NuGet Packages…**</span></span>
    <span data-ttu-id="ae55e-136">Hinweis: Wenn Sie nicht über die **nuget-Pakete verwalten verfügen...**</span><span class="sxs-lookup"><span data-stu-id="ae55e-136">Note: If you don’t have the **Manage NuGet Packages…**</span></span> <span data-ttu-id="ae55e-137">Option Sie sollten die [neueste Version von nuget](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installieren.</span><span class="sxs-lookup"><span data-stu-id="ae55e-137">option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span></span>
-   <span data-ttu-id="ae55e-138">Auswählen der Registerkarte " **Online** "</span><span class="sxs-lookup"><span data-stu-id="ae55e-138">Select the **Online** tab</span></span>
-   <span data-ttu-id="ae55e-139">Auswählen des " **EntityFramework** "-Pakets</span><span class="sxs-lookup"><span data-stu-id="ae55e-139">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="ae55e-140">Klicken Sie auf **Installieren**</span><span class="sxs-lookup"><span data-stu-id="ae55e-140">Click **Install**</span></span>

<span data-ttu-id="ae55e-141">Fügen Sie am Anfang von Program.cs eine using-Anweisung für System. Data. Entity hinzu.</span><span class="sxs-lookup"><span data-stu-id="ae55e-141">Add a using statement for System.Data.Entity at the top of Program.cs.</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="ae55e-142">Fügen Sie unter der Post-Klasse in Program.cs den folgenden abgeleiteten Kontext hinzu.</span><span class="sxs-lookup"><span data-stu-id="ae55e-142">Below the Post class in Program.cs add the following derived context.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

<span data-ttu-id="ae55e-143">Im folgenden finden Sie eine komplette Liste der Komponenten, die Program.cs jetzt enthalten sollte.</span><span class="sxs-lookup"><span data-stu-id="ae55e-143">Here is a complete listing of what Program.cs should now contain.</span></span>

``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Entity;

namespace CodeFirstNewDatabaseSample
{
    class Program
    {
        static void Main(string[] args)
        {
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }

        public virtual List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }

    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
    }
}
```

<span data-ttu-id="ae55e-144">Dies ist der gesamte Code, der zum Speichern und Abrufen von Daten erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="ae55e-144">That is all the code we need to start storing and retrieving data.</span></span> <span data-ttu-id="ae55e-145">Natürlich gibt es im Hintergrund etwas, das wir in einem Moment sehen werden, aber zuerst sehen wir uns dies in Aktion an.</span><span class="sxs-lookup"><span data-stu-id="ae55e-145">Obviously there is quite a bit going on behind the scenes and we’ll take a look at that in a moment but first let’s see it in action.</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="ae55e-146">4. Lesen & Schreiben von Daten</span><span class="sxs-lookup"><span data-stu-id="ae55e-146">4. Reading & Writing Data</span></span>

<span data-ttu-id="ae55e-147">Implementieren Sie die Main-Methode in Program.cs, wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="ae55e-147">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="ae55e-148">Mit diesem Code wird eine neue Instanz des Kontexts erstellt und anschließend verwendet, um einen neuen Blog einzufügen.</span><span class="sxs-lookup"><span data-stu-id="ae55e-148">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="ae55e-149">Anschließend wird eine LINQ-Abfrage verwendet, um alle Blogs aus der Datenbank alphabetisch nach Titel abzurufen.</span><span class="sxs-lookup"><span data-stu-id="ae55e-149">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="ae55e-150">Sie können die Anwendung jetzt ausführen und testen.</span><span class="sxs-lookup"><span data-stu-id="ae55e-150">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a><span data-ttu-id="ae55e-151">Wo sind meine Daten?</span><span class="sxs-lookup"><span data-stu-id="ae55e-151">Where’s My Data?</span></span>

<span data-ttu-id="ae55e-152">Gemäß der Konvention hat dbcontext eine Datenbank für Sie erstellt.</span><span class="sxs-lookup"><span data-stu-id="ae55e-152">By convention DbContext has created a database for you.</span></span>

-   <span data-ttu-id="ae55e-153">Wenn eine lokale SQL Express-Instanz verfügbar ist (standardmäßig mit Visual Studio 2010 installiert), hat Code First die Datenbank auf dieser Instanz erstellt.</span><span class="sxs-lookup"><span data-stu-id="ae55e-153">If a local SQL Express instance is available (installed by default with Visual Studio 2010) then Code First has created the database on that instance</span></span>
-   <span data-ttu-id="ae55e-154">Wenn SQL Express nicht verfügbar ist, werden Code First versuchen, [localdb](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (standardmäßig mit Visual Studio 2012 installiert) zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="ae55e-154">If SQL Express isn’t available then Code First will try and use [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (installed by default with Visual Studio 2012)</span></span>
-   <span data-ttu-id="ae55e-155">Die Datenbank wird nach dem voll qualifizierten Namen des abgeleiteten Kontexts benannt, in unserem Fall **codefirstnewdatabasesample. bloggingcontext.**</span><span class="sxs-lookup"><span data-stu-id="ae55e-155">The database is named after the fully qualified name of the derived context, in our case that is **CodeFirstNewDatabaseSample.BloggingContext**</span></span>

<span data-ttu-id="ae55e-156">Dabei handelt es sich nur um die Standard Konventionen, und es gibt verschiedene Möglichkeiten, die Datenbank zu ändern, die Code First verwendet. Weitere Informationen finden Sie im Thema **wie dbcontext das Modell und die Datenbankverbindung** erkennt.</span><span class="sxs-lookup"><span data-stu-id="ae55e-156">These are just the default conventions and there are various ways to change the database that Code First uses, more information is available in the **How DbContext Discovers the Model and Database Connection** topic.</span></span>
<span data-ttu-id="ae55e-157">Sie können mit Server-Explorer in Visual Studio eine Verbindung mit dieser Datenbank herstellen.</span><span class="sxs-lookup"><span data-stu-id="ae55e-157">You can connect to this database using Server Explorer in Visual Studio</span></span>

-   <span data-ttu-id="ae55e-158">**&gt; Server-Explorer anzeigen**</span><span class="sxs-lookup"><span data-stu-id="ae55e-158">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="ae55e-159">Klicken Sie mit der rechten Maustaste auf **Datenverbindungen** , und wählen Sie **Verbindung hinzufügen aus.**</span><span class="sxs-lookup"><span data-stu-id="ae55e-159">Right click on **Data Connections** and select **Add Connection…**</span></span>
-   <span data-ttu-id="ae55e-160">Wenn Sie über Server-Explorer keine Verbindung mit einer Datenbank hergestellt haben, müssen Sie Microsoft SQL Server als Datenquelle auswählen.</span><span class="sxs-lookup"><span data-stu-id="ae55e-160">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Datenquelle auswählen](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="ae55e-162">Stellen Sie eine Verbindung mit localdb oder SQL Express her, je nachdem, welche installiert wurde.</span><span class="sxs-lookup"><span data-stu-id="ae55e-162">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>

<span data-ttu-id="ae55e-163">Wir können nun das Schema überprüfen, das von Code First erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="ae55e-163">We can now inspect the schema that Code First created.</span></span>

![Schema anfänglich](~/ef6/media/schemainitial.png)

<span data-ttu-id="ae55e-165">Dbcontext hat festgelegt, welche Klassen in das Modell aufgenommen werden sollen, indem wir die von uns definierten dbset-Eigenschaften betrachten.</span><span class="sxs-lookup"><span data-stu-id="ae55e-165">DbContext worked out what classes to include in the model by looking at the DbSet properties that we defined.</span></span> <span data-ttu-id="ae55e-166">Anschließend wird der Standardsatz von Code First Konventionen verwendet, um Tabellen-und Spaltennamen zu bestimmen, Datentypen zu ermitteln, Primärschlüssel zu suchen usw.</span><span class="sxs-lookup"><span data-stu-id="ae55e-166">It then uses the default set of Code First conventions to determine table and column names, determine data types, find primary keys, etc.</span></span> <span data-ttu-id="ae55e-167">Später in dieser exemplarischen Vorgehensweise wird erläutert, wie Sie diese Konventionen überschreiben können.</span><span class="sxs-lookup"><span data-stu-id="ae55e-167">Later in this walkthrough we’ll look at how you can override these conventions.</span></span>

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="ae55e-168">5. Umgang mit Modelländerungen</span><span class="sxs-lookup"><span data-stu-id="ae55e-168">5. Dealing with Model Changes</span></span>

<span data-ttu-id="ae55e-169">Nun ist es an der Zeit, einige Änderungen an unserem Modell vorzunehmen, wenn wir diese Änderungen vornehmen, müssen wir auch das Datenbankschema aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="ae55e-169">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span> <span data-ttu-id="ae55e-170">Zu diesem Zweck wird eine Funktion namens "Code First-Migrationen" oder "Migrationen" verwendet.</span><span class="sxs-lookup"><span data-stu-id="ae55e-170">To do this we are going to use a feature called Code First Migrations, or Migrations for short.</span></span>

<span data-ttu-id="ae55e-171">Migrationen ermöglichen es uns, einen geordneten Satz von Schritten zu haben, die beschreiben, wie das Datenbankschema aktualisiert (und herabgestuft) werden kann.</span><span class="sxs-lookup"><span data-stu-id="ae55e-171">Migrations allows us to have an ordered set of steps that describe how to upgrade (and downgrade) our database schema.</span></span> <span data-ttu-id="ae55e-172">Jeder dieser Schritte, die als Migration bezeichnet wird, enthält Code, der die anzuwendenden Änderungen beschreibt.</span><span class="sxs-lookup"><span data-stu-id="ae55e-172">Each of these steps, known as a migration, contains some code that describes the changes to be applied.</span></span> 

<span data-ttu-id="ae55e-173">Der erste Schritt besteht darin, Code First-Migrationen für den bloggingcontext zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="ae55e-173">The first step is to enable Code First Migrations for our BloggingContext.</span></span>

-   <span data-ttu-id="ae55e-174">**Tools-&gt; Bibliothekspaket-Manager-&gt; Paket-Manager-Konsole**</span><span class="sxs-lookup"><span data-stu-id="ae55e-174">**Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
-   <span data-ttu-id="ae55e-175">Führen Sie in der Paket-Manager-Konsole den Befehl **Enable-Migrations** aus.</span><span class="sxs-lookup"><span data-stu-id="ae55e-175">Run the **Enable-Migrations** command in Package Manager Console</span></span>
-   <span data-ttu-id="ae55e-176">Dem Projekt wurde ein neuer Migrations Ordner hinzugefügt, der zwei Elemente enthält:</span><span class="sxs-lookup"><span data-stu-id="ae55e-176">A new Migrations folder has been added to our project that contains two items:</span></span>
    -   <span data-ttu-id="ae55e-177">**Configuration.cs** – diese Datei enthält die Einstellungen, die Migrationen zum Migrieren von bloggingcontext verwenden.</span><span class="sxs-lookup"><span data-stu-id="ae55e-177">**Configuration.cs** – This file contains the settings that Migrations will use for migrating BloggingContext.</span></span> <span data-ttu-id="ae55e-178">Wir müssen nichts für diese exemplarische Vorgehensweise ändern, aber hier können Sie Seed-Daten angeben, Anbieter für andere Datenbanken registrieren, den Namespace ändern, in dem Migrationen generiert werden usw.</span><span class="sxs-lookup"><span data-stu-id="ae55e-178">We don’t need to change anything for this walkthrough, but here is where you can specify seed data, register providers for other databases, changes the namespace that migrations are generated in etc.</span></span>
    -   <span data-ttu-id="ae55e-179">**&lt;Zeitstempel&gt;\_InitialCreate.cs** – Dies ist die erste Migration. Sie stellt die Änderungen dar, die bereits auf die Datenbank angewendet wurden, damit Sie aus einer leeren Datenbank zu einer Datenbank mit den Tabellen Blogs und Posts geführt werden.</span><span class="sxs-lookup"><span data-stu-id="ae55e-179">**&lt;timestamp&gt;\_InitialCreate.cs** – This is your first migration, it represents the changes that have already been applied to the database to take it from being an empty database to one that includes the Blogs and Posts tables.</span></span> <span data-ttu-id="ae55e-180">Obwohl wir Code First diese Tabellen für uns automatisch erstellen lassen, haben wir nun die Migration in eine Migration durchführen.</span><span class="sxs-lookup"><span data-stu-id="ae55e-180">Although we let Code First automatically create these tables for us, now that we have opted in to Migrations they have been converted into a Migration.</span></span> <span data-ttu-id="ae55e-181">Code First wurde auch in unserer lokalen Datenbank aufgezeichnet, dass diese Migration bereits angewendet wurde.</span><span class="sxs-lookup"><span data-stu-id="ae55e-181">Code First has also recorded in our local database that this Migration has already been applied.</span></span> <span data-ttu-id="ae55e-182">Der Zeitstempel des Datei namens wird zu bestellzwecken verwendet.</span><span class="sxs-lookup"><span data-stu-id="ae55e-182">The timestamp on the filename is used for ordering purposes.</span></span>

    <span data-ttu-id="ae55e-183">Nehmen wir nun eine Änderung an unserem Modell vor. Fügen Sie der Blog-Klasse eine URL-Eigenschaft hinzu:</span><span class="sxs-lookup"><span data-stu-id="ae55e-183">Now let’s make a change to our model, add a Url property to the Blog class:</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   <span data-ttu-id="ae55e-184">Führen Sie den Befehl **Add-Migration addurl** in der Paket-Manager-Konsole aus.</span><span class="sxs-lookup"><span data-stu-id="ae55e-184">Run the **Add-Migration AddUrl** command in Package Manager Console.</span></span>
    <span data-ttu-id="ae55e-185">Der Befehl "Add-Migration" prüft seit der letzten Migration auf Änderungen und richtet eine neue Migration mit gefundenen Änderungen ein.</span><span class="sxs-lookup"><span data-stu-id="ae55e-185">The Add-Migration command checks for changes since your last migration and scaffolds a new migration with any changes that are found.</span></span> <span data-ttu-id="ae55e-186">Wir können Migrationen einen Namen übergeben. in diesem Fall rufen wir die Migration "addUrl" auf.</span><span class="sxs-lookup"><span data-stu-id="ae55e-186">We can give migrations a name; in this case we are calling the migration ‘AddUrl’.</span></span>
    <span data-ttu-id="ae55e-187">Der Gerüst Code besagt, dass wir eine URL-Spalte, die Zeichen folgen Daten enthalten kann, zum dbo hinzufügen müssen. Blogs-Tabelle.</span><span class="sxs-lookup"><span data-stu-id="ae55e-187">The scaffolded code is saying that we need to add a Url column, that can hold string data, to the dbo.Blogs table.</span></span> <span data-ttu-id="ae55e-188">Bei Bedarf könnten wir den gerüdierten Code bearbeiten, dies ist in diesem Fall jedoch nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="ae55e-188">If needed, we could edit the scaffolded code but that’s not required in this case.</span></span>

``` csharp
namespace CodeFirstNewDatabaseSample.Migrations
{
    using System;
    using System.Data.Entity.Migrations;

    public partial class AddUrl : DbMigration
    {
        public override void Up()
        {
            AddColumn("dbo.Blogs", "Url", c => c.String());
        }

        public override void Down()
        {
            DropColumn("dbo.Blogs", "Url");
        }
    }
}
```

-   <span data-ttu-id="ae55e-189">Führen Sie den Befehl **Update-Database** in der Paket-Manager-Konsole aus.</span><span class="sxs-lookup"><span data-stu-id="ae55e-189">Run the **Update-Database** command in Package Manager Console.</span></span> <span data-ttu-id="ae55e-190">Mit diesem Befehl werden alle ausstehenden Migrationen auf die Datenbank angewendet.</span><span class="sxs-lookup"><span data-stu-id="ae55e-190">This command will apply any pending migrations to the database.</span></span> <span data-ttu-id="ae55e-191">Unsere InitialCreate-Migration wurde bereits angewendet, sodass bei der Migration nur die neue addurl-Migration angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="ae55e-191">Our InitialCreate migration has already been applied so migrations will just apply our new AddUrl migration.</span></span>
    <span data-ttu-id="ae55e-192">Tipp: Sie können den Schalter " **– verbose** " verwenden, wenn Sie "Update-Database" aufrufen, um die SQL-Anweisung anzuzeigen, die für die Datenbank ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="ae55e-192">Tip: You can use the **–Verbose** switch when calling Update-Database to see the SQL that is being executed against the database.</span></span>

<span data-ttu-id="ae55e-193">Die neue URL-Spalte wird nun der Tabelle Blogs in der-Datenbank hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="ae55e-193">The new Url column is now added to the Blogs table in the database:</span></span>

![Schema mit URL](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a><span data-ttu-id="ae55e-195">6. Daten Anmerkungen</span><span class="sxs-lookup"><span data-stu-id="ae55e-195">6. Data Annotations</span></span>

<span data-ttu-id="ae55e-196">Bisher haben wir EF das Modell nur mit seinen Standard Konventionen ermitteln lassen, aber es gibt Zeiten, in denen unsere Klassen nicht den Konventionen folgen und eine weitere Konfiguration durchführen müssen.</span><span class="sxs-lookup"><span data-stu-id="ae55e-196">So far we’ve just let EF discover the model using its default conventions, but there are going to be times when our classes don’t follow the conventions and we need to be able to perform further configuration.</span></span> <span data-ttu-id="ae55e-197">Hierfür gibt es zwei Optionen: Wir betrachten die Daten Anmerkungen in diesem Abschnitt und dann die fließende API im nächsten Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="ae55e-197">There are two options for this; we’ll look at Data Annotations in this section and then the fluent API in the next section.</span></span>

-   <span data-ttu-id="ae55e-198">Fügen wir unserem Modell eine Benutzerklasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="ae55e-198">Let’s add a User class to our model</span></span>

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="ae55e-199">Wir müssen dem abgeleiteten Kontext auch eine Menge hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="ae55e-199">We also need to add a set to our derived context</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   <span data-ttu-id="ae55e-200">Wenn wir versuchen, eine Migration hinzuzufügen, wird eine Fehlermeldung angezeigt, die besagt, dass für*EntityType ' User ' kein Schlüssel definiert ist. Hiermit wird der Schlüssel für diesen EntityType definiert. "*</span><span class="sxs-lookup"><span data-stu-id="ae55e-200">If we tried to add a migration we’d get an error saying “*EntityType ‘User’ has no key defined. Define the key for this EntityType.”*</span></span> <span data-ttu-id="ae55e-201">EF kann nicht wissen, dass der Benutzername der Primärschlüssel für den Benutzer sein sollte.</span><span class="sxs-lookup"><span data-stu-id="ae55e-201">because EF has no way of knowing that Username should be the primary key for User.</span></span>
-   <span data-ttu-id="ae55e-202">Wir verwenden jetzt Daten Anmerkungen, damit wir am Anfang von Program.cs eine using-Anweisung hinzufügen müssen.</span><span class="sxs-lookup"><span data-stu-id="ae55e-202">We’re going to use Data Annotations now so we need to add a using statement at the top of Program.cs</span></span>

```csharp
using System.ComponentModel.DataAnnotations;
```

-   <span data-ttu-id="ae55e-203">Kommentieren Sie nun die UserName-Eigenschaft, um festzustellen, dass es sich um den Primärschlüssel handelt.</span><span class="sxs-lookup"><span data-stu-id="ae55e-203">Now annotate the Username property to identify that it is the primary key</span></span>

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="ae55e-204">Verwenden Sie den Befehl **Add-Migration adduser** zum Gerüstbau einer Migration, um diese Änderungen auf die Datenbank anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="ae55e-204">Use the **Add-Migration AddUser** command to scaffold a migration to apply these changes to the database</span></span>
-   <span data-ttu-id="ae55e-205">Führen Sie den **Update-Database-** Befehl aus, um die neue Migration auf die Datenbank anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="ae55e-205">Run the **Update-Database** command to apply the new migration to the database</span></span>

<span data-ttu-id="ae55e-206">Die neue Tabelle wird nun der-Datenbank hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="ae55e-206">The new table is now added to the database:</span></span>

![Schema mit Benutzern](~/ef6/media/schemawithusers.png)

<span data-ttu-id="ae55e-208">Die vollständige Liste der von EF unterstützten Anmerkungen lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="ae55e-208">The full list of annotations supported by EF is:</span></span>

-   [<span data-ttu-id="ae55e-209">Keyattribute</span><span class="sxs-lookup"><span data-stu-id="ae55e-209">KeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [<span data-ttu-id="ae55e-210">StringLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="ae55e-210">StringLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [<span data-ttu-id="ae55e-211">MaxLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="ae55e-211">MaxLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [<span data-ttu-id="ae55e-212">"-Attribut"</span><span class="sxs-lookup"><span data-stu-id="ae55e-212">ConcurrencyCheckAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [<span data-ttu-id="ae55e-213">RequiredAttribute</span><span class="sxs-lookup"><span data-stu-id="ae55e-213">RequiredAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [<span data-ttu-id="ae55e-214">Timestampattribute</span><span class="sxs-lookup"><span data-stu-id="ae55e-214">TimestampAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [<span data-ttu-id="ae55e-215">Complextypeattribute</span><span class="sxs-lookup"><span data-stu-id="ae55e-215">ComplexTypeAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [<span data-ttu-id="ae55e-216">ColumnAttribute</span><span class="sxs-lookup"><span data-stu-id="ae55e-216">ColumnAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [<span data-ttu-id="ae55e-217">TableAttribute</span><span class="sxs-lookup"><span data-stu-id="ae55e-217">TableAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [<span data-ttu-id="ae55e-218">Invertspropertyattribute</span><span class="sxs-lookup"><span data-stu-id="ae55e-218">InversePropertyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [<span data-ttu-id="ae55e-219">Fremdschlüssel Attribut</span><span class="sxs-lookup"><span data-stu-id="ae55e-219">ForeignKeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [<span data-ttu-id="ae55e-220">Databasegeneratedattribute</span><span class="sxs-lookup"><span data-stu-id="ae55e-220">DatabaseGeneratedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [<span data-ttu-id="ae55e-221">Notmappeer dattribute</span><span class="sxs-lookup"><span data-stu-id="ae55e-221">NotMappedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a><span data-ttu-id="ae55e-222">7. fließende API</span><span class="sxs-lookup"><span data-stu-id="ae55e-222">7. Fluent API</span></span>

<span data-ttu-id="ae55e-223">Im vorherigen Abschnitt haben wir uns mit der Verwendung von Daten Anmerkungen beschäftigt, um zu ergänzen oder zu überschreiben, was von der Konvention erkannt wurde.</span><span class="sxs-lookup"><span data-stu-id="ae55e-223">In the previous section we looked at using Data Annotations to supplement or override what was detected by convention.</span></span> <span data-ttu-id="ae55e-224">Die andere Möglichkeit, das Modell zu konfigurieren, ist über die Code First fließende API.</span><span class="sxs-lookup"><span data-stu-id="ae55e-224">The other way to configure the model is via the Code First fluent API.</span></span>

<span data-ttu-id="ae55e-225">Die meisten Modell Konfigurationen können mithilfe von einfachen Daten Anmerkungen durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="ae55e-225">Most model configuration can be done using simple data annotations.</span></span> <span data-ttu-id="ae55e-226">Die fließende API ist eine fortgeschrittenere Methode zum Angeben der Modell Konfiguration, die alles abdeckt, was Daten Anmerkungen zusätzlich zu einer erweiterten Konfiguration tun können, die mit Daten Anmerkungen nicht möglich ist.</span><span class="sxs-lookup"><span data-stu-id="ae55e-226">The fluent API is a more advanced way of specifying model configuration that covers everything that data annotations can do in addition to some more advanced configuration not possible with data annotations.</span></span> <span data-ttu-id="ae55e-227">Daten Anmerkungen und die fließende API können gleichzeitig verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="ae55e-227">Data annotations and the fluent API can be used together.</span></span>

<span data-ttu-id="ae55e-228">Für den Zugriff auf die fließende API überschreiben Sie die onmodelcreating-Methode in dbcontext.</span><span class="sxs-lookup"><span data-stu-id="ae55e-228">To access the fluent API you override the OnModelCreating method in DbContext.</span></span> <span data-ttu-id="ae55e-229">Nehmen wir an, dass die Spalte, in der User. Display Name gespeichert ist, umbenannt werden soll, um\_Namen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="ae55e-229">Let’s say we wanted to rename the column that User.DisplayName is stored in to display\_name.</span></span>

-   <span data-ttu-id="ae55e-230">Überschreiben Sie die onmodelcreating-Methode auf bloggingcontext mit dem folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="ae55e-230">Override the OnModelCreating method on BloggingContext with the following code</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<User>()
            .Property(u => u.DisplayName)
            .HasColumnName("display_name");
    }
}
```

-   <span data-ttu-id="ae55e-231">Verwenden Sie den Befehl **Add-Migration changedisplayname** , um ein Gerüst für eine Migration zum Anwenden dieser Änderungen auf die Datenbank zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="ae55e-231">Use the **Add-Migration ChangeDisplayName** command to scaffold a migration to apply these changes to the database.</span></span>
-   <span data-ttu-id="ae55e-232">Führen Sie den **Update-Database-** Befehl aus, um die neue Migration auf die Datenbank anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="ae55e-232">Run the **Update-Database** command to apply the new migration to the database.</span></span>

<span data-ttu-id="ae55e-233">Die DisplayName-Spalte wurde nun in Anzeige\_namens umbenannt:</span><span class="sxs-lookup"><span data-stu-id="ae55e-233">The DisplayName column is now renamed to display\_name:</span></span>

![Schema mit umbenannten anzeigen Amen](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a><span data-ttu-id="ae55e-235">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="ae55e-235">Summary</span></span>

<span data-ttu-id="ae55e-236">In dieser exemplarischen Vorgehensweise haben wir Code First Entwicklung mit einer neuen Datenbank untersucht.</span><span class="sxs-lookup"><span data-stu-id="ae55e-236">In this walkthrough we looked at Code First development using a new database.</span></span> <span data-ttu-id="ae55e-237">Wir haben ein Modell mithilfe von Klassen definiert und dann das Modell verwendet, um eine Datenbank zu erstellen und Daten zu speichern und abzurufen.</span><span class="sxs-lookup"><span data-stu-id="ae55e-237">We defined a model using classes then used that model to create a database and store and retrieve data.</span></span> <span data-ttu-id="ae55e-238">Nachdem die Datenbank erstellt wurde, haben wir Code First-Migrationen verwendet, um das Schema zu ändern, als das Modell entwickelt wurde.</span><span class="sxs-lookup"><span data-stu-id="ae55e-238">Once the database was created we used Code First Migrations to change the schema as our model evolved.</span></span> <span data-ttu-id="ae55e-239">Außerdem wurde erläutert, wie ein Modell mithilfe von Daten Anmerkungen und der fließenden API konfiguriert wird.</span><span class="sxs-lookup"><span data-stu-id="ae55e-239">We also saw how to configure a model using Data Annotations and the Fluent API.</span></span>
