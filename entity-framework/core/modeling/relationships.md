---
title: "Beziehungen – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
ms.technology: entity-framework-core
uid: core/modeling/relationships
ms.openlocfilehash: 1732d32643effb0f12111191ed4ba3abb4182d93
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="relationships"></a>Beziehungen

Eine Beziehung definiert, wie zwei Entitäten aufeinander beziehen. In einer relationalen Datenbank wird dies durch eine foreign Key-Einschränkung dargestellt.

> [!NOTE]  
> Die meisten der Beispiele in diesem Artikel verwenden eine 1: n-Beziehung Konzepte verdeutlicht. Beispiele für 1: 1- und m: n-Beziehungen finden Sie die [andere Beziehung Muster](#other-relationship-patterns) Abschnitt am Ende des Artikels.

## <a name="definition-of-terms"></a>Begriffsdefinition

Es gibt eine Reihe von Begriffen, die zum Beschreiben von Beziehungen

* **Abhängige Entität:** Dies ist die Entität, die die foreign Schlüsseleigenschaften enthält. Manchmal bezeichnet als "Child" der Beziehung.

* **Prinzipalentität:** Dies ist die Entität, die primäre/alternativen Schlüsseleigenschaften enthält. Manchmal bezeichnet als "Parent" der Beziehung.

* **Fremdschlüssel:** der Eigenschaft(en) in der abhängigen Entität ab, das die Werte der Prinzipal Schlüsseleigenschaft zu speichern, die mit die Entität verknüpft ist.

* **Principal-Schlüssel:** der Eigenschaft(en), die die prinzipalentität eindeutig identifiziert. Dies kann die primary key- oder ein alternativer Schlüssel sein.

* **Navigationseigenschaft:** eine Eigenschaft für die principal und/oder abhängige Entität mit einer Verweise auf die zugehörige Entity(s) definiert.

  * **Auflistungsnavigationseigenschaft:** eine Navigationseigenschaft, die Verweise auf viele verknüpfte Entitäten enthält.

  * **Navigationseigenschaft verweisen:** eine Navigationseigenschaft, die einen Verweis auf eine einzelne verknüpfte Entität enthält.

  * **Inverse Navigationseigenschaft:** im Zusammenhang mit eine bestimmten Navigationseigenschaft dieser Begriff bezieht sich, mit der Navigationseigenschaft am anderen Ende der Beziehung.

Der folgende Code zeigt einer 1: n-Beziehung zwischen `Blog` und`Post`

* `Post`die abhängige Entität ist

* `Blog`ist die prinzipalentität

* `Post.BlogId`ist der Fremdschlüssel

* `Blog.BlogId`ist der Dienstprinzipalname-Schlüssel (in diesem Fall ist es ein alternativer Schlüssel, statt einen Primärschlüssel)

* `Post.Blog`ist eine Navigationseigenschaft Verweis

* `Blog.Posts`ist eine auflistungsnavigationseigenschaft

* `Post.Blog`wird von die inversen Navigationseigenschaft `Blog.Posts` (und umgekehrt)

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a>Konventionen

Gemäß der Konvention wird eine Beziehung erstellt werden, wenn es eine Navigationseigenschaft, die für einen Typ ermittelt ist. Eine Eigenschaft ist eine Navigationseigenschaft berücksichtigt, wenn der Typ, auf die verwiesen, als einen skalaren Typ durch den aktuellen Anbieter nicht zugeordnet werden kann.

> [!NOTE]  
> Beziehungen, die gemäß der Konvention ermittelt werden, werden immer den Primärschlüssel der prinzipalentität abzielen. Um einen alternativen Schlüssel als Ziel festzulegen, muss zusätzliche Konfiguration mithilfe der Fluent-API ausgeführt werden.

### <a name="fully-defined-relationships"></a>Vollständig definierten Beziehungen

Das bekannteste Muster für Beziehungen werden Navigationseigenschaften, die an beiden Enden der Beziehung und eine Fremdschlüsseleigenschaft definiert, in der abhängigen Entität-Klasse definiert haben.

* Wenn ein Paar von Navigationseigenschaften zwischen zwei Typen gefunden wird, werden sie als inverse Navigationseigenschaften derselben Beziehung modellieren konfiguriert werden.

* Wenn die abhängige Entität eine Eigenschaft namens enthält `<primary key property name>`, `<navigation property name><primary key property name>`, oder `<principal entity name><primary key property name>` und dann als Fremdschlüssel konfiguriert wird.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> Wenn es mehrere Navigationseigenschaften, die zwischen zwei Typen definiert sind (d. h. mehr als ein distinct Spaltenpaar Navigationen, die aufeinander zeigen), gemäß der Konvention werden dann keine Beziehungen erstellt werden, und Sie müssen manuell konfigurieren, um sich zu identifizieren wie die Navigationseigenschaften koppeln einrichten.

### <a name="no-foreign-key-property"></a>Keine Foreign Key-Eigenschaft

Zwar wird empfohlen, eine Fremdschlüsseleigenschaft in der abhängigen Entität-Klasse definiert haben, ist es nicht erforderlich. Wenn keine foreign Key-Eigenschaft gefunden wird, wird eine Fremdschlüsseleigenschaft Schatten eingeleitet werden, mit dem Namen `<navigation property name><principal key property name>` (siehe [Shadowing von Eigenschaften](shadow-properties.md) für Weitere Informationen).

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a>Einzelne Navigationseigenschaft

Nur eine Navigationseigenschaft (keine umgekehrte Navigation und keine Fremdschlüsseleigenschaft) einschließlich ist ausreichend, um eine Beziehung definiert, die gemäß der Konvention verfügen. Sie können auch eine einzelne Navigationseigenschaft und eine foreign Key-Eigenschaft verfügen.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a>Kaskadierte Löschung

Gemäß der Konvention kaskadierte Löschung festgelegt, um *Cascade* für die erforderlichen Beziehungen und *ClientSetNull* für optionale Beziehungen. *CASCADE* bedeutet, dass abhängige Entitäten werden ebenfalls gelöscht. *ClientSetNull* bedeutet, der abhängige Entitäten, die nicht in den Arbeitsspeicher geladen werden verbleibt unverändert und müssen manuell gelöscht oder aktualisiert, um auf eine gültige Prinzipal Entität verweisen. Für Entitäten, die in den Arbeitsspeicher geladen werden, versucht EF Core, die Fremdschlüsseleigenschaften auf null festgelegt.

Finden Sie unter der [erforderlichen und optionalen Beziehungen](#required-and-optional-relationships) Abschnitt, um den Unterschied zwischen der erforderlichen und optionalen Beziehungen.

Finden Sie unter [Löschweitergabe](../saving/cascade-delete.md) für ausführliche Informationen zu den verschiedenen Verhalten und die Standardwerte, die gemäß der Konvention verwendet löschen.

## <a name="data-annotations"></a>Datenanmerkungen

Es gibt zwei datenanmerkungen, die verwendet werden können, so konfigurieren Sie Beziehungen, `[ForeignKey]` und `[InverseProperty]`.

### <a name="foreignkey"></a>[ForeignKey]

Die Datenanmerkungen können Sie konfigurieren, welche Eigenschaft als foreign Key-Eigenschaft für eine bestimmte Beziehung verwendet werden soll. Dies erfolgt in der Regel zusammen mit der foreign Key-Eigenschaft gemäß der Konvention nicht ermittelt werden.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?name=Entities&highlight=17)]

