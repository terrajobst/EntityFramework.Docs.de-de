---
title: Räumliche - EF Designer - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
caps.latest.revision: 3
ms.openlocfilehash: 2332818275763fd90274f426b7fced4c3c14ac17
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121324"
---
# <a name="spatial---ef-designer"></a>Räumliche - EF-Designer
> [!NOTE]
> **EF5 oder höher, nur** -APIs, die Funktionen erläutert, die auf dieser Seite usw. in Entity Framework 5 eingeführt wurden. Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.

Die video und schrittweise exemplarische Vorgehensweise zeigt, wie räumliche Typen, mit dem Entity Framework Designer zugeordnet wird. Darüber hinaus veranschaulicht eine LINQ-Abfrage verwenden, um einen Abstand zwischen zwei Orten zu finden.

In dieser exemplarischen Vorgehensweise wird zum Erstellen einer neuen Datenbank Model First verwenden, aber dem EF Designer kann auch verwendet werden, mit der [Database First](~/ef6/modeling/designer/workflows/database-first.md) Workflow um eine vorhandene Datenbank zuzuordnen.

Unterstützung von räumlichen Typen wurde in Entity Framework 5 eingeführt. Beachten Sie, um die neuen Features wie räumlichen Typen, Enumerationen und Tabellenwertfunktionen zu verwenden, Sie .NET Framework 4.5 als Ziel haben müssen. Visual Studio 2012 ist standardmäßig die Zielversion .NET 4.5.

Verwendung der Typen von räumlichen Daten müssen Sie auch einen Entity Framework-Anbieter verwenden, der Unterstützung von räumlichen Daten verfügt. Finden Sie unter [anbieterunterstützung für räumliche Typen](~/ef6/fundamentals/providers/spatial-support.md) für Weitere Informationen.

Es gibt zwei Arten von main räumliche Daten: Geography und Geometry. Der Geography-Datentyp speichert ellipsenförmige Daten (z. B. GPS-Breiten- und Längengrad koordiniert). Der Geometry-Datentyp stellt euklidischen (flachen) Koordinatensystem dar.

## <a name="watch-the-video"></a>Video ansehen
Dieses Video zeigt, wie Sie räumliche Typen, mit dem Entity Framework Designer zuordnen. Darüber hinaus veranschaulicht eine LINQ-Abfrage verwenden, um einen Abstand zwischen zwei Orten zu finden.

**Präsentiert von**: Julia Kornich

**Video**: [WMV](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)

## <a name="pre-requisites"></a>Voraussetzungen

Sie müssen Visual Studio 2012, Ultimate, Premium, Professional oder Web Express Edition installiert, um diese exemplarische Vorgehensweise abgeschlossen haben.

## <a name="set-up-the-project"></a>Einrichten des Projekts

1.  Visual Studio 2012 öffnen
2.  Auf der **Datei** Startmenü **neu**, und klicken Sie dann auf **Projekt**
3.  Klicken Sie im linken Bereich auf **Visual C\#**, und wählen Sie dann die **Konsole** Vorlage
4.  Geben Sie **SpatialEFDesigner** als Namen für das Projekt und auf **OK**

## <a name="create-a-new-model-using-the-ef-designer"></a>Erstellen eines neuen Modells mit dem EF-Designer

1.  Mit der rechten Maustaste des Projektnamen im Projektmappen-Explorer, zeigen Sie auf **hinzufügen**, und klicken Sie dann auf **neues Element**
2.  Wählen Sie **Daten** aus dem linken Menü und wählen Sie dann **ADO.NET Entity Data Model** im Bereich Vorlagen
3.  Geben Sie **UniversityModel.edmx** für den Dateinamen ein, und klicken Sie dann auf **hinzufügen**
4.  Wählen Sie auf der Seite Assistenten für Entity Data Model **leeres Modell** im Dialogfeld ' Modellinhalte auswählen '
5.  Klicken Sie auf **Fertig stellen**

Im Entity Designer, der bietet eine Entwurfsoberfläche zum Bearbeiten des Modells, wird angezeigt.

Der Assistent führt die folgenden Aktionen aus:

-   Generiert die EnumTestModel.edmx-Datei, die das konzeptionelle Modell, Speichermodell und die Zuordnung definiert. Legt die Verarbeitung der Metadatenartefakte-Eigenschaft der EDMX-Datei zum Einbetten in Ausgabeassembly fest, damit die generierten Metadaten-Dateien in die Assembly eingebettet werden.
-   Fügt einen Verweis auf die folgenden Assemblys hinzu: EntityFramework System.ComponentModel.DataAnnotations und System.Data.Entity.
-   UniversityModel.tt und UniversityModel.Context.tt-Dateien erstellt, und klicken Sie unter der EDMX-Datei hinzugefügt. Diese Dateien der T4-Vorlage generieren den Code, der definiert, die "DbContext" abgeleitet und POCO-Typen, die die Entitäten in der EDMX-Modell zuordnen

## <a name="add-a-new-entity-type"></a>Fügen Sie einen neuen Entitätstyp hinzu

