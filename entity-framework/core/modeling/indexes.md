---
title: Indizes - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
ms.technology: entity-framework-core
uid: core/modeling/indexes
ms.openlocfilehash: f57b545d53613cec6887734bf434958ee8fff4d8
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="indexes"></a>Indizes

Indizes sind ein Konzept gemein über viele Datenspeichern. Während ihre Implementierung in den Datenspeicher variieren kann, werden sie verwendet, um Suchvorgänge basierend auf einer Spalte (oder eine Gruppe von Spalten) machen effizient.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention ist ein Index erstellt, in jeder Eigenschaft (oder einen Satz von Eigenschaften) an, die als Fremdschlüssel verwendet werden.

## <a name="data-annotations"></a>Datenanmerkungen

Indizes können nicht mithilfe von datenanmerkungen erstellt werden.

## <a name="fluent-api"></a>Fluent-API

Die Fluent-API können Sie einen Index für eine einzelne Eigenschaft angeben. Standardmäßig werden Indizes nicht eindeutig.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Index.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

Sie können auch angeben, dass ein Index eindeutig ist, was bedeutet, dass keine zwei Entitäten die gleichen Werte für den angegebenen Eigenschaft(en) enthalten kann.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexUnique.cs?highlight=3)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .IsUnique();
```

Sie können auch einen Index über mehr als eine Spalte angeben.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/IndexComposite.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Person> People { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .HasIndex(p => new { p.FirstName, p.LastName });
    }
}

public class Person
{
    public int PersonId { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
}
```

> [!TIP]  
> Es ist nur eine Index pro Satz von Eigenschaften. Wenn Sie die Fluent-API verwenden, so konfigurieren Sie einen Index für eine Gruppe von Eigenschaften, die bereits einen Index definiert, indem entweder Konvention oder vorherige Konfiguration werden dann Sie die Definition dieses Indexes ändern. Dies ist nützlich, wenn Sie möchten, um einen Index zu konfigurieren, der gemäß der Konvention erstellt wurde.
