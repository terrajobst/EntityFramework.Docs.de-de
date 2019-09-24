---
title: Schlüssel (primär)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 8b32bf6417890a954c933a5973a2c90c609beeca
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197269"
---
# <a name="keys-primary"></a><span data-ttu-id="e7116-102">Schlüssel (primäre)</span><span class="sxs-lookup"><span data-stu-id="e7116-102">Keys (primary)</span></span>

<span data-ttu-id="e7116-103">Ein Schlüssel dient als primärer eindeutiger Bezeichner für jede Entitäts Instanz.</span><span class="sxs-lookup"><span data-stu-id="e7116-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="e7116-104">Wenn eine relationale Datenbank verwendet wird, wird dies dem Konzept eines *Primärschlüssels*zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="e7116-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="e7116-105">Sie können auch einen eindeutigen Bezeichner konfigurieren, bei dem es sich nicht um den Primärschlüssel handelt (Weitere Informationen finden Sie in den [Alternativen Schlüsseln](alternate-keys.md) ).</span><span class="sxs-lookup"><span data-stu-id="e7116-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span> 

<span data-ttu-id="e7116-106">Eine der folgenden Methoden kann verwendet werden, um einen Primärschlüssel einzurichten oder zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="e7116-106">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="e7116-107">Konventionen</span><span class="sxs-lookup"><span data-stu-id="e7116-107">Conventions</span></span>

<span data-ttu-id="e7116-108">Gemäß der Konvention wird eine Eigenschaft `Id` mit `<type name>Id` dem Namen oder als Schlüssel einer Entität konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="e7116-108">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/KeyId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string Id { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/KeyTypeNameId.cs?highlight=3)] -->
``` csharp
class Car
{
    public string CarId { get; set; }

    public string Make { get; set; }
    public string Model { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="e7116-109">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="e7116-109">Data Annotations</span></span>

<span data-ttu-id="e7116-110">Sie können Daten Anmerkungen verwenden, um eine einzelne Eigenschaft als Schlüssel für eine Entität zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="e7116-110">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="e7116-111">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="e7116-111">Fluent API</span></span>

<span data-ttu-id="e7116-112">Sie können die fließende API verwenden, um eine einzelne Eigenschaft als Schlüssel für eine Entität zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="e7116-112">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="e7116-113">Sie können auch die fließende API verwenden, um mehrere Eigenschaften als Schlüssel einer Entität (als zusammengesetzten Schlüssel bezeichnet) zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="e7116-113">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="e7116-114">Zusammengesetzte Schlüssel können nur mithilfe der fließenden API konfiguriert werden, und Sie können niemals einen zusammengesetzten Schlüssel einrichten, und Sie können keine Daten Anmerkungen verwenden, um einen zusammengesetzten Schlüssel zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="e7116-114">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]
