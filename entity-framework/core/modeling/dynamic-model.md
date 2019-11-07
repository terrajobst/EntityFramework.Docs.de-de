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
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Wechsel zwischen mehreren Modellen mit demselben dbcontext-Typ

Das in `OnModelCreating` integrierte Modell könnte eine Eigenschaft im Kontext verwenden, um die Art und Weise zu ändern, wie das Modell erstellt wird. Beispielsweise könnte Sie verwendet werden, um eine bestimmte Eigenschaft auszuschließen:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicContext.cs?name=Class)]

## <a name="imodelcachekeyfactory"></a>Imodelcachekeyfactory

Wenn Sie jedoch den obigen Vorgang ohne zusätzliche Änderungen durchgeführt haben, erhalten Sie jedes Mal dasselbe Modell, wenn ein neuer Kontext für einen beliebigen Wert `IgnoreIntProperty`erstellt wird. Dies wird durch den Mechanismus zum Zwischenspeichern von Modellen verursacht, der EF verwendet, um die Leistung zu verbessern, indem nur `OnModelCreating` einmal und Zwischenspeichern des Modells aufgerufen

Standardmäßig geht EF davon aus, dass das Modell für jeden beliebigen Kontexttyp identisch ist. Um dies zu erreichen, gibt die Standard Implementierung von `IModelCacheKeyFactory` einen Schlüssel zurück, der nur den Kontexttyp enthält. Um dies zu ändern, müssen Sie den `IModelCacheKeyFactory`-Dienst ersetzen. Die neue Implementierung muss ein Objekt zurückgeben, das mit anderen Modell Schlüsseln verglichen werden kann, wobei die `Equals`-Methode verwendet wird, die alle Variablen berücksichtigt, die sich auf das Modell auswirken:

[!code-csharp[Main](../../../samples/core/DynamicModel/DynamicModelCacheKeyFactory.cs?name=Class)]
