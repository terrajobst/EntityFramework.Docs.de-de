---
title: Designer Tabellenaufteilung - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
ms.openlocfilehash: 8b0ca6778a06ed43b1365d2e5969ff15948f8004
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490693"
---
# <a name="designer-table-splitting"></a><span data-ttu-id="865ba-102">Designer Tabellenaufteilung</span><span class="sxs-lookup"><span data-stu-id="865ba-102">Designer Table Splitting</span></span>
<span data-ttu-id="865ba-103">In dieser exemplarischen Vorgehensweise wird gezeigt, wie mehrere Entitätstypen durch Ändern eines Modells mit dem Entity Framework Designer (EF-Designer) einer einzelnen Tabelle zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="865ba-103">This walkthrough shows how to map multiple entity types to a single table by modifying a model with the Entity Framework Designer (EF Designer).</span></span>

<span data-ttu-id="865ba-104">Ein Grund, die Sie möglicherweise tabellenaufteilung verwenden möchten, ist das Laden einiger Eigenschaften verzögern, Verwendung von lazy loading-um die Objekte zu laden.</span><span class="sxs-lookup"><span data-stu-id="865ba-104">One reason you may want to use table splitting is delaying the loading of some properties when using lazy loading to load your objects.</span></span> <span data-ttu-id="865ba-105">Sie können die Eigenschaften trennen, die möglicherweise große Menge von Daten in eine getrennte Entität enthalten und nur bei Bedarf laden.</span><span class="sxs-lookup"><span data-stu-id="865ba-105">You can separate the properties that might contain very large amount of data into a seperate entity and only load it when required.</span></span>

<span data-ttu-id="865ba-106">Die folgende Abbildung zeigt die wichtigsten Windows, die bei der Arbeit mit dem EF Designer verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="865ba-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF-Designer](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="865ba-108">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="865ba-108">Prerequisites</span></span>

