---
title: EF Core und EF6 – Featurevergleich
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: f22f29ef-efc0-475d-b0b2-12a054f80f95
uid: efcore-and-ef6/features
ms.openlocfilehash: ed6ce51e7560e19e0d572f3d81cef7cbb310beec
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995334"
---
# <a name="ef-core-and-ef6-feature-by-feature-comparison"></a>EF Core und EF6 – Featurevergleich

Die folgende Tabelle zeigt die in EF Core und EF6 verfügbaren Features. Die Tabelle bietet einen Featurevergleich auf allgemeiner Ebene. Es werden weder alle Funktionen aufgeführt, noch werden Details zu den möglichen Unterschieden in der Funktionsweise gleicher Features bereitgestellt.

Die Spalte „EF Core“ zeigt die Produktversion, in der das Feature zum ersten Mal eingeführt wurde.

| **Erstellen eines Modells**                                  | **EF 6** | **EF Core**                           |
|:------------------------------------------------------|:---------|:--------------------------------------|
| Grundlegende Klassenzuordnung                                   | Ja      | 1.0                                   |
| Konstruktoren mit Parametern                          |          | 2.1                                   |
| Konvertierungen von Eigenschaftswerten                            |          | 2.1                                   |
| Zugeordnete Typen ohne Schlüssel (Abfragetypen)               |          | 2.1                                   |
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
| Räumliche Daten                                          | Ja      |                                       |
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
|                                                       |          |                                       |
| **Abfragen von Daten**                                     | **EF6**  | **EF Core**                           |
| LINQ-Abfragen                                          | Ja      | 1.0 (für komplexe Abfragen in Bearbeitung) |
| Lesbares generiertes SQL                                | Schlecht     | 1.0                                   |
| Gemischte Client-/Serverauswertung                        |          | 1.0                                   |
| GroupBy-Übersetzung                                   | Ja      | 2.1                                   |
| Laden zugehöriger Daten: Eager                           | Ja      | 1.0                                   |
| Laden von verknüpften Daten: Eager Loading für abgeleitete Typen |          | 2.1                                   |
| Laden zugehöriger Daten: Lazy                            | Ja      | 2.1                                   |
| Laden zugehöriger Daten: Explizit                        | Ja      | 1.1                                   |
| RAW-SQL-Abfragen: Entitätstypen                         | Ja      | 1.0                                   |
| Reine SQL-Abfragen: Andere Typen als Entitätstypen (Abfragetypen)       | Ja      | 2.1                                   |
| Unformatierte SQL-Abfragen: Erstellung mit LINQ                  |          | 1.0                                   |
| Explizit kompilierte Abfragen                           | Schlecht     | 2.0                                   |
| Textbasierte Abfragesprache (Entity SQL)                | Ja      |                                       |
|                                                       |          |                                       |
| **Speichern von Daten**                                       | **EF6**  | **EF Core**                           |
| Änderungsnachverfolgung: Momentaufnahme                             | Ja      | 1.0                                   |
| Änderungsnachverfolgung: Benachrichtigung                         | Ja      | 1.0                                   |
| Änderungsnachverfolgung: Proxys                              | Ja      |                                       |
| Zugriff auf den erfassten Zustand                               | Ja      | 1.0                                   |
| Optimistische Nebenläufigkeit                                | Ja      | 1.0                                   |
| Transaktionen                                          | Ja      | 1.0                                   |
| Anweisungsbatches                                |          | 1.0                                   |
| Mapping der gespeicherten Prozedur                              | Ja      |                                       |
| Abgekoppelter Graph: Lowlevel-APIs                     | Schlecht     | 1.0                                   |
| Abgekoppelter Graph: End-to-End                         |          | 1.0 (teilweise)                         |
|                                                       |          |                                       |
| **Weitere Features**                                    | **EF6**  | **EF Core**                           |
| Migrationen                                            | Ja      | 1.0                                   |
| APIs zum Erstellen/Löschen von Datenbanken                       | Ja      | 1.0                                   |
| Ausgangswerte                                             | Ja      | 2.1                                   |
| Verbindungsresilienz                                 | Ja      | 1.1                                   |
| Lebenszyklushooks (Ereignisse, Abfangen)                | Ja      |                                       |
| Einfache Protokollierung („Database.Log“)                         | Ja      |                                       |
| DbContext-Pooling                                     |          | 2.0                                   |
|                                                       |          |                                       |
| **Datenbankanbieter**                                | **EF6**  | **EF Core**                           |
| SQL Server                                            | Ja      | 1.0                                   |
| MySQL                                                 | Ja      | 1.0                                   |
| PostgreSQL                                            | Ja      | 1.0                                   |
| Oracle                                                | Ja      | 1.0 <sup>(1)</sup>                    |
| SQLite                                                | Ja      | 1.0                                   |
| SQL Server Compact                                    | Ja      | 1.0 <sup>(2)</sup>                    |
| DB2                                                   | Ja      | 1.0                                   |
| Firebird                                              | Ja      | 2.0                                   |
| Jet (Microsoft Access)                                |          | 2.0 <sup>(2)</sup>                    |
| In-Memory (für Tests)                               |          | 1.0                                   |
|                                                       |          |                                       |
| **Plattformen**                                         | **EF6**  | **EF Core**                           |
| .NET Framework (Konsole, WinForms, WPF, ASP.NET)      | Ja      | 1.0                                   |
| .NET Core (Konsole, ASP.NET Core)                     |          | 1.0                                   |
| Mono & Xamarin                                        |          | 1.0 (in Bearbeitung)                     |
| UWP                                                   |          | 1.0 (in Bearbeitung)                     |

<sup>1</sup> Zurzeit ist ein kostenpflichtiger Anbieter verfügbar. An einem kostenlosen offiziellen Anbieter für Oracle wird gearbeitet.
<sup>2</sup> Dieser Anbieter funktioniert nur für .NET Framework (nicht für .NET Core).
