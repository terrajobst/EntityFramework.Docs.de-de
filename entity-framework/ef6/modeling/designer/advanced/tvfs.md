---
title: Tabellenwertfunktionen (TVFs) – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
ms.openlocfilehash: f4b8c1bd442bac67a06584bd23c040381374f303
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993246"
---
# <a name="table-valued-functions-tvfs"></a><span data-ttu-id="7835f-102">Tabellenwertfunktionen (TVFs)</span><span class="sxs-lookup"><span data-stu-id="7835f-102">Table-Valued Functions (TVFs)</span></span>
> [!NOTE]
> <span data-ttu-id="7835f-103">**EF5 oder höher, nur** -APIs, die Funktionen erläutert, die auf dieser Seite usw. in Entity Framework 5 eingeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="7835f-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="7835f-104">Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.</span><span class="sxs-lookup"><span data-stu-id="7835f-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="7835f-105">Die video und schrittweise exemplarische Vorgehensweise zeigt, wie Tabellenwertfunktionen (TVFs) mit dem Entity Framework Designer zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="7835f-105">The video and step-by-step walkthrough shows how to map table-valued functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="7835f-106">Darüber hinaus veranschaulicht eine TVF von LINQ-Abfragen aufrufen.</span><span class="sxs-lookup"><span data-stu-id="7835f-106">It also demonstrates how to call a TVF from a LINQ query.</span></span>

<span data-ttu-id="7835f-107">TVFs werden derzeit nur in der Datenbank-First-Workflow unterstützt.</span><span class="sxs-lookup"><span data-stu-id="7835f-107">TVFs are currently only supported in the Database First workflow.</span></span>

<span data-ttu-id="7835f-108">TVF-Unterstützung wurde in Entity Framework, Version 5 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="7835f-108">TVF support was introduced in Entity Framework version 5.</span></span> <span data-ttu-id="7835f-109">Beachten Sie, um die neuen Features wie Table-valued Function, Enumerationen und räumliche Typen, die Sie .NET Framework 4.5 als Ziel müssen zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="7835f-109">Note that to use the new features like table-valued functions, enums, and spatial types you must target .NET Framework 4.5.</span></span> <span data-ttu-id="7835f-110">Visual Studio 2012 ist standardmäßig die Zielversion .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="7835f-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="7835f-111">TVFs sind sehr ähnlich wie gespeicherte Prozeduren mit einem wichtigen Unterschied: das Ergebnis einer TVF zusammensetzbar ist.</span><span class="sxs-lookup"><span data-stu-id="7835f-111">TVFs are very similar to stored procedures with one key difference: the result of a TVF is composable.</span></span> <span data-ttu-id="7835f-112">Das bedeutet, dass die Ergebnisse von einer TVF in einer LINQ-Abfrage verwendet werden können, während die Ergebnisse einer gespeicherten Prozedur nicht.</span><span class="sxs-lookup"><span data-stu-id="7835f-112">That means the results from a TVF can be used in a LINQ query while the results of a stored procedure cannot.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="7835f-113">Video ansehen</span><span class="sxs-lookup"><span data-stu-id="7835f-113">Watch the video</span></span>

