---
title: Alternative Schlüssel – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: b26d8bc1630af9e811d9c4e7da850a618bc8042e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996970"
---
# <a name="alternate-keys"></a>Alternative Schlüssel

Ein alternativer Schlüssel dient als eines alternativen eindeutigen Bezeichners für jede Entitätsinstanz zusätzlich zu den primären Schlüssel. Alternative Schlüssel können als Ziel einer Beziehung verwendet werden. Bei Verwendung eine relationale Datenbank wird hier das Konzept der eine eindeutige Index/Einschränkung für die alternative Schlüsselspalte(n) und eine oder mehrere foreign Key-Einschränkungen, die auf Spalten verweisen zugeordnet.

> [!TIP]  
> Wenn Sie nur die Eindeutigkeit einer Spalte zu erzwingen, sollten Sie einen eindeutigen Index anstelle eines alternativen Schlüssels möchten, finden Sie unter [Indizes](indexes.md). In EF bieten alternative Schlüssel mehr Funktionen als eindeutige Indizes, da sie als Ziel eines Fremdschlüssels verwendet werden können.

Alternative Schlüssel werden in der Regel für die Sie bei Bedarf eingeführt, und Sie müssen nicht manuell konfigurieren. Finden Sie unter [Konventionen](#conventions) Weitere Details.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention wird als alternativer Schlüssel für Sie eingeführt, wenn Sie eine Eigenschaft identifizieren, die den Primärschlüssel, nicht als Ziel einer Beziehung ist.

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

Alternative Schlüssel können nicht mithilfe von Datenanmerkungen konfiguriert werden.

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

Sie können auch die Fluent-API verwenden, so konfigurieren Sie mehrere Eigenschaften um einen alternativen Schlüssel (bekannt als einen zusammengesetzten Schlüssel für die alternative) sein.

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
