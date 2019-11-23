---
title: Model First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
ms.openlocfilehash: 1b37805beb3d33f0b6dad2577a8abb3ea8f7b1e4
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182441"
---
# <a name="model-first"></a>Model First
Dieses Video und die schrittweise exemplarische Vorgehensweise bieten eine Einführung in die Model First Entwicklung mit Entity Framework. Mit Model First können Sie ein neues Modell mithilfe des Entity Framework Designer erstellen und dann ein Datenbankschema aus dem Modell generieren. Das Modell wird in einer EDMX-Datei (edmx-Erweiterung) gespeichert und kann in der Entity Framework Designer angezeigt und bearbeitet werden. Die Klassen, mit denen Sie in Ihrer Anwendung interagieren, werden automatisch aus der EDMX-Datei generiert.

## <a name="watch-the-video"></a>Video ansehen
Dieses Video und die schrittweise exemplarische Vorgehensweise bieten eine Einführung in die Model First Entwicklung mit Entity Framework. Mit Model First können Sie ein neues Modell mithilfe des Entity Framework Designer erstellen und dann ein Datenbankschema aus dem Modell generieren. Das Modell wird in einer EDMX-Datei (edmx-Erweiterung) gespeichert und kann in der Entity Framework Designer angezeigt und bearbeitet werden. Die Klassen, mit denen Sie in Ihrer Anwendung interagieren, werden automatisch aus der EDMX-Datei generiert.

