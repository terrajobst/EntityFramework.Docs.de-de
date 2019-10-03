---
title: Gespeicherte Prozeduren für Designer-CUD-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: bdb0df969c33d5ad3f103bfa9af6002c9c2bb9b3
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813558"
---
# <a name="designer-cud-stored-procedures"></a><span data-ttu-id="a593f-102">Designer-CUD-Prozeduren</span><span class="sxs-lookup"><span data-stu-id="a593f-102">Designer CUD Stored Procedures</span></span>

<span data-ttu-id="a593f-103">In dieser schrittweisen exemplarischen Vorgehensweise wird veranschaulicht, wie die Vorgänge Create @ no__t-0insert, Update und DELETE (CUD) eines Entitäts Typs mithilfe der Entity Framework Designer (EF-Designer) zu gespeicherten Prozeduren zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="a593f-103">This step-by-step walkthrough show how to map the create\\insert, update, and delete (CUD) operations of an entity type to stored procedures using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="a593f-104"> Standardmäßig generiert das Entity Framework automatisch die SQL-Anweisungen für die CUD-Vorgänge, Sie können jedoch auch gespeicherte Prozeduren zu diesen Vorgängen zuordnen.</span><span class="sxs-lookup"><span data-stu-id="a593f-104"> By default, the Entity Framework automatically generates the SQL statements for the CUD operations, but you can also map stored procedures to these operations.</span></span>  

<span data-ttu-id="a593f-105">Beachten Sie, dass Code First keine Zuordnung zu gespeicherten Prozeduren oder Funktionen unterstützt.</span><span class="sxs-lookup"><span data-stu-id="a593f-105">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="a593f-106">Allerdings können Sie gespeicherte Prozeduren oder Funktionen mithilfe der System. Data. Entity. dbset. sqlQuery-Methode abrufen.</span><span class="sxs-lookup"><span data-stu-id="a593f-106">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="a593f-107">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a593f-107">For example:</span></span>

``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a><span data-ttu-id="a593f-108">Überlegungen bei der Zuordnung der CUD-Vorgänge zu gespeicherten Prozeduren</span><span class="sxs-lookup"><span data-stu-id="a593f-108">Considerations when Mapping the CUD Operations to Stored Procedures</span></span>

<span data-ttu-id="a593f-109">Bei der Zuordnung der CUD-Vorgänge zu gespeicherten Prozeduren gelten die folgenden Überlegungen:</span><span class="sxs-lookup"><span data-stu-id="a593f-109">When mapping the CUD operations to stored procedures, the following considerations apply:</span></span>

- <span data-ttu-id="a593f-110">Wenn Sie einen der CUD-Vorgänge einer gespeicherten Prozedur zuordnen, ordnen Sie alle zu.</span><span class="sxs-lookup"><span data-stu-id="a593f-110">If you are mapping one of the CUD operations to a stored procedure, map all of them.</span></span> <span data-ttu-id="a593f-111">Wenn Sie nicht alle drei zuordnen, schlagen die nicht zugeordneten Vorgänge fehl, wenn Sie ausgeführt werden und eine **Update Exception**- ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="a593f-111">If you do not map all three, the unmapped operations will fail if executed and an **UpdateException** will be thrown.</span></span>
- <span data-ttu-id="a593f-112">Sie müssen den einzelnen Parametern der gespeicherten Prozedur Entitäts Eigenschaften zuordnen.</span><span class="sxs-lookup"><span data-stu-id="a593f-112">You must map every parameter of the stored procedure to entity properties.</span></span>
- <span data-ttu-id="a593f-113">Wenn der Server den Primärschlüssel Wert für die eingefügte Zeile generiert, müssen Sie diesen Wert wieder der Schlüsseleigenschaft der Entität zuordnen.</span><span class="sxs-lookup"><span data-stu-id="a593f-113">If the server generates the primary key value for the inserted row, you must map this value back to the entity's key property.</span></span> <span data-ttu-id="a593f-114">Im folgenden Beispiel gibt die gespeicherte Prozedur **InsertPerson** Den neu erstellten Primärschlüssel als Teil des Resultsets der gespeicherten Prozedur zurück.</span><span class="sxs-lookup"><span data-stu-id="a593f-114">In the example that follows, the **InsertPerson** stored procedure returns the newly created primary key as part of the stored procedure's result set.</span></span> <span data-ttu-id="a593f-115">Der Primärschlüssel wird dem Entitäts Schlüssel (**PersonID**) mithilfe des **&lt;ADD result Binding @ no__t-3** feature des EF-Designers zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="a593f-115">The primary key is mapped to the entity key (**PersonID**) using the **&lt;Add Result Bindings&gt;** feature of the EF Designer.</span></span>
- <span data-ttu-id="a593f-116">Die Aufrufe gespeicherter Prozeduren werden 1:1 mit den Entitäten im konzeptionellen Modell zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="a593f-116">The stored procedure calls are mapped 1:1 with the entities in the conceptual model.</span></span> <span data-ttu-id="a593f-117">Wenn Sie z. b. eine Vererbungs Hierarchie in ihrem konzeptionellen Modell implementieren und dann die gespeicherten CUD-Prozeduren für die übergeordneten **(Basis** -) und untergeordneten (abgeleiteten) Entitäten zuordnen, wird beim Speichern der untergeordneten **Änderungen nur** das unter **geordnete** **Element "** s gespeicherte Prozeduren, löst keine Aufrufe der gespeicherten Prozeduren des über **geordneten**Elements aus.</span><span class="sxs-lookup"><span data-stu-id="a593f-117">For example, if you implement an inheritance hierarchy in your conceptual model and then map the CUD stored procedures for the **Parent** (base) and the **Child** (derived) entities, saving the **Child** changes will only call the **Child**’s stored procedures, it will not trigger the **Parent**’s stored procedures calls.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a593f-118">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="a593f-118">Prerequisites</span></span>

