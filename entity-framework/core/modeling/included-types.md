---
title: Einschließen & Ausschließen von Typen EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: ca83b1c432bdf4853dba81e12ec4a739bc8218dc
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197378"
---
# <a name="including--excluding-types"></a>Einschließen und Ausschließen von Typen

Das Einschließen eines Typs in das Modell bedeutet, dass EF über Metadaten zu diesem Typ verfügt und versucht, Instanzen aus der Datenbank zu lesen und zu schreiben.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention sind Typen, die in `DbSet` Eigenschaften in ihrem Kontext verfügbar gemacht werden, in Ihrem Modell enthalten. Außerdem werden Typen, die in der `OnModelCreating` -Methode erwähnt werden, ebenfalls eingeschlossen. Schließlich sind alle Typen, die gefunden werden, indem die Navigations Eigenschaften von ermittelten Typen rekursiv untersucht werden, auch im Modell enthalten.

**Im folgenden Code werden z. b. alle drei Typen ermittelt:**

* `Blog`Da es in einer `DbSet` Eigenschaft im Kontext verfügbar gemacht wird.

* `Post`Da es über die `Blog.Posts` Navigations Eigenschaft erkannt wird.

* `AuditEntry`Da es in`OnModelCreating`

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/IncludedTypes.cs?highlight=3,7,16)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<AuditEntry>();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}

public class AuditEntry
{
    public int AuditEntryId { get; set; }
    public string Username { get; set; }
    public string Action { get; set; }
}
```

## <a name="data-annotations"></a>Datenanmerkungen

Mithilfe von Daten Anmerkungen können Sie einen Typ aus dem Modell ausschließen.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a>Fluent-API

Mit der fließend-API können Sie einen Typ aus dem Modell ausschließen.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
