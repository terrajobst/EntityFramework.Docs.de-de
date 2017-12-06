---
title: "Schlüssel (primär) – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
ms.technology: entity-framework-core
uid: core/modeling/keys
ms.openlocfilehash: f3bf3c7f2a28e065b350fe000a5164406cd5ca08
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="keys-primary"></a><span data-ttu-id="ffc31-102">Schlüssel (primär)</span><span class="sxs-lookup"><span data-stu-id="ffc31-102">Keys (primary)</span></span>

<span data-ttu-id="ffc31-103">Ein Schlüssel dient als der primäre eindeutige Bezeichner für jede Entitätsinstanz.</span><span class="sxs-lookup"><span data-stu-id="ffc31-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="ffc31-104">Bei Verwendung eine relationale Datenbank entspricht dem das Konzept einer *Primärschlüssel*.</span><span class="sxs-lookup"><span data-stu-id="ffc31-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="ffc31-105">Sie können auch einen eindeutigen Bezeichner, der nicht den Primärschlüssel konfigurieren (finden Sie unter [Alternativschlüssel](alternate-keys.md) für Weitere Informationen).</span><span class="sxs-lookup"><span data-stu-id="ffc31-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

## <a name="conventions"></a><span data-ttu-id="ffc31-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="ffc31-106">Conventions</span></span>

<span data-ttu-id="ffc31-107">Gemäß der Konvention, eine Eigenschaft mit dem Namen `Id` oder `<type name>Id` wird als Schlüssel für eine Entität konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="ffc31-107">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyTypeNameId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string CarId { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="ffc31-108">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="ffc31-108">Data Annotations</span></span>

<span data-ttu-id="ffc31-109">Sie können Datenanmerkungen verwenden, so konfigurieren Sie eine einzelne Eigenschaft, um die Schlüssel einer Entität sein.</span><span class="sxs-lookup"><span data-stu-id="ffc31-109">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/KeySingle.cs?highlight=3,4)] -->
``` csharp
class Car
{
    [Key]
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="ffc31-110">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="ffc31-110">Fluent API</span></span>

<span data-ttu-id="ffc31-111">Sie können die Fluent-API verwenden, so konfigurieren Sie eine einzelne Eigenschaft, um die Schlüssel einer Entität sein.</span><span class="sxs-lookup"><span data-stu-id="ffc31-111">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/KeySingle.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasKey(c => c.LicensePlate);
    }
}

class Car
{
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<span data-ttu-id="ffc31-112">Die Fluent-API können auch um mehrere Eigenschaften für den Schlüssel einer Entität (als einen zusammengesetzten Schlüssel bezeichnet) werden zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="ffc31-112">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="ffc31-113">Zusammengesetzte Schlüssel können nur konfiguriert werden mithilfe der Fluent-API - Konventionen richtet einen zusammengesetzten Schlüssel nie und Datenanmerkungen Konfiguration kann nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="ffc31-113">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/KeyComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Car>()
            .HasKey(c => new { c.State, c.LicensePlate });
    }
}

class Car
{
    public string State { get; set; }
    public string LicensePlate { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```
