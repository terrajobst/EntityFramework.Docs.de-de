---
title: Beziehungen – EF-Designer - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 402fe960-754b-470f-976b-e5de3e9986b5
ms.openlocfilehash: 72efe76956c930a787449e6cce453ab0317adc7c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994647"
---
# <a name="relationships---ef-designer"></a><span data-ttu-id="00f0e-102">Beziehungen – EF-Designer</span><span class="sxs-lookup"><span data-stu-id="00f0e-102">Relationships - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="00f0e-103">Diese Seite enthält Informationen zum Einrichten der Beziehungen in Ihrem Modell, mit dem EF Designer.</span><span class="sxs-lookup"><span data-stu-id="00f0e-103">This page provides information about setting up relationships in your model using the EF Designer.</span></span> <span data-ttu-id="00f0e-104">Allgemeine Informationen zu Beziehungen in Entity Framework und das Zugreifen auf und Bearbeiten von Daten mithilfe von Beziehungen, finden Sie unter [Beziehungen und Navigationseigenschaften](~/ef6/fundamentals/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="00f0e-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>

<span data-ttu-id="00f0e-105">Zuordnungen definieren Beziehungen zwischen Entitätstypen in einem Modell.</span><span class="sxs-lookup"><span data-stu-id="00f0e-105">Associations define relationships between entity types in a model.</span></span> <span data-ttu-id="00f0e-106">In diesem Thema veranschaulicht, wie Zuordnungen mit dem Entity Framework Designer (EF-Designer) zuordnen.</span><span class="sxs-lookup"><span data-stu-id="00f0e-106">This topic shows how to map associations with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="00f0e-107">Die folgende Abbildung zeigt die wichtigsten Windows, die bei der Arbeit mit dem EF Designer verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="00f0e-107">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EFDesigner](~/ef6/media/efdesigner.png)

> [!NOTE]
> <span data-ttu-id="00f0e-109">Wenn Sie das konzeptionelle Modell erstellen, können Warnungen zu nicht zugeordneten Entitäten und Zuordnungen in der Fehlerliste angezeigt.</span><span class="sxs-lookup"><span data-stu-id="00f0e-109">When you build the conceptual model, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="00f0e-110">Sie können diese Warnungen ignorieren, da nach der Auswahl, um die Datenbank aus dem Modell zu generieren, die Fehler verschwinden werden.</span><span class="sxs-lookup"><span data-stu-id="00f0e-110">You can ignore these warnings because after you choose to generate the database from the model, the errors will go away.</span></span>

## <a name="associations-overview"></a><span data-ttu-id="00f0e-111">Übersicht über die Zuordnungen</span><span class="sxs-lookup"><span data-stu-id="00f0e-111">Associations Overview</span></span>

<span data-ttu-id="00f0e-112">Wenn Sie Ihr Modell mit dem EF Designer entworfen haben, stellt eine EDMX-Datei das Modell dar.</span><span class="sxs-lookup"><span data-stu-id="00f0e-112">When you design your model using the EF Designer, an .edmx file represents your model.</span></span> <span data-ttu-id="00f0e-113">In der EDMX-Datei ein **Zuordnung** -Element definiert eine Beziehung zwischen zwei Entitätstypen.</span><span class="sxs-lookup"><span data-stu-id="00f0e-113">In the .edmx file, an **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="00f0e-114">Eine Zuordnung muss die Entitätstypen, die in der Beziehung enthalten sind, und die mögliche Anzahl von Entitätstypen an den Enden der Beziehung angeben, die auch als Multiplizität bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="00f0e-114">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="00f0e-115">Die Multiplizität eines zuordnungsendes kann einen Wert von eins (1), NULL oder eins (0.. 1) oder viele haben (\*).</span><span class="sxs-lookup"><span data-stu-id="00f0e-115">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="00f0e-116">Diese Informationen sind in zwei untergeordneten angegeben **End** Elemente.</span><span class="sxs-lookup"><span data-stu-id="00f0e-116">This information is specified in two child **End** elements.</span></span>

