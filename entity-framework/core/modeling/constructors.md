---
title: Entitäts Typen mit Konstruktoren-EF Core
author: ajcvickers
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
uid: core/modeling/constructors
ms.openlocfilehash: ddfaa8eebde388a9d3309f21b8891de593077956
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811894"
---
# <a name="entity-types-with-constructors"></a>Entitäts Typen mit Konstruktoren

> [!NOTE]  
> Dieses Feature ist neu in EF Core 2.1.

Ab EF Core 2,1 ist es möglich, einen Konstruktor mit Parametern zu definieren, und EF Core diesen Konstruktor aufzurufen, wenn eine Instanz der Entität erstellt wird. Die Konstruktorparameter können an zugeordnete Eigenschaften oder an verschiedene Arten von Diensten gebunden werden, um Verhalten wie Lazy Load zu vereinfachen.

> [!NOTE]  
> Ab EF Core 2,1 ist die gesamte konstruktorbindung gemäß der Konvention. Die Konfiguration bestimmter zu verwendende Konstruktoren ist für eine zukünftige Version geplant.

## <a name="binding-to-mapped-properties"></a>Binden an zugeordnete Eigenschaften

Ein typisches Blog-/Post-Modell:

``` csharp
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

Wenn EF Core Instanzen dieser Typen erstellt, z. b. für die Ergebnisse einer Abfrage, wird zuerst der standardmäßige Parameter lose Konstruktor aufgerufen und dann jede Eigenschaft auf den Wert aus der Datenbank festgelegt. Wenn EF Core jedoch einen parametrisierten Konstruktor mit Parameternamen und-Typen findet, die mit den zugeordneten Eigenschaften identisch sind, wird stattdessen der parametrisierte Konstruktor mit Werten für diese Eigenschaften aufgerufen, und jede Eigenschaft wird nicht explizit festgelegt. Beispiel:

``` csharp
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

Beachten Sie Folgendes:

* Nicht alle Eigenschaften müssen über Konstruktorparameter verfügen. Beispielsweise wird die Post. Content-Eigenschaft nicht von einem Konstruktorparameter festgelegt, sodass EF Core Sie nach dem Aufrufen des Konstruktors auf die normale Weise festgelegt wird.
* Die Parametertypen und-Namen müssen mit den Eigenschafts Typen und-Namen identisch sein, mit dem Unterschied, dass die Eigenschaften in Pascal-Schreibweise angegeben werden können, während die Parameter
* EF Core können keine Navigations Eigenschaften (z. b. Blog oder Beiträge oben) mithilfe eines Konstruktors festlegen.
* Der Konstruktor kann öffentlich oder privat sein oder über andere Zugriffsmöglichkeiten verfügen. Zum verzögerten Laden von Proxys ist es jedoch erforderlich, dass der Konstruktor von der vererbenden Proxy Klasse zugänglich ist. Dies bedeutet in der Regel, dass Sie entweder öffentlich oder geschützt ist.

### <a name="read-only-properties"></a>Schreibgeschützte Eigenschaften

Nachdem Eigenschaften über den Konstruktor festgelegt wurden, kann es sinnvoll sein, einige davon als schreibgeschützt zu deklarimachen. EF Core unterstützt dies, aber es gibt einige Dinge, die Sie nachschlagen sollten:

* Eigenschaften ohne Setter werden nicht anhand der Konvention zugeordnet. (Dies bewirkt tendenziell, dass Eigenschaften zugeordnet werden, die nicht zugeordnet werden sollten, z. b. berechnete Eigenschaften.)
* Die Verwendung automatisch generierter Schlüsselwerte erfordert eine Schlüsseleigenschaft mit Lese-/Schreibzugriff, da der Schlüsselwert beim Einfügen neuer Entitäten vom Schlüssel Generator festgelegt werden muss.

Eine einfache Möglichkeit, diese Dinge zu vermeiden, ist die Verwendung privater Setter. Beispiel:

``` csharp
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

EF Core eine Eigenschaft mit einem privaten Setter als Lese-/Schreibzugriff sehen, bedeutet dies, dass alle Eigenschaften wie zuvor zugeordnet sind und der Schlüssel weiterhin vom Speicher generiert werden kann.

Eine Alternative zur Verwendung von privaten Setter besteht darin, dass Eigenschaften wirklich schreibgeschützt sind und in onmodelcreating eine explizite Zuordnung hinzugefügt wird. Ebenso können einige Eigenschaften vollständig entfernt und durch nur Felder ersetzt werden. Beachten Sie z. b. die folgenden Entitäts Typen:

``` csharp
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

Und diese Konfiguration in onmodelcreating:

``` csharp
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

Beachten Sie Folgendes:

* Der Schlüssel "Property" ist jetzt ein Feld. Es handelt sich nicht um ein `readonly` Feld, sodass vom Speicher generierte Schlüssel verwendet werden können.
* Die anderen Eigenschaften sind schreibgeschützte Eigenschaften, die nur im Konstruktor festgelegt sind.
* Wenn der Primärschlüssel Wert nur von EF festgelegt oder aus der Datenbank gelesen wird, muss er nicht in den Konstruktor eingeschlossen werden. Dadurch bleibt der Schlüssel "Property" als einfaches Feld, und es wird klar, dass er beim Erstellen neuer Blogs oder Beiträge nicht explizit festgelegt werden sollte.

> [!NOTE]  
> Dieser Code führt zu einer Compilerwarnung "169", die angibt, dass das Feld niemals verwendet wird. Dies kann ignoriert werden, da EF Core das Feld in der Praxis auf extralweise verwendet.

## <a name="injecting-services"></a>Einschleusen von Diensten

EF Core können auch "Dienste" in den Konstruktor eines Entitäts Typs einfügen. Beispielsweise kann Folgendes eingefügt werden:

* `DbContext`-die aktuelle Kontext Instanz, die auch als abgeleiteter dbcontext-Typ typisiert werden kann.
* `ILazyLoader`-der Dienst für verzögertes Laden: Weitere Informationen finden Sie in der [Dokumentation zum Lazy Load](../querying/related-data.md) .
* `Action<object, string>`-einen Delegaten mit verzögerter Auslastung: Weitere Informationen finden Sie in der [Dokumentation zum Lazy Load](../querying/related-data.md) .
* `IEntityType`-die EF Core Metadaten, die diesem Entitätstyp zugeordnet sind.

> [!NOTE]  
> Ab EF Core 2,1 können nur von EF Core bekannte Dienste eingefügt werden. Die Unterstützung für das Einfügen von Anwendungsdiensten wird für eine zukünftige Version berücksichtigt.

Beispielsweise kann ein eingefügter dbcontext zum selektiven zugreifen auf die Datenbank verwendet werden, um Informationen zu verknüpften Entitäten abzurufen, ohne Sie zu laden. Im folgenden Beispiel wird dies verwendet, um die Anzahl von Beiträgen in einem Blog abzurufen, ohne die Beiträge zu laden:

``` csharp
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

Hier sind einige Punkte zu beachten:

* Der Konstruktor ist privat, da er nur von EF Core aufgerufen wird und es einen anderen öffentlichen Konstruktor für die allgemeine Verwendung gibt.
* Der Code, der den eingefügten Dienst (d. h. den Kontext) verwendet, ist gegen die `null`, um Fälle zu behandeln, in denen EF Core die Instanz nicht erstellt.
* Da der Dienst in einer Lese-/Schreibeigenschaft gespeichert wird, wird er zurückgesetzt, wenn die Entität an eine neue Kontext Instanz angefügt wird.

> [!WARNING]  
> Das Einfügen von dbcontext wie diesem wird häufig als Antimuster betrachtet, da die Entitäts Typen direkt an EF Core gekoppelt werden. Berücksichtigen Sie alle Optionen sorgfältig, bevor Sie Service Injection wie folgt verwenden.
