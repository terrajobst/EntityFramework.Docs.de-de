---
title: Designer-TPH-Vererbung-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 43ba34a98c3960a7a3052a00e2ed2751c2f2b121
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415232"
---
# <a name="designer-tph-inheritance"></a><span data-ttu-id="cea6f-102">Designer-TPH-Vererbung</span><span class="sxs-lookup"><span data-stu-id="cea6f-102">Designer TPH Inheritance</span></span>
<span data-ttu-id="cea6f-103">Diese schrittweise exemplarische Vorgehensweise veranschaulicht, wie Sie die "Tabelle pro Hierarchie"-Vererbung (TPH) im konzeptionellen Modell mit dem Entity Framework Designer (EF-Designer) implementieren.</span><span class="sxs-lookup"><span data-stu-id="cea6f-103">This step-by-step walkthrough shows how to implement table-per-hierarchy (TPH) inheritance in your conceptual model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="cea6f-104">Die TPH-Vererbung verwendet eine Datenbanktabelle, um Daten für alle Entitäts Typen in einer Vererbungs Hierarchie zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="cea6f-104">TPH inheritance uses one database table to maintain data for all of the entity types in an inheritance hierarchy.</span></span>

<span data-ttu-id="cea6f-105">In dieser exemplarischen Vorgehensweise ordnen wir die Person-Tabelle drei Entitäts Typen zu: Person (der Basistyp), Student (abgeleitet von Person) und Instructor (abgeleitet von Person).</span><span class="sxs-lookup"><span data-stu-id="cea6f-105">In this walkthrough we will map the Person table to three entity types: Person (the base type), Student (derives from Person), and Instructor (derives from Person).</span></span> <span data-ttu-id="cea6f-106">Wir erstellen ein konzeptionelles Modell aus der Datenbank (Database First) und ändern dann das Modell, um die TPH-Vererbung mit dem EF-Designer zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="cea6f-106">We'll create a conceptual model from the database (Database First) and then alter the model to implement the TPH inheritance using the EF Designer.</span></span>

<span data-ttu-id="cea6f-107">Es ist möglich, eine TPH-Vererbung mithilfe Model First zuzuordnen, aber Sie müssen einen komplexen Daten Bank Generierungs Workflow schreiben, der Komplex ist.</span><span class="sxs-lookup"><span data-stu-id="cea6f-107">It is possible to map to a TPH inheritance using Model First but you would have to write your own database generation workflow which is complex.</span></span> <span data-ttu-id="cea6f-108">Anschließend weisen Sie diesen Workflow der Eigenschaft **Daten Bank Generierungs Workflow** im EF-Designer zu.</span><span class="sxs-lookup"><span data-stu-id="cea6f-108">You would then assign this workflow to the **Database Generation Workflow** property in the EF Designer.</span></span> <span data-ttu-id="cea6f-109">Eine einfachere Alternative ist die Verwendung von Code First.</span><span class="sxs-lookup"><span data-stu-id="cea6f-109">An easier alternative is to use Code First.</span></span>

## <a name="other-inheritance-options"></a><span data-ttu-id="cea6f-110">Andere Vererbungs Optionen</span><span class="sxs-lookup"><span data-stu-id="cea6f-110">Other Inheritance Options</span></span>

<span data-ttu-id="cea6f-111">Tabelle pro Typ (TPT) ist eine andere Art von Vererbung, bei der separate Tabellen in der Datenbank Entitäten zugeordnet werden, die an der Vererbung teilnehmen.</span><span class="sxs-lookup"><span data-stu-id="cea6f-111">Table-per-Type (TPT) is another type of inheritance in which separate tables in the database are mapped to entities that participate in the inheritance.</span></span> <span data-ttu-id="cea6f-112"> Informationen zum Zuordnen der "Tabelle pro Typ"-Vererbung mit dem EF-Designer finden Sie unter [EF-Designer-TPT-Vererbung](~/ef6/modeling/designer/inheritance/tpt.md).</span><span class="sxs-lookup"><span data-stu-id="cea6f-112"> For information about how to map Table-per-Type inheritance with the EF Designer, see [EF Designer TPT Inheritance](~/ef6/modeling/designer/inheritance/tpt.md).</span></span>