> [!TIP]  
> Die `[ForeignKey]` Anmerkung für eine Navigationseigenschaft in der Beziehung platziert werden kann. Es muss nicht wechseln auf die Navigationseigenschaft, die in der abhängigen Entität-Klasse.

### <a name="inverseproperty"></a>[InverseProperty]

Die Anmerkungen Daten können Sie konfigurieren, wie Navigationseigenschaften für Entitäten abhängigen und koppeln einrichten. Dies erfolgt normalerweise, wenn mehr als ein Paar von Navigationseigenschaften zwischen zwei Entitätstypen wird.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?name=Entities&highlight=20,23)]

## <a name="fluent-api"></a>Fluent-API

Um eine Beziehung in der Fluent-API konfigurieren möchten, starten Sie durch Identifizieren der Navigationseigenschaften, aus denen die Beziehung besteht. `HasOne`oder `HasMany` identifiziert die Navigationseigenschaft für den Entitätstyp, der Sie die Konfiguration ab. Sie verketten, klicken Sie dann einen Aufruf von `WithOne` oder `WithMany` die umgekehrte Navigation zu identifizieren. `HasOne`/`WithOne`dienen zur Navigation Verweiseigenschaften und `HasMany` / `WithMany` werden für Navigationseigenschaften der Auflistung verwendet.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?name=Model&highlight=8,9,10)]

### <a name="single-navigation-property"></a>Einzelne Navigationseigenschaft

Wenn Ihnen nur einer Navigationseigenschaft sind parameterlose Überladungen der `WithOne` und `WithMany`. Dies gibt an, dass es ist im Grunde ein Verweis oder eine Auflistung am anderen Ende der Beziehung, aber es keine Navigationseigenschaft, die in die Entitätsklasse enthalten ist.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?name=Model&highlight=10)]

### <a name="foreign-key"></a>Fremdschlüssel

Sie können die Fluent-API verwenden, konfigurieren, welche Eigenschaft als foreign Key-Eigenschaft für eine bestimmte Beziehung verwendet werden soll.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?name=Model&highlight=11)]

Das folgende Codebeispiel zeigt, wie so konfigurieren Sie einen zusammengesetzten foreign Key.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?name=Model&highlight=13)]

Können Sie die zeichenfolgenüberladung der `HasForeignKey(...)` so konfigurieren Sie eine Schattenkopie-Eigenschaft als Fremdschlüssel (finden Sie unter [Shadowing von Eigenschaften](shadow-properties.md) für Weitere Informationen). Es wird empfohlen, das Modell explizit die Shadow-Eigenschaft hinzugefügt werden, bevor es als Fremdschlüssel verwendet wird (wie unten gezeigt).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a>Prinzipal-Schlüssel

