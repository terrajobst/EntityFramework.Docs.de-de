---
title: Neue Features in EF Core 3.0
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: 528733d6eec33de2c9538541a6ed5be704b9d433
ms.sourcegitcommit: d01fc19aa42ca34c3bebccbc96ee26d06fcecaa2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/16/2019
ms.locfileid: "71005558"
---
# <a name="new-features-included-in-ef-core-30"></a>Neue Features in EF Core 3.0

Die folgende Liste enthält die wichtigsten neuen Features, die für EF Core 3.0 geplant sind.

EF Core 3.0 ist eine Hauptversion und enthält auch zahlreiche [Breaking Changes](xref:core/what-is-new/ef-core-3.0/breaking-changes), bei denen es sich um Verbesserungen der API handelt, die einen negativen Einfluss auf vorhandene Anwendungen haben können.  

## <a name="linq-improvements"></a>LINQ-Verbesserungen 

LINQ ermöglicht es Ihnen, Datenbankabfragen zu schreiben, ohne die Sprache Ihrer Wahl zu verlassen. Rich-Type-Informationen werden genutzt, um IntelliSense und Typprüfung zur Kompilierungszeit zu bieten.
LINQ ermöglicht es Ihnen darüber hinaus, eine unbegrenzte Anzahl komplizierter Abfragen zu schreiben, die beliebige Ausdrücke enthalten (Methodenaufrufe oder Operationen).
Die Verarbeitung all dieser Kombinationen hat immer eine erhebliche Herausforderung für LINQ-Anbieter dargestellt.
In EF Core 3.0 haben wir unsere LINQ-Implementierung neu geschrieben, um die Übersetzung weiterer Ausdrücke in SQL zu ermöglichen, um in mehr Fällen effiziente Abfragen zu generieren, um zu verhindern, dass ineffiziente Abfragen nicht erkannt werden, und um uns die schrittweise Einführung neuer Abfragefunktionen und Leistungsoptimierungen ohne Bruch mit vorhandenen Anwendungen und Datenanbietern zu erleichtern.

### <a name="client-evaluation"></a>Clientauswertung

Die wichtigste Entwurfsänderung in EF Core 3.0 hat mit der Weise zu tun, wie LINQ Ausdrücke behandelt, die es nicht in SQL oder Parameter übersetzen kann:

In den ersten Versionen ermittelte EF Core einfach, welche Anteile einer Abfrage in SQL übersetzt werden konnten, und führte den Rest der Abfrage auf dem Client aus.
Diese Art von clientseitiger Ausführung kann in einigen Situationen wünschenswert sein, aber in vielen anderen Fällen kann sie zu ineffizienten Abfragen führen.
Wenn beispielsweise EF Core 2.2 ein Prädikat in einem `Where()`-Aufruf nicht übersetzen konnte, führte es eine SQL-Anweisung ohne Filter aus, las alle Zeilen aus der Datenbank und filterte sie dann im Arbeitsspeicher.
Das kann akzeptabel sein, wenn die Datenbank eine kleine Anzahl Zeilen enthält, kann aber auch zu erheblichen Leistungsproblemen oder sogar dem Ausfall der Anwendung führen, wenn eine Datenbank eine große Anzahl Zeilen enthält.
In EF Core 3.0 haben wir die Clientauswertung auf die Projektion der obersten Ebene (den letzten Aufruf von `Select()`) eingeschränkt.
Wenn EF Core 3.0 an irgendeiner anderen Stelle in der Abfrage Ausdrücke erkennt, die nicht übersetzt werden können, gibt es eine Ausnahme zur Laufzeit aus.

## <a name="cosmos-db-support"></a>Cosmos DB-Unterstützung 

Der Cosmos DB-Anbieter für EF Core ermöglicht es Entwicklern, die mit dem EF-Programmiermodell vertraut sind, Azure Cosmos DB problemlos als Anwendungsdatenbank-Ziel einzusetzen.
Ziel ist es, einige der Vorteile von Cosmos DB – beispielsweise die globale Verteilung, Always On-Verfügbarkeit, elastische Skalierbarkeit und geringe Latenz – noch besser für .NET-Entwickler zugänglich zu machen.
Der Anbieter wird die meisten EF Core-Features unterstützen, darunter die automatische Änderungsnachverfolgung, LINQ und Wertekonvertierungen anhand der SQL-API in Cosmos DB.

