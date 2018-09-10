---
title: Designer CUD gespeicherte Prozeduren – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: 36c9b97b77fec30136cba1d850a0259c689e69ae
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250919"
---
# <a name="designer-cud-stored-procedures"></a><span data-ttu-id="25742-102">Designer CRUD-gespeicherte Prozeduren</span><span class="sxs-lookup"><span data-stu-id="25742-102">Designer CUD Stored Procedures</span></span>
<span data-ttu-id="25742-103">Dieser schrittweise erläuterten exemplarischen zeigen, wie Sie ordnen den erstellen\\einfügen, aktualisieren und Löschvorgänge eines Entitätstyps zu gespeicherten Prozeduren, die mit dem Entity Framework Designer (EF-Designer) (CRUD).</span><span class="sxs-lookup"><span data-stu-id="25742-103">This step-by-step walkthrough show how to map the create\\insert, update, and delete (CUD) operations of an entity type to stored procedures using the Entity Framework Designer (EF Designer).</span></span>  <span data-ttu-id="25742-104">Standardmäßig generiert Entity Framework automatisch die SQL-Anweisungen für die CRUD-Vorgänge, aber Sie können auch gespeicherte Prozeduren zuordnen, um diese Vorgänge.</span><span class="sxs-lookup"><span data-stu-id="25742-104">By default, the Entity Framework automatically generates the SQL statements for the CUD operations, but you can also map stored procedures to these operations.</span></span>  

<span data-ttu-id="25742-105">Beachten Sie, dass Code First-Zuordnung zu gespeicherten Prozeduren oder Funktionen nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="25742-105">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="25742-106">Allerdings können Sie gespeicherte Prozeduren oder Funktionen mithilfe der System.Data.Entity.DbSet.SqlQuery-Methode aufrufen.</span><span class="sxs-lookup"><span data-stu-id="25742-106">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="25742-107">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="25742-107">For example:</span></span>
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a><span data-ttu-id="25742-108">Überlegungen bei der Zuordnung die CUD-Vorgänge für gespeicherte Prozeduren</span><span class="sxs-lookup"><span data-stu-id="25742-108">Considerations when Mapping the CUD Operations to Stored Procedures</span></span>

<span data-ttu-id="25742-109">Beim Zuordnen von CRUD-Vorgänge zu gespeicherten Prozeduren gelten die folgenden Überlegungen:</span><span class="sxs-lookup"><span data-stu-id="25742-109">When mapping the CUD operations to stored procedures, the following considerations apply:</span></span> 

