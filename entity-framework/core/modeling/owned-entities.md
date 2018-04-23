---
title: Im Besitz Entitätstypen - EF Core
author: julielerman
ms.author: divega
ms.date: 2/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
ms.technology: entity-framework-core
uid: core/modeling/owned-entities
ms.openlocfilehash: f2f05499a3e3494f420d916df2db19667a6f1e29
ms.sourcegitcommit: 26f33758c47399ae933f22fec8e1d19fa7d2c0b7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
---
# <a name="owned-entity-types"></a>Im Besitz befindlichen Entitätstypen

>[!NOTE]
> Dieses Feature ist neu in EF Core 2.0.

EF Core ermöglicht Ihnen das Modell Entitätstypen, die immer nur auf andere Entitätstypen Navigationseigenschaften angezeigt werden können. Diese heißen _im Besitz von Entitätstypen_. Die Entität, die im Besitz befindlichen Entitätstyp ist seine _Besitzer_.

## <a name="explicit-configuration"></a>Explizite Konfiguration

Besitz befindliche Entität, die Typen nie gemäß der Konvention von EF Core im Modell enthalten sind. Können Sie die `OwnsOne` Methode im `OnModelCreating` oder kommentieren Sie den Typ mit `OwnedAttribute` (neu in EF Core 2.1) so konfigurieren Sie den Typ als Besitzer Typ.

In diesem Beispiel ist StreetAddress ein Typ mit keine Identity-Eigenschaft. Dieser Typ wird als Eigenschaft des Typs „Order“ verwendet, um die Lieferadresse für eine bestimmte Bestellung anzugeben. In `OnModelCreating`, verwenden wir die `OwnsOne` Methode, um anzugeben, dass die ShippingAddress-Eigenschaft einer Entität gehören, der den Auftragstyp ist.

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

Wenn die Eigenschaft ShippingAddress in den Auftragstyp privat ist, können Sie die Version der Zeichenfolge die `OwnsOne` Methode:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(typeof(StreetAddress), "ShippingAddress");
```

In diesem Beispiel verwenden wir die `OwnedAttribute` auf das gleiche Ziel zu erreichen:

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

EF Core 2.0 und 2.1 können nur den Verweis Navigationseigenschaften im Besitz befindlichen Typen zeigen. Sammlungen im Besitz befindlichen Typen werden nicht unterstützt. Dieser Verweis gehört immer aufweisen: 1-Beziehung mit dem Besitzer, daher nicht benötigte ihren eigenen Werten. Im vorherigen Beispiel muss der StreetAddress-Typ nicht um eine Schlüsseleigenschaft definiert.  

In der Reihenfolge für das Verständnis, wie diese Objekte von EF Core verfolgt nach, es ist sinnvoll, Sie glauben, dass ein Primärschlüssel, als erstellt wird ein [Shadowing Eigenschaft](xref:core/modeling/shadow-properties) für den Besitzer Typ. Der Wert des Schlüssels einer Instanz des Typs im Besitz befindlichen wird den Wert des Schlüssels der Besitzer Instanz identisch sein.      

## <a name="mapping-owned-types-with-table-splitting"></a>Zuordnung, die im Besitz von Typen mit Teilen der Tabelle

Bei relationalen Datenbanken, werden gemäß der Konvention, die im Besitz von Typen als Besitzer der gleichen Tabelle zugeordnet. Dies erfordert die Tabelle in zwei Teilen: einige Spalten zum Speichern der Daten des Besitzers verwendet werden, und einige Spalten zum Speichern von Daten, die im Besitz befindlichen Entität verwendet werden. Dies ist ein gängiges Feature als Tabelle Aufteilung bezeichnet.

> [!TIP]
> Besitzer können Typen mit Teilen der Tabelle gespeichert werden verwendet sehr ähnlich wie komplexe Typen in EF6 verwendet werden.

Gemäß der Konvention EF Core die Datenbankspalten für die Eigenschaften, die im Besitz befindlichen Entitätstyps, die nach dem Muster benennt _EntityProperty_OwnedEntityProperty_. Aus diesem Grund werden die StreetAddress-Eigenschaften in der Orders-Tabelle mit den Namen ShippingAddress_Street und ShippingAddress_City angezeigt.

Sie können den Anfügen der `HasColumnName` Methode, um diese Spalten umbenennen. In der Fall, in dem StreetAddress eine öffentliche Eigenschaft ist, wäre die Zuordnungen

``` csharp
modelBuilder.Entity<Order>().OwnsOne(
    o => o.ShippingAddress,
    sa =>
        {
            sa.Property(p=>p.Street).HasColumnName("ShipsToStreet");
            sa.Property(p=>p.City).HasColumnName("ShipsToCity");
        });
