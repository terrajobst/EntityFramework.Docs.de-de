---
title: "Einschließlich & Arten - EF Core ausschließen"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
ms.technology: entity-framework-core
uid: core/modeling/included-types
ms.openlocfilehash: a8d7293a144968d2506bdcc76e55a1a0b1e3fd4b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="including--excluding-types"></a>Einschließlich & Typen ausschließen

Einschließlich eines Typs in das Modell bedeutet, denen EF über Metadaten verfügt, die geben und versucht, lesen und Schreiben von Instanzen von/zu der Datenbank.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention werden die Typen, die in verfügbar gemacht werden `DbSet` Eigenschaften auf den Kontext in Ihrem Modell enthalten sind. Typen, die in erwähnt werden außerdem die `OnModelCreating` -Methode sind ebenfalls enthalten. Schließlich sind alle Typen, die durch Untersuchen der Eigenschaften von ermittelten Typen rekursiv gefunden werden ebenfalls im Modell enthalten.

**Im folgenden Codebeispiel werden z. B. alle drei Typen ermittelt:**

* `Blog`Da es in verfügbar gemacht wurde eine `DbSet` Eigenschaft im Kontext

* `Post`Da es sich bei der Erkennung über die `Blog.Posts` Navigationseigenschaft

* `AuditEntry`Da es in erwähnte`OnModelCreating`

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/IncludedTypes.cs?highlight=3,7,16)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<AuditEntry>();
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

public class AuditEntry
{
    public int AuditEntryId { get; set; }
    public string Username { get; set; }
    public string Action { get; set; }
}
```

## <a name="data-annotations"></a>Datenanmerkungen

Sie können Datenanmerkungen verwenden, um einen Typ aus dem Modell auszuschließen.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/IgnoreType.cs?highlight=9)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogMetadata Metadata { get; set; }
}

[NotMapped]
public class BlogMetadata
{
    public DateTime LoadedFromDatabase { get; set; }
}
```

## <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, um einen Typ aus dem Modell auszuschließen.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IgnoreType.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Ignore<BlogMetadata>();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogMetadata Metadata { get; set; }
}

public class BlogMetadata
{
    public DateTime LoadedFromDatabase { get; set; }
}
```
