---
title: Wechsel zwischen mehreren Modellen mit demselben dbcontext-Typ-EF Core
author: AndriySvyryd
ms.date: 12/10/2017
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 034076b1595894e80b98467354f6c9f139bd7426
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655724"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="ae87c-102">Wechsel zwischen mehreren Modellen mit demselben dbcontext-Typ</span><span class="sxs-lookup"><span data-stu-id="ae87c-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="ae87c-103">Das in `OnModelCreating` integrierte Modell könnte eine Eigenschaft im Kontext verwenden, um die Art und Weise zu ändern, wie das Modell erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="ae87c-103">The model built in `OnModelCreating` could use a property on the context to change how the model is built.</span></span> <span data-ttu-id="ae87c-104">Beispielsweise könnte Sie verwendet werden, um eine bestimmte Eigenschaft auszuschließen:</span><span class="sxs-lookup"><span data-stu-id="ae87c-104">For example it could be used to exclude a certain property:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="ae87c-105">Imodelcachekeyfactory</span><span class="sxs-lookup"><span data-stu-id="ae87c-105">IModelCacheKeyFactory</span></span>

<span data-ttu-id="ae87c-106">Wenn Sie jedoch den obigen Vorgang ohne zusätzliche Änderungen durchgeführt haben, erhalten Sie jedes Mal dasselbe Modell, wenn ein neuer Kontext für einen beliebigen Wert `IgnoreIntProperty`erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="ae87c-106">However if you tried doing the above without additional changes you would get the same model every time a new context is created for any value of `IgnoreIntProperty`.</span></span> <span data-ttu-id="ae87c-107">Dies wird durch den Mechanismus zum Zwischenspeichern von Modellen verursacht, der EF verwendet, um die Leistung zu verbessern, indem nur `OnModelCreating` einmal und Zwischenspeichern des Modells aufgerufen</span><span class="sxs-lookup"><span data-stu-id="ae87c-107">This is caused by the model caching mechanism EF uses to improve the performance by only invoking `OnModelCreating` once and caching the model.</span></span>

<span data-ttu-id="ae87c-108">Standardmäßig geht EF davon aus, dass das Modell für jeden beliebigen Kontexttyp identisch ist.</span><span class="sxs-lookup"><span data-stu-id="ae87c-108">By default EF assumes that for any given context type the model will be the same.</span></span> <span data-ttu-id="ae87c-109">Um dies zu erreichen, gibt die Standard Implementierung von `IModelCacheKeyFactory` einen Schlüssel zurück, der nur den Kontexttyp enthält.</span><span class="sxs-lookup"><span data-stu-id="ae87c-109">To accomplish this the default implementation of `IModelCacheKeyFactory` returns a key that just contains the context type.</span></span> <span data-ttu-id="ae87c-110">Um dies zu ändern, müssen Sie den `IModelCacheKeyFactory`-Dienst ersetzen.</span><span class="sxs-lookup"><span data-stu-id="ae87c-110">To change this you need to replace the `IModelCacheKeyFactory` service.</span></span> <span data-ttu-id="ae87c-111">Die neue Implementierung muss ein Objekt zurückgeben, das mit anderen Modell Schlüsseln verglichen werden kann, wobei die `Equals`-Methode verwendet wird, die alle Variablen berücksichtigt, die sich auf das Modell auswirken:</span><span class="sxs-lookup"><span data-stu-id="ae87c-111">The new implementation needs to return an object that can be compared to other model keys using the `Equals` method that takes into account all the variables that affect the model:</span></span>

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
