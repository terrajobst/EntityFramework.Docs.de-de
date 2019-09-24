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
# <a name="including--excluding-properties"></a><span data-ttu-id="3ad45-102">Einschließen und Ausschließen von Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="3ad45-102">Including & Excluding Properties</span></span>

<span data-ttu-id="3ad45-103">Das Einschließen einer Eigenschaft in das Modell bedeutet, dass EF über Metadaten zu dieser Eigenschaft verfügt und versucht, Werte aus der Datenbank zu lesen und zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="3ad45-103">Including a property in the model means that EF has metadata about that property and will attempt to read and write values from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="3ad45-104">Konventionen</span><span class="sxs-lookup"><span data-stu-id="3ad45-104">Conventions</span></span>

<span data-ttu-id="3ad45-105">Gemäß der Konvention werden öffentliche Eigenschaften mit einem Getter und einem Setter in das Modell eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="3ad45-105">By convention, public properties with a getter and a setter will be included in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="3ad45-106">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="3ad45-106">Data Annotations</span></span>

<span data-ttu-id="3ad45-107">Mithilfe von Daten Anmerkungen können Sie eine Eigenschaft aus dem Modell ausschließen.</span><span class="sxs-lookup"><span data-stu-id="3ad45-107">You can use Data Annotations to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a><span data-ttu-id="3ad45-108">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="3ad45-108">Fluent API</span></span>

<span data-ttu-id="3ad45-109">Sie können die fließende API verwenden, um eine Eigenschaft aus dem Modell auszuschließen.</span><span class="sxs-lookup"><span data-stu-id="3ad45-109">You can use the Fluent API to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?highlight=12,13)]
