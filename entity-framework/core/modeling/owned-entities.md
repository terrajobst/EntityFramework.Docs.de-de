---
title: Eigene Entitäts Typen-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: a0665bfa27134b8dc3eba854ff3f7b1af4b69217
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655936"
---
# <a name="owned-entity-types"></a><span data-ttu-id="424ce-102">Nicht eigenständige Entitätstypen</span><span class="sxs-lookup"><span data-stu-id="424ce-102">Owned Entity Types</span></span>

> [!NOTE]
> <span data-ttu-id="424ce-103">Diese Funktion ist neu in EF Core 2,0.</span><span class="sxs-lookup"><span data-stu-id="424ce-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="424ce-104">EF Core ermöglicht es Ihnen, Entitäts Typen zu modellieren, die nur in den Navigations Eigenschaften anderer Entitäts Typen angezeigt werden können.</span><span class="sxs-lookup"><span data-stu-id="424ce-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="424ce-105">Diese werden als _eigene Entitäts Typen_bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="424ce-105">These are called _owned entity types_.</span></span> <span data-ttu-id="424ce-106">Die Entität, die einen eigenen Entitätstyp enthält, ist Ihr _Besitzer_.</span><span class="sxs-lookup"><span data-stu-id="424ce-106">The entity containing an owned entity type is its _owner_.</span></span>

<span data-ttu-id="424ce-107">Besitzende Entitäten sind im wesentlichen Teil des Besitzers und können nicht ohne Sie vorhanden sein, Sie sind konzeptionell ähnlich wie [Aggregate](https://martinfowler.com/bliki/DDD_Aggregate.html).</span><span class="sxs-lookup"><span data-stu-id="424ce-107">Owned entities are essentially a part of the owner and cannot exist without it, they are conceptually similar to [aggregates](https://martinfowler.com/bliki/DDD_Aggregate.html).</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="424ce-108">Explizite Konfiguration</span><span class="sxs-lookup"><span data-stu-id="424ce-108">Explicit configuration</span></span>

<span data-ttu-id="424ce-109">Eigene Entitäts Typen werden nie von EF Core im Modell nach Konvention eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="424ce-109">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="424ce-110">Sie können die `OwnsOne`-Methode in `OnModelCreating` verwenden oder den Typ mit `OwnedAttribute` (neu in EF Core 2,1) kommentieren, um den Typ als eigenen Typ zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="424ce-110">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="424ce-111">In diesem Beispiel ist `StreetAddress` ein Typ ohne Identity-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="424ce-111">In this example, `StreetAddress` is a type with no identity property.</span></span> <span data-ttu-id="424ce-112">Dieser Typ wird als Eigenschaft des Typs „Order“ verwendet, um die Lieferadresse für eine bestimmte Bestellung anzugeben.</span><span class="sxs-lookup"><span data-stu-id="424ce-112">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span>

<span data-ttu-id="424ce-113">Sie können den `OwnedAttribute` verwenden, um ihn als eigene Entität zu behandeln, wenn er von einem anderen Entitätstyp referenziert wird:</span><span class="sxs-lookup"><span data-stu-id="424ce-113">We can use the `OwnedAttribute` to treat it as an owned entity when referenced from another entity type:</span></span>

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

<span data-ttu-id="424ce-114">Es ist auch möglich, die `OwnsOne`-Methode in `OnModelCreating` zu verwenden, um anzugeben, dass die `ShippingAddress`-Eigenschaft eine eigene Entität des `Order` Entitäts Typs ist, und um ggf. weitere Facetten zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="424ce-114">It is also possible to use the `OwnsOne` method in `OnModelCreating` to specify that the `ShippingAddress` property is an Owned Entity of the `Order` entity type and to configure additional facets if needed.</span></span>

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

<span data-ttu-id="424ce-115">Wenn die `ShippingAddress`-Eigenschaft im `Order`-Typ privat ist, können Sie die Zeichen folgen Version der `OwnsOne`-Methode verwenden:</span><span class="sxs-lookup"><span data-stu-id="424ce-115">If the `ShippingAddress` property is private in the `Order` type, you can use the string version of the `OwnsOne` method:</span></span>

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

<span data-ttu-id="424ce-116">Weitere Informationen finden Sie im [vollständigen Beispiel Projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) .</span><span class="sxs-lookup"><span data-stu-id="424ce-116">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) for more context.</span></span>