Wenn Sie den Fremdschlüssel auf eine Eigenschaft als Primärschlüssel verweisen möchten, können Sie die Fluent-API, so konfigurieren Sie die Dienstprinzipalname Schlüsseleigenschaft für die Beziehung. Die Eigenschaft, die Sie als Prinzipal Schlüssel wird automatisch konfiguriert werden, Setup als ein alternativer Schlüssel (siehe [Alternativschlüssel](alternate-keys.md) für Weitere Informationen).

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/PrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => s.CarLicensePlate)
            .HasPrincipalKey(c => c.LicensePlate);
    }
}

public class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

Das folgende Codebeispiel zeigt, wie so konfigurieren Sie einen zusammengesetzten Schlüssel für Dienstprinzipalnamen.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CompositePrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => new { s.CarState, s.CarLicensePlate })
            .HasPrincipalKey(c => new { c.State, c.LicensePlate });
    }
}

public class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarState { get; set; }
    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

> [!WARNING]  
> Die Reihenfolge, in der Sie die Schlüsseleigenschaften für Dienstprinzipalname angeben, muss der Reihenfolge entsprechen, in der für den Fremdschlüssel angegeben werden.

### <a name="required-and-optional-relationships"></a>Erforderliche und optionale Beziehungen

Sie können die Fluent-API verwenden, konfigurieren Sie, ob die Beziehung erforderlich oder optional ist. Letztlich steuert diese Eigenschaft, ob die foreign Key-Eigenschaft erforderlich oder optional ist. Dies ist besonders hilfreich, bei Verwendung von Schatten Zustand Fremdschlüssel. Wenn Sie über eine Fremdschlüsseleigenschaft in Ihrer Entitätsklasse verfügen, und klicken Sie dann die Requiredness der Beziehung bestimmt wird, je nachdem, ob die foreign Key-Eigenschaft erforderlich oder optional ist (finden Sie unter [erforderliche und optionale Eigenschaften](required-optional.md) Weitere Informationen).

<!-- [!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/Required.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}
```

### <a name="cascade-delete"></a>Kaskadierte Löschung

Die Fluent-API können Sie um das Löschverhalten Cascade für eine bestimmte Beziehung explizit zu konfigurieren.

Finden Sie unter [Löschweitergabe](../saving/cascade-delete.md) für eine ausführliche Erläuterung der einzelnen Optionen im Abschnitt "Speichern von Daten".

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CascadeDelete.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .OnDelete(DeleteBehavior.Cascade);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int? BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="other-relationship-patterns"></a>Andere Beziehung-Muster

### <a name="one-to-one"></a>1:1

1: 1-Beziehungen haben eine Navigationseigenschaft für den Verweis auf beiden Seiten. Sie entsprechen die gleichen Konventionen wie n-Beziehungen, jedoch ein eindeutiger Index wird eingeführt, auf die Fremdschlüsseleigenschaft, um sicherzustellen, dass nur eine abhängige, der jedem Prinzipal verknüpft ist.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/Relationships/OneToOne.cs?highlight=6,15,16)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

> [!NOTE]  
> EF wählt eine der Entitäten, die basierend auf der Fähigkeit zum Erkennen einer Fremdschlüsseleigenschaft abhängig sein. Wenn die falsche Entität als die abhängigen ausgewählt wird, können Sie die Fluent-API, um dieses Problem zu beheben.

Wenn Sie die Beziehung mit der Fluent-API konfigurieren möchten, verwenden Sie die `HasOne` und `WithOne` Methoden.

Wenn Sie den Fremdschlüssel konfigurieren, Sie den Typ der abhängigen Entität - angeben müssen, beachten Sie den generischen Parameter bereitgestellt, um `HasForeignKey` in der Liste unten. In einer 1: n-Beziehung ist klar, dass die Entität mit dem Verweis-Navigation der abhängigen und der Datensatz mit der Auflistung der Prinzipal ist. Dies ist jedoch nicht in einer 1: 1-Beziehung - daher so muss er explizit zu definieren.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/OneToOne.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<BlogImage> BlogImages { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasOne(p => p.BlogImage)
            .WithOne(i => i.Blog)
            .HasForeignKey<BlogImage>(b => b.BlogForeignKey);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogForeignKey { get; set; }
    public Blog Blog { get; set; }
}
```

### <a name="many-to-many"></a>m:n

Viele-zu-viele-Beziehungen ohne eine Entitätsklasse zur Darstellung der Jointabelle werden noch nicht unterstützt. Sie können jedoch eine m: n-Beziehung darstellen, indem einschließlich einer Entitätsklasse für die Jointabelle und die Zuordnung zwei separaten 1: n-Beziehungen.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<PostTag>()
            .HasKey(t => new { t.PostId, t.TagId });

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Post)
            .WithMany(p => p.PostTags)
            .HasForeignKey(pt => pt.PostId);

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Tag)
            .WithMany(t => t.PostTags)
            .HasForeignKey(pt => pt.TagId);
    }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class Tag
{
    public string TagId { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class PostTag
{
    public int PostId { get; set; }
    public Post Post { get; set; }

    public string TagId { get; set; }
    public Tag Tag { get; set; }
}
```
