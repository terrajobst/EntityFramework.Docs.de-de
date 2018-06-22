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
ms.locfileid: "26052850"
---
# <a name="required-and-optional-properties"></a>Erforderliche und optionale Eigenschaften

Eine Eigenschaft ist optional, wenn es gültig ist, damit enthalten ist als `null`. Wenn `null` ist kein gültiger Wert, eine Eigenschaft zugewiesen werden soll, und klicken Sie dann dieses als betrachtet ist eine erforderliche Eigenschaft.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention, eine Eigenschaft, deren CLR-Typ Null darf, konfiguriert werden soll als optional (`string`, `int?`, `byte[]`usw..). Eigenschaften, deren CLR-Typ kann nicht Null enthalten, konfiguriert werden wie erforderlich (`int`, `decimal`, `bool`usw..).

> [!NOTE]  
> Eine Eigenschaft, deren CLR-Typ nicht null enthalten kann, kann nicht als optional konfiguriert werden. Die Eigenschaft wird immer erforderlich, die für Entity Framework berücksichtigt werden.

## <a name="data-annotations"></a>Datenanmerkungen

Sie können Datenanmerkungen verwenden, um anzugeben, dass eine Eigenschaft erforderlich ist.

<!-- [!code-csharp[Main](samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=4)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    [Required]
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, um anzugeben, dass eine Eigenschaft erforderlich ist.

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
