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
# <a name="default-schema"></a><span data-ttu-id="9e49c-102">Standardschema</span><span class="sxs-lookup"><span data-stu-id="9e49c-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="9e49c-103">Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="9e49c-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="9e49c-104">Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="9e49c-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="9e49c-105">Das Standardschema ist das Datenbankschema, dem Objekte in erstellt wird, wenn ein Schema für dieses Objekt nicht explizit konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="9e49c-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="9e49c-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="9e49c-106">Conventions</span></span>

<span data-ttu-id="9e49c-107">Gemäß der Konvention wird der Datenbankanbieter das am besten geeignete Standardschema wählen.</span><span class="sxs-lookup"><span data-stu-id="9e49c-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="9e49c-108">Microsoft SQL Server verwendet z. B. die `dbo` Schema und SQLite wird nicht verwenden, ein Schema (da in SQLite keine Schemas unterstützt werden).</span><span class="sxs-lookup"><span data-stu-id="9e49c-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="9e49c-109">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="9e49c-109">Data Annotations</span></span>

<span data-ttu-id="9e49c-110">Sie können das Standardschema mit Datenanmerkungen nicht festlegen.</span><span class="sxs-lookup"><span data-stu-id="9e49c-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="9e49c-111">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="9e49c-111">Fluent API</span></span>

<span data-ttu-id="9e49c-112">Sie können die Fluent-API verwenden, um ein Standardschema angegeben.</span><span class="sxs-lookup"><span data-stu-id="9e49c-112">You can use the Fluent API to specify a default schema.</span></span>

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
