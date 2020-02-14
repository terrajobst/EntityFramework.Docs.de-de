---
title: Neuerungen in EF Core 5.0
author: ajcvickers
ms.date: 01/29/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: e858379cc46abbef999fd32a3685e1d522524889
ms.sourcegitcommit: 89567d08c9d8bf9c33bb55a62f17067094a4065a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/06/2020
ms.locfileid: "77052017"
---
# <a name="whats-new-in-ef-core-50"></a>Neuerungen in EF Core 5.0

EF Core 5.0 befindet sich derzeit in der Entwicklung.
Diese Seite bietet eine Übersicht über interessante Änderungen in den einzelnen Vorschauversionen.
Die erste Vorschau von EF Core 5.0 wird für das erste Quartal 2020 erwartet.

Auf dieser Seite wird der [Plan für EF Core 5.0](plan.md) nicht erneut aufgeführt.
Der Plan beschreibt die allgemeinen Themen für EF Core 5.0 einschließlich sämtlicher Features, die wir vor Auslieferung des finalen Releases integrieren möchten.

Wir werden an dieser Stelle Links zur offiziellen Dokumentation einfügen, sobald diese veröffentlicht ist.

## <a name="preview-1-not-yet-shipped"></a>Vorschau 1 (noch nicht ausgeliefert)

### <a name="simple-logging"></a>Einfache Protokollierung

Dieses Feature fügt eine Funktion hinzu, die `Database.Log` in EF6 ähnelt.
Es bietet eine einfache Möglichkeit zum Abrufen von Protokollen aus EF Core, ohne ein externes Protokollierungsframework konfigurieren zu müssen.

Eine vorläufige Dokumentation ist im [Wöchentlichen Status von EF vom 5. Dezember 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863) (in englischer Sprache) enthalten.

