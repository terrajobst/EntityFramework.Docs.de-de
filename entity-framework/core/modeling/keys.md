---
title: Schlüssel (primär) - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 9e6946100ebabc6ba57cb792b3672219098b1e21
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994020"
---
# <a name="keys-primary"></a><span data-ttu-id="a0e06-102">Schlüssel (primär)</span><span class="sxs-lookup"><span data-stu-id="a0e06-102">Keys (primary)</span></span>

<span data-ttu-id="a0e06-103">Ein Schlüssel dient als primärer Eindeutiger Bezeichner für jede Entitätsinstanz.</span><span class="sxs-lookup"><span data-stu-id="a0e06-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="a0e06-104">Bei Verwendung eine relationale Datenbank zugeordnet, das Konzept von einem *Primärschlüssel*.</span><span class="sxs-lookup"><span data-stu-id="a0e06-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="a0e06-105">Sie können auch einen eindeutigen Bezeichner, die nicht den Primärschlüssel konfigurieren (finden Sie unter [Alternativschlüssel](alternate-keys.md) Informationen).</span><span class="sxs-lookup"><span data-stu-id="a0e06-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

## <a name="conventions"></a><span data-ttu-id="a0e06-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="a0e06-106">Conventions</span></span>

<span data-ttu-id="a0e06-107">Gemäß der Konvention, eine Eigenschaft mit dem Namen `Id` oder `<type name>Id` wird als Schlüssel für eine Entität konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="a0e06-107">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="a0e06-108">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="a0e06-108">Data Annotations</span></span>

<span data-ttu-id="a0e06-109">Sie können Datenanmerkungen verwenden, so konfigurieren Sie eine einzelne Eigenschaft als Schlüssel einer Entität.</span><span class="sxs-lookup"><span data-stu-id="a0e06-109">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

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

## <a name="fluent-api"></a><span data-ttu-id="a0e06-110">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="a0e06-110">Fluent API</span></span>

<span data-ttu-id="a0e06-111">Sie können die Fluent-API verwenden, so konfigurieren Sie eine einzelne Eigenschaft als Schlüssel einer Entität.</span><span class="sxs-lookup"><span data-stu-id="a0e06-111">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

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

<span data-ttu-id="a0e06-112">Sie können auch die Fluent-API verwenden, so konfigurieren Sie mehrere Eigenschaften als Schlüssel einer Entität (als einen zusammengesetzten Schlüssel bezeichnet).</span><span class="sxs-lookup"><span data-stu-id="a0e06-112">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="a0e06-113">Zusammengesetzte Schlüssel können nur mithilfe der Fluent-API konfiguriert werden – Konventionen werden einen zusammengesetzten Schlüssel niemals eingerichtet und Datenanmerkungen Konfiguration kann nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="a0e06-113">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

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