```

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Freigeben von .NET Typparameter für mehrere Besitzer

Entitätstyp im Besitz befindlichen möglich desselben Typs .NET als einen anderen Besitzer Entitätstyp daher, dass der .NET Typ möglicherweise nicht mit genug, um ein im Besitz befindlichen Typ zu identifizieren.

In diesen Fällen wird die Eigenschaft, die vom Besitzer auf die im Besitz befindlichen Entität zeigen die _Navigation definieren_ des Entitätstyps Eigentum ist. Aus der Perspektive des EF-Kerns stammt die definierende Navigation die Typidentität zusammen mit den Typ .NET.   

Beispielsweise sind in der folgenden Klasse ShippingAddress und BillingAddress vom gleichen .NET-Typ StreetAddress:

``` csharp
public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
    public StreetAddress BillingAddress { get; set; }
}
```

Um zu erfahren, wie EF Core überwachte Instanzen dieser Objekte unterschieden wird, kann es nützlich sein, glauben, dass die definierende Navigation Teil des Schlüssels der Instanz zusammen mit dem Wert des Schlüssels des Besitzers und der .NET Typ des im Besitz befindlichen geworden ist.

## <a name="nested-owned-types"></a>Geschachtelte Typen im Besitz befindlichen

In diesem Beispiel besitzt OrderDetails BillingAddress und ShippingAddress, bei denen beide StreetAddress-Typen handelt. Klicken Sie dann gehört OrderDetails den Auftragstyp.

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

Es ist möglich, die Kette der `OwnsOne` Methode in einer fluent-Zuordnung zu diesem Modell zu konfigurieren:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    });
```

Es ist möglich, die gleiche Bedeutung mit erzielen `OwnedAttribute` OrderDetails und StreetAdress.

Zusätzlich zu den geschachtelten Typen besitzt kann ein Typ im Besitz eine gewöhnlichen Entität verweisen. Im folgenden Beispiel wird Land einer gewöhnlichen Entität mit (d. h. nicht im Besitz der):

``` csharp
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
    public Country Country { get; set; }
}
```

Diese Funktion wird im Besitz befindlichen Entitätstypen abgesehen von komplexen Typen im EF6 festgelegt.

## <a name="storing-owned-types-in-separate-tables"></a>Speichern von Typen in separaten Tabellen gehören

Im Gegensatz zu EF6 komplexe Typen können auch im Besitz befindlichen Typen in einer separaten Tabelle vom Besitzer gespeichert werden. Sie können einfach aufrufen, um der Konvention zu überschreiben, die der gleichen Tabelle wie der Besitzer ein Eigentümer Typs zuordnet, `ToTable` , und geben Sie einen anderen Tabellennamen. Im folgende Beispiel wird in eine separate Tabelle OrderDetails und den beiden Adressen von Reihenfolge zugeordnet:

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, od =>
    {
        od.OwnsOne(c => c.BillingAddress);
        od.OwnsOne(c => c.ShippingAddress);
    }).ToTable("OrderDetails");
```

## <a name="querying-owned-types"></a>Abfragen von im Besitz befindlichen Typen

Beim Abfragen des Besitzers werden standardmäßig eigene Typen eingeschlossen. Es ist nicht notwendig, verwenden Sie die `Include` -Methode, auch wenn die im Besitz befindlichen Typen in einer separaten Tabelle gespeichert sind. Je nachdem welches Modell beschrieben vor, ruft die folgende Abfrage Order "," OrderDetails "und" der zwei im Besitz befindlichen StreeAddresses für alle ausstehenden Aufträge aus der Datenbank:

``` csharp
var orders = context.Orders.Where(o => o.Status == OrderStatus.Pending);
```  

## <a name="limitations"></a>Einschränkungen

Hier sind einige Einschränkungen im Besitz befindlichen Entitätstypen. Diese Einschränkungen sind grundlegende wie im Besitz befindlichen Typen arbeiten, aber andere Point-in-Time-Einschränkungen, die wir in zukünftigen Versionen entfernt möchten werden:

### <a name="current-shortcomings"></a>Aktuelle Mängel
- Vererbung von im Besitz befindlichen Typen wird nicht unterstützt.
- Im Besitz befindlichen Typen können nicht auf eine auflistungsnavigationseigenschaft zeigt werden
- Da sie die Tabelle durch Aufteilen verwenden gehören Standard, dass Typen auch die folgenden Einschränkungen verfügen, es sei denn, die explizit einer anderen Tabelle zugeordnet:
   - Sie können nicht von einem abgeleiteten Typ Benutzer
   - Die definierenden Navigationseigenschaft kann nicht festgelegt werden auf Null (d. h. gehören Typen in der gleichen Tabelle immer erforderlich)

### <a name="by-design-restrictions"></a>Standardmäßig Einschränkungen
- Sie können nicht erstellt werden ein `DbSet<T>`
- Sie können nicht aufrufen `Entity<T>()` mit einem Datentyp auf, die im Besitz befindlichen `ModelBuilder`
