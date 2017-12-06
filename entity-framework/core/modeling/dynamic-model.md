---
title: "Mehrere Modelle mit den gleichen DbContext-Typ – EF Core abwechselnd"
author: AndriySvyryd
uid: core/modeling/dynamic-model
ms.openlocfilehash: e6828c62c55ae6f48d46a20528268264c3f22b26
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="add8a-102">Wechsel zwischen mehrere Modelle mit den gleichen DbContext-Typ</span><span class="sxs-lookup"><span data-stu-id="add8a-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="add8a-103">Das Modell integriert `OnModelCreating` eine Eigenschaft für den Kontext verwenden, so ändern, wie das Modell erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="add8a-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="add8a-104">Z. B. könnte verwendet werden, um eine bestimmte Eigenschaft ausschließen:</span><span class="sxs-lookup"><span data-stu-id="add8a-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="add8a-105">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="add8a-105">IModelCacheKeyFactory</span></span>
<span data-ttu-id="add8a-106">Jedoch wenn Sie versucht, die oben genannten ohne weitere Änderungen Sie das gleiche Modell erhalten würden jedes Mal, wenn ein neuer Kontext erstellt wird, für jeden Wert von `IgnoreIntProperty`.</span><span class="sxs-lookup"><span data-stu-id="add8a-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="add8a-107">Dies wird durch das Modell Cachemechanismus EF verwendet wird, um die Leistung zu verbessern, indem Sie nur aufrufen verursacht `OnModelCreating` einmal und das Zwischenspeichern des Modells.</span><span class="sxs-lookup"><span data-stu-id="add8a-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="add8a-108">Standardmäßig wird in EF davon ausgegangen, dass für jeden angegebenen Kontext Typ des Modells gleich bleiben.</span><span class="sxs-lookup"><span data-stu-id="add8a-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="add8a-109">Um dies zu erreichen die standardmäßige Implementierung des `IModelCacheKeyFactory` gibt einen Schlüssel, die nur den Typ des enthält.</span><span class="sxs-lookup"><span data-stu-id="add8a-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="add8a-110">Dazu Sie ersetzen müssen die `IModelCacheKeyFactory` Dienst.</span><span class="sxs-lookup"><span data-stu-id="add8a-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="add8a-111">Die neue Implementierung muss ein Objekt, das verglichen werden kann, andere Modell-Schlüsseln mithilfe von Zurückgeben der `Equals` -Methode, die alle Variablen berücksichtigt, die das Modell beeinflussen:</span><span class="sxs-lookup"><span data-stu-id="add8a-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
