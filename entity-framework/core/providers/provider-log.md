---
title: Protokoll des Anbieter-Auswirkungen auf die Änderungen – EF Core
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: 9ef87a737111053df0359f3b2d7a4f82d25c578a
ms.sourcegitcommit: 5280dcac4423acad8b440143433459b18886115b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "58867943"
---
# <a name="provider-impacting-changes"></a>Anbieter-Auswirkungen auf Änderungen

Diese Seite enthält Links zum pull-Anforderungen, die auf das EF Core-Repository, das Autoren von anderen Datenbankanbietern, reagieren möglicherweise erfordern. Die Absicht ist einen Ausgangspunkt für Autoren von vorhandene Drittanbieter-Datenbankanbieter angeben, wenn es sich bei ihren Anbieter zu einer neuen Version zu aktualisieren.

Dieses Protokoll beginnen wir mit Änderungen vom 2.1 und 2.2. Vor dem 2.1 verwendet die [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) und [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) Bezeichnungen auf unseren Problemen und Pull-Anforderungen.

## <a name="22-----30"></a>2.2 ---> 3.0

Beachten Sie, dass viele der der [wichtige Änderungen auf Anwendungsebene](../what-is-new/ef-core-3.0/breaking-changes.md) wirkt sich auch auf die Anbieter.

* https://github.com/aspnet/EntityFrameworkCore/pull/14022
  * Entfernt veraltete APIs und reduzierte optionalen Parameter-Überladungen
  * Entfernte DatabaseColumn.GetUnderlyingStoreType()
* https://github.com/aspnet/EntityFrameworkCore/pull/14589
  * Veraltete APIs entfernt
* https://github.com/aspnet/EntityFrameworkCore/pull/15044
  * Unterklassen von CharTypeMapping möglicherweise aufgrund von verhaltensänderungen, die erforderlich sind, für die Behebung von ein paar Fehler in der basisimplementierung unterbrochen wurden.
* https://github.com/aspnet/EntityFrameworkCore/pull/15090
  * Eine Basisklasse für IDatabaseModelFactory hinzugefügt und aktualisiert, um ein Objekt für die Parameter zu verwenden, um zukünftige Unterbrechungen zu minimieren.
* https://github.com/aspnet/EntityFrameworkCore/pull/15123
  * Parameter-Objekten können Sie in MigrationsSqlGenerator verwendet, um zukünftige Unterbrechungen zu minimieren.
* https://github.com/aspnet/EntityFrameworkCore/pull/14972
  * Explizite Konfiguration der Protokollebenen erforderlich, einige Änderungen an APIs, die möglicherweise von Anbietern verwendet werden. Insbesondere wenn der Anbieter der Protokollierungsinfrastruktur direkt verwenden, kann dann diese Änderung verwenden beeinträchtigt werden. Darüber hinaus Anbieter, die die Infrastruktur zu verwenden (die öffentlich sein wird) in Zukunft für die Ableitung müssen `LoggingDefinitions` oder `RelationalLoggingDefinitions`. Die SQL Server und in-Memory-Anbieter für Beispiele angezeigt.
* https://github.com/aspnet/EntityFrameworkCore/pull/15091
  * Core, relationale und Abstraktionen Ressourcenzeichenfolgen sind jetzt öffentlich.
  * `CoreLoggerExtensions` und `RelationalLoggerExtensions` jetzt öffentlich sind. Anbieter sollten diese APIs verwenden, beim Protokollieren von Ereignissen, die auf Core oder relationalen Ebene definiert werden. Ressourcen für die Protokollierung nicht direkt auf; Diese sind weiterhin intern.
  * `IRawSqlCommandBuilder` wurde von einem Singleton-Dienst in einen bereichsbezogenen Dienst geändert
  * `IMigrationsSqlGenerator` wurde von einem Singleton-Dienst in einen bereichsbezogenen Dienst geändert
* https://github.com/aspnet/EntityFrameworkCore/pull/14706
  * Die Infrastruktur für die Erstellung von relationaler Befehlen wurde veröffentlicht, damit es sicher von Anbietern verwendet und etwas umgestaltet werden kann.
* https://github.com/aspnet/EntityFrameworkCore/pull/14733
  * `ILazyLoader` wurde von einen bereichsbezogenen Dienst in einem vorübergehenden Dienst geändert
