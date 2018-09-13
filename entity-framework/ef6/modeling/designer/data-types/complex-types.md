---
title: Komplexe Typen - EF Designer - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9a8228ef-acfd-4575-860d-769d2c0e18a1
ms.openlocfilehash: a3c5578acee23688c04772d2dd0a2221779af562
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489907"
---
# <a name="complex-types---ef-designer"></a><span data-ttu-id="59c56-102">Komplexe Typen - EF-Designer</span><span class="sxs-lookup"><span data-stu-id="59c56-102">Complex Types - EF Designer</span></span>
<span data-ttu-id="59c56-103">In diesem Thema wird gezeigt, wie komplexe Typen mit dem Entity Framework Designer (EF-Designer) zugeordnet und für Entitäten Abfragen, die Eigenschaften des komplexen Typs enthalten.</span><span class="sxs-lookup"><span data-stu-id="59c56-103">This topic shows how to map complex types with the Entity Framework Designer (EF Designer) and how to query for entities that contain properties of complex type.</span></span>

<span data-ttu-id="59c56-104">Die folgende Abbildung zeigt die wichtigsten Windows, die bei der Arbeit mit dem EF Designer verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="59c56-104">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF-Designer](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="59c56-106">Wenn Sie das konzeptionelle Modell erstellen, können Warnungen zu nicht zugeordneten Entitäten und Zuordnungen in der Fehlerliste angezeigt.</span><span class="sxs-lookup"><span data-stu-id="59c56-106">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="59c56-107">Sie können diese Warnungen ignorieren, da nach der Auswahl, um die Datenbank aus dem Modell zu generieren, die Fehler verschwinden werden.</span><span class="sxs-lookup"><span data-stu-id="59c56-107">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="what-is-a-complex-type"></a><span data-ttu-id="59c56-108">Was ist ein komplexer Typ</span><span class="sxs-lookup"><span data-stu-id="59c56-108">What is a Complex Type</span></span>

<span data-ttu-id="59c56-109">Komplexe Typen sind nicht skalare Eigenschaften von Entitätstypen, mit deren Hilfe skalare Eigenschaften in Entitäten organisiert werden können.</span><span class="sxs-lookup"><span data-stu-id="59c56-109">Complex types are non-scalar properties of entity types that enable scalar properties to be organized within entities.</span></span> <span data-ttu-id="59c56-110">Wie Entitäten bestehen komplexe Typen aus skalaren Eigenschaften oder anderen Eigenschaften von komplexem Typ.</span><span class="sxs-lookup"><span data-stu-id="59c56-110">Like entities, complex types consist of scalar properties or other complex type properties.</span></span>

<span data-ttu-id="59c56-111">Wenn Sie mit Objekten, die komplexe Typen darstellen arbeiten, werden Sie folgende Punkte zu beachten:</span><span class="sxs-lookup"><span data-stu-id="59c56-111">When you work with objects that represent complex types, be aware of the following:</span></span>

-   <span data-ttu-id="59c56-112">Komplexe Typen verfügen über keine Schlüssel, und daher nicht unabhängig sein.</span><span class="sxs-lookup"><span data-stu-id="59c56-112">Complex types do not have keys and therefore cannot exist independently.</span></span> <span data-ttu-id="59c56-113">Komplexe Typen können nur Eigenschaften von Entitätstypen oder anderen komplexen Typen sein.</span><span class="sxs-lookup"><span data-stu-id="59c56-113">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="59c56-114">Komplexe Typen können nicht an Zuordnungen beteiligt und können keine Navigationseigenschaften enthalten.</span><span class="sxs-lookup"><span data-stu-id="59c56-114">Complex types cannot participate in associations and cannot contain navigation properties.</span></span>
-   <span data-ttu-id="59c56-115">Eigenschaften eines komplexen Typs darf nicht sein **null**.</span><span class="sxs-lookup"><span data-stu-id="59c56-115">Complex type properties cannot be **null**.</span></span> <span data-ttu-id="59c56-116">Ein \*\* "InvalidOperationException" \*\* tritt auf, wenn **"DbContext.SaveChanges"** aufgerufen wird und ein null-komplexeres Objekt festgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="59c56-116">An \*\*InvalidOperationException \*\*occurs when **DbContext.SaveChanges** is called and a null complex object is encountered.</span></span> <span data-ttu-id="59c56-117">Skalare Eigenschaften komplexer Objekte möglich **null**.</span><span class="sxs-lookup"><span data-stu-id="59c56-117">Scalar properties of complex objects can be **null**.</span></span>
-   <span data-ttu-id="59c56-118">Komplexe Typen können nicht von anderen komplexen Typen erben.</span><span class="sxs-lookup"><span data-stu-id="59c56-118">Complex types cannot inherit from other complex types.</span></span>
-   <span data-ttu-id="59c56-119">Sie müssen den komplexen Typ als definieren eine **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="59c56-119">You must define the complex type as a **class**.</span></span> 
-   <span data-ttu-id="59c56-120">EF erkennt Änderungen an Elemente in ein komplexes Typobjekt beim **DbContext.DetectChanges** aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="59c56-120">EF detects changes to members on a complex type object when **DbContext.DetectChanges** is called.</span></span> <span data-ttu-id="59c56-121">Entitätsframework ruft **DetectChanges** automatisch die folgenden Elemente werden aufgerufen: **DbSet.Find**, **DbSet.Local**, **DbSet.Remove**, **"DbSet.Add"**, **DbSet.Attach**, **"DbContext.SaveChanges"**, **DbContext.GetValidationErrors**, **"DbContext.Entry"**, **DbChangeTracker.Entries**.</span><span class="sxs-lookup"><span data-stu-id="59c56-121">Entity Framework calls **DetectChanges** automatically when the following members are called: **DbSet.Find**, **DbSet.Local**, **DbSet.Remove**, **DbSet.Add**, **DbSet.Attach**, **DbContext.SaveChanges**, **DbContext.GetValidationErrors**, **DbContext.Entry**, **DbChangeTracker.Entries**.</span></span>

## <a name="refactor-an-entitys-properties-into-new-complex-type"></a><span data-ttu-id="59c56-122">Umgestalten Sie Eigenschaften einer Entität in neuen komplexen Typ</span><span class="sxs-lookup"><span data-stu-id="59c56-122">Refactor an Entity’s Properties into New Complex Type</span></span>

<span data-ttu-id="59c56-123">Wenn Sie bereits eine Entität im konzeptionellen Modell, die möglicherweise möchten Sie einige der Eigenschaften in eine Eigenschaft eines komplexen Typs umzugestalten haben.</span><span class="sxs-lookup"><span data-stu-id="59c56-123">If you already have an entity in your conceptual model you may want to refactor some of the properties into a complex type property.</span></span>

<span data-ttu-id="59c56-124">Wählen Sie auf die Designeroberfläche, eine oder mehrere Eigenschaften (keine Navigationseigenschaften) einer Entität, klicken Sie dann mit der rechten Maustaste und wählen Sie **Umgestalten -&gt; verschieben in neuen komplexen Typ**.</span><span class="sxs-lookup"><span data-stu-id="59c56-124">On the designer surface, select one or more properties (excluding navigation properties) of an entity, then right-click and select **Refactor -&gt; Move to New Complex Type**.</span></span>

![Umgestalten](~/ef6/media/refactor.png)

<span data-ttu-id="59c56-126">Ein neuer komplexer Typ mit den ausgewählten Eigenschaften wird hinzugefügt, um die **Modellbrowser**.</span><span class="sxs-lookup"><span data-stu-id="59c56-126">A new complex type with the selected properties is added to the **Model Browser**.</span></span> <span data-ttu-id="59c56-127">Dem komplexen Typ wird ein Standardname zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="59c56-127">The complex type is given a default name.</span></span>

<span data-ttu-id="59c56-128">Die ausgewählten Eigenschaften werden durch eine komplexe Eigenschaft des neu erstellten Typs ersetzt.</span><span class="sxs-lookup"><span data-stu-id="59c56-128">A complex property of the newly created type replaces the selected properties.</span></span> <span data-ttu-id="59c56-129">Alle Eigenschaftenzuordnungen werden beibehalten.</span><span class="sxs-lookup"><span data-stu-id="59c56-129">All property mappings are preserved.</span></span>

![Umgestalten von 2](~/ef6/media/refactor2.png)

## <a name="create-a-new-complex-type"></a><span data-ttu-id="59c56-131">Erstellen Sie einen neuen komplexen Typ</span><span class="sxs-lookup"><span data-stu-id="59c56-131">Create a New Complex Type</span></span>

<span data-ttu-id="59c56-132">Sie können auch einen neuen komplexen Typ erstellen, der keine Eigenschaften einer vorhandenen Entität enthält.</span><span class="sxs-lookup"><span data-stu-id="59c56-132">You can also create a new complex type that does not contain properties of an existing entity.</span></span>

<span data-ttu-id="59c56-133">Mit der rechten Maustaste die **komplexe Typen** Ordner zeigen Sie im Modellbrowser auf **AddNew komplexen Typs...** .</span><span class="sxs-lookup"><span data-stu-id="59c56-133">Right-click the **Complex Types** folder in the Model Browser, point to **AddNew Complex Type…**.</span></span> <span data-ttu-id="59c56-134">Alternativ können Sie auswählen der **komplexe Typen** Ordner, und drücken Sie die **einfügen** auf der Tastatur die Taste.</span><span class="sxs-lookup"><span data-stu-id="59c56-134">Alternatively, you can select the **Complex Types** folder and press the **Insert** key on your keyboard.</span></span>

![Hinzufügen von neuen komplexen Typ](~/ef6/media/addnewcomplextype.png)

<span data-ttu-id="59c56-136">Dem Ordner wird ein neuer komplexer Typ mit einem Standardnamen hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="59c56-136">A new complex type is added to the folder with a default name.</span></span> <span data-ttu-id="59c56-137">Jetzt können Sie Eigenschaften in den Typ hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="59c56-137">You can now add properties to the type.</span></span>

## <a name="add-properties-to-a-complex-type"></a><span data-ttu-id="59c56-138">Fügen Sie Eigenschaften in einen komplexen Typ hinzu.</span><span class="sxs-lookup"><span data-stu-id="59c56-138">Add Properties to a Complex Type</span></span>

<span data-ttu-id="59c56-139">Bei den Eigenschaften eines komplexen Typs kann es sich um skalare Typen oder vorhandene komplexe Typen handeln.</span><span class="sxs-lookup"><span data-stu-id="59c56-139">Properties of a complex type can be scalar types or existing complex types.</span></span> <span data-ttu-id="59c56-140">Für die Eigenschaften eines komplexen Typs können jedoch keine Zirkelverweise verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="59c56-140">However, complex type properties cannot have circular references.</span></span> <span data-ttu-id="59c56-141">Z. B. einen komplexen Typ **OnsiteCourseDetails** eine Eigenschaft eines komplexen Typs sind keine **OnsiteCourseDetails**.</span><span class="sxs-lookup"><span data-stu-id="59c56-141">For example, a complex type **OnsiteCourseDetails** cannot have a property of complex type **OnsiteCourseDetails**.</span></span>

<span data-ttu-id="59c56-142">Einem komplexen Typ können anhand der unten aufgeführten Methoden Eigenschaften hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="59c56-142">You can add a property to a complex type in any of the ways listed below.</span></span>

-   <span data-ttu-id="59c56-143">Mit der rechten Maustaste im Modellbrowser eines komplexen Typs, zeigen Sie auf **hinzufügen**, zeigen Sie dann auf **Skalareigenschaft** oder **komplexe Eigenschaft**, wählen Sie dann den gewünschten Eigenschaftentyp.</span><span class="sxs-lookup"><span data-stu-id="59c56-143">Right-click a complex type in the Model Browser, point to **Add**, then point to **Scalar Property** or **Complex Property**, then select the desired property type.</span></span> <span data-ttu-id="59c56-144">Alternativ können Sie wählen einen komplexen Typ und drücken Sie dann die **einfügen** auf der Tastatur die Taste.</span><span class="sxs-lookup"><span data-stu-id="59c56-144">Alternatively, you can select a complex type and then press the **Insert** key on your keyboard.</span></span>  

    ![Fügen Sie Eigenschaften in komplexen Typ hinzu.](~/ef6/media/addpropertiestocomplextype.png)

    <span data-ttu-id="59c56-146">Dem komplexen Typ wird eine neue Eigenschaft mit einem Standardnamen hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="59c56-146">A new property is added to the complex type with a default name.</span></span>

- <span data-ttu-id="59c56-147">OR-</span><span class="sxs-lookup"><span data-stu-id="59c56-147">OR -</span></span>

-   <span data-ttu-id="59c56-148">Mit der rechten Maustaste einer Entitätseigenschaft auf der **EF Designer** Entwurfsoberfläche, und wählen **Kopie**, per Rechtsklick komplexen Typs in der **Modellbrowser** , und wählen Sie  **Fügen Sie**.</span><span class="sxs-lookup"><span data-stu-id="59c56-148">Right-click an entity property on the **EF  Designer** surface and select **Copy**, then right-click the complex type in the **Model Browser** and select **Paste**.</span></span>

## <a name="rename-a-complex-type"></a><span data-ttu-id="59c56-149">Umbenennen eines komplexen Typs</span><span class="sxs-lookup"><span data-stu-id="59c56-149">Rename a Complex Type</span></span>

<span data-ttu-id="59c56-150">Wenn Sie einen komplexen Typ umbenennen, werden alle Verweise auf den Typ im gesamten Projekt aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="59c56-150">When you rename a complex type, all references to the type are updated throughout the project.</span></span>

-   <span data-ttu-id="59c56-151">Doppelklicken Sie langsam auf einen komplexen Typ in der **Modellbrowser**.</span><span class="sxs-lookup"><span data-stu-id="59c56-151">Slowly double-click a complex type in the **Model Browser**.</span></span>
    <span data-ttu-id="59c56-152">Der Name wird markiert und kann bearbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="59c56-152">The name will be selected and in edit mode.</span></span>

- <span data-ttu-id="59c56-153">OR-</span><span class="sxs-lookup"><span data-stu-id="59c56-153">OR -</span></span>

-   <span data-ttu-id="59c56-154">Mit der rechten Maustaste in eines komplexen Typs in der **Modellbrowser** , und wählen Sie **umbenennen**.</span><span class="sxs-lookup"><span data-stu-id="59c56-154">Right-click a complex type in the **Model Browser** and select **Rename**.</span></span>

- <span data-ttu-id="59c56-155">OR-</span><span class="sxs-lookup"><span data-stu-id="59c56-155">OR -</span></span>

-   <span data-ttu-id="59c56-156">Markieren Sie im Modellbrowser einen komplexen Typ, und drücken Sie die F2-TASTE.</span><span class="sxs-lookup"><span data-stu-id="59c56-156">Select a complex type in the Model Browser and press the F2 key.</span></span>

- <span data-ttu-id="59c56-157">OR-</span><span class="sxs-lookup"><span data-stu-id="59c56-157">OR -</span></span>

-   <span data-ttu-id="59c56-158">Mit der rechten Maustaste in eines komplexen Typs in der **Modellbrowser** , und wählen Sie **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="59c56-158">Right-click a complex type in the **Model Browser** and select **Properties**.</span></span> <span data-ttu-id="59c56-159">Bearbeiten Sie den Namen in der **Eigenschaften** Fenster.</span><span class="sxs-lookup"><span data-stu-id="59c56-159">Edit the name in the **Properties** window.</span></span>

## <a name="add-an-existing-complex-type-to-an-entity-and-map-its-properties-to-table-columns"></a><span data-ttu-id="59c56-160">Fügen Sie einen vorhandenen komplexen Typ auf eine Entität hinzu, und ordnen Sie seine Eigenschaften zu Tabellenspalten</span><span class="sxs-lookup"><span data-stu-id="59c56-160">Add an Existing Complex Type to an Entity and Map its Properties to Table Columns</span></span>

1.  <span data-ttu-id="59c56-161">Mit der rechten Maustaste einer Entitätstyps, zeigen Sie auf **Add New**, und wählen Sie **komplexe Eigenschaft**.</span><span class="sxs-lookup"><span data-stu-id="59c56-161">Right-click an entity, point to **Add New**, and select **Complex Property**.</span></span>
    <span data-ttu-id="59c56-162">Der Entität wird eine Eigenschaft eines komplexen Typs mit einem Standardnamen hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="59c56-162">A complex type property with a default name is added to the entity.</span></span> <span data-ttu-id="59c56-163">Der Eigenschaft wird ein Standardtyp zugewiesen (ausgewählt aus den vorhandenen komplexen Typen).</span><span class="sxs-lookup"><span data-stu-id="59c56-163">A default type (chosen from the existing complex types) is assigned to the property.</span></span>
2.  <span data-ttu-id="59c56-164">Weisen Sie den gewünschten Typ der Eigenschaft in der **Eigenschaften** Fenster.</span><span class="sxs-lookup"><span data-stu-id="59c56-164">Assign the desired type to the property in the **Properties** window.</span></span>
    <span data-ttu-id="59c56-165">Nachdem Sie einer Entität eine Eigenschaft eines komplexen Typs hinzugefügt haben, müssen Sie die zugehörigen Eigenschaften Tabellenspalten zuordnen.</span><span class="sxs-lookup"><span data-stu-id="59c56-165">After adding a complex type property to an entity, you must map its properties to table columns.</span></span>
3.  <span data-ttu-id="59c56-166">Mit der rechten Maustaste in eines Entitätstyps auf der Entwurfsoberfläche oder in der **Modellbrowser** , und wählen Sie **Tabellenzuordnungen**.</span><span class="sxs-lookup"><span data-stu-id="59c56-166">Right-click an entity type on the design surface or in the **Model Browser** and select **Table Mappings**.</span></span>
    <span data-ttu-id="59c56-167">Die tabellenzuordnungen werden angezeigt, der **Mappingdetails** Fenster.</span><span class="sxs-lookup"><span data-stu-id="59c56-167">The table mappings are displayed in the **Mapping Details** window.</span></span>
4.  <span data-ttu-id="59c56-168">Erweitern Sie die **ordnet &lt;Tabellenname&gt;**  Knoten.</span><span class="sxs-lookup"><span data-stu-id="59c56-168">Expand the **Maps to &lt;Table Name&gt;** node.</span></span>
    <span data-ttu-id="59c56-169">Ein **Spaltenzuordnungen** Knoten angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="59c56-169">A **Column Mappings** node appears.</span></span>
5.  <span data-ttu-id="59c56-170">Erweitern Sie die **Spaltenzuordnungen** Knoten.</span><span class="sxs-lookup"><span data-stu-id="59c56-170">Expand the **Column Mappings** node.</span></span>
    <span data-ttu-id="59c56-171">Eine Liste aller Spalten in der Tabelle wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="59c56-171">A list of all the columns in the table appears.</span></span> <span data-ttu-id="59c56-172">Die Standardeigenschaften (sofern vorhanden), denen die Spalten zugeordnet sind aufgeführt, unter dem **Wert/Eigenschaft** Überschrift.</span><span class="sxs-lookup"><span data-stu-id="59c56-172">The default properties (if any) to which the columns map are listed under the **Value/Property** heading.</span></span>
6.  <span data-ttu-id="59c56-173">Wählen Sie die Spalte, die Sie zuordnen möchten und dann auf den entsprechenden **Wert/Eigenschaft** Feld.</span><span class="sxs-lookup"><span data-stu-id="59c56-173">Select the column you want to map, and then right-click the corresponding **Value/Property** field.</span></span>
    <span data-ttu-id="59c56-174">Eine Dropdownliste aller skalaren Eigenschaften wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="59c56-174">A drop-down list of all the scalar properties is displayed.</span></span>
7.  <span data-ttu-id="59c56-175">Wählen Sie die entsprechende Eigenschaft aus.</span><span class="sxs-lookup"><span data-stu-id="59c56-175">Select the appropriate property.</span></span>

    ![Ordnen komplexen Typ zu](~/ef6/media/mapcomplextype.png)

8.  <span data-ttu-id="59c56-177">Wiederholen Sie die Schritte 6 und 7 für jede Tabellenspalte.</span><span class="sxs-lookup"><span data-stu-id="59c56-177">Repeat steps 6 and 7 for each table column.</span></span>

>[!NOTE]
> <span data-ttu-id="59c56-178">Um eine spaltenzuordnung zu löschen, wählen Sie die Spalte, die Sie verwenden möchten, ordnen, und klicken Sie dann auf die **Wert/Eigenschaft** Feld.</span><span class="sxs-lookup"><span data-stu-id="59c56-178">To delete a column mapping, select the column that you want to map, and then click the **Value/Property** field.</span></span> <span data-ttu-id="59c56-179">Wählen Sie dann **löschen** aus der Dropdown-Liste.</span><span class="sxs-lookup"><span data-stu-id="59c56-179">Then, select **Delete** from the drop-down list.</span></span>

## <a name="map-a-function-import-to-a-complex-type"></a><span data-ttu-id="59c56-180">Zuordnen eines Funktionsimports zu einem komplexen Typ</span><span class="sxs-lookup"><span data-stu-id="59c56-180">Map a Function Import to a Complex Type</span></span>

<span data-ttu-id="59c56-181">Funktionsimporte basieren auf gespeicherten Prozeduren.</span><span class="sxs-lookup"><span data-stu-id="59c56-181">Function imports are based on stored procedures.</span></span> <span data-ttu-id="59c56-182">Um einen Funktionsimport einem komplexen Typ zuzuordnen, muss die Zahl der von der entsprechenden gespeicherten Prozedur zurückgegebenen Spalten der Zahl der Eigenschaften des komplexen Typs entsprechen, und die Spalten müssen einen Speichertyp aufweisen, der mit den Typen der Eigenschaften kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="59c56-182">To map a function import to a complex type, the columns returned by the corresponding stored procedure must match the properties of the complex type in number and must have storage types that are compatible with the property types.</span></span>

-   <span data-ttu-id="59c56-183">Doppelklicken Sie auf einer importierten Funktion, die Sie in einen komplexen Typ zuordnen möchten.</span><span class="sxs-lookup"><span data-stu-id="59c56-183">Double-click on an imported function that you want to map to a complex type.</span></span>

    ![Funktionsimporte](~/ef6/media/functionimports.png)

-   <span data-ttu-id="59c56-185">Geben Sie wie folgt die Einstellungen für den neuen Funktionsimport ein:</span><span class="sxs-lookup"><span data-stu-id="59c56-185">Fill in the settings for the new function import, as follows:</span></span>
    -   <span data-ttu-id="59c56-186">Geben Sie die gespeicherte Prozedur, die für die Sie einen Funktionsimport im Erstellen der **Name der gespeicherten Prozedur** Feld.</span><span class="sxs-lookup"><span data-stu-id="59c56-186">Specify the stored procedure for which you are creating a function import in the **Stored Procedure Name** field.</span></span> <span data-ttu-id="59c56-187">Dieses Feld wird als Dropdownliste angezeigt, die alle gespeicherten Prozeduren im Speichermodell enthält.</span><span class="sxs-lookup"><span data-stu-id="59c56-187">This field is a drop-down list that displays all the stored procedures in the storage model.</span></span>
    -   <span data-ttu-id="59c56-188">Geben Sie den Namen des Funktionsimports im der **Funktionsnamen importieren** Feld.</span><span class="sxs-lookup"><span data-stu-id="59c56-188">Specify the name of the function import in the **Function Import Name** field.</span></span>
    -   <span data-ttu-id="59c56-189">Wählen Sie **komplexe** als Rückgabetyp geben, und geben Sie dann den speziellen komplexen Rückgabetyp durch Auswählen des entsprechenden Typs aus der Dropdown-Liste.</span><span class="sxs-lookup"><span data-stu-id="59c56-189">Select **Complex** as the return type and then specify the specific complex return type by choosing the appropriate type from the drop-down list.</span></span>

        ![Funktionsimport bearbeiten](~/ef6/media/editfunctionimport.png)

-   <span data-ttu-id="59c56-191">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="59c56-191">Click **OK**.</span></span>
    <span data-ttu-id="59c56-192">Der Funktionsimport-Eintrag wird im konzeptionellen Modell erstellt.</span><span class="sxs-lookup"><span data-stu-id="59c56-192">The function import entry is created in the conceptual model.</span></span>

### <a name="customize-column-mapping-for-function-import"></a><span data-ttu-id="59c56-193">Anpassen der Spaltenzuordnung für den Funktionsimport</span><span class="sxs-lookup"><span data-stu-id="59c56-193">Customize Column Mapping for Function Import</span></span>

-   <span data-ttu-id="59c56-194">Den Funktionsimport im Modellbrowser Maustaste und wählen Sie **Funktionsimportmapping**.</span><span class="sxs-lookup"><span data-stu-id="59c56-194">Right-click the function import in the Model Browser and select **Function Import Mapping**.</span></span>
    <span data-ttu-id="59c56-195">Die **Mappingdetails** Fenster angezeigt, und zeigt die standardzuordnung für den Funktionsimport.</span><span class="sxs-lookup"><span data-stu-id="59c56-195">The **Mapping Details** window appears and shows the default mapping for the function import.</span></span> <span data-ttu-id="59c56-196">Pfeilsymbole geben die Zuordnungen der Spaltenwerte zu den Eigenschaftswerten an.</span><span class="sxs-lookup"><span data-stu-id="59c56-196">Arrows indicate the mappings between column values and property values.</span></span> <span data-ttu-id="59c56-197">Standardmäßig wird angenommen, dass die Spaltennamen mit den Namen der Eigenschaften des komplexen Typs identisch sind.</span><span class="sxs-lookup"><span data-stu-id="59c56-197">By default, the column names are assumed to be the same as the complex type's property names.</span></span> <span data-ttu-id="59c56-198">Die Standardspaltennamen werden in grauem Text angezeigt.</span><span class="sxs-lookup"><span data-stu-id="59c56-198">The default column names appear in gray text.</span></span>
-   <span data-ttu-id="59c56-199">Ändern Sie, falls erforderlich, die Spaltennamen so, dass sie mit den Spaltennamen übereinstimmen, die von der gespeicherten Prozedur zurückgegeben werden, die dem Funktionsimport entspricht.</span><span class="sxs-lookup"><span data-stu-id="59c56-199">If necessary, change the column names to match the column names that are returned by the stored procedure that corresponds to the function import.</span></span>

## <a name="delete-a-complex-type"></a><span data-ttu-id="59c56-200">Löschen eines komplexen Typs</span><span class="sxs-lookup"><span data-stu-id="59c56-200">Delete a Complex Type</span></span>

<span data-ttu-id="59c56-201">Wenn Sie einen komplexen Typ löschen, wird der Typ aus dem konzeptionellen Modell gelöscht, und die Mappings für alle Instanzen des Typs werden gelöscht.</span><span class="sxs-lookup"><span data-stu-id="59c56-201">When you delete a complex type, the type is deleted from the conceptual model and mappings for all instances of the type are deleted.</span></span> <span data-ttu-id="59c56-202">Verweise auf den Typ werden jedoch nicht aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="59c56-202">However, references to the type are not updated.</span></span> <span data-ttu-id="59c56-203">Angenommen, eine Entität hat die Eigenschaft eines komplexen Typs complexType1 verfügt und ComplexType1 im gelöscht wird die **Modellbrowser**, die entsprechende Entitätseigenschaft nicht aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="59c56-203">For example, if an entity has a complex type property of type ComplexType1 and ComplexType1 is deleted in the **Model Browser**, the corresponding entity property is not updated.</span></span> <span data-ttu-id="59c56-204">Das Modell wird nicht überprüft werden, da es sich um eine Entität enthält, die auf einen gelöschten komplexen Typ verweist.</span><span class="sxs-lookup"><span data-stu-id="59c56-204">The model will not validate  because it contains an entity that references a deleted complex type.</span></span> <span data-ttu-id="59c56-205">Verweise auf gelöschte komplexe Typen können mit dem Entity Designer aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="59c56-205">You can update or delete references to deleted complex types by using the Entity Designer.</span></span>

-   <span data-ttu-id="59c56-206">Mit der rechten Maustaste im Modellbrowser eines komplexen Typs, und wählen Sie **löschen**.</span><span class="sxs-lookup"><span data-stu-id="59c56-206">Right-click a complex type in the Model Browser and select **Delete**.</span></span>

- <span data-ttu-id="59c56-207">OR-</span><span class="sxs-lookup"><span data-stu-id="59c56-207">OR -</span></span>

-   <span data-ttu-id="59c56-208">Markieren Sie im Modellbrowser einen komplexen Typ, und drücken Sie dann die ENTF-TASTE auf der Tastatur.</span><span class="sxs-lookup"><span data-stu-id="59c56-208">Select a complex type in the Model Browser and press the Delete key on your keyboard.</span></span>

## <a name="query-for-entities-containing-properties-of-complex-type"></a><span data-ttu-id="59c56-209">Abfragen von Entitäten, die Eigenschaften des komplexen Typs enthält.</span><span class="sxs-lookup"><span data-stu-id="59c56-209">Query for Entities Containing Properties of Complex Type</span></span>

<span data-ttu-id="59c56-210">Der folgende Code zeigt, wie Sie eine Abfrage auszuführen, die eine Auflistung der Entität zurückgibt, die Eigenschaft eines komplexen Typs enthalten.</span><span class="sxs-lookup"><span data-stu-id="59c56-210">The following code shows how to execute a query that returns a collection of entity type objects that contain a complex type property.</span></span>

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        var courses =
            from c in context.OnsiteCourses
            order by c.Details.Time
            select c;

        foreach (var c in courses)
        {
            Console.WriteLine("Time: " + c.Details.Time);
            Console.WriteLine("Days: " + c.Details.Days);
            Console.WriteLine("Location: " + c.Details.Location);
        }
    }
```
