---
title: Tabellen Zuordnung-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
uid: core/modeling/relational/tables
ms.openlocfilehash: 474c49aca4c65cd5d58b184b1f3c2d30e7abff84
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656107"
---
# <a name="table-mapping"></a>Tabellenzuordnung

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken. Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).

Die Tabellen Zuordnung identifiziert, welche Tabellendaten in der Datenbank abgefragt und in der Datenbank gespeichert werden sollen.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention ist jede Entität so eingerichtet, dass sie einer Tabelle mit dem gleichen Namen wie die `DbSet<TEntity>`-Eigenschaft zugeordnet ist, die die Entität für den abgeleiteten Kontext verfügbar macht. Wenn keine `DbSet<TEntity>` für die angegebene Entität enthalten ist, wird der Klassenname verwendet.

## <a name="data-annotations"></a>Datenanmerkungen

Sie können Daten Anmerkungen verwenden, um die Tabelle zu konfigurieren, der ein Typ zugeordnet ist.

``` csharp
using System.ComponentModel.DataAnnotations.Schema;

[Table("blogs")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

Sie können auch ein Schema angeben, zu dem die Tabelle gehört.

``` csharp
[Table("blogs", Schema = "blogging")]
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```

## <a name="fluent-api"></a>Fluent-API

Sie können die fließende API verwenden, um die Tabelle zu konfigurieren, der ein Typ zugeordnet ist.

``` csharp
using Microsoft.EntityFrameworkCore;

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

Sie können auch ein Schema angeben, zu dem die Tabelle gehört.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/TableAndSchema.cs?name=Table&highlight=2)]
