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
# <a name="default-schema"></a><span data-ttu-id="2d960-102">Standardschema</span><span class="sxs-lookup"><span data-stu-id="2d960-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="2d960-103">Die Konfiguration in diesem Abschnitt ist im Allgemeinen gilt für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="2d960-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="2d960-104">Die Erweiterungsmethoden, die hier gezeigten werden verfügbar, wenn Sie einen relationale Datenbank-Anbieter installieren (aufgrund der freigegebenen *Microsoft.EntityFrameworkCore.Relational* Paket).</span><span class="sxs-lookup"><span data-stu-id="2d960-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="2d960-105">Das Standardschema ist das Datenbankschema, dem Objekte in erstellt wird, wenn ein Schema für dieses Objekt nicht explizit konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="2d960-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="2d960-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="2d960-106">Conventions</span></span>

<span data-ttu-id="2d960-107">Gemäß der Konvention wird die Datenbankanbieter am besten geeignete Standardschema auswählen.</span><span class="sxs-lookup"><span data-stu-id="2d960-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="2d960-108">Microsoft SQL Server wird verwendet, z. B. die `dbo` Schema und SQLite wird nicht verwenden, ein Schema (da in SQLite keine Schemas unterstützt werden).</span><span class="sxs-lookup"><span data-stu-id="2d960-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="2d960-109">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="2d960-109">Data Annotations</span></span>

<span data-ttu-id="2d960-110">Sie können das Standardschema mithilfe von Datenanmerkungen nicht festlegen.</span><span class="sxs-lookup"><span data-stu-id="2d960-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="2d960-111">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="2d960-111">Fluent API</span></span>

<span data-ttu-id="2d960-112">Sie können die Fluent-API verwenden, auch ein Standardschema angegeben.</span><span class="sxs-lookup"><span data-stu-id="2d960-112">You can use the Fluent API to specify a default schema.</span></span>

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
