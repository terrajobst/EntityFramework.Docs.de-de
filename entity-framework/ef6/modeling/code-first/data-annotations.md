---
title: Der erste Datenanmerkungen – EF6 Code
author: divega
ms.date: 10/23/2016
ms.assetid: 80abefbd-23c9-4fce-9cd3-520e5df9856e
ms.openlocfilehash: e6b017306b4f66f5bac2a9964e11391da28ceb40
ms.sourcegitcommit: a013e243a14f384999ceccaf9c779b8c1ae3b936
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57463281"
---
# <a name="code-first-data-annotations"></a><span data-ttu-id="6f990-102">Code der ersten Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="6f990-102">Code First Data Annotations</span></span>
> [!NOTE]
> <span data-ttu-id="6f990-103">**Ef4. 1 oder höher, nur** -APIs, die Funktionen erläutert, die auf dieser Seite usw. in Entity Framework 4.1 eingeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="6f990-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="6f990-104">Wenn Sie eine frühere Version verwenden, gilt einige oder alle diese Informationen nicht.</span><span class="sxs-lookup"><span data-stu-id="6f990-104">If you are using an earlier version, some or all of this information does not apply.</span></span>

<span data-ttu-id="6f990-105">Der Inhalt auf dieser Seite ist aus einem Artikel, die ursprünglich von Julie Lerman geschrieben (\<http://thedatafarm.com>).</span><span class="sxs-lookup"><span data-stu-id="6f990-105">The content on this page is adapted from an article originally written by Julie Lerman (\<http://thedatafarm.com>).</span></span>

<span data-ttu-id="6f990-106">Entity Framework Code First können Sie Ihre eigenen Domänenklassen verwenden, um die Darstellung des Modells, dem zum Ausführen von Abfragen verwendet EF ändern, nachverfolgen und Aktualisieren von Funktionen.</span><span class="sxs-lookup"><span data-stu-id="6f990-106">Entity Framework Code First allows you to use your own domain classes to represent the model that EF relies on to perform querying, change tracking, and updating functions.</span></span> <span data-ttu-id="6f990-107">Code nutzt zuerst ein spezifisches programmierschema bezeichnet als "Konvention geht vor Konfiguration."</span><span class="sxs-lookup"><span data-stu-id="6f990-107">Code First leverages a programming pattern referred to as 'convention over configuration.'</span></span> <span data-ttu-id="6f990-108">Code wird zunächst davon ausgegangen, dass Ihre Klassen den Konventionen von Entity Framework folgen und in diesem Fall automatisch, Sie funktionsfähig, wie Sie es geleistet.</span><span class="sxs-lookup"><span data-stu-id="6f990-108">Code First will assume that your classes follow the conventions of Entity Framework, and in that case, will automatically work out how to perform it's job.</span></span> <span data-ttu-id="6f990-109">Wenn Ihre Klassen diese Konventionen nicht befolgen, müssen Sie jedoch die Möglichkeit, die auf Ihre Klassen EF mit den erforderlichen Informationen zu Konfigurationen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="6f990-109">However, if your classes do not follow those conventions, you have the ability to add configurations to your classes to provide EF with the requisite information.</span></span>

<span data-ttu-id="6f990-110">Code können Sie zuerst zwei Möglichkeiten, diese Konfigurationen auf Ihre Klassen hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="6f990-110">Code First gives you two ways to add these configurations to your classes.</span></span> <span data-ttu-id="6f990-111">Ist eine einfache Attribute, die Namen "DataAnnotations" verwenden, und die zweite die Verwendung von Code First Fluent-API, die Ihnen eine Möglichkeit, Konfigurationen imperativ im Code beschreiben bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="6f990-111">One is using simple attributes called DataAnnotations, and the second is using Code First’s Fluent API, which provides you with a way to describe configurations imperatively, in code.</span></span>

<span data-ttu-id="6f990-112">Dieser Artikel konzentriert sich auf die Verwendung von "DataAnnotations" (in der System.ComponentModel.DataAnnotations-Namespace) so konfigurieren Sie Ihre Klassen – markieren die am häufigsten verwendeten Konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="6f990-112">This article will focus on using DataAnnotations (in the System.ComponentModel.DataAnnotations namespace) to configure your classes – highlighting the most commonly needed configurations.</span></span> <span data-ttu-id="6f990-113">"DataAnnotations" werden auch durch eine Anzahl von .NET-Anwendungen, wie ASP.NET MVC verstanden, dadurch kann diese Anwendungen nutzen dieselben-Anmerkungen für die clientseitige Überprüfungen.</span><span class="sxs-lookup"><span data-stu-id="6f990-113">DataAnnotations are also understood by a number of .NET applications, such as ASP.NET MVC which allows these applications to leverage the same annotations for client-side validations.</span></span>


## <a name="the-model"></a><span data-ttu-id="6f990-114">Das Modell</span><span class="sxs-lookup"><span data-stu-id="6f990-114">The model</span></span>

<span data-ttu-id="6f990-115">Ich zeige Ihnen, Code erste DataAnnotations mit ein paar einfache Klassen: Blog "und" Post ".</span><span class="sxs-lookup"><span data-stu-id="6f990-115">I’ll demonstrate Code First DataAnnotations with a simple pair of classes: Blog and Post.</span></span>

``` csharp
    public class Blog
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
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

<span data-ttu-id="6f990-116">Als dies der Fall, führen Sie erste Konvention Code der Blog und Post-Klasse einfach und erfordern keine Anpassungen an EF-Kompatibilität aktivieren.</span><span class="sxs-lookup"><span data-stu-id="6f990-116">As they are, the Blog and Post classes conveniently follow code first convention and require no tweaks to enable EF compatability.</span></span> <span data-ttu-id="6f990-117">Allerdings auch können die Anmerkungen Sie zusätzliche Informationen für EF bereit, über die Klassen und der Datenbank, die sie zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="6f990-117">However, you can also use the annotations to provide more information to EF about the classes and the database to which they map.</span></span>

 

## <a name="key"></a><span data-ttu-id="6f990-118">Key</span><span class="sxs-lookup"><span data-stu-id="6f990-118">Key</span></span>

<span data-ttu-id="6f990-119">Entitätsframework basiert auf jede Entität an, dass einen Schlüssel-Wert, der für die Entität, die nachverfolgung verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="6f990-119">Entity Framework relies on every entity having a key value that is used for entity tracking.</span></span> <span data-ttu-id="6f990-120">Eine Konvention für die Code First ist die implizite Schlüsseleigenschaften. Code sucht zuerst nach einer Eigenschaft mit dem Namen "Id" oder eine Kombination von Klassennamen und "Id", wie z. B. "BlogId".</span><span class="sxs-lookup"><span data-stu-id="6f990-120">One convention of Code First is implicit key properties; Code First will look for a property named “Id”, or a combination of class name and “Id”, such as “BlogId”.</span></span> <span data-ttu-id="6f990-121">Diese Eigenschaft wird auf eine primäre Schlüsselspalte in der Datenbank zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="6f990-121">This property will map to a primary key column in the database.</span></span>

<span data-ttu-id="6f990-122">Den Blog "und" Post beide Klassen entsprechen dieser Konvention.</span><span class="sxs-lookup"><span data-stu-id="6f990-122">The Blog and Post classes both follow this convention.</span></span> <span data-ttu-id="6f990-123">Was geschieht, wenn sie nicht?</span><span class="sxs-lookup"><span data-stu-id="6f990-123">What if they didn’t?</span></span> <span data-ttu-id="6f990-124">Was geschieht, wenn der Name von Blog verwendet *PrimaryTrackingKey* stattdessen oder sogar *"Foo"*?</span><span class="sxs-lookup"><span data-stu-id="6f990-124">What if Blog used the name *PrimaryTrackingKey* instead, or even *foo*?</span></span> <span data-ttu-id="6f990-125">Wenn der Code zuerst eine Eigenschaft nicht finden kann, die diese Konvention entspricht löst er eine Ausnahme aufgrund des Entity Framework-Anforderung, dass Sie eine Schlüsseleigenschaft verfügen müssen.</span><span class="sxs-lookup"><span data-stu-id="6f990-125">If code first does not find a property that matches this convention it will throw an exception because of Entity Framework’s requirement that you must have a key property.</span></span> <span data-ttu-id="6f990-126">Sie können die wichtigsten Anmerkung verwenden, um anzugeben, welche Eigenschaft als EntityKey verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="6f990-126">You can use the key annotation to specify which property is to be used as the EntityKey.</span></span>

``` csharp
    public class Blog
    {
        [Key]
        public int PrimaryTrackingKey { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
        public virtual ICollection<Post> Posts { get; set; }
    }
```

<span data-ttu-id="6f990-127">Wenn Sie sind mithilfe von Code first Funktion zum Generieren von Datenbanken ist, wird die Tabelle "Blog" eine Primärschlüsselspalte namens PrimaryTrackingKey, die standardmäßig auch als Identität definiert ist.</span><span class="sxs-lookup"><span data-stu-id="6f990-127">If you are using code first’s database generation feature, the Blog table will have a primary key column named PrimaryTrackingKey, which is also defined as Identity by default.</span></span>

![Blog-Tabelle mit Primärschlüssel](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a><span data-ttu-id="6f990-129">Zusammengesetzte Schlüssel</span><span class="sxs-lookup"><span data-stu-id="6f990-129">Composite keys</span></span>

<span data-ttu-id="6f990-130">Zusammengesetzte Schlüssel - Primärschlüssel, die aus mehr als eine Eigenschaft bestehen, Entitätsframework unterstützt.</span><span class="sxs-lookup"><span data-stu-id="6f990-130">Entity Framework supports composite keys - primary keys that consist of more than one property.</span></span> <span data-ttu-id="6f990-131">Beispielsweise kann eine Passport-Klasse haben, deren Primärschlüssel eine Kombination von PassportNumber und IssuingCountry ist.</span><span class="sxs-lookup"><span data-stu-id="6f990-131">For example, you could have a Passport class whose primary key is a combination of PassportNumber and IssuingCountry.</span></span>

``` csharp
    public class Passport
    {
        [Key]
        public int PassportNumber { get; set; }
        [Key]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

<span data-ttu-id="6f990-132">Würde es wird versucht, die oben genannten Klasse in Ihr EF-Modell verwenden eine `InvalidOperationException`:</span><span class="sxs-lookup"><span data-stu-id="6f990-132">Attempting to use the above class in your EF model would result in an `InvalidOperationException`:</span></span>

<span data-ttu-id="6f990-133">*Kann nicht bestimmt zusammengesetzte primary Key Sortierung für den Typ "Passport". Verwenden Sie die ColumnAttribute oder die HasKey-Methode, um einen Auftrag für den zusammengesetzten Primärschlüsseln anzugeben.*</span><span class="sxs-lookup"><span data-stu-id="6f990-133">*Unable to determine composite primary key ordering for type 'Passport'. Use the ColumnAttribute or the HasKey method to specify an order for composite primary keys.*</span></span>

<span data-ttu-id="6f990-134">Um zusammengesetzte Schlüssel verwenden zu können, erfordert Entity Framework Sie einen Auftrag für die wichtigsten Eigenschaften zu definieren.</span><span class="sxs-lookup"><span data-stu-id="6f990-134">In order to use composite keys, Entity Framework requires you to define an order for the key properties.</span></span> <span data-ttu-id="6f990-135">Sie erreichen dies, indem Sie die Anmerkung für die Spalte eine Reihenfolge angegeben.</span><span class="sxs-lookup"><span data-stu-id="6f990-135">You can do this by using the Column annotation to specify an order.</span></span>

>[!NOTE]
> <span data-ttu-id="6f990-136">Den Wert der ist relativ, (und nicht der Index basiert), damit alle Werte verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="6f990-136">The order value is relative (rather than index based) so any values can be used.</span></span> <span data-ttu-id="6f990-137">Beispielsweise würden 100 und 200 anstelle von 1 und 2 akzeptabel sein.</span><span class="sxs-lookup"><span data-stu-id="6f990-137">For example, 100 and 200 would be acceptable in place of 1 and 2.</span></span>

``` csharp
    public class Passport
    {
        [Key]
        [Column(Order=1)]
        public int PassportNumber { get; set; }
        [Key]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

<span data-ttu-id="6f990-138">Wenn Sie Entitäten mit zusammengesetzter Fremdschlüssel haben, müssen Sie angeben die gleiche Spalte sortieren, die Sie für die entsprechenden Eigenschaften des primären Schlüssels verwendet.</span><span class="sxs-lookup"><span data-stu-id="6f990-138">If you have entities with composite foreign keys, then you must specify the same column ordering that you used for the corresponding primary key properties.</span></span>

<span data-ttu-id="6f990-139">Nur die relative Reihenfolge in der Fremdschlüsseleigenschaften muss übereinstimmen, die genauen Werte, die für **Reihenfolge** brauchen nicht übereinzustimmen.</span><span class="sxs-lookup"><span data-stu-id="6f990-139">Only the relative ordering within the foreign key properties needs to be the same, the exact values assigned to **Order** do not need to match.</span></span> <span data-ttu-id="6f990-140">Beispielsweise konnte in der folgenden Klasse 3 und 4 anstelle von 1 und 2 verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6f990-140">For example, in the following class, 3 and 4 could be used in place of 1 and 2.</span></span>

``` csharp
    public class PassportStamp
    {
        [Key]
        public int StampId { get; set; }
        public DateTime Stamped { get; set; }
        public string StampingCountry { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 1)]
        public int PassportNumber { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }

        public Passport Passport { get; set; }
    }
```

## <a name="required"></a><span data-ttu-id="6f990-141">Erforderlich</span><span class="sxs-lookup"><span data-stu-id="6f990-141">Required</span></span>

<span data-ttu-id="6f990-142">Die erforderlichen Anmerkung wird Entity Framework angewiesen, dass eine bestimmte Eigenschaft erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="6f990-142">The Required annotation tells EF that a particular property is required.</span></span>

<span data-ttu-id="6f990-143">Hinzufügen von erforderlich, um die Title-Eigenschaft wird erzwungen, EF (und MVC), um sicherzustellen, dass die Eigenschaft bereits Daten enthält.</span><span class="sxs-lookup"><span data-stu-id="6f990-143">Adding Required to the Title property will force EF (and MVC) to ensure that the property has data in it.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="6f990-144">Keine zusätzlichen ohne Änderungen des Codes oder Markups in der Anwendung, in eine MVC-Anwendung führt Validierung auf Clientseite, sogar dynamisch Erstellen einer Nachricht, die unter Verwendung der Eigenschaft und der Anmerkung.</span><span class="sxs-lookup"><span data-stu-id="6f990-144">With no additional no code or markup changes in the application, an MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![Erstellen Sie Seite mit dem Titel ist erforderlich, Fehler](~/ef6/media/jj591583-figure02.png)

<span data-ttu-id="6f990-146">Das Required-Attribut wirkt außerdem generierte Datenbank durch Festlegen der zugeordnete Eigenschaft keine NULL-Werte zulässt.</span><span class="sxs-lookup"><span data-stu-id="6f990-146">The Required attribute will also affect the generated database by making the mapped property non-nullable.</span></span> <span data-ttu-id="6f990-147">Beachten Sie, dass das Feld "Titel", "not null" geändert hat.</span><span class="sxs-lookup"><span data-stu-id="6f990-147">Notice that the Title field has changed to “not null”.</span></span>

>[!NOTE]
> <span data-ttu-id="6f990-148">In einigen Fällen kann es nicht möglich für die Spalte in der Datenbank NULL-Werte zulässt, obwohl die Eigenschaft erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="6f990-148">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="6f990-149">Beispielsweise wird bei Verwendung einer TPH-Vererbung Strategie für die Datentyps für mehrere Typen in einer einzelnen Tabelle gespeichert.</span><span class="sxs-lookup"><span data-stu-id="6f990-149">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="6f990-150">Wenn ein abgeleiteter Typ eine erforderliche Eigenschaft, die die Spalte NULL-hergestellt werden kann enthält, da nicht alle Typen in der Hierarchie dieser Eigenschaft hat.</span><span class="sxs-lookup"><span data-stu-id="6f990-150">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>

 

![Blogs-Tabelle](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a><span data-ttu-id="6f990-152">MaxLength und MinLength</span><span class="sxs-lookup"><span data-stu-id="6f990-152">MaxLength and MinLength</span></span>

<span data-ttu-id="6f990-153">Die Attribute MaxLength und MinLength ermöglichen Ihnen die Angabe der zusätzlichen Eigenschaft-Überprüfungen, genauso wie Sie mit erforderlich.</span><span class="sxs-lookup"><span data-stu-id="6f990-153">The MaxLength and MinLength attributes allow you to specify additional property validations, just as you did with Required.</span></span>

<span data-ttu-id="6f990-154">Hier ist die BloggerName mit Anforderungen im Hinblick auf.</span><span class="sxs-lookup"><span data-stu-id="6f990-154">Here is the BloggerName with length requirements.</span></span> <span data-ttu-id="6f990-155">Darüber hinaus wird veranschaulicht, wie Sie die Attribute zu kombinieren.</span><span class="sxs-lookup"><span data-stu-id="6f990-155">The example also demonstrates how to combine attributes.</span></span>

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="6f990-156">Die MaxLength-Anmerkung wird die Datenbank durch Festlegen der Eigenschaft Länge auf 10 beeinträchtigt.</span><span class="sxs-lookup"><span data-stu-id="6f990-156">The MaxLength annotation will impact the database by setting the property’s length to 10.</span></span>

![Blogs-Tabelle, die mit die maximale Länge für BloggerName-Spalte](~/ef6/media/jj591583-figure04.png)

<span data-ttu-id="6f990-158">MVC-Client-Side-Anmerkung, und EF 4.1 serverseitige Anmerkung berücksichtigt beide dieser Überprüfung erneut dynamisch erstellen eine Fehlermeldung angezeigt: "Das Feld BloggerName muss eine Zeichenfolge oder Array-Typ mit einer maximalen Länge von '10'." Diese Meldung ist ein wenig lang.</span><span class="sxs-lookup"><span data-stu-id="6f990-158">MVC client-side annotation and EF 4.1 server-side annotation will both honor this validation, again dynamically building an error message: “The field BloggerName must be a string or array type with a maximum length of '10'.” That message is a little long.</span></span> <span data-ttu-id="6f990-159">Viele Anmerkungen können Sie eine Fehlermeldung mit dem ErrorMessage-Attribut angeben.</span><span class="sxs-lookup"><span data-stu-id="6f990-159">Many annotations let you specify an error message with the ErrorMessage attribute.</span></span>

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="6f990-160">Sie können auch in der erforderlichen Anmerkung ErrorMessage angeben.</span><span class="sxs-lookup"><span data-stu-id="6f990-160">You can also specify ErrorMessage in the Required annotation.</span></span>

![Erstellen Sie die Seite mit benutzerdefinierten Fehlermeldung.](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a><span data-ttu-id="6f990-162">NotMapped</span><span class="sxs-lookup"><span data-stu-id="6f990-162">NotMapped</span></span>

<span data-ttu-id="6f990-163">Erste Konvention Code schreibt vor, dass jede Eigenschaft, die einen unterstützten Datentyp ist in der Datenbank dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="6f990-163">Code first convention dictates that every property that is of a supported data type is represented in the database.</span></span> <span data-ttu-id="6f990-164">Dies ist jedoch immer der Fall, in Ihren Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="6f990-164">But this isn’t always the case in your applications.</span></span> <span data-ttu-id="6f990-165">Z. B. können Sie in der Blog-Klasse, die einen Code, der basierend auf den Titel und BloggerName Feldern erstellt, eine Eigenschaft verfügen.</span><span class="sxs-lookup"><span data-stu-id="6f990-165">For example you might have a property in the Blog class that creates a code based on the Title and BloggerName fields.</span></span> <span data-ttu-id="6f990-166">Diese Eigenschaft kann dynamisch erstellt werden und muss nicht gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="6f990-166">That property can be created dynamically and does not need to be stored.</span></span> <span data-ttu-id="6f990-167">Sie können alle Eigenschaften, die kennzeichnen, die nicht mit der Datenbank mit der Anmerkung NotMapped, z. B. diese BlogCode-Eigenschaft zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="6f990-167">You can mark any properties that do not map to the database with the NotMapped annotation such as this BlogCode property.</span></span>

``` csharp
    [NotMapped]
    public string BlogCode
    {
        get
        {
            return Title.Substring(0, 1) + ":" + BloggerName.Substring(0, 1);
        }
    }
```

 

## <a name="complextype"></a><span data-ttu-id="6f990-168">ComplexType</span><span class="sxs-lookup"><span data-stu-id="6f990-168">ComplexType</span></span>

<span data-ttu-id="6f990-169">Es ist nicht ungewöhnlich, dass Ihre Domänenentitäten über einen Satz von Klassen zu beschreiben, und klicken Sie dann layer diese Klassen zum Beschreiben einer vollständigen Entität.</span><span class="sxs-lookup"><span data-stu-id="6f990-169">It’s not uncommon to describe your domain entities across a set of classes and then layer those classes to describe a complete entity.</span></span> <span data-ttu-id="6f990-170">Beispielsweise können Sie eine Klasse namens BlogDetails für das Modell hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="6f990-170">For example, you may add a class called BlogDetails to your model.</span></span>

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="6f990-171">Beachten Sie, dass BlogDetails keine Art von "Key"-Eigenschaft verfügt.</span><span class="sxs-lookup"><span data-stu-id="6f990-171">Notice that BlogDetails does not have any type of key property.</span></span> <span data-ttu-id="6f990-172">BlogDetails wird in Domain-driven Design als Wertobjekt bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="6f990-172">In domain driven design, BlogDetails is referred to as a value object.</span></span> <span data-ttu-id="6f990-173">Entitätsframework bezeichnet Wertobjekte als komplexe Typen.</span><span class="sxs-lookup"><span data-stu-id="6f990-173">Entity Framework refers to value objects as complex types.</span></span><span data-ttu-id="6f990-174">  Komplexe Typen können nicht auf ihre eigenen nachverfolgt werden.</span><span class="sxs-lookup"><span data-stu-id="6f990-174">  Complex types cannot be tracked on their own.</span></span>

<span data-ttu-id="6f990-175">Jedoch als Eigenschaft in der Blog-Klasse BlogDetails, die sie als Teil einer Blog-Objekts nachverfolgt.</span><span class="sxs-lookup"><span data-stu-id="6f990-175">However as a property in the Blog class, BlogDetails it will be tracked as part of a Blog object.</span></span> <span data-ttu-id="6f990-176">Damit Code, um dies zu erkennen müssen Sie die BlogDetails-Klasse als ComplexType markieren.</span><span class="sxs-lookup"><span data-stu-id="6f990-176">In order for code first to recognize this, you must mark the BlogDetails class as a ComplexType.</span></span>

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="6f990-177">Jetzt können Sie eine Eigenschaft in der Blog-Klasse zur Darstellung der BlogDetails für dieses Blog hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="6f990-177">Now you can add a property in the Blog class to represent the BlogDetails for that blog.</span></span>

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

<span data-ttu-id="6f990-178">In der Datenbank enthält die Tabelle "Blog" alle Eigenschaften des Blogs, einschließlich der Eigenschaften, die in der BlogDetail-Eigenschaft enthalten.</span><span class="sxs-lookup"><span data-stu-id="6f990-178">In the database, the Blog table will contain all of the properties of the blog including the properties contained in its BlogDetail property.</span></span> <span data-ttu-id="6f990-179">Standardmäßig wird jede mit dem Namen des komplexen Typs, BlogDetail vorangestellt.</span><span class="sxs-lookup"><span data-stu-id="6f990-179">By default, each one is preceded with the name of the complex type, BlogDetail.</span></span>

![Tabelle "Blog" mit komplexen Typen](~/ef6/media/jj591583-figure06.png)


## <a name="concurrencycheck"></a><span data-ttu-id="6f990-181">ConcurrencyCheck</span><span class="sxs-lookup"><span data-stu-id="6f990-181">ConcurrencyCheck</span></span>

<span data-ttu-id="6f990-182">Die Anmerkung ConcurrencyCheck können Sie so kennzeichnen Sie eine oder mehrere Eigenschaften für die parallelitätsprüfung in der Datenbank, wenn ein Benutzer bearbeitet oder eine Entität löscht verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="6f990-182">The ConcurrencyCheck annotation allows you to flag one or more properties to be used for concurrency checking in the database when a user edits or deletes an entity.</span></span> <span data-ttu-id="6f990-183">Wenn Sie mit dem EF Designer gearbeitet haben, entspricht dies durch Festlegen einer Eigenschaft ConcurrencyMode auf fest.</span><span class="sxs-lookup"><span data-stu-id="6f990-183">If you've been working with the EF Designer, this aligns with setting a property's ConcurrencyMode to Fixed.</span></span>

<span data-ttu-id="6f990-184">Sehen wir uns an, wie ConcurrencyCheck funktioniert, indem sie auf die BloggerName-Eigenschaft hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="6f990-184">Let’s see how ConcurrencyCheck works by adding it to the BloggerName property.</span></span>

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="6f990-185">Wenn SaveChanges, aufgrund der ConcurrencyCheck-Anmerkung auf das Feld BloggerName aufgerufen wird wird der ursprüngliche Wert der Eigenschaft in dem Update verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6f990-185">When SaveChanges is called, because of the ConcurrencyCheck annotation on the BloggerName field, the original value of that property will be used in the update.</span></span> <span data-ttu-id="6f990-186">Der Befehl versucht, die richtige Zeile suchen, filtern, nicht nur auf die Schlüssel-Wert, sondern auch auf den ursprünglichen Wert von BloggerName.</span><span class="sxs-lookup"><span data-stu-id="6f990-186">The command will attempt to locate the correct row by filtering not only on the key value but also on the original value of BloggerName.</span></span><span data-ttu-id="6f990-187">  Hier sind die wichtigen Teile der UPDATE-Befehl gesendet, um die Datenbank, hier sehen Sie der Befehl aktualisiert die Zeile mit einem PrimaryTrackingKey ist 1 und eine BloggerName von "Julie", das den ursprünglichen Wert wurde bei diesem Blog aus der Datenbank abgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="6f990-187">  Here are the critical parts of the UPDATE command sent to the database, where you can see the command will update the row that has a PrimaryTrackingKey is 1 and a BloggerName of “Julie” which was the original value when that blog was retrieved from the database.</span></span>

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

<span data-ttu-id="6f990-188">Wenn jemand den Blogger-Namen für dieses Blogs in der Zwischenzeit geändert wurde, wird dieses Update schlägt fehl, und Sie erhalten eine DbUpdateConcurrencyException, die Sie behandeln müssen.</span><span class="sxs-lookup"><span data-stu-id="6f990-188">If someone has changed the blogger name for that blog in the meantime, this update will fail and you’ll get a DbUpdateConcurrencyException that you'll need to handle.</span></span>

 

## <a name="timestamp"></a><span data-ttu-id="6f990-189">TimeStamp</span><span class="sxs-lookup"><span data-stu-id="6f990-189">TimeStamp</span></span>

<span data-ttu-id="6f990-190">Es ist üblich, ein Rowversion oder Timestamp-Felder für die parallelitätsüberprüfung zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="6f990-190">It's more common to use rowversion or timestamp fields for concurrency checking.</span></span> <span data-ttu-id="6f990-191">Aber statt der ConcurrencyCheck-Anmerkung zu verwenden, können Sie die spezifischere TimeStamp-Anmerkung, solange der Typ der Eigenschaft Byte-Array ist.</span><span class="sxs-lookup"><span data-stu-id="6f990-191">But rather than using the ConcurrencyCheck annotation, you can use the more specific TimeStamp annotation as long as the type of the property is byte array.</span></span> <span data-ttu-id="6f990-192">Code zuerst behandelt Timestamp-Eigenschaften identisch als ConcurrencyCheck-Eigenschaften, aber es wird auch sichergestellt, dass die Datenbankfeld, das Code zuerst erzeugt NULL-Werte zulässt.</span><span class="sxs-lookup"><span data-stu-id="6f990-192">Code first will treat Timestamp properties the same as ConcurrencyCheck properties, but it will also ensure that the database field that code first generates is non-nullable.</span></span> <span data-ttu-id="6f990-193">Sie können nur eine Timestamp-Eigenschaft in einer bestimmten Klasse haben.</span><span class="sxs-lookup"><span data-stu-id="6f990-193">You can only have one timestamp property in a given class.</span></span>

<span data-ttu-id="6f990-194">Der Blog-Klasse hinzugefügt die folgende Eigenschaft:</span><span class="sxs-lookup"><span data-stu-id="6f990-194">Adding the following property to the Blog class:</span></span>

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

<span data-ttu-id="6f990-195">die Ergebnisse im Code erstellen zunächst eine NULL-Timestamp-Spalte in der Datenbanktabelle.</span><span class="sxs-lookup"><span data-stu-id="6f990-195">results in code first creating a non-nullable timestamp column in the database table.</span></span>

![Blogs-Tabelle mit timestamp-Spalte](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a><span data-ttu-id="6f990-197">Tabellen- und Spaltennamen</span><span class="sxs-lookup"><span data-stu-id="6f990-197">Table and Column</span></span>

<span data-ttu-id="6f990-198">Wenn Sie Code First die Datenbank erstellen können, empfiehlt es sich, ändern Sie den Namen der Tabellen und Spalten, die sie erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="6f990-198">If you are letting Code First create the database, you may want to change the name of the tables and columns it is creating.</span></span> <span data-ttu-id="6f990-199">Sie können Code First auch mit einer vorhandenen Datenbank verwenden.</span><span class="sxs-lookup"><span data-stu-id="6f990-199">You can also use Code First with an existing database.</span></span> <span data-ttu-id="6f990-200">Es ist jedoch nicht immer der Fall, dass die Namen von Klassen und Eigenschaften in der Domäne den Namen der Tabellen und Spalten in der Datenbank übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="6f990-200">But it's not always the case that the names of the classes and properties in your domain match the names of the tables and columns in your database.</span></span>

<span data-ttu-id="6f990-201">Meine Klasse heißt Blog und gemäß der Konvention Code zuerst wird davon ausgegangen, dass dies in eine Tabelle namens Blogs zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="6f990-201">My class is named Blog and by convention, code first presumes this will map to a table named Blogs.</span></span> <span data-ttu-id="6f990-202">Wenn dies nicht der Fall ist, können Sie den Namen der Tabelle mit dem Attribut für die Tabelle angeben.</span><span class="sxs-lookup"><span data-stu-id="6f990-202">If that's not the case you can specify the name of the table with the Table attribute.</span></span> <span data-ttu-id="6f990-203">Hier ist z. B. die Anmerkung gibt an, dass der Tabellenname InternalBlogs ist.</span><span class="sxs-lookup"><span data-stu-id="6f990-203">Here for example, the annotation is specifying that the table name is InternalBlogs.</span></span>

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

<span data-ttu-id="6f990-204">Die Anmerkung für die Spalte ist eine weitere fundierte Kenntnisse in die Attribute der zugeordneten Spalte angeben.</span><span class="sxs-lookup"><span data-stu-id="6f990-204">The Column annotation is a more adept in specifying the attributes of a mapped column.</span></span> <span data-ttu-id="6f990-205">Sie können festlegen, einen Namen, Datentyp und sogar die Reihenfolge, in der eine Spalte in der Tabelle angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="6f990-205">You can stipulate a name, data type or even the order in which a column appears in the table.</span></span> <span data-ttu-id="6f990-206">Hier ist ein Beispiel für die Column-Attribut.</span><span class="sxs-lookup"><span data-stu-id="6f990-206">Here is an example of the Column attribute.</span></span>

``` csharp
    [Column("BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

<span data-ttu-id="6f990-207">Verwechseln Sie nicht die TypeName-Attribut mit dem DataType DataAnnotation Spalte.</span><span class="sxs-lookup"><span data-stu-id="6f990-207">Don’t confuse Column’s TypeName attribute with the DataType DataAnnotation.</span></span> <span data-ttu-id="6f990-208">Datentyp ist eine Anmerkung, die für die Benutzeroberfläche verwendet und wird durch Code First ignoriert.</span><span class="sxs-lookup"><span data-stu-id="6f990-208">DataType is an annotation used for the UI and is ignored by Code First.</span></span>

<span data-ttu-id="6f990-209">Hier ist die Tabelle auf, nachdem es erneut generiert wird.</span><span class="sxs-lookup"><span data-stu-id="6f990-209">Here is the table after it’s been regenerated.</span></span> <span data-ttu-id="6f990-210">Der Tabellenname InternalBlogs geändert hat, und Spalte "Beschreibung" aus dem komplexen Typ ist jetzt BlogDescription.</span><span class="sxs-lookup"><span data-stu-id="6f990-210">The table name has changed to InternalBlogs and Description column from the complex type is now BlogDescription.</span></span> <span data-ttu-id="6f990-211">Da der Name in der Anmerkung angegeben wurde, wird Code zunächst nicht die Konvention, starten Sie den Namen der Spalte mit dem Namen des komplexen Typs verwendet.</span><span class="sxs-lookup"><span data-stu-id="6f990-211">Because the name was specified in the annotation, code first will not use the convention of starting the column name with the name of the complex type.</span></span>

![Blogs-Tabelle und Spalte, die umbenannt](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a><span data-ttu-id="6f990-213">DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="6f990-213">DatabaseGenerated</span></span>

<span data-ttu-id="6f990-214">Eine wichtige Datenbankfunktionen ist die Möglichkeit, Eigenschaften berechnet haben.</span><span class="sxs-lookup"><span data-stu-id="6f990-214">An important database features is the ability to have computed properties.</span></span> <span data-ttu-id="6f990-215">Wenn Sie Ihren Code First Klassen, um eine Zuordnung sind Tabellen mit berechneten Spalten, Sie nicht möchten, Entity Framework, um zu versuchen, diese Spalten aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="6f990-215">If you're mapping your Code First classes to tables that contain computed columns, you don't want Entity Framework to try to update those columns.</span></span> <span data-ttu-id="6f990-216">Jedoch EF diese Werte aus der Datenbank zurück, nachdem Sie eingefügt oder aktualisiert Daten haben sollen.</span><span class="sxs-lookup"><span data-stu-id="6f990-216">But you do want EF to return those values from the database after you've inserted or updated data.</span></span> <span data-ttu-id="6f990-217">Die Anmerkung "databasegenerated" können Sie um die Eigenschaften in der Klasse zusammen mit der Enumeration berechnet zu kennzeichnen.</span><span class="sxs-lookup"><span data-stu-id="6f990-217">You can use the DatabaseGenerated annotation to flag those properties in your class along with the Computed enum.</span></span> <span data-ttu-id="6f990-218">Keine anderen Enumerationen sind und die Identität.</span><span class="sxs-lookup"><span data-stu-id="6f990-218">Other enums are None and Identity.</span></span>

``` csharp
    [DatabaseGenerated(DatabaseGeneratedOption.Computed)]
    public DateTime DateCreated { get; set; }
```

<span data-ttu-id="6f990-219">Sie können die Datenbank in Byte oder Timestamp-Spalten generiert wird, wenn Code zuerst die Datenbank generiert, andernfalls sollte nur verwendet, wenn zu vorhandenen Datenbanken verweisen, da der Code zunächst nicht die Formel für die berechnete Spalte bestimmen kann.</span><span class="sxs-lookup"><span data-stu-id="6f990-219">You can use database generated on byte or timestamp columns when code first is generating the database, otherwise you should only use this when pointing to existing databases because code first won't be able to determine the formula for the computed column.</span></span>

<span data-ttu-id="6f990-220">Sie lesen höher, in der Standardeinstellung wird eine wichtige Eigenschaft, die eine ganze Zahl ist ein Identitätsschlüssel in der Datenbank sein.</span><span class="sxs-lookup"><span data-stu-id="6f990-220">You read above that by default, a key property that is an integer will become an identity key in the database.</span></span> <span data-ttu-id="6f990-221">Das wäre identisch mit "databasegenerated" auf DatabaseGeneratedOption.Identity festlegen.</span><span class="sxs-lookup"><span data-stu-id="6f990-221">That would be the same as setting DatabaseGenerated to DatabaseGeneratedOption.Identity.</span></span> <span data-ttu-id="6f990-222">Wenn Sie einen Identitätsschlüssel werden nicht möchten, können Sie den Wert auf DatabaseGeneratedOption.None festlegen.</span><span class="sxs-lookup"><span data-stu-id="6f990-222">If you do not want it to be an identity key, you can set the value to DatabaseGeneratedOption.None.</span></span>

 

## <a name="index"></a><span data-ttu-id="6f990-223">Index</span><span class="sxs-lookup"><span data-stu-id="6f990-223">Index</span></span>

> [!NOTE]
> <span data-ttu-id="6f990-224">**EF6.1 oder höher, nur** -der Index-Attribut wurde in Entity Framework 6.1 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="6f990-224">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="6f990-225">Die Informationen in diesem Abschnitt gelten nicht, wenn Sie eine frühere Version verwenden.</span><span class="sxs-lookup"><span data-stu-id="6f990-225">If you are using an earlier version the information in this section does not apply.</span></span>

<span data-ttu-id="6f990-226">Sie können einen Index erstellen, auf eine oder mehrere Spalten mit den **IndexAttribute**.</span><span class="sxs-lookup"><span data-stu-id="6f990-226">You can create an index on one or more columns using the **IndexAttribute**.</span></span> <span data-ttu-id="6f990-227">Hinzufügen des Attributs auf eine oder mehrere Eigenschaften werden dazu führen, dass EF den entsprechenden Index in der Datenbank erstellen, wenn es sich um die Datenbank erstellt, oder Erstellen des Gerüsts für das entsprechende **CreateIndex** aufgerufen, wenn Sie Code First-Migrationen verwenden.</span><span class="sxs-lookup"><span data-stu-id="6f990-227">Adding the attribute to one or more properties will cause EF to create the corresponding index in the database when it creates the database, or scaffold the corresponding **CreateIndex** calls if you are using Code First Migrations.</span></span>

<span data-ttu-id="6f990-228">Der folgende Code führt z. B. in einem Index erstellt wird, auf die **Bewertung** Spalte die **Beiträge** Tabelle in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="6f990-228">For example, the following code will result in an index being created on the **Rating** column of the **Posts** table in the database.</span></span>

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index]
        public int Rating { get; set; }
        public int BlogId { get; set; }
    }
```

<span data-ttu-id="6f990-229">Standardmäßig wird der Index mit dem Namen werden **IX\_&lt;Eigenschaftennamen&gt;**  (IX\_Bewertung im obigen Beispiel).</span><span class="sxs-lookup"><span data-stu-id="6f990-229">By default, the index will be named **IX\_&lt;property name&gt;** (IX\_Rating in the above example).</span></span> <span data-ttu-id="6f990-230">Sie können jedoch auch einen Namen für den Index angeben.</span><span class="sxs-lookup"><span data-stu-id="6f990-230">You can also specify a name for the index though.</span></span> <span data-ttu-id="6f990-231">Das folgende Beispiel gibt an, dass der Index muss benannt werden **PostRatingIndex**.</span><span class="sxs-lookup"><span data-stu-id="6f990-231">The following example specifies that the index should be named **PostRatingIndex**.</span></span>

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

<span data-ttu-id="6f990-232">Indizes werden standardmäßig nicht eindeutig, jedoch können Sie die **IsUnique** benannter Parameter, um anzugeben, dass ein Index nicht eindeutig sein sollte.</span><span class="sxs-lookup"><span data-stu-id="6f990-232">By default, indexes are non-unique, but you can use the **IsUnique** named parameter to specify that an index should be unique.</span></span> <span data-ttu-id="6f990-233">Im folgende Beispiel führt einen eindeutigen Index auf eine **Benutzer**der Anmeldenamen ein.</span><span class="sxs-lookup"><span data-stu-id="6f990-233">The following example introduces a unique index on a **User**'s login name.</span></span>

``` csharp
    public class User
    {
        public int UserId { get; set; }

        [Index(IsUnique = true)]
        [StringLength(200)]
        public string Username { get; set; }

        public string DisplayName { get; set; }
    }
```

### <a name="multiple-column-indexes"></a><span data-ttu-id="6f990-234">Indizes für mehrere Spalten</span><span class="sxs-lookup"><span data-stu-id="6f990-234">Multiple-Column Indexes</span></span>

<span data-ttu-id="6f990-235">Indizes, die mehrere Spalten erstrecken, werden mit dem gleichen Namen in mehrere Index Anmerkungen für eine bestimmte Tabelle angegeben.</span><span class="sxs-lookup"><span data-stu-id="6f990-235">Indexes that span multiple columns are specified by using the same name in multiple Index annotations for a given table.</span></span> <span data-ttu-id="6f990-236">Bei der Erstellung mehrspaltiger Indexe müssen Sie einen Auftrag für die Spalten im Index anzugeben.</span><span class="sxs-lookup"><span data-stu-id="6f990-236">When you create multi-column indexes, you need to specify an order for the columns in the index.</span></span> <span data-ttu-id="6f990-237">Der folgende Code erstellt z. B. einen mehrspaltigen Index auf **Bewertung** und **BlogId** namens **IX\_BlogIdAndRating**.</span><span class="sxs-lookup"><span data-stu-id="6f990-237">For example, the following code creates a multi-column index on **Rating** and **BlogId** called **IX\_BlogIdAndRating**.</span></span> <span data-ttu-id="6f990-238">**BlogId** ist die erste Spalte im Index und **Bewertung** ist die zweite.</span><span class="sxs-lookup"><span data-stu-id="6f990-238">**BlogId** is the first column in the index and **Rating** is the second.</span></span>

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index("IX_BlogIdAndRating", 2)]
        public int Rating { get; set; }
        [Index("IX_BlogIdAndRating", 1)]
        public int BlogId { get; set; }
    }
```

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a><span data-ttu-id="6f990-239">Beziehungsattribute: InverseProperty und ForeignKey</span><span class="sxs-lookup"><span data-stu-id="6f990-239">Relationship Attributes: InverseProperty and ForeignKey</span></span>

> [!NOTE]
> <span data-ttu-id="6f990-240">Diese Seite enthält Informationen zum Einrichten der Beziehungen in Ihrem Code First-Modell mit Datenanmerkungen.</span><span class="sxs-lookup"><span data-stu-id="6f990-240">This page provides information about setting up relationships in your Code First model using Data Annotations.</span></span> <span data-ttu-id="6f990-241">Allgemeine Informationen zu Beziehungen in Entity Framework und das Zugreifen auf und Bearbeiten von Daten mithilfe von Beziehungen, finden Sie unter [Beziehungen und Navigationseigenschaften](~/ef6/fundamentals/relationships.md). \*</span><span class="sxs-lookup"><span data-stu-id="6f990-241">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).\*</span></span>

<span data-ttu-id="6f990-242">Erste Konvention Code übernimmt die am häufigsten verwendeten Beziehungen im Modell, aber es gibt einige Fälle, in dem Hilfe benötigt.</span><span class="sxs-lookup"><span data-stu-id="6f990-242">Code first convention will take care of the most common relationships in your model, but there are some cases where it needs help.</span></span>

<span data-ttu-id="6f990-243">Ändern den Namen der Schlüsseleigenschaft in der Blog-Klasse, die ein Problem mit der Beziehung mit Post erstellt.</span><span class="sxs-lookup"><span data-stu-id="6f990-243">Changing the name of the key property in the Blog class created a problem with its relationship to Post.</span></span> 

<span data-ttu-id="6f990-244">Zur Erstellung die Datenbank wird in Code zuerst sieht die BlogId-Eigenschaft in der Post-Klasse und erkennt, indem Sie die Konvention, dass sie einen Namen sowie den "Id", als Fremdschlüssel für die Blog-Klasse übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="6f990-244">When generating the database, code first sees the BlogId property in the Post class and recognizes it, by the convention that it matches a class name plus “Id”, as a foreign key to the Blog class.</span></span> <span data-ttu-id="6f990-245">Es gibt jedoch keine BlogId-Eigenschaft in der Blog-Klasse.</span><span class="sxs-lookup"><span data-stu-id="6f990-245">But there is no BlogId property in the blog class.</span></span> <span data-ttu-id="6f990-246">Die Lösung dafür besteht darin, eine Navigationseigenschaft in der POST-Anforderung erstellen und nutzen die Fremdschlüssel DataAnnotation beim Code zuerst zu verstehen, wie die Beziehung zwischen den beiden Klassen zu erstellen – mit der Eigenschaft Post.BlogId – sowie das Angeben von Einschränkungen in der die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="6f990-246">The solution for this is to create a navigation property in the Post and use the Foreign DataAnnotation to help code first understand how to build the relationship between the two classes —using the Post.BlogId property — as well as how to specify constraints in the database.</span></span>

``` csharp
    public class Post
    {
            public int Id { get; set; }
            public string Title { get; set; }
            public DateTime DateCreated { get; set; }
            public string Content { get; set; }
            public int BlogId { get; set; }
            [ForeignKey("BlogId")]
            public Blog Blog { get; set; }
            public ICollection<Comment> Comments { get; set; }
    }
```

<span data-ttu-id="6f990-247">Die Einschränkung in der Datenbank wird eine Beziehung zwischen InternalBlogs.PrimaryTrackingKey und Posts.BlogId gezeigt.</span><span class="sxs-lookup"><span data-stu-id="6f990-247">The constraint in the database shows a relationship between InternalBlogs.PrimaryTrackingKey and Posts.BlogId.</span></span> 

![Beziehung zwischen InternalBlogs.PrimaryTrackingKey und Posts.BlogId](~/ef6/media/jj591583-figure09.png)

<span data-ttu-id="6f990-249">Die InverseProperty wird verwendet, wenn Sie über mehrere Beziehungen zwischen Klassen verfügen.</span><span class="sxs-lookup"><span data-stu-id="6f990-249">The InverseProperty is used when you have multiple relationships between classes.</span></span>

<span data-ttu-id="6f990-250">In der Post-Klasse, sollten Sie zum Nachverfolgen, die einen Blogbeitrag geschrieben und, die sie bearbeitet.</span><span class="sxs-lookup"><span data-stu-id="6f990-250">In the Post class, you may want to keep track of who wrote a blog post as well as who edited it.</span></span> <span data-ttu-id="6f990-251">Hier sind zwei neue Navigationseigenschaften für die Post-Klasse.</span><span class="sxs-lookup"><span data-stu-id="6f990-251">Here are two new navigation properties for the Post class.</span></span>

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

<span data-ttu-id="6f990-252">Sie müssen auch die Person-Klasse, die auf die verwiesen wird anhand dieser Eigenschaften hinzu.</span><span class="sxs-lookup"><span data-stu-id="6f990-252">You’ll also need to add in the Person class referenced by these properties.</span></span> <span data-ttu-id="6f990-253">Die Person-Klasse verfügt über Navigationseigenschaften zurück an das Posting, eine für alle von den Beiträgen, die von "Person" und eine für alle der Beiträge aktualisiert, indem diese Person geschrieben wurde.</span><span class="sxs-lookup"><span data-stu-id="6f990-253">The Person class has navigation properties back to the Post, one for all of the posts written by the person and one for all of the posts updated by that person.</span></span>

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

<span data-ttu-id="6f990-254">Code first ist nicht mit übereinstimmen, werden die Eigenschaften in den beiden Klassen selbst.</span><span class="sxs-lookup"><span data-stu-id="6f990-254">Code first is not able to match up the properties in the two classes on its own.</span></span> <span data-ttu-id="6f990-255">Die Datenbanktabelle für Beiträge müssen ein Fremdschlüssel für die Person, die CreatedBy-Feld und eine für die Person, die UpdatedBy jedoch zuerst erstellt vier Fremdschlüsseleigenschaften: Person\_-Id, Person\_"id1", CreatedBy\_-Id und UpdatedBy\_Id.</span><span class="sxs-lookup"><span data-stu-id="6f990-255">The database table for Posts should have one foreign key for the CreatedBy person and one for the UpdatedBy person but code first will create four foreign key properties: Person\_Id, Person\_Id1, CreatedBy\_Id and UpdatedBy\_Id.</span></span>

![Tabelle mit Fremdschlüsseln, zusätzliche Beiträge](~/ef6/media/jj591583-figure10.png)

<span data-ttu-id="6f990-257">Um diese Probleme zu beheben, können Sie die Anmerkung InverseProperty verwenden, die Ausrichtung der Eigenschaften an.</span><span class="sxs-lookup"><span data-stu-id="6f990-257">To fix these problems, you can use the InverseProperty annotation to specify the alignment of the properties.</span></span>

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

<span data-ttu-id="6f990-258">Da die PostsWritten-Eigenschaft in der Person weiß, dass dies auf der Post-Typ bezieht, wird die Beziehung zu Post.CreatedBy erstellt.</span><span class="sxs-lookup"><span data-stu-id="6f990-258">Because the PostsWritten property in Person knows that this refers to the Post type, it will build the relationship to Post.CreatedBy.</span></span> <span data-ttu-id="6f990-259">Auf ähnliche Weise wird die PostsUpdated mit Post.UpdatedBy verbunden sein.</span><span class="sxs-lookup"><span data-stu-id="6f990-259">Similarly, PostsUpdated will be connected to Post.UpdatedBy.</span></span> <span data-ttu-id="6f990-260">Und Code erstellt zunächst keine zusätzliche Fremdschlüssel.</span><span class="sxs-lookup"><span data-stu-id="6f990-260">And code first will not create the extra foreign keys.</span></span>

![Tabelle ohne zusätzliche Fremdschlüssel Beiträge](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a><span data-ttu-id="6f990-262">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="6f990-262">Summary</span></span>

<span data-ttu-id="6f990-263">"DataAnnotations" können nicht nur Sie Client- und clientseitige Validierung in Ihren Code First-Klassen beschreiben, aber auch können Sie zu verbessern und noch korrigieren Sie die Annahmen, die Code zunächst zu Ihrer Klassen basierend auf dessen Konventionen bilden.</span><span class="sxs-lookup"><span data-stu-id="6f990-263">DataAnnotations not only let you describe client and server side validation in your code first classes, but they also allow you to enhance and even correct the assumptions that code first will make about your classes based on its conventions.</span></span> <span data-ttu-id="6f990-264">Mit "DataAnnotations" Sie können nicht nur Laufwerk schemagenerierung für die Datenbank, aber Sie können auch Ihren Code First-Klassen in einer bereits vorhandenen Datenbank zuordnen.</span><span class="sxs-lookup"><span data-stu-id="6f990-264">With DataAnnotations you can not only drive database schema generation, but you can also map your code first classes to a pre-existing database.</span></span>

<span data-ttu-id="6f990-265">Während sie äußerst flexibel sind, sollten Sie bedenken, die "DataAnnotations" bereitstellen, nur die am häufigsten häufig Änderungen an der Konfiguration benötigt, die Sie Ihren Code First-Klassen vornehmen können.</span><span class="sxs-lookup"><span data-stu-id="6f990-265">While they are very flexible, keep in mind that DataAnnotations provide only the most commonly needed configuration changes you can make on your code first classes.</span></span> <span data-ttu-id="6f990-266">Um die Klassen für einige der die Grenzfälle zu konfigurieren, sollten Sie die Alternativkonfiguration-Mechanismus, Code First Fluent-API ansehen.</span><span class="sxs-lookup"><span data-stu-id="6f990-266">To configure your classes for some of the edge cases, you should look to the alternate configuration mechanism, Code First’s Fluent API .</span></span>