<span data-ttu-id="00f0e-117">Zur Laufzeit können auf Entitätstypinstanzen an einem Ende einer Zuordnung über Navigationseigenschaften oder Fremdschlüssel zugegriffen werden, (Wenn Sie auswählen, Fremdschlüssel in Ihren Entitäten verfügbar zu machen).</span><span class="sxs-lookup"><span data-stu-id="00f0e-117">At run time, entity type instances at one end of an association can be accessed through navigation properties or foreign keys (if you choose to expose foreign keys in your entities).</span></span> <span data-ttu-id="00f0e-118">Wurden Fremdschlüssel verfügbar gemacht, die die Beziehung zwischen den Entitäten Verwaltung erfolgt über eine **ReferentialConstraint** Element (ein untergeordnetes Element von der **Zuordnung** Element).</span><span class="sxs-lookup"><span data-stu-id="00f0e-118">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element (a child element of the **Association** element).</span></span> <span data-ttu-id="00f0e-119">Es wird empfohlen, dass Sie immer für die Beziehung zwischen Fremdschlüssel in Ihren Entitäten verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="00f0e-119">It is recommended that you always expose foreign keys for relationships in your entities.</span></span>

> [!NOTE]
> <span data-ttu-id="00f0e-120">In der m: n (\*:\*) die Entitäten können keine Fremdschlüssel hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="00f0e-120">In many-to-many (\*:\*) you cannot add foreign keys to the entities.</span></span> <span data-ttu-id="00f0e-121">In einem \*:\* Beziehung, die Informationen zur Zuordnung mit der ein unabhängiges Objekt verwaltet wird.</span><span class="sxs-lookup"><span data-stu-id="00f0e-121">In a \*:\* relationship, the association information is managed with an independent object.</span></span>

