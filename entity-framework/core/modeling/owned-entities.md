---
title: "Im Besitz Entitätstypen - EF Core"
author: julielerman
ms.author: divega
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
ms.technology: entity-framework-core
uid: core/modeling/owned-entities
ms.openlocfilehash: a6823377eb626ca92263c31351e1aef61db5a787
ms.sourcegitcommit: 4b7d3d3e258b0d9cb778bb45a9f4a33c0792e38e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="owned-entity-types"></a><span data-ttu-id="a7d94-102">Im Besitz befindlichen Entitätstypen</span><span class="sxs-lookup"><span data-stu-id="a7d94-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="a7d94-103">Dieses Feature ist neu in EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="a7d94-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="a7d94-104">EF Core ermöglicht Ihnen das Modell Entitätstypen, die immer nur auf andere Entitätstypen Navigationseigenschaften angezeigt werden können.</span><span class="sxs-lookup"><span data-stu-id="a7d94-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="a7d94-105">Diese heißen _im Besitz von Entitätstypen_.</span><span class="sxs-lookup"><span data-stu-id="a7d94-105">These are called _owned entity types_.</span></span> <span data-ttu-id="a7d94-106">Die Entität, die im Besitz befindlichen Entitätstyp ist seine _Besitzer_.</span><span class="sxs-lookup"><span data-stu-id="a7d94-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="a7d94-107">Explizite Konfiguration</span><span class="sxs-lookup"><span data-stu-id="a7d94-107">Explicit configuration</span></span>

<span data-ttu-id="a7d94-108">Besitz befindliche Entität, die Typen nie gemäß der Konvention von EF Core im Modell enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="a7d94-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="a7d94-109">Können Sie die `OwnsOne` Methode im `OnModelCreating` oder kommentieren Sie den Typ mit `OwnedAttrbibute` (neu in EF Core 2.1) so konfigurieren Sie den Typ als Besitzer Typ.</span><span class="sxs-lookup"><span data-stu-id="a7d94-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttrbibute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="a7d94-110">In diesem Beispiel ist StreetAddress ein Typ mit keine Identity-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="a7d94-110">In this example, StreetAddress is a type with no identity property.</span></span> <span data-ttu-id="a7d94-111">Dieser Typ wird als Eigenschaft des Typs „Order“ verwendet, um die Lieferadresse für eine bestimmte Bestellung anzugeben.</span><span class="sxs-lookup"><span data-stu-id="a7d94-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span> <span data-ttu-id="a7d94-112">In `OnModelCreating`, verwenden wir die `OwnsOne` Methode, um anzugeben, dass die ShippingAddress-Eigenschaft einer Entität gehören, der den Auftragstyp ist.</span><span class="sxs-lookup"><span data-stu-id="a7d94-112">In `OnModelCreating`, we use the `OwnsOne` method to specify that the ShippingAddress property is an Owned Entity of the Order type.</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

