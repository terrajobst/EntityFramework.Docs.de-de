---
title: Designer-TPH-Vererbung-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 72d26a8e-20ab-4500-bd13-394a08e73394
ms.openlocfilehash: 43ba34a98c3960a7a3052a00e2ed2751c2f2b121
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415232"
---
# <a name="designer-tph-inheritance"></a>Designer-TPH-Vererbung
Diese schrittweise exemplarische Vorgehensweise veranschaulicht, wie Sie die "Tabelle pro Hierarchie"-Vererbung (TPH) im konzeptionellen Modell mit dem Entity Framework Designer (EF-Designer) implementieren. Die TPH-Vererbung verwendet eine Datenbanktabelle, um Daten für alle Entitäts Typen in einer Vererbungs Hierarchie zu verwalten.

In dieser exemplarischen Vorgehensweise ordnen wir die Person-Tabelle drei Entitäts Typen zu: Person (der Basistyp), Student (abgeleitet von Person) und Instructor (abgeleitet von Person). Wir erstellen ein konzeptionelles Modell aus der Datenbank (Database First) und ändern dann das Modell, um die TPH-Vererbung mit dem EF-Designer zu implementieren.

Es ist möglich, eine TPH-Vererbung mithilfe Model First zuzuordnen, aber Sie müssen einen komplexen Daten Bank Generierungs Workflow schreiben, der Komplex ist. Anschließend weisen Sie diesen Workflow der Eigenschaft **Daten Bank Generierungs Workflow** im EF-Designer zu. Eine einfachere Alternative ist die Verwendung von Code First.

## <a name="other-inheritance-options"></a>Andere Vererbungs Optionen

Tabelle pro Typ (TPT) ist eine andere Art von Vererbung, bei der separate Tabellen in der Datenbank Entitäten zugeordnet werden, die an der Vererbung teilnehmen.  Informationen zum Zuordnen der "Tabelle pro Typ"-Vererbung mit dem EF-Designer finden Sie unter [EF-Designer-TPT-Vererbung](~/ef6/modeling/designer/inheritance/tpt.md).

"Tabelle pro konkrete Typvererbung" (TPC) und gemischte Vererbungs Modelle werden von der Entity Framework Runtime unterstützt, jedoch nicht vom EF-Designer unterstützt. Wenn Sie TPC oder gemischte Vererbung verwenden möchten, haben Sie zwei Möglichkeiten: Verwenden Sie Code First, oder bearbeiten Sie die EDMX-Datei manuell. Wenn Sie sich für die Arbeit mit der EDMX-Datei entscheiden, wird das Fenster Mappingdetails in den "abgesicherten Modus" versetzt, und Sie können den Designer nicht verwenden, um die Zuordnungen zu ändern.

## <a name="prerequisites"></a>Voraussetzungen

Um die exemplarische Vorgehensweise nachzuvollziehen, benötigen Sie Folgendes:

- Eine aktuelle Version von Visual Studio.
- Die [Beispieldatenbank "School](~/ef6/resources/school-database.md)".

## <a name="set-up-the-project"></a>Einrichten des Projekts

-   Öffnen Sie Visual Studio 2012.
-   **Datei&gt; neues&gt; Projekt** auswählen
-   Klicken Sie im linken Bereich auf **Visual C-\#** , und wählen Sie dann die **Konsolen** Vorlage aus.
-   Geben Sie **tphdbfirstsample** als Namen ein.
-   Wählen Sie **OK** aus.

## <a name="create-a-model"></a>Modellerstellung

-   Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, und wählen Sie **Hinzufügen-&gt; neues Element**aus.
-   Wählen Sie im linken Menü **Daten** aus, und wählen Sie dann im Bereich Vorlagen die Option **ADO.NET Entity Data Model** aus.
-   Geben Sie als Dateiname **tphmodel. edmx** ein, und klicken Sie dann auf **Hinzufügen**.
-   Wählen Sie im Dialogfeld Modell Inhalte auswählen die Option **aus Datenbank generieren aus**, und klicken Sie dann auf **weiter**.
-   Klicken Sie auf **neue Verbindung**.
    Geben Sie im Dialogfeld Verbindungs Eigenschaften den Servernamen ein (z. b. **(localdb)\\mssqllocaldb**), wählen Sie die Authentifizierungsmethode aus, geben Sie **School** als Datenbanknamen ein, und klicken Sie dann auf **OK**.
    Das Dialogfeld Wählen Sie Ihre Datenverbindung aus wird mit Ihrer Daten bankverbindungs Einstellung aktualisiert.
-   Wählen Sie im Dialogfeld Datenbankobjekte auswählen unter dem Knoten Tabellen die **Person** -Tabelle aus.
-   Klicken Sie auf **Finish**.

Die Entity Designer, die eine Entwurfs Oberfläche zum Bearbeiten des Modells bereitstellt, wird angezeigt. Alle Objekte, die Sie im Dialogfeld Wählen Sie Ihre Datenbankobjekte ausgewählt haben, werden dem Modell hinzugefügt.

So sucht die **Person** -Tabelle in der Datenbank.

![Person-Tabelle](~/ef6/media/persontable.png) 

## <a name="implement-table-per-hierarchy-inheritance"></a>Implementieren der "Tabelle pro Hierarchie"-Vererbung

Die **Person** -Tabelle verfügt über die **diskriminatorspalte** , die einen von zwei Werten aufweisen kann: "Student" und "Instructor". Abhängig vom Wert wird die **Person** -Tabelle der **Student** -Entität oder der **Instructor** -Entität zugeordnet. Die **Person** -Tabelle verfügt auch über zwei Spalten, **HireDate** und **registrimentdate**, die **NULL-Werte** zulassen müssen, da eine Person nicht gleichzeitig Student und Dozenten sein kann (zumindest nicht in dieser exemplarischen Vorgehensweise).

### <a name="add-new-entities"></a>Neue Entitäten hinzufügen

-   Fügen Sie eine neue Entität hinzu.
    Klicken Sie dazu mit der rechten Maustaste auf einen leeren Bereich der Entwurfs Oberfläche der Entity Framework Designer, und wählen Sie **Add-&gt;Entity**aus.
-   Geben Sie **Instructor** als **Entitäts Name** ein, und wählen Sie in der Dropdown Liste für den **Basistyp**die Option  **Person** aus.
-   Klicken Sie auf **OK**.
-   Fügen Sie eine neue Entität hinzu. Geben Sie **Student** als **Entitäts Name** ein, und wählen Sie in der Dropdown Liste für den **Basistyp**die Option  **Person** aus.

Der Entwurfs Oberfläche wurden zwei neue Entitäts Typen hinzugefügt. Ein Pfeil zeigt von den neuen Entitäts Typen auf den Entitätstyp **Person** ; Dies gibt an, dass die **Person** der Basistyp für die neuen Entitäts Typen ist.

-   Klicken Sie mit der rechten Maustaste auf das **HireDate** -Eigenschaft der **Person** Entität. Wählen Sie **Ausschneiden** (oder drücken Sie STRG + X).
-   Klicken Sie mit der rechten Maustaste auf den **Dozenten** Entität, und wählen Sie **Einfügen** (oder drücken Sie STRG + V).
-   Klicken Sie mit der rechten Maustaste auf die Eigenschaft **HireDate** und wählen Sie **Eigenschaften**aus.
-   Legen Sie im Fenster **Eigenschaften** die Eigenschaft , die **NULL-Werte** zulässt, auf **false**fest.
-   Klicken Sie mit der rechten Maustaste auf die Eigenschaft **registrimentdate** der Entität **Person** . Wählen Sie **Ausschneiden** (oder drücken Sie STRG + X).
-   Klicken Sie mit der rechten Maustaste auf die Entität **Student** , und wählen Sie **Einfügen aus (oder drücken Sie STRG + V).**
-   Wählen Sie die Eigenschaft **registrimentdate** aus, und legen Sie für die Eigenschaft **Nullable** den Wert **false**fest.
-   Wählen Sie die **Person** Entitätstyp aus. Legen Sie im Fenster **Eigenschaften** die Eigenschaft **abstract** auf **true**fest.
-   Löschen Sie die **diskriminatoreigenschaft** von **Person**. Der Grund für das Löschen sollte im folgenden Abschnitt erläutert werden.

### <a name="map-the-entities"></a>Zuordnen der Entitäten

-   Klicken Sie mit der rechten Maustaste auf den **Dozenten** und wählen Sie **Tabellen Zuordnung.**
    Die Entität "Instructor" ist im Fenster "Mappingdetails" ausgewählt.
-   Klicken Sie im Fenster **Mappingdetails** auf **&lt;Tabelle oder Sicht&gt; hinzufügen** .
    Das **&lt;eine Tabelle oder Sicht hinzufügen&gt;**  Feld wird zu einer Dropdown Liste mit Tabellen oder Sichten, denen die ausgewählte Entität zugeordnet werden kann.
-   Wählen Sie in der Dropdown Liste **Person** aus.
-   Das Fenster **Mappingdetails** wird mit Standard Spalten Zuordnungen und einer Option zum Hinzufügen einer Bedingung aktualisiert.
-   Klicken Sie **&lt;Hinzufügen einer Bedingung&gt;** .
    Das **&lt;eine Bedingung&gt; Feld hinzufügen** wird zu einer Dropdown Liste von Spalten, für die Bedingungen festgelegt werden können.
-   Wählen Sie in der Dropdown Liste **Diskriminator ** aus.
-   Wählen Sie in der Spalte **Operator** des Fensters **Mappingdetails** in der Dropdown Liste die Option = aus.
-   Geben Sie in der Spalte **Wert/Eigenschaft den Wert**  **Instructor**ein. Das Endergebnis sollte wie folgt aussehen:

    ![Mappingdetails](~/ef6/media/mappingdetails2.png)

-   Wiederholen Sie diese Schritte für den Entitätstyp **Student** , aber legen Sie die Bedingung auf den Wert **Student** fest.  
    *Der Grund, warum wir die **diskriminatoreigenschaft** entfernen wollten, liegt darin, dass Sie eine Tabellenspalte nicht mehrmals zuordnen können. Diese Spalte wird für die bedingte Zuordnung verwendet und kann daher nicht auch für die Eigenschaften Zuordnung verwendet werden. Die einzige Möglichkeit, wie Sie für beides verwendet werden kann, wenn eine Bedingung einen **is NULL** - oder **nicht NULL** - Vergleich verwendet.*

Die "Tabelle pro Hierarchie"-Vererbung ist jetzt implementiert.

![Endgültige TPH](~/ef6/media/finaltph.png)

## <a name="use-the-model"></a>Verwenden des Modells

Öffnen Sie die Datei **Program.cs** , in der die **Main** -Methode definiert ist. Fügen Sie den folgenden Code in die **Main** -Funktion ein. Der Code führt drei Abfragen aus. Die erste Abfrage führt alle **Person** -Objekte zurück. Die zweite Abfrage verwendet die **OfType** -Methode, um **Instructor** -Objekte zurückzugeben. Die dritte Abfrage verwendet die **OfType** -Methode, um **Student** -Objekte zurückzugeben.

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
