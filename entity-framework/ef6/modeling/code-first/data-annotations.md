---
title: Der erste Datenanmerkungen – EF6 Code
author: divega
ms.date: 10/23/2016
ms.assetid: 80abefbd-23c9-4fce-9cd3-520e5df9856e
ms.openlocfilehash: 54e27f1b866da14d68db66ca5eca5a6dde819e26
ms.sourcegitcommit: 15022dd06d919c29b1189c82611ea32f9fdc6617
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2018
ms.locfileid: "47415808"
---
# <a name="code-first-data-annotations"></a>Code der ersten Datenanmerkungen
> [!NOTE]
> **Ef4. 1 oder höher, nur** -APIs, die Funktionen erläutert, die auf dieser Seite usw. in Entity Framework 4.1 eingeführt wurden. Wenn Sie eine frühere Version verwenden, gilt einige oder alle diese Informationen nicht.

Der Inhalt auf dieser Seite ist aus einem Artikel, die ursprünglich von Julie Lerman geschrieben (\<http://thedatafarm.com>).

Entity Framework Code First können Sie Ihre eigenen Domänenklassen verwenden, um die Darstellung des Modells, dem zum Ausführen von Abfragen verwendet EF ändern, nachverfolgen und Aktualisieren von Funktionen. Code nutzt zuerst ein spezifisches programmierschema bezeichnet als "Konvention geht vor Konfiguration." Code wird zunächst davon ausgegangen, dass Ihre Klassen den Konventionen von Entity Framework folgen und in diesem Fall automatisch, Sie funktionsfähig, wie Sie es geleistet. Wenn Ihre Klassen diese Konventionen nicht befolgen, müssen Sie jedoch die Möglichkeit, die auf Ihre Klassen EF mit den erforderlichen Informationen zu Konfigurationen hinzufügen.

Code können Sie zuerst zwei Möglichkeiten, diese Konfigurationen auf Ihre Klassen hinzuzufügen. Ist eine einfache Attribute, die Namen "DataAnnotations" verwenden, und die zweite die Verwendung von Code First Fluent-API, die Ihnen eine Möglichkeit, Konfigurationen imperativ im Code beschreiben bereitstellt.

Dieser Artikel konzentriert sich auf die Verwendung von "DataAnnotations" (in der System.ComponentModel.DataAnnotations-Namespace) so konfigurieren Sie Ihre Klassen – markieren die am häufigsten verwendeten Konfigurationen. "DataAnnotations" werden auch durch eine Anzahl von .NET-Anwendungen, wie ASP.NET MVC verstanden, dadurch kann diese Anwendungen nutzen dieselben-Anmerkungen für die clientseitige Überprüfungen.


## <a name="the-model"></a>Das Modell

Ich zeige Ihnen, Code erste DataAnnotations mit ein paar einfache Klassen: Blog und Post.

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

Als dies der Fall, führen Sie erste Konvention Code der Blog und Post-Klasse einfach und erfordern keine Anpassungen an EF-Kompatibilität aktivieren. Allerdings auch können die Anmerkungen Sie zusätzliche Informationen für EF bereit, über die Klassen und der Datenbank, die sie zugeordnet.

 

## <a name="key"></a>Key

Entitätsframework basiert auf jede Entität an, dass einen Schlüssel-Wert, der für die Entität, die nachverfolgung verwendet wird. Eine Konvention für die Code First ist die implizite Schlüsseleigenschaften. Code sucht zuerst nach einer Eigenschaft mit dem Namen "Id" oder eine Kombination von Klassennamen und "Id", wie z. B. "BlogId". Diese Eigenschaft wird auf eine primäre Schlüsselspalte in der Datenbank zugeordnet.

Den Blog "und" Post beide Klassen entsprechen dieser Konvention. Was geschieht, wenn sie nicht? Was geschieht, wenn der Name von Blog verwendet *PrimaryTrackingKey* stattdessen oder sogar *"Foo"*? Wenn der Code zuerst eine Eigenschaft nicht finden kann, die diese Konvention entspricht löst er eine Ausnahme aufgrund des Entity Framework-Anforderung, dass Sie eine Schlüsseleigenschaft verfügen müssen. Sie können die wichtigsten Anmerkung verwenden, um anzugeben, welche Eigenschaft als EntityKey verwendet werden soll.

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

Wenn Sie sind mithilfe von Code first Funktion zum Generieren von Datenbanken ist, wird die Tabelle "Blog" eine Primärschlüsselspalte namens PrimaryTrackingKey, die standardmäßig auch als Identität definiert ist.

![Blog-Tabelle mit Primärschlüssel](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a>Zusammengesetzte Schlüssel

Zusammengesetzte Schlüssel - Primärschlüssel, die aus mehr als eine Eigenschaft bestehen, Entitätsframework unterstützt. Beispielsweise kann eine Passport-Klasse haben, deren Primärschlüssel eine Kombination von PassportNumber und IssuingCountry ist.

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

Würde es wird versucht, die oben genannten Klasse in Ihr EF-Modell verwenden eine `InvalidOperationException`:

*Kann nicht bestimmt zusammengesetzte primary Key Sortierung für den Typ "Passport". Verwenden Sie die ColumnAttribute oder die HasKey-Methode, um einen Auftrag für den zusammengesetzten Primärschlüsseln anzugeben.*

Um zusammengesetzte Schlüssel verwenden zu können, erfordert Entity Framework Sie einen Auftrag für die wichtigsten Eigenschaften zu definieren. Sie erreichen dies, indem Sie die Anmerkung für die Spalte eine Reihenfolge angegeben.

>[!NOTE]
> Den Wert der ist relativ, (und nicht der Index basiert), damit alle Werte verwendet werden können. Beispielsweise würden 100 und 200 anstelle von 1 und 2 akzeptabel sein.

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

Wenn Sie Entitäten mit zusammengesetzter Fremdschlüssel haben, müssen Sie angeben die gleiche Spalte sortieren, die Sie für die entsprechenden Eigenschaften des primären Schlüssels verwendet.

Nur die relative Reihenfolge in der Fremdschlüsseleigenschaften muss übereinstimmen, die genauen Werte, die für **Reihenfolge** brauchen nicht übereinzustimmen. Beispielsweise konnte in der folgenden Klasse 3 und 4 anstelle von 1 und 2 verwendet werden.

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

## <a name="required"></a>Erforderlich

Die erforderlichen Anmerkung wird Entity Framework angewiesen, dass eine bestimmte Eigenschaft erforderlich ist.

Hinzufügen von erforderlich, um die Title-Eigenschaft wird erzwungen, EF (und MVC), um sicherzustellen, dass die Eigenschaft bereits Daten enthält.

``` csharp
    [Required]
    public string Title { get; set; }
```

Keine zusätzlichen ohne Änderungen des Codes oder Markups in der Anwendung, in eine MVC-Anwendung führt Validierung auf Clientseite, sogar dynamisch Erstellen einer Nachricht, die unter Verwendung der Eigenschaft und der Anmerkung.

![Erstellen Sie Seite mit dem Titel ist erforderlich, Fehler](~/ef6/media/jj591583-figure02.png)

Das Required-Attribut wirkt außerdem generierte Datenbank durch Festlegen der zugeordnete Eigenschaft keine NULL-Werte zulässt. Beachten Sie, dass das Feld "Titel", "not null" geändert hat.

>[!NOTE]
> In einigen Fällen kann es nicht möglich für die Spalte in der Datenbank NULL-Werte zulässt, obwohl die Eigenschaft erforderlich ist. Beispielsweise wird bei Verwendung einer TPH-Vererbung Strategie für die Datentyps für mehrere Typen in einer einzelnen Tabelle gespeichert. Wenn ein abgeleiteter Typ eine erforderliche Eigenschaft, die die Spalte NULL-hergestellt werden kann enthält, da nicht alle Typen in der Hierarchie dieser Eigenschaft hat.

 

![Blogs-Tabelle](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a>MaxLength und MinLength

Die Attribute MaxLength und MinLength ermöglichen Ihnen die Angabe der zusätzlichen Eigenschaft-Überprüfungen, genauso wie Sie mit erforderlich.

Hier ist die BloggerName mit Anforderungen im Hinblick auf. Darüber hinaus wird veranschaulicht, wie Sie die Attribute zu kombinieren.

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

Die MaxLength-Anmerkung wird die Datenbank durch Festlegen der Eigenschaft Länge auf 10 beeinträchtigt.

![Blogs-Tabelle, die mit die maximale Länge für BloggerName-Spalte](~/ef6/media/jj591583-figure04.png)

MVC-Client-Side-Anmerkung, und EF 4.1 serverseitige Anmerkung sowohl berücksichtigt dieser Überprüfung erneut dynamisch erstellen eine Fehlermeldung angezeigt: "das Feld BloggerName muss eine Zeichenfolge oder Array-Typ mit einer maximalen Länge von '10'." Diese Meldung ist ein wenig lang. Viele Anmerkungen können Sie eine Fehlermeldung mit dem ErrorMessage-Attribut angeben.

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

Sie können auch in der erforderlichen Anmerkung ErrorMessage angeben.

![Erstellen Sie die Seite mit benutzerdefinierten Fehlermeldung.](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a>NotMapped

Erste Konvention Code schreibt vor, dass jede Eigenschaft, die einen unterstützten Datentyp ist in der Datenbank dargestellt wird. Dies ist jedoch immer der Fall, in Ihren Anwendungen. Z. B. können Sie in der Blog-Klasse, die einen Code, der basierend auf den Titel und BloggerName Feldern erstellt, eine Eigenschaft verfügen. Diese Eigenschaft kann dynamisch erstellt werden und muss nicht gespeichert werden. Sie können alle Eigenschaften, die kennzeichnen, die nicht mit der Datenbank mit der Anmerkung NotMapped, z. B. diese BlogCode-Eigenschaft zugeordnet sind.

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

 

## <a name="complextype"></a>ComplexType

Es ist nicht ungewöhnlich, dass Ihre Domänenentitäten über einen Satz von Klassen zu beschreiben, und klicken Sie dann layer diese Klassen zum Beschreiben einer vollständigen Entität. Beispielsweise können Sie eine Klasse namens BlogDetails für das Modell hinzufügen.

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

Beachten Sie, dass BlogDetails keine Art von "Key"-Eigenschaft verfügt. BlogDetails wird in Domain-driven Design als Wertobjekt bezeichnet. Entitätsframework bezeichnet Wertobjekte als komplexe Typen.  Komplexe Typen können nicht auf ihre eigenen nachverfolgt werden.

Jedoch als Eigenschaft in der Blog-Klasse BlogDetails, die sie als Teil einer Blog-Objekts nachverfolgt. Damit Code, um dies zu erkennen müssen Sie die BlogDetails-Klasse als ComplexType markieren.

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

Jetzt können Sie eine Eigenschaft in der Blog-Klasse zur Darstellung der BlogDetails für dieses Blog hinzufügen.

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

In der Datenbank enthält die Tabelle "Blog" alle Eigenschaften des Blogs, einschließlich der Eigenschaften, die in der BlogDetail-Eigenschaft enthalten. Standardmäßig wird jede mit dem Namen des komplexen Typs, BlogDetail vorangestellt.

![Tabelle "Blog" mit komplexen Typen](~/ef6/media/jj591583-figure06.png)

Ein weiterer interessanter Hinweis ist, dass auch die DateCreated-Eigenschaft als null-DateTime in der Klasse definiert wurde, das entsprechende Datenbankfeld mit NULL-Werte zulässt. Sie müssen die erforderlichen Anmerkung verwenden, wenn Sie, um das Datenbankschema zu beeinflussen möchten.

 

## <a name="concurrencycheck"></a>ConcurrencyCheck

Die Anmerkung ConcurrencyCheck können Sie so kennzeichnen Sie eine oder mehrere Eigenschaften für die parallelitätsprüfung in der Datenbank, wenn ein Benutzer bearbeitet oder eine Entität löscht verwendet werden soll. Wenn Sie mit dem EF Designer gearbeitet haben, entspricht dies durch Festlegen einer Eigenschaft ConcurrencyMode auf fest.

Sehen wir uns an, wie ConcurrencyCheck funktioniert, indem sie auf die BloggerName-Eigenschaft hinzufügen.

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

Wenn SaveChanges, aufgrund der ConcurrencyCheck-Anmerkung auf das Feld BloggerName aufgerufen wird wird der ursprüngliche Wert der Eigenschaft in dem Update verwendet werden. Der Befehl versucht, die richtige Zeile suchen, filtern, nicht nur auf die Schlüssel-Wert, sondern auch auf den ursprünglichen Wert von BloggerName.  Hier sind die wichtigen Teile der UPDATE-Befehl gesendet, um die Datenbank, hier sehen Sie der Befehl aktualisiert die Zeile mit einem PrimaryTrackingKey ist 1 und eine BloggerName von "Julie", das den ursprünglichen Wert wurde bei diesem Blog aus der Datenbank abgerufen wurde.

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

Wenn jemand den Blogger-Namen für dieses Blogs in der Zwischenzeit geändert wurde, wird dieses Update schlägt fehl, und Sie erhalten eine DbUpdateConcurrencyException, die Sie behandeln müssen.

 

## <a name="timestamp"></a>Zeitstempel

Es ist üblich, ein Rowversion oder Timestamp-Felder für die parallelitätsüberprüfung zu verwenden. Aber statt der ConcurrencyCheck-Anmerkung zu verwenden, können Sie die spezifischere TimeStamp-Anmerkung, solange der Typ der Eigenschaft Byte-Array ist. Code zuerst behandelt Timestamp-Eigenschaften identisch als ConcurrencyCheck-Eigenschaften, aber es wird auch sichergestellt, dass die Datenbankfeld, das Code zuerst erzeugt NULL-Werte zulässt. Sie können nur eine Timestamp-Eigenschaft in einer bestimmten Klasse haben.

Der Blog-Klasse hinzugefügt die folgende Eigenschaft:

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

die Ergebnisse im Code erstellen zunächst eine NULL-Timestamp-Spalte in der Datenbanktabelle.

![Blogs-Tabelle mit timestamp-Spalte](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a>Tabellen- und Spaltennamen

Wenn Sie Code First die Datenbank erstellen können, empfiehlt es sich, ändern Sie den Namen der Tabellen und Spalten, die sie erstellt wird. Sie können Code First auch mit einer vorhandenen Datenbank verwenden. Es ist jedoch nicht immer der Fall, dass die Namen von Klassen und Eigenschaften in der Domäne den Namen der Tabellen und Spalten in der Datenbank übereinstimmen.

Meine Klasse heißt Blog und gemäß der Konvention Code zuerst wird davon ausgegangen, dass dies in eine Tabelle namens Blogs zugeordnet wird. Wenn dies nicht der Fall ist, können Sie den Namen der Tabelle mit dem Attribut für die Tabelle angeben. Hier ist z. B. die Anmerkung gibt an, dass der Tabellenname InternalBlogs ist.

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

Die Anmerkung für die Spalte ist eine weitere fundierte Kenntnisse in die Attribute der zugeordneten Spalte angeben. Sie können festlegen, einen Namen, Datentyp und sogar die Reihenfolge, in der eine Spalte in der Tabelle angezeigt wird. Hier ist ein Beispiel für die Column-Attribut.

``` csharp
    [Column(“BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

Verwechseln Sie nicht die TypeName-Attribut mit dem DataType DataAnnotation Spalte. Datentyp ist eine Anmerkung, die für die Benutzeroberfläche verwendet und wird durch Code First ignoriert.

Hier ist die Tabelle auf, nachdem es erneut generiert wird. Der Tabellenname InternalBlogs geändert hat, und Spalte "Beschreibung" aus dem komplexen Typ ist jetzt BlogDescription. Da der Name in der Anmerkung angegeben wurde, wird Code zunächst nicht die Konvention, starten Sie den Namen der Spalte mit dem Namen des komplexen Typs verwendet.

![Blogs-Tabelle und Spalte, die umbenannt](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a>"Databasegenerated"

Eine wichtige Datenbankfunktionen ist die Möglichkeit, Eigenschaften berechnet haben. Wenn Sie Ihren Code First Klassen, um eine Zuordnung sind Tabellen mit berechneten Spalten, Sie nicht möchten, Entity Framework, um zu versuchen, diese Spalten aktualisieren. Jedoch EF diese Werte aus der Datenbank zurück, nachdem Sie eingefügt oder aktualisiert Daten haben sollen. Die Anmerkung "databasegenerated" können Sie um die Eigenschaften in der Klasse zusammen mit der Enumeration berechnet zu kennzeichnen. Keine anderen Enumerationen sind und die Identität.

``` csharp
    [DatabaseGenerated(DatabaseGeneratedOption.Computed)]
    public DateTime DateCreated { get; set; }
```

Sie können die Datenbank in Byte oder Timestamp-Spalten generiert wird, wenn Code zuerst die Datenbank generiert, andernfalls sollte nur verwendet, wenn zu vorhandenen Datenbanken verweisen, da der Code zunächst nicht die Formel für die berechnete Spalte bestimmen kann.

Sie lesen höher, in der Standardeinstellung wird eine wichtige Eigenschaft, die eine ganze Zahl ist ein Identitätsschlüssel in der Datenbank sein. Das wäre identisch mit "databasegenerated" auf DatabaseGeneratedOption.Identity festlegen. Wenn Sie einen Identitätsschlüssel werden nicht möchten, können Sie den Wert auf DatabaseGeneratedOption.None festlegen.

 

## <a name="index"></a>Index

> [!NOTE]
> **EF6.1 oder höher, nur** -der Index-Attribut wurde in Entity Framework 6.1 eingeführt. Die Informationen in diesem Abschnitt gelten nicht, wenn Sie eine frühere Version verwenden.

Sie können einen Index erstellen, auf eine oder mehrere Spalten mit den **IndexAttribute**. Hinzufügen des Attributs auf eine oder mehrere Eigenschaften werden dazu führen, dass EF den entsprechenden Index in der Datenbank erstellen, wenn es sich um die Datenbank erstellt, oder Erstellen des Gerüsts für das entsprechende **CreateIndex** aufgerufen, wenn Sie Code First-Migrationen verwenden.

Der folgende Code führt z. B. in einem Index erstellt wird, auf die **Bewertung** Spalte die **Beiträge** Tabelle in der Datenbank.

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

Standardmäßig wird der Index mit dem Namen werden **IX\_&lt;Eigenschaftennamen&gt;**  (IX\_Bewertung im obigen Beispiel). Sie können jedoch auch einen Namen für den Index angeben. Das folgende Beispiel gibt an, dass der Index muss benannt werden **PostRatingIndex**.

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

Indizes werden standardmäßig nicht eindeutig, jedoch können Sie die **IsUnique** benannter Parameter, um anzugeben, dass ein Index nicht eindeutig sein sollte. Im folgende Beispiel führt einen eindeutigen Index auf eine **Benutzer**der Anmeldenamen ein.

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

### <a name="multiple-column-indexes"></a>Indizes für mehrere Spalten

Indizes, die mehrere Spalten erstrecken, werden mit dem gleichen Namen in mehrere Index Anmerkungen für eine bestimmte Tabelle angegeben. Bei der Erstellung mehrspaltiger Indexe müssen Sie einen Auftrag für die Spalten im Index anzugeben. Der folgende Code erstellt z. B. einen mehrspaltigen Index auf **Bewertung** und **BlogId** namens **IX\_BlogAndRating**. **BlogId** ist die erste Spalte im Index und **Bewertung** ist die zweite.

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

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a>Beziehung Attribute: InverseProperty und ForeignKey für

> [!NOTE]
> Diese Seite enthält Informationen zum Einrichten der Beziehungen in Ihrem Code First-Modell mit Datenanmerkungen. Allgemeine Informationen zu Beziehungen in Entity Framework und das Zugreifen auf und Bearbeiten von Daten mithilfe von Beziehungen, finden Sie unter [Beziehungen und Navigationseigenschaften](~/ef6/fundamentals/relationships.md). *

Erste Konvention Code übernimmt die am häufigsten verwendeten Beziehungen im Modell, aber es gibt einige Fälle, in dem Hilfe benötigt.

Ändern den Namen der Schlüsseleigenschaft in der Blog-Klasse, die ein Problem mit der Beziehung mit Post erstellt. 

Zur Erstellung die Datenbank wird in Code zuerst sieht die BlogId-Eigenschaft in der Post-Klasse und erkennt, indem Sie die Konvention, dass sie einen Namen sowie den "Id", als Fremdschlüssel für die Blog-Klasse übereinstimmt. Es gibt jedoch keine BlogId-Eigenschaft in der Blog-Klasse. Die Lösung dafür besteht darin, eine Navigationseigenschaft in der POST-Anforderung erstellen und nutzen die Fremdschlüssel DataAnnotation beim Code zuerst zu verstehen, wie die Beziehung zwischen den beiden Klassen zu erstellen – mit der Eigenschaft Post.BlogId – sowie das Angeben von Einschränkungen in der die Datenbank.

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

Die Einschränkung in der Datenbank wird eine Beziehung zwischen InternalBlogs.PrimaryTrackingKey und Posts.BlogId gezeigt. 

![Beziehung zwischen InternalBlogs.PrimaryTrackingKey und Posts.BlogId](~/ef6/media/jj591583-figure09.png)

Die InverseProperty wird verwendet, wenn Sie über mehrere Beziehungen zwischen Klassen verfügen.

In der Post-Klasse, sollten Sie zum Nachverfolgen, die einen Blogbeitrag geschrieben und, die sie bearbeitet. Hier sind zwei neue Navigationseigenschaften für die Post-Klasse.

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

Sie müssen auch die Person-Klasse, die auf die verwiesen wird anhand dieser Eigenschaften hinzu. Die Person-Klasse verfügt über Navigationseigenschaften zurück an das Posting, eine für alle von den Beiträgen, die von "Person" und eine für alle der Beiträge aktualisiert, indem diese Person geschrieben wurde.

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

Code first ist nicht mit übereinstimmen, werden die Eigenschaften in den beiden Klassen selbst. Die Datenbanktabelle für Beiträge müssen ein Fremdschlüssel für die Person CreatedBy-Feld und eine für die UpdatedBy Person aber der Code erstellt zunächst vier werden Fremdschlüsseleigenschaften: Person\_-Id, Person\_"id1", CreatedBy\_Id und UpdatedBy\_Id.

![Tabelle mit Fremdschlüsseln, zusätzliche Beiträge](~/ef6/media/jj591583-figure10.png)

Um diese Probleme zu beheben, können Sie die Anmerkung InverseProperty verwenden, die Ausrichtung der Eigenschaften an.

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

Da die PostsWritten-Eigenschaft in der Person weiß, dass dies auf der Post-Typ bezieht, wird die Beziehung zu Post.CreatedBy erstellt. Auf ähnliche Weise wird die PostsUpdated mit Post.UpdatedBy verbunden sein. Und Code erstellt zunächst keine zusätzliche Fremdschlüssel.

![Tabelle ohne zusätzliche Fremdschlüssel Beiträge](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a>Zusammenfassung

"DataAnnotations" können nicht nur Sie Client- und clientseitige Validierung in Ihren Code First-Klassen beschreiben, aber auch können Sie zu verbessern und noch korrigieren Sie die Annahmen, die Code zunächst zu Ihrer Klassen basierend auf dessen Konventionen bilden. Mit "DataAnnotations" Sie können nicht nur Laufwerk schemagenerierung für die Datenbank, aber Sie können auch Ihren Code First-Klassen in einer bereits vorhandenen Datenbank zuordnen.

Während sie äußerst flexibel sind, sollten Sie bedenken, die "DataAnnotations" bereitstellen, nur die am häufigsten häufig Änderungen an der Konfiguration benötigt, die Sie Ihren Code First-Klassen vornehmen können. Um die Klassen für einige der die Grenzfälle zu konfigurieren, sollten Sie die Alternativkonfiguration-Mechanismus, Code First Fluent-API ansehen.
