---
title: Beziehungen – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
ms.technology: entity-framework-core
uid: core/modeling/relationships
ms.openlocfilehash: 0e60ade605ffb73b02934ea32f1c757affce970d
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949217"
---
# <a name="relationships"></a>Beziehungen

Eine Beziehung definiert, wie zwei Entitäten miteinander verknüpfen. In einer relationalen Datenbank wird dies durch eine foreign Key-Einschränkung dargestellt.

> [!NOTE]  
> Die meisten Beispiele in diesem Artikel verwenden Sie eine 1: n Beziehung zur Veranschaulichung der Konzepte. Weitere Beispiele für 1: 1- und m: n Beziehungen finden Sie unter den [andere Beziehung Muster](#other-relationship-patterns) Abschnitt am Ende des Artikels.

## <a name="definition-of-terms"></a>Begriffsdefinition

Es wird eine Reihe von Begriffen verwendet, um Beziehungen in Datenbanken zu beschreiben

* **Abhängige Entität:** Dies ist die Entität, die den Fremdschlüssel enthält. Auch "Child" genannt.

* **Primären Entität:** Dies ist die Entität, die den Primär-  bzw. Alternativschlüssel enthält.  Auch "Parent" genannt.

* **Fremdschlüssel:**  Die Eigenschaft in einer Abhängigen Entität, welche die Inhalte des Primärschlüssels der Primären Entität enthält.

* **Primärschlüssel:** Die Eigenschaft, welche das primäre Attribut einer Entität eindeutig identifiziert. Dies kann der Primärschlüssel oder eine Alternativeschlüssel sein.

* **Navigationseigenschaft:** eine Eigenschaft der Primären und/oder Abhängigen Entität, welche Hinweise zu den verbundenen Entitäten enthält.

  * **Auflistungsnavigationseigenschaft:** eine Navigationseigenschaft, die Verweise auf viele verknüpfte Entitäten enthält.

  * **Verweisnavigationseigenschaft:** eine Navigationseigenschaft, die einen Verweis auf eine einzelne verknüpfte Entität enthält.

  * **Inverse-Navigationseigenschaft:** im Zusammenhang mit einer belibigen anderen Navigationseigenschaft, stellt diese Eigenschaft die Navigationseigenschaft am anderen Ende der Beziehung dar.

Das folgende Codebeispiel veranschaulicht eine 1: n Beziehung zwischen `Blog` und `Post`

* `Post` die abhängige Entität

* `Blog` ist die prinzipalentität

* `Post.BlogId` ist der Fremdschlüssel

* `Blog.BlogId` der prinzipalschlüssel (in diesem Fall ist es ein primärer Schlüssel anstelle eines alternativen Schlüssels)

* `Post.Blog` ist eine verweisnavigationseigenschaft

* `Blog.Posts` ist eine auflistungsnavigationseigenschaft

* `Post.Blog` ist die umgekehrte Navigationseigenschaft des `Blog.Posts` (und umgekehrt)

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a>Konventionen

Gemäß der Konvention wird eine Beziehung erstellt werden, wenn eine Navigationseigenschaft, die ermittelt, die für einen Typ vorhanden ist. Eine Eigenschaft gilt eine Navigationseigenschaft, wenn der Typ, auf die verwiesen, durch den aktuellen Anbieter nicht als ein skalarer Typ zugeordnet werden kann.

> [!NOTE]  
> Beziehungen, die gemäß der Konvention ermittelt werden, werden immer den Primärschlüssel der prinzipalentität Ziel. Um einen alternativen Schlüssel als Ziel verwenden, muss zusätzliche Konfigurationsschritte mithilfe der Fluent-API ausgeführt werden.

### <a name="fully-defined-relationships"></a>Vollständig definierten Beziehungen

Das bekannteste Muster für Beziehungen werden Navigationseigenschaften, die definiert, die an beiden Enden der Beziehung und eine Fremdschlüsseleigenschaft in der abhängigen Entität-Klasse definiert haben.

* Wenn ein Paar von Navigationseigenschaften zwischen zwei Typen gefunden wird, werden sie als umgekehrte Navigationseigenschaften, die von der gleichen Beziehung konfiguriert werden.

* Wenn die abhängige Entität eine Eigenschaft namens enthält `<primary key property name>`, `<navigation property name><primary key property name>`, oder `<principal entity name><primary key property name>` als Fremdschlüssel so konfiguriert werden, wird.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> Treten mehrere Navigationseigenschaften, die zwischen zwei Arten definiert (d. h. mehrere unterschiedliche Paar von Navigationen, die auf den jeweils anderen verweisen), gemäß der Konvention werden dann keine Beziehungen erstellt werden, und Sie müssen manuell konfigurieren, um sich zu identifizieren wie die Navigationseigenschaften, kombinieren Sie.

### <a name="no-foreign-key-property"></a>Keine Foreign Key-Eigenschaft

Zwar wird empfohlen, eine Fremdschlüsseleigenschaft in der abhängigen Entität-Klasse definiert haben, ist es nicht erforderlich. Wenn keine foreign Key-Eigenschaft gefunden wird, wird eine Fremdschlüsseleigenschaft Volumeschattenkopie eingeführt werden, mit dem Namen `<navigation property name><principal key property name>` (finden Sie unter [Schatteneigenschaften](shadow-properties.md) Informationen).

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a>Einzelne Navigationseigenschaft

Nur eine Navigationseigenschaft (keine umgekehrte Navigation und keine Fremdschlüsseleigenschaft) einschließlich ist ausreichend, um eine Beziehung, die gemäß der Konvention definiert haben. Sie können auch die Möglichkeit, eine einzelne Navigationseigenschaft und eine Fremdschlüsseleigenschaft haben.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a>Kaskadierendes Delete

Gemäß der Konvention Löschweitergabe festgelegt *Cascade* für erforderliche Beziehungen und *ClientSetNull* für optionale Beziehungen. *CASCADE* bedeutet, dass abhängige Entitäten werden ebenfalls gelöscht. *ClientSetNull* bedeutet, der abhängigen Entitäten, die nicht in den Speicher geladen bleibt unverändert und müssen manuell gelöscht oder aktualisiert, um auf eine gültige Prinzipal Entität verweisen. Für Entitäten, die in den Arbeitsspeicher geladen werden, versucht EF Core, die die foreign Key-Eigenschaften auf null festgelegt.

Finden Sie unter den [erforderlichen und optionalen Beziehungen](#required-and-optional-relationships) Abschnitt für den Unterschied zwischen erforderlichen und optionalen Beziehungen.

Finden Sie unter [Löschweitergabe](../saving/cascade-delete.md) für Weitere Informationen zu den verschiedenen Verhalten und die Standardwerte, die gemäß der Konvention verwendet löschen.

## <a name="data-annotations"></a>Datenanmerkungen

Es gibt zwei datenanmerkungen, die verwendet werden können, so konfigurieren Sie Beziehungen, `[ForeignKey]` und `[InverseProperty]`.

### <a name="foreignkey"></a>[ForeignKey]

Sie können die Datenanmerkungen verwenden, so konfigurieren Sie die Eigenschaft als die Fremdschlüsseleigenschaft für eine bestimmte Beziehung verwendet werden soll. Dies erfolgt normalerweise, wenn die Fremdschlüsseleigenschaft nicht gemäß der Konvention ermittelt wird.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?name=Entities&highlight=17)]

