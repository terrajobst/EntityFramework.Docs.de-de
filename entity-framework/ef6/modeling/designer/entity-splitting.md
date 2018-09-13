---
title: Designer Entitätsaufteilung - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: ba1895ae491cec909ff88a8784eea82f1876f595
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490846"
---
# <a name="designer-entity-splitting"></a><span data-ttu-id="16451-102">Designer Entitätsaufteilung</span><span class="sxs-lookup"><span data-stu-id="16451-102">Designer Entity Splitting</span></span>
<span data-ttu-id="16451-103">In dieser exemplarischen Vorgehensweise wird gezeigt, wie einen Entitätstyp zwei Tabellen zugeordnet werden, durch Ändern eines Modells mit dem Entity Framework Designer (EF-Designer).</span><span class="sxs-lookup"><span data-stu-id="16451-103">This walkthrough shows how to map an entity type to two tables by modifying a model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="16451-104">Sie können eine Entität mehreren Tabellen zuordnen, sofern für die Tabellen ein gemeinsamer Schlüssel vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="16451-104">You can map an entity to multiple tables when the tables share a common key.</span></span> <span data-ttu-id="16451-105">Die Prinzipien, nach denen ein Entitätstyp zwei Tabellen zugeordnet wird, lassen sich leicht auf das Mapping zu mehr als zwei Tabellen erweitern.</span><span class="sxs-lookup"><span data-stu-id="16451-105">The concepts that apply to mapping an entity type to two tables are easily extended to mapping an entity type to more than two tables.</span></span>

<span data-ttu-id="16451-106">Die folgende Abbildung zeigt die wichtigsten Windows, die bei der Arbeit mit dem EF Designer verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="16451-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF-Designer](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="16451-108">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="16451-108">Prerequisites</span></span>

<span data-ttu-id="16451-109">Visual Studio 2012 oder Visual Studio 2010, Ultimate, Premium, Professional oder Web Express Edition.</span><span class="sxs-lookup"><span data-stu-id="16451-109">Visual Studio 2012 or Visual Studio 2010, Ultimate, Premium, Professional, or Web Express edition.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="16451-110">Erstellen Sie die Datenbank</span><span class="sxs-lookup"><span data-stu-id="16451-110">Create the Database</span></span>

<span data-ttu-id="16451-111">Der Datenbankserver, der mit Visual Studio installiert ist, unterscheidet sich abhängig von der Version von Visual Studio, die Sie installiert haben:</span><span class="sxs-lookup"><span data-stu-id="16451-111">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="16451-112">Wenn Sie Visual Studio 2012 verwenden, wird Sie eine LocalDB-Datenbank erstellen.</span><span class="sxs-lookup"><span data-stu-id="16451-112">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="16451-113">Wenn Sie Visual Studio 2010 verwenden erstellen Sie eine SQL Express-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="16451-113">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="16451-114">Zunächst erstellen wir eine Datenbank mit zwei Tabellen, die wir in einer einzelnen Entität kombinieren möchte.</span><span class="sxs-lookup"><span data-stu-id="16451-114">First we'll create a database with two tables that we are going to combine into a single entity.</span></span>