<span data-ttu-id="865ba-109">Um die exemplarische Vorgehensweise nachzuvollziehen, benötigen Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="865ba-109">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="865ba-110">Eine aktuelle Version von Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="865ba-110">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="865ba-111">Die [Beispieldatenbank ' School '](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="865ba-111">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="865ba-112">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="865ba-112">Set up the Project</span></span>

<span data-ttu-id="865ba-113">In dieser exemplarischen Vorgehensweise wird Visual Studio 2012 verwenden.</span><span class="sxs-lookup"><span data-stu-id="865ba-113">This walkthrough is using Visual Studio 2012.</span></span>

-   <span data-ttu-id="865ba-114">Öffnen Sie Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="865ba-114">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="865ba-115">Zeigen Sie im Menü **Datei** auf **Neu**, und klicken Sie dann auf **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="865ba-115">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="865ba-116">Klicken Sie im linken Bereich auf Visual C\#, und wählen Sie dann die Vorlage für die Konsolenanwendung.</span><span class="sxs-lookup"><span data-stu-id="865ba-116">In the left pane, click Visual C\#, and then select the Console Application template.</span></span>
-   <span data-ttu-id="865ba-117">Geben Sie **TableSplittingSample** als Namen für das Projekt und auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="865ba-117">Enter **TableSplittingSample** as the name of the project and click **OK**.</span></span>

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="865ba-118">Erstellen Sie ein Modell basierend auf der Datenbank "School"</span><span class="sxs-lookup"><span data-stu-id="865ba-118">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="865ba-119">Mit der rechten Maustaste des Projektnamen im Projektmappen-Explorer, zeigen Sie auf **hinzufügen**, und klicken Sie dann auf **neues Element**.</span><span class="sxs-lookup"><span data-stu-id="865ba-119">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="865ba-120">Wählen Sie **Daten** aus dem linken Menü und wählen Sie dann **ADO.NET Entity Data Model** im Bereich Vorlagen.</span><span class="sxs-lookup"><span data-stu-id="865ba-120">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="865ba-121">Geben Sie **TableSplittingModel.edmx** für den Dateinamen ein, und klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="865ba-121">Enter **TableSplittingModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="865ba-122">Wählen Sie im Dialogfeld "Modellinhalte" **aus Datenbank generieren**, und klicken Sie dann auf **weiter.**</span><span class="sxs-lookup"><span data-stu-id="865ba-122">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="865ba-123">Klicken Sie auf die neue Verbindung.</span><span class="sxs-lookup"><span data-stu-id="865ba-123">Click New Connection.</span></span> <span data-ttu-id="865ba-124">Geben Sie den Servernamen, klicken Sie im Dialogfeld "Verbindungseigenschaften" (z. B. **(Localdb)\\Mssqllocaldb**), wählen die Authentifizierungsmethode, Typ **School** für den Datenbanknamen, und klicken Sie dann Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="865ba-124">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="865ba-125">Das Dialogfeld "Wählen Sie Ihre Datenverbindung" wird mit Ihrem datenbankverbindungseinstellung aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="865ba-125">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="865ba-126">Erweitern Sie in das Dialogfeld "Datenbankobjekte auswählen" die **Tabellen** Knoten und überprüfen Sie die **Person** Tabelle.</span><span class="sxs-lookup"><span data-stu-id="865ba-126">In the Choose Your Database Objects dialog box, unfold the **Tables** node and check the **Person** table.</span></span> <span data-ttu-id="865ba-127">Dadurch wird hinzugefügt, die angegebene Tabelle auf die **School** Modell.</span><span class="sxs-lookup"><span data-stu-id="865ba-127">This will add the specified table to the **School** model.</span></span>
-   <span data-ttu-id="865ba-128">Klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="865ba-128">Click **Finish**.</span></span>

<span data-ttu-id="865ba-129">Im Entity Designer, der bietet eine Entwurfsoberfläche zum Bearbeiten des Modells, wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="865ba-129">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="865ba-130">Alle Objekte, die Sie ausgewählt haben, in der **Datenbankobjekte auswählen** im Dialogfeld zum Modell hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="865ba-130">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>

## <a name="map-two-entities-to-a-single-table"></a><span data-ttu-id="865ba-131">Zuordnen von zwei Entitäten zu einer einzelnen Tabelle</span><span class="sxs-lookup"><span data-stu-id="865ba-131">Map Two Entities to a Single Table</span></span>

<span data-ttu-id="865ba-132">In diesem Abschnitt unterteilen Sie die **Person** Entität in beiden Entitäten und deren Zuordnung zu einer einzelnen Tabelle.</span><span class="sxs-lookup"><span data-stu-id="865ba-132">In this section you will split the **Person** entity into two entities and then map them to a single table.</span></span>

> [!NOTE]
> <span data-ttu-id="865ba-133">Die **Person** Entität enthält keine Eigenschaften, die möglicherweise große Datenmengen enthalten; es wird nur als Beispiel verwendet.</span><span class="sxs-lookup"><span data-stu-id="865ba-133">The **Person** entity does not contain any properties that may contain large amount of data; it is just used as an example.</span></span>

-   <span data-ttu-id="865ba-134">Mit der rechten Maustaste in eines leeren Bereichs der Entwurfsoberfläche, zeigen Sie auf **Add New**, und klicken Sie auf **Entität**.</span><span class="sxs-lookup"><span data-stu-id="865ba-134">Right-click an empty area of the design surface, point to **Add New**, and click **Entity**.</span></span>
    <span data-ttu-id="865ba-135">Die **neue Entität** Dialogfeld wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="865ba-135">The **New Entity** dialog box appears.</span></span>
-   <span data-ttu-id="865ba-136">Typ **HireInfo** für die **Entitätsname** und **PersonID** für die **Schlüsseleigenschaft** Name.</span><span class="sxs-lookup"><span data-stu-id="865ba-136">Type **HireInfo** for the **Entity name** and **PersonID** for the **Key Property** name.</span></span>
-   <span data-ttu-id="865ba-137">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="865ba-137">Click **OK**.</span></span>
-   <span data-ttu-id="865ba-138">Ein neuer Entitätstyp wird erstellt und auf der Entwurfsoberfläche angezeigt.</span><span class="sxs-lookup"><span data-stu-id="865ba-138">A new entity type is created and displayed on the design surface.</span></span>
-   <span data-ttu-id="865ba-139">Wählen Sie die **HireDate** Eigenschaft der **Person** Entitätstyp, und drücken Sie **STRG + X** Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="865ba-139">Select the **HireDate** property of the **Person** entity type and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="865ba-140">Wählen Sie die **HireInfo** Entität, und drücken Sie **STRG + V** Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="865ba-140">Select the **HireInfo** entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="865ba-141">Erstellen einer Assoziation zwischen **Person** und **HireInfo**.</span><span class="sxs-lookup"><span data-stu-id="865ba-141">Create an association between **Person** and **HireInfo**.</span></span> <span data-ttu-id="865ba-142">Zu diesem Zweck mit der rechten Maustaste in eines leeren Bereichs der Entwurfsoberfläche, zeigen Sie auf **Add New**, und klicken Sie auf **Zuordnung**.</span><span class="sxs-lookup"><span data-stu-id="865ba-142">To do this, right-click an empty area of the design surface, point to **Add New**, and click **Association**.</span></span>
-   <span data-ttu-id="865ba-143">Die **Zuordnung hinzufügen** Dialogfeld wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="865ba-143">The **Add Association** dialog box appears.</span></span> <span data-ttu-id="865ba-144">Die **PersonHireInfo** wird standardmäßig der Name angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="865ba-144">The **PersonHireInfo** name is given by default.</span></span>
-   <span data-ttu-id="865ba-145">Geben Sie die Multiplizität **1(One)** an beiden Enden der Beziehung.</span><span class="sxs-lookup"><span data-stu-id="865ba-145">Specify multiplicity **1(One)** on both ends of the relationship.</span></span>
-   <span data-ttu-id="865ba-146">Drücken Sie **OK**.</span><span class="sxs-lookup"><span data-stu-id="865ba-146">Press **OK**.</span></span>

<span data-ttu-id="865ba-147">Der nächste Schritt ist erforderlich. die **Mappingdetails** Fenster.</span><span class="sxs-lookup"><span data-stu-id="865ba-147">The next step requires the **Mapping Details** window.</span></span> <span data-ttu-id="865ba-148">Wenn Sie dieses Fenster nicht sehen, mit der rechten Maustaste Entwurfsoberfläche, und wählen **Mappingdetails**.</span><span class="sxs-lookup"><span data-stu-id="865ba-148">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="865ba-149">Wählen Sie die **HireInfo** Entitätstyp, und klicken Sie auf **&lt;Hinzufügen einer Tabelle oder Sicht&gt;** in die **Mappingdetails** Fenster.</span><span class="sxs-lookup"><span data-stu-id="865ba-149">Select the **HireInfo** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="865ba-150">Wählen Sie **Person** aus der **&lt;Hinzufügen einer Tabelle oder Sicht&gt;** Feld Dropdown-Liste.</span><span class="sxs-lookup"><span data-stu-id="865ba-150">Select **Person** from the **&lt;Add a Table or View&gt;** field drop-down list.</span></span> <span data-ttu-id="865ba-151">Die Liste enthält die Tabellen oder Sichten, die die ausgewählte Entität zugeordnet werden können.</span><span class="sxs-lookup"><span data-stu-id="865ba-151">The list contains tables or views to which the selected entity can be mapped.</span></span>
    <span data-ttu-id="865ba-152">Die entsprechenden Eigenschaften sollten standardmäßig zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="865ba-152">The appropriate properties should be mapped by default.</span></span>

    ![Zuordnung](~/ef6/media/mapping.png)

-   <span data-ttu-id="865ba-154">Wählen Sie die **PersonHireInfo** Zuordnung auf der Entwurfsoberfläche angezeigt.</span><span class="sxs-lookup"><span data-stu-id="865ba-154">Select the **PersonHireInfo** association on the design surface.</span></span>
-   <span data-ttu-id="865ba-155">Mit der rechten Maustaste in der Zuordnung auf die Entwurfsoberfläche, und wählen **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="865ba-155">Right-click the association on the design surface and select **Properties**.</span></span>
-   <span data-ttu-id="865ba-156">In der **Eigenschaften** wählen Sie im Fenster der **referenziellen Einschränkungen** Eigenschaft, und klicken Sie auf die Schaltfläche mit den Auslassungspunkten.</span><span class="sxs-lookup"><span data-stu-id="865ba-156">In the **Properties** window, select the **Referential Constraints** property and click the ellipses button.</span></span>
-   <span data-ttu-id="865ba-157">Wählen Sie **Person** aus der **Principal** Dropdown-Liste.</span><span class="sxs-lookup"><span data-stu-id="865ba-157">Select **Person** from the **Principal** drop-down list.</span></span>
-   <span data-ttu-id="865ba-158">Drücken Sie **OK**.</span><span class="sxs-lookup"><span data-stu-id="865ba-158">Press **OK**.</span></span>

 

## <a name="use-the-model"></a><span data-ttu-id="865ba-159">Verwenden Sie das Modell</span><span class="sxs-lookup"><span data-stu-id="865ba-159">Use the Model</span></span>

-   <span data-ttu-id="865ba-160">Fügen Sie den folgenden Code in der Main-Methode.</span><span class="sxs-lookup"><span data-stu-id="865ba-160">Paste the following code in the Main method.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        Person person = new Person()
        {
            FirstName = "Kimberly",
            LastName = "Morgan",
            Discriminator = "Instructor",
        };

        person.HireInfo = new HireInfo()
        {
            HireDate = DateTime.Now
        };

        // Add the new person to the context.
        context.People.Add(person);

        // Insert a row into the Person table.  
        context.SaveChanges();

        // Execute a query against the Person table.
        // The query returns columns that map to the Person entity.
        var existingPerson = context.People.FirstOrDefault();

        // Execute a query against the Person table.
        // The query returns columns that map to the Instructor entity.
        var hireInfo = existingPerson.HireInfo;

        Console.WriteLine("{0} was hired on {1}",
            existingPerson.LastName, hireInfo.HireDate);
    }
