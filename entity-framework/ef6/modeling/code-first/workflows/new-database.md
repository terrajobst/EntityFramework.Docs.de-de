---
title: Code First für eine neue Datenbank – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: a19db575b685cde98509fff4a0efaf26106b26bc
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46284121"
---
# <a name="code-first-to-a-new-database"></a><span data-ttu-id="10a4a-102">Code First für eine neue Datenbank</span><span class="sxs-lookup"><span data-stu-id="10a4a-102">Code First to a New Database</span></span>
<span data-ttu-id="10a4a-103">Dieses video und schrittweise exemplarische Vorgehensweise bieten eine Einführung in die Code First-Entwicklung für eine neue Datenbank.</span><span class="sxs-lookup"><span data-stu-id="10a4a-103">This video and step-by-step walkthrough provide an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="10a4a-104">Dieses Szenario umfasst das Ziel einer Datenbank, die nicht vorhanden ist und Code First erstellt oder eine leere Datenbank, Code First werden neue Tabellen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="10a4a-104">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables to.</span></span> <span data-ttu-id="10a4a-105">Code können Sie Ihr Modell mithilfe von C definieren zuerst\# oder VB.Net-Klassen.</span><span class="sxs-lookup"><span data-stu-id="10a4a-105">Code First allows you to define your model using C\# or VB.Net classes.</span></span> <span data-ttu-id="10a4a-106">Verwenden von Attributen auf Klassen und Eigenschaften oder mithilfe einer fluent-API kann optional zusätzliche Konfigurationsschritte ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="10a4a-106">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="10a4a-107">Video ansehen</span><span class="sxs-lookup"><span data-stu-id="10a4a-107">Watch the video</span></span>
<span data-ttu-id="10a4a-108">Dieses Video bietet eine Einführung in die Code First-Entwicklung für eine neue Datenbank.</span><span class="sxs-lookup"><span data-stu-id="10a4a-108">This video provides an introduction to Code First development targeting a new database.</span></span> <span data-ttu-id="10a4a-109">Dieses Szenario umfasst das Ziel einer Datenbank, die nicht vorhanden ist und Code First erstellt oder eine leere Datenbank, Code First werden neue Tabellen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="10a4a-109">This scenario includes targeting a database that doesn’t exist and Code First will create, or an empty database that Code First will add new tables to.</span></span> <span data-ttu-id="10a4a-110">Code zuerst können Sie Ihr Modell mithilfe von c# oder VB.NET. Klassen definieren.</span><span class="sxs-lookup"><span data-stu-id="10a4a-110">Code First allows you to define your model using C# or VB.Net classes.</span></span> <span data-ttu-id="10a4a-111">Verwenden von Attributen auf Klassen und Eigenschaften oder mithilfe einer fluent-API kann optional zusätzliche Konfigurationsschritte ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="10a4a-111">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span>

