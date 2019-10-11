---
title: Gespeicherte Prozeduren für Designer Abfragen EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9554ed25-c5c1-43be-acad-5da37739697f
ms.openlocfilehash: 2e0092b526278597e8477d47eeb642598647bb91
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182481"
---
# <a name="designer-query-stored-procedures"></a>Gespeicherte Prozeduren für Designer Abfragen
In dieser schrittweisen exemplarischen Vorgehensweise wird veranschaulicht, wie Sie mit dem Entity Framework Designer (EF-Designer) gespeicherte Prozeduren in ein Modell importieren und dann die importierten gespeicherten Prozeduren aufrufen, um Ergebnisse abzurufen. 

Beachten Sie, dass Code First keine Zuordnung zu gespeicherten Prozeduren oder Funktionen unterstützt. Allerdings können Sie gespeicherte Prozeduren oder Funktionen mithilfe der System. Data. Entity. dbset. sqlQuery-Methode abrufen. Zum Beispiel:
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]")`;
```

## <a name="prerequisites"></a>Erforderliche Komponenten

Um die exemplarische Vorgehensweise nachzuvollziehen, benötigen Sie Folgendes:

- Eine aktuelle Version von Visual Studio.
- Die [Beispieldatenbank "School](~/ef6/resources/school-database.md)".

## <a name="set-up-the-project"></a>Einrichten des Projekts

-   Öffnen Sie Visual Studio 2012.
-   **Datei &gt; New-&gt;-Projekt** auswählen
-   Klicken Sie im linken Bereich auf **Visual C @ no__t-1**, und wählen Sie dann die **Konsolen** Vorlage aus.
-   Geben Sie **efwithsprocssample** Als Name ein.
-   Wählen Sie **OK**aus.

## <a name="create-a-model"></a>Erstellen eines Modells

-   Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **Add-&gt; New Item**aus.
-   Wählen Sie im linken Menü **Daten** aus, und wählen Sie dann im Bereich Vorlagen die Option **ADO.NET Entity Data Model** aus.
-   Geben Sie als Dateiname **efwithsprocsmodel. edmx** ein, und klicken Sie dann auf **Hinzufügen**.
-   Wählen Sie im Dialogfeld Modell Inhalte auswählen die Option **aus Datenbank generieren aus**, und klicken Sie dann auf **weiter**.
-   Klicken Sie auf **neue Verbindung**.  
    Geben Sie im Dialogfeld Verbindungs Eigenschaften den Servernamen ein (z. b. **(localdb) \\mssqllocaldb**), wählen Sie die Authentifizierungsmethode aus, geben Sie **School** Als Datenbankname ein, und klicken Sie dann auf **OK**.  
    Das Dialogfeld Wählen Sie Ihre Datenverbindung aus wird mit Ihrer Daten bankverbindungs Einstellung aktualisiert.
-   Aktivieren Sie im Dialogfeld Wählen Sie Ihre Datenbankobjekte aus das Kontrollkästchen **Tabellen** , um alle Tabellen auszuwählen.  
    Wählen Sie außerdem im Knoten **gespeicherte Prozeduren und Funktionen** die folgenden gespeicherten Prozeduren aus: **GetStudentGrades** und **GetDepartmentName**. 

    ![Importieren](~/ef6/media/import.jpg)

    *beginnend mit Visual Studio 2012 unterstützt der EF-Designer den Massen Import gespeicherter Prozeduren. Die Option zum **importieren ausgewählter gespeicherter Prozeduren und Funktionen in das Entitäts Modell** ist standardmäßig aktiviert.*
-   Klicken Sie auf **Finish**.

Standardmäßig wird die Ergebnis Form jeder importierten gespeicherten Prozedur oder Funktion, die mehr als eine Spalte zurückgibt, automatisch zu einem neuen komplexen Typ. In diesem Beispiel möchten wir die Ergebnisse der Funktion " **GetStudentGrades** " der Entität " **StudentGrade** " und die Ergebnisse von " **GetDepartmentName** " zu " **None** " zuordnen ("**None** " ist der Standardwert).

Damit ein Funktions Import einen Entitätstyp zurückgibt, müssen die von der entsprechenden gespeicherten Prozedur zurückgegebenen Spalten genau mit den skalaren Eigenschaften des zurückgegebenen Entitäts Typs übereinstimmen. Ein Funktions Import kann auch Auflistungen von einfachen Typen, komplexen Typen oder keinen Wert zurückgeben.

-   Klicken Sie mit der rechten Maustaste auf die Entwurfs Oberfläche, und wählen Sie **Modell Browser**
-   Wählen Sie im **Modell Browser** **Funktions Importe**aus, und doppelklicken Sie dann auf die Funktion **GetStudentGrades** .
-   Wählen Sie im Dialogfeld Funktions Import bearbeiten die Option **Entitäten** Aus, und wählen Sie dann **StudentGrade**aus.  
    *Das Kontrollkästchen **Funktions Import ist zusammensetzbar** am oberen Rand des Dialog Felds **Funktions Importe** ermöglicht Ihnen die Zuordnung zu Zusammensetz baren Funktionen. Wenn Sie dieses Kontrollkästchen aktivieren, werden nur Zusammensetz Bare Funktionen (Tabellenwert Funktionen) in der Dropdown Liste **Name der gespeicherten Prozedur/Funktion** angezeigt. Wenn Sie dieses Kontrollkästchen nicht aktivieren, werden nur nicht Zusammensetz Bare Funktionen in der Liste angezeigt.*

## <a name="use-the-model"></a>Verwenden des Modells

Öffnen Sie die Datei **Program.cs** , in der die **Main** -Methode definiert ist. Fügen Sie der Main-Funktion den folgenden Code hinzu.

Der Code ruft zwei gespeicherte Prozeduren auf: **GetStudentGrades** (gibt **studentgrades** für die angegebene *Studentin TID*zurück) und **GetDepartmentName** (gibt den Namen der Abteilung im Output-Parameter zurück).  

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

```console
StudentID: 2
Student grade: 4.00
StudentID: 2
Student grade: 3.50
The department name is Engineering
```

<a name="output-parameters"></a>Ausgabeparameter
-----------------

Wenn Output-Parameter verwendet werden, sind ihre Werte erst verfügbar, wenn die Ergebnisse vollständig gelesen wurden. Dies ist auf das zugrunde liegende Verhalten von DbDataReader zurückzuführen. Weitere Informationen finden [Sie unter Abrufen von Daten mithilfe eines DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) .
