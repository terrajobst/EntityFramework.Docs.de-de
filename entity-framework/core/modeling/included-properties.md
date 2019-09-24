---
title: Einschließen & Ausschließen von Eigenschaften-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: cd111af891ef0bbaccf515eed0c1991f105bd362
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197411"
---
# <a name="including--excluding-properties"></a>Einschließen und Ausschließen von Eigenschaften

Das Einschließen einer Eigenschaft in das Modell bedeutet, dass EF über Metadaten zu dieser Eigenschaft verfügt und versucht, Werte aus der Datenbank zu lesen und zu schreiben.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention werden öffentliche Eigenschaften mit einem Getter und einem Setter in das Modell eingeschlossen.

## <a name="data-annotations"></a>Datenanmerkungen

Mithilfe von Daten Anmerkungen können Sie eine Eigenschaft aus dem Modell ausschließen.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a>Fluent-API

Sie können die fließende API verwenden, um eine Eigenschaft aus dem Modell auszuschließen.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?highlight=12,13)]
