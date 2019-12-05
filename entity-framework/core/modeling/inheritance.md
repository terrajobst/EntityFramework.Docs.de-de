---
title: Vererbung-EF Core
description: Konfigurieren der Vererbung von Entitäts Typen mit Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 10/27/2016
uid: core/modeling/inheritance
ms.openlocfilehash: 4d43a432174c92ab7f3f9d78a234aefb0a4a17e8
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824675"
---
# <a name="inheritance"></a><span data-ttu-id="2c096-103">Vererbung</span><span class="sxs-lookup"><span data-stu-id="2c096-103">Inheritance</span></span>

<span data-ttu-id="2c096-104">Die Vererbung im EF-Modell wird verwendet, um zu steuern, wie die Vererbung in den Entitäts Klassen in der Datenbank dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="2c096-104">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="2c096-105">Konventionen</span><span class="sxs-lookup"><span data-stu-id="2c096-105">Conventions</span></span>

<span data-ttu-id="2c096-106">Standardmäßig liegt es an dem Datenbankanbieter, zu bestimmen, wie die Vererbung in der Datenbank dargestellt werden soll.</span><span class="sxs-lookup"><span data-stu-id="2c096-106">By default, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="2c096-107">Informationen zur Behandlung eines relationalen Datenbankanbieters finden Sie unter [Vererbung (relationale Datenbank)](relational/inheritance.md) .</span><span class="sxs-lookup"><span data-stu-id="2c096-107">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="2c096-108">EF wird nur dann die Vererbung einrichten, wenn mindestens zwei geerbte Typen explizit im Modell enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="2c096-108">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="2c096-109">EF sucht nicht nach Basis Typen oder abgeleiteten Typen, die ansonsten nicht im Modell enthalten waren.</span><span class="sxs-lookup"><span data-stu-id="2c096-109">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="2c096-110">Sie können Typen in das Modell einschließen, indem Sie für jeden Typ in der Vererbungs Hierarchie eine *dbset-\<TEntity->* verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="2c096-110">You can include types in the model by exposing a *DbSet\<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="2c096-111">Wenn Sie für eine oder mehrere Entitäten in der Hierarchie keinen *dbset-\<TEntity->* verfügbar machen möchten, können Sie die fließende API verwenden, um sicherzustellen, dass Sie im Modell enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="2c096-111">If you don't want to expose a *DbSet\<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="2c096-112">Wenn Sie sich nicht auf Konventionen verlassen, können Sie den Basistyp explizit mithilfe `HasBaseType`angeben.</span><span class="sxs-lookup"><span data-stu-id="2c096-112">And if you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="2c096-113">Sie können `.HasBaseType((Type)null)` verwenden, um einen Entitätstyp aus der Hierarchie zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="2c096-113">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="2c096-114">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="2c096-114">Data Annotations</span></span>

<span data-ttu-id="2c096-115">Zum Konfigurieren der Vererbung können keine Daten Anmerkungen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="2c096-115">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="2c096-116">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="2c096-116">Fluent API</span></span>

<span data-ttu-id="2c096-117">Die fließende API für die Vererbung hängt vom verwendeten Datenbankanbieter ab.</span><span class="sxs-lookup"><span data-stu-id="2c096-117">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="2c096-118">Die Konfiguration, die Sie für einen relationalen Datenbankanbieter ausführen können, finden Sie unter [Vererbung (relationale Datenbank)](relational/inheritance.md)</span><span class="sxs-lookup"><span data-stu-id="2c096-118">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