1.  Mit der rechten Maustaste in eines leeren Bereichs der Entwurfsoberfläche, wählen **hinzufügen –&gt; Entität**, das Dialogfeld "neue Entität" angezeigt wird
2.  Geben Sie **University** für den Typ ein, und geben **Universitäts** für den Namen "Key"-Eigenschaft wählen Sie in den Typ als **Int32**
3.  Klicken Sie auf **OK**.
4.  Mit der rechten Maustaste in der Entitäts, und wählen Sie **Add New -&gt; Skalareigenschaft**
5.  Benennen Sie die neue Eigenschaft **Name**
6.  Fügen Sie eine andere skalare Eigenschaft hinzu und benennen Sie sie in **Speicherort** öffnen Sie das Fenster "Eigenschaften" aus, und ändern Sie den Typ der neuen Eigenschaft, die **Geography**
7.  Speichern Sie das Modell, und erstellen Sie das Projekt
    > [!NOTE]
    > Bei der Erstellung können Warnungen zu nicht zugeordneten Entitäten und Zuordnungen in der Fehlerliste angezeigt. Sie können diese Warnungen ignorieren, da nach dem wir die Datenbank aus dem Modell generieren möchten, die Fehler verschwinden werden.

## <a name="generate-database-from-model"></a>Datenbank aus Modell generieren

Jetzt können wir eine Datenbank generieren, die für das Modell basiert.

1.  Mit der rechten Maustaste in eines leeren Bereich der Entity-Designer, und wählen **Datenbank aus Modell generieren**
2.  Wählen Sie Ihre Daten Verbindungsdialogfeld des Assistenten zum Generieren von Datenbanken wird angezeigt, klicken Sie auf die **neue Verbindung** Schaltfläche geben **(Localdb)\\Mssqllocaldb** für den Servernamen und  **University** für die Datenbank und auf **OK**
3.  Ein Dialogfeld gefragt, ob Sie eine neue Datenbank erstellen möchten, wird angezeigt, klicken Sie auf **Ja**.
4.  Klicken Sie auf **Weiter** und der Assistent zur Datenbankgenerierung generiert die Datendefinitionssprache (DDL) für eine Datenbank erstellen, die generierte DDL angezeigt wird, in der Zusammenfassung und Einstellungen Dialogfeld das Beachten Sie, dass, die die DDL-Anweisungen keine Definition für enthält eine Tabelle, in den Enumerationstyp zugeordnet.
5.  Klicken Sie auf **Fertig stellen** durch Klicken auf Fertig stellen das DDL-Skript nicht ausgeführt.
6.  Der Assistent zur Datenbankgenerierung bewirkt Folgendes: Öffnet die **UniversityModel.edmx.sql** in T-SQL-Editor generiert Datei in den Store-Schemas und der Mapping-Abschnitten, die Zielimplementierung des zu Informationen zur Verbindungszeichenfolge fügt die Datei "App.config"
7.  Klicken Sie auf der rechten Maustaste im T-SQL-Editor, und wählen Sie **Execute** das Herstellen einer Verbindung mit Server-Dialogfeld angezeigt wird, geben Sie die Verbindungsinformationen aus Schritt 2, und klicken Sie auf **verbinden**
8.  Klicken Sie zum Anzeigen des generierten Schemas mit der rechten Maustaste auf den Datenbanknamen im Objekt-Explorer von SQL Server, und wählen Sie **aktualisieren**

## <a name="persist-and-retrieve-data"></a>Speichern und Abrufen von Daten

Öffnen Sie die Datei "Program.cs", die, in die Main-Methode definiert ist. Fügen Sie der Main-Funktion mit den folgenden Code.

Der Code fügt zwei neue University-Objekte in den Kontext an. Räumliche Eigenschaften werden mithilfe der Methode DbGeography.FromText initialisiert. Der Geography-Punkt dargestellt, wie WellKnownText an die Methode übergeben wird. Der Code speichert dann die Daten. Klicken Sie dann die LINQ-Abfrage, die ein University-Objekt zurückgegeben, bei denen am Speicherort am nächsten am angegebenen Speicherort erstellt und ausgeführt wird.

``` csharp
using (var context = new UniversityModelContainer())
{
    context.Universities.Add(new University()
    {
        Name = "Graphic Design Institute",
        Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
    });

    context.Universities.Add(new University()
    {
        Name = "School of Fine Art",
        Location = DbGeography.FromText("POINT(-122.335197 47.646711)"),
    });

    context.SaveChanges();

    var myLocation = DbGeography.FromText("POINT(-122.296623 47.640405)");

    var university = (from u in context.Universities
                                orderby u.Location.Distance(myLocation)
                                select u).FirstOrDefault();

    Console.WriteLine(
        "The closest University to you is: {0}.",
        university.Name);
}
```

Kompilieren Sie die Anwendung, und führen Sie sie aus. Das Programm erzeugt die folgende Ausgabe:

```
The closest University to you is: School of Fine Art.
```

Klicken Sie zum Anzeigen von Daten in der Datenbank, mit der rechten Maustaste auf den Datenbanknamen im Objekt-Explorer von SQL Server, und wählen Sie **aktualisieren**. Klicken Sie dann auf der rechten Maustaste auf die Tabelle, und wählen **Ansichtsdaten**.

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise erläutert, wie räumliche Typen, die mit dem Entity Framework Designer zuordnen und räumliche Typen im Code verwenden. 
