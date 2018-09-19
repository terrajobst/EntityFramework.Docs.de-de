---
title: Database First – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: cc6ffdb3-388d-4e79-a201-01ec2577c949
ms.openlocfilehash: c81025fe7c3ad6398f003f7be2a3f9f072eec327
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46284082"
---
# <a name="database-first"></a>Zunächst-Datenbank
Dieses video und schrittweise exemplarische Vorgehensweise bieten eine Einführung in die Verwendung von Entity Framework Database First-Entwicklung. Datenbank können Sie für das reverse Engineering zuerst ein Modell aus einer vorhandenen Datenbank. Das Modell kann im Entity Framework Designer wird in einer EDMX-Datei (EDMX-Erweiterung) gespeichert und angezeigt und bearbeitet werden. Die Klassen, die Sie in Ihrer Anwendung interagieren, werden automatisch aus der EDMX-Datei generiert.

## <a name="watch-the-video"></a>Video ansehen
Dieses Video bietet eine Einführung in die Verwendung von Entity Framework Database First-Entwicklung. Datenbank können Sie für das reverse Engineering zuerst ein Modell aus einer vorhandenen Datenbank. Das Modell kann im Entity Framework Designer wird in einer EDMX-Datei (EDMX-Erweiterung) gespeichert und angezeigt und bearbeitet werden. Die Klassen, die Sie in Ihrer Anwendung interagieren, werden automatisch aus der EDMX-Datei generiert.