<span data-ttu-id="a593f-119">Um die exemplarische Vorgehensweise nachzuvollziehen, benötigen Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="a593f-119">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="a593f-120">Eine aktuelle Version von Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a593f-120">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="a593f-121">Die [Beispieldatenbank "School](~/ef6/resources/school-database.md)".</span><span class="sxs-lookup"><span data-stu-id="a593f-121">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="a593f-122">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="a593f-122">Set up the Project</span></span>

- <span data-ttu-id="a593f-123">Öffnen Sie Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="a593f-123">Open Visual Studio 2012.</span></span>
- <span data-ttu-id="a593f-124">**Datei &gt; New-&gt;-Projekt** auswählen</span><span class="sxs-lookup"><span data-stu-id="a593f-124">Select **File-&gt; New -&gt; Project**</span></span>
- <span data-ttu-id="a593f-125">Klicken Sie im linken Bereich auf **Visual C @ no__t-1**, und wählen Sie dann die **Konsolen** Vorlage aus.</span><span class="sxs-lookup"><span data-stu-id="a593f-125">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
- <span data-ttu-id="a593f-126">Geben Sie " **cudsprocssample**"  Als Name ein.</span><span class="sxs-lookup"><span data-stu-id="a593f-126">Enter **CUDSProcsSample** as the name.</span></span>
- <span data-ttu-id="a593f-127">Wählen Sie **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="a593f-127">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="a593f-128">Erstellen eines Modells</span><span class="sxs-lookup"><span data-stu-id="a593f-128">Create a Model</span></span>

