---
title: Standardschema - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
ms.technology: entity-framework-core
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 26106deb2d4e35ecf33e97790a83f9af77991aed
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052750"
---
# <a name="default-schema"></a>Standardschema

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt ist im Allgemeinen gilt für relationale Datenbanken. Die Erweiterungsmethoden, die hier gezeigten werden verfügbar, wenn Sie einen relationale Datenbank-Anbieter installieren (aufgrund der freigegebenen *Microsoft.EntityFrameworkCore.Relational* Paket).

Das Standardschema ist das Datenbankschema, dem Objekte in erstellt wird, wenn ein Schema für dieses Objekt nicht explizit konfiguriert ist.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention wird die Datenbankanbieter am besten geeignete Standardschema auswählen. Microsoft SQL Server wird verwendet, z. B. die `dbo` Schema und SQLite wird nicht verwenden, ein Schema (da in SQLite keine Schemas unterstützt werden).

## <a name="data-annotations"></a>Datenanmerkungen

Sie können das Standardschema mithilfe von Datenanmerkungen nicht festlegen.

## <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, auch ein Standardschema angegeben.

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
