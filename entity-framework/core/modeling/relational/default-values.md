---
title: Standardwerte - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
ms.technology: entity-framework-core
uid: core/modeling/relational/default-values
ms.openlocfilehash: 73b916b6d9f9c984c8ea010f2319eafa7d031a58
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="default-values"></a><span data-ttu-id="5580b-102">Standardwerte</span><span class="sxs-lookup"><span data-stu-id="5580b-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="5580b-103">Die Konfiguration in diesem Abschnitt ist im Allgemeinen gilt für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="5580b-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="5580b-104">Die Erweiterungsmethoden, die hier gezeigten werden verfügbar, wenn Sie einen relationale Datenbank-Anbieter installieren (aufgrund der freigegebenen *Microsoft.EntityFrameworkCore.Relational* Paket).</span><span class="sxs-lookup"><span data-stu-id="5580b-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="5580b-105">Der Standardwert einer Spalte ist der Wert, der eingefügt werden soll, wenn eine neue Zeile eingefügt wird, aber kein Wert für die Spalte angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="5580b-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="5580b-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="5580b-106">Conventions</span></span>

<span data-ttu-id="5580b-107">Gemäß der Konvention ist ein Standardwert nicht konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="5580b-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="5580b-108">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="5580b-108">Data Annotations</span></span>

<span data-ttu-id="5580b-109">Sie können einen Standardwert, der mithilfe von Datenanmerkungen nicht festlegen.</span><span class="sxs-lookup"><span data-stu-id="5580b-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="5580b-110">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="5580b-110">Fluent API</span></span>

<span data-ttu-id="5580b-111">Sie können die Fluent-API verwenden, der Standardwert für eine Eigenschaft angeben.</span><span class="sxs-lookup"><span data-stu-id="5580b-111">You can use the Fluent API to specify the default value for a property.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultValue.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Rating)
            .HasDefaultValue(3);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public int Rating { get; set; }
}
```

<span data-ttu-id="5580b-112">Sie können auch eine SQL-Fragment angeben, die zum Berechnen des Standardwerts verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="5580b-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultValueSql.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Created)
            .HasDefaultValueSql("getdate()");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public DateTime Created { get; set; }
}
```