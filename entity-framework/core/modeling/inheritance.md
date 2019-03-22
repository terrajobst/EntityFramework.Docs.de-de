---
title: Vererbung - ' Entity Framework Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: f6b5c8f5a398ac1e28e29bc17f0674c5b76d7b20
ms.sourcegitcommit: eb8359b7ab3b0a1a08522faf67b703a00ecdcefd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2019
ms.locfileid: "58319126"
---
# <a name="inheritance"></a>Vererbung

Vererbung in das EF-Modell dient zum Steuern, wie die Vererbung in Entitätsklassen in der Datenbank dargestellt wird.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention ist es bis zu der Datenbankanbieter, um zu bestimmen, wie Vererbung in der Datenbank dargestellt wird. Finden Sie unter [Vererbung (relationale Datenbank)](relational/inheritance.md) für wie dies mit einer relationalen Datenbank-Anbieter behandelt wird.

EF wird Vererbung nur eingerichtet, wenn mindestens zwei geerbte Typen explizit in das Modell enthalten sind. EF wird für den Basistyp bzw. abgeleiteten Typen, die andernfalls nicht im Modell enthalten waren, nicht überprüft. Sie können Typen im Modell einschließen, indem verfügbar machen eine *"DbSet"<TEntity>*  für jeden Typ in der Vererbungshierarchie.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Wenn Sie verfügbar machen möchten, keine *"DbSet"<TEntity>*  für eine oder mehrere Entitäten in der Hierarchie, können Sie die Fluent-API verwenden, um sicherzustellen, dass sie die im Modell enthalten sind.
Und wenn Sie Konventionen nicht benötigen, können Sie angeben, dass den Basistyp ausdrücklich mit `HasBaseType`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Sie können `.HasBaseType((Type)null)` auf einen Entitätstyp aus der Hierarchie zu entfernen.

## <a name="data-annotations"></a>Datenanmerkungen

Sie können nicht von Datenanmerkungen verwenden, die Vererbung konfigurieren.

## <a name="fluent-api"></a>Fluent-API

Die Fluent-API für die Vererbung, hängt von den verwendeten Datenbankanbieter ab. Finden Sie unter [Vererbung (relationale Datenbank)](relational/inheritance.md) für die Konfiguration für einen relationalen Datenbankanbieter durchführen können.
