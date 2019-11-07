---
title: Neue Features in EF Core 2.2 – EF Core
author: divega
ms.date: 11/14/2018
ms.assetid: 998C04F3-676A-4FCF-8450-CFB0457B4198
uid: core/what-is-new/ef-core-2.2
ms.openlocfilehash: fb9de799753bebd7b4092cd8f4af74703dee3e45
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656186"
---
# <a name="new-features-in-ef-core-22"></a><span data-ttu-id="22483-102">Neue Features in EF Core 2.2</span><span class="sxs-lookup"><span data-stu-id="22483-102">New features in EF Core 2.2</span></span>

## <a name="spatial-data-support"></a><span data-ttu-id="22483-103">Unterstützung räumlicher Daten</span><span class="sxs-lookup"><span data-stu-id="22483-103">Spatial data support</span></span>

<span data-ttu-id="22483-104">Räumliche Daten können verwendet werden, um die physische Position und die Form von Objekten darzustellen.</span><span class="sxs-lookup"><span data-stu-id="22483-104">Spatial data can be used to represent the physical location and shape of objects.</span></span>
<span data-ttu-id="22483-105">Viele Datenbanken können räumliche Daten nativ speichern, indizieren und abfragen.</span><span class="sxs-lookup"><span data-stu-id="22483-105">Many databases can natively store, index, and query spatial data.</span></span>
<span data-ttu-id="22483-106">Häufige Szenarien sind das Abfragen von Objekten mit einer bestimmten Distanz und das Testen, ob ein Polygon eine bestimmte Position enthält.</span><span class="sxs-lookup"><span data-stu-id="22483-106">Common scenarios include querying for objects within a given distance, and testing if a polygon contains a given location.</span></span>
<span data-ttu-id="22483-107">EF Core 2.2 unterstützt nun das Arbeiten mit räumlichen Daten aus verschiedenen Datenbanken mit Typen aus der [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite)-Bibliothek (NTS-Bibliothek).</span><span class="sxs-lookup"><span data-stu-id="22483-107">EF Core 2.2 now supports working with spatial data from various databases using types from the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) (NTS) library.</span></span>