- <span data-ttu-id="a593f-129">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, und wählen Sie **Add-&gt; New Item**aus.</span><span class="sxs-lookup"><span data-stu-id="a593f-129">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
- <span data-ttu-id="a593f-130">Wählen Sie im linken Menü **Daten** aus, und wählen Sie dann im Bereich Vorlagen die Option **ADO.NET Entity Data Model** aus.</span><span class="sxs-lookup"><span data-stu-id="a593f-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
- <span data-ttu-id="a593f-131">Geben Sie für den Dateinamen " **cudsprocs. edmx** " ein, und klicken Sie dann auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="a593f-131">Enter **CUDSProcs.edmx** for the file name, and then click **Add**.</span></span>
- <span data-ttu-id="a593f-132">Wählen Sie im Dialogfeld Modell Inhalte auswählen die Option **aus Datenbank generieren aus**, und klicken Sie dann auf **weiter**.</span><span class="sxs-lookup"><span data-stu-id="a593f-132">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
- <span data-ttu-id="a593f-133">Klicken Sie auf **neue Verbindung**.</span><span class="sxs-lookup"><span data-stu-id="a593f-133">Click **New Connection**.</span></span> <span data-ttu-id="a593f-134">Geben Sie im Dialogfeld Verbindungs Eigenschaften den Servernamen ein (z. b. **(localdb\\) mssqllocaldb**), wählen Sie die Authentifizierungsmethode aus, geben Sie als Datenbanknamen **School** ein, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="a593f-134">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="a593f-135">Das Dialogfeld Wählen Sie Ihre Datenverbindung aus wird mit Ihrer Daten bankverbindungs Einstellung aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="a593f-135">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
- <span data-ttu-id="a593f-136">Wählen Sie im Dialogfeld Datenbankobjekte auswählen unter dem Knoten **Tabellen** Die **Person** -Tabelle aus.</span><span class="sxs-lookup"><span data-stu-id="a593f-136">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person** table.</span></span>
- <span data-ttu-id="a593f-137">Wählen Sie außerdem im Knoten **gespeicherte Prozeduren und Funktionen** die folgenden gespeicherten Prozeduren aus: **DeletePerson**, **InsertPerson**und **updateperson**.</span><span class="sxs-lookup"><span data-stu-id="a593f-137">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **DeletePerson**, **InsertPerson**, and **UpdatePerson**.</span></span>
- <span data-ttu-id="a593f-138">Ab Visual Studio 2012 unterstützt der EF-Designer den Massen Import gespeicherter Prozeduren.</span><span class="sxs-lookup"><span data-stu-id="a593f-138">Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures.</span></span> <span data-ttu-id="a593f-139">Die Option zum **importieren ausgewählter gespeicherter Prozeduren und Funktionen in das Entitäts Modell** ist standardmäßig aktiviert.</span><span class="sxs-lookup"><span data-stu-id="a593f-139">The **Import selected stored procedures and functions into the entity model** is checked by default.</span></span> <span data-ttu-id="a593f-140">Da in diesem Beispiel gespeicherte Prozeduren zum Einfügen, aktualisieren und Löschen von Entitäts Typen verwendet werden, die nicht importiert werden sollen, wird dieses Kontrollkästchen nicht mehr unternimmt.</span><span class="sxs-lookup"><span data-stu-id="a593f-140">Since in this example we have stored procedures that insert, update, and delete entity types, we do not want to import them and will uncheck this checkbox.</span></span>

    ![S-Prozeduren importieren](~/ef6/media/importsprocs.jpg)

- <span data-ttu-id="a593f-142">Klicken Sie auf **Finish**.</span><span class="sxs-lookup"><span data-stu-id="a593f-142">Click **Finish**.</span></span>
    <span data-ttu-id="a593f-143">Der EF-Designer, der eine Entwurfs Oberfläche zum Bearbeiten des Modells bereitstellt, wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a593f-143">The EF Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-the-person-entity-to-stored-procedures"></a><span data-ttu-id="a593f-144">Zuordnen der Person-Entität zu gespeicherten Prozeduren</span><span class="sxs-lookup"><span data-stu-id="a593f-144">Map the Person Entity to Stored Procedures</span></span>

- <span data-ttu-id="a593f-145">Klicken Sie mit der rechten Maustaste auf den Entitätstyp **Person** , und wählen Sie **Zuordnung der gespeicherten Prozedur**</span><span class="sxs-lookup"><span data-stu-id="a593f-145">Right-click the **Person** entity type and select **Stored Procedure Mapping**.</span></span>
- <span data-ttu-id="a593f-146">Die Zuordnungen gespeicherter Prozeduren werden in den **Mappingdetails** window angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a593f-146">The stored procedure mappings appear in the **Mapping Details** window.</span></span>
- <span data-ttu-id="a593f-147">Klicken Sie auf **&lt;select Function @ no__t-2**.</span><span class="sxs-lookup"><span data-stu-id="a593f-147">Click **&lt;Select Insert Function&gt;**.</span></span>
    <span data-ttu-id="a593f-148">Das Feld wird zu einer Dropdownliste mit den gespeicherten Prozeduren im Speichermodell, die Entitätstypen im konzeptionellen Modell zugeordnet werden können.</span><span class="sxs-lookup"><span data-stu-id="a593f-148">The field becomes a drop-down list of the stored procedures in the storage model that can be mapped to entity types in the conceptual model.</span></span>
    <span data-ttu-id="a593f-149">Wählen Sie **InsertPerson** In der Dropdown Liste aus.</span><span class="sxs-lookup"><span data-stu-id="a593f-149">Select **InsertPerson** from the drop-down list.</span></span>
