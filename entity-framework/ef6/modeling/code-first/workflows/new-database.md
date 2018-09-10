---
title: Code First für eine neue Datenbank – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 2df6cb0a-7d8b-4e28-9d05-e2b9a90125af
ms.openlocfilehash: 8ed1bfbc3536acc0d83b9c8ecdd180aeb44eff83
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251049"
---
# <a name="code-first-to-a-new-database"></a>Code First für eine neue Datenbank
Dieses video und schrittweise exemplarische Vorgehensweise bieten eine Einführung in die Code First-Entwicklung für eine neue Datenbank. Dieses Szenario umfasst das Ziel einer Datenbank, die nicht vorhanden ist und Code First erstellt oder eine leere Datenbank, Code First werden neue Tabellen hinzufügen. Code können Sie Ihr Modell mithilfe von C definieren zuerst\# oder VB.Net-Klassen. Verwenden von Attributen auf Klassen und Eigenschaften oder mithilfe einer fluent-API kann optional zusätzliche Konfigurationsschritte ausgeführt werden.

## <a name="watch-the-video"></a>Video ansehen
Dieses Video bietet eine Einführung in die Code First-Entwicklung für eine neue Datenbank. Dieses Szenario umfasst das Ziel einer Datenbank, die nicht vorhanden ist und Code First erstellt oder eine leere Datenbank, Code First werden neue Tabellen hinzufügen. Code zuerst können Sie Ihr Modell mithilfe von c# oder VB.NET. Klassen definieren. Verwenden von Attributen auf Klassen und Eigenschaften oder mithilfe einer fluent-API kann optional zusätzliche Konfigurationsschritte ausgeführt werden.

**Präsentation:** [Rowan Miller](http://romiller.com/)

**Video**: [WMV](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.wmv) | [MP4](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-mp4Video-CodeFirstNewDatabase.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/B/A/5/BA57BADE-D558-4693-8F82-29E64E4084AB/HDI-ITPro-MSDN-winvideo-CodeFirstNewDatabase.zip)

# <a name="pre-requisites"></a>Voraussetzungen

Sie benötigen mindestens Visual Studio 2010 oder Visual Studio 2012 installiert werden, um diese exemplarische Vorgehensweise abgeschlossen haben.

Wenn Sie Visual Studio 2010 verwenden, müssen Sie auch haben [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installiert.

## <a name="1-create-the-application"></a>1. Erstellen der Anwendung

Einfachheit halber werden wir eine einfache Konsolenanwendung zu erstellen, die Code First verwendet, um die Daten zugreift.

-   Öffnen Sie Visual Studio.
-   **Datei -&gt; neu –&gt; Projekt...**
-   Wählen Sie **Windows** im linken Menü und **-Konsolenanwendung**
-   Geben Sie **CodeFirstNewDatabaseSample** als Name
-   Wählen Sie **OK** aus.

## <a name="2-create-the-model"></a>2. Erstellen des Modells

Definieren Sie ein sehr einfaches Modell, die mithilfe von Klassen. Wir definieren einfach sie in der Datei "Program.cs" jedoch in einer realen Anwendung, die Sie Ihre Klassen sich in separaten Dateien und ggf. ein separates Projekt Teilen würde.

Fügen Sie unterhalb der Definition der Programmklasse in "Program.cs" die folgenden zwei Klassen ein.

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }

    public virtual List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; }
    public virtual Blog Blog { get; set; }
}
```

Sie werden feststellen, dass wir die zwei Navigationseigenschaften (Blog.Posts und Post.Blog) virtuellen vornehmen. Dies ermöglicht die Lazy Loading-Funktion von Entity Framework. Lazy Loading bedeutet, dass der Inhalt dieser Eigenschaften automatisch aus der Datenbank geladen werden, wenn Sie versuchen, darauf zuzugreifen.

## <a name="3-create-a-context"></a>3. Erstellen Sie einen Kontext

Jetzt ist es Zeit, einen abgeleiteten Kontext, steht für eine Sitzung mit der Datenbank, die es uns ermöglicht, Abfragen und Speichern von Daten zu definieren. Wir definieren einen Kontext an, die von System.Data.Entity.DbContext abgeleitet und stellt einen typisierten "DbSet"&lt;TEntity&gt; für jede Klasse in unserem Modell.

Beginnen wir nun auf die Typen aus dem Entity Framework verwenden, daher wir das EntityFramework NuGet-Paket hinzufügen müssen.

-   **Projekt:&gt; NuGet-Pakete verwalten...**
    Hinweis: Wenn Sie nicht über verfügen den **NuGet-Pakete verwalten...** Option Sie installieren die [neueste Version von NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)
-   Wählen Sie die **Online** Registerkarte
-   Wählen Sie die **EntityFramework** Paket
-   Klicken Sie auf **installieren**

Hinzufügen einer using-Anweisung für System.Data.Entity am Anfang der Datei "Program.cs".

``` csharp
using System.Data.Entity;
```

Fügen Sie unterhalb der Post-Klasse in "Program.cs" die folgenden abgeleiteten Kontext aus.

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

Hier ist eine vollständige Liste der was "Program.cs" jetzt enthalten soll.

``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Data.Entity;

namespace CodeFirstNewDatabaseSample
{
    class Program
    {
        static void Main(string[] args)
        {
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }

        public virtual List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }

    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
    }
}
```

