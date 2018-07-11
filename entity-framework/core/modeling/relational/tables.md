---
title: Der Tabellenzuordnung – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
ms.technology: entity-framework-core
uid: core/modeling/relational/tables
ms.openlocfilehash: ef6080b5d543c2a68a680be2b9effc1c9d531030
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949008"
---
# <a name="table-mapping"></a><span data-ttu-id="6de27-102">Tabellenzuordnung</span><span class="sxs-lookup"><span data-stu-id="6de27-102">Table Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="6de27-103">Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="6de27-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="6de27-104">Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="6de27-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="6de27-105">Zuordnung zu einer Tabelle identifiziert die Tabellendaten aus abgefragt und in der Datenbank gespeichert werden sollten.</span><span class="sxs-lookup"><span data-stu-id="6de27-105">Table mapping identifies which table data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="6de27-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="6de27-106">Conventions</span></span>

<span data-ttu-id="6de27-107">Gemäß der Konvention, jede Entität wird so eingerichtet werden in eine Tabelle mit dem gleichen Namen wie ordnen die `DbSet<TEntity>` -Eigenschaft, die die Entität für den abgeleiteten Kontext verfügbar macht.</span><span class="sxs-lookup"><span data-stu-id="6de27-107">By convention, each entity will be set up to map to a table with the same name as the `DbSet<TEntity>` property that exposes the entity on the derived context.</span></span> <span data-ttu-id="6de27-108">Wenn kein `DbSet<TEntity>` enthalten ist für die angegebene Entität, den Namen der Klasse verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="6de27-108">If no `DbSet<TEntity>` is included for the given entity, the class name is used.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="6de27-109">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="6de27-109">Data Annotations</span></span>

<span data-ttu-id="6de27-110">Sie können Datenanmerkungen verwenden, so konfigurieren Sie die Tabelle, der die ein Typ zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="6de27-110">You can use Data Annotations to configure the table that a type maps to.</span></span>

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

<span data-ttu-id="6de27-111">Sie können auch ein Schema angeben, zu der die Tabelle gehört.</span><span class="sxs-lookup"><span data-stu-id="6de27-111">You can also specify a schema that the table belongs to.</span></span>

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="6de27-112">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="6de27-112">Fluent API</span></span>

<span data-ttu-id="6de27-113">Sie können die Fluent-API verwenden, so konfigurieren Sie die Tabelle, der die ein Typ zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="6de27-113">You can use the Fluent API to configure the table that a type maps to.</span></span>

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

<span data-ttu-id="6de27-114">Sie können auch ein Schema angeben, zu der die Tabelle gehört.</span><span class="sxs-lookup"><span data-stu-id="6de27-114">You can also specify a schema that the table belongs to.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
