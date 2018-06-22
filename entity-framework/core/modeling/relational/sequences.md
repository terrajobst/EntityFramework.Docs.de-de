---
title: Sequenzen - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
ms.technology: entity-framework-core
uid: core/modeling/relational/sequences
ms.openlocfilehash: 98a40aeecbec0fd9fb9cc108d6b5f98178dea403
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052710"
---
# <a name="sequences"></a><span data-ttu-id="28ed0-102">Sequenzen</span><span class="sxs-lookup"><span data-stu-id="28ed0-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="28ed0-103">Die Konfiguration in diesem Abschnitt ist im Allgemeinen gilt für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="28ed0-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="28ed0-104">Die Erweiterungsmethoden, die hier gezeigten werden verfügbar, wenn Sie einen relationale Datenbank-Anbieter installieren (aufgrund der freigegebenen *Microsoft.EntityFrameworkCore.Relational* Paket).</span><span class="sxs-lookup"><span data-stu-id="28ed0-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="28ed0-105">Eine Sequenz generiert eine fortlaufende numerische Werte in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="28ed0-105">A sequence generates a sequential numeric values in the database.</span></span> <span data-ttu-id="28ed0-106">Sequenzen werden nicht mit einer bestimmten Tabelle verknüpft.</span><span class="sxs-lookup"><span data-stu-id="28ed0-106">Sequences are not associated with a specific table.</span></span>

## <a name="conventions"></a><span data-ttu-id="28ed0-107">Konventionen</span><span class="sxs-lookup"><span data-stu-id="28ed0-107">Conventions</span></span>

<span data-ttu-id="28ed0-108">Gemäß der Konvention werden Sequenzen nicht in dem Modell eingeführt.</span><span class="sxs-lookup"><span data-stu-id="28ed0-108">By convention, sequences are not introduced in to the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="28ed0-109">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="28ed0-109">Data Annotations</span></span>

<span data-ttu-id="28ed0-110">Sie können eine Sequenz, die mithilfe von Datenanmerkungen nicht konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="28ed0-110">You can not configure a sequence using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="28ed0-111">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="28ed0-111">Fluent API</span></span>

<span data-ttu-id="28ed0-112">Die Fluent-API können Sie um eine Sequenz im Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="28ed0-112">You can use the Fluent API to create a sequence in the model.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/Sequence.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers");
    }
}

public class Order
{
    public int OrderId { get; set; }
    public int OrderNo { get; set; }
    public string Url { get; set; }
}
```

<span data-ttu-id="28ed0-113">Sie können auch zusätzliche Aspekte der Sequenz, z. B. Schemas, Startwert und Inkrement konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="28ed0-113">You can also configure additional aspect of the sequence, such as its schema, start value, and increment.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/SequenceConfigured.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers", schema: "shared")
            .StartsAt(1000)
            .IncrementsBy(5);
    }
}
```

<span data-ttu-id="28ed0-114">Sobald eine Sequenz eingeführt wird, können Sie es verwenden, um Werte für Eigenschaften im Modell zu generieren.</span><span class="sxs-lookup"><span data-stu-id="28ed0-114">Once a sequence is introduced, you can use it to generate values for properties in your model.</span></span> <span data-ttu-id="28ed0-115">Beispielsweise können Sie [Standardwerte](default-values.md) den nächsten Wert aus der Sequenz eingefügt.</span><span class="sxs-lookup"><span data-stu-id="28ed0-115">For example, you can use [Default Values](default-values.md) to insert the next value from the sequence.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/SequenceUsed.cs?highlight=11,12,13)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Order> Orders { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasSequence<int>("OrderNumbers", schema: "shared")
            .StartsAt(1000)
            .IncrementsBy(5);

        modelBuilder.Entity<Order>()
            .Property(o => o.OrderNo)
            .HasDefaultValueSql("NEXT VALUE FOR shared.OrderNumbers");
    }
}

public class Order
{
    public int OrderId { get; set; }
    public int OrderNo { get; set; }
    public string Url { get; set; }
}
```
