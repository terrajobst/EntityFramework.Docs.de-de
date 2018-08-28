---
title: Erforderliche bzw. optionale Eigenschaften – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: b6716a5b03e1afc2933e317d606ef50f986c22c7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995496"
---
# <a name="required-and-optional-properties"></a>Erforderliche und optionale Eigenschaften

Eine Eigenschaft ist optional, wenn er gültig ist, damit enthalten ist als `null`. Wenn `null` ist kein gültiger Wert, der einer Eigenschaft zugewiesen werden soll, und es gilt eine erforderliche Eigenschaft.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention, eine Eigenschaft, dessen CLR-Typ Null darf, wird so konfiguriert, als optional (`string`, `int?`, `byte[]`usw..). Eigenschaften, deren CLR-Typ darf nicht Null enthalten, werden so konfiguriert, wie erforderlich (`int`, `decimal`, `bool`usw..).

> [!NOTE]  
> Eine Eigenschaft, dessen CLR-Typ nicht null enthalten kann, kann nicht als optional konfiguriert werden. Die Eigenschaft wird von Entity Framework benötigt immer berücksichtigt werden.

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