<span data-ttu-id="cea6f-113">"Tabelle pro konkrete Typvererbung" (TPC) und gemischte Vererbungs Modelle werden von der Entity Framework Runtime unterstützt, jedoch nicht vom EF-Designer unterstützt.</span><span class="sxs-lookup"><span data-stu-id="cea6f-113">Table-per-Concrete Type Inheritance (TPC) and mixed inheritance models are supported by the Entity Framework runtime but are not supported by the EF Designer.</span></span> <span data-ttu-id="cea6f-114">Wenn Sie TPC oder gemischte Vererbung verwenden möchten, haben Sie zwei Möglichkeiten: Verwenden Sie Code First, oder bearbeiten Sie die EDMX-Datei manuell.</span><span class="sxs-lookup"><span data-stu-id="cea6f-114">If you want to use TPC or mixed inheritance, you have two options: use Code First, or manually edit the EDMX file.</span></span> <span data-ttu-id="cea6f-115">Wenn Sie sich für die Arbeit mit der EDMX-Datei entscheiden, wird das Fenster Mappingdetails in den "abgesicherten Modus" versetzt, und Sie können den Designer nicht verwenden, um die Zuordnungen zu ändern.</span><span class="sxs-lookup"><span data-stu-id="cea6f-115">If you choose to work with the EDMX file, the Mapping Details Window will be put into “safe mode” and you will not be able to use the designer to change the mappings.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cea6f-116">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="cea6f-116">Prerequisites</span></span>

<span data-ttu-id="cea6f-117">Um die exemplarische Vorgehensweise nachzuvollziehen, benötigen Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="cea6f-117">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="cea6f-118">Eine aktuelle Version von Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cea6f-118">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="cea6f-119">Die [Beispieldatenbank "School](~/ef6/resources/school-database.md)".</span><span class="sxs-lookup"><span data-stu-id="cea6f-119">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="cea6f-120">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="cea6f-120">Set up the Project</span></span>

-   <span data-ttu-id="cea6f-121">Öffnen Sie Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="cea6f-121">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="cea6f-122">**Datei&gt; neues&gt; Projekt** auswählen</span><span class="sxs-lookup"><span data-stu-id="cea6f-122">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="cea6f-123">Klicken Sie im linken Bereich auf **Visual C-\#** , und wählen Sie dann die **Konsolen** Vorlage aus.</span><span class="sxs-lookup"><span data-stu-id="cea6f-123">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="cea6f-124">Geben Sie **tphdbfirstsample** als Namen ein.</span><span class="sxs-lookup"><span data-stu-id="cea6f-124">Enter **TPHDBFirstSample** as the name.</span></span>
-   <span data-ttu-id="cea6f-125">Wählen Sie **OK** aus.</span><span class="sxs-lookup"><span data-stu-id="cea6f-125">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="cea6f-126">Modellerstellung</span><span class="sxs-lookup"><span data-stu-id="cea6f-126">Create a Model</span></span>

-   <span data-ttu-id="cea6f-127">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, und wählen Sie **Hinzufügen-&gt; neues Element**aus.</span><span class="sxs-lookup"><span data-stu-id="cea6f-127">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="cea6f-128">Wählen Sie im linken Menü **Daten** aus, und wählen Sie dann im Bereich Vorlagen die Option **ADO.NET Entity Data Model** aus.</span><span class="sxs-lookup"><span data-stu-id="cea6f-128">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="cea6f-129">Geben Sie als Dateiname **tphmodel. edmx** ein, und klicken Sie dann auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="cea6f-129">Enter **TPHModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="cea6f-130">Wählen Sie im Dialogfeld Modell Inhalte auswählen die Option **aus Datenbank generieren aus**, und klicken Sie dann auf **weiter**.</span><span class="sxs-lookup"><span data-stu-id="cea6f-130">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="cea6f-131">Klicken Sie auf **neue Verbindung**.</span><span class="sxs-lookup"><span data-stu-id="cea6f-131">Click **New Connection**.</span></span>
    <span data-ttu-id="cea6f-132">Geben Sie im Dialogfeld Verbindungs Eigenschaften den Servernamen ein (z. b. **(localdb)\\mssqllocaldb**), wählen Sie die Authentifizierungsmethode aus, geben Sie **School** als Datenbanknamen ein, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="cea6f-132">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="cea6f-133">Das Dialogfeld Wählen Sie Ihre Datenverbindung aus wird mit Ihrer Daten bankverbindungs Einstellung aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="cea6f-133">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="cea6f-134">Wählen Sie im Dialogfeld Datenbankobjekte auswählen unter dem Knoten Tabellen die **Person** -Tabelle aus.</span><span class="sxs-lookup"><span data-stu-id="cea6f-134">In the Choose Your Database Objects dialog box, under the Tables node, select the **Person** table.</span></span>
