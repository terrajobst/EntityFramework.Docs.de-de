---
title: Gespeicherte Prozeduren – EF6 Designer Abfragen
author: divega
ms.date: 10/23/2016
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
ms.openlocfilehash: 6284b10261e6f3b9bf69d1c15e121988e4976d48
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489946"
---
# <a name="designer-query-stored-procedures"></a><span data-ttu-id="b0b0e-102">Designer-Abfrage, gespeicherten Prozeduren</span><span class="sxs-lookup"><span data-stu-id="b0b0e-102">Designer Query Stored Procedures</span></span>
<span data-ttu-id="b0b0e-103">Dieser schrittweise erläuterten exemplarischen zeigen, wie das Entity Framework Designer (EF-Designer) zum Importieren von gespeicherter Prozeduren verwenden, in ein Modell aus, und rufen Sie dann die importierten gespeicherten Prozeduren, um Ergebnisse abzurufen.</span><span class="sxs-lookup"><span data-stu-id="b0b0e-103">This step-by-step walkthrough show how to use the Entity Framework Designer (EF Designer) to import stored procedures into a model and then call the imported stored procedures to retrieve results.</span></span> 

<span data-ttu-id="b0b0e-104">Beachten Sie, dass Code First-Zuordnung zu gespeicherten Prozeduren oder Funktionen nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="b0b0e-104">Note, that Code First does not support mapping to stored procedures or functions.</span></span> <span data-ttu-id="b0b0e-105">Allerdings können Sie gespeicherte Prozeduren oder Funktionen mithilfe der System.Data.Entity.DbSet.SqlQuery-Methode aufrufen.</span><span class="sxs-lookup"><span data-stu-id="b0b0e-105">However, you can call stored procedures or functions by using the System.Data.Entity.DbSet.SqlQuery method.</span></span> <span data-ttu-id="b0b0e-106">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b0b0e-106">For example:</span></span>
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a><span data-ttu-id="b0b0e-107">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="b0b0e-107">Prerequisites</span></span>

<span data-ttu-id="b0b0e-108">Um die exemplarische Vorgehensweise nachzuvollziehen, benötigen Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="b0b0e-108">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="b0b0e-109">Eine aktuelle Version von Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b0b0e-109">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="b0b0e-110">Die [Beispieldatenbank ' School '](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="b0b0e-110">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="b0b0e-111">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="b0b0e-111">Set up the Project</span></span>

-   <span data-ttu-id="b0b0e-112">Öffnen Sie Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="b0b0e-112">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="b0b0e-113">Wählen Sie **Dateien&gt; neu –&gt; Projekt**</span><span class="sxs-lookup"><span data-stu-id="b0b0e-113">Select **File-&gt; New -&gt; Project**</span></span>
-   <span data-ttu-id="b0b0e-114">Klicken Sie im linken Bereich auf **Visual C\#**, und wählen Sie dann die **Konsole** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="b0b0e-114">In the left pane, click **Visual C\#**, and then select the **Console** template.</span></span>
-   <span data-ttu-id="b0b0e-115">Geben Sie **EFwithSProcsSample** als Namen.</span><span class="sxs-lookup"><span data-stu-id="b0b0e-115">Enter **EFwithSProcsSample** as the name.</span></span>
-   <span data-ttu-id="b0b0e-116">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="b0b0e-116">Select **OK**.</span></span>

## <a name="create-a-model"></a><span data-ttu-id="b0b0e-117">Erstellen eines Modells</span><span class="sxs-lookup"><span data-stu-id="b0b0e-117">Create a Model</span></span>

