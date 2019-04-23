---
title: Einschließen und Ausschließen von Eigenschaften – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: 022534091bb48e491c8808791a401216a339d7b0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929822"
---
# <a name="including--excluding-properties"></a>Einschließen und Ausschließen von Eigenschaften

Einschließlich einer Eigenschaft im Modell bedeutet, dass EF Metadaten über diese Eigenschaft, und versucht, lesen und Schreiben von Werten aus bzw. nach der Datenbank.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention werden die öffentliche Eigenschaften mit Getter und Setter im Modell enthalten sein.

## <a name="data-annotations"></a>Datenanmerkungen

Sie können Datenanmerkungen verwenden, um eine Eigenschaft aus dem Modell auszuschließen.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, um eine Eigenschaft aus dem Modell auszuschließen.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/IgnoreProperty.cs?highlight=12,13)]
