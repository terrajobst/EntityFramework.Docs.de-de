---
title: Räumliche - EF Designer - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
ms.openlocfilehash: 39fe54ffe9d0ffd1753844f7bffcd1832d82eec5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994366"
---
# <a name="spatial---ef-designer"></a><span data-ttu-id="82c66-102">Räumliche - EF-Designer</span><span class="sxs-lookup"><span data-stu-id="82c66-102">Spatial - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="82c66-103">**EF5 oder höher, nur** -APIs, die Funktionen erläutert, die auf dieser Seite usw. in Entity Framework 5 eingeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="82c66-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="82c66-104">Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.</span><span class="sxs-lookup"><span data-stu-id="82c66-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="82c66-105">Die video und schrittweise exemplarische Vorgehensweise zeigt, wie räumliche Typen, mit dem Entity Framework Designer zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="82c66-105">The video and step-by-step walkthrough shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="82c66-106">Darüber hinaus veranschaulicht eine LINQ-Abfrage verwenden, um einen Abstand zwischen zwei Orten zu finden.</span><span class="sxs-lookup"><span data-stu-id="82c66-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="82c66-107">In dieser exemplarischen Vorgehensweise wird zum Erstellen einer neuen Datenbank Model First verwenden, aber dem EF Designer kann auch verwendet werden, mit der [Database First](~/ef6/modeling/designer/workflows/database-first.md) Workflow um eine vorhandene Datenbank zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="82c66-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="82c66-108">Unterstützung von räumlichen Typen wurde in Entity Framework 5 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="82c66-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="82c66-109">Beachten Sie, um die neuen Features wie räumlichen Typen, Enumerationen und Tabellenwertfunktionen zu verwenden, Sie .NET Framework 4.5 als Ziel haben müssen.</span><span class="sxs-lookup"><span data-stu-id="82c66-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="82c66-110">Visual Studio 2012 ist standardmäßig die Zielversion .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="82c66-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="82c66-111">Verwendung der Typen von räumlichen Daten müssen Sie auch einen Entity Framework-Anbieter verwenden, der Unterstützung von räumlichen Daten verfügt.</span><span class="sxs-lookup"><span data-stu-id="82c66-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="82c66-112">Finden Sie unter [anbieterunterstützung für räumliche Typen](~/ef6/fundamentals/providers/spatial-support.md) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="82c66-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="82c66-113">Es gibt zwei Arten von main räumliche Daten: Geography und Geometry.</span><span class="sxs-lookup"><span data-stu-id="82c66-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="82c66-114">Der Geography-Datentyp speichert ellipsenförmige Daten (z. B. GPS-Breiten- und Längengrad koordiniert).</span><span class="sxs-lookup"><span data-stu-id="82c66-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="82c66-115">Der Geometry-Datentyp stellt euklidischen (flachen) Koordinatensystem dar.</span><span class="sxs-lookup"><span data-stu-id="82c66-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="82c66-116">Video ansehen</span><span class="sxs-lookup"><span data-stu-id="82c66-116">Watch the video</span></span>
<span data-ttu-id="82c66-117">Dieses Video zeigt, wie Sie räumliche Typen, mit dem Entity Framework Designer zuordnen.</span><span class="sxs-lookup"><span data-stu-id="82c66-117">This video shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="82c66-118">Darüber hinaus veranschaulicht eine LINQ-Abfrage verwenden, um einen Abstand zwischen zwei Orten zu finden.</span><span class="sxs-lookup"><span data-stu-id="82c66-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="82c66-119">**Präsentiert von**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="82c66-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="82c66-120">**Video**: [WMV](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span><span class="sxs-lookup"><span data-stu-id="82c66-120">**Video**: [WMV](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="82c66-121">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="82c66-121">Pre-Requisites</span></span>

<span data-ttu-id="82c66-122">Sie müssen Visual Studio 2012, Ultimate, Premium, Professional oder Web Express Edition installiert, um diese exemplarische Vorgehensweise abgeschlossen haben.</span><span class="sxs-lookup"><span data-stu-id="82c66-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="82c66-123">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="82c66-123">Set up the Project</span></span>

1.  <span data-ttu-id="82c66-124">Visual Studio 2012 öffnen</span><span class="sxs-lookup"><span data-stu-id="82c66-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="82c66-125">Auf der **Datei** Startmenü **neu**, und klicken Sie dann auf **Projekt**</span><span class="sxs-lookup"><span data-stu-id="82c66-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="82c66-126">Klicken Sie im linken Bereich auf **Visual C\#**, und wählen Sie dann die **Konsole** Vorlage</span><span class="sxs-lookup"><span data-stu-id="82c66-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="82c66-127">Geben Sie **SpatialEFDesigner** als Namen für das Projekt und auf **OK**</span><span class="sxs-lookup"><span data-stu-id="82c66-127">Enter **SpatialEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="82c66-128">Erstellen eines neuen Modells mit dem EF-Designer</span><span class="sxs-lookup"><span data-stu-id="82c66-128">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="82c66-129">Mit der rechten Maustaste des Projektnamen im Projektmappen-Explorer, zeigen Sie auf **hinzufügen**, und klicken Sie dann auf **neues Element**</span><span class="sxs-lookup"><span data-stu-id="82c66-129">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="82c66-130">Wählen Sie **Daten** aus dem linken Menü und wählen Sie dann **ADO.NET Entity Data Model** im Bereich Vorlagen</span><span class="sxs-lookup"><span data-stu-id="82c66-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="82c66-131">Geben Sie **UniversityModel.edmx** für den Dateinamen ein, und klicken Sie dann auf **hinzufügen**</span><span class="sxs-lookup"><span data-stu-id="82c66-131">Enter **UniversityModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="82c66-132">Wählen Sie auf der Seite Assistenten für Entity Data Model **leeres Modell** im Dialogfeld ' Modellinhalte auswählen '</span><span class="sxs-lookup"><span data-stu-id="82c66-132">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="82c66-133">Klicken Sie auf **Fertig stellen**</span><span class="sxs-lookup"><span data-stu-id="82c66-133">Click **Finish**</span></span>

<span data-ttu-id="82c66-134">Im Entity Designer, der bietet eine Entwurfsoberfläche zum Bearbeiten des Modells, wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="82c66-134">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="82c66-135">Der Assistent führt die folgenden Aktionen aus:</span><span class="sxs-lookup"><span data-stu-id="82c66-135">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="82c66-136">Generiert die EnumTestModel.edmx-Datei, die das konzeptionelle Modell, Speichermodell und die Zuordnung definiert.</span><span class="sxs-lookup"><span data-stu-id="82c66-136">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="82c66-137">Legt die Verarbeitung der Metadatenartefakte-Eigenschaft der EDMX-Datei zum Einbetten in Ausgabeassembly fest, damit die generierten Metadaten-Dateien in die Assembly eingebettet werden.</span><span class="sxs-lookup"><span data-stu-id="82c66-137">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="82c66-138">Fügt einen Verweis auf die folgenden Assemblys hinzu: EntityFramework System.ComponentModel.DataAnnotations und System.Data.Entity.</span><span class="sxs-lookup"><span data-stu-id="82c66-138">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="82c66-139">UniversityModel.tt und UniversityModel.Context.tt-Dateien erstellt, und klicken Sie unter der EDMX-Datei hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="82c66-139">Creates UniversityModel.tt and UniversityModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="82c66-140">Diese Dateien der T4-Vorlage generieren den Code, der definiert, die "DbContext" abgeleitet und POCO-Typen, die die Entitäten in der EDMX-Modell zuordnen</span><span class="sxs-lookup"><span data-stu-id="82c66-140">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="82c66-141">Fügen Sie einen neuen Entitätstyp hinzu</span><span class="sxs-lookup"><span data-stu-id="82c66-141">Add a New Entity Type</span></span>

1.  <span data-ttu-id="82c66-142">Mit der rechten Maustaste in eines leeren Bereichs der Entwurfsoberfläche, wählen **hinzufügen –&gt; Entität**, das Dialogfeld "neue Entität" angezeigt wird</span><span class="sxs-lookup"><span data-stu-id="82c66-142">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="82c66-143">Geben Sie **University** für den Typ ein, und geben **Universitäts** für den Namen "Key"-Eigenschaft wählen Sie in den Typ als **Int32**</span><span class="sxs-lookup"><span data-stu-id="82c66-143">Specify **University** for the type name and specify **UniversityID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="82c66-144">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="82c66-144">Click **OK**</span></span>
4.  <span data-ttu-id="82c66-145">Mit der rechten Maustaste in der Entitäts, und wählen Sie **Add New -&gt; Skalareigenschaft**</span><span class="sxs-lookup"><span data-stu-id="82c66-145">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="82c66-146">Benennen Sie die neue Eigenschaft **Name**</span><span class="sxs-lookup"><span data-stu-id="82c66-146">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="82c66-147">Fügen Sie eine andere skalare Eigenschaft hinzu und benennen Sie sie in **Speicherort** öffnen Sie das Fenster "Eigenschaften" aus, und ändern Sie den Typ der neuen Eigenschaft, die **Geography**</span><span class="sxs-lookup"><span data-stu-id="82c66-147">Add another scalar property and rename it to **Location** Open the Properties window and change the type of the new property to **Geography**</span></span>
7.  <span data-ttu-id="82c66-148">Speichern Sie das Modell, und erstellen Sie das Projekt</span><span class="sxs-lookup"><span data-stu-id="82c66-148">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="82c66-149">Bei der Erstellung können Warnungen zu nicht zugeordneten Entitäten und Zuordnungen in der Fehlerliste angezeigt.</span><span class="sxs-lookup"><span data-stu-id="82c66-149">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="82c66-150">Sie können diese Warnungen ignorieren, da nach dem wir die Datenbank aus dem Modell generieren möchten, die Fehler verschwinden werden.</span><span class="sxs-lookup"><span data-stu-id="82c66-150">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="82c66-151">Datenbank aus Modell generieren</span><span class="sxs-lookup"><span data-stu-id="82c66-151">Generate Database from Model</span></span>

<span data-ttu-id="82c66-152">Jetzt können wir eine Datenbank generieren, die für das Modell basiert.</span><span class="sxs-lookup"><span data-stu-id="82c66-152">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="82c66-153">Mit der rechten Maustaste in eines leeren Bereich der Entity-Designer, und wählen **Datenbank aus Modell generieren**</span><span class="sxs-lookup"><span data-stu-id="82c66-153">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="82c66-154">Wählen Sie Ihre Daten Verbindungsdialogfeld des Assistenten zum Generieren von Datenbanken wird angezeigt, klicken Sie auf die **neue Verbindung** Schaltfläche geben **(Localdb)\\Mssqllocaldb** für den Servernamen und  **University** für die Datenbank und auf **OK**</span><span class="sxs-lookup"><span data-stu-id="82c66-154">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **University** for the database and click **OK**</span></span>
3.  <span data-ttu-id="82c66-155">Ein Dialogfeld gefragt, ob Sie eine neue Datenbank erstellen möchten, wird angezeigt, klicken Sie auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="82c66-155">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="82c66-156">Klicken Sie auf **Weiter** und der Assistent zur Datenbankgenerierung generiert die Datendefinitionssprache (DDL) für eine Datenbank erstellen, die generierte DDL angezeigt wird, in der Zusammenfassung und Einstellungen Dialogfeld das Beachten Sie, dass, die die DDL-Anweisungen keine Definition für enthält eine Tabelle, in den Enumerationstyp zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="82c66-156">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="82c66-157">Klicken Sie auf **Fertig stellen** durch Klicken auf Fertig stellen das DDL-Skript nicht ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="82c66-157">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="82c66-158">Der Assistent zur Datenbankgenerierung bewirkt Folgendes: Öffnet die **UniversityModel.edmx.sql** in T-SQL-Editor generiert Datei in den Store-Schemas und der Mapping-Abschnitten, die Zielimplementierung des zu Informationen zur Verbindungszeichenfolge fügt die Datei "App.config"</span><span class="sxs-lookup"><span data-stu-id="82c66-158">The Create Database Wizard does the following: Opens the **UniversityModel.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="82c66-159">Klicken Sie auf der rechten Maustaste im T-SQL-Editor, und wählen Sie **Execute** das Herstellen einer Verbindung mit Server-Dialogfeld angezeigt wird, geben Sie die Verbindungsinformationen aus Schritt 2, und klicken Sie auf **verbinden**</span><span class="sxs-lookup"><span data-stu-id="82c66-159">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="82c66-160">Klicken Sie zum Anzeigen des generierten Schemas mit der rechten Maustaste auf den Datenbanknamen im Objekt-Explorer von SQL Server, und wählen Sie **aktualisieren**</span><span class="sxs-lookup"><span data-stu-id="82c66-160">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="82c66-161">Speichern und Abrufen von Daten</span><span class="sxs-lookup"><span data-stu-id="82c66-161">Persist and Retrieve Data</span></span>

<span data-ttu-id="82c66-162">Öffnen Sie die Datei "Program.cs", die, in die Main-Methode definiert ist.</span><span class="sxs-lookup"><span data-stu-id="82c66-162">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="82c66-163">Fügen Sie der Main-Funktion mit den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="82c66-163">Add the following code into the Main function.</span></span>

<span data-ttu-id="82c66-164">Der Code fügt zwei neue University-Objekte in den Kontext an.</span><span class="sxs-lookup"><span data-stu-id="82c66-164">The code adds two new University objects to the context.</span></span> <span data-ttu-id="82c66-165">Räumliche Eigenschaften werden mithilfe der Methode DbGeography.FromText initialisiert.</span><span class="sxs-lookup"><span data-stu-id="82c66-165">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="82c66-166">Der Geography-Punkt dargestellt, wie WellKnownText an die Methode übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="82c66-166">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="82c66-167">Der Code speichert dann die Daten.</span><span class="sxs-lookup"><span data-stu-id="82c66-167">The code then saves the data.</span></span> <span data-ttu-id="82c66-168">Klicken Sie dann die LINQ-Abfrage, die ein University-Objekt zurückgegeben, bei denen am Speicherort am nächsten am angegebenen Speicherort erstellt und ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="82c66-168">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

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

<span data-ttu-id="82c66-169">Kompilieren Sie die Anwendung, und führen Sie sie aus.</span><span class="sxs-lookup"><span data-stu-id="82c66-169">Compile and run the application.</span></span> <span data-ttu-id="82c66-170">Das Programm erzeugt die folgende Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="82c66-170">The program produces the following output:</span></span>

```
The closest University to you is: School of Fine Art.
```

<span data-ttu-id="82c66-171">Klicken Sie zum Anzeigen von Daten in der Datenbank, mit der rechten Maustaste auf den Datenbanknamen im Objekt-Explorer von SQL Server, und wählen Sie **aktualisieren**.</span><span class="sxs-lookup"><span data-stu-id="82c66-171">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="82c66-172">Klicken Sie dann auf der rechten Maustaste auf die Tabelle, und wählen **Ansichtsdaten**.</span><span class="sxs-lookup"><span data-stu-id="82c66-172">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="82c66-173">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="82c66-173">Summary</span></span>

<span data-ttu-id="82c66-174">In dieser exemplarischen Vorgehensweise erläutert, wie räumliche Typen, die mit dem Entity Framework Designer zuordnen und räumliche Typen im Code verwenden.</span><span class="sxs-lookup"><span data-stu-id="82c66-174">In this walkthrough we looked at how to map spatial types using the Entity Framework Designer and how to use spatial types in code.</span></span> 