-   <span data-ttu-id="16451-115">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="16451-115">Open Visual Studio</span></span>
-   <span data-ttu-id="16451-116">**Ansicht – Profiler -&gt; Server-Explorer**</span><span class="sxs-lookup"><span data-stu-id="16451-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="16451-117">Klicken Sie mit der rechten Maustaste auf **Datenverbindungen -&gt; Verbindung hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="16451-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="16451-118">Wenn Sie vor dem müssen auswählen, im Server-Explorer mit einer Datenbank verbunden haben **Microsoft SQL Server** als Datenquelle</span><span class="sxs-lookup"><span data-stu-id="16451-118">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="16451-119">Eine Verbindung mit LocalDB oder SQL Express, je nachdem, welches Sie installiert haben</span><span class="sxs-lookup"><span data-stu-id="16451-119">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="16451-120">Geben Sie **EntitySplitting** als Datenbankname</span><span class="sxs-lookup"><span data-stu-id="16451-120">Enter **EntitySplitting** as the database name</span></span>
-   <span data-ttu-id="16451-121">Wählen Sie **OK** und Sie werden gefragt, ob Sie eine neue Datenbank, die auf erstellen möchten **Ja**</span><span class="sxs-lookup"><span data-stu-id="16451-121">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="16451-122">Die neue Datenbank wird jetzt im Server-Explorer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="16451-122">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="16451-123">Bei Verwendung von Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="16451-123">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="16451-124">Mit der rechten Maustaste auf die Datenbank im Server-Explorer, und wählen Sie **neue Abfrage**</span><span class="sxs-lookup"><span data-stu-id="16451-124">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="16451-125">Kopieren Sie die folgende SQL-Anweisung in die neue Abfrage, und klicken Sie dann mit der rechten Maustaste auf die Abfrage, und wählen **ausführen**</span><span class="sxs-lookup"><span data-stu-id="16451-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="16451-126">Wenn Sie Visual Studio 2010 verwenden</span><span class="sxs-lookup"><span data-stu-id="16451-126">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="16451-127">Wählen Sie **Daten per Push –&gt; Transact-SQL-Editor –&gt; neue Abfrageverbindung...**</span><span class="sxs-lookup"><span data-stu-id="16451-127">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="16451-128">Geben Sie **.\\ SQLEXPRESS** als Server ein, und klicken Sie auf **OK**</span><span class="sxs-lookup"><span data-stu-id="16451-128">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="16451-129">Wählen Sie die **EntitySplitting** Datenbank aus der Dropdownliste unten am oberen Rand des Abfrage-Editors</span><span class="sxs-lookup"><span data-stu-id="16451-129">Select the **EntitySplitting** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="16451-130">Kopieren Sie die folgende SQL-Anweisung in die neue Abfrage, und klicken Sie dann mit der rechten Maustaste auf die Abfrage, und wählen **SQL ausführen**</span><span class="sxs-lookup"><span data-stu-id="16451-130">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

``` SQL
CREATE TABLE [dbo].[Person] (
[PersonId] INT IDENTITY (1, 1) NOT NULL,
[FirstName] NVARCHAR (200) NULL,
[LastName] NVARCHAR (200) NULL,
CONSTRAINT [PK_Person] PRIMARY KEY CLUSTERED ([PersonId] ASC)
);

CREATE TABLE [dbo].[PersonInfo] (
[PersonId] INT NOT NULL,
[Email] NVARCHAR (200) NULL,
[Phone] NVARCHAR (50) NULL,
CONSTRAINT [PK_PersonInfo] PRIMARY KEY CLUSTERED ([PersonId] ASC),
CONSTRAINT [FK_Person_PersonInfo] FOREIGN KEY ([PersonId]) REFERENCES [dbo].[Person] ([PersonId]) ON DELETE CASCADE
);
```

## <a name="create-the-project"></a><span data-ttu-id="16451-131">Erstellen des Projekts</span><span class="sxs-lookup"><span data-stu-id="16451-131">Create the Project</span></span>

-   <span data-ttu-id="16451-132">Zeigen Sie im Menü **Datei** auf **Neu**, und klicken Sie dann auf **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="16451-132">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="16451-133">Klicken Sie im linken Bereich auf **Visual C\#**, und wählen Sie dann die **Konsolenanwendung** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="16451-133">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="16451-134">Geben Sie **MapEntityToTablesSample** als Namen für das Projekt und auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="16451-134">Enter **MapEntityToTablesSample** as the name of the project and click **OK**.</span></span>
-   <span data-ttu-id="16451-135">Klicken Sie auf **keine** bei Aufforderung zum Speichern der SQL-Abfrage, die im ersten Abschnitt erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="16451-135">Click **No** if prompted to save the SQL query created in the first section.</span></span>

## <a name="create-a-model-based-on-the-database"></a><span data-ttu-id="16451-136">Erstellen eines Modells auf Grundlage der Datenbank</span><span class="sxs-lookup"><span data-stu-id="16451-136">Create a Model based on the Database</span></span>

