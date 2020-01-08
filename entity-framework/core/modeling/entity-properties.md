---
title: Entitäts Eigenschaften-EF Core
description: Konfigurieren und Zuordnen von Entitäts Eigenschaften mithilfe von Entity Framework Core
author: roji
ms.date: 12/10/2019
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/entity-properties
ms.openlocfilehash: b67603fbffd1f1c8506bc21f8972c851eb8eef29
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502426"
---
# <a name="entity-properties"></a><span data-ttu-id="a2306-103">Entitätseigenschaften</span><span class="sxs-lookup"><span data-stu-id="a2306-103">Entity Properties</span></span>

<span data-ttu-id="a2306-104">Jeder Entitätstyp in Ihrem Modell verfügt über eine Reihe von Eigenschaften, die EF Core aus der Datenbank lesen und schreiben.</span><span class="sxs-lookup"><span data-stu-id="a2306-104">Each entity type in your model has a set of properties, which EF Core will read and write from the database.</span></span> <span data-ttu-id="a2306-105">Wenn Sie eine relationale Datenbank verwenden, werden die Entitäts Eigenschaften Tabellen Spalten zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="a2306-105">If you're using a relational database, entity properties map to table columns.</span></span>

## <a name="included-and-excluded-properties"></a><span data-ttu-id="a2306-106">Enthaltene und ausgeschlossene Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="a2306-106">Included and excluded properties</span></span>

<span data-ttu-id="a2306-107">Gemäß der Konvention werden alle öffentlichen Eigenschaften mit einem Getter und einem Setter in das Modell eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="a2306-107">By convention, all public properties with a getter and a setter will be included in the model.</span></span>

<span data-ttu-id="a2306-108">Bestimmte Eigenschaften können wie folgt ausgeschlossen werden:</span><span class="sxs-lookup"><span data-stu-id="a2306-108">Specific properties can be excluded as follows:</span></span>

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="a2306-109">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="a2306-109">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?name=IgnoreProperty&highlight=6)]

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="a2306-110">Fließende API</span><span class="sxs-lookup"><span data-stu-id="a2306-110">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?name=IgnoreProperty&highlight=3,4)]

***

## <a name="column-names"></a><span data-ttu-id="a2306-111">Spaltennamen</span><span class="sxs-lookup"><span data-stu-id="a2306-111">Column names</span></span>

<span data-ttu-id="a2306-112">Gemäß der Konvention werden bei Verwendung einer relationalen Datenbank Entitäts Eigenschaften Tabellen Spalten mit dem gleichen Namen wie die Eigenschaft zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="a2306-112">By convention, when using a relational database, entity properties are mapped to table columns having the same name as the property.</span></span>

<span data-ttu-id="a2306-113">Wenn Sie Ihre Spalten lieber mit unterschiedlichen Namen konfigurieren möchten, können Sie dies wie folgt tun:</span><span class="sxs-lookup"><span data-stu-id="a2306-113">If you prefer to configure your columns with different names, you can do so as following:</span></span>

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="a2306-114">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="a2306-114">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnName.cs?Name=ColumnName&highlight=3)]

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="a2306-115">Fließende API</span><span class="sxs-lookup"><span data-stu-id="a2306-115">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnName.cs?Name=ColumnName&highlight=3-5)]

***

## <a name="column-data-types"></a><span data-ttu-id="a2306-116">Spaltendatentypen</span><span class="sxs-lookup"><span data-stu-id="a2306-116">Column data types</span></span>

<span data-ttu-id="a2306-117">Wenn Sie eine relationale Datenbank verwenden, wählt der Datenbankanbieter einen Datentyp aus, der auf dem .NET-Typ der Eigenschaft basiert.</span><span class="sxs-lookup"><span data-stu-id="a2306-117">When using a relational database, the database provider selects a data type based on the .NET type of the property.</span></span> <span data-ttu-id="a2306-118">Es berücksichtigt auch andere Metadaten, z. b. die konfigurierte [Maximale Länge](#maximum-length), ob die Eigenschaft Teil eines Primärschlüssels ist usw.</span><span class="sxs-lookup"><span data-stu-id="a2306-118">It also takes into account other metadata, such as the configured [maximum length](#maximum-length), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="a2306-119">SQL Server ordnet z. b. `DateTime` Eigenschaften `datetime2(7)` Spalten zu und `string` Eigenschaften `nvarchar(max)` Spalten (oder `nvarchar(450)` für Eigenschaften, die als Schlüssel verwendet werden).</span><span class="sxs-lookup"><span data-stu-id="a2306-119">For example, SQL Server maps `DateTime` properties to `datetime2(7)` columns, and `string` properties to `nvarchar(max)` columns (or to `nvarchar(450)` for properties that are used as a key).</span></span>

<span data-ttu-id="a2306-120">Sie können auch die Spalten so konfigurieren, dass Sie einen exakten Datentyp für eine Spalte angeben.</span><span class="sxs-lookup"><span data-stu-id="a2306-120">You can also configure your columns to specify an exact data type for a column.</span></span> <span data-ttu-id="a2306-121">Der folgende Code konfiguriert z. b. `Url` als nicht-Unicode-Zeichenfolge mit maximaler Länge von `200` und `Rating` als Dezimal Länge mit der Genauigkeit `5` und der `2`Skala:</span><span class="sxs-lookup"><span data-stu-id="a2306-121">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`:</span></span>

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="a2306-122">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="a2306-122">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnDataType.cs?name=ColumnDataType&highlight=4,6)]

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="a2306-123">Fließende API</span><span class="sxs-lookup"><span data-stu-id="a2306-123">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnDataType.cs?name=ColumnDataType&highlight=5-6)]

***

### <a name="maximum-length"></a><span data-ttu-id="a2306-124">Maximale Länge</span><span class="sxs-lookup"><span data-stu-id="a2306-124">Maximum length</span></span>

<span data-ttu-id="a2306-125">Das Konfigurieren einer maximalen Länge gibt dem Datenbankanbieter einen Hinweis zum entsprechenden Spaltendatentyp, der für eine bestimmte Eigenschaft ausgewählt werden soll.</span><span class="sxs-lookup"><span data-stu-id="a2306-125">Configuring a maximum length provides a hint to the database provider about the appropriate column data type to choose for a given property.</span></span> <span data-ttu-id="a2306-126">Die maximale Länge gilt nur für Array Datentypen, z. b. `string` und `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="a2306-126">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]
> <span data-ttu-id="a2306-127">Entity Framework führt keine Überprüfung der maximalen Länge durch, bevor Daten an den Anbieter übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="a2306-127">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="a2306-128">Der Anbieter oder Datenspeicher muss ggf. überprüft werden.</span><span class="sxs-lookup"><span data-stu-id="a2306-128">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="a2306-129">Wenn Sie z. b. auf SQL Server abzielen, führt das Überschreiten der maximalen Länge zu einer Ausnahme, da der Datentyp der zugrunde liegenden Spalte nicht zulässt, dass überschüssige Daten gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="a2306-129">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

<span data-ttu-id="a2306-130">Im folgenden Beispiel bewirkt das Konfigurieren einer maximalen Länge von 500, dass eine Spalte vom Typ "`nvarchar(500)`" in SQL Server erstellt wird:</span><span class="sxs-lookup"><span data-stu-id="a2306-130">In the following example, configuring a maximum length of 500 will cause a column of type `nvarchar(500)` to be created on SQL Server:</span></span>

#### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="a2306-131">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="a2306-131">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?name=MaxLength&highlight=4)]

#### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="a2306-132">Fließende API</span><span class="sxs-lookup"><span data-stu-id="a2306-132">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?name=MaxLength&highlight=3-5)]

***

## <a name="required-and-optional-properties"></a><span data-ttu-id="a2306-133">Erforderliche und optionale Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="a2306-133">Required and optional properties</span></span>

