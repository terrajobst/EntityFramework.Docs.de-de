---
title: Entitätstypen - EF Core im Besitz
author: julielerman
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: 1104a8a9a4540e33624fad69c47f2f950c6669bf
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489413"
---
# <a name="owned-entity-types"></a><span data-ttu-id="ec858-102">Entitätstypen im Besitz</span><span class="sxs-lookup"><span data-stu-id="ec858-102">Owned Entity Types</span></span>

>[!NOTE]
> <span data-ttu-id="ec858-103">Dieses Feature ist neu in EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="ec858-103">This feature is new in EF Core 2.0.</span></span>

<span data-ttu-id="ec858-104">EF Core können Sie die Modell-Entitätstypen, die nur für Navigationseigenschaften von anderen Entitätstypen angezeigt werden können.</span><span class="sxs-lookup"><span data-stu-id="ec858-104">EF Core allows you to model entity types that can only ever appear on navigation properties of other entity types.</span></span> <span data-ttu-id="ec858-105">Diese heißen _Entitätstypen im Besitz_.</span><span class="sxs-lookup"><span data-stu-id="ec858-105">These are called _owned entity types_.</span></span> <span data-ttu-id="ec858-106">Die Entität, die einen eigener Entitätstyp enthält, ist die _Besitzer_.</span><span class="sxs-lookup"><span data-stu-id="ec858-106">The entity containing an owned entity type is its _owner_.</span></span>

## <a name="explicit-configuration"></a><span data-ttu-id="ec858-107">Explizite Konfiguration</span><span class="sxs-lookup"><span data-stu-id="ec858-107">Explicit configuration</span></span>

<span data-ttu-id="ec858-108">Im Besitz Entität, die Typen nicht gemäß der Konvention von EF Core im Modell enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="ec858-108">Owned entity types are never included by EF Core in the model by convention.</span></span> <span data-ttu-id="ec858-109">Sie können die `OwnsOne` -Methode in der `OnModelCreating` oder fügen Sie den Typ mit `OwnedAttribute` (neu in EF Core 2.1) so konfigurieren Sie den Typ als ein eigener Typ.</span><span class="sxs-lookup"><span data-stu-id="ec858-109">You can use the `OwnsOne` method in `OnModelCreating` or annotate the type with `OwnedAttribute` (new in EF Core 2.1) to configure the type as an owned type.</span></span>

<span data-ttu-id="ec858-110">In diesem Beispiel ist die StreetAddress einen Typ ohne Identitätseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="ec858-110">In this example, StreetAddress is a type with no identity property.</span></span> <span data-ttu-id="ec858-111">Dieser Typ wird als Eigenschaft des Typs „Order“ verwendet, um die Lieferadresse für eine bestimmte Bestellung anzugeben.</span><span class="sxs-lookup"><span data-stu-id="ec858-111">It is used as a property of the Order type to specify the shipping address for a particular order.</span></span> <span data-ttu-id="ec858-112">In `OnModelCreating`, verwenden wir die `OwnsOne` Methode, um anzugeben, dass die ShippingAddress Eigenschaft einer Entität im Besitz des Typs "Order".</span><span class="sxs-lookup"><span data-stu-id="ec858-112">In `OnModelCreating`, we use the `OwnsOne` method to specify that the ShippingAddress property is an Owned Entity of the Order type.</span></span>

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

<span data-ttu-id="ec858-113">Wenn die ShippingAddress-Eigenschaft in den Auftragstyp privat ist, können Sie die Version der Zeichenfolge die `OwnsOne` Methode:</span><span class="sxs-lookup"><span data-stu-id="ec858-113">If the ShippingAddress property is private in the Order type, you can use the string version of the `OwnsOne` method:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

<span data-ttu-id="ec858-114">In diesem Beispiel verwenden wir die `OwnedAttribute` um das gleiche Ziel zu erreichen:</span><span class="sxs-lookup"><span data-stu-id="ec858-114">In this example, we use the `OwnedAttribute` to achieve the same goal:</span></span>

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

## <a name="implicit-keys"></a><span data-ttu-id="ec858-115">Implizite Schlüssel</span><span class="sxs-lookup"><span data-stu-id="ec858-115">Implicit keys</span></span>

