---
title: Standardschema – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 800551bbadd0a9e8b5eb7070a8ccf6ed2407e3d2
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995365"
---
# <a name="default-schema"></a>Standardschema

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken. Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).

Das Standardschema ist das Datenbankschema, dem Objekte in erstellt wird, wenn ein Schema für dieses Objekt nicht explizit konfiguriert ist.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention wird der Datenbankanbieter das am besten geeignete Standardschema wählen. Microsoft SQL Server verwendet z. B. die `dbo` Schema und SQLite wird nicht verwenden, ein Schema (da in SQLite keine Schemas unterstützt werden).

## <a name="data-annotations"></a>Datenanmerkungen

Sie können das Standardschema mit Datenanmerkungen nicht festlegen.

## <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, um ein Standardschema angegeben.

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Samples/Relational/DefaultSchema.cs?highlight=7)] -->
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
