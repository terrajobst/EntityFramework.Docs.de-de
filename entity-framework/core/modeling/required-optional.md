---
title: Erforderliche und optionale Eigenschaften-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: fd9e96e6f79965e63b07c21217edd004fd5c4d54
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197849"
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="52f68-102">Erforderliche und optionale Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="52f68-102">Required and Optional Properties</span></span>

<span data-ttu-id="52f68-103">Eine Eigenschaft wird als optional eingestuft, `null`Wenn Sie gültig ist.</span><span class="sxs-lookup"><span data-stu-id="52f68-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="52f68-104">Wenn `null` kein gültiger Wert ist, der einer Eigenschaft zugewiesen werden soll, wird er als erforderliche Eigenschaft betrachtet.</span><span class="sxs-lookup"><span data-stu-id="52f68-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

<span data-ttu-id="52f68-105">Bei der Zuordnung zu einem relationalen Datenbankschema werden erforderliche Eigenschaften als Spalten erstellt, die keine NULL-Werte zulassen, und optionale Eigenschaften werden als Spalten erstellt, die NULL-Werte zulassen.</span><span class="sxs-lookup"><span data-stu-id="52f68-105">When mapping to a relational database schema, required properties are created as non-nullable columns, and optional properties are created as nullable columns.</span></span>

## <a name="conventions"></a><span data-ttu-id="52f68-106">Konventionen</span><span class="sxs-lookup"><span data-stu-id="52f68-106">Conventions</span></span>

<span data-ttu-id="52f68-107">Gemäß der Konvention wird eine Eigenschaft, deren .NET-Typ NULL enthalten kann, als optional konfiguriert, wohingegen Eigenschaften, deren .NET-Typ keinen NULL-Wert enthalten darf, als erforderlich konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="52f68-107">By convention, a property whose .NET type can contain null will be configured as optional, whereas properties whose .NET type cannot contain null will be configured as required.</span></span> <span data-ttu-id="52f68-108">Beispielsweise werden alle Eigenschaften mit .net-Werttypen `decimal`( `bool``int`,, usw.) als erforderlich konfiguriert, und alle Eigenschaften mit .net-Werttypen,`int?`die NULL `bool?`-Werte zulassen (, `decimal?`, usw.) sind als optional konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="52f68-108">For example, all properties with .NET value types (`int`, `decimal`, `bool`, etc.) are configured as required, and all properties with nullable .NET value types (`int?`, `decimal?`, `bool?`, etc.) are configured as optional.</span></span>

<span data-ttu-id="52f68-109">C#in 8 wurde ein neues Feature namens " [Werte zulässt Reference Types](/dotnet/csharp/tutorials/nullable-reference-types)" eingeführt, mit dem Verweis Typen mit Anmerkungen versehen werden können. Dies gibt an, ob es zulässig ist, dass NULL-Werte enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="52f68-109">C# 8 introduced a new feature called [nullable reference types](/dotnet/csharp/tutorials/nullable-reference-types), which allows reference types to be annotated, indicating whether it is valid for them to contain null or not.</span></span> <span data-ttu-id="52f68-110">Diese Funktion ist standardmäßig deaktiviert. Wenn Sie aktiviert ist, ändert Sie das Verhalten der EF Core auf folgende Weise:</span><span class="sxs-lookup"><span data-stu-id="52f68-110">This feature is disabled by default, and if enabled, it modifies EF Core's behavior in the following way:</span></span>

* <span data-ttu-id="52f68-111">Wenn NULL-Werte zulässig sind (Standardeinstellung), werden alle Eigenschaften mit .net-Verweis Typen gemäß Konvention (z. b. `string`) als optional konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="52f68-111">If nullable reference types are disabled (the default), all properties with .NET reference types are configured as optional by convention (e.g. `string`).</span></span>
* <span data-ttu-id="52f68-112">Wenn Verweis Typen, die NULL-Werte zulassen, aktiviert sind, werden die C# Eigenschaften basierend auf der NULL-Zulässigkeit `string?` Ihres .net-Typs konfiguriert: wird `string` als optional konfiguriert, während als erforderlich konfiguriert wird.</span><span class="sxs-lookup"><span data-stu-id="52f68-112">If nullable reference types are enabled, properties will be configured based on the C# nullability of their .NET type: `string?` will be configured as optional, whereas `string` will be configured as required.</span></span>