-   <span data-ttu-id="16451-137">Mit der rechten Maustaste des Projektnamen im Projektmappen-Explorer, zeigen Sie auf **hinzufügen**, und klicken Sie dann auf **neues Element**.</span><span class="sxs-lookup"><span data-stu-id="16451-137">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="16451-138">Wählen Sie **Daten** aus dem linken Menü und wählen Sie dann **ADO.NET Entity Data Model** im Bereich Vorlagen.</span><span class="sxs-lookup"><span data-stu-id="16451-138">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="16451-139">Geben Sie **MapEntityToTablesModel.edmx** für den Dateinamen ein, und klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="16451-139">Enter **MapEntityToTablesModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="16451-140">Wählen Sie im Dialogfeld "Modellinhalte" **aus Datenbank generieren**, und klicken Sie dann auf **weiter.**</span><span class="sxs-lookup"><span data-stu-id="16451-140">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="16451-141">Wählen Sie die **EntitySplitting** Verbindung aus der Dropdownliste aus, und klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="16451-141">Select the **EntitySplitting** connection from the drop down and click **Next**.</span></span>
-   <span data-ttu-id="16451-142">Klicken Sie im Dialogfeld "Datenbankobjekte auswählen" das Kontrollkästchen neben den **Tabellen** Knoten.</span><span class="sxs-lookup"><span data-stu-id="16451-142">In the Choose Your Database Objects dialog box, check the box next to the **Tables** node.</span></span>
    <span data-ttu-id="16451-143">Hiermit werden alle Tabellen aus der **EntitySplitting** Datenbank für das Modell.</span><span class="sxs-lookup"><span data-stu-id="16451-143">This will add all the tables from the **EntitySplitting** database to the model.</span></span>
-   <span data-ttu-id="16451-144">Klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="16451-144">Click **Finish**.</span></span>

<span data-ttu-id="16451-145">Im Entity Designer, der bietet eine Entwurfsoberfläche zum Bearbeiten des Modells, wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="16451-145">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-an-entity-to-two-tables"></a><span data-ttu-id="16451-146">Zuordnen einer Entität zu zwei Tabellen</span><span class="sxs-lookup"><span data-stu-id="16451-146">Map an Entity to Two Tables</span></span>

<span data-ttu-id="16451-147">In diesem Schritt aktualisieren wir die **Person** Entitätstyp zum Kombinieren von Daten aus der **Person** und **PersonInfo** Tabellen.</span><span class="sxs-lookup"><span data-stu-id="16451-147">In this step we will update the **Person** entity type to combine data from the **Person** and **PersonInfo** tables.</span></span>

-   <span data-ttu-id="16451-148">Wählen Sie die **-e-Mail** und **Phone** Eigenschaften der \*\* PersonInfo \*\* Entität, und drücken Sie **STRG + X** Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="16451-148">Select the **Email** and **Phone** properties of the \*\*PersonInfo \*\*entity and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="16451-149">Wählen Sie die \*\* Person \*\* Entität, und drücken Sie **STRG + V** Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="16451-149">Select the \*\*Person \*\*entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="16451-150">Wählen Sie auf der Entwurfsoberfläche der **PersonInfo** Entität, und drücken Sie **löschen** Taste auf der Tastatur.</span><span class="sxs-lookup"><span data-stu-id="16451-150">On the design surface, select the **PersonInfo** entity and press **Delete** button on the keyboard.</span></span>
-   <span data-ttu-id="16451-151">Klicken Sie auf **keine** Wenn gefragt, ob Sie entfernen möchten die **PersonInfo** Tabelle aus dem Modell werden etwa für die Zuordnung der **Person** Entität.</span><span class="sxs-lookup"><span data-stu-id="16451-151">Click **No** when asked if you want to remove the **PersonInfo** table from the model, we are about to map it to the **Person** entity.</span></span>

    ![Löschen von Tabellen](~/ef6/media/deletetables.png)