<span data-ttu-id="a2306-134">Eine Eigenschaft wird als optional eingestuft, wenn Sie `null`enthalten soll.</span><span class="sxs-lookup"><span data-stu-id="a2306-134">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="a2306-135">Wenn `null` kein gültiger Wert ist, der einer Eigenschaft zugewiesen werden soll, wird er als erforderliche Eigenschaft betrachtet.</span><span class="sxs-lookup"><span data-stu-id="a2306-135">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span> <span data-ttu-id="a2306-136">Bei der Zuordnung zu einem relationalen Datenbankschema werden erforderliche Eigenschaften als Spalten erstellt, die keine NULL-Werte zulassen, und optionale Eigenschaften werden als Spalten erstellt, die NULL-Werte zulassen.</span><span class="sxs-lookup"><span data-stu-id="a2306-136">When mapping to a relational database schema, required properties are created as non-nullable columns, and optional properties are created as nullable columns.</span></span>

### <a name="conventions"></a><span data-ttu-id="a2306-137">Konventionen</span><span class="sxs-lookup"><span data-stu-id="a2306-137">Conventions</span></span>

<span data-ttu-id="a2306-138">Gemäß der Konvention wird eine Eigenschaft, deren .NET-Typ NULL enthalten kann, als optional konfiguriert, wohingegen Eigenschaften, deren .NET-Typ keinen NULL-Wert enthalten darf, als erforderlich konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="a2306-138">By convention, a property whose .NET type can contain null will be configured as optional, whereas properties whose .NET type cannot contain null will be configured as required.</span></span> <span data-ttu-id="a2306-139">Beispielsweise werden alle Eigenschaften mit .net-Werttypen (`int`, `decimal`, `bool`usw.) als erforderlich konfiguriert, und alle Eigenschaften mit .net-Werttypen, die NULL-Werte zulassen (`int?`, `decimal?`, `bool?`usw.), werden als optional konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="a2306-139">For example, all properties with .NET value types (`int`, `decimal`, `bool`, etc.) are configured as required, and all properties with nullable .NET value types (`int?`, `decimal?`, `bool?`, etc.) are configured as optional.</span></span>

<span data-ttu-id="a2306-140">C#in 8 wurde ein neues Feature namens " [Werte zulässt Reference Types](/dotnet/csharp/tutorials/nullable-reference-types)" eingeführt, mit dem Verweis Typen mit Anmerkungen versehen werden können. Dies gibt an, ob es zulässig ist, dass NULL-Werte enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="a2306-140">C# 8 introduced a new feature called [nullable reference types](/dotnet/csharp/tutorials/nullable-reference-types), which allows reference types to be annotated, indicating whether it is valid for them to contain null or not.</span></span> <span data-ttu-id="a2306-141">Diese Funktion ist standardmäßig deaktiviert. Wenn Sie aktiviert ist, ändert Sie das Verhalten der EF Core auf folgende Weise:</span><span class="sxs-lookup"><span data-stu-id="a2306-141">This feature is disabled by default, and if enabled, it modifies EF Core's behavior in the following way:</span></span>

* <span data-ttu-id="a2306-142">Wenn NULL-Werte zulassen (Standardeinstellung), werden alle Eigenschaften mit .net-Verweis Typen gemäß der Konvention (z. b. `string`) als optional konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="a2306-142">If nullable reference types are disabled (the default), all properties with .NET reference types are configured as optional by convention (e.g. `string`).</span></span>
* <span data-ttu-id="a2306-143">Wenn Verweis Typen, die NULL-Werte zulassen, aktiviert sind, werden die C# Eigenschaften basierend auf der NULL-Zulässigkeit ihres .net-Typs konfiguriert: `string?` als optional konfiguriert werden, während `string` als erforderlich konfiguriert wird.</span><span class="sxs-lookup"><span data-stu-id="a2306-143">If nullable reference types are enabled, properties will be configured based on the C# nullability of their .NET type: `string?` will be configured as optional, whereas `string` will be configured as required.</span></span>

