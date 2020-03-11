---
title: Aufteilung von Designer Entitäten-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: ba1895ae491cec909ff88a8784eea82f1876f595
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415262"
---
# <a name="designer-entity-splitting"></a><span data-ttu-id="5a661-102">Aufteilung von Designer Entitäten</span><span class="sxs-lookup"><span data-stu-id="5a661-102">Designer Entity Splitting</span></span>
<span data-ttu-id="5a661-103">In dieser exemplarischen Vorgehensweise wird gezeigt, wie ein Entitätstyp zwei Tabellen zugeordnet wird, indem ein Modell mit dem Entity Framework Designer (EF-Designer) geändert wird.</span><span class="sxs-lookup"><span data-stu-id="5a661-103">This walkthrough shows how to map an entity type to two tables by modifying a model with the Entity Framework Designer (EF Designer).</span></span> <span data-ttu-id="5a661-104">Sie können eine Entität mehreren Tabellen zuordnen, sofern für die Tabellen ein gemeinsamer Schlüssel vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="5a661-104">You can map an entity to multiple tables when the tables share a common key.</span></span> <span data-ttu-id="5a661-105">Die Konzepte, die für die Zuordnung eines Entitäts Typs zu zwei Tabellen gelten, können leicht erweitert werden, um einen Entitätstyp mehr als zwei Tabellen zu ordnen.</span><span class="sxs-lookup"><span data-stu-id="5a661-105">The concepts that apply to mapping an entity type to two tables are easily extended to mapping an entity type to more than two tables.</span></span>

<span data-ttu-id="5a661-106">Die folgende Abbildung zeigt die Hauptfenster, die bei der Arbeit mit dem EF-Designer verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="5a661-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF-Designer](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="5a661-108">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="5a661-108">Prerequisites</span></span>

<span data-ttu-id="5a661-109">Visual Studio 2012 oder Visual Studio 2010, Ultimate, Premium, Professional oder Web Express Edition.</span><span class="sxs-lookup"><span data-stu-id="5a661-109">Visual Studio 2012 or Visual Studio 2010, Ultimate, Premium, Professional, or Web Express edition.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="5a661-110">Erstellen der Datenbank</span><span class="sxs-lookup"><span data-stu-id="5a661-110">Create the Database</span></span>

<span data-ttu-id="5a661-111">Der Datenbankserver, der mit Visual Studio installiert wird, unterscheidet sich abhängig von der installierten Version von Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="5a661-111">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="5a661-112">Wenn Sie Visual Studio 2012 verwenden, erstellen Sie eine localdb-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="5a661-112">If you are using Visual Studio 2012 then you'll be creating a LocalDB database.</span></span>
-   <span data-ttu-id="5a661-113">Wenn Sie Visual Studio 2010 verwenden, erstellen Sie eine SQL Express-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="5a661-113">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>

<span data-ttu-id="5a661-114">Zuerst erstellen wir eine Datenbank mit zwei Tabellen, die in einer einzelnen Entität kombiniert werden.</span><span class="sxs-lookup"><span data-stu-id="5a661-114">First we'll create a database with two tables that we are going to combine into a single entity.</span></span>

