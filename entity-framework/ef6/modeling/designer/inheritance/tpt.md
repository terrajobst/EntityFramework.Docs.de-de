---
title: Designer TPT-Vererbung – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: efc78c31-b4ea-4ea3-a0cd-c69eb507020e
ms.openlocfilehash: 84330fba4807620aa242a70cd8ac76a60284416d
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489452"
---
# <a name="designer-tpt-inheritance"></a>Designer TPT-Vererbung
Schritt für Schritt dieser exemplarischen Vorgehensweise veranschaulicht implementieren Sie eine Tabelle pro Typ (TPT)-Vererbung in Ihrem Modell, mit dem Entity Framework Designer (EF-Designer). Bei der "Tabelle pro Typ"-Vererbung wird eine separate Tabelle in der Datenbank verwendet, um die Daten für nicht geerbte Eigenschaften und Schlüsseleigenschaften für jeden Typ in der Vererbungshierarchie zu verwalten.

In dieser exemplarischen Vorgehensweise ordnen wir den **Kurs** (Basistyp), **OnlineCourse** (abgeleitet aus dem Kurs wird), und **"OnsiteCourse"** (leitet sich von **Kurs**) Entitäten, die Tabellen mit dem gleichen Namen. Wir erstellen ein Modell aus der Datenbank und ändern Sie dann das Modell, um die TPT-Vererbung zu implementieren.

Sie können auch mit den Model First beginnen und generieren Sie dann die Datenbank aus dem Modell. Dem EF Designer verwendet die TPT-Strategie in der Standardeinstellung, und daher Vererbung in das Modell wird separaten Tabellen zugeordnet werden.

## <a name="other-inheritance-options"></a>Andere Vererbungsoptionen

Tabelle pro Hierarchie (TPH) ist eine andere Art von Vererbung in der eine Tabelle verwendet wird, um die Daten aller Entitätstypen in einer Vererbungshierarchie zu verwalten.  Informationen dazu, wie Sie eine Tabelle pro Hierarchie-Vererbung mit dem Entity Designer zuordnen, finden Sie unter [EF Designer TPH-Vererbung](~/ef6/modeling/designer/inheritance/tph.md). 

Beachten Sie, dass die Tabelle pro konkreter geben, Vererbung (TPC) und gemischte Vererbung Modelle werden von der Entity Framework-Laufzeit unterstützt jedoch nicht von dem EF Designer unterstützt werden. Wenn Sie die TPC oder gemischte Vererbung verwenden möchten, haben Sie zwei Optionen: Code First verwenden, oder die EDMX-Datei manuell bearbeiten. Wenn Sie mit der EDMX-Datei arbeiten möchten, Detailfenster Zuordnung wird in "abgesicherten Modus" versetzt werden, und Sie werden nicht in der Lage, den Designer zu verwenden, um die Zuordnungen ändern.

## <a name="prerequisites"></a>Erforderliche Komponenten

Um die exemplarische Vorgehensweise nachzuvollziehen, benötigen Sie Folgendes:

