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
# <a name="inheritance-relational-database"></a><span data-ttu-id="94db7-102">Vererbung (relationale Datenbank)</span><span class="sxs-lookup"><span data-stu-id="94db7-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="94db7-103">Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="94db7-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="94db7-104">Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="94db7-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="94db7-105">Vererbung in der EF-Modell dient zum Steuern, wie Vererbung in Entitätsklassen in der Datenbank dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="94db7-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="94db7-106">Derzeit wird nur das Tabelle pro Hierarchie (TPH)-Muster in EF Core implementiert.</span><span class="sxs-lookup"><span data-stu-id="94db7-106">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="94db7-107">Weitere allgemeine Muster wie die Tabelle pro Typ (TPT) und Tabelle pro konkret-Typ (TPC) sind noch nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="94db7-107">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="94db7-108">Konventionen</span><span class="sxs-lookup"><span data-stu-id="94db7-108">Conventions</span></span>

<span data-ttu-id="94db7-109">Gemäß der Konvention wird Vererbung mit dem Muster für die Tabelle pro Hierarchie (TPH) zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="94db7-109">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="94db7-110">TPH verwendet eine einzelne Tabelle zum Speichern der Daten für alle Typen in der Hierarchie.</span><span class="sxs-lookup"><span data-stu-id="94db7-110">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="94db7-111">Eine Unterscheidungsspalte wird verwendet, um welche Art identifizieren jede Zeile stellt.</span><span class="sxs-lookup"><span data-stu-id="94db7-111">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="94db7-112">EF Core nur Vererbung richtet, wenn mindestens zwei geerbte Typen explizit in das Modell eingeschlossen sind (finden Sie unter [Vererbung](../inheritance.md) Weitere Details).</span><span class="sxs-lookup"><span data-stu-id="94db7-112">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="94db7-113">Im folgenden Beispiel wird eine einfache Vererbungsszenario und die Daten in einer relationalen Datenbanktabelle TPH Muster gespeichert.</span><span class="sxs-lookup"><span data-stu-id="94db7-113">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="94db7-114">Die *Unterscheidungseigenschaft* Spalte identifiziert, welche Art von *Blog* in jeder Zeile gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="94db7-114">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="94db7-116">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="94db7-116">Data Annotations</span></span>

<span data-ttu-id="94db7-117">Sie können keine Datenanmerkungen verwenden, um Vererbung zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="94db7-117">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="94db7-118">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="94db7-118">Fluent API</span></span>

<span data-ttu-id="94db7-119">Sie können die Fluent-API verwenden, so konfigurieren Sie den Namen und Typ der Diskriminatorspalte sowie die Werte, die verwendet werden, um jeden Typ in der Hierarchie zu identifizieren.</span><span class="sxs-lookup"><span data-stu-id="94db7-119">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

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