## <a name="c-80-support"></a>C# 8.0-Unterstützung

EF Core 3.0 nutzt einige der neuen Funktionen in C# 8.0:

### <a name="asynchronous-streams"></a>Asynchrone Streams

Asynchrone Abfrageergebnisse werden jetzt mithilfe der neuen `IAsyncEnumerable<T>`-Standardschnittelle verfügbar gemacht und können mithilfe von `await foreach` konsumiert werden.

``` csharp
var orders = 
  from o in context.Orders
  where o.Status == OrderStatus.Pending
  select o;

await foreach(var o in orders)
{
  Proccess(o);
} 
```

### <a name="nullable-reference-types"></a>Nullwerte zulassende Verweistypen 

Wenn diese neue Funktion in Ihrem Code aktiviert ist, kann EF Core Überlegungen zur NULL-Zulässigkeit für Eigenschaften von Verweistypen (entweder primitiver Typen wie Zeichenfolgen oder Navigationseigenschaften) anstellen, um die NULL-Zulässigkeit von Spalten und Beziehungen in der Datenbank zu entscheiden.

## <a name="interception"></a>Interception

Die neue Abfang-API in EF Core 3.0 ermöglicht das programmgesteuerte Beobachten und Ändern des Ergebnisses von Datenbankvorgängen auf unteren Ebenen, die im Rahmen des normalen Betriebs von EF Core auftreten, z. B. das Öffnen von Verbindungen, das Einleiten von Transaktionen und das Ausführen von Befehlen. 

## <a name="reverse-engineering-of-database-views"></a>Reverse Engineering (Zurückentwicklung) von Datenbanksichten

Entitätstypen ohne Schlüssel (bisher als [Abfragetypen](xref:core/modeling/query-types) bezeichnet) stellen Daten dar, die aus der Datenbank gelesen, aber nicht aktualisiert werden können.
Dank dieser Eigenschaft eignen sich diese Typen in den meisten Szenarien hervorragend für die Zuordnung von Datenbanksichten. Für das Reverse Engineering von Datenbanksichten haben wir daher die Erstellung von Entitätstypen ohne Schlüssel automatisiert.

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a>Abhängige Entitäten, die die Tabelle gemeinsam mit dem Prinzipal verwenden, sind jetzt optional

Ab EF Core 3.0 gibt es die Möglichkeit, wenn `OrderDetails` im Besitz von `Order` ist oder explizit derselben Tabelle zugeordnet ist, dass `Order` ohne `OrderDetails` hinzugefügt werden kann, und alle `OrderDetails`-Eigenschaften, mit Ausnahme des primären Schlüssels, werden Spalten zugeordnet, die NULL-Werte zulassen.

Bei Abfragen von EF Core wird `OrderDetails` auf `null` festgelegt, wenn eine der erforderlichen Eigenschaften keinen Wert aufweist oder keine erforderlichen Eigenschaften außer dem primären Schlüssel vorhanden und alle Eigenschaften `null` sind.

``` csharp
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

[Owned]
public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

## <a name="ef-63-on-net-core"></a>EF 6.3 unter .NET Core

Uns ist bewusst, dass viele bestehende Anwendungen frühere Versionen von EF verwenden und dass die Portierung auf EF Core nur zur Nutzung von .NET Core unter Umständen mit einem erheblichen Aufwand verbunden ist.
Aus diesem Grund haben wir für die neueste Version von EF 6 die Ausführung unter .NET Core 3.0 ermöglicht.
Es gibt eine Reihe von Beschränkungen, darunter:
- Neue Anbieter sind für die Arbeit mit .NET Core erforderlich
- Die Unterstützung räumlicher Daten für SQL Server wird nicht aktiviert.

## <a name="postponed-features"></a>Verschobene Features

Einige Features, die ursprünglich für EF Core 3.0 geplant waren, wurden auf zukünftige Versionen verschoben: 

- Die Fähigkeit, Teile eines Modells in Migrationen zu ignorieren, wie in [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725) nachverfolgt.
- Entitäten von Eigenschaftenbehältern, die in zwei separaten Problemen nachverfolgt werden: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) über Entitäten von gemeinsam genutzten Typen und [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) über Unterstützung der Zuordnung von indizierten Eigenschaften.
