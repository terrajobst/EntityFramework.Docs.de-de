---
title: Definieren von Abfrage - Designer, EF - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
ms.openlocfilehash: 60d5310589bb9bc3fdb971673422e80537357e55
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996306"
---
# <a name="defining-query---ef-designer"></a><span data-ttu-id="3c957-102">Definieren der Abfrage – EF-Designer</span><span class="sxs-lookup"><span data-stu-id="3c957-102">Defining Query - EF Designer</span></span>
<span data-ttu-id="3c957-103">In dieser exemplarischen Vorgehensweise veranschaulicht das Hinzufügen eine definierende Abfrage und eine entsprechende Entität Geben Sie auf ein Modell mit dem EF Designer.</span><span class="sxs-lookup"><span data-stu-id="3c957-103">This walkthrough demonstrates how to add a defining query and a corresponding entity type to a model using the EF Designer.</span></span> <span data-ttu-id="3c957-104">Eine definierende Abfrage wird häufig verwendet, um Funktionen wie mit einer Datenbankansicht bereitzustellen, aber die Ansicht im Modell nicht in der Datenbank definiert ist.</span><span class="sxs-lookup"><span data-stu-id="3c957-104">A defining query is commonly used to provide functionality similar to that provided by a database view, but the view is defined in the model, not the database.</span></span> <span data-ttu-id="3c957-105">Einer definierenden Abfrage können Sie eine SQL-Anweisung ausführen, die im angegebenen die **DefiningQuery** Element eine EDMX-Datei.</span><span class="sxs-lookup"><span data-stu-id="3c957-105">A defining query allows you to execute a SQL statement that is specified in the **DefiningQuery** element of an .edmx file.</span></span> <span data-ttu-id="3c957-106">Weitere Informationen finden Sie unter **DefiningQuery** in die [SSDL-Spezifikation](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="3c957-106">For more information, see **DefiningQuery** in the [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="3c957-107">Wenn definierende Abfragen verwenden, müssen Sie auch in Ihrem Modell einen Entitätstyp definiert.</span><span class="sxs-lookup"><span data-stu-id="3c957-107">When using defining queries, you also have to define an entity type in your model.</span></span> <span data-ttu-id="3c957-108">Der Entitätstyp wird verwendet, Anzeigen von Daten, die von der definierenden Abfrage verfügbar gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="3c957-108">The entity type is used to surface data exposed by the defining query.</span></span> <span data-ttu-id="3c957-109">Beachten Sie, dass Daten über diesen Entitätstyp verfügbar gemacht ist schreibgeschützt.</span><span class="sxs-lookup"><span data-stu-id="3c957-109">Note that data surfaced through this entity type is read-only.</span></span>

<span data-ttu-id="3c957-110">Parametrisierte Abfragen können nicht als definierende Abfragen ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="3c957-110">Parameterized queries cannot be executed as defining queries.</span></span> <span data-ttu-id="3c957-111">Die Daten können jedoch aktualisiert werden, indem die Insert-, Update- und Delete-Funktionen des Entitätstyps, der die Daten zugänglich macht, gespeicherten Prozeduren zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="3c957-111">However, the data can be updated by mapping the insert, update, and delete functions of the entity type that surfaces the data to stored procedures.</span></span> <span data-ttu-id="3c957-112">Weitere Informationen finden Sie unter [INSERT-, Update- und Delete mit gespeicherten Prozeduren](~/ef6/modeling/designer/stored-procedures/cud.md).</span><span class="sxs-lookup"><span data-stu-id="3c957-112">For more information, see [Insert, Update, and Delete with Stored Procedures](~/ef6/modeling/designer/stored-procedures/cud.md).</span></span>

<span data-ttu-id="3c957-113">In diesem Thema wird gezeigt, wie Sie die folgenden Aufgaben ausführen.</span><span class="sxs-lookup"><span data-stu-id="3c957-113">This topic shows how to perform the following tasks.</span></span>

-   <span data-ttu-id="3c957-114">Hinzufügen einer definierenden Abfrage</span><span class="sxs-lookup"><span data-stu-id="3c957-114">Add a Defining Query</span></span>
-   <span data-ttu-id="3c957-115">Hinzufügen eines Entitätstyps zum Modell</span><span class="sxs-lookup"><span data-stu-id="3c957-115">Add an Entity Type to the Model</span></span>
-   <span data-ttu-id="3c957-116">Ordnen Sie den Entitätstyp der definierenden Abfrage</span><span class="sxs-lookup"><span data-stu-id="3c957-116">Map the Defining Query to the Entity Type</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3c957-117">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="3c957-117">Prerequisites</span></span>

<span data-ttu-id="3c957-118">Um die exemplarische Vorgehensweise nachzuvollziehen, benötigen Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="3c957-118">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="3c957-119">Eine aktuelle Version von Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3c957-119">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="3c957-120">Die [Beispieldatenbank ' School '](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="3c957-120">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="3c957-121">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="3c957-121">Set up the Project</span></span>

<span data-ttu-id="3c957-122">In dieser exemplarischen Vorgehensweise wird Visual Studio 2012 oder höher verwendet.</span><span class="sxs-lookup"><span data-stu-id="3c957-122">This walkthrough is using Visual Studio 2012 or newer.</span></span>

-   <span data-ttu-id="3c957-123">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3c957-123">Open Visual Studio.</span></span>
-   <span data-ttu-id="3c957-124">Zeigen Sie im Menü **Datei** auf **Neu**, und klicken Sie dann auf **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="3c957-124">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="3c957-125">Klicken Sie im linken Bereich auf **Visual C\#**, und wählen Sie dann die **Konsolenanwendung** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="3c957-125">In the left pane, click **Visual C\#**, and then select the **Console Application** template.</span></span>
-   <span data-ttu-id="3c957-126">Geben Sie **DefiningQuerySample** als Namen für das Projekt und auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="3c957-126">Enter **DefiningQuerySample** as the name of the project and click **OK**.</span></span>

 

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="3c957-127">Erstellen Sie ein Modell basierend auf der Datenbank "School"</span><span class="sxs-lookup"><span data-stu-id="3c957-127">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="3c957-128">Mit der rechten Maustaste des Projektnamen im Projektmappen-Explorer, zeigen Sie auf **hinzufügen**, und klicken Sie dann auf **neues Element**.</span><span class="sxs-lookup"><span data-stu-id="3c957-128">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="3c957-129">Wählen Sie **Daten** aus dem linken Menü und wählen Sie dann **ADO.NET Entity Data Model** im Bereich Vorlagen.</span><span class="sxs-lookup"><span data-stu-id="3c957-129">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="3c957-130">Geben Sie **DefiningQueryModel.edmx** für den Dateinamen ein, und klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="3c957-130">Enter **DefiningQueryModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="3c957-131">Wählen Sie im Dialogfeld "Modellinhalte" **aus Datenbank generieren**, und klicken Sie dann auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="3c957-131">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="3c957-132">Klicken Sie auf die neue Verbindung.</span><span class="sxs-lookup"><span data-stu-id="3c957-132">Click New Connection.</span></span> <span data-ttu-id="3c957-133">Geben Sie den Servernamen, klicken Sie im Dialogfeld "Verbindungseigenschaften" (z. B. **(Localdb)\\Mssqllocaldb**), wählen die Authentifizierungsmethode, Typ **School** für den Datenbanknamen, und klicken Sie dann Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="3c957-133">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="3c957-134">Das Dialogfeld "Wählen Sie Ihre Datenverbindung" wird mit Ihrem datenbankverbindungseinstellung aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="3c957-134">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="3c957-135">Klicken Sie im Dialogfeld "Datenbankobjekte auswählen" Überprüfen der **Tabellen** Knoten.</span><span class="sxs-lookup"><span data-stu-id="3c957-135">In the Choose Your Database Objects dialog box, check the **Tables** node.</span></span> <span data-ttu-id="3c957-136">Hiermit werden alle Tabellen aus, die **School** Modell.</span><span class="sxs-lookup"><span data-stu-id="3c957-136">This will add all the tables to the **School** model.</span></span>
-   <span data-ttu-id="3c957-137">Klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="3c957-137">Click **Finish**.</span></span>
-   <span data-ttu-id="3c957-138">Klicken Sie im Projektmappen-Explorer mit der Maustaste der **DefiningQueryModel.edmx** und wählen Sie **Öffnen mit...** .</span><span class="sxs-lookup"><span data-stu-id="3c957-138">In Solution Explorer, right-click the **DefiningQueryModel.edmx** file and select **Open With…**.</span></span>
-   <span data-ttu-id="3c957-139">Wählen Sie **XML (Text)-Editor**.</span><span class="sxs-lookup"><span data-stu-id="3c957-139">Select **XML (Text) Editor**.</span></span>

    ![XMLEditor](~/ef6/media/xmleditor.png)

-   <span data-ttu-id="3c957-141">Klicken Sie auf **Ja** , wenn die folgende Meldung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="3c957-141">Click **Yes** if prompted with the following message:</span></span>

    ![Warning2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a><span data-ttu-id="3c957-143">Hinzufügen einer definierenden Abfrage</span><span class="sxs-lookup"><span data-stu-id="3c957-143">Add a Defining Query</span></span>

<span data-ttu-id="3c957-144">In diesem Schritt aus der XML-Editor Hinzufügen eine definierende Abfrage aus, und geben Sie eine Entität, die SSDL-Abschnitt der EDMX-Datei.</span><span class="sxs-lookup"><span data-stu-id="3c957-144">In this step we will use the XML Editor to add a defining query and an entity type to the SSDL section of the .edmx file.</span></span> 

-   <span data-ttu-id="3c957-145">Hinzufügen einer **EntitySet** Element die SSDL-Abschnitt der EDMX-Datei (Zeile 5 bis 13).</span><span class="sxs-lookup"><span data-stu-id="3c957-145">Add an **EntitySet** element to the SSDL section of the .edmx file (line 5 thru 13).</span></span> <span data-ttu-id="3c957-146">Geben Sie Folgendes an:</span><span class="sxs-lookup"><span data-stu-id="3c957-146">Specify the following:</span></span>
    -   <span data-ttu-id="3c957-147">Nur die **Namen** und **EntityType** Attribute der **EntitySet** -Element angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="3c957-147">Only the **Name** and **EntityType** attributes of the **EntitySet** element are specified.</span></span>
    -   <span data-ttu-id="3c957-148">Der vollqualifizierte Name des Entitätstyps wird verwendet, der **EntityType** Attribut.</span><span class="sxs-lookup"><span data-stu-id="3c957-148">The fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="3c957-149">Die SQL-Anweisung ausgeführt werden wird angegeben, der **DefiningQuery** Element.</span><span class="sxs-lookup"><span data-stu-id="3c957-149">The SQL statement to be executed is specified in the **DefiningQuery** element.</span></span>

``` xml
    <!-- SSDL content -->
    <edmx:StorageModels>
      <Schema Namespace="SchoolModel.Store" Alias="Self" Provider="System.Data.SqlClient" ProviderManifestToken="2008" xmlns:store="http://schemas.microsoft.com/ado/2007/12/edm/EntityStoreSchemaGenerator" xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
        <EntityContainer Name="SchoolModelStoreContainer">
           <EntitySet Name="GradeReport" EntityType="SchoolModel.Store.GradeReport">
              <DefiningQuery>
                SELECT CourseID, Grade, FirstName, LastName
                FROM StudentGrade
                JOIN
                (SELECT * FROM Person WHERE EnrollmentDate IS NOT NULL) AS p
                ON StudentID = p.PersonID
              </DefiningQuery>
          </EntitySet>
          <EntitySet Name="Course" EntityType="SchoolModel.Store.Course" store:Type="Tables" Schema="dbo" />
```

-   <span data-ttu-id="3c957-150">Hinzufügen der **EntityType** Element die SSDL-Abschnitt der EDMX-Datei.</span><span class="sxs-lookup"><span data-stu-id="3c957-150">Add the **EntityType** element to the SSDL section of the .edmx.</span></span> <span data-ttu-id="3c957-151">Datei siehe unten.</span><span class="sxs-lookup"><span data-stu-id="3c957-151">file as shown below.</span></span> <span data-ttu-id="3c957-152">Beachten Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="3c957-152">Note the following:</span></span>
    -   <span data-ttu-id="3c957-153">Den Wert des der **Namen** Attribut entspricht dem Wert des der **EntityType** -Attribut in der **EntitySet** Element oben zwar den vollqualifizierten Namen des der Entitätstyp wird verwendet, der **EntityType** Attribut.</span><span class="sxs-lookup"><span data-stu-id="3c957-153">The value of the **Name** attribute corresponds to the value of the **EntityType** attribute in the **EntitySet** element above, although the fully-qualified name of the entity type is used in the **EntityType** attribute.</span></span>
    -   <span data-ttu-id="3c957-154">Die Eigenschaftennamen entsprechen, die von SQL-Anweisung im zurückgegebenen Spaltennamen die **DefiningQuery** -Element (oben).</span><span class="sxs-lookup"><span data-stu-id="3c957-154">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element (above).</span></span>
    -   <span data-ttu-id="3c957-155">In diesem Beispiel wird durch einen aus drei Eigenschaften bestehenden Entitätsschlüssel sichergestellt, dass der Schlüsselwert eindeutig ist.</span><span class="sxs-lookup"><span data-stu-id="3c957-155">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

``` xml
    <EntityType Name="GradeReport">
      <Key>
        <PropertyRef Name="CourseID" />
        <PropertyRef Name="FirstName" />
        <PropertyRef Name="LastName" />
      </Key>
      <Property Name="CourseID"
                Type="int"
                Nullable="false" />
      <Property Name="Grade"
                Type="decimal"
                Precision="3"
                Scale="2" />
      <Property Name="FirstName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
      <Property Name="LastName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
    </EntityType>
```

>[!NOTE]
> <span data-ttu-id="3c957-156">Wenn Sie später ausführen, die **Modellaktualisierungs-Assistent** Dialogfeld alle am Speichermodell, einschließlich der definierenden Abfragen vorgenommenen Änderungen überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="3c957-156">If later you run the **Update Model Wizard** dialog, any changes made to the storage model, including defining queries, will be overwritten.</span></span>

 

## <a name="add-an-entity-type-to-the-model"></a><span data-ttu-id="3c957-157">Hinzufügen eines Entitätstyps zum Modell</span><span class="sxs-lookup"><span data-stu-id="3c957-157">Add an Entity Type to the Model</span></span>

<span data-ttu-id="3c957-158">In diesem Schritt fügen wir der Entitätstyp am konzeptionellen Modell mit dem EF Designer hinzu.</span><span class="sxs-lookup"><span data-stu-id="3c957-158">In this step we will add the entity type to the conceptual model using the EF Designer.</span></span>  <span data-ttu-id="3c957-159">Beachten Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="3c957-159">Note the following:</span></span>

-   <span data-ttu-id="3c957-160">Die **Namen** der Entität entspricht dem Wert von der **EntityType** -Attribut in der **EntitySet** Element oben.</span><span class="sxs-lookup"><span data-stu-id="3c957-160">The **Name** of the entity corresponds to the value of the **EntityType** attribute in the **EntitySet** element above.</span></span>
-   <span data-ttu-id="3c957-161">Die Eigenschaftennamen entsprechen, die von SQL-Anweisung im zurückgegebenen Spaltennamen die **DefiningQuery** Element oben.</span><span class="sxs-lookup"><span data-stu-id="3c957-161">The property names correspond to the column names returned by the SQL statement in the **DefiningQuery** element above.</span></span>
-   <span data-ttu-id="3c957-162">In diesem Beispiel wird durch einen aus drei Eigenschaften bestehenden Entitätsschlüssel sichergestellt, dass der Schlüsselwert eindeutig ist.</span><span class="sxs-lookup"><span data-stu-id="3c957-162">In this example, the entity key is composed of three properties to ensure a unique key value.</span></span>

<span data-ttu-id="3c957-163">Öffnen Sie das Modell im EF Designer.</span><span class="sxs-lookup"><span data-stu-id="3c957-163">Open the model in the EF Designer.</span></span>

-   <span data-ttu-id="3c957-164">Doppelklicken Sie auf die DefiningQueryModel.edmx.</span><span class="sxs-lookup"><span data-stu-id="3c957-164">Double-click the DefiningQueryModel.edmx.</span></span>
-   <span data-ttu-id="3c957-165">Sagen Sie **Ja** , die folgende Meldung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="3c957-165">Say **Yes** to the following message:</span></span>

    ![Warning2](~/ef6/media/warning2.png)

 

<span data-ttu-id="3c957-167">Im Entity Designer, der bietet eine Entwurfsoberfläche zum Bearbeiten des Modells, wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="3c957-167">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

-   <span data-ttu-id="3c957-168">Mit der rechten Maustaste in des Designers-Entwurfsoberfläche, und wählen **Add New**-&gt;**Entität...** .</span><span class="sxs-lookup"><span data-stu-id="3c957-168">Right-click the designer surface and select **Add New**-&gt;**Entity…**.</span></span>
-   <span data-ttu-id="3c957-169">Geben Sie **GradeReport** für den Namen der Entität und **CourseID** für die **Schlüsseleigenschaft**.</span><span class="sxs-lookup"><span data-stu-id="3c957-169">Specify **GradeReport** for the entity name and **CourseID** for the **Key Property**.</span></span>
-   <span data-ttu-id="3c957-170">Mit der rechten Maustaste die **GradeReport** Entität, und wählen **Add New** - &gt; **Skalareigenschaft**.</span><span class="sxs-lookup"><span data-stu-id="3c957-170">Right-click the **GradeReport** entity and select **Add New**-&gt; **Scalar Property**.</span></span>
-   <span data-ttu-id="3c957-171">Ändern Sie den Standardnamen der Eigenschaft, die **FirstName**.</span><span class="sxs-lookup"><span data-stu-id="3c957-171">Change the default name of the property to **FirstName**.</span></span>
-   <span data-ttu-id="3c957-172">Fügen Sie eine andere skalare Eigenschaft hinzu, und geben Sie **"LastName"** für den Namen.</span><span class="sxs-lookup"><span data-stu-id="3c957-172">Add another scalar property and specify **LastName** for the name.</span></span>
-   <span data-ttu-id="3c957-173">Fügen Sie eine andere skalare Eigenschaft hinzu, und geben Sie **auf Unternehmensniveau** für den Namen.</span><span class="sxs-lookup"><span data-stu-id="3c957-173">Add another scalar property and specify **Grade** for the name.</span></span>
-   <span data-ttu-id="3c957-174">In der **Eigenschaften** Ändern der **auf Unternehmensniveau**des **Typ** Eigenschaft **Decimal**.</span><span class="sxs-lookup"><span data-stu-id="3c957-174">In the **Properties** window, change the **Grade**’s **Type** property to **Decimal**.</span></span>
-   <span data-ttu-id="3c957-175">Wählen Sie die **FirstName** und **"LastName"** Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="3c957-175">Select the **FirstName** and **LastName** properties.</span></span>
-   <span data-ttu-id="3c957-176">In der **Eigenschaften** Ändern der **EntityKey** Eigenschaftswert **"true"**.</span><span class="sxs-lookup"><span data-stu-id="3c957-176">In the **Properties** window, change the **EntityKey** property value to **True**.</span></span>

<span data-ttu-id="3c957-177">Daher wurden die folgenden Elemente hinzugefügt, um die **CSDL** Abschnitt der EDMX-Datei.</span><span class="sxs-lookup"><span data-stu-id="3c957-177">As a result, the following elements were added to the **CSDL** section of the .edmx file.</span></span>

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a><span data-ttu-id="3c957-178">Ordnen Sie den Entitätstyp der definierenden Abfrage</span><span class="sxs-lookup"><span data-stu-id="3c957-178">Map the Defining Query to the Entity Type</span></span>

<span data-ttu-id="3c957-179">In diesem Schritt verwenden wir das Fenster "Mappingdetails" Zuordnen der konzeptionellen Entitätstypen und speicherentitätstypen.</span><span class="sxs-lookup"><span data-stu-id="3c957-179">In this step, we will use the Mapping Details window to map the conceptual and storage entity types.</span></span>

-   <span data-ttu-id="3c957-180">Mit der rechten Maustaste die **GradeReport** Entität auf die Entwurfsoberfläche, und wählen **Tabellenzuordnung**.</span><span class="sxs-lookup"><span data-stu-id="3c957-180">Right-click the **GradeReport** entity on the design surface and select **Table Mapping**.</span></span>  
    <span data-ttu-id="3c957-181">Die **Mappingdetails** Fenster wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="3c957-181">The **Mapping Details** window is displayed.</span></span>
-   <span data-ttu-id="3c957-182">Wählen Sie **GradeReport** aus der **&lt;Hinzufügen einer Tabelle oder Sicht&gt;** Dropdown-Liste (befindet sich im **Tabelle**s).</span><span class="sxs-lookup"><span data-stu-id="3c957-182">Select **GradeReport** from the **&lt;Add a Table or View&gt;** dropdown list (located under **Table**s).</span></span>  
    <span data-ttu-id="3c957-183">Zuordnungen zwischen dem konzeptionellen Standard und Storage **GradeReport** Entitätstyp angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="3c957-183">Default mappings between the conceptual and storage **GradeReport** entity type appear.</span></span>  
    <span data-ttu-id="3c957-184">![MappingDetails3](~/ef6/media/mappingdetails.png)</span><span class="sxs-lookup"><span data-stu-id="3c957-184">![MappingDetails3](~/ef6/media/mappingdetails.png)</span></span>

<span data-ttu-id="3c957-185">Daher die **EntitySetMapping** Element dem mappingabschnitt der EDMX-Datei hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="3c957-185">As a result, the **EntitySetMapping** element is added to the mapping section of the .edmx file.</span></span> 

``` xml
    <EntitySetMapping Name="GradeReports">
      <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.GradeReport)">
        <MappingFragment StoreEntitySet="GradeReport">
          <ScalarProperty Name="LastName" ColumnName="LastName" />
          <ScalarProperty Name="FirstName" ColumnName="FirstName" />
          <ScalarProperty Name="Grade" ColumnName="Grade" />
          <ScalarProperty Name="CourseID" ColumnName="CourseID" />
        </MappingFragment>
      </EntityTypeMapping>
    </EntitySetMapping>
```

-   <span data-ttu-id="3c957-186">Kompilieren Sie die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="3c957-186">Compile the application.</span></span>

 

## <a name="call-the-defining-query-in-your-code"></a><span data-ttu-id="3c957-187">Die definierende Abfrage im Code aufrufen.</span><span class="sxs-lookup"><span data-stu-id="3c957-187">Call the Defining Query in your Code</span></span>

<span data-ttu-id="3c957-188">Sie können jetzt die definierende Abfrage ausführen, mit der **GradeReport** Entitätstyp.</span><span class="sxs-lookup"><span data-stu-id="3c957-188">You can now execute the defining query by using the **GradeReport** entity type.</span></span> 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