* https://github.com/aspnet/EntityFrameworkCore/pull/14610
  * `IUpdateSqlGenerator` wurde von einen bereichsbezogenen Dienst in ein Singleton-Dienst geändert
  * Darüber hinaus `ISingletonUpdateSqlGenerator` wurde entfernt
* https://github.com/aspnet/EntityFrameworkCore/pull/15067
  * Ein Großteil der internen Code, der von Anbietern verwendet wird, wurde jetzt öffentliche wurde
  * Wird nicht mehr Necssary auf `IndentedStringBuilder` , da sie aus den Stellen aufgeteilt hat, die sie verfügbar gemacht
  * Verwendung von `NonCapturingLazyInitializer` sollte ersetzt werden durch `LazyInitializer` aus der BCL
* https://github.com/aspnet/EntityFrameworkCore/pull/14608
  * Diese Änderung wird in der Anwendung, die wichtige Änderungen Dokument vollständig behandelt. Für Anbieter kann dies sein, weitere beeinträchtigt, da Tests EF Core oft dazu dieses Problem zu drücken führen kann, damit der Test-Infrastruktur geändert hat, um, die weniger wahrscheinlich zu machen.
* https://github.com/aspnet/EntityFrameworkCore/issues/13961
  * `EntityMaterializerSource` wurde vereinfacht
* https://github.com/aspnet/EntityFrameworkCore/pull/14895
  * "StartsWith" Übersetzung hat auf eine Weise geändert, die Anbieter möchten/müssen möglicherweise reagieren
* https://github.com/aspnet/EntityFrameworkCore/pull/15168
  * Konvention Satz Dienste haben sich geändert. Anbieter sollten jetzt über "ProviderConventionSet" oder "RelationalConventionSet" erben.
  * Anpassungen können hinzugefügt werden, über `IConventionSetCustomizer` Services, aber dies richtet sich an, die von anderen Erweiterungen, die nicht von Anbietern verwendet werden.
  * Zur Laufzeit verwendete Konventionen aus aufgelöst werden soll `IConventionSetBuilder`.

## <a name="21-----22"></a>2.1 ---> 2.2

### <a name="test-only-changes"></a>Änderungen nur im Test

