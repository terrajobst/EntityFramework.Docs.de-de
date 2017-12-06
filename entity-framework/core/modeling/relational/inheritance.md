---
title: Vererbung (Relational Database) - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9a7c5488-aaf4-4b40-b1ff-f435ff30f6ec
ms.technology: entity-framework-core
uid: core/modeling/relational/inheritance
ms.openlocfilehash: a7f697dfe2b93c7b93a2dd14945732db4f37628c
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="b7e3a-102">Vererbung (relationale Datenbank)</span><span class="sxs-lookup"><span data-stu-id="b7e3a-102">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="b7e3a-103">Die Konfiguration in diesem Abschnitt ist im Allgemeinen gilt für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="b7e3a-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="b7e3a-104">Die Erweiterungsmethoden, die hier gezeigten werden verfügbar, wenn Sie einen relationale Datenbank-Anbieter installieren (aufgrund der freigegebenen *Microsoft.EntityFrameworkCore.Relational* Paket).</span><span class="sxs-lookup"><span data-stu-id="b7e3a-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="b7e3a-105">Vererbung in der EF-Modell dient zum Steuern, wie Vererbung in Entitätsklassen in der Datenbank dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="b7e3a-105">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="b7e3a-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="b7e3a-106">Conventions</span></span>

<span data-ttu-id="b7e3a-107">Gemäß der Konvention wird Vererbung mit dem Muster für die Tabelle pro Hierarchie (TPH) zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="b7e3a-107">By convention, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="b7e3a-108">TPH verwendet eine einzelne Tabelle zum Speichern der Daten für alle Typen in der Hierarchie.</span><span class="sxs-lookup"><span data-stu-id="b7e3a-108">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="b7e3a-109">Eine Unterscheidungsspalte wird verwendet, um welche Art identifizieren jede Zeile stellt.</span><span class="sxs-lookup"><span data-stu-id="b7e3a-109">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="b7e3a-110">EF wird nur die Vererbung einrichten, wenn mindestens zwei geerbte Typen explizit in das Modell eingeschlossen sind (finden Sie unter [Vererbung](../inheritance.md) Weitere Details).</span><span class="sxs-lookup"><span data-stu-id="b7e3a-110">EF will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="b7e3a-111">Im folgenden Beispiel wird eine einfache Vererbungsszenario und die Daten in einer relationalen Datenbanktabelle TPH Muster gespeichert.</span><span class="sxs-lookup"><span data-stu-id="b7e3a-111">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="b7e3a-112">Die *Unterscheidungseigenschaft* Spalte identifiziert, welche Art von *Blog* in jeder Zeile gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="b7e3a-112">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="b7e3a-114">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="b7e3a-114">Data Annotations</span></span>

<span data-ttu-id="b7e3a-115">Sie können keine Datenanmerkungen verwenden, um Vererbung zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="b7e3a-115">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="b7e3a-116">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="b7e3a-116">Fluent API</span></span>

<span data-ttu-id="b7e3a-117">Sie können die Fluent-API verwenden, so konfigurieren Sie den Namen und Typ der Diskriminatorspalte sowie die Werte, die verwendet werden, um jeden Typ in der Hierarchie zu identifizieren.</span><span class="sxs-lookup"><span data-stu-id="b7e3a-117">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

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
