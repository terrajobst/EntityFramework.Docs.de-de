---
title: Model First – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
ms.openlocfilehash: 8e010f95db40261073b4af80a3c0e3225a2cd1cf
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490479"
---
# <a name="model-first"></a>Model First
Dieses video und schrittweise exemplarische Vorgehensweise bieten eine Einführung in die Verwendung von Entity Framework Model First-Entwicklung. Modell zuerst können Sie ein neues Modell mit dem Entity Framework Designer erstellen und anschließend ein Datenbankschema aus dem Modell generieren. Das Modell kann im Entity Framework Designer wird in einer EDMX-Datei (EDMX-Erweiterung) gespeichert und angezeigt und bearbeitet werden. Die Klassen, die Sie in Ihrer Anwendung interagieren, werden automatisch aus der EDMX-Datei generiert.

## <a name="watch-the-video"></a>Video ansehen
Dieses video und schrittweise exemplarische Vorgehensweise bieten eine Einführung in die Verwendung von Entity Framework Model First-Entwicklung. Modell zuerst können Sie ein neues Modell mit dem Entity Framework Designer erstellen und anschließend ein Datenbankschema aus dem Modell generieren. Das Modell kann im Entity Framework Designer wird in einer EDMX-Datei (EDMX-Erweiterung) gespeichert und angezeigt und bearbeitet werden. Die Klassen, die Sie in Ihrer Anwendung interagieren, werden automatisch aus der EDMX-Datei generiert.

