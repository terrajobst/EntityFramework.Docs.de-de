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
# <a name="alternating-between-multiple-models-with-the-same-dbcontext-type"></a>Wechsel zwischen mehreren Modellen mit demselben dbcontext-Typ

Das in `OnModelCreating` integrierte Modell kann eine Eigenschaft im Kontext verwenden, um die Art und Weise zu ändern, wie das Modell erstellt wird. Angenommen, Sie möchten eine Entität auf der Grundlage einer Eigenschaft auf unterschiedliche Weise konfigurieren:

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnModelCreating)]

Leider funktioniert dieser Code nicht unverändert, da EF das Modell erstellt und `OnModelCreating` nur einmal ausgeführt wird. das Ergebnis wird aus Leistungsgründen zwischengespeichert. Sie können jedoch einen Hook in den Mechanismus zum Zwischenspeichern von Modellen durchführen, um EF die Eigenschaft zu berücksichtigen, die verschiedene Modelle erzeugt.

## <a name="imodelcachekeyfactory"></a>IModelCacheKeyFactory

EF verwendet die `IModelCacheKeyFactory`, um Cache Schlüssel für Modelle zu generieren. Standardmäßig geht EF davon aus, dass das Modell für jeden beliebigen Kontexttyp identisch ist, sodass die Standard Implementierung dieses dienstantors einen Schlüssel zurückgibt, der nur den Kontexttyp enthält. Um verschiedene Modelle desselben Kontext Typs zu entwickeln, müssen Sie den `IModelCacheKeyFactory` Dienst durch die richtige Implementierung ersetzen. der generierte Schlüssel wird mithilfe der `Equals`-Methode mit anderen Modell Schlüsseln verglichen, wobei alle Variablen berücksichtigt werden, die sich auf das Modell auswirken:

Die folgende Implementierung berücksichtigt die `IgnoreIntProperty` beim Erstellen eines Modell Cache Schlüssels:

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicModelCacheKeyFactory.cs?name=DynamicModel)]

Registrieren Sie schließlich ihre neuen `IModelCacheKeyFactory` im `OnConfiguring`Ihres Kontexts:

[!code-csharp[Main](../../../samples/core/Modeling/DynamicModel/DynamicContext.cs?name=OnConfiguring)]

Weitere Informationen finden Sie im [vollständigen Beispiel Projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DynamicModel) .
