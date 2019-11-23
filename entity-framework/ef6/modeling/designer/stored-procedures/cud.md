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
# <a name="designer-cud-stored-procedures"></a><span data-ttu-id="da670-102">Designer-CUD-Prozeduren</span><span class="sxs-lookup"><span data-stu-id="da670-102">Designer CUD Stored Procedures</span></span>

<span data-ttu-id="da670-103">In dieser schrittweisen exemplarischen Vorgehensweise wird veranschaulicht, wie die Vorgänge Create\\INSERT, Update und DELETE (CUD) eines Entitäts Typs mithilfe des Entity Framework Designer (EF-Designers) zu gespeicherten Prozeduren zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="da670-103">This step-by-step walkthrough show how to map the create\\insert, update, and delete (CUD) operations of an entity type to stored procedures using the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="da670-104"> Standardmäßig generiert das Entity Framework automatisch die SQL-Anweisungen für die CUD-Vorgänge, Sie können jedoch auch gespeicherte Prozeduren zu diesen Vorgängen zuordnen.</span><span class="sxs-lookup"><span data-stu-id="da670-104"> By default, the Entity Framework automatically generates the SQL statements for the CUD operations, but you can also map stored procedures to these operations.</span></span>  

<span data-ttu-id="da670-105">Beachten Sie, dass Code First keine Zuordnung zu gespeicherten Prozeduren oder Funktionen unterstützt.</span><span class="sxs-lookup"><span data-stu-id="da670-105">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="da670-106">Allerdings können Sie gespeicherte Prozeduren oder Funktionen mithilfe der System. Data. Entity. dbset. sqlQuery-Methode abrufen.</span><span class="sxs-lookup"><span data-stu-id="da670-106">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="da670-107">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="da670-107">For example:</span></span>

``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a><span data-ttu-id="da670-108">Überlegungen bei der Zuordnung der CUD-Vorgänge zu gespeicherten Prozeduren</span><span class="sxs-lookup"><span data-stu-id="da670-108">Considerations when Mapping the CUD Operations to Stored Procedures</span></span>

<span data-ttu-id="da670-109">Bei der Zuordnung der CUD-Vorgänge zu gespeicherten Prozeduren gelten die folgenden Überlegungen:</span><span class="sxs-lookup"><span data-stu-id="da670-109">When mapping the CUD operations to stored procedures, the following considerations apply:</span></span>

- <span data-ttu-id="da670-110">Wenn Sie einen der CUD-Vorgänge einer gespeicherten Prozedur zuordnen, ordnen Sie alle zu.</span><span class="sxs-lookup"><span data-stu-id="da670-110">If you are mapping one of the CUD operations to a stored procedure, map all of them.</span></span> <span data-ttu-id="da670-111">Wenn Sie nicht alle drei zuordnen, schlagen die nicht zugeordneten Vorgänge fehl, wenn Sie ausgeführt werden und ein **Update Exception** - ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="da670-111">If you do not map all three, the unmapped operations will fail if executed and an **UpdateException** will be thrown.</span></span>
- <span data-ttu-id="da670-112">Sie müssen den einzelnen Parametern der gespeicherten Prozedur Entitäts Eigenschaften zuordnen.</span><span class="sxs-lookup"><span data-stu-id="da670-112">You must map every parameter of the stored procedure to entity properties.</span></span>
- <span data-ttu-id="da670-113">Wenn der Server den Primärschlüssel Wert für die eingefügte Zeile generiert, müssen Sie diesen Wert wieder der Schlüsseleigenschaft der Entität zuordnen.</span><span class="sxs-lookup"><span data-stu-id="da670-113">If the server generates the primary key value for the inserted row, you must map this value back to the entity's key property.</span></span> <span data-ttu-id="da670-114">Im folgenden Beispiel gibt die gespeicherte Prozedur **InsertPerson** den neu erstellten Primärschlüssel als Teil des Resultsets der gespeicherten Prozedur zurück.</span><span class="sxs-lookup"><span data-stu-id="da670-114">In the example that follows, the **InsertPerson** stored procedure returns the newly created primary key as part of the stored procedure's result set.</span></span> <span data-ttu-id="da670-115">Der Primärschlüssel wird dem Entitäts Schlüssel (**PersonID**) mithilfe der **&lt;Hinzufügen von Ergebnis Bindungen&gt;**  Features des EF-Designers zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="da670-115">The primary key is mapped to the entity key (**PersonID**) using the **&lt;Add Result Bindings&gt;** feature of the EF Designer.</span></span>
- <span data-ttu-id="da670-116">Die Aufrufe gespeicherter Prozeduren werden 1:1 mit den Entitäten im konzeptionellen Modell zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="da670-116">The stored procedure calls are mapped 1:1 with the entities in the conceptual model.</span></span> <span data-ttu-id="da670-117">Wenn Sie z. b. eine Vererbungs Hierarchie in ihrem konzeptionellen Modell implementieren und dann die gespeicherten CUD-Prozeduren für die übergeordneten (Basis-) **und unter** **geordneten** (abgeleiteten) Entitäten zuordnen, werden beim Speichern der unter **geordneten Änderungen nur** die gespeicherten Prozeduren **des unter** **geordneten**Elements aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="da670-117">For example, if you implement an inheritance hierarchy in your conceptual model and then map the CUD stored procedures for the **Parent** (base) and the **Child** (derived) entities, saving the **Child** changes will only call the **Child**’s stored procedures, it will not trigger the **Parent**’s stored procedures calls.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="da670-118">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="da670-118">Prerequisites</span></span>

