---
title: Unterstützung von Enumerationen - EF Designer - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
ms.openlocfilehash: a94a497e8c5b3213dd7eb4215de90164d437507d
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250634"
---
# <a name="enum-support---ef-designer"></a>Unterstützung von Enumerationen - EF-Designer
> [!NOTE]
> **EF5 oder höher, nur** -APIs, die Funktionen erläutert, die auf dieser Seite usw. in Entity Framework 5 eingeführt wurden. Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.

Dieses video und schrittweise exemplarische Vorgehensweise zeigt, wie Enumerationstypen mit dem Entity Framework Designer verwendet wird. Es wird veranschaulicht, wie Enumerationen in einer LINQ-Abfrage verwendet wird.

In dieser exemplarischen Vorgehensweise wird zum Erstellen einer neuen Datenbank Model First verwenden, aber dem EF Designer kann auch verwendet werden, mit der [Database First](~/ef6/modeling/designer/workflows/database-first.md) Workflow um eine vorhandene Datenbank zuzuordnen.

Enum-Unterstützung wurde in Entity Framework 5 eingeführt. Um die neuen Features wie Enumerationen, räumliche Datentypen und Tabellenwertfunktionen zu verwenden, müssen Sie .NET Framework 4.5 ausrichten. Visual Studio 2012 ist standardmäßig die Zielversion .NET 4.5.

Im Entity Framework kann eine Enumeration der folgenden zugrunde liegende Typen aufweisen: **Byte**, **Int16**, **Int32**, **Int64** , oder **SByte**.

## <a name="watch-the-video"></a>Video ansehen
Dieses Video zeigt, wie Sie mit der Enum-Typen mit dem Entity Framework Designer. Es wird veranschaulicht, wie Enumerationen in einer LINQ-Abfrage verwendet wird.

**Präsentiert von**: Julia Kornich

**Video**: [WMV](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)

## <a name="pre-requisites"></a>Voraussetzungen

Sie müssen Visual Studio 2012, Ultimate, Premium, Professional oder Web Express Edition installiert, um diese exemplarische Vorgehensweise abgeschlossen haben.

## <a name="set-up-the-project"></a>Einrichten des Projekts

1.  Visual Studio 2012 öffnen
2.  Auf der **Datei** Startmenü **neu**, und klicken Sie dann auf **Projekt**
3.  Klicken Sie im linken Bereich auf **Visual C\#**, und wählen Sie dann die **Konsole** Vorlage
4.  Geben Sie **EnumEFDesigner** als Namen für das Projekt und auf **OK**

## <a name="create-a-new-model-using-the-ef-designer"></a>Erstellen eines neuen Modells mit dem EF-Designer

1.  Mit der rechten Maustaste des Projektnamen im Projektmappen-Explorer, zeigen Sie auf **hinzufügen**, und klicken Sie dann auf **neues Element**
2.  Wählen Sie **Daten** aus dem linken Menü und wählen Sie dann **ADO.NET Entity Data Model** im Bereich Vorlagen
3.  Geben Sie **EnumTestModel.edmx** für den Dateinamen ein, und klicken Sie dann auf **hinzufügen**
4.  Wählen Sie auf der Seite Assistenten für Entity Data Model **leeres Modell** im Dialogfeld ' Modellinhalte auswählen '
5.  Klicken Sie auf **Fertig stellen**

Im Entity Designer, der bietet eine Entwurfsoberfläche zum Bearbeiten des Modells, wird angezeigt.

Der Assistent führt die folgenden Aktionen aus:

-   Generiert die EnumTestModel.edmx-Datei, die das konzeptionelle Modell, Speichermodell und die Zuordnung definiert. Legt die Verarbeitung der Metadatenartefakte-Eigenschaft der EDMX-Datei zum Einbetten in Ausgabeassembly fest, damit die generierten Metadaten-Dateien in die Assembly eingebettet werden.
-   Fügt einen Verweis auf die folgenden Assemblys hinzu: EntityFramework System.ComponentModel.DataAnnotations und System.Data.Entity.
-   EnumTestModel.tt und EnumTestModel.Context.tt-Dateien erstellt, und klicken Sie unter der EDMX-Datei hinzugefügt. Diese Dateien der T4-Vorlage generieren den Code, der definiert, die "DbContext" abgeleitet und POCO-Typen, die die Entitäten in der EDMX-Modell zugeordnet.

## <a name="add-a-new-entity-type"></a>Fügen Sie einen neuen Entitätstyp hinzu

1.  Mit der rechten Maustaste in eines leeren Bereichs der Entwurfsoberfläche, wählen **hinzufügen –&gt; Entität**, das Dialogfeld "neue Entität" angezeigt wird
2.  Geben Sie **Abteilung** für den Typ ein, und geben **"DepartmentID"** für den Namen "Key"-Eigenschaft wählen Sie in den Typ als **Int32**
3.  Klicken Sie auf **OK**.
4.  Mit der rechten Maustaste in der Entitäts, und wählen Sie **Add New -&gt; Skalareigenschaft**
5.  Benennen Sie die neue Eigenschaft **Name**
6.  Ändern Sie den Typ der neuen Eigenschaft, die **Int32** (die neue Eigenschaft wird standardmäßig der Zeichenfolgentyp) um den Typ zu ändern, öffnen Sie das Fenster "Eigenschaften" aus, und ändern Sie die Type-Eigenschaft, um **Int32**
7.  Fügen Sie eine andere skalare Eigenschaft hinzu und benennen Sie sie in **Budget**, ändern Sie den Typ in **Decimal**