-   <span data-ttu-id="b0b0e-118">Mit der rechten Maustaste in des Projekts im Projektmappen-Explorer, und wählen Sie **hinzufügen –&gt; neues Element**.</span><span class="sxs-lookup"><span data-stu-id="b0b0e-118">Right-click the project in Solution Explorer and select **Add -&gt; New Item**.</span></span>
-   <span data-ttu-id="b0b0e-119">Wählen Sie **Daten** aus dem linken Menü und wählen Sie dann **ADO.NET Entity Data Model** im Bereich Vorlagen.</span><span class="sxs-lookup"><span data-stu-id="b0b0e-119">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="b0b0e-120">Geben Sie **EFwithSProcsModel.edmx** für den Dateinamen ein, und klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="b0b0e-120">Enter **EFwithSProcsModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="b0b0e-121">Wählen Sie im Dialogfeld "Modellinhalte" **aus Datenbank generieren**, und klicken Sie dann auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="b0b0e-121">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**.</span></span>
-   <span data-ttu-id="b0b0e-122">Klicken Sie auf **neue Verbindung**.</span><span class="sxs-lookup"><span data-stu-id="b0b0e-122">Click **New Connection**.</span></span>  
    <span data-ttu-id="b0b0e-123">Geben Sie den Servernamen, klicken Sie im Dialogfeld "Verbindungseigenschaften" (z. B. **(Localdb)\\Mssqllocaldb**), wählen die Authentifizierungsmethode, Typ **School** für den Datenbanknamen, und klicken Sie dann Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="b0b0e-123">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>  
    <span data-ttu-id="b0b0e-124">Das Dialogfeld "Wählen Sie Ihre Datenverbindung" wird mit Ihrem datenbankverbindungseinstellung aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="b0b0e-124">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="b0b0e-125">Klicken Sie im Dialogfeld "Datenbankobjekte auswählen" Überprüfen der **Tabellen** Kontrollkästchen, um alle Tabellen auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="b0b0e-125">In the Choose Your Database Objects dialog box, check the **Tables** checkbox to select all the tables.</span></span>  
    <span data-ttu-id="b0b0e-126">Wählen Sie außerdem die folgenden gespeicherten Prozeduren in der **gespeicherte Prozeduren und Funktionen** Knoten: **GetStudentGrades** und **GetDepartmentName**.</span><span class="sxs-lookup"><span data-stu-id="b0b0e-126">Also, select the following stored procedures under the **Stored Procedures and Functions** node: **GetStudentGrades** and **GetDepartmentName**.</span></span> 

    ![Importieren](~/ef6/media/import.jpg)

    <span data-ttu-id="b0b0e-128">*Ab Visual Studio 2012 die EF-Designer unterstützt Massenexport und-Import von gespeicherten Prozeduren. Die **Import ausgewählten gespeicherten Prozeduren und Funktionen in Elem Modell** ist standardmäßig aktiviert.*</span><span class="sxs-lookup"><span data-stu-id="b0b0e-128">*Starting with Visual Studio 2012 the EF Designer supports bulk import of stored procedures. The **Import selected stored procedures and functions into theentity model** is checked by default.*</span></span>
-   <span data-ttu-id="b0b0e-129">Klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="b0b0e-129">Click **Finish**.</span></span>

<span data-ttu-id="b0b0e-130">Standardmäßig wird die Ergebnisform jedes importierte gespeicherte Prozedur oder Funktion, die mehr als eine Spalte zurückgibt automatisch ein neuer komplexer Typ sein.</span><span class="sxs-lookup"><span data-stu-id="b0b0e-130">By default, the result shape of each imported stored procedure or function that returns more than one column will automatically become a new complex type.</span></span> <span data-ttu-id="b0b0e-131">In diesem Beispiel die Ergebnisse der zugeordnet werden soll die **GetStudentGrades** -Funktion der **StudentGrade** Entität und die Ergebnisse der **GetDepartmentName** zu **keine** (**keine** ist der Standardwert).</span><span class="sxs-lookup"><span data-stu-id="b0b0e-131">In this example we want to map the results of the **GetStudentGrades** function to the **StudentGrade** entity and the results of the **GetDepartmentName** to **none** (**none** is the default value).</span></span>

<span data-ttu-id="b0b0e-132">Damit ein Funktionsimport einen Entitätstyp zurückgegeben müssen die von der entsprechenden gespeicherten Prozedur zurückgegebenen Spalten genau mit die skalaren Eigenschaften der zurückgegebenen Entitätstyp übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="b0b0e-132">For a function import to return an entity type, the columns returned by the corresponding stored procedure must exactly match the scalar properties of the returned entity type.</span></span> <span data-ttu-id="b0b0e-133">Ein Funktionsimport kann Auflistungen von einfachen Typen, komplexen Typen oder kein Wert zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="b0b0e-133">A function import can also return collections of simple types, complex types, or no value.</span></span>

