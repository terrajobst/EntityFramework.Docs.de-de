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
# <a name="required-and-optional-properties"></a><span data-ttu-id="d2ee9-102">Erforderliche und optionale Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="d2ee9-102">Required and Optional Properties</span></span>

<span data-ttu-id="d2ee9-103">Eine Eigenschaft ist optional, wenn er gültig ist, damit enthalten ist als `null`.</span><span class="sxs-lookup"><span data-stu-id="d2ee9-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="d2ee9-104">Wenn `null` ist kein gültiger Wert, der einer Eigenschaft zugewiesen werden soll, und es gilt eine erforderliche Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="d2ee9-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="d2ee9-105">Konventionen</span><span class="sxs-lookup"><span data-stu-id="d2ee9-105">Conventions</span></span>

<span data-ttu-id="d2ee9-106">Gemäß der Konvention, eine Eigenschaft, dessen CLR-Typ Null darf, wird so konfiguriert, als optional (`string`, `int?`, `byte[]`usw..).</span><span class="sxs-lookup"><span data-stu-id="d2ee9-106">By convention, a property whose CLR type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="d2ee9-107">Eigenschaften, deren CLR-Typ darf nicht Null enthalten, werden so konfiguriert, wie erforderlich (`int`, `decimal`, `bool`usw..).</span><span class="sxs-lookup"><span data-stu-id="d2ee9-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="d2ee9-108">Eine Eigenschaft, dessen CLR-Typ nicht null enthalten kann, kann nicht als optional konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="d2ee9-108">A property whose CLR type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="d2ee9-109">Die Eigenschaft wird von Entity Framework benötigt immer berücksichtigt werden.</span><span class="sxs-lookup"><span data-stu-id="d2ee9-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="d2ee9-110">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="d2ee9-110">Data Annotations</span></span>

<span data-ttu-id="d2ee9-111">Sie können Datenanmerkungen verwenden, um anzugeben, dass eine Eigenschaft erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="d2ee9-111">You can use Data Annotations to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a><span data-ttu-id="d2ee9-112">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="d2ee9-112">Fluent API</span></span>

<span data-ttu-id="d2ee9-113">Sie können die Fluent-API verwenden, um anzugeben, dass eine Eigenschaft erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="d2ee9-113">You can use the Fluent API to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

