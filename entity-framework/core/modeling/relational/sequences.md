---
title: Sequenzen-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/relational/sequences
ms.openlocfilehash: ce02b9840e58102a60c1d8eacf6810365104d7d7
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196908"
---
# <a name="sequences"></a><span data-ttu-id="c7383-102">Sequenzen</span><span class="sxs-lookup"><span data-stu-id="c7383-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="c7383-103">Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="c7383-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="c7383-104">Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="c7383-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="c7383-105">Eine Sequenz generiert einen sequenziellen numerischen Wert in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="c7383-105">A sequence generates a sequential numeric values in the database.</span></span> <span data-ttu-id="c7383-106">Sequenzen sind einer bestimmten Tabelle nicht zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="c7383-106">Sequences are not associated with a specific table.</span></span>

## <a name="conventions"></a><span data-ttu-id="c7383-107">Konventionen</span><span class="sxs-lookup"><span data-stu-id="c7383-107">Conventions</span></span>

<span data-ttu-id="c7383-108">Gemäß der Konvention werden Sequenzen nicht in das Modell eingeführt.</span><span class="sxs-lookup"><span data-stu-id="c7383-108">By convention, sequences are not introduced in to the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="c7383-109">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="c7383-109">Data Annotations</span></span>

<span data-ttu-id="c7383-110">Es ist nicht möglich, eine Sequenz mithilfe von Daten Anmerkungen zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="c7383-110">You can not configure a sequence using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="c7383-111">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="c7383-111">Fluent API</span></span>

<span data-ttu-id="c7383-112">Sie können die fließende API verwenden, um eine Sequenz im Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="c7383-112">You can use the Fluent API to create a sequence in the model.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/Sequence.cs?highlight=7)] -->
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

<span data-ttu-id="c7383-113">Sie können auch zusätzlichen Aspekt der Sequenz konfigurieren, z. b. das Schema, den Startwert und das Inkrement.</span><span class="sxs-lookup"><span data-stu-id="c7383-113">You can also configure additional aspect of the sequence, such as its schema, start value, and increment.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/SequenceConfigured.cs?highlight=7,8,9)] -->
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

<span data-ttu-id="c7383-114">Nachdem eine Sequenz eingeführt wurde, können Sie Sie verwenden, um Werte für Eigenschaften in Ihrem Modell zu generieren.</span><span class="sxs-lookup"><span data-stu-id="c7383-114">Once a sequence is introduced, you can use it to generate values for properties in your model.</span></span> <span data-ttu-id="c7383-115">Beispielsweise können Sie die [Standardwerte](default-values.md) verwenden, um den nächsten Wert aus der Sequenz einzufügen.</span><span class="sxs-lookup"><span data-stu-id="c7383-115">For example, you can use [Default Values](default-values.md) to insert the next value from the sequence.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/SequenceUsed.cs?highlight=11,12,13)] -->
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
