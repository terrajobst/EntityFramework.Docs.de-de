---
title: Definieren von Abfrage - Designer, EF - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
ms.openlocfilehash: 8415a265cdbe078422e0467ee97da955a81b873d
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250971"
---
# <a name="defining-query---ef-designer"></a>Definieren der Abfrage – EF-Designer
In dieser exemplarischen Vorgehensweise veranschaulicht das Hinzufügen eine definierende Abfrage und eine entsprechende Entität Geben Sie auf ein Modell mit dem EF Designer. Eine definierende Abfrage wird häufig verwendet, um Funktionen wie mit einer Datenbankansicht bereitzustellen, aber die Ansicht im Modell nicht in der Datenbank definiert ist. Einer definierenden Abfrage können Sie eine SQL-Anweisung ausführen, die im angegebenen die **DefiningQuery** Element eine EDMX-Datei. Weitere Informationen finden Sie unter **DefiningQuery** in die [SSDL-Spezifikation](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).

Wenn definierende Abfragen verwenden, müssen Sie auch in Ihrem Modell einen Entitätstyp definiert. Der Entitätstyp wird verwendet, Anzeigen von Daten, die von der definierenden Abfrage verfügbar gemacht werden. Beachten Sie, dass Daten über diesen Entitätstyp verfügbar gemacht ist schreibgeschützt.

Parametrisierte Abfragen können nicht als definierende Abfragen ausgeführt werden. Die Daten können jedoch aktualisiert werden, indem die Insert-, Update- und Delete-Funktionen des Entitätstyps, der die Daten zugänglich macht, gespeicherten Prozeduren zugeordnet werden. Weitere Informationen finden Sie unter [INSERT-, Update- und Delete mit gespeicherten Prozeduren](~/ef6/modeling/designer/stored-procedures/cud.md).

In diesem Thema wird gezeigt, wie Sie die folgenden Aufgaben ausführen.

-   Hinzufügen einer definierenden Abfrage
-   Hinzufügen eines Entitätstyps zum Modell
-   Ordnen Sie den Entitätstyp der definierenden Abfrage

## <a name="prerequisites"></a>Erforderliche Komponenten

Um die exemplarische Vorgehensweise nachzuvollziehen, benötigen Sie Folgendes:

- Eine aktuelle Version von Visual Studio.
- Die [Beispieldatenbank ' School '](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Einrichten des Projekts

In dieser exemplarischen Vorgehensweise wird Visual Studio 2012 oder höher verwendet.

-   Öffnen Sie Visual Studio.
-   Zeigen Sie im Menü **Datei** auf **Neu**, und klicken Sie dann auf **Projekt**.
-   Klicken Sie im linken Bereich auf **Visual C\#**, und wählen Sie dann die **Konsolenanwendung** Vorlage.
-   Geben Sie **DefiningQuerySample** als Namen für das Projekt und auf **OK**.

 

## <a name="create-a-model-based-on-the-school-database"></a>Erstellen Sie ein Modell basierend auf der Datenbank "School"

-   Mit der rechten Maustaste des Projektnamen im Projektmappen-Explorer, zeigen Sie auf **hinzufügen**, und klicken Sie dann auf **neues Element**.
-   Wählen Sie **Daten** aus dem linken Menü und wählen Sie dann **ADO.NET Entity Data Model** im Bereich Vorlagen.
-   Geben Sie **DefiningQueryModel.edmx** für den Dateinamen ein, und klicken Sie dann auf **hinzufügen**.
-   Wählen Sie im Dialogfeld "Modellinhalte" **aus Datenbank generieren**, und klicken Sie dann auf **Weiter**.
-   Klicken Sie auf die neue Verbindung. Geben Sie den Servernamen, klicken Sie im Dialogfeld "Verbindungseigenschaften" (z. B. **(Localdb)\\Mssqllocaldb**), wählen die Authentifizierungsmethode, Typ **School** für den Datenbanknamen, und klicken Sie dann Klicken Sie auf **OK**.
    Das Dialogfeld "Wählen Sie Ihre Datenverbindung" wird mit Ihrem datenbankverbindungseinstellung aktualisiert.
-   Klicken Sie im Dialogfeld "Datenbankobjekte auswählen" Überprüfen der **Tabellen** Knoten. Hiermit werden alle Tabellen aus, die **School** Modell.
-   Klicken Sie auf **Fertig stellen**.
-   Klicken Sie im Projektmappen-Explorer mit der Maustaste der **DefiningQueryModel.edmx** und wählen Sie **Öffnen mit...** .
-   Wählen Sie **XML (Text)-Editor**.

    ![XML-Editor](~/ef6/media/xmleditor.png)

-   Klicken Sie auf **Ja** , wenn die folgende Meldung angezeigt:

    ![Warnung 2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a>Hinzufügen einer definierenden Abfrage

In diesem Schritt aus der XML-Editor Hinzufügen eine definierende Abfrage aus, und geben Sie eine Entität, die SSDL-Abschnitt der EDMX-Datei. 

-   Hinzufügen einer **EntitySet** Element die SSDL-Abschnitt der EDMX-Datei (Zeile 5 bis 13). Geben Sie Folgendes an:
    -   Nur die **Namen** und **EntityType** Attribute der **EntitySet** -Element angegeben werden.
    -   Der vollqualifizierte Name des Entitätstyps wird verwendet, der **EntityType** Attribut.
    -   Die SQL-Anweisung ausgeführt werden wird angegeben, der **DefiningQuery** Element.

``` xml
    <!-- SSDL content -->
    <edmx:StorageModels>
      <Schema Namespace="SchoolModel.Store" Alias="Self" Provider="System.Data.SqlClient" ProviderManifestToken="2008" xmlns:store="http://schemas.microsoft.com/ado/2007/12/edm/EntityStoreSchemaGenerator" xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
        <EntityContainer Name="SchoolModelStoreContainer">
           <EntitySet Name="GradeReport" EntityType="SchoolModel.Store.GradeReport">
              <DefiningQuery>
                SELECT CourseID, Grade, FirstName, LastName
                FROM StudentGrade
                JOIN
                (SELECT * FROM Person WHERE EnrollmentDate IS NOT NULL) AS p
                ON StudentID = p.PersonID
              </DefiningQuery>
          </EntitySet>
          <EntitySet Name="Course" EntityType="SchoolModel.Store.Course" store:Type="Tables" Schema="dbo" />
```

-   Hinzufügen der **EntityType** Element die SSDL-Abschnitt der EDMX-Datei. Datei siehe unten. Beachten Sie Folgendes:
    -   Den Wert des der **Namen** Attribut entspricht dem Wert des der **EntityType** -Attribut in der **EntitySet** Element oben zwar den vollqualifizierten Namen des der Entitätstyp wird verwendet, der **EntityType** Attribut.
    -   Die Eigenschaftennamen entsprechen, die von SQL-Anweisung im zurückgegebenen Spaltennamen die **DefiningQuery** -Element (oben).
    -   In diesem Beispiel wird durch einen aus drei Eigenschaften bestehenden Entitätsschlüssel sichergestellt, dass der Schlüsselwert eindeutig ist.

``` xml
    <EntityType Name="GradeReport">
      <Key>
        <PropertyRef Name="CourseID" />
        <PropertyRef Name="FirstName" />
        <PropertyRef Name="LastName" />
      </Key>
      <Property Name="CourseID"
                Type="int"
                Nullable="false" />
      <Property Name="Grade"
                Type="decimal"
                Precision="3"
                Scale="2" />
      <Property Name="FirstName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
      <Property Name="LastName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
    </EntityType>
```

>[!NOTE]
> Wenn Sie später ausführen, die **Modellaktualisierungs-Assistent** Dialogfeld alle am Speichermodell, einschließlich der definierenden Abfragen vorgenommenen Änderungen überschrieben werden.

 

## <a name="add-an-entity-type-to-the-model"></a>Hinzufügen eines Entitätstyps zum Modell

In diesem Schritt fügen wir der Entitätstyp am konzeptionellen Modell mit dem EF Designer hinzu.  Beachten Sie Folgendes:

-   Die **Namen** der Entität entspricht dem Wert von der **EntityType** -Attribut in der **EntitySet** Element oben.
-   Die Eigenschaftennamen entsprechen, die von SQL-Anweisung im zurückgegebenen Spaltennamen die **DefiningQuery** Element oben.
-   In diesem Beispiel wird durch einen aus drei Eigenschaften bestehenden Entitätsschlüssel sichergestellt, dass der Schlüsselwert eindeutig ist.

Öffnen Sie das Modell im EF Designer.

-   Doppelklicken Sie auf die DefiningQueryModel.edmx.
-   Sagen Sie **Ja** , die folgende Meldung angezeigt:

    ![Warnung 2](~/ef6/media/warning2.png)

 

Im Entity Designer, der bietet eine Entwurfsoberfläche zum Bearbeiten des Modells, wird angezeigt.

-   Mit der rechten Maustaste in des Designers-Entwurfsoberfläche, und wählen **Add New**-&gt;**Entität...** .
-   Geben Sie **GradeReport** für den Namen der Entität und **CourseID** für die **Schlüsseleigenschaft**.
-   Mit der rechten Maustaste die **GradeReport** Entität, und wählen **Add New** - &gt; **Skalareigenschaft**.
-   Ändern Sie den Standardnamen der Eigenschaft, die **FirstName**.
-   Fügen Sie eine andere skalare Eigenschaft hinzu, und geben Sie **"LastName"** für den Namen.
-   Fügen Sie eine andere skalare Eigenschaft hinzu, und geben Sie **auf Unternehmensniveau** für den Namen.
-   In der **Eigenschaften** Ändern der **auf Unternehmensniveau**des **Typ** Eigenschaft **Decimal**.
-   Wählen Sie die **FirstName** und **"LastName"** Eigenschaften.
-   In der **Eigenschaften** Ändern der **EntityKey** Eigenschaftswert **"true"**.

Daher wurden die folgenden Elemente hinzugefügt, um die **CSDL** Abschnitt der EDMX-Datei.

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a>Ordnen Sie den Entitätstyp der definierenden Abfrage

In diesem Schritt verwenden wir das Fenster "Mappingdetails" Zuordnen der konzeptionellen Entitätstypen und speicherentitätstypen.

-   Mit der rechten Maustaste die **GradeReport** Entität auf die Entwurfsoberfläche, und wählen **Tabellenzuordnung**.  
    Die **Mappingdetails** Fenster wird angezeigt.
-   Wählen Sie **GradeReport** aus der **&lt;Hinzufügen einer Tabelle oder Sicht&gt;** Dropdown-Liste (befindet sich im **Tabelle**s).  
    Zuordnungen zwischen dem konzeptionellen Standard und Storage **GradeReport** Entitätstyp angezeigt werden.  
    ![Zuordnen von Details3](~/ef6/media/mappingdetails.png)

Daher die **EntitySetMapping** Element dem mappingabschnitt der EDMX-Datei hinzugefügt wird. 

``` xml
    <EntitySetMapping Name="GradeReports">
      <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.GradeReport)">
        <MappingFragment StoreEntitySet="GradeReport">
          <ScalarProperty Name="LastName" ColumnName="LastName" />
          <ScalarProperty Name="FirstName" ColumnName="FirstName" />
          <ScalarProperty Name="Grade" ColumnName="Grade" />
          <ScalarProperty Name="CourseID" ColumnName="CourseID" />
        </MappingFragment>
      </EntityTypeMapping>
    </EntitySetMapping>
```

-   Kompilieren Sie die Anwendung.

 

## <a name="call-the-defining-query-in-your-code"></a>Die definierende Abfrage im Code aufrufen.

Sie können jetzt die definierende Abfrage ausführen, mit der **GradeReport** Entitätstyp. 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