<span data-ttu-id="52f68-113">Das folgende Beispiel zeigt einen Entitätstyp mit erforderlichen und optionalen Eigenschaften, wobei die Verweis Funktion NULL-Werte ist deaktiviert (Standard) und aktiviert ist:</span><span class="sxs-lookup"><span data-stu-id="52f68-113">The following example shows an entity type with required and optional properties, with the nullable reference feature disabled (the default) and enabled:</span></span>

# <a name="without-nullable-reference-types-defaulttabwithout-nrt"></a>[<span data-ttu-id="52f68-114">Ohne Verweis Typen, die NULL-Werte zulassen (Standard)</span><span class="sxs-lookup"><span data-stu-id="52f68-114">Without nullable reference types (default)</span></span>](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

# <a name="with-nullable-reference-typestabwith-nrt"></a>[<span data-ttu-id="52f68-115">Mit auf NULL festleg baren Verweis Typen</span><span class="sxs-lookup"><span data-stu-id="52f68-115">With nullable reference types</span></span>](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

<span data-ttu-id="52f68-116">Die Verwendung von Verweis Typen, die NULL-Werte zulassen, wird empfohlen, C# da Sie die im Code ausgedrückte NULL-Zulässigkeit in EF Core Modell und in der Datenbank überträgt und die Verwendung der flüssigen API oder Daten Anmerkungen zum doppelten Ausdrücken des gleichen Konzepts überflüssig macht.</span><span class="sxs-lookup"><span data-stu-id="52f68-116">Using nullable reference types is recommended since it flows the nullability expressed in C# code to EF Core's model and to the database, and obviates the use of the Fluent API or Data Annotations to express the same concept twice.</span></span>

> [!NOTE]
> <span data-ttu-id="52f68-117">Vorsicht beim Aktivieren von Verweis Typen, die NULL-Werte zulassen, für ein vorhandenes Projekt: Verweistyp Eigenschaften, die zuvor als optional konfiguriert wurden, werden nun als erforderlich konfiguriert, es sei denn, Sie sind explizit mit Anmerkungen versehen, die NULL-Werte zulassen.</span><span class="sxs-lookup"><span data-stu-id="52f68-117">Exercise caution when enabling nullable reference types on an existing project: reference type properties which were previously configured as optional will now be configured as required, unless they are explicitly annotated to be nullable.</span></span> <span data-ttu-id="52f68-118">Wenn Sie ein relationales Datenbankschema verwalten, kann dies dazu führen, dass Migrationen generiert werden, die die NULL-Zulässigkeit der Daten Bank Spalte ändern.</span><span class="sxs-lookup"><span data-stu-id="52f68-118">When managing a relational database schema, this may cause migrations to be generated which alter the database column's nullability.</span></span>

<span data-ttu-id="52f68-119">Weitere Informationen zu Verweis Typen, die NULL-Werte zulassen, und deren Verwendung mit EF Core finden Sie auf [der dedizierten Dokumentationsseite für dieses Feature](xref:core/miscellaneous/nullable-reference-types).</span><span class="sxs-lookup"><span data-stu-id="52f68-119">For more information on nullable reference types and how to use them with EF Core, [see the dedicated documentation page for this feature](xref:core/miscellaneous/nullable-reference-types).</span></span>

## <a name="configuration"></a><span data-ttu-id="52f68-120">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="52f68-120">Configuration</span></span>

<span data-ttu-id="52f68-121">Eine Eigenschaft, die gemäß der Konvention optional ist, kann so konfiguriert werden, dass Sie wie folgt erforderlich ist:</span><span class="sxs-lookup"><span data-stu-id="52f68-121">A property that would be optional by convention can be configured to be required as follows:</span></span>

# <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="52f68-122">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="52f68-122">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=14)]

# <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="52f68-123">Fließende API</span><span class="sxs-lookup"><span data-stu-id="52f68-123">Fluent API</span></span>](#tab/fluent-api) 

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=11-13)]

***