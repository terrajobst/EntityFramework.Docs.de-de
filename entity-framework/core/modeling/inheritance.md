---
title: Vererbung-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: 1f20c455176b5922364584f8c7688c15a4c3f0f9
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197290"
---
# <a name="inheritance"></a><span data-ttu-id="4061c-102">Vererbung</span><span class="sxs-lookup"><span data-stu-id="4061c-102">Inheritance</span></span>

<span data-ttu-id="4061c-103">Die Vererbung im EF-Modell wird verwendet, um zu steuern, wie die Vererbung in den Entitäts Klassen in der Datenbank dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="4061c-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="4061c-104">Konventionen</span><span class="sxs-lookup"><span data-stu-id="4061c-104">Conventions</span></span>

<span data-ttu-id="4061c-105">Gemäß der Konvention liegt es an dem Datenbankanbieter, zu bestimmen, wie die Vererbung in der Datenbank dargestellt werden soll.</span><span class="sxs-lookup"><span data-stu-id="4061c-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="4061c-106">Informationen zur Behandlung eines relationalen Datenbankanbieters finden Sie unter [Vererbung (relationale Datenbank)](relational/inheritance.md) .</span><span class="sxs-lookup"><span data-stu-id="4061c-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="4061c-107">EF wird nur dann die Vererbung einrichten, wenn mindestens zwei geerbte Typen explizit im Modell enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="4061c-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="4061c-108">EF sucht nicht nach Basis Typen oder abgeleiteten Typen, die ansonsten nicht im Modell enthalten waren.</span><span class="sxs-lookup"><span data-stu-id="4061c-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="4061c-109">Sie können Typen in das Modell einschließen, indem Sie ein *dbset<TEntity>*  für jeden Typ in der Vererbungs Hierarchie verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="4061c-109">You can include types in the model by exposing a *DbSet<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="4061c-110">Wenn Sie kein *dbset<TEntity>*  für eine oder mehrere Entitäten in der Hierarchie verfügbar machen möchten, können Sie die fließende API verwenden, um sicherzustellen, dass Sie im Modell enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="4061c-110">If you don't want to expose a *DbSet<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="4061c-111">Wenn Sie sich nicht auf Konventionen verlassen, können Sie den Basistyp explizit mithilfe `HasBaseType`von angeben.</span><span class="sxs-lookup"><span data-stu-id="4061c-111">And if you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="4061c-112">Sie können verwenden `.HasBaseType((Type)null)` , um einen Entitätstyp aus der Hierarchie zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="4061c-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="4061c-113">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="4061c-113">Data Annotations</span></span>

<span data-ttu-id="4061c-114">Zum Konfigurieren der Vererbung können keine Daten Anmerkungen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="4061c-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="4061c-115">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="4061c-115">Fluent API</span></span>

<span data-ttu-id="4061c-116">Die fließende API für die Vererbung hängt vom verwendeten Datenbankanbieter ab.</span><span class="sxs-lookup"><span data-stu-id="4061c-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="4061c-117">Die Konfiguration, die Sie für einen relationalen Datenbankanbieter ausführen können, finden Sie unter [Vererbung (relationale Datenbank)](relational/inheritance.md)</span><span class="sxs-lookup"><span data-stu-id="4061c-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
