---
title: Automatische Code First-Migrationen – EF 6
author: divega
ms.date: 2016-10-23
ms.assetid: 0eb86787-2161-4cb4-9cb8-67c5d6e95650
ms.openlocfilehash: 256d1c774a2165dc12daf3d04550566c1a44b751
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250451"
---
# <a name="automatic-code-first-migrations"></a>Automatische Code First-Migrationen
Automatische Migrationen können Sie Code First-Migrationen verwenden, ohne dass eine Codedatei im Projekt für jede Änderung, die Sie vornehmen. Nicht alle Änderungen automatisch angewendet werden können – z. B. Umbenennen der Spalte ist die Verwendung einer codebasierten Migration erforderlich.

> [!NOTE]
> In diesem Artikel wird davon ausgegangen, dass Sie wissen, wie Code First-Migrationen in grundlegenden Szenarien verwenden. Wenn Sie dies nicht tun, müssen Sie lesen [Code First-Migrationen](~/ef6/modeling/code-first/migrations/index.md) vor dem fortfahren.

## <a name="recommendation-for-team-environments"></a>Empfehlung für Teamumgebungen

Sie können automatische als auch Code basierende Migrationen hier und da, aber dies wird nicht empfohlen, im Team-Entwicklungsszenarios. Wenn Sie ein Team von Entwicklern gehören, die quellcodeverwaltung verwenden, sollten Sie entweder ausschließlich automatische Migrationen oder ausschließlich codebasierten Migrationen verwenden. Aufgrund der Einschränkungen der automatische Migrationen wird die Verwendung von Code-basierte Migrationen in teamumgebungen empfohlen.

## <a name="building-an-initial-model--database"></a>Erstellen eines Ausgangsmodells und einer Ausgangsdatenbank

Bevor Sie Migrationen verwenden können, benötigen Sie ein Projekt und ein Code First-Modell. In dieser exemplarischen Vorgehensweise werden die kanonischen Modelle **Blog** und **Post** verwendet.

-   Erstellen Sie ein neues **MigrationsAutomaticDemo** Konsolenanwendung
-   Fügen Sie dem Projekt die neueste Version des NuGet-Pakets **EntityFramework** hinzu.
    -   Wählen Sie **Tools > Bibliotheks-Paket-Manger > Paket-Manager-Konsole** aus.
    -   Führen Sie den Befehl **Install-Package EntityFramework** aus.
-   Fügen Sie eine **Model.cs**-Datei mit dem unten aufgeführten Code hinzu. Mit diesem Code wird eine einzelne **Blog**-Klasse, die das Domänenmodell bildet, und eine **BlogContext**-Klasse, die den EF Code First-Kontext darstellt, definiert.

  ``` csharp
      using System.Data.Entity;
      using System.Collections.Generic;
      using System.ComponentModel.DataAnnotations;
      using System.Data.Entity.Infrastructure;

      namespace MigrationsAutomaticDemo
      {
          public class BlogContext : DbContext
          {
              public DbSet<Blog> Blogs { get; set; }
          }

          public class Blog
          {
              public int BlogId { get; set; }
              public string Name { get; set; }
          }
      }
  ```

-   Mit dem Modell können Sie nun auf die Daten zugreifen. Aktualisieren Sie die **Program.cs**-Datei mit dem folgenden Code:

  ``` csharp
      using System;
      using System.Collections.Generic;
      using System.Linq;
      using System.Text;

      namespace MigrationsAutomaticDemo
      {
          class Program
          {
              static void Main(string[] args)
              {
                  using (var db = new BlogContext())
                  {
                      db.Blogs.Add(new Blog { Name = "Another Blog " });
                      db.SaveChanges();

                      foreach (var blog in db.Blogs)
                      {
                          Console.WriteLine(blog.Name);
                      }
                  }

                  Console.WriteLine("Press any key to exit...");
                  Console.ReadKey();
              }
          }
      }
  ```

