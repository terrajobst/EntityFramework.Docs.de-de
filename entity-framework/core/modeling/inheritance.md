---
title: Vererbung-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: 1f20c455176b5922364584f8c7688c15a4c3f0f9
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197290"
---
# <a name="inheritance"></a>Vererbung

Die Vererbung im EF-Modell wird verwendet, um zu steuern, wie die Vererbung in den Entitäts Klassen in der Datenbank dargestellt wird.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention liegt es an dem Datenbankanbieter, zu bestimmen, wie die Vererbung in der Datenbank dargestellt werden soll. Informationen zur Behandlung eines relationalen Datenbankanbieters finden Sie unter [Vererbung (relationale Datenbank)](relational/inheritance.md) .

EF wird nur dann die Vererbung einrichten, wenn mindestens zwei geerbte Typen explizit im Modell enthalten sind. EF sucht nicht nach Basis Typen oder abgeleiteten Typen, die ansonsten nicht im Modell enthalten waren. Sie können Typen in das Modell einschließen, indem Sie ein *dbset<TEntity>*  für jeden Typ in der Vererbungs Hierarchie verfügbar machen.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Wenn Sie kein *dbset<TEntity>*  für eine oder mehrere Entitäten in der Hierarchie verfügbar machen möchten, können Sie die fließende API verwenden, um sicherzustellen, dass Sie im Modell enthalten sind.
Wenn Sie sich nicht auf Konventionen verlassen, können Sie den Basistyp explizit mithilfe `HasBaseType`von angeben.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Sie können verwenden `.HasBaseType((Type)null)` , um einen Entitätstyp aus der Hierarchie zu entfernen.

## <a name="data-annotations"></a>Datenanmerkungen

Zum Konfigurieren der Vererbung können keine Daten Anmerkungen verwendet werden.

## <a name="fluent-api"></a>Fluent-API

Die fließende API für die Vererbung hängt vom verwendeten Datenbankanbieter ab. Die Konfiguration, die Sie für einen relationalen Datenbankanbieter ausführen können, finden Sie unter [Vererbung (relationale Datenbank)](relational/inheritance.md)