// OnModelCreating
modelBuilder.Entity<Order>().OwnsOne(p => p.ShippingAddress);
```

<span data-ttu-id="a7d94-113">Wenn die Eigenschaft ShippingAddress in den Auftragstyp privat ist, können Sie die Version der Zeichenfolge die `OwnsOne` Methode:</span><span class="sxs-lookup"><span data-stu-id="a7d94-113">If the ShippingAddress property is private in the Order type, you can use the string version of the `OwnsOne` method:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

<span data-ttu-id="a7d94-114">In diesem Beispiel verwenden wir die `OwnedAttribute` auf das gleiche Ziel zu erreichen:</span><span class="sxs-lookup"><span data-stu-id="a7d94-114">In this example, we use the `OwnedAttribute` to achieve the same goal:</span></span>

``` csharp
[Owned]
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}
```

## <a name="implicit-keys"></a><span data-ttu-id="a7d94-115">Implizite Schlüssel</span><span class="sxs-lookup"><span data-stu-id="a7d94-115">Implicit keys</span></span>

<span data-ttu-id="a7d94-116">EF Core 2.0 und 2.1 können nur den Verweis Navigationseigenschaften im Besitz befindlichen Typen zeigen.</span><span class="sxs-lookup"><span data-stu-id="a7d94-116">In EF Core 2.0 and 2.1, only reference navigation properties can point to owned types.</span></span> <span data-ttu-id="a7d94-117">Sammlungen im Besitz befindlichen Typen werden nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="a7d94-117">Collections of owned types are not supported.</span></span> <span data-ttu-id="a7d94-118">Dieser Verweis gehört immer aufweisen: 1-Beziehung mit dem Besitzer, daher nicht benötigte ihren eigenen Werten.</span><span class="sxs-lookup"><span data-stu-id="a7d94-118">These reference owned types always have a one-to-one relationship with the owner, therefore they don't need their own key values.</span></span> <span data-ttu-id="a7d94-119">Im vorherigen Beispiel muss der StreetAddress-Typ nicht um eine Schlüsseleigenschaft definiert.</span><span class="sxs-lookup"><span data-stu-id="a7d94-119">In the previous example, the StreetAddress type does not need to define a key property.</span></span>  

<span data-ttu-id="a7d94-120">In der Reihenfolge für das Verständnis, wie diese Objekte von EF Core verfolgt nach, es ist sinnvoll, Sie glauben, dass ein Primärschlüssel, als erstellt wird ein [Shadowing Eigenschaft](xref:core/modeling/shadow-properties) für den Besitzer Typ.</span><span class="sxs-lookup"><span data-stu-id="a7d94-120">In order to understanding how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="a7d94-121">Der Wert des Schlüssels einer Instanz des Typs im Besitz befindlichen wird den Wert des Schlüssels der Besitzer Instanz identisch sein.</span><span class="sxs-lookup"><span data-stu-id="a7d94-121">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>      

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="a7d94-122">Zuordnung, die im Besitz von Typen mit Teilen der Tabelle</span><span class="sxs-lookup"><span data-stu-id="a7d94-122">Mapping owned types with table splitting</span></span>

<span data-ttu-id="a7d94-123">Bei relationalen Datenbanken, werden gemäß der Konvention, die im Besitz von Typen als Besitzer der gleichen Tabelle zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="a7d94-123">When using relational databases, by convention owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="a7d94-124">Dies erfordert die Tabelle in zwei Teilen: einige Spalten zum Speichern der Daten des Besitzers verwendet werden, und einige Spalten zum Speichern von Daten, die im Besitz befindlichen Entität verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="a7d94-124">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="a7d94-125">Dies ist ein gängiges Feature als Tabelle Aufteilung bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="a7d94-125">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="a7d94-126">Besitzer können Typen mit Teilen der Tabelle gespeichert werden verwendet sehr ähnlich wie komplexe Typen in EF6 verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="a7d94-126">Owned types stored with table splitting can be used very similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="a7d94-127">Gemäß der Konvention EF Core die Datenbankspalten für die Eigenschaften, die im Besitz befindlichen Entitätstyps, die nach dem Muster benennt _EntityProperty_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="a7d94-127">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _EntityProperty_OwnedEntityProperty_.</span></span> <span data-ttu-id="a7d94-128">Aus diesem Grund werden die StreetAddress-Eigenschaften in der Orders-Tabelle mit den Namen ShippingAddress_Street und ShippingAddress_City angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a7d94-128">Therefore the StreetAddress properties will appear in the Orders table with the names ShippingAddress_Street and ShippingAddress_City.</span></span>

<span data-ttu-id="a7d94-129">Sie können den Anfügen der `HasColumnName` Methode, um diese Spalten umbenennen.</span><span class="sxs-lookup"><span data-stu-id="a7d94-129">You can append the `HasColumnName` method to rename those columns.</span></span> <span data-ttu-id="a7d94-130">In der Fall, in dem StreetAddress eine öffentliche Eigenschaft ist, wäre die Zuordnungen</span><span class="sxs-lookup"><span data-stu-id="a7d94-130">In the case where StreetAddress is a public property, the mappings would be</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="a7d94-131">Freigeben von .NET Typparameter für mehrere Besitzer</span><span class="sxs-lookup"><span data-stu-id="a7d94-131">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="a7d94-132">Entitätstyp im Besitz befindlichen möglich desselben Typs .NET als einen anderen Besitzer Entitätstyp daher, dass der .NET Typ möglicherweise nicht mit genug, um ein im Besitz befindlichen Typ zu identifizieren.</span><span class="sxs-lookup"><span data-stu-id="a7d94-132">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="a7d94-133">In diesen Fällen wird die Eigenschaft, die vom Besitzer auf die im Besitz befindlichen Entität zeigen die _Navigation definieren_ des Entitätstyps Eigentum ist.</span><span class="sxs-lookup"><span data-stu-id="a7d94-133">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="a7d94-134">Aus der Perspektive des EF-Kerns stammt die definierende Navigation die Typidentität zusammen mit den Typ .NET.</span><span class="sxs-lookup"><span data-stu-id="a7d94-134">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="a7d94-135">Beispielsweise sind in der folgenden Klasse ShippingAddress und BillingAddress vom gleichen .NET-Typ StreetAddress:</span><span class="sxs-lookup"><span data-stu-id="a7d94-135">For example, in the following class, ShippingAddress and BillingAddress are both of the same .NET type, StreetAddress:</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

<span data-ttu-id="a7d94-136">Um zu erfahren, wie EF Core überwachte Instanzen dieser Objekte unterschieden wird, kann es nützlich sein, glauben, dass die definierende Navigation Teil des Schlüssels der Instanz zusammen mit dem Wert des Schlüssels des Besitzers und der .NET Typ des im Besitz befindlichen geworden ist.</span><span class="sxs-lookup"><span data-stu-id="a7d94-136">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="a7d94-137">Geschachtelte Typen im Besitz befindlichen</span><span class="sxs-lookup"><span data-stu-id="a7d94-137">Nested owned types</span></span>

<span data-ttu-id="a7d94-138">In diesem Beispiel besitzt OrderDetails BillingAddress und ShippingAddress, bei denen beide StreetAddress-Typen handelt.</span><span class="sxs-lookup"><span data-stu-id="a7d94-138">In this example OrderDetails owns BillingAddress and ShippingAddress, which are both StreetAddress types.</span></span> <span data-ttu-id="a7d94-139">Klicken Sie dann gehört OrderDetails den Auftragstyp.</span><span class="sxs-lookup"><span data-stu-id="a7d94-139">Then OrderDetails is owned by the Order type.</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public OrderDetails OrderDetails { get; set; }
    public OrderStatus Status { get; set; }
}

public class OrderDetails
{
    public StreetAddress BillingAddress { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```