-   <span data-ttu-id="5a661-115">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5a661-115">Open Visual Studio</span></span>
-   <span data-ttu-id="5a661-116">**&gt; Server-Explorer anzeigen**</span><span class="sxs-lookup"><span data-stu-id="5a661-116">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="5a661-117">Klicken Sie mit der rechten Maustaste auf **Datenverbindungen,&gt; Verbindung hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="5a661-117">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="5a661-118">Wenn Sie über Server-Explorer keine Verbindung mit einer Datenbank hergestellt haben, müssen Sie **Microsoft SQL Server** als Datenquelle auswählen.</span><span class="sxs-lookup"><span data-stu-id="5a661-118">If you haven’t connected to a database from Server Explorer before you’ll need to select **Microsoft SQL Server** as the data source</span></span>
-   <span data-ttu-id="5a661-119">Stellen Sie eine Verbindung mit localdb oder SQL Express her, je nachdem, welche installiert wurde.</span><span class="sxs-lookup"><span data-stu-id="5a661-119">Connect to either LocalDB or SQL Express, depending on which one you have installed</span></span>
-   <span data-ttu-id="5a661-120">Geben Sie **entitysplit** als Datenbanknamen ein.</span><span class="sxs-lookup"><span data-stu-id="5a661-120">Enter **EntitySplitting** as the database name</span></span>
-   <span data-ttu-id="5a661-121">Wählen Sie **OK** aus, und Sie werden gefragt, ob Sie eine neue Datenbank erstellen möchten, und wählen Sie **Ja** aus.</span><span class="sxs-lookup"><span data-stu-id="5a661-121">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>
-   <span data-ttu-id="5a661-122">Die neue Datenbank wird nun in Server-Explorer</span><span class="sxs-lookup"><span data-stu-id="5a661-122">The new database will now appear in Server Explorer</span></span>
-   <span data-ttu-id="5a661-123">Wenn Sie Visual Studio 2012 verwenden</span><span class="sxs-lookup"><span data-stu-id="5a661-123">If you are using Visual Studio 2012</span></span>
    -   <span data-ttu-id="5a661-124">Klicken Sie in Server-Explorer mit der rechten Maustaste auf die Datenbank, und wählen Sie **neue Abfrage** .</span><span class="sxs-lookup"><span data-stu-id="5a661-124">Right-click on the database in Server Explorer and select **New Query**</span></span>
    -   <span data-ttu-id="5a661-125">Kopieren Sie den folgenden SQL-Befehl in die neue Abfrage, klicken Sie mit der rechten Maustaste auf die Abfrage, und wählen Sie **Ausführen** .</span><span class="sxs-lookup"><span data-stu-id="5a661-125">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>
-   <span data-ttu-id="5a661-126">Wenn Sie Visual Studio 2010 verwenden</span><span class="sxs-lookup"><span data-stu-id="5a661-126">If you are using Visual Studio 2010</span></span>
    -   <span data-ttu-id="5a661-127">Wählen Sie **Data-&gt; Transact SQL-Editor-&gt; neue Abfrage Verbindung...**</span><span class="sxs-lookup"><span data-stu-id="5a661-127">Select **Data -&gt; Transact SQL Editor -&gt; New Query Connection...**</span></span>
    -   <span data-ttu-id="5a661-128">Geben Sie **.\\SQLExpress** als Servernamen ein, und klicken Sie auf **OK** .</span><span class="sxs-lookup"><span data-stu-id="5a661-128">Enter **.\\SQLEXPRESS** as the server name and click **OK**</span></span>
    -   <span data-ttu-id="5a661-129">Wählen Sie in der Dropdown-Datei am oberen Rand des Abfrage-Editors die **entitysplit** -Datenbank aus.</span><span class="sxs-lookup"><span data-stu-id="5a661-129">Select the **EntitySplitting** database from the drop down at the top of the query editor</span></span>
    -   <span data-ttu-id="5a661-130">Kopieren Sie das folgende SQL in die neue Abfrage, klicken Sie mit der rechten Maustaste auf die Abfrage, und wählen Sie **SQL ausführen** aus.</span><span class="sxs-lookup"><span data-stu-id="5a661-130">Copy the following SQL into the new query, then right-click on the query and select **Execute SQL**</span></span>

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

## <a name="create-the-project"></a><span data-ttu-id="5a661-131">Erstellen des Projekts</span><span class="sxs-lookup"><span data-stu-id="5a661-131">Create the Project</span></span>

-   <span data-ttu-id="5a661-132">Zeigen Sie im Menü **Datei** auf **Neu**, und klicken Sie dann auf **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="5a661-132">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="5a661-133">Klicken Sie im linken Bereich auf **Visual C-\#** , und wählen Sie dann die Vorlage **Konsolenanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="5a661-133">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="5a661-134">Geben Sie als Name für das Projekt **matztityytablessample** ein, und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="5a661-134">Enter **MapEntityToTablesSample** as the name of the project and click **OK**.</span></span>
-   <span data-ttu-id="5a661-135">Klicken Sie auf **Nein** , wenn Sie dazu aufgefordert werden, die im ersten Abschnitt erstellte SQL-Abfrage zu speichern.</span><span class="sxs-lookup"><span data-stu-id="5a661-135">Click **No** if prompted to save the SQL query created in the first section.</span></span>

