---
title: Gespeicherte Prozeduren für Designer-CUD-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1e773972-2da5-45e0-85a2-3cf3fbcfa5cf
ms.openlocfilehash: bdb0df969c33d5ad3f103bfa9af6002c9c2bb9b3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415142"
---
# <a name="designer-cud-stored-procedures"></a>Designer-CUD-Prozeduren

In dieser schrittweisen exemplarischen Vorgehensweise wird veranschaulicht, wie die Vorgänge Create\\INSERT, Update und DELETE (CUD) eines Entitäts Typs mithilfe des Entity Framework Designer (EF-Designers) zu gespeicherten Prozeduren zugeordnet werden.  Standardmäßig generiert das Entity Framework automatisch die SQL-Anweisungen für die CUD-Vorgänge, Sie können jedoch auch gespeicherte Prozeduren zu diesen Vorgängen zuordnen.  

Beachten Sie, dass Code First keine Zuordnung zu gespeicherten Prozeduren oder Funktionen unterstützt. Allerdings können Sie gespeicherte Prozeduren oder Funktionen mithilfe der System. Data. Entity. dbset. sqlQuery-Methode abrufen. Beispiel:

``` csharp
var query = context.Products.SqlQuery("EXECUTE [dbo].[GetAllProducts]");
```

## <a name="considerations-when-mapping-the-cud-operations-to-stored-procedures"></a>Überlegungen bei der Zuordnung der CUD-Vorgänge zu gespeicherten Prozeduren

Bei der Zuordnung der CUD-Vorgänge zu gespeicherten Prozeduren gelten die folgenden Überlegungen:

- Wenn Sie einen der CUD-Vorgänge einer gespeicherten Prozedur zuordnen, ordnen Sie alle zu. Wenn Sie nicht alle drei zuordnen, schlagen die nicht zugeordneten Vorgänge fehl, wenn Sie ausgeführt werden und ein **Update Exception** - ausgelöst wird.
- Sie müssen den einzelnen Parametern der gespeicherten Prozedur Entitäts Eigenschaften zuordnen.
- Wenn der Server den Primärschlüssel Wert für die eingefügte Zeile generiert, müssen Sie diesen Wert wieder der Schlüsseleigenschaft der Entität zuordnen. Im folgenden Beispiel gibt die gespeicherte Prozedur **InsertPerson** den neu erstellten Primärschlüssel als Teil des Resultsets der gespeicherten Prozedur zurück. Der Primärschlüssel wird dem Entitäts Schlüssel (**PersonID**) mithilfe der **&lt;Hinzufügen von Ergebnis Bindungen&gt;**  Features des EF-Designers zugeordnet.
- Die Aufrufe gespeicherter Prozeduren werden 1:1 mit den Entitäten im konzeptionellen Modell zugeordnet. Wenn Sie z. b. eine Vererbungs Hierarchie in ihrem konzeptionellen Modell implementieren und dann die gespeicherten CUD-Prozeduren für die übergeordneten (Basis-) **und unter** **geordneten** (abgeleiteten) Entitäten zuordnen, werden beim Speichern der unter **geordneten Änderungen nur** die gespeicherten Prozeduren **des unter** **geordneten**Elements aufgerufen.

## <a name="prerequisites"></a>Voraussetzungen

Um die exemplarische Vorgehensweise nachzuvollziehen, benötigen Sie Folgendes:

- Eine aktuelle Version von Visual Studio.
- Die [Beispieldatenbank "School](~/ef6/resources/school-database.md)".

## <a name="set-up-the-project"></a>Einrichten des Projekts

- Öffnen Sie Visual Studio 2012.
- **Datei&gt; neues&gt; Projekt** auswählen
- Klicken Sie im linken Bereich auf **Visual C-\#** , und wählen Sie dann die **Konsolen** Vorlage aus.
- Geben Sie " **cudsprocssample** " als Namen ein.
- Wählen Sie **OK** aus.

## <a name="create-a-model"></a>Modellerstellung

- Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen, und wählen Sie **Hinzufügen-&gt; neues Element**aus.
- Wählen Sie im linken Menü **Daten** aus, und wählen Sie dann im Bereich Vorlagen die Option **ADO.NET Entity Data Model** aus.
- Geben Sie für den Dateinamen " **cudsprocs. edmx** " ein, und klicken Sie dann auf **Hinzufügen**.
- Wählen Sie im Dialogfeld Modell Inhalte auswählen die Option **aus Datenbank generieren aus**, und klicken Sie dann auf **weiter**.
- Klicken Sie auf **neue Verbindung**. Geben Sie im Dialogfeld Verbindungs Eigenschaften den Servernamen ein (z. b. **(localdb)\\mssqllocaldb**), wählen Sie die Authentifizierungsmethode aus, geben Sie **School** als Datenbanknamen ein, und klicken Sie dann auf **OK**.
    Das Dialogfeld Wählen Sie Ihre Datenverbindung aus wird mit Ihrer Daten bankverbindungs Einstellung aktualisiert.
- Wählen Sie im Dialogfeld Wählen Sie Ihre Datenbankobjekte aus unter dem Knoten **Tabellen** die **Person** -Tabelle aus.
- Wählen Sie außerdem die folgenden gespeicherten Prozeduren unter dem Knoten **gespeicherte Prozeduren und Funktionen** aus: **DeletePerson**, **InsertPerson**und **updateperson**.
- Ab Visual Studio 2012 unterstützt der EF-Designer den Massen Import gespeicherter Prozeduren. Die Option zum **importieren ausgewählter gespeicherter Prozeduren und Funktionen in das Entitäts Modell** ist standardmäßig aktiviert. Da in diesem Beispiel gespeicherte Prozeduren zum Einfügen, aktualisieren und Löschen von Entitäts Typen verwendet werden, die nicht importiert werden sollen, wird dieses Kontrollkästchen nicht mehr unternimmt.

    ![S-Prozeduren importieren](~/ef6/media/importsprocs.jpg)

- Klicken Sie auf **Finish**.
    Der EF-Designer, der eine Entwurfs Oberfläche zum Bearbeiten des Modells bereitstellt, wird angezeigt.

## <a name="map-the-person-entity-to-stored-procedures"></a>Zuordnen der Person-Entität zu gespeicherten Prozeduren

- Klicken Sie mit der rechten Maustaste auf den Entitätstyp **Person** und wählen Sie **Zuordnung der gespeicherten Prozedur**
- Die Zuordnungen gespeicherter Prozeduren werden im Fenster **Mappingdetails** angezeigt.
- Klicken Sie auf **&lt;INSERT-Funktion&gt;** .
    Das Feld wird zu einer Dropdownliste mit den gespeicherten Prozeduren im Speichermodell, die Entitätstypen im konzeptionellen Modell zugeordnet werden können.
    Wählen Sie in der Dropdown Liste **InsertPerson** - aus.
- Es werden Standardmappings zwischen Parametern gespeicherter Prozeduren und Entitätseigenschaften angezeigt. Pfeile geben die Mappingrichtung an: Eigenschaftswerte sind Parametern gespeicherter Prozeduren zugeordnet.
- Klicken Sie auf **&lt;Ergebnis Bindungs&gt;hinzufügen **.
- Geben Sie **NewPersonID**ein, den Namen des Parameters, der vom **InsertPerson** - gespeicherten Prozedur zurückgegeben wird. Stellen Sie sicher, dass Sie keine führenden oder nachfolgenden Leerzeichen eingeben.
- Drücken **Sie Eingabe**Taste.
- Standardmäßig wird **NewPersonID** dem Entitäts Schlüssel **PersonID**zugeordnet. Ein Pfeil gibt die Mappingrichtung an: Der Wert der Ergebnisspalte ist der Eigenschaft zugeordnet.

    ![Mappingdetails](~/ef6/media/mappingdetails.png)

- Klicken Sie **&lt;wählen Sie &gt;Funktion aktualisieren** aus, und wählen Sie in der resultierenden Dropdown Liste **updateperson** aus.
- Es werden Standardmappings zwischen Parametern gespeicherter Prozeduren und Entitätseigenschaften angezeigt.
- Klicken Sie **&lt;klicken Sie auf Funktion löschen&gt;**  , und wählen Sie in der resultierenden Dropdown Liste die Option **DeletePerson** aus.
- Es werden Standardmappings zwischen Parametern gespeicherter Prozeduren und Entitätseigenschaften angezeigt.

Die Einfüge-, Aktualisierungs-und Löschvorgänge der **Person** Entitätstyp werden nun gespeicherten Prozeduren zugeordnet.

Wenn Sie die Parallelitäts Überprüfung beim Aktualisieren oder Löschen einer Entität mit gespeicherten Prozeduren aktivieren möchten, verwenden Sie eine der folgenden Optionen:

- Verwenden Sie einen **Output** -Parameter, um die Anzahl der betroffenen Zeilen von der gespeicherten Prozedur zurückzugeben, und aktivieren Sie das Kontrollkästchen **betroffene Zeilen** Kontrollkästchen neben dem Parameternamen. Wenn der zurückgegebene Wert 0 (null) ist, wenn der Vorgang aufgerufen wird, wird ein  [**optimistic-** ](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) Ereignis ausgelöst.
- Aktivieren Sie das Kontrollkästchen **ursprünglichen Wert verwenden** neben einer Eigenschaft, die Sie für die Parallelitäts Überprüfung verwenden möchten. Wenn versucht wird, ein Update auszuführen, wird der Wert der Eigenschaft, die ursprünglich aus der Datenbank gelesen wurde, beim Zurückschreiben von Daten in die Datenbank verwendet. Wenn der Wert nicht mit dem Wert in der Datenbank identisch ist, wird eine **optimistic-** Ereignis ausgelöst.

## <a name="use-the-model"></a>Verwenden des Modells

Öffnen Sie die Datei **Program.cs** , in der die **Main** -Methode definiert ist. Fügen Sie der Main-Funktion den folgenden Code hinzu.

Der Code erstellt ein neues **Person** -Objekt, aktualisiert dann das-Objekt und löscht schließlich das-Objekt.

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

- Kompilieren Sie die Anwendung, und führen Sie sie aus. Das Programm erzeugt die folgende Ausgabe *

> [!NOTE]
> PersonID wird vom Server automatisch generiert, sodass Sie wahrscheinlich eine andere Anzahl sehen *

``` Output
Added Robyn Martin to the context.
Before SaveChanges, the PersonID is: 0
After SaveChanges, the PersonID is: 51
A person with PersonID 51 was deleted.
```

Wenn Sie mit der Ultimate-Version von Visual Studio arbeiten, können Sie IntelliTrace mit dem Debugger verwenden, um die ausgeführten SQL-Anweisungen anzuzeigen.

![IntelliTrace](~/ef6/media/intellitrace.png)
