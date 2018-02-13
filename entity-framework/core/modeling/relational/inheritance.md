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
# <a name="inheritance-relational-database"></a><span data-ttu-id="8a5c1-102">Vererbung (relationale Datenbank)</span><span class="sxs-lookup"><span data-stu-id="8a5c1-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="8a5c1-103">Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="8a5c1-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="8a5c1-104">Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="8a5c1-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="8a5c1-105">Vererbung in der EF-Modell dient zum Steuern, wie Vererbung in Entitätsklassen in der Datenbank dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="8a5c1-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="8a5c1-106">Derzeit wird nur das Tabelle pro Hierarchie (TPH)-Muster in EF Core implementiert.</span><span class="sxs-lookup"><span data-stu-id="8a5c1-106">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="8a5c1-107">Weitere allgemeine Muster wie die Tabelle pro Typ (TPT) und Tabelle pro konkret-Typ (TPC) sind noch nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="8a5c1-107">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="8a5c1-108">Konventionen</span><span class="sxs-lookup"><span data-stu-id="8a5c1-108">Conventions</span></span>

<span data-ttu-id="8a5c1-109">Gemäß der Konvention wird Vererbung mit dem Muster für die Tabelle pro Hierarchie (TPH) zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="8a5c1-109">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="8a5c1-110">TPH verwendet eine einzelne Tabelle zum Speichern der Daten für alle Typen in der Hierarchie.</span><span class="sxs-lookup"><span data-stu-id="8a5c1-110">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="8a5c1-111">Eine Unterscheidungsspalte wird verwendet, um welche Art identifizieren jede Zeile stellt.</span><span class="sxs-lookup"><span data-stu-id="8a5c1-111">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="8a5c1-112">EF Core nur Vererbung richtet, wenn mindestens zwei geerbte Typen explizit in das Modell eingeschlossen sind (finden Sie unter [Vererbung](../inheritance.md) Weitere Details).</span><span class="sxs-lookup"><span data-stu-id="8a5c1-112">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="8a5c1-113">Im folgenden Beispiel wird eine einfache Vererbungsszenario und die Daten in einer relationalen Datenbanktabelle TPH Muster gespeichert.</span><span class="sxs-lookup"><span data-stu-id="8a5c1-113">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="8a5c1-114">Die *Unterscheidungseigenschaft* Spalte identifiziert, welche Art von *Blog* in jeder Zeile gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="8a5c1-114">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="8a5c1-116">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="8a5c1-116">Data Annotations</span></span>

<span data-ttu-id="8a5c1-117">Sie können keine Datenanmerkungen verwenden, um Vererbung zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="8a5c1-117">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="8a5c1-118">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="8a5c1-118">Fluent API</span></span>

<span data-ttu-id="8a5c1-119">Sie können die Fluent-API verwenden, so konfigurieren Sie den Namen und Typ der Diskriminatorspalte sowie die Werte, die verwendet werden, um jeden Typ in der Hierarchie zu identifizieren.</span><span class="sxs-lookup"><span data-stu-id="8a5c1-119">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

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

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="8a5c1-120">Konfigurieren die Unterscheidungseigenschaft</span><span class="sxs-lookup"><span data-stu-id="8a5c1-120">Configuring the discriminator property</span></span>

<span data-ttu-id="8a5c1-121">In den Beispielen oben wird als Diskriminators erstellt eine [Shadowing Eigenschaft](xref:core/modeling/shadow-properties) auf der Basisentität der Hierarchie.</span><span class="sxs-lookup"><span data-stu-id="8a5c1-121">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="8a5c1-122">Da es sich um eine Eigenschaft im Modell handelt, können sie genau wie andere Eigenschaften konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="8a5c1-122">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="8a5c1-123">Geben Sie beispielsweise Folgendes ein, um die maximale Länge festgelegt werden soll, wenn die standardmäßig von Konvention Unterscheidungseigenschaft verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="8a5c1-123">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

<span data-ttu-id="8a5c1-124">Diskriminators kann auch eine tatsächliche CLR-Eigenschaft in der Entität zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="8a5c1-124">The discriminator can also be mapped to an actual CLR property in your entity.</span></span> <span data-ttu-id="8a5c1-125">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="8a5c1-125">For example:</span></span>
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

<span data-ttu-id="8a5c1-126">Kombinieren diese zwei Dinge ist es möglich, sowohl eine reelle Eigenschaft Diskriminators zuordnen, und konfigurieren Sie ihn:</span><span class="sxs-lookup"><span data-stu-id="8a5c1-126">Combining these two things together it is possible to both map the discriminator to a real property and configure it:</span></span>
```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
