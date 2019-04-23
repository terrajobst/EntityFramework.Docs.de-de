---
title: 'Einschließen und Ausschließen von Typen: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: f533b24312af37634ce4957e43c39ce776bf0bf0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929796"
---
# <a name="including--excluding-types"></a>Einschließen und Ausschließen von Typen

Einschließlich eines Typs in das Modell bedeutet, die EF Metadaten zu, die geben, und versucht, lesen und Schreiben von Instanzen aus bzw. nach der Datenbank.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention, Typen, die in verfügbar gemacht werden `DbSet` Eigenschaften für den Kontext im Modell enthalten sind. Darüber hinaus Typen, die in erwähnt werden die `OnModelCreating` Methode sind ebenfalls enthalten. Schließlich sind alle Typen, die durch Untersuchen der Eigenschaften von entdeckten Typen rekursiv gefunden werden, ebenfalls im Modell enthalten.

**Im folgenden Codebeispiel werden z. B. alle drei Typen ermittelt:**

* `Blog` Da es in verfügbar gemacht wird eine `DbSet` Eigenschaft im Kontext

* `Post` Da es sich bei der Erkennung wird über die `Blog.Posts` Navigationseigenschaft

* `AuditEntry` Da es im genannt wird `OnModelCreating`

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/IncludedTypes.cs?highlight=3,7,16)] -->
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

Sie können Datenanmerkungen verwenden, um einen Typ aus dem Modell auszuschließen.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, um einen Typ aus dem Modell auszuschließen.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/IgnoreType.cs?highlight=12)]
