---
title: Vererbung - ' Entity Framework Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: c5fa9d13dec8cfc3e1cac69e471f509cbbb9e4c5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995895"
---
# <a name="inheritance"></a>Vererbung

Vererbung in das EF-Modell dient zum Steuern, wie die Vererbung in Entitätsklassen in der Datenbank dargestellt wird.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention ist es bis zu der Datenbankanbieter, um zu bestimmen, wie Vererbung in der Datenbank dargestellt wird. Finden Sie unter [Vererbung (relationale Datenbank)](relational/inheritance.md) für wie dies mit einer relationalen Datenbank-Anbieter behandelt wird.

EF wird Vererbung nur eingerichtet, wenn mindestens zwei geerbte Typen explizit in das Modell enthalten sind. EF wird für den Basistyp bzw. abgeleiteten Typen, die andernfalls nicht im Modell enthalten waren, nicht überprüft. Sie können Typen im Modell einschließen, indem verfügbar machen eine *"DbSet"<TEntity>*  für jeden Typ in der Vererbungshierarchie.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Wenn Sie verfügbar machen möchten, keine *"DbSet"<TEntity>*  für eine oder mehrere Entitäten in der Hierarchie, können Sie die Fluent-API verwenden, um sicherzustellen, dass sie die im Modell enthalten sind.
Und wenn Sie auf Konventionen basieren nicht können Sie angeben, dass den Basistyp ausdrücklich mit `HasBaseType`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Sie können `.HasBaseType((Type)null)` auf einen Entitätstyp aus der Hierarchie zu entfernen.

## <a name="data-annotations"></a>Datenanmerkungen

Sie können nicht von Datenanmerkungen verwenden, die Vererbung konfigurieren.

## <a name="fluent-api"></a>Fluent-API

Die Fluent-API für die Vererbung, hängt von den verwendeten Datenbankanbieter ab. Finden Sie unter [Vererbung (relationale Datenbank)](relational/inheritance.md) für die Konfiguration für einen relationalen Datenbankanbieter durchführen können.