<span data-ttu-id="22483-108">Die Unterstützung räumlicher Daten wird als eine Reihe von anbieterspezifischen Erweiterungspaketen implementiert.</span><span class="sxs-lookup"><span data-stu-id="22483-108">Spatial data support is implemented as a series of provider-specific extension packages.</span></span>
<span data-ttu-id="22483-109">Jedes dieser Pakete stellt Mappings für NTS-Typen und -Methoden sowie die entsprechenden räumlichen Typen und Funktionen in der Datenbank bereit.</span><span class="sxs-lookup"><span data-stu-id="22483-109">Each of these packages contributes mappings for NTS types and methods, and the corresponding spatial types and functions in the database.</span></span>
<span data-ttu-id="22483-110">Diese Anbietererweiterungen sind nun für [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/) und [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (aus dem [Npgsql-Projekt](https://www.npgsql.org/)) verfügbar.</span><span class="sxs-lookup"><span data-stu-id="22483-110">Such provider extensions are now available for [SQL Server](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite/), [SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite/), and [PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite/) (from the [Npgsql project](https://www.npgsql.org/)).</span></span>
<span data-ttu-id="22483-111">Räumliche Typen können direkt mit dem [EF Core-In-Memory-Anbieter](xref:core/providers/in-memory/index) ohne zusätzliche Erweiterungen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="22483-111">Spatial types can be used directly with the [EF Core in-memory provider](xref:core/providers/in-memory/index) without additional extensions.</span></span>

<span data-ttu-id="22483-112">Wenn die Anbietererweiterung installiert ist, können Sie Ihren Entitäten Eigenschaften der unterstützten Typen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="22483-112">Once the provider extension is installed, you can add properties of supported types to your entities.</span></span> <span data-ttu-id="22483-113">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="22483-113">For example:</span></span>

``` csharp
using NetTopologySuite.Geometries;

namespace MyApp
{
  public class Friend
  {
    [Key]
    public string Name { get; set; }
  
    [Required]
    public Point Location { get; set; }
  }
}
```

<span data-ttu-id="22483-114">Dann können Entitäten neben räumlichen Daten bestehen:</span><span class="sxs-lookup"><span data-stu-id="22483-114">You can then persist entities with spatial data:</span></span>

``` csharp
using (var context = new MyDbContext())
{
    context.Add(
        new Friend
        {
            Name = "Bill",
            Location = new Point(-122.34877, 47.6233355) {SRID = 4326 }
        });
    context.SaveChanges();
}
```

<span data-ttu-id="22483-115">Außerdem können Sie Datenbankabfragen auf Grundlage von räumlichen Daten und Vorgängen ausführen:</span><span class="sxs-lookup"><span data-stu-id="22483-115">And you can execute database queries based on spatial data and operations:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="22483-116">Weitere Informationen zu diesem Feature finden Sie in der [Dokumentation über räumliche Typen](xref:core/modeling/spatial).</span><span class="sxs-lookup"><span data-stu-id="22483-116">For more information on this feature, see the [spatial types documentation](xref:core/modeling/spatial).</span></span>

## <a name="collections-of-owned-entities"></a><span data-ttu-id="22483-117">Sammlungen von nicht eigenständigen Entitäten</span><span class="sxs-lookup"><span data-stu-id="22483-117">Collections of owned entities</span></span>

<span data-ttu-id="22483-118">EF Core 2.0 bietet nun die Option, die Eigentümerschaft in 1:1-Zuordnungen zu modellieren.</span><span class="sxs-lookup"><span data-stu-id="22483-118">EF Core 2.0 added the ability to model ownership in one-to-one associations.</span></span>
<span data-ttu-id="22483-119">EF Core 2.2 erweitert die Funktion, die Eigentümerschaft in 1:n-Zuordnungen auszudrücken.</span><span class="sxs-lookup"><span data-stu-id="22483-119">EF Core 2.2 extends the ability to express ownership to one-to-many associations.</span></span>
<span data-ttu-id="22483-120">Besitzer können einschränken, wie Entitäten verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="22483-120">Ownership helps constrain how entities are used.</span></span>

<span data-ttu-id="22483-121">Nicht eigenständige Entitäten:</span><span class="sxs-lookup"><span data-stu-id="22483-121">For example, owned entities:</span></span>

- <span data-ttu-id="22483-122">Können immer nur in Navigationseigenschaften anderer Entitätstypen angezeigt werden</span><span class="sxs-lookup"><span data-stu-id="22483-122">Can only ever appear on navigation properties of other entity types.</span></span>
- <span data-ttu-id="22483-123">Werden automatisch geladen und können nur von einem „DbContext“-Objekt vom Besitzer verfolgt werden</span><span class="sxs-lookup"><span data-stu-id="22483-123">Are automatically loaded, and can only be tracked by a DbContext alongside their owner.</span></span>

<span data-ttu-id="22483-124">In relationalen Datenbanken werden Sammlungen mit Besitzern separaten Tabellen des Besitzers zugeordnet, wie normale 1:n-Zuordnungen.</span><span class="sxs-lookup"><span data-stu-id="22483-124">In relational databases, owned collections are mapped to separate tables from the owner, just like regular one-to-many associations.</span></span>
<span data-ttu-id="22483-125">Für dokumentorientierte Datenbanken planen wir jedoch eine Schachtelung nicht eigenständiger Entitäten (in nicht eigenständigen Sammlungen oder Referenzen) innerhalb des dem Besitzer zugeordneten Dokuments.</span><span class="sxs-lookup"><span data-stu-id="22483-125">But in document-oriented databases, we plan to nest owned entities (in owned collections or references) within the same document as the owner.</span></span>

<span data-ttu-id="22483-126">Sie können die Funktion verwenden, indem Sie die neue OwnsMany()-API aufrufen:</span><span class="sxs-lookup"><span data-stu-id="22483-126">You can use the feature by calling the new OwnsMany() API:</span></span>

``` csharp
modelBuilder.Entity<Customer>().OwnsMany(c => c.Addresses);
```

<span data-ttu-id="22483-127">Weitere Informationen finden Sie in der [aktualisierten Dokumentation über nicht eigenständige Entitäten](xref:core/modeling/owned-entities#collections-of-owned-types).</span><span class="sxs-lookup"><span data-stu-id="22483-127">For more information, see the [updated owned entities documentation](xref:core/modeling/owned-entities#collections-of-owned-types).</span></span>

## <a name="query-tags"></a><span data-ttu-id="22483-128">Abfragetags</span><span class="sxs-lookup"><span data-stu-id="22483-128">Query tags</span></span>

<span data-ttu-id="22483-129">Diese Funktion vereinfacht das Korrelieren zwischen LINQ-Abfragen im Code und generierten SQL-Abfragen, die in Protokollen erfasst werden.</span><span class="sxs-lookup"><span data-stu-id="22483-129">This feature simplifies the correlation of LINQ queries in code with generated SQL queries captured in logs.</span></span>

<span data-ttu-id="22483-130">Kommentieren Sie eine LINQ-Abfrage mit der neuen TagWith()-Methode, um Abfragetags zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="22483-130">To take advantage of query tags, you annotate a LINQ query using the new TagWith() method.</span></span>
<span data-ttu-id="22483-131">So sieht die Verwendung der Abfrage nach räumlichen Daten aus einem vorherigen Beispiel aus:</span><span class="sxs-lookup"><span data-stu-id="22483-131">Using the spatial query from a previous example:</span></span>

``` csharp
  var nearestFriends =
      (from f in context.Friends.TagWith(@"This is my spatial query!")
      orderby f.Location.Distance(myLocation) descending
      select f).Take(5).ToList();
```

<span data-ttu-id="22483-132">Diese LINQ-Abfrage erzeugt die folgende SQL-Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="22483-132">This LINQ query will produce the following SQL output:</span></span>

``` sql
-- This is my spatial query!

SELECT TOP(@__p_1) [f].[Name], [f].[Location]
FROM [Friends] AS [f]
ORDER BY [f].[Location].STDistance(@__myLocation_0) DESC
```

<span data-ttu-id="22483-133">Weitere Informationen finden Sie in der [Dokumentation über Abfragetags](xref:core/querying/tags).</span><span class="sxs-lookup"><span data-stu-id="22483-133">For more information, see the [query tags documentation](xref:core/querying/tags).</span></span>