<span data-ttu-id="a2306-144">Das folgende Beispiel zeigt einen Entitätstyp mit erforderlichen und optionalen Eigenschaften, wobei die Verweis Funktion NULL-Werte ist deaktiviert (Standard) und aktiviert ist:</span><span class="sxs-lookup"><span data-stu-id="a2306-144">The following example shows an entity type with required and optional properties, with the nullable reference feature disabled (the default) and enabled:</span></span>

#### <a name="without-nullable-reference-types-defaulttabwithout-nrt"></a>[<span data-ttu-id="a2306-145">Ohne Verweis Typen, die NULL-Werte zulassen (Standard)</span><span class="sxs-lookup"><span data-stu-id="a2306-145">Without nullable reference types (default)</span></span>](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

#### <a name="with-nullable-reference-typestabwith-nrt"></a>[<span data-ttu-id="a2306-146">Mit auf NULL festleg baren Verweis Typen</span><span class="sxs-lookup"><span data-stu-id="a2306-146">With nullable reference types</span></span>](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

<span data-ttu-id="a2306-147">Die Verwendung von Verweis Typen, die NULL-Werte zulassen, wird empfohlen, C# da Sie die im Code ausgedrückte NULL-Zulässigkeit in EF Core Modell und in der Datenbank überträgt und die Verwendung der flüssigen API oder Daten Anmerkungen zum doppelten Ausdrücken des gleichen Konzepts überflüssig macht.</span><span class="sxs-lookup"><span data-stu-id="a2306-147">Using nullable reference types is recommended since it flows the nullability expressed in C# code to EF Core's model and to the database, and obviates the use of the Fluent API or Data Annotations to express the same concept twice.</span></span>

> [!NOTE]
> <span data-ttu-id="a2306-148">Vorsicht beim Aktivieren von Verweis Typen, die NULL-Werte zulassen, für ein vorhandenes Projekt: Verweistyp Eigenschaften, die zuvor als optional konfiguriert wurden, werden nun als erforderlich konfiguriert, es sei denn, Sie sind explizit mit Anmerkungen versehen, die NULL-Werte zulassen.</span><span class="sxs-lookup"><span data-stu-id="a2306-148">Exercise caution when enabling nullable reference types on an existing project: reference type properties which were previously configured as optional will now be configured as required, unless they are explicitly annotated to be nullable.</span></span> <span data-ttu-id="a2306-149">Wenn Sie ein relationales Datenbankschema verwalten, kann dies dazu führen, dass Migrationen generiert werden, die die NULL-Zulässigkeit der Daten Bank Spalte ändern.</span><span class="sxs-lookup"><span data-stu-id="a2306-149">When managing a relational database schema, this may cause migrations to be generated which alter the database column's nullability.</span></span>

<span data-ttu-id="a2306-150">Weitere Informationen zu Verweis Typen, die NULL-Werte zulassen, und deren Verwendung mit EF Core finden Sie auf [der dedizierten Dokumentationsseite für dieses Feature](xref:core/miscellaneous/nullable-reference-types).</span><span class="sxs-lookup"><span data-stu-id="a2306-150">For more information on nullable reference types and how to use them with EF Core, [see the dedicated documentation page for this feature](xref:core/miscellaneous/nullable-reference-types).</span></span>

### <a name="explicit-configuration"></a><span data-ttu-id="a2306-151">Explizite Konfiguration</span><span class="sxs-lookup"><span data-stu-id="a2306-151">Explicit configuration</span></span>

<span data-ttu-id="a2306-152">Eine Eigenschaft, die gemäß der Konvention optional ist, kann so konfiguriert werden, dass Sie wie folgt erforderlich ist:</span><span class="sxs-lookup"><span data-stu-id="a2306-152">A property that would be optional by convention can be configured to be required as follows:</span></span>

#### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="a2306-153">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="a2306-153">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?name=Required&highlight=4)]

#### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="a2306-154">Fließende API</span><span class="sxs-lookup"><span data-stu-id="a2306-154">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?name=Required&highlight=3-5)]

***