Zusätzliche Dokumentation finden Sie im Issue [#2085](https://github.com/aspnet/EntityFramework.Docs/issues/2085).

### <a name="simple-way-to-get-generated-sql"></a>Einfache Möglichkeit zum Abrufen von generiertem SQL-Code

In EF Core 5.0 wird die Erweiterungsmethode `ToQueryString` eingeführt, die die von EF Core beim Ausführen einer LINQ-Abfrage generierte SQL-Abfrage zurückgibt.

Eine vorläufige Dokumentation ist im [Wöchentlichen Status von EF vom 9. Januar 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246) (in englischer Sprache) enthalten.

Zusätzliche Dokumentation finden Sie im Issue [#1331](https://github.com/aspnet/EntityFramework.Docs/issues/1331).

### <a name="enhanced-debug-views"></a>Erweiterte Debugansichten

Debugansichten sind eine einfache Möglichkeit, beim Debuggen von Problemen interne Vorgänge in EF Core zu untersuchen.
Ein Debugansicht für das Modell wurde bereits vor einiger Zeit implementiert.
Für EF Core 5.0 haben wir die Modellansicht vereinfacht und im Zustands-Manager eine neue Debugansicht für nachverfolgte Entitäten hinzugefügt.

Eine vorläufige Dokumentation ist im [Wöchentlichen Status von EF vom 12. Dezember 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206) (in englischer Sprache) enthalten.

Zusätzliche Dokumentation finden Sie im Issue [#2086](https://github.com/aspnet/EntityFramework.Docs/issues/2086).

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a>Ändern von Verbindung oder Verbindungszeichenfolge in initialisiertem DbContext möglich

Es ist jetzt einfacher, eine DbContext-Instanz ohne Verbindung oder Verbindungszeichenfolge zu erstellen.
Die Verbindung oder Verbindungszeichenfolge kann jetzt auch in der Kontextinstanz verändert werden.
So kann ein und dieselbe Kontextinstanz dynamisch eine Verbindung mit verschiedenen Datenbanken herstellen.

Die Dokumentation finden Sie im Issue [#2075](https://github.com/aspnet/EntityFramework.Docs/issues/2075).

### <a name="change-tracking-proxies"></a>Proxys zur Änderungsnachverfolgung

EF Core kann jetzt Runtimeproxys generieren, die automatisch [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) und [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1) implementieren.
Diese melden dann Wertänderungen bei Entitätseigenschaften direkt an EF Core, sodass die Notwendigkeit einer Überprüfung auf Änderungen entfällt.
Für Proxys gelten jedoch bestimmte Einschränkungen, daher eignen sie sich nicht in jedem Fall.

Die Dokumentation finden Sie im Issue [#2076](https://github.com/aspnet/EntityFramework.Docs/issues/2076).

### <a name="improved-handling-of-database-null-semantics"></a>Verbesserte Verarbeitung von NULL-Semantik in der Datenbank

Relationale Datenbanken behandeln NULL in der Regel als unbekannten Wert und daher als ungleich jedem anderen NULL-Wert.
C# dagegen behandelt NULL als definierten Wert, der gleich jedem anderen NULL-Wert sein kann.
EF Core übersetzt Abfragen standardmäßig so, dass sie die NULL-Semantik von C# verwenden.
In EF Core 5.0 wurde die Effizienz dieser Übersetzungen erheblich verbessert.

Die Dokumentation finden Sie im Issue [#1612](https://github.com/aspnet/EntityFramework.Docs/issues/1612).

### <a name="indexer-properties"></a>Indexereigenschaften

EF Core 5.0 unterstützt die Zuordnung von C#-Indexereigenschaften.
So können Entitäten als Eigenschaftenbehälter fungieren, wobei Spalten benannten Eigenschaften im Behälter zugeordnet werden.

Die Dokumentation finden Sie im Issue [#2018](https://github.com/aspnet/EntityFramework.Docs/issues/2018).

### <a name="generation-of-check-constraints-for-enum-mappings"></a>Generieren von CHECK-Einschränkungen für Enumerationszuordnungen

EF Core 5.0-Migrationen können jetzt CHECK-Einschränkungen aus Zuordnungen der enum-Eigenschaft generieren.
Zum Beispiel:

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN('Useful', 'Useless', 'Unknown'))
```

Die Dokumentation finden Sie im Issue [#2082](https://github.com/aspnet/EntityFramework.Docs/issues/2082).

### <a name="query-translations-for-more-datetime-constructs"></a>Abfrageübersetzungen für weitere DateTime-Konstrukte

Abfragen mit neuer DateTime-Konstruktion werden jetzt übersetzt.
Darüber hinaus ist die SQL Server-Funktion DateDiffWeek jetzt zugeordnet.

Die Dokumentation finden Sie im Issue [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).

### <a name="query-translations-for-more-byte-array-constructs"></a>Abfrageübersetzungen für weitere Bytearraykonstrukte

Abfragen, die Contains, Length, SequenceEqual usw. in byte[]-Eigenschaften verwenden, werden jetzt in SQL übersetzt.

Eine vorläufige Dokumentation ist im [Wöchentlichen Status von EF vom 5. Dezember 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863) (in englischer Sprache) enthalten.

Zusätzliche Dokumentation finden Sie im Issue [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-reverse"></a>Abfrageübersetzung für Reverse

Abfragen, die `Reverse` verwenden, werden jetzt übersetzt.
Zum Beispiel:

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

Die Dokumentation finden Sie im Issue [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-bitwise-operators"></a>Abfrageübersetzung für bitweise Operatoren

Abfragen, die bitweise Operatoren verwenden, werden jetzt in mehr Fällen übersetzt. Beispiel:

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

Die Dokumentation finden Sie im Issue [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-strings-on-cosmos"></a>Abfrageübersetzung für Zeichenfolgen in Cosmos

Abfragen, die die Zeichenfolgenmethoden Contains, StartsWith und EndsWith verwenden, werden jetzt übersetzt, wenn der Azure Cosmos DB-Anbieter verwendet wird.

Die Dokumentation finden Sie im Issue [#2079](https://github.com/aspnet/EntityFramework.Docs/issues/2079).
