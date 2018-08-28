---
title: Einschließen und Ausschließen von Eigenschaften – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: 07b70e4517b67490e04a9ec9fa22b9b5d5217681
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998254"
---
# <a name="including--excluding-properties"></a><span data-ttu-id="70819-102">Einschließen und Ausschließen von Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="70819-102">Including & Excluding Properties</span></span>

<span data-ttu-id="70819-103">Einschließlich einer Eigenschaft im Modell bedeutet, dass EF Metadaten über diese Eigenschaft, und versucht, lesen und Schreiben von Werten aus bzw. nach der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="70819-103">Including a property in the model means that EF has metadata about that property and will attempt to read and write values from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="70819-104">Konventionen</span><span class="sxs-lookup"><span data-stu-id="70819-104">Conventions</span></span>

<span data-ttu-id="70819-105">Gemäß der Konvention werden die öffentliche Eigenschaften mit Getter und Setter im Modell enthalten sein.</span><span class="sxs-lookup"><span data-stu-id="70819-105">By convention, public properties with a getter and a setter will be included in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="70819-106">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="70819-106">Data Annotations</span></span>

<span data-ttu-id="70819-107">Sie können Datenanmerkungen verwenden, um eine Eigenschaft aus dem Modell auszuschließen.</span><span class="sxs-lookup"><span data-stu-id="70819-107">You can use Data Annotations to exclude a property from the model.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/IgnoreProperty.cs?highlight=6)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    [NotMapped]
    public DateTime LoadedFromDatabase { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="70819-108">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="70819-108">Fluent API</span></span>

<span data-ttu-id="70819-109">Sie können die Fluent-API verwenden, um eine Eigenschaft aus dem Modell auszuschließen.</span><span class="sxs-lookup"><span data-stu-id="70819-109">You can use the Fluent API to exclude a property from the model.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IgnoreProperty.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Ignore(b => b.LoadedFromDatabase);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public DateTime LoadedFromDatabase { get; set; }
}
```
