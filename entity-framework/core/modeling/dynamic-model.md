---
title: Wechsel zwischen mehreren Modellen mit demselben DbContext - EF Core
author: AndriySvyryd
ms.date: 12/10/2017
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 1d87efb668c12a2862583fba16a6c444b3cda9de
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994985"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="d5ba2-102">Wechsel zwischen mehrere Modelle mit den gleichen Typ von "DbContext"</span><span class="sxs-lookup"><span data-stu-id="d5ba2-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="d5ba2-103">Das Modell integriert `OnModelCreating` können eine Eigenschaft für den Kontext ändern, wie das Modell erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="d5ba2-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="d5ba2-104">Es könnte z. B. verwendet werden, um eine bestimmte Eigenschaft ausschließen:</span><span class="sxs-lookup"><span data-stu-id="d5ba2-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="d5ba2-105">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="d5ba2-105">IModelCacheKeyFactory</span></span>
<span data-ttu-id="d5ba2-106">Wenn Sie versucht, den oben genannten ohne weitere Änderungen Sie jedoch das gleiche Modell nutzen würden jedes Mal ein neuer Kontext, für jeden Wert von erstellt wird `IgnoreIntProperty`.</span><span class="sxs-lookup"><span data-stu-id="d5ba2-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="d5ba2-107">Dies wird durch das Modell Cachemechanismus EF verwendet wird, um die Leistung zu verbessern, indem Sie nur aufrufen verursacht `OnModelCreating` einmal und Zwischenspeichern des Modells.</span><span class="sxs-lookup"><span data-stu-id="d5ba2-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="d5ba2-108">Standardmäßig wird in EF davon ausgegangen, dass für jeden angegebenen Kontext Typ des Modells identisch ist.</span><span class="sxs-lookup"><span data-stu-id="d5ba2-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="d5ba2-109">Hierfür die standardmäßige Implementierung des `IModelCacheKeyFactory` gibt einen Schlüssel, der den Typ des gerade enthält.</span><span class="sxs-lookup"><span data-stu-id="d5ba2-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="d5ba2-110">Dies ändern, Sie ersetzen müssen, die `IModelCacheKeyFactory` Service.</span><span class="sxs-lookup"><span data-stu-id="d5ba2-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="d5ba2-111">Die neue Implementierung muss zurückgegeben wird ein Objekt, das verglichen werden soll, kann andere Modell Schlüssel mithilfe der `Equals` -Methode, die alle Variablen berücksichtigt, die das Modell auswirken:</span><span class="sxs-lookup"><span data-stu-id="d5ba2-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