**Präsentation:** [Rowan Miller](http://romiller.com/)

**Video**: [WMV](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)

## <a name="pre-requisites"></a>Voraussetzungen

Sie benötigen mindestens Visual Studio 2010 oder Visual Studio 2012 installiert werden, um diese exemplarische Vorgehensweise abgeschlossen haben.

Wenn Sie Visual Studio 2010 verwenden, müssen Sie auch haben [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installiert.

 

## <a name="1-create-an-existing-database"></a>1. Erstellen Sie eine vorhandene Datenbank

In der Regel müssen, wenn Sie eine vorhandene Datenbank verwenden möchten, die sie bereits erstellt wird, aber in dieser exemplarischen Vorgehensweise wir zum Erstellen einer Datenbank auf.

Der Datenbankserver, der mit Visual Studio installiert ist, unterscheidet sich abhängig von der Version von Visual Studio, die Sie installiert haben:

-   Wenn Sie Visual Studio 2010 verwenden erstellen Sie eine SQL Express-Datenbank.
-   Wenn Sie Visual Studio 2012 verwenden, und klicken Sie dann Sie erstellen eine [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) Datenbank.

 

Wir jetzt, und Erstellen der Datenbank.

-   Öffnen Sie Visual Studio.
-   **Ansicht – Profiler -&gt; Server-Explorer**
-   Klicken Sie mit der rechten Maustaste auf **Datenverbindungen -&gt; Verbindung hinzufügen...**
-   Wenn Sie vor dem müssen Sie Microsoft SQL Server als Datenquelle wählen im Server-Explorer mit einer Datenbank verbunden haben

    ![Auswählen einer Datenquelle](~/ef6/media/selectdatasource.png)

-   Eine Verbindung mit LocalDB oder SQL Express, je nachdem, welches Sie installiert haben, und geben Sie **DatabaseFirst.Blogging** als Datenbankname

    ![SQL Express-Verbindung DF](~/ef6/media/sqlexpressconnectiondf.png)

    ![LocalDB Verbindung DF](~/ef6/media/localdbconnectiondf.png)

-   Wählen Sie **OK** und Sie werden gefragt, ob Sie eine neue Datenbank, die auf erstellen möchten **Ja**

    ![Datenbank-Dialogfeld "erstellen"](~/ef6/media/createdatabasedialog.png)

-   Die neue Datenbank wird jetzt im Server-Explorer angezeigt. mit der rechten Maustaste darauf und wählen Sie **neue Abfrage**
-   Kopieren Sie die folgende SQL-Anweisung in die neue Abfrage, und klicken Sie dann mit der rechten Maustaste auf die Abfrage, und wählen **ausführen**

``` SQL
CREATE TABLE [dbo].[Blogs] (
    [BlogId] INT IDENTITY (1, 1) NOT NULL,
    [Name] NVARCHAR (200) NULL,
    [Url]  NVARCHAR (200) NULL,
    CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
);

CREATE TABLE [dbo].[Posts] (
    [PostId] INT IDENTITY (1, 1) NOT NULL,
    [Title] NVARCHAR (200) NULL,
    [Content] NTEXT NULL,
    [BlogId] INT NOT NULL,
    CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
    CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
);
```

## <a name="2-create-the-application"></a>2. Erstellen der Anwendung

Einfachheit halber werden wir eine einfache Konsolenanwendung zu erstellen, die das Database First verwendet, auf Daten zugreift:

-   Öffnen Sie Visual Studio.
-   **Datei -&gt; neu –&gt; Projekt...**
-   Wählen Sie **Windows** im linken Menü und **-Konsolenanwendung**
-   Geben Sie **DatabaseFirstSample** als Name
-   Wählen Sie **OK** aus.

 

## <a name="3-reverse-engineer-model"></a>3. Reverse Engineering-Modells

Wir werden die Verwendung von Entity Framework Designer, die als Teil von Visual Studio enthalten ist, um unser Modell zu erstellen.

-   **Projekt -&gt; neues Element hinzufügen...**
-   Wählen Sie **Daten** im linken Menü und dann **ADO.NET Entity Data Model**
-   Geben Sie **BloggingModel** als ein, und klicken Sie auf **OK**
-   Dadurch wird die **Entity Data Model-Assistenten**
-   Wählen Sie **aus Datenbank generieren** , und klicken Sie auf **weiter**

    ![Assistent, Schritt 1](~/ef6/media/wizardstep1.png)

-   Wählen Sie die Verbindung mit der Datenbank, die Sie im ersten Abschnitt erstellt haben, geben Sie **BloggingContext** als Namen für die Verbindungszeichenfolge und klicken Sie auf **weiter**

    ![Schritt 2](~/ef6/media/wizardstep2.png)

-   Klicken Sie auf das Kontrollkästchen neben "Tabellen", um alle Tabellen importiert werden, und klicken Sie auf "Fertig stellen"

    ![Schritt 3](~/ef6/media/wizardstep3.png)

 

Nach Abschluss der reverse-Engineering-Prozess ist das neue Modell dem Projekt hinzugefügt und eröffnet Ihnen im Entity Framework Designer angezeigt. Zu Ihrem Projekt mit den Verbindungsdetails für die Datenbank wurde auch eine Datei "App.config" hinzugefügt.

![Anfängliche modellieren](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a>Zusätzliche Schritte in Visual Studio 2010

Wenn Sie in Visual Studio 2010 arbeiten gibt es jedoch einige zusätzliche Schritte, die Sie befolgen, um auf die neueste Version von Entity Framework zu aktualisieren müssen. Es ist wichtig, ein Upgrade, da sie Zugriff auf eine verbesserte API-Oberfläche, die viel einfacher erhalten zu verwenden ist, sowie die neuesten Updates.

Zuerst muss, um die neueste Version von Entity Framework von NuGet zu erhalten.

-   **Projekt:&gt; NuGet-Pakete verwalten... ** 
     *Wenn Ihnen keine der **NuGet-Pakete verwalten... ** Option Sie installieren die [neueste Version von NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*
-   Wählen Sie die **Online** Registerkarte
-   Wählen Sie die **EntityFramework** Paket
-   Klicken Sie auf **installieren**

Als Nächstes müssen wir unser Modell zum Generieren von Code, mit der die DbContext-API verwenden, die in höheren Versionen von Entity Framework eingeführte, austauschen.

-   Mit der rechten Maustaste auf eine leere Stelle des Modells im EF Designer, und wählen Sie **Codegenerierungselement hinzufügen...**
-   Wählen Sie **Onlinevorlagen** aus dem linken Menü und die Suche nach **"DbContext"**
-   Wählen Sie die EF **5.x DbContext Generator für C\#**, geben Sie **BloggingModel** als ein, und klicken Sie auf **hinzufügen**

    !["DbContext"-Vorlage](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a>4. Lesen und Schreiben von Daten

Nun, da es sich um ein Modell verfügen, ist es Zeit, die sie verwenden, um einige Daten zuzugreifen. Die Klassen werden wir Sie verwenden, um den Zugriff auf Daten werden wird automatisch generiert basierend auf die EDMX-Datei.

*Dieser Screenshot stammt aus Visual Studio 2012, bei Verwendung von Visual Studio 2010 die BloggingModel.tt und BloggingModel.Context.tt Dateien werden direkt unterhalb des Projekts und nicht als geschachtelte unter der EDMX-Datei.*

![Generierte Klassen DF](~/ef6/media/generatedclassesdf.png)

 

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
 

## <a name="5-dealing-with-database-changes"></a>5. Umgang mit Änderungen in der Datenbank

Jetzt ist es Zeit für unsere Datenbankschema, einige Änderungen vornehmen, wenn wir diese Änderungen vornehmen, die wir auch unser Modell entsprechend aktualisieren müssen.

Der erste Schritt ist, einige Änderungen am Datenbankschema vornehmen. Wir werden das Schema einer Benutzertabelle hinzu.

-   Mit der rechten Maustaste auf die **DatabaseFirst.Blogging** Datenbank im Server-Explorer, und wählen Sie **neue Abfrage**
-   Kopieren Sie die folgende SQL-Anweisung in die neue Abfrage, und klicken Sie dann mit der rechten Maustaste auf die Abfrage, und wählen **ausführen**

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

Nachdem das Schema aktualisiert wird, ist es Zeit, um das Modell mit diesen Änderungen aktualisieren.

-   Mit der rechten Maustaste auf eine leere Stelle Ihres Modells im EF Designer und wählen "Updatemodell aus der Datenbank...", dadurch wird die Update-Assistenten
-   Auf der Registerkarte für das Hinzufügen der Update-Assistenten aktivieren Sie das Kontrollkästchen neben den Tabellen gibt dies an, dass alle neuen Tabellen aus dem Schema hinzugefügt werden soll.
    *Die Registerkarte "Aktualisieren" zeigt alle bestehenden Tabellen im Modell, das während des Updates auf Änderungen überprüft werden. Die Delete-Registerkarten zeigen alle Tabellen, die aus dem Schema entfernt wurden, und werden ebenfalls aus dem Modell im Rahmen des Updates entfernt werden. Die Informationen zu diesen beiden Registerkarten wird automatisch erkannt und dient nur zu Informationszwecken können nicht Sie keine Einstellungen geändert.*

    ![Assistent zum Aktualisieren](~/ef6/media/refreshwizard.png)

-   Klicken Sie auf "Fertig stellen", die Update-Assistenten

 

Das Modell wurde aktualisiert, um eine neue Benutzerentität enthalten, die der Tabelle zugeordnet, die wir in der Datenbank hinzugefügt.

![Modell aktualisiert](~/ef6/media/modelupdated.png)

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise, die wir uns Database First-Entwicklung angesehen haben, hatten die wir ein Modell im EF Designer basierend auf einer vorhandenen Datenbank zu erstellen. Klicken Sie dann verwendet das Modell lesen und Schreiben von Daten aus der Datenbank. Abschließend aktualisiert wir das Modell, um die Änderungen, die wir am Datenbankschema vorgenommen.
