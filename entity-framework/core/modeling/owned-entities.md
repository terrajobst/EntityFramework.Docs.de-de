---
title: Entitätstypen - EF Core im Besitz
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: b2d72b08de79939904bf4e726c695440c906a8aa
ms.sourcegitcommit: 06073f8efde97dd5f540dbfb69f574d8380566fe
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/24/2019
ms.locfileid: "67333793"
---
# <a name="owned-entity-types"></a>Entitätstypen im Besitz

>[!NOTE]
> Dieses Feature ist neu in EF Core 2.0.

EF Core können Sie die Modell-Entitätstypen, die nur für Navigationseigenschaften von anderen Entitätstypen angezeigt werden können. Diese heißen _Entitätstypen im Besitz_. Die Entität, die einen eigener Entitätstyp enthält, ist die _Besitzer_.

## <a name="explicit-configuration"></a>Explizite Konfiguration

Im Besitz Entität, die Typen nicht gemäß der Konvention von EF Core im Modell enthalten sind. Sie können die `OwnsOne` -Methode in der `OnModelCreating` oder fügen Sie den Typ mit `OwnedAttribute` (neu in EF Core 2.1) so konfigurieren Sie den Typ als ein eigener Typ.

In diesem Beispiel `StreetAddress` ist ein Typ ohne Identitätseigenschaft. Dieser Typ wird als Eigenschaft des Typs „Order“ verwendet, um die Lieferadresse für eine bestimmte Bestellung anzugeben.

Wir können die `OwnedAttribute` , dies als eine Entität im Besitz, wenn auf die verwiesen wird von einem anderen Entitätstyp behandeln:

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

Es ist auch möglich, mit der `OwnsOne` -Methode in der `OnModelCreating` an, dass die `ShippingAddress` -Eigenschaft ist eine Entität im Besitz von der `Order` Entitätstyp und bei Bedarf zusätzliche Facetten zu konfigurieren.

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

Wenn die `ShippingAddress` -Eigenschaft ist privat in der `Order` geben, können Sie die Version der Zeichenfolge die `OwnsOne` Methode:

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

Finden Sie unter den [vollständigen Beispielprojekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) für weiteren Kontext. 

## <a name="implicit-keys"></a>Implizite Schlüssel

Eigene Typen, die mit konfigurierten `OwnsOne` oder über eine Reference-Navigation ermittelt haben immer eine direkte Beziehung mit dem Besitzer, die aus diesem Grund nicht sie ihre eigenen Werte erforderlich, da die foreign Key-Werte eindeutig sind. Im vorherigen Beispiel die `StreetAddress` Typ muss es sich nicht um eine wichtige Eigenschaft zu definieren.  

Um zu verstehen, wie diese Objekte von EF Core nachverfolgt wird, ist es sinnvoll, die glauben, dass ein Primärschlüssel, als erstellt wird eine [schatteneigenschaft](xref:core/modeling/shadow-properties) für den eigenen Typ. Der Wert des Schlüssels für eine Instanz des eigenen Typs wird der Wert des Schlüssels für die Besitzer-Instanz identisch sein.

## <a name="collections-of-owned-types"></a>Auflistungen von eigenen Typen

>[!NOTE]
> Dieses Feature ist neu in EF Core 2.2.

So konfigurieren Sie eine Auflistung von Typen im Besitz `OwnsMany` sollte verwendet werden, `OnModelCreating`. Der Primärschlüssel wird jedoch nicht gemäß der Konvention konfiguriert werden, muss er explizit angegeben werden. Es ist üblich, verwenden Sie einen komplexen Schlüssel für diese Art von Entitäten, die Integration des Fremdschlüssels für den Besitzer und eine weitere eindeutige Eigenschaft, die auch Schatten Status haben kann:

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

## <a name="mapping-owned-types-with-table-splitting"></a>Zuordnung, die im Besitz von Typen mit tabellenaufteilung

Verwendung von relationalen eigene Datenbanken, als Konvention Verweis als Besitzer der gleichen Tabelle zugeordnet werden. Dies erfordert in der Tabelle in zwei Teilen: einige Spalten zum Speichern der Daten des Besitzers verwendet werden, und einige Spalten zum Speichern von Daten der Entität im Besitz des Benutzers verwendet werden. Dies ist eine Standardfunktion in tabellenaufteilung genannt.

> [!TIP]
> Besitzer des Typen, die mit der tabellenaufteilung gespeichert sein können auf ähnliche Weise verwendet wird, wie komplexe Typen in EF 6 verwendeten werden.

Gemäß der Konvention wird EF Core die Datenbankspalten für die Eigenschaften der eigene Entitätstyp mit dem folgenden Format benennen _Navigation_OwnedEntityProperty_. Aus diesem Grund die `StreetAddress` Eigenschaften werden in der Tabelle "Orders" mit dem Namen 'ShippingAddress_Street' und "ShippingAddress_City" angezeigt.