<span data-ttu-id="16451-153">Die nächsten Schritte erfordern die **Mappingdetails** Fenster.</span><span class="sxs-lookup"><span data-stu-id="16451-153">The next steps require the **Mapping Details** window.</span></span> <span data-ttu-id="16451-154">Wenn Sie dieses Fenster nicht sehen, mit der rechten Maustaste Entwurfsoberfläche, und wählen **Mappingdetails**.</span><span class="sxs-lookup"><span data-stu-id="16451-154">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="16451-155">Wählen Sie die **Person** Entitätstyp, und klicken Sie auf **&lt;Hinzufügen einer Tabelle oder Sicht&gt;** in die **Mappingdetails** Fenster.</span><span class="sxs-lookup"><span data-stu-id="16451-155">Select the **Person** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="16451-156">Wählen Sie \*\* PersonInfo \*\* aus der Dropdown-Liste.</span><span class="sxs-lookup"><span data-stu-id="16451-156">Select \*\*PersonInfo \*\* from the drop-down list.</span></span>
    <span data-ttu-id="16451-157">Die **Mappingdetails** Fenster wird mit standardspaltenzuordnungen aktualisiert, das für dieses Szenario in Ordnung sind.</span><span class="sxs-lookup"><span data-stu-id="16451-157">The **Mapping Details** window is updated with default column mappings, these are fine for our scenario.</span></span>

<span data-ttu-id="16451-158">Die **Person** Entitätstyp ist nun zugeordnet, um die **Person** und **PersonInfo** Tabellen.</span><span class="sxs-lookup"><span data-stu-id="16451-158">The **Person** entity type is now mapped to the **Person** and **PersonInfo** tables.</span></span>

![Zuordnen von 2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a><span data-ttu-id="16451-160">Verwenden Sie das Modell</span><span class="sxs-lookup"><span data-stu-id="16451-160">Use the Model</span></span>

-   <span data-ttu-id="16451-161">Fügen Sie den folgenden Code in der Main-Methode.</span><span class="sxs-lookup"><span data-stu-id="16451-161">Paste the following code in the Main method.</span></span>

``` csharp
    using (var context = new EntitySplittingEntities())
    {
        var person = new Person
        {
            FirstName = "John",
            LastName = "Doe",
            Email = "john@example.com",
            Phone = "555-555-5555"
        };

        context.People.Add(person);
        context.SaveChanges();

        foreach (var item in context.People)
        {
            Console.WriteLine(item.FirstName);
        }
    }
```

-   <span data-ttu-id="16451-162">Kompilieren Sie die Anwendung, und führen Sie sie aus.</span><span class="sxs-lookup"><span data-stu-id="16451-162">Compile and run the application.</span></span>

<span data-ttu-id="16451-163">Die folgenden T-SQL-Anweisungen, die für die Datenbank durch Ausführen der Anwendung ausgeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="16451-163">The following T-SQL statements were executed against the database as a result of running this application.</span></span> 

-   <span data-ttu-id="16451-164">Die folgenden beiden **einfügen** Anweisungen ausgeführt wurden, als Ergebnis der Ausführung von Kontext. SaveChanges().</span><span class="sxs-lookup"><span data-stu-id="16451-164">The following two **INSERT** statements were executed as a result of executing context.SaveChanges().</span></span> <span data-ttu-id="16451-165">Dafür, dass die Daten aus der **Person** Entität und Aufteilung zwischen den **Person** und **PersonInfo** Tabellen.</span><span class="sxs-lookup"><span data-stu-id="16451-165">They take the data from the **Person** entity and split it between the **Person** and **PersonInfo** tables.</span></span>

    ![Legen Sie 1](~/ef6/media/insert1.png)

    ![Legen Sie 2](~/ef6/media/insert2.png)
-   <span data-ttu-id="16451-168">Die folgenden **wählen** als Ergebnis Auflisten von Personen in der Datenbank ausgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="16451-168">The following **SELECT** was executed as a result of enumerating the people in the database.</span></span> <span data-ttu-id="16451-169">Sie kombiniert die Daten aus der **Person** und **PersonInfo** Tabelle.</span><span class="sxs-lookup"><span data-stu-id="16451-169">It combines the data from the **Person** and **PersonInfo** table.</span></span>

    ![Auswählen](~/ef6/media/select.png)
