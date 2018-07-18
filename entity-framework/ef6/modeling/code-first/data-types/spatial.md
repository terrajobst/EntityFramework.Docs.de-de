---
title: Räumliche - Code First – EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
caps.latest.revision: 3
ms.openlocfilehash: 03557820bc689d8e7389a1f84121b7eeaa904df1
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121357"
---
# <a name="spatial---code-first"></a><span data-ttu-id="d8ccc-102">Räumliche - Code First</span><span class="sxs-lookup"><span data-stu-id="d8ccc-102">Spatial - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="d8ccc-103">**EF5 oder höher, nur** -APIs, die Funktionen erläutert, die auf dieser Seite usw. in Entity Framework 5 eingeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="d8ccc-104">Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="d8ccc-105">Die Video- und schrittweise exemplarische Vorgehensweise veranschaulicht das Zuordnen von räumlichen Typen mit Entity Framework Code First.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-105">The video and step-by-step walkthrough shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="d8ccc-106">Darüber hinaus veranschaulicht eine LINQ-Abfrage verwenden, um einen Abstand zwischen zwei Orten zu finden.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="d8ccc-107">In dieser exemplarischen Vorgehensweise wird Code First verwenden, um eine neue Datenbank zu erstellen, aber Sie können auch [Code First mit einer vorhandenen Datenbank](~/ef6/modeling/code-first/workflows/existing-database.md).</span><span class="sxs-lookup"><span data-stu-id="d8ccc-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="d8ccc-108">Unterstützung von räumlichen Typen wurde in Entity Framework 5 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="d8ccc-109">Beachten Sie, um die neuen Features wie räumlichen Typen, Enumerationen und Tabellenwertfunktionen zu verwenden, Sie .NET Framework 4.5 als Ziel haben müssen.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="d8ccc-110">Visual Studio 2012 ist standardmäßig die Zielversion .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="d8ccc-111">Verwendung der Typen von räumlichen Daten müssen Sie auch einen Entity Framework-Anbieter verwenden, der Unterstützung von räumlichen Daten verfügt.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="d8ccc-112">Finden Sie unter [anbieterunterstützung für räumliche Typen](~/ef6/fundamentals/providers/spatial-support.md) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="d8ccc-113">Es gibt zwei Arten von main räumliche Daten: Geography und Geometry.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="d8ccc-114">Der Geography-Datentyp speichert ellipsenförmige Daten (z. B. GPS-Breiten- und Längengrad koordiniert).</span><span class="sxs-lookup"><span data-stu-id="d8ccc-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="d8ccc-115">Der Geometry-Datentyp stellt euklidischen (flachen) Koordinatensystem dar.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="d8ccc-116">Video ansehen</span><span class="sxs-lookup"><span data-stu-id="d8ccc-116">Watch the video</span></span>
<span data-ttu-id="d8ccc-117">Dieses Video zeigt, wie räumliche Typen mit Entity Framework Code First zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-117">This video shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="d8ccc-118">Darüber hinaus veranschaulicht eine LINQ-Abfrage verwenden, um einen Abstand zwischen zwei Orten zu finden.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="d8ccc-119">**Präsentiert von**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="d8ccc-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="d8ccc-120">**Video**: [WMV](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="d8ccc-120">**Video**: [WMV](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="d8ccc-121">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="d8ccc-121">Pre-Requisites</span></span>

<span data-ttu-id="d8ccc-122">Sie müssen Visual Studio 2012, Ultimate, Premium, Professional oder Web Express Edition installiert, um diese exemplarische Vorgehensweise abgeschlossen haben.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="d8ccc-123">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="d8ccc-123">Set up the Project</span></span>

1.  <span data-ttu-id="d8ccc-124">Visual Studio 2012 öffnen</span><span class="sxs-lookup"><span data-stu-id="d8ccc-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="d8ccc-125">Auf der **Datei** Startmenü **neu**, und klicken Sie dann auf **Projekt**</span><span class="sxs-lookup"><span data-stu-id="d8ccc-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="d8ccc-126">Klicken Sie im linken Bereich auf **Visual C\#**, und wählen Sie dann die **Konsole** Vorlage</span><span class="sxs-lookup"><span data-stu-id="d8ccc-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="d8ccc-127">Geben Sie **SpatialCodeFirst** als Namen für das Projekt und auf **OK**</span><span class="sxs-lookup"><span data-stu-id="d8ccc-127">Enter **SpatialCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="d8ccc-128">Definieren Sie ein neues Modell mit Code First</span><span class="sxs-lookup"><span data-stu-id="d8ccc-128">Define a New Model using Code First</span></span>

<span data-ttu-id="d8ccc-129">Bei Verwendung von Code First-Entwicklung beginnen Sie in der Regel durch das Schreiben von .NET Framework-Klassen, die das konzeptionelle (Domäne)-Modell zu definieren.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-129">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="d8ccc-130">Der folgende Code definiert die University-Klasse.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-130">The code below defines the University class.</span></span>

<span data-ttu-id="d8ccc-131">Die Universität hat die Location-Eigenschaft des Typs DbGeography.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-131">The University has the Location property of the DbGeography type.</span></span> <span data-ttu-id="d8ccc-132">Um den Typ der DbGeography verwenden zu können, müssen Sie fügen einen Verweis auf die Assembly System.Data.Entity und auch die System.Data.Spatial using-Anweisung.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-132">To use the DbGeography type, you must add a reference to the System.Data.Entity assembly and also add the System.Data.Spatial using statement.</span></span>

<span data-ttu-id="d8ccc-133">Öffnen Sie die Datei "Program.cs", und fügen Sie die folgenden using-Anweisungen am Anfang der Datei:</span><span class="sxs-lookup"><span data-stu-id="d8ccc-133">Open the Program.cs file and paste the following using statements at the top of the file:</span></span>

``` csharp
using System.Data.Spatial;
```

<span data-ttu-id="d8ccc-134">Fügen Sie die folgende Klassendefinition in University in die Datei "Program.cs" hinzu.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-134">Add the following University class definition to the Program.cs file.</span></span>

``` csharp
public class University  
{
    public int UniversityID { get; set; }
    public string Name { get; set; }
    public DbGeography Location { get; set; }
}
```

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="d8ccc-135">Definieren Sie die "DbContext" abgeleiteten Typ</span><span class="sxs-lookup"><span data-stu-id="d8ccc-135">Define the DbContext Derived Type</span></span>

<span data-ttu-id="d8ccc-136">Zusätzlich zum Definieren von Entitäten, müssen Sie eine Klasse definieren, die von "DbContext" abgeleitet und stellt "DbSet"&lt;TEntity&gt; Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-136">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="d8ccc-137">"DbSet"&lt;TEntity&gt; Eigenschaften können Sie den Kontext, die wissen, welche Typen im Modell enthalten sein sollen.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-137">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="d8ccc-138">Eine Instanz des Typs "DbContext" abgeleitet verwaltet die Entitätsobjekte während der Laufzeit, einschließlich ausfüllenden Objekten mit Daten aus einer Datenbank, verfolgen und Speichern von Daten in der Datenbank zu ändern.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-138">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="d8ccc-139">Die "DbContext" und "DbSet"-Typen werden in der EntityFramework-Assembly definiert.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-139">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="d8ccc-140">Es wird einen Verweis auf diese DLL-Datei mit EntityFramework NuGet-Paket hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-140">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="d8ccc-141">Im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-141">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="d8ccc-142">Wählen Sie **NuGet-Pakete verwalten...**</span><span class="sxs-lookup"><span data-stu-id="d8ccc-142">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="d8ccc-143">Wählen Sie in das Dialogfeld "NuGet-Pakete verwalten" die **Online** Registerkarte, und wählen Sie die **EntityFramework** Paket.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-143">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="d8ccc-144">Klicken Sie auf **installieren**</span><span class="sxs-lookup"><span data-stu-id="d8ccc-144">Click **Install**</span></span>

<span data-ttu-id="d8ccc-145">Beachten Sie, dass zusätzlich zu den EntityFramework-Assembly auch ein Verweis auf die System.ComponentModel.DataAnnotations-Assembly hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-145">Note, that in addition to the EntityFramework  assembly, a reference to the System.ComponentModel.DataAnnotations assembly is also added.</span></span>

<span data-ttu-id="d8ccc-146">Fügen Sie am Anfang der Datei "Program.cs" die folgenden using-Anweisung:</span><span class="sxs-lookup"><span data-stu-id="d8ccc-146">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="d8ccc-147">Fügen Sie die Kontextdefinition in "Program.cs" hinzu.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-147">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="d8ccc-148">Speichern und Abrufen von Daten</span><span class="sxs-lookup"><span data-stu-id="d8ccc-148">Persist and Retrieve Data</span></span>

<span data-ttu-id="d8ccc-149">Öffnen Sie die Datei "Program.cs", die, in die Main-Methode definiert ist.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-149">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="d8ccc-150">Fügen Sie der Main-Funktion mit den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-150">Add the following code into the Main function.</span></span>

<span data-ttu-id="d8ccc-151">Der Code fügt zwei neue University-Objekte in den Kontext an.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-151">The code adds two new University objects to the context.</span></span> <span data-ttu-id="d8ccc-152">Räumliche Eigenschaften werden mithilfe der Methode DbGeography.FromText initialisiert.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-152">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="d8ccc-153">Der Geography-Punkt dargestellt, wie WellKnownText an die Methode übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-153">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="d8ccc-154">Der Code speichert dann die Daten.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-154">The code then saves the data.</span></span> <span data-ttu-id="d8ccc-155">Klicken Sie dann die LINQ-Abfrage, die ein University-Objekt zurückgegeben, bei denen am Speicherort am nächsten am angegebenen Speicherort erstellt und ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-155">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

``` csharp
using (var context = new UniversityContext ())
{
    context.Universities.Add(new University()
        {
            Name = "Graphic Design Institute",
            Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
        });

    context. Universities.Add(new University()
        {
            Name = "School of Fine Art",
            Location = DbGeography.FromText("POINT(-122.335197 47.646711)"),
        });

    context.SaveChanges();

    var myLocation = DbGeography.FromText("POINT(-122.296623 47.640405)");

    var university = (from u in context.Universities
                        orderby u.Location.Distance(myLocation)
                        select u).FirstOrDefault();

    Console.WriteLine(
        "The closest University to you is: {0}.",
        university.Name);
}
```

<span data-ttu-id="d8ccc-156">Kompilieren Sie die Anwendung, und führen Sie sie aus.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-156">Compile and run the application.</span></span> <span data-ttu-id="d8ccc-157">Das Programm erzeugt die folgende Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="d8ccc-157">The program produces the following output:</span></span>

```
The closest University to you is: School of Fine Art.
```

## <a name="view-the-generated-database"></a><span data-ttu-id="d8ccc-158">Anzeigen der generierten Datenbank.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-158">View the Generated Database</span></span>

<span data-ttu-id="d8ccc-159">Wenn Sie die Anwendung zum ersten Mal ausführen, erstellt Entity Framework eine Datenbank für Sie.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-159">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="d8ccc-160">Da wir Visual Studio 2012 installiert haben, wird die Datenbank für die LocalDB-Instanz erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-160">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="d8ccc-161">In der Standardeinstellung benennt Entity Framework die Datenbank nach der vollqualifizierte Name des abgeleiteten Kontexts (in diesem Beispiel **SpatialCodeFirst.UniversityContext**).</span><span class="sxs-lookup"><span data-stu-id="d8ccc-161">By default, the Entity Framework names the database after the fully qualified name of the derived context (in this example that is **SpatialCodeFirst.UniversityContext**).</span></span> <span data-ttu-id="d8ccc-162">Die nachfolgende, wie oft die vorhandene Datenbank verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-162">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="d8ccc-163">Beachten Sie, dass wenn Sie Änderungen an Ihrem Modell vornehmen, nachdem die Datenbank erstellt wurde, sollten Sie Code First-Migrationen verwenden, um das Datenbankschema aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-163">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="d8ccc-164">Finden Sie unter [Code First in eine neue Datenbank](~/ef6/modeling/code-first/workflows/new-database.md) ein Beispiel für die Verwendung von Migrationen.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-164">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="d8ccc-165">Um die Datenbank und die Daten anzuzeigen, führen Sie folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="d8ccc-165">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="d8ccc-166">Wählen Sie im Hauptmenü von Visual Studio 2012 **Ansicht**  - &gt; **Objekt-Explorer von SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-166">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="d8ccc-167">Wenn LocalDB nicht in der Liste der Server ist, klicken Sie auf die rechten Maustaste **SQL Server** , und wählen Sie **SQL Server hinzufügen** verwenden Sie die Standardeinstellung **Windows-Authentifizierung** zum Herstellen einer Verbindung mit der die LocalDB-Instanz</span><span class="sxs-lookup"><span data-stu-id="d8ccc-167">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the the LocalDB instance</span></span>
3.  <span data-ttu-id="d8ccc-168">Erweitern Sie den Knoten für LocalDB</span><span class="sxs-lookup"><span data-stu-id="d8ccc-168">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="d8ccc-169">Erweitern der **Datenbanken** Ordner finden Sie unter der neuen Datenbank aus, und navigieren Sie zu der **Universitäten** Tabelle</span><span class="sxs-lookup"><span data-stu-id="d8ccc-169">Unfold the **Databases** folder to see the new database and browse to the **Universities** table</span></span>
5.  <span data-ttu-id="d8ccc-170">Daten anzeigen, mit der rechten Maustaste auf die Tabelle aus, und wählen Sie **Anzeigedaten**</span><span class="sxs-lookup"><span data-stu-id="d8ccc-170">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="d8ccc-171">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="d8ccc-171">Summary</span></span>

<span data-ttu-id="d8ccc-172">In dieser exemplarischen Vorgehensweise erläutert, wie Sie räumliche Typen mit Entity Framework Code First zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="d8ccc-172">In this walkthrough we looked at how to use spatial types with Entity Framework Code First.</span></span> 
