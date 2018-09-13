---
title: Designer TPH-Vererbung – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 43ba34a98c3960a7a3052a00e2ed2751c2f2b121
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490113"
---
# <a name="designer-tph-inheritance"></a><span data-ttu-id="635d5-102">Designer TPH-Vererbung</span><span class="sxs-lookup"><span data-stu-id="635d5-102">Designer TPH Inheritance</span></span>
<span data-ttu-id="635d5-103">Diese exemplarische Vorgehensweise veranschaulicht Tabelle pro Hierarchie (TPH)-Vererbung im konzeptionellen Modell mit dem Entity Framework Designer (EF-Designer) zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="635d5-103">This step-by-step walkthrough shows how to implement table-per-hierarchy (TPH) inheritance in your conceptual model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="635d5-104">TPH-Vererbung verwendet eine Datenbanktabelle, um die Daten aller Entitätstypen in einer Vererbungshierarchie zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="635d5-104">TPH inheritance uses one database table to maintain data for all of the entity types in an inheritance hierarchy.</span></span>

<span data-ttu-id="635d5-105">In dieser exemplarischen Vorgehensweise ordnen wir die Person-Tabelle auf drei Entitätstypen: Person (Basistyp), (abgeleitet von Person) "Student" und "Instructor" (von Person abgeleitet ist).</span><span class="sxs-lookup"><span data-stu-id="635d5-105">In this walkthrough we will map the Person table to three entity types: Person (the base type), Student (derives from Person), and Instructor (derives from Person).</span></span> <span data-ttu-id="635d5-106">Wir ein konzeptionelles Modell aus der Datenbank (Database First) erstellen und ändern Sie dann das Modell zur Implementierung der TPH-Vererbung mit dem EF Designer.</span><span class="sxs-lookup"><span data-stu-id="635d5-106">We'll create a conceptual model from the database (Database First) and then alter the model to implement the TPH inheritance using the EF Designer.</span></span>

<span data-ttu-id="635d5-107">Es ist möglich, eine TPH-Vererbung mit Model First zuzuordnen, jedoch müssen Sie Ihren eigenen Workflow zur datenbankgenerierung schreiben, der komplex ist.</span><span class="sxs-lookup"><span data-stu-id="635d5-107">It is possible to map to a TPH inheritance using Model First but you would have to write your own database generation workflow which is complex.</span></span> <span data-ttu-id="635d5-108">Sie würden diesen Workflow Zuweisen der **Workflow zur Datenbankgenerierung** Eigenschaft im EF Designer.</span><span class="sxs-lookup"><span data-stu-id="635d5-108">You would then assign this workflow to the **Database Generation Workflow** property in the EF Designer.</span></span> <span data-ttu-id="635d5-109">Eine einfachere Alternative ist mit Code First.</span><span class="sxs-lookup"><span data-stu-id="635d5-109">An easier alternative is to use Code First.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="635d5-110">Andere Vererbungsoptionen</span><span class="sxs-lookup"><span data-stu-id="635d5-110">Other Inheritance Options</span></span>

<span data-ttu-id="635d5-111">Tabelle pro Typ (TPT) ist eine andere Art von Vererbung in der separate Tabellen in der Datenbank Entitäten zugeordnet werden, die in der Vererbung beteiligt sind.</span><span class="sxs-lookup"><span data-stu-id="635d5-111">Table-per-Type (TPT) is another type of inheritance in which separate tables in the database are mapped to entities that participate in the inheritance.</span></span>  <span data-ttu-id="635d5-112">Informationen dazu, wie Sie eine Tabelle pro Typ Vererbung mit dem EF Designer zuordnen, finden Sie unter [EF Designer TPT-Vererbung](~/ef6/modeling/designer/inheritance/tpt.md).</span><span class="sxs-lookup"><span data-stu-id="635d5-112">For information about how to map Table-per-Type inheritance with the EF Designer, see [EF Designer TPT Inheritance](~/ef6/modeling/designer/inheritance/tpt.md).</span></span>

<span data-ttu-id="635d5-113">Tabelle pro konkretem Typ '-Vererbung (TPC) und gemischte vererbungsmodellen werden von der Entity Framework-Laufzeit unterstützt, jedoch werden von dem EF Designer nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="635d5-113">Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="635d5-114">Wenn Sie die TPC oder gemischte Vererbung verwenden möchten, haben Sie zwei Optionen: Code First verwenden, oder die EDMX-Datei manuell bearbeiten.</span><span class="sxs-lookup"><span data-stu-id="635d5-114">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="635d5-115">Wenn Sie mit der EDMX-Datei arbeiten möchten, Detailfenster Zuordnung wird in "abgesicherten Modus" versetzt werden, und Sie werden nicht in der Lage, den Designer zu verwenden, um die Zuordnungen ändern.</span><span class="sxs-lookup"><span data-stu-id="635d5-115">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="635d5-116">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="635d5-116">Prerequisites</span></span>

