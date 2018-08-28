---
title: Vererbung - ' Entity Framework Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: c5fa9d13dec8cfc3e1cac69e471f509cbbb9e4c5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995895"
---
# <a name="inheritance"></a><span data-ttu-id="c4f1c-102">Vererbung</span><span class="sxs-lookup"><span data-stu-id="c4f1c-102">Inheritance</span></span>

<span data-ttu-id="c4f1c-103">Vererbung in das EF-Modell dient zum Steuern, wie die Vererbung in Entitätsklassen in der Datenbank dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="c4f1c-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="c4f1c-104">Konventionen</span><span class="sxs-lookup"><span data-stu-id="c4f1c-104">Conventions</span></span>

<span data-ttu-id="c4f1c-105">Gemäß der Konvention ist es bis zu der Datenbankanbieter, um zu bestimmen, wie Vererbung in der Datenbank dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="c4f1c-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="c4f1c-106">Finden Sie unter [Vererbung (relationale Datenbank)](relational/inheritance.md) für wie dies mit einer relationalen Datenbank-Anbieter behandelt wird.</span><span class="sxs-lookup"><span data-stu-id="c4f1c-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="c4f1c-107">EF wird Vererbung nur eingerichtet, wenn mindestens zwei geerbte Typen explizit in das Modell enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="c4f1c-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="c4f1c-108">EF wird für den Basistyp bzw. abgeleiteten Typen, die andernfalls nicht im Modell enthalten waren, nicht überprüft.</span><span class="sxs-lookup"><span data-stu-id="c4f1c-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="c4f1c-109">Sie können Typen im Modell einschließen, indem verfügbar machen eine *"DbSet"<TEntity>*  für jeden Typ in der Vererbungshierarchie.</span><span class="sxs-lookup"><span data-stu-id="c4f1c-109">You can include types in the model by exposing a *DbSet<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="c4f1c-110">Wenn Sie verfügbar machen möchten, keine *"DbSet"<TEntity>*  für eine oder mehrere Entitäten in der Hierarchie, können Sie die Fluent-API verwenden, um sicherzustellen, dass sie die im Modell enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="c4f1c-110">If you don't want to expose a *DbSet<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="c4f1c-111">Und wenn Sie auf Konventionen basieren nicht können Sie angeben, dass den Basistyp ausdrücklich mit `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="c4f1c-111">And if you don't rely on conventions you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="c4f1c-112">Sie können `.HasBaseType((Type)null)` auf einen Entitätstyp aus der Hierarchie zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="c4f1c-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="c4f1c-113">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="c4f1c-113">Data Annotations</span></span>

<span data-ttu-id="c4f1c-114">Sie können nicht von Datenanmerkungen verwenden, die Vererbung konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="c4f1c-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="c4f1c-115">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="c4f1c-115">Fluent API</span></span>

<span data-ttu-id="c4f1c-116">Die Fluent-API für die Vererbung, hängt von den verwendeten Datenbankanbieter ab.</span><span class="sxs-lookup"><span data-stu-id="c4f1c-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="c4f1c-117">Finden Sie unter [Vererbung (relationale Datenbank)](relational/inheritance.md) für die Konfiguration für einen relationalen Datenbankanbieter durchführen können.</span><span class="sxs-lookup"><span data-stu-id="c4f1c-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