> [!TIP]  
> Die `[ForeignKey]` Anmerkung in beiden Navigationseigenschaft in der Beziehung platziert werden kann. Es muss nicht für die Navigationseigenschaft in der abhängigen Entität-Klasse zu wechseln.

### <a name="inverseproperty"></a>[InverseProperty]

Sie können die Datenanmerkungen verwenden, konfigurieren, wie Navigationseigenschaften für Entitäten abhängigen und der Prinzipaleigenschaft kombinieren Sie. Dies erfolgt normalerweise, wenn mehr als ein Paar von Navigationseigenschaften zwischen zwei Entitätstypen vorhanden ist.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?name=Entities&highlight=20,23)]

## <a name="fluent-api"></a>Fluent-API

Um eine Beziehung in der Fluent-API zu konfigurieren, starten Sie durch das Identifizieren der Navigationseigenschaften, die die Beziehung bilden. `HasOne` oder `HasMany` identifiziert die Navigationseigenschaft für den Entitätstyp, der Sie die Konfiguration ab. Sie verketten, klicken Sie dann einen Aufruf von `WithOne` oder `WithMany` die umgekehrte Navigation zu identifizieren. `HasOne`/`WithOne` werden verwendet, für die verweisnavigationseigenschaften und `HasMany` / `WithMany` werden für Navigationseigenschaften für Auflistungen verwendet.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?name=Model&highlight=8,9,10)]

### <a name="single-navigation-property"></a>Einzelne Navigationseigenschaft

Wenn Sie nur eine Navigationseigenschaft stehen die parameterlose Überladung von `WithOne` und `WithMany`. Dies bedeutet, dass im Prinzip ein Verweis oder eine Auflistung am anderen Ende der Beziehung vorhanden ist, aber es keine Navigationseigenschaft, die in der Entitätsklasse enthalten gibt.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?name=Model&highlight=10)]

