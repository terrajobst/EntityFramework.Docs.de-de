---
title: Tabellenzuordnung - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
ms.technology: entity-framework-core
uid: core/modeling/relational/tables
ms.openlocfilehash: 73957d9c77e6856bfb65e10e6b373c337101f7d9
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052730"
---
# <a name="table-mapping"></a><span data-ttu-id="d8039-102">Zuordnung zu einer Tabelle</span><span class="sxs-lookup"><span data-stu-id="d8039-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="d8039-103">Die Konfiguration in diesem Abschnitt ist im Allgemeinen gilt für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="d8039-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="d8039-104">Die Erweiterungsmethoden, die hier gezeigten werden verfügbar, wenn Sie einen relationale Datenbank-Anbieter installieren (aufgrund der freigegebenen *Microsoft.EntityFrameworkCore.Relational* Paket).</span><span class="sxs-lookup"><span data-stu-id="d8039-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="d8039-105">Tabellenzuordnung identifiziert, welche Tabellendaten abgefragt wurden und in der Datenbank gespeichert werden soll.</span><span class="sxs-lookup"><span data-stu-id="d8039-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="d8039-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="d8039-106">Conventions</span></span>

<span data-ttu-id="d8039-107">Jede Entität werden gemäß der Konvention Setup zuordnen zu einer Tabelle mit dem gleichen Namen wie die `DbSet<TEntity>` Eigenschaft, die die Entität für den abgeleiteten Kontext verfügbar macht.</span><span class="sxs-lookup"><span data-stu-id="d8039-107">By convention, each entity will be setup to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="d8039-108">Wenn kein `DbSet<TEntity>` enthalten ist für die angegebene Entität wird der Name der Klasse verwendet.</span><span class="sxs-lookup"><span data-stu-id="d8039-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="d8039-109">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="d8039-109">Data Annotations</span></span>

<span data-ttu-id="d8039-110">Sie können Datenanmerkungen verwenden, so konfigurieren Sie die Tabelle, der ein Typ zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="d8039-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

``` csharp
using System.ComponentModel.DataAnnotations.Schema;
```
``` csharp
[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="d8039-111">Sie können auch ein Schema angeben, zu der die Tabelle gehört.</span><span class="sxs-lookup"><span data-stu-id="d8039-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="d8039-112">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="d8039-112">Fluent API</span></span>

<span data-ttu-id="d8039-113">Sie können die Fluent-API verwenden, so konfigurieren Sie die Tabelle, der ein Typ zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="d8039-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
```
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .ToTable("blogs");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="d8039-114">Sie können auch ein Schema angeben, zu der die Tabelle gehört.</span><span class="sxs-lookup"><span data-stu-id="d8039-114">You can also specify a schema that the table belongs to.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
