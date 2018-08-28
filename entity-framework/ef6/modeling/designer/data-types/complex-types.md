---
title: Komplexe Typen - EF Designer - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 9a8228ef-acfd-4575-860d-769d2c0e18a1
ms.openlocfilehash: d35504cbe60823249d54385962568802b3e41308
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994852"
---
# <a name="complex-types---ef-designer"></a>Komplexe Typen - EF-Designer
In diesem Thema wird gezeigt, wie komplexe Typen mit dem Entity Framework Designer (EF-Designer) zugeordnet und für Entitäten Abfragen, die Eigenschaften des komplexen Typs enthalten.

Die folgende Abbildung zeigt die wichtigsten Windows, die bei der Arbeit mit dem EF Designer verwendet werden.

![EFDesigner](~/ef6/media/efdesigner.png)

> [!NOTE]
> Wenn Sie das konzeptionelle Modell erstellen, können Warnungen zu nicht zugeordneten Entitäten und Zuordnungen in der Fehlerliste angezeigt. Sie können diese Warnungen ignorieren, da nach der Auswahl, um die Datenbank aus dem Modell zu generieren, die Fehler verschwinden werden.

## <a name="what-is-a-complex-type"></a>Was ist ein komplexer Typ

Komplexe Typen sind nicht skalare Eigenschaften von Entitätstypen, mit deren Hilfe skalare Eigenschaften in Entitäten organisiert werden können. Wie Entitäten bestehen komplexe Typen aus skalaren Eigenschaften oder anderen Eigenschaften von komplexem Typ.

Wenn Sie mit Objekten, die komplexe Typen darstellen arbeiten, werden Sie folgende Punkte zu beachten:

-   Komplexe Typen verfügen über keine Schlüssel, und daher nicht unabhängig sein. Komplexe Typen können nur Eigenschaften von Entitätstypen oder anderen komplexen Typen sein.
-   Komplexe Typen können nicht an Zuordnungen beteiligt und können keine Navigationseigenschaften enthalten.
-   Eigenschaften eines komplexen Typs darf nicht sein **null**. Ein ** "InvalidOperationException" ** tritt auf, wenn **"DbContext.SaveChanges"** aufgerufen wird und ein null-komplexeres Objekt festgestellt wird. Skalare Eigenschaften komplexer Objekte möglich **null**.
-   Komplexe Typen können nicht von anderen komplexen Typen erben.
-   Sie müssen den komplexen Typ als definieren eine **Klasse**. 
-   EF erkennt Änderungen an Elemente in ein komplexes Typobjekt beim **DbContext.DetectChanges** aufgerufen wird. Entitätsframework ruft **DetectChanges** automatisch die folgenden Elemente werden aufgerufen: **DbSet.Find**, **DbSet.Local**, **DbSet.Remove**, **"DbSet.Add"**, **DbSet.Attach**, **"DbContext.SaveChanges"**, **DbContext.GetValidationErrors**, **"DbContext.Entry"**, **DbChangeTracker.Entries**.

## <a name="refactor-an-entitys-properties-into-new-complex-type"></a>Umgestalten Sie Eigenschaften einer Entität in neuen komplexen Typ

Wenn Sie bereits eine Entität im konzeptionellen Modell, die möglicherweise möchten Sie einige der Eigenschaften in eine Eigenschaft eines komplexen Typs umzugestalten haben.

Wählen Sie auf die Designeroberfläche, eine oder mehrere Eigenschaften (keine Navigationseigenschaften) einer Entität, klicken Sie dann mit der rechten Maustaste und wählen Sie **Umgestalten -&gt; verschieben in neuen komplexen Typ**.

![Umgestalten](~/ef6/media/refactor.png)

Ein neuer komplexer Typ mit den ausgewählten Eigenschaften wird hinzugefügt, um die **Modellbrowser**. Dem komplexen Typ wird ein Standardname zugewiesen.

Die ausgewählten Eigenschaften werden durch eine komplexe Eigenschaft des neu erstellten Typs ersetzt. Alle Eigenschaftenzuordnungen werden beibehalten.

![Refactor2](~/ef6/media/refactor2.png)

## <a name="create-a-new-complex-type"></a>Erstellen Sie einen neuen komplexen Typ

Sie können auch einen neuen komplexen Typ erstellen, der keine Eigenschaften einer vorhandenen Entität enthält.

Mit der rechten Maustaste die **komplexe Typen** Ordner zeigen Sie im Modellbrowser auf **AddNew komplexen Typs...** . Alternativ können Sie auswählen der **komplexe Typen** Ordner, und drücken Sie die **einfügen** auf der Tastatur die Taste.

![AddNewComplextype](~/ef6/media/addnewcomplextype.png)