-   Führen Sie die Anwendung, und Sie sehen, dass eine **MigrationsAutomaticCodeDemo.BlogContext** Datenbank wird für Sie erstellt.

    ![Datenbank LocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a>Aktivieren von Migrationen

Im Folgenden erfahren Sie, wie Sie weitere Änderungen am Modell vornehmen.

-   Fügen Sie der Blog-Klasse eine URL-Eigenschaft hinzu.

``` csharp
    public string Url { get; set; }
```

Wenn Sie zu diesem Zeitpunkt die Anwendung erneut ausführen würden, würden Sie die Ausnahme „InvalidOperationException“ mit folgendem Text erhalten: *The model backing the 'BlogContext' context has changed since the database was created. Consider using Code First Migrations to update the database (* [*http://go.microsoft.com/fwlink/?LinkId=238269*](http://go.microsoft.com/fwlink/?LinkId=238269)*) (Das Modell, das den Kontext „BlogContext“ unterstützt, wurde seit der Erstellung der Datenbank verändert. Verwenden Sie Code First-Migrationen, um die Datenbank zu aktualisieren).*

Beginnen Sie daher mit der Verwendung von Code First-Migrationen. Da wir automatische Migrationen verwenden möchten wir werden an die **– EnableAutomaticMigrations** wechseln.

-   Führen Sie die **Enable-Migrations-EnableAutomaticMigrations** Befehl in der Paket-Manager-Konsole für diesen Befehl wurde hinzugefügt. eine **Migrationen** Ordner, um das Projekt. Diesen neue Ordner enthält eine Datei:

-   **Die Dateikonfigurationsklasse:** Mit dieser Klasse können Sie das Verhalten von Migrationen für den Kontext konfigurieren. Verwenden Sie für diese exemplarische Vorgehensweise die Standardkonfiguration.
    *Da in Ihrem Projekt nur ein einzelner Code First-Kontext vorhanden ist, wurde der Kontexttyp, der für diese Konfiguration gilt, automatisch von „Enable-Migrations“ ausgefüllt.*

 

## <a name="your-first-automatic-migration"></a>Ihre erste automatische Migration

Code First-Migrationen verfügt über zwei primäre Befehle, die Sie im Folgenden kennenlernen werden:

-   **Add-Migration** erstellt den Entwurf für die nächste Migration basierend auf Änderungen, die Sie seit dem Erstellen der letzten Migration an Ihrem Modell vorgenommen haben.
-   **Update-Database** übernimmt alle ausstehenden Migrationen für die Datenbank.

Wir wollen vermeiden Sie mithilfe der Add-Migration (es sei denn, es unbedingt erforderlich) und den Fokus auf, sodass Code First-Migrationen automatisch berechnen und die Änderungen anzuwenden. Verwenden wir **Update-Database** zum Abrufen von Code First-Migrationen unseres Modells die Änderungen übertragen (die neue **Blog.Ur**l-Eigenschaft) in der Datenbank.

-   Führen Sie die **Update-Database** im Paket-Manager-Konsole den Befehl.

Die **MigrationsAutomaticDemo.BlogContext** Datenbank wird jetzt aktualisiert die **Url** -Spalte in der **Blogs** Tabelle.

 

## <a name="your-second-automatic-migration"></a>Der zweiten automatische Migration

Lassen Sie uns eine andere ändern, sodass Code First-Migrationen automatisch die Änderungen in der Datenbank übernommen, für uns darüber.

-   Fügen Sie zudem eine neue **Post**-Klasse hinzu.

``` csharp
    public class Post
    {
        public int PostId { get; set; }
        [MaxLength(200)]
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
```

-   Fügen Sie anschließend der **Blog**-Klasse eine **Posts**-Sammlung hinzu, die das andere Ende der Beziehung zwischen **Blog** und **Post** bilden soll.

``` csharp
    public virtual List<Post> Posts { get; set; }
```

Verwenden Sie jetzt **Update-Database** auf die Datenbank auf dem neuesten Stand zu bringen. Geben Sie dieses Mal das Flag **–Verbose** an, damit das SQL angezeigt wird, mit dem Code First-Migrationen ausgeführt wird.

-   Führen Sie in der Paket-Manager-Konsole den Befehl **Update-Database –Verbose** aus.

## <a name="adding-a-code-based-migration"></a>Hinzufügen eines Codes Sammlungsbasierten Migration

Jetzt sehen wir uns etwas möglicherweise eine Migration codebasierte für verwendet werden soll.

-   Fügen Sie eine **Bewertung** Eigenschaft, um die **Blog** Klasse

``` csharp
    public int Rating { get; set; }
```

Wir können einfach ausführen **Update-Database** diese Änderungen in der Datenbank zu übertragen. Allerdings fügen wir einen NULL- **Blogs.Rating** Spalte, wenn alle vorhandenen Daten in der Tabelle vorhanden ist. es zugewiesen den CLR-Standardwert des Datentyps für die neue Spalte (Bewertung ganze Zahl ist, also **0**). Damit die vorhandenen Spalten der **Blogs**-Tabelle mit einem vernünftigen Rating-Wert beginnen, geben Sie den Standardwert **3** an.
Wir verwenden Sie den Befehl Add-Migration, um diese Änderung sich für eine Migration codebasierte zu schreiben, damit wir diese bearbeiten können. Die **Add-Migration** Befehl ermöglicht es uns, benennen Sie diese Migrationen, wir nennen unsere **AddBlogRating**.

-   Führen Sie die **Add-Migration AddBlogRating** im Paket-Manager-Konsole den Befehl.
-   In der **Migrationen** Ordner "" Wir verfügen jetzt über ein neues **AddBlogRating** Migration. Der Dateiname für die Migration ist bereits mit einem Zeitstempel fest, um mit der Sortierung zu. Ermöglicht das Bearbeiten des generierten Codes zum Angeben von eines Standardwert von 3 für Blog.Rating (Zeile 10 in den folgenden Code)

*Die Migration besteht auch eine CodeBehind-Datei, die einige Metadaten erfasst werden. Diese Metadaten können auf den Code First-Migrationen die automatische Migrationen zu replizieren, die wir vor diesem codebasierte Migration durchgeführt. Dies ist wichtig, wenn ein anderer Entwickler unsere Migrationen ausgeführt werden soll, oder wenn es Zeit für die Bereitstellung der Anwendung ist.*

``` csharp
    namespace MigrationsAutomaticDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddBlogRating : DbMigration
        {
            public override void Up()
            {
                AddColumn("Blogs", "Rating", c => c.Int(nullable: false, defaultValue: 3));
            }

            public override void Down()
            {
                DropColumn("Blogs", "Rating");
            }
        }
    }
```

Die Migration ist nun fertig bearbeitet. Aktualisieren Sie daher die Datenbank mit **Update-Database**.

-   Führen Sie die **Update-Database** im Paket-Manager-Konsole den Befehl.

## <a name="back-to-automatic-migrations"></a>Automatische Migrationen an

Wir können jetzt wechseln Sie zurück zur automatische Migrationen für unsere Änderungen einfacheren. Code First-Migrationen gelten für die automatischen und Code-basierte Migrationen durchführen, in der richtigen Reihenfolge basierend auf den Metadaten, die, den Sie in der CodeBehind-Datei für jede Migration codebasierte gespeichert werden.

-   Fügen Sie eine Eigenschaft Post.Abstract unseres Modells

``` csharp
    public string Abstract { get; set; }
```

Nun können wir **Update-Database** zum Abrufen von Code First-Migrationen diese Änderung für eine automatische Migration mithilfe der Datenbank mithilfe von Push übertragen.

-   Führen Sie die **Update-Database** im Paket-Manager-Konsole den Befehl.

## <a name="summary"></a>Zusammenfassung

Modell wird in dieser exemplarischen Vorgehensweise, die Sie haben gesehen, wie Sie mit, dass automatische Migrationen mithilfe von Push übertragen an die Datenbank geändert. Sie haben auch das Gerüst erstellen und Code-basierte Migrationen zwischen automatische Migrationen ausgeführt werden, wenn Sie mehr Kontrolle benötigen.
