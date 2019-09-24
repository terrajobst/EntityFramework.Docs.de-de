---
title: Schatten Eigenschaften-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 5fdc4c50c295f73d0fa5eef3518adf4d3eb95599
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197702"
---
# <a name="shadow-properties"></a><span data-ttu-id="a022a-102">Schatteneigenschaften</span><span class="sxs-lookup"><span data-stu-id="a022a-102">Shadow Properties</span></span>

<span data-ttu-id="a022a-103">Schatten Eigenschaften sind Eigenschaften, die nicht in der .net-Entitäts Klasse definiert sind, aber für diesen Entitätstyp im EF Core Modell definiert sind.</span><span class="sxs-lookup"><span data-stu-id="a022a-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="a022a-104">Der Wert und der Status dieser Eigenschaften werden ausschließlich in der Änderungs Nachverfolgung beibehalten.</span><span class="sxs-lookup"><span data-stu-id="a022a-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="a022a-105">Schatten Eigenschaften sind nützlich, wenn Daten in der Datenbank vorhanden sind, die für die zugeordneten Entitäts Typen nicht verfügbar gemacht werden sollen.</span><span class="sxs-lookup"><span data-stu-id="a022a-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="a022a-106">Sie werden am häufigsten für Fremdschlüssel Eigenschaften verwendet, bei denen die Beziehung zwischen zwei Entitäten durch einen Fremdschlüssel Wert in der Datenbank dargestellt wird, die Beziehung jedoch für die Entitäts Typen mithilfe von Navigations Eigenschaften zwischen den Entitäts Typen verwaltet wird.</span><span class="sxs-lookup"><span data-stu-id="a022a-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="a022a-107">Schatten Eigenschaftswerte können über die `ChangeTracker` API abgerufen und geändert werden.</span><span class="sxs-lookup"><span data-stu-id="a022a-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="a022a-108">Auf Schatten Eigenschaften kann in LINQ-Abfragen über die `EF.Property` statische-Methode verwiesen werden.</span><span class="sxs-lookup"><span data-stu-id="a022a-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="a022a-109">Konventionen</span><span class="sxs-lookup"><span data-stu-id="a022a-109">Conventions</span></span>

<span data-ttu-id="a022a-110">Schatten Eigenschaften können nach der Konvention erstellt werden, wenn eine Beziehung gefunden wird, aber keine Fremdschlüssel Eigenschaft in der abhängigen Entitäts Klasse gefunden wurde.</span><span class="sxs-lookup"><span data-stu-id="a022a-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="a022a-111">In diesem Fall wird eine Schatten Fremdschlüssel-Eigenschaft eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a022a-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="a022a-112">Die Schatten-Fremdschlüssel Eigenschaft wird benannt `<navigation property name><principal key property name>` (die Navigation auf der abhängigen Entität, die auf die Prinzipal Entität verweist, wird für die Benennung verwendet).</span><span class="sxs-lookup"><span data-stu-id="a022a-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="a022a-113">Wenn der Name der Prinzipal Schlüsseleigenschaft den Namen der Navigations Eigenschaft enthält, ist der Name einfach `<principal key property name>`.</span><span class="sxs-lookup"><span data-stu-id="a022a-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="a022a-114">Wenn für die abhängige Entität keine Navigations Eigenschaft vorhanden ist, wird der Prinzipal Typname an seiner Stelle verwendet.</span><span class="sxs-lookup"><span data-stu-id="a022a-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="a022a-115">Beispielsweise führt das folgende Codebeispiel dazu, dass der `BlogId` `Post` Entität eine Schatten Eigenschaft hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="a022a-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/ShadowForeignKey.cs)] -->
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

## <a name="data-annotations"></a><span data-ttu-id="a022a-116">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="a022a-116">Data Annotations</span></span>

<span data-ttu-id="a022a-117">Schatten Eigenschaften können nicht mit Daten Anmerkungen erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="a022a-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="a022a-118">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="a022a-118">Fluent API</span></span>

<span data-ttu-id="a022a-119">Sie können die fließende API verwenden, um Schatten Eigenschaften zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a022a-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="a022a-120">Nachdem Sie die Zeichen folgen Überladung von `Property` aufgerufen haben, können Sie alle Konfigurations Aufrufe verketten, die Sie für andere Eigenschaften haben.</span><span class="sxs-lookup"><span data-stu-id="a022a-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="a022a-121">Wenn der für die `Property` Methode angegebene Name mit dem Namen einer vorhandenen Eigenschaft übereinstimmt (eine Schatten Eigenschaft oder eine, die für die Entitäts Klasse definiert ist), konfiguriert der Code diese vorhandene Eigenschaft, anstatt eine neue Schatten Eigenschaft einzuführen.</span><span class="sxs-lookup"><span data-stu-id="a022a-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/ShadowProperty.cs?highlight=7,8)] -->
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
