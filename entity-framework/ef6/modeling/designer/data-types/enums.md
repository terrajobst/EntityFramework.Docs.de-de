---
title: Aufzählungs Unterstützung-EF-Designer-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
ms.openlocfilehash: 92a763b84a04d3ce7ec0853ef2a4852356cf7997
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415352"
---
# <a name="enum-support---ef-designer"></a><span data-ttu-id="26877-102">Aufzählungs Unterstützung-EF-Designer</span><span class="sxs-lookup"><span data-stu-id="26877-102">Enum Support - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="26877-103">**Nur EF5** : die Features, APIs usw., die auf dieser Seite erläutert wurden, wurden in Entity Framework 5 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="26877-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="26877-104">Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.</span><span class="sxs-lookup"><span data-stu-id="26877-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="26877-105">In diesem Video und der schrittweisen exemplarischen Vorgehensweise wird die Verwendung von Enumerationstypen mit dem Entity Framework Designer erläutert.</span><span class="sxs-lookup"><span data-stu-id="26877-105">This video and step-by-step walkthrough shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="26877-106">Außerdem wird veranschaulicht, wie Sie-auf--Aufstände in einer LINQ-Abfrage verwenden.</span><span class="sxs-lookup"><span data-stu-id="26877-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="26877-107">In dieser exemplarischen Vorgehensweise wird Model First verwendet, um eine neue Datenbank zu erstellen, aber der EF-Designer kann auch mit dem [Database First](~/ef6/modeling/designer/workflows/database-first.md) Workflow verwendet werden, um eine Zuordnung zu einer vorhandenen Datenbank herzustellen.</span><span class="sxs-lookup"><span data-stu-id="26877-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="26877-108">Die Aufzählungs Unterstützung wurde in Entity Framework 5 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="26877-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="26877-109">Wenn Sie die neuen Funktionen wie Enumerationstypen, räumliche Datentypen und Tabellenwert Funktionen verwenden möchten, müssen Sie .NET Framework 4,5-Zielversion verwenden.</span><span class="sxs-lookup"><span data-stu-id="26877-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="26877-110">Visual Studio 2012 hat standardmäßig .NET 4,5 als Ziel.</span><span class="sxs-lookup"><span data-stu-id="26877-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="26877-111">In Entity Framework kann eine Enumeration die folgenden zugrunde liegenden Typen aufweisen: **Byte**, **Int16**, **Int32**, **Int64** oder **SByte**.</span><span class="sxs-lookup"><span data-stu-id="26877-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="26877-112">Video ansehen</span><span class="sxs-lookup"><span data-stu-id="26877-112">Watch the Video</span></span>
<span data-ttu-id="26877-113">In diesem Video wird gezeigt, wie Enumerationstypen mit dem Entity Framework Designer verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="26877-113">This video shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="26877-114">Außerdem wird veranschaulicht, wie Sie-auf--Aufstände in einer LINQ-Abfrage verwenden.</span><span class="sxs-lookup"><span data-stu-id="26877-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="26877-115">**Präsentiert von**: Julia kornich</span><span class="sxs-lookup"><span data-stu-id="26877-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="26877-116">**Video**: [WMV](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (zip)](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span><span class="sxs-lookup"><span data-stu-id="26877-116">**Video**: [WMV](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="26877-117">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="26877-117">Pre-Requisites</span></span>

<span data-ttu-id="26877-118">Sie müssen Visual Studio 2012, Ultimate, Premium, Professional oder Web Express Edition installiert haben, um diese exemplarische Vorgehensweise abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="26877-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="26877-119">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="26877-119">Set up the Project</span></span>

1.  <span data-ttu-id="26877-120">Öffnen Sie Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="26877-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="26877-121">Zeigen Sie im Menü **Datei** auf **neu**, und klicken Sie dann auf **Projekt** .</span><span class="sxs-lookup"><span data-stu-id="26877-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="26877-122">Klicken Sie im linken Bereich auf **Visual C-\#** , und wählen Sie dann die **Konsolen** Vorlage aus.</span><span class="sxs-lookup"><span data-stu-id="26877-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="26877-123">Geben Sie **enumef Designer** als Namen für das Projekt ein, und klicken Sie auf **OK** .</span><span class="sxs-lookup"><span data-stu-id="26877-123">Enter **EnumEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="26877-124">Erstellen eines neuen Modells mit dem EF-Designer</span><span class="sxs-lookup"><span data-stu-id="26877-124">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="26877-125">Klicken Sie mit der rechten Maustaste auf den Projektnamen Projektmappen-Explorer, zeigen Sie auf **Hinzufügen**, und klicken Sie dann auf **Neues Element**</span><span class="sxs-lookup"><span data-stu-id="26877-125">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="26877-126">Wählen Sie im linken Menü **Daten** aus, und wählen Sie dann im Bereich Vorlagen die Option **ADO.NET Entity Data Model** aus.</span><span class="sxs-lookup"><span data-stu-id="26877-126">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="26877-127">Geben Sie als Dateiname **enumtestmodel. edmx** ein, und klicken Sie dann auf **Hinzufügen** .</span><span class="sxs-lookup"><span data-stu-id="26877-127">Enter **EnumTestModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="26877-128">Wählen Sie auf der Seite des Entity Data Model-Assistenten im Dialogfeld Modell Inhalte auswählen die Option **leeres Modell** aus.</span><span class="sxs-lookup"><span data-stu-id="26877-128">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="26877-129">Klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="26877-129">Click **Finish**</span></span>

<span data-ttu-id="26877-130">Die Entity Designer, die eine Entwurfs Oberfläche zum Bearbeiten des Modells bereitstellt, wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="26877-130">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="26877-131">Der Assistent führt die folgenden Aktionen aus:</span><span class="sxs-lookup"><span data-stu-id="26877-131">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="26877-132">Generiert die Datei "enumtestmodel. edmx", die das konzeptionelle Modell, das Speichermodell und die Zuordnung zwischen Ihnen definiert.</span><span class="sxs-lookup"><span data-stu-id="26877-132">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="26877-133">Legt die metadatenartefaktverarbeitungs-Eigenschaft der EDMX-Datei fest, die in die Ausgabeassembly eingebettet werden soll, sodass die generierten Metadatendateien in die Assembly eingebettet werden</span><span class="sxs-lookup"><span data-stu-id="26877-133">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="26877-134">Fügt einen Verweis auf die folgenden Assemblys hinzu: EntityFramework, System. ComponentModel. DataAnnotations und System. Data. Entity.</span><span class="sxs-lookup"><span data-stu-id="26877-134">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="26877-135">Erstellt EnumTestModel.TT-und EnumTestModel.Context.tt-Dateien und fügt Sie der EDMX-Datei hinzu.</span><span class="sxs-lookup"><span data-stu-id="26877-135">Creates EnumTestModel.tt and EnumTestModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="26877-136">Diese T4-Vorlagen Dateien generieren den Code, der den von dbcontext abgeleiteten Typ und poco-Typen definiert, die den Entitäten im edmx-Modell zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="26877-136">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model.</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="26877-137">Neuen Entitätstyp hinzufügen</span><span class="sxs-lookup"><span data-stu-id="26877-137">Add a New Entity Type</span></span>

1.  <span data-ttu-id="26877-138">Klicken Sie mit der rechten Maustaste auf einen leeren Bereich der Entwurfs Oberfläche, und wählen Sie **Add-&gt; Entity**, das Dialogfeld neue Entität wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="26877-138">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="26877-139">Geben Sie die **Abteilung** für den Typnamen an, und geben Sie **DepartmentID** als Schlüssel Eigenschaftsnamen an, belassen Sie den Typ als **Int32** .</span><span class="sxs-lookup"><span data-stu-id="26877-139">Specify **Department** for the type name and specify **DepartmentID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="26877-140">Klicken Sie auf **OK**</span><span class="sxs-lookup"><span data-stu-id="26877-140">Click **OK**</span></span>
4.  <span data-ttu-id="26877-141">Klicken Sie mit der rechten Maustaste auf die Entität, und wählen Sie **Add New-&gt; Scalar**</span><span class="sxs-lookup"><span data-stu-id="26877-141">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="26877-142">Benennen Sie die neue Eigenschaft in **Name** um.</span><span class="sxs-lookup"><span data-stu-id="26877-142">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="26877-143">Ändern Sie den Typ der neuen Eigenschaft in **Int32** (Standardmäßig ist die neue Eigenschaft vom Typ String), um den Typ zu ändern, öffnen Sie die Eigenschaftenfenster, und ändern Sie die Type-Eigenschaft in **Int32** .</span><span class="sxs-lookup"><span data-stu-id="26877-143">Change the type of the new property to **Int32** (by default, the new property is of String type) To change the type, open the Properties window and change the Type property to **Int32**</span></span>
7.  <span data-ttu-id="26877-144">Fügen Sie eine weitere skalare Eigenschaft hinzu, und benennen Sie Sie in **Budget**um. ändern Sie den Typ in **Decimal**</span><span class="sxs-lookup"><span data-stu-id="26877-144">Add another scalar property and rename it to **Budget**, change the type to **Decimal**</span></span>

## <a name="add-an-enum-type"></a><span data-ttu-id="26877-145">Hinzufügen eines Aufzählungs Typs</span><span class="sxs-lookup"><span data-stu-id="26877-145">Add an Enum Type</span></span>

1.  <span data-ttu-id="26877-146">Klicken Sie im Entity Framework Designer mit der rechten Maustaste auf die Eigenschaft Name, und wählen Sie in **Enumeration konvertieren** aus.</span><span class="sxs-lookup"><span data-stu-id="26877-146">In the Entity Framework Designer, right-click the Name property, select **Convert to enum**</span></span>

    ![In Aufzählung konvertieren](~/ef6/media/converttoenum.png)

2.  <span data-ttu-id="26877-148">Geben Sie im Dialogfeld Enumeration **Hinzufügen** den Namen **departmentnames** für den Enumerationstypnamen ein, ändern Sie den zugrunde liegenden Typ in **Int32**, und fügen Sie dann die folgenden Member zum-Typ hinzu: Englisch, Math und Economics.</span><span class="sxs-lookup"><span data-stu-id="26877-148">In the **Add Enum** dialog box type **DepartmentNames** for the Enum Type Name, change the Underlying Type to **Int32**, and then add the following members to the type: English, Math, and Economics</span></span>

    ![Umerationstyp hinzufügen](~/ef6/media/addenumtype.png)

3.  <span data-ttu-id="26877-150">**OK** drücken</span><span class="sxs-lookup"><span data-stu-id="26877-150">Press **OK**</span></span>
4.  <span data-ttu-id="26877-151">Speichern Sie das Modell, und erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="26877-151">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="26877-152">Wenn Sie erstellen, werden möglicherweise Warnungen zu nicht zugeordneten Entitäten und Zuordnungen in der Fehlerliste angezeigt.</span><span class="sxs-lookup"><span data-stu-id="26877-152">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="26877-153">Sie können diese Warnungen ignorieren, da die Fehler nach dem Generieren der Datenbank aus dem Modell entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="26877-153">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

<span data-ttu-id="26877-154">Wenn Sie die Eigenschaftenfenster betrachten, werden Sie feststellen, dass der Typ der Name-Eigenschaft in **departmentnames** geändert wurde und der neu hinzugefügte Enumerationstyp der Liste der Typen hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="26877-154">If you look at the Properties window, you will notice that the type of the Name property was changed to **DepartmentNames** and the newly added enum type was added to the list of types.</span></span>

<span data-ttu-id="26877-155">Wenn Sie zum Fenstermodell Browser wechseln, sehen Sie, dass der Typ auch dem Knoten Enumerationstypen hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="26877-155">If you switch to the Model Browser window, you will see that the type was also added to the Enum Types node.</span></span>

![Modell Browser](~/ef6/media/modelbrowser.png)

>[!NOTE]
> <span data-ttu-id="26877-157">Sie können in diesem Fenster auch neue Enumerationstypen hinzufügen, indem Sie auf die Rechte Maustaste klicken und **Enumerationstyp hinzufügen**auswählen.</span><span class="sxs-lookup"><span data-stu-id="26877-157">You can also add new enum types from this window by clicking the right mouse button and selecting **Add Enum Type**.</span></span> <span data-ttu-id="26877-158">Nachdem der Typ erstellt wurde, wird er in der Liste der Typen angezeigt, und Sie können einer Eigenschaft zuordnen.</span><span class="sxs-lookup"><span data-stu-id="26877-158">Once the type is created it will appear in the list of types and you would be able to associate with a property</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="26877-159">Datenbank aus Modell generieren</span><span class="sxs-lookup"><span data-stu-id="26877-159">Generate Database from Model</span></span>

<span data-ttu-id="26877-160">Nun können wir eine Datenbank generieren, die auf dem Modell basiert.</span><span class="sxs-lookup"><span data-stu-id="26877-160">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="26877-161">Klicken Sie mit der rechten Maustaste auf einen leeren Bereich auf der Entity Designer Oberfläche, und wählen Sie **Datenbank aus Modell generieren** .</span><span class="sxs-lookup"><span data-stu-id="26877-161">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="26877-162">Das Dialog Feld Wählen Sie Ihre Datenverbindung aus des Assistenten zum Generieren von Datenbanken wird angezeigt. Klicken Sie auf die Schaltfläche **neue Verbindung** , und **Geben Sie** **(localdb)\\mssqllocaldb** für den Servernamen und den **enumeringwert** für die Datenbank an.</span><span class="sxs-lookup"><span data-stu-id="26877-162">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **EnumTest** for the database and click **OK**</span></span>
3.  <span data-ttu-id="26877-163">Wenn Sie in einem Dialogfeld gefragt werden, ob Sie eine neue Datenbank erstellen möchten, klicken Sie auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="26877-163">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="26877-164">Klicken Sie auf **weiter** , und der Assistent zum Erstellen einer Datenbank generiert die Datendefinitionssprache (DDL) zum Erstellen einer Datenbank. die generierte DDL wird im Dialog Feld Zusammenfassung und Einstellungen angezeigt. Hinweis: die DDL enthält keine Definition für eine Tabelle, die dem Enumerationstyp zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="26877-164">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="26877-165">Klicken Sie auf **Fertig** stellen klicken Sie das DDL-Skript nicht aus.</span><span class="sxs-lookup"><span data-stu-id="26877-165">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="26877-166">Der Assistent zum Erstellen einer Datenbank führt Folgendes aus: öffnet die Datei " **enumtest. edmx. SQL** " im T-SQL-Editor, generiert die Abschnitte "Store Schema" und "Mapping" der Datei "App. config".</span><span class="sxs-lookup"><span data-stu-id="26877-166">The Create Database Wizard does the following: Opens the **EnumTest.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="26877-167">Klicken Sie im T-SQL-Editor auf die Rechte Maustaste, und wählen Sie das Dialogfeld Verbindung mit Server herstellen aus, und **Geben Sie** die Verbindungsinformationen aus Schritt 2 ein.</span><span class="sxs-lookup"><span data-stu-id="26877-167">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="26877-168">Um das generierte Schema anzuzeigen, klicken Sie in SQL Server-Objekt-Explorer mit der rechten Maustaste auf den Datenbanknamen, und wählen Sie **Aktualisieren** .</span><span class="sxs-lookup"><span data-stu-id="26877-168">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="26877-169">Persistenz und Abrufen von Daten</span><span class="sxs-lookup"><span data-stu-id="26877-169">Persist and Retrieve Data</span></span>

<span data-ttu-id="26877-170">Öffnen Sie die Datei Program.cs, in der die Main-Methode definiert ist.</span><span class="sxs-lookup"><span data-stu-id="26877-170">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="26877-171">Fügen Sie der Main-Funktion den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="26877-171">Add the following code into the Main function.</span></span> <span data-ttu-id="26877-172">Der Code fügt dem Kontext ein neues Abteilungs Objekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="26877-172">The code adds a new Department object to the context.</span></span> <span data-ttu-id="26877-173">Anschließend werden die Daten gespeichert.</span><span class="sxs-lookup"><span data-stu-id="26877-173">It then saves the data.</span></span> <span data-ttu-id="26877-174">Der Code führt außerdem eine LINQ-Abfrage aus, die eine Abteilung zurückgibt, in der der Name departmentnames. English lautet.</span><span class="sxs-lookup"><span data-stu-id="26877-174">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

``` csharp
using (var context = new EnumTestModelContainer())
{
    context.Departments.Add(new Department{ Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} and Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

<span data-ttu-id="26877-175">Kompilieren Sie die Anwendung, und führen Sie sie aus.</span><span class="sxs-lookup"><span data-stu-id="26877-175">Compile and run the application.</span></span> <span data-ttu-id="26877-176">Das Programm erzeugt die folgende Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="26877-176">The program produces the following output:</span></span>

```console
DepartmentID: 1 Name: English
```

<span data-ttu-id="26877-177">Zum Anzeigen von Daten in der Datenbank klicken Sie in SQL Server-Objekt-Explorer mit der rechten Maustaste auf den Datenbanknamen, und wählen Sie **Aktualisieren**aus.</span><span class="sxs-lookup"><span data-stu-id="26877-177">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="26877-178">Klicken Sie dann auf die Rechte Maustaste in der Tabelle, und wählen Sie **Daten anzeigen**aus.</span><span class="sxs-lookup"><span data-stu-id="26877-178">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="26877-179">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="26877-179">Summary</span></span>

<span data-ttu-id="26877-180">In dieser exemplarischen Vorgehensweise wurde erläutert, wie Enumerationstypen mithilfe der Entity Framework Designer zugeordnet werden und wie Enumerationen im Code verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="26877-180">In this walkthrough we looked at how to map enum types using the Entity Framework Designer and how to use enums in code.</span></span> 