Sie können den Anfügen der `HasColumnName` Methode, um diese Spalten umzubenennen:

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Freigabe desselben Typs .NET für mehrere im Besitz des Benutzers

Ein eigener Entitätstyp möglich des gleichen Typs .NET als einem anderen eigenen Entitätstyp, daher, dass der .NET Typ möglicherweise nicht mit ausreichend, um ein eigener Typ zu identifizieren.

In diesen Fällen wird die Eigenschaft, die auf die Entität im Besitz des Besitzers der _Definieren der Navigation_ des eigenen Entitätstyps. Aus der Perspektive von EF Core ist die definierende Navigation Teil der Identität für den Typ zusammen mit der .NET-Typ.   

Z. B. in der folgenden Klasse `ShippingAddress` und `BillingAddress` sind beide des gleichen Typs .NET `StreetAddress`:

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

Um zu verstehen, wie EF Core für überwachte Instanzen dieser Objekte unterscheiden, wird, kann es hilfreich sein zu denken, dass die definierende Navigation Teil des Schlüssels der Instanz zusammen mit dem Wert des Schlüssels des Besitzers und der .NET-Typ des eigenen Typs geworden ist.

## <a name="nested-owned-types"></a>Geschachtelte eigene Typen

In diesem Beispiel `OrderDetails` besitzt `BillingAddress` und `ShippingAddress`, sind die `StreetAddress` Typen. Dann besitzt der `DetailedOrder`-Typ `OrderDetails`.

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

Zusätzlich zu den geschachtelten Typen im Besitz des Benutzers ein eigener Typ kann eine reguläre Entität verweisen, kann es sein entweder der Besitzer oder eine andere Entität, solange die Entität im Besitz des Benutzers auf die abhängige Seite ist. Diese Funktion legt die Entitätstypen, die abgesehen von komplexen Typen im Besitz in EF 6 fest.

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

Es ist möglich, eine Verkettung der `OwnsOne` -Methode in einer fluent-Aufruf so konfigurieren Sie dieses Modell:

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

Es ist auch möglich, die gleiche Sache mit `OwnedAttribute` sowohl `OrderDetails` und `StreetAdress`.

## <a name="storing-owned-types-in-separate-tables"></a>Das Speichern von eigene Typen in separaten Tabellen

Im Gegensatz zu EF6 komplexe Typen können auch eigene Typen in einer separaten Tabelle vom Besitzer gespeichert werden. Sie können einfach aufrufen, um die Konvention überschreiben, der einen eigenen Typ als Besitzer der gleichen Tabelle zugeordnet, `ToTable` , und geben Sie einen anderen Tabellennamen an. Im folgende Beispiel ordnet `OrderDetails` und die zwei Adressen in eine separate Tabelle von `DetailedOrder`:

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a>Abfragen von Typen im Besitz

Beim Abfragen des Besitzers werden standardmäßig eigene Typen eingeschlossen. Es ist nicht erforderlich, verwenden die `Include` -Methode, selbst wenn Sie eigenen Typen in einer separaten Tabelle gespeichert sind. Auf der Grundlage des Modells beschrieben vor, die folgende Abfrage erhalten `Order`, `OrderDetails` und die beiden gehören `StreetAddresses` aus der Datenbank:

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a>Einschränkungen

Einige dieser Einschränkungen ist die Grundlage wie die im Besitz des Benutzers Entität Typen arbeiten, aber einige andere werden Einschränkungen, die wir in zukünftigen Versionen entfernt werden kann:

### <a name="by-design-restrictions"></a>Standardmäßig Einschränkungen
- Sie können nicht erstellt werden ein `DbSet<T>` für einen Typ im Besitz des Benutzers
- Sie können nicht aufrufen `Entity<T>()` mit einem Datentyp auf, die im Besitz des Benutzers `ModelBuilder`

### <a name="current-shortcomings"></a>Aktuelle Mängel
- Vererbungshierarchien, die enthalten, das im Besitz, Entitätstypen werden nicht unterstützt
- Referenz-Navigationen hinweg, um eigene Entitätstypen darf nicht null sein, es sei denn, sie explizit in eine separate Tabelle vom Besitzer zugeordnet sind
- Instanzen von Entitätstypen im Besitz des Benutzers können nicht von mehreren Besitzern gemeinsam genutzt werden (Dies ist ein bekanntes Szenario für die Value-Objekte, die mithilfe von Entitätstypen im Besitz nicht implementiert werden kann.)

### <a name="shortcomings-in-previous-versions"></a>Mängel in früheren Versionen
- In EF Core 2.0 gehören zu, dass die Entitätstypen können nicht in abgeleiteten Entitätstypen deklariert werden, es sei denn, die eigenen Entitäten explizit in eine separate Tabelle aus der Besitzerhierarchie zugeordnet sind. Diese Einschränkung wurde in EF Core 2.1 entfernt
- In EF Core 2.0 und 2.1 nur Verweis wurden zu eigene Typen unterstützt. Diese Einschränkung wurde in EF Core 2.2 entfernt
