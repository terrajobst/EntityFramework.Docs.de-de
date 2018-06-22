---
title: Alternativschlüssel - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
ms.technology: entity-framework-core
uid: core/modeling/alternate-keys
ms.openlocfilehash: 09f86a8932b71ec8f30ee90a088091a00233c20f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052470"
---
# <a name="alternate-keys"></a>Alternativen Schlüssel

Ein alternativer Schlüssel dient als eines alternativen eindeutigen Bezeichners für jede Entitätsinstanz zusätzlich zu den primären Schlüssel. Alternativer Schlüssel können als Ziel einer Beziehung verwendet werden. Bei Verwendung eine relationale Datenbank ordnet dies das Konzept einer eindeutigen Index/Einschränkung auf die alternativen Schlüsselspalte(n) und eine oder mehrere foreign Key-Einschränkungen, die die Spalten verweisen.

> [!TIP]  
> Wenn Sie nur die Eindeutigkeit einer Spalte zu erzwingen, sollten Sie einen eindeutigen Index, anstatt ein alternativer Schlüssel möchten, finden Sie unter [Indizes](indexes.md). In der EF Funktionalität Alternativschlüssel größer als eindeutige Indizes, da sie als Ziel eines Fremdschlüssels verwendet werden können.

Alternativen Schlüssel in der Regel bei Bedarf eingeführt, und Sie müssen nicht manuell konfigurieren. Finden Sie unter [Konventionen](#conventions) Weitere Details.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention ist ein alternativer Schlüssel für Sie eingeführt, wenn Sie eine Eigenschaft identifizieren, die den Primärschlüssel, nicht als Ziel einer Beziehung ist.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/AlternateKey.cs?highlight=12)] -->
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
            .HasForeignKey(p => p.BlogUrl)
            .HasPrincipalKey(b => b.Url);
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

    public string BlogUrl { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a>Datenanmerkungen

Alternativen Schlüssel können nicht mithilfe von Datenanmerkungen konfiguriert werden.

## <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, so konfigurieren Sie eine einzelne Eigenschaft, um einen alternativen Schlüssel sein.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeySingle.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => c.LicensePlate);
    }
}

class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```

Die Fluent-API können auch um mehrere Eigenschaften für einen alternativen Schlüssel (bekannt als eine zusammengesetzte Alternativschlüssel) werden zu konfigurieren.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/AlternateKeyComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasAlternateKey(c => new { c.State, c.LicensePlate });
    }
}

class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }
}
```
