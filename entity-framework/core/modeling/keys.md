---
title: Schlüssel (primär)-EF Core
description: Konfigurieren von Schlüsseln für Entitäts Typen bei Verwendung von Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/keys
ms.openlocfilehash: fdaccb42259ba9dad97a05c626edd0291ca96cb0
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824621"
---
# <a name="keys-primary"></a><span data-ttu-id="bbe2d-103">Schlüssel (primäre)</span><span class="sxs-lookup"><span data-stu-id="bbe2d-103">Keys (primary)</span></span>

<span data-ttu-id="bbe2d-104">Ein Schlüssel dient als primärer eindeutiger Bezeichner für jede Entitäts Instanz.</span><span class="sxs-lookup"><span data-stu-id="bbe2d-104">A key serves as the primary unique identifier for each entity instance.</span></span> <span data-ttu-id="bbe2d-105">Wenn eine relationale Datenbank verwendet wird, wird dies dem Konzept eines *Primärschlüssels*zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="bbe2d-105">When using a relational database this maps to the concept of a *primary key*.</span></span> <span data-ttu-id="bbe2d-106">Sie können auch einen eindeutigen Bezeichner konfigurieren, bei dem es sich nicht um den Primärschlüssel handelt (Weitere Informationen finden Sie in den [Alternativen Schlüsseln](alternate-keys.md) ).</span><span class="sxs-lookup"><span data-stu-id="bbe2d-106">You can also configure a unique identifier that is not the primary key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<span data-ttu-id="bbe2d-107">Eine der folgenden Methoden kann verwendet werden, um einen Primärschlüssel einzurichten oder zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="bbe2d-107">One of the following methods can be used to setup/create a primary key.</span></span>

## <a name="conventions"></a><span data-ttu-id="bbe2d-108">Konventionen</span><span class="sxs-lookup"><span data-stu-id="bbe2d-108">Conventions</span></span>

<span data-ttu-id="bbe2d-109">Standardmäßig wird eine Eigenschaft mit dem Namen `Id` oder `<type name>Id` als Schlüssel für eine Entität konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="bbe2d-109">By default, a property named `Id` or `<type name>Id` will be configured as the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyId&highlight=3)]

> [!NOTE]
> <span data-ttu-id="bbe2d-110">[Besitzende Entitäts Typen](xref:core/modeling/owned-entities) verwenden verschiedene Regeln, um Schlüssel zu definieren.</span><span class="sxs-lookup"><span data-stu-id="bbe2d-110">[Owned entity types](xref:core/modeling/owned-entities) use different rules to define keys.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="bbe2d-111">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="bbe2d-111">Data Annotations</span></span>

<span data-ttu-id="bbe2d-112">Sie können Daten Anmerkungen verwenden, um eine einzelne Eigenschaft als Schlüssel für eine Entität zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="bbe2d-112">You can use Data Annotations to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="bbe2d-113">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="bbe2d-113">Fluent API</span></span>

<span data-ttu-id="bbe2d-114">Sie können die fließende API verwenden, um eine einzelne Eigenschaft als Schlüssel für eine Entität zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="bbe2d-114">You can use the Fluent API to configure a single property to be the key of an entity.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

<span data-ttu-id="bbe2d-115">Sie können auch die fließende API verwenden, um mehrere Eigenschaften als Schlüssel einer Entität (als zusammengesetzten Schlüssel bezeichnet) zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="bbe2d-115">You can also use the Fluent API to configure multiple properties to be the key of an entity (known as a composite key).</span></span> <span data-ttu-id="bbe2d-116">Zusammengesetzte Schlüssel können nur mithilfe der fließenden API konfiguriert werden, und Sie können niemals einen zusammengesetzten Schlüssel einrichten, und Sie können keine Daten Anmerkungen verwenden, um einen zusammengesetzten Schlüssel zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="bbe2d-116">Composite keys can only be configured using the Fluent API - conventions will never setup a composite key and you can not use Data Annotations to configure one.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]

## <a name="key-types-and-values"></a><span data-ttu-id="bbe2d-117">Schlüsseltypen und-Werte</span><span class="sxs-lookup"><span data-stu-id="bbe2d-117">Key types and values</span></span>

<span data-ttu-id="bbe2d-118">EF Core unterstützt die Verwendung von Eigenschaften eines beliebigen primitiven Typs als Primärschlüssel, einschließlich `string`, `Guid`, `byte[]` und anderen.</span><span class="sxs-lookup"><span data-stu-id="bbe2d-118">EF Core supports using properties of any primitive type as the primary key, including `string`, `Guid`, `byte[]` and others.</span></span> <span data-ttu-id="bbe2d-119">Diese werden jedoch nicht von allen Datenbanken unterstützt.</span><span class="sxs-lookup"><span data-stu-id="bbe2d-119">But not all databases support them.</span></span> <span data-ttu-id="bbe2d-120">In einigen Fällen können die Schlüsselwerte automatisch in einen unterstützten Typ konvertiert werden. andernfalls sollte die Konvertierung [manuell angegeben](xref:core/modeling/value-conversions)werden.</span><span class="sxs-lookup"><span data-stu-id="bbe2d-120">In some cases the key values can be converted to a supported type automatically, otherwise the conversion should be [specified manually](xref:core/modeling/value-conversions).</span></span>

<span data-ttu-id="bbe2d-121">Schlüsseleigenschaften müssen immer einen nicht standardmäßigen Wert aufweisen, wenn eine neue Entität zum Kontext hinzugefügt wird, aber einige Typen werden [von der Datenbank generiert](xref:core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="bbe2d-121">Key properties must always have a non-default value when adding a new entity to the context, but some types will be [generated by the database](xref:core/modeling/generated-properties).</span></span> <span data-ttu-id="bbe2d-122">In diesem Fall versucht EF, einen temporären Wert zu generieren, wenn die Entität zu nach Verfolgungs Zwecken hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="bbe2d-122">In that case EF will try to generate a temporary value when the entity is added for tracking purposes.</span></span> <span data-ttu-id="bbe2d-123">Nachdem [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) aufgerufen wurde, wird der temporäre Wert durch den von der Datenbank generierten Wert ersetzt.</span><span class="sxs-lookup"><span data-stu-id="bbe2d-123">After [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) is called the temporary value will be replaced by the value generated by the database.</span></span>

> [!Important]
> <span data-ttu-id="bbe2d-124">Wenn eine Schlüsseleigenschaft einen von der Datenbank generierten Wert aufweist und beim Hinzufügen einer Entität ein nicht standardmäßiger Wert angegeben wird, geht EF davon aus, dass die Entität bereits in der Datenbank vorhanden ist, und versucht dann, Sie zu aktualisieren, anstatt eine neue einzufügen.</span><span class="sxs-lookup"><span data-stu-id="bbe2d-124">If a key property has value generated by the database and a non-default value is specified when an entity is added then EF will assume that the entity already exists in the database and will try to update it instead of inserting a new one.</span></span> <span data-ttu-id="bbe2d-125">Um dies zu vermeiden, deaktivieren Sie die Wert Generierung, oder erfahren Sie, [wie Sie explizite Werte für generierte Eigenschaften angeben](../saving/explicit-values-generated-properties.md).</span><span class="sxs-lookup"><span data-stu-id="bbe2d-125">To avoid this turn off value generation or see [how to specify explicit values for generated properties](../saving/explicit-values-generated-properties.md).</span></span>