## <a name="add-an-enum-type"></a>Fügen Sie einen Enum-Typ

1.  Im Entity Framework Designer mit der Maustaste der Name-Eigenschaft, wählen Sie **in Enumeration konvertieren**

    ![In Enumeration konvertieren](~/ef6/media/converttoenum.png)

2.  In der **Enumerator hinzufügen** Dialogfeldtyp **DepartmentNames** für den Enumerationstypnamen, ändern Sie den zugrunde liegenden Typ, **Int32**, und klicken Sie dann die folgenden Elemente in den Typ hinzufügen: Englisch, Mathematische und Wirtschaftlichkeit

    ![Hinzufügen von Enum-Typ](~/ef6/media/addenumtype.png)

3.  Drücken Sie **OK**
4.  Speichern Sie das Modell, und erstellen Sie das Projekt
    > [!NOTE]
    > Bei der Erstellung können Warnungen zu nicht zugeordneten Entitäten und Zuordnungen in der Fehlerliste angezeigt. Sie können diese Warnungen ignorieren, da nach dem wir die Datenbank aus dem Modell generieren möchten, die Fehler verschwinden werden.

Wenn Sie das Fenster "Eigenschaften" betrachten, werden Sie sehen, dass der Typ der Eigenschaft Name, um geändert wurde **DepartmentNames** und der neu hinzugefügten Enum-Typ wurde hinzugefügt, um die Liste der Typen.

Wenn Sie in das Fenster "Modellbrowser" wechseln, sehen Sie sich, dass der Typ der Knoten Enum-Typen auch hinzugefügt wurde.

![Modellbrowser](~/ef6/media/modelbrowser.png)

>[!NOTE]
> Sie können auch neue Enum-Typen aus diesem Fenster hinzufügen, indem Sie auf der rechten Maustaste und auswählen **Enum-Typ hinzufügen**. Wenn der Typ erstellt wird, es in der Liste der Typen angezeigt, und Sie würde in der Lage, eine Eigenschaft zugeordnet werden soll

## <a name="generate-database-from-model"></a>Datenbank aus Modell generieren

Jetzt können wir eine Datenbank generieren, die für das Modell basiert.

1.  Mit der rechten Maustaste in eines leeren Bereich der Entity-Designer, und wählen **Datenbank aus Modell generieren**
2.  Wählen Sie Ihre Daten Verbindungsdialogfeld des Assistenten zum Generieren von Datenbanken wird angezeigt, klicken Sie auf die **neue Verbindung** Schaltfläche geben **(Localdb)\\Mssqllocaldb** für den Servernamen und  **EnumTest** für die Datenbank und auf **OK**
3.  Ein Dialogfeld gefragt, ob Sie eine neue Datenbank erstellen möchten, wird angezeigt, klicken Sie auf **Ja**.
4.  Klicken Sie auf **Weiter** und der Assistent zur Datenbankgenerierung generiert die Datendefinitionssprache (DDL) für eine Datenbank erstellen, die generierte DDL angezeigt wird, in der Zusammenfassung und Einstellungen Dialogfeld das Beachten Sie, dass, die die DDL-Anweisungen keine Definition für enthält eine Tabelle, in den Enumerationstyp zugeordnet.
5.  Klicken Sie auf **Fertig stellen** durch Klicken auf Fertig stellen das DDL-Skript nicht ausgeführt.
6.  Der Assistent zur Datenbankgenerierung bewirkt Folgendes: Öffnet die **EnumTest.edmx.sql** in T-SQL-Editor generiert Datei in den Store-Schemas und der Mapping-Abschnitten, die Zielimplementierung des zu Informationen zur Verbindungszeichenfolge fügt die Datei "App.config"
7.  Klicken Sie auf der rechten Maustaste im T-SQL-Editor, und wählen Sie **Execute** das Herstellen einer Verbindung mit Server-Dialogfeld angezeigt wird, geben Sie die Verbindungsinformationen aus Schritt 2, und klicken Sie auf **verbinden**
8.  Klicken Sie zum Anzeigen des generierten Schemas mit der rechten Maustaste auf den Datenbanknamen im Objekt-Explorer von SQL Server, und wählen Sie **aktualisieren**

## <a name="persist-and-retrieve-data"></a>Speichern und Abrufen von Daten

Öffnen Sie die Datei "Program.cs", die, in die Main-Methode definiert ist. Fügen Sie der Main-Funktion mit den folgenden Code. Der Code Fügt ein neues Objekt für die Abteilung, in den Kontext. Klicken Sie dann die Daten gespeichert. Der Code führt auch eine LINQ-Abfrage, die eine Abteilung zurückgibt, wobei der Name DepartmentNames.English ist.

``` csharp
using (var context = new EnumTestModelContainer())
{
    context.Departments.Add(new Department{ Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} and Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

Kompilieren Sie die Anwendung, und führen Sie sie aus. Das Programm erzeugt die folgende Ausgabe:

```
DepartmentID: 1 Name: English
```

Klicken Sie zum Anzeigen von Daten in der Datenbank, mit der rechten Maustaste auf den Datenbanknamen im Objekt-Explorer von SQL Server, und wählen Sie **aktualisieren**. Klicken Sie dann auf der rechten Maustaste auf die Tabelle, und wählen **Ansichtsdaten**.

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise erläutert, wie Enum-Typen, die mit dem Entity Framework Designer zuordnen und Enumerationen in Code zu verwenden. 