<span data-ttu-id="ec858-116">In EF Core 2.0 und 2.1 können nur die verweisnavigationseigenschaften auf eigene Typen verweisen.</span><span class="sxs-lookup"><span data-stu-id="ec858-116">In EF Core 2.0 and 2.1, only reference navigation properties can point to owned types.</span></span> <span data-ttu-id="ec858-117">Auflistungen von eigenen Typen werden nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="ec858-117">Collections of owned types are not supported.</span></span> <span data-ttu-id="ec858-118">Dieser Verweis gehört-Typen haben immer eine direkte Beziehung mit dem Besitzer, daher nicht benötigten ihre eigenen Werte.</span><span class="sxs-lookup"><span data-stu-id="ec858-118">These reference owned types always have a one-to-one relationship with the owner, therefore they don't need their own key values.</span></span> <span data-ttu-id="ec858-119">Im vorherigen Beispiel muss der StreetAddress-Typ nicht um eine Schlüsseleigenschaft definiert.</span><span class="sxs-lookup"><span data-stu-id="ec858-119">In the previous example, the StreetAddress type does not need to define a key property.</span></span>  

<span data-ttu-id="ec858-120">Um zu verstehen, wie diese Objekte von EF Core nachverfolgt wird, ist es sinnvoll, die glauben, dass ein Primärschlüssel, als erstellt wird eine [schatteneigenschaft](xref:core/modeling/shadow-properties) für den eigenen Typ.</span><span class="sxs-lookup"><span data-stu-id="ec858-120">In order to understand how EF Core tracks these objects, it is useful to think that a primary key is created as a [shadow property](xref:core/modeling/shadow-properties) for the owned type.</span></span> <span data-ttu-id="ec858-121">Der Wert des Schlüssels für eine Instanz des eigenen Typs wird der Wert des Schlüssels für die Besitzer-Instanz identisch sein.</span><span class="sxs-lookup"><span data-stu-id="ec858-121">The value of the key of an instance of the owned type will be the same as the value of the key of the owner instance.</span></span>      

## <a name="mapping-owned-types-with-table-splitting"></a><span data-ttu-id="ec858-122">Zuordnung, die im Besitz von Typen mit tabellenaufteilung</span><span class="sxs-lookup"><span data-stu-id="ec858-122">Mapping owned types with table splitting</span></span>

<span data-ttu-id="ec858-123">Verwendung von relationalen Datenbanken werden standardmäßig eigene Typen als Besitzer der gleichen Tabelle zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="ec858-123">When using relational databases, by convention owned types are mapped to the same table as the owner.</span></span> <span data-ttu-id="ec858-124">Dies erfordert in der Tabelle in zwei Teilen: einige Spalten zum Speichern der Daten des Besitzers verwendet werden, und einige Spalten zum Speichern von Daten der Entität im Besitz des Benutzers verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="ec858-124">This requires splitting the table in two: some columns will be used to store the data of the owner, and some columns will be used to store data of the owned entity.</span></span> <span data-ttu-id="ec858-125">Dies ist eine Standardfunktion in tabellenaufteilung genannt.</span><span class="sxs-lookup"><span data-stu-id="ec858-125">This is a common feature known as table splitting.</span></span>

> [!TIP]
> <span data-ttu-id="ec858-126">Besitzer des Typen, die mit der tabellenaufteilung gespeichert sein können wie komplexe Typen in EF 6 verwendeten sind sehr ähnlich wie verwendet.</span><span class="sxs-lookup"><span data-stu-id="ec858-126">Owned types stored with table splitting can be used very similarly to how complex types are used in EF6.</span></span>

<span data-ttu-id="ec858-127">Gemäß der Konvention wird EF Core die Datenbankspalten für die Eigenschaften der eigene Entitätstyp mit dem folgenden Format benennen _EntityProperty_OwnedEntityProperty_.</span><span class="sxs-lookup"><span data-stu-id="ec858-127">By convention, EF Core will name the database columns for the properties of the owned entity type following the pattern _EntityProperty_OwnedEntityProperty_.</span></span> <span data-ttu-id="ec858-128">Aus diesem Grund werden die StreetAddress-Eigenschaften in der Orders-Tabelle mit den Namen ShippingAddress_Street und ShippingAddress_City angezeigt.</span><span class="sxs-lookup"><span data-stu-id="ec858-128">Therefore the StreetAddress properties will appear in the Orders table with the names ShippingAddress_Street and ShippingAddress_City.</span></span>

