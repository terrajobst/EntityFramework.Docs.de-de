---
title: Eigenschaften der Schattenkopie - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
ms.technology: entity-framework-core
uid: core/modeling/shadow-properties
ms.openlocfilehash: 8c7f70789ddc6ebd24f9bfae931069834db593c2
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2017
---
# <a name="shadow-properties"></a><span data-ttu-id="75b49-102">Shadowing von Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="75b49-102">Shadow Properties</span></span>

<span data-ttu-id="75b49-103">Shadowing von Eigenschaften sind Eigenschaften, die nicht in Ihrer Entitätsklasse .NET definiert werden, aber für diesen Entitätstyp in der EF-Core-Modell definiert sind.</span><span class="sxs-lookup"><span data-stu-id="75b49-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="75b49-104">Der Wert und der Status dieser Eigenschaften wird ausschließlich in das System zur Änderungsnachverfolgung beibehalten.</span><span class="sxs-lookup"><span data-stu-id="75b49-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="75b49-105">Shadowing von Eigenschaften sind nützlich, wenn Daten vorhanden sind, in der Datenbank, die nicht auf die zugeordneten Entitätstypen verfügbar gemacht werden sollen.</span><span class="sxs-lookup"><span data-stu-id="75b49-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="75b49-106">Sie werden am häufigsten verwendet für foreign Key-Eigenschaften, in dem die Beziehung zwischen zwei Entitäten wird durch einen Fremdschlüsselwert in der Datenbank dargestellt, aber die Beziehung wird für Entitätstypen, die mithilfe von Navigationseigenschaften zwischen den Entitätstypen verwaltet.</span><span class="sxs-lookup"><span data-stu-id="75b49-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="75b49-107">Schatten Eigenschaftswerte durch geändert und abgerufen werden können die `ChangeTracker` API.</span><span class="sxs-lookup"><span data-stu-id="75b49-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="75b49-108">Shadowing von Eigenschaften in LINQ-Abfragen über verwiesen werden können die `EF.Property` statische Methode.</span><span class="sxs-lookup"><span data-stu-id="75b49-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="75b49-109">Konventionen</span><span class="sxs-lookup"><span data-stu-id="75b49-109">Conventions</span></span>

<span data-ttu-id="75b49-110">Shadowing von Eigenschaften können gemäß der Konvention erstellt werden, wenn eine Beziehung ermittelt werden kann, aber keine foreign Key-Eigenschaft in der Klasse der abhängigen Entität gefunden wird.</span><span class="sxs-lookup"><span data-stu-id="75b49-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="75b49-111">In diesem Fall wird eine Schattenkopie Fremdschlüsseleigenschaft eingeführt werden.</span><span class="sxs-lookup"><span data-stu-id="75b49-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="75b49-112">Den Namen der Schatten Fremdschlüsseleigenschaft `<navigation property name><principal key property name>` (die Navigation auf der abhängigen Entität, die auf die Dienstprinzipalnamen Entität verweist, wird für die Benennung verwendet).</span><span class="sxs-lookup"><span data-stu-id="75b49-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="75b49-113">Wenn der Prinzipal Schlüsseleigenschaft Name umfasst den Namen der Navigationseigenschaft, die Namen werden nur `<principal key property name>`.</span><span class="sxs-lookup"><span data-stu-id="75b49-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="75b49-114">Wenn keine Navigationseigenschaft für die abhängige Entität vorhanden ist, wird der Name der Prinzipaltyp an seiner Stelle verwendet.</span><span class="sxs-lookup"><span data-stu-id="75b49-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="75b49-115">Beispielsweise führt das folgende Codelisting eine `BlogId` Shadow-Eigenschaft wird eine Einführung in die `Post` Entität.</span><span class="sxs-lookup"><span data-stu-id="75b49-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/ShadowForeignKey.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
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

    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="75b49-116">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="75b49-116">Data Annotations</span></span>

<span data-ttu-id="75b49-117">Shadowing von Eigenschaften können nicht mit datenanmerkungen erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="75b49-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="75b49-118">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="75b49-118">Fluent API</span></span>

<span data-ttu-id="75b49-119">Sie können die Fluent-API verwenden, so konfigurieren Sie Shadowing von Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="75b49-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="75b49-120">Nachdem Sie die zeichenfolgenüberladung aufgerufen haben `Property` können Sie alle von der Konfiguration aufrufen möchten, für die anderen Eigenschaften verketten.</span><span class="sxs-lookup"><span data-stu-id="75b49-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="75b49-121">Wenn der Name, um angegeben die `Property` Methoden entsprechen den Namen einer vorhandenen Eigenschaft (eine Schattenkopie-Eigenschaft oder eine Entitätsklasse definiert) und dann im Code konfigurieren, vorhandene Eigenschaft anstatt eine neue Shadow-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="75b49-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/ShadowProperty.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property<DateTime>("LastUpdated");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
