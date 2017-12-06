---
title: Vererbung - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
ms.technology: entity-framework-core
uid: core/modeling/inheritance
ms.openlocfilehash: f0394bc55dfbfea8277b1ddf898361165dd1fe51
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="inheritance"></a><span data-ttu-id="881a0-102">Vererbung</span><span class="sxs-lookup"><span data-stu-id="881a0-102">Inheritance</span></span>

<span data-ttu-id="881a0-103">Vererbung in der EF-Modell dient zum Steuern, wie Vererbung in Entitätsklassen in der Datenbank dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="881a0-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="881a0-104">Konventionen</span><span class="sxs-lookup"><span data-stu-id="881a0-104">Conventions</span></span>

<span data-ttu-id="881a0-105">Gemäß der Konvention ist es bis zu den Datenbankanbieter, um zu bestimmen, wie Vererbung in der Datenbank dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="881a0-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="881a0-106">Finden Sie unter [Vererbung (Relational Database)](relational/inheritance.md) für wie dies mit einer relationalen Datenbank-Anbieter behandelt wird.</span><span class="sxs-lookup"><span data-stu-id="881a0-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="881a0-107">EF richtet Vererbung nur, wenn mindestens zwei geerbte Typen explizit in das Modell enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="881a0-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="881a0-108">EF gesucht wird nicht für den Basistyp bzw. abgeleiteten Typen, die andernfalls nicht im Modell enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="881a0-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="881a0-109">Sie können Typen im Modell einschließen, indem verfügbar gemacht werden eine *DbSet<TEntity>*  für jeden Typ in der Vererbungshierarchie.</span><span class="sxs-lookup"><span data-stu-id="881a0-109">You can include types in the model by exposing a *DbSet<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="881a0-110">Wenn Sie verfügbar machen möchten, keine *DbSet<TEntity>*  für eine oder mehrere Entitäten in der Hierarchie, können Sie die Fluent-API verwenden, um sicherzustellen, dass sie im Modell enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="881a0-110">If you don't want to expose a *DbSet<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="881a0-111">Und wenn Sie auf die Konventionen basieren nicht können Sie angeben, dass den Basistyp explizit mit `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="881a0-111">And if you don't rely on conventions you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="881a0-112">Sie können `.HasBaseType((Type)null)` ein Entitätstyps aus der Hierarchie entfernen.</span><span class="sxs-lookup"><span data-stu-id="881a0-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="881a0-113">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="881a0-113">Data Annotations</span></span>

<span data-ttu-id="881a0-114">Sie können keine Datenanmerkungen verwenden, um Vererbung zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="881a0-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="881a0-115">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="881a0-115">Fluent API</span></span>

<span data-ttu-id="881a0-116">Die Fluent-API für die Vererbung, hängt von den verwendeten Anbieter ab.</span><span class="sxs-lookup"><span data-stu-id="881a0-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="881a0-117">Finden Sie unter [Vererbung (Relational Database)](relational/inheritance.md) für die Konfiguration, die Sie für einen Anbieter für die relationale Datenbank ausführen können.</span><span class="sxs-lookup"><span data-stu-id="881a0-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
