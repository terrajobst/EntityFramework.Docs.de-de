---
title: Eigene Entitäts Typen-EF Core
description: Konfigurieren eigener Entitäts Typen oder Aggregate bei Verwendung von Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/owned-entities
ms.openlocfilehash: 7b6d1b3bccbfceb85f03a580ba03a45984d29c74
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824593"
---
# <a name="owned-entity-types"></a>Nicht eigenständige Entitätstypen

> [!NOTE]
> Diese Funktion ist neu in EF Core 2,0.

EF Core ermöglicht es Ihnen, Entitäts Typen zu modellieren, die nur in den Navigations Eigenschaften anderer Entitäts Typen angezeigt werden können. Diese werden als _eigene Entitäts Typen_bezeichnet. Die Entität, die einen eigenen Entitätstyp enthält, ist Ihr _Besitzer_.

Besitzende Entitäten sind im wesentlichen Teil des Besitzers und können nicht ohne Sie vorhanden sein, Sie sind konzeptionell ähnlich wie [Aggregate](https://martinfowler.com/bliki/DDD_Aggregate.html). Dies bedeutet, dass der eigene Typ definitionsgemäß auf der abhängigen Seite der Beziehung mit dem Besitzer ist.

## <a name="explicit-configuration"></a>Explizite Konfiguration

Eigene Entitäts Typen werden nie von EF Core im Modell nach Konvention eingeschlossen. Sie können die `OwnsOne`-Methode in `OnModelCreating` verwenden oder den Typ mit `OwnedAttribute` (neu in EF Core 2,1) kommentieren, um den Typ als eigenen Typ zu konfigurieren.

In diesem Beispiel ist `StreetAddress` ein Typ ohne Identity-Eigenschaft. Dieser Typ wird als Eigenschaft des Typs „Order“ verwendet, um die Lieferadresse für eine bestimmte Bestellung anzugeben.

Sie können den `OwnedAttribute` verwenden, um ihn als eigene Entität zu behandeln, wenn er von einem anderen Entitätstyp referenziert wird:

[!code-csharp[StreetAddress](../../../samples/core/Modeling/OwnedEntities/StreetAddress.cs?name=StreetAddress)]

[!code-csharp[Order](../../../samples/core/Modeling/OwnedEntities/Order.cs?name=Order)]

Es ist auch möglich, die `OwnsOne`-Methode in `OnModelCreating` zu verwenden, um anzugeben, dass die `ShippingAddress`-Eigenschaft eine eigene Entität des `Order` Entitäts Typs ist, und um ggf. weitere Facetten zu konfigurieren.

[!code-csharp[OwnsOne](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOne)]

Wenn die `ShippingAddress`-Eigenschaft im `Order`-Typ privat ist, können Sie die Zeichen folgen Version der `OwnsOne`-Methode verwenden:

[!code-csharp[OwnsOneString](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneString)]

Weitere Informationen finden Sie im [vollständigen Beispiel Projekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/OwnedEntities) .

## <a name="implicit-keys"></a>Implizite Schlüssel

Besitzer Typen, die mit `OwnsOne` konfiguriert oder über eine Verweis Navigation ermittelt wurden, verfügen immer über eine 1:1-Beziehung mit dem Besitzer und benötigen daher keine eigenen Schlüsselwerte, da die Fremdschlüssel Werte eindeutig sind. Im vorherigen Beispiel muss der `StreetAddress` Typ keine Schlüsseleigenschaft definieren.  

Um zu verstehen, wie EF Core diese Objekte verfolgt, ist es hilfreich zu wissen, dass ein Primärschlüssel als [Schatten Eigenschaft](xref:core/modeling/shadow-properties) für den eigenen Typ erstellt wird. Der Wert des Schlüssels einer Instanz des eigenen Typs ist mit dem Wert des Schlüssels der Besitzer Instanz identisch.

## <a name="collections-of-owned-types"></a>Auflistungen von eigenen Typen

> [!NOTE]
> Dieses Feature ist neu in EF Core 2.2.

Verwenden Sie `OwnsMany` in `OnModelCreating`, um eine Sammlung von eigenen Typen zu konfigurieren.

Für eigene Typen ist ein Primärschlüssel erforderlich. Wenn für den .NET-Typ keine guten Kandidaten Eigenschaften vorhanden sind, können EF Core versuchen, eine zu erstellen. Wenn jedoch eigene Typen durch eine Auflistung definiert werden, reicht es nicht aus, eine Schatten Eigenschaft zu erstellen, die sowohl als Fremdschlüssel für den Besitzer als auch als Primärschlüssel der eigenen Instanz fungiert, wie es bei `OwnsOne`der Fall ist: Es können mehrere eigene Typinstanzen für jede der Besitzer. Daher reicht der Schlüssel des Besitzers nicht aus, um eine eindeutige Identität für jede eigene Instanz bereitzustellen.

Die beiden einfachsten Lösungen hierfür sind:

- Definieren eines Ersatz Primärschlüssels für eine neue Eigenschaft, unabhängig vom Fremdschlüssel, der auf den Besitzer zeigt. Die enthaltenen Werte müssen für alle Besitzer eindeutig sein (z. b. wenn die übergeordnete {1} über untergeordnete {1}verfügen, die übergeordnete {2} nicht über untergeordnete {1}verfügen), sodass der Wert keine inhärente Bedeutung hat. Da der Fremdschlüssel nicht Teil des Primärschlüssels ist, können seine Werte geändert werden, sodass Sie ein untergeordnetes Element von einem übergeordneten Element zu einem anderen verschieben können. Dies ist jedoch in der Regel gegen Aggregat Semantik.
- Verwenden des fremd Schlüssels und einer zusätzlichen Eigenschaft als zusammengesetzten Schlüssel. Der zusätzliche Eigenschafts Wert muss nun nur für ein bestimmtes übergeordnetes Element eindeutig sein (wenn der übergeordnete {1} also über eine untergeordnete {1,1} verfügt, kann der übergeordnete {2} weiterhin untergeordnete {2,1}) haben. Durch den Fremdschlüssel Teil des Primärschlüssels wird die Beziehung zwischen dem Besitzer und der Entität, die im Besitz des Primärschlüssels ist, unveränderlich und die Aggregat Semantik ist besser. Dies geschieht EF Core standardmäßig.

In diesem Beispiel verwenden wir die `Distributor`-Klasse:

[!code-csharp[Distributor](../../../samples/core/Modeling/OwnedEntities/Distributor.cs?name=Distributor)]

Standardmäßig wird der Primärschlüssel, der für den eigenen Typ verwendet wird, auf den über die `ShippingCenters` Navigations Eigenschaft verwiesen wird, `("DistributorId", "Id")`, wobei `"DistributorId"` der FK und `"Id"` ein eindeutiger `int` Wert ist.

So konfigurieren Sie einen anderen PK-Rückruf `HasKey`:

[!code-csharp[OwnsMany](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsMany)]

> [!NOTE]
> Bevor EF Core 3,0 `WithOwner()`-Methode nicht vorhanden war, sollte dieser-Befehl entfernt werden. Außerdem wurde der Primärschlüssel nicht automatisch erkannt, sodass er immer angegeben wurde.

## <a name="mapping-owned-types-with-table-splitting"></a>Zuordnung eigener Typen mit Tabellen Aufteilung

Bei der Verwendung von relationalen Datenbanken werden Verweis eigene Typen standardmäßig derselben Tabelle wie der Besitzer zugeordnet. Dies erfordert das Aufteilen der Tabelle in zwei: einige Spalten werden zum Speichern der Daten des Besitzers verwendet, und einige Spalten werden zum Speichern von Daten der Entität verwendet. Dies ist ein gängiges Feature, das als [Tabellen Aufteilung](table-splitting.md)bezeichnet wird.

Standardmäßig benennen EF Core die Daten Bank Spalten für die Eigenschaften des eigenen Entitäts Typs nach dem Muster _Navigation_OwnedEntityProperty_. Daher werden die `StreetAddress` Eigenschaften in der Tabelle Orders mit den Namen ' ShippingAddress_Street ' und ' ShippingAddress_City ' angezeigt.

Sie können die `HasColumnName`-Methode verwenden, um diese Spalten umzubenennen:

[!code-csharp[ColumnNames](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=ColumnNames)]

> [!NOTE]
> Die meisten der normalen Entitätstyp-Konfigurations Methoden wie [Ignore](/dotnet/api/microsoft.entityframeworkcore.metadata.builders.ownednavigationbuilder.ignore) können auf die gleiche Weise aufgerufen werden.

## <a name="sharing-the-same-net-type-among-multiple-owned-types"></a>Gemeinsame Nutzung desselben .net-Typs für mehrere eigene Typen

Ein eigener Entitätstyp kann vom gleichen .NET-Typ wie ein anderer eigener Entitätstyp sein. Daher reicht der .NET-Typ möglicherweise nicht aus, um einen eigenen Typ zu identifizieren.

In diesen Fällen wird die-Eigenschaft, die vom Besitzer auf die eigene Entität zeigt, zur _definierenden Navigation_ des eigenen Entitäts Typs. Aus Sicht der EF Core ist die definierende Navigation Teil der Typidentität neben dem .NET-Typ.

In der folgenden Klasse sind z. b. `ShippingAddress` und `BillingAddress` beide denselben .NET-Typ haben: `StreetAddress`:

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

Um zu verstehen, wie EF Core überwachte Instanzen dieser Objekte unterscheiden, kann es hilfreich sein zu wissen, dass die definierende Navigation neben dem Wert des Schlüssels des Besitzers und dem .NET-Typ des eigenen Typs zu einem Teil des Schlüssels der Instanz geworden ist.

## <a name="nested-owned-types"></a>Im Besitz befindliche Typen

In diesem Beispiel `OrderDetails` `BillingAddress` und `ShippingAddress`, bei denen es sich um `StreetAddress` Typen handelt. Dann besitzt der `DetailedOrder`-Typ `OrderDetails`.

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/OwnedEntities/DetailedOrder.cs?name=DetailedOrder)]

