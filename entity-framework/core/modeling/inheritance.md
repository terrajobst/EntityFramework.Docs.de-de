---
title: Vererbung-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: abc1caa4d3839b7cdb52b316bcfc8f648b609b70
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655688"
---
# <a name="inheritance"></a>Vererbung

Die Vererbung im EF-Modell wird verwendet, um zu steuern, wie die Vererbung in den Entitäts Klassen in der Datenbank dargestellt wird.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention liegt es an dem Datenbankanbieter, zu bestimmen, wie die Vererbung in der Datenbank dargestellt werden soll. Informationen zur Behandlung eines relationalen Datenbankanbieters finden Sie unter [Vererbung (relationale Datenbank)](relational/inheritance.md) .

EF wird nur dann die Vererbung einrichten, wenn mindestens zwei geerbte Typen explizit im Modell enthalten sind. EF sucht nicht nach Basis Typen oder abgeleiteten Typen, die ansonsten nicht im Modell enthalten waren. Sie können Typen in das Modell einschließen, indem Sie für jeden Typ in der Vererbungs Hierarchie eine *dbset-\<TEntity->* verfügbar machen.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Wenn Sie für eine oder mehrere Entitäten in der Hierarchie keinen *dbset-\<TEntity->* verfügbar machen möchten, können Sie die fließende API verwenden, um sicherzustellen, dass Sie im Modell enthalten sind.
Wenn Sie sich nicht auf Konventionen verlassen, können Sie den Basistyp explizit mithilfe `HasBaseType`angeben.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Sie können `.HasBaseType((Type)null)` verwenden, um einen Entitätstyp aus der Hierarchie zu entfernen.

## <a name="data-annotations"></a>Datenanmerkungen

Zum Konfigurieren der Vererbung können keine Daten Anmerkungen verwendet werden.

## <a name="fluent-api"></a>Fluent-API

Die fließende API für die Vererbung hängt vom verwendeten Datenbankanbieter ab. Die Konfiguration, die Sie für einen relationalen Datenbankanbieter ausführen können, finden Sie unter [Vererbung (relationale Datenbank)](relational/inheritance.md)
