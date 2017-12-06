---
title: Indizes - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: 683b580bb155e0416f13c5d63e3280078fbcee21
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="indexes"></a><span data-ttu-id="a86cf-102">Indizes</span><span class="sxs-lookup"><span data-stu-id="a86cf-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="a86cf-103">Die Konfiguration in diesem Abschnitt ist im Allgemeinen gilt für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="a86cf-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="a86cf-104">Die Erweiterungsmethoden, die hier gezeigten werden verfügbar, wenn Sie einen relationale Datenbank-Anbieter installieren (aufgrund der freigegebenen *Microsoft.EntityFrameworkCore.Relational* Paket).</span><span class="sxs-lookup"><span data-stu-id="a86cf-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="a86cf-105">Ein Index in einer relationalen Datenbank ordnet dasselbe Konzept wie ein Index im Kern des Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a86cf-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="a86cf-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="a86cf-106">Conventions</span></span>

<span data-ttu-id="a86cf-107">Indizes werden gemäß der Konvention benannt `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="a86cf-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="a86cf-108">Bei zusammengesetzten Indizes `<property name>` wird eine Unterstrich getrennte Liste von Eigenschaftsnamen.</span><span class="sxs-lookup"><span data-stu-id="a86cf-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="a86cf-109">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="a86cf-109">Data Annotations</span></span>

<span data-ttu-id="a86cf-110">Indizes können nicht mithilfe von Datenanmerkungen konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="a86cf-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="a86cf-111">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="a86cf-111">Fluent API</span></span>

<span data-ttu-id="a86cf-112">So konfigurieren Sie den Namen eines Indexes können Sie die Fluent-API verwenden.</span><span class="sxs-lookup"><span data-stu-id="a86cf-112">You can use the Fluent API to configure the name of an index.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/IndexName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .HasName("Index_Url");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