-   <span data-ttu-id="cea6f-135">Klicken Sie auf **Finish**.</span><span class="sxs-lookup"><span data-stu-id="cea6f-135">Click **Finish**.</span></span>

<span data-ttu-id="cea6f-136">Die Entity Designer, die eine Entwurfs Oberfläche zum Bearbeiten des Modells bereitstellt, wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="cea6f-136">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="cea6f-137">Alle Objekte, die Sie im Dialogfeld Wählen Sie Ihre Datenbankobjekte ausgewählt haben, werden dem Modell hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="cea6f-137">All the objects that you selected in the Choose Your Database Objects dialog box are added to the model.</span></span>

<span data-ttu-id="cea6f-138">So sucht die **Person** -Tabelle in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="cea6f-138">That is how the **Person** table looks in the database.</span></span>

![Person-Tabelle](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a><span data-ttu-id="cea6f-140">Implementieren der "Tabelle pro Hierarchie"-Vererbung</span><span class="sxs-lookup"><span data-stu-id="cea6f-140">Implement Table-per-Hierarchy Inheritance</span></span>

<span data-ttu-id="cea6f-141">Die **Person** -Tabelle verfügt über die **diskriminatorspalte** , die einen von zwei Werten aufweisen kann: "Student" und "Instructor".</span><span class="sxs-lookup"><span data-stu-id="cea6f-141">The **Person** table has the **Discriminator** column, which can have one of two values: “Student” and “Instructor”.</span></span> <span data-ttu-id="cea6f-142">Abhängig vom Wert wird die **Person** -Tabelle der **Student** -Entität oder der **Instructor** -Entität zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="cea6f-142">Depending on the value the **Person** table will be mapped to the **Student** entity or the **Instructor** entity.</span></span> <span data-ttu-id="cea6f-143">Die **Person** -Tabelle verfügt auch über zwei Spalten, **HireDate** und **registrimentdate**, die **NULL-Werte** zulassen müssen, da eine Person nicht gleichzeitig Student und Dozenten sein kann (zumindest nicht in dieser exemplarischen Vorgehensweise).</span><span class="sxs-lookup"><span data-stu-id="cea6f-143">The **Person** table also has two columns, **HireDate** and **EnrollmentDate**, which must be **nullable** because a person cannot be a student and an instructor at the same time (at least not in this walkthrough).</span></span>

### <a name="add-new-entities"></a><span data-ttu-id="cea6f-144">Neue Entitäten hinzufügen</span><span class="sxs-lookup"><span data-stu-id="cea6f-144">Add new Entities</span></span>

-   <span data-ttu-id="cea6f-145">Fügen Sie eine neue Entität hinzu.</span><span class="sxs-lookup"><span data-stu-id="cea6f-145">Add a new entity.</span></span>
    <span data-ttu-id="cea6f-146">Klicken Sie dazu mit der rechten Maustaste auf einen leeren Bereich der Entwurfs Oberfläche der Entity Framework Designer, und wählen Sie **Add-&gt;Entity**aus.</span><span class="sxs-lookup"><span data-stu-id="cea6f-146">To do this, right-click on an empty space of the design surface of the Entity Framework Designer, and select **Add-&gt;Entity**.</span></span>
-   <span data-ttu-id="cea6f-147">Geben Sie **Instructor** als **Entitäts Name** ein, und wählen Sie in der Dropdown Liste für den **Basistyp**die Option  **Person** aus.</span><span class="sxs-lookup"><span data-stu-id="cea6f-147">Type **Instructor** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>
-   <span data-ttu-id="cea6f-148">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="cea6f-148">Click **OK**.</span></span>
-   <span data-ttu-id="cea6f-149">Fügen Sie eine neue Entität hinzu.</span><span class="sxs-lookup"><span data-stu-id="cea6f-149">Add another new entity.</span></span> <span data-ttu-id="cea6f-150">Geben Sie **Student** als **Entitäts Name** ein, und wählen Sie in der Dropdown Liste für den **Basistyp**die Option  **Person** aus.</span><span class="sxs-lookup"><span data-stu-id="cea6f-150">Type **Student** for the **Entity name** and select **Person** from the drop-down list for the **Base type**.</span></span>

<span data-ttu-id="cea6f-151">Der Entwurfs Oberfläche wurden zwei neue Entitäts Typen hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="cea6f-151">Two new entity types were added to the design surface.</span></span> <span data-ttu-id="cea6f-152">Ein Pfeil zeigt von den neuen Entitäts Typen auf den Entitätstyp **Person** ; Dies gibt an, dass die **Person** der Basistyp für die neuen Entitäts Typen ist.</span><span class="sxs-lookup"><span data-stu-id="cea6f-152">An arrow points from the new entity types to the **Person** entity type; this indicates that **Person** is the base type for the new entity types.</span></span>

-   <span data-ttu-id="cea6f-153">Klicken Sie mit der rechten Maustaste auf das **HireDate** -Eigenschaft der **Person** Entität.</span><span class="sxs-lookup"><span data-stu-id="cea6f-153">Right-click the **HireDate** property of the **Person** entity.</span></span> <span data-ttu-id="cea6f-154">Wählen Sie **Ausschneiden** (oder drücken Sie STRG + X).</span><span class="sxs-lookup"><span data-stu-id="cea6f-154">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="cea6f-155">Klicken Sie mit der rechten Maustaste auf den **Dozenten** Entität, und wählen Sie **Einfügen** (oder drücken Sie STRG + V).</span><span class="sxs-lookup"><span data-stu-id="cea6f-155">Right-click the **Instructor** entity and select **Paste** (or use the Ctrl-V key).</span></span>
-   <span data-ttu-id="cea6f-156">Klicken Sie mit der rechten Maustaste auf die Eigenschaft **HireDate** und wählen Sie **Eigenschaften**aus.</span><span class="sxs-lookup"><span data-stu-id="cea6f-156">Right-click the **HireDate** property and select **Properties**.</span></span>
-   <span data-ttu-id="cea6f-157">Legen Sie im Fenster **Eigenschaften** die Eigenschaft , die **NULL-Werte** zulässt, auf **false**fest.</span><span class="sxs-lookup"><span data-stu-id="cea6f-157">In the **Properties** window, set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="cea6f-158">Klicken Sie mit der rechten Maustaste auf die Eigenschaft **registrimentdate** der Entität **Person** .</span><span class="sxs-lookup"><span data-stu-id="cea6f-158">Right-click the **EnrollmentDate** property of the **Person** entity.</span></span> <span data-ttu-id="cea6f-159">Wählen Sie **Ausschneiden** (oder drücken Sie STRG + X).</span><span class="sxs-lookup"><span data-stu-id="cea6f-159">Select **Cut** (or use the Ctrl-X key).</span></span>
-   <span data-ttu-id="cea6f-160">Klicken Sie mit der rechten Maustaste auf die Entität **Student** , und wählen Sie **Einfügen aus (oder drücken Sie STRG + V).**</span><span class="sxs-lookup"><span data-stu-id="cea6f-160">Right-click the **Student** entity and select **Paste(or use the Ctrl-V key).**</span></span>
-   <span data-ttu-id="cea6f-161">Wählen Sie die Eigenschaft **registrimentdate** aus, und legen Sie für die Eigenschaft **Nullable** den Wert **false**fest.</span><span class="sxs-lookup"><span data-stu-id="cea6f-161">Select the **EnrollmentDate** property and set the **Nullable** property to **false**.</span></span>
-   <span data-ttu-id="cea6f-162">Wählen Sie die **Person** Entitätstyp aus.</span><span class="sxs-lookup"><span data-stu-id="cea6f-162">Select the **Person** entity type.</span></span> <span data-ttu-id="cea6f-163">Legen Sie im Fenster **Eigenschaften** die Eigenschaft **abstract** auf **true**fest.</span><span class="sxs-lookup"><span data-stu-id="cea6f-163">In the **Properties** window, set its **Abstract** property to **true**.</span></span>
-   <span data-ttu-id="cea6f-164">Löschen Sie die **diskriminatoreigenschaft** von **Person**.</span><span class="sxs-lookup"><span data-stu-id="cea6f-164">Delete the **Discriminator** property from **Person**.</span></span> <span data-ttu-id="cea6f-165">Der Grund für das Löschen sollte im folgenden Abschnitt erläutert werden.</span><span class="sxs-lookup"><span data-stu-id="cea6f-165">The reason it should be deleted is explained in the following section.</span></span>

### <a name="map-the-entities"></a><span data-ttu-id="cea6f-166">Zuordnen der Entitäten</span><span class="sxs-lookup"><span data-stu-id="cea6f-166">Map the entities</span></span>

-   <span data-ttu-id="cea6f-167">Klicken Sie mit der rechten Maustaste auf den **Dozenten** und wählen Sie **Tabellen Zuordnung.**</span><span class="sxs-lookup"><span data-stu-id="cea6f-167">Right-click the **Instructor** and select **Table Mapping.**</span></span>
    <span data-ttu-id="cea6f-168">Die Entität "Instructor" ist im Fenster "Mappingdetails" ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="cea6f-168">The Instructor entity is selected in the Mapping Details window.</span></span>
-   <span data-ttu-id="cea6f-169">Klicken Sie im Fenster **Mappingdetails** auf **&lt;Tabelle oder Sicht&gt; hinzufügen** .</span><span class="sxs-lookup"><span data-stu-id="cea6f-169">Click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
    <span data-ttu-id="cea6f-170">Das **&lt;eine Tabelle oder Sicht hinzufügen&gt;**  Feld wird zu einer Dropdown Liste mit Tabellen oder Sichten, denen die ausgewählte Entität zugeordnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="cea6f-170">The **&lt;Add a Table or View&gt;** field becomes a drop-down list of tables or views to which the selected entity can be mapped.</span></span>
-   <span data-ttu-id="cea6f-171">Wählen Sie in der Dropdown Liste **Person** aus.</span><span class="sxs-lookup"><span data-stu-id="cea6f-171">Select **Person** from the drop-down list.</span></span>
-   <span data-ttu-id="cea6f-172">Das Fenster **Mappingdetails** wird mit Standard Spalten Zuordnungen und einer Option zum Hinzufügen einer Bedingung aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="cea6f-172">The **Mapping Details** window is updated with default column mappings and an option for adding a condition.</span></span>
-   <span data-ttu-id="cea6f-173">Klicken Sie **&lt;Hinzufügen einer Bedingung&gt;** .</span><span class="sxs-lookup"><span data-stu-id="cea6f-173">Click on **&lt;Add a Condition&gt;**.</span></span>
    <span data-ttu-id="cea6f-174">Das **&lt;eine Bedingung&gt; Feld hinzufügen** wird zu einer Dropdown Liste von Spalten, für die Bedingungen festgelegt werden können.</span><span class="sxs-lookup"><span data-stu-id="cea6f-174">The **&lt;Add a Condition&gt;** field becomes a drop-down list of columns for which conditions can be set.</span></span>
-   <span data-ttu-id="cea6f-175">Wählen Sie in der Dropdown Liste **Diskriminator ** aus.</span><span class="sxs-lookup"><span data-stu-id="cea6f-175">Select **Discriminator** from the drop-down list.</span></span>
-   <span data-ttu-id="cea6f-176">Wählen Sie in der Spalte **Operator** des Fensters **Mappingdetails** in der Dropdown Liste die Option = aus.</span><span class="sxs-lookup"><span data-stu-id="cea6f-176">In the **Operator** column of the **Mapping Details** window, select = from the drop-down list.</span></span>
-   <span data-ttu-id="cea6f-177">Geben Sie in der Spalte **Wert/Eigenschaft den Wert**  **Instructor**ein.</span><span class="sxs-lookup"><span data-stu-id="cea6f-177">In the **Value/Property** column, type **Instructor**.</span></span> <span data-ttu-id="cea6f-178">Das Endergebnis sollte wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="cea6f-178">The end result should look like this:</span></span>

    ![Mappingdetails](~/ef6/media/mappingdetails2.png)

-   <span data-ttu-id="cea6f-180">Wiederholen Sie diese Schritte für den Entitätstyp **Student** , aber legen Sie die Bedingung auf den Wert **Student** fest.</span><span class="sxs-lookup"><span data-stu-id="cea6f-180">Repeat these steps for the **Student** entity type, but make the condition equal to **Student** value.</span></span>  
    <span data-ttu-id="cea6f-181">*Der Grund, warum wir die **diskriminatoreigenschaft** entfernen wollten, liegt darin, dass Sie eine Tabellenspalte nicht mehrmals zuordnen können. Diese Spalte wird für die bedingte Zuordnung verwendet und kann daher nicht auch für die Eigenschaften Zuordnung verwendet werden. Die einzige Möglichkeit, wie Sie für beides verwendet werden kann, wenn eine Bedingung einen \*\*is NULL*\* - oder **nicht NULL** - Vergleich verwendet.\*</span><span class="sxs-lookup"><span data-stu-id="cea6f-181">*The reason we wanted to remove the **Discriminator** property, is because you cannot map a table column more than once. This column will be used for conditional mapping, so it cannot be used for property mapping as well. The only way it can be used for both, if a condition uses an **Is Null** or **Is Not Null** comparison.*</span></span>

<span data-ttu-id="cea6f-182">Die "Tabelle pro Hierarchie"-Vererbung ist jetzt implementiert.</span><span class="sxs-lookup"><span data-stu-id="cea6f-182">Table-per-hierarchy inheritance is now implemented.</span></span>

![Endgültige TPH](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a><span data-ttu-id="cea6f-184">Verwenden des Modells</span><span class="sxs-lookup"><span data-stu-id="cea6f-184">Use the Model</span></span>

<span data-ttu-id="cea6f-185">Öffnen Sie die Datei **Program.cs** , in der die **Main** -Methode definiert ist.</span><span class="sxs-lookup"><span data-stu-id="cea6f-185">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="cea6f-186">Fügen Sie den folgenden Code in die **Main** -Funktion ein.</span><span class="sxs-lookup"><span data-stu-id="cea6f-186">Paste the following code into the **Main** function.</span></span> <span data-ttu-id="cea6f-187">Der Code führt drei Abfragen aus.</span><span class="sxs-lookup"><span data-stu-id="cea6f-187">The code executes three queries.</span></span> <span data-ttu-id="cea6f-188">Die erste Abfrage führt alle **Person** -Objekte zurück.</span><span class="sxs-lookup"><span data-stu-id="cea6f-188">The first query brings back all **Person** objects.</span></span> <span data-ttu-id="cea6f-189">Die zweite Abfrage verwendet die **OfType** -Methode, um **Instructor** -Objekte zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="cea6f-189">The second query uses the **OfType** method to return **Instructor** objects.</span></span> <span data-ttu-id="cea6f-190">Die dritte Abfrage verwendet die **OfType** -Methode, um **Student** -Objekte zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="cea6f-190">The third query uses the **OfType** method to return **Student** objects.</span></span>

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
