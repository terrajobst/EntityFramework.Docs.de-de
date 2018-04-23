---
title: Entitätstypen mit Konstruktoren - EF Core
author: ajcvickers
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
ms.technology: entity-framework-core
uid: core/modeling/constructors
ms.openlocfilehash: 3f861d54c5bff637ae28f38b08da7aff7d0ea5c0
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="entity-types-with-constructors"></a>Entitätstypen mit Konstruktoren

> [!NOTE]  
> Dieses Feature ist neu in EF Core 2.1.

Beginnend mit EF Core 2.1, ist es jetzt möglich, definieren Sie einen Konstruktor mit Parametern und EF Core Aufruf dieses Konstruktors beim Erstellen einer Instanz der Entität verfügen. Die Parameter des Konstruktors an zugeordneten Eigenschaften gebunden werden können, oder auf verschiedene Arten von Diensten zu ermöglichen like Verhaltensweisen verzögertes Laden.

> [!NOTE]  
> Ab EF Core 2.1 ist alle Konstruktor-Bindung gemäß der Konvention. Konfiguration von bestimmten Konstruktoren zu verwenden, ist für eine zukünftige Version geplant.

## <a name="binding-to-mapped-properties"></a>Binden an die zugeordnete Eigenschaften

Betrachten Sie ein typisches Modell für den Blogbeitrag/aus:

```Csharp
public class Blog
{
    public int Id { get; set; }

    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public int Id { get; set; }
    
    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```

EF Core Instanzen dieser Typen erstellt werden, werden z. B. für die Ergebnisse einer Abfrage zuerst parameterlose Standardkonstruktor aufrufen und legen Sie dann jede Eigenschaft mit dem Wert aus der Datenbank. Allerdings findet EF Kern ein parametrisierten Konstruktors mit Parameternamen und Typen, die übereinstimmen zugeordneten Eigenschaften, und es wird stattdessen rufen Sie die parametrisierten Konstruktor mit Werten für diese Eigenschaften und jede Eigenschaft nicht explizit festgelegt. Zum Beispiel:

```Csharp
public class Blog
{
    public Blog(int id, string name, string author)
    {
        Id = id;
        Name = name;
        Author = author;
    }

    public int Id { get; set; }

    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public Post(int id, string title, DateTime postedOn)
    {
        Id = id;
        Title = title;
        PostedOn = postedOn;
    }

    public int Id { get; set; }

    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```
Einige Aspekte zu beachten:
* Nicht alle Eigenschaften erforderlich Konstruktorparameter sind. Beispielsweise ist die Post.Content-Eigenschaft nicht durch einen Konstruktorparameter festgelegt, damit EF Core er nach dem Aufruf des Konstruktors, auf die übliche Weise festgelegt werden.
* Der Parametertypen und muss mit dem Unterschied, dass Eigenschaften Pascal-Schreibweise angegeben werden, während die Parameter in Kamel-Schreibweise werden Eigenschaftentypen und Namen übereinstimmen.
* EF Core Navigationseigenschaften (z. B. Blog oder Ihre Beiträge, die oben genannten) festlegen kann nicht mit einem Konstruktor.
* Der Konstruktor kann öffentliche, private, oder eine beliebige andere zugreifbarkeit besitzen.

### <a name="read-only-properties"></a>Schreibschutzeigenschaften

Nach Eigenschaften über den Konstruktor festgelegt werden, kann es sinnvoll, von denen einige schreibgeschützt machen. EF Core unterstützt, aber es gibt einige Dinge, um nach zu suchen:
* Eigenschaften ohne Settern für Eigenschaften werden nicht gemäß der Konvention zugeordnet werden. (Auf diese Weise tendenziell Eigenschaften zugeordnet werden, die nicht, z. B. berechnete Eigenschaften zugeordnet werden soll.)
* Die Verwendung von automatisch generierten Schlüsselwerte erfordert eine Schlüsseleigenschaft, die Lese-/ Schreibzugriff, da der Schlüsselwert muss von der Schlüssel-Generator festgelegt werden, wenn Sie neue Entitäten einfügen.

Eine einfache Möglichkeit, vermeiden Sie die folgenden Schritte ist die Verwendung von privaten Settern für Eigenschaften. Zum Beispiel:
```Csharp
public class Blog
{
    public Blog(int id, string name, string author)
    {
        Id = id;
        Name = name;
        Author = author;
    }

    public int Id { get; private set; }

    public string Name { get; private set; }
    public string Author { get; private set; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    public Post(int id, string title, DateTime postedOn)
    {
        Id = id;
        Title = title;
        PostedOn = postedOn;
    }

    public int Id { get; private set; }

    public string Title { get; private set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; private set; }

    public Blog Blog { get; set; }
}

```
EF Core sieht eine Eigenschaft mit einem privaten Setter mit Lese-/ Schreibzugriff, d. h., die alle Eigenschaften wie zuvor zugeordnet sind, und der Schlüssel kann immer noch vom Speicher generierte werden.

