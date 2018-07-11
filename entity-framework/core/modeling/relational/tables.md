---
title: Der Tabellenzuordnung – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c807aa4c-7845-443d-b8d0-bfc9b42691a3
ms.technology: entity-framework-core
uid: core/modeling/relational/tables
ms.openlocfilehash: ef6080b5d543c2a68a680be2b9effc1c9d531030
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949008"
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
