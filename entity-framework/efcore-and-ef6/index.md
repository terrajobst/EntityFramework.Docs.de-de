---
title: Vergleich von Entity Framework 6 und Entity Framework Core
description: Hilfestellung zur Entscheidung zwischen Entity Framework 6 und Entity Framework Core.
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: ad0807a3cfd62c6c09a97df1a45134db7a538623
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149062"
---
# <a name="compare-ef-core--ef6"></a>Vergleichen von EF Core und EF 6

Entity Framework ist ein objektrelationaler Mapper (O/RM) für .NET. In diesem Artikel werden die beiden Versionen verglichen: Entity Framework 6 und Entity Framework Core.

## <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework 6 (EF6) ist eine bewährte und getestete Datenzugriffstechnologie. Es wurde erstmals 2008 im Rahmen von .NET Framework 3.5 SP1 und Visual Studio 2008 SP1 veröffentlicht. Seit Version 4.1 wurde es als Bestandteil des [EntityFramework](https://www.nuget.org/packages/EntityFramework/)-NuGet-Pakets geliefert. EF6 wird unter .NET Framework 4. x und .NET Core ab Version 3.0 ausgeführt.

EF 6 ist nach wie vor ein unterstütztes Produkt, für das weiterhin Support durch Fehlerbehebungen und kleinere Verbesserungen bereitgestellt wird.

## <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework Core (EF Core) ist eine vollständig neue Version von EF6, die erstmals im Jahr 2016 veröffentlicht wurde. Es wird in Form von NuGet-Paketen geliefert, von denen das wichtigste [Microsoft.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/) ist. EF Core ist ein plattformübergreifendes Produkt, das unter .NET Core ausgeführt werden kann.

EF Core wurde entwickelt, um eine ähnliche Entwicklererfahrung wie EF6 bereitzustellen. Die meisten APIs der obersten Ebene bleibt unverändert, so dass EF Core Entwicklern, die EF 6 verwendet haben, vertraut erscheint.

## <a name="feature-comparison"></a>Funktionsvergleich

