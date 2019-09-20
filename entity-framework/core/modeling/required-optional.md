---
title: Erforderliche/optionale Eigenschaften-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 7200cd2eeeba2f22365ef09b1f50edd077240130
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149147"
---
# <a name="required-and-optional-properties"></a>Erforderliche und optionale Eigenschaften

Eine Eigenschaft wird als optional eingestuft, `null`Wenn Sie gültig ist. Wenn `null` kein gültiger Wert ist, der einer Eigenschaft zugewiesen werden soll, wird er als erforderliche Eigenschaft betrachtet.

## <a name="conventions"></a>Konventionen

Gemäß der Konvention wird eine Eigenschaft, deren .NET-Typ NULL enthalten kann, als optional`string`konfiguriert `int?`( `byte[]`,, usw.). Eigenschaften, deren CLR-Typ keine NULL-Werte enthalten darf,`int`werden `decimal`als `bool`erforderlich konfiguriert (,, usw.).

> [!NOTE]  
> Eine Eigenschaft, deren .NET-Typ nicht NULL enthalten darf, kann nicht als optional konfiguriert werden. Die Eigenschaft wird von Entity Framework immer als erforderlich angesehen.

## <a name="data-annotations"></a>Datenanmerkungen

Mithilfe von Daten Anmerkungen können Sie angeben, dass eine Eigenschaft erforderlich ist.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a>Fluent-API

Sie können die fließende API verwenden, um anzugeben, dass eine Eigenschaft erforderlich ist.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

