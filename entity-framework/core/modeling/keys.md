---
title: Schlüssel (primär) - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 51d163b867085f42f415dbd7afa9e311ab1781a0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929835"
---
# <a name="keys-primary"></a><span data-ttu-id="7855c-102">Schlüssel (primär)</span><span class="sxs-lookup"><span data-stu-id="7855c-102">Keys (primary)</span></span>

<span data-ttu-id="7855c-103">Ein Schlüssel dient als primärer Eindeutiger Bezeichner für jede Entitätsinstanz.</span><span class="sxs-lookup"><span data-stu-id="7855c-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="7855c-104">Bei Verwendung eine relationale Datenbank zugeordnet, das Konzept von einem *Primärschlüssel*.</span><span class="sxs-lookup"><span data-stu-id="7855c-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="7855c-105">Sie können auch einen eindeutigen Bezeichner, die nicht den Primärschlüssel konfigurieren (finden Sie unter [Alternativschlüssel](alternate-keys.md) Informationen).</span><span class="sxs-lookup"><span data-stu-id="7855c-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span> 

<span data-ttu-id="7855c-106">Eine der folgenden Methoden kann verwendet werden, um einen Primärschlüssel Einrichtung/erstellen.</span><span class="sxs-lookup"><span data-stu-id="7855c-106">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="7855c-107">Konventionen</span><span class="sxs-lookup"><span data-stu-id="7855c-107">Conventions</span></span>

<span data-ttu-id="7855c-108">Gemäß der Konvention, eine Eigenschaft mit dem Namen `Id` oder `<type name>Id` wird als Schlüssel für eine Entität konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="7855c-108">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/KeyTypeNameId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string CarId { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="7855c-109">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="7855c-109">Data Annotations</span></span>

<span data-ttu-id="7855c-110">Sie können Datenanmerkungen verwenden, so konfigurieren Sie eine einzelne Eigenschaft als Schlüssel einer Entität.</span><span class="sxs-lookup"><span data-stu-id="7855c-110">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="7855c-111">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="7855c-111">Fluent API</span></span>

<span data-ttu-id="7855c-112">Sie können die Fluent-API verwenden, so konfigurieren Sie eine einzelne Eigenschaft als Schlüssel einer Entität.</span><span class="sxs-lookup"><span data-stu-id="7855c-112">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="7855c-113">Sie können auch die Fluent-API verwenden, so konfigurieren Sie mehrere Eigenschaften als Schlüssel einer Entität (als einen zusammengesetzten Schlüssel bezeichnet).</span><span class="sxs-lookup"><span data-stu-id="7855c-113">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="7855c-114">Zusammengesetzte Schlüssel können nur mithilfe der Fluent-API konfiguriert werden – Konventionen werden einen zusammengesetzten Schlüssel niemals eingerichtet und Datenanmerkungen Konfiguration kann nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="7855c-114">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/KeyComposite.cs?highlight=11,12)]