<span data-ttu-id="00f0e-122">Informationen zu CSDL-Elemente (**ReferentialConstraint**, **Zuordnung**usw.) finden Sie unter den [CSDL-Spezifikation](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="00f0e-122">For information about CSDL elements (**ReferentialConstraint**, **Association**, etc.) see the [CSDL specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span>

## <a name="create-and-delete-associations"></a><span data-ttu-id="00f0e-123">Erstellen und Löschen von Zuordnungen</span><span class="sxs-lookup"><span data-stu-id="00f0e-123">Create and Delete Associations</span></span>

<span data-ttu-id="00f0e-124">Erstellen eine Zuordnung mit den EF Designer-Updates des Inhalts der EDMX-Datei.</span><span class="sxs-lookup"><span data-stu-id="00f0e-124">Creating an association with the EF Designer updates the model content of the .edmx file.</span></span> <span data-ttu-id="00f0e-125">Nach dem Erstellen der Zuordnung müssen Sie die Zuordnungen für die Zuordnung (weiter unten in diesem Thema beschrieben) erstellen.</span><span class="sxs-lookup"><span data-stu-id="00f0e-125">After creating an association, you must create the mappings for the association (discussed later in this topic).</span></span>

> [!NOTE]
> <span data-ttu-id="00f0e-126">In diesem Abschnitt wird davon ausgegangen, dass Sie die Entitäten, die Sie eine Zuordnung zwischen dem Modell erstellen möchten, bereits hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="00f0e-126">This section assumes that you already added the entities you wish to create an association between to your model.</span></span>

### <a name="to-create-an-association"></a><span data-ttu-id="00f0e-127">So erstellen Sie eine Zuordnung</span><span class="sxs-lookup"><span data-stu-id="00f0e-127">To create an association</span></span>

1.  <span data-ttu-id="00f0e-128">Mit der rechten Maustaste in eines leeren Bereichs der Entwurfsoberfläche, zeigen Sie auf **Add New**, und wählen Sie **Zuordnung...** .</span><span class="sxs-lookup"><span data-stu-id="00f0e-128">Right-click an empty area of the design surface, point to **Add New**, and select **Association…**.</span></span>
2.  <span data-ttu-id="00f0e-129">Geben Sie die Einstellungen für die Zuordnung in der **Zuordnung hinzufügen** Dialogfeld.</span><span class="sxs-lookup"><span data-stu-id="00f0e-129">Fill in the settings for the association in the **Add Association** dialog.</span></span>

    ![AddAssociation](~/ef6/media/addassociation.png)

    > [!NOTE]
    > <span data-ttu-id="00f0e-131">Sie können auch keine Navigationseigenschaften oder Fremdschlüsseleigenschaften für die Entitäten an den Enden der Zuordnung löschen hinzufügen der ** Navigationseigenschaft ** und ** Fremdschlüsseleigenschaften zum Hinzufügen der &lt;Entitätstypname&gt; Entität ** Kontrollkästchen.</span><span class="sxs-lookup"><span data-stu-id="00f0e-131">You can choose to not add navigation properties or foreign key properties to the entities at the ends of the association by clearing the **Navigation Property **and **Add foreign key properties to the &lt;entity type name&gt; Entity **checkboxes.</span></span> <span data-ttu-id="00f0e-132">Wird nur eine Navigationseigenschaft hinzugefügt, kann die Zuordnung nur in einer Richtung traversiert werden.</span><span class="sxs-lookup"><span data-stu-id="00f0e-132">If you add only one navigation property, the association will be traversable in only one direction.</span></span> <span data-ttu-id="00f0e-133">Falls Sie keine Navigationseigenschaften hinzufügen, müssen Sie Fremdschlüsseleigenschaften hinzufügen, um auf Entitäten an den Enden der Zuordnung zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="00f0e-133">If you add no navigation properties, you must choose to add foreign key properties in order to access entities at the ends of the association.</span></span>
    
3.  <span data-ttu-id="00f0e-134">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="00f0e-134">Click **OK**.</span></span>

### <a name="to-delete-an-association"></a><span data-ttu-id="00f0e-135">So löschen Sie eine Zuordnung</span><span class="sxs-lookup"><span data-stu-id="00f0e-135">To delete an association</span></span>

<span data-ttu-id="00f0e-136">So löschen Sie eine Zuordnung führen Sie einen der folgenden:</span><span class="sxs-lookup"><span data-stu-id="00f0e-136">To delete an association do one of the following:</span></span>

-   <span data-ttu-id="00f0e-137">Mit der rechten Maustaste in der Zuordnung auf die Entwurfsoberfläche, und wählen die EF-Designer **löschen**.</span><span class="sxs-lookup"><span data-stu-id="00f0e-137">Right-click the association on the EF Designer surface and select **Delete**.</span></span>

- <span data-ttu-id="00f0e-138">OR-</span><span class="sxs-lookup"><span data-stu-id="00f0e-138">OR -</span></span>

-   <span data-ttu-id="00f0e-139">Wählen Sie eine oder mehrere Zuordnungen aus, und drücken Sie die ENTF-Taste.</span><span class="sxs-lookup"><span data-stu-id="00f0e-139">Select one or more associations and press the DELETE key.</span></span>

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a><span data-ttu-id="00f0e-140">Einschließen von Fremdschlüsseleigenschaften in Ihren Entitäten (referenzielle Einschränkungen)</span><span class="sxs-lookup"><span data-stu-id="00f0e-140">Include Foreign Key Properties in Your Entities (Referential Constraints)</span></span>

<span data-ttu-id="00f0e-141">Es wird empfohlen, dass Sie immer für die Beziehung zwischen Fremdschlüssel in Ihren Entitäten verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="00f0e-141">It is recommended that you always expose foreign keys for relationships in your entities.</span></span> <span data-ttu-id="00f0e-142">Entitätsframework verwendet eine referenzielle Einschränkung um zu erkennen, dass eine Eigenschaft als Fremdschlüssel für eine Beziehung fungiert.</span><span class="sxs-lookup"><span data-stu-id="00f0e-142">Entity Framework uses a referential constraint to identify that a property acts as the foreign key for a relationship.</span></span>

<span data-ttu-id="00f0e-143">Wenn Sie aktiviert das ***Fremdschlüsseleigenschaften zum Hinzufügen der &lt;Entitätstypname&gt; Entität*** Kontrollkästchen beim Erstellen einer Beziehung, wurde diese referenzielle Einschränkung für die Sie hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="00f0e-143">If you checked the ***Add foreign key properties to the &lt;entity type name&gt; Entity*** checkbox when creating a relationship, this referential constraint was added for you.</span></span>

<span data-ttu-id="00f0e-144">Bei Verwendung der EF-Designer zum Hinzufügen oder Bearbeiten einer referenziellen Einschränkung, die EF-Designer fügt hinzu oder ändert einen **ReferentialConstraint** Element im CSDL-Inhalt der EDMX-Datei.</span><span class="sxs-lookup"><span data-stu-id="00f0e-144">When you use the EF Designer to add or edit a referential constraint, the EF Designer adds or modifies a **ReferentialConstraint** element in the CSDL content of the .edmx file.</span></span>

-   <span data-ttu-id="00f0e-145">Doppelklicken Sie auf die Zuordnung, die Sie bearbeiten möchten.</span><span class="sxs-lookup"><span data-stu-id="00f0e-145">Double-click the association that you want to edit.</span></span>
    <span data-ttu-id="00f0e-146">Die **referenzielle Einschränkung** Dialogfeld wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="00f0e-146">The **Referential Constraint** dialog box appears.</span></span>
-   <span data-ttu-id="00f0e-147">Von der **Principal** Dropdown-Liste, wählen Sie die prinzipalentität in der referenziellen Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="00f0e-147">From the **Principal** drop-down list, select the principal entity in the referential constraint.</span></span>
    <span data-ttu-id="00f0e-148">Schlüsseleigenschaften der Entität werden hinzugefügt, um die **Dienstprinzipalschlüssel** Liste im Dialogfeld.</span><span class="sxs-lookup"><span data-stu-id="00f0e-148">The entity's key properties are added to the **Principal Key** list in the dialog box.</span></span>
-   <span data-ttu-id="00f0e-149">Von der **abhängige** Dropdown-Liste, wählen Sie die abhängige Entität in der referenziellen Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="00f0e-149">From the **Dependent** drop-down list, select the dependent entity in the referential constraint.</span></span>
-   <span data-ttu-id="00f0e-150">Wählen Sie für jeden prinzipalschlüssel, die von einem abhängigen Schlüssel einen entsprechenden abhängigen Schlüssel aus den Dropdown-Listen in die **abhängiger Schlüssel** Spalte.</span><span class="sxs-lookup"><span data-stu-id="00f0e-150">For each principal key that has a dependent key, select a corresponding dependent key from the drop-down lists in the **Dependent Key** column.</span></span>

    ![RefConstraint](~/ef6/media/refconstraint.png)

-   <span data-ttu-id="00f0e-152">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="00f0e-152">Click **OK**.</span></span>

## <a name="create-and-edit-association-mappings"></a><span data-ttu-id="00f0e-153">Erstellen und Bearbeiten von Zuordnungsmappings</span><span class="sxs-lookup"><span data-stu-id="00f0e-153">Create and Edit Association Mappings</span></span>

<span data-ttu-id="00f0e-154">Sie können angeben, wie die Datenbank in eine Zuordnung zugeordnet wird die **Mappingdetails** Fenster des EF-Designers.</span><span class="sxs-lookup"><span data-stu-id="00f0e-154">You can specify how an association maps to the database in the **Mapping Details** window of the EF Designer.</span></span>

> [!NOTE]
> <span data-ttu-id="00f0e-155">Sie können nur Informationen für die Zuordnungen zuordnen, die keine referenzielle Einschränkung angegeben haben.</span><span class="sxs-lookup"><span data-stu-id="00f0e-155">You can only map details for the associations that do not have a referential constraint specified.</span></span> <span data-ttu-id="00f0e-156">Wenn eine referenzielle Einschränkung angegeben wird dann eine Fremdschlüsseleigenschaft ist in der Entität enthalten, und können Sie die Mapping-Details für die Entität aus, um zu steuern, welche Spalte der Fremdschlüssel zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="00f0e-156">If a referential constraint is specified then a foreign key property is included in the entity and you can use the Mapping Details for the entity to control which column the foreign key maps to.</span></span>

### <a name="create-an-association-mapping"></a><span data-ttu-id="00f0e-157">Erstellen Sie ein Zuordnungsmapping</span><span class="sxs-lookup"><span data-stu-id="00f0e-157">Create an association mapping</span></span>

-   <span data-ttu-id="00f0e-158">Mit der rechten Maustaste einer Zuordnung in die Entwurfsoberfläche, und wählen **Tabellenzuordnung**.</span><span class="sxs-lookup"><span data-stu-id="00f0e-158">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="00f0e-159">Dies das Zuordnungsmapping in die **Mappingdetails** Fenster.</span><span class="sxs-lookup"><span data-stu-id="00f0e-159">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="00f0e-160">Klicken Sie auf **Hinzufügen einer Tabelle oder Sicht**.</span><span class="sxs-lookup"><span data-stu-id="00f0e-160">Click **Add a Table or View**.</span></span>
    <span data-ttu-id="00f0e-161">Eine Dropdownliste wird angezeigt, die die Tabellen im Speichermodell enthält.</span><span class="sxs-lookup"><span data-stu-id="00f0e-161">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="00f0e-162">Wählen Sie die Tabelle aus, der die Zuordnung zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="00f0e-162">Select the table to which the association will map.</span></span>
    <span data-ttu-id="00f0e-163">Die **Mappingdetails** Fenster zeigt beide Enden der Zuordnung und die Schlüsseleigenschaften des Entitätstyps an jedem **End**.</span><span class="sxs-lookup"><span data-stu-id="00f0e-163">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each **End**.</span></span>
-   <span data-ttu-id="00f0e-164">Klicken Sie für jede Schlüsseleigenschaft auf das **Spalte** ein, und wählen Sie die Spalte, der die Eigenschaft zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="00f0e-164">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

    ![MappingDetails4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a><span data-ttu-id="00f0e-166">Bearbeiten Sie ein Zuordnungsmapping</span><span class="sxs-lookup"><span data-stu-id="00f0e-166">Edit an association mapping</span></span>

-   <span data-ttu-id="00f0e-167">Mit der rechten Maustaste einer Zuordnung in die Entwurfsoberfläche, und wählen **Tabellenzuordnung**.</span><span class="sxs-lookup"><span data-stu-id="00f0e-167">Right-click an association in the design surface and select **Table Mapping**.</span></span>
    <span data-ttu-id="00f0e-168">Dies das Zuordnungsmapping in die **Mappingdetails** Fenster.</span><span class="sxs-lookup"><span data-stu-id="00f0e-168">This displays the association mapping in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="00f0e-169">Klicken Sie auf **ordnet &lt;Tabellenname&gt;**.</span><span class="sxs-lookup"><span data-stu-id="00f0e-169">Click **Maps to &lt;Table Name&gt;**.</span></span>
    <span data-ttu-id="00f0e-170">Eine Dropdownliste wird angezeigt, die die Tabellen im Speichermodell enthält.</span><span class="sxs-lookup"><span data-stu-id="00f0e-170">A drop-down list appears that includes all the tables in the storage model.</span></span>
-   <span data-ttu-id="00f0e-171">Wählen Sie die Tabelle aus, der die Zuordnung zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="00f0e-171">Select the table to which the association will map.</span></span>
    <span data-ttu-id="00f0e-172">Die **Mappingdetails** Fenster werden beide Enden der Zuordnung und die Schlüsseleigenschaften des Entitätstyps an jedem Ende angezeigt.</span><span class="sxs-lookup"><span data-stu-id="00f0e-172">The **Mapping Details** window displays both ends of the association and the key properties for the entity type at each End.</span></span>
-   <span data-ttu-id="00f0e-173">Klicken Sie für jede Schlüsseleigenschaft auf das **Spalte** ein, und wählen Sie die Spalte, der die Eigenschaft zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="00f0e-173">For each key property, click the **Column** field, and select the column to which the property will map.</span></span>

## <a name="edit-and-delete-navigation-properties"></a><span data-ttu-id="00f0e-174">Bearbeiten und Löschen von Navigationseigenschaften</span><span class="sxs-lookup"><span data-stu-id="00f0e-174">Edit and Delete Navigation Properties</span></span>

<span data-ttu-id="00f0e-175">Navigationseigenschaften sind Verknüpfungseigenschaften, mit denen die Entitäten an den Enden einer Zuordnung in einem Modell zu suchen.</span><span class="sxs-lookup"><span data-stu-id="00f0e-175">Navigation properties are shortcut properties that are used to locate the entities at the ends of an association in a model.</span></span> <span data-ttu-id="00f0e-176">Navigationseigenschaften können beim Erstellen einer Zuordnung zwischen zwei Entitätstypen erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="00f0e-176">Navigation properties can be created when you create an association between two entity types.</span></span>

#### <a name="to-edit-navigation-properties"></a><span data-ttu-id="00f0e-177">Bearbeiten von Navigationseigenschaften</span><span class="sxs-lookup"><span data-stu-id="00f0e-177">To edit navigation properties</span></span>

-   <span data-ttu-id="00f0e-178">Wählen Sie eine Navigationseigenschaft auf der EF Designer-Oberfläche.</span><span class="sxs-lookup"><span data-stu-id="00f0e-178">Select a navigation property on the EF Designer surface.</span></span>
    <span data-ttu-id="00f0e-179">Informationen über die Navigationseigenschaft wird angezeigt, in der Visual Studio **Eigenschaften** Fenster.</span><span class="sxs-lookup"><span data-stu-id="00f0e-179">Information about the navigation property is displayed in the Visual Studio **Properties** window.</span></span>
-   <span data-ttu-id="00f0e-180">Ändern der Einstellungen der Eigenschaften in der **Eigenschaften** Fenster.</span><span class="sxs-lookup"><span data-stu-id="00f0e-180">Change the property settings in the **Properties** window.</span></span>

#### <a name="to-delete-navigation-properties"></a><span data-ttu-id="00f0e-181">Löschen von Navigationseigenschaften</span><span class="sxs-lookup"><span data-stu-id="00f0e-181">To delete navigation properties</span></span>

-   <span data-ttu-id="00f0e-182">Wenn Fremdschlüssel nicht für Entitätstypen im konzeptionellen Modell verfügbar gemacht werden, kann das Löschen einer Navigationseigenschaft dazu führen, dass die entsprechende Zuordnung nur in einer Richtung oder gar nicht traversiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="00f0e-182">If foreign keys are not exposed on entity types in the conceptual model, deleting a navigation property may make the corresponding association traversable in only one direction or not traversable at all.</span></span>
-   <span data-ttu-id="00f0e-183">Mit der rechten Maustaste einer Navigationseigenschaft auf der Entwurfsoberfläche, und wählen den EF Designer **löschen**.</span><span class="sxs-lookup"><span data-stu-id="00f0e-183">Right-click a navigation property on the EF Designer surface and select **Delete**.</span></span>
