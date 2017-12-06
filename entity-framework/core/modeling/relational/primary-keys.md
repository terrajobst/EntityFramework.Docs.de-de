---
title: "Primärschlüssel - EF Core"
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
---
# <a name="primary-keys"></a><span data-ttu-id="65042-102">Primärschlüssel</span><span class="sxs-lookup"><span data-stu-id="65042-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="65042-103">Die Konfiguration in diesem Abschnitt ist im Allgemeinen gilt für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="65042-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="65042-104">Die Erweiterungsmethoden, die hier gezeigten werden verfügbar, wenn Sie einen relationale Datenbank-Anbieter installieren (aufgrund der freigegebenen *Microsoft.EntityFrameworkCore.Relational* Paket).</span><span class="sxs-lookup"><span data-stu-id="65042-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="65042-105">Eine primary Key-Einschränkung wird für den Schlüssel für jeden Entitätstyp eingeführt.</span><span class="sxs-lookup"><span data-stu-id="65042-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="65042-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="65042-106">Conventions</span></span>

<span data-ttu-id="65042-107">Wird der Primärschlüssel in der Datenbank gemäß der Konvention benannt werden `PK_<type name>`.</span><span class="sxs-lookup"><span data-stu-id="65042-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="65042-108">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="65042-108">Data Annotations</span></span>

<span data-ttu-id="65042-109">Keine relationalen Datenbank bestimmte Aspekte des Primärschlüssels, können mithilfe von Datenanmerkungen konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="65042-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="65042-110">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="65042-110">Fluent API</span></span>

<span data-ttu-id="65042-111">Sie können die Fluent-API verwenden, so konfigurieren Sie den Namen der primary Key-Einschränkung in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="65042-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

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
