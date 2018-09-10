---
title: Designer CUD gespeicherte Prozeduren – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: 36c9b97b77fec30136cba1d850a0259c689e69ae
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250919"
---
# <a name="designer-cud-stored-procedures"></a>Designer CRUD-gespeicherte Prozeduren
Dieser schrittweise erläuterten exemplarischen zeigen, wie Sie ordnen den erstellen\\einfügen, aktualisieren und Löschvorgänge eines Entitätstyps zu gespeicherten Prozeduren, die mit dem Entity Framework Designer (EF-Designer) (CRUD).  Standardmäßig generiert Entity Framework automatisch die SQL-Anweisungen für die CRUD-Vorgänge, aber Sie können auch gespeicherte Prozeduren zuordnen, um diese Vorgänge.  

Beachten Sie, dass Code First-Zuordnung zu gespeicherten Prozeduren oder Funktionen nicht unterstützt. Allerdings können Sie gespeicherte Prozeduren oder Funktionen mithilfe der System.Data.Entity.DbSet.SqlQuery-Methode aufrufen. Zum Beispiel:
``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a>Überlegungen bei der Zuordnung die CUD-Vorgänge für gespeicherte Prozeduren

Beim Zuordnen von CRUD-Vorgänge zu gespeicherten Prozeduren gelten die folgenden Überlegungen: 

-   Wenn Sie eines der CRUD-Operationen zu einer gespeicherten Prozedur zuordnen, ordnen Sie alle. Wenn Sie nicht alle drei zuordnen, die nicht zugeordneten Vorgänge fehl, wenn ausgeführt, und eine **"UpdateException"** ausgelöst.
-   Sie müssen jeden Parameter der gespeicherten Prozedur, an Entitätseigenschaften zuordnen.
-   Wenn der Server den Primärschlüsselwert für die eingefügte Zeile generiert, müssen Sie diesen Wert an die Schlüsseleigenschaft der Entität zuordnen. In der folgenden Beispiel die **InsertPerson** gespeicherte Prozedur gibt die neu erstellten Primärschlüssel als Teil des Resultsets für die gespeicherte Prozedur zurück. Der Entitätsschlüssel der primäre Schlüssel zugeordnet ist (**PersonID**) mithilfe der **&lt;Result Binding hinzufügen&gt;** Feature von EF Designer.
-   Aufrufe der gespeicherten Prozedur werden die zugeordneten 1:1 mit den Entitäten im konzeptionellen Modell. Z. B. Wenn Sie eine Vererbungshierarchie in Ihrem konzeptionellen Modell und klicken Sie dann Zuordnung implementieren die CUD-Prozeduren für die **übergeordneten** (Basis-) und die **untergeordneten** (abgeleitete) Entitäten, die beim Speichern der **Untergeordneten** Änderungen ruft nur die **untergeordneten**die gespeicherten Prozeduren, nicht löst die **übergeordneten**die gespeicherten Prozeduren aufrufen.

## <a name="prerequisites"></a>Erforderliche Komponenten

Um die exemplarische Vorgehensweise nachzuvollziehen, benötigen Sie Folgendes:

