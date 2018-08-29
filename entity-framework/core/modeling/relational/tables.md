---
title: Der Tabellenzuordnung – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
uid: core/modeling/relational/tables
ms.openlocfilehash: 32c5e3cc0e498005ce8e6be1f1ee7e8ddf9b510d
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994136"
---
# <a name="table-mapping"></a>Tabellenzuordnung

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken. Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).

Zuordnung zu einer Tabelle identifiziert die Tabellendaten aus abgefragt und in der Datenbank gespeichert werden sollten.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention, jede Entität wird so eingerichtet werden in eine Tabelle mit dem gleichen Namen wie ordnen die `DbSet<TEntity>` -Eigenschaft, die die Entität für den abgeleiteten Kontext verfügbar macht. Wenn kein `DbSet<TEntity>` enthalten ist für die angegebene Entität, den Namen der Klasse verwendet wird.

## <a name="data-annotations"></a>Datenanmerkungen

Sie können Datenanmerkungen verwenden, so konfigurieren Sie die Tabelle, der die ein Typ zugeordnet ist.

``` csharp
using System.ComponentModel.DataAnnotations.Schema;
```
``` csharp
[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

Sie können auch ein Schema angeben, zu der die Tabelle gehört.

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, so konfigurieren Sie die Tabelle, der die ein Typ zugeordnet ist.

``` csharp
using Microsoft.EntityFrameworkCore;
```
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .ToTable("blogs");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

Sie können auch ein Schema angeben, zu der die Tabelle gehört.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/TableAndSchema.cs?highlight=2)] -->
``` csharp
        modelBuilder.Entity<Blog>()
            .ToTable("blogs", schema: "blogging");
```
