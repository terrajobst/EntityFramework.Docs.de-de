---
title: Primärschlüssel-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: bdb31964d717c64bddc28e7f1ce55b261e285a9b
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71196946"
---
# <a name="primary-keys"></a><span data-ttu-id="617e9-102">Primärschlüssel</span><span class="sxs-lookup"><span data-stu-id="617e9-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="617e9-103">Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="617e9-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="617e9-104">Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="617e9-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="617e9-105">Eine PRIMARY KEY-Einschränkung wird für den Schlüssel jedes Entitäts Typs eingeführt.</span><span class="sxs-lookup"><span data-stu-id="617e9-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="617e9-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="617e9-106">Conventions</span></span>

<span data-ttu-id="617e9-107">Gemäß der Konvention wird der Primärschlüssel in der Datenbank benannt `PK_<type name>`.</span><span class="sxs-lookup"><span data-stu-id="617e9-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="617e9-108">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="617e9-108">Data Annotations</span></span>

<span data-ttu-id="617e9-109">Keine relationalen datenbankspezifischen Aspekte eines Primärschlüssels können mithilfe von Daten Anmerkungen konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="617e9-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="617e9-110">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="617e9-110">Fluent API</span></span>

<span data-ttu-id="617e9-111">Sie können die fließende API verwenden, um den Namen der PRIMARY KEY-Einschränkung in der Datenbank zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="617e9-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/KeyName.cs?highlight=9)] -->
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
