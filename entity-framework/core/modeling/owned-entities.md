---
title: Eigene Entitäts Typen-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 02/26/2018
ms.assetid: 2B0BADCE-E23E-4B28-B8EE-537883E16DF3
uid: core/modeling/owned-entities
ms.openlocfilehash: f69bae2de28156876e0aa57376b5dfac053adb9c
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149131"
---
# <a name="owned-entity-types"></a>Eigene Entitäts Typen

>[!NOTE]
> Diese Funktion ist neu in EF Core 2,0.

EF Core ermöglicht es Ihnen, Entitäts Typen zu modellieren, die nur in den Navigations Eigenschaften anderer Entitäts Typen angezeigt werden können. Diese werden als _eigene Entitäts Typen_bezeichnet. Die Entität, die einen eigenen Entitätstyp enthält, ist Ihr _Besitzer_.

Besitzende Entitäten sind im wesentlichen Teil des Besitzers und können nicht ohne Sie vorhanden sein, Sie sind konzeptionell ähnlich wie [Aggregate](https://martinfowler.com/bliki/DDD_Aggregate.html).

## <a name="explicit-configuration"></a>Explizite Konfiguration

Eigene Entitäts Typen werden nie von EF Core im Modell nach Konvention eingeschlossen. Sie können `OwnsOne` die-Methode in `OnModelCreating` oder mit dem-Typ mit `OwnedAttribute` versehen (neu in EF Core 2,1), um den Typ als eigenen Typ zu konfigurieren.

In diesem Beispiel `StreetAddress` ist ein Typ ohne Identity-Eigenschaft. Dieser Typ wird als Eigenschaft des Typs „Order“ verwendet, um die Lieferadresse für eine bestimmte Bestellung anzugeben.

Wir können das `OwnedAttribute` verwenden, um es als eigene Entität zu behandeln, wenn von einem anderen Entitätstyp darauf verwiesen wird:

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

Es ist auch möglich, mit der `OwnsOne` -Methode `OnModelCreating` in anzugeben, dass `ShippingAddress` es sich bei der Eigenschaft um eine `Order` Entität des Entitäts Typs handelt, und um ggf. weitere Facetten zu konfigurieren.

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

Wenn die `ShippingAddress` Eigenschaft `Order` im-Typ privat ist, können Sie `OwnsOne` die Zeichen folgen Version der-Methode verwenden:

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

Weitere Informationen finden Sie im [vollständigen Beispiel Projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) . 

## <a name="implicit-keys"></a>Implizite Schlüssel

Besitzer Typen, die `OwnsOne` mit einer Verweis Navigation konfiguriert oder ermittelt wurden, haben immer eine eins-zu-eins-Beziehung mit dem Besitzer und benötigen daher keine eigenen Schlüsselwerte, da die Fremdschlüssel Werte eindeutig sind. Im vorherigen Beispiel muss der `StreetAddress` -Typ keine Schlüsseleigenschaft definieren.  

Um zu verstehen, wie EF Core diese Objekte verfolgt, ist es hilfreich zu wissen, dass ein Primärschlüssel als [Schatten Eigenschaft](xref:core/modeling/shadow-properties) für den eigenen Typ erstellt wird. Der Wert des Schlüssels einer Instanz des eigenen Typs ist mit dem Wert des Schlüssels der Besitzer Instanz identisch.

## <a name="collections-of-owned-types"></a>Auflistungen von eigenen Typen

> [!NOTE]
> Dieses Feature ist neu in EF Core 2.2.

Verwenden `OwnsMany` Sie zum Konfigurieren einer Sammlung von eigenen Typen `OnModelCreating`in.

Für eigene Typen ist ein Primärschlüssel erforderlich. Wenn für den .NET-Typ keine guten Kandidaten Eigenschaften vorhanden sind, können EF Core versuchen, eine zu erstellen. Wenn jedoch eigene Typen durch eine Auflistung definiert werden, reicht es nicht aus, eine Schatten Eigenschaft zu erstellen, die sowohl als Fremdschlüssel für den Besitzer als auch als Primärschlüssel der eigenen Instanz fungiert. dies `OwnsOne`geschieht wie folgt: Es können mehrere eigene Typinstanzen für Folgendes vorhanden sein: Jeder Besitzer und somit ist der Schlüssel des Besitzers nicht ausreichen, um für jede Instanz eine eindeutige Identität bereitzustellen.

Die beiden einfachsten Lösungen hierfür sind:
- Definieren eines Ersatz Primärschlüssels für eine neue Eigenschaft, unabhängig vom Fremdschlüssel, der auf den Besitzer zeigt. Die enthaltenen Werte müssen für alle Besitzer eindeutig sein (z. b. wenn das {1} übergeordnete {1}Element über ein {2} untergeordnetes Element {1}verfügt, das übergeordnete Element kein untergeordnetes Element haben kann), sodass der Wert keine inhärente Bedeutung hat Da der Fremdschlüssel nicht Teil des Primärschlüssels ist, können seine Werte geändert werden, sodass Sie ein untergeordnetes Element von einem übergeordneten Element zu einem anderen verschieben können. Dies ist jedoch in der Regel gegen Aggregat Semantik.
- Verwenden des fremd Schlüssels und einer zusätzlichen Eigenschaft als zusammengesetzten Schlüssel. Der zusätzliche Eigenschafts Wert muss nun nur für ein bestimmtes übergeordnetes Element eindeutig sein (wenn {1} das {1,1} übergeordnete Element {2} über ein untergeordnetes {2,1}Element verfügt, kann das übergeordnete Element nach wie vor über Durch den Fremdschlüssel Teil des Primärschlüssels wird die Beziehung zwischen dem Besitzer und der Entität, die im Besitz des Primärschlüssels ist, unveränderlich und die Aggregat Semantik ist besser. Dies geschieht EF Core standardmäßig.

In diesem Beispiel verwenden wir die `Distributor` -Klasse:

[!code-csharp[Distributor](../../../samples/core/Modeling/OwnedEntities/Distributor.cs?name=Distributor)]

Standardmäßig ist der Primärschlüssel, der für den eigenen Typ verwendet `ShippingCenters` wird, auf den `("DistributorId", "Id")` über `"DistributorId"` die Navigations Eigenschaft verwiesen `"Id"` wird, wobei `int` der FK und ein eindeutiger Wert ist.

So konfigurieren Sie einen anderen PK `HasKey`-Befehl:

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

> [!NOTE]
> Bevor EF Core 3,0 `WithOwner()` -Methode nicht vorhanden war, sollte dieser-Befehl entfernt werden.

## <a name="mapping-owned-types-with-table-splitting"></a>Zuordnung eigener Typen mit Tabellen Aufteilung

Bei der Verwendung von relationalen Datenbanken werden Verweis eigene Typen standardmäßig derselben Tabelle wie der Besitzer zugeordnet. Dies erfordert das Aufteilen der Tabelle in zwei: einige Spalten werden zum Speichern der Daten des Besitzers verwendet, und einige Spalten werden zum Speichern von Daten der Entität verwendet. Dies ist ein gängiges Feature, das als [Tabellen Aufteilung](table-splitting.md)bezeichnet wird.

Standardmäßig werden EF Core die Daten Bank Spalten für die Eigenschaften des eigenen Entitäts Typs nach dem Muster _Navigation_OwnedEntityProperty_benennen. Daher werden `StreetAddress` die Eigenschaften in der Tabelle "Orders" mit den Namen "ShippingAddress_Street" und "ShippingAddress_City" angezeigt.

Sie können die `HasColumnName` -Methode verwenden, um diese Spalten umzubenennen:

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Gemeinsame Nutzung desselben .net-Typs für mehrere eigene Typen

Ein eigener Entitätstyp kann vom gleichen .NET-Typ wie ein anderer eigener Entitätstyp sein. Daher reicht der .NET-Typ möglicherweise nicht aus, um einen eigenen Typ zu identifizieren.

In diesen Fällen wird die-Eigenschaft, die vom Besitzer auf die eigene Entität zeigt, zur _definierenden Navigation_ des eigenen Entitäts Typs. Aus Sicht der EF Core ist die definierende Navigation Teil der Typidentität neben dem .NET-Typ.   

Beispielsweise sind in der folgenden Klasse `ShippingAddress` und `BillingAddress` beide identisch mit `StreetAddress`dem .NET-Typ:

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

Um zu verstehen, wie EF Core überwachte Instanzen dieser Objekte unterscheiden, kann es hilfreich sein zu wissen, dass die definierende Navigation neben dem Wert des Schlüssels des Besitzers und dem .NET-Typ des eigenen Typs zu einem Teil des Schlüssels der Instanz geworden ist.

## <a name="nested-owned-types"></a>Im Besitz befindliche Typen

In diesem Beispiel `OrderDetails` `BillingAddress` gehören und `ShippingAddress`, bei denen es `StreetAddress` sich um beide Typen handelt. Dann besitzt der `DetailedOrder`-Typ `OrderDetails`.

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

Zusätzlich zu den Typen, die im Besitz eines Typs sind, kann ein eigener Typ auf eine reguläre Entität verweisen, es kann sich um den Besitzer oder eine andere Entität handeln, solange sich die zugehörige Entität auf der abhängigen Seite befindet. Diese Funktion legt eigene Entitäts Typen außer komplexen Typen in EF6 fest.

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

Es ist möglich, die `OwnsOne` -Methode in einem fließend aufzurufenden aufzurufen, um dieses Modell zu konfigurieren:

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

Beachten Sie `WithOwner` den-Befehl, der zum Konfigurieren der Navigations Eigenschaft verwendet wird, die auf den Besitzer zeigt.

Das Ergebnis kann mithilfe von sowohl `OwnedAttribute` `OrderDetails` für als auch `StreetAdress`durch erzielt werden.

## <a name="storing-owned-types-in-separate-tables"></a>Speichern von eigenen Typen in separaten Tabellen

Im Gegensatz zu komplexen EF6-Typen können eigene Typen in einer separaten Tabelle vom Besitzer gespeichert werden. Um die Konvention zu überschreiben, die einen eigenen Typ derselben Tabelle wie der Besitzer zuordnet, können Sie einfach aufzurufen `ToTable` und einen anderen Tabellennamen angeben. Im folgenden Beispiel werden `OrderDetails` und die beiden Adressen einer separaten Tabelle von `DetailedOrder`zugeordnet:

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

## <a name="querying-owned-types"></a>Abfragen von eigenen Typen

Beim Abfragen des Besitzers werden standardmäßig eigene Typen eingeschlossen. Es ist nicht erforderlich, die `Include` -Methode zu verwenden, auch wenn die eigenen Typen in einer separaten Tabelle gespeichert werden. Basierend auf dem zuvor beschriebenen Modell wird `Order` `OrderDetails` die folgende Abfrage und die beiden im Besitz `StreetAddresses` der-Datenbank angezeigt:

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a>Einschränkungen

Einige dieser Einschränkungen sind grundlegend für die Funktionsweise von Entitäts Typen im Besitz, einige andere sind jedoch Einschränkungen, die in zukünftigen Versionen möglicherweise entfernt werden könnten:

### <a name="by-design-restrictions"></a>Entwurfs Einschränkungen
- Sie können keinen `DbSet<T>` für einen eigenen Typ erstellen.
- Der-Typ `Entity<T>()` kann nicht mit einem eigenen Typ aufgerufen werden.`ModelBuilder`

### <a name="current-shortcomings"></a>Aktuelle Mängel
- Vererbungs Hierarchien, die eigene Entitäts Typen einschließen, werden nicht unterstützt
- Verweis Navigation auf eigene Entitäts Typen darf nicht NULL sein, es sei denn, Sie sind explizit einer separaten Tabelle vom Besitzer zugeordnet.
- Instanzen von eigenen Entitäts Typen können nicht von mehreren Besitzern gemeinsam genutzt werden (Dies ist ein bekanntes Szenario für Wert Objekte, das nicht mit Entitäts Typen im Besitz von Entitäten implementiert werden kann).

### <a name="shortcomings-in-previous-versions"></a>Mängel in früheren Versionen
- In EF Core 2,0 können Navigationen zu besitzenden Entitäts Typen nicht in abgeleiteten Entitäts Typen deklariert werden, es sei denn, die im Besitz befindlichen Entitäten werden explizit einer separaten Tabelle der Besitzer Hierarchie zugeordnet. Diese Einschränkung wurde in EF Core 2,1 entfernt.
- In EF Core 2,0 und 2,1 wurden nur Verweis Navigation auf eigene Typen unterstützt. Diese Einschränkung wurde in EF Core 2,2 entfernt.
