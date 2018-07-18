---
title: Designer Tabellenaufteilung - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
caps.latest.revision: 3
ms.openlocfilehash: 6ecf9a77f7c6a743e3902de65bb75349c6ef799f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2018
ms.locfileid: "39120823"
---
# <a name="designer-table-splitting"></a>Designer Tabellenaufteilung
In dieser exemplarischen Vorgehensweise wird gezeigt, wie mehrere Entitätstypen durch Ändern eines Modells mit dem Entity Framework Designer (EF-Designer) einer einzelnen Tabelle zugeordnet.

Ein Grund, die Sie möglicherweise tabellenaufteilung verwenden möchten, ist das Laden einiger Eigenschaften verzögern, Verwendung von lazy loading-um die Objekte zu laden. Sie können die Eigenschaften trennen, die möglicherweise große Menge von Daten in eine getrennte Entität enthalten und nur bei Bedarf laden.

Die folgende Abbildung zeigt die wichtigsten Windows, die bei der Arbeit mit dem EF Designer verwendet werden.

![EFDesigner](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>Erforderliche Komponenten

Um die exemplarische Vorgehensweise nachzuvollziehen, benötigen Sie Folgendes:

- Eine aktuelle Version von Visual Studio.
- Die [Beispieldatenbank ' School '](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Einrichten des Projekts

In dieser exemplarischen Vorgehensweise wird Visual Studio 2012 verwenden.

-   Öffnen Sie Visual Studio 2012.
-   Zeigen Sie im Menü **Datei** auf **Neu**, und klicken Sie dann auf **Projekt**.
-   Klicken Sie im linken Bereich auf Visual C\#, und wählen Sie dann die Vorlage für die Konsolenanwendung.
-   Geben Sie **TableSplittingSample** als Namen für das Projekt und auf **OK**.

## <a name="create-a-model-based-on-the-school-database"></a>Erstellen Sie ein Modell basierend auf der Datenbank "School"

-   Mit der rechten Maustaste des Projektnamen im Projektmappen-Explorer, zeigen Sie auf **hinzufügen**, und klicken Sie dann auf **neues Element**.
-   Wählen Sie **Daten** aus dem linken Menü und wählen Sie dann **ADO.NET Entity Data Model** im Bereich Vorlagen.
-   Geben Sie **TableSplittingModel.edmx** für den Dateinamen ein, und klicken Sie dann auf **hinzufügen**.
-   Wählen Sie im Dialogfeld "Modellinhalte" **aus Datenbank generieren**, und klicken Sie dann auf **weiter.**
-   Klicken Sie auf die neue Verbindung. Geben Sie den Servernamen, klicken Sie im Dialogfeld "Verbindungseigenschaften" (z. B. **(Localdb)\\Mssqllocaldb**), wählen die Authentifizierungsmethode, Typ **School** für den Datenbanknamen, und klicken Sie dann Klicken Sie auf **OK**.
    Das Dialogfeld "Wählen Sie Ihre Datenverbindung" wird mit Ihrem datenbankverbindungseinstellung aktualisiert.
-   Erweitern Sie in das Dialogfeld "Datenbankobjekte auswählen" die **Tabellen** Knoten und überprüfen Sie die **Person** Tabelle. Dadurch wird hinzugefügt, die angegebene Tabelle auf die **School** Modell.
-   Klicken Sie auf **Fertig stellen**.

Im Entity Designer, der bietet eine Entwurfsoberfläche zum Bearbeiten des Modells, wird angezeigt. Alle Objekte, die Sie ausgewählt haben, in der **Datenbankobjekte auswählen** im Dialogfeld zum Modell hinzugefügt werden.

## <a name="map-two-entities-to-a-single-table"></a>Zuordnen von zwei Entitäten zu einer einzelnen Tabelle

In diesem Abschnitt unterteilen Sie die **Person** Entität in beiden Entitäten und deren Zuordnung zu einer einzelnen Tabelle.

> [!NOTE]
> Die **Person** Entität enthält keine Eigenschaften, die möglicherweise große Datenmengen enthalten; es wird nur als Beispiel verwendet.

-   Mit der rechten Maustaste in eines leeren Bereichs der Entwurfsoberfläche, zeigen Sie auf **Add New**, und klicken Sie auf **Entität**.
    Die **neue Entität** Dialogfeld wird angezeigt.
-   Typ **HireInfo** für die **Entitätsname** und **PersonID** für die **Schlüsseleigenschaft** Name.
-   Klicken Sie auf **OK**.
-   Ein neuer Entitätstyp wird erstellt und auf der Entwurfsoberfläche angezeigt.
-   Wählen Sie die **HireDate** Eigenschaft der **Person** Entitätstyp, und drücken Sie **STRG + X** Schlüssel.
-   Wählen Sie die **HireInfo** Entität, und drücken Sie **STRG + V** Schlüssel.
-   Erstellen einer Assoziation zwischen **Person** und **HireInfo**. Zu diesem Zweck mit der rechten Maustaste in eines leeren Bereichs der Entwurfsoberfläche, zeigen Sie auf **Add New**, und klicken Sie auf **Zuordnung**.
-   Die **Zuordnung hinzufügen** Dialogfeld wird angezeigt. Die **PersonHireInfo** wird standardmäßig der Name angegeben wird.
-   Geben Sie die Multiplizität **1(One)** an beiden Enden der Beziehung.
-   Drücken Sie **OK**.

Der nächste Schritt ist erforderlich. die **Mappingdetails** Fenster. Wenn Sie dieses Fenster nicht sehen, mit der rechten Maustaste Entwurfsoberfläche, und wählen **Mappingdetails**.

-   Wählen Sie die **HireInfo** Entitätstyp, und klicken Sie auf **&lt;Hinzufügen einer Tabelle oder Sicht&gt;** in die **Mappingdetails** Fenster.
-   Wählen Sie **Person** aus der **&lt;Hinzufügen einer Tabelle oder Sicht&gt;** Feld Dropdown-Liste. Die Liste enthält die Tabellen oder Sichten, die die ausgewählte Entität zugeordnet werden können.
    Die entsprechenden Eigenschaften sollten standardmäßig zugeordnet werden.

    ![Zuordnung](~/ef6/media/mapping.png)

-   Wählen Sie die **PersonHireInfo** Zuordnung auf der Entwurfsoberfläche angezeigt.
-   Mit der rechten Maustaste in der Zuordnung auf die Entwurfsoberfläche, und wählen **Eigenschaften**.
-   In der **Eigenschaften** wählen Sie im Fenster der **referenziellen Einschränkungen** Eigenschaft, und klicken Sie auf die Schaltfläche mit den Auslassungspunkten.
-   Wählen Sie **Person** aus der **Principal** Dropdown-Liste.
-   Drücken Sie **OK**.

 

## <a name="use-the-model"></a>Verwenden Sie das Modell

-   Fügen Sie den folgenden Code in der Main-Methode.

``` csharp
    using (var context = new SchoolEntities())
    {
        Person person = new Person()
        {
            FirstName = "Kimberly",
            LastName = "Morgan",
            Discriminator = "Instructor",
        };

        person.HireInfo = new HireInfo()
        {
            HireDate = DateTime.Now
        };

        // Add the new person to the context.
        context.People.Add(person);

        // Insert a row into the Person table.  
        context.SaveChanges();

        // Execute a query against the Person table.
        // The query returns columns that map to the Person entity.
        var existingPerson = context.People.FirstOrDefault();

        // Execute a query against the Person table.
        // The query returns columns that map to the Instructor entity.
        var hireInfo = existingPerson.HireInfo;

        Console.WriteLine("{0} was hired on {1}",
            existingPerson.LastName, hireInfo.HireDate);
    }
```
-   Kompilieren Sie die Anwendung, und führen Sie sie aus.

Auf die folgenden T-SQL-Anweisungen ausgeführt wurden die **School** Datenbank aufgrund der Ausführung dieser Anwendungsprogramms. 

-   Die folgenden **einfügen** wurde als Ergebnis der Ausführung von Kontext ausgeführt. SaveChanges() und kombiniert Daten aus der **Person** und **HireInfo** Entitäten

    ![Insert](~/ef6/media/insert.png)

-   Die folgenden **wählen** wurde als Ergebnis der Ausführung von Kontext ausgeführt. People.FirstOrDefault() und wählt nur die Spalten zugeordnet **Person**

    ![Select1](~/ef6/media/select1.png)

-   Die folgenden **wählen** ausgeführt wurde, da der Zugriff auf die Navigation Eigenschaft existingPerson.Instructor und wählt nur die Spalten zugeordnet **HireInfo**

    ![Select2](~/ef6/media/select2.png)