- <span data-ttu-id="a593f-150">Es werden Standardmappings zwischen Parametern gespeicherter Prozeduren und Entitätseigenschaften angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a593f-150">Default mappings between stored procedure parameters and entity properties appear.</span></span> <span data-ttu-id="a593f-151">Beachten Sie, dass Pfeile die Mappingrichtung angeben: Eigenschaftswerte werden für Parameter gespeicherter Prozeduren bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="a593f-151">Note that arrows indicate the mapping direction: Property values are supplied to stored procedure parameters.</span></span>
- <span data-ttu-id="a593f-152">Klicken Sie **&lt;ADD result Binding @ no__t-2**.</span><span class="sxs-lookup"><span data-stu-id="a593f-152">Click **&lt;Add Result Binding&gt;**.</span></span>
- <span data-ttu-id="a593f-153">Geben Sie **NewPersonID**ein, den Namen des Parameters, der von der **InsertPerson** gespeicherten Prozedur zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="a593f-153">Type **NewPersonID**, the name of the parameter returned by the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="a593f-154">Stellen Sie sicher, dass Sie keine führenden oder nachfolgenden Leerzeichen eingeben.</span><span class="sxs-lookup"><span data-stu-id="a593f-154">Make sure not to type leading or trailing spaces.</span></span>
- <span data-ttu-id="a593f-155">Drücken **Sie Eingabe**Taste.</span><span class="sxs-lookup"><span data-stu-id="a593f-155">Press **Enter**.</span></span>
- <span data-ttu-id="a593f-156">Standardmäßig wird **NewPersonID** dem Entitäts Schlüssel **PersonID**zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="a593f-156">By default, **NewPersonID** is mapped to the entity key **PersonID**.</span></span> <span data-ttu-id="a593f-157">Beachten Sie, dass ein Pfeil die Richtung der Zuordnung angibt: Der Wert der Ergebnisspalte wird für die-Eigenschaft bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="a593f-157">Note that an arrow indicates the direction of the mapping: The value of the result column is supplied to the property.</span></span>

    ![Mappingdetails](~/ef6/media/mappingdetails.png)

- <span data-ttu-id="a593f-159">Klicken Sie **&lt;Wählen Sie Update Funktion @ no__t-2** aus, und wählen Sie in der resultierenden Dropdown Liste **updateperson** aus.</span><span class="sxs-lookup"><span data-stu-id="a593f-159">Click **&lt;Select Update Function&gt;** and select **UpdatePerson** from the resulting drop-down list.</span></span>
- <span data-ttu-id="a593f-160">Es werden Standardmappings zwischen Parametern gespeicherter Prozeduren und Entitätseigenschaften angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a593f-160">Default mappings between stored procedure parameters and entity properties appear.</span></span>
- <span data-ttu-id="a593f-161">Klicken Sie **&lt;Wählen Sie Delete Function @ no__t-2** aus, und wählen Sie in der resultierenden Dropdown Liste die Option **DeletePerson** aus.</span><span class="sxs-lookup"><span data-stu-id="a593f-161">Click **&lt;Select Delete Function&gt;** and select **DeletePerson** from the resulting drop-down list.</span></span>
- <span data-ttu-id="a593f-162">Es werden Standardmappings zwischen Parametern gespeicherter Prozeduren und Entitätseigenschaften angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a593f-162">Default mappings between stored procedure parameters and entity properties appear.</span></span>

<span data-ttu-id="a593f-163">Die Einfüge-, Aktualisierungs-und Löschvorgänge des Entitäts Typs **Person**  sind nun gespeicherten Prozeduren zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="a593f-163">The insert, update, and delete operations of the **Person** entity type are now mapped to stored procedures.</span></span>

<span data-ttu-id="a593f-164">Wenn Sie die Parallelitäts Überprüfung beim Aktualisieren oder Löschen einer Entität mit gespeicherten Prozeduren aktivieren möchten, verwenden Sie eine der folgenden Optionen:</span><span class="sxs-lookup"><span data-stu-id="a593f-164">If you want to enable concurrency checking when updating or deleting an entity with stored procedures, use one of the following options:</span></span>