<span data-ttu-id="7835f-114">**Präsentiert von**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="7835f-114">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="7835f-115">[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span><span class="sxs-lookup"><span data-stu-id="7835f-115">[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="7835f-116">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="7835f-116">Pre-Requisites</span></span>

<span data-ttu-id="7835f-117">Um diese exemplarische Vorgehensweise abzuschließen, müssen Sie:</span><span class="sxs-lookup"><span data-stu-id="7835f-117">To complete this walkthrough, you need to:</span></span>

- <span data-ttu-id="7835f-118">Installieren Sie die [Datenbank "School"](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="7835f-118">Install the [School database](~/ef6/resources/school-database.md).</span></span>

- <span data-ttu-id="7835f-119">Eine aktuelle Version von Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7835f-119">Have a recent version of Visual Studio</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="7835f-120">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="7835f-120">Set up the Project</span></span>

1.  <span data-ttu-id="7835f-121">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7835f-121">Open Visual Studio</span></span>
2.  <span data-ttu-id="7835f-122">Auf der **Datei** Startmenü **neu**, und klicken Sie dann auf **Projekt**</span><span class="sxs-lookup"><span data-stu-id="7835f-122">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="7835f-123">Klicken Sie im linken Bereich auf **Visual C\#**, und wählen Sie dann die **Konsole** Vorlage</span><span class="sxs-lookup"><span data-stu-id="7835f-123">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="7835f-124">Geben Sie **TVF** als Namen für das Projekt und auf **OK**</span><span class="sxs-lookup"><span data-stu-id="7835f-124">Enter **TVF** as the name of the project and click **OK**</span></span>

## <a name="add-a-tvf-to-the-database"></a><span data-ttu-id="7835f-125">Hinzufügen einer Tabellenwertfunktion in der Datenbank</span><span class="sxs-lookup"><span data-stu-id="7835f-125">Add a TVF to the Database</span></span>

-   <span data-ttu-id="7835f-126">Wählen Sie **Ansicht – Profiler -&gt; Objekt-Explorer von SQL Server**</span><span class="sxs-lookup"><span data-stu-id="7835f-126">Select **View -&gt; SQL Server Object Explorer**</span></span>
-   <span data-ttu-id="7835f-127">Wenn LocalDB nicht in der Liste der Server ist: mit der rechten Maustaste auf **SQL Server** , und wählen Sie **SQL Server hinzufügen** verwenden Sie die Standardeinstellung **Windows-Authentifizierung** für die Verbindung mit dem Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="7835f-127">If LocalDB is not in the list of servers: Right-click on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB server</span></span>
-   <span data-ttu-id="7835f-128">Erweitern Sie den Knoten für LocalDB</span><span class="sxs-lookup"><span data-stu-id="7835f-128">Expand the LocalDB node</span></span>
-   <span data-ttu-id="7835f-129">Unter dem Knoten "Datenbanken", mit der rechten Maustaste des Knotens der Datenbank "School", und wählen Sie **neue Abfrage...**</span><span class="sxs-lookup"><span data-stu-id="7835f-129">Under the Databases node, right-click the School database node and select **New Query…**</span></span>
-   <span data-ttu-id="7835f-130">Fügen Sie in T-SQL-Editor die folgende Definition einer Tabellenwertfunktion</span><span class="sxs-lookup"><span data-stu-id="7835f-130">In T-SQL Editor, paste the following TVF definition</span></span>

``` SQL
CREATE FUNCTION [dbo].[GetStudentGradesForCourse]

(@CourseID INT)

RETURNS TABLE

RETURN
    SELECT [EnrollmentID],
           [CourseID],
           [StudentID],
           [Grade]
    FROM   [dbo].[StudentGrade]
    WHERE  CourseID = @CourseID
```

-   <span data-ttu-id="7835f-131">Klicken Sie auf der rechten Maustaste auf den T-SQL-Editor, und wählen Sie **ausführen**</span><span class="sxs-lookup"><span data-stu-id="7835f-131">Click the right mouse button on the T-SQL editor and select **Execute**</span></span>
-   <span data-ttu-id="7835f-132">Die GetStudentGradesForCourse-Funktion wird die Datenbank "School" hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="7835f-132">The GetStudentGradesForCourse function is added to the School database</span></span>

 

## <a name="create-a-model"></a><span data-ttu-id="7835f-133">Erstellen eines Modells</span><span class="sxs-lookup"><span data-stu-id="7835f-133">Create a Model</span></span>

1.  <span data-ttu-id="7835f-134">Mit der rechten Maustaste des Projektnamen im Projektmappen-Explorer, zeigen Sie auf **hinzufügen**, und klicken Sie dann auf **neues Element**</span><span class="sxs-lookup"><span data-stu-id="7835f-134">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="7835f-135">Wählen Sie **Daten** aus dem linken Menü und wählen Sie dann **ADO.NET Entity Data Model** in die **Vorlagen** Bereich</span><span class="sxs-lookup"><span data-stu-id="7835f-135">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the **Templates** pane</span></span>
3.  <span data-ttu-id="7835f-136">Geben Sie **TVFModel.edmx** für den Dateinamen ein, und klicken Sie dann auf **hinzufügen**</span><span class="sxs-lookup"><span data-stu-id="7835f-136">Enter **TVFModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="7835f-137">Wählen Sie im Dialogfeld "Modellinhalte" **aus Datenbank generieren**, und klicken Sie dann auf **weiter**</span><span class="sxs-lookup"><span data-stu-id="7835f-137">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next**</span></span>
5.  <span data-ttu-id="7835f-138">Klicken Sie auf **neue Verbindung** EINGABETASTE **(Localdb)\\Mssqllocaldb** die Textlänge des Namens im Feld Geben Sie **School** benennen Sie für die Datenbank auf **OK**</span><span class="sxs-lookup"><span data-stu-id="7835f-138">Click **New Connection** Enter **(localdb)\\mssqllocaldb** in the Server name text box Enter **School** for the database name Click **OK**</span></span>
6.  <span data-ttu-id="7835f-139">Auswählen Ihrer Datenbankobjekte Dialogfeld unter die **Tabellen** Knoten die **Person**, **StudentGrade**, und **Kurs** Tabellen</span><span class="sxs-lookup"><span data-stu-id="7835f-139">In the Choose Your Database Objects dialog box, under the **Tables** node, select the **Person**, **StudentGrade**, and **Course** tables</span></span>
7.  <span data-ttu-id="7835f-140">Wählen Sie die **GetStudentGradesForCourse** Funktion befindet sich unter der **gespeicherte Prozeduren und Funktionen** Knoten beachten Sie, beginnend mit Visual Studio 2012, der Entity Designer können Sie für den Batch-Import Ihre gespeicherten Prozeduren und Funktionen</span><span class="sxs-lookup"><span data-stu-id="7835f-140">Select the **GetStudentGradesForCourse** function located under the **Stored Procedures and Functions** node Note, that starting with Visual Studio 2012, the Entity Designer allows you to batch import your Stored Procedures and Functions</span></span>
8.  <span data-ttu-id="7835f-141">Klicken Sie auf **Fertig stellen**</span><span class="sxs-lookup"><span data-stu-id="7835f-141">Click **Finish**</span></span>
9.  <span data-ttu-id="7835f-142">Im Entity Designer, der bietet eine Entwurfsoberfläche zum Bearbeiten des Modells, wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="7835f-142">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="7835f-143">Alle Objekte, die Sie ausgewählt haben, in der **Datenbankobjekte auswählen** im Dialogfeld zum Modell hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="7835f-143">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>
10. <span data-ttu-id="7835f-144">Standardmäßig wird die Ergebnisform jedes importierte gespeicherte Prozedur oder Funktion automatisch ein neuer komplexer Typ in Ihr Entitätsmodell sein.</span><span class="sxs-lookup"><span data-stu-id="7835f-144">By default, the result shape of each imported stored procedure or function will automatically become a new complex type in your entity model.</span></span> <span data-ttu-id="7835f-145">Aber die Ergebnisse der GetStudentGradesForCourse-Funktion auf die StudentGrade-Entität zugeordnet werden soll: mit der rechten Maustaste Entwurfsoberfläche, und wählen **Modellbrowser** im Modellbrowser wählen **Funktionsimporte**, und doppelklicken Sie dann auf die **GetStudentGradesForCourse** Funktion In der Funktionsimport bearbeiten wählen Sie im Dialogfeld **Entitäten** , und wählen Sie **StudentGrade**</span><span class="sxs-lookup"><span data-stu-id="7835f-145">But we want to map the results of the GetStudentGradesForCourse function to the StudentGrade entity: Right-click the design surface and select **Model Browser** In Model Browser, select **Function Imports**, and then double-click the **GetStudentGradesForCourse** function In the Edit Function Import dialog box, select **Entities** and choose **StudentGrade**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="7835f-146">Speichern und Abrufen von Daten</span><span class="sxs-lookup"><span data-stu-id="7835f-146">Persist and Retrieve Data</span></span>

<span data-ttu-id="7835f-147">Öffnen Sie die Datei, in die Main-Methode definiert ist.</span><span class="sxs-lookup"><span data-stu-id="7835f-147">Open the file where the Main method is defined.</span></span> <span data-ttu-id="7835f-148">Fügen Sie der Main-Funktion mit den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="7835f-148">Add the following code into the Main function.</span></span>

<span data-ttu-id="7835f-149">Der folgende Code veranschaulicht, wie Sie eine Abfrage zu erstellen, die eine Funktion mit Tabellenrückgabe verwendet, wird.</span><span class="sxs-lookup"><span data-stu-id="7835f-149">The following code demonstrates how to build a query that uses a Table-valued Function.</span></span> <span data-ttu-id="7835f-150">Die Abfrage projiziert die Ergebnisse in einen anonymen Typ, der verwandte Kurstitel und verwandte Schüler/Studenten mit eine Grade-Eigenschaft größer oder gleich 3.5 enthält.</span><span class="sxs-lookup"><span data-stu-id="7835f-150">The query projects the results into an anonymous type that contains the related Course title and related students with a grade greater or equal to 3.5.</span></span>

``` csharp
using (var context = new SchoolEntities())
{
    var CourseID = 4022;
    var Grade = 3.5M;

    // Return all the best students in the Microeconomics class.
    var students = from s in context.GetStudentGradesForCourse(CourseID)
                            where s.Grade >= Grade
                            select new
                            {
                                s.Person,
                                s.Course.Title
                            };

    foreach (var result in students)
    {
        Console.WriteLine(
            "Couse: {0}, Student: {1} {2}",
            result.Title,  
            result.Person.FirstName,  
            result.Person.LastName);
    }
}
```

<span data-ttu-id="7835f-151">Kompilieren Sie die Anwendung, und führen Sie sie aus.</span><span class="sxs-lookup"><span data-stu-id="7835f-151">Compile and run the application.</span></span> <span data-ttu-id="7835f-152">Das Programm erzeugt die folgende Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="7835f-152">The program produces the following output:</span></span>

```
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a><span data-ttu-id="7835f-153">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="7835f-153">Summary</span></span>

<span data-ttu-id="7835f-154">In dieser exemplarischen Vorgehensweise erläutert, wie Tabellenwertfunktionen (TVFs) mit dem Entity Framework Designer zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="7835f-154">In this walkthrough we looked at how to map Table-valued Functions (TVFs) using the Entity Framework Designer.</span></span> <span data-ttu-id="7835f-155">Es wurde auch veranschaulicht, wie eine TVF von LINQ-Abfragen aufrufen.</span><span class="sxs-lookup"><span data-stu-id="7835f-155">It also demonstrated how to call a TVF from a LINQ query.</span></span>
