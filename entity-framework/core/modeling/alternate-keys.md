---
title: Alternative Schlüssel-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: e5bb0602adb465435c8e88d045395a9424b2d9a3
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655779"
---
# <a name="alternate-keys"></a><span data-ttu-id="0b060-102">Alternativschlüssel</span><span class="sxs-lookup"><span data-stu-id="0b060-102">Alternate Keys</span></span>

<span data-ttu-id="0b060-103">Ein alternativer Schlüssel dient als alternativer eindeutiger Bezeichner für jede Entitäts Instanz zusätzlich zum Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="0b060-103">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key.</span></span> <span data-ttu-id="0b060-104">Alternative Schlüssel können als Ziel einer Beziehung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0b060-104">Alternate keys can be used as the target of a relationship.</span></span> <span data-ttu-id="0b060-105">Bei Verwendung einer relationalen Datenbank wird diese dem Konzept eines eindeutigen Indexes/einer eindeutigen Einschränkung in den alternativen Schlüssel Spalten und einer oder mehrerer Foreign Key-Einschränkungen zugeordnet, die auf die Spalte (n) verweisen.</span><span class="sxs-lookup"><span data-stu-id="0b060-105">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]  
> <span data-ttu-id="0b060-106">Wenn Sie nur die Eindeutigkeit einer Spalte erzwingen möchten, benötigen Sie einen eindeutigen Index anstelle eines alternativen Schlüssels. Weitere Informationen finden Sie unter [Indizes](indexes.md).</span><span class="sxs-lookup"><span data-stu-id="0b060-106">If you just want to enforce uniqueness of a column then you want a unique index rather than an alternate key, see [Indexes](indexes.md).</span></span> <span data-ttu-id="0b060-107">In EF bieten alternative Schlüssel mehr Funktionalität als eindeutige Indizes, da Sie als Ziel eines fremd Schlüssels verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0b060-107">In EF, alternate keys provide greater functionality than unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="0b060-108">Alternative Schlüssel werden in der Regel bei Bedarf für Sie eingeführt, und Sie müssen Sie nicht manuell konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="0b060-108">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="0b060-109">Weitere Informationen finden Sie unter [Konventionen](#conventions) .</span><span class="sxs-lookup"><span data-stu-id="0b060-109">See [Conventions](#conventions) for more details.</span></span>

## <a name="conventions"></a><span data-ttu-id="0b060-110">Konventionen</span><span class="sxs-lookup"><span data-stu-id="0b060-110">Conventions</span></span>

<span data-ttu-id="0b060-111">Gemäß der Konvention wird ein alternativer Schlüssel für Sie eingeführt, wenn Sie eine Eigenschaft, die nicht der Primärschlüssel ist, als Ziel einer Beziehung identifizieren.</span><span class="sxs-lookup"><span data-stu-id="0b060-111">By convention, an alternate key is introduced for you when you identify a property, that is not the primary key, as the target of a relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/AlternateKey.cs?name=AlternateKey&highlight=12)]

## <a name="data-annotations"></a><span data-ttu-id="0b060-112">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="0b060-112">Data Annotations</span></span>

<span data-ttu-id="0b060-113">Alternative Schlüssel können nicht mithilfe von Daten Anmerkungen konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="0b060-113">Alternate keys can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="0b060-114">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="0b060-114">Fluent API</span></span>

<span data-ttu-id="0b060-115">Sie können die fließende API verwenden, um eine einzelne Eigenschaft als alternativen Schlüssel zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="0b060-115">You can use the Fluent API to configure a single property to be an alternate key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?name=AlternateKeySingle&highlight=7,8)]

<span data-ttu-id="0b060-116">Sie können auch die fließende API verwenden, um mehrere Eigenschaften als alternativen Schlüssel (als zusammengesetzten alternativen Schlüssel bezeichnet) zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="0b060-116">You can also use the Fluent API to configure multiple properties to be an alternate key (known as a composite alternate key).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?name=AlternateKeyComposite&highlight=7,8)]