<span data-ttu-id="da670-119">Um die exemplarische Vorgehensweise nachzuvollziehen, benötigen Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="da670-119">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="da670-120">Eine aktuelle Version von Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="da670-120">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="da670-121">Die [Beispieldatenbank "School](~/ef6/resources/school-database.md)".</span><span class="sxs-lookup"><span data-stu-id="da670-121">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="da670-122">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="da670-122">Set up the Project</span></span>

- <span data-ttu-id="da670-123">Öffnen Sie Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="da670-123">Open Visual Studio 2012.</span></span>
- <span data-ttu-id="da670-124">**Datei&gt; neues&gt; Projekt** auswählen</span><span class="sxs-lookup"><span data-stu-id="da670-124">Select **File-&gt; New -&gt; Project**</span></span>
- <span data-ttu-id="da670-125">Klicken Sie im linken Bereich auf **Visual C-\#** , und wählen Sie dann die **Konsolen** Vorlage aus.</span><span class="sxs-lookup"><span data-stu-id="da670-125">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
- <span data-ttu-id="da670-126">Geben Sie " **cudsprocssample** " als Namen ein.</span><span class="sxs-lookup"><span data-stu-id="da670-126">Enter **CUDSProcsSample** as the name.</span></span>
- <span data-ttu-id="da670-127">Wählen Sie **OK**aus.</span><span class="sxs-lookup"><span data-stu-id="da670-127">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="da670-128">Erstellen eines Modells</span><span class="sxs-lookup"><span data-stu-id="da670-128">Create a Model</span></span>

- <span data-ttu-id="da670-129">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, und wählen Sie **Hinzufügen-&gt; neues Element**aus.</span><span class="sxs-lookup"><span data-stu-id="da670-129">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
- <span data-ttu-id="da670-130">Wählen Sie im linken Menü **Daten** aus, und wählen Sie dann im Bereich Vorlagen die Option **ADO.NET Entity Data Model** aus.</span><span class="sxs-lookup"><span data-stu-id="da670-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
- <span data-ttu-id="da670-131">Geben Sie für den Dateinamen " **cudsprocs. edmx** " ein, und klicken Sie dann auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="da670-131">Enter **CUDSProcs.edmx** for the file name, and then click **Add**.</span></span>
- <span data-ttu-id="da670-132">Wählen Sie im Dialogfeld Modell Inhalte auswählen die Option **aus Datenbank generieren aus**, und klicken Sie dann auf **weiter**.</span><span class="sxs-lookup"><span data-stu-id="da670-132">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
- <span data-ttu-id="da670-133">Klicken Sie auf **neue Verbindung**.</span><span class="sxs-lookup"><span data-stu-id="da670-133">Click **New Connection**.</span></span> <span data-ttu-id="da670-134">Geben Sie im Dialogfeld Verbindungs Eigenschaften den Servernamen ein (z. b. **(localdb)\\mssqllocaldb**), wählen Sie die Authentifizierungsmethode aus, geben Sie **School** als Datenbanknamen ein, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="da670-134">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="da670-135">Das Dialogfeld Wählen Sie Ihre Datenverbindung aus wird mit Ihrer Daten bankverbindungs Einstellung aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="da670-135">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
- <span data-ttu-id="da670-136">Wählen Sie im Dialogfeld Wählen Sie Ihre Datenbankobjekte aus unter dem Knoten **Tabellen** die **Person** -Tabelle aus.</span><span class="sxs-lookup"><span data-stu-id="da670-136">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person** table.</span></span>
- <span data-ttu-id="da670-137">Wählen Sie außerdem die folgenden gespeicherten Prozeduren unter dem Knoten **gespeicherte Prozeduren und Funktionen** aus: **DeletePerson**, **InsertPerson**und **updateperson**.</span><span class="sxs-lookup"><span data-stu-id="da670-137">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **DeletePerson**, **InsertPerson**, and **UpdatePerson**.</span></span>
- <span data-ttu-id="da670-138">Ab Visual Studio 2012 unterstützt der EF-Designer den Massen Import gespeicherter Prozeduren.</span><span class="sxs-lookup"><span data-stu-id="da670-138">Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures.</span></span> <span data-ttu-id="da670-139">Die Option zum **importieren ausgewählter gespeicherter Prozeduren und Funktionen in das Entitäts Modell** ist standardmäßig aktiviert.</span><span class="sxs-lookup"><span data-stu-id="da670-139">The **Import selected stored procedures and functions into the entity model** is checked by default.</span></span> <span data-ttu-id="da670-140">Da in diesem Beispiel gespeicherte Prozeduren zum Einfügen, aktualisieren und Löschen von Entitäts Typen verwendet werden, die nicht importiert werden sollen, wird dieses Kontrollkästchen nicht mehr unternimmt.</span><span class="sxs-lookup"><span data-stu-id="da670-140">Since in this example we have stored procedures that insert, update, and delete entity types, we do not want to import them and will uncheck this checkbox.</span></span>

    ![S-Prozeduren importieren](~/ef6/media/importsprocs.jpg)

- <span data-ttu-id="da670-142">Klicken Sie auf **Finish**.</span><span class="sxs-lookup"><span data-stu-id="da670-142">Click **Finish**.</span></span>
    <span data-ttu-id="da670-143">Der EF-Designer, der eine Entwurfs Oberfläche zum Bearbeiten des Modells bereitstellt, wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="da670-143">The EF Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-the-person-entity-to-stored-procedures"></a><span data-ttu-id="da670-144">Zuordnen der Person-Entität zu gespeicherten Prozeduren</span><span class="sxs-lookup"><span data-stu-id="da670-144">Map the Person Entity to Stored Procedures</span></span>

- <span data-ttu-id="da670-145">Klicken Sie mit der rechten Maustaste auf den Entitätstyp **Person** und wählen Sie **Zuordnung der gespeicherten Prozedur**</span><span class="sxs-lookup"><span data-stu-id="da670-145">Right-click the **Person** entity type and select **Stored Procedure Mapping**.</span></span>
- <span data-ttu-id="da670-146">Die Zuordnungen gespeicherter Prozeduren werden im Fenster **Mappingdetails** angezeigt.</span><span class="sxs-lookup"><span data-stu-id="da670-146">The stored procedure mappings appear in the **Mapping Details** window.</span></span>
- <span data-ttu-id="da670-147">Klicken Sie auf **&lt;INSERT-Funktion&gt;** .</span><span class="sxs-lookup"><span data-stu-id="da670-147">Click **&lt;Select Insert Function&gt;**.</span></span>
    <span data-ttu-id="da670-148">Das Feld wird zu einer Dropdownliste mit den gespeicherten Prozeduren im Speichermodell, die Entitätstypen im konzeptionellen Modell zugeordnet werden können.</span><span class="sxs-lookup"><span data-stu-id="da670-148">The field becomes a drop-down list of the stored procedures in the storage model that can be mapped to entity types in the conceptual model.</span></span>
    <span data-ttu-id="da670-149">Wählen Sie in der Dropdown Liste **InsertPerson** - aus.</span><span class="sxs-lookup"><span data-stu-id="da670-149">Select **InsertPerson** from the drop-down list.</span></span>
- <span data-ttu-id="da670-150">Es werden Standardmappings zwischen Parametern gespeicherter Prozeduren und Entitätseigenschaften angezeigt.</span><span class="sxs-lookup"><span data-stu-id="da670-150">Default mappings between stored procedure parameters and entity properties appear.</span></span> <span data-ttu-id="da670-151">Pfeile geben die Mappingrichtung an: Eigenschaftswerte sind Parametern gespeicherter Prozeduren zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="da670-151">Note that arrows indicate the mapping direction: Property values are supplied to stored procedure parameters.</span></span>
- <span data-ttu-id="da670-152">Klicken Sie auf \*\*&lt;Ergebnis Bindungs&gt;hinzufügen \*\*.</span><span class="sxs-lookup"><span data-stu-id="da670-152">Click **&lt;Add Result Binding&gt;**.</span></span>
- <span data-ttu-id="da670-153">Geben Sie **NewPersonID**ein, den Namen des Parameters, der vom **InsertPerson** - gespeicherten Prozedur zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="da670-153">Type **NewPersonID**, the name of the parameter returned by the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="da670-154">Stellen Sie sicher, dass Sie keine führenden oder nachfolgenden Leerzeichen eingeben.</span><span class="sxs-lookup"><span data-stu-id="da670-154">Make sure not to type leading or trailing spaces.</span></span>
- <span data-ttu-id="da670-155">Drücken **Sie Eingabe**Taste.</span><span class="sxs-lookup"><span data-stu-id="da670-155">Press **Enter**.</span></span>
- <span data-ttu-id="da670-156">Standardmäßig wird **NewPersonID** dem Entitäts Schlüssel **PersonID**zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="da670-156">By default, **NewPersonID** is mapped to the entity key **PersonID**.</span></span> <span data-ttu-id="da670-157">Ein Pfeil gibt die Mappingrichtung an: Der Wert der Ergebnisspalte ist der Eigenschaft zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="da670-157">Note that an arrow indicates the direction of the mapping: The value of the result column is supplied to the property.</span></span>

    ![Mappingdetails](~/ef6/media/mappingdetails.png)

- <span data-ttu-id="da670-159">Klicken Sie **&lt;wählen Sie &gt;Funktion aktualisieren** aus, und wählen Sie in der resultierenden Dropdown Liste **updateperson** aus.</span><span class="sxs-lookup"><span data-stu-id="da670-159">Click **&lt;Select Update Function&gt;** and select **UpdatePerson** from the resulting drop-down list.</span></span>
- <span data-ttu-id="da670-160">Es werden Standardmappings zwischen Parametern gespeicherter Prozeduren und Entitätseigenschaften angezeigt.</span><span class="sxs-lookup"><span data-stu-id="da670-160">Default mappings between stored procedure parameters and entity properties appear.</span></span>
- <span data-ttu-id="da670-161">Klicken Sie **&lt;klicken Sie auf Funktion löschen&gt;**  , und wählen Sie in der resultierenden Dropdown Liste die Option **DeletePerson** aus.</span><span class="sxs-lookup"><span data-stu-id="da670-161">Click **&lt;Select Delete Function&gt;** and select **DeletePerson** from the resulting drop-down list.</span></span>
- <span data-ttu-id="da670-162">Es werden Standardmappings zwischen Parametern gespeicherter Prozeduren und Entitätseigenschaften angezeigt.</span><span class="sxs-lookup"><span data-stu-id="da670-162">Default mappings between stored procedure parameters and entity properties appear.</span></span>

<span data-ttu-id="da670-163">Die Einfüge-, Aktualisierungs-und Löschvorgänge der **Person** Entitätstyp werden nun gespeicherten Prozeduren zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="da670-163">The insert, update, and delete operations of the **Person** entity type are now mapped to stored procedures.</span></span>

<span data-ttu-id="da670-164">Wenn Sie die Parallelitäts Überprüfung beim Aktualisieren oder Löschen einer Entität mit gespeicherten Prozeduren aktivieren möchten, verwenden Sie eine der folgenden Optionen:</span><span class="sxs-lookup"><span data-stu-id="da670-164">If you want to enable concurrency checking when updating or deleting an entity with stored procedures, use one of the following options:</span></span>

- <span data-ttu-id="da670-165">Verwenden Sie einen **Output** -Parameter, um die Anzahl der betroffenen Zeilen von der gespeicherten Prozedur zurückzugeben, und aktivieren Sie das Kontrollkästchen **betroffene Zeilen** Kontrollkästchen neben dem Parameternamen.</span><span class="sxs-lookup"><span data-stu-id="da670-165">Use an **OUTPUT** parameter to return the number of affected rows from the stored procedure and check the **Rows Affected Parameter** checkbox next to the parameter name.</span></span> <span data-ttu-id="da670-166">Wenn der zurückgegebene Wert 0 (null) ist, wenn der Vorgang aufgerufen wird, wird ein  [**optimistic-** ](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) Ereignis ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="da670-166">If the value returned is zero when the operation is called, an  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) will be thrown.</span></span>
- <span data-ttu-id="da670-167">Aktivieren Sie das Kontrollkästchen **ursprünglichen Wert verwenden** neben einer Eigenschaft, die Sie für die Parallelitäts Überprüfung verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="da670-167">Check the **Use Original Value** checkbox next to a property that you want to use for concurrency checking.</span></span> <span data-ttu-id="da670-168">Wenn versucht wird, ein Update auszuführen, wird der Wert der Eigenschaft, die ursprünglich aus der Datenbank gelesen wurde, beim Zurückschreiben von Daten in die Datenbank verwendet.</span><span class="sxs-lookup"><span data-stu-id="da670-168">When an update is attempted, the value of the property that was originally read from the database will be used when writing data back to the database.</span></span> <span data-ttu-id="da670-169">Wenn der Wert nicht mit dem Wert in der Datenbank identisch ist, wird eine **optimistic-** Ereignis ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="da670-169">If the value does not match the value in the database, an **OptimisticConcurrencyException** will be thrown.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="da670-170">Verwenden des Modells</span><span class="sxs-lookup"><span data-stu-id="da670-170">Use the Model</span></span>