- <span data-ttu-id="a593f-165">Verwenden Sie einen **Output** -Parameter, um die Anzahl der betroffenen Zeilen von der gespeicherten Prozedur zurückzugeben, und überprüfen Sie den **betroffenen Zeilen Parameter** checkbox neben dem Parameternamen.</span><span class="sxs-lookup"><span data-stu-id="a593f-165">Use an **OUTPUT** parameter to return the number of affected rows from the stored procedure and check the **Rows Affected Parameter** checkbox next to the parameter name.</span></span> <span data-ttu-id="a593f-166">Wenn der zurückgegebene Wert 0 (null) ist, wenn der Vorgang aufgerufen wird, wird eine  [**optimistic-** ](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx)Ereignis   ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="a593f-166">If the value returned is zero when the operation is called, an  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) will be thrown.</span></span>
- <span data-ttu-id="a593f-167">Aktivieren Sie das Kontrollkästchen **ursprünglichen Wert verwenden** neben einer Eigenschaft, die Sie für die Parallelitäts Überprüfung verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="a593f-167">Check the **Use Original Value** checkbox next to a property that you want to use for concurrency checking.</span></span> <span data-ttu-id="a593f-168">Wenn versucht wird, ein Update auszuführen, wird der Wert der Eigenschaft, die ursprünglich aus der Datenbank gelesen wurde, beim Zurückschreiben von Daten in die Datenbank verwendet.</span><span class="sxs-lookup"><span data-stu-id="a593f-168">When an update is attempted, the value of the property that was originally read from the database will be used when writing data back to the database.</span></span> <span data-ttu-id="a593f-169">Wenn der Wert nicht mit dem Wert in der Datenbank identisch ist, wird eine **optimistic-** Ereignis  ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="a593f-169">If the value does not match the value in the database, an **OptimisticConcurrencyException** will be thrown.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="a593f-170">Verwenden des Modells</span><span class="sxs-lookup"><span data-stu-id="a593f-170">Use the Model</span></span>

<span data-ttu-id="a593f-171">Öffnen Sie die Datei **Program.cs** , in der die **Main** -Methode definiert ist.</span><span class="sxs-lookup"><span data-stu-id="a593f-171">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="a593f-172">Fügen Sie der Main-Funktion den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="a593f-172">Add the following code into the Main function.</span></span>

<span data-ttu-id="a593f-173">Der Code erstellt ein neues **Person** -Objekt, aktualisiert dann das-Objekt und löscht schließlich das-Objekt.</span><span class="sxs-lookup"><span data-stu-id="a593f-173">The code creates a new **Person** object, then updates the object, and finally deletes the object.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        var newInstructor = new Person
        {
            FirstName = "Robyn",
            LastName = "Martin",
            HireDate = DateTime.Now,
            Discriminator = "Instructor"
        }

        // Add the new object to the context.
        context.People.Add(newInstructor);

        Console.WriteLine("Added {0} {1} to the context.",
            newInstructor.FirstName, newInstructor.LastName);

        Console.WriteLine("Before SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // SaveChanges will call the InsertPerson sproc.  
        // The PersonID property will be assigned the value
        // returned by the sproc.
        context.SaveChanges();

        Console.WriteLine("After SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // Modify the object and call SaveChanges.
        // This time, the UpdatePerson will be called.
        newInstructor.FirstName = "Rachel";
        context.SaveChanges();

        // Remove the object from the context and call SaveChanges.
        // The DeletePerson sproc will be called.
        context.People.Remove(newInstructor);
        context.SaveChanges();

        Person deletedInstructor = context.People.
            Where(p => p.PersonID == newInstructor.PersonID).
            FirstOrDefault();

        if (deletedInstructor == null)
            Console.WriteLine("A person with PersonID {0} was deleted.",
                newInstructor.PersonID);
    }
```

- <span data-ttu-id="a593f-174">Kompilieren Sie die Anwendung, und führen Sie sie aus.</span><span class="sxs-lookup"><span data-stu-id="a593f-174">Compile and run the application.</span></span> <span data-ttu-id="a593f-175">Das Programm erzeugt die folgende Ausgabe \*</span><span class="sxs-lookup"><span data-stu-id="a593f-175">The program produces the following output \*</span></span>

> [!NOTE]
> <span data-ttu-id="a593f-176">PersonID wird vom Server automatisch generiert, sodass Sie wahrscheinlich eine andere Anzahl sehen \*</span><span class="sxs-lookup"><span data-stu-id="a593f-176">PersonID is auto-generated by the server, so you will most likely see a different number\*</span></span>

``` Output
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

<span data-ttu-id="a593f-177">Wenn Sie mit der Ultimate-Version von Visual Studio arbeiten, können Sie IntelliTrace mit dem Debugger verwenden, um die ausgeführten SQL-Anweisungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="a593f-177">If you are working with the Ultimate version of Visual Studio, you can use Intellitrace with the debugger to see the SQL statements that get executed.</span></span>

![IntelliTrace](~/ef6/media/intellitrace.png)