## <a name="create-a-model-based-on-the-database"></a><span data-ttu-id="5a661-136">Erstellen eines Modells auf der Grundlage der Datenbank</span><span class="sxs-lookup"><span data-stu-id="5a661-136">Create a Model based on the Database</span></span>

-   <span data-ttu-id="5a661-137">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, zeigen Sie auf **Hinzufügen**, und klicken Sie dann auf **Neues Element**.</span><span class="sxs-lookup"><span data-stu-id="5a661-137">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="5a661-138">Wählen Sie im linken Menü **Daten** aus, und wählen Sie dann im Bereich Vorlagen die Option **ADO.NET Entity Data Model** aus.</span><span class="sxs-lookup"><span data-stu-id="5a661-138">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="5a661-139">Geben Sie als Dateiname **matztitydetablesmodel. edmx** ein, und klicken Sie dann auf **Hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="5a661-139">Enter **MapEntityToTablesModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="5a661-140">Wählen Sie im Dialogfeld Modell Inhalte auswählen die Option **aus Datenbank generieren aus**, und klicken Sie dann auf **Weiter.**</span><span class="sxs-lookup"><span data-stu-id="5a661-140">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="5a661-141">Wählen **Sie in**der Dropdown-Dropdown-Option die **entitysplitverbindung** aus</span><span class="sxs-lookup"><span data-stu-id="5a661-141">Select the **EntitySplitting** connection from the drop down and click **Next**.</span></span>
-   <span data-ttu-id="5a661-142">Aktivieren Sie im Dialogfeld Wählen Sie Ihre Datenbankobjekte aus das Kontrollkästchen neben dem Knoten **Tabellen** .</span><span class="sxs-lookup"><span data-stu-id="5a661-142">In the Choose Your Database Objects dialog box, check the box next to the **Tables** node.</span></span>
    <span data-ttu-id="5a661-143">Dadurch werden alle Tabellen aus der **entitysplit** -Datenbank dem Modell hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="5a661-143">This will add all the tables from the **EntitySplitting** database to the model.</span></span>
-   <span data-ttu-id="5a661-144">Klicken Sie auf **Finish**.</span><span class="sxs-lookup"><span data-stu-id="5a661-144">Click **Finish**.</span></span>

<span data-ttu-id="5a661-145">Die Entity Designer, die eine Entwurfs Oberfläche zum Bearbeiten des Modells bereitstellt, wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="5a661-145">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

## <a name="map-an-entity-to-two-tables"></a><span data-ttu-id="5a661-146">Zuordnen einer Entität zu zwei Tabellen</span><span class="sxs-lookup"><span data-stu-id="5a661-146">Map an Entity to Two Tables</span></span>

<span data-ttu-id="5a661-147">In diesem Schritt aktualisieren wir den Entitätstyp **Person** , um Daten aus den Tabellen " **Person** " und " **personinfo** " zu kombinieren.</span><span class="sxs-lookup"><span data-stu-id="5a661-147">In this step we will update the **Person** entity type to combine data from the **Person** and **PersonInfo** tables.</span></span>

-   <span data-ttu-id="5a661-148">Wählen Sie die Eigenschaften Email und **Phone** der **personinfo **-Entität aus, und drücken Sie **STRG + X**  **-** Taste.</span><span class="sxs-lookup"><span data-stu-id="5a661-148">Select the **Email** and **Phone** properties of the **PersonInfo **entity and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="5a661-149">Wählen Sie die Entität **Person **aus, und drücken Sie **STRG + V** Tasten.</span><span class="sxs-lookup"><span data-stu-id="5a661-149">Select the **Person **entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="5a661-150">Wählen Sie auf der Entwurfs Oberfläche die **personinfo** - Entität aus, und drücken Sie auf der Tastatur die Schaltfläche **Löschen** .</span><span class="sxs-lookup"><span data-stu-id="5a661-150">On the design surface, select the **PersonInfo** entity and press **Delete** button on the keyboard.</span></span>
-   <span data-ttu-id="5a661-151">Klicken Sie auf **Nein** , wenn Sie gefragt werden, ob Sie die **personinfo** -Tabelle aus dem Modell entfernen möchten. Wir sind im Begriff, Sie der **Person** -Entität zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="5a661-151">Click **No** when asked if you want to remove the **PersonInfo** table from the model, we are about to map it to the **Person** entity.</span></span>

    ![Löschen von Tabellen](~/ef6/media/deletetables.png)

