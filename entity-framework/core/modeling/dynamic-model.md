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
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Wechsel zwischen mehrere Modelle mit den gleichen Typ von "DbContext"

Das Modell integriert `OnModelCreating` können eine Eigenschaft für den Kontext ändern, wie das Modell erstellt wurde. Es könnte z. B. verwendet werden, um eine bestimmte Eigenschaft ausschließen:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory
Wenn Sie versucht, den oben genannten ohne weitere Änderungen Sie jedoch das gleiche Modell nutzen würden jedes Mal ein neuer Kontext, für jeden Wert von erstellt wird `IgnoreIntProperty`. Dies wird durch das Modell Cachemechanismus EF verwendet wird, um die Leistung zu verbessern, indem Sie nur aufrufen verursacht `OnModelCreating` einmal und Zwischenspeichern des Modells.

Standardmäßig wird in EF davon ausgegangen, dass für jeden angegebenen Kontext Typ des Modells identisch ist. Hierfür die standardmäßige Implementierung des `IModelCacheKeyFactory` gibt einen Schlüssel, der den Typ des gerade enthält. Dies ändern, Sie ersetzen müssen, die `IModelCacheKeyFactory` Service. Die neue Implementierung muss zurückgegeben wird ein Objekt, das verglichen werden soll, kann andere Modell Schlüssel mithilfe der `Equals` -Methode, die alle Variablen berücksichtigt, die das Modell auswirken:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
