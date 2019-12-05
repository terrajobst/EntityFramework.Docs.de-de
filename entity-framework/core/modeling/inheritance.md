---
title: Vererbung-EF Core
description: Konfigurieren der Vererbung von Entitäts Typen mit Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 10/27/2016
uid: core/modeling/inheritance
ms.openlocfilehash: 4d43a432174c92ab7f3f9d78a234aefb0a4a17e8
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824675"
---
# <a name="inheritance"></a>Vererbung

Die Vererbung im EF-Modell wird verwendet, um zu steuern, wie die Vererbung in den Entitäts Klassen in der Datenbank dargestellt wird.

## <a name="conventions"></a>Konventionen

Standardmäßig liegt es an dem Datenbankanbieter, zu bestimmen, wie die Vererbung in der Datenbank dargestellt werden soll. Informationen zur Behandlung eines relationalen Datenbankanbieters finden Sie unter [Vererbung (relationale Datenbank)](relational/inheritance.md) .

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
