---
title: Vererbung - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
ms.technology: entity-framework-core
uid: core/modeling/inheritance
ms.openlocfilehash: f0394bc55dfbfea8277b1ddf898361165dd1fe51
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="inheritance"></a>Vererbung

Vererbung in der EF-Modell dient zum Steuern, wie Vererbung in Entitätsklassen in der Datenbank dargestellt wird.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention ist es bis zu den Datenbankanbieter, um zu bestimmen, wie Vererbung in der Datenbank dargestellt wird. Finden Sie unter [Vererbung (Relational Database)](relational/inheritance.md) für wie dies mit einer relationalen Datenbank-Anbieter behandelt wird.

EF richtet Vererbung nur, wenn mindestens zwei geerbte Typen explizit in das Modell enthalten sind. EF gesucht wird nicht für den Basistyp bzw. abgeleiteten Typen, die andernfalls nicht im Modell enthalten sind. Sie können Typen im Modell einschließen, indem verfügbar gemacht werden eine *DbSet<TEntity>*  für jeden Typ in der Vererbungshierarchie.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Wenn Sie verfügbar machen möchten, keine *DbSet<TEntity>*  für eine oder mehrere Entitäten in der Hierarchie, können Sie die Fluent-API verwenden, um sicherzustellen, dass sie im Modell enthalten sind.
Und wenn Sie auf die Konventionen basieren nicht können Sie angeben, dass den Basistyp explizit mit `HasBaseType`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Sie können `.HasBaseType((Type)null)` ein Entitätstyps aus der Hierarchie entfernen.

## <a name="data-annotations"></a>Datenanmerkungen

Sie können keine Datenanmerkungen verwenden, um Vererbung zu konfigurieren.

## <a name="fluent-api"></a>Fluent-API

Die Fluent-API für die Vererbung, hängt von den verwendeten Anbieter ab. Finden Sie unter [Vererbung (Relational Database)](relational/inheritance.md) für die Konfiguration, die Sie für einen Anbieter für die relationale Datenbank ausführen können.
