---
title: 'Überprüfung: EF6'
author: divega
ms.date: 10/23/2016
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: 3aeb33763819544618c4a3068bb278c9b23409b6
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490629"
---
# <a name="data-validation"></a>Datenvalidierung
> [!NOTE]
> **Ef4. 1 oder höher, nur** -APIs, die Funktionen erläutert, die auf dieser Seite usw. in Entity Framework 4.1 eingeführt wurden. Wenn Sie eine frühere Version verwenden, ist einige oder alle Informationen, die nicht relevant

Der Inhalt auf dieser Seite aus und Artikel von Julie Lerman obenstehend beschrieben ([http://thedatafarm.com](http://thedatafarm.com)).

Entitätsframework bietet eine Vielzahl von Validierungsfunktionen, die über eine Benutzeroberfläche für die clientseitige Validierung feed können oder für eine serverseitige Validierung verwendet werden. Wenn Sie Code zuerst zu verwenden, können Sie Überprüfungen, die mithilfe der-Anmerkung oder eines fluent-API-Konfigurationen angeben. Zusätzliche Überprüfungen und komplexer und können in Code angegeben werden und funktionieren, ob das Modell zunächst aus Code stammt zuerst modellieren oder die Datenbank zuerst.

## <a name="the-model"></a>Das Modell

Ich zeige Ihnen, die Überprüfungen mit ein paar einfache Klassen: Blog und Post.

``` csharp
    public class Blog
      {
          public int Id { get; set; }
          public string Title { get; set; }
          public string BloggerName { get; set; }
          public DateTime DateCreated { get; set; }
          public virtual ICollection<Post> Posts { get; set; }
          }
      }

      public class Post
      {
          public int Id { get; set; }
          public string Title { get; set; }
          public DateTime DateCreated { get; set; }
          public string Content { get; set; }
          public int BlogId { get; set; }
          public ICollection<Comment> Comments { get; set; }
      }
```
## <a name="data-annotations"></a>Datenanmerkungen

Code wird zuerst Anmerkungen aus der System.ComponentModel.DataAnnotations-Assembly als eine Methode zur Konfiguration von Code First-Klassen verwendet. Auf diese Anmerkungen sind die Regeln, z. B. erforderlich, MinLength und MaxLength bereitstellen. Diese Anmerkungen, z. B. ASP.NET MVC ist auch eine Anzahl von .NET Clientanwendungen bewusst. Sie können sowohl clientseitige und clientseitige Validierung mit diese Anmerkungen erreichen. Beispielsweise können Sie die Blog-Title-Eigenschaft auf eine erforderliche Eigenschaft werden erzwingen.

``` csharp
    [Required]
    public string Title { get; set; }
```

Ohne zusätzlichen Code oder Markupänderungen in der Anwendung führt eine vorhandene MVC-Anwendung Validierung auf Clientseite, sogar dynamisch Erstellen einer Nachricht, die unter Verwendung der Eigenschaft und der Anmerkung.

![Abbildung 1](~/ef6/media/figure01.png)

Sichern in der Post-Methode der in dieser Ansicht erstellen, Entity Framework wird verwendet, um die neuen Blog in der Datenbank zu speichern, aber MVC die clientseitige Validierung wird ausgelöst, bevor die Anwendung dieses Codes erreicht.

Clientseitige Validierung ist jedoch nicht hundertprozentig. Benutzer können Features von ihren Browser beeinträchtigen oder noch schlimmer ist noch ein Hacker können einige kleiner Trick erforderlich, um die UI-Überprüfungen zu vermeiden. Aber auch Entity Framework erkennt die erforderlichen Anmerkung und überprüft wird.

Eine einfache Möglichkeit, diese zu testen ist, MVC Validierung auf dem Client-Funktion zu deaktivieren. Dies ist in web.config-Datei für die MVC-Anwendung möglich. Der Abschnitt "AppSettings" verfügt über einen Schlüssel für ClientValidationEnabled. Das Festlegen dieses Schlüssels auf "false" wird verhindert, dass die Benutzeroberfläche durchführen von Validierungen.

``` xml
    <appSettings>
        <add key="ClientValidationEnabled"value="false"/>
        ...
    </appSettings>
```

Auch bei der die clientseitige Validierung deaktiviert erhalten Sie die gleiche Antwort in Ihrer Anwendung. Die Fehlermeldung "das Feld" Titel "ist erforderlich" wird als angezeigt werden. Mit Ausnahme der jetzt ist es ein Ergebnis der Validierung auf dem Server. Entitätsframework führt die Überprüfung auf die erforderlichen Anmerkung (bevor es selbst mit den Build- und INSERT-Befehl an der Datenbank draußen) und eine Fehlermeldung angezeigt, MVC, der die Meldung angezeigt wird.

## <a name="fluent-api"></a>Fluent-API

Sie können Code First fluent-API anstelle von Anmerkungen zum selben Client Side & Server Seite Überprüfung. Anstatt verwenden, die erforderlich sind, zeige ich Ihnen diese verwenden einer MaxLength-Validierung.

Fluent-API-Konfigurationen werden angewendet, wie der Code zuerst das Modell von den Klassen erstellt. Sie können die Konfigurationen einfügen, durch Überschreiben der OnModelCreating-Methode der DbContext-Klasse. Hier ist eine Konfiguration, der angibt, dass die Eigenschaft BloggerName nicht länger als 10 Zeichen sein kann.

``` csharp
    public class BlogContext : DbContext
      {
          public DbSet<Blog> Blogs { get; set; }
          public DbSet<Post> Posts { get; set; }
          public DbSet<Comment> Comments { get; set; }

          protected override void OnModelCreating(DbModelBuilder modelBuilder)
          {
              modelBuilder.Entity<Blog>().Property(p => p.BloggerName).HasMaxLength(10);
          }
        }
```

Überprüfungsfehler ausgelöst werden basierend auf den Fluent-API-Konfigurationen nicht automatisch Reichweite der Benutzeroberfläche, aber Sie es im Code, und klicken Sie dann die auf sie reagieren entsprechend erfassen können.

Hier einige Fehlercode zur Ausnahmebehandlung in der Anwendung BlogController-Klasse, die diese Validierungsfehler erfasst, wenn Entity Framework versucht, einen Blog mit einer BloggerName zu speichern, die die maximal 10 Zeichen überschreitet.

``` csharp
    [HttpPost]
    public ActionResult Edit(int id, Blog blog)
    {
        try
        {
            db.Entry(blog).State = EntityState.Modified;
            db.SaveChanges();
            return RedirectToAction("Index");
        }
        catch(DbEntityValidationException ex)
        {
            var error = ex.EntityValidationErrors.First().ValidationErrors.First();
            this.ModelState.AddModelError(error.PropertyName, error.ErrorMessage);
            return View();
        }
    }
```

Die Überprüfung nicht automatisch wieder in die Ansicht übergeben wird der zusätzliche Code, ModelState.AddModelError verwendet wird. Dadurch wird sichergestellt, dass die Fehlerdetails für die Ansicht zu der klicken Sie dann die ValidationMessageFor Htmlhelper verwendet wird vereinfachen, um den Fehler anzuzeigen.

``` csharp
    @Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a>IValidatableObject

IValidatableObject ist eine Schnittstelle, die in der System.ComponentModel.DataAnnotations befindet. Obwohl es nicht Teil der Entity Framework-API ist, können Sie weiterhin diese für die serverseitige Validierung in den Entity Framework-Klassen nutzen. IValidatableObject bietet, dass eine Validate-Methode, die Entity Framework, während "SaveChanges", oder Sie aufgerufen wird sich jederzeit aufrufen kann die Klassen überprüft werden sollen.

Konfigurationen wie z. B. und MaxLength führen Sie Validaton auf ein einzelnes Feld aus. In der Validate-Methode können Sie auch eine komplexeren Logik, z. B. haben, Vergleichen von zwei Feldern.

Im folgenden Beispiel wurde die Blog-Klasse zum Implementieren von IValidatableObject, und geben Sie eine Regel, die den Titel und die BloggerName entsprechen kann nicht erweitert.

``` csharp
    public class Blog : IValidatableObject
     {
         public int Id { get; set; }
         [Required]
         public string Title { get; set; }
         public string BloggerName { get; set; }
         public DateTime DateCreated { get; set; }
         public virtual ICollection<Post> Posts { get; set; }

         public IEnumerable<ValidationResult> Validate(ValidationContext validationContext)
         {
             if (Title == BloggerName)
             {
                 yield return new ValidationResult
                  ("Blog Title cannot match Blogger Name", new[] { "Title", “BloggerName” });
             }
         }
     }
```

Der ValidationResult-Konstruktor akzeptiert eine Zeichenfolge, die darstellt, die Fehlermeldung und einem Array von Zeichenfolgen, die die Namen der Member darstellen, die mit der Überprüfung verknüpft sind. Da diese Überprüfung sowohl den Titel und die BloggerName eincheckt, werden beide Eigenschaftennamen zurückgegeben.

Anders als bei der Überprüfung der Fluent-API dieses Überprüfungsergebnis von der Sicht erkannt werden, und der Ausnahmehandler, den ich zuvor verwendet, um den Fehler ModelState hinzuzufügen ist nicht erforderlich. Da ich beide Eigenschaftennamen in dem ValidationResult festgelegt, wird die Fehlermeldung der MVC-HtmlHelpers für beide diese Eigenschaften angezeigt.

![Abbildung 2](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a>DbContext.ValidateEntity

"DbContext" verfügt über eine überschreibbare Methode namens ValidateEntity. Wenn Sie "SaveChanges" aufrufen, wird Entity Framework diese Methode für jede Entität in seinem Cache aufrufen, die, deren Zustand nicht unverändert ist. Sie können Validierungslogik direkt in den hier oder sogar verwenden diese Methode, um z. B. das Aufrufen der Blog.Validate-Methode, die im vorherigen Abschnitt hinzugefügten einfügen.

Hier ist ein Beispiel für eine ValidateEntity außer Kraft setzen, der überprüft neue Beiträge, um sicherzustellen, dass der Post-Titel nicht bereits verwendet wurde. Es überprüft zuerst, ob die Entität einen Beitrag ist und seinen Zustand hinzugefügt wird. Wenn dies der Fall ist, sucht sie in der Datenbank, um festzustellen, ob bereits ein Beitrag mit diesem Titel vorhanden ist. Wenn es bereits ein vorhandener Beitrag ist, wird eine neue DbEntityValidationResult erstellt.

DbEntityValidationResult enthält eine "dbentityentry" und eine ICollection DbValidationErrors für eine einzelne Entität. Am Anfang dieser Methode eine DbEntityValidationResult instanziiert wird, und anschließend alle Fehler, die ermittelt werden in der ValidationErrors-Auflistung hinzugefügt.

``` csharp
    protected override DbEntityValidationResult ValidateEntity (
        System.Data.Entity.Infrastructure.DbEntityEntry entityEntry,
        IDictionary\<object, object> items)
    {
        var result = new DbEntityValidationResult(entityEntry, new List<DbValidationError>());
        if (entityEntry.Entity is Post && entityEntry.State == EntityState.Added)
        {
            Post post = entityEntry.Entity as Post;
            //check for uniqueness of post title
            if (Posts.Where(p => p.Title == post.Title).Count() > 0)
            {
                result.ValidationErrors.Add(
                        new System.Data.Entity.Validation.DbValidationError("Title",
                        "Post title must be unique."));
            }
        }

        if (result.ValidationErrors.Count > 0)
        {
            return result;
        }
        else
        {
         return base.ValidateEntity(entityEntry, items);
        }
    }
```

## <a name="explicitly-triggering-validation"></a>Expliziten Auslösung der Überprüfung

Ein Aufruf von "SaveChanges" wird ausgelöst, alle in diesem Artikel beschriebenen Überprüfungen. Sie müssen jedoch nicht von "SaveChanges" abhängig zu sein. Sie können aber auch an anderer Stelle in Ihrer Anwendung zu überprüfen.

DbContext.GetValidationErrors löst alle Überprüfungen, Anmerkungen oder die Fluent-API definiert, die Überprüfung in IValidatableObject (z. B. Blog.Validate) erstellt und in der DbContext.ValidateEntity ausgeführten Überprüfungen -Methode.

Der folgende Code ruft GetValidationErrors für die aktuelle Instanz von einem "DbContext". ValidationErrors werden nach Entitätstyp in DbValidationRestuls gruppiert. Der Code durchläuft zunächst über die von der Methode zurückgegebenen DbValidationResults und anschließend über jeden ValidationError in.

``` csharp
    foreach (var validationResults in db.GetValidationErrors())
        {
            foreach (var error in validationResults.ValidationErrors)
            {
                Debug.WriteLine(
                                  "Entity Property: {0}, Error {1}",
                                  error.PropertyName,
                                  error.ErrorMessage);
            }
        }
```

## <a name="other-considerations-when-using-validation"></a>Weitere Überlegungen zur Verwendung von Validierung

Hier sind einige andere Punkte zu berücksichtigen bei Verwendung von Entity Framework-Überprüfung:

-   Lazy Loading wird während der Überprüfung deaktiviert.
-   EF wird überprüft, datenanmerkungen auf nicht zugeordnete Eigenschaften (Eigenschaften, die nicht an eine Spalte in der Datenbank zugeordnet sind).
-   Überprüfung wird ausgeführt, nachdem Änderungen während SaveChanges erkannt werden. Wenn Sie während der Validierung Änderungen vornehmen, ist es Ihre Aufgabe, die die änderungsnachverfolgung zu benachrichtigen.
-   DbUnexpectedValidationException wird ausgelöst, wenn während der Überprüfung Fehler auftreten.
-   Facets, die Entity Framework im Modell (maximale Länge, die erforderlich sind, usw.) enthält, bewirkt Validierung, auch wenn nicht von datenanmerkungen auf Ihre Klassen vorhanden sind bzw. die Sie dem EF Designer verwendet, um Ihr Modell zu erstellen.
-   Regeln:
    -   Fluent-API-Aufrufe, überschreiben die entsprechenden datenanmerkungen
-   Reihenfolge der Ausführung:
    -   Überprüfung der Eigenschaften tritt auf, bevor die Überprüfung von Typ
    -   Typüberprüfung tritt nur auf, wenn die Eigenschaft Überprüfung erfolgreich ist
-   Wenn eine Eigenschaft komplexer ist auch bei der Validierung gehören:
    -   Überprüfung der Eigenschaften eines komplexen Typs auf Eigenschaftenebene
    -   Geben Sie die Überprüfung auf Abonnementebene des komplexen Typs, einschließlich der IValidatableObject Validierung des komplexen Typs

## <a name="summary"></a>Zusammenfassung

Die überprüfungs-API im Entity Framework spielt sich sehr gut mit Validierung auf Clientseite in MVC, aber Sie müssen keine clientseitige Validierung abhängen. Entitätsframework übernimmt die Überprüfung auf dem Server "DataAnnotations" oder Konfigurationen, die Sie mit der Code zuerst Fluent-API angewendet haben.

Sie haben auch eine Reihe von Erweiterungspunkten zum Anpassen des Verhaltens, ob Sie die IValidatableObject-Benutzeroberfläche verwenden, oder profitieren Sie von der DbContet.ValidateEntity-Methode. Und diese letzten beiden bedeutet, dass der Überprüfung sind über die "DbContext" verfügbar, ob Sie den Code First "," Model First "oder" Database First-Workflow verwenden, um das konzeptionelle Modell beschreiben.
