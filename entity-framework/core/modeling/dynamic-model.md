---
title: Wechsel zwischen mehreren Modellen mit demselben dbcontext-Typ-EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/dynamic-model
ms.openlocfilehash: 156d5666cbd9352b274ddc70c99704ca62aeb1fd
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781130"
---
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a><span data-ttu-id="7c355-102">Wechsel zwischen mehreren Modellen mit demselben dbcontext-Typ</span><span class="sxs-lookup"><span data-stu-id="7c355-102">Alternating between multiple models with the same DbContext type</span></span>

<span data-ttu-id="7c355-103">Das in `OnModelCreating` integrierte Modell kann eine Eigenschaft im Kontext verwenden, um die Art und Weise zu ändern, wie das Modell erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="7c355-103">The model built in `OnModelCreating` can use a property on the context to change how the model is built.</span></span> <span data-ttu-id="7c355-104">Angenommen, Sie möchten eine Entität auf der Grundlage einer Eigenschaft auf unterschiedliche Weise konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="7c355-104">For example, suppose you wanted to configure an entity differently based on some property:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnModelCreating)]

<span data-ttu-id="7c355-105">Leider funktioniert dieser Code nicht unverändert, da EF das Modell erstellt und `OnModelCreating` nur einmal ausgeführt wird. das Ergebnis wird aus Leistungsgründen zwischengespeichert.</span><span class="sxs-lookup"><span data-stu-id="7c355-105">Unfortunately, this code wouldn't work as-is, since EF builds the model and runs `OnModelCreating` only once, caching the result for performance reasons.</span></span> <span data-ttu-id="7c355-106">Sie können jedoch einen Hook in den Mechanismus zum Zwischenspeichern von Modellen durchführen, um EF die Eigenschaft zu berücksichtigen, die verschiedene Modelle erzeugt.</span><span class="sxs-lookup"><span data-stu-id="7c355-106">However, you can hook into the model caching mechanism to make EF aware of the property producing different models.</span></span>

## <a name="imodelcachekeyfactory"></a><span data-ttu-id="7c355-107">IModelCacheKeyFactory</span><span class="sxs-lookup"><span data-stu-id="7c355-107">IModelCacheKeyFactory</span></span>

<span data-ttu-id="7c355-108">EF verwendet die `IModelCacheKeyFactory`, um Cache Schlüssel für Modelle zu generieren. Standardmäßig geht EF davon aus, dass das Modell für jeden beliebigen Kontexttyp identisch ist, sodass die Standard Implementierung dieses dienstantors einen Schlüssel zurückgibt, der nur den Kontexttyp enthält.</span><span class="sxs-lookup"><span data-stu-id="7c355-108">EF uses the `IModelCacheKeyFactory` to generate cache keys for models; by default, EF assumes that for any given context type the model will be the same, so the default implementation of this service returns a key that just contains the context type.</span></span> <span data-ttu-id="7c355-109">Um verschiedene Modelle desselben Kontext Typs zu entwickeln, müssen Sie den `IModelCacheKeyFactory` Dienst durch die richtige Implementierung ersetzen. der generierte Schlüssel wird mithilfe der `Equals`-Methode mit anderen Modell Schlüsseln verglichen, wobei alle Variablen berücksichtigt werden, die sich auf das Modell auswirken:</span><span class="sxs-lookup"><span data-stu-id="7c355-109">To produce different models from the same context type, you need to replace the `IModelCacheKeyFactory` service with the correct  implementation; the generated key will be compared to other model keys using the `Equals` method, taking into account all the variables that affect the model:</span></span>

<span data-ttu-id="7c355-110">Die folgende Implementierung berücksichtigt die `IgnoreIntProperty` beim Erstellen eines Modell Cache Schlüssels:</span><span class="sxs-lookup"><span data-stu-id="7c355-110">The following implementation takes the `IgnoreIntProperty` into account when producing a model cache key:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicModelCacheKeyFactory.cs?name=DynamicModel)]

<span data-ttu-id="7c355-111">Registrieren Sie schließlich ihre neuen `IModelCacheKeyFactory` im `OnConfiguring`Ihres Kontexts:</span><span class="sxs-lookup"><span data-stu-id="7c355-111">Finally, register your new `IModelCacheKeyFactory` in your context's `OnConfiguring`:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnConfiguring)]

<span data-ttu-id="7c355-112">Weitere Informationen finden Sie im [vollständigen Beispiel Projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) .</span><span class="sxs-lookup"><span data-stu-id="7c355-112">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) for more context.</span></span>
