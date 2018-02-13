---
title: Vererbung (Relational Database) - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
ms.technology: entity-framework-core
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 22eed0002b5903d3cfd18a7e4af0fcd2d46a5c4c
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2018
---
# <a name="inheritance-relational-database"></a>Vererbung (relationale Datenbank)

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken. Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).

Vererbung in der EF-Modell dient zum Steuern, wie Vererbung in Entitätsklassen in der Datenbank dargestellt wird.

> [!NOTE]  
> Derzeit wird nur das Tabelle pro Hierarchie (TPH)-Muster in EF Core implementiert. Weitere allgemeine Muster wie die Tabelle pro Typ (TPT) und Tabelle pro konkret-Typ (TPC) sind noch nicht verfügbar.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention wird Vererbung mit dem Muster für die Tabelle pro Hierarchie (TPH) zugeordnet werden. TPH verwendet eine einzelne Tabelle zum Speichern der Daten für alle Typen in der Hierarchie. Eine Unterscheidungsspalte wird verwendet, um welche Art identifizieren jede Zeile stellt.

EF Core nur Vererbung richtet, wenn mindestens zwei geerbte Typen explizit in das Modell eingeschlossen sind (finden Sie unter [Vererbung](../inheritance.md) Weitere Details).

Im folgenden Beispiel wird eine einfache Vererbungsszenario und die Daten in einer relationalen Datenbanktabelle TPH Muster gespeichert. Die *Unterscheidungseigenschaft* Spalte identifiziert, welche Art von *Blog* in jeder Zeile gespeichert wird.

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

Sie können keine Datenanmerkungen verwenden, um Vererbung zu konfigurieren.

## <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, so konfigurieren Sie den Namen und Typ der Diskriminatorspalte sowie die Werte, die verwendet werden, um jeden Typ in der Hierarchie zu identifizieren.

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

## <a name="configuring-the-discriminator-property"></a>Konfigurieren die Unterscheidungseigenschaft

In den Beispielen oben wird als Diskriminators erstellt eine [Shadowing Eigenschaft](xref:core/modeling/shadow-properties) auf der Basisentität der Hierarchie. Da es sich um eine Eigenschaft im Modell handelt, können sie genau wie andere Eigenschaften konfiguriert werden. Geben Sie beispielsweise Folgendes ein, um die maximale Länge festgelegt werden soll, wenn die standardmäßig von Konvention Unterscheidungseigenschaft verwendet wird:

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

Diskriminators kann auch eine tatsächliche CLR-Eigenschaft in der Entität zugeordnet werden. Zum Beispiel:
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

Kombinieren diese zwei Dinge ist es möglich, sowohl eine reelle Eigenschaft Diskriminators zuordnen, und konfigurieren Sie ihn:
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
