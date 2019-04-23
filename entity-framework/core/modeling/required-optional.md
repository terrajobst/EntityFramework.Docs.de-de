---
title: Erforderliche bzw. optionale Eigenschaften – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 564d9e62e2ed4f1a52b569630ed4994529e31dc1
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929809"
---
# <a name="required-and-optional-properties"></a>Erforderliche und optionale Eigenschaften

Eine Eigenschaft ist optional, wenn er gültig ist, damit enthalten ist als `null`. Wenn `null` ist kein gültiger Wert, der einer Eigenschaft zugewiesen werden soll, und es gilt eine erforderliche Eigenschaft.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention, eine Eigenschaft, dessen CLR-Typ Null darf, wird so konfiguriert, als optional (`string`, `int?`, `byte[]`usw..). Eigenschaften, deren CLR-Typ darf nicht Null enthalten, werden so konfiguriert, wie erforderlich (`int`, `decimal`, `bool`usw..).

> [!NOTE]  
> Eine Eigenschaft, dessen CLR-Typ nicht null enthalten kann, kann nicht als optional konfiguriert werden. Die Eigenschaft wird von Entity Framework benötigt immer berücksichtigt werden.

## <a name="data-annotations"></a>Datenanmerkungen

Sie können Datenanmerkungen verwenden, um anzugeben, dass eine Eigenschaft erforderlich ist.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a>Fluent-API

Sie können die Fluent-API verwenden, um anzugeben, dass eine Eigenschaft erforderlich ist.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

