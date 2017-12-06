---
title: Eigenschaften der Schattenkopie - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
ms.technology: entity-framework-core
uid: core/modeling/shadow-properties
ms.openlocfilehash: 8c7f70789ddc6ebd24f9bfae931069834db593c2
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2017
---
# <a name="shadow-properties"></a>Shadowing von Eigenschaften

Shadowing von Eigenschaften sind Eigenschaften, die nicht in Ihrer Entitätsklasse .NET definiert werden, aber für diesen Entitätstyp in der EF-Core-Modell definiert sind. Der Wert und der Status dieser Eigenschaften wird ausschließlich in das System zur Änderungsnachverfolgung beibehalten.

Shadowing von Eigenschaften sind nützlich, wenn Daten vorhanden sind, in der Datenbank, die nicht auf die zugeordneten Entitätstypen verfügbar gemacht werden sollen. Sie werden am häufigsten verwendet für foreign Key-Eigenschaften, in dem die Beziehung zwischen zwei Entitäten wird durch einen Fremdschlüsselwert in der Datenbank dargestellt, aber die Beziehung wird für Entitätstypen, die mithilfe von Navigationseigenschaften zwischen den Entitätstypen verwaltet.

Schatten Eigenschaftswerte durch geändert und abgerufen werden können die `ChangeTracker` API.

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

Shadowing von Eigenschaften in LINQ-Abfragen über verwiesen werden können die `EF.Property` statische Methode.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>Konventionen

Shadowing von Eigenschaften können gemäß der Konvention erstellt werden, wenn eine Beziehung ermittelt werden kann, aber keine foreign Key-Eigenschaft in der Klasse der abhängigen Entität gefunden wird. In diesem Fall wird eine Schattenkopie Fremdschlüsseleigenschaft eingeführt werden. Den Namen der Schatten Fremdschlüsseleigenschaft `<navigation property name><principal key property name>` (die Navigation auf der abhängigen Entität, die auf die Dienstprinzipalnamen Entität verweist, wird für die Benennung verwendet). Wenn der Prinzipal Schlüsseleigenschaft Name umfasst den Namen der Navigationseigenschaft, die Namen werden nur `<principal key property name>`. Wenn keine Navigationseigenschaft für die abhängige Entität vorhanden ist, wird der Name der Prinzipaltyp an seiner Stelle verwendet.

Beispielsweise führt das folgende Codelisting eine `BlogId` Shadow-Eigenschaft wird eine Einführung in die `Post` Entität.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/ShadowForeignKey.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
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

## <a name="data-annotations"></a>Datenanmerkungen

Shadowing von Eigenschaften können nicht mit datenanmerkungen erstellt werden.

## <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, so konfigurieren Sie Shadowing von Eigenschaften. Nachdem Sie die zeichenfolgenüberladung aufgerufen haben `Property` können Sie alle von der Konfiguration aufrufen möchten, für die anderen Eigenschaften verketten.

Wenn der Name, um angegeben die `Property` Methoden entsprechen den Namen einer vorhandenen Eigenschaft (eine Schattenkopie-Eigenschaft oder eine Entitätsklasse definiert) und dann im Code konfigurieren, vorhandene Eigenschaft anstatt eine neue Shadow-Eigenschaft.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/ShadowProperty.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property<DateTime>("LastUpdated");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
