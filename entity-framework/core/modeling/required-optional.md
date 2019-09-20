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
# <a name="required-and-optional-properties"></a><span data-ttu-id="85744-102">Erforderliche und optionale Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="85744-102">Required and Optional Properties</span></span>

<span data-ttu-id="85744-103">Eine Eigenschaft wird als optional eingestuft, `null`Wenn Sie gültig ist.</span><span class="sxs-lookup"><span data-stu-id="85744-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="85744-104">Wenn `null` kein gültiger Wert ist, der einer Eigenschaft zugewiesen werden soll, wird er als erforderliche Eigenschaft betrachtet.</span><span class="sxs-lookup"><span data-stu-id="85744-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="85744-105">Konventionen</span><span class="sxs-lookup"><span data-stu-id="85744-105">Conventions</span></span>

<span data-ttu-id="85744-106">Gemäß der Konvention wird eine Eigenschaft, deren .NET-Typ NULL enthalten kann, als optional`string`konfiguriert `int?`( `byte[]`,, usw.).</span><span class="sxs-lookup"><span data-stu-id="85744-106">By convention, a property whose .NET type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="85744-107">Eigenschaften, deren CLR-Typ keine NULL-Werte enthalten darf,`int`werden `decimal`als `bool`erforderlich konfiguriert (,, usw.).</span><span class="sxs-lookup"><span data-stu-id="85744-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="85744-108">Eine Eigenschaft, deren .NET-Typ nicht NULL enthalten darf, kann nicht als optional konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="85744-108">A property whose .NET type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="85744-109">Die Eigenschaft wird von Entity Framework immer als erforderlich angesehen.</span><span class="sxs-lookup"><span data-stu-id="85744-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="85744-110">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="85744-110">Data Annotations</span></span>

<span data-ttu-id="85744-111">Mithilfe von Daten Anmerkungen können Sie angeben, dass eine Eigenschaft erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="85744-111">You can use Data Annotations to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a><span data-ttu-id="85744-112">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="85744-112">Fluent API</span></span>

<span data-ttu-id="85744-113">Sie können die fließende API verwenden, um anzugeben, dass eine Eigenschaft erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="85744-113">You can use the Fluent API to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

