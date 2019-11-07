---
title: Vererbung-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: abc1caa4d3839b7cdb52b316bcfc8f648b609b70
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655688"
---
# <a name="inheritance"></a><span data-ttu-id="52ab4-102">Vererbung</span><span class="sxs-lookup"><span data-stu-id="52ab4-102">Inheritance</span></span>

<span data-ttu-id="52ab4-103">Die Vererbung im EF-Modell wird verwendet, um zu steuern, wie die Vererbung in den Entitäts Klassen in der Datenbank dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="52ab4-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="52ab4-104">Konventionen</span><span class="sxs-lookup"><span data-stu-id="52ab4-104">Conventions</span></span>

<span data-ttu-id="52ab4-105">Gemäß der Konvention liegt es an dem Datenbankanbieter, zu bestimmen, wie die Vererbung in der Datenbank dargestellt werden soll.</span><span class="sxs-lookup"><span data-stu-id="52ab4-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="52ab4-106">Informationen zur Behandlung eines relationalen Datenbankanbieters finden Sie unter [Vererbung (relationale Datenbank)](relational/inheritance.md) .</span><span class="sxs-lookup"><span data-stu-id="52ab4-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="52ab4-107">EF wird nur dann die Vererbung einrichten, wenn mindestens zwei geerbte Typen explizit im Modell enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="52ab4-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="52ab4-108">EF sucht nicht nach Basis Typen oder abgeleiteten Typen, die ansonsten nicht im Modell enthalten waren.</span><span class="sxs-lookup"><span data-stu-id="52ab4-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="52ab4-109">Sie können Typen in das Modell einschließen, indem Sie für jeden Typ in der Vererbungs Hierarchie eine *dbset-\<TEntity->* verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="52ab4-109">You can include types in the model by exposing a *DbSet\<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="52ab4-110">Wenn Sie für eine oder mehrere Entitäten in der Hierarchie keinen *dbset-\<TEntity->* verfügbar machen möchten, können Sie die fließende API verwenden, um sicherzustellen, dass Sie im Modell enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="52ab4-110">If you don't want to expose a *DbSet\<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="52ab4-111">Wenn Sie sich nicht auf Konventionen verlassen, können Sie den Basistyp explizit mithilfe `HasBaseType`angeben.</span><span class="sxs-lookup"><span data-stu-id="52ab4-111">And if you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="52ab4-112">Sie können `.HasBaseType((Type)null)` verwenden, um einen Entitätstyp aus der Hierarchie zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="52ab4-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="52ab4-113">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="52ab4-113">Data Annotations</span></span>

<span data-ttu-id="52ab4-114">Zum Konfigurieren der Vererbung können keine Daten Anmerkungen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="52ab4-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="52ab4-115">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="52ab4-115">Fluent API</span></span>

<span data-ttu-id="52ab4-116">Die fließende API für die Vererbung hängt vom verwendeten Datenbankanbieter ab.</span><span class="sxs-lookup"><span data-stu-id="52ab4-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="52ab4-117">Die Konfiguration, die Sie für einen relationalen Datenbankanbieter ausführen können, finden Sie unter [Vererbung (relationale Datenbank)](relational/inheritance.md)</span><span class="sxs-lookup"><span data-stu-id="52ab4-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
