---
title: Vererbung (relationale Datenbank)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
uid: core/modeling/relational/inheritance
ms.openlocfilehash: c660107619470a726fe13ad8eee2850749e6dcd9
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812081"
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="2b4ad-102">Vererbung (relationale Datenbank)</span><span class="sxs-lookup"><span data-stu-id="2b4ad-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="2b4ad-103">Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="2b4ad-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="2b4ad-104">Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="2b4ad-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="2b4ad-105">Die Vererbung im EF-Modell wird verwendet, um zu steuern, wie die Vererbung in den Entitäts Klassen in der Datenbank dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="2b4ad-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="2b4ad-106">Derzeit wird nur das TPH-Muster (Table-per Hierarchy) in EF Core implementiert.</span><span class="sxs-lookup"><span data-stu-id="2b4ad-106">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="2b4ad-107">Andere gängige Muster wie Tabelle pro Typ (TPT) und Table-per-konkrete-Type (TPC) sind noch nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="2b4ad-107">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="2b4ad-108">Konventionen</span><span class="sxs-lookup"><span data-stu-id="2b4ad-108">Conventions</span></span>

<span data-ttu-id="2b4ad-109">Gemäß der Konvention wird die Vererbung mithilfe des TPH-Musters (Table-per Hierarchy) zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="2b4ad-109">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="2b4ad-110">TPH verwendet eine einzelne Tabelle, um die Daten für alle Typen in der Hierarchie zu speichern.</span><span class="sxs-lookup"><span data-stu-id="2b4ad-110">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="2b4ad-111">Eine diskriminatorspalte wird verwendet, um den Typ zu identifizieren, den jede Zeile darstellt.</span><span class="sxs-lookup"><span data-stu-id="2b4ad-111">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="2b4ad-112">EF Core wird nur dann eine Vererbung eingerichtet, wenn mindestens zwei geerbte Typen explizit in das Modell eingeschlossen werden (Weitere Informationen finden Sie unter [Vererbung](../inheritance.md) ).</span><span class="sxs-lookup"><span data-stu-id="2b4ad-112">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="2b4ad-113">Im folgenden finden Sie ein Beispiel für ein einfaches Vererbungs Szenario und die in einer relationalen Datenbanktabelle gespeicherten Daten mithilfe des TPH-Musters.</span><span class="sxs-lookup"><span data-stu-id="2b4ad-113">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="2b4ad-114">Die *diskriminatorspalte* gibt an, welcher Typ von *Blog* in den einzelnen Zeilen gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="2b4ad-114">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/Conventions/InheritanceDbSets.cs)] -->
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

>[!NOTE]
> <span data-ttu-id="2b4ad-116">Bei Verwendung der TPH-Zuordnung werden bei Bedarf automatisch NULL-Werte für Daten Bank Spalten festgelegt.</span><span class="sxs-lookup"><span data-stu-id="2b4ad-116">Database columns are automatically made nullable as necessary when using TPH mapping.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="2b4ad-117">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="2b4ad-117">Data Annotations</span></span>

<span data-ttu-id="2b4ad-118">Zum Konfigurieren der Vererbung können keine Daten Anmerkungen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="2b4ad-118">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="2b4ad-119">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="2b4ad-119">Fluent API</span></span>

<span data-ttu-id="2b4ad-120">Sie können die fließende API verwenden, um den Namen und den Typ der diskriminatorspalte und die Werte zu konfigurieren, die zum Identifizieren der einzelnen Typen in der Hierarchie verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="2b4ad-120">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs?highlight=7,8,9,10)] -->
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

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="2b4ad-121">Konfigurieren der diskriminatoreigenschaft</span><span class="sxs-lookup"><span data-stu-id="2b4ad-121">Configuring the discriminator property</span></span>

<span data-ttu-id="2b4ad-122">In den obigen Beispielen wird der Diskriminator als [Schatten Eigenschaft](xref:core/modeling/shadow-properties) für die Basis Entität der Hierarchie erstellt.</span><span class="sxs-lookup"><span data-stu-id="2b4ad-122">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="2b4ad-123">Da es sich um eine Eigenschaft im Modell handelt, kann Sie genau wie andere Eigenschaften konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="2b4ad-123">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="2b4ad-124">So legen Sie z. b. die maximale Länge fest, wenn der standardmäßige Diskriminator verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="2b4ad-124">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

```C#
modelBuilder.Entity<Blog>()
    .Property("Discriminator")
    .HasMaxLength(200);
```

<span data-ttu-id="2b4ad-125">Der Diskriminator kann auch einer tatsächlichen CLR-Eigenschaft in der Entität zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="2b4ad-125">The discriminator can also be mapped to an actual CLR property in your entity.</span></span> <span data-ttu-id="2b4ad-126">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="2b4ad-126">For example:</span></span>

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

<span data-ttu-id="2b4ad-127">Wenn Sie diese beiden Elemente miteinander kombinieren, ist es möglich, den Diskriminator einer echten Eigenschaft zuzuordnen und zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="2b4ad-127">Combining these two things together it is possible to both map the discriminator to a real property and configure it:</span></span>

```C#
modelBuilder.Entity<Blog>(b =>
{
    b.HasDiscriminator<string>("BlogType");

    b.Property(e => e.BlogType)
        .HasMaxLength(200)
        .HasColumnName("blog_type");
});
```
