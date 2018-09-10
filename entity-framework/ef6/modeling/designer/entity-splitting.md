---
title: Designer Entitätsaufteilung - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: aa2dd48a-1f0e-49dd-863d-d6b4f5834832
ms.openlocfilehash: 06199be977276cd3656e2550df79bac24276ec51
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250596"
---
# <a name="designer-entity-splitting"></a>Designer Entitätsaufteilung
In dieser exemplarischen Vorgehensweise wird gezeigt, wie einen Entitätstyp zwei Tabellen zugeordnet werden, durch Ändern eines Modells mit dem Entity Framework Designer (EF-Designer). Sie können eine Entität mehreren Tabellen zuordnen, sofern für die Tabellen ein gemeinsamer Schlüssel vorhanden ist. Die Prinzipien, nach denen ein Entitätstyp zwei Tabellen zugeordnet wird, lassen sich leicht auf das Mapping zu mehr als zwei Tabellen erweitern.

Die folgende Abbildung zeigt die wichtigsten Windows, die bei der Arbeit mit dem EF Designer verwendet werden.

![EF-Designer](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a>Erforderliche Komponenten

Visual Studio 2012 oder Visual Studio 2010, Ultimate, Premium, Professional oder Web Express Edition.

## <a name="create-the-database"></a>Erstellen Sie die Datenbank

Der Datenbankserver, der mit Visual Studio installiert ist, unterscheidet sich abhängig von der Version von Visual Studio, die Sie installiert haben:

-   Wenn Sie Visual Studio 2012 verwenden, wird Sie eine LocalDB-Datenbank erstellen.
-   Wenn Sie Visual Studio 2010 verwenden erstellen Sie eine SQL Express-Datenbank.

Zunächst erstellen wir eine Datenbank mit zwei Tabellen, die wir in einer einzelnen Entität kombinieren möchte.

-   Öffnen Sie Visual Studio.
-   **Ansicht – Profiler -&gt; Server-Explorer**
-   Klicken Sie mit der rechten Maustaste auf **Datenverbindungen -&gt; Verbindung hinzufügen...**
-   Wenn Sie vor dem müssen auswählen, im Server-Explorer mit einer Datenbank verbunden haben **Microsoft SQL Server** als Datenquelle
-   Eine Verbindung mit LocalDB oder SQL Express, je nachdem, welches Sie installiert haben
-   Geben Sie **EntitySplitting** als Datenbankname
-   Wählen Sie **OK** und Sie werden gefragt, ob Sie eine neue Datenbank, die auf erstellen möchten **Ja**
-   Die neue Datenbank wird jetzt im Server-Explorer angezeigt.
-   Bei Verwendung von Visual Studio 2012
    -   Mit der rechten Maustaste auf die Datenbank im Server-Explorer, und wählen Sie **neue Abfrage**
    -   Kopieren Sie die folgende SQL-Anweisung in die neue Abfrage, und klicken Sie dann mit der rechten Maustaste auf die Abfrage, und wählen **ausführen**
-   Wenn Sie Visual Studio 2010 verwenden
    -   Wählen Sie **Daten per Push –&gt; Transact-SQL-Editor –&gt; neue Abfrageverbindung...**
    -   Geben Sie **.\\ SQLEXPRESS** als Server ein, und klicken Sie auf **OK**
    -   Wählen Sie die **EntitySplitting** Datenbank aus der Dropdownliste unten am oberen Rand des Abfrage-Editors
    -   Kopieren Sie die folgende SQL-Anweisung in die neue Abfrage, und klicken Sie dann mit der rechten Maustaste auf die Abfrage, und wählen **SQL ausführen**

``` SQL
CREATE TABLE [dbo].[Person] (
[PersonId] INT IDENTITY (1, 1) NOT NULL,
[FirstName] NVARCHAR (200) NULL,
[LastName] NVARCHAR (200) NULL,
CONSTRAINT [PK_Person] PRIMARY KEY CLUSTERED ([PersonId] ASC)
);

CREATE TABLE [dbo].[PersonInfo] (
[PersonId] INT NOT NULL,
[Email] NVARCHAR (200) NULL,
[Phone] NVARCHAR (50) NULL,
CONSTRAINT [PK_PersonInfo] PRIMARY KEY CLUSTERED ([PersonId] ASC),
CONSTRAINT [FK_Person_PersonInfo] FOREIGN KEY ([PersonId]) REFERENCES [dbo].[Person] ([PersonId]) ON DELETE CASCADE
);
```

## <a name="create-the-project"></a>Erstellen des Projekts

-   Zeigen Sie im Menü **Datei** auf **Neu**, und klicken Sie dann auf **Projekt**.
-   Klicken Sie im linken Bereich auf **Visual C\#**, und wählen Sie dann die **Konsolenanwendung** Vorlage.
-   Geben Sie **MapEntityToTablesSample** als Namen für das Projekt und auf **OK**.
-   Klicken Sie auf **keine** bei Aufforderung zum Speichern der SQL-Abfrage, die im ersten Abschnitt erstellt haben.

## <a name="create-a-model-based-on-the-database"></a>Erstellen eines Modells auf Grundlage der Datenbank

-   Mit der rechten Maustaste des Projektnamen im Projektmappen-Explorer, zeigen Sie auf **hinzufügen**, und klicken Sie dann auf **neues Element**.
-   Wählen Sie **Daten** aus dem linken Menü und wählen Sie dann **ADO.NET Entity Data Model** im Bereich Vorlagen.
-   Geben Sie **MapEntityToTablesModel.edmx** für den Dateinamen ein, und klicken Sie dann auf **hinzufügen**.
-   Wählen Sie im Dialogfeld "Modellinhalte" **aus Datenbank generieren**, und klicken Sie dann auf **weiter.**
-   Wählen Sie die **EntitySplitting** Verbindung aus der Dropdownliste aus, und klicken Sie auf **Weiter**.
-   Klicken Sie im Dialogfeld "Datenbankobjekte auswählen" das Kontrollkästchen neben den **Tabellen** Knoten.
    Hiermit werden alle Tabellen aus der **EntitySplitting** Datenbank für das Modell.
-   Klicken Sie auf **Fertig stellen**.

Im Entity Designer, der bietet eine Entwurfsoberfläche zum Bearbeiten des Modells, wird angezeigt.

## <a name="map-an-entity-to-two-tables"></a>Zuordnen einer Entität zu zwei Tabellen

In diesem Schritt aktualisieren wir die **Person** Entitätstyp zum Kombinieren von Daten aus der **Person** und **PersonInfo** Tabellen.

-   Wählen Sie die **-e-Mail** und **Phone** Eigenschaften der ** PersonInfo ** Entität, und drücken Sie **STRG + X** Schlüssel.
-   Wählen Sie die ** Person ** Entität, und drücken Sie **STRG + V** Schlüssel.
-   Wählen Sie auf der Entwurfsoberfläche der **PersonInfo** Entität, und drücken Sie **löschen** Taste auf der Tastatur.
-   Klicken Sie auf **keine** Wenn gefragt, ob Sie entfernen möchten die **PersonInfo** Tabelle aus dem Modell werden etwa für die Zuordnung der **Person** Entität.

    ![Löschen von Tabellen](~/ef6/media/deletetables.png)

Die nächsten Schritte erfordern die **Mappingdetails** Fenster. Wenn Sie dieses Fenster nicht sehen, mit der rechten Maustaste Entwurfsoberfläche, und wählen **Mappingdetails**.

-   Wählen Sie die **Person** Entitätstyp, und klicken Sie auf **&lt;Hinzufügen einer Tabelle oder Sicht&gt;** in die **Mappingdetails** Fenster.
-   Wählen Sie ** PersonInfo ** aus der Dropdown-Liste.
    Die **Mappingdetails** Fenster wird mit standardspaltenzuordnungen aktualisiert, das für dieses Szenario in Ordnung sind.

Die **Person** Entitätstyp ist nun zugeordnet, um die **Person** und **PersonInfo** Tabellen.

![Zuordnen von 2](~/ef6/media/mapping2.png)

## <a name="use-the-model"></a>Verwenden Sie das Modell

-   Fügen Sie den folgenden Code in der Main-Methode.

``` csharp
    using (var context = new EntitySplittingEntities())
    {
        var person = new Person
        {
            FirstName = "John",
            LastName = "Doe",
            Email = "john@example.com",
            Phone = "555-555-5555"
        };

        context.People.Add(person);
        context.SaveChanges();

        foreach (var item in context.People)
        {
            Console.WriteLine(item.FirstName);
        }
    }
```

-   Kompilieren Sie die Anwendung, und führen Sie sie aus.

Die folgenden T-SQL-Anweisungen, die für die Datenbank durch Ausführen der Anwendung ausgeführt wurden. 

-   Die folgenden beiden **einfügen** Anweisungen ausgeführt wurden, als Ergebnis der Ausführung von Kontext. SaveChanges(). Dafür, dass die Daten aus der **Person** Entität und Aufteilung zwischen den **Person** und **PersonInfo** Tabellen.

    ![Legen Sie 1](~/ef6/media/insert1.png)

    ![Legen Sie 2](~/ef6/media/insert2.png)
-   Die folgenden **wählen** als Ergebnis Auflisten von Personen in der Datenbank ausgeführt wurde. Sie kombiniert die Daten aus der **Person** und **PersonInfo** Tabelle.

    ![Auswählen](~/ef6/media/select.png)
