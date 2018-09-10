---
title: Designer TPH-Vererbung – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 1eb935414b20d6e93e9d470ccc845bc13626ed3a
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250841"
---
# <a name="designer-tph-inheritance"></a>Designer TPH-Vererbung
Diese exemplarische Vorgehensweise veranschaulicht Tabelle pro Hierarchie (TPH)-Vererbung im konzeptionellen Modell mit dem Entity Framework Designer (EF-Designer) zu implementieren. TPH-Vererbung verwendet eine Datenbanktabelle, um die Daten aller Entitätstypen in einer Vererbungshierarchie zu verwalten.

In dieser exemplarischen Vorgehensweise ordnen wir die Person-Tabelle auf drei Entitätstypen: Person (Basistyp), (abgeleitet von Person) "Student" und "Instructor" (von Person abgeleitet ist). Wir ein konzeptionelles Modell aus der Datenbank (Database First) erstellen und ändern Sie dann das Modell zur Implementierung der TPH-Vererbung mit dem EF Designer.

Es ist möglich, eine TPH-Vererbung mit Model First zuzuordnen, jedoch müssen Sie Ihren eigenen Workflow zur datenbankgenerierung schreiben, der komplex ist. Sie würden diesen Workflow Zuweisen der **Workflow zur Datenbankgenerierung** Eigenschaft im EF Designer. Eine einfachere Alternative ist mit Code First.

## <a name="other-inheritance-options"></a>Andere Vererbungsoptionen

Tabelle pro Typ (TPT) ist eine andere Art von Vererbung in der separate Tabellen in der Datenbank Entitäten zugeordnet werden, die in der Vererbung beteiligt sind.  Informationen dazu, wie Sie eine Tabelle pro Typ Vererbung mit dem EF Designer zuordnen, finden Sie unter [EF Designer TPT-Vererbung](~/ef6/modeling/designer/inheritance/tpt.md).

Tabelle pro konkretem Typ '-Vererbung (TPC) und gemischte vererbungsmodellen werden von der Entity Framework-Laufzeit unterstützt, jedoch werden von dem EF Designer nicht unterstützt. Wenn Sie die TPC oder gemischte Vererbung verwenden möchten, haben Sie zwei Optionen: Code First verwenden, oder die EDMX-Datei manuell bearbeiten. Wenn Sie mit der EDMX-Datei arbeiten möchten, Detailfenster Zuordnung wird in "abgesicherten Modus" versetzt werden, und Sie werden nicht in der Lage, den Designer zu verwenden, um die Zuordnungen ändern.

## <a name="prerequisites"></a>Erforderliche Komponenten

Um die exemplarische Vorgehensweise nachzuvollziehen, benötigen Sie Folgendes:

