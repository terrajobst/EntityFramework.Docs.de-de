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
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Wechsel zwischen mehrere Modelle mit den gleichen DbContext-Typ

Das Modell integriert `OnModelCreating` eine Eigenschaft für den Kontext verwenden, so ändern, wie das Modell erstellt wird. Z. B. könnte verwendet werden, um eine bestimmte Eigenschaft ausschließen:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory
Jedoch wenn Sie versucht, die oben genannten ohne weitere Änderungen Sie das gleiche Modell erhalten würden jedes Mal, wenn ein neuer Kontext erstellt wird, für jeden Wert von `IgnoreIntProperty`. Dies wird durch das Modell Cachemechanismus EF verwendet wird, um die Leistung zu verbessern, indem Sie nur aufrufen verursacht `OnModelCreating` einmal und das Zwischenspeichern des Modells.

Standardmäßig wird in EF davon ausgegangen, dass für jeden angegebenen Kontext Typ des Modells gleich bleiben. Um dies zu erreichen die standardmäßige Implementierung des `IModelCacheKeyFactory` gibt einen Schlüssel, die nur den Typ des enthält. Dazu Sie ersetzen müssen die `IModelCacheKeyFactory` Dienst. Die neue Implementierung muss ein Objekt, das verglichen werden kann, andere Modell-Schlüsseln mithilfe von Zurückgeben der `Equals` -Methode, die alle Variablen berücksichtigt, die das Modell beeinflussen:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