- Eine aktuelle Version von Visual Studio.
- Die [Beispieldatenbank ' School '](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Einrichten des Projekts

-   Öffnen Sie Visual Studio 2012.
-   Wählen Sie **Dateien&gt; neu –&gt; Projekt**
-   Klicken Sie im linken Bereich auf **Visual C\#**, und wählen Sie dann die **Konsole** Vorlage.
-   Geben Sie **TPTDBFirstSample** als Namen.
-   Klicken Sie auf **OK**.

## <a name="create-a-model"></a>Erstellen eines Modells

-   Mit der rechten Maustaste in des Projekts im Projektmappen-Explorer, und wählen **hinzufügen –&gt; neues Element**.
-   Wählen Sie **Daten** aus dem linken Menü und wählen Sie dann **ADO.NET Entity Data Model** im Bereich Vorlagen.
-   Geben Sie **TPTModel.edmx** für den Dateinamen ein, und klicken Sie dann auf **hinzufügen**.
-   Im Dialogfeld "Modellinhalte auswählen" das Feld, wählen Sie ** generieren, aus der Datenbank **, und klicken Sie dann auf **Weiter**.
-   Klicken Sie auf **neue Verbindung**.
    Geben Sie den Servernamen, klicken Sie im Dialogfeld "Verbindungseigenschaften" (z. B. **(Localdb)\\Mssqllocaldb**), wählen die Authentifizierungsmethode, Typ **School** für den Datenbanknamen, und klicken Sie dann Klicken Sie auf **OK**.
    Das Dialogfeld "Wählen Sie Ihre Datenverbindung" wird mit Ihrem datenbankverbindungseinstellung aktualisiert.
-   Wählen Sie in das Dialogfeld Datenbankobjekte auswählen, unter dem Knoten Tabellen der **Abteilung**, **Kurs OnlineCourse und "OnsiteCourse"** Tabellen.
-   Klicken Sie auf **Fertig stellen**.

Im Entity Designer, der bietet eine Entwurfsoberfläche zum Bearbeiten des Modells, wird angezeigt. Alle Objekte, die Sie im Dialogfeld "Datenbankobjekte auswählen" ausgewählt werden mit dem Modell hinzugefügt.

## <a name="implement-table-per-type-inheritance"></a>Implementieren von Vererbung von Tabelle pro Typ

-   Klicken Sie auf der Entwurfsoberfläche mit der Maustaste der **OnlineCourse** Entitätstyp, und wählen Sie **Eigenschaften**.
-   In der **Eigenschaften** legen die Base-Type-Eigenschaft auf **Kurs**.
-   Mit der rechten Maustaste die **"OnsiteCourse"** Entitätstyp, und wählen Sie **Eigenschaften**.
-   In der **Eigenschaften** legen die Base-Type-Eigenschaft auf **Kurs**.
-   Mit der rechten Maustaste der Zuordnung (die Linie) zwischen den **OnlineCourse** und **Kurs** Entitätstypen.
    Wählen Sie **aus Modell löschen**.
-   Mit der rechten Maustaste in der Zuordnung zwischen der **"OnsiteCourse"** und **Kurs** Entitätstypen.
    Wählen Sie **aus Modell löschen**.

Wir werden nun gelöscht. die **CourseID** Eigenschaft **OnlineCourse** und **"OnsiteCourse"** , da diese Klassen erben **CourseID** aus die **Kurs** Basistyp.

-   Mit der rechten Maustaste die **CourseID** Eigenschaft der **OnlineCourse** Entitätstyps, und wählen Sie dann **aus Modell löschen**.
-   Mit der rechten Maustaste die **CourseID** Eigenschaft der **"OnsiteCourse"** Entitätstyps, und wählen Sie dann **aus Modell löschen**
-   Die "Tabelle pro Typ"-Vererbung ist damit implementiert.

![TPT](~/ef6/media/tpt.png)

## <a name="use-the-model"></a>Verwenden Sie das Modell

Öffnen der **"Program.cs"** -Datei, in der **Main** Methode definiert ist. Fügen Sie den folgenden Code in die **Main** Funktion. Der Code ausgeführt wird, drei Abfragen. Die erste Abfrage gibt wieder alle **Kurse** im Zusammenhang mit der angegebenen Abteilung. Die zweite Abfrage verwendet die **OfType** -Methode zur Rückgabe **OnlineCourses** im Zusammenhang mit der angegebenen Abteilung. Gibt zurück, die dritte Abfrage **OnsiteCourses**.

``` csharp
    using (var context = new SchoolEntities())
    {
        foreach (var department in context.Departments)
        {
            Console.WriteLine("The {0} department has the following courses:",
                               department.Name);

            Console.WriteLine("   All courses");
            foreach (var course in department.Courses )
            {
                Console.WriteLine("     {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnlineCourse>())
            {
                Console.WriteLine("   Online - {0}", course.Title);
            }

            foreach (var course in department.Courses.
                OfType<OnsiteCourse>())
            {
                Console.WriteLine("   Onsite - {0}", course.Title);
            }
        }
    }
```
