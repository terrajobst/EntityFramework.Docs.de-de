---
title: Entitätstypen - EF Core im Besitz
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: b2d72b08de79939904bf4e726c695440c906a8aa
ms.sourcegitcommit: 7bde8e6ad3c4565a4638646ce04bcf5e66f7b5fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/07/2019
ms.locfileid: "54069203"
---
# <a name="owned-entity-types"></a><span data-ttu-id="c7389-102">Entitätstypen im Besitz</span><span class="sxs-lookup"><span data-stu-id="c7389-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="c7389-103">Dieses Feature ist neu in EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="c7389-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="c7389-104">EF Core können Sie die Modell-Entitätstypen, die nur für Navigationseigenschaften von anderen Entitätstypen angezeigt werden können.</span><span class="sxs-lookup"><span data-stu-id="c7389-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="c7389-105">Diese heißen _Entitätstypen im Besitz_.</span><span class="sxs-lookup"><span data-stu-id="c7389-105">These are called _owned entity types_.</span></span> <span data-ttu-id="c7389-106">Die Entität, die einen eigener Entitätstyp enthält, ist die _Besitzer_.</span><span class="sxs-lookup"><span data-stu-id="c7389-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="c7389-107">Explizite Konfiguration</span><span class="sxs-lookup"><span data-stu-id="c7389-107">Explicit configuration</span></span>

<span data-ttu-id="c7389-108">Im Besitz Entität, die Typen nicht gemäß der Konvention von EF Core im Modell enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="c7389-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="c7389-109">Sie können die `OwnsOne` -Methode in der `OnModelCreating` oder fügen Sie den Typ mit `OwnedAttribute` (neu in EF Core 2.1) so konfigurieren Sie den Typ als ein eigener Typ.</span><span class="sxs-lookup"><span data-stu-id="c7389-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="c7389-110">In diesem Beispiel `StreetAddress` ist ein Typ ohne Identitätseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="c7389-110">In this example, `StreetAddress` is a type with no identity property.</span></span> <span data-ttu-id="c7389-111">Dieser Typ wird als Eigenschaft des Typs „Order“ verwendet, um die Lieferadresse für eine bestimmte Bestellung anzugeben.</span><span class="sxs-lookup"><span data-stu-id="c7389-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span>

<span data-ttu-id="c7389-112">Wir können die `OwnedAttribute` , dies als eine Entität im Besitz, wenn auf die verwiesen wird von einem anderen Entitätstyp behandeln:</span><span class="sxs-lookup"><span data-stu-id="c7389-112">We can use the `OwnedAttribute` to treat it as an owned entity when referenced from another entity type:</span></span>

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

<span data-ttu-id="c7389-113">Es ist auch möglich, mit der `OwnsOne` -Methode in der `OnModelCreating` an, dass die `ShippingAddress` -Eigenschaft ist eine Entität im Besitz von der `Order` Entitätstyp und bei Bedarf zusätzliche Facetten zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="c7389-113">It is also possible to use the `OwnsOne` method in `OnModelCreating` to specify that the `ShippingAddress` property is an Owned Entity of the `Order` entity type and to configure additional facets if needed.</span></span>

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

<span data-ttu-id="c7389-114">Wenn die `ShippingAddress` -Eigenschaft ist privat in der `Order` geben, können Sie die Version der Zeichenfolge die `OwnsOne` Methode:</span><span class="sxs-lookup"><span data-stu-id="c7389-114">If the `ShippingAddress` property is private in the `Order` type, you can use the string version of the `OwnsOne` method:</span></span>

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

<span data-ttu-id="c7389-115">Finden Sie unter den [vollständigen Beispielprojekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) für weiteren Kontext.</span><span class="sxs-lookup"><span data-stu-id="c7389-115">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) for more context.</span></span> 

## <a name="implicit-keys"></a><span data-ttu-id="c7389-116">Implizite Schlüssel</span><span class="sxs-lookup"><span data-stu-id="c7389-116">Implicit keys</span></span>

<span data-ttu-id="c7389-117">Eigene Typen, die mit konfigurierten `OwnsOne` oder über eine Reference-Navigation ermittelt haben immer eine direkte Beziehung mit dem Besitzer, die aus diesem Grund nicht sie ihre eigenen Werte erforderlich, da die foreign Key-Werte eindeutig sind.</span><span class="sxs-lookup"><span data-stu-id="c7389-117">Owned types configured with `OwnsOne` or discovered through a reference navigation always have a one-to-one relationship with the owner, therefore they don't need their own key values as the foreign key values are unique.</span></span> <span data-ttu-id="c7389-118">Im vorherigen Beispiel die `StreetAddress` Typ muss es sich nicht um eine wichtige Eigenschaft zu definieren.</span><span class="sxs-lookup"><span data-stu-id="c7389-118">In the previous example, the `StreetAddress` type does not need to define a key property.</span></span>  

