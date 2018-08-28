---
title: Sequenzen – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/relational/sequences
ms.openlocfilehash: eb9d9896966af0ad6b778047a1ed6af7358e8eb2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994515"
---
# <a name="sequences"></a><span data-ttu-id="1d32a-102">Sequenzen</span><span class="sxs-lookup"><span data-stu-id="1d32a-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="1d32a-103">Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="1d32a-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="1d32a-104">Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="1d32a-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="1d32a-105">Eine Sequenz generiert eine fortlaufende numerische Werte in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="1d32a-105">A sequence generates a sequential numeric values in the database.</span></span> <span data-ttu-id="1d32a-106">Sequenzen sind nicht mit einer bestimmten Tabelle verknüpft.</span><span class="sxs-lookup"><span data-stu-id="1d32a-106">Sequences are not associated with a specific table.</span></span>

## <a name="conventions"></a><span data-ttu-id="1d32a-107">Konventionen</span><span class="sxs-lookup"><span data-stu-id="1d32a-107">Conventions</span></span>

<span data-ttu-id="1d32a-108">Gemäß der Konvention werden Sequenzen nicht in dem Modell eingeführt.</span><span class="sxs-lookup"><span data-stu-id="1d32a-108">By convention, sequences are not introduced in to the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="1d32a-109">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="1d32a-109">Data Annotations</span></span>

<span data-ttu-id="1d32a-110">Sie können eine Sequenz, die mithilfe von Datenanmerkungen nicht konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="1d32a-110">You can not configure a sequence using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="1d32a-111">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="1d32a-111">Fluent API</span></span>

<span data-ttu-id="1d32a-112">Sie können die Fluent-API verwenden, um eine Sequenz im Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="1d32a-112">You can use the Fluent API to create a sequence in the model.</span></span>

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

<span data-ttu-id="1d32a-113">Sie können auch zusätzliche Aspekt der Sequenz, z. B. Schemas, Start-Wert, und erhöhen konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="1d32a-113">You can also configure additional aspect of the sequence, such as its schema, start value, and increment.</span></span>

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

<span data-ttu-id="1d32a-114">Nach der Einführung einer Sequenz ist, können Sie es verwenden, um Werte für Eigenschaften in Ihrem Modell zu generieren.</span><span class="sxs-lookup"><span data-stu-id="1d32a-114">Once a sequence is introduced, you can use it to generate values for properties in your model.</span></span> <span data-ttu-id="1d32a-115">Beispielsweise können Sie [Standardwerte](default-values.md) den nächsten Wert aus der Sequenz eingefügt.</span><span class="sxs-lookup"><span data-stu-id="1d32a-115">For example, you can use [Default Values](default-values.md) to insert the next value from the sequence.</span></span>

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