[!code-csharp[OrderStatus](../../../samples/core/Modeling/OwnedEntities/OrderStatus.cs?name=OrderStatus)]

Jede Navigation zu einem eigenen Typ definiert einen separaten Entitätstyp mit vollständig unabhängiger Konfiguration.

Zusätzlich zu den Typen, die im Besitz eines Typs sind, kann ein eigener Typ auf eine reguläre Entität verweisen, es kann sich um den Besitzer oder eine andere Entität handeln, solange sich die zugehörige Entität auf der abhängigen Seite befindet. Diese Funktion legt eigene Entitäts Typen außer komplexen Typen in EF6 fest.

[!code-csharp[OrderDetails](../../../samples/core/Modeling/OwnedEntities/OrderDetails.cs?name=OrderDetails)]

Es ist möglich, die `OwnsOne`-Methode in einem fließend aufzurufenden Befehl zum Konfigurieren dieses Modells zu verketten:

[!code-csharp[OwnsOneNested](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneNested)]

Beachten Sie den `WithOwner`-Befehl, der zum Konfigurieren der Navigations Eigenschaft verwendet wird, die auf den Besitzer zeigt. Zum Konfigurieren einer Navigation zum Owner-Entitätstyp, der nicht Teil der Besitz Beziehung ist `WithOwner()` sollte ohne Argumente aufgerufen werden.