<span data-ttu-id="c7389-119">Um zu verstehen, wie diese Objekte von EF Core nachverfolgt wird, ist es sinnvoll, die glauben, dass ein Primärschlüssel, als erstellt wird eine [schatteneigenschaft](xref:core/modeling/shadow-properties) für den eigenen Typ.</span><span class="sxs-lookup"><span data-stu-id="c7389-119">In order to understand how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="c7389-120">Der Wert des Schlüssels für eine Instanz des eigenen Typs wird der Wert des Schlüssels für die Besitzer-Instanz identisch sein.</span><span class="sxs-lookup"><span data-stu-id="c7389-120">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>

## <a name="collections-of-owned-types"></a><span data-ttu-id="c7389-121">Auflistungen von eigenen Typen</span><span class="sxs-lookup"><span data-stu-id="c7389-121">Collections of owned types</span></span>

>[!NOTE]
> <span data-ttu-id="c7389-122">Dieses Feature ist neu in EF Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="c7389-122">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="c7389-123">So konfigurieren Sie eine Auflistung von Typen im Besitz `OwnsMany` sollte verwendet werden, `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="c7389-123">To configure a collection of owned types `OwnsMany` should be used in `OnModelCreating`.</span></span> <span data-ttu-id="c7389-124">Der Primärschlüssel wird jedoch nicht gemäß der Konvention konfiguriert werden, muss er explizit angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="c7389-124">However the primary key will not be configured by convention, so it needs to be specified explicitly.</span></span> <span data-ttu-id="c7389-125">Es ist üblich, verwenden Sie einen komplexen Schlüssel für diese Art von Entitäten, die Integration des Fremdschlüssels für den Besitzer und eine weitere eindeutige Eigenschaft, die auch Schatten Status haben kann:</span><span class="sxs-lookup"><span data-stu-id="c7389-125">It is common to use a complex key for these type of entities incorporating the foreign key to the owner and an additional unique property that can also be in shadow state:</span></span>

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="c7389-126">Zuordnung, die im Besitz von Typen mit tabellenaufteilung</span><span class="sxs-lookup"><span data-stu-id="c7389-126">Mapping owned types with table splitting</span></span>

<span data-ttu-id="c7389-127">Verwendung von relationalen eigene Datenbanken, als Konvention Verweis als Besitzer der gleichen Tabelle zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="c7389-127">When using relational databases, by convention reference owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="c7389-128">Dies erfordert in der Tabelle in zwei Teilen: einige Spalten zum Speichern der Daten des Besitzers verwendet werden, und einige Spalten zum Speichern von Daten der Entität im Besitz des Benutzers verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="c7389-128">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="c7389-129">Dies ist eine Standardfunktion in tabellenaufteilung genannt.</span><span class="sxs-lookup"><span data-stu-id="c7389-129">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="c7389-130">Besitzer des Typen, die mit der tabellenaufteilung gespeichert sein können auf ähnliche Weise verwendet wird, wie komplexe Typen in EF 6 verwendeten werden.</span><span class="sxs-lookup"><span data-stu-id="c7389-130">Owned types stored with table splitting can be used similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="c7389-131">Gemäß der Konvention wird EF Core die Datenbankspalten für die Eigenschaften der eigene Entitätstyp mit dem folgenden Format benennen _Navigation_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="c7389-131">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _Navigation_OwnedEntityProperty_.</span></span> <span data-ttu-id="c7389-132">Aus diesem Grund die `StreetAddress` Eigenschaften werden in der Tabelle "Orders" mit dem Namen 'ShippingAddress_Street' und "ShippingAddress_City" angezeigt.</span><span class="sxs-lookup"><span data-stu-id="c7389-132">Therefore the `StreetAddress` properties will appear in the 'Orders' table with the names 'ShippingAddress_Street' and 'ShippingAddress_City'.</span></span>

<span data-ttu-id="c7389-133">Sie können den Anfügen der `HasColumnName` Methode, um diese Spalten umzubenennen:</span><span class="sxs-lookup"><span data-stu-id="c7389-133">You can append the `HasColumnName` method to rename those columns:</span></span>

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="c7389-134">Freigabe desselben Typs .NET für mehrere im Besitz des Benutzers</span><span class="sxs-lookup"><span data-stu-id="c7389-134">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="c7389-135">Ein eigener Entitätstyp möglich des gleichen Typs .NET als einem anderen eigenen Entitätstyp, daher, dass der .NET Typ möglicherweise nicht mit ausreichend, um ein eigener Typ zu identifizieren.</span><span class="sxs-lookup"><span data-stu-id="c7389-135">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="c7389-136">In diesen Fällen wird die Eigenschaft, die auf die Entität im Besitz des Besitzers der _Definieren der Navigation_ des eigenen Entitätstyps.</span><span class="sxs-lookup"><span data-stu-id="c7389-136">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="c7389-137">Aus der Perspektive von EF Core ist die definierende Navigation Teil der Identität für den Typ zusammen mit der .NET-Typ.</span><span class="sxs-lookup"><span data-stu-id="c7389-137">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="c7389-138">Z. B. in der folgenden Klasse `ShippingAddress` und `BillingAddress` sind beide des gleichen Typs .NET `StreetAddress`:</span><span class="sxs-lookup"><span data-stu-id="c7389-138">For example, in the following class `ShippingAddress` and `BillingAddress` are both of the same .NET type, `StreetAddress`:</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="c7389-139">Um zu verstehen, wie EF Core für überwachte Instanzen dieser Objekte unterscheiden, wird, kann es hilfreich sein zu denken, dass die definierende Navigation Teil des Schlüssels der Instanz zusammen mit dem Wert des Schlüssels des Besitzers und der .NET-Typ des eigenen Typs geworden ist.</span><span class="sxs-lookup"><span data-stu-id="c7389-139">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="c7389-140">Geschachtelte eigene Typen</span><span class="sxs-lookup"><span data-stu-id="c7389-140">Nested owned types</span></span>

<span data-ttu-id="c7389-141">In diesem Beispiel `OrderDetails` besitzt `BillingAddress` und `ShippingAddress`, sind die `StreetAddress` Typen.</span><span class="sxs-lookup"><span data-stu-id="c7389-141">In this example `OrderDetails` owns `BillingAddress` and `ShippingAddress`, which are both `StreetAddress` types.</span></span> <span data-ttu-id="c7389-142">Dann besitzt der `DetailedOrder`-Typ `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="c7389-142">Then `OrderDetails` is owned by the `DetailedOrder` type.</span></span>

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

