---
title: Einschließen & Ausschließen von Typen EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: 1249e71c02e58afe7fe06b3fdcf523dfa0c9b17c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655738"
---
# <a name="including--excluding-types"></a><span data-ttu-id="f620a-102">Einschließen und Ausschließen von Typen</span><span class="sxs-lookup"><span data-stu-id="f620a-102">Including & Excluding Types</span></span>

<span data-ttu-id="f620a-103">Das Einschließen eines Typs in das Modell bedeutet, dass EF über Metadaten zu diesem Typ verfügt und versucht, Instanzen aus der Datenbank zu lesen und zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="f620a-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="f620a-104">Konventionen</span><span class="sxs-lookup"><span data-stu-id="f620a-104">Conventions</span></span>

<span data-ttu-id="f620a-105">Gemäß der Konvention sind Typen, die in `DbSet` Eigenschaften in ihrem Kontext verfügbar gemacht werden, in Ihrem Modell enthalten.</span><span class="sxs-lookup"><span data-stu-id="f620a-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="f620a-106">Außerdem sind Typen, die in der `OnModelCreating`-Methode erwähnt werden, ebenfalls enthalten.</span><span class="sxs-lookup"><span data-stu-id="f620a-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="f620a-107">Schließlich sind alle Typen, die gefunden werden, indem die Navigations Eigenschaften von ermittelten Typen rekursiv untersucht werden, auch im Modell enthalten.</span><span class="sxs-lookup"><span data-stu-id="f620a-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="f620a-108">**Im folgenden Code werden z. b. alle drei Typen ermittelt:**</span><span class="sxs-lookup"><span data-stu-id="f620a-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="f620a-109">`Blog`, da es in einer `DbSet`-Eigenschaft im Kontext verfügbar gemacht wird.</span><span class="sxs-lookup"><span data-stu-id="f620a-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="f620a-110">`Post`, da Sie über die `Blog.Posts` Navigations Eigenschaft erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="f620a-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="f620a-111">`AuditEntry`, da Sie in `OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="f620a-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/IncludedTypes.cs?name=IncludedTypes&highlight=3,7,16)]

## <a name="data-annotations"></a><span data-ttu-id="f620a-112">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="f620a-112">Data Annotations</span></span>

<span data-ttu-id="f620a-113">Mithilfe von Daten Anmerkungen können Sie einen Typ aus dem Modell ausschließen.</span><span class="sxs-lookup"><span data-stu-id="f620a-113">You can use Data Annotations to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a><span data-ttu-id="f620a-114">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="f620a-114">Fluent API</span></span>

<span data-ttu-id="f620a-115">Mit der fließend-API können Sie einen Typ aus dem Modell ausschließen.</span><span class="sxs-lookup"><span data-stu-id="f620a-115">You can use the Fluent API to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