Es ist möglich, das Ergebnis mithilfe von `OwnedAttribute` sowohl auf `OrderDetails` als auch `StreetAddress`zu erzielen.

## <a name="storing-owned-types-in-separate-tables"></a>Speichern von eigenen Typen in separaten Tabellen

Im Gegensatz zu komplexen EF6-Typen können eigene Typen in einer separaten Tabelle vom Besitzer gespeichert werden. Um die Konvention zu überschreiben, die einen eigenen Typ derselben Tabelle wie der Besitzer zuordnet, können Sie einfach `ToTable` aufzurufen und einen anderen Tabellennamen angeben. Im folgenden Beispiel werden `OrderDetails` und die beiden Adressen einer separaten Tabelle aus `DetailedOrder`zugeordnet:

[!code-csharp[OwnsOneTable](../../../samples/core/Modeling/OwnedEntities/OwnedEntityContext.cs?name=OwnsOneTable)]

Es ist auch möglich, die `TableAttribute` zu verwenden, aber beachten Sie, dass dies fehlschlägt, wenn mehrere Navigationen zum eigenen Typ vorhanden sind, da in diesem Fall mehrere Entitäts Typen derselben Tabelle zugeordnet werden.

## <a name="querying-owned-types"></a>Abfragen von eigenen Typen

Beim Abfragen des Besitzers werden standardmäßig eigene Typen eingeschlossen. Es ist nicht erforderlich, die `Include`-Methode zu verwenden, auch wenn die eigenen Typen in einer separaten Tabelle gespeichert werden. Basierend auf dem zuvor beschriebenen Modell werden mit der folgenden Abfrage `Order`, `OrderDetails` und die beiden im Besitz befindlichen `StreetAddresses` aus der-Datenbank angezeigt:

[!code-csharp[DetailedOrderQuery](../../../samples/core/Modeling/OwnedEntities/Program.cs?name=DetailedOrderQuery)]

## <a name="limitations"></a>Einschränkungen

Einige dieser Einschränkungen sind grundlegend für die Funktionsweise von Entitäts Typen im Besitz, einige andere sind jedoch Einschränkungen, die in zukünftigen Versionen möglicherweise entfernt werden könnten:

### <a name="by-design-restrictions"></a>Entwurfs Einschränkungen

- Sie können keine `DbSet<T>` für einen eigenen Typ erstellen.
- `Entity<T>()` können nicht mit einem eigenen Typ aufgerufen werden `ModelBuilder`

### <a name="current-shortcomings"></a>Aktuelle Mängel

- Besitzende Entitäts Typen dürfen keine Vererbungs Hierarchien aufweisen
- Verweis Navigation auf eigene Entitäts Typen darf nicht NULL sein, es sei denn, Sie sind explizit einer separaten Tabelle vom Besitzer zugeordnet.
- Instanzen von eigenen Entitäts Typen können nicht von mehreren Besitzern gemeinsam genutzt werden (Dies ist ein bekanntes Szenario für Wert Objekte, das nicht mit Entitäts Typen im Besitz von Entitäten implementiert werden kann).

### <a name="shortcomings-in-previous-versions"></a>Mängel in früheren Versionen

- In EF Core 2,0 können Navigationen zu besitzenden Entitäts Typen nicht in abgeleiteten Entitäts Typen deklariert werden, es sei denn, die im Besitz befindlichen Entitäten werden explizit einer separaten Tabelle der Besitzer Hierarchie zugeordnet. Diese Einschränkung wurde in EF Core 2,1 entfernt.
- In EF Core 2,0 und 2,1 wurden nur Verweis Navigation auf eigene Typen unterstützt. Diese Einschränkung wurde in EF Core 2,2 entfernt.
