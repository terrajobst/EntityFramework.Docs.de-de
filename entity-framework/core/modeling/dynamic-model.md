---
title: Wechsel zwischen mehreren Modellen mit demselben dbcontext-Typ-EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: a160f0d382ee2a3ac7130ce1ac98eb24b3f79394
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413912"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="c6535-102">Wechsel zwischen mehreren Modellen mit demselben dbcontext-Typ</span><span class="sxs-lookup"><span data-stu-id="c6535-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="c6535-103">Das in `OnModelCreating` integrierte Modell kann eine Eigenschaft im Kontext verwenden, um die Art und Weise zu ändern, wie das Modell erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="c6535-103">The model built in `OnModelCreating` can use a property on the context to change how the model is built.</span></span> <span data-ttu-id="c6535-104">Angenommen, Sie möchten eine Entität auf der Grundlage einer Eigenschaft auf unterschiedliche Weise konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="c6535-104">For example, suppose you wanted to configure an entity differently based on some property:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnModelCreating)]

<span data-ttu-id="c6535-105">Leider funktioniert dieser Code nicht unverändert, da EF das Modell erstellt und `OnModelCreating` nur einmal ausgeführt wird. das Ergebnis wird aus Leistungsgründen zwischengespeichert.</span><span class="sxs-lookup"><span data-stu-id="c6535-105">Unfortunately, this code wouldn't work as-is, since EF builds the model and runs `OnModelCreating` only once, caching the result for performance reasons.</span></span> <span data-ttu-id="c6535-106">Sie können jedoch einen Hook in den Mechanismus zum Zwischenspeichern von Modellen durchführen, um EF die Eigenschaft zu berücksichtigen, die verschiedene Modelle erzeugt.</span><span class="sxs-lookup"><span data-stu-id="c6535-106">However, you can hook into the model caching mechanism to make EF aware of the property producing different models.</span></span>

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="c6535-107">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="c6535-107">IModelCacheKeyFactory</span></span>

<span data-ttu-id="c6535-108">EF verwendet die `IModelCacheKeyFactory`, um Cache Schlüssel für Modelle zu generieren. Standardmäßig geht EF davon aus, dass das Modell für jeden beliebigen Kontexttyp identisch ist, sodass die Standard Implementierung dieses dienstantors einen Schlüssel zurückgibt, der nur den Kontexttyp enthält.</span><span class="sxs-lookup"><span data-stu-id="c6535-108">EF uses the `IModelCacheKeyFactory` to generate cache keys for models; by default, EF assumes that for any given context type the model will be the same, so the default implementation of this service returns a key that just contains the context type.</span></span> <span data-ttu-id="c6535-109">Um verschiedene Modelle desselben Kontext Typs zu entwickeln, müssen Sie den `IModelCacheKeyFactory` Dienst durch die richtige Implementierung ersetzen. der generierte Schlüssel wird mithilfe der `Equals`-Methode mit anderen Modell Schlüsseln verglichen, wobei alle Variablen berücksichtigt werden, die sich auf das Modell auswirken:</span><span class="sxs-lookup"><span data-stu-id="c6535-109">To produce different models from the same context type, you need to replace the `IModelCacheKeyFactory` service with the correct  implementation; the generated key will be compared to other model keys using the `Equals` method, taking into account all the variables that affect the model:</span></span>

<span data-ttu-id="c6535-110">Die folgende Implementierung berücksichtigt die `IgnoreIntProperty` beim Erstellen eines Modell Cache Schlüssels:</span><span class="sxs-lookup"><span data-stu-id="c6535-110">The following implementation takes the `IgnoreIntProperty` into account when producing a model cache key:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicModelCacheKeyFactory.cs?name=DynamicModel)]

<span data-ttu-id="c6535-111">Registrieren Sie schließlich ihre neuen `IModelCacheKeyFactory` im `OnConfiguring`Ihres Kontexts:</span><span class="sxs-lookup"><span data-stu-id="c6535-111">Finally, register your new `IModelCacheKeyFactory` in your context's `OnConfiguring`:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnConfiguring)]

<span data-ttu-id="c6535-112">Weitere Informationen finden Sie im [vollständigen Beispiel Projekt](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) .</span><span class="sxs-lookup"><span data-stu-id="c6535-112">See the [full sample project](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) for more context.</span></span>