<span data-ttu-id="635d5-117">Um die exemplarische Vorgehensweise nachzuvollziehen, benötigen Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="635d5-117">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="635d5-118">Eine aktuelle Version von Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="635d5-118">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="635d5-119">Die [Beispieldatenbank ' School '](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="635d5-119">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="635d5-120">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="635d5-120">Set up the Project</span></span>

-   <span data-ttu-id="635d5-121">Öffnen Sie Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="635d5-121">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="635d5-122">Wählen Sie **Dateien&gt; neu –&gt; Projekt**</span><span class="sxs-lookup"><span data-stu-id="635d5-122">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="635d5-123">Klicken Sie im linken Bereich auf **Visual C\#**, und wählen Sie dann die **Konsole** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="635d5-123">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="635d5-124">Geben Sie **TPHDBFirstSample** als Namen.</span><span class="sxs-lookup"><span data-stu-id="635d5-124">Enter **TPHDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="635d5-125">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="635d5-125">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="635d5-126">Erstellen eines Modells</span><span class="sxs-lookup"><span data-stu-id="635d5-126">Create a Model</span></span>

-   <span data-ttu-id="635d5-127">Mit der rechten Maustaste des Projektnamen im Projektmappen-Explorer, und wählen **hinzufügen –&gt; neues Element**.</span><span class="sxs-lookup"><span data-stu-id="635d5-127">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="635d5-128">Wählen Sie **Daten** aus dem linken Menü und wählen Sie dann **ADO.NET Entity Data Model** im Bereich Vorlagen.</span><span class="sxs-lookup"><span data-stu-id="635d5-128">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="635d5-129">Geben Sie **TPHModel.edmx** für den Dateinamen ein, und klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="635d5-129">Enter **TPHModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="635d5-130">Wählen Sie im Dialogfeld "Modellinhalte" **aus Datenbank generieren**, und klicken Sie dann auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="635d5-130">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="635d5-131">Klicken Sie auf **neue Verbindung**.</span><span class="sxs-lookup"><span data-stu-id="635d5-131">Click **New Connection**.</span></span>
    <span data-ttu-id="635d5-132">Geben Sie den Servernamen, klicken Sie im Dialogfeld "Verbindungseigenschaften" (z. B. **(Localdb)\\Mssqllocaldb**), wählen die Authentifizierungsmethode, Typ **School** für den Datenbanknamen, und klicken Sie dann Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="635d5-132">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="635d5-133">Das Dialogfeld "Wählen Sie Ihre Datenverbindung" wird mit Ihrem datenbankverbindungseinstellung aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="635d5-133">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="635d5-134">Wählen Sie in das Dialogfeld Datenbankobjekte auswählen, unter dem Knoten Tabellen der **Person** Tabelle.</span><span class="sxs-lookup"><span data-stu-id="635d5-134">In the Choose Your Database Objects dialog box, under the Tables node, select the **Person** table.</span></span>
-   <span data-ttu-id="635d5-135">Klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="635d5-135">Click **Finish**.</span></span>

<span data-ttu-id="635d5-136">Im Entity Designer, der bietet eine Entwurfsoberfläche zum Bearbeiten des Modells, wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="635d5-136">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="635d5-137">Alle Objekte, die Sie im Dialogfeld "Datenbankobjekte auswählen" ausgewählt werden mit dem Modell hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="635d5-137">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

<span data-ttu-id="635d5-138">D. h. wie die **Person** Tabelle sieht einwandfrei aus, in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="635d5-138">That is how the **Person** table looks in the database.</span></span>

![Person-Tabelle](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a><span data-ttu-id="635d5-140">Implementieren Sie die Tabelle pro Hierarchie-Vererbung</span><span class="sxs-lookup"><span data-stu-id="635d5-140">Implement Table-per-Hierarchy Inheritance</span></span>

<span data-ttu-id="635d5-141">Die **Person** Tabelle besitzt die **Diskriminator** Spalte, die einen von zwei Werten haben kann: "Student" und "Instructor".</span><span class="sxs-lookup"><span data-stu-id="635d5-141">The **Person** table has the **Discriminator** column, which can have one of two values: “Student” and “Instructor”.</span></span> <span data-ttu-id="635d5-142">Abhängig vom Wert der **Person** Tabelle zugeordnet der **für Schüler und Studenten** Entität oder die **"Instructor"** Entität.</span><span class="sxs-lookup"><span data-stu-id="635d5-142">Depending on the value the **Person** table will be mapped to the **Student** entity or the **Instructor** entity.</span></span> <span data-ttu-id="635d5-143">Die **Person** Tabelle besitzt außerdem zwei Spalten, **HireDate** und **EnrollmentDate**, muss **auf NULL festlegbare** , da eine Person sein, kann kein "Student" und einen Dozenten zur gleichen Zeit (zumindest nicht in dieser exemplarischen Vorgehensweise).</span><span class="sxs-lookup"><span data-stu-id="635d5-143">The **Person** table also has two columns, **HireDate** and **EnrollmentDate**, which must be **nullable** because a person cannot be a student and an instructor at the same time (at least not in this walkthrough).</span></span>

### <a name="add-new-entities"></a><span data-ttu-id="635d5-144">Fügen Sie neue Entitäten hinzu</span><span class="sxs-lookup"><span data-stu-id="635d5-144">Add new Entities</span></span>

-   <span data-ttu-id="635d5-145">Fügen Sie eine neue Entität hinzu.</span><span class="sxs-lookup"><span data-stu-id="635d5-145">Add a new entity.</span></span>
    <span data-ttu-id="635d5-146">Zu diesem Zweck mit der rechten Maustaste auf einen leeren Bereich der Entwurfsoberfläche des Entity Framework Designer, und wählen **Add-&gt;Entität**.</span><span class="sxs-lookup"><span data-stu-id="635d5-146">To do this, right-click on an empty space of the design surface of the Entity Framework Designer, and select **Add-&gt;Entity**.</span></span>
-   <span data-ttu-id="635d5-147">Typ **"Instructor"** für die **Entitätsname** , und wählen Sie **Person** aus der Dropdown-Liste für die **Basistyp**.</span><span class="sxs-lookup"><span data-stu-id="635d5-147">Type **Instructor** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>
-   <span data-ttu-id="635d5-148">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="635d5-148">Click **OK**.</span></span>
-   <span data-ttu-id="635d5-149">Fügen Sie eine andere neue Entität hinzu.</span><span class="sxs-lookup"><span data-stu-id="635d5-149">Add another new entity.</span></span> <span data-ttu-id="635d5-150">Typ **für Schüler und Studenten** für die **Entitätsname** , und wählen Sie **Person** aus der Dropdown-Liste für die **Basistyp**.</span><span class="sxs-lookup"><span data-stu-id="635d5-150">Type **Student** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>

<span data-ttu-id="635d5-151">Die Entwurfsoberfläche wurden zwei neue Entitätstypen hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="635d5-151">Two new entity types were added to the design surface.</span></span> <span data-ttu-id="635d5-152">Ein Pfeil zeigt in der neuen Entitätstypen auf den **Person** Entitätstyp; Dies bedeutet, dass **Person** ist der Basistyp für den neuen Entitätstypen.</span><span class="sxs-lookup"><span data-stu-id="635d5-152">An arrow points from the new entity types to the **Person** entity type; this indicates that **Person** is the base type for the new entity types.</span></span>

-   <span data-ttu-id="635d5-153">Mit der rechten Maustaste die **HireDate** Eigenschaft der **Person** Entität.</span><span class="sxs-lookup"><span data-stu-id="635d5-153">Right-click the **HireDate** property of the **Person** entity.</span></span> <span data-ttu-id="635d5-154">Wählen Sie **Ausschneiden** (oder verwenden Sie die Taste STRG + X).</span><span class="sxs-lookup"><span data-stu-id="635d5-154">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="635d5-155">Mit der rechten Maustaste die **"Instructor"** Entität, und wählen **einfügen** (oder verwenden Sie die Taste STRG + V).</span><span class="sxs-lookup"><span data-stu-id="635d5-155">Right-click the **Instructor** entity and select **Paste** (or use the Ctrl-V key).</span></span>
-   <span data-ttu-id="635d5-156">Mit der rechten Maustaste die **HireDate** Eigenschaft, und wählen **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="635d5-156">Right-click the **HireDate** property and select **Properties**.</span></span>
-   <span data-ttu-id="635d5-157">In der **Eigenschaften** legen die **Nullable** Eigenschaft **"false"**.</span><span class="sxs-lookup"><span data-stu-id="635d5-157">In the **Properties** window, set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="635d5-158">Mit der rechten Maustaste die **EnrollmentDate** Eigenschaft der **Person** Entität.</span><span class="sxs-lookup"><span data-stu-id="635d5-158">Right-click the **EnrollmentDate** property of the **Person** entity.</span></span> <span data-ttu-id="635d5-159">Wählen Sie **Ausschneiden** (oder verwenden Sie die Taste STRG + X).</span><span class="sxs-lookup"><span data-stu-id="635d5-159">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="635d5-160">Mit der rechten Maustaste die **für Schüler und Studenten** Entität, und wählen **einfügen (oder Schlüssel verwenden Sie die STRG + V).**</span><span class="sxs-lookup"><span data-stu-id="635d5-160">Right-click the **Student** entity and select **Paste(or use the Ctrl-V key).**</span></span>
-   <span data-ttu-id="635d5-161">Wählen Sie die **EnrollmentDate** Eigenschaft, und legen die **Nullable** Eigenschaft **"false"**.</span><span class="sxs-lookup"><span data-stu-id="635d5-161">Select the **EnrollmentDate** property and set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="635d5-162">Wählen Sie die **Person** Entitätstyp.</span><span class="sxs-lookup"><span data-stu-id="635d5-162">Select the **Person** entity type.</span></span> <span data-ttu-id="635d5-163">In der **Eigenschaften** legen die **abstrakte** Eigenschaft **"true"**.</span><span class="sxs-lookup"><span data-stu-id="635d5-163">In the **Properties** window, set its **Abstract** property to **true**.</span></span>
-   <span data-ttu-id="635d5-164">Löschen der **Diskriminator** Eigenschaft **Person**.</span><span class="sxs-lookup"><span data-stu-id="635d5-164">Delete the **Discriminator** property from **Person**.</span></span> <span data-ttu-id="635d5-165">Der Grund, die, den es gelöscht werden soll, wird im folgenden Abschnitt erläutert.</span><span class="sxs-lookup"><span data-stu-id="635d5-165">The reason it should be deleted is explained in the following section.</span></span>

### <a name="map-the-entities"></a><span data-ttu-id="635d5-166">Ordnen Sie die Entitäten</span><span class="sxs-lookup"><span data-stu-id="635d5-166">Map the entities</span></span>

-   <span data-ttu-id="635d5-167">Mit der rechten Maustaste die **"Instructor"** , und wählen Sie **Tabellenzuordnung.**</span><span class="sxs-lookup"><span data-stu-id="635d5-167">Right-click the **Instructor** and select **Table Mapping.**</span></span>
    <span data-ttu-id="635d5-168">Die Instructor-Entität, die in das Fenster "Mappingdetails" ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="635d5-168">The Instructor entity is selected in the Mapping Details window.</span></span>
-   <span data-ttu-id="635d5-169">Klicken Sie auf **&lt;Hinzufügen einer Tabelle oder Sicht&gt;** in die **Mappingdetails** Fenster.</span><span class="sxs-lookup"><span data-stu-id="635d5-169">Click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
    <span data-ttu-id="635d5-170">Die **&lt;Hinzufügen einer Tabelle oder Sicht&gt;** Feld wird eine Dropdown-Liste von Tabellen oder Sichten, die die ausgewählte Entität zugeordnet werden können.</span><span class="sxs-lookup"><span data-stu-id="635d5-170">The **&lt;Add a Table or View&gt;** field becomes a drop-down list of tables or views to which the selected entity can be mapped.</span></span>
-   <span data-ttu-id="635d5-171">Wählen Sie **Person** aus der Dropdown-Liste.</span><span class="sxs-lookup"><span data-stu-id="635d5-171">Select **Person** from the drop-down list.</span></span>
-   <span data-ttu-id="635d5-172">Die **Mappingdetails** Fenster wird mit Standardspaltenmappings und einer Option zum Hinzufügen einer Bedingung aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="635d5-172">The **Mapping Details** window is updated with default column mappings and an option for adding a condition.</span></span>
-   <span data-ttu-id="635d5-173">Klicken Sie auf  **&lt;Hinzufügen einer Bedingung&gt;**.</span><span class="sxs-lookup"><span data-stu-id="635d5-173">Click on **&lt;Add a Condition&gt;**.</span></span>
    <span data-ttu-id="635d5-174">Die **&lt;Hinzufügen einer Bedingung&gt;** Feld wird eine Dropdown-Liste der Spalten, die für die Bedingungen festgelegt werden können.</span><span class="sxs-lookup"><span data-stu-id="635d5-174">The **&lt;Add a Condition&gt;** field becomes a drop-down list of columns for which conditions can be set.</span></span>
-   <span data-ttu-id="635d5-175">Wählen Sie **Diskriminator** aus der Dropdown-Liste.</span><span class="sxs-lookup"><span data-stu-id="635d5-175">Select **Discriminator** from the drop-down list.</span></span>
-   <span data-ttu-id="635d5-176">In der **Operator** Spalte die **Mappingdetails** wählen Sie im Fenster = aus der Dropdown-Liste.</span><span class="sxs-lookup"><span data-stu-id="635d5-176">In the **Operator** column of the **Mapping Details** window, select = from the drop-down list.</span></span>
-   <span data-ttu-id="635d5-177">In der **Wert/Eigenschaft** Spalte, Datentyp **"Instructor"**.</span><span class="sxs-lookup"><span data-stu-id="635d5-177">In the **Value/Property** column, type **Instructor**.</span></span> <span data-ttu-id="635d5-178">Das Endergebnis sollte wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="635d5-178">The end result should look like this:</span></span>

    ![Zuordnungsdetails](~/ef6/media/mappingdetails2.png)

-   <span data-ttu-id="635d5-180">Wiederholen Sie diese Schritte für die **für Schüler und Studenten** Entitätstyp, aber stellen die Bedingung gleich **für Schüler und Studenten** Wert.</span><span class="sxs-lookup"><span data-stu-id="635d5-180">Repeat these steps for the **Student** entity type, but make the condition equal to **Student** value.</span></span>  
    <span data-ttu-id="635d5-181">*Der Grund, wollten wir entfernen, das **Diskriminator** -Eigenschaft ist, da Sie eine Spalte mehrmals zugeordnet werden können. Diese Spalte wird für die bedingten Zuordnung verwendet werden, daher kann nicht für die auch eigenschaftenzuordnung verwendet werden. Die einzige Möglichkeit, es kann für beide verwendet werden, wenn eine Bedingung verwendet eine **Is Null** oder **Is Not Null** Vergleich.*</span><span class="sxs-lookup"><span data-stu-id="635d5-181">*The reason we wanted to remove the **Discriminator** property, is because you cannot map a table column more than once. This column will be used for conditional mapping, so it cannot be used for property mapping as well. The only way it can be used for both, if a condition uses an **Is Null** or **Is Not Null** comparison.*</span></span>

<span data-ttu-id="635d5-182">Die "Tabelle pro Hierarchie"-Vererbung ist jetzt implementiert.</span><span class="sxs-lookup"><span data-stu-id="635d5-182">Table-per-hierarchy inheritance is now implemented.</span></span>

![Letzte TPH](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a><span data-ttu-id="635d5-184">Verwenden Sie das Modell</span><span class="sxs-lookup"><span data-stu-id="635d5-184">Use the Model</span></span>

<span data-ttu-id="635d5-185">Öffnen der **"Program.cs"** -Datei, in der **Main** Methode definiert ist.</span><span class="sxs-lookup"><span data-stu-id="635d5-185">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="635d5-186">Fügen Sie den folgenden Code in die **Main** Funktion.</span><span class="sxs-lookup"><span data-stu-id="635d5-186">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="635d5-187">Der Code ausgeführt wird, drei Abfragen.</span><span class="sxs-lookup"><span data-stu-id="635d5-187">The code executes three queries.</span></span> <span data-ttu-id="635d5-188">Die erste Abfrage gibt wieder alle **Person** Objekte.</span><span class="sxs-lookup"><span data-stu-id="635d5-188">The first query brings back all **Person** objects.</span></span> <span data-ttu-id="635d5-189">Die zweite Abfrage verwendet die **OfType** -Methode zur Rückgabe **"Instructor"** Objekte.</span><span class="sxs-lookup"><span data-stu-id="635d5-189">The second query uses the **OfType** method to return **Instructor** objects.</span></span> <span data-ttu-id="635d5-190">Die dritte Abfrage verwendet die **OfType** -Methode zur Rückgabe **für Schüler und Studenten** Objekte.</span><span class="sxs-lookup"><span data-stu-id="635d5-190">The third query uses the **OfType** method to return **Student** objects.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        Console.WriteLine("All people:");
        foreach (var person in context.People)
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Instructors only: ");
        foreach (var person in context.People.OfType<Instructor>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Students only: ");
        foreach (var person in context.People.OfType<Student>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }
    }
```
