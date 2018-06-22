---
title: Primärschlüssel - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
ms.technology: entity-framework-core
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: fcb1871149c0f20a2576864028b4171904de1982
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052720"
---
# <a name="primary-keys"></a>Primärschlüssel

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt ist im Allgemeinen gilt für relationale Datenbanken. Die Erweiterungsmethoden, die hier gezeigten werden verfügbar, wenn Sie einen relationale Datenbank-Anbieter installieren (aufgrund der freigegebenen *Microsoft.EntityFrameworkCore.Relational* Paket).

Eine primary Key-Einschränkung wird für den Schlüssel für jeden Entitätstyp eingeführt.

## <a name="conventions"></a>Konventionen

Wird der Primärschlüssel in der Datenbank gemäß der Konvention benannt werden `PK_<type name>`.

## <a name="data-annotations"></a>Datenanmerkungen

Keine relationalen Datenbank bestimmte Aspekte des Primärschlüssels, können mithilfe von Datenanmerkungen konfiguriert werden.

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
