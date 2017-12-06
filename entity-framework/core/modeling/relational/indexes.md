---
title: Indizes - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: 683b580bb155e0416f13c5d63e3280078fbcee21
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="indexes"></a>Indizes

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt ist im Allgemeinen gilt für relationale Datenbanken. Die Erweiterungsmethoden, die hier gezeigten werden verfügbar, wenn Sie einen relationale Datenbank-Anbieter installieren (aufgrund der freigegebenen *Microsoft.EntityFrameworkCore.Relational* Paket).

Ein Index in einer relationalen Datenbank ordnet dasselbe Konzept wie ein Index im Kern des Entity Framework.

## <a name="conventions"></a>Konventionen

Indizes werden gemäß der Konvention benannt `IX_<type name>_<property name>`. Bei zusammengesetzten Indizes `<property name>` wird eine Unterstrich getrennte Liste von Eigenschaftsnamen.

## <a name="data-annotations"></a>Datenanmerkungen

Indizes können nicht mithilfe von Datenanmerkungen konfiguriert werden.

## <a name="fluent-api"></a>Fluent-API

So konfigurieren Sie den Namen eines Indexes können Sie die Fluent-API verwenden.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/IndexName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasIndex(b => b.Url)
            .HasName("Index_Url");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
