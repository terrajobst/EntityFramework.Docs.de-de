---
title: Entitätstypen mit Konstruktoren – EF Core
author: ajcvickers
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 420AFFE7-B709-4A68-9149-F06F8746FB33
ms.technology: entity-framework-core
uid: core/modeling/constructors
ms.openlocfilehash: 80c7ee04d3bb0dd45b66ec7d6fec5ee7e3343026
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949204"
---
# <a name="entity-types-with-constructors"></a>Entitätstypen mit Konstruktoren

> [!NOTE]  
> Dieses Feature ist neu in EF Core 2.1.

Ab EF Core 2.1, ist es jetzt möglich, definieren Sie einen Konstruktor mit Parametern und dass EF Core, die dieser Konstruktor aufgerufen, wenn Sie eine Instanz der Entität zu erstellen. Parameter des Konstruktors an zugeordneten Eigenschaften gebunden werden können, oder auf verschiedene Arten von Diensten zu ermöglichen wie Verhalten lazy Loading.

> [!NOTE]  
> Ab EF Core 2.1 ist alle Konstruktor-Bindung gemäß der Konvention. Konfiguration von bestimmten Konstruktoren verwenden, ist für eine künftige Version geplant.

## <a name="binding-to-mapped-properties"></a>Binden an die zugeordnete Eigenschaften

Betrachten Sie ein typisches Modell für die Blog-Beitrag:

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

Wenn Instanzen dieser Typen von EF Core erstellt wird, wie z. B. für die Ergebnisse einer Abfrage, sie rufen Sie zuerst dem parameterlosen Standardkonstruktor und klicken Sie dann auf den Wert jeder Eigenschaft festgelegt, aus der Datenbank. Jedoch wenn EF Core mit einen parametrisierten Konstruktor gefunden Parameternamen und -Typen, die übereinstimmen zugeordnet Eigenschaften, und es den parametrisierten Konstruktor mit den Werten für diese Eigenschaften aufzurufen, und jede Eigenschaft nicht explizit festgelegt. Zum Beispiel:

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
Einige Punkte zu beachten:
* Nicht alle Eigenschaften müssen Konstruktorparameter. Beispielsweise wird die Post.Content-Eigenschaft von einzelnen Konstruktorparameter nicht festgelegt, damit EF Core er nach dem Aufruf des Konstruktors, auf die übliche Weise festgelegt werden.
* Der Parametertypen und muss mit dem Unterschied, dass Eigenschaften Pascal-Schreibweise verwendet werden, während die Parameter in Kamel-Schreibweise werden Eigenschaftstypen und -Namen übereinstimmen.
* EF Core kann nicht festgelegt Navigationseigenschaften (z. B. Blog oder Ihre Beiträge, die oben genannten) über einen Konstruktor.
* Der Konstruktor kann als öffentlich, privat, oder eine beliebige andere zugriffsmöglichkeiten aufweisen.

### <a name="read-only-properties"></a>Schreibgeschützte Eigenschaften

Nach Eigenschaften über den Konstruktor festgelegt werden können einige Schreibschutz sinnvoll. EF Core unterstützt, aber es gibt einige Dinge achten:
* Eigenschaften ohne Setter werden gemäß der Konvention nicht zugeordnet. (Dies ist zum Zuordnen von Eigenschaften, die nicht, wie z. B. berechnete Eigenschaften zugeordnet werden soll.)
* Die Verwendung von automatisch generierten Schlüsselwerte erfordert eine wichtige Eigenschaft, die Lese-/ Schreibzugriff, da der Schlüsselwert durch den Generator für Zugriffsschlüssel festgelegt werden, wenn neue Entitäten einfügen muss.

Eine einfache Möglichkeit, diese Probleme vermeiden werden private Setter verwendet. Zum Beispiel:
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
EF Core sieht eine Eigenschaft mit einem privaten Setter als Lese-/ Schreibzugriff, d. h., die alle Eigenschaften wie zuvor zugeordnet sind, und der Schlüssel kann immer noch vom Speicher generierte werden.