Das ist der Code zu speichern und Abrufen von Daten benötigten. Natürlich gibt es einiges hinter den Kulissen passiert ist, und wir werden sehen Sie sich, dass in Kürze jedoch lassen Sie uns in Aktion sehen.

## <a name="4-reading--writing-data"></a>4. Lesen und Schreiben von Daten

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
### <a name="wheres-my-data"></a>Wo sind meine Daten?

Gemäß der Konvention erstellt "DbContext" eine Datenbank für Sie.

-   Wenn eine lokale Instanz von SQL Express verfügbar ist (standardmäßig installiert, mit Visual Studio 2010) klicken Sie dann hat Code First die Datenbank für diese Instanz
-   Wenn Sie SQL Express ist nicht verfügbar, und Code First wird zu verwenden, [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (standardmäßig installiert, mit Visual Studio 2012)
-   Die Datenbank ist nach der vollqualifizierte Name des abgeleiteten Kontexts, in unserem Fall, die mit dem Namen **CodeFirstNewDatabaseSample.BloggingContext**

Hierbei handelt es sich um nur den Standardkonventionen und es gibt verschiedene Möglichkeiten zum Ändern der Datenbank, die Code First verwendet, Weitere Informationen finden Sie in der **wie "DbContext" für das Modell und die Datenbankverbindung ermittelt** Thema.
Sie können mit dieser Datenbank mithilfe von Server-Explorer in Visual Studio verbinden.

-   **Ansicht – Profiler -&gt; Server-Explorer**
-   Klicken Sie mit der rechten Maustaste auf **Datenverbindungen** , und wählen Sie **Verbindung hinzufügen...**
-   Wenn Sie vor dem müssen Sie Microsoft SQL Server als Datenquelle wählen im Server-Explorer mit einer Datenbank verbunden haben

    ![Auswählen einer Datenquelle](~/ef6/media/selectdatasource.png)

-   Eine Verbindung mit LocalDB oder SQL Express, je nachdem, welches Sie installiert haben

Wir können nun das Schema zu überprüfen, das Code First erstellt haben.

![Schema anfängliche](~/ef6/media/schemainitial.png)

"DbContext" hat, welche Klassen sollen in das Modell anhand der DbSet-Eigenschaften, die wir definiert haben. Dann wird den Standardsatz von Code First-Konventionen verwendet, um festzustellen, Tabellen- und Spaltennamen, Datentypen zu bestimmen, Primärschlüssel, usw. zu finden. Weiter unten in dieser exemplarischen Vorgehensweise betrachten wir, wie Sie diese Konventionen überschreiben können.

## <a name="5-dealing-with-model-changes"></a>5. Umgang mit Änderungen des Datenmodells

Jetzt ist es Zeit, einige Änderungen auf unserem Modell vornehmen, wenn wir diese Änderungen vornehmen, die wir auch das Datenbankschema zu aktualisieren müssen. Zu diesem Zweck werden wir eine Funktion wird aufgerufen, kurz Code First-Migrationen oder Migrationen verwenden.

Migrationen können wir eine geordnete Menge von Schritten, die beschreiben, wie Sie unsere Datenbankschema aktualisieren (und ein Downgrade durchführen). Jeder dieser Schritte, die eine Migration genannt enthält Code, der beschreibt, die Änderungen angewendet werden. 

Der erste Schritt ist für unsere BloggingContext Code First-Migrationen zu aktivieren.

-   **Extras -&gt; Bibliothekspaket-Manager -&gt; -Paket-Manager-Konsole**
-   Führen Sie in der Paket-Manager-Konsole den Befehl **Enable-Migrations** aus.
-   Ein neuer Ordner "Migrations" wurde zu Ihrem Projekt hinzugefügt, die zwei Elemente enthält:
    -   **Configuration.cs** – diese Datei enthält die Einstellungen, die für die Migration Migrationen verwenden BloggingContext. Wir müssen nicht alles in dieser exemplarischen Vorgehensweise ändern, aber dies ist in dem Sie die Seed-Daten, Register-Anbieter für andere Datenbanken geändert wird, den Namespace angeben können, dass Migrationen etc. generiert werden.
    -   **&lt;Zeitstempel&gt;\_InitialCreate.cs** – Dies ist Ihre erste Migration, es stellt die Änderungen, die bereits in der Datenbank wird eine leere Datenbank zu einem mit der Blogs und Beiträgen machen angewendet wurden . Obwohl wir können Code First, erstellen Sie diese Tabellen automatisch für uns jetzt an, dass es sich um Migrationen entschieden haben, die sie in einer Migration konvertiert wurden. Code hat, dass diese Migration bereits angewendet wurde zuerst auch in der lokalen Datenbank aufgezeichnet. Der Zeitstempel auf den Dateinamen wird verwendet, für die Sortierung zu.

    Jetzt sehen wir eine Änderung an unser Modell vornehmen, fügen Sie eine Url-Eigenschaft der Blog-Klasse:

``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Name { get; set; }
    public string Url { get; set; }

    public virtual List<Post> Posts { get; set; }
}
```

-   Führen Sie die **Add-Migration AddUrl** im Paket-Manager-Konsole den Befehl.
    Der Befehl Add-Migration Änderungen seit der letzten Migration überprüft und erstellt das Gerüst für einer neuen Migrations mit den Änderungen, die gefunden werden. Wir können die Migrationen einen Namen geben, In diesem Fall werden wir die Migration "AddUrl" aufrufen.
    Der eingerüstete Code wird darüber informiert, dass wir eine Url-Spalte, die Zeichenfolgendaten enthalten kann, das "Dbo" hinzufügen. Blogs-Tabelle. Bei Bedarf bearbeiten wir konnten den Gerüstcode, aber, die in diesem Fall nicht erforderlich.

``` csharp
namespace CodeFirstNewDatabaseSample.Migrations
{
    using System;
    using System.Data.Entity.Migrations;