### <a name="foreign-key"></a>Fremdschlüssel

Sie können die Fluent-API verwenden, so konfigurieren Sie die Eigenschaft als die Fremdschlüsseleigenschaft für eine bestimmte Beziehung verwendet werden soll.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?name=Model&highlight=11)]

Das folgende Codebeispiel veranschaulicht einen zusammengesetzten Fremdschlüssel zu konfigurieren.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?name=Model&highlight=13)]

Können Sie die zeichenfolgenüberladung von `HasForeignKey(...)` so konfigurieren Sie eine schatteneigenschaft als Fremdschlüssel (finden Sie unter [Schatteneigenschaften](shadow-properties.md) Informationen). Es wird empfohlen, das Modell explizit die Shadow-Eigenschaft hinzugefügt werden, bevor es als Fremdschlüssel verwendet wird (siehe unten).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a>Dienstprinzipalschlüssel

Wenn Sie den Fremdschlüssel auf eine Eigenschaft als Primärschlüssel verweisen möchten, können Sie die Fluent-API, so konfigurieren Sie die wichtigsten Prinzipaleigenschaft für die Beziehung. Die Eigenschaft, die Sie als Prinzipal Schlüssel wird automatisch konfigurieren, werden Setup als alternativer Schlüssel (finden Sie unter [Alternativschlüssel](alternate-keys.md) Informationen).

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

Das folgende Codebeispiel zeigt einen zusammengesetzten Schlüssel für Prinzipal zu konfigurieren.

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
> Die Reihenfolge, in der Sie wichtige prinzipaleigenschaften angeben, muss der Reihenfolge entsprechen, in der sie für den Fremdschlüssel angegeben sind.

### <a name="required-and-optional-relationships"></a>Erforderliche und optionale Beziehungen

Sie können die Fluent-API verwenden, konfigurieren Sie, ob die Beziehung erforderlich oder optional ist. Letzten Endes steuert, ob die Fremdschlüsseleigenschaft erforderlich oder optional ist. Dies ist besonders hilfreich, wenn Sie einen Fremdschlüssel für die Schattenkopien Zustand verwenden. Wenn Sie eine Fremdschlüsseleigenschaft in Ihrer Entitätsklasse verfügen, und klicken Sie dann die Requiredness der Beziehung bestimmt wird, je nachdem, ob die Fremdschlüsseleigenschaft erforderlich oder optional ist (finden Sie unter [erforderliche und optionale Eigenschaften](required-optional.md) Weitere (Informationen).

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

### <a name="cascade-delete"></a>Kaskadierendes Delete

Sie können die Fluent-API verwenden, das Cascade Löschverhalten für eine bestimmte Beziehung explizit zu konfigurieren.

Finden Sie unter [Löschweitergabe](../saving/cascade-delete.md) im Abschnitt Speichern von Daten für eine detaillierte Erläuterung der einzelnen Optionen.

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

### <a name="one-to-one"></a>1: 1

1: 1-Beziehungen haben einer verweisnavigationseigenschaft auf beiden Seiten. Folgen sie die gleichen Konventionen wie 1: n Beziehungen, aber ein eindeutiger Index wird eingeführt, auf die Fremdschlüsseleigenschaft, um sicherzustellen, dass nur eine abhängige, der jedem Prinzipal verknüpft ist.

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
> EF wählt eine der Entitäten, die basierend auf der Fähigkeit zum Erkennen von Fremdschlüsseleigenschaften abhängig sein. Wenn die falsche Entität als die abhängigen ausgewählt wird, können Sie die Fluent-API, um dies zu korrigieren.

Wenn Sie die Beziehung mit der Fluent-API konfigurieren möchten, verwenden Sie die `HasOne` und `WithOne` Methoden.

Wenn die foreign Key konfigurieren, Sie den abhängige Entitätstyp - angeben müssen, beachten Sie den generischen Parameter bereitgestellt, um `HasForeignKey` in der folgenden Liste. In einer 1: n Beziehung wird deutlich, dass die Entität mit dem die Navigation Verweis des abhängigen und der Auftrag mit dem die Auflistung der Prinzipal ist. Dies ist jedoch nicht in einer 1: 1-Beziehung – daher so müssen sie explizit zu definieren.

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

### <a name="many-to-many"></a>M: n

M: n Beziehungen ohne eine Entitätsklasse zur Darstellung der Jointabelle werden noch nicht unterstützt. Allerdings können Sie eine m: n Beziehung darstellen, durch Einschließen einer Entitätsklasse für die Jointabelle und die Zuordnung zwei separate 1: n Beziehungen.

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