-   <span data-ttu-id="b0b0e-134">Mit der rechten Maustaste Entwurfsoberfläche, und wählen **Modellbrowser**.</span><span class="sxs-lookup"><span data-stu-id="b0b0e-134">Right-click the design surface and select **Model Browser**.</span></span>
-   <span data-ttu-id="b0b0e-135">In **Modellbrowser**Option **Funktionsimporte**, und doppelklicken Sie dann auf die **GetStudentGrades** Funktion.</span><span class="sxs-lookup"><span data-stu-id="b0b0e-135">In **Model Browser**, select **Function Imports**, and then double-click the **GetStudentGrades** function.</span></span>
-   <span data-ttu-id="b0b0e-136">Wählen Sie in das Dialogfeld Funktionsimport bearbeiten, **Entitäten** , und wählen Sie **StudentGrade**.</span><span class="sxs-lookup"><span data-stu-id="b0b0e-136">In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**.</span></span>  
    <span data-ttu-id="b0b0e-137">*Die **Funktionsimport zusammensetzbar ist** Kontrollkästchen am Anfang der **Funktionsimporte** Dialogfeld können Sie die zusammensetzbare Funktionen zuordnen. Wenn Sie dieses Kontrollkästchen deaktivieren, erscheinen nur zusammensetzbare Funktionen (Table-valued Function) in der **gespeicherte Prozedur / Funktionsnamen** Dropdown-Liste. Wenn dieses Kontrollkästchen nicht aktiviert, werden nur für nicht zusammensetzbare Funktionen in der Liste angezeigt werden.*</span><span class="sxs-lookup"><span data-stu-id="b0b0e-137">*The **Function Import is composable** checkbox at the top of the **Function Imports** dialog will let you map to composable functions. If you do check this box, only composable functions (Table-valued Functions) will appear in the **Stored Procedure / Function Name** drop-down list. If you do not check this box, only non-composable functions will be shown in the list.*</span></span>

## <a name="use-the-model"></a><span data-ttu-id="b0b0e-138">Verwenden Sie das Modell</span><span class="sxs-lookup"><span data-stu-id="b0b0e-138">Use the Model</span></span>

<span data-ttu-id="b0b0e-139">Öffnen der **"Program.cs"** -Datei, in der **Main** Methode definiert ist.</span><span class="sxs-lookup"><span data-stu-id="b0b0e-139">Open the **Program.cs** file where the **Main** method is defined.</span></span> <span data-ttu-id="b0b0e-140">Fügen Sie der Main-Funktion mit den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="b0b0e-140">Add the following code into the Main function.</span></span>

<span data-ttu-id="b0b0e-141">Der Code ruft zwei gespeicherte Prozeduren: **GetStudentGrades** (gibt **StudentGrades** für den angegebenen *"StudentID"*) und **GetDepartmentName** (der Name der Abteilung im Ausgabeparameter zurückgegeben).</span><span class="sxs-lookup"><span data-stu-id="b0b0e-141">The code calls two stored procedures: **GetStudentGrades** (returns **StudentGrades** for the specified *StudentId*) and **GetDepartmentName** (returns the name of the department in the output parameter).</span></span>  

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        // Specify the Student ID.
        int studentId = 2;

        // Call GetStudentGrades and iterate through the returned collection.
        foreach (StudentGrade grade in context.GetStudentGrades(studentId))
        {
            Console.WriteLine("StudentID: {0}\tSubject={1}", studentId, grade.Subject);
            Console.WriteLine("Student grade: " + grade.Grade);
        }

        // Call GetDepartmentName.
        // Declare the name variable that will contain the value returned by the output parameter.
        ObjectParameter name = new ObjectParameter("Name", typeof(String));
        context.GetDepartmentName(1, name);
        Console.WriteLine("The department name is {0}", name.Value);

    }
```

<span data-ttu-id="b0b0e-142">Kompilieren Sie die Anwendung, und führen Sie sie aus.</span><span class="sxs-lookup"><span data-stu-id="b0b0e-142">Compile and run the application.</span></span> <span data-ttu-id="b0b0e-143">Das Programm erzeugt die folgende Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="b0b0e-143">The program produces the following output:</span></span>

```
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a><span data-ttu-id="b0b0e-144">Ausgabeparameter</span><span class="sxs-lookup"><span data-stu-id="b0b0e-144">Output Parameters</span></span>
-----------------

<span data-ttu-id="b0b0e-145">Wenn Output-Parameter verwendet werden, werden ihre Werte nicht verfügbar, bis die Ergebnisse vollständig gelesen wurden.</span><span class="sxs-lookup"><span data-stu-id="b0b0e-145">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="b0b0e-146">Dies liegt an der zugrunde liegende Verhalten von DbDataReader, finden Sie unter [Abrufen von Daten mit einem DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) Weitere Details.</span><span class="sxs-lookup"><span data-stu-id="b0b0e-146">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>