    public partial class AddUrl : DbMigration
    {
        public override void Up()
        {
            AddColumn("dbo.Blogs", "Url", c => c.String());
        }

        public override void Down()
        {
            DropColumn("dbo.Blogs", "Url");
        }
    }
}
```

-   Führen Sie die **Update-Database** im Paket-Manager-Konsole den Befehl. Mit diesem Befehl wird die ausstehenden Migrationen auf die Datenbank anzuwenden. Unsere InitialCreate Migration wurde bereits angewendet wurde, wendet nur Migrationen unsere neue AddUrl Migration an.
    Tipp: Sie können die **– Verbose** wechseln, beim Aufrufen des Update-Database, um die SQL-Anweisung anzuzeigen, die für die Datenbank ausgeführt wird.

Die neue Url-Spalte wird nun die Blogs-Tabelle in der Datenbank hinzugefügt:

![Schema mit Url](~/ef6/media/schemawithurl.png)

## <a name="6-data-annotations"></a>6. Datenanmerkungen

Bisher haben wir nur EF, die das Modell unter Verwendung der Standardkonventionen ermitteln können, aber es wird zunächst wie oft werden, wenn die Klassen nicht die Konventionen strikt befolgen, und wir müssen weitere Konfigurationsschritte ausführen können. Es gibt zwei Optionen für diesen ein. Wir werden Datenanmerkungen in diesem Abschnitt, und klicken Sie dann die fluent-API im nächsten Abschnitt betrachten.

-   Fügen Sie eine Klasse zum Modell

``` csharp
public class User
{
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   Wir müssen auch einen Satz an unsere abgeleiteten Kontext hinzufügen

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }
}
```

-   Wenn wir versuchen, eine Migration hinzuzufügen erhalten wir einen Fehler mit dem Text "*EntityType 'User' ist kein Schlüssel definiert. Definieren Sie den Schlüssel für diesen EntityType."* Da EF keine Möglichkeit hat, zu wissen, dass die Benutzernamen, den primären Schlüssel für den Benutzer werden soll.
-   Wir werden jetzt von Datenanmerkungen zu verwenden, daher wir zum Hinzufügen einer using müssen-Anweisung am oberen Rand der Datei "Program.cs"

```
using System.ComponentModel.DataAnnotations;
```

-   Kommentieren Sie die Username-Eigenschaft, um anzugeben, dass sie den primären Schlüssel ist jetzt

``` csharp
public class User
{
    [Key]
    public string Username { get; set; }
    public string DisplayName { get; set; }
}
```

-   Verwenden der **Add-Migration AddUser** Befehl zum Erstellen des Gerüsts für einer Migrations zum Anwenden dieser Änderungen in der Datenbank
-   Führen Sie die **Update-Database** Befehl aus, um die neue Migration auf die Datenbank anzuwenden.

Die neue Tabelle wird jetzt in der Datenbank hinzugefügt werden:

![Schema für Benutzer](~/ef6/media/schemawithusers.png)

Die vollständige Liste der Anmerkungen, die von EF unterstützt wird:

-   [KeyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.keyattribute)
-   [StringLengthAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute)
-   [MaxLengthAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.maxlengthattribute)
-   [ConcurrencyCheckAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute)
-   [RequiredAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute)
-   [TimestampAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute)
-   [ComplexTypeAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.complextypeattribute)
-   [ColumnAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute)
-   [TableAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.tableattribute)
-   [InversePropertyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.inversepropertyattribute)
-   [ForeignKeyAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute)
-   [DatabaseGeneratedAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute)
-   [NotMappedAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.notmappedattribute)

## <a name="7-fluent-api"></a>7. Fluent-API

Im vorherigen Abschnitt haben Sie mithilfe von Datenanmerkungen zu ergänzen oder zu überschreiben, was gemäß der Konvention erkannt wurde. Eine andere Möglichkeit, die das Modell konfigurieren, erfolgt über die Code First fluent-API.

Konfiguration für die meisten erfolgen mithilfe von einfachen Daten-Anmerkungen. Die fluent-API ist eine erweiterte Methode der Angabe der Konfiguration, die alles abdeckt, die von datenanmerkungen auf einige Informationen zur erweiterten Konfiguration mit datenanmerkungen nicht möglich. darüber hinaus durchführen können. Datenanmerkungen und die fluent-API können zusammen verwendet werden.

Für den Zugriff auf die fluent-API überschreiben Sie die OnModelCreating-Methode in der "DbContext". Angenommen, wir wollten die Spalte umbenennen, die User.DisplayName anzuzeigende in gespeicherten\_Name.

-   Überschreiben der OnModelCreating-Methode auf BloggingContext durch den folgenden code

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
    public DbSet<User> Users { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Entity<User>()
            .Property(u => u.DisplayName)
            .HasColumnName("display_name");
    }
}
```

-   Verwenden der **Add-Migration ChangeDisplayName** Befehl zum Erstellen des Gerüsts für einer Migrations zum Anwenden dieser Änderungen in der Datenbank.
-   Führen Sie die **Update-Database** Befehl aus, um die neue Migration auf die Datenbank anzuwenden.

Zum Anzeigen die Spalte "DisplayName" umbenannt\_Name:

![Schema mit dem Anzeigenamen, die umbenannt](~/ef6/media/schemawithdisplaynamerenamed.png)

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise haben Sie eine neue Datenbank mit Code First-Entwicklung. Wir definierten ein Modell mithilfe von Klassen und verwendet dieses Modell zum Erstellen einer Datenbank und speichern und Abrufen von Daten. Nach der Erstellung der Datenbank verwendet haben wir Code First-Migrationen zum Ändern des Schemas als unser Modell entwickelt. Wir haben auch ein Modell mit Datenanmerkungen und die Fluent-API zu konfigurieren.
