---
title: Validation - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77d6a095-c0d0-471e-80b9-8f9aea6108b2
ms.openlocfilehash: 98d7bd08d841ee400afb62e1079f1a965f65e139
ms.sourcegitcommit: b4a5ed177b86bf7f81602106dab6b4acc18dfc18
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2019
ms.locfileid: "54316646"
---
# <a name="data-validation"></a><span data-ttu-id="a986b-102">Datenvalidierung</span><span class="sxs-lookup"><span data-stu-id="a986b-102">Data Validation</span></span>
> [!NOTE]
> <span data-ttu-id="a986b-103">**Ef4. 1 oder höher, nur** -APIs, die Funktionen erläutert, die auf dieser Seite usw. in Entity Framework 4.1 eingeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="a986b-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="a986b-104">Wenn Sie eine frühere Version verwenden, ist einige oder alle Informationen, die nicht relevant</span><span class="sxs-lookup"><span data-stu-id="a986b-104">If you are using an earlier version, some or all of the information does not apply</span></span>

<span data-ttu-id="a986b-105">Der Inhalt auf dieser Seite ist aus einem Artikel, die ursprünglich von Julie Lerman geschrieben ([http://thedatafarm.com](http://thedatafarm.com)).</span><span class="sxs-lookup"><span data-stu-id="a986b-105">The content on this page is adapted from an article originally written by Julie Lerman ([http://thedatafarm.com](http://thedatafarm.com)).</span></span>

<span data-ttu-id="a986b-106">Entitätsframework bietet eine Vielzahl von Validierungsfunktionen, die über eine Benutzeroberfläche für die clientseitige Validierung feed können oder für eine serverseitige Validierung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="a986b-106">Entity Framework provides a great variety of validation features that can feed through to a user interface for client-side validation or be used for server-side validation.</span></span> <span data-ttu-id="a986b-107">Wenn Sie Code zuerst zu verwenden, können Sie Überprüfungen, die mithilfe der-Anmerkung oder eines fluent-API-Konfigurationen angeben.</span><span class="sxs-lookup"><span data-stu-id="a986b-107">When using code first, you can specify validations using annotation or fluent API configurations.</span></span> <span data-ttu-id="a986b-108">Zusätzliche Überprüfungen und komplexer und können in Code angegeben werden und funktionieren, ob das Modell zunächst aus Code stammt zuerst modellieren oder die Datenbank zuerst.</span><span class="sxs-lookup"><span data-stu-id="a986b-108">Additional validations, and more complex, can be specified in code and will work whether your model hails from code first, model first or database first.</span></span>

## <a name="the-model"></a><span data-ttu-id="a986b-109">Das Modell</span><span class="sxs-lookup"><span data-stu-id="a986b-109">The model</span></span>

<span data-ttu-id="a986b-110">Ich zeige Ihnen, die Überprüfungen mit ein paar einfache Klassen: Blog "und" Post ".</span><span class="sxs-lookup"><span data-stu-id="a986b-110">I’ll demonstrate the validations with a simple pair of classes: Blog and Post.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="a986b-111">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="a986b-111">Data Annotations</span></span>

<span data-ttu-id="a986b-112">Code wird zuerst Anmerkungen aus der System.ComponentModel.DataAnnotations-Assembly als eine Methode zur Konfiguration von Code First-Klassen verwendet.</span><span class="sxs-lookup"><span data-stu-id="a986b-112">Code First uses annotations from the System.ComponentModel.DataAnnotations assembly as one means of configuring code first classes.</span></span> <span data-ttu-id="a986b-113">Auf diese Anmerkungen sind die Regeln, z. B. erforderlich, MinLength und MaxLength bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="a986b-113">Among these annotations are those which provide rules such as the Required, MaxLength and MinLength.</span></span> <span data-ttu-id="a986b-114">Diese Anmerkungen, z. B. ASP.NET MVC ist auch eine Anzahl von .NET Clientanwendungen bewusst.</span><span class="sxs-lookup"><span data-stu-id="a986b-114">A number of .NET client applications also recognize these annotations, for example, ASP.NET MVC.</span></span> <span data-ttu-id="a986b-115">Sie können sowohl clientseitige und clientseitige Validierung mit diese Anmerkungen erreichen.</span><span class="sxs-lookup"><span data-stu-id="a986b-115">You can achieve both client side and server side validation with these annotations.</span></span> <span data-ttu-id="a986b-116">Beispielsweise können Sie die Blog-Title-Eigenschaft auf eine erforderliche Eigenschaft werden erzwingen.</span><span class="sxs-lookup"><span data-stu-id="a986b-116">For example, you can force the Blog Title property to be a required property.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="a986b-117">Ohne zusätzlichen Code oder Markupänderungen in der Anwendung führt eine vorhandene MVC-Anwendung Validierung auf Clientseite, sogar dynamisch Erstellen einer Nachricht, die unter Verwendung der Eigenschaft und der Anmerkung.</span><span class="sxs-lookup"><span data-stu-id="a986b-117">With no additional code or markup changes in the application, an existing MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![Abbildung 1](~/ef6/media/figure01.png)

<span data-ttu-id="a986b-119">Sichern in der Post-Methode der in dieser Ansicht erstellen, Entity Framework wird verwendet, um die neuen Blog in der Datenbank zu speichern, aber MVC die clientseitige Validierung wird ausgelöst, bevor die Anwendung dieses Codes erreicht.</span><span class="sxs-lookup"><span data-stu-id="a986b-119">In the post back method of this Create view, Entity Framework is used to save the new blog to the database, but MVC’s client-side validation is triggered before the application reaches that code.</span></span>

<span data-ttu-id="a986b-120">Clientseitige Validierung ist jedoch nicht hundertprozentig.</span><span class="sxs-lookup"><span data-stu-id="a986b-120">Client side validation is not bullet-proof however.</span></span> <span data-ttu-id="a986b-121">Benutzer können Features von ihren Browser beeinträchtigen oder noch schlimmer ist noch ein Hacker können einige kleiner Trick erforderlich, um die UI-Überprüfungen zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="a986b-121">Users can impact features of their browser or worse yet, a hacker might use some trickery to avoid the UI validations.</span></span> <span data-ttu-id="a986b-122">Aber auch Entity Framework erkennt die erforderlichen Anmerkung und überprüft wird.</span><span class="sxs-lookup"><span data-stu-id="a986b-122">But Entity Framework will also recognize the Required annotation and validate it.</span></span>

<span data-ttu-id="a986b-123">Eine einfache Möglichkeit, diese zu testen ist, MVC Validierung auf dem Client-Funktion zu deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="a986b-123">A simple way to test this is to disable MVC’s client-side validation feature.</span></span> <span data-ttu-id="a986b-124">Dies ist in web.config-Datei für die MVC-Anwendung möglich.</span><span class="sxs-lookup"><span data-stu-id="a986b-124">You can do this in the MVC application’s web.config file.</span></span> <span data-ttu-id="a986b-125">Der Abschnitt "AppSettings" verfügt über einen Schlüssel für ClientValidationEnabled.</span><span class="sxs-lookup"><span data-stu-id="a986b-125">The appSettings section has a key for ClientValidationEnabled.</span></span> <span data-ttu-id="a986b-126">Das Festlegen dieses Schlüssels auf "false" wird verhindert, dass die Benutzeroberfläche durchführen von Validierungen.</span><span class="sxs-lookup"><span data-stu-id="a986b-126">Setting this key to false will prevent the UI from performing validations.</span></span>

``` xml
    <appSettings>
        <add key="ClientValidationEnabled"value="false"/>
        ...
    </appSettings>
```

<span data-ttu-id="a986b-127">Auch bei der die clientseitige Validierung deaktiviert erhalten Sie die gleiche Antwort in Ihrer Anwendung.</span><span class="sxs-lookup"><span data-stu-id="a986b-127">Even with the client-side validation disabled, you will get the same response in your application.</span></span> <span data-ttu-id="a986b-128">Die Fehlermeldung "das Feld" Titel "ist erforderlich" wird als angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="a986b-128">The error message “The Title field is required” will be displayed as.</span></span> <span data-ttu-id="a986b-129">Mit Ausnahme der jetzt ist es ein Ergebnis der Validierung auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="a986b-129">Except now it will be a result of server-side validation.</span></span> <span data-ttu-id="a986b-130">Entitätsframework führt die Überprüfung auf die erforderlichen Anmerkung (bevor es selbst mit den Build- und INSERT-Befehl an der Datenbank draußen) und eine Fehlermeldung angezeigt, MVC, der die Meldung angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="a986b-130">Entity Framework will perform the validation on the Required annotation (before it even bothers to build and INSERT command to send to the database) and return the error to MVC which will display the message.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="a986b-131">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="a986b-131">Fluent API</span></span>

<span data-ttu-id="a986b-132">Sie können Code First fluent-API anstelle von Anmerkungen zum selben Client Side & Server Seite Überprüfung.</span><span class="sxs-lookup"><span data-stu-id="a986b-132">You can use code first’s fluent API instead of annotations to get the same client side & server side validation.</span></span> <span data-ttu-id="a986b-133">Anstatt verwenden, die erforderlich sind, zeige ich Ihnen diese verwenden einer MaxLength-Validierung.</span><span class="sxs-lookup"><span data-stu-id="a986b-133">Rather than use Required, I’ll show you this using a MaxLength validation.</span></span>

<span data-ttu-id="a986b-134">Fluent-API-Konfigurationen werden angewendet, wie der Code zuerst das Modell von den Klassen erstellt.</span><span class="sxs-lookup"><span data-stu-id="a986b-134">Fluent API configurations are applied as code first is building the model from the classes.</span></span> <span data-ttu-id="a986b-135">Sie können die Konfigurationen einfügen, durch Überschreiben der OnModelCreating-Methode der DbContext-Klasse.</span><span class="sxs-lookup"><span data-stu-id="a986b-135">You can inject the configurations by overriding the DbContext class’ OnModelCreating  method.</span></span> <span data-ttu-id="a986b-136">Hier ist eine Konfiguration, der angibt, dass die Eigenschaft BloggerName nicht länger als 10 Zeichen sein kann.</span><span class="sxs-lookup"><span data-stu-id="a986b-136">Here is a configuration specifying that the BloggerName property can be no longer than 10 characters.</span></span>

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

<span data-ttu-id="a986b-137">Überprüfungsfehler ausgelöst werden basierend auf den Fluent-API-Konfigurationen nicht automatisch Reichweite der Benutzeroberfläche, aber Sie es im Code, und klicken Sie dann die auf sie reagieren entsprechend erfassen können.</span><span class="sxs-lookup"><span data-stu-id="a986b-137">Validation errors thrown based on the Fluent API configurations will not automatically reach the UI, but you can capture it in code and then respond to it accordingly.</span></span>

<span data-ttu-id="a986b-138">Hier einige Fehlercode zur Ausnahmebehandlung in der Anwendung BlogController-Klasse, die diese Validierungsfehler erfasst, wenn Entity Framework versucht, einen Blog mit einer BloggerName zu speichern, die die maximal 10 Zeichen überschreitet.</span><span class="sxs-lookup"><span data-stu-id="a986b-138">Here’s some exception handling error code in the application’s BlogController class that captures that validation error when Entity Framework attempts to save a blog with a BloggerName that exceeds the 10 character maximum.</span></span>

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

<span data-ttu-id="a986b-139">Die Überprüfung nicht automatisch wieder in die Ansicht übergeben wird der zusätzliche Code, ModelState.AddModelError verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="a986b-139">The validation doesn’t automatically get passed back into the view which is why the additional code that uses ModelState.AddModelError is being used.</span></span> <span data-ttu-id="a986b-140">Dadurch wird sichergestellt, dass die Fehlerdetails für die Ansicht zu der klicken Sie dann die ValidationMessageFor Htmlhelper verwendet wird vereinfachen, um den Fehler anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="a986b-140">This ensures that the error details make it to the view which will then use the ValidationMessageFor Htmlhelper to display the error.</span></span>

``` csharp
    @Html.ValidationMessageFor(model => model.BloggerName)
```

## <a name="ivalidatableobject"></a><span data-ttu-id="a986b-141">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="a986b-141">IValidatableObject</span></span>

<span data-ttu-id="a986b-142">IValidatableObject ist eine Schnittstelle, die in der System.ComponentModel.DataAnnotations befindet.</span><span class="sxs-lookup"><span data-stu-id="a986b-142">IValidatableObject is an interface that lives in System.ComponentModel.DataAnnotations.</span></span> <span data-ttu-id="a986b-143">Obwohl es nicht Teil der Entity Framework-API ist, können Sie weiterhin diese für die serverseitige Validierung in den Entity Framework-Klassen nutzen.</span><span class="sxs-lookup"><span data-stu-id="a986b-143">While it is not part of the Entity Framework API, you can still leverage it for server-side validation in your Entity Framework classes.</span></span> <span data-ttu-id="a986b-144">IValidatableObject bietet, dass eine Validate-Methode, die Entity Framework, während "SaveChanges", oder Sie aufgerufen wird sich jederzeit aufrufen kann die Klassen überprüft werden sollen.</span><span class="sxs-lookup"><span data-stu-id="a986b-144">IValidatableObject provides a Validate method that Entity Framework will call during SaveChanges or you can call yourself any time you want to validate the classes.</span></span>

<span data-ttu-id="a986b-145">Konfigurationen wie z. B. und MaxLength führen Sie Validaton auf ein einzelnes Feld aus.</span><span class="sxs-lookup"><span data-stu-id="a986b-145">Configurations such as Required and MaxLength perform validaton on a single field.</span></span> <span data-ttu-id="a986b-146">In der Validate-Methode können Sie auch eine komplexeren Logik, z. B. haben, Vergleichen von zwei Feldern.</span><span class="sxs-lookup"><span data-stu-id="a986b-146">In the Validate method you can have even more complex logic, for example, comparing two fields.</span></span>

<span data-ttu-id="a986b-147">Im folgenden Beispiel wurde die Blog-Klasse zum Implementieren von IValidatableObject, und geben Sie eine Regel, die den Titel und die BloggerName entsprechen kann nicht erweitert.</span><span class="sxs-lookup"><span data-stu-id="a986b-147">In the following example, the Blog class has been extended to implement IValidatableObject and then provide a rule that the Title and BloggerName cannot match.</span></span>

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

<span data-ttu-id="a986b-148">Der ValidationResult-Konstruktor akzeptiert eine Zeichenfolge, die darstellt, die Fehlermeldung und einem Array von Zeichenfolgen, die die Namen der Member darstellen, die mit der Überprüfung verknüpft sind.</span><span class="sxs-lookup"><span data-stu-id="a986b-148">The ValidationResult constructor takes a string that represents the error message and an array of strings that represent the member names that are associated with the validation.</span></span> <span data-ttu-id="a986b-149">Da diese Überprüfung sowohl den Titel und die BloggerName eincheckt, werden beide Eigenschaftennamen zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="a986b-149">Since this validation checks both the Title and the BloggerName, both property names are returned.</span></span>

<span data-ttu-id="a986b-150">Anders als bei der Überprüfung der Fluent-API dieses Überprüfungsergebnis von der Sicht erkannt werden, und der Ausnahmehandler, den ich zuvor verwendet, um den Fehler ModelState hinzuzufügen ist nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="a986b-150">Unlike the validation provided by the Fluent API, this validation result will be recognized by the View and the exception handler that I used earlier to add the error into ModelState is unnecessary.</span></span> <span data-ttu-id="a986b-151">Da ich beide Eigenschaftennamen in dem ValidationResult festgelegt, wird die Fehlermeldung der MVC-HtmlHelpers für beide diese Eigenschaften angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a986b-151">Because I set both property names in the ValidationResult, the MVC HtmlHelpers display the error message for both of those properties.</span></span>

![Abbildung 2](~/ef6/media/figure02.png)

## <a name="dbcontextvalidateentity"></a><span data-ttu-id="a986b-153">DbContext.ValidateEntity</span><span class="sxs-lookup"><span data-stu-id="a986b-153">DbContext.ValidateEntity</span></span>

<span data-ttu-id="a986b-154">"DbContext" verfügt über eine überschreibbare Methode namens ValidateEntity.</span><span class="sxs-lookup"><span data-stu-id="a986b-154">DbContext has an Overridable method called ValidateEntity.</span></span> <span data-ttu-id="a986b-155">Wenn Sie "SaveChanges" aufrufen, wird Entity Framework diese Methode für jede Entität in seinem Cache aufrufen, die, deren Zustand nicht unverändert ist.</span><span class="sxs-lookup"><span data-stu-id="a986b-155">When you call SaveChanges, Entity Framework will call this method for each entity in its cache whose state is not Unchanged.</span></span> <span data-ttu-id="a986b-156">Sie können Validierungslogik direkt in den hier oder sogar verwenden diese Methode, um z. B. das Aufrufen der Blog.Validate-Methode, die im vorherigen Abschnitt hinzugefügten einfügen.</span><span class="sxs-lookup"><span data-stu-id="a986b-156">You can put validation logic directly in here or even use this method to call, for example, the Blog.Validate method added in the previous section.</span></span>

<span data-ttu-id="a986b-157">Hier ist ein Beispiel für eine ValidateEntity außer Kraft setzen, der überprüft neue Beiträge, um sicherzustellen, dass der Post-Titel nicht bereits verwendet wurde.</span><span class="sxs-lookup"><span data-stu-id="a986b-157">Here’s an example of a ValidateEntity override that validates new Posts to ensure that the post title hasn’t been used already.</span></span> <span data-ttu-id="a986b-158">Es überprüft zuerst, ob die Entität einen Beitrag ist und seinen Zustand hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="a986b-158">It first checks to see if the entity is a post and that its state is Added.</span></span> <span data-ttu-id="a986b-159">Wenn dies der Fall ist, sucht sie in der Datenbank, um festzustellen, ob bereits ein Beitrag mit diesem Titel vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="a986b-159">If that’s the case, then it looks in the database to see if there is already a post with the same title.</span></span> <span data-ttu-id="a986b-160">Wenn es bereits ein vorhandener Beitrag ist, wird eine neue DbEntityValidationResult erstellt.</span><span class="sxs-lookup"><span data-stu-id="a986b-160">If there is an existing post already, then a new DbEntityValidationResult is created.</span></span>

<span data-ttu-id="a986b-161">DbEntityValidationResult enthält eine "dbentityentry" und eine ICollection DbValidationErrors für eine einzelne Entität.</span><span class="sxs-lookup"><span data-stu-id="a986b-161">DbEntityValidationResult houses a DbEntityEntry and an ICollection of DbValidationErrors for a single entity.</span></span> <span data-ttu-id="a986b-162">Am Anfang dieser Methode eine DbEntityValidationResult instanziiert wird, und anschließend alle Fehler, die ermittelt werden in der ValidationErrors-Auflistung hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="a986b-162">At the start of this method, a DbEntityValidationResult is instantiated and then any errors that are discovered are added into its ValidationErrors collection.</span></span>

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

## <a name="explicitly-triggering-validation"></a><span data-ttu-id="a986b-163">Expliziten Auslösung der Überprüfung</span><span class="sxs-lookup"><span data-stu-id="a986b-163">Explicitly triggering validation</span></span>

<span data-ttu-id="a986b-164">Ein Aufruf von "SaveChanges" wird ausgelöst, alle in diesem Artikel beschriebenen Überprüfungen.</span><span class="sxs-lookup"><span data-stu-id="a986b-164">A call to SaveChanges triggers all of the validations covered in this article.</span></span> <span data-ttu-id="a986b-165">Sie müssen jedoch nicht von "SaveChanges" abhängig zu sein.</span><span class="sxs-lookup"><span data-stu-id="a986b-165">But you don’t need to rely on SaveChanges.</span></span> <span data-ttu-id="a986b-166">Sie können aber auch an anderer Stelle in Ihrer Anwendung zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="a986b-166">You may prefer to validate elsewhere in your application.</span></span>

<span data-ttu-id="a986b-167">DbContext.GetValidationErrors löst alle Überprüfungen, Anmerkungen oder die Fluent-API definiert, die Überprüfung in IValidatableObject (z. B. Blog.Validate) erstellt und in der DbContext.ValidateEntity ausgeführten Überprüfungen -Methode.</span><span class="sxs-lookup"><span data-stu-id="a986b-167">DbContext.GetValidationErrors will trigger all of the validations, those defined by annotations or the Fluent API, the validation created in IValidatableObject (for example, Blog.Validate), and the validations performed in the DbContext.ValidateEntity method.</span></span>

<span data-ttu-id="a986b-168">Der folgende Code ruft GetValidationErrors für die aktuelle Instanz von einem "DbContext".</span><span class="sxs-lookup"><span data-stu-id="a986b-168">The following code will call GetValidationErrors on the current instance of a DbContext.</span></span> <span data-ttu-id="a986b-169">ValidationErrors werden nach Entitätstyp in DbValidationResults gruppiert.</span><span class="sxs-lookup"><span data-stu-id="a986b-169">ValidationErrors are grouped by entity type into DbValidationResults.</span></span> <span data-ttu-id="a986b-170">Der Code durchläuft zunächst über die von der Methode zurückgegebenen DbValidationResults und anschließend über jeden ValidationError in.</span><span class="sxs-lookup"><span data-stu-id="a986b-170">The code iterates first through the DbValidationResults returned by the method and then through each ValidationError inside.</span></span>

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

## <a name="other-considerations-when-using-validation"></a><span data-ttu-id="a986b-171">Weitere Überlegungen zur Verwendung von Validierung</span><span class="sxs-lookup"><span data-stu-id="a986b-171">Other considerations when using validation</span></span>

<span data-ttu-id="a986b-172">Hier sind einige andere Punkte zu berücksichtigen bei Verwendung von Entity Framework-Überprüfung:</span><span class="sxs-lookup"><span data-stu-id="a986b-172">Here are a few other points to consider when using Entity Framework validation:</span></span>

-   <span data-ttu-id="a986b-173">Lazy Loading wird während der Überprüfung deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="a986b-173">Lazy loading is disabled during validation.</span></span>
-   <span data-ttu-id="a986b-174">EF wird überprüft, datenanmerkungen auf nicht zugeordnete Eigenschaften (Eigenschaften, die nicht an eine Spalte in der Datenbank zugeordnet sind).</span><span class="sxs-lookup"><span data-stu-id="a986b-174">EF will validate data annotations on non-mapped properties (properties that are not mapped to a column in the database).</span></span>
-   <span data-ttu-id="a986b-175">Überprüfung wird ausgeführt, nachdem Änderungen während SaveChanges erkannt werden.</span><span class="sxs-lookup"><span data-stu-id="a986b-175">Validation is performed after changes are detected during SaveChanges.</span></span> <span data-ttu-id="a986b-176">Wenn Sie während der Validierung Änderungen vornehmen, ist es Ihre Aufgabe, die die änderungsnachverfolgung zu benachrichtigen.</span><span class="sxs-lookup"><span data-stu-id="a986b-176">If you make changes during validation it is your responsibility to notify the change tracker.</span></span>
-   <span data-ttu-id="a986b-177">DbUnexpectedValidationException wird ausgelöst, wenn während der Überprüfung Fehler auftreten.</span><span class="sxs-lookup"><span data-stu-id="a986b-177">DbUnexpectedValidationException is thrown if errors occur during validation.</span></span>
-   <span data-ttu-id="a986b-178">Facets, die Entity Framework im Modell (maximale Länge, die erforderlich sind, usw.) enthält, bewirkt Validierung, auch wenn nicht von datenanmerkungen auf Ihre Klassen vorhanden sind bzw. die Sie dem EF Designer verwendet, um Ihr Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="a986b-178">Facets that Entity Framework includes in the model (maximum length, required, etc.) will cause validation, even if there are not data annotations on your classes and/or you used the EF Designer to create your model.</span></span>
-   <span data-ttu-id="a986b-179">Regeln:</span><span class="sxs-lookup"><span data-stu-id="a986b-179">Precedence rules:</span></span>
    -   <span data-ttu-id="a986b-180">Fluent-API-Aufrufe, überschreiben die entsprechenden datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="a986b-180">Fluent API calls override the corresponding data annotations</span></span>
-   <span data-ttu-id="a986b-181">Reihenfolge der Ausführung:</span><span class="sxs-lookup"><span data-stu-id="a986b-181">Execution order:</span></span>
    -   <span data-ttu-id="a986b-182">Überprüfung der Eigenschaften tritt auf, bevor die Überprüfung von Typ</span><span class="sxs-lookup"><span data-stu-id="a986b-182">Property validation occurs before type validation</span></span>
    -   <span data-ttu-id="a986b-183">Typüberprüfung tritt nur auf, wenn die Eigenschaft Überprüfung erfolgreich ist</span><span class="sxs-lookup"><span data-stu-id="a986b-183">Type validation only occurs if property validation succeeds</span></span>
-   <span data-ttu-id="a986b-184">Wenn eine Eigenschaft komplexer ist auch bei der Validierung gehören:</span><span class="sxs-lookup"><span data-stu-id="a986b-184">If a property is complex its validation will also include:</span></span>
    -   <span data-ttu-id="a986b-185">Überprüfung der Eigenschaften eines komplexen Typs auf Eigenschaftenebene</span><span class="sxs-lookup"><span data-stu-id="a986b-185">Property-level validation on the complex type properties</span></span>
    -   <span data-ttu-id="a986b-186">Geben Sie die Überprüfung auf Abonnementebene des komplexen Typs, einschließlich der IValidatableObject Validierung des komplexen Typs</span><span class="sxs-lookup"><span data-stu-id="a986b-186">Type level validation on the complex type, including IValidatableObject validation on the complex type</span></span>

## <a name="summary"></a><span data-ttu-id="a986b-187">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="a986b-187">Summary</span></span>

<span data-ttu-id="a986b-188">Die überprüfungs-API im Entity Framework spielt sich sehr gut mit Validierung auf Clientseite in MVC, aber Sie müssen keine clientseitige Validierung abhängen.</span><span class="sxs-lookup"><span data-stu-id="a986b-188">The validation API in Entity Framework plays very nicely with client side validation in MVC but you don't have to rely on client-side validation.</span></span> <span data-ttu-id="a986b-189">Entitätsframework übernimmt die Überprüfung auf dem Server "DataAnnotations" oder Konfigurationen, die Sie mit der Code zuerst Fluent-API angewendet haben.</span><span class="sxs-lookup"><span data-stu-id="a986b-189">Entity Framework will take care of the validation on the server side for DataAnnotations or configurations you've applied with the code first Fluent API.</span></span>

<span data-ttu-id="a986b-190">Sie haben auch eine Reihe von Erweiterungspunkten zum Anpassen des Verhaltens, ob Sie die IValidatableObject-Benutzeroberfläche verwenden, oder profitieren Sie von der DbContet.ValidateEntity-Methode.</span><span class="sxs-lookup"><span data-stu-id="a986b-190">You also saw a number of extensibility points for customizing the behavior whether you use the IValidatableObject interface or tap into the DbContet.ValidateEntity method.</span></span> <span data-ttu-id="a986b-191">Und diese letzten beiden bedeutet, dass der Überprüfung sind über die "DbContext" verfügbar, ob Sie den Code First "," Model First "oder" Database First-Workflow verwenden, um das konzeptionelle Modell beschreiben.</span><span class="sxs-lookup"><span data-stu-id="a986b-191">And these last two means of validation are available through the DbContext, whether you use the Code First, Model First or Database First workflow to describe your conceptual model.</span></span>
