---
title: Gespeicherte Prozeduren – EF6 Designer Abfragen
author: divega
ms.date: 10/23/2016
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
ms.openlocfilehash: 04478ea1c8cd43a7ba4ee788e464992af3de7f64
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283900"
---
# <a name="designer-query-stored-procedures"></a>Designer-Abfrage, gespeicherten Prozeduren
Dieser schrittweise erläuterten exemplarischen zeigen, wie das Entity Framework Designer (EF-Designer) zum Importieren von gespeicherter Prozeduren verwenden, in ein Modell aus, und rufen Sie dann die importierten gespeicherten Prozeduren, um Ergebnisse abzurufen. 

Beachten Sie, dass Code First-Zuordnung zu gespeicherten Prozeduren oder Funktionen nicht unterstützt. Allerdings können Sie gespeicherte Prozeduren oder Funktionen mithilfe der System.Data.Entity.DbSet.SqlQuery-Methode aufrufen. Zum Beispiel:
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a>Erforderliche Komponenten

Um die exemplarische Vorgehensweise nachzuvollziehen, benötigen Sie Folgendes:

- Eine aktuelle Version von Visual Studio.
- Die [Beispieldatenbank ' School '](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Einrichten des Projekts

-   Öffnen Sie Visual Studio 2012.
-   Wählen Sie **Dateien&gt; neu –&gt; Projekt**
-   Klicken Sie im linken Bereich auf **Visual C\#**, und wählen Sie dann die **Konsole** Vorlage.
-   Geben Sie **EFwithSProcsSample** als Namen.
-   Klicken Sie auf **OK**.

## <a name="create-a-model"></a>Erstellen eines Modells

-   Mit der rechten Maustaste in des Projekts im Projektmappen-Explorer, und wählen Sie **hinzufügen –&gt; neues Element**.
-   Wählen Sie **Daten** aus dem linken Menü und wählen Sie dann **ADO.NET Entity Data Model** im Bereich Vorlagen.
-   Geben Sie **EFwithSProcsModel.edmx** für den Dateinamen ein, und klicken Sie dann auf **hinzufügen**.
-   Wählen Sie im Dialogfeld "Modellinhalte" **aus Datenbank generieren**, und klicken Sie dann auf **Weiter**.
-   Klicken Sie auf **neue Verbindung**.  
    Geben Sie den Servernamen, klicken Sie im Dialogfeld "Verbindungseigenschaften" (z. B. **(Localdb)\\Mssqllocaldb**), wählen die Authentifizierungsmethode, Typ **School** für den Datenbanknamen, und klicken Sie dann Klicken Sie auf **OK**.  
    Das Dialogfeld "Wählen Sie Ihre Datenverbindung" wird mit Ihrem datenbankverbindungseinstellung aktualisiert.
-   Klicken Sie im Dialogfeld "Datenbankobjekte auswählen" Überprüfen der **Tabellen** Kontrollkästchen, um alle Tabellen auszuwählen.  
    Wählen Sie außerdem die folgenden gespeicherten Prozeduren in der **gespeicherte Prozeduren und Funktionen** Knoten: **GetStudentGrades** und **GetDepartmentName**. 

    ![Importieren](~/ef6/media/import.jpg)

    *Ab Visual Studio 2012 die EF-Designer unterstützt Massenexport und-Import von gespeicherten Prozeduren. Die **Import ausgewählten gespeicherten Prozeduren und Funktionen in Elem Modell** ist standardmäßig aktiviert.*
-   Klicken Sie auf **Fertig stellen**.

Standardmäßig wird die Ergebnisform jedes importierte gespeicherte Prozedur oder Funktion, die mehr als eine Spalte zurückgibt automatisch ein neuer komplexer Typ sein. In diesem Beispiel die Ergebnisse der zugeordnet werden soll die **GetStudentGrades** -Funktion der **StudentGrade** Entität und die Ergebnisse der **GetDepartmentName** zu **keine** (**keine** ist der Standardwert).

Damit ein Funktionsimport einen Entitätstyp zurückgegeben müssen die von der entsprechenden gespeicherten Prozedur zurückgegebenen Spalten genau mit die skalaren Eigenschaften der zurückgegebenen Entitätstyp übereinstimmen. Ein Funktionsimport kann Auflistungen von einfachen Typen, komplexen Typen oder kein Wert zurückgegeben werden.

-   Mit der rechten Maustaste Entwurfsoberfläche, und wählen **Modellbrowser**.
-   In **Modellbrowser**Option **Funktionsimporte**, und doppelklicken Sie dann auf die **GetStudentGrades** Funktion.
-   Wählen Sie in das Dialogfeld Funktionsimport bearbeiten, **Entitäten** , und wählen Sie **StudentGrade**.  
    *Die **Funktionsimport zusammensetzbar ist** Kontrollkästchen am Anfang der **Funktionsimporte** Dialogfeld können Sie die zusammensetzbare Funktionen zuordnen. Wenn Sie dieses Kontrollkästchen deaktivieren, erscheinen nur zusammensetzbare Funktionen (Table-valued Function) in der **gespeicherte Prozedur / Funktionsnamen** Dropdown-Liste. Wenn dieses Kontrollkästchen nicht aktiviert, werden nur für nicht zusammensetzbare Funktionen in der Liste angezeigt werden.*

## <a name="use-the-model"></a>Verwenden Sie das Modell

Öffnen der **"Program.cs"** -Datei, in der **Main** Methode definiert ist. Fügen Sie der Main-Funktion mit den folgenden Code.

Der Code ruft zwei gespeicherte Prozeduren: **GetStudentGrades** (gibt **StudentGrades** für den angegebenen *"StudentID"*) und **GetDepartmentName** (der Name der Abteilung im Ausgabeparameter zurückgegeben).  

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

Kompilieren Sie die Anwendung, und führen Sie sie aus. Das Programm erzeugt die folgende Ausgabe:

```
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a>Ausgabeparameter
-----------------

Wenn Output-Parameter verwendet werden, werden ihre Werte nicht verfügbar, bis die Ergebnisse vollständig gelesen wurden. Dies liegt an der zugrunde liegende Verhalten von DbDataReader, finden Sie unter [Abrufen von Daten mit einem DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) Weitere Details.