<span data-ttu-id="5a661-153">Für die nächsten Schritte ist das Fenster **Mappingdetails** erforderlich.</span><span class="sxs-lookup"><span data-stu-id="5a661-153">The next steps require the **Mapping Details** window.</span></span> <span data-ttu-id="5a661-154">Wenn dieses Fenster nicht angezeigt wird, klicken Sie mit der rechten Maustaste auf die Entwurfs Oberfläche, und wählen Sie **Mappingdetails**.</span><span class="sxs-lookup"><span data-stu-id="5a661-154">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="5a661-155">Wählen Sie den Entitätstyp **Person** aus, und klicken Sie im Fenster **Mappingdetails** auf **&lt;Hinzufügen einer Tabelle oder Sicht&gt;**  .</span><span class="sxs-lookup"><span data-stu-id="5a661-155">Select the **Person** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="5a661-156">Wählen Sie in der Dropdown Liste **personinfo ** - aus.</span><span class="sxs-lookup"><span data-stu-id="5a661-156">Select **PersonInfo ** from the drop-down list.</span></span>
    <span data-ttu-id="5a661-157">Das Fenster **Mappingdetails** wird mit Standard Spalten Zuordnungen aktualisiert. diese sind für das Szenario in Ordnung.</span><span class="sxs-lookup"><span data-stu-id="5a661-157">The **Mapping Details** window is updated with default column mappings, these are fine for our scenario.</span></span>

<span data-ttu-id="5a661-158">Die **Person** Entitätstyp ist nun der **Person** und **personinfo** Tabellen zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="5a661-158">The **Person** entity type is now mapped to the **Person** and **PersonInfo** tables.</span></span>

![Zuordnung 2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a><span data-ttu-id="5a661-160">Verwenden des Modells</span><span class="sxs-lookup"><span data-stu-id="5a661-160">Use the Model</span></span>

-   <span data-ttu-id="5a661-161">Fügen Sie den folgenden Code in die Main-Methode ein.</span><span class="sxs-lookup"><span data-stu-id="5a661-161">Paste the following code in the Main method.</span></span>

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

-   <span data-ttu-id="5a661-162">Kompilieren Sie die Anwendung, und führen Sie sie aus.</span><span class="sxs-lookup"><span data-stu-id="5a661-162">Compile and run the application.</span></span>

<span data-ttu-id="5a661-163">Die folgenden T-SQL-Anweisungen wurden für die Datenbank ausgeführt, weil diese Anwendung ausgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="5a661-163">The following T-SQL statements were executed against the database as a result of running this application.</span></span> 

-   <span data-ttu-id="5a661-164">Die folgenden zwei **Insert** -Anweisungen wurden als Ergebnis der Ausführung des Kontexts ausgeführt. SaveChanges ().</span><span class="sxs-lookup"><span data-stu-id="5a661-164">The following two **INSERT** statements were executed as a result of executing context.SaveChanges().</span></span> <span data-ttu-id="5a661-165">Die Daten werden von der **Person** -Entität übernommen und zwischen den Tabellen " **Person** " und " **personinfo** " aufgeteilt.</span><span class="sxs-lookup"><span data-stu-id="5a661-165">They take the data from the **Person** entity and split it between the **Person** and **PersonInfo** tables.</span></span>

    ![1 einfügen](~/ef6/media/insert1.png)

    ![2 einfügen](~/ef6/media/insert2.png)
-   <span data-ttu-id="5a661-168">Die folgende **Select** -Anweisung wurde als Ergebnis der Enumeration der Personen in der Datenbank ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="5a661-168">The following **SELECT** was executed as a result of enumerating the people in the database.</span></span> <span data-ttu-id="5a661-169">Dabei werden die Daten aus der Tabelle " **Person** " und " **personinfo** " kombiniert.</span><span class="sxs-lookup"><span data-stu-id="5a661-169">It combines the data from the **Person** and **PersonInfo** table.</span></span>

    ![Select](~/ef6/media/select.png)
