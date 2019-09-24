---
title: Foreign Key-Einschränkungen-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: d7ed4466f4df9ec01267b048ba1bbcc6e8bbdad5
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197067"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="3a074-102">Fremdschlüsseleinschränkungen</span><span class="sxs-lookup"><span data-stu-id="3a074-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="3a074-103">Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="3a074-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="3a074-104">Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="3a074-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="3a074-105">Eine FOREIGN KEY-Einschränkung wird für jede Beziehung im Modell eingeführt.</span><span class="sxs-lookup"><span data-stu-id="3a074-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="3a074-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="3a074-106">Conventions</span></span>

<span data-ttu-id="3a074-107">Gemäß der Konvention werden Foreign Key-Einschränkungen `FK_<dependent type name>_<principal type name>_<foreign key property name>`benannt.</span><span class="sxs-lookup"><span data-stu-id="3a074-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="3a074-108">Für zusammengesetzte fremd `<foreign key property name>` Schlüssel wird zu einer durch Trennzeichen getrennten Liste von Fremdschlüssel Eigenschaftsnamen.</span><span class="sxs-lookup"><span data-stu-id="3a074-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="3a074-109">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="3a074-109">Data Annotations</span></span>

<span data-ttu-id="3a074-110">Namen von Foreign Key-Einschränkungen können nicht mithilfe von Daten Anmerkungen konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="3a074-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="3a074-111">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="3a074-111">Fluent API</span></span>

<span data-ttu-id="3a074-112">Sie können die fließende API verwenden, um den Namen der FOREIGN KEY-Einschränkung für eine Beziehung zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="3a074-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

<!-- [!code-csharp[Main](samples/core/relational/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?highlight=12)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .HasForeignKey(p => p.BlogId)
            .HasConstraintName("ForeignKey_Post_Blog");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```
