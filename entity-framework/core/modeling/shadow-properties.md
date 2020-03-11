---
title: Schatten Eigenschaften-EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 229cfd83f75b01dff9ac9ad30ee55c7cc727c19e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414710"
---
# <a name="shadow-properties"></a><span data-ttu-id="a4ccc-102">Schatteneigenschaften</span><span class="sxs-lookup"><span data-stu-id="a4ccc-102">Shadow Properties</span></span>

<span data-ttu-id="a4ccc-103">Schatten Eigenschaften sind Eigenschaften, die nicht in der .net-Entitäts Klasse definiert sind, aber für diesen Entitätstyp im EF Core Modell definiert sind.</span><span class="sxs-lookup"><span data-stu-id="a4ccc-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="a4ccc-104">Der Wert und der Status dieser Eigenschaften werden ausschließlich in der Änderungs Nachverfolgung beibehalten.</span><span class="sxs-lookup"><span data-stu-id="a4ccc-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span> <span data-ttu-id="a4ccc-105">Schatten Eigenschaften sind nützlich, wenn Daten in der Datenbank vorhanden sind, die für die zugeordneten Entitäts Typen nicht verfügbar gemacht werden sollen.</span><span class="sxs-lookup"><span data-stu-id="a4ccc-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span>

## <a name="foreign-key-shadow-properties"></a><span data-ttu-id="a4ccc-106">Fremdschlüssel Schatten-Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="a4ccc-106">Foreign key shadow properties</span></span>

<span data-ttu-id="a4ccc-107">Schatten Eigenschaften werden am häufigsten für Fremdschlüssel Eigenschaften verwendet, bei denen die Beziehung zwischen zwei Entitäten durch einen Fremdschlüssel Wert in der Datenbank dargestellt wird, die Beziehung jedoch für die Entitäts Typen mithilfe von Navigations Eigenschaften zwischen der Entität verwaltet wird. solche.</span><span class="sxs-lookup"><span data-stu-id="a4ccc-107">Shadow properties are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span> <span data-ttu-id="a4ccc-108">Gemäß der Konvention führt EF eine Schatten Eigenschaft ein, wenn eine Beziehung entdeckt wird, aber in der abhängigen Entitäts Klasse keine Fremdschlüssel Eigenschaft gefunden wird.</span><span class="sxs-lookup"><span data-stu-id="a4ccc-108">By convention, EF will introduce a shadow property when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span>

<span data-ttu-id="a4ccc-109">Die-Eigenschaft wird `<navigation property name><principal key property name>` benannt (die Navigation auf der abhängigen Entität, die auf die Prinzipal Entität verweist, wird für die Benennung verwendet).</span><span class="sxs-lookup"><span data-stu-id="a4ccc-109">The property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="a4ccc-110">Wenn der Name der Prinzipal Schlüsseleigenschaft den Namen der Navigations Eigenschaft enthält, wird der Name nur `<principal key property name>`.</span><span class="sxs-lookup"><span data-stu-id="a4ccc-110">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="a4ccc-111">Wenn für die abhängige Entität keine Navigations Eigenschaft vorhanden ist, wird der Prinzipal Typname an seiner Stelle verwendet.</span><span class="sxs-lookup"><span data-stu-id="a4ccc-111">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="a4ccc-112">Beispielsweise führt das folgende Codebeispiel dazu, dass eine `BlogId` Shadow-Eigenschaft in die `Post` Entität eingefügt wird:</span><span class="sxs-lookup"><span data-stu-id="a4ccc-112">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions&highlight=21-23)]

## <a name="configuring-shadow-properties"></a><span data-ttu-id="a4ccc-113">Konfigurieren von Schatten Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="a4ccc-113">Configuring shadow properties</span></span>

<span data-ttu-id="a4ccc-114">Sie können die fließende API verwenden, um Schatten Eigenschaften zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a4ccc-114">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="a4ccc-115">Nachdem Sie die Zeichen folgen Überladung `Property`aufgerufen haben, können Sie alle Konfigurations Aufrufe verketten, die für andere Eigenschaften gelten.</span><span class="sxs-lookup"><span data-stu-id="a4ccc-115">Once you have called the string overload of `Property`, you can chain any of the configuration calls you would for other properties.</span></span> <span data-ttu-id="a4ccc-116">Im folgenden Beispiel wird eine Schatten Eigenschaft erstellt, da `Blog` keine CLR-Eigenschaft mit dem Namen `LastUpdated`hat:</span><span class="sxs-lookup"><span data-stu-id="a4ccc-116">In the following sample, since `Blog` has no CLR property named `LastUpdated`, a shadow property is created:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]

<span data-ttu-id="a4ccc-117">Wenn der für die `Property` Methode angegebene Name mit dem Namen einer vorhandenen Eigenschaft übereinstimmt (eine Schatten Eigenschaft oder eine, die für die Entitäts Klasse definiert ist), konfiguriert der Code diese vorhandene Eigenschaft, anstatt eine neue Schatten Eigenschaft einzuführen.</span><span class="sxs-lookup"><span data-stu-id="a4ccc-117">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

## <a name="accessing-shadow-properties"></a><span data-ttu-id="a4ccc-118">Zugreifen auf Schatten Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="a4ccc-118">Accessing shadow properties</span></span>

<span data-ttu-id="a4ccc-119">Schatten Eigenschaftswerte können über die `ChangeTracker`-API abgerufen und geändert werden:</span><span class="sxs-lookup"><span data-stu-id="a4ccc-119">Shadow property values can be obtained and changed through the `ChangeTracker` API:</span></span>

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="a4ccc-120">Auf Schatten Eigenschaften kann in LINQ-Abfragen über die `EF.Property` statische Methode verwiesen werden:</span><span class="sxs-lookup"><span data-stu-id="a4ccc-120">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method:</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```