Dem Ordner wird ein neuer komplexer Typ mit einem Standardnamen hinzugefügt. Jetzt können Sie Eigenschaften in den Typ hinzufügen.

## <a name="add-properties-to-a-complex-type"></a>Fügen Sie Eigenschaften in einen komplexen Typ hinzu.

Bei den Eigenschaften eines komplexen Typs kann es sich um skalare Typen oder vorhandene komplexe Typen handeln. Für die Eigenschaften eines komplexen Typs können jedoch keine Zirkelverweise verwendet werden. Z. B. einen komplexen Typ **OnsiteCourseDetails** eine Eigenschaft eines komplexen Typs sind keine **OnsiteCourseDetails**.

Einem komplexen Typ können anhand der unten aufgeführten Methoden Eigenschaften hinzugefügt werden.

-   Mit der rechten Maustaste im Modellbrowser eines komplexen Typs, zeigen Sie auf **hinzufügen**, zeigen Sie dann auf **Skalareigenschaft** oder **komplexe Eigenschaft**, wählen Sie dann den gewünschten Eigenschaftentyp. Alternativ können Sie wählen einen komplexen Typ und drücken Sie dann die **einfügen** auf der Tastatur die Taste.  

    ![AddPropertiestoComplexType](~/ef6/media/addpropertiestocomplextype.png)

    Dem komplexen Typ wird eine neue Eigenschaft mit einem Standardnamen hinzugefügt.

- OR-

-   Mit der rechten Maustaste einer Entitätseigenschaft auf der **EF Designer** Entwurfsoberfläche, und wählen **Kopie**, per Rechtsklick komplexen Typs in der **Modellbrowser** , und wählen Sie  **Fügen Sie**.

## <a name="rename-a-complex-type"></a>Umbenennen eines komplexen Typs

Wenn Sie einen komplexen Typ umbenennen, werden alle Verweise auf den Typ im gesamten Projekt aktualisiert.

-   Doppelklicken Sie langsam auf einen komplexen Typ in der **Modellbrowser**.
    Der Name wird markiert und kann bearbeitet werden.

- OR-

-   Mit der rechten Maustaste in eines komplexen Typs in der **Modellbrowser** , und wählen Sie **umbenennen**.

- OR-

-   Markieren Sie im Modellbrowser einen komplexen Typ, und drücken Sie die F2-TASTE.

- OR-

-   Mit der rechten Maustaste in eines komplexen Typs in der **Modellbrowser** , und wählen Sie **Eigenschaften**. Bearbeiten Sie den Namen in der **Eigenschaften** Fenster.

## <a name="add-an-existing-complex-type-to-an-entity-and-map-its-properties-to-table-columns"></a>Fügen Sie einen vorhandenen komplexen Typ auf eine Entität hinzu, und ordnen Sie seine Eigenschaften zu Tabellenspalten

1.  Mit der rechten Maustaste einer Entitätstyps, zeigen Sie auf **Add New**, und wählen Sie **komplexe Eigenschaft**.
    Der Entität wird eine Eigenschaft eines komplexen Typs mit einem Standardnamen hinzugefügt. Der Eigenschaft wird ein Standardtyp zugewiesen (ausgewählt aus den vorhandenen komplexen Typen).
2.  Weisen Sie den gewünschten Typ der Eigenschaft in der **Eigenschaften** Fenster.
    Nachdem Sie einer Entität eine Eigenschaft eines komplexen Typs hinzugefügt haben, müssen Sie die zugehörigen Eigenschaften Tabellenspalten zuordnen.
3.  Mit der rechten Maustaste in eines Entitätstyps auf der Entwurfsoberfläche oder in der **Modellbrowser** , und wählen Sie **Tabellenzuordnungen**.
    Die tabellenzuordnungen werden angezeigt, der **Mappingdetails** Fenster.
4.  Erweitern Sie die **ordnet &lt;Tabellenname&gt;**  Knoten.
    Ein **Spaltenzuordnungen** Knoten angezeigt wird.
5.  Erweitern Sie die **Spaltenzuordnungen** Knoten.
    Eine Liste aller Spalten in der Tabelle wird angezeigt. Die Standardeigenschaften (sofern vorhanden), denen die Spalten zugeordnet sind aufgeführt, unter dem **Wert/Eigenschaft** Überschrift.
6.  Wählen Sie die Spalte, die Sie zuordnen möchten und dann auf den entsprechenden **Wert/Eigenschaft** Feld.
    Eine Dropdownliste aller skalaren Eigenschaften wird angezeigt.
7.  Wählen Sie die entsprechende Eigenschaft aus.

    ![MapComplexType](~/ef6/media/mapcomplextype.png)