-   <span data-ttu-id="25742-110">Wenn Sie eines der CRUD-Operationen zu einer gespeicherten Prozedur zuordnen, ordnen Sie alle.</span><span class="sxs-lookup"><span data-stu-id="25742-110">If you are mapping one of the CUD operations to a stored procedure, map all of them.</span></span> <span data-ttu-id="25742-111">Wenn Sie nicht alle drei zuordnen, die nicht zugeordneten Vorgänge fehl, wenn ausgeführt, und eine **"UpdateException"** ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="25742-111">If you do not map all three, the unmapped operations will fail if executed and an **UpdateException** will be thrown.</span></span>
-   <span data-ttu-id="25742-112">Sie müssen jeden Parameter der gespeicherten Prozedur, an Entitätseigenschaften zuordnen.</span><span class="sxs-lookup"><span data-stu-id="25742-112">You must map every parameter of the stored procedure to entity properties.</span></span>
-   <span data-ttu-id="25742-113">Wenn der Server den Primärschlüsselwert für die eingefügte Zeile generiert, müssen Sie diesen Wert an die Schlüsseleigenschaft der Entität zuordnen.</span><span class="sxs-lookup"><span data-stu-id="25742-113">If the server generates the primary key value for the inserted row, you must map this value back to the entity's key property.</span></span> <span data-ttu-id="25742-114">In der folgenden Beispiel die **InsertPerson** gespeicherte Prozedur gibt die neu erstellten Primärschlüssel als Teil des Resultsets für die gespeicherte Prozedur zurück.</span><span class="sxs-lookup"><span data-stu-id="25742-114">In the example that follows, the **InsertPerson** stored procedure returns the newly created primary key as part of the stored procedure's result set.</span></span> <span data-ttu-id="25742-115">Der Entitätsschlüssel der primäre Schlüssel zugeordnet ist (**PersonID**) mithilfe der **&lt;Result Binding hinzufügen&gt;** Feature von EF Designer.</span><span class="sxs-lookup"><span data-stu-id="25742-115">The primary key is mapped to the entity key (**PersonID**) using the **&lt;Add Result Bindings&gt;** feature of the EF Designer.</span></span>
-   <span data-ttu-id="25742-116">Aufrufe der gespeicherten Prozedur werden die zugeordneten 1:1 mit den Entitäten im konzeptionellen Modell.</span><span class="sxs-lookup"><span data-stu-id="25742-116">The stored procedure calls are mapped 1:1 with the entities in the conceptual model.</span></span> <span data-ttu-id="25742-117">Z. B. Wenn Sie eine Vererbungshierarchie in Ihrem konzeptionellen Modell und klicken Sie dann Zuordnung implementieren die CUD-Prozeduren für die **übergeordneten** (Basis-) und die **untergeordneten** (abgeleitete) Entitäten, die beim Speichern der **Untergeordneten** Änderungen ruft nur die **untergeordneten**die gespeicherten Prozeduren, nicht löst die **übergeordneten**die gespeicherten Prozeduren aufrufen.</span><span class="sxs-lookup"><span data-stu-id="25742-117">For example, if you implement an inheritance hierarchy in your conceptual model and then map the CUD stored procedures for the **Parent** (base) and the **Child** (derived) entities, saving the **Child** changes will only call the **Child**’s stored procedures, it will not trigger the **Parent**’s stored procedures calls.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="25742-118">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="25742-118">Prerequisites</span></span>

