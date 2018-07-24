---
title: Entitätstypen - EF Core im Besitz
author: julielerman
ms.author: divega
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
ms.technology: entity-framework-core
uid: core/modeling/owned-entities
ms.openlocfilehash: 3eb7480625db4ebc3ce0b7a18d042139f888dab8
ms.sourcegitcommit: 0935ff275ae739243297f5b97eb21414398125c6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2018
ms.locfileid: "39201892"
---
# <a name="owned-entity-types"></a>Entitätstypen im Besitz

>[!NOTE]
> Dieses Feature ist neu in EF Core 2.0.

EF Core können Sie die Modell-Entitätstypen, die nur für Navigationseigenschaften von anderen Entitätstypen angezeigt werden können. Diese heißen _Entitätstypen im Besitz_. Die Entität, die einen eigener Entitätstyp enthält, ist die _Besitzer_.

## <a name="explicit-configuration"></a>Explizite Konfiguration

Im Besitz Entität, die Typen nicht gemäß der Konvention von EF Core im Modell enthalten sind. Sie können die `OwnsOne` -Methode in der `OnModelCreating` oder fügen Sie den Typ mit `OwnedAttribute` (neu in EF Core 2.1) so konfigurieren Sie den Typ als ein eigener Typ.

In diesem Beispiel ist die StreetAddress einen Typ ohne Identitätseigenschaft. Dieser Typ wird als Eigenschaft des Typs „Order“ verwendet, um die Lieferadresse für eine bestimmte Bestellung anzugeben. In `OnModelCreating`, verwenden wir die `OwnsOne` Methode, um anzugeben, dass die ShippingAddress Eigenschaft einer Entität im Besitz des Typs "Order".

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

Wenn die ShippingAddress-Eigenschaft in den Auftragstyp privat ist, können Sie die Version der Zeichenfolge die `OwnsOne` Methode:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

In diesem Beispiel verwenden wir die `OwnedAttribute` um das gleiche Ziel zu erreichen:

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

## <a name="implicit-keys"></a>Implizite Schlüssel

In EF Core 2.0 und 2.1 können nur die verweisnavigationseigenschaften auf eigene Typen verweisen. Auflistungen von eigenen Typen werden nicht unterstützt. Dieser Verweis gehört-Typen haben immer eine direkte Beziehung mit dem Besitzer, daher nicht benötigten ihre eigenen Werte. Im vorherigen Beispiel muss der StreetAddress-Typ nicht um eine Schlüsseleigenschaft definiert.  

Um zu verstehen, wie diese Objekte von EF Core nachverfolgt wird, ist es sinnvoll, die glauben, dass ein Primärschlüssel, als erstellt wird eine [schatteneigenschaft](xref:core/modeling/shadow-properties) für den eigenen Typ. Der Wert des Schlüssels für eine Instanz des eigenen Typs wird der Wert des Schlüssels für die Besitzer-Instanz identisch sein.      

## <a name="mapping-owned-types-with-table-splitting"></a>Zuordnung, die im Besitz von Typen mit tabellenaufteilung

Verwendung von relationalen Datenbanken werden standardmäßig eigene Typen als Besitzer der gleichen Tabelle zugeordnet. Dies erfordert in der Tabelle in zwei Teilen: einige Spalten zum Speichern der Daten des Besitzers verwendet werden, und einige Spalten zum Speichern von Daten der Entität im Besitz des Benutzers verwendet werden. Dies ist eine Standardfunktion in tabellenaufteilung genannt.

> [!TIP]
> Besitzer des Typen, die mit der tabellenaufteilung gespeichert sein können wie komplexe Typen in EF 6 verwendeten sind sehr ähnlich wie verwendet.

Gemäß der Konvention wird EF Core die Datenbankspalten für die Eigenschaften der eigene Entitätstyp mit dem folgenden Format benennen _EntityProperty_OwnedEntityProperty_. Aus diesem Grund werden die StreetAddress-Eigenschaften in der Orders-Tabelle mit den Namen ShippingAddress_Street und ShippingAddress_City angezeigt.

Sie können den Anfügen der `HasColumnName` Methode, um diese Spalten umzubenennen. In der Fall, in denen eine öffentliche Eigenschaft von StreetAddress ist, wäre die Zuordnungen

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Freigabe desselben Typs .NET für mehrere im Besitz des Benutzers

Ein eigener Entitätstyp möglich des gleichen Typs .NET als einem anderen eigenen Entitätstyp, daher, dass der .NET Typ möglicherweise nicht mit ausreichend, um ein eigener Typ zu identifizieren.