<span data-ttu-id="a7d94-140">Es ist möglich, die Kette der `OwnsOne` Methode in einer fluent-Zuordnung zu diesem Modell zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="a7d94-140">It is possible to chain the `OwnsOne` method in a fluent mapping to configure this model:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

<span data-ttu-id="a7d94-141">Es ist möglich, die gleiche Bedeutung mit erzielen `OwnedAttribute` OrderDetails und StreetAdress.</span><span class="sxs-lookup"><span data-stu-id="a7d94-141">It is possible to achieve the same thing using `OwnedAttribute` on both OrderDetails and StreetAdress.</span></span>

<span data-ttu-id="a7d94-142">Zusätzlich zu den geschachtelten Typen besitzt kann ein Typ im Besitz eine gewöhnlichen Entität verweisen.</span><span class="sxs-lookup"><span data-stu-id="a7d94-142">In addition to nested owned types, an owned type can reference a regular entity.</span></span> <span data-ttu-id="a7d94-143">Im folgenden Beispiel wird Land einer gewöhnlichen Entität mit (d. h. nicht im Besitz der):</span><span class="sxs-lookup"><span data-stu-id="a7d94-143">In the following example, Country is a regular (i.e. non-owned) entity:</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

<span data-ttu-id="a7d94-144">Diese Funktion wird im Besitz befindlichen Entitätstypen abgesehen von komplexen Typen im EF6 festgelegt.</span><span class="sxs-lookup"><span data-stu-id="a7d94-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="a7d94-145">Speichern von Typen in separaten Tabellen gehören</span><span class="sxs-lookup"><span data-stu-id="a7d94-145">Storing owned types in separate tables</span></span>

