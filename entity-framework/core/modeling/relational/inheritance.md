---
title: Vererbung (Relational Database) - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
ms.technology: entity-framework-core
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 55286adf08a6a1c3286b7059d747a62e1feffd22
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/22/2017
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