Eine Alternative zur Verwendung von private Setter, werden Eigenschaften wirklich schreibgeschützt machen, und fügen weitere explizite Zuordnung in "onmodelcreating" ab. Einige Eigenschaften können auch vollständig entfernt und durch nur Felder ersetzt werden. Betrachten Sie beispielsweise die folgenden Entitätstypen:

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
Und diese Konfiguration in "onmodelcreating":
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
Beachten Sie Folgendes:
* Der Schlüssel "Property" ist jetzt ein Feld. Es ist keiner `readonly` Feld, damit vom Speicher generierte Schlüssel verwendet werden können.
* Die anderen Eigenschaften sind schreibgeschützte Eigenschaften, die nur im Konstruktor festgelegt.
* Wenn der primäre Schlüsselwert ist immer nur, indem EF festlegen oder aus der Datenbank gelesen, dann besteht keine Notwendigkeit zum Einschließen in den Konstruktor. Dies bewirkt, dass den Schlüssel "Property" als ein einfaches Feld und macht deutlich, dass sie nicht explizit festgelegt werden soll, wenn neue Blogs und Beiträgen zu erstellen.

> [!NOTE]  
> Dieser Code führt zu compilerwarnung "169" gibt an, dass das Feld nie verwendet wird. Dies kann ignoriert werden, da in der Praxis EF Core das Feld in einer Weise extralinguistic verwendet wird.

## <a name="injecting-services"></a>Einfügen von Diensten

EF Core kann auch "Dienste" in einen Entitätstyp-Konstruktor einfügen. Beispielsweise kann die folgenden eingefügt werden:
* `DbContext` – die aktuelle Context-Instanz, die auch als Ihr abgeleiteter Typ von "DbContext" eingegeben werden kann
* `ILazyLoader` -der-Lazy Load-Dienst – finden Sie unter der [lazy Loading-Dokumentation](../querying/related-data.md) Weitere Details
* `Action<object, string>` -einen lazy Loading-Delegaten – finden Sie unter der [lazy Loading Dokumentation](../querying/related-data.md) Weitere Details
* `IEntityType` -die EF Core-Metadaten, die dieser Entitätstyp zugeordnet

> [!NOTE]  
> Ab EF Core 2.1 können nur Dienste, die von EF Core bekannte eingefügt werden. Unterstützung für das Einschleusen von Anwendungsdiensten ist für eine spätere Version in Betracht gezogen.

Beispielsweise kann ein eingefügter "DbContext" verwendet werden, selektiv Zugriff auf die Datenbank Informationen zu verknüpften Entitäten zu erhalten, ohne Sie zu laden, alle. Im folgenden Beispiel wird dies verwendet, um die Anzahl an Beiträgen, die in einem Blog zu erhalten, ohne Sie zu laden die Beiträge:

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
Einige Punkte zu beachten Sie dazu zu erhalten:
* Der Konstruktor ist privat, da immer nur durch von EF Core Aufruf ist und eine andere öffentlicher Konstruktor zur allgemeinen Verwendung vorhanden ist.
* Der Code, der mit dem eingefügten Dienst (d. h. der Kontext) ist dafür defensive wird `null` um Fälle zu behandeln, in denen EF Core ist nicht das Erstellen die Instanz.
* Da Service in einer Lese-/Schreibzugriff-Eigenschaft gespeichert wird werden sie zurückgesetzt werden, wenn die Entität an eine neue Kontextinstanz angefügt ist.

> [!WARNING]  
> Einfügen von "DbContext" wie folgt, wird häufig als Antimuster betrachtet, da die Entitätstypen direkt mit EF Core verkoppelt. Vor der Verwendung von Dienst-Injection wie folgt alle Optionen sorgfältig prüfen.
