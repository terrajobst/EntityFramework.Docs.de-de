---
title: Frühere Releases von Entitätsframework – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1060bb99-765f-4f32-aaeb-d6635d3dbd3e
ms.openlocfilehash: 4fa27f8259ecc011d9d30080aee3c44353ef533d
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283991"
---
# <a name="past-releases-of-entity-framework"></a>Frühere Releases von Entitätsframework

Die erste Version von Entity Framework wurde in 2008 als Teil von .NET Framework 3.5 SP1 und Visual Studio 2008 SP1 veröffentlicht.

Ab Release EF 4.1 wurde es im Lieferumfang der [EntityFramework NuGet-Paket](https://www.nuget.org/packages/EntityFramework/) -derzeit eines der am häufigsten verwendeten Pakete auf NuGet.org.

Zwischen den Versionen 4.1 und 5.0 erweiterte EntityFramework NuGet-Paket die EF-Bibliotheken, die als Teil von .NET Framework zur Verfügung gestellt.   

Ab Version 6, EF ist ein open-Source-Projekt und auch verschoben vollständig Out-of-Band-Formular .NET Framework.
Dies bedeutet, dass wenn Sie das EntityFramework-Version 6-NuGet-Paket zu einer Anwendung hinzufügen, werden Sie eine vollständige Kopie der EF-Bibliothek erhalten, die nicht von der EF-Bits abhängt, die Teil von .NET Framework enthalten sind.
Ein wenig zu beschleunigen der Geschwindigkeit der Entwicklung und Bereitstellung neuer Features haben.

Im Juni 2016 haben wir die EF Core 1.0 veröffentlicht. EF Core basiert auf einer neuen Codebasis und dient als eine einfache und erweiterbare Version von EF.
Derzeit ist EF Core das Hauptaugenmerk der Entwicklung für das Entity Framework-Team bei Microsoft.
Dies bedeutet, es sind keine neuen wichtigeren Funktionen für EF6 geplant. EF6 jedoch immer noch als open Source-Projekt und ein unterstütztes Produkt von Microsoft verwaltet wird.

Hier ist die Liste der letzten Versionen in umgekehrter chronologischer Reihenfolge mit Informationen zu den neuen Features, die in jeder Version eingeführt wurden.

## <a name="ef-613"></a>EF 6.1.3
Die EF 6.1.3 Common Language Runtime wurde im Oktober 2015 auf NuGet veröffentlicht.
Diese Version enthält nur Korrekturen für von Fehlern mit hoher Priorität und Regressionen gemeldet, auf die 6.1.2 Version.
Die Korrekturen gehören:

- Abfrage: Regression in EF 6.1.2: OUTER APPLY eingeführt und komplexere Abfragen für 1:1-Beziehungen und "let"-Klausel
- TPT-Problem mit dem Basisklasseneigenschaft in geerbten Klasse ausblenden
- DbMigration.Sql schlägt fehl, wenn das Wort "Go" im Text enthalten ist
- Erstellen von Kompatibilitätsflag für UnionAll und Intersect-Unterstützung für flache Ansicht
- Abfrage mit mehreren enthält funktioniert in 6.1.2 (Arbeit in 6.1.1) nicht.
- "Sie haben einen Fehler in Ihrer SQL-Syntax" Ausnahme nach dem Upgrade von EF 6.1.1, 6.1.2

## <a name="ef-612"></a>EF 6.1.2
6.1.2 die EF Runtime wurde im Dezember 2014 auf NuGet veröffentlicht.
Diese Version geht es vor allem Fehlerkorrekturen. Es akzeptiert auch einige wichtige Änderungen von Mitgliedern der Community:
- **Cache-Abfrageparameter können aus der Datei app/web.configuration konfiguriert werden**
    ``` xml
    <entityFramework>
      <queryCache size='1000' cleaningIntervalInSeconds='-1'/>
    </entityFramework>
    ```
- **Methoden für SqlFile und SqlResource für "dbmigration"** ermöglichen es Ihnen, führen Sie eine SQL Skript als Datei gespeichert oder eingebettete Ressource.

## <a name="ef-611"></a>EF 6.1.1
Die EF 6.1.1 im Juni 2014 wurde der Common Language Runtime auf NuGet veröffentlicht.
Diese Version enthält Fehlerbehebungen für Probleme, die eine Anzahl von Personen aufgetreten sind. Unter anderem:
- Designer: Fehler EF5 EDMX-Datei mit der Anzahl von Dezimalstellen im EF6-Designer öffnen
- Standard-Instanz Erkennungslogik für LocalDB funktioniert nicht mit SQL Server 2014

## <a name="ef-610"></a>EF 6.1.0
Die EF 6.1.0 Common Language Runtime wurde im März 2014 auf NuGet veröffentlicht.
Dieses kleinere Update enthält eine Reihe neuer Features:

- **Konsolidierung Tools** bietet eine einheitliche Möglichkeit zum Erstellen eines neuen EF-Modells. Dieses Feature [erweitert den ADO.NET Entity Data Model-Assistenten zum Erstellen von Code First-Modelle unterstützen](~/ef6/modeling/code-first/workflows/existing-database.md), einschließlich der reverse-Engineering aus einer vorhandenen Datenbank. Diese Features waren zuvor in der Beta-Qualität in EF Power Tools verfügbar.
- **[Behandlung von Fehler bei Commit](~/ef6/fundamentals/connection-resiliency/commit-failures.md)**  bietet die CommitFailureHandler, die mit der der neu eingeführten Möglichkeit Transaktionsvorgänge abfangen. Die CommitFailureHandler ermöglicht eine automatische Wiederherstellung bei Verbindungsfehlern, während ein Commit der Transaktion durchgeführt.
- **[IndexAttribute](~/ef6/modeling/code-first/data-annotations.md)**  können Indizes durch Platzieren von festzulegende ein `[Index]` Attribut auf eine Eigenschaft (oder Eigenschaften) in Ihrem Code First-Modell. Code wird zunächst klicken Sie dann einen entsprechenden Index in der Datenbank erstellt.
- **Die öffentliche API-Zuordnung** ermöglicht den Zugriff auf die Informationen, die EF hat, auf wie die Eigenschaften und Typen für Spalten und Tabellen in der Datenbank zugeordnet werden. In früheren Versionen war diese API interne.
- **[Möglichkeit zum Konfigurieren von Interceptors über die Datei App/Web.config](~/ef6/fundamentals/configuring/config-file.md)**  interceptoren, um ohne Erneutes Kompilieren der Anwendung hinzugefügt werden können.
- **System.Data.Entity.Infrastructure.Interception.DatabaseLogger**ist ein neuer Interceptor, die es einfach macht, die alle Datenbankvorgänge in einer Datei zu protokollieren. In Kombination mit der vorherigen Funktion, dadurch können Sie problemlos [wechseln Sie zur Protokollierung von Datenbankvorgängen für eine bereitgestellte Anwendung](~/ef6/fundamentals/configuring/config-file.md), ohne neu kompilieren.
- **Änderungserkennung für Migrationen Modell** wurde verbessert, damit erstellte Migrationen sind eine präzisere; Leistung des Änderungsprozesses Erkennung wurde ebenfalls verbessert wurde.
- **Leistungsverbesserungen** einschließlich reduzierten Datenbankvorgänge während der Initialisierung generieren (Modellerstellung) Optimierungen für Vergleiche von null auf Gleichheit in LINQ-Abfragen schneller anzeigen, in anderen Szenarien und effizienter die Materialisierung der nachverfolgten Entitäten mit mehreren Zuordnungen.

## <a name="ef-602"></a>EF 6.0.2
Die EF 6.0.2 Common Language Runtime wurde im Dezember 2013 auf NuGet veröffentlicht.
Dieser Patchversion ist begrenzt, für die Behebung von Problemen, die eingeführt wurden in der EF6-Version (Regressionen im Leistungsverhalten/seit EF5).

## <a name="ef-601"></a>EF 6.0.1
Die EF 6.0.1 Common Language Runtime wurde in NuGet im Oktober des 2013 gleichzeitig mit der EF 6.0.0, freigegeben, da letztere in einer Version von Visual Studio eingebettet wurde, die einige Monate vor gesperrt haben.
Dieser Patchversion ist begrenzt, für die Behebung von Problemen, die eingeführt wurden in der EF6-Version (Regressionen im Leistungsverhalten/seit EF5).
Die wichtigsten Änderungen wurden zur Behebung von einige Leistungsprobleme während der Aufwärmphase für EF-Modellen.
Dies ist wichtig, da Sie ein Bereich des Fokus in EF6 und diese Probleme wurden einige der anderen in EF6 vorgenommenen Leistungsvorteile negiert, war die Leistung der Aufwärmphase.

## <a name="ef-60"></a>EF 6.0
Der EF 6.0.0 Common Language Runtime wurde im Oktober 2013 auf NuGet veröffentlicht.
Dies ist die erste Version, in dem eine vollständige EF Runtime befindet sich im, die [EntityFramework NuGet-Paket](https://www.nuget.org/packages/EntityFramework/) dem ist nicht auf die EF-Bits, die Teil von .NET Framework abhängig.
Verschieben die restlichen Teile der Laufzeit auf das NuGet-Paket werden zahlreiche wichtige Änderung für vorhandenen Code erforderlich.
Finden Sie im Abschnitt [ein Upgrade auf Entity Framework 6](upgrading-to-ef6.md) ausführliche Informationen zum manuellen Schritte erforderlich, um ein upgrade.

Diese Version bietet zahlreiche neue Features.
Die folgenden Features funktionieren für Modelle, die mit Code First oder dem EF Designer erstellt wurden:

- **[Async-Abfragen und speichern](~/ef6/fundamentals/async.md)**  fügt Unterstützung für die aufgabenbasierte asynchrone Muster, die eingeführt wurden in .NET 4.5.
- **[Verbindungsresilienz](~/ef6/fundamentals/connection-resiliency/retry-logic.md)**  ermöglicht eine automatische Wiederherstellung bei vorübergehenden verbindungsausfällen führen.
- **[Codebasierte Konfiguration](~/ef6/fundamentals/configuring/code-based.md)**  Ihnen die Möglichkeit der Ausführung der Konfiguration –, die normalerweise in einer Konfigurationsdatei – durchgeführt wurde im Code.
- **[Abhängigkeitsauflösung](~/ef6/fundamentals/configuring/dependency-resolution.md)**  führt die Unterstützung für den Service Locator-Muster, und wir haben ausgelagert einige Teile der Funktionalität, die durch benutzerdefinierte Implementierungen ersetzt werden können.
- **[Die Abfangfunktion/SQL-Protokollierung](~/ef6/fundamentals/logging-and-interception.md)**  einfache SQL-Protokollierung, die oben erstellte Low-Level-Bausteine für die Abfangfunktion EF-Vorgänge bietet.
- **Prüfbarkeitsverbesserungen** erleichtern das Erstellen von Testdoubles für "DbContext" und "DbSet" Wenn [Verwendung eines Simulationsframeworks](~/ef6/fundamentals/testing/mocking.md) oder [Verfassen Ihres eigenen Testdoubles](~/ef6/fundamentals/testing/writing-test-doubles.md).
- **["DbContext" kann jetzt mit einem "DbConnection", die bereits geöffnet ist, erstellt werden](~/ef6/fundamentals/connection-management.md)**  das ermöglicht Szenarien, in denen es wäre hilfreich, wenn die Verbindung geöffnet werden konnte, wenn Sie den Kontext zu erstellen (z. B. der Freigabe einer Verbindungs zwischen Komponenten, in denen Sie können nicht den Status der Verbindung sicherstellen).
- **[Verbesserte Transaktionsunterstützung](~/ef6/saving/transactions.md)**  bietet Unterstützung für eine Transaktion, die außerhalb der Framework sowie die verbesserten Möglichkeiten beim Erstellen einer Transaktion innerhalb des Frameworks.
- **Enumerationen, räumliche und eine bessere Leistung in .NET 4.0** – durch das Verschieben der Kernkomponenten, die mit der in .NET Framework in der EF-NuGet-Paket werden wir sind jetzt Unterstützung von Enumerationen, räumliche Datentypen und die leistungsverbesserungen von EF5 auf .NET anbieten 4.0.
- **Verbesserte Leistung der Enumerable.Contains in LINQ-Abfragen**.
- **Verbesserte betriebsbereiten Betriebszeit (Generieren von Sichten)**, insbesondere bei großen Modellen.
- **Austauschbare Pluralisierung &amp; Singularisierung Service**.
- **Benutzerdefinierte Implementierungen der gleich oder GetHashCode** Entität Klassen werden jetzt unterstützt.
- **DbSet.AddRange/RemoveRange** bietet eine optimierte Methode zum Hinzufügen oder Entfernen von mehreren Entitäten aus einem Satz.
- **DbChangeTracker.HasChanges** bietet eine einfache und effiziente Möglichkeit, festzustellen, ob alle ausstehenden Änderungen an der Datenbank gespeichert werden.
- **SqlCeFunctions** stellt eine SQL Compact, die die entspricht "SqlFunctions" entspricht.

Die folgenden Funktionen gelten nur für Code First:

- **[Benutzerdefinierte Code First-Konventionen](~/ef6/modeling/code-first/conventions/custom.md)**  ermöglichen schreiben Ihre eigenen Konventionen, um sich wiederholende Konfiguration zu vermeiden. Wir bieten eine einfache API für kompakte Konventionen sowie einige komplexere Bausteine Sie kompliziertere Konventionen erstellen können.
- **[Code der ersten Zuordnung zu gespeicherten Prozeduren von Insert/Update/Delete](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md)**  wird jetzt unterstützt.
- **[Idempotente Migrationsskripts](~/ef6/modeling/code-first/migrations/index.md)**  ermöglichen es Ihnen, ein SQL-Skript generieren, die eine Datenbank auf eine beliebige Version, auf die neueste Version aktualisieren können.
- **[Konfigurierbare Migrations History Table](~/ef6/modeling/code-first/migrations/history-customization.md)**  können Sie die Definition der Verlaufstabelle Migrationen anpassen. Dies ist besonders nützlich für Datenbankanbieter, die die entsprechenden Datentypen usw. für die Verlaufstabelle mit Migrationen funktioniert ordnungsgemäß angegeben werden müssen.
- **Mehrere Kontexte pro Datenbank** wird die vorherige Beschränkung einer Code First-Modell pro Datenbank, wenn Sie Migrationen verwenden oder Code First die Datenbank für Sie automatisch erstellt.
- **[DbModelBuilder.HasDefaultSchema](~/ef6/modeling/code-first/fluent/types-and-properties.md)**  ist eine neue Code First-API, mit der das standarddatenbankschema für ein Code First-Modell in einem zentralen Ort konfiguriert werden können. Das Code First Standardschema zuvor Programmcode &quot;Dbo&quot; und die einzige Möglichkeit, die das Schema zu konfigurieren, zu der eine Tabelle gehörte, wurde über die ToTable-API.
- **DbModelBuilder.Configurations.AddFromAssembly Methode** können Sie auf einfache Weise alle Konfigurationsklassen, die in einer Assembly definiert, bei der Verwendung von Konfigurationsklassen mit dem Code First Fluent-nach API hinzufügen.
- **[Benutzerdefinierte Migrationsoperationen](http://romiller.com/2013/02/27/ef6-writing-your-own-code-first-migration-operations/)**  aktiviert hinzufügen zusätzliche Vorgänge, die in Ihrem Code-basierte Migrationen verwendet werden.
- **Standardmäßige Transaktionsisolationsstufe wird geändert, um READ_COMMITTED_SNAPSHOT** für Datenbanken mit Code First, wodurch größere Skalierbarkeit und weniger Deadlocks erstellt.
- **Entitäts- und komplexen Typen können jetzt Nestedinside Klassen werden**. |

## <a name="ef-50"></a>EF 5.0
Die EF 5.0.0 Common Language Runtime wurde im August von 2012 auf NuGet veröffentlicht.
Diese Version führt einige neue Features, einschließlich der Unterstützung von Enumerationen, Table-valued Function, Typen von räumlichen Daten und verschiedene leistungsverbesserungen.

Entity Framework Designer in Visual Studio 2012 bietet auch Unterstützung für mehrere-Diagramme pro Modell einfärben Formen auf den Entwurf Oberfläche und der Batch-Import von gespeicherten Prozeduren.

Hier ist eine Liste der Inhalte, die wir speziell für die Version von EF 5 zusammengestellt.

-   [EF 5 Release Post](https://blogs.msdn.com/b/adonet/archive/2012/08/15/ef5-released.aspx)
-   Neue Features in EF5
    -   [Enum zuerst im Code unterstützen](~/ef6/modeling/code-first/data-types/enums.md)
    -   [Unterstützung von Enumerationen im EF Designer](~/ef6/modeling/designer/data-types/enums.md)
    -   [Räumliche Datentypen im Code zuerst](~/ef6/modeling/code-first/data-types/spatial.md)
    -   [Typen von räumlichen Daten im EF Designer](~/ef6/modeling/designer/data-types/spatial.md)
    -   [Unterstützung für räumliche Typen](~/ef6/fundamentals/providers/spatial-support.md)
    -   [Tabellenwertfunktionen](~/ef6/modeling/designer/advanced/tvfs.md)
    -   [Mehrere Diagramme pro Modell](~/ef6/modeling/designer/multiple-diagrams.md)
-   Ihr Modell festlegen
    -   [Erstellen eines Modells](~/ef6/modeling/index.md)
    -   [Verbindungen und Modelle](~/ef6/fundamentals/configuring/connection-strings.md)
    -   [Überlegungen zur Leistung](~/ef6/fundamentals/performance/perf-whitepaper.md)
    -   [Arbeiten mit Microsoft SQL Azure](~/ef6/fundamentals/connection-resiliency/retry-logic.md)
    -   [Dienstkonfigurations-Dateieinstellungen](~/ef6/fundamentals/configuring/config-file.md)
    -   [Glossar](~/ef6/resources/glossary.md)
    -   Code First
        -   [Code First für eine neue Datenbank (Exemplarische Vorgehensweise und Video)](~/ef6/modeling/code-first/workflows/new-database.md)
        -   [Code First mit einer vorhandenen Datenbank (Exemplarische Vorgehensweise und Video)](~/ef6/modeling/code-first/workflows/existing-database.md)
        -   [Konventionen](~/ef6/modeling/code-first/conventions/built-in.md)
        -   [Datenanmerkungen](~/ef6/modeling/code-first/data-annotations.md)
        -   [Fluent-API – konfigurieren und Zuordnung von Eigenschaften & Typen](~/ef6/modeling/code-first/fluent/types-and-properties.md)
        -   [Fluent-API – Konfigurieren von Beziehungen](~/ef6/modeling/code-first/fluent/relationships.md)
        -   [Fluent-API in VB.NET](~/ef6/modeling/code-first/fluent/vb.md)
        -   [Code First-Migrationen](~/ef6/modeling/code-first/migrations/index.md)
        -   [Automatische Code First-Migrationen](~/ef6/modeling/code-first/migrations/automatic.md)
        -   [Migrate.exe](~/ef6/modeling/code-first/migrations/migrate-exe.md)
        -   [Definieren von "dbsets"](~/ef6/modeling/code-first/dbsets.md)
    -   EF-Designer
        -   [Model First (Exemplarische Vorgehensweise und Video)](~/ef6/modeling/designer/workflows/model-first.md)
        -   [Database First (Exemplarische Vorgehensweise und Video)](~/ef6/modeling/designer/workflows/database-first.md)
        -   [Komplexe Typen](~/ef6/modeling/designer/data-types/complex-types.md)
        -   [Zuordnungen/Relationships](~/ef6/modeling/designer/relationships.md)
        -   [Muster der TPT-Vererbung](~/ef6/modeling/designer/inheritance/tpt.md)
        -   [TPH-Vererbungsmuster](~/ef6/modeling/designer/inheritance/tph.md)
        -   [Abfrage mit gespeicherten Prozeduren](~/ef6/modeling/designer/stored-procedures/query.md)
        -   [Gespeicherte Prozeduren mit mehreren Resultsets](~/ef6/modeling/designer/advanced/multiple-result-sets.md)
        -   [Fügen Sie ein, aktualisieren Sie und löschen Sie mit gespeicherten Prozeduren](~/ef6/modeling/designer/stored-procedures/cud.md)
        -   [Zuordnen einer Entität zu mehreren Tabellen (Aufteilung von Entitäten)](~/ef6/modeling/designer/entity-splitting.md)
        -   [Zuordnen von mehreren Entitäten zu einer Tabelle (Tabelle geteilt wird)](~/ef6/modeling/designer/table-splitting.md)
        -   [Definieren von Abfragen](~/ef6/modeling/designer/advanced/defining-query.md)
        -   [Code Generation Templates](~/ef6/modeling/designer/codegen/index.md)
        -   [Wiederherstellen der ObjectContext](~/ef6/modeling/designer/codegen/legacy-objectcontext.md)
-   Das Modell
    -   [Arbeiten mit DbContext](~/ef6/fundamentals/working-with-dbcontext.md)
    -   [Abfragen von/Suchen von Entitäten](~/ef6/querying/index.md)
    -   [Arbeiten mit Beziehungen](~/ef6/fundamentals/relationships.md)
    -   [Laden verwandter Entitäten](~/ef6/querying/related-data.md)
    -   [Arbeiten mit lokalen Daten](~/ef6/querying/local-data.md)
    -   [N-Tier-Anwendungen](~/ef6/fundamentals/disconnected-entities/index.md)
    -   [Leere SQL-Abfragen](~/ef6/querying/raw-sql.md)
    -   [Muster für vollständige Parallelität](~/ef6/saving/concurrency.md)
    -   [Arbeiten mit Proxys in](~/ef6/fundamentals/proxies.md)
    -   [Automatisches Erkennen von Änderungen](~/ef6/saving/change-tracking/auto-detect-changes.md)
    -   [Abfragen ohne nachverfolgung](~/ef6/querying/no-tracking.md)
    -   [Load-Methode](~/ef6/querying/load-method.md)
    -   [Hinzufügen/Attach "und" Status von Entitäten](~/ef6/saving/change-tracking/entity-state.md)
    -   [Verwenden von Eigenschaftswerten](~/ef6/saving/change-tracking/property-values.md)
    -   [Datenbindung mit WPF (Windows Presentation Foundation)](~/ef6/fundamentals/databinding/wpf.md)
    -   [Datenbindung mit Windows Forms (Windows Forms)](~/ef6/fundamentals/databinding/winforms.md)

## <a name="ef-431"></a>EF 4.3.1
Der EF 4.3.1 Common Language Runtime wurde im Februar 2012 kurz nach EF 4.3.0 auf NuGet veröffentlicht.
Dieser Patchversion enthalten einige Fehlerbehebungen auf die EF 4.3-Version und eine bessere LocalDB-Unterstützung für Kunden, die mithilfe von EF 4.3 mit Visual Studio 2012 eingeführt.

Hier ist eine Liste der Inhalte, die wir zusammengestellt, speziell für die EF 4.3.1-Version, die meisten Inhalte zur EF 4.1 gilt weiterhin für EF 4.3 auch.

-   [EF 4.3.1-Version – Blogbeitrag](https://blogs.msdn.com/b/adonet/archive/2012/02/29/ef4-3-1-and-ef5-beta-1-available-on-nuget.aspx)

## <a name="ef-43"></a>EF 4.3
Das EF-Version 4.3.0 Common Language Runtime wurde im Februar von 2012 auf NuGet veröffentlicht.
Diese Version enthielt die Code First-Migrationen Funktion, mit einer Datenbank erstellt, die durch Code First inkrementell geändert werden, wie Ihr Code First-Datenmodell entwickelt wurde.

Es folgt eine Liste von Inhalten, die wir speziell für die Version von EF 4.3 zusammengestellt, die meisten Inhalte zur EF 4.1 gilt weiterhin für EF 4.3 ebenfalls:
-   [EF-Version 4.3-Post](https://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-released.aspx)
-   [EF 4.3 Code-basierte Migrationen Exemplarische Vorgehensweise](https://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-code-based-migrations-walkthrough.aspx)
-   [EF 4.3 automatische Migrationen Exemplarische Vorgehensweise](https://blogs.msdn.com/b/adonet/archive/2012/02/09/ef-4-3-automatic-migrations-walkthrough.aspx)

## <a name="ef-42"></a>EF 4.2
Die EF 4.2.0 Common Language Runtime wurde im November 2011 auf NuGet veröffentlicht.
Diese Version enthält Fehlerbehebungen, die EF 4.1.1 freizugeben.
Da diese Version nur enthalten, Fehlerbehebungen, die es hätte das EF-4.1.2 patch Version, aber wir entschieden, um in 4.2 zu verschieben, sodass von das Datum basierend zu verschieben, Patch-Versionsnummern haben wir die 4.1.x verwendet frei, und übernehmen die [semantische Versionsing](https://semver.org) standard für die semantische versionsverwaltung.

Hier ist eine Liste der Inhalte, die wir speziell für die Version 4.2 von EF zusammengestellt, die für EF 4.1 bereitgestellte Inhalte gilt weiterhin für EF 4.2 auch.

-   [EF-Version 4.2-Post](https://blogs.msdn.com/b/adonet/archive/2011/11/01/ef-4-2-released.aspx)
-   [Exemplarische Vorgehensweise für Code First](https://blogs.msdn.com/b/adonet/archive/2011/09/28/ef-4-2-code-first-walkthrough.aspx)
-   [Modell & Datenbank der ersten exemplarischen Vorgehensweise](https://blogs.msdn.com/b/adonet/archive/2011/09/28/ef-4-2-model-amp-database-first-walkthrough.aspx)

## <a name="ef-411"></a>EF 4.1.1
Die EF 4.1.10715 Common Language Runtime wurde im Juli 2011 auf NuGet veröffentlicht.
Zusätzlich zu Fehlerkorrekturen eingeführt dieser Patchversion einige Komponenten, um es für die Entwurfszeit-Tools zum Arbeiten mit Code First-Modells erleichtern.
Diese Komponenten werden von Code First-Migrationen (enthalten in EF 4.3) und das Entity Framework Power Tools verwendet.

Sie werden feststellen, dass die ungewöhnliche 4.1.10715 des Pakets Versionsnummer.
Es verwendet, um Datum basierend Patchversionen zu verwenden, bevor wir uns entschieden, übernehmen [semantische Versionierung](https://semver.org).
Betrachten Sie diese Version als EF 4.1 Patch 1 (oder EF 4.1.1).

Es folgt eine Liste von Inhalten, die wir für die 4.1.1 zusammengestellt Version:

-   [EF 4.1.1 Release Post](https://blogs.msdn.com/b/adonet/archive/2011/07/25/ef-4-1-update-1-released.aspx)

## <a name="ef-41"></a>EF 4.1
Die EF 4.1.10331 Runtime war der erste im April 2011 über NuGet veröffentlicht.
Diese Version enthielt die vereinfachte DbContext-API und die Code First-Workflow.

Sie werden feststellen, die ungewöhnliche Versionsnummer 4.1.10331, die wirklich 4.1 wurden sollten. Darüber hinaus steht eine 4.1.10311 Version die 4.1.0-rc wurden sollte (das "rc" steht für "Release Candidate").
Es verwendet, um Datum basierend Patchversionen zu verwenden, bevor wir uns entschieden, übernehmen [semantische Versionierung](https://semver.org).

Hier ist eine Liste der Inhalte, die wir für die Version 4.1 zusammengestellt. Viel davon gilt weiterhin für spätere Versionen von Entity Framework:

-   [EF-Version 4.1-Post](https://blogs.msdn.com/b/adonet/archive/2011/04/11/ef-4-1-released.aspx)
-   [Exemplarische Vorgehensweise für Code First](https://blogs.msdn.com/b/adonet/archive/2011/03/15/ef-4-1-code-first-walkthrough.aspx)
-   [Modell & Datenbank der ersten exemplarischen Vorgehensweise](https://blogs.msdn.com/b/adonet/archive/2011/03/15/ef-4-1-model-amp-database-first-walkthrough.aspx)
-   [SQL Azure-Verbunde und Entitätsframework](https://blogs.msdn.com/b/adonet/archive/2012/01/10/sql-azure-federations-and-the-entity-framework.aspx)

## <a name="ef-40"></a>EF 4.0
Diese Version war in .NET Framework 4 und Visual Studio 2010 im April des 2010 enthalten.
Wichtige neue Features in dieser Version enthalten POCO-Unterstützung, foreign Key-Zuordnung, lazy Loading, prüfbarkeitsverbesserungen, anpassbare codegenerierung und Model First-Workflow.

Auch wenn sie auf der zweiten Version von Entity Framework war, hieß es EF 4, .NET Framework-Version ausgerichtet, die sie mit geliefert.
Diesem Release haben wir Schritte Entity Framework auf NuGet verfügbar gemacht und semantische Versionierung übernommen, da wir nicht mehr an die .NET Framework-Version gebunden wurden.

Beachten Sie, dass einige nachfolgende Versionen von .NET Framework mit wesentlichen Aktualisierungen auf die enthaltenen Version von EF bereitgestellt haben.
Tatsächlich wurden viele der neuen Features von EF 5.0 als Verbesserungen auf diese Komponenten implementiert.
Allerdings um die Geschichte der versionsverwaltung für EF zu rationalisieren, wir weiterhin auf die EF-Bits zu verweisen, die Teil von .NET Framework als die EF 4.0-Laufzeit, während alle neuere Versionen bestehen aus den [EntityFramework NuGet-Paket](https://www.nuget.org/packages/EntityFramework/).         

## <a name="ef-35"></a>EF 3.5
Die erste Version von Entity Framework wurde in .NET 3.5 Service Pack 1 und Visual Studio 2008 SP1, veröffentlicht im August von 2008 enthalten.
Diese Version bietet grundlegende O/RM-Unterstützung, die mit dem Database First-Workflow.