- Eine aktuelle Version von Visual Studio.
- Die [Beispieldatenbank ' School '](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Einrichten des Projekts

-   Öffnen Sie Visual Studio 2012.
-   Wählen Sie **Dateien&gt; neu –&gt; Projekt**
-   Klicken Sie im linken Bereich auf **Visual C\#**, und wählen Sie dann die **Konsole** Vorlage.
-   Geben Sie **CUDSProcsSample** als Namen.
-   Klicken Sie auf **OK**.

## <a name="create-a-model"></a>Erstellen eines Modells

-   Mit der rechten Maustaste des Projektnamen im Projektmappen-Explorer, und wählen **hinzufügen –&gt; neues Element**.
-   Wählen Sie **Daten** aus dem linken Menü und wählen Sie dann **ADO.NET Entity Data Model** im Bereich Vorlagen.
-   Geben Sie **CUDSProcs.edmx** für den Dateinamen ein, und klicken Sie dann auf **hinzufügen**.
-   Wählen Sie im Dialogfeld "Modellinhalte" **aus Datenbank generieren**, und klicken Sie dann auf **Weiter**.
-   Klicken Sie auf **neue Verbindung**. Geben Sie den Servernamen, klicken Sie im Dialogfeld "Verbindungseigenschaften" (z. B. **(Localdb)\\Mssqllocaldb**), wählen die Authentifizierungsmethode, Typ **School** für den Datenbanknamen, und klicken Sie dann Klicken Sie auf **OK**.
    Das Dialogfeld "Wählen Sie Ihre Datenverbindung" wird mit Ihrem datenbankverbindungseinstellung aktualisiert.
-   Auswählen Ihrer Datenbankobjekte Dialogfeld unter die **Tabellen** Knoten die **Person** Tabelle.
-   Wählen Sie außerdem die folgenden gespeicherten Prozeduren in der **gespeicherte Prozeduren und Funktionen** Knoten: **DeletePerson**, **InsertPerson**, und **UpdatePerson** . 
-   Ab Visual Studio 2012 die EF-Designer unterstützt Massenexport und-Import von gespeicherten Prozeduren. Die **Import ausgewählten gespeicherten Prozeduren und Funktionen in das Entitätsmodell** ist standardmäßig aktiviert. Da in diesem Beispiel wir Prozeduren, die einfügen gespeichert haben, aktualisieren und Löschen von Entitätstypen, wir nicht importieren möchten, und es wird, deaktivieren Sie dieses Kontrollkästchen. 

    ![S Prozesse importieren](~/ef6/media/importsprocs.jpg)

-   Klicken Sie auf **Fertig stellen**.
    Im EF Designer, der bietet eine Entwurfsoberfläche zum Bearbeiten des Modells, wird angezeigt.

## <a name="map-the-person-entity-to-stored-procedures"></a>Zuordnen von der Person-Entität zu gespeicherten Prozeduren

-   Mit der rechten Maustaste die **Person** Entitätstyp, und wählen Sie **Zuordnung der gespeicherten Prozedur**.
-   Die gespeicherte Prozedur Zuordnungen angezeigt, der **Mappingdetails** Fenster.
-   Klicken Sie auf  **&lt;Insert Select-Funktion&gt;**.
    Das Feld wird zu einer Dropdownliste mit den gespeicherten Prozeduren im Speichermodell, die Entitätstypen im konzeptionellen Modell zugeordnet werden können.
    Wählen Sie **InsertPerson** aus der Dropdown-Liste.
-   Es werden Standardmappings zwischen Parametern gespeicherter Prozeduren und Entitätseigenschaften angezeigt. Pfeile geben die Mappingrichtung an: Eigenschaftswerte sind Parametern gespeicherter Prozeduren zugeordnet.
-   Klicken Sie auf  **&lt;Result Binding hinzufügen&gt;**.
-   Typ **NewPersonID**, den Namen des Parameters, die zurückgegeben werden, indem die **InsertPerson** gespeicherte Prozedur. Stellen Sie sicher, dass keine führende oder nachfolgende Leerzeichen eingeben.
-   Drücken Sie die **EINGABETASTE**.
-   In der Standardeinstellung **NewPersonID** zugeordnet ist, um den Entitätsschlüssel **PersonID**. Ein Pfeil gibt die Mappingrichtung an: Der Wert der Ergebnisspalte ist der Eigenschaft zugeordnet.

    ![Zuordnungsdetails](~/ef6/media/mappingdetails.png)

-   Klicken Sie auf **&lt;Update Function auswählen&gt;** , und wählen Sie **UpdatePerson** aus der resultierenden Dropdownliste aus.
-   Es werden Standardmappings zwischen Parametern gespeicherter Prozeduren und Entitätseigenschaften angezeigt.
-   Klicken Sie auf **&lt;Delete Function auswählen&gt;** , und wählen Sie **DeletePerson** aus der resultierenden Dropdownliste aus.
-   Es werden Standardmappings zwischen Parametern gespeicherter Prozeduren und Entitätseigenschaften angezeigt.

Einfügen, aktualisieren und löschen Sie Prozesse für die **Person** Entitätstyp werden nun gespeicherten Prozeduren zugeordnet.

Wenn Sie die parallelitätsprüfung beim Aktualisieren oder Löschen einer Entität mit gespeicherten Prozeduren aktivieren möchten, verwenden Sie eine der folgenden Optionen aus:

-   Verwenden einer **Ausgabe** Parameter zum Zurückgeben der Anzahl der betroffenen Zeilen aus der gespeicherten Prozedur und überprüfen Sie die **Parameter für betroffene Zeilen** Kontrollkästchen neben den Namen des Parameters. Wenn der zurückgegebene Wert 0 (null) ist, wenn der Vorgang aufgerufen wird, eine [ **OptimisticConcurrencyException** ](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) ausgelöst.
-   Überprüfen Sie die **Originalwert verwenden** Kontrollkästchen neben einer Eigenschaft, die Sie für die parallelitätsüberprüfung verwenden möchten. Wenn ein Update versucht wird, wird der Wert der Eigenschaft, die ursprünglich aus der Datenbank gelesen wurde verwendet werden, beim Schreiben von Daten in der Datenbank gesichert. Wenn der Wert wird nicht den Wert in der Datenbank entspricht einer **OptimisticConcurrencyException** ausgelöst.

## <a name="use-the-model"></a>Verwenden Sie das Modell

Öffnen der **"Program.cs"** -Datei, in der **Main** Methode definiert ist. Fügen Sie der Main-Funktion mit den folgenden Code.

Der Code erstellt ein neues **Person** -Objekt, klicken Sie dann das Objekt aktualisiert und löscht schließlich das-Objekt.         

``` csharp
    using (var context = new SchoolEntities())
    {
        var newInstructor = new Person
        {
            FirstName = "Robyn",
            LastName = "Martin",
            HireDate = DateTime.Now,
            Discriminator = "Instructor"
        }

        // Add the new object to the context.
        context.People.Add(newInstructor);

        Console.WriteLine("Added {0} {1} to the context.",
            newInstructor.FirstName, newInstructor.LastName);

        Console.WriteLine("Before SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // SaveChanges will call the InsertPerson sproc.  
        // The PersonID property will be assigned the value
        // returned by the sproc.
        context.SaveChanges();

        Console.WriteLine("After SaveChanges, the PersonID is: {0}",
            newInstructor.PersonID);

        // Modify the object and call SaveChanges.
        // This time, the UpdatePerson will be called.
        newInstructor.FirstName = "Rachel";
        context.SaveChanges();

        // Remove the object from the context and call SaveChanges.
        // The DeletePerson sproc will be called.
        context.People.Remove(newInstructor);
        context.SaveChanges();

        Person deletedInstructor = context.People.
            Where(p => p.PersonID == newInstructor.PersonID).
            FirstOrDefault();

        if (deletedInstructor == null)
            Console.WriteLine("A person with PersonID {0} was deleted.",
                newInstructor.PersonID);
    }
```

-   Kompilieren Sie die Anwendung, und führen Sie sie aus. Das Programm erzeugt die folgende Ausgabe *
    >[!NOTE]
> PersonID wird vom Server automatisch generiert, damit wahrscheinlich eine unterschiedliche Anzahl * angezeigt wird

```
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

Wenn Sie mit der endgültigen Version von Visual Studio arbeiten, können Sie Intellitrace mit dem Debugger verwenden, um die SQL-Anweisungen, die ausgeführt werden, finden Sie unter.

![IntelliTrace](~/ef6/media/intellitrace.png)
