---
title: Validierung-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: 4162c2eb60109459c799da7cf4c1a9c8e84548b6
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182128"
---
# <a name="data-validation"></a>Datenvalidierung
> [!NOTE]
> **EF 4.1** und höher: die Features, APIs usw., die auf dieser Seite erläutert wurden, wurden in Entity Framework 4,1 eingeführt. Wenn Sie eine frühere Version verwenden, werden einige oder alle Informationen nicht angewendet.

Der Inhalt auf dieser Seite wird von einem Artikel angepasst, der ursprünglich von Julie Lerman ([https://thedatafarm.com](http://thedatafarm.com)) geschrieben wurde.

Entity Framework bietet eine Vielzahl von Validierungs Features, die für die Client seitige Validierung in eine Benutzeroberfläche übertragen oder für die serverseitige Validierung verwendet werden können. Wenn Sie Code First verwenden, können Sie Validierungen mithilfe von Anmerkungen oder überflüssige API-Konfigurationen angeben. Weitere Überprüfungen und komplexere können im Code angegeben werden und funktionieren unabhängig davon, ob Ihr Modell von Code First, Model First oder Database First stammt.

## <a name="the-model"></a>Das Modell

Ich zeige die Validierungen mit einem einfachen paar von Klassen: Blog und Post.

``` csharp
public class Blog
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string BloggerName { get; set; }
    public DateTime DateCreated { get; set; }
    public virtual ICollection<Post> Posts { get; set; }
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

Code First verwendet Anmerkungen aus der `System.ComponentModel.DataAnnotations`-Assembly als eine Methode zum Konfigurieren von Code First-Klassen. Zu diesen Anmerkungen zählen beispielsweise die `Required`, `MaxLength` und `MinLength`. Eine Reihe von .NET-Client Anwendungen erkennt diese Anmerkungen auch, z. b. ASP.NET MVC. Sie können mit diesen Anmerkungen sowohl die Client seitige als auch die serverseitige Validierung erreichen. Beispielsweise können Sie erzwingen, dass die Eigenschaft "Blog Titel" eine erforderliche Eigenschaft ist.

``` csharp
[Required]
public string Title { get; set; }
```

Ohne zusätzlichen Code oder Markup Änderungen in der Anwendung führt eine vorhandene MVC-Anwendung eine Client seitige Validierung durch, wobei eine Nachricht auch dynamisch mit den Namen der Eigenschaft und der Anmerkung erstellt wird.

![Abbildung 1](~/ef6/media/figure01.png)

In der Post Back-Methode dieser CREATE VIEW-Methode wird Entity Framework verwendet, um den neuen Blog in der Datenbank zu speichern. die Client seitige Validierung von MVC wird jedoch ausgelöst, bevor die Anwendung den Code erreicht.

Die Client seitige Validierung ist jedoch nicht Aufzählungs sicher. Benutzer können sich auf die Features Ihres Browsers auswirken oder noch schlimmer, ein Hacker könnte eine gewisse Schwierigkeit verwenden, um die Validierung der Benutzeroberfläche zu vermeiden. Entity Framework erkennt jedoch auch die Anmerkung `Required` und überprüft sie.

Eine einfache Möglichkeit, dies zu testen, besteht darin, das Client seitige Validierungs Feature von MVC zu deaktivieren. Dies können Sie in der Datei "Web. config" der MVC-Anwendung tun. Der appSettings-Abschnitt enthält einen Schlüssel für clientvalidationaktivierte. Wenn dieser Schlüssel auf false festgelegt wird, kann die Benutzeroberfläche keine Validierungen durchführen.

``` xml
<appSettings>
    <add key="ClientValidationEnabled"value="false"/>
    ...
</appSettings>
```

Auch wenn die Client seitige Validierung deaktiviert ist, erhalten Sie dieselbe Antwort in Ihrer Anwendung. Die Fehlermeldung "das Titelfeld ist erforderlich" wird wie zuvor angezeigt. Mit der Ausnahme, dass es sich nun um das Ergebnis der serverseitigen Validierung handelt. Entity Framework führt die Überprüfung für die `Required`-Anmerkung durch (bevor es sogar zu einem Fehler führt, einen `INSERT`-Befehl zu erstellen, der an die Datenbank gesendet wird), und gibt den Fehler an MVC zurück, der die Meldung anzeigt.

## <a name="fluent-api"></a>Fluent-API

Sie können die fließende API von Code First anstelle von Anmerkungen verwenden, um dieselbe Clientseite & serverseitigen Validierung zu erhalten. Anstatt `Required` zu verwenden, zeige ich Ihnen dies mithilfe einer MaxLength-Validierung.

Fließende API-Konfigurationen werden angewendet, wenn Code First das Modell aus den Klassen aufbaut. Sie können die Konfigurationen einfügen, indem Sie die onmodelcreating-Methode der dbcontext-Klasse überschreiben. Es folgt eine Konfiguration, die angibt, dass die bloggername-Eigenschaft nicht länger als 10 Zeichen sein darf.

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

Validierungs Fehler, die auf der Grundlage der überflüssigen API-Konfigurationen ausgelöst werden, erreichen die Benutzeroberfläche nicht automatisch, Sie können Sie jedoch im Code erfassen und entsprechend darauf reagieren.

Im folgenden finden Sie einen Fehlercode für die Ausnahmebehandlung in der blogcontroller-Klasse der Anwendung, der diesen Überprüfungs Fehler erfasst, wenn Entity Framework versucht, einen Blog mit einem Blog Name zu speichern, der das Maximum von 10 Zeichen überschreitet.

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
    catch (DbEntityValidationException ex)
    {
        var error = ex.EntityValidationErrors.First().ValidationErrors.First();
        this.ModelState.AddModelError(error.PropertyName, error.ErrorMessage);
        return View();
    }
}
```

Die Überprüfung wird nicht automatisch an die Ansicht zurückgegeben, weshalb der zusätzliche Code verwendet wird, der `ModelState.AddModelError` verwendet. Dadurch wird sichergestellt, dass die Fehlerdetails in der Ansicht vorgenommen werden, die dann die `ValidationMessageFor`-htmlhelper verwendet, um den Fehler anzuzeigen.

``` csharp
@Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a>IValidatableObject

`IValidatableObject` ist eine Schnittstelle, die in `System.ComponentModel.DataAnnotations` Leben. Obwohl es nicht Teil der Entity Framework-API ist, können Sie es für die serverseitige Validierung in ihren Entity Framework-Klassen weiterhin nutzen. `IValidatableObject` stellt eine `Validate`-Methode bereit, die Entity Framework während SaveChanges aufruft, oder Sie können sich jederzeit selbst anrufen, wenn Sie die Klassen überprüfen möchten.

Konfigurationen wie `Required` und `MaxLength` führen eine Validierung für ein einzelnes Feld aus. In der `Validate`-Methode können Sie noch komplexere Logik haben, z. b. zwei Felder vergleichen.

Im folgenden Beispiel wurde die `Blog`-Klasse so erweitert, dass Sie `IValidatableObject` implementiert und dann eine Regel bereitstellt, die die `Title` und `BloggerName` nicht erfüllen können.

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
            yield return new ValidationResult(
                "Blog Title cannot match Blogger Name",
                new[] { nameof(Title), nameof(BloggerName) });
        }
    }
}
```

Der `ValidationResult`-Konstruktor nimmt einen `string`, der die Fehlermeldung darstellt, und ein Array von `string`S an, die die Elementnamen darstellen, die der Validierung zugeordnet sind. Da diese Überprüfung sowohl den `Title` als auch das `BloggerName` überprüft, werden beide Eigenschaftsnamen zurückgegeben.

Anders als bei der von der fließend-API bereitgestellten Validierung wird dieses Validierungs Ergebnis von der Sicht und dem Ausnahmehandler erkannt, den ich zuvor zum Hinzufügen des Fehlers verwendet habe, `ModelState` unnötig ist. Da ich beide Eigenschaftsnamen in der `ValidationResult` festgelegt habe, zeigen die MVC-htmlhilfsprogramme die Fehlermeldung für beide Eigenschaften an.

![Abbildung 2](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a>DbContext.ValidateEntity

`DbContext` verfügt über eine über schreibbare Methode namens `ValidateEntity`. Wenn Sie `SaveChanges` aufrufen, wird Entity Framework diese Methode für jede Entität im Cache aufrufen, deren Zustand nicht `Unchanged` ist. Sie können die Validierungs Logik direkt in die-Methode einfügen oder sogar mit dieser Methode aufzurufen, z. b. die `Blog.Validate`-Methode, die im vorherigen Abschnitt hinzugefügt wurde.

Im folgenden finden Sie ein Beispiel für eine `ValidateEntity`-außer Kraft setzung, mit der neue `Post`S überprüft werden, um sicherzustellen, dass der Beitrags Titel nicht bereits verwendet wurde. Zuerst wird überprüft, ob es sich bei der Entität um einen Beitrag handelt und ob Ihr Zustand hinzugefügt wurde. Wenn dies der Fall ist, wird in der Datenbank gesucht, ob bereits ein Beitrag mit dem gleichen Titel vorhanden ist. Wenn bereits ein Beitrag vorhanden ist, wird ein neuer `DbEntityValidationResult` erstellt.

`DbEntityValidationResult` enthält eine `DbEntityEntry` und eine `ICollection<DbValidationErrors>` für eine einzelne Entität. Am Anfang dieser Methode wird eine `DbEntityValidationResult` instanziiert, und anschließend werden alle ermittelten Fehler in der `ValidationErrors`-Auflistung hinzugefügt.

``` csharp
protected override DbEntityValidationResult ValidateEntity (
    System.Data.Entity.Infrastructure.DbEntityEntry entityEntry,
    IDictionary<object, object> items)
{
    var result = new DbEntityValidationResult(entityEntry, new List<DbValidationError>());

    if (entityEntry.Entity is Post post && entityEntry.State == EntityState.Added)
    {
        // Check for uniqueness of post title
        if (Posts.Where(p => p.Title == post.Title).Any())
        {
            result.ValidationErrors.Add(
                    new System.Data.Entity.Validation.DbValidationError(
                        nameof(Title),
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

## <a name="explicitly-triggering-validation"></a>Explizite Auslösen der Validierung

Ein `SaveChanges`-aufrufstrigger löst alle in diesem Artikel behandelten Überprüfungen aus. Sie müssen sich jedoch nicht auf `SaveChanges` verlassen. Möglicherweise möchten Sie an anderer Stelle in der Anwendung überprüfen.

`DbContext.GetValidationErrors` löst alle Überprüfungen, die durch Anmerkungen oder die fließende API definiert werden, die in `IValidatableObject` (z. b. `Blog.Validate`) erstellte Validierung und die in der `DbContext.ValidateEntity`-Methode ausgeführten Überprüfungen aus.

Der folgende Code ruft `GetValidationErrors` für die aktuelle Instanz einer `DbContext` auf. `ValidationErrors` sind nach Entitätstyp in `DbEntityValidationResult` gruppiert. Der Code durchläuft zuerst durch die `DbEntityValidationResult`s, die von der-Methode zurückgegeben werden, und dann durch jede `DbValidationError` in.

``` csharp
foreach (var validationResult in db.GetValidationErrors())
{
    foreach (var error in validationResult.ValidationErrors)
    {
        Debug.WriteLine(
            "Entity Property: {0}, Error {1}",
            error.PropertyName,
            error.ErrorMessage);
    }
}
```

## <a name="other-considerations-when-using-validation"></a>Weitere Überlegungen bei der Verwendung der Validierung

Dies sind einige andere Punkte, die bei der Verwendung Entity Framework Validierung zu beachten sind:

- Lazy Load ist während der Überprüfung deaktiviert.
- EF überprüft Daten Anmerkungen in nicht zugeordneten Eigenschaften (Eigenschaften, die keiner Spalte in der Datenbank zugeordnet sind).
- Die Überprüfung wird durchgeführt, nachdem Änderungen während `SaveChanges` erkannt wurden. Wenn Sie während der Überprüfung Änderungen vornehmen, liegt es in ihrer Verantwortung, die Änderungs Protokollierung zu benachrichtigen.
- `DbUnexpectedValidationException` wird ausgelöst, wenn während der Überprüfung Fehler auftreten.
- Facetten, die Entity Framework im Modell enthalten (maximale Länge, erforderlich usw.), führen zu einer Validierung, auch wenn keine Daten Anmerkungen in ihren Klassen vorhanden sind und/oder Sie den EF-Designer zum Erstellen des Modells verwendet haben.
- Rang Folge Regeln:
  - Fließende API-Aufrufe überschreiben die entsprechenden Daten Anmerkungen.
- Ausführungsreihenfolge:
  - Überprüfung der Eigenschaften vor Typvalidierung
  - Die Typvalidierung tritt nur auf, wenn die Eigenschaften Überprüfung erfolgreich
- Wenn eine Eigenschaft Komplex ist, enthält die Überprüfung auch Folgendes:
  - Überprüfung auf Eigenschafts Ebene für die Eigenschaften komplexer Typen
  - Typüberprüfung für den komplexen Typ, einschließlich `IValidatableObject`-Validierung für den komplexen Typ

## <a name="summary"></a>Zusammenfassung

Die Überprüfungs-API in Entity Framework wird sehr gut mit der Client seitigen Validierung in MVC abgespielt, aber Sie müssen sich nicht auf die Client seitige Validierung verlassen. Entity Framework kümmert sich um die Überprüfung auf Serverseite für DataAnnotations oder Konfigurationen, die Sie mit der Code First-überflüssigen API angewendet haben.

Außerdem wurde eine Reihe von Erweiterungs Punkten zum Anpassen des Verhaltens angezeigt, unabhängig davon, ob Sie die `IValidatableObject`-Schnittstelle verwenden oder auf die `DbContext.ValidateEntity`-Methode tippen. Und die beiden letzten Überprüfungs Möglichkeiten sind über die `DbContext` verfügbar, unabhängig davon, ob Sie die Code First, Model First oder Database First Workflow verwenden, um das konzeptionelle Modell zu beschreiben.
