---
title: Shadowing von Eigenschaften – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 4029539f3642f539a427f5901577d4df96c00f30
ms.sourcegitcommit: 119058fefd7f35952048f783ada68be9aa612256
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749703"
---
# <a name="shadow-properties"></a><span data-ttu-id="8dad0-102">Shadowing von Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="8dad0-102">Shadow Properties</span></span>

<span data-ttu-id="8dad0-103">Shadowing von Eigenschaften sind Eigenschaften, die nicht in der Entitätsklasse .NET definiert werden, aber für den betreffenden Entitätstyp in EF Core-Modell definiert sind.</span><span class="sxs-lookup"><span data-stu-id="8dad0-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="8dad0-104">Der Wert und der Status dieser Eigenschaften wird ausschließlich in die Änderungsnachverfolgung verwaltet.</span><span class="sxs-lookup"><span data-stu-id="8dad0-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="8dad0-105">Shadowing von Eigenschaften sind nützlich, wenn Daten vorhanden sind, in der Datenbank, die nicht für die zugeordnete Entitätstypen verfügbar gemacht werden sollen.</span><span class="sxs-lookup"><span data-stu-id="8dad0-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="8dad0-106">Sie werden am häufigsten verwendet für Fremdschlüsseleigenschaften, in dem die Beziehung zwischen zwei Entitäten wird durch einen Fremdschlüsselwert in der Datenbank dargestellt, aber die Beziehung wird für die Entitätstypen, die mithilfe von Navigationseigenschaften zwischen den Entitätstypen verwaltet.</span><span class="sxs-lookup"><span data-stu-id="8dad0-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="8dad0-107">Volumeschattenkopie-Eigenschaftswerte durch geändert und abgerufen werden können die `ChangeTracker` API.</span><span class="sxs-lookup"><span data-stu-id="8dad0-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="8dad0-108">Shadowing von Eigenschaften in LINQ-Abfragen über verwiesen werden können die `EF.Property` statische Methode.</span><span class="sxs-lookup"><span data-stu-id="8dad0-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="8dad0-109">Konventionen</span><span class="sxs-lookup"><span data-stu-id="8dad0-109">Conventions</span></span>

<span data-ttu-id="8dad0-110">Shadowing von Eigenschaften können gemäß der Konvention erstellt werden, wenn eine Beziehung wird ermittelt, aber keine foreign Key-Eigenschaft in der Klasse der abhängigen Entität gefunden wird.</span><span class="sxs-lookup"><span data-stu-id="8dad0-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="8dad0-111">In diesem Fall wird eine Fremdschlüsseleigenschaft Volumeschattenkopie eingeführt.</span><span class="sxs-lookup"><span data-stu-id="8dad0-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="8dad0-112">Den Namen der Schatten Fremdschlüsseleigenschaft `<navigation property name><principal key property name>` (im Navigationsbereich auf die abhängige Entität, die auf die prinzipalentität verweist, wird für die Benennung verwendet).</span><span class="sxs-lookup"><span data-stu-id="8dad0-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="8dad0-113">Wenn der Name des dienstprinzipals Schlüsseleigenschaft den Namen der Navigationseigenschaft enthält, dann ist der Name nur `<principal key property name>`.</span><span class="sxs-lookup"><span data-stu-id="8dad0-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="8dad0-114">Wenn auf die abhängige Entität keine Navigationseigenschaft vorhanden ist, wird der Name des dienstprinzipals an seiner Stelle verwendet.</span><span class="sxs-lookup"><span data-stu-id="8dad0-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="8dad0-115">Das folgende Codebeispiel führt beispielsweise zu einem `BlogId` Shadow-Eigenschaft wird eingeführt, um die `Post` Entität.</span><span class="sxs-lookup"><span data-stu-id="8dad0-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="8dad0-116">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="8dad0-116">Data Annotations</span></span>

<span data-ttu-id="8dad0-117">Shadowing von Eigenschaften können mit datenanmerkungen nicht erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="8dad0-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="8dad0-118">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="8dad0-118">Fluent API</span></span>

<span data-ttu-id="8dad0-119">Sie können die Fluent-API verwenden, zum Shadowing von Eigenschaften zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="8dad0-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="8dad0-120">Nachdem Sie die zeichenfolgenüberladung von aufgerufen haben `Property` können Sie verketten, eine von der Konfiguration aufrufen möchten, für die anderen Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="8dad0-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="8dad0-121">Wenn der Name, um angegeben die `Property` Methode entspricht der Name einer vorhandenen Eigenschaft (eine schatteneigenschaft oder eine Entitätsklasse definiert), und dann der Code, vorhandene Eigenschaft anstatt eine neue Eigenschaft für die Schattenkopien konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="8dad0-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

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