<span data-ttu-id="c7389-143">Zusätzlich zu den geschachtelten Typen im Besitz des Benutzers ein eigener Typ kann eine reguläre Entität verweisen, kann es sein entweder der Besitzer oder eine andere Entität, solange die Entität im Besitz des Benutzers auf die abhängige Seite ist.</span><span class="sxs-lookup"><span data-stu-id="c7389-143">In addition to nested owned types, an owned type can reference a regular entity, it can be either the owner or a different entity as long as the owned entity is on the dependent side.</span></span> <span data-ttu-id="c7389-144">Diese Funktion legt die Entitätstypen, die abgesehen von komplexen Typen im Besitz in EF 6 fest.</span><span class="sxs-lookup"><span data-stu-id="c7389-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="c7389-145">Es ist möglich, eine Verkettung der `OwnsOne` -Methode in einer fluent-Aufruf so konfigurieren Sie dieses Modell:</span><span class="sxs-lookup"><span data-stu-id="c7389-145">It is possible to chain the `OwnsOne` method in a fluent call to configure this model:</span></span>

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

<span data-ttu-id="c7389-146">Es ist auch möglich, die gleiche Sache mit `OwnedAttribute` sowohl `OrderDetails` und `StreetAdress`.</span><span class="sxs-lookup"><span data-stu-id="c7389-146">It is also possible to achieve the same thing using `OwnedAttribute` on both `OrderDetails` and `StreetAdress`.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="c7389-147">Das Speichern von eigene Typen in separaten Tabellen</span><span class="sxs-lookup"><span data-stu-id="c7389-147">Storing owned types in separate tables</span></span>

<span data-ttu-id="c7389-148">Im Gegensatz zu EF6 komplexe Typen können auch eigene Typen in einer separaten Tabelle vom Besitzer gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="c7389-148">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="c7389-149">Sie können einfach aufrufen, um die Konvention überschreiben, der einen eigenen Typ als Besitzer der gleichen Tabelle zugeordnet, `ToTable` , und geben Sie einen anderen Tabellennamen an.</span><span class="sxs-lookup"><span data-stu-id="c7389-149">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="c7389-150">Im folgende Beispiel ordnet `OrderDetails` und die zwei Adressen in eine separate Tabelle von `DetailedOrder`:</span><span class="sxs-lookup"><span data-stu-id="c7389-150">The following example will map `OrderDetails` and its two addresses to a separate table from `DetailedOrder`:</span></span>

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a><span data-ttu-id="c7389-151">Abfragen von Typen im Besitz</span><span class="sxs-lookup"><span data-stu-id="c7389-151">Querying owned types</span></span>