**Präsentation:** [Rowan Miller](http://romiller.com/)

**Video**: [WMV](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)

## <a name="pre-requisites"></a>Voraussetzungen

Sie benötigen Visual Studio 2010 oder Visual Studio 2012 installiert werden, um diese exemplarische Vorgehensweise abgeschlossen haben.

Wenn Sie Visual Studio 2010 verwenden, müssen Sie auch haben [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installiert.

## <a name="1-create-the-application"></a>1. Erstellen der Anwendung

Einfachheit halber werden wir eine einfache Konsolenanwendung zu erstellen, die den Model First verwendet, auf Daten zugreift:

-   Öffnen Sie Visual Studio.
-   **Datei -&gt; neu –&gt; Projekt...**
-   Wählen Sie **Windows** im linken Menü und **-Konsolenanwendung**
-   Geben Sie **ModelFirstSample** als Name
-   Wählen Sie **OK** aus.

## <a name="2-create-model"></a>2. Modell erstellen

Wir werden die Verwendung von Entity Framework Designer, die als Teil von Visual Studio enthalten ist, um unser Modell zu erstellen.

-   **Projekt -&gt; neues Element hinzufügen...**
-   Wählen Sie **Daten** im linken Menü und dann **ADO.NET Entity Data Model**
-   Geben Sie **BloggingModel** als ein, und klicken Sie auf **OK**, dadurch wird der Assistent für Entity Data Model
-   Wählen Sie **leeres Modell** , und klicken Sie auf **Fertig stellen**

    ![Leeres Modell erstellen](~/ef6/media/createemptymodel.png)

Entity Framework Designer wird mit einem leeren Modell geöffnet. Jetzt können wir beginnen, dem Modell Entitäten, Eigenschaften und Zuordnungen hinzufügen.

-   Mit der rechten Maustaste auf die Entwurfsoberfläche, und wählen **Eigenschaften**
-   Die Änderung der Eigenschaften im Fenster der **Entitätencontainername** zu **BloggingContext**
    *Dies ist der Name des abgeleiteten Kontexts, die für Sie, den Kontext generiert werden repräsentiert eine Sitzung mit der Datenbank, die es uns ermöglicht, Abfragen und Speichern von Daten*
-   Mit der rechten Maustaste auf die Entwurfsoberfläche, und wählen **Add New -&gt; Entität...**
-   Geben Sie **Blog** als den Namen der Entität und **BlogId** als Schlüssel ein, und klicken Sie auf **OK**

    ![Blog-Entität hinzufügen](~/ef6/media/addblogentity.png)

-   Mit der rechten Maustaste auf die neue Entität, auf die Entwurfsoberfläche, und wählen **Add New -&gt; Skalareigenschaft**, geben Sie **Namen** als Name der Eigenschaft.
-   Wiederholen Sie diesen Vorgang zum Hinzufügen einer **Url** Eigenschaft.
-   Mit der rechten Maustaste auf die **Url** Eigenschaft auf die Entwurfsoberfläche, und wählen **Eigenschaften**, in das Ändern der Eigenschaften im Fenster der **Nullable** auf **"true"** 
     *Dadurch können wir einen Blog in der Datenbank zu speichern, ohne dass sie eine Url zugewiesen*
-   Mit den Verfahren, die Sie gerade gelernt haben, fügen einen **Post** Entität mit einem **Beitrags** Schlüsseleigenschaft
-   Hinzufügen **Titel** und **Content** skalare Eigenschaften der **Post** Entität

Nun, da wir eine Reihe von Entitäten haben, ist es Zeit zum Hinzufügen einer Zuordnung (oder einer Beziehung) zwischen ihnen.

-   Mit der rechten Maustaste auf die Entwurfsoberfläche, und wählen **Add New -&gt; Zuordnung...**
-   Stellen Sie ein Ende der Beziehung auf verweisen **Blog** mit einer Multiplizität von **eine** und der andere Endpunkt zu **Post** mit einer Multiplizität von **viele** 
     *Dies bedeutet, dass ein Blog viele Beiträge hat und ein Blog ein Posting gehört*
-   Stellen Sie sicher die **Fremdschlüsseleigenschaften auf "Post" Entität hinzufügen** wird überprüft, und klicken Sie auf **OK**

    ![Zuordnung MF hinzufügen](~/ef6/media/addassociationmf.png)

Wir haben nun ein einfaches Modell, das wir Generieren einer Datenbank aus und zum Lesen und Schreiben von Daten verwenden können.

![Anfängliche modellieren](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a>Zusätzliche Schritte in Visual Studio 2010

Wenn Sie in Visual Studio 2010 arbeiten gibt es jedoch einige zusätzliche Schritte, die Sie befolgen, um auf die neueste Version von Entity Framework zu aktualisieren müssen. Es ist wichtig, ein Upgrade, da sie Zugriff auf eine verbesserte API-Oberfläche, die viel einfacher erhalten zu verwenden ist, sowie die neuesten Updates.

Zuerst muss, um die neueste Version von Entity Framework von NuGet zu erhalten.

-   **Projekt:&gt; NuGet-Pakete verwalten... ** 
     *Wenn Ihnen keine der **NuGet-Pakete verwalten... ** Option Sie installieren die [neueste Version von NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*
-   Wählen Sie die **Online** Registerkarte
-   Wählen Sie die **EntityFramework** Paket
-   Klicken Sie auf **installieren**

Als Nächstes müssen wir unser Modell zum Generieren von Code, mit der die DbContext-API verwenden, die in höheren Versionen von Entity Framework eingeführte, austauschen.

-   Mit der rechten Maustaste auf eine leere Stelle des Modells im EF Designer, und wählen Sie **Codegenerierungselement hinzufügen...**
-   Wählen Sie **Onlinevorlagen** aus dem linken Menü und die Suche nach **"DbContext"**
-   Wählen Sie die EF **5.x DbContext Generator für C\#**, geben Sie **BloggingModel** als ein, und klicken Sie auf **hinzufügen**

    !["DbContext"-Vorlage](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a>3. Der datenbankgenerierung

Wenn unser Modell ist, können Entity Framework ein Datenbankschema berechnen, die es ermöglichen, speichern und Abrufen von Daten mithilfe des Datenmodells.

Der Datenbankserver, der mit Visual Studio installiert ist, unterscheidet sich abhängig von der Version von Visual Studio, die Sie installiert haben:

-   Wenn Sie Visual Studio 2010 verwenden erstellen Sie eine SQL Express-Datenbank.
-   Wenn Sie Visual Studio 2012 verwenden, und klicken Sie dann Sie erstellen eine [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) Datenbank.

Wir jetzt, und Erstellen der Datenbank.

-   Mit der rechten Maustaste auf die Entwurfsoberfläche, und wählen **Datenbank aus Modell generieren...**
-   Klicken Sie auf **neue Verbindung...** und geben Sie entweder LocalDB oder SQL Express, je nachdem, welche Version von Visual Studio Sie verwenden, geben Sie **ModelFirst.Blogging** als Datenbankname verwendet.

    ![LocalDB Verbindung MF](~/ef6/media/localdbconnectionmf.png)

    ![SQL Express-Verbindung MF](~/ef6/media/sqlexpressconnectionmf.png)

-   Wählen Sie **OK** und Sie werden gefragt, ob Sie eine neue Datenbank, die auf erstellen möchten **Ja**
-   Wählen Sie **Weiter** und Entity Framework Designer berechnet ein Skript, um das Datenbankschema zu erstellen.
-   Nachdem das Skript angezeigt wird, klicken Sie auf **Fertig stellen** und das Skript zu Ihrem Projekt hinzugefügt und geöffnet werden
-   Mit der rechten Maustaste auf das Skript, und wählen Sie **Execute**, werden Sie aufgefordert, geben Sie die Datenbank eine Verbindung herstellen, geben Sie LocalDB oder SQLServer Express, abhängig von der Version von Visual Studio Sie verwenden

## <a name="4-reading--writing-data"></a>4. Lesen und Schreiben von Daten

Nun, da es sich um ein Modell verfügen, ist es Zeit, die sie verwenden, um einige Daten zuzugreifen. Die Klassen werden wir Sie verwenden, um den Zugriff auf Daten werden wird automatisch generiert basierend auf die EDMX-Datei.

*Dieser Screenshot stammt aus Visual Studio 2012, bei Verwendung von Visual Studio 2010 die BloggingModel.tt und BloggingModel.Context.tt Dateien werden direkt unterhalb des Projekts und nicht als geschachtelte unter der EDMX-Datei.*

![Generierte Klassen](~/ef6/media/generatedclasses.png)

Implementieren Sie die Main-Methode in "Program.cs" ein, wie unten dargestellt. Dieser Code erstellt eine neue Instanz unseres Kontexts und anschließend verwendet, um einen neuen Blog einfügen. Anschließend wird eine LINQ-Abfrage zum Abrufen aller Blogs aus der Datenbank nach Titel alphabetisch angeordnet sind.

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

Sie können jetzt die Anwendung auszuführen, und probieren Sie es aus.

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```

## <a name="5-dealing-with-model-changes"></a>5. Umgang mit Änderungen des Datenmodells

Jetzt ist es Zeit, einige Änderungen auf unserem Modell vornehmen, wenn wir diese Änderungen vornehmen, die wir auch das Datenbankschema zu aktualisieren müssen.

Wir beginnen, durch das Hinzufügen einer neuen Benutzerentität auf unserem Modell.

-   Fügen Sie einen neuen **Benutzer** Entitätsname mit **Benutzername** des Schlüsselnamens und **Zeichenfolge** weil der Eigenschaftentyp, für den Schlüssel

    ![Benutzerentität "" hinzufügen](~/ef6/media/adduserentity.png)

-   Mit der rechten Maustaste auf die **Benutzername** Eigenschaft auf die Entwurfsoberfläche, und wählen **Eigenschaften**, Eigenschaften In der Fenster Ändern der **MaxLength** auf **50 ** 
     *Dies schränkt die Daten, die im Benutzernamen auf 50 Zeichen gespeichert werden können*
-   Hinzufügen einer **"DisplayName"** skalare Eigenschaft, um die **Benutzer** Entität

Wir verfügen jetzt über einen aktualisierten Modell, und wir können zum Aktualisieren der Datenbank, um unsere neuen Entitätstyp für den Benutzer zu ermöglichen.

-   Mit der rechten Maustaste auf die Entwurfsoberfläche, und wählen **Datenbank aus Modell generieren...** , Entity Framework ein Skript zum Erstellen Sie ein Schema basierend auf dem aktualisierten Modell neu berechnet.
-   Klicken Sie auf **Fertig stellen**
-   Sie können Warnungen zum Überschreiben der vorhandenen DDL-Skript und die Zuordnung und Speicher Teile des Modells angezeigt, klicken Sie auf **Ja** für beide diese Warnungen
-   Das aktualisierte SQL-Skript zum Erstellen der Datenbank ist für Sie geöffnet.  
    *Das Skript, das generiert wird, werden alle vorhandene Tabellen löschen und erstellen Sie das Schema von Grund auf neu. Dies funktioniert für die lokale Entwicklung ist jedoch kein gültiges pushübermittlung von Änderungen in einer Datenbank, die bereits bereitgestellt wurde. Wenn Sie müssen Änderungen in einer Datenbank zu veröffentlichen, die bereits bereitgestellt wurde, müssen Sie bearbeiten Sie das Skript aus, oder verwenden ein Schema vergleichen-Tool, um ein Migrationsskript zu berechnen.*
-   Mit der rechten Maustaste auf das Skript, und wählen Sie **Execute**, werden Sie aufgefordert, geben Sie die Datenbank eine Verbindung herstellen, geben Sie LocalDB oder SQLServer Express, abhängig von der Version von Visual Studio Sie verwenden

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise, die wir uns Model First-Entwicklung angesehen haben, hatten die wir ein Modell im EF Designer erstellen und anschließend eine Datenbank von diesem Modell generieren. Klicken Sie dann verwendet das Modell lesen und Schreiben von Daten aus der Datenbank. Abschließend das Modell aktualisiert und dann neu erstellt das Datenbankschema entsprechend das Modell.
