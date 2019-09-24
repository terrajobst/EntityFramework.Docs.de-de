---
title: Standardwerte-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: 0d3613606f21a78e22cfe0ee752ea982a6a17f93
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196988"
---
# <a name="default-values"></a><span data-ttu-id="42904-102">Standardwerte</span><span class="sxs-lookup"><span data-stu-id="42904-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="42904-103">Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="42904-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="42904-104">Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="42904-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="42904-105">Der Standardwert einer Spalte ist der Wert, der eingefügt wird, wenn eine neue Zeile eingefügt wird, aber für die Spalte kein Wert angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="42904-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="42904-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="42904-106">Conventions</span></span>

<span data-ttu-id="42904-107">Gemäß der Konvention ist kein Standardwert konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="42904-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="42904-108">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="42904-108">Data Annotations</span></span>

<span data-ttu-id="42904-109">Mithilfe von Daten Anmerkungen können Sie keinen Standardwert festlegen.</span><span class="sxs-lookup"><span data-stu-id="42904-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="42904-110">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="42904-110">Fluent API</span></span>

<span data-ttu-id="42904-111">Sie können die fließende API verwenden, um den Standardwert für eine Eigenschaft anzugeben.</span><span class="sxs-lookup"><span data-stu-id="42904-111">You can use the Fluent API to specify the default value for a property.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/DefaultValue.cs?highlight=9)] -->
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

<span data-ttu-id="42904-112">Sie können auch ein SQL-Fragment angeben, das verwendet wird, um den Standardwert zu berechnen.</span><span class="sxs-lookup"><span data-stu-id="42904-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/DefaultValueSql.cs?highlight=9)] -->
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