<span data-ttu-id="ec858-129">Sie können den Anfügen der `HasColumnName` Methode, um diese Spalten umzubenennen.</span><span class="sxs-lookup"><span data-stu-id="ec858-129">You can append the `HasColumnName` method to rename those columns.</span></span> <span data-ttu-id="ec858-130">In der Fall, in denen eine öffentliche Eigenschaft von StreetAddress ist, wäre die Zuordnungen</span><span class="sxs-lookup"><span data-stu-id="ec858-130">In the case where StreetAddress is a public property, the mappings would be</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a><span data-ttu-id="ec858-131">Freigabe desselben Typs .NET für mehrere im Besitz des Benutzers</span><span class="sxs-lookup"><span data-stu-id="ec858-131">Sharing the same .NET type among multiple owned types</span></span>

<span data-ttu-id="ec858-132">Ein eigener Entitätstyp möglich des gleichen Typs .NET als einem anderen eigenen Entitätstyp, daher, dass der .NET Typ möglicherweise nicht mit ausreichend, um ein eigener Typ zu identifizieren.</span><span class="sxs-lookup"><span data-stu-id="ec858-132">An owned entity type can be of the same .NET type as another owned entity type, therefore the .NET type may not be enough to identify an owned type.</span></span>

<span data-ttu-id="ec858-133">In diesen Fällen wird die Eigenschaft, die auf die Entität im Besitz des Besitzers der _Definieren der Navigation_ des eigenen Entitätstyps.</span><span class="sxs-lookup"><span data-stu-id="ec858-133">In those cases, the property pointing from the owner to the owned entity becomes the _defining navigation_ of the owned entity type.</span></span> <span data-ttu-id="ec858-134">Aus der Perspektive von EF Core ist die definierende Navigation Teil der Identität für den Typ zusammen mit der .NET-Typ.</span><span class="sxs-lookup"><span data-stu-id="ec858-134">From the perspective of EF Core, the defining navigation is part of the type's identity alongside the .NET type.</span></span>   

<span data-ttu-id="ec858-135">Beispielsweise sind in der folgenden Klasse Shippingaddress- und BillingAddress des gleichen .NET-Typs, StreetAddress:</span><span class="sxs-lookup"><span data-stu-id="ec858-135">For example, in the following class, ShippingAddress and BillingAddress are both of the same .NET type, StreetAddress:</span></span>

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

<span data-ttu-id="ec858-136">Um zu verstehen, wie EF Core für überwachte Instanzen dieser Objekte unterscheiden, wird, kann es hilfreich sein zu denken, dass die definierende Navigation Teil des Schlüssels der Instanz zusammen mit dem Wert des Schlüssels des Besitzers und der .NET-Typ des eigenen Typs geworden ist.</span><span class="sxs-lookup"><span data-stu-id="ec858-136">In order to understand how EF Core will distinguish tracked instances of these objects, it may be useful to think that the defining navigation has become part of the key of the instance alongside the value of the key of the owner and the .NET type of the owned type.</span></span>

## <a name="nested-owned-types"></a><span data-ttu-id="ec858-137">Geschachtelte eigene Typen</span><span class="sxs-lookup"><span data-stu-id="ec858-137">Nested owned types</span></span>

<span data-ttu-id="ec858-138">In diesem Beispiel besitzt die OrderDetails BillingAddress und ShippingAddress, die beide StreetAddress-Typen sind.</span><span class="sxs-lookup"><span data-stu-id="ec858-138">In this example OrderDetails owns BillingAddress and ShippingAddress, which are both StreetAddress types.</span></span> <span data-ttu-id="ec858-139">Wird der Auftrag vom Typ OrderDetails gehören.</span><span class="sxs-lookup"><span data-stu-id="ec858-139">Then OrderDetails is owned by the Order type.</span></span>

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