* [https://github.com/aspnet/EntityFrameworkCore/pull/12057](https://github.com/aspnet/EntityFrameworkCore/pull/12057) -Anpassbare SQL Trennzeichen in Tests zulassen
  * Testen von Änderungen, die außerhalb des strict-floating-Point-Vergleiche in BuiltInDataTypesTestBase zu ermöglichen.
  * Test-Änderungen, die Abfragetests erneut mit anderen SQL-Trennzeichen verwendet werden können.
* [https://github.com/aspnet/EntityFrameworkCore/pull/12072](https://github.com/aspnet/EntityFrameworkCore/pull/12072) -DbFunction Tests für die relationale Spezifikation Tests hinzufügen
  * So, dass diese Tests für alle Datenbankanbieter ausgeführt werden können
* [https://github.com/aspnet/EntityFrameworkCore/pull/12362](https://github.com/aspnet/EntityFrameworkCore/pull/12362) -Async testbereinigung
  * Entfernen Sie `Wait` Aufrufe nicht benötigte Async und einige Testmethoden umbenannt
* [https://github.com/aspnet/EntityFrameworkCore/pull/12666](https://github.com/aspnet/EntityFrameworkCore/pull/12666) -Vereinheitlichen der Protokollierung von Test-Infrastruktur
  * Hinzugefügt `CreateListLoggerFactory` und einige vorherige Infrastruktur für die Protokollierung, die Anbieter mit, dass diese Tests reagieren erforderlich ist, wird entfernt
* [https://github.com/aspnet/EntityFrameworkCore/pull/12500](https://github.com/aspnet/EntityFrameworkCore/pull/12500) – Führen Sie weitere Abfragetests, synchrone und asynchrone
  * Testnamen und die faktorisierung geändert hat, müssen die Anbieter mit, dass diese Tests reagieren
* [https://github.com/aspnet/EntityFrameworkCore/pull/12766](https://github.com/aspnet/EntityFrameworkCore/pull/12766) -Umbenennen von Navigationen in das Modell ComplexNavigations
  * Anbieter, die mithilfe dieser Tests möglicherweise reagieren
* [https://github.com/aspnet/EntityFrameworkCore/pull/12141](https://github.com/aspnet/EntityFrameworkCore/pull/12141) -Zurückgeben Sie Kontext an den Pool anstatt in Funktionstests disposing
  * Diese Änderung umfasst einige Tests refactoring Anbieter, zu reagieren ist möglicherweise erforderlich


### <a name="test-and-product-code-changes"></a>Test und Produkt codeänderungen

* [https://github.com/aspnet/EntityFrameworkCore/pull/12109](https://github.com/aspnet/EntityFrameworkCore/pull/12109) -Konsolidieren RelationalTypeMapping.Clone-Methoden
  * Änderungen in 2.1 die RelationalTypeMapping für eine Vereinfachung in abgeleiteten Klassen zulässig. Wir glauben nicht diese für Anbieter wichtige wurde allerdings Anbieter profitieren von dieser Änderung in ihrer abgeleiteten Typ zuordnen von Klassen.
* [https://github.com/aspnet/EntityFrameworkCore/pull/12069](https://github.com/aspnet/EntityFrameworkCore/pull/12069) -Markierten oder benannte Abfragen
  * Fügt die Infrastruktur für die Kennzeichnung von LINQ-Abfragen und having-diese Tags als Kommentare in die SQL-Anweisung angezeigt werden. Dies erfordert möglicherweise die Anbieter, in der SQL-Generierung zu reagieren.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13115](https://github.com/aspnet/EntityFrameworkCore/pull/13115) – Unterstützung von räumlichen Daten über NTS
  * Ermöglicht die Typmappings und Member Übersetzer außerhalb der Anbieter registriert werden
    * Anbieter müssen Basisklasse aufrufen. FindMapping() in ihrer Implementierung ITypeMappingSource, damit es funktioniert
  * Führen Sie dieses Muster zum Hinzufügen von Unterstützung von räumlichen Daten an Ihren Anbieter, die über Anbieter hinweg konsistent ist.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13199](https://github.com/aspnet/EntityFrameworkCore/pull/13199) – Fügen Sie erweiterte Debuggen für die diensterstellung-Anbieter hinzu
  * Ermöglicht das DbContextOptionsExtensions, um eine neue Schnittstelle zu implementieren, mit deren Hilfe können Benutzer zu verstehen, warum der internen Dienstanbieter neu erstellt wird
* [https://github.com/aspnet/EntityFrameworkCore/pull/13289](https://github.com/aspnet/EntityFrameworkCore/pull/13289) -Fügt CanConnect-API für die Verwendung von integritätsprüfungen
  * Dieser Pull Request hinzugefügt, das Konzept der `CanConnect` das von ASP.NET Core-Health verwendet wird überprüft, um festzustellen, ob die Datenbank verfügbar ist. Standardmäßig ruft die relationalen Implementierung nur `Exist`, aber der Anbieter etwas anderes implementieren können, falls erforderlich. Nicht relationale Anbieter müssen zum Implementieren der neuen API in der Reihenfolge für die integritätsprüfung verwendet werden kann.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13306](https://github.com/aspnet/EntityFrameworkCore/pull/13306) -Basis RelationalTypeMapping DbParameter-Größe nicht entsprechend zu aktualisieren
  * Beenden Sie die Größe wird standardmäßig festgelegt, da dies Abschneiden führen kann. Anbieter müssen möglicherweise ihre eigene Logik hinzufügen, wenn die Größe muss festgelegt werden.
* (https://github.com/aspnet/EntityFrameworkCore/pull/13372) -RevEng: Geben Sie immer Spaltentyp für die decimal-Spalten
  * Konfigurieren Sie immer Spaltentyp für dezimalspalten in eingerüsteten Code und nicht gemäß der Konvention konfigurieren.
  * Anbieter sollten keine Änderungen auf ihrer Seite erforderlich.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13469](https://github.com/aspnet/EntityFrameworkCore/pull/13469) -Fügt CaseExpression zum Generieren von SQL-CASE-Ausdrücke
* [https://github.com/aspnet/EntityFrameworkCore/pull/13648](https://github.com/aspnet/EntityFrameworkCore/pull/13648) -Fügt die Möglichkeit, geben Sie die replikationsdatentyp-Zuordnungen auf SqlFunctionExpression Store Typrückschluss von Argumenten und Ergebnisse zu verbessern.
