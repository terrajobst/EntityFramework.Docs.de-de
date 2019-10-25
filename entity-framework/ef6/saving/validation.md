---
title: Validierung-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: 2c5e6f1b3f60862124bafcac42e8859a7591f8e6
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812149"
---
# <a name="data-validation"></a><span data-ttu-id="2e83a-102">Datenvalidierung</span><span class="sxs-lookup"><span data-stu-id="2e83a-102">Data Validation</span></span>
> [!NOTE]
> <span data-ttu-id="2e83a-103">**EF 4.1** und höher: die Features, APIs usw., die auf dieser Seite erläutert wurden, wurden in Entity Framework 4,1 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="2e83a-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="2e83a-104">Wenn Sie eine frühere Version verwenden, werden einige oder alle Informationen nicht angewendet.</span><span class="sxs-lookup"><span data-stu-id="2e83a-104">If you are using an earlier version, some or all of the information does not apply</span></span>

<span data-ttu-id="2e83a-105">Der Inhalt auf dieser Seite wird von einem Artikel angepasst, der ursprünglich von Julie Lerman ([https://thedatafarm.com](https://thedatafarm.com)) geschrieben wurde.</span><span class="sxs-lookup"><span data-stu-id="2e83a-105">The content on this page is adapted from an article originally written by Julie Lerman ([https://thedatafarm.com](https://thedatafarm.com)).</span></span>

<span data-ttu-id="2e83a-106">Entity Framework bietet eine Vielzahl von Validierungs Features, die für die Client seitige Validierung in eine Benutzeroberfläche übertragen oder für die serverseitige Validierung verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="2e83a-106">Entity Framework provides a great variety of validation features that can feed through to a user interface for client-side validation or be used for server-side validation.</span></span> <span data-ttu-id="2e83a-107">Wenn Sie Code First verwenden, können Sie Validierungen mithilfe von Anmerkungen oder überflüssige API-Konfigurationen angeben.</span><span class="sxs-lookup"><span data-stu-id="2e83a-107">When using code first, you can specify validations using annotation or fluent API configurations.</span></span> <span data-ttu-id="2e83a-108">Weitere Überprüfungen und komplexere können im Code angegeben werden und funktionieren unabhängig davon, ob Ihr Modell von Code First, Model First oder Database First stammt.</span><span class="sxs-lookup"><span data-stu-id="2e83a-108">Additional validations, and more complex, can be specified in code and will work whether your model hails from code first, model first or database first.</span></span>

## <a name="the-model"></a><span data-ttu-id="2e83a-109">Das Modell</span><span class="sxs-lookup"><span data-stu-id="2e83a-109">The model</span></span>

<span data-ttu-id="2e83a-110">Ich zeige die Validierungen mit einem einfachen paar von Klassen: Blog und Post.</span><span class="sxs-lookup"><span data-stu-id="2e83a-110">I'll demonstrate the validations with a simple pair of classes: Blog and Post.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="2e83a-111">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="2e83a-111">Data Annotations</span></span>

<span data-ttu-id="2e83a-112">Code First verwendet Anmerkungen aus der `System.ComponentModel.DataAnnotations` Assembly als eine Methode zum Konfigurieren von Code First-Klassen.</span><span class="sxs-lookup"><span data-stu-id="2e83a-112">Code First uses annotations from the `System.ComponentModel.DataAnnotations` assembly as one means of configuring code first classes.</span></span> <span data-ttu-id="2e83a-113">Zu diesen Anmerkungen zählen beispielsweise die `Required`, `MaxLength` und `MinLength`.</span><span class="sxs-lookup"><span data-stu-id="2e83a-113">Among these annotations are those which provide rules such as the `Required`, `MaxLength` and `MinLength`.</span></span> <span data-ttu-id="2e83a-114">Eine Reihe von .NET-Client Anwendungen erkennt diese Anmerkungen auch, z. b. ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="2e83a-114">A number of .NET client applications also recognize these annotations, for example, ASP.NET MVC.</span></span> <span data-ttu-id="2e83a-115">Sie können mit diesen Anmerkungen sowohl die Client seitige als auch die serverseitige Validierung erreichen.</span><span class="sxs-lookup"><span data-stu-id="2e83a-115">You can achieve both client side and server side validation with these annotations.</span></span> <span data-ttu-id="2e83a-116">Beispielsweise können Sie erzwingen, dass die Eigenschaft "Blog Titel" eine erforderliche Eigenschaft ist.</span><span class="sxs-lookup"><span data-stu-id="2e83a-116">For example, you can force the Blog Title property to be a required property.</span></span>

``` csharp
[Required]
public string Title { get; set; }
```

<span data-ttu-id="2e83a-117">Ohne zusätzlichen Code oder Markup Änderungen in der Anwendung führt eine vorhandene MVC-Anwendung eine Client seitige Validierung durch, wobei eine Nachricht auch dynamisch mit den Namen der Eigenschaft und der Anmerkung erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="2e83a-117">With no additional code or markup changes in the application, an existing MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![Abbildung 1](~/ef6/media/figure01.png)

<span data-ttu-id="2e83a-119">In der Post Back-Methode dieser CREATE VIEW-Methode wird Entity Framework verwendet, um den neuen Blog in der Datenbank zu speichern. die Client seitige Validierung von MVC wird jedoch ausgelöst, bevor die Anwendung den Code erreicht.</span><span class="sxs-lookup"><span data-stu-id="2e83a-119">In the post back method of this Create view, Entity Framework is used to save the new blog to the database, but MVC's client-side validation is triggered before the application reaches that code.</span></span>

<span data-ttu-id="2e83a-120">Die Client seitige Validierung ist jedoch nicht Aufzählungs sicher.</span><span class="sxs-lookup"><span data-stu-id="2e83a-120">Client side validation is not bullet-proof however.</span></span> <span data-ttu-id="2e83a-121">Benutzer können sich auf die Features Ihres Browsers auswirken oder noch schlimmer, ein Hacker könnte eine gewisse Schwierigkeit verwenden, um die Validierung der Benutzeroberfläche zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="2e83a-121">Users can impact features of their browser or worse yet, a hacker might use some trickery to avoid the UI validations.</span></span> <span data-ttu-id="2e83a-122">Entity Framework wird jedoch auch die `Required` Anmerkung erkennen und überprüfen.</span><span class="sxs-lookup"><span data-stu-id="2e83a-122">But Entity Framework will also recognize the `Required` annotation and validate it.</span></span>

<span data-ttu-id="2e83a-123">Eine einfache Möglichkeit, dies zu testen, besteht darin, das Client seitige Validierungs Feature von MVC zu deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="2e83a-123">A simple way to test this is to disable MVC's client-side validation feature.</span></span> <span data-ttu-id="2e83a-124">Dies können Sie in der Datei "Web. config" der MVC-Anwendung tun.</span><span class="sxs-lookup"><span data-stu-id="2e83a-124">You can do this in the MVC application's web.config file.</span></span> <span data-ttu-id="2e83a-125">Der appSettings-Abschnitt enthält einen Schlüssel für clientvalidationaktivierte.</span><span class="sxs-lookup"><span data-stu-id="2e83a-125">The appSettings section has a key for ClientValidationEnabled.</span></span> <span data-ttu-id="2e83a-126">Wenn dieser Schlüssel auf false festgelegt wird, kann die Benutzeroberfläche keine Validierungen durchführen.</span><span class="sxs-lookup"><span data-stu-id="2e83a-126">Setting this key to false will prevent the UI from performing validations.</span></span>

``` xml
<appSettings>
    <add key="ClientValidationEnabled"value="false"/>
    ...
</appSettings>
```

<span data-ttu-id="2e83a-127">Auch wenn die Client seitige Validierung deaktiviert ist, erhalten Sie dieselbe Antwort in Ihrer Anwendung.</span><span class="sxs-lookup"><span data-stu-id="2e83a-127">Even with the client-side validation disabled, you will get the same response in your application.</span></span> <span data-ttu-id="2e83a-128">Die Fehlermeldung "das Titelfeld ist erforderlich" wird wie zuvor angezeigt.</span><span class="sxs-lookup"><span data-stu-id="2e83a-128">The error message "The Title field is required" will be displayed as before.</span></span> <span data-ttu-id="2e83a-129">Mit der Ausnahme, dass es sich nun um das Ergebnis der serverseitigen Validierung handelt.</span><span class="sxs-lookup"><span data-stu-id="2e83a-129">Except now it will be a result of server-side validation.</span></span> <span data-ttu-id="2e83a-130">Entity Framework führt die Überprüfung für die `Required` Anmerkung durch (bevor es sogar stört ist, einen `INSERT` Befehl zum Senden an die Datenbank zu erstellen), und gibt den Fehler an MVC zurück, der die Meldung anzeigt.</span><span class="sxs-lookup"><span data-stu-id="2e83a-130">Entity Framework will perform the validation on the `Required` annotation (before it even bothers to build an `INSERT` command to send to the database) and return the error to MVC which will display the message.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="2e83a-131">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="2e83a-131">Fluent API</span></span>

<span data-ttu-id="2e83a-132">Sie können die fließende API von Code First anstelle von Anmerkungen verwenden, um dieselbe Clientseite & serverseitigen Validierung zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="2e83a-132">You can use code first's fluent API instead of annotations to get the same client side & server side validation.</span></span> <span data-ttu-id="2e83a-133">Anstatt `Required`zu verwenden, zeige ich Ihnen dies mithilfe einer MaxLength-Validierung.</span><span class="sxs-lookup"><span data-stu-id="2e83a-133">Rather than use `Required`, I'll show you this using a MaxLength validation.</span></span>

<span data-ttu-id="2e83a-134">Fließende API-Konfigurationen werden angewendet, wenn Code First das Modell aus den Klassen aufbaut.</span><span class="sxs-lookup"><span data-stu-id="2e83a-134">Fluent API configurations are applied as code first is building the model from the classes.</span></span> <span data-ttu-id="2e83a-135">Sie können die Konfigurationen einfügen, indem Sie die onmodelcreating-Methode der dbcontext-Klasse überschreiben.</span><span class="sxs-lookup"><span data-stu-id="2e83a-135">You can inject the configurations by overriding the DbContext class' OnModelCreating  method.</span></span> <span data-ttu-id="2e83a-136">Es folgt eine Konfiguration, die angibt, dass die bloggername-Eigenschaft nicht länger als 10 Zeichen sein darf.</span><span class="sxs-lookup"><span data-stu-id="2e83a-136">Here is a configuration specifying that the BloggerName property can be no longer than 10 characters.</span></span>

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

<span data-ttu-id="2e83a-137">Validierungs Fehler, die auf der Grundlage der überflüssigen API-Konfigurationen ausgelöst werden, erreichen die Benutzeroberfläche nicht automatisch, Sie können Sie jedoch im Code erfassen und entsprechend darauf reagieren.</span><span class="sxs-lookup"><span data-stu-id="2e83a-137">Validation errors thrown based on the Fluent API configurations will not automatically reach the UI, but you can capture it in code and then respond to it accordingly.</span></span>

<span data-ttu-id="2e83a-138">Im folgenden finden Sie einen Fehlercode für die Ausnahmebehandlung in der blogcontroller-Klasse der Anwendung, der diesen Überprüfungs Fehler erfasst, wenn Entity Framework versucht, einen Blog mit einem Blog Name zu speichern, der das Maximum von 10 Zeichen überschreitet.</span><span class="sxs-lookup"><span data-stu-id="2e83a-138">Here's some exception handling error code in the application's BlogController class that captures that validation error when Entity Framework attempts to save a blog with a BloggerName that exceeds the 10 character maximum.</span></span>

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

<span data-ttu-id="2e83a-139">Die Überprüfung wird nicht automatisch an die Ansicht zurückgegeben, weshalb der zusätzliche Code, der `ModelState.AddModelError` verwendet, verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="2e83a-139">The validation doesn't automatically get passed back into the view which is why the additional code that uses `ModelState.AddModelError` is being used.</span></span> <span data-ttu-id="2e83a-140">Dadurch wird sichergestellt, dass die Fehlerdetails in der Ansicht vorgenommen werden, die dann die `ValidationMessageFor` htmlhelper verwendet, um den Fehler anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="2e83a-140">This ensures that the error details make it to the view which will then use the `ValidationMessageFor` Htmlhelper to display the error.</span></span>

``` csharp
@Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a><span data-ttu-id="2e83a-141">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="2e83a-141">IValidatableObject</span></span>

<span data-ttu-id="2e83a-142">`IValidatableObject` ist eine Schnittstelle, die sich in `System.ComponentModel.DataAnnotations`befindet.</span><span class="sxs-lookup"><span data-stu-id="2e83a-142">`IValidatableObject` is an interface that lives in `System.ComponentModel.DataAnnotations`.</span></span> <span data-ttu-id="2e83a-143">Obwohl es nicht Teil der Entity Framework-API ist, können Sie es für die serverseitige Validierung in ihren Entity Framework-Klassen weiterhin nutzen.</span><span class="sxs-lookup"><span data-stu-id="2e83a-143">While it is not part of the Entity Framework API, you can still leverage it for server-side validation in your Entity Framework classes.</span></span> <span data-ttu-id="2e83a-144">`IValidatableObject` stellt eine `Validate` Methode bereit, die Entity Framework während SaveChanges aufruft, oder Sie können sich jederzeit selbst anrufen, wenn Sie die Klassen überprüfen möchten.</span><span class="sxs-lookup"><span data-stu-id="2e83a-144">`IValidatableObject` provides a `Validate` method that Entity Framework will call during SaveChanges or you can call yourself any time you want to validate the classes.</span></span>

<span data-ttu-id="2e83a-145">Konfigurationen wie `Required` und `MaxLength` führen eine Validierung für ein einzelnes Feld aus.</span><span class="sxs-lookup"><span data-stu-id="2e83a-145">Configurations such as `Required` and `MaxLength` perform validation on a single field.</span></span> <span data-ttu-id="2e83a-146">In der `Validate`-Methode können Sie noch komplexere Logik haben, z. b. zwei Felder vergleichen.</span><span class="sxs-lookup"><span data-stu-id="2e83a-146">In the `Validate` method you can have even more complex logic, for example, comparing two fields.</span></span>

<span data-ttu-id="2e83a-147">Im folgenden Beispiel wurde die `Blog`-Klasse erweitert, um `IValidatableObject` zu implementieren und dann eine Regel bereitzustellen, die die `Title` und `BloggerName` nicht erfüllen können.</span><span class="sxs-lookup"><span data-stu-id="2e83a-147">In the following example, the `Blog` class has been extended to implement `IValidatableObject` and then provide a rule that the `Title` and `BloggerName` cannot match.</span></span>

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

<span data-ttu-id="2e83a-148">Der `ValidationResult`-Konstruktor nimmt eine `string`, die die Fehlermeldung darstellt, und ein Array von `string`en, die die Elementnamen darstellen, die der Validierung zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="2e83a-148">The `ValidationResult` constructor takes a `string` that represents the error message and an array of `string`s that represent the member names that are associated with the validation.</span></span> <span data-ttu-id="2e83a-149">Da bei dieser Überprüfung sowohl die `Title` als auch die `BloggerName`überprüft werden, werden beide Eigenschaftsnamen zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="2e83a-149">Since this validation checks both the `Title` and the `BloggerName`, both property names are returned.</span></span>

<span data-ttu-id="2e83a-150">Anders als bei der Überprüfung, die von der reflüssigen API bereitgestellt wird, wird dieses Validierungs Ergebnis von der Sicht und dem Ausnahmehandler erkannt, den ich zuvor zum Hinzufügen des Fehlers verwendet habe, `ModelState` unnötig ist.</span><span class="sxs-lookup"><span data-stu-id="2e83a-150">Unlike the validation provided by the Fluent API, this validation result will be recognized by the View and the exception handler that I used earlier to add the error into `ModelState` is unnecessary.</span></span> <span data-ttu-id="2e83a-151">Da ich beide Eigenschaftsnamen im `ValidationResult`festgelegt habe, zeigen die MVC-htmlhilfsprogramme die Fehlermeldung für beide Eigenschaften an.</span><span class="sxs-lookup"><span data-stu-id="2e83a-151">Because I set both property names in the `ValidationResult`, the MVC HtmlHelpers display the error message for both of those properties.</span></span>

![Abbildung 2](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a><span data-ttu-id="2e83a-153">Dbcontext. validateentity</span><span class="sxs-lookup"><span data-stu-id="2e83a-153">DbContext.ValidateEntity</span></span>

<span data-ttu-id="2e83a-154">`DbContext` verfügt über eine über schreibbare Methode namens `ValidateEntity`.</span><span class="sxs-lookup"><span data-stu-id="2e83a-154">`DbContext` has an overridable method called `ValidateEntity`.</span></span> <span data-ttu-id="2e83a-155">Wenn Sie `SaveChanges`aufrufen, wird Entity Framework diese Methode für jede Entität im Cache aufrufen, deren Zustand nicht `Unchanged`ist.</span><span class="sxs-lookup"><span data-stu-id="2e83a-155">When you call `SaveChanges`, Entity Framework will call this method for each entity in its cache whose state is not `Unchanged`.</span></span> <span data-ttu-id="2e83a-156">Sie können die Validierungs Logik direkt in diese einfügen oder sogar mit dieser Methode aufzurufen, z. b. die `Blog.Validate` Methode, die im vorherigen Abschnitt hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="2e83a-156">You can put validation logic directly in here or even use this method to call, for example, the `Blog.Validate` method added in the previous section.</span></span>

<span data-ttu-id="2e83a-157">Im folgenden finden Sie ein Beispiel für eine `ValidateEntity` Außerkraftsetzung, mit der neue `Post`s überprüft werden, um sicherzustellen, dass der Beitrags Titel nicht bereits verwendet wurde.</span><span class="sxs-lookup"><span data-stu-id="2e83a-157">Here's an example of a `ValidateEntity` override that validates new `Post`s to ensure that the post title hasn't been used already.</span></span> <span data-ttu-id="2e83a-158">Zuerst wird überprüft, ob es sich bei der Entität um einen Beitrag handelt und ob Ihr Zustand hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="2e83a-158">It first checks to see if the entity is a post and that its state is Added.</span></span> <span data-ttu-id="2e83a-159">Wenn dies der Fall ist, wird in der Datenbank gesucht, ob bereits ein Beitrag mit dem gleichen Titel vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="2e83a-159">If that's the case, then it looks in the database to see if there is already a post with the same title.</span></span> <span data-ttu-id="2e83a-160">Wenn bereits ein Beitrag vorhanden ist, wird ein neuer `DbEntityValidationResult` erstellt.</span><span class="sxs-lookup"><span data-stu-id="2e83a-160">If there is an existing post already, then a new `DbEntityValidationResult` is created.</span></span>

<span data-ttu-id="2e83a-161">`DbEntityValidationResult` ein `DbEntityEntry` und eine `ICollection<DbValidationErrors>` für eine einzelne Entität enthält.</span><span class="sxs-lookup"><span data-stu-id="2e83a-161">`DbEntityValidationResult` houses a `DbEntityEntry` and an `ICollection<DbValidationErrors>` for a single entity.</span></span> <span data-ttu-id="2e83a-162">Am Anfang dieser Methode wird eine `DbEntityValidationResult` instanziiert, und dann werden alle ermittelten Fehler in der `ValidationErrors` Auflistung hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="2e83a-162">At the start of this method, a `DbEntityValidationResult` is instantiated and then any errors that are discovered are added into its `ValidationErrors` collection.</span></span>

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

## <a name="explicitly-triggering-validation"></a><span data-ttu-id="2e83a-163">Explizite Auslösen der Validierung</span><span class="sxs-lookup"><span data-stu-id="2e83a-163">Explicitly triggering validation</span></span>

<span data-ttu-id="2e83a-164">Ein `SaveChanges`-Aufrufe löst alle in diesem Artikel behandelten Überprüfungen aus.</span><span class="sxs-lookup"><span data-stu-id="2e83a-164">A call to `SaveChanges` triggers all of the validations covered in this article.</span></span> <span data-ttu-id="2e83a-165">Sie müssen sich jedoch nicht auf `SaveChanges`verlassen.</span><span class="sxs-lookup"><span data-stu-id="2e83a-165">But you don't need to rely on `SaveChanges`.</span></span> <span data-ttu-id="2e83a-166">Möglicherweise möchten Sie an anderer Stelle in der Anwendung überprüfen.</span><span class="sxs-lookup"><span data-stu-id="2e83a-166">You may prefer to validate elsewhere in your application.</span></span>

<span data-ttu-id="2e83a-167">`DbContext.GetValidationErrors` löst alle Überprüfungen, die durch Anmerkungen oder die fließende API definiert werden, die in `IValidatableObject` erstellte Validierung (z. b. `Blog.Validate`) und die in der `DbContext.ValidateEntity` Methode ausgeführten Überprüfungen aus.</span><span class="sxs-lookup"><span data-stu-id="2e83a-167">`DbContext.GetValidationErrors` will trigger all of the validations, those defined by annotations or the Fluent API, the validation created in `IValidatableObject` (for example, `Blog.Validate`), and the validations performed in the `DbContext.ValidateEntity` method.</span></span>

<span data-ttu-id="2e83a-168">Mit dem folgenden Code wird `GetValidationErrors` auf der aktuellen Instanz eines-`DbContext`aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="2e83a-168">The following code will call `GetValidationErrors` on the current instance of a `DbContext`.</span></span> <span data-ttu-id="2e83a-169">`ValidationErrors` werden nach Entitätstyp in `DbEntityValidationResult`gruppiert.</span><span class="sxs-lookup"><span data-stu-id="2e83a-169">`ValidationErrors` are grouped by entity type into `DbEntityValidationResult`.</span></span> <span data-ttu-id="2e83a-170">Der Code durchläuft zuerst die `DbEntityValidationResult`en, die von der-Methode zurückgegeben werden, und dann alle `DbValidationError` in.</span><span class="sxs-lookup"><span data-stu-id="2e83a-170">The code iterates first through the `DbEntityValidationResult`s returned by the method and then through each `DbValidationError` inside.</span></span>

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

## <a name="other-considerations-when-using-validation"></a><span data-ttu-id="2e83a-171">Weitere Überlegungen bei der Verwendung der Validierung</span><span class="sxs-lookup"><span data-stu-id="2e83a-171">Other considerations when using validation</span></span>

<span data-ttu-id="2e83a-172">Dies sind einige andere Punkte, die bei der Verwendung Entity Framework Validierung zu beachten sind:</span><span class="sxs-lookup"><span data-stu-id="2e83a-172">Here are a few other points to consider when using Entity Framework validation:</span></span>

- <span data-ttu-id="2e83a-173">Lazy Load ist während der Überprüfung deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="2e83a-173">Lazy loading is disabled during validation</span></span>
- <span data-ttu-id="2e83a-174">EF überprüft Daten Anmerkungen in nicht zugeordneten Eigenschaften (Eigenschaften, die keiner Spalte in der Datenbank zugeordnet sind).</span><span class="sxs-lookup"><span data-stu-id="2e83a-174">EF will validate data annotations on non-mapped properties (properties that are not mapped to a column in the database)</span></span>
- <span data-ttu-id="2e83a-175">Die Überprüfung wird durchgeführt, nachdem während `SaveChanges`Änderungen erkannt wurden.</span><span class="sxs-lookup"><span data-stu-id="2e83a-175">Validation is performed after changes are detected during `SaveChanges`.</span></span> <span data-ttu-id="2e83a-176">Wenn Sie während der Überprüfung Änderungen vornehmen, liegt es in ihrer Verantwortung, die Änderungs Protokollierung zu benachrichtigen.</span><span class="sxs-lookup"><span data-stu-id="2e83a-176">If you make changes during validation it is your responsibility to notify the change tracker</span></span>
- <span data-ttu-id="2e83a-177">`DbUnexpectedValidationException` wird ausgelöst, wenn während der Überprüfung Fehler auftreten.</span><span class="sxs-lookup"><span data-stu-id="2e83a-177">`DbUnexpectedValidationException` is thrown if errors occur during validation</span></span>
- <span data-ttu-id="2e83a-178">Facetten, die Entity Framework im Modell enthalten (maximale Länge, erforderlich usw.), führen zu einer Validierung, auch wenn keine Daten Anmerkungen in ihren Klassen vorhanden sind und/oder Sie den EF-Designer zum Erstellen des Modells verwendet haben.</span><span class="sxs-lookup"><span data-stu-id="2e83a-178">Facets that Entity Framework includes in the model (maximum length, required, etc.) will cause validation, even if there are no data annotations in your classes and/or you used the EF Designer to create your model</span></span>
- <span data-ttu-id="2e83a-179">Rang Folge Regeln:</span><span class="sxs-lookup"><span data-stu-id="2e83a-179">Precedence rules:</span></span>
  - <span data-ttu-id="2e83a-180">Fließende API-Aufrufe überschreiben die entsprechenden Daten Anmerkungen.</span><span class="sxs-lookup"><span data-stu-id="2e83a-180">Fluent API calls override the corresponding data annotations</span></span>
- <span data-ttu-id="2e83a-181">Ausführungsreihenfolge:</span><span class="sxs-lookup"><span data-stu-id="2e83a-181">Execution order:</span></span>
  - <span data-ttu-id="2e83a-182">Überprüfung der Eigenschaften vor Typvalidierung</span><span class="sxs-lookup"><span data-stu-id="2e83a-182">Property validation occurs before type validation</span></span>
  - <span data-ttu-id="2e83a-183">Die Typvalidierung tritt nur auf, wenn die Eigenschaften Überprüfung erfolgreich</span><span class="sxs-lookup"><span data-stu-id="2e83a-183">Type validation only occurs if property validation succeeds</span></span>
- <span data-ttu-id="2e83a-184">Wenn eine Eigenschaft Komplex ist, enthält die Überprüfung auch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="2e83a-184">If a property is complex, its validation will also include:</span></span>
  - <span data-ttu-id="2e83a-185">Überprüfung auf Eigenschafts Ebene für die Eigenschaften komplexer Typen</span><span class="sxs-lookup"><span data-stu-id="2e83a-185">Property-level validation on the complex type properties</span></span>
  - <span data-ttu-id="2e83a-186">Validierung der Typebene für den komplexen Typ, einschließlich `IValidatableObject` Validierung für den komplexen Typ</span><span class="sxs-lookup"><span data-stu-id="2e83a-186">Type level validation on the complex type, including `IValidatableObject` validation on the complex type</span></span>

## <a name="summary"></a><span data-ttu-id="2e83a-187">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="2e83a-187">Summary</span></span>

<span data-ttu-id="2e83a-188">Die Überprüfungs-API in Entity Framework wird sehr gut mit der Client seitigen Validierung in MVC abgespielt, aber Sie müssen sich nicht auf die Client seitige Validierung verlassen.</span><span class="sxs-lookup"><span data-stu-id="2e83a-188">The validation API in Entity Framework plays very nicely with client side validation in MVC but you don't have to rely on client-side validation.</span></span> <span data-ttu-id="2e83a-189">Entity Framework kümmert sich um die Überprüfung auf Serverseite für DataAnnotations oder Konfigurationen, die Sie mit der Code First-überflüssigen API angewendet haben.</span><span class="sxs-lookup"><span data-stu-id="2e83a-189">Entity Framework will take care of the validation on the server side for DataAnnotations or configurations you've applied with the code first Fluent API.</span></span>

<span data-ttu-id="2e83a-190">Außerdem wurde eine Reihe von Erweiterungs Punkten zum Anpassen des Verhaltens angezeigt, unabhängig davon, ob Sie die `IValidatableObject`-Schnittstelle verwenden oder auf die `DbContext.ValidateEntity`-Methode tippen.</span><span class="sxs-lookup"><span data-stu-id="2e83a-190">You also saw a number of extensibility points for customizing the behavior whether you use the `IValidatableObject` interface or tap into the `DbContext.ValidateEntity` method.</span></span> <span data-ttu-id="2e83a-191">Und die beiden letzten Überprüfungs Möglichkeiten sind über die `DbContext`verfügbar, unabhängig davon, ob Sie die Code First, Model First oder Database First Workflow verwenden, um das konzeptionelle Modell zu beschreiben.</span><span class="sxs-lookup"><span data-stu-id="2e83a-191">And these last two means of validation are available through the `DbContext`, whether you use the Code First, Model First or Database First workflow to describe your conceptual model.</span></span>