<span data-ttu-id="25742-119">Um die exemplarische Vorgehensweise nachzuvollziehen, benötigen Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="25742-119">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="25742-120">Eine aktuelle Version von Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="25742-120">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="25742-121">Die [Beispieldatenbank ' School '](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="25742-121">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="25742-122">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="25742-122">Set up the Project</span></span>

-   <span data-ttu-id="25742-123">Öffnen Sie Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="25742-123">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="25742-124">Wählen Sie **Dateien&gt; neu –&gt; Projekt**</span><span class="sxs-lookup"><span data-stu-id="25742-124">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="25742-125">Klicken Sie im linken Bereich auf **Visual C\#**, und wählen Sie dann die **Konsole** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="25742-125">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="25742-126">Geben Sie **CUDSProcsSample** als Namen.</span><span class="sxs-lookup"><span data-stu-id="25742-126">Enter **CUDSProcsSample** as the name.</span></span>
-   <span data-ttu-id="25742-127">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="25742-127">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="25742-128">Erstellen eines Modells</span><span class="sxs-lookup"><span data-stu-id="25742-128">Create a Model</span></span>

-   <span data-ttu-id="25742-129">Mit der rechten Maustaste des Projektnamen im Projektmappen-Explorer, und wählen **hinzufügen –&gt; neues Element**.</span><span class="sxs-lookup"><span data-stu-id="25742-129">Right-click the project name in Solution Explorer, and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="25742-130">Wählen Sie **Daten** aus dem linken Menü und wählen Sie dann **ADO.NET Entity Data Model** im Bereich Vorlagen.</span><span class="sxs-lookup"><span data-stu-id="25742-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="25742-131">Geben Sie **CUDSProcs.edmx** für den Dateinamen ein, und klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="25742-131">Enter **CUDSProcs.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="25742-132">Wählen Sie im Dialogfeld "Modellinhalte" **aus Datenbank generieren**, und klicken Sie dann auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="25742-132">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="25742-133">Klicken Sie auf **neue Verbindung**.</span><span class="sxs-lookup"><span data-stu-id="25742-133">Click **New Connection**.</span></span> <span data-ttu-id="25742-134">Geben Sie den Servernamen, klicken Sie im Dialogfeld "Verbindungseigenschaften" (z. B. **(Localdb)\\Mssqllocaldb**), wählen die Authentifizierungsmethode, Typ **School** für den Datenbanknamen, und klicken Sie dann Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="25742-134">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="25742-135">Das Dialogfeld "Wählen Sie Ihre Datenverbindung" wird mit Ihrem datenbankverbindungseinstellung aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="25742-135">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="25742-136">Auswählen Ihrer Datenbankobjekte Dialogfeld unter die **Tabellen** Knoten die **Person** Tabelle.</span><span class="sxs-lookup"><span data-stu-id="25742-136">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person** table.</span></span>
-   <span data-ttu-id="25742-137">Wählen Sie außerdem die folgenden gespeicherten Prozeduren in der **gespeicherte Prozeduren und Funktionen** Knoten: **DeletePerson**, **InsertPerson**, und **UpdatePerson** .</span><span class="sxs-lookup"><span data-stu-id="25742-137">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **DeletePerson**, **InsertPerson**, and **UpdatePerson**.</span></span> 
-   <span data-ttu-id="25742-138">Ab Visual Studio 2012 die EF-Designer unterstützt Massenexport und-Import von gespeicherten Prozeduren.</span><span class="sxs-lookup"><span data-stu-id="25742-138">Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures.</span></span> <span data-ttu-id="25742-139">Die **Import ausgewählten gespeicherten Prozeduren und Funktionen in das Entitätsmodell** ist standardmäßig aktiviert.</span><span class="sxs-lookup"><span data-stu-id="25742-139">The **Import selected stored procedures and functions into the entity model** is checked by default.</span></span> <span data-ttu-id="25742-140">Da in diesem Beispiel wir Prozeduren, die einfügen gespeichert haben, aktualisieren und Löschen von Entitätstypen, wir nicht importieren möchten, und es wird, deaktivieren Sie dieses Kontrollkästchen.</span><span class="sxs-lookup"><span data-stu-id="25742-140">Since in this example we have stored procedures that insert, update, and delete entity types, we do not want to import them and will uncheck this checkbox.</span></span> 

    ![S Prozesse importieren](~/ef6/media/importsprocs.jpg)

-   <span data-ttu-id="25742-142">Klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="25742-142">Click **Finish**.</span></span>
    <span data-ttu-id="25742-143">Im EF Designer, der bietet eine Entwurfsoberfläche zum Bearbeiten des Modells, wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="25742-143">The EF Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-the-person-entity-to-stored-procedures"></a><span data-ttu-id="25742-144">Zuordnen von der Person-Entität zu gespeicherten Prozeduren</span><span class="sxs-lookup"><span data-stu-id="25742-144">Map the Person Entity to Stored Procedures</span></span>

-   <span data-ttu-id="25742-145">Mit der rechten Maustaste die **Person** Entitätstyp, und wählen Sie **Zuordnung der gespeicherten Prozedur**.</span><span class="sxs-lookup"><span data-stu-id="25742-145">Right-click the **Person** entity type and select **Stored Procedure Mapping**.</span></span>
-   <span data-ttu-id="25742-146">Die gespeicherte Prozedur Zuordnungen angezeigt, der **Mappingdetails** Fenster.</span><span class="sxs-lookup"><span data-stu-id="25742-146">The stored procedure mappings appear in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="25742-147">Klicken Sie auf  **&lt;Insert Select-Funktion&gt;**.</span><span class="sxs-lookup"><span data-stu-id="25742-147">Click **&lt;Select Insert Function&gt;**.</span></span>
    <span data-ttu-id="25742-148">Das Feld wird zu einer Dropdownliste mit den gespeicherten Prozeduren im Speichermodell, die Entitätstypen im konzeptionellen Modell zugeordnet werden können.</span><span class="sxs-lookup"><span data-stu-id="25742-148">The field becomes a drop-down list of the stored procedures in the storage model that can be mapped to entity types in the conceptual model.</span></span>
    <span data-ttu-id="25742-149">Wählen Sie **InsertPerson** aus der Dropdown-Liste.</span><span class="sxs-lookup"><span data-stu-id="25742-149">Select **InsertPerson** from the drop-down list.</span></span>
-   <span data-ttu-id="25742-150">Es werden Standardmappings zwischen Parametern gespeicherter Prozeduren und Entitätseigenschaften angezeigt.</span><span class="sxs-lookup"><span data-stu-id="25742-150">Default mappings between stored procedure parameters and entity properties appear.</span></span> <span data-ttu-id="25742-151">Pfeile geben die Mappingrichtung an: Eigenschaftswerte sind Parametern gespeicherter Prozeduren zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="25742-151">Note that arrows indicate the mapping direction: Property values are supplied to stored procedure parameters.</span></span>
-   <span data-ttu-id="25742-152">Klicken Sie auf  **&lt;Result Binding hinzufügen&gt;**.</span><span class="sxs-lookup"><span data-stu-id="25742-152">Click **&lt;Add Result Binding&gt;**.</span></span>
-   <span data-ttu-id="25742-153">Typ **NewPersonID**, den Namen des Parameters, die zurückgegeben werden, indem die **InsertPerson** gespeicherte Prozedur.</span><span class="sxs-lookup"><span data-stu-id="25742-153">Type **NewPersonID**, the name of the parameter returned by the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="25742-154">Stellen Sie sicher, dass keine führende oder nachfolgende Leerzeichen eingeben.</span><span class="sxs-lookup"><span data-stu-id="25742-154">Make sure not to type leading or trailing spaces.</span></span>
-   <span data-ttu-id="25742-155">Drücken Sie die **EINGABETASTE**.</span><span class="sxs-lookup"><span data-stu-id="25742-155">Press **Enter**.</span></span>
-   <span data-ttu-id="25742-156">In der Standardeinstellung **NewPersonID** zugeordnet ist, um den Entitätsschlüssel **PersonID**.</span><span class="sxs-lookup"><span data-stu-id="25742-156">By default, **NewPersonID** is mapped to the entity key **PersonID**.</span></span> <span data-ttu-id="25742-157">Ein Pfeil gibt die Mappingrichtung an: Der Wert der Ergebnisspalte ist der Eigenschaft zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="25742-157">Note that an arrow indicates the direction of the mapping: The value of the result column is supplied to the property.</span></span>

    ![Zuordnungsdetails](~/ef6/media/mappingdetails.png)

-   <span data-ttu-id="25742-159">Klicken Sie auf **&lt;Update Function auswählen&gt;** , und wählen Sie **UpdatePerson** aus der resultierenden Dropdownliste aus.</span><span class="sxs-lookup"><span data-stu-id="25742-159">Click **&lt;Select Update Function&gt;** and select **UpdatePerson** from the resulting drop-down list.</span></span>
-   <span data-ttu-id="25742-160">Es werden Standardmappings zwischen Parametern gespeicherter Prozeduren und Entitätseigenschaften angezeigt.</span><span class="sxs-lookup"><span data-stu-id="25742-160">Default mappings between stored procedure parameters and entity properties appear.</span></span>
-   <span data-ttu-id="25742-161">Klicken Sie auf **&lt;Delete Function auswählen&gt;** , und wählen Sie **DeletePerson** aus der resultierenden Dropdownliste aus.</span><span class="sxs-lookup"><span data-stu-id="25742-161">Click **&lt;Select Delete Function&gt;** and select **DeletePerson** from the resulting drop-down list.</span></span>
-   <span data-ttu-id="25742-162">Es werden Standardmappings zwischen Parametern gespeicherter Prozeduren und Entitätseigenschaften angezeigt.</span><span class="sxs-lookup"><span data-stu-id="25742-162">Default mappings between stored procedure parameters and entity properties appear.</span></span>

<span data-ttu-id="25742-163">Einfügen, aktualisieren und löschen Sie Prozesse für die **Person** Entitätstyp werden nun gespeicherten Prozeduren zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="25742-163">The insert, update, and delete operations of the **Person** entity type are now mapped to stored procedures.</span></span>

<span data-ttu-id="25742-164">Wenn Sie die parallelitätsprüfung beim Aktualisieren oder Löschen einer Entität mit gespeicherten Prozeduren aktivieren möchten, verwenden Sie eine der folgenden Optionen aus:</span><span class="sxs-lookup"><span data-stu-id="25742-164">If you want to enable concurrency checking when updating or deleting an entity with stored procedures, use one of the following options:</span></span>

-   <span data-ttu-id="25742-165">Verwenden einer **Ausgabe** Parameter zum Zurückgeben der Anzahl der betroffenen Zeilen aus der gespeicherten Prozedur und überprüfen Sie die **Parameter für betroffene Zeilen** Kontrollkästchen neben den Namen des Parameters.</span><span class="sxs-lookup"><span data-stu-id="25742-165">Use an **OUTPUT** parameter to return the number of affected rows from the stored procedure and check the **Rows Affected Parameter** checkbox next to the parameter name.</span></span> <span data-ttu-id="25742-166">Wenn der zurückgegebene Wert 0 (null) ist, wenn der Vorgang aufgerufen wird, eine [ **OptimisticConcurrencyException** ](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="25742-166">If the value returned is zero when the operation is called, an  [**OptimisticConcurrencyException**](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) will be thrown.</span></span>
-   <span data-ttu-id="25742-167">Überprüfen Sie die **Originalwert verwenden** Kontrollkästchen neben einer Eigenschaft, die Sie für die parallelitätsüberprüfung verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="25742-167">Check the **Use Original Value** checkbox next to a property that you want to use for concurrency checking.</span></span> <span data-ttu-id="25742-168">Wenn ein Update versucht wird, wird der Wert der Eigenschaft, die ursprünglich aus der Datenbank gelesen wurde verwendet werden, beim Schreiben von Daten in der Datenbank gesichert.</span><span class="sxs-lookup"><span data-stu-id="25742-168">When an update is attempted, the value of the property that was originally read from the database will be used when writing data back to the database.</span></span> <span data-ttu-id="25742-169">Wenn der Wert wird nicht den Wert in der Datenbank entspricht einer **OptimisticConcurrencyException** ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="25742-169">If the value does not match the value in the database, an **OptimisticConcurrencyException** will be thrown.</span></span>

## <a name="use-the-model"></a><span data-ttu-id="25742-170">Verwenden Sie das Modell</span><span class="sxs-lookup"><span data-stu-id="25742-170">Use the Model</span></span>

<span data-ttu-id="25742-171">Öffnen der **"Program.cs"** -Datei, in der **Main** Methode definiert ist.</span><span class="sxs-lookup"><span data-stu-id="25742-171">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="25742-172">Fügen Sie der Main-Funktion mit den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="25742-172">Add the following code into the Main function.</span></span>

<span data-ttu-id="25742-173">Der Code erstellt ein neues **Person** -Objekt, klicken Sie dann das Objekt aktualisiert und löscht schließlich das-Objekt.</span><span class="sxs-lookup"><span data-stu-id="25742-173">The code creates a new **Person** object, then updates the object, and finally deletes the object.</span></span>         

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

-   <span data-ttu-id="25742-174">Kompilieren Sie die Anwendung, und führen Sie sie aus.</span><span class="sxs-lookup"><span data-stu-id="25742-174">Compile and run the application.</span></span> <span data-ttu-id="25742-175">Das Programm erzeugt die folgende Ausgabe \*</span><span class="sxs-lookup"><span data-stu-id="25742-175">The program produces the following output \*</span></span>
    >[!NOTE]
> <span data-ttu-id="25742-176">PersonID wird vom Server automatisch generiert, damit wahrscheinlich eine unterschiedliche Anzahl \* angezeigt wird</span><span class="sxs-lookup"><span data-stu-id="25742-176">PersonID is auto-generated by the server, so you will most likely see a different number\*</span></span>

```
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

<span data-ttu-id="25742-177">Wenn Sie mit der endgültigen Version von Visual Studio arbeiten, können Sie Intellitrace mit dem Debugger verwenden, um die SQL-Anweisungen, die ausgeführt werden, finden Sie unter.</span><span class="sxs-lookup"><span data-stu-id="25742-177">If you are working with the Ultimate version of Visual Studio, you can use Intellitrace with the debugger to see the SQL statements that get executed.</span></span>

![IntelliTrace](~/ef6/media/intellitrace.png)
