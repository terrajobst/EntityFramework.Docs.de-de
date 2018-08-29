---
title: Shadowing von Eigenschaften – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: b7b7b10642564dfa3dbc05755188b5b5c63e0d03
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993801"
---
# <a name="shadow-properties"></a>Shadowing von Eigenschaften

Shadowing von Eigenschaften sind Eigenschaften, die nicht in der Entitätsklasse .NET definiert werden, aber für den betreffenden Entitätstyp in EF Core-Modell definiert sind. Der Wert und der Status dieser Eigenschaften wird ausschließlich in die Änderungsnachverfolgung verwaltet.

Shadowing von Eigenschaften sind nützlich, wenn Daten vorhanden sind, in der Datenbank, die nicht für die zugeordnete Entitätstypen verfügbar gemacht werden sollen. Sie werden am häufigsten verwendet für Fremdschlüsseleigenschaften, in dem die Beziehung zwischen zwei Entitäten wird durch einen Fremdschlüsselwert in der Datenbank dargestellt, aber die Beziehung wird für die Entitätstypen, die mithilfe von Navigationseigenschaften zwischen den Entitätstypen verwaltet.

Volumeschattenkopie-Eigenschaftswerte durch geändert und abgerufen werden können die `ChangeTracker` API.

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

Shadowing von Eigenschaften in LINQ-Abfragen über verwiesen werden können die `EF.Property` statische Methode.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>Konventionen

Shadowing von Eigenschaften können gemäß der Konvention erstellt werden, wenn eine Beziehung wird ermittelt, aber keine foreign Key-Eigenschaft in der Klasse der abhängigen Entität gefunden wird. In diesem Fall wird eine Fremdschlüsseleigenschaft Volumeschattenkopie eingeführt. Den Namen der Schatten Fremdschlüsseleigenschaft `<navigation property name><principal key property name>` (im Navigationsbereich auf die abhängige Entität, die auf die prinzipalentität verweist, wird für die Benennung verwendet). Wenn der Name des dienstprinzipals Schlüsseleigenschaft den Namen der Navigationseigenschaft enthält, dann ist der Name nur `<principal key property name>`. Wenn auf die abhängige Entität keine Navigationseigenschaft vorhanden ist, wird der Name des dienstprinzipals an seiner Stelle verwendet.

Das folgende Codebeispiel führt beispielsweise zu einem `BlogId` Shadow-Eigenschaft wird eingeführt, um die `Post` Entität.

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

Shadowing von Eigenschaften können mit datenanmerkungen nicht erstellt werden.

## <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, zum Shadowing von Eigenschaften zu konfigurieren. Nachdem Sie die zeichenfolgenüberladung von aufgerufen haben `Property` können Sie verketten, eine von der Konfiguration aufrufen möchten, für die anderen Eigenschaften.

Wenn der Name, um angegeben die `Property` Methode entspricht der Name einer vorhandenen Eigenschaft (eine schatteneigenschaft oder eine Entitätsklasse definiert), und dann der Code, vorhandene Eigenschaft anstatt eine neue Eigenschaft für die Schattenkopien konfigurieren.

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
