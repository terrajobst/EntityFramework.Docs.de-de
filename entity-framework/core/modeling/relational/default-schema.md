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
# <a name="default-schema"></a><span data-ttu-id="9a61a-102">Standardschema</span><span class="sxs-lookup"><span data-stu-id="9a61a-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="9a61a-103">Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="9a61a-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="9a61a-104">Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="9a61a-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="9a61a-105">Das Standardschema ist das Datenbankschema, in dem Objekte erstellt werden, wenn ein Schema nicht explizit für dieses Objekt konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="9a61a-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="9a61a-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="9a61a-106">Conventions</span></span>

<span data-ttu-id="9a61a-107">Gemäß der Konvention wählt der Datenbankanbieter das geeignetste Standardschema aus.</span><span class="sxs-lookup"><span data-stu-id="9a61a-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="9a61a-108">Beispielsweise wird Microsoft SQL Server das `dbo` Schema verwenden, und SQLite verwendet kein Schema (da Schemas in SQLite nicht unterstützt werden).</span><span class="sxs-lookup"><span data-stu-id="9a61a-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="9a61a-109">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="9a61a-109">Data Annotations</span></span>

<span data-ttu-id="9a61a-110">Sie können das Standardschema nicht mithilfe von Daten Anmerkungen festlegen.</span><span class="sxs-lookup"><span data-stu-id="9a61a-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="9a61a-111">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="9a61a-111">Fluent API</span></span>

<span data-ttu-id="9a61a-112">Mit der fließend-API können Sie ein Standardschema angeben.</span><span class="sxs-lookup"><span data-stu-id="9a61a-112">You can use the Fluent API to specify a default schema.</span></span>

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