- Eine aktuelle Version von Visual Studio.
- Die [Beispieldatenbank ' School '](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Einrichten des Projekts

-   Öffnen Sie Visual Studio 2012.
-   Wählen Sie **Dateien&gt; neu –&gt; Projekt**
-   Klicken Sie im linken Bereich auf **Visual C\#**, und wählen Sie dann die **Konsole** Vorlage.
-   Geben Sie **TPHDBFirstSample** als Namen.
-   Klicken Sie auf **OK**.

## <a name="create-a-model"></a>Erstellen eines Modells

-   Mit der rechten Maustaste des Projektnamen im Projektmappen-Explorer, und wählen **hinzufügen –&gt; neues Element**.
-   Wählen Sie **Daten** aus dem linken Menü und wählen Sie dann **ADO.NET Entity Data Model** im Bereich Vorlagen.
-   Geben Sie **TPHModel.edmx** für den Dateinamen ein, und klicken Sie dann auf **hinzufügen**.
-   Wählen Sie im Dialogfeld "Modellinhalte" **aus Datenbank generieren**, und klicken Sie dann auf **Weiter**.
-   Klicken Sie auf **neue Verbindung**.
    Geben Sie den Servernamen, klicken Sie im Dialogfeld "Verbindungseigenschaften" (z. B. **(Localdb)\\Mssqllocaldb**), wählen die Authentifizierungsmethode, Typ **School** für den Datenbanknamen, und klicken Sie dann Klicken Sie auf **OK**.
    Das Dialogfeld "Wählen Sie Ihre Datenverbindung" wird mit Ihrem datenbankverbindungseinstellung aktualisiert.
-   Wählen Sie in das Dialogfeld Datenbankobjekte auswählen, unter dem Knoten Tabellen der **Person** Tabelle.
-   Klicken Sie auf **Fertig stellen**.

Im Entity Designer, der bietet eine Entwurfsoberfläche zum Bearbeiten des Modells, wird angezeigt. Alle Objekte, die Sie im Dialogfeld "Datenbankobjekte auswählen" ausgewählt werden mit dem Modell hinzugefügt.

D. h. wie die **Person** Tabelle sieht einwandfrei aus, in der Datenbank.

![Person-Tabelle](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a>Implementieren Sie die Tabelle pro Hierarchie-Vererbung

Die **Person** Tabelle besitzt die **Diskriminator** Spalte, die einen von zwei Werten haben kann: "Student" und "Instructor". Abhängig vom Wert der **Person** Tabelle zugeordnet der **für Schüler und Studenten** Entität oder die **"Instructor"** Entität. Die **Person** Tabelle besitzt außerdem zwei Spalten, **HireDate** und **EnrollmentDate**, muss **auf NULL festlegbare** , da eine Person sein, kann kein "Student" und einen Dozenten zur gleichen Zeit (zumindest nicht in dieser exemplarischen Vorgehensweise).

### <a name="add-new-entities"></a>Fügen Sie neue Entitäten hinzu

-   Fügen Sie eine neue Entität hinzu.
    Zu diesem Zweck mit der rechten Maustaste auf einen leeren Bereich der Entwurfsoberfläche des Entity Framework Designer, und wählen **Add-&gt;Entität**.
-   Typ **"Instructor"** für die **Entitätsname** , und wählen Sie **Person** aus der Dropdown-Liste für die **Basistyp**.
-   Klicken Sie auf **OK**.
-   Fügen Sie eine andere neue Entität hinzu. Typ **für Schüler und Studenten** für die **Entitätsname** , und wählen Sie **Person** aus der Dropdown-Liste für die **Basistyp**.

Die Entwurfsoberfläche wurden zwei neue Entitätstypen hinzugefügt. Ein Pfeil zeigt in der neuen Entitätstypen auf den **Person** Entitätstyp; Dies bedeutet, dass **Person** ist der Basistyp für den neuen Entitätstypen.

-   Mit der rechten Maustaste die **HireDate** Eigenschaft der **Person** Entität. Wählen Sie **Ausschneiden** (oder verwenden Sie die Taste STRG + X).
-   Mit der rechten Maustaste die **"Instructor"** Entität, und wählen **einfügen** (oder verwenden Sie die Taste STRG + V).
-   Mit der rechten Maustaste die **HireDate** Eigenschaft, und wählen **Eigenschaften**.
-   In der **Eigenschaften** legen die **Nullable** Eigenschaft **"false"**.
-   Mit der rechten Maustaste die **EnrollmentDate** Eigenschaft der **Person** Entität. Wählen Sie **Ausschneiden** (oder verwenden Sie die Taste STRG + X).
-   Mit der rechten Maustaste die **für Schüler und Studenten** Entität, und wählen **einfügen (oder Schlüssel verwenden Sie die STRG + V).**
-   Wählen Sie die **EnrollmentDate** Eigenschaft, und legen die **Nullable** Eigenschaft **"false"**.
-   Wählen Sie die **Person** Entitätstyp. In der **Eigenschaften** legen die **abstrakte** Eigenschaft **"true"**.
-   Löschen der **Diskriminator** Eigenschaft **Person**. Der Grund, die, den es gelöscht werden soll, wird im folgenden Abschnitt erläutert.

### <a name="map-the-entities"></a>Ordnen Sie die Entitäten

-   Mit der rechten Maustaste die **"Instructor"** , und wählen Sie **Tabellenzuordnung.**
    Die Instructor-Entität, die in das Fenster "Mappingdetails" ausgewählt ist.
-   Klicken Sie auf **&lt;Hinzufügen einer Tabelle oder Sicht&gt;** in die **Mappingdetails** Fenster.
    Die **&lt;Hinzufügen einer Tabelle oder Sicht&gt;** Feld wird eine Dropdown-Liste von Tabellen oder Sichten, die die ausgewählte Entität zugeordnet werden können.
-   Wählen Sie **Person** aus der Dropdown-Liste.
-   Die **Mappingdetails** Fenster wird mit Standardspaltenmappings und einer Option zum Hinzufügen einer Bedingung aktualisiert.
-   Klicken Sie auf  **&lt;Hinzufügen einer Bedingung&gt;**.
    Die **&lt;Hinzufügen einer Bedingung&gt;** Feld wird eine Dropdown-Liste der Spalten, die für die Bedingungen festgelegt werden können.
-   Wählen Sie **Diskriminator** aus der Dropdown-Liste.
-   In der **Operator** Spalte die **Mappingdetails** wählen Sie im Fenster = aus der Dropdown-Liste.
-   In der **Wert/Eigenschaft** Spalte, Datentyp **"Instructor"**. Das Endergebnis sollte wie folgt aussehen:

    ![Zuordnungsdetails](~/ef6/media/mappingdetails2.png)

-   Wiederholen Sie diese Schritte für die **für Schüler und Studenten** Entitätstyp, aber stellen die Bedingung gleich **für Schüler und Studenten** Wert.  
    *Der Grund, wollten wir entfernen, das **Diskriminator** -Eigenschaft ist, da Sie eine Spalte mehrmals zugeordnet werden können. Diese Spalte wird für die bedingten Zuordnung verwendet werden, daher kann nicht für die auch eigenschaftenzuordnung verwendet werden. Die einzige Möglichkeit, es kann für beide verwendet werden, wenn eine Bedingung verwendet eine **Is Null** oder **Is Not Null** Vergleich.*

Die "Tabelle pro Hierarchie"-Vererbung ist jetzt implementiert.

![Letzte TPH](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a>Verwenden Sie das Modell

Öffnen der **"Program.cs"** -Datei, in der **Main** Methode definiert ist. Fügen Sie den folgenden Code in die **Main** Funktion. Der Code ausgeführt wird, drei Abfragen. Die erste Abfrage gibt wieder alle **Person** Objekte. Die zweite Abfrage verwendet die **OfType** -Methode zur Rückgabe **"Instructor"** Objekte. Die dritte Abfrage verwendet die **OfType** -Methode zur Rückgabe **für Schüler und Studenten** Objekte.

``` csharp
    using (var context = new SchoolEntities())
    {
        Console.WriteLine("All people:");
        foreach (var person in context.People)
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Instructors only: ");
        foreach (var person in context.People.OfType<Instructor>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }

        Console.WriteLine("Students only: ");
        foreach (var person in context.People.OfType<Student>())
        {
            Console.WriteLine("    {0} {1}", person.FirstName, person.LastName);
        }
    }
```
