---
title: Primärschlüssel - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: 916f3adbcd08cb1037c7fbf68e99630feb321a61
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998067"
---
# <a name="primary-keys"></a>Primärschlüssel

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken. Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).

Eine primary Key-Einschränkung wird für den Schlüssel jedes Entitätstyps, eingeführt.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention wird der Primärschlüssel in der Datenbank mit dem Namen werden `PK_<type name>`.

## <a name="data-annotations"></a>Datenanmerkungen

Keine relationalen Datenbank bestimmte Aspekte eines Primärschlüssels können mithilfe von Datenanmerkungen konfiguriert werden.

## <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, so konfigurieren Sie den Namen der primary Key-Einschränkung in der Datenbank.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/KeyName.cs?highlight=9)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasKey(b => b.BlogId)
            .HasName("PrimaryKey_BlogId");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
