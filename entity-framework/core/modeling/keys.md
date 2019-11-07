---
title: Schlüssel (primär)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 66c64c389294e8e109a614a2bea8311932660dea
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655948"
---
# <a name="keys-primary"></a><span data-ttu-id="7cc22-102">Schlüssel (primäre)</span><span class="sxs-lookup"><span data-stu-id="7cc22-102">Keys (primary)</span></span>

<span data-ttu-id="7cc22-103">Ein Schlüssel dient als primärer eindeutiger Bezeichner für jede Entitäts Instanz.</span><span class="sxs-lookup"><span data-stu-id="7cc22-103">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="7cc22-104">Wenn eine relationale Datenbank verwendet wird, wird dies dem Konzept eines *Primärschlüssels*zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="7cc22-104">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="7cc22-105">Sie können auch einen eindeutigen Bezeichner konfigurieren, bei dem es sich nicht um den Primärschlüssel handelt (Weitere Informationen finden Sie in den [Alternativen Schlüsseln](alternate-keys.md) ).</span><span class="sxs-lookup"><span data-stu-id="7cc22-105">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<span data-ttu-id="7cc22-106">Eine der folgenden Methoden kann verwendet werden, um einen Primärschlüssel einzurichten oder zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="7cc22-106">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="7cc22-107">Konventionen</span><span class="sxs-lookup"><span data-stu-id="7cc22-107">Conventions</span></span>

<span data-ttu-id="7cc22-108">Gemäß der Konvention wird eine Eigenschaft mit dem Namen `Id` oder `<type name>Id` als Schlüssel für eine Entität konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="7cc22-108">By convention, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyIdhighlight=3)]

## <a name="data-annotations"></a><span data-ttu-id="7cc22-109">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="7cc22-109">Data Annotations</span></span>

<span data-ttu-id="7cc22-110">Sie können Daten Anmerkungen verwenden, um eine einzelne Eigenschaft als Schlüssel für eine Entität zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="7cc22-110">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="7cc22-111">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="7cc22-111">Fluent API</span></span>

<span data-ttu-id="7cc22-112">Sie können die fließende API verwenden, um eine einzelne Eigenschaft als Schlüssel für eine Entität zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="7cc22-112">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="7cc22-113">Sie können auch die fließende API verwenden, um mehrere Eigenschaften als Schlüssel einer Entität (als zusammengesetzten Schlüssel bezeichnet) zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="7cc22-113">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="7cc22-114">Zusammengesetzte Schlüssel können nur mithilfe der fließenden API konfiguriert werden, und Sie können niemals einen zusammengesetzten Schlüssel einrichten, und Sie können keine Daten Anmerkungen verwenden, um einen zusammengesetzten Schlüssel zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="7cc22-114">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]
