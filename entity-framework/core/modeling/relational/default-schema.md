---
title: Standard Schema-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: ae903ed7200859430aecc55073651236759bc6ce
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197132"
---
# <a name="default-schema"></a>Standardschema

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken. Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).

Das Standardschema ist das Datenbankschema, in dem Objekte erstellt werden, wenn ein Schema nicht explizit für dieses Objekt konfiguriert ist.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention wählt der Datenbankanbieter das geeignetste Standardschema aus. Beispielsweise wird Microsoft SQL Server das `dbo` Schema verwenden, und SQLite verwendet kein Schema (da Schemas in SQLite nicht unterstützt werden).

## <a name="data-annotations"></a>Datenanmerkungen

Sie können das Standardschema nicht mithilfe von Daten Anmerkungen festlegen.

## <a name="fluent-api"></a>Fluent-API

Mit der fließend-API können Sie ein Standardschema angeben.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/DefaultSchema.cs?highlight=7)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.HasDefaultSchema("blogging");
    }
}
```
