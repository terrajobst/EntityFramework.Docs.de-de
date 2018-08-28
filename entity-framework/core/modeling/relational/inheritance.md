---
title: Vererbung (relationale Datenbank) – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 019893ec8268ef9e59d581799a13d63610c80616
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996321"
---
# <a name="inheritance-relational-database"></a>Vererbung (relationale Datenbank)

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken. Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).

Vererbung in das EF-Modell dient zum Steuern, wie die Vererbung in Entitätsklassen in der Datenbank dargestellt wird.

> [!NOTE]  
> Derzeit wird nur die Tabelle pro Hierarchie (TPH)-Muster in EF Core implementiert. Weitere allgemeine Muster wie die Tabelle pro Typ (TPT) und Tabelle pro konkreten-Typ (TPC) sind noch nicht verfügbar.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention wird Vererbung mit dem Muster für die Tabelle pro Hierarchie (TPH) zugeordnet werden. TPH verwendet eine einzelne Tabelle, um die Daten für alle Typen in der Hierarchie zu speichern. Eine Diskriminatorspalte wird verwendet, um welche Art zu identifizieren, jede Zeile steht.

EF Core wird nur die Vererbung eingerichtet, wenn mindestens zwei geerbte Typen explizit in das Modell enthalten sind (finden Sie unter [Vererbung](../inheritance.md) Weitere Details).

Im folgenden finden ein Beispiel für ein Szenario für die einfache Vererbung und die Daten, die in einer relationalen Datenbanktabelle, die mit dem TPH-Muster. Die *Diskriminator* Spalte identifiziert, welche Art von *Blog* in jeder Zeile gespeichert wird.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/Conventions/Samples/InheritanceDbSets.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<RssBlog> RssBlogs { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

![Bild](_static/inheritance-tph-data.png)

## <a name="data-annotations"></a>Datenanmerkungen

Sie können nicht von Datenanmerkungen verwenden, die Vererbung konfigurieren.

## <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, konfigurieren Sie den Namen und Typ der Unterscheidungsspalte und die Werte, die verwendet werden, um jeden Typ in der Hierarchie zu identifizieren.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/InheritanceTPHDiscriminator.cs?highlight=7,8,9,10)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("blog_type")
            .HasValue<Blog>("blog_base")
            .HasValue<RssBlog>("blog_rss");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

## <a name="configuring-the-discriminator-property"></a>Konfigurieren von Unterscheidungseigenschaft

In den Beispielen oben wird als Diskriminator erstellt eine [schatteneigenschaft](xref:core/modeling/shadow-properties) auf der Basisentität der Hierarchie. Da es sich um eine Eigenschaft im Modell ist, können sie genau wie andere Eigenschaften konfiguriert werden. Geben Sie beispielsweise Folgendes ein, um die maximale Länge festgelegt werden, wenn die Standardeinstellung, nach Konvention Diskriminator verwendet wird:

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

Die Unterscheidung kann auch auf eine tatsächliche CLR-Eigenschaft in der Entität zugeordnet werden. Zum Beispiel:
```C#
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasDiscriminator<string>("BlogType");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public string BlogType { get; set; }
}

public class RssBlog : Blog
{
    public string RssUrl { get; set; }
}
```

Kombinieren diese beiden Schritte ist es möglich, sowohl die Unterscheidung einer Echtzeit-Eigenschaft zugeordnet, und konfigurieren Sie sie:
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