```
-   <span data-ttu-id="865ba-161">Kompilieren Sie die Anwendung, und führen Sie sie aus.</span><span class="sxs-lookup"><span data-stu-id="865ba-161">Compile and run the application.</span></span>

<span data-ttu-id="865ba-162">Auf die folgenden T-SQL-Anweisungen ausgeführt wurden die **School** Datenbank aufgrund der Ausführung dieser Anwendungsprogramms.</span><span class="sxs-lookup"><span data-stu-id="865ba-162">The following T-SQL statements were executed against the **School** database as a result of running this application.</span></span> 

-   <span data-ttu-id="865ba-163">Die folgenden **einfügen** wurde als Ergebnis der Ausführung von Kontext ausgeführt. SaveChanges() und kombiniert Daten aus der **Person** und **HireInfo** Entitäten</span><span class="sxs-lookup"><span data-stu-id="865ba-163">The following **INSERT** was executed as a result of executing context.SaveChanges() and combines data from the **Person** and **HireInfo** entities</span></span>

    ![Insert](~/ef6/media/insert.png)

-   <span data-ttu-id="865ba-165">Die folgenden **wählen** wurde als Ergebnis der Ausführung von Kontext ausgeführt. People.FirstOrDefault() und wählt nur die Spalten zugeordnet **Person**</span><span class="sxs-lookup"><span data-stu-id="865ba-165">The following **SELECT** was executed as a result of executing context.People.FirstOrDefault() and selects just the columns mapped to **Person**</span></span>

    ![Wählen Sie 1](~/ef6/media/select1.png)

-   <span data-ttu-id="865ba-167">Die folgenden **wählen** ausgeführt wurde, da der Zugriff auf die Navigation Eigenschaft existingPerson.Instructor und wählt nur die Spalten zugeordnet **HireInfo**</span><span class="sxs-lookup"><span data-stu-id="865ba-167">The following **SELECT** was executed as a result of accessing the navigation property existingPerson.Instructor and selects just the columns mapped to **HireInfo**</span></span>

    ![Wählen Sie 2](~/ef6/media/select2.png)
