---
title: Erforderlich/optionalen Eigenschaften - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
ms.technology: entity-framework-core
uid: core/modeling/required-optional
ms.openlocfilehash: 2af1d49e12ef980f81cb9c00556dee471673ccae
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="9581b-102">Erforderliche und optionale Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="9581b-102">Required and Optional Properties</span></span>

<span data-ttu-id="9581b-103">Eine Eigenschaft ist optional, wenn es gültig ist, damit enthalten ist als `null`.</span><span class="sxs-lookup"><span data-stu-id="9581b-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="9581b-104">Wenn `null` ist kein gültiger Wert, eine Eigenschaft zugewiesen werden soll, und klicken Sie dann dieses als betrachtet ist eine erforderliche Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="9581b-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="9581b-105">Konventionen</span><span class="sxs-lookup"><span data-stu-id="9581b-105">Conventions</span></span>

<span data-ttu-id="9581b-106">Gemäß der Konvention, eine Eigenschaft, deren CLR-Typ Null darf, konfiguriert werden soll als optional (`string`, `int?`, `byte[]`usw..).</span><span class="sxs-lookup"><span data-stu-id="9581b-106">By convention, a property whose CLR type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="9581b-107">Eigenschaften, deren CLR-Typ kann nicht Null enthalten, konfiguriert werden wie erforderlich (`int`, `decimal`, `bool`usw..).</span><span class="sxs-lookup"><span data-stu-id="9581b-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="9581b-108">Eine Eigenschaft, deren CLR-Typ nicht null enthalten kann, kann nicht als optional konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="9581b-108">A property whose CLR type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="9581b-109">Die Eigenschaft wird immer erforderlich, die für Entity Framework berücksichtigt werden.</span><span class="sxs-lookup"><span data-stu-id="9581b-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="9581b-110">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="9581b-110">Data Annotations</span></span>

<span data-ttu-id="9581b-111">Sie können Datenanmerkungen verwenden, um anzugeben, dass eine Eigenschaft erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="9581b-111">You can use Data Annotations to indicate that a property is required.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a><span data-ttu-id="9581b-112">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="9581b-112">Fluent API</span></span>

<span data-ttu-id="9581b-113">Sie können die Fluent-API verwenden, um anzugeben, dass eine Eigenschaft erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="9581b-113">You can use the Fluent API to indicate that a property is required.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=7,8,9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property(b => b.Url)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