<span data-ttu-id="c7389-152">Beim Abfragen des Besitzers werden standardmäßig eigene Typen eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="c7389-152">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="c7389-153">Es ist nicht erforderlich, verwenden die `Include` -Methode, selbst wenn Sie eigenen Typen in einer separaten Tabelle gespeichert sind.</span><span class="sxs-lookup"><span data-stu-id="c7389-153">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="c7389-154">Auf der Grundlage des Modells beschrieben vor, die folgende Abfrage erhalten `Order`, `OrderDetails` und die beiden gehören `StreetAddresses` aus der Datenbank:</span><span class="sxs-lookup"><span data-stu-id="c7389-154">Based on the model described before, the following query will get `Order`, `OrderDetails` and the two owned `StreetAddresses` from the database:</span></span>

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a><span data-ttu-id="c7389-155">Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="c7389-155">Limitations</span></span>

<span data-ttu-id="c7389-156">Einige dieser Einschränkungen ist die Grundlage wie die im Besitz des Benutzers Entität Typen arbeiten, aber einige andere werden Einschränkungen, die wir in zukünftigen Versionen entfernt werden kann:</span><span class="sxs-lookup"><span data-stu-id="c7389-156">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="c7389-157">Standardmäßig Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="c7389-157">By-design restrictions</span></span>
- <span data-ttu-id="c7389-158">Sie können nicht erstellt werden ein `DbSet<T>` für einen Typ im Besitz des Benutzers</span><span class="sxs-lookup"><span data-stu-id="c7389-158">You cannot create a `DbSet<T>` for an owned type</span></span>
- <span data-ttu-id="c7389-159">Sie können nicht aufrufen `Entity<T>()` mit einem Datentyp auf, die im Besitz des Benutzers `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="c7389-159">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="c7389-160">Aktuelle Mängel</span><span class="sxs-lookup"><span data-stu-id="c7389-160">Current shortcomings</span></span>
- <span data-ttu-id="c7389-161">Vererbungshierarchien, die enthalten, das im Besitz, Entitätstypen werden nicht unterstützt</span><span class="sxs-lookup"><span data-stu-id="c7389-161">Inheritance hierarchies that include owned entity types are not supported</span></span>
- <span data-ttu-id="c7389-162">Referenz-Navigationen hinweg, um eigene Entitätstypen darf nicht null sein, es sei denn, sie explizit in eine separate Tabelle vom Besitzer zugeordnet sind</span><span class="sxs-lookup"><span data-stu-id="c7389-162">Reference navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span>
- <span data-ttu-id="c7389-163">Instanzen von Entitätstypen im Besitz des Benutzers können nicht von mehreren Besitzern gemeinsam genutzt werden (Dies ist ein bekanntes Szenario für die Value-Objekte, die mithilfe von Entitätstypen im Besitz nicht implementiert werden kann.)</span><span class="sxs-lookup"><span data-stu-id="c7389-163">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="c7389-164">Mängel in früheren Versionen</span><span class="sxs-lookup"><span data-stu-id="c7389-164">Shortcomings in previous versions</span></span>
- <span data-ttu-id="c7389-165">In EF Core 2.0 gehören zu, dass die Entitätstypen können nicht in abgeleiteten Entitätstypen deklariert werden, es sei denn, die eigenen Entitäten explizit in eine separate Tabelle aus der Besitzerhierarchie zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="c7389-165">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="c7389-166">Diese Einschränkung wurde in EF Core 2.1 entfernt</span><span class="sxs-lookup"><span data-stu-id="c7389-166">This limitation has been removed in EF Core 2.1</span></span>
- <span data-ttu-id="c7389-167">In EF Core 2.0 und 2.1 nur Verweis wurden zu eigene Typen unterstützt.</span><span class="sxs-lookup"><span data-stu-id="c7389-167">In EF Core 2.0 and 2.1 only reference navigations to owned types were supported.</span></span> <span data-ttu-id="c7389-168">Diese Einschränkung wurde in EF Core 2.2 entfernt</span><span class="sxs-lookup"><span data-stu-id="c7389-168">This limitation has been removed in EF Core 2.2</span></span>