**Präsentation:** [Rowan Miller](https://romiller.com/)

**Video**: [WMV](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (zip)](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)

## <a name="pre-requisites"></a>Voraussetzungen

Sie müssen Visual Studio 2010 oder Visual Studio 2012 installiert haben, um diese exemplarische Vorgehensweise abzuschließen.

Wenn Sie Visual Studio 2010 verwenden, müssen Sie auch [nuget](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installieren.

## <a name="1-create-the-application"></a>1. Erstellen Sie die Anwendung.

Um dies zu gewährleisten, erstellen wir eine einfache Konsolenanwendung, die die Model First für den Datenzugriff verwendet:

-   Öffnen Sie Visual Studio
-   **Datei&gt; Projekt für neue&gt;...**
-   Wählen Sie im Menü auf der linken Seite und **Konsolenanwendung** **Windows** aus.
-   Geben Sie **modelfirstsample** als Name ein.
-   Wählen Sie **OK** aus.

## <a name="2-create-model"></a>2. Modell erstellen

Wir verwenden Entity Framework Designer, die als Teil von Visual Studio enthalten ist, um das Modell zu erstellen.

-   **Projekt&gt; neues Element hinzufügen...**
-   Wählen Sie im linken Menü **Daten** aus, und klicken Sie dann auf **ADO.NET Entity Data Model**
-   Geben Sie **bloggingmodel** als Name ein, und klicken Sie auf **OK**. Dadurch wird der Entity Data Model-Assistent gestartet.
-   **Leeres Modell** auswählen und auf **Fertig** stellen klicken

    ![Leeres Modell erstellen](~/ef6/media/createemptymodel.png)

Die Entity Framework Designer wird mit einem leeren Modell geöffnet. Nun können Sie dem Modell Entitäten, Eigenschaften und Zuordnungen hinzufügen.

-   Klicken Sie mit der rechten Maustaste auf die Entwurfs Oberfläche, und wählen Sie **Eigenschaften**
-   In der Eigenschaftenfenster den **Namen des Entitäts Containers** in **bloggingcontext** ändern
    *dies der Name des abgeleiteten Kontexts, der für Sie generiert wird. der Kontext stellt eine Sitzung mit der Datenbank dar und ermöglicht es uns, Daten abzufragen und zu speichern* .
-   Klicken Sie mit der rechten Maustaste auf die Entwurfs Oberfläche, und wählen Sie **Add New-&gt; Entity...**
-   Geben **Sie** den Namen der Entität und die **BlogId** als Schlüssel Name ein, und klicken Sie auf **OK** .

    ![Blog Entität hinzufügen](~/ef6/media/addblogentity.png)

-   Klicken Sie in der Entwurfs Oberfläche mit der rechten Maustaste auf die neue Entität, und wählen Sie **neue&gt; skalare Eigenschaft hinzufügen**aus, und geben Sie **Name** als Namen für die Eigenschaft ein.
-   Wiederholen Sie diesen Vorgang, um eine **URL** -Eigenschaft hinzuzufügen.
-   Klicken Sie mit der rechten Maustaste auf die **URL** -Eigenschaft auf der Entwurfs Oberfläche, und wählen Sie **Eigenschaften**aus. **in der Eigenschaftenfenster** ändern Sie die Einstellung auf "true", die auf " **true** " festgelegt ist
    *dadurch können wir einen Blog in der Datenbank speichern, ohne*
-   Fügen Sie mit den Techniken, die Sie soeben gelernt haben, eine **Post** -Entität mit der Eigenschaft " **POSID** Key
-   Hinzufügen von **Titel** -und **inhaltsskalaren** Eigenschaften zur **Post** -Entität

Nachdem wir nun über mehrere Entitäten verfügen, ist es an der Zeit, eine Zuordnung (oder Beziehung) zwischen Ihnen hinzuzufügen.

-   Klicken Sie mit der rechten Maustaste auf die Entwurfs Oberfläche, und wählen Sie **Hinzufügen New-&gt; Association...**
-   Erstellen Sie ein Ende der **Beziehung mit einer** Multiplizität von **einem** und dem anderen Endpunkt, um ihn mit einer Multiplizität von **vielen**
    zu **veröffentlichen** . *Dies bedeutet, dass ein Blog viele Beiträge enthält und ein Beitrag zu einem Blog gehört* .
-   Stellen Sie sicher, dass das Feld **Fremdschlüssel Eigenschaften zu ' Post ' hinzufügen** aktiviert ist, und klicken Sie auf **OK**

    ![Association-MF hinzufügen](~/ef6/media/addassociationmf.png)

Wir verfügen jetzt über ein einfaches Modell, aus dem eine Datenbank generiert und zum Lesen und Schreiben von Daten verwendet werden kann.

![Modell anfänglich](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a>Weitere Schritte in Visual Studio 2010

Wenn Sie in Visual Studio 2010 arbeiten, müssen Sie einige zusätzliche Schritte ausführen, um ein Upgrade auf die neueste Version von Entity Framework durchzuführen. Das Upgrade ist wichtig, da Sie Ihnen Zugriff auf eine verbesserte API-Oberfläche, die viel einfacher zu verwenden ist, sowie auf die neuesten Fehlerbehebungen bietet.

Zuerst müssen wir die neueste Version von Entity Framework von nuget erhalten.

-   **Project –&gt; nuget-Pakete verwalten...** 
    *Wenn Sie nicht über die Option " **nuget-Pakete verwalten..** ." verfügen, sollten Sie die [neueste Version von nuget](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installieren.*
-   Auswählen der Registerkarte " **Online** "
-   Auswählen des " **EntityFramework** "-Pakets
-   Klicken Sie auf **Installieren**

Als nächstes müssen wir das Modell austauschen, um Code zu generieren, der die dbcontext-API nutzt, die in späteren Versionen von Entity Framework eingeführt wurde.

-   Klicken Sie mit der rechten Maustaste auf eine leere Stelle des Modells im EF-Designer, und wählen Sie **Code Generierungs Element hinzufügen... aus.**
-   Wählen Sie im linken Menü **Online Vorlagen** aus, und suchen Sie nach **dbcontext** .
-   Wählen Sie den EF **5. x dbcontext Generator für C\#aus** , geben Sie **bloggingmodel** als Name ein, und klicken Sie auf **Hinzufügen** .

    ![Dbcontext-Vorlage](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a>3. Erstellen der Datenbank

Mit unserem Modell können Entity Framework ein Datenbankschema berechnen, das es uns ermöglicht, Daten mit dem Modell zu speichern und abzurufen.

Der Datenbankserver, der mit Visual Studio installiert wird, unterscheidet sich abhängig von der installierten Version von Visual Studio:

-   Wenn Sie Visual Studio 2010 verwenden, erstellen Sie eine SQL Express-Datenbank.
-   Wenn Sie Visual Studio 2012 verwenden, erstellen Sie eine [localdb](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) -Datenbank.

Nun generieren wir die Datenbank.

-   Klicken Sie mit der rechten Maustaste auf die Entwurfs Oberfläche, und wählen Sie **Datenbank aus Modell generieren... aus.**
-   Klicken Sie auf **neue Verbindung...** und geben Sie entweder localdb oder SQL Express an, je nachdem, welche Version von Visual Studio Sie verwenden, geben Sie **modelfirst. blogals** Datenbanknamen ein.

    ![Localdb-Verbindungs-MF](~/ef6/media/localdbconnectionmf.png)

    ![SQL Express-Verbindung (MF)](~/ef6/media/sqlexpressconnectionmf.png)

-   Wählen Sie **OK** aus, und Sie werden gefragt, ob Sie eine neue Datenbank erstellen möchten, und wählen Sie **Ja** aus.
-   Wählen Sie **weiter** aus, und der Entity Framework Designer berechnet ein Skript, um das Datenbankschema zu erstellen.
-   Nachdem das Skript angezeigt wird, klicken Sie auf **Fertig** stellen, und das Skript wird dem Projekt hinzugefügt und geöffnet.
-   Klicken Sie mit der rechten Maustaste auf das Skript, und wählen Sie **Ausführen**aus. Sie werden aufgefordert, die Datenbank anzugeben, mit der eine Verbindung hergestellt werden soll, und localdb oder SQL Server Express anzugeben, abhängig von der verwendeten Version von Visual Studio.

## <a name="4-reading--writing-data"></a>4. Lesen & Schreiben von Daten

Nachdem wir nun über ein Modell verfügen, ist es an der Zeit, es für den Zugriff auf einige Daten zu verwenden. Die Klassen, die für den Zugriff auf Daten verwendet werden, werden automatisch für Sie basierend auf der EDMX-Datei generiert.

*Dieser Screenshot ist von Visual Studio 2012. Wenn Sie Visual Studio 2010 verwenden, befinden sich die Dateien BloggingModel.TT und BloggingModel.Context.tt direkt unter Ihrem Projekt, anstatt Sie in der EDMX-Datei zu untersuchen.*

![Generierte Klassen](~/ef6/media/generatedclasses.png)

Implementieren Sie die Main-Methode in Program.cs, wie unten gezeigt. Mit diesem Code wird eine neue Instanz des Kontexts erstellt und anschließend verwendet, um einen neuen Blog einzufügen. Anschließend wird eine LINQ-Abfrage verwendet, um alle Blogs aus der Datenbank alphabetisch nach Titel abzurufen.

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

Sie können die Anwendung jetzt ausführen und testen.

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```

## <a name="5-dealing-with-model-changes"></a>5. Umgang mit Modelländerungen

Nun ist es an der Zeit, einige Änderungen an unserem Modell vorzunehmen, wenn wir diese Änderungen vornehmen, müssen wir auch das Datenbankschema aktualisieren.

Wir beginnen mit dem Hinzufügen einer neuen Benutzer Entität zum Modell.

-   Fügen Sie einen neuen **Benutzer** Entitäts Namen mit **Benutzername** als Schlüssel Name und **Zeichenfolge** als Eigenschaftentyp für den Schlüssel hinzu.

    ![Benutzer Entität hinzufügen](~/ef6/media/adduserentity.png)

-   Klicken Sie mit der rechten Maustaste auf die **username** -Eigenschaft auf der Entwurfs Oberfläche, und wählen Sie **Eigenschaften**aus. in der Eigenschaftenfenster ändern Sie die **MaxLength** -Einstellung in **50**
    *damit werden die Daten, die in username gespeichert werden können, auf 50 Zeichen beschränkt*
-   Hinzufügen einer **DisplayName** -Skalareigenschaft zur **Benutzer** Entität

Wir haben jetzt ein aktualisiertes Modell, und wir können die Datenbank aktualisieren, um den neuen Benutzer Entitätstyp aufzunehmen.

-   Klicken Sie mit der rechten Maustaste auf die Entwurfs Oberfläche, und wählen Sie **Datenbank aus Modell generieren... aus**, Entity Framework ein Skript berechnen, um ein Schema basierend auf dem aktualisierten Modell neu zu erstellen.
-   Klicken auf **Fertig** stellen
-   Sie erhalten möglicherweise Warnungen zum Überschreiben des vorhandenen DDL-Skripts und die Zuordnung und Speicher Teile des Modells. Klicken Sie für beide Warnungen auf " **Ja** ".
-   Das aktualisierte SQL-Skript zum Erstellen der Datenbank wird für Sie geöffnet.  
    *Das Skript, das generiert wird, löscht alle vorhandenen Tabellen und erstellt das Schema neu. Dies funktioniert möglicherweise für die lokale Entwicklung, ist jedoch nicht für das Übertragen von Änderungen an eine Datenbank geeignet, die bereits bereitgestellt wurde. Wenn Sie Änderungen an einer Datenbank veröffentlichen müssen, die bereits bereitgestellt wurde, müssen Sie das Skript bearbeiten oder ein Schema Vergleichstool verwenden, um ein Migrations Skript zu berechnen.*
-   Klicken Sie mit der rechten Maustaste auf das Skript, und wählen Sie **Ausführen**aus. Sie werden aufgefordert, die Datenbank anzugeben, mit der eine Verbindung hergestellt werden soll, und localdb oder SQL Server Express anzugeben, abhängig von der verwendeten Version von Visual Studio.

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise haben wir uns mit Model First Entwicklung beschäftigt, mit der wir ein Modell im EF-Designer erstellen und dann eine Datenbank aus diesem Modell generieren konnten. Anschließend haben wir das Modell verwendet, um Daten aus der Datenbank zu lesen und zu schreiben. Schließlich haben wir das Modell aktualisiert und dann das Datenbankschema neu erstellt, um es mit dem Modell zu vergleichen.
