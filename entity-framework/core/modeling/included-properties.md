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
# <a name="including--excluding-properties"></a><span data-ttu-id="0aaaf-102">Einschließen und Ausschließen von Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="0aaaf-102">Including & Excluding Properties</span></span>

<span data-ttu-id="0aaaf-103">Einschließlich einer Eigenschaft im Modell bedeutet, dass EF Metadaten über diese Eigenschaft, und versucht, lesen und Schreiben von Werten aus bzw. nach der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="0aaaf-103">Including a property in the model means that EF has metadata about that property and will attempt to read and write values from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="0aaaf-104">Konventionen</span><span class="sxs-lookup"><span data-stu-id="0aaaf-104">Conventions</span></span>

<span data-ttu-id="0aaaf-105">Gemäß der Konvention werden die öffentliche Eigenschaften mit Getter und Setter im Modell enthalten sein.</span><span class="sxs-lookup"><span data-stu-id="0aaaf-105">By convention, public properties with a getter and a setter will be included in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="0aaaf-106">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="0aaaf-106">Data Annotations</span></span>

<span data-ttu-id="0aaaf-107">Sie können Datenanmerkungen verwenden, um eine Eigenschaft aus dem Modell auszuschließen.</span><span class="sxs-lookup"><span data-stu-id="0aaaf-107">You can use Data Annotations to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a><span data-ttu-id="0aaaf-108">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="0aaaf-108">Fluent API</span></span>

<span data-ttu-id="0aaaf-109">Sie können die Fluent-API verwenden, um eine Eigenschaft aus dem Modell auszuschließen.</span><span class="sxs-lookup"><span data-stu-id="0aaaf-109">You can use the Fluent API to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/IgnoreProperty.cs?highlight=12,13)]