In diesen Fällen wird die Eigenschaft, die auf die Entität im Besitz des Besitzers der _Definieren der Navigation_ des eigenen Entitätstyps. Aus der Perspektive von EF Core ist die definierende Navigation Teil der Identität für den Typ zusammen mit der .NET-Typ.   

Beispielsweise sind in der folgenden Klasse Shippingaddress- und BillingAddress des gleichen .NET-Typs, StreetAddress:

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

Um zu verstehen, wie EF Core für überwachte Instanzen dieser Objekte unterscheiden, wird, kann es hilfreich sein zu denken, dass die definierende Navigation Teil des Schlüssels der Instanz zusammen mit dem Wert des Schlüssels des Besitzers und der .NET-Typ des eigenen Typs geworden ist.

## <a name="nested-owned-types"></a>Geschachtelte eigene Typen

In diesem Beispiel besitzt die OrderDetails BillingAddress und ShippingAddress, die beide StreetAddress-Typen sind. Wird der Auftrag vom Typ OrderDetails gehören.

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

Es ist möglich, eine Verkettung der `OwnsOne` -Methode in eine fluentzuordnung so konfigurieren Sie dieses Modell:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

Es ist möglich, die gleiche Sache mit `OwnedAttribute` auf OrderDetails und StreetAdress.

Zusätzlich zu den geschachtelten Typen im Besitz des Benutzers kann ein eigener Typ eine normale Entität verweisen. Im folgenden Beispiel wird die Land eine reguläre, nicht im Besitz der Entität:

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

Diese Funktion legt die Entitätstypen, die abgesehen von komplexen Typen im Besitz in EF 6 fest.

## <a name="storing-owned-types-in-separate-tables"></a>Das Speichern von eigene Typen in separaten Tabellen

Im Gegensatz zu EF6 komplexe Typen können auch eigene Typen in einer separaten Tabelle vom Besitzer gespeichert werden. Sie können einfach aufrufen, um die Konvention überschreiben, der einen eigenen Typ als Besitzer der gleichen Tabelle zugeordnet, `ToTable` , und geben Sie einen anderen Tabellennamen an. Im folgende Beispiel wird in eine separate Tabelle OrderDetails und die zwei Adressen von Reihenfolge zugeordnet:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a>Abfragen von Typen im Besitz

Beim Abfragen des Besitzers werden standardmäßig eigene Typen eingeschlossen. Es ist nicht erforderlich, verwenden die `Include` -Methode, selbst wenn Sie eigenen Typen in einer separaten Tabelle gespeichert sind. Basierend auf das Modell beschrieben vor, wird die folgende Abfrage Order "," OrderDetails "und" die beiden im Besitz des Benutzers StreetAddresses für alle ausstehenden Aufträge aus der Datenbank abzurufen:

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a>Einschränkungen

Einige dieser Einschränkungen ist die Grundlage wie die im Besitz des Benutzers Entität Typen arbeiten, aber einige andere werden Einschränkungen, die wir in zukünftigen Versionen entfernt werden kann:

### <a name="shortcomings-in-previous-versions"></a>Mängel in früheren Versionen
- In EF Core 2.0 gehören zu, dass die Entitätstypen können nicht in abgeleiteten Entitätstypen deklariert werden, es sei denn, die eigenen Entitäten explizit in eine separate Tabelle aus der Besitzerhierarchie zugeordnet sind. Diese Einschränkung wurde in EF Core 2.1 entfernt

### <a name="current-shortcomings"></a>Aktuelle Mängel
- Vererbungshierarchien, die enthalten, das im Besitz, Entitätstypen werden nicht unterstützt
- Entitätstypen im Besitz des Benutzers können nicht auf durch eine auflistungsnavigationseigenschaft (nur Referenz, die derzeit Navigationen unterstützten) verwiesen werden
- Zu den im Besitz von Entitätstypen darf nicht null sein, es sei denn, sie explizit in eine separate Tabelle vom Besitzer zugeordnet sind
- Instanzen von Entitätstypen im Besitz des Benutzers können nicht von mehreren Besitzern gemeinsam genutzt werden (Dies ist ein bekanntes Szenario für die Value-Objekte, die mithilfe von Entitätstypen im Besitz nicht implementiert werden kann.)

### <a name="by-design-restrictions"></a>Standardmäßig Einschränkungen
- Sie können keine erstellen ein `DbSet<T>`
- Sie können nicht aufrufen `Entity<T>()` mit einem Datentyp auf, die im Besitz des Benutzers `ModelBuilder`