## <a name="implicit-keys"></a><span data-ttu-id="424ce-117">Implizite Schlüssel</span><span class="sxs-lookup"><span data-stu-id="424ce-117">Implicit keys</span></span>

<span data-ttu-id="424ce-118">Besitzer Typen, die mit `OwnsOne` konfiguriert oder über eine Verweis Navigation ermittelt wurden, verfügen immer über eine 1:1-Beziehung mit dem Besitzer und benötigen daher keine eigenen Schlüsselwerte, da die Fremdschlüssel Werte eindeutig sind.</span><span class="sxs-lookup"><span data-stu-id="424ce-118">Owned types configured with `OwnsOne` or discovered through a reference navigation always have a one-to-one relationship with the owner, therefore they don't need their own key values as the foreign key values are unique.</span></span> <span data-ttu-id="424ce-119">Im vorherigen Beispiel muss der `StreetAddress` Typ keine Schlüsseleigenschaft definieren.</span><span class="sxs-lookup"><span data-stu-id="424ce-119">In the previous example, the `StreetAddress` type does not need to define a key property.</span></span>  

<span data-ttu-id="424ce-120">Um zu verstehen, wie EF Core diese Objekte verfolgt, ist es hilfreich zu wissen, dass ein Primärschlüssel als [Schatten Eigenschaft](xref:core/modeling/shadow-properties) für den eigenen Typ erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="424ce-120">In order to understand how EF Core tracks these objects, it is useful to know that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="424ce-121">Der Wert des Schlüssels einer Instanz des eigenen Typs ist mit dem Wert des Schlüssels der Besitzer Instanz identisch.</span><span class="sxs-lookup"><span data-stu-id="424ce-121">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>

## <a name="collections-of-owned-types"></a><span data-ttu-id="424ce-122">Auflistungen von eigenen Typen</span><span class="sxs-lookup"><span data-stu-id="424ce-122">Collections of owned types</span></span>

> [!NOTE]
> <span data-ttu-id="424ce-123">Dieses Feature ist neu in EF Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="424ce-123">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="424ce-124">Verwenden Sie `OwnsMany` in `OnModelCreating`, um eine Sammlung von eigenen Typen zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="424ce-124">To configure a collection of owned types use `OwnsMany` in `OnModelCreating`.</span></span>

<span data-ttu-id="424ce-125">Für eigene Typen ist ein Primärschlüssel erforderlich.</span><span class="sxs-lookup"><span data-stu-id="424ce-125">Owned types need a primary key.</span></span> <span data-ttu-id="424ce-126">Wenn für den .NET-Typ keine guten Kandidaten Eigenschaften vorhanden sind, können EF Core versuchen, eine zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="424ce-126">If there are no good candidates properties on the .NET type, EF Core can try to create one.</span></span> <span data-ttu-id="424ce-127">Wenn jedoch eigene Typen durch eine Auflistung definiert werden, reicht es nicht aus, eine Schatten Eigenschaft zu erstellen, die sowohl als Fremdschlüssel für den Besitzer als auch als Primärschlüssel der eigenen Instanz fungiert, wie es bei `OwnsOne`der Fall ist: Es können mehrere eigene Typinstanzen für jede der Besitzer. Daher reicht der Schlüssel des Besitzers nicht aus, um eine eindeutige Identität für jede eigene Instanz bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="424ce-127">However, when owned types are defined through a collection, it isn't enough to just create a shadow property to act as both the foreign key into the owner and the primary key of the owned instance, as we do for `OwnsOne`: there can be multiple owned type instances for each owner, and hence the key of the owner isn't enough to provide a unique identity for each owned instance.</span></span>

<span data-ttu-id="424ce-128">Die beiden einfachsten Lösungen hierfür sind:</span><span class="sxs-lookup"><span data-stu-id="424ce-128">The two most straightforward solutions to this are:</span></span>

- <span data-ttu-id="424ce-129">Definieren eines Ersatz Primärschlüssels für eine neue Eigenschaft, unabhängig vom Fremdschlüssel, der auf den Besitzer zeigt.</span><span class="sxs-lookup"><span data-stu-id="424ce-129">Defining a surrogate primary key on a new property independent of the foreign key that points to the owner.</span></span> <span data-ttu-id="424ce-130">Die enthaltenen Werte müssen für alle Besitzer eindeutig sein (z. b. wenn die übergeordnete {1} über untergeordnete {1}verfügen, die übergeordnete {2} nicht über untergeordnete {1}verfügen), sodass der Wert keine inhärente Bedeutung hat.</span><span class="sxs-lookup"><span data-stu-id="424ce-130">The contained values would need to be unique across all owners (e.g. if Parent {1} has Child {1}, then Parent {2} cannot have Child {1}), so the value doesn't have any inherent meaning.</span></span> <span data-ttu-id="424ce-131">Da der Fremdschlüssel nicht Teil des Primärschlüssels ist, können seine Werte geändert werden, sodass Sie ein untergeordnetes Element von einem übergeordneten Element zu einem anderen verschieben können. Dies ist jedoch in der Regel gegen Aggregat Semantik.</span><span class="sxs-lookup"><span data-stu-id="424ce-131">Since the foreign key is not part of the primary key its values can be changed, so you could move a child from one parent to another one, however this usually goes against aggregate semantics.</span></span>
- <span data-ttu-id="424ce-132">Verwenden des fremd Schlüssels und einer zusätzlichen Eigenschaft als zusammengesetzten Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="424ce-132">Using the foreign key and an additional property as a composite key.</span></span> <span data-ttu-id="424ce-133">Der zusätzliche Eigenschafts Wert muss nun nur für ein bestimmtes übergeordnetes Element eindeutig sein (wenn der übergeordnete {1} also über eine untergeordnete {1,1} verfügt, kann der übergeordnete {2} weiterhin untergeordnete {2,1}) haben.</span><span class="sxs-lookup"><span data-stu-id="424ce-133">The additional property value now only needs to be unique for a given parent (so if Parent {1} has Child {1,1} then Parent {2} can still have Child {2,1}).</span></span> <span data-ttu-id="424ce-134">Durch den Fremdschlüssel Teil des Primärschlüssels wird die Beziehung zwischen dem Besitzer und der Entität, die im Besitz des Primärschlüssels ist, unveränderlich und die Aggregat Semantik ist besser.</span><span class="sxs-lookup"><span data-stu-id="424ce-134">By making the foreign key part of the primary key the relationship between the owner and the owned entity becomes immutable and reflects aggregate semantics better.</span></span> <span data-ttu-id="424ce-135">Dies geschieht EF Core standardmäßig.</span><span class="sxs-lookup"><span data-stu-id="424ce-135">This is what EF Core does by default.</span></span>

<span data-ttu-id="424ce-136">In diesem Beispiel verwenden wir die `Distributor`-Klasse:</span><span class="sxs-lookup"><span data-stu-id="424ce-136">In this example we'll use the `Distributor` class:</span></span>

[!code-csharp[Distributor](../../../samples/core/Modeling/OwnedEntities/Distributor.cs?name=Distributor)]

<span data-ttu-id="424ce-137">Standardmäßig wird der Primärschlüssel, der für den eigenen Typ verwendet wird, auf den über die `ShippingCenters` Navigations Eigenschaft verwiesen wird, `("DistributorId", "Id")`, wobei `"DistributorId"` der FK und `"Id"` ein eindeutiger `int` Wert ist.</span><span class="sxs-lookup"><span data-stu-id="424ce-137">By default the primary key used for the owned type referenced through the `ShippingCenters` navigation property will be `("DistributorId", "Id")` where `"DistributorId"` is the FK and `"Id"` is a unique `int` value.</span></span>

<span data-ttu-id="424ce-138">So konfigurieren Sie einen anderen PK-Rückruf `HasKey`:</span><span class="sxs-lookup"><span data-stu-id="424ce-138">To configure a different PK call `HasKey`:</span></span>

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

> [!NOTE]
> <span data-ttu-id="424ce-139">Bevor EF Core 3,0 `WithOwner()`-Methode nicht vorhanden war, sollte dieser-Befehl entfernt werden.</span><span class="sxs-lookup"><span data-stu-id="424ce-139">Before EF Core 3.0 `WithOwner()` method didn't exist so this call should be removed.</span></span>

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="424ce-140">Zuordnung eigener Typen mit Tabellen Aufteilung</span><span class="sxs-lookup"><span data-stu-id="424ce-140">Mapping owned types with table splitting</span></span>

<span data-ttu-id="424ce-141">Bei der Verwendung von relationalen Datenbanken werden Verweis eigene Typen standardmäßig derselben Tabelle wie der Besitzer zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="424ce-141">When using relational databases, by default reference owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="424ce-142">Dies erfordert das Aufteilen der Tabelle in zwei: einige Spalten werden zum Speichern der Daten des Besitzers verwendet, und einige Spalten werden zum Speichern von Daten der Entität verwendet.</span><span class="sxs-lookup"><span data-stu-id="424ce-142">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="424ce-143">Dies ist ein gängiges Feature, das als [Tabellen Aufteilung](table-splitting.md)bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="424ce-143">This is a common feature known as [table splitting](table-splitting.md).</span></span>

<span data-ttu-id="424ce-144">Standardmäßig werden EF Core die Daten Bank Spalten für die Eigenschaften des eigenen Entitäts Typs nach dem Muster _Navigation_OwnedEntityProperty_benennen.</span><span class="sxs-lookup"><span data-stu-id="424ce-144">By default, EF Core will name the database columns for the properties of the owned entity type following the pattern _Navigation_OwnedEntityProperty_.</span></span> <span data-ttu-id="424ce-145">Daher werden die `StreetAddress` Eigenschaften in der Tabelle "Orders" mit den Namen "ShippingAddress_Street" und "ShippingAddress_City" angezeigt.</span><span class="sxs-lookup"><span data-stu-id="424ce-145">Therefore the `StreetAddress` properties will appear in the 'Orders' table with the names 'ShippingAddress_Street' and 'ShippingAddress_City'.</span></span>

<span data-ttu-id="424ce-146">Sie können die `HasColumnName`-Methode verwenden, um diese Spalten umzubenennen:</span><span class="sxs-lookup"><span data-stu-id="424ce-146">You can use the `HasColumnName` method to rename those columns:</span></span>

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="424ce-147">Gemeinsame Nutzung desselben .net-Typs für mehrere eigene Typen</span><span class="sxs-lookup"><span data-stu-id="424ce-147">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="424ce-148">Ein eigener Entitätstyp kann vom gleichen .NET-Typ wie ein anderer eigener Entitätstyp sein. Daher reicht der .NET-Typ möglicherweise nicht aus, um einen eigenen Typ zu identifizieren.</span><span class="sxs-lookup"><span data-stu-id="424ce-148">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="424ce-149">In diesen Fällen wird die-Eigenschaft, die vom Besitzer auf die eigene Entität zeigt, zur _definierenden Navigation_ des eigenen Entitäts Typs.</span><span class="sxs-lookup"><span data-stu-id="424ce-149">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="424ce-150">Aus Sicht der EF Core ist die definierende Navigation Teil der Typidentität neben dem .NET-Typ.</span><span class="sxs-lookup"><span data-stu-id="424ce-150">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>

<span data-ttu-id="424ce-151">In der folgenden Klasse sind z. b. `ShippingAddress` und `BillingAddress` beide denselben .NET-Typ haben: `StreetAddress`:</span><span class="sxs-lookup"><span data-stu-id="424ce-151">For example, in the following class `ShippingAddress` and `BillingAddress` are both of the same .NET type, `StreetAddress`:</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="424ce-152">Um zu verstehen, wie EF Core überwachte Instanzen dieser Objekte unterscheiden, kann es hilfreich sein zu wissen, dass die definierende Navigation neben dem Wert des Schlüssels des Besitzers und dem .NET-Typ des eigenen Typs zu einem Teil des Schlüssels der Instanz geworden ist.</span><span class="sxs-lookup"><span data-stu-id="424ce-152">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="424ce-153">Im Besitz befindliche Typen</span><span class="sxs-lookup"><span data-stu-id="424ce-153">Nested owned types</span></span>

<span data-ttu-id="424ce-154">In diesem Beispiel `OrderDetails` `BillingAddress` und `ShippingAddress`, bei denen es sich um `StreetAddress` Typen handelt.</span><span class="sxs-lookup"><span data-stu-id="424ce-154">In this example `OrderDetails` owns `BillingAddress` and `ShippingAddress`, which are both `StreetAddress` types.</span></span> <span data-ttu-id="424ce-155">Dann besitzt der `DetailedOrder`-Typ `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="424ce-155">Then `OrderDetails` is owned by the `DetailedOrder` type.</span></span>

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

<span data-ttu-id="424ce-156">Zusätzlich zu den Typen, die im Besitz eines Typs sind, kann ein eigener Typ auf eine reguläre Entität verweisen, es kann sich um den Besitzer oder eine andere Entität handeln, solange sich die zugehörige Entität auf der abhängigen Seite befindet.</span><span class="sxs-lookup"><span data-stu-id="424ce-156">In addition to nested owned types, an owned type can reference a regular entity, it can be either the owner or a different entity as long as the owned entity is on the dependent side.</span></span> <span data-ttu-id="424ce-157">Diese Funktion legt eigene Entitäts Typen außer komplexen Typen in EF6 fest.</span><span class="sxs-lookup"><span data-stu-id="424ce-157">This capability sets owned entity types apart from complex types in EF6.</span></span>

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

<span data-ttu-id="424ce-158">Es ist möglich, die `OwnsOne`-Methode in einem fließend aufzurufenden Befehl zum Konfigurieren dieses Modells zu verketten:</span><span class="sxs-lookup"><span data-stu-id="424ce-158">It is possible to chain the `OwnsOne` method in a fluent call to configure this model:</span></span>

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

<span data-ttu-id="424ce-159">Beachten Sie den `WithOwner`-Befehl, der zum Konfigurieren der Navigations Eigenschaft verwendet wird, die auf den Besitzer zeigt.</span><span class="sxs-lookup"><span data-stu-id="424ce-159">Notice the `WithOwner` call used to configure the navigation property pointing back at the owner.</span></span>

<span data-ttu-id="424ce-160">Es ist möglich, das Ergebnis mithilfe von `OwnedAttribute` sowohl auf `OrderDetails` als auch `StreetAdress`zu erzielen.</span><span class="sxs-lookup"><span data-stu-id="424ce-160">It is possible to achieve the result using `OwnedAttribute` on both `OrderDetails` and `StreetAdress`.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="424ce-161">Speichern von eigenen Typen in separaten Tabellen</span><span class="sxs-lookup"><span data-stu-id="424ce-161">Storing owned types in separate tables</span></span>

<span data-ttu-id="424ce-162">Im Gegensatz zu komplexen EF6-Typen können eigene Typen in einer separaten Tabelle vom Besitzer gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="424ce-162">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="424ce-163">Um die Konvention zu überschreiben, die einen eigenen Typ derselben Tabelle wie der Besitzer zuordnet, können Sie einfach `ToTable` aufzurufen und einen anderen Tabellennamen angeben.</span><span class="sxs-lookup"><span data-stu-id="424ce-163">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="424ce-164">Im folgenden Beispiel werden `OrderDetails` und die beiden Adressen einer separaten Tabelle aus `DetailedOrder`zugeordnet:</span><span class="sxs-lookup"><span data-stu-id="424ce-164">The following example will map `OrderDetails` and its two addresses to a separate table from `DetailedOrder`:</span></span>

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a><span data-ttu-id="424ce-165">Abfragen von eigenen Typen</span><span class="sxs-lookup"><span data-stu-id="424ce-165">Querying owned types</span></span>

<span data-ttu-id="424ce-166">Beim Abfragen des Besitzers werden standardmäßig eigene Typen eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="424ce-166">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="424ce-167">Es ist nicht erforderlich, die `Include`-Methode zu verwenden, auch wenn die eigenen Typen in einer separaten Tabelle gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="424ce-167">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="424ce-168">Basierend auf dem zuvor beschriebenen Modell werden mit der folgenden Abfrage `Order`, `OrderDetails` und die beiden im Besitz befindlichen `StreetAddresses` aus der-Datenbank angezeigt:</span><span class="sxs-lookup"><span data-stu-id="424ce-168">Based on the model described before, the following query will get `Order`, `OrderDetails` and the two owned `StreetAddresses` from the database:</span></span>

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a><span data-ttu-id="424ce-169">Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="424ce-169">Limitations</span></span>

<span data-ttu-id="424ce-170">Einige dieser Einschränkungen sind grundlegend für die Funktionsweise von Entitäts Typen im Besitz, einige andere sind jedoch Einschränkungen, die in zukünftigen Versionen möglicherweise entfernt werden könnten:</span><span class="sxs-lookup"><span data-stu-id="424ce-170">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="424ce-171">Entwurfs Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="424ce-171">By-design restrictions</span></span>

- <span data-ttu-id="424ce-172">Sie können keine `DbSet<T>` für einen eigenen Typ erstellen.</span><span class="sxs-lookup"><span data-stu-id="424ce-172">You cannot create a `DbSet<T>` for an owned type</span></span>
- <span data-ttu-id="424ce-173">`Entity<T>()` können nicht mit einem eigenen Typ aufgerufen werden `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="424ce-173">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="424ce-174">Aktuelle Mängel</span><span class="sxs-lookup"><span data-stu-id="424ce-174">Current shortcomings</span></span>

- <span data-ttu-id="424ce-175">Vererbungs Hierarchien, die eigene Entitäts Typen einschließen, werden nicht unterstützt</span><span class="sxs-lookup"><span data-stu-id="424ce-175">Inheritance hierarchies that include owned entity types are not supported</span></span>
- <span data-ttu-id="424ce-176">Verweis Navigation auf eigene Entitäts Typen darf nicht NULL sein, es sei denn, Sie sind explizit einer separaten Tabelle vom Besitzer zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="424ce-176">Reference navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span>
- <span data-ttu-id="424ce-177">Instanzen von eigenen Entitäts Typen können nicht von mehreren Besitzern gemeinsam genutzt werden (Dies ist ein bekanntes Szenario für Wert Objekte, das nicht mit Entitäts Typen im Besitz von Entitäten implementiert werden kann).</span><span class="sxs-lookup"><span data-stu-id="424ce-177">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="424ce-178">Mängel in früheren Versionen</span><span class="sxs-lookup"><span data-stu-id="424ce-178">Shortcomings in previous versions</span></span>

- <span data-ttu-id="424ce-179">In EF Core 2,0 können Navigationen zu besitzenden Entitäts Typen nicht in abgeleiteten Entitäts Typen deklariert werden, es sei denn, die im Besitz befindlichen Entitäten werden explizit einer separaten Tabelle der Besitzer Hierarchie zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="424ce-179">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="424ce-180">Diese Einschränkung wurde in EF Core 2,1 entfernt.</span><span class="sxs-lookup"><span data-stu-id="424ce-180">This limitation has been removed in EF Core 2.1</span></span>
- <span data-ttu-id="424ce-181">In EF Core 2,0 und 2,1 wurden nur Verweis Navigation auf eigene Typen unterstützt.</span><span class="sxs-lookup"><span data-stu-id="424ce-181">In EF Core 2.0 and 2.1 only reference navigations to owned types were supported.</span></span> <span data-ttu-id="424ce-182">Diese Einschränkung wurde in EF Core 2,2 entfernt.</span><span class="sxs-lookup"><span data-stu-id="424ce-182">This limitation has been removed in EF Core 2.2</span></span>
