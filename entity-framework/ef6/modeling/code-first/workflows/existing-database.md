---
title: Code First mit einer vorhandenen Datenbank - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: a7e60b74-973d-4480-868f-500a3899932e
ms.openlocfilehash: 29f959265e0fd0d5e14c156519e6931fd8da0677
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995089"
---
# <a name="code-first-to-an-existing-database"></a>Code First mit einer vorhandenen Datenbank
Dieses video und schrittweise exemplarische Vorgehensweise bieten eine Einführung in die Code First-Entwicklung für eine vorhandene Datenbank. Code können Sie Ihr Modell mithilfe von C definieren zuerst\# oder VB.Net-Klassen. Optional zusätzliche Konfiguration kann mithilfe von Attributen auf Klassen und Eigenschaften oder mithilfe einer fluent-API ausgeführt werden.

## <a name="watch-the-video"></a>Video ansehen
Dieses Video ist [jetzt verfügbar auf Channel 9](http://channel9.msdn.com/blogs/ef/code-first-to-existing-database-ef6-1-onwards-).

## <a name="pre-requisites"></a>Voraussetzungen

Sie müssen **Visual Studio 2012** oder **Visual Studio 2013** zum Durchführen dieser exemplarischen Vorgehensweise installiert.

Sie benötigen außerdem die Version **6.1** (oder höher) von der **Entity Framework-Tools für Visual Studio** installiert. Finden Sie unter [Entity Framework erhalten](~/ef6/fundamentals/install.md) Informationen zum Installieren der neuesten Version von Entity Framework-Tools.

## <a name="1-create-an-existing-database"></a>1. Erstellen Sie eine vorhandene Datenbank

In der Regel müssen, wenn Sie eine vorhandene Datenbank verwenden möchten, die sie bereits erstellt wird, aber in dieser exemplarischen Vorgehensweise wir zum Erstellen einer Datenbank auf.

Wir jetzt, und Erstellen der Datenbank.

-   Öffnen Sie Visual Studio.
-   **Ansicht – Profiler -&gt; Server-Explorer**
-   Klicken Sie mit der rechten Maustaste auf **Datenverbindungen -&gt; Verbindung hinzufügen...**
-   Wenn Sie mit einer Datenbank aus verbunden haben **Server-Explorer** , bevor Sie auswählen müssen, **Microsoft SQL Server** als Datenquelle

    ![SelectDataSource](~/ef6/media/selectdatasource.png)

-   Verbinden mit der LocalDB-Instanz, und geben Sie **Blogging** als Datenbankname

    ![LocalDBConnection](~/ef6/media/localdbconnection.png)

-   Wählen Sie **OK** und Sie werden gefragt, ob Sie eine neue Datenbank, die auf erstellen möchten **Ja**

    ![CreateDatabaseDialog](~/ef6/media/createdatabasedialog.png)

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

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('The Visual Studio Blog', 'http://blogs.msdn.com/visualstudio/')

INSERT INTO [dbo].[Blogs] ([Name],[Url])
VALUES ('.NET Framework Blog', 'http://blogs.msdn.com/dotnet/')
```

## <a name="2-create-the-application"></a>2. Erstellen der Anwendung

Einfachheit halber werden wir eine einfache Konsolenanwendung zu erstellen, die Code First verwendet, auf Daten zugreift:

-   Öffnen Sie Visual Studio.
-   **Datei -&gt; neu –&gt; Projekt...**
-   Wählen Sie **Windows** im linken Menü und **-Konsolenanwendung**
-   Geben Sie **CodeFirstExistingDatabaseSample** als Name
-   Wählen Sie **OK** aus.

 

## <a name="3-reverse-engineer-model"></a>3. Reverse Engineering-Modells

Wir werden zu der die Entity Framework-Tools für Visual Studio verwenden, um uns die anfänglichen Code in der Datenbank zuordnen zu generieren. Diese Tools sind nur Code generiert, die Sie auch per hand eingeben könnten, falls gewünscht.

-   **Projekt -&gt; neues Element hinzufügen...**
-   Wählen Sie **Daten** im linken Menü und dann **ADO.NET Entity Data Model**
-   Geben Sie **BloggingContext** als ein, und klicken Sie auf **OK**
-   Dadurch wird die **Entity Data Model-Assistenten**
-   Wählen Sie **Code First aus Datenbank** , und klicken Sie auf **weiter**

    ![WizardOneCFE](~/ef6/media/wizardonecfe.png)

-   Wählen Sie die Verbindung mit der Datenbank, die Sie im ersten Abschnitt erstellt haben, und klicken Sie auf **weiter**

    ![WizardTwoCFE](~/ef6/media/wizardtwocfe.png)

-   Klicken Sie auf das Kontrollkästchen neben **Tabellen** importieren Sie alle Tabellen aus, und klicken Sie auf **Fertig stellen**

    ![WizardThreeCFE](~/ef6/media/wizardthreecfe.png)

Sobald der reverse Engineering-Prozess auf eine Anzahl von Elementen abgeschlossen ist wird wurde das Projekt, wir sehen Sie sich, was hinzugefügt wurde.

### <a name="configuration-file"></a>Konfigurationsdatei

Eine App.config-Datei wurde dem Projekt diese Datei die Verbindungszeichenfolge für die vorhandene Datenbank enthält.

``` xml
<connectionStrings>
  <add  
    name="BloggingContext"  
    connectionString="data source=(localdb)\mssqllocaldb;initial catalog=Blogging;integrated security=True;MultipleActiveResultSets=True;App=EntityFramework"  
    providerName="System.Data.SqlClient" />
</connectionStrings>
```

*Sehen Sie einige andere Einstellungen in der Konfigurationsdatei zu, Hierbei handelt es sich um EF-Standardeinstellungen, die Code First mitzuteilen, wohin Datenbanken erstellt werden soll. Da wir zu einer vorhandenen Datenbank werden diese Einstellungen ignoriert in unserer Anwendung zugeordnet sind.*

### <a name="derived-context"></a>Abgeleiteten Kontext

Ein **BloggingContext** Klasse dem Projekt hinzugefügt wurde. Der Kontext stellt eine Sitzung mit der Datenbank, die es uns ermöglicht, Abfragen und Speichern von Daten dar.
Stellt der Kontext einer **"DbSet"&lt;TEntity&gt;**  für jeden Typ in unserem Modell. Sie werden bemerken, dass der Standardkonstruktor einer Basiskonstruktor mit Ruft die **Name =** Syntax. Dies weist Code First, dass die Verbindungszeichenfolge für diesen Kontext verwenden, die aus der Konfigurationsdatei geladen werden soll.

``` csharp
public partial class BloggingContext : DbContext
    {
        public BloggingContext()
            : base("name=BloggingContext")
        {
        }

        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
        }
    }
