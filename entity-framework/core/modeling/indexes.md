---
title: Indizes – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: 87fe893243377e3ab83d419ae9bedf813ca50c3f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995479"
---
# <a name="indexes"></a>Indizes

Indizes sind ein gängiges Konzept für viele Datenspeicher. Obwohl deren Implementierung im Datenspeicher variieren kann, müssen sie dienen zum Suchen auf Grundlage einer Spalte (oder eine Gruppe von Spalten) stärker effizient.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention ist ein Index erstellt, in jeder Eigenschaft (oder einen Satz von Eigenschaften) an, die als Fremdschlüssel verwendet werden.

## <a name="data-annotations"></a>Datenanmerkungen

Indizes können nicht mithilfe von datenanmerkungen erstellt werden.

## <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, um einen Index für eine einzelne Eigenschaft angeben. Standardmäßig sind die Indizes nicht eindeutig.

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

Sie können auch angeben, dass ein Index muss eindeutig sein, was bedeutet, dass keine zwei Entitäten, die gleichen Werte für die angegebenen Eigenschaften verfügen können.

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
> Es gibt nur einen Index pro Satz von Eigenschaften. Wenn Sie die Fluent-API verwenden, so konfigurieren Sie einen Index für eine Gruppe von Eigenschaften, die bereits einen Index definiert wird, entweder durch Konventionen oder vorherigen Konfiguration werden dann Sie die Definition des Index ändern. Dies ist nützlich, wenn Sie möchten, um einen Index zu konfigurieren, der gemäß der Konvention erstellt wurde.