Eine Alternative zur Verwendung von privaten Setter ist wirklich schreibgeschützte Eigenschaften machen und mehr explizite Zuordnung in der OnModelCreating hinzufügen. Einige Eigenschaften können ebenfalls vollständig entfernt und durch nur Felder ersetzt werden. Betrachten Sie beispielsweise die folgenden Entitätstypen:

```Csharp
public class Blog
{
    private int _id;

    public Blog(string name, string author)
    {
        Name = name;
        Author = author;
    }

    public string Name { get; }
    public string Author { get; }

    public ICollection<Post> Posts { get; } = new List<Post>();
}

public class Post
{
    private int _id;

    public Post(string title, DateTime postedOn)
    {
        Title = title;
        PostedOn = postedOn;
    }

    public string Title { get; }
    public string Content { get; set; }
    public DateTime PostedOn { get; }

    public Blog Blog { get; set; }
}
```
Und diese Konfiguration in der OnModelCreating:
```Csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>(
        b =>
        {
            b.HasKey("_id");
            b.Property(e => e.Author);
            b.Property(e => e.Name);
        });

    modelBuilder.Entity<Post>(
        b =>
        {
            b.HasKey("_id");
            b.Property(e => e.Title);
            b.Property(e => e.PostedOn);
        });
}
```
Beachten Folgendes:
* Der Schlüssel "Property" ist jetzt ein Feld. Es ist kein `readonly` Feld, damit vom Speicher generierte Schlüssel verwendet werden können. 
* Die anderen Eigenschaften sind schreibgeschützte Eigenschaften, die nur im Konstruktor festgelegt.
* Wenn der primäre Schlüssel-Wert ist immer nur von EF festgelegt oder aus der Datenbank gelesen, dann besteht keine Notwendigkeit zum Einschließen in den Konstruktor. Dadurch bleibt den Schlüssel "Property" als einen einfachen Feldverweis und verdeutlichen, dass es nicht explizit soll beim neuen Blogs oder Beiträge zu erstellen festgelegt werden.

> [!NOTE]  
> Dieser Code führt zu compilerwarnung "169" gibt an, dass das Feld nie verwendet wird. Dies kann ignoriert werden, da in der Realität EF Core automatisierbar extralinguistic Feld verwendet wird.

## <a name="injecting-services"></a>Räumen Dienste

EF Core kann auch "Dienste" in einen Entitätstyp Konstruktor einfügen. Beispielsweise können die folgenden eingegeben werden:
* `DbContext` -der aktuelle Kontextinstanz, die auch als Ihre abgeleiteten DbContext-Typ eingegeben werden kann
* `ILazyLoader` -der verzögerten Laden-Dienst finden Sie unter der [verzögertes Laden Dokumentation](../querying/related-data.md) Weitere Details
* `Action<object, string>` -ein verzögertes Laden Delegat--finden Sie unter der [verzögertes Laden Dokumentation](../querying/related-data.md) Weitere Details
* `IEntityType` -Dieses Entitätstyps zugeordneten EF-Core-Metadaten

> [!NOTE]  
> Zum Zeitpunkt der EF Core 2.1 können nur Dienste bekannt EF Core eingegeben werden. Unterstützung für Anwendungsdienste Räumen ist für eine künftige Version berücksichtigt wird.

Beispielsweise kann ein eingefügter DbContext verwendet werden, selektiv Zugriff auf die Datenbank zum Abrufen von Informationen zu verknüpften Entitäten ohne Laden sie alle. Im folgenden Beispiel wird dies verwendet, um die Anzahl der Einträge in einem Blog zu erhalten, ohne die Beiträgen zu laden:

```Csharp
public class Blog
{
    public Blog()
    {
    }

    private Blog(BloggingContext context)
    {
        Context = context;
    }

    private BloggingContext Context { get; set; }

    public int Id { get; set; }
    public string Name { get; set; }
    public string Author { get; set; }

    public ICollection<Post> Posts { get; set; }

    public int PostsCount
        => Posts?.Count
           ?? Context?.Set<Post>().Count(p => Id == EF.Property<int?>(p, "BlogId"))
           ?? 0;
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PostedOn { get; set; }

    public Blog Blog { get; set; }
}
```
Einige Punkte, die zu diesem beachten:
* Der Konstruktor ist privat, da es immer nur von EF Core aufgerufen wird und eine andere öffentliche Konstruktor für den allgemeinen Gebrauch vorhanden ist.
* Den Code mithilfe des eingefügten-Diensts (d. h. der Kontext) ist gegenüber diesem defensiven wird `null` Fälle gehandhabt werden, in denen EF Core ist die Instanz nicht erstellen.
* Da Service in eine Lese-Schreib-Eigenschaft gespeichert ist wird sie zurückgesetzt werden, wenn die Entität an eine neue Kontextinstanz angefügt ist.

> [!WARNING]  
> Räumen ' DbContext ' wie folgt häufig einer Antimuster gilt seit Ihrer Entitätstypen direkt mit EF Core verkoppelt. Bedenken Sie sorgfältig alle Optionen vor der Verwendung von Dienst-Injection wie folgt.