EF Core bietet neue Funktionen, die in EF6 nicht implementiert werden (wie etwa [alternative Schlüssel](xref:core/modeling/alternate-keys), [Batchupdates](xref:core/what-is-new/ef-core-1.0#relational-batching-of-statements) und [gemischte Client-/Datenbankauswertung in LINQ-Abfragen](xref:core/querying/client-eval). Da es sich aber um eine neue Codebasis handelt, fehlen auch einige Funktionen, über die EF6 verfügt.

In den folgenden Tabellen werden die in EF Core und EF6 verfügbaren Funktionen verglichen. Das ist ein allgemeiner Vergleich, in dem nicht alle Funktionen aufgelistet und nicht alle Unterschiede zwischen der gleichen Funktion in verschiedenen EF-Versionen erläutert sind.

Die Spalte „EF Core“ zeigt die Produktversion, in der die Funktion zum ersten Mal eingeführt wurde.

### <a name="creating-a-model"></a>Erstellen eines Modells

| **Funktion**                                           | **EF 6** | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Grundlegende Klassenzuordnung                                   | Ja      | 1.0                                   |
| Konstruktoren mit Parametern                          |          | 2.1                                   |
| Konvertierungen von Eigenschaftswerten                            |          | 2.1                                   |
| Zugeordnete Typen ohne Schlüssel                             |          | 2.1                                   |
| Konventionen                                           | Ja      | 1.0                                   |
| Benutzerdefinierte Konventionen                                    | Ja      | 1.0 (teilweise)                         |
| Datenanmerkungen                                      | Ja      | 1.0                                   |
| Fluent-API                                            | Ja      | 1.0                                   |
| Vererbung: Tabelle pro Hierarchie (TPH)                | Ja      | 1.0                                   |
| Vererbung: Tabelle pro Typ (TPT)                     | Ja      |                                       |
| Vererbung: Tabelle pro konkrete Klasse (TPC)           | Ja      |                                       |
| Eigenschaften im Schattenzustand                               |          | 1.0                                   |
| Alternative Schlüssel                                        |          | 1.0                                   |
| m:n ohne Joinentität                      | Ja      |                                       |
| Schlüsselgenerierung: Datenbank                              | Ja      | 1.0                                   |
| Schlüsselgenerierung: Client                                |          | 1.0                                   |
| Complex-/Owned-Typen                                   | Ja      | 2.0                                   |
| Räumliche Daten                                          | Ja      | 2.2                                   |
| Grafische Visualisierung eines Modells                      | Ja      |                                       |
| Grafischer Modell-Editor                                | Ja      |                                       |
| Modellformat: Code                                    | Ja      | 1.0                                   |
| Modellformat: EDMX (XML)                              | Ja      |                                       |
| Modell aus Datenbank erstellen: Befehlszeile              | Ja      | 1.0                                   |
| Modell aus Datenbank erstellen: VS-Assistent                 | Ja      |                                       |
| Modell aus Datenbank aktualisieren                            | Partial  |                                       |
| Globale Abfragefilter                                  |          | 2.0                                   |
| Tabellenaufteilung                                       | Ja      | 2.0                                   |
| Entitätsaufteilung                                      | Ja      |                                       |
| Zuordnung von Skalarfunktionen für Datenbanken                      | Schlecht     | 2.0                                   |
| Feldzuordnung                                         |          | 1.1                                   |
| Nullwerte zulassende Verweistypen (C# 8.0)                     |          | 3.0                                   |

### <a name="querying-data"></a>Abfragen von Daten

| **Funktion**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| LINQ-Abfragen                                          | Ja      | 1.0 (für komplexe Abfragen in Bearbeitung) |
| Lesbares generiertes SQL                                | Schlecht     | 1.0                                   |
| GroupBy-Übersetzung                                   | Ja      | 2.1                                   |
| Laden zugehöriger Daten: Eager Loading                           | Ja      | 1.0                                   |
| Laden zugehöriger Daten: Eager Loading für abgeleitete Typen |          | 2.1                                   |
| Laden zugehöriger Daten: Verzögert                            | Ja      | 2.1                                   |
| Laden zugehöriger Daten: Explicit                        | Ja      | 1.1                                   |
| Unformatierte SQL-Abfragen: Entitätstypen                         | Ja      | 1.0                                   |
| Unformatierte SQL-Abfragen: Schlüssellose Entitätstypen                 | Ja      | 2.1                                   |
| Unformatierte SQL-Abfragen: Zusammensetzen mit LINQ                  |          | 1.0                                   |
| Explizit kompilierte Abfragen                           | Schlecht     | 2.0                                   |
| Textbasierte Abfragesprache (Entity SQL)                | Ja      |                                       |
| await foreach (C# 8.0)                                |          | 3.0                                   |

### <a name="saving-data"></a>Speichern von Daten

| **Funktion**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Änderungsnachverfolgung: Snapshot                             | Ja      | 1.0                                   |
| Änderungsnachverfolgung: benachrichtigungs-                         | Ja      | 1.0                                   |
| Änderungsnachverfolgung: Proxys                              | Ja      |                                       |
| Zugriff auf den erfassten Zustand                               | Ja      | 1.0                                   |
| Optimistische Nebenläufigkeit                                | Ja      | 1.0                                   |
| Transaktionen                                          | Ja      | 1.0                                   |
| Anweisungsbatches                                |          | 1.0                                   |
| Mapping der gespeicherten Prozedur                              | Ja      |                                       |
| Abgekoppelter Graph: Lowlevel-APIs                     | Schlecht     | 1.0                                   |
| Abgekoppelter Graph: End-to-End                         |          | 1.0 (teilweise)                         |

### <a name="other-features"></a>Weitere Funktionen

| **Funktion**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Migrationen                                            | Ja      | 1.0                                   |
| APIs zum Erstellen/Löschen von Datenbanken                       | Ja      | 1.0                                   |
| Ausgangswerte                                             | Ja      | 2.1                                   |
| Verbindungsresilienz                                 | Ja      | 1.1                                   |
| Lebenszyklushooks (Ereignisse, Abfangen)                | Ja      |                                       |
| Einfache Protokollierung („Database.Log“)                         | Ja      |                                       |
| DbContext-Pooling                                     |          | 2.0                                   |

### <a name="database-providers"></a>Datenbankanbieter

| **Funktion**                                           | **EF6**  | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| SQL Server                                            | Ja      | 1.0                                   |
| MySQL                                                 | Ja      | 1.0                                   |
| PostgreSQL                                            | Ja      | 1.0                                   |
| Oracle                                                | Ja      | 1.0                                   |
| SQLite                                                | Ja      | 1.0                                   |
| SQL Server Compact                                    | Ja      | 1.0 <sup>(1)</sup>                    |
| DB2                                                   | Ja      | 1.0                                   |
| Firebird                                              | Ja      | 2.0                                   |
| Jet (Microsoft Access)                                |          | 2.0 <sup>(1)</sup>                    |
| Cosmos DB                                             |          | 3.0                                   |
| In-Memory (für Tests)                               |          | 1.0                                   |

<sup>1</sup> Die SQL Server Compact- und Jet-Anbieter funktionieren nur für .NET Framework (nicht für .NET Core).

### <a name="net-implementations"></a>.NET-Implementierungen

| **Funktion**                                           | **EF6**            | **EF Core**                           |
|:------------------------------------------------------|:-------------------|:--------------------------------------|
| .NET Framework                                        | Ja                | 1.0 (in 3.0 entfernt)                  |
| .NET Core                                             | Ja (in 6.3 hinzugefügt) | 1.0                                   |
| Mono & Xamarin                                        |                    | 1.0 (in Bearbeitung)                     |
| UWP                                                   |                    | 1.0 (in Bearbeitung)                     |

## <a name="guidance-for-new-applications"></a>Leitfaden für neue Anwendungen

Ziehen Sie für neue Anwendungen EF Core in Erwägung, wenn die beiden folgenden Bedingungen erfüllt sind:
* Die App benötigt die Funktionen von .NET Core. Weitere Informationen finden Sie unter [Wahl zwischen .NET Core und .NET Framework für Server-Apps](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server).
* EF Core unterstützt alle Funktionen, die für die App erforderlich sind. Wenn eine gewünschte Funktion fehlt, überprüfen Sie die [EF Core Roadmap](xref:core/what-is-new/roadmap), um herauszufinden, ob eine zukünftige Unterstützung geplant ist.

Ziehen Sie EF6 in Erwägung, wenn die beiden folgenden Bedingungen erfüllt sind:
* Die App wird unter Windows und .NET Framework 4.0 oder höher ausgeführt.
* EF6 unterstützt alle Funktionen, die für die App erforderlich sind.

## <a name="guidance-for-existing-ef6-applications"></a>Leitfaden für vorhandene EF6-Anwendungen

Aufgrund der wesentlichen Änderungen in EF Core wird nicht empfohlen, eine EF 6-Anwendung auf EF Core umzustellen – es sei denn, es gibt einen zwingenden Grund für eine solche Änderung. Wenn Sie zu EF Core wechseln möchten, um neue Funktionen zu nutzen, sollten Sie sich über die Einschränkungen im Klaren sein. Weitere Informationen finden Sie unter [Portieren von EF6 nach EF Core](porting/index.md). **Der Wechsel von EF6 zu EF Core ist eher eine Portierung als ein Upgrade.**

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen finden Sie in der Dokumentation:
* <xref:core/index>
* <xref:ef6/index>
