---
title: Alternative Schlüssel-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: 87df5d174a1db12fb3ab763ac76c3b863a83087e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197474"
---
# <a name="alternate-keys"></a>Alternativschlüssel

Ein alternativer Schlüssel dient als alternativer eindeutiger Bezeichner für jede Entitäts Instanz zusätzlich zum Primärschlüssel. Alternative Schlüssel können als Ziel einer Beziehung verwendet werden. Bei Verwendung einer relationalen Datenbank wird diese dem Konzept eines eindeutigen Indexes/einer eindeutigen Einschränkung in den alternativen Schlüssel Spalten und einer oder mehrerer Foreign Key-Einschränkungen zugeordnet, die auf die Spalte (n) verweisen.

> [!TIP]  
> Wenn Sie nur die Eindeutigkeit einer Spalte erzwingen möchten, benötigen Sie einen eindeutigen Index anstelle eines alternativen Schlüssels. Weitere Informationen finden Sie unter [Indizes](indexes.md). In EF bieten alternative Schlüssel mehr Funktionalität als eindeutige Indizes, da Sie als Ziel eines fremd Schlüssels verwendet werden können.

Alternative Schlüssel werden in der Regel bei Bedarf für Sie eingeführt, und Sie müssen Sie nicht manuell konfigurieren. Weitere Informationen finden Sie unter [Konventionen](#conventions) .

## <a name="conventions"></a>Konventionen

Gemäß der Konvention wird ein alternativer Schlüssel für Sie eingeführt, wenn Sie eine Eigenschaft, die nicht der Primärschlüssel ist, als Ziel einer Beziehung identifizieren.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/AlternateKey.cs?highlight=12)] -->
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

Alternative Schlüssel können nicht mithilfe von Daten Anmerkungen konfiguriert werden.

## <a name="fluent-api"></a>Fluent-API

Sie können die fließende API verwenden, um eine einzelne Eigenschaft als alternativen Schlüssel zu konfigurieren.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?highlight=7,8)] -->
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

Sie können auch die fließende API verwenden, um mehrere Eigenschaften als alternativen Schlüssel (als zusammengesetzten alternativen Schlüssel bezeichnet) zu konfigurieren.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?highlight=7,8)] -->
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
