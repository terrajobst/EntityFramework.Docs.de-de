---
title: Unterstützung von Enumerationen - EF Designer - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
ms.openlocfilehash: 331182c4311565c94cf072eb9b9ad372ac76180a
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283939"
---
# <a name="enum-support---ef-designer"></a><span data-ttu-id="3220b-102">Unterstützung von Enumerationen - EF-Designer</span><span class="sxs-lookup"><span data-stu-id="3220b-102">Enum Support - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="3220b-103">**EF5 oder höher, nur** -APIs, die Funktionen erläutert, die auf dieser Seite usw. in Entity Framework 5 eingeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="3220b-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="3220b-104">Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.</span><span class="sxs-lookup"><span data-stu-id="3220b-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="3220b-105">Dieses video und schrittweise exemplarische Vorgehensweise zeigt, wie Enumerationstypen mit dem Entity Framework Designer verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="3220b-105">This video and step-by-step walkthrough shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="3220b-106">Es wird veranschaulicht, wie Enumerationen in einer LINQ-Abfrage verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="3220b-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="3220b-107">In dieser exemplarischen Vorgehensweise wird zum Erstellen einer neuen Datenbank Model First verwenden, aber dem EF Designer kann auch verwendet werden, mit der [Database First](~/ef6/modeling/designer/workflows/database-first.md) Workflow um eine vorhandene Datenbank zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="3220b-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="3220b-108">Enum-Unterstützung wurde in Entity Framework 5 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="3220b-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="3220b-109">Um die neuen Features wie Enumerationen, räumliche Datentypen und Tabellenwertfunktionen zu verwenden, müssen Sie .NET Framework 4.5 ausrichten.</span><span class="sxs-lookup"><span data-stu-id="3220b-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="3220b-110">Visual Studio 2012 ist standardmäßig die Zielversion .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="3220b-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="3220b-111">Im Entity Framework kann eine Enumeration der folgenden zugrunde liegende Typen aufweisen: **Byte**, **Int16**, **Int32**, **Int64** , oder **SByte**.</span><span class="sxs-lookup"><span data-stu-id="3220b-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="3220b-112">Video ansehen</span><span class="sxs-lookup"><span data-stu-id="3220b-112">Watch the Video</span></span>
<span data-ttu-id="3220b-113">Dieses Video zeigt, wie Sie mit der Enum-Typen mit dem Entity Framework Designer.</span><span class="sxs-lookup"><span data-stu-id="3220b-113">This video shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="3220b-114">Es wird veranschaulicht, wie Enumerationen in einer LINQ-Abfrage verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="3220b-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="3220b-115">**Präsentiert von**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="3220b-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="3220b-116">**Video**: [WMV](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span><span class="sxs-lookup"><span data-stu-id="3220b-116">**Video**: [WMV](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="3220b-117">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="3220b-117">Pre-Requisites</span></span>

<span data-ttu-id="3220b-118">Sie müssen Visual Studio 2012, Ultimate, Premium, Professional oder Web Express Edition installiert, um diese exemplarische Vorgehensweise abgeschlossen haben.</span><span class="sxs-lookup"><span data-stu-id="3220b-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="3220b-119">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="3220b-119">Set up the Project</span></span>

1.  <span data-ttu-id="3220b-120">Visual Studio 2012 öffnen</span><span class="sxs-lookup"><span data-stu-id="3220b-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="3220b-121">Auf der **Datei** Startmenü **neu**, und klicken Sie dann auf **Projekt**</span><span class="sxs-lookup"><span data-stu-id="3220b-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="3220b-122">Klicken Sie im linken Bereich auf **Visual C\#**, und wählen Sie dann die **Konsole** Vorlage</span><span class="sxs-lookup"><span data-stu-id="3220b-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="3220b-123">Geben Sie **EnumEFDesigner** als Namen für das Projekt und auf **OK**</span><span class="sxs-lookup"><span data-stu-id="3220b-123">Enter **EnumEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="3220b-124">Erstellen eines neuen Modells mit dem EF-Designer</span><span class="sxs-lookup"><span data-stu-id="3220b-124">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="3220b-125">Mit der rechten Maustaste des Projektnamen im Projektmappen-Explorer, zeigen Sie auf **hinzufügen**, und klicken Sie dann auf **neues Element**</span><span class="sxs-lookup"><span data-stu-id="3220b-125">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="3220b-126">Wählen Sie **Daten** aus dem linken Menü und wählen Sie dann **ADO.NET Entity Data Model** im Bereich Vorlagen</span><span class="sxs-lookup"><span data-stu-id="3220b-126">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="3220b-127">Geben Sie **EnumTestModel.edmx** für den Dateinamen ein, und klicken Sie dann auf **hinzufügen**</span><span class="sxs-lookup"><span data-stu-id="3220b-127">Enter **EnumTestModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="3220b-128">Wählen Sie auf der Seite Assistenten für Entity Data Model **leeres Modell** im Dialogfeld ' Modellinhalte auswählen '</span><span class="sxs-lookup"><span data-stu-id="3220b-128">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="3220b-129">Klicken Sie auf **Fertig stellen**</span><span class="sxs-lookup"><span data-stu-id="3220b-129">Click **Finish**</span></span>

<span data-ttu-id="3220b-130">Im Entity Designer, der bietet eine Entwurfsoberfläche zum Bearbeiten des Modells, wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="3220b-130">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="3220b-131">Der Assistent führt die folgenden Aktionen aus:</span><span class="sxs-lookup"><span data-stu-id="3220b-131">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="3220b-132">Generiert die EnumTestModel.edmx-Datei, die das konzeptionelle Modell, Speichermodell und die Zuordnung definiert.</span><span class="sxs-lookup"><span data-stu-id="3220b-132">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="3220b-133">Legt die Verarbeitung der Metadatenartefakte-Eigenschaft der EDMX-Datei zum Einbetten in Ausgabeassembly fest, damit die generierten Metadaten-Dateien in die Assembly eingebettet werden.</span><span class="sxs-lookup"><span data-stu-id="3220b-133">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="3220b-134">Fügt einen Verweis auf die folgenden Assemblys hinzu: EntityFramework System.ComponentModel.DataAnnotations und System.Data.Entity.</span><span class="sxs-lookup"><span data-stu-id="3220b-134">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="3220b-135">EnumTestModel.tt und EnumTestModel.Context.tt-Dateien erstellt, und klicken Sie unter der EDMX-Datei hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="3220b-135">Creates EnumTestModel.tt and EnumTestModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="3220b-136">Diese Dateien der T4-Vorlage generieren den Code, der definiert, die "DbContext" abgeleitet und POCO-Typen, die die Entitäten in der EDMX-Modell zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="3220b-136">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model.</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="3220b-137">Fügen Sie einen neuen Entitätstyp hinzu</span><span class="sxs-lookup"><span data-stu-id="3220b-137">Add a New Entity Type</span></span>

1.  <span data-ttu-id="3220b-138">Mit der rechten Maustaste in eines leeren Bereichs der Entwurfsoberfläche, wählen **hinzufügen –&gt; Entität**, das Dialogfeld "neue Entität" angezeigt wird</span><span class="sxs-lookup"><span data-stu-id="3220b-138">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="3220b-139">Geben Sie **Abteilung** für den Typ ein, und geben **"DepartmentID"** für den Namen "Key"-Eigenschaft wählen Sie in den Typ als **Int32**</span><span class="sxs-lookup"><span data-stu-id="3220b-139">Specify **Department** for the type name and specify **DepartmentID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="3220b-140">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="3220b-140">Click **OK**</span></span>
4.  <span data-ttu-id="3220b-141">Mit der rechten Maustaste in der Entitäts, und wählen Sie **Add New -&gt; Skalareigenschaft**</span><span class="sxs-lookup"><span data-stu-id="3220b-141">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="3220b-142">Benennen Sie die neue Eigenschaft **Name**</span><span class="sxs-lookup"><span data-stu-id="3220b-142">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="3220b-143">Ändern Sie den Typ der neuen Eigenschaft, die **Int32** (die neue Eigenschaft wird standardmäßig der Zeichenfolgentyp) um den Typ zu ändern, öffnen Sie das Fenster "Eigenschaften" aus, und ändern Sie die Type-Eigenschaft, um **Int32**</span><span class="sxs-lookup"><span data-stu-id="3220b-143">Change the type of the new property to **Int32** (by default, the new property is of String type) To change the type, open the Properties window and change the Type property to **Int32**</span></span>
7.  <span data-ttu-id="3220b-144">Fügen Sie eine andere skalare Eigenschaft hinzu und benennen Sie sie in **Budget**, ändern Sie den Typ in **Decimal**</span><span class="sxs-lookup"><span data-stu-id="3220b-144">Add another scalar property and rename it to **Budget**, change the type to **Decimal**</span></span>

## <a name="add-an-enum-type"></a><span data-ttu-id="3220b-145">Fügen Sie einen Enum-Typ</span><span class="sxs-lookup"><span data-stu-id="3220b-145">Add an Enum Type</span></span>

1.  <span data-ttu-id="3220b-146">Im Entity Framework Designer mit der Maustaste der Name-Eigenschaft, wählen Sie **in Enumeration konvertieren**</span><span class="sxs-lookup"><span data-stu-id="3220b-146">In the Entity Framework Designer, right-click the Name property, select **Convert to enum**</span></span>

    ![In Enumeration konvertieren](~/ef6/media/converttoenum.png)

2.  <span data-ttu-id="3220b-148">In der **Enumerator hinzufügen** Dialogfeldtyp **DepartmentNames** für den Enumerationstypnamen, ändern Sie den zugrunde liegenden Typ, **Int32**, und klicken Sie dann die folgenden Elemente in den Typ hinzufügen: Englisch, Mathematische und Wirtschaftlichkeit</span><span class="sxs-lookup"><span data-stu-id="3220b-148">In the **Add Enum** dialog box type **DepartmentNames** for the Enum Type Name, change the Underlying Type to **Int32**, and then add the following members to the type: English, Math, and Economics</span></span>

    ![Hinzufügen von Enum-Typ](~/ef6/media/addenumtype.png)

3.  <span data-ttu-id="3220b-150">Drücken Sie **OK**</span><span class="sxs-lookup"><span data-stu-id="3220b-150">Press **OK**</span></span>
4.  <span data-ttu-id="3220b-151">Speichern Sie das Modell, und erstellen Sie das Projekt</span><span class="sxs-lookup"><span data-stu-id="3220b-151">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="3220b-152">Bei der Erstellung können Warnungen zu nicht zugeordneten Entitäten und Zuordnungen in der Fehlerliste angezeigt.</span><span class="sxs-lookup"><span data-stu-id="3220b-152">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="3220b-153">Sie können diese Warnungen ignorieren, da nach dem wir die Datenbank aus dem Modell generieren möchten, die Fehler verschwinden werden.</span><span class="sxs-lookup"><span data-stu-id="3220b-153">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

<span data-ttu-id="3220b-154">Wenn Sie das Fenster "Eigenschaften" betrachten, werden Sie sehen, dass der Typ der Eigenschaft Name, um geändert wurde **DepartmentNames** und der neu hinzugefügten Enum-Typ wurde hinzugefügt, um die Liste der Typen.</span><span class="sxs-lookup"><span data-stu-id="3220b-154">If you look at the Properties window, you will notice that the type of the Name property was changed to **DepartmentNames** and the newly added enum type was added to the list of types.</span></span>

<span data-ttu-id="3220b-155">Wenn Sie in das Fenster "Modellbrowser" wechseln, sehen Sie sich, dass der Typ der Knoten Enum-Typen auch hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="3220b-155">If you switch to the Model Browser window, you will see that the type was also added to the Enum Types node.</span></span>

![Modellbrowser](~/ef6/media/modelbrowser.png)

>[!NOTE]
> <span data-ttu-id="3220b-157">Sie können auch neue Enum-Typen aus diesem Fenster hinzufügen, indem Sie auf der rechten Maustaste und auswählen **Enum-Typ hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="3220b-157">You can also add new enum types from this window by clicking the right mouse button and selecting **Add Enum Type**.</span></span> <span data-ttu-id="3220b-158">Wenn der Typ erstellt wird, es in der Liste der Typen angezeigt, und Sie würde in der Lage, eine Eigenschaft zugeordnet werden soll</span><span class="sxs-lookup"><span data-stu-id="3220b-158">Once the type is created it will appear in the list of types and you would be able to associate with a property</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="3220b-159">Datenbank aus Modell generieren</span><span class="sxs-lookup"><span data-stu-id="3220b-159">Generate Database from Model</span></span>

<span data-ttu-id="3220b-160">Jetzt können wir eine Datenbank generieren, die für das Modell basiert.</span><span class="sxs-lookup"><span data-stu-id="3220b-160">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="3220b-161">Mit der rechten Maustaste in eines leeren Bereich der Entity-Designer, und wählen **Datenbank aus Modell generieren**</span><span class="sxs-lookup"><span data-stu-id="3220b-161">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="3220b-162">Wählen Sie Ihre Daten Verbindungsdialogfeld des Assistenten zum Generieren von Datenbanken wird angezeigt, klicken Sie auf die **neue Verbindung** Schaltfläche geben **(Localdb)\\Mssqllocaldb** für den Servernamen und  **EnumTest** für die Datenbank und auf **OK**</span><span class="sxs-lookup"><span data-stu-id="3220b-162">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **EnumTest** for the database and click **OK**</span></span>
3.  <span data-ttu-id="3220b-163">Ein Dialogfeld gefragt, ob Sie eine neue Datenbank erstellen möchten, wird angezeigt, klicken Sie auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="3220b-163">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="3220b-164">Klicken Sie auf **Weiter** und der Assistent zur Datenbankgenerierung generiert die Datendefinitionssprache (DDL) für eine Datenbank erstellen, die generierte DDL angezeigt wird, in der Zusammenfassung und Einstellungen Dialogfeld das Beachten Sie, dass, die die DDL-Anweisungen keine Definition für enthält eine Tabelle, in den Enumerationstyp zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="3220b-164">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="3220b-165">Klicken Sie auf **Fertig stellen** durch Klicken auf Fertig stellen das DDL-Skript nicht ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="3220b-165">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="3220b-166">Der Assistent zur Datenbankgenerierung bewirkt Folgendes: Öffnet die **EnumTest.edmx.sql** in T-SQL-Editor generiert Datei in den Store-Schemas und der Mapping-Abschnitten, die Zielimplementierung des zu Informationen zur Verbindungszeichenfolge fügt die Datei "App.config"</span><span class="sxs-lookup"><span data-stu-id="3220b-166">The Create Database Wizard does the following: Opens the **EnumTest.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="3220b-167">Klicken Sie auf der rechten Maustaste im T-SQL-Editor, und wählen Sie **Execute** das Herstellen einer Verbindung mit Server-Dialogfeld angezeigt wird, geben Sie die Verbindungsinformationen aus Schritt 2, und klicken Sie auf **verbinden**</span><span class="sxs-lookup"><span data-stu-id="3220b-167">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="3220b-168">Klicken Sie zum Anzeigen des generierten Schemas mit der rechten Maustaste auf den Datenbanknamen im Objekt-Explorer von SQL Server, und wählen Sie **aktualisieren**</span><span class="sxs-lookup"><span data-stu-id="3220b-168">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="3220b-169">Speichern und Abrufen von Daten</span><span class="sxs-lookup"><span data-stu-id="3220b-169">Persist and Retrieve Data</span></span>

<span data-ttu-id="3220b-170">Öffnen Sie die Datei "Program.cs", die, in die Main-Methode definiert ist.</span><span class="sxs-lookup"><span data-stu-id="3220b-170">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="3220b-171">Fügen Sie der Main-Funktion mit den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="3220b-171">Add the following code into the Main function.</span></span> <span data-ttu-id="3220b-172">Der Code Fügt ein neues Objekt für die Abteilung, in den Kontext.</span><span class="sxs-lookup"><span data-stu-id="3220b-172">The code adds a new Department object to the context.</span></span> <span data-ttu-id="3220b-173">Klicken Sie dann die Daten gespeichert.</span><span class="sxs-lookup"><span data-stu-id="3220b-173">It then saves the data.</span></span> <span data-ttu-id="3220b-174">Der Code führt auch eine LINQ-Abfrage, die eine Abteilung zurückgibt, wobei der Name DepartmentNames.English ist.</span><span class="sxs-lookup"><span data-stu-id="3220b-174">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

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

<span data-ttu-id="3220b-175">Kompilieren Sie die Anwendung, und führen Sie sie aus.</span><span class="sxs-lookup"><span data-stu-id="3220b-175">Compile and run the application.</span></span> <span data-ttu-id="3220b-176">Das Programm erzeugt die folgende Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="3220b-176">The program produces the following output:</span></span>

```
DepartmentID: 1 Name: English
```

<span data-ttu-id="3220b-177">Klicken Sie zum Anzeigen von Daten in der Datenbank, mit der rechten Maustaste auf den Datenbanknamen im Objekt-Explorer von SQL Server, und wählen Sie **aktualisieren**.</span><span class="sxs-lookup"><span data-stu-id="3220b-177">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="3220b-178">Klicken Sie dann auf der rechten Maustaste auf die Tabelle, und wählen **Ansichtsdaten**.</span><span class="sxs-lookup"><span data-stu-id="3220b-178">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="3220b-179">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="3220b-179">Summary</span></span>

<span data-ttu-id="3220b-180">In dieser exemplarischen Vorgehensweise erläutert, wie Enum-Typen, die mit dem Entity Framework Designer zuordnen und Enumerationen in Code zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="3220b-180">In this walkthrough we looked at how to map enum types using the Entity Framework Designer and how to use enums in code.</span></span> 