public enum OrderStatus
{
    Pending,
    Shipped
}

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```

<span data-ttu-id="ec858-140">Es ist möglich, eine Verkettung der `OwnsOne` -Methode in eine fluentzuordnung so konfigurieren Sie dieses Modell:</span><span class="sxs-lookup"><span data-stu-id="ec858-140">It is possible to chain the `OwnsOne` method in a fluent mapping to configure this model:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

<span data-ttu-id="ec858-141">Es ist möglich, die gleiche Sache mit `OwnedAttribute` auf OrderDetails und StreetAdress.</span><span class="sxs-lookup"><span data-stu-id="ec858-141">It is possible to achieve the same thing using `OwnedAttribute` on both OrderDetails and StreetAdress.</span></span>

<span data-ttu-id="ec858-142">Zusätzlich zu den geschachtelten Typen im Besitz des Benutzers kann ein eigener Typ eine normale Entität verweisen.</span><span class="sxs-lookup"><span data-stu-id="ec858-142">In addition to nested owned types, an owned type can reference a regular entity.</span></span> <span data-ttu-id="ec858-143">Im folgenden Beispiel wird die Land eine reguläre, nicht im Besitz der Entität:</span><span class="sxs-lookup"><span data-stu-id="ec858-143">In the following example, Country is a regular non-owned entity:</span></span>

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

<span data-ttu-id="ec858-144">Diese Funktion legt die Entitätstypen, die abgesehen von komplexen Typen im Besitz in EF 6 fest.</span><span class="sxs-lookup"><span data-stu-id="ec858-144">This capability sets owned entity types apart from complex types in EF6.</span></span>

## <a name="storing-owned-types-in-separate-tables"></a><span data-ttu-id="ec858-145">Das Speichern von eigene Typen in separaten Tabellen</span><span class="sxs-lookup"><span data-stu-id="ec858-145">Storing owned types in separate tables</span></span>

<span data-ttu-id="ec858-146">Im Gegensatz zu EF6 komplexe Typen können auch eigene Typen in einer separaten Tabelle vom Besitzer gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="ec858-146">Also unlike EF6 complex types, owned types can be stored in a separate table from the owner.</span></span> <span data-ttu-id="ec858-147">Sie können einfach aufrufen, um die Konvention überschreiben, der einen eigenen Typ als Besitzer der gleichen Tabelle zugeordnet, `ToTable` , und geben Sie einen anderen Tabellennamen an.</span><span class="sxs-lookup"><span data-stu-id="ec858-147">In order to override the convention that maps an owned type to the same table as the owner, you can simply call `ToTable` and provide a different table name.</span></span> <span data-ttu-id="ec858-148">Im folgende Beispiel wird in eine separate Tabelle OrderDetails und die zwei Adressen von Reihenfolge zugeordnet:</span><span class="sxs-lookup"><span data-stu-id="ec858-148">The following example will map OrderDetails and its two addresses to a separate table from Order:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
        od.ToTable("OrderDetails");
    });
```

## <a name="querying-owned-types"></a><span data-ttu-id="ec858-149">Abfragen von Typen im Besitz</span><span class="sxs-lookup"><span data-stu-id="ec858-149">Querying owned types</span></span>

<span data-ttu-id="ec858-150">Beim Abfragen des Besitzers werden standardmäßig eigene Typen eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="ec858-150">When querying the owner the owned types will be included by default.</span></span> <span data-ttu-id="ec858-151">Es ist nicht erforderlich, verwenden die `Include` -Methode, selbst wenn Sie eigenen Typen in einer separaten Tabelle gespeichert sind.</span><span class="sxs-lookup"><span data-stu-id="ec858-151">It is not necessary to use the `Include` method, even if the owned types are stored in a separate table.</span></span> <span data-ttu-id="ec858-152">Basierend auf das Modell beschrieben vor, wird die folgende Abfrage Order "," OrderDetails "und" die beiden im Besitz des Benutzers StreetAddresses für alle ausstehenden Aufträge aus der Datenbank abzurufen:</span><span class="sxs-lookup"><span data-stu-id="ec858-152">Based on the model described before, the following query will pull Order, OrderDetails and the two owned StreetAddresses for all pending orders from the database:</span></span>

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a><span data-ttu-id="ec858-153">Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="ec858-153">Limitations</span></span>