<span data-ttu-id="10a4a-112">**Präsentation:** [Rowan Miller](http://romiller.com/)</span><span class="sxs-lookup"><span data-stu-id="10a4a-112">**Presented By**: [Rowan Miller](http://romiller.com/)</span></span>

<span data-ttu-id="10a4a-113">**Video**: [WMV](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span><span class="sxs-lookup"><span data-stu-id="10a4a-113">**Video**: [WMV](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="10a4a-114">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="10a4a-114">Pre-Requisites</span></span>

<span data-ttu-id="10a4a-115">Sie benötigen mindestens Visual Studio 2010 oder Visual Studio 2012 installiert werden, um diese exemplarische Vorgehensweise abgeschlossen haben.</span><span class="sxs-lookup"><span data-stu-id="10a4a-115">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="10a4a-116">Wenn Sie Visual Studio 2010 verwenden, müssen Sie auch haben [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installiert.</span><span class="sxs-lookup"><span data-stu-id="10a4a-116">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed.</span></span>

## <a name="1-create-the-application"></a><span data-ttu-id="10a4a-117">1. Erstellen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="10a4a-117">1. Create the Application</span></span>

<span data-ttu-id="10a4a-118">Einfachheit halber werden wir eine einfache Konsolenanwendung zu erstellen, die Code First verwendet, um die Daten zugreift.</span><span class="sxs-lookup"><span data-stu-id="10a4a-118">To keep things simple we’re going to build a basic console application that uses Code First to perform data access.</span></span>

-   <span data-ttu-id="10a4a-119">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="10a4a-119">Open Visual Studio</span></span>
-   <span data-ttu-id="10a4a-120">**Datei -&gt; neu –&gt; Projekt...**</span><span class="sxs-lookup"><span data-stu-id="10a4a-120">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="10a4a-121">Wählen Sie **Windows** im linken Menü und **-Konsolenanwendung**</span><span class="sxs-lookup"><span data-stu-id="10a4a-121">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="10a4a-122">Geben Sie **CodeFirstNewDatabaseSample** als Name</span><span class="sxs-lookup"><span data-stu-id="10a4a-122">Enter **CodeFirstNewDatabaseSample** as the name</span></span>
-   <span data-ttu-id="10a4a-123">Wählen Sie **OK** aus.</span><span class="sxs-lookup"><span data-stu-id="10a4a-123">Select **OK**</span></span>

## <a name="2-create-the-model"></a><span data-ttu-id="10a4a-124">2. Erstellen des Modells</span><span class="sxs-lookup"><span data-stu-id="10a4a-124">2. Create the Model</span></span>

<span data-ttu-id="10a4a-125">Definieren Sie ein sehr einfaches Modell, die mithilfe von Klassen.</span><span class="sxs-lookup"><span data-stu-id="10a4a-125">Let’s define a very simple model using classes.</span></span> <span data-ttu-id="10a4a-126">Wir definieren einfach sie in der Datei "Program.cs" jedoch in einer realen Anwendung, die Sie Ihre Klassen sich in separaten Dateien und ggf. ein separates Projekt Teilen würde.</span><span class="sxs-lookup"><span data-stu-id="10a4a-126">We’re just defining them in the Program.cs file but in a real world application you would split your classes out into separate files and potentially a separate project.</span></span>

<span data-ttu-id="10a4a-127">Fügen Sie unterhalb der Definition der Programmklasse in "Program.cs" die folgenden zwei Klassen ein.</span><span class="sxs-lookup"><span data-stu-id="10a4a-127">Below the Program class definition in Program.cs add the following two classes.</span></span>

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

<span data-ttu-id="10a4a-128">Sie werden feststellen, dass wir die zwei Navigationseigenschaften (Blog.Posts und Post.Blog) virtuellen vornehmen.</span><span class="sxs-lookup"><span data-stu-id="10a4a-128">You’ll notice that we’re making the two navigation properties (Blog.Posts and Post.Blog) virtual.</span></span> <span data-ttu-id="10a4a-129">Dies ermöglicht die Lazy Loading-Funktion von Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="10a4a-129">This enables the Lazy Loading feature of Entity Framework.</span></span> <span data-ttu-id="10a4a-130">Lazy Loading bedeutet, dass der Inhalt dieser Eigenschaften automatisch aus der Datenbank geladen werden, wenn Sie versuchen, darauf zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="10a4a-130">Lazy Loading means that the contents of these properties will be automatically loaded from the database when you try to access them.</span></span>

## <a name="3-create-a-context"></a><span data-ttu-id="10a4a-131">3. Erstellen Sie einen Kontext</span><span class="sxs-lookup"><span data-stu-id="10a4a-131">3. Create a Context</span></span>

<span data-ttu-id="10a4a-132">Jetzt ist es Zeit, einen abgeleiteten Kontext, steht für eine Sitzung mit der Datenbank, die es uns ermöglicht, Abfragen und Speichern von Daten zu definieren.</span><span class="sxs-lookup"><span data-stu-id="10a4a-132">Now it’s time to define a derived context, which represents a session with the database, allowing us to query and save data.</span></span> <span data-ttu-id="10a4a-133">Wir definieren einen Kontext an, die von System.Data.Entity.DbContext abgeleitet und stellt einen typisierten "DbSet"&lt;TEntity&gt; für jede Klasse in unserem Modell.</span><span class="sxs-lookup"><span data-stu-id="10a4a-133">We define a context that derives from System.Data.Entity.DbContext and exposes a typed DbSet&lt;TEntity&gt; for each class in our model.</span></span>

<span data-ttu-id="10a4a-134">Beginnen wir nun auf die Typen aus dem Entity Framework verwenden, daher wir das EntityFramework NuGet-Paket hinzufügen müssen.</span><span class="sxs-lookup"><span data-stu-id="10a4a-134">We’re now starting to use types from the Entity Framework so we need to add the EntityFramework NuGet package.</span></span>

-   <span data-ttu-id="10a4a-135">**Projekt:&gt; NuGet-Pakete verwalten...**</span><span class="sxs-lookup"><span data-stu-id="10a4a-135">**Project –&gt; Manage NuGet Packages…**</span></span>
    <span data-ttu-id="10a4a-136">Hinweis: Wenn Sie nicht über verfügen den **NuGet-Pakete verwalten...**</span><span class="sxs-lookup"><span data-stu-id="10a4a-136">Note: If you don’t have the **Manage NuGet Packages…**</span></span> <span data-ttu-id="10a4a-137">Option Sie installieren die [neueste Version von NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span><span class="sxs-lookup"><span data-stu-id="10a4a-137">option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span></span>
-   <span data-ttu-id="10a4a-138">Wählen Sie die **Online** Registerkarte</span><span class="sxs-lookup"><span data-stu-id="10a4a-138">Select the **Online** tab</span></span>
-   <span data-ttu-id="10a4a-139">Wählen Sie die **EntityFramework** Paket</span><span class="sxs-lookup"><span data-stu-id="10a4a-139">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="10a4a-140">Klicken Sie auf **installieren**</span><span class="sxs-lookup"><span data-stu-id="10a4a-140">Click **Install**</span></span>

<span data-ttu-id="10a4a-141">Hinzufügen einer using-Anweisung für System.Data.Entity am Anfang der Datei "Program.cs".</span><span class="sxs-lookup"><span data-stu-id="10a4a-141">Add a using statement for System.Data.Entity at the top of Program.cs.</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="10a4a-142">Fügen Sie unterhalb der Post-Klasse in "Program.cs" die folgenden abgeleiteten Kontext aus.</span><span class="sxs-lookup"><span data-stu-id="10a4a-142">Below the Post class in Program.cs add the following derived context.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

<span data-ttu-id="10a4a-143">Hier ist eine vollständige Liste der was "Program.cs" jetzt enthalten soll.</span><span class="sxs-lookup"><span data-stu-id="10a4a-143">Here is a complete listing of what Program.cs should now contain.</span></span>

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

<span data-ttu-id="10a4a-144">Das ist der Code zu speichern und Abrufen von Daten benötigten.</span><span class="sxs-lookup"><span data-stu-id="10a4a-144">That is all the code we need to start storing and retrieving data.</span></span> <span data-ttu-id="10a4a-145">Natürlich gibt es einiges hinter den Kulissen passiert ist, und wir werden sehen Sie sich, dass in Kürze jedoch lassen Sie uns in Aktion sehen.</span><span class="sxs-lookup"><span data-stu-id="10a4a-145">Obviously there is quite a bit going on behind the scenes and we’ll take a look at that in a moment but first let’s see it in action.</span></span>

## <a name="4-reading--writing-data"></a><span data-ttu-id="10a4a-146">4. Lesen und Schreiben von Daten</span><span class="sxs-lookup"><span data-stu-id="10a4a-146">4. Reading & Writing Data</span></span>

<span data-ttu-id="10a4a-147">Implementieren Sie die Main-Methode in "Program.cs" ein, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="10a4a-147">Implement the Main method in Program.cs as shown below.</span></span> <span data-ttu-id="10a4a-148">Dieser Code erstellt eine neue Instanz unseres Kontexts und anschließend verwendet, um einen neuen Blog einfügen.</span><span class="sxs-lookup"><span data-stu-id="10a4a-148">This code creates a new instance of our context and then uses it to insert a new Blog.</span></span> <span data-ttu-id="10a4a-149">Anschließend wird eine LINQ-Abfrage zum Abrufen aller Blogs aus der Datenbank nach Titel alphabetisch angeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="10a4a-149">Then it uses a LINQ query to retrieve all Blogs from the database ordered alphabetically by Title.</span></span>

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

<span data-ttu-id="10a4a-150">Sie können jetzt die Anwendung auszuführen, und probieren Sie es aus.</span><span class="sxs-lookup"><span data-stu-id="10a4a-150">You can now run the application and test it out.</span></span>

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
### <a name="wheres-my-data"></a><span data-ttu-id="10a4a-151">Wo sind meine Daten?</span><span class="sxs-lookup"><span data-stu-id="10a4a-151">Where’s My Data?</span></span>

<span data-ttu-id="10a4a-152">Gemäß der Konvention erstellt "DbContext" eine Datenbank für Sie.</span><span class="sxs-lookup"><span data-stu-id="10a4a-152">By convention DbContext has created a database for you.</span></span>

-   <span data-ttu-id="10a4a-153">Wenn eine lokale Instanz von SQL Express verfügbar ist (standardmäßig installiert, mit Visual Studio 2010) klicken Sie dann hat Code First die Datenbank für diese Instanz</span><span class="sxs-lookup"><span data-stu-id="10a4a-153">If a local SQL Express instance is available (installed by default with Visual Studio 2010) then Code First has created the database on that instance</span></span>
-   <span data-ttu-id="10a4a-154">Wenn Sie SQL Express ist nicht verfügbar, und Code First wird zu verwenden, [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (standardmäßig installiert, mit Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="10a4a-154">If SQL Express isn’t available then Code First will try and use [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (installed by default with Visual Studio 2012)</span></span>
-   <span data-ttu-id="10a4a-155">Die Datenbank ist nach der vollqualifizierte Name des abgeleiteten Kontexts, in unserem Fall, die mit dem Namen **CodeFirstNewDatabaseSample.BloggingContext**</span><span class="sxs-lookup"><span data-stu-id="10a4a-155">The database is named after the fully qualified name of the derived context, in our case that is **CodeFirstNewDatabaseSample.BloggingContext**</span></span>

<span data-ttu-id="10a4a-156">Hierbei handelt es sich um nur den Standardkonventionen und es gibt verschiedene Möglichkeiten zum Ändern der Datenbank, die Code First verwendet, Weitere Informationen finden Sie in der **wie "DbContext" für das Modell und die Datenbankverbindung ermittelt** Thema.</span><span class="sxs-lookup"><span data-stu-id="10a4a-156">These are just the default conventions and there are various ways to change the database that Code First uses, more information is available in the **How DbContext Discovers the Model and Database Connection** topic.</span></span>
<span data-ttu-id="10a4a-157">Sie können mit dieser Datenbank mithilfe von Server-Explorer in Visual Studio verbinden.</span><span class="sxs-lookup"><span data-stu-id="10a4a-157">You can connect to this database using Server Explorer in Visual Studio</span></span>

-   <span data-ttu-id="10a4a-158">**Ansicht – Profiler -&gt; Server-Explorer**</span><span class="sxs-lookup"><span data-stu-id="10a4a-158">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="10a4a-159">Klicken Sie mit der rechten Maustaste auf **Datenverbindungen** , und wählen Sie **Verbindung hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="10a4a-159">Right click on **Data Connections** and select **Add Connection…**</span></span>
-   <span data-ttu-id="10a4a-160">Wenn Sie vor dem müssen Sie Microsoft SQL Server als Datenquelle wählen im Server-Explorer mit einer Datenbank verbunden haben</span><span class="sxs-lookup"><span data-stu-id="10a4a-160">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Auswählen einer Datenquelle](~/ef6/media/selectdatasource.png)

-   <span data-ttu-id="10a4a-162">Eine Verbindung mit LocalDB oder SQL Express, je nachdem, welches Sie installiert haben</span><span class="sxs-lookup"><span data-stu-id="10a4a-162">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>

<span data-ttu-id="10a4a-163">Wir können nun das Schema zu überprüfen, das Code First erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="10a4a-163">We can now inspect the schema that Code First created.</span></span>

![Schema anfängliche](~/ef6/media/schemainitial.png)

<span data-ttu-id="10a4a-165">"DbContext" hat, welche Klassen sollen in das Modell anhand der DbSet-Eigenschaften, die wir definiert haben.</span><span class="sxs-lookup"><span data-stu-id="10a4a-165">DbContext worked out what classes to include in the model by looking at the DbSet properties that we defined.</span></span> <span data-ttu-id="10a4a-166">Dann wird den Standardsatz von Code First-Konventionen verwendet, um festzustellen, Tabellen- und Spaltennamen, Datentypen zu bestimmen, Primärschlüssel, usw. zu finden.</span><span class="sxs-lookup"><span data-stu-id="10a4a-166">It then uses the default set of Code First conventions to determine table and column names, determine data types, find primary keys, etc.</span></span> <span data-ttu-id="10a4a-167">Weiter unten in dieser exemplarischen Vorgehensweise betrachten wir, wie Sie diese Konventionen überschreiben können.</span><span class="sxs-lookup"><span data-stu-id="10a4a-167">Later in this walkthrough we’ll look at how you can override these conventions.</span></span>

## <a name="5-dealing-with-model-changes"></a><span data-ttu-id="10a4a-168">5. Umgang mit Änderungen des Datenmodells</span><span class="sxs-lookup"><span data-stu-id="10a4a-168">5. Dealing with Model Changes</span></span>

<span data-ttu-id="10a4a-169">Jetzt ist es Zeit, einige Änderungen auf unserem Modell vornehmen, wenn wir diese Änderungen vornehmen, die wir auch das Datenbankschema zu aktualisieren müssen.</span><span class="sxs-lookup"><span data-stu-id="10a4a-169">Now it’s time to make some changes to our model, when we make these changes we also need to update the database schema.</span></span> <span data-ttu-id="10a4a-170">Zu diesem Zweck werden wir eine Funktion wird aufgerufen, kurz Code First-Migrationen oder Migrationen verwenden.</span><span class="sxs-lookup"><span data-stu-id="10a4a-170">To do this we are going to use a feature called Code First Migrations, or Migrations for short.</span></span>

<span data-ttu-id="10a4a-171">Migrationen können wir eine geordnete Menge von Schritten, die beschreiben, wie Sie unsere Datenbankschema aktualisieren (und ein Downgrade durchführen).</span><span class="sxs-lookup"><span data-stu-id="10a4a-171">Migrations allows us to have an ordered set of steps that describe how to upgrade (and downgrade) our database schema.</span></span> <span data-ttu-id="10a4a-172">Jeder dieser Schritte, die eine Migration genannt enthält Code, der beschreibt, die Änderungen angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="10a4a-172">Each of these steps, known as a migration, contains some code that describes the changes to be applied.</span></span> 

<span data-ttu-id="10a4a-173">Der erste Schritt ist für unsere BloggingContext Code First-Migrationen zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="10a4a-173">The first step is to enable Code First Migrations for our BloggingContext.</span></span>

-   <span data-ttu-id="10a4a-174">**Extras -&gt; Bibliothekspaket-Manager -&gt; -Paket-Manager-Konsole**</span><span class="sxs-lookup"><span data-stu-id="10a4a-174">**Tools -&gt; Library Package Manager -&gt; Package Manager Console**</span></span>
-   <span data-ttu-id="10a4a-175">Führen Sie in der Paket-Manager-Konsole den Befehl **Enable-Migrations** aus.</span><span class="sxs-lookup"><span data-stu-id="10a4a-175">Run the **Enable-Migrations** command in Package Manager Console</span></span>
-   <span data-ttu-id="10a4a-176">Ein neuer Ordner "Migrations" wurde zu Ihrem Projekt hinzugefügt, die zwei Elemente enthält:</span><span class="sxs-lookup"><span data-stu-id="10a4a-176">A new Migrations folder has been added to our project that contains two items:</span></span>
    -   <span data-ttu-id="10a4a-177">**Configuration.cs** – diese Datei enthält die Einstellungen, die für die Migration Migrationen verwenden BloggingContext.</span><span class="sxs-lookup"><span data-stu-id="10a4a-177">**Configuration.cs** – This file contains the settings that Migrations will use for migrating BloggingContext.</span></span> <span data-ttu-id="10a4a-178">Wir müssen nicht alles in dieser exemplarischen Vorgehensweise ändern, aber dies ist in dem Sie die Seed-Daten, Register-Anbieter für andere Datenbanken geändert wird, den Namespace angeben können, dass Migrationen etc. generiert werden.</span><span class="sxs-lookup"><span data-stu-id="10a4a-178">We don’t need to change anything for this walkthrough, but here is where you can specify seed data, register providers for other databases, changes the namespace that migrations are generated in etc.</span></span>
    -   <span data-ttu-id="10a4a-179">**&lt;Zeitstempel&gt;\_InitialCreate.cs** – Dies ist Ihre erste Migration, es stellt die Änderungen, die bereits in der Datenbank wird eine leere Datenbank zu einem mit der Blogs und Beiträgen machen angewendet wurden .</span><span class="sxs-lookup"><span data-stu-id="10a4a-179">**&lt;timestamp&gt;\_InitialCreate.cs** – This is your first migration, it represents the changes that have already been applied to the database to take it from being an empty database to one that includes the Blogs and Posts tables.</span></span> <span data-ttu-id="10a4a-180">Obwohl wir können Code First, erstellen Sie diese Tabellen automatisch für uns jetzt an, dass es sich um Migrationen entschieden haben, die sie in einer Migration konvertiert wurden.</span><span class="sxs-lookup"><span data-stu-id="10a4a-180">Although we let Code First automatically create these tables for us, now that we have opted in to Migrations they have been converted into a Migration.</span></span> <span data-ttu-id="10a4a-181">Code hat, dass diese Migration bereits angewendet wurde zuerst auch in der lokalen Datenbank aufgezeichnet.</span><span class="sxs-lookup"><span data-stu-id="10a4a-181">Code First has also recorded in our local database that this Migration has already been applied.</span></span> <span data-ttu-id="10a4a-182">Der Zeitstempel auf den Dateinamen wird verwendet, für die Sortierung zu.</span><span class="sxs-lookup"><span data-stu-id="10a4a-182">The timestamp on the filename is used for ordering purposes.</span></span>

    <span data-ttu-id="10a4a-183">Jetzt sehen wir eine Änderung an unser Modell vornehmen, fügen Sie eine Url-Eigenschaft der Blog-Klasse:</span><span class="sxs-lookup"><span data-stu-id="10a4a-183">Now let’s make a change to our model, add a Url property to the Blog class:</span></span>

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   <span data-ttu-id="10a4a-184">Führen Sie die **Add-Migration AddUrl** im Paket-Manager-Konsole den Befehl.</span><span class="sxs-lookup"><span data-stu-id="10a4a-184">Run the **Add-Migration AddUrl** command in Package Manager Console.</span></span>
    <span data-ttu-id="10a4a-185">Der Befehl Add-Migration Änderungen seit der letzten Migration überprüft und erstellt das Gerüst für einer neuen Migrations mit den Änderungen, die gefunden werden.</span><span class="sxs-lookup"><span data-stu-id="10a4a-185">The Add-Migration command checks for changes since your last migration and scaffolds a new migration with any changes that are found.</span></span> <span data-ttu-id="10a4a-186">Wir können die Migrationen einen Namen geben, In diesem Fall werden wir die Migration "AddUrl" aufrufen.</span><span class="sxs-lookup"><span data-stu-id="10a4a-186">We can give migrations a name; in this case we are calling the migration ‘AddUrl’.</span></span>
    <span data-ttu-id="10a4a-187">Der eingerüstete Code wird darüber informiert, dass wir eine Url-Spalte, die Zeichenfolgendaten enthalten kann, das "Dbo" hinzufügen. Blogs-Tabelle.</span><span class="sxs-lookup"><span data-stu-id="10a4a-187">The scaffolded code is saying that we need to add a Url column, that can hold string data, to the dbo.Blogs table.</span></span> <span data-ttu-id="10a4a-188">Bei Bedarf bearbeiten wir konnten den Gerüstcode, aber, die in diesem Fall nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="10a4a-188">If needed, we could edit the scaffolded code but that’s not required in this case.</span></span>

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

-   <span data-ttu-id="10a4a-189">Führen Sie die **Update-Database** im Paket-Manager-Konsole den Befehl.</span><span class="sxs-lookup"><span data-stu-id="10a4a-189">Run the **Update-Database** command in Package Manager Console.</span></span> <span data-ttu-id="10a4a-190">Mit diesem Befehl wird die ausstehenden Migrationen auf die Datenbank anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="10a4a-190">This command will apply any pending migrations to the database.</span></span> <span data-ttu-id="10a4a-191">Unsere InitialCreate Migration wurde bereits angewendet wurde, wendet nur Migrationen unsere neue AddUrl Migration an.</span><span class="sxs-lookup"><span data-stu-id="10a4a-191">Our InitialCreate migration has already been applied so migrations will just apply our new AddUrl migration.</span></span>
    <span data-ttu-id="10a4a-192">Tipp: Sie können die **– Verbose** wechseln, beim Aufrufen des Update-Database, um die SQL-Anweisung anzuzeigen, die für die Datenbank ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="10a4a-192">Tip: You can use the **–Verbose** switch when calling Update-Database to see the SQL that is being executed against the database.</span></span>

<span data-ttu-id="10a4a-193">Die neue Url-Spalte wird nun die Blogs-Tabelle in der Datenbank hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="10a4a-193">The new Url column is now added to the Blogs table in the database:</span></span>

![Schema mit Url](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a><span data-ttu-id="10a4a-195">6. Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="10a4a-195">6. Data Annotations</span></span>

<span data-ttu-id="10a4a-196">Bisher haben wir nur EF, die das Modell unter Verwendung der Standardkonventionen ermitteln können, aber es wird zunächst wie oft werden, wenn die Klassen nicht die Konventionen strikt befolgen, und wir müssen weitere Konfigurationsschritte ausführen können.</span><span class="sxs-lookup"><span data-stu-id="10a4a-196">So far we’ve just let EF discover the model using its default conventions, but there are going to be times when our classes don’t follow the conventions and we need to be able to perform further configuration.</span></span> <span data-ttu-id="10a4a-197">Es gibt zwei Optionen für diesen ein. Wir werden Datenanmerkungen in diesem Abschnitt, und klicken Sie dann die fluent-API im nächsten Abschnitt betrachten.</span><span class="sxs-lookup"><span data-stu-id="10a4a-197">There are two options for this; we’ll look at Data Annotations in this section and then the fluent API in the next section.</span></span>

-   <span data-ttu-id="10a4a-198">Fügen Sie eine Klasse zum Modell</span><span class="sxs-lookup"><span data-stu-id="10a4a-198">Let’s add a User class to our model</span></span>

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="10a4a-199">Wir müssen auch einen Satz an unsere abgeleiteten Kontext hinzufügen</span><span class="sxs-lookup"><span data-stu-id="10a4a-199">We also need to add a set to our derived context</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   <span data-ttu-id="10a4a-200">Wenn wir versuchen, eine Migration hinzuzufügen erhalten wir einen Fehler mit dem Text "*EntityType 'User' ist kein Schlüssel definiert. Definieren Sie den Schlüssel für diesen EntityType."*</span><span class="sxs-lookup"><span data-stu-id="10a4a-200">If we tried to add a migration we’d get an error saying “*EntityType ‘User’ has no key defined. Define the key for this EntityType.”*</span></span> <span data-ttu-id="10a4a-201">Da EF keine Möglichkeit hat, zu wissen, dass die Benutzernamen, den primären Schlüssel für den Benutzer werden soll.</span><span class="sxs-lookup"><span data-stu-id="10a4a-201">because EF has no way of knowing that Username should be the primary key for User.</span></span>
-   <span data-ttu-id="10a4a-202">Wir werden jetzt von Datenanmerkungen zu verwenden, daher wir zum Hinzufügen einer using müssen-Anweisung am oberen Rand der Datei "Program.cs"</span><span class="sxs-lookup"><span data-stu-id="10a4a-202">We’re going to use Data Annotations now so we need to add a using statement at the top of Program.cs</span></span>

```
using System.ComponentModel.DataAnnotations;
```

-   <span data-ttu-id="10a4a-203">Kommentieren Sie die Username-Eigenschaft, um anzugeben, dass sie den primären Schlüssel ist jetzt</span><span class="sxs-lookup"><span data-stu-id="10a4a-203">Now annotate the Username property to identify that it is the primary key</span></span>

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   <span data-ttu-id="10a4a-204">Verwenden der **Add-Migration AddUser** Befehl zum Erstellen des Gerüsts für einer Migrations zum Anwenden dieser Änderungen in der Datenbank</span><span class="sxs-lookup"><span data-stu-id="10a4a-204">Use the **Add-Migration AddUser** command to scaffold a migration to apply these changes to the database</span></span>
-   <span data-ttu-id="10a4a-205">Führen Sie die **Update-Database** Befehl aus, um die neue Migration auf die Datenbank anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="10a4a-205">Run the **Update-Database** command to apply the new migration to the database</span></span>

<span data-ttu-id="10a4a-206">Die neue Tabelle wird jetzt in der Datenbank hinzugefügt werden:</span><span class="sxs-lookup"><span data-stu-id="10a4a-206">The new table is now added to the database:</span></span>

![Schema für Benutzer](~/ef6/media/schemawithusers.png)

<span data-ttu-id="10a4a-208">Die vollständige Liste der Anmerkungen, die von EF unterstützt wird:</span><span class="sxs-lookup"><span data-stu-id="10a4a-208">The full list of annotations supported by EF is:</span></span>

-   [<span data-ttu-id="10a4a-209">KeyAttribute</span><span class="sxs-lookup"><span data-stu-id="10a4a-209">KeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [<span data-ttu-id="10a4a-210">StringLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="10a4a-210">StringLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [<span data-ttu-id="10a4a-211">MaxLengthAttribute</span><span class="sxs-lookup"><span data-stu-id="10a4a-211">MaxLengthAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [<span data-ttu-id="10a4a-212">ConcurrencyCheckAttribute</span><span class="sxs-lookup"><span data-stu-id="10a4a-212">ConcurrencyCheckAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [<span data-ttu-id="10a4a-213">RequiredAttribute</span><span class="sxs-lookup"><span data-stu-id="10a4a-213">RequiredAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [<span data-ttu-id="10a4a-214">TimestampAttribute</span><span class="sxs-lookup"><span data-stu-id="10a4a-214">TimestampAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [<span data-ttu-id="10a4a-215">ComplexTypeAttribute</span><span class="sxs-lookup"><span data-stu-id="10a4a-215">ComplexTypeAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [<span data-ttu-id="10a4a-216">ColumnAttribute</span><span class="sxs-lookup"><span data-stu-id="10a4a-216">ColumnAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [<span data-ttu-id="10a4a-217">TableAttribute</span><span class="sxs-lookup"><span data-stu-id="10a4a-217">TableAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [<span data-ttu-id="10a4a-218">InversePropertyAttribute</span><span class="sxs-lookup"><span data-stu-id="10a4a-218">InversePropertyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [<span data-ttu-id="10a4a-219">ForeignKeyAttribute</span><span class="sxs-lookup"><span data-stu-id="10a4a-219">ForeignKeyAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [<span data-ttu-id="10a4a-220">DatabaseGeneratedAttribute</span><span class="sxs-lookup"><span data-stu-id="10a4a-220">DatabaseGeneratedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [<span data-ttu-id="10a4a-221">NotMappedAttribute</span><span class="sxs-lookup"><span data-stu-id="10a4a-221">NotMappedAttribute</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a><span data-ttu-id="10a4a-222">7. Fluent-API</span><span class="sxs-lookup"><span data-stu-id="10a4a-222">7. Fluent API</span></span>

<span data-ttu-id="10a4a-223">Im vorherigen Abschnitt haben Sie mithilfe von Datenanmerkungen zu ergänzen oder zu überschreiben, was gemäß der Konvention erkannt wurde.</span><span class="sxs-lookup"><span data-stu-id="10a4a-223">In the previous section we looked at using Data Annotations to supplement or override what was detected by convention.</span></span> <span data-ttu-id="10a4a-224">Eine andere Möglichkeit, die das Modell konfigurieren, erfolgt über die Code First fluent-API.</span><span class="sxs-lookup"><span data-stu-id="10a4a-224">The other way to configure the model is via the Code First fluent API.</span></span>

<span data-ttu-id="10a4a-225">Konfiguration für die meisten erfolgen mithilfe von einfachen Daten-Anmerkungen.</span><span class="sxs-lookup"><span data-stu-id="10a4a-225">Most model configuration can be done using simple data annotations.</span></span> <span data-ttu-id="10a4a-226">Die fluent-API ist eine erweiterte Methode der Angabe der Konfiguration, die alles abdeckt, die von datenanmerkungen auf einige Informationen zur erweiterten Konfiguration mit datenanmerkungen nicht möglich. darüber hinaus durchführen können.</span><span class="sxs-lookup"><span data-stu-id="10a4a-226">The fluent API is a more advanced way of specifying model configuration that covers everything that data annotations can do in addition to some more advanced configuration not possible with data annotations.</span></span> <span data-ttu-id="10a4a-227">Datenanmerkungen und die fluent-API können zusammen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="10a4a-227">Data annotations and the fluent API can be used together.</span></span>

<span data-ttu-id="10a4a-228">Für den Zugriff auf die fluent-API überschreiben Sie die OnModelCreating-Methode in der "DbContext".</span><span class="sxs-lookup"><span data-stu-id="10a4a-228">To access the fluent API you override the OnModelCreating method in DbContext.</span></span> <span data-ttu-id="10a4a-229">Angenommen, wir wollten die Spalte umbenennen, die User.DisplayName anzuzeigende in gespeicherten\_Name.</span><span class="sxs-lookup"><span data-stu-id="10a4a-229">Let’s say we wanted to rename the column that User.DisplayName is stored in to display\_name.</span></span>

-   <span data-ttu-id="10a4a-230">Überschreiben der OnModelCreating-Methode auf BloggingContext durch den folgenden code</span><span class="sxs-lookup"><span data-stu-id="10a4a-230">Override the OnModelCreating method on BloggingContext with the following code</span></span>

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

-   <span data-ttu-id="10a4a-231">Verwenden der **Add-Migration ChangeDisplayName** Befehl zum Erstellen des Gerüsts für einer Migrations zum Anwenden dieser Änderungen in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="10a4a-231">Use the **Add-Migration ChangeDisplayName** command to scaffold a migration to apply these changes to the database.</span></span>
-   <span data-ttu-id="10a4a-232">Führen Sie die **Update-Database** Befehl aus, um die neue Migration auf die Datenbank anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="10a4a-232">Run the **Update-Database** command to apply the new migration to the database.</span></span>

<span data-ttu-id="10a4a-233">Zum Anzeigen die Spalte "DisplayName" umbenannt\_Name:</span><span class="sxs-lookup"><span data-stu-id="10a4a-233">The DisplayName column is now renamed to display\_name:</span></span>

![Schema mit dem Anzeigenamen, die umbenannt](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a><span data-ttu-id="10a4a-235">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="10a4a-235">Summary</span></span>

<span data-ttu-id="10a4a-236">In dieser exemplarischen Vorgehensweise haben Sie eine neue Datenbank mit Code First-Entwicklung.</span><span class="sxs-lookup"><span data-stu-id="10a4a-236">In this walkthrough we looked at Code First development using a new database.</span></span> <span data-ttu-id="10a4a-237">Wir definierten ein Modell mithilfe von Klassen und verwendet dieses Modell zum Erstellen einer Datenbank und speichern und Abrufen von Daten.</span><span class="sxs-lookup"><span data-stu-id="10a4a-237">We defined a model using classes then used that model to create a database and store and retrieve data.</span></span> <span data-ttu-id="10a4a-238">Nach der Erstellung der Datenbank verwendet haben wir Code First-Migrationen zum Ändern des Schemas als unser Modell entwickelt.</span><span class="sxs-lookup"><span data-stu-id="10a4a-238">Once the database was created we used Code First Migrations to change the schema as our model evolved.</span></span> <span data-ttu-id="10a4a-239">Wir haben auch ein Modell mit Datenanmerkungen und die Fluent-API zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="10a4a-239">We also saw how to configure a model using Data Annotations and the Fluent API.</span></span>