```

*Verwenden Sie immer die **Name =** Syntax, wenn Sie eine Verbindungszeichenfolge in der Config-Datei verwenden. Dadurch wird sichergestellt, dass wenn die Verbindungszeichenfolge nicht vorhanden ist. Klicken Sie dann auf Entity Framework ausgelöst wird anstatt eine neue Datenbank gemäß der Konvention zu erstellen.*

### <a name="model-classes"></a>ViewModel-Klassen

Zum Schluss eine **Blog** und **Post** Klasse wurden ebenfalls dem Projekt hinzugefügt. Hierbei handelt es sich um Domänenklassen, die unser Modell bilden. Sehen Sie Datenanmerkungen auf Klassen angewendet, an der Konfiguration, würde der Code First-Konventionen nicht die Struktur der vorhandenen Datenbank ausgerichtet. Beispielsweise sehen Sie die **StringLength** Anmerkung zu **Blog.Name** und **Blog.Url** , da sie eine Höchstlänge von haben **200** in der Datenbank (der Code First standardmäßig wird die maximale Länge vom Datenbankanbieter - unterstützt verwendet **nvarchar(max)** in SQL Server).

``` csharp
public partial class Blog
{
    public Blog()
    {
        Posts = new HashSet<Post>();
    }

    public int BlogId { get; set; }

    [StringLength(200)]
    public string Name { get; set; }

    [StringLength(200)]
    public string Url { get; set; }

    public virtual ICollection<Post> Posts { get; set; }
}
```

## <a name="4-reading--writing-data"></a>4. Lesen und Schreiben von Daten

Nun, da es sich um ein Modell verfügen, ist es Zeit, die sie verwenden, um einige Daten zuzugreifen. Implementieren der **Main** -Methode in der **"Program.cs"** wie unten dargestellt. Dieser Code erstellt eine neue Instanz unseres Kontexts und anschließend verwendet, um eine neue einfügen **Blog**. Anschließend er eine LINQ-Abfrage, verwendet um alle abzurufen **Blogs** aus der Datenbank, die alphabetisch nach **Titel**.

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
.NET Framework Blog
ADO.NET Blog
The Visual Studio Blog
Press any key to exit...
```
 
## <a name="what-if-my-database-changes"></a>Was geschieht, wenn meine Datenbank geändert wird?

Der Code First, Datenbank-Assistent dient zum Starten zeigen einige Klassen zu generieren, die Sie anschließend anpassen und ändern können. Wenn am Datenbankschema Änderungen können Sie entweder manuell bearbeiten die Klassen oder führen Sie einen anderen reverse Engineering, um die Klassen zu überschreiben.

## <a name="using-code-first-migrations-to-an-existing-database"></a>Mithilfe von Code First-Migrationen zu einer vorhandenen Datenbank

Wenn Sie Code First-Migrationen mit einer vorhandenen Datenbank verwenden möchten, finden Sie unter [Code First-Migrationen zu einer vorhandenen Datenbank](~/ef6/modeling/code-first/migrations/existing-database.md).

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise haben Sie Code First-Entwicklung mithilfe einer vorhandenen Datenbank. Verwendet die Entity Framework-Tools für Visual Studio für das reverse Engineering einer Reihe von Klassen, die in der Datenbank zugeordnet und kann zum Speichern und Abrufen von Daten verwendet werden.
