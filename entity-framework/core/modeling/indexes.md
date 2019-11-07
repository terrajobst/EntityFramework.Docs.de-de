---
title: Indizes-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: d1b5cd6853cd24f7e1aa3aace2f0a3455c657cc1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655697"
---
# <a name="indexes"></a><span data-ttu-id="b4b8e-102">Indizes</span><span class="sxs-lookup"><span data-stu-id="b4b8e-102">Indexes</span></span>

<span data-ttu-id="b4b8e-103">Indizes sind ein gängiges Konzept in vielen Daten speichern.</span><span class="sxs-lookup"><span data-stu-id="b4b8e-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="b4b8e-104">Während Ihre Implementierung im Datenspeicher variieren kann, werden Sie verwendet, um Suchvorgänge auf Grundlage einer Spalte (oder einer Gruppe von Spalten) effizienter zu gestalten.</span><span class="sxs-lookup"><span data-stu-id="b4b8e-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="b4b8e-105">Konventionen</span><span class="sxs-lookup"><span data-stu-id="b4b8e-105">Conventions</span></span>

<span data-ttu-id="b4b8e-106">Gemäß der Konvention wird ein Index in jeder Eigenschaft (oder einem Satz von Eigenschaften) erstellt, die als Fremdschlüssel verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="b4b8e-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="b4b8e-107">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="b4b8e-107">Data Annotations</span></span>

<span data-ttu-id="b4b8e-108">Indizes können nicht mithilfe von Daten Anmerkungen erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="b4b8e-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="b4b8e-109">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="b4b8e-109">Fluent API</span></span>

<span data-ttu-id="b4b8e-110">Sie können die fließende API verwenden, um einen Index für eine einzelne Eigenschaft anzugeben.</span><span class="sxs-lookup"><span data-stu-id="b4b8e-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="b4b8e-111">Standardmäßig sind Indizes nicht eindeutig.</span><span class="sxs-lookup"><span data-stu-id="b4b8e-111">By default, indexes are non-unique.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=7,8)]

<span data-ttu-id="b4b8e-112">Sie können auch angeben, dass ein Index eindeutig sein soll. Dies bedeutet, dass keine zwei Entitäten für die angegebene Eigenschaft (n) die gleichen Werte aufweisen dürfen.</span><span class="sxs-lookup"><span data-stu-id="b4b8e-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=ModelBuilder&highlight=3)]

<span data-ttu-id="b4b8e-113">Sie können einen Index auch über mehr als eine Spalte angeben.</span><span class="sxs-lookup"><span data-stu-id="b4b8e-113">You can also specify an index over more than one column.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=7,8)]

> [!TIP]  
> <span data-ttu-id="b4b8e-114">Pro eindeutigem Satz von Eigenschaften gibt es nur einen Index.</span><span class="sxs-lookup"><span data-stu-id="b4b8e-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="b4b8e-115">Wenn Sie die fließende API verwenden, um einen Index für eine Gruppe von Eigenschaften zu konfigurieren, für die bereits ein Index definiert wurde (entweder durch Konvention oder vorherige Konfiguration), ändern Sie die Definition des Indexes.</span><span class="sxs-lookup"><span data-stu-id="b4b8e-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="b4b8e-116">Dies ist hilfreich, wenn Sie einen von der Konvention erstellten Index weiter konfigurieren möchten.</span><span class="sxs-lookup"><span data-stu-id="b4b8e-116">This is useful if you want to further configure an index that was created by convention.</span></span>