<span data-ttu-id="a7d94-146">Im Gegensatz zu EF6 komplexe Typen können auch im Besitz befindlichen Typen in einer separaten Tabelle vom Besitzer gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="a7d94-146">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="a7d94-147">Sie können einfach aufrufen, um der Konvention zu überschreiben, die der gleichen Tabelle wie der Besitzer ein Eigentümer Typs zuordnet, `ToTable` , und geben Sie einen anderen Tabellennamen.</span><span class="sxs-lookup"><span data-stu-id="a7d94-147">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="a7d94-148">Im folgende Beispiel wird in eine separate Tabelle OrderDetails und den beiden Adressen von Reihenfolge zugeordnet:</span><span class="sxs-lookup"><span data-stu-id="a7d94-148">The following example will map OrderDetails and its two addresses to a separate table from Order:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a><span data-ttu-id="a7d94-149">Abfragen von im Besitz befindlichen Typen</span><span class="sxs-lookup"><span data-stu-id="a7d94-149">Querying owned types</span></span>

<span data-ttu-id="a7d94-150">Beim Abfragen des Besitzers werden standardmäßig eigene Typen eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="a7d94-150">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="a7d94-151">Es ist nicht notwendig, verwenden Sie die `Include` -Methode, auch wenn die im Besitz befindlichen Typen in einer separaten Tabelle gespeichert sind.</span><span class="sxs-lookup"><span data-stu-id="a7d94-151">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="a7d94-152">Je nachdem welches Modell beschrieben vor, ruft die folgende Abfrage Order "," OrderDetails "und" der zwei im Besitz befindlichen StreeAddresses für alle ausstehenden Aufträge aus der Datenbank:</span><span class="sxs-lookup"><span data-stu-id="a7d94-152">Based on the model described before, the following query will pull Order, OrderDetails and the two owned StreeAddresses for all pending orders from the database:</span></span>

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a><span data-ttu-id="a7d94-153">Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="a7d94-153">Limitations</span></span>

<span data-ttu-id="a7d94-154">Hier sind einige Einschränkungen im Besitz befindlichen Entitätstypen.</span><span class="sxs-lookup"><span data-stu-id="a7d94-154">Here are some limitations of owned entity types.</span></span> <span data-ttu-id="a7d94-155">Diese Einschränkungen sind grundlegende wie im Besitz befindlichen Typen arbeiten, aber andere Point-in-Time-Einschränkungen, die wir in zukünftigen Versionen entfernt möchten werden:</span><span class="sxs-lookup"><span data-stu-id="a7d94-155">Some of these limitations are fundamental to how owned types work, but some others are point-in-time restrictions that we would like to remove in future releases:</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="a7d94-156">Aktuelle Mängel</span><span class="sxs-lookup"><span data-stu-id="a7d94-156">Current shortcomings</span></span>
- <span data-ttu-id="a7d94-157">Vererbung von im Besitz befindlichen Typen wird nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="a7d94-157">Inheritance of owned types is not supported</span></span>
- <span data-ttu-id="a7d94-158">Im Besitz befindlichen Typen können nicht auf eine auflistungsnavigationseigenschaft zeigt werden</span><span class="sxs-lookup"><span data-stu-id="a7d94-158">Owned types cannot be pointed at by a collection navigation property</span></span>
- <span data-ttu-id="a7d94-159">Da sie die Tabelle durch Aufteilen verwenden gehören Standard, dass Typen auch die folgenden Einschränkungen verfügen, es sei denn, die explizit einer anderen Tabelle zugeordnet:</span><span class="sxs-lookup"><span data-stu-id="a7d94-159">Since they use table splitting by default owned types also have the following restrictions unless explicitly mapped to a different table:</span></span>
   - <span data-ttu-id="a7d94-160">Sie können nicht von einem abgeleiteten Typ Benutzer</span><span class="sxs-lookup"><span data-stu-id="a7d94-160">They cannot be owned by a derived type</span></span>
   - <span data-ttu-id="a7d94-161">Die definierenden Navigationseigenschaft kann nicht festgelegt werden auf Null (d. h. gehören Typen in der gleichen Tabelle immer erforderlich)</span><span class="sxs-lookup"><span data-stu-id="a7d94-161">The defining navigation property cannot be set to null (i.e. owned types on the same table are always required)</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="a7d94-162">Standardmäßig Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="a7d94-162">By-design restrictions</span></span>
- <span data-ttu-id="a7d94-163">Sie können nicht erstellt werden ein `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="a7d94-163">You cannot create a `DbSet<T>`</span></span>
- <span data-ttu-id="a7d94-164">Sie können nicht aufrufen `Entity<T>()` mit einem Datentyp auf, die im Besitz befindlichen `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="a7d94-164">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>