<span data-ttu-id="ec858-154">Einige dieser Einschränkungen ist die Grundlage wie die im Besitz des Benutzers Entität Typen arbeiten, aber einige andere werden Einschränkungen, die wir in zukünftigen Versionen entfernt werden kann:</span><span class="sxs-lookup"><span data-stu-id="ec858-154">Some of these limitations are fundamental to how owned entity types work, but some others are restrictions that we may be able to remove in future releases:</span></span>

### <a name="shortcomings-in-previous-versions"></a><span data-ttu-id="ec858-155">Mängel in früheren Versionen</span><span class="sxs-lookup"><span data-stu-id="ec858-155">Shortcomings in previous versions</span></span>
- <span data-ttu-id="ec858-156">In EF Core 2.0 gehören zu, dass die Entitätstypen können nicht in abgeleiteten Entitätstypen deklariert werden, es sei denn, die eigenen Entitäten explizit in eine separate Tabelle aus der Besitzerhierarchie zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="ec858-156">In EF Core 2.0, navigations to owned entity types cannot be declared in derived entity types unless the owned entities are explicitly mapped to a separate table from the owner hierarchy.</span></span> <span data-ttu-id="ec858-157">Diese Einschränkung wurde in EF Core 2.1 entfernt</span><span class="sxs-lookup"><span data-stu-id="ec858-157">This limitation has been removed in EF Core 2.1</span></span>

### <a name="current-shortcomings"></a><span data-ttu-id="ec858-158">Aktuelle Mängel</span><span class="sxs-lookup"><span data-stu-id="ec858-158">Current shortcomings</span></span>
- <span data-ttu-id="ec858-159">Vererbungshierarchien, die enthalten, das im Besitz, Entitätstypen werden nicht unterstützt</span><span class="sxs-lookup"><span data-stu-id="ec858-159">Inheritance hierarchies that include owned entity types are not supported</span></span>
- <span data-ttu-id="ec858-160">Entitätstypen im Besitz des Benutzers können nicht auf durch eine auflistungsnavigationseigenschaft (nur Referenz, die derzeit Navigationen unterstützten) verwiesen werden</span><span class="sxs-lookup"><span data-stu-id="ec858-160">Owned entity types cannot be pointed at by a collection navigation property (only reference navigations are currently supported)</span></span>
- <span data-ttu-id="ec858-161">Zu den im Besitz von Entitätstypen darf nicht null sein, es sei denn, sie explizit in eine separate Tabelle vom Besitzer zugeordnet sind</span><span class="sxs-lookup"><span data-stu-id="ec858-161">Navigations to owned entity types cannot be null unless they are explicitly mapped to a separate table from the owner</span></span>
- <span data-ttu-id="ec858-162">Instanzen von Entitätstypen im Besitz des Benutzers können nicht von mehreren Besitzern gemeinsam genutzt werden (Dies ist ein bekanntes Szenario für die Value-Objekte, die mithilfe von Entitätstypen im Besitz nicht implementiert werden kann.)</span><span class="sxs-lookup"><span data-stu-id="ec858-162">Instances of owned entity types cannot be shared by multiple owners (this is a well-known scenario for value objects that cannot be implemented using owned entity types)</span></span>

### <a name="by-design-restrictions"></a><span data-ttu-id="ec858-163">Standardmäßig Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="ec858-163">By-design restrictions</span></span>
- <span data-ttu-id="ec858-164">Sie können keine erstellen ein `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="ec858-164">You cannot create a `DbSet<T>`</span></span>
- <span data-ttu-id="ec858-165">Sie können nicht aufrufen `Entity<T>()` mit einem Datentyp auf, die im Besitz des Benutzers `ModelBuilder`</span><span class="sxs-lookup"><span data-stu-id="ec858-165">You cannot call `Entity<T>()` with an owned type on `ModelBuilder`</span></span>
