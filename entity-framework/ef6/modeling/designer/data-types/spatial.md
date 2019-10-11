---
title: Spatial-EF-Designer-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
ms.openlocfilehash: a9c54fbc14dd02ce5d4d91449a0d5f9e72f7f0f7
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182501"
---
# <a name="spatial---ef-designer"></a><span data-ttu-id="4ff5f-102">Räumlich: EF-Designer</span><span class="sxs-lookup"><span data-stu-id="4ff5f-102">Spatial - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="4ff5f-103">**Nur EF5** : die Features, APIs usw., die auf dieser Seite erläutert wurden, wurden in Entity Framework 5 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="4ff5f-104">Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="4ff5f-105">Im Video und in der exemplarischen Vorgehensweise wird gezeigt, wie räumliche Typen mit dem Entity Framework Designer zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-105">The video and step-by-step walkthrough shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="4ff5f-106">Außerdem wird veranschaulicht, wie eine LINQ-Abfrage verwendet wird, um einen Abstand zwischen zwei Standorten zu ermitteln.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="4ff5f-107">In dieser exemplarischen Vorgehensweise wird Model First verwendet, um eine neue Datenbank zu erstellen, aber der EF-Designer kann auch mit dem [Database First](~/ef6/modeling/designer/workflows/database-first.md) Workflow verwendet werden, um eine Zuordnung zu einer vorhandenen Datenbank herzustellen.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="4ff5f-108">Die Unterstützung räumlicher Typen wurde in Entity Framework 5 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="4ff5f-109">Beachten Sie, dass Sie für die Verwendung der neuen Funktionen, wie z. b. räumlichem Typ, aufzählen und Tabellenwert Funktionen, auf .NET Framework 4,5 abzielen müssen.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="4ff5f-110">Visual Studio 2012 hat standardmäßig .NET 4,5 als Ziel.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="4ff5f-111">Um räumliche Datentypen zu verwenden, müssen Sie auch einen Entity Framework Anbieter verwenden, der über räumliche Unterstützung verfügt.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="4ff5f-112">Weitere Informationen finden Sie [unter Anbieter Unterstützung für räumliche Typen](~/ef6/fundamentals/providers/spatial-support.md) .</span><span class="sxs-lookup"><span data-stu-id="4ff5f-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="4ff5f-113">Es gibt zwei Haupt Datentypen für räumliche Daten: Geography und Geometry.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="4ff5f-114">Der geography-Datentyp speichert Ellipsen Daten (z. b. GPS-breiten-und Längenkoordinaten).</span><span class="sxs-lookup"><span data-stu-id="4ff5f-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="4ff5f-115">Der geometry-Datentyp stellt das euklidische (flache) Koordinatensystem dar.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="4ff5f-116">Video ansehen</span><span class="sxs-lookup"><span data-stu-id="4ff5f-116">Watch the video</span></span>
<span data-ttu-id="4ff5f-117">In diesem Video wird gezeigt, wie räumliche Typen dem Entity Framework Designer zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-117">This video shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="4ff5f-118">Außerdem wird veranschaulicht, wie eine LINQ-Abfrage verwendet wird, um einen Abstand zwischen zwei Standorten zu ermitteln.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="4ff5f-119">**Präsentiert von**: Julia kornich</span><span class="sxs-lookup"><span data-stu-id="4ff5f-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="4ff5f-120">**Video**: [WMV](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span><span class="sxs-lookup"><span data-stu-id="4ff5f-120">**Video**: [WMV](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="4ff5f-121">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="4ff5f-121">Pre-Requisites</span></span>

<span data-ttu-id="4ff5f-122">Sie müssen Visual Studio 2012, Ultimate, Premium, Professional oder Web Express Edition installiert haben, um diese exemplarische Vorgehensweise abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="4ff5f-123">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="4ff5f-123">Set up the Project</span></span>

1.  <span data-ttu-id="4ff5f-124">Öffnen Sie Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="4ff5f-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="4ff5f-125">Zeigen Sie im Menü **Datei** auf **neu**, und klicken Sie dann auf **Projekt** .</span><span class="sxs-lookup"><span data-stu-id="4ff5f-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="4ff5f-126">Klicken Sie im linken Bereich auf **Visual C @ no__t-1**, und wählen Sie dann die **Konsolen** Vorlage aus.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="4ff5f-127">Geben Sie **spatialefdesigner** als Namen für das Projekt ein, und klicken Sie auf **OK** .</span><span class="sxs-lookup"><span data-stu-id="4ff5f-127">Enter **SpatialEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="4ff5f-128">Erstellen eines neuen Modells mit dem EF-Designer</span><span class="sxs-lookup"><span data-stu-id="4ff5f-128">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="4ff5f-129">Klicken Sie mit der rechten Maustaste auf den Projektnamen Projektmappen-Explorer, zeigen Sie auf **Hinzufügen**, und klicken Sie dann auf **Neues Element**</span><span class="sxs-lookup"><span data-stu-id="4ff5f-129">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="4ff5f-130">Wählen Sie im linken Menü **Daten** aus, und wählen Sie dann im Bereich Vorlagen die Option **ADO.NET Entity Data Model** aus.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="4ff5f-131">Geben Sie für den Dateinamen " **universitymodel. edmx** " ein, und klicken Sie auf **Hinzufügen** .</span><span class="sxs-lookup"><span data-stu-id="4ff5f-131">Enter **UniversityModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="4ff5f-132">Wählen Sie auf der Seite des Entity Data Model-Assistenten im Dialogfeld Modell Inhalte auswählen die Option **leeres Modell** aus.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-132">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="4ff5f-133">Klicken auf **Fertig** stellen</span><span class="sxs-lookup"><span data-stu-id="4ff5f-133">Click **Finish**</span></span>

<span data-ttu-id="4ff5f-134">Die Entity Designer, die eine Entwurfs Oberfläche zum Bearbeiten des Modells bereitstellt, wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-134">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="4ff5f-135">Der Assistent führt die folgenden Aktionen aus:</span><span class="sxs-lookup"><span data-stu-id="4ff5f-135">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="4ff5f-136">Generiert die Datei "enumtestmodel. edmx", die das konzeptionelle Modell, das Speichermodell und die Zuordnung zwischen Ihnen definiert.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-136">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="4ff5f-137">Legt die metadatenartefaktverarbeitungs-Eigenschaft der EDMX-Datei fest, die in die Ausgabeassembly eingebettet werden soll, sodass die generierten Metadatendateien in die Assembly eingebettet werden</span><span class="sxs-lookup"><span data-stu-id="4ff5f-137">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="4ff5f-138">Fügt einen Verweis auf die folgenden Assemblys hinzu: EntityFramework, System. ComponentModel. DataAnnotations und System. Data. Entity.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-138">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="4ff5f-139">Erstellt UniversityModel.TT-und UniversityModel.Context.tt-Dateien und fügt Sie der EDMX-Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-139">Creates UniversityModel.tt and UniversityModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="4ff5f-140">Diese T4-Vorlagen Dateien generieren den Code, der den von dbcontext abgeleiteten Typ und poco-Typen definiert, die den Entitäten im edmx-Modell zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-140">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="4ff5f-141">Neuen Entitätstyp hinzufügen</span><span class="sxs-lookup"><span data-stu-id="4ff5f-141">Add a New Entity Type</span></span>

1.  <span data-ttu-id="4ff5f-142">Klicken Sie mit der rechten Maustaste auf einen leeren Bereich der Entwurfs Oberfläche, wählen Sie **Add-&gt; Entity**aus, das Dialogfeld neue Entität wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-142">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="4ff5f-143">Geben Sie **University** **als Typnamen** an, und geben Sie für den Schlüssel Eigenschaftsnamen die Angabe " **universityid** " an.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-143">Specify **University** for the type name and specify **UniversityID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="4ff5f-144">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-144">Click **OK**</span></span>
4.  <span data-ttu-id="4ff5f-145">Klicken Sie mit der rechten Maustaste auf die Entität, und wählen Sie **Add New-&gt; Scalar Property**</span><span class="sxs-lookup"><span data-stu-id="4ff5f-145">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="4ff5f-146">Benennen Sie die neue Eigenschaft in **Name** um.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-146">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="4ff5f-147">Fügen Sie eine weitere skalare Eigenschaft hinzu, und benennen Sie Sie in **Location** um. Öffnen Sie die Eigenschaftenfenster, und ändern Sie den Typ der neuen Eigenschaft in **geography**</span><span class="sxs-lookup"><span data-stu-id="4ff5f-147">Add another scalar property and rename it to **Location** Open the Properties window and change the type of the new property to **Geography**</span></span>
7.  <span data-ttu-id="4ff5f-148">Speichern Sie das Modell, und erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-148">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="4ff5f-149">Wenn Sie erstellen, werden möglicherweise Warnungen zu nicht zugeordneten Entitäten und Zuordnungen in der Fehlerliste angezeigt.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-149">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="4ff5f-150">Sie können diese Warnungen ignorieren, da die Fehler nach dem Generieren der Datenbank aus dem Modell entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-150">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="4ff5f-151">Datenbank aus Modell generieren</span><span class="sxs-lookup"><span data-stu-id="4ff5f-151">Generate Database from Model</span></span>

<span data-ttu-id="4ff5f-152">Nun können wir eine Datenbank generieren, die auf dem Modell basiert.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-152">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="4ff5f-153">Klicken Sie mit der rechten Maustaste auf einen leeren Bereich auf der Entity Designer Oberfläche, und wählen Sie **Datenbank aus Modell generieren** .</span><span class="sxs-lookup"><span data-stu-id="4ff5f-153">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="4ff5f-154">Das Dialog Feld Datenverbindung auswählen des Assistenten zum Generieren von Datenbanken wird angezeigt. Klicken Sie auf die Schaltfläche **neue Verbindung** , und geben Sie **(localdb) \\mssqllocaldb** für den Servernamen und die **University** für die Datenbank an, und klicken Sie auf **OK**</span><span class="sxs-lookup"><span data-stu-id="4ff5f-154">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **University** for the database and click **OK**</span></span>
3.  <span data-ttu-id="4ff5f-155">Wenn Sie in einem Dialogfeld gefragt werden, ob Sie eine neue Datenbank erstellen möchten, klicken Sie auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-155">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="4ff5f-156">Klicken Sie auf **weiter** , und der Assistent zum Erstellen einer Datenbank generiert die Datendefinitionssprache (DDL) zum Erstellen einer Datenbank. die generierte DDL wird im Dialog Feld Zusammenfassung und Einstellungen angezeigt. Hinweis: die DDL enthält keine Definition für eine Tabelle, die dem Enumerationstyp</span><span class="sxs-lookup"><span data-stu-id="4ff5f-156">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="4ff5f-157">Klicken Sie auf **Fertig** stellen klicken Sie das DDL-Skript nicht aus.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-157">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="4ff5f-158">Der Assistent zum Erstellen einer Datenbank führt folgende Schritte aus: Öffnet die Datei " **universitymodel. edmx. SQL** " im T-SQL-Editor, generiert die Abschnitte "Store Schema" und "Mapping" der EDMX-Datei und fügt der Datei "App. config" Verbindungs Zeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="4ff5f-158">The Create Database Wizard does the following: Opens the **UniversityModel.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="4ff5f-159">Klicken Sie im T-SQL-Editor auf die Rechte Maustaste, und wählen Sie das Dialogfeld Verbindung mit Server herstellen aus, und **Geben Sie** die Verbindungsinformationen aus Schritt 2 ein.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-159">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="4ff5f-160">Um das generierte Schema anzuzeigen, klicken Sie in SQL Server-Objekt-Explorer mit der rechten Maustaste auf den Datenbanknamen, und wählen Sie **Aktualisieren** .</span><span class="sxs-lookup"><span data-stu-id="4ff5f-160">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="4ff5f-161">Persistenz und Abrufen von Daten</span><span class="sxs-lookup"><span data-stu-id="4ff5f-161">Persist and Retrieve Data</span></span>

<span data-ttu-id="4ff5f-162">Öffnen Sie die Datei Program.cs, in der die Main-Methode definiert ist.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-162">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="4ff5f-163">Fügen Sie der Main-Funktion den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-163">Add the following code into the Main function.</span></span>

<span data-ttu-id="4ff5f-164">Der Code fügt dem Kontext zwei neue University-Objekte hinzu.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-164">The code adds two new University objects to the context.</span></span> <span data-ttu-id="4ff5f-165">Räumliche Eigenschaften werden mithilfe der Methode "dbgeography. fromtext" initialisiert.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-165">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="4ff5f-166">Der als wellknowntext dargestellte geografiepunkt wird an die-Methode übermittelt.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-166">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="4ff5f-167">Der Code speichert die Daten dann.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-167">The code then saves the data.</span></span> <span data-ttu-id="4ff5f-168">Anschließend wird die LINQ-Abfrage, die ein University-Objekt zurückgibt, dessen Standort dem angegebenen Speicherort am nächsten liegt, erstellt und ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-168">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

``` csharp
using (var context = new UniversityModelContainer())
{
    context.Universities.Add(new University()
    {
        Name = "Graphic Design Institute",
        Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
    });

    context.Universities.Add(new University()
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

<span data-ttu-id="4ff5f-169">Kompilieren Sie die Anwendung, und führen Sie sie aus.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-169">Compile and run the application.</span></span> <span data-ttu-id="4ff5f-170">Das Programm erzeugt die folgende Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="4ff5f-170">The program produces the following output:</span></span>

```console
The closest University to you is: School of Fine Art.
```

<span data-ttu-id="4ff5f-171">Zum Anzeigen von Daten in der Datenbank klicken Sie in SQL Server-Objekt-Explorer mit der rechten Maustaste auf den Datenbanknamen, und wählen Sie **Aktualisieren**aus.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-171">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="4ff5f-172">Klicken Sie dann auf die Rechte Maustaste in der Tabelle, und wählen Sie **Daten anzeigen**aus.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-172">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="4ff5f-173">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="4ff5f-173">Summary</span></span>

<span data-ttu-id="4ff5f-174">In dieser exemplarischen Vorgehensweise wurde erläutert, wie räumliche Typen mithilfe der Entity Framework Designer zugeordnet werden und wie räumliche Typen im Code verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="4ff5f-174">In this walkthrough we looked at how to map spatial types using the Entity Framework Designer and how to use spatial types in code.</span></span> 