8.  Wiederholen Sie die Schritte 6 und 7 für jede Tabellenspalte.

>[!NOTE]
> Um eine spaltenzuordnung zu löschen, wählen Sie die Spalte, die Sie verwenden möchten, ordnen, und klicken Sie dann auf die **Wert/Eigenschaft** Feld. Wählen Sie dann **löschen** aus der Dropdown-Liste.

## <a name="map-a-function-import-to-a-complex-type"></a>Zuordnen eines Funktionsimports zu einem komplexen Typ

Funktionsimporte basieren auf gespeicherten Prozeduren. Um einen Funktionsimport einem komplexen Typ zuzuordnen, muss die Zahl der von der entsprechenden gespeicherten Prozedur zurückgegebenen Spalten der Zahl der Eigenschaften des komplexen Typs entsprechen, und die Spalten müssen einen Speichertyp aufweisen, der mit den Typen der Eigenschaften kompatibel ist.

-   Doppelklicken Sie auf einer importierten Funktion, die Sie in einen komplexen Typ zuordnen möchten.

    ![FunctionImports](~/ef6/media/functionimports.png)

-   Geben Sie wie folgt die Einstellungen für den neuen Funktionsimport ein:
    -   Geben Sie die gespeicherte Prozedur, die für die Sie einen Funktionsimport im Erstellen der **Name der gespeicherten Prozedur** Feld. Dieses Feld wird als Dropdownliste angezeigt, die alle gespeicherten Prozeduren im Speichermodell enthält.
    -   Geben Sie den Namen des Funktionsimports im der **Funktionsnamen importieren** Feld.
    -   Wählen Sie **komplexe** als Rückgabetyp geben, und geben Sie dann den speziellen komplexen Rückgabetyp durch Auswählen des entsprechenden Typs aus der Dropdown-Liste.

        ![EditFunctionImport](~/ef6/media/editfunctionimport.png)

-   Klicken Sie auf **OK**.
    Der Funktionsimport-Eintrag wird im konzeptionellen Modell erstellt.

### <a name="customize-column-mapping-for-function-import"></a>Anpassen der Spaltenzuordnung für den Funktionsimport

-   Den Funktionsimport im Modellbrowser Maustaste und wählen Sie **Funktionsimportmapping**.
    Die **Mappingdetails** Fenster angezeigt, und zeigt die standardzuordnung für den Funktionsimport. Pfeilsymbole geben die Zuordnungen der Spaltenwerte zu den Eigenschaftswerten an. Standardmäßig wird angenommen, dass die Spaltennamen mit den Namen der Eigenschaften des komplexen Typs identisch sind. Die Standardspaltennamen werden in grauem Text angezeigt.
-   Ändern Sie, falls erforderlich, die Spaltennamen so, dass sie mit den Spaltennamen übereinstimmen, die von der gespeicherten Prozedur zurückgegeben werden, die dem Funktionsimport entspricht.

## <a name="delete-a-complex-type"></a>Löschen eines komplexen Typs

Wenn Sie einen komplexen Typ löschen, wird der Typ aus dem konzeptionellen Modell gelöscht, und die Mappings für alle Instanzen des Typs werden gelöscht. Verweise auf den Typ werden jedoch nicht aktualisiert. Angenommen, eine Entität hat die Eigenschaft eines komplexen Typs complexType1 verfügt und ComplexType1 im gelöscht wird die **Modellbrowser**, die entsprechende Entitätseigenschaft nicht aktualisiert. Das Modell wird nicht überprüft werden, da es sich um eine Entität enthält, die auf einen gelöschten komplexen Typ verweist. Verweise auf gelöschte komplexe Typen können mit dem Entity Designer aktualisiert werden.

-   Mit der rechten Maustaste im Modellbrowser eines komplexen Typs, und wählen Sie **löschen**.

- OR-

-   Markieren Sie im Modellbrowser einen komplexen Typ, und drücken Sie dann die ENTF-TASTE auf der Tastatur.

## <a name="query-for-entities-containing-properties-of-complex-type"></a>Abfragen von Entitäten, die Eigenschaften des komplexen Typs enthält.

Der folgende Code zeigt, wie Sie eine Abfrage auszuführen, die eine Auflistung der Entität zurückgibt, die Eigenschaft eines komplexen Typs enthalten.

``` csharp
    using (SchoolEntities context = new SchoolEntities())
    {
        var courses =
            from c in context.OnsiteCourses
            order by c.Details.Time
            select c;

        foreach (var c in courses)
        {
            Console.WriteLine("Time: " + c.Details.Time);
            Console.WriteLine("Days: " + c.Details.Days);
            Console.WriteLine("Location: " + c.Details.Location);
        }
    }
```
