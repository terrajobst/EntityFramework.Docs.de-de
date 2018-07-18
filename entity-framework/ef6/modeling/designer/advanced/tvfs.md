---
title: Tabellenwertfunktionen (TVFs) – EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: f019c97b-87b0-4e93-98f4-2c539f77b2dc
caps.latest.revision: 3
ms.openlocfilehash: 7d652725a2655b691b03aa3f43103753fe72ede7
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121375"
---
# <a name="table-valued-functions-tvfs"></a>Tabellenwertfunktionen (TVFs)
> [!NOTE]
> **EF5 oder höher, nur** -APIs, die Funktionen erläutert, die auf dieser Seite usw. in Entity Framework 5 eingeführt wurden. Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.

Die video und schrittweise exemplarische Vorgehensweise zeigt, wie Tabellenwertfunktionen (TVFs) mit dem Entity Framework Designer zugeordnet wird. Darüber hinaus veranschaulicht eine TVF von LINQ-Abfragen aufrufen.

TVFs werden derzeit nur in der Datenbank-First-Workflow unterstützt.

TVF-Unterstützung wurde in Entity Framework, Version 5 eingeführt. Beachten Sie, um die neuen Features wie Table-valued Function, Enumerationen und räumliche Typen, die Sie .NET Framework 4.5 als Ziel müssen zu verwenden. Visual Studio 2012 ist standardmäßig die Zielversion .NET 4.5.

TVFs sind sehr ähnlich wie gespeicherte Prozeduren mit einem wichtigen Unterschied: das Ergebnis einer TVF zusammensetzbar ist. Das bedeutet, dass die Ergebnisse von einer TVF in einer LINQ-Abfrage verwendet werden können, während die Ergebnisse einer gespeicherten Prozedur nicht.

## <a name="watch-the-video"></a>Video ansehen

**Präsentiert von**: Julia Kornich

[WMV](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.wmv) | [MP4](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-mp4video-tvf.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/6/0/A/60A6E474-5EF3-4E1E-B9EA-F51D2DDB446A/HDI-ITPro-MSDN-winvideo-tvf.zip)

## <a name="pre-requisites"></a>Voraussetzungen

Um diese exemplarische Vorgehensweise abzuschließen, müssen Sie:

- Installieren Sie die [Datenbank "School"](~/ef6/resources/school-database.md).

- Eine aktuelle Version von Visual Studio

## <a name="set-up-the-project"></a>Einrichten des Projekts

1.  Öffnen Sie Visual Studio.
2.  Auf der **Datei** Startmenü **neu**, und klicken Sie dann auf **Projekt**
3.  Klicken Sie im linken Bereich auf **Visual C\#**, und wählen Sie dann die **Konsole** Vorlage
4.  Geben Sie **TVF** als Namen für das Projekt und auf **OK**

## <a name="add-a-tvf-to-the-database"></a>Hinzufügen einer Tabellenwertfunktion in der Datenbank

-   Wählen Sie **Ansicht – Profiler -&gt; Objekt-Explorer von SQL Server**
-   Wenn LocalDB nicht in der Liste der Server ist: mit der rechten Maustaste auf **SQL Server** , und wählen Sie **SQL Server hinzufügen** verwenden Sie die Standardeinstellung **Windows-Authentifizierung** für die Verbindung mit dem Server LocalDB
-   Erweitern Sie den Knoten für LocalDB
-   Unter dem Knoten "Datenbanken", mit der rechten Maustaste des Knotens der Datenbank "School", und wählen Sie **neue Abfrage...**
-   Fügen Sie in T-SQL-Editor die folgende Definition einer Tabellenwertfunktion

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

-   Klicken Sie auf der rechten Maustaste auf den T-SQL-Editor, und wählen Sie **ausführen**
-   Die GetStudentGradesForCourse-Funktion wird die Datenbank "School" hinzugefügt.

 

## <a name="create-a-model"></a>Erstellen eines Modells

1.  Mit der rechten Maustaste des Projektnamen im Projektmappen-Explorer, zeigen Sie auf **hinzufügen**, und klicken Sie dann auf **neues Element**
2.  Wählen Sie **Daten** aus dem linken Menü und wählen Sie dann **ADO.NET Entity Data Model** in die **Vorlagen** Bereich
3.  Geben Sie **TVFModel.edmx** für den Dateinamen ein, und klicken Sie dann auf **hinzufügen**
4.  Wählen Sie im Dialogfeld "Modellinhalte" **aus Datenbank generieren**, und klicken Sie dann auf **weiter**
5.  Klicken Sie auf **neue Verbindung** EINGABETASTE **(Localdb)\\Mssqllocaldb** die Textlänge des Namens im Feld Geben Sie **School** benennen Sie für die Datenbank auf **OK**
6.  Auswählen Ihrer Datenbankobjekte Dialogfeld unter die **Tabellen** Knoten die **Person**, **StudentGrade**, und **Kurs** Tabellen
7.  Wählen Sie die **GetStudentGradesForCourse** Funktion befindet sich unter der **gespeicherte Prozeduren und Funktionen** Knoten beachten Sie, beginnend mit Visual Studio 2012, der Entity Designer können Sie für den Batch-Import Ihre gespeicherten Prozeduren und Funktionen
8.  Klicken Sie auf **Fertig stellen**
9.  Im Entity Designer, der bietet eine Entwurfsoberfläche zum Bearbeiten des Modells, wird angezeigt. Alle Objekte, die Sie ausgewählt haben, in der **Datenbankobjekte auswählen** im Dialogfeld zum Modell hinzugefügt werden.
10. Standardmäßig wird die Ergebnisform jedes importierte gespeicherte Prozedur oder Funktion automatisch ein neuer komplexer Typ in Ihr Entitätsmodell sein. Aber die Ergebnisse der GetStudentGradesForCourse-Funktion auf die StudentGrade-Entität zugeordnet werden soll: mit der rechten Maustaste Entwurfsoberfläche, und wählen **Modellbrowser** im Modellbrowser wählen **Funktionsimporte**, und doppelklicken Sie dann auf die **GetStudentGradesForCourse** Funktion In der Funktionsimport bearbeiten wählen Sie im Dialogfeld **Entitäten** , und wählen Sie **StudentGrade**

## <a name="persist-and-retrieve-data"></a>Speichern und Abrufen von Daten

Öffnen Sie die Datei, in die Main-Methode definiert ist. Fügen Sie der Main-Funktion mit den folgenden Code.

Der folgende Code veranschaulicht, wie Sie eine Abfrage zu erstellen, die eine Funktion mit Tabellenrückgabe verwendet, wird. Die Abfrage projiziert die Ergebnisse in einen anonymen Typ, der verwandte Kurstitel und verwandte Schüler/Studenten mit eine Grade-Eigenschaft größer oder gleich 3.5 enthält.

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

Kompilieren Sie die Anwendung, und führen Sie sie aus. Das Programm erzeugt die folgende Ausgabe:

```
Couse: Microeconomics, Student: Arturo Anand
Couse: Microeconomics, Student: Carson Bryant
```

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise erläutert, wie Tabellenwertfunktionen (TVFs) mit dem Entity Framework Designer zugeordnet. Es wurde auch veranschaulicht, wie eine TVF von LINQ-Abfragen aufrufen.
