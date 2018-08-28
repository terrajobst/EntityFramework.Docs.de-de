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
# <a name="primary-keys"></a><span data-ttu-id="a0943-102">Primärschlüssel</span><span class="sxs-lookup"><span data-stu-id="a0943-102">Primary Keys</span></span>

> [!NOTE]  
> <span data-ttu-id="a0943-103">Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="a0943-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="a0943-104">Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="a0943-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="a0943-105">Eine primary Key-Einschränkung wird für den Schlüssel jedes Entitätstyps, eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0943-105">A primary key constraint is introduced for the key of each entity type.</span></span>

## <a name="conventions"></a><span data-ttu-id="a0943-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="a0943-106">Conventions</span></span>

<span data-ttu-id="a0943-107">Gemäß der Konvention wird der Primärschlüssel in der Datenbank mit dem Namen werden `PK_<type name>`.</span><span class="sxs-lookup"><span data-stu-id="a0943-107">By convention, the primary key in the database will be named `PK_<type name>`.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="a0943-108">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="a0943-108">Data Annotations</span></span>

<span data-ttu-id="a0943-109">Keine relationalen Datenbank bestimmte Aspekte eines Primärschlüssels können mithilfe von Datenanmerkungen konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="a0943-109">No relational database specific aspects of a primary key can be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="a0943-110">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="a0943-110">Fluent API</span></span>

<span data-ttu-id="a0943-111">Sie können die Fluent-API verwenden, so konfigurieren Sie den Namen der primary Key-Einschränkung in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="a0943-111">You can use the Fluent API to configure the name of the primary key constraint in the database.</span></span>

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