<span data-ttu-id="da670-171">Öffnen Sie die Datei **Program.cs** , in der die **Main** -Methode definiert ist.</span><span class="sxs-lookup"><span data-stu-id="da670-171">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="da670-172">Fügen Sie der Main-Funktion den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="da670-172">Add the following code into the Main function.</span></span>

<span data-ttu-id="da670-173">Der Code erstellt ein neues **Person** -Objekt, aktualisiert dann das-Objekt und löscht schließlich das-Objekt.</span><span class="sxs-lookup"><span data-stu-id="da670-173">The code creates a new **Person** object, then updates the object, and finally deletes the object.</span></span>

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

- <span data-ttu-id="da670-174">Kompilieren Sie die Anwendung, und führen Sie sie aus.</span><span class="sxs-lookup"><span data-stu-id="da670-174">Compile and run the application.</span></span> <span data-ttu-id="da670-175">Das Programm erzeugt die folgende Ausgabe \*</span><span class="sxs-lookup"><span data-stu-id="da670-175">The program produces the following output \*</span></span>

> [!NOTE]
> <span data-ttu-id="da670-176">PersonID wird vom Server automatisch generiert, sodass Sie wahrscheinlich eine andere Anzahl sehen \*</span><span class="sxs-lookup"><span data-stu-id="da670-176">PersonID is auto-generated by the server, so you will most likely see a different number\*</span></span>

``` Output
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

<span data-ttu-id="da670-177">Wenn Sie mit der Ultimate-Version von Visual Studio arbeiten, können Sie IntelliTrace mit dem Debugger verwenden, um die ausgeführten SQL-Anweisungen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="da670-177">If you are working with the Ultimate version of Visual Studio, you can use Intellitrace with the debugger to see the SQL statements that get executed.</span></span>

![IntelliTrace](~/ef6/media/intellitrace.png)
