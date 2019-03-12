---
title: Neue Features in EF Core 3.0
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: cf0d2cf032b9aa319fe706aece5b1ea66a5d6251
ms.sourcegitcommit: a013e243a14f384999ceccaf9c779b8c1ae3b936
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57463362"
---
# <a name="new-features-included-in-ef-core-30-currently-in-preview"></a>Neue Features in EF Core 3.0 (aktuell in der Vorschauversion)

> [!IMPORTANT]
> Bitte beachten Sie, dass die Featuregruppen und Zeitpläne für künftige Releases jederzeit geändert werden können. Obwohl diese Seite bestmöglich aktualisiert wird, entspricht sie nicht immer den neuesten Plänen.

Die folgende Liste enthält die wichtigsten neuen Features, die für EF Core 3.0 geplant sind.
Die meisten davon sind aktuell nicht in der Vorschauversion enthalten und werden im Verlauf der RTM-Vorbereitung zur Verfügung gestellt.

Der Grund dafür ist, dass zu Releasebeginn zuerst die geplanten [Breaking Changes](xref:core/what-is-new/ef-core-3.0/breaking-changes) implementiert werden.
Allein durch viele dieser Änderungen wird EF Core bereits deutlich verbessert.
Für weitere Optimierungen werden zusätzliche Breaking Changes erforderlich sein. 

Eine vollständige Liste der aktuellen Fehlerbehebungen und Verbesserungen finden Sie [mit der entsprechenden Abfrage in unserem Issuetracker](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).

## <a name="linq-improvements"></a>LINQ-Verbesserungen 

[Issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

Dieses Feature befindet sich noch in der Entwicklungsphase und ist nicht in der aktuellen Vorschauversion enthalten.

LINQ ermöglicht es Ihnen, Datenbankabfragen zu schreiben, ohne die Sprache Ihrer Wahl zu verlassen. Gleichzeitig werden Rich-Type-Informationen verwendet, um von IntelliSense und der Typprüfung zur Kompilierungszeit zu profitieren.
Aber mit LINQ können Sie auch eine unbegrenzte Anzahl komplizierter Abfragen schreiben, und dies war schon immer eine große Herausforderung für LINQ-Anbieter.
In den ersten Versionen von EF Core wurde dies teilweise gelöst, indem wir herausgefunden haben, welche Teile einer Abfrage in SQL übersetzt werden konnten, und dann den Rest der Abfrage im Speicher des Clients ausführen ließen.
Diese clientseitige Ausführung kann in einigen Situationen wünschenswert sein, aber in vielen anderen Fällen kann sie zu ineffizienten Abfragen führen, die möglicherweise erst nach der Bereitstellung einer Anwendung in der Produktion identifiziert werden.
In EF Core 3.0 planen wir, tiefgreifende Änderungen an der Funktionsweise unserer LINQ-Implementierung und der Art ihrer Überprüfung vorzunehmen.
Die Ziele bestehen darin, die Implementierung robuster zu machen (z.B., um Breaking Changes in Patchreleases zu vermeiden), mehr Ausdrücke korrekt in SQL übersetzen zu können, in mehr Fällen effiziente Abfragen zu erzeugen und zu verhindern, dass ineffiziente Abfragen unentdeckt bleiben.

## <a name="cosmos-db-support"></a>Cosmos DB-Unterstützung 

[Issue #8443](https://github.com/aspnet/EntityFrameworkCore/issues/8443)

Dieses Feature ist in der aktuellen Vorschauversion enthalten. Die Arbeit daran ist aber noch nicht abgeschlossen. 

Wir arbeiten an einem Cosmos DB-Anbieter für EF Core, damit Entwickler, die mit dem EF-Programmiermodell vertraut sind, Azure Cosmos DB problemlos als Anwendungsdatenbank einsetzen können.
Ziel ist es, einige der Vorteile von Cosmos DB – beispielsweise die globale Verteilung, Always On-Verfügbarkeit, elastische Skalierbarkeit und geringe Latenz – noch besser für .NET-Entwickler zugänglich zu machen.
Der Anbieter wird die meisten EF Core-Features unterstützen, darunter die automatische Änderungsnachverfolgung, LINQ und Wertekonvertierungen anhand der SQL-API in Cosmos DB.
Diese Bemühungen begannen vor EF Core 2.2, und [wir haben einige Vorschauversionen des Anbieters zur Verfügung gestellt](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).
Der neue Plan sieht vor, den Anbieter neben EF Core 3.0 weiterzuentwickeln. 

## <a name="c-80-support"></a>C# 8.0-Unterstützung

[Issue #12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[Issue #10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)

Dieses Feature befindet sich noch in der Entwicklungsphase und ist nicht in der aktuellen Vorschauversion enthalten.

Wir möchten, dass unsere Kunden bei der Verwendung von EF Core einige der [neuen Features in C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) nutzen können, z.B. asynchrone Streams (einschließlich `await foreach`) und Nullable-Verweistypen.

## <a name="reverse-engineering-of-database-views"></a>Reverse Engineering (Zurückentwicklung) von Datenbanksichten

[Issue #1679](https://github.com/aspnet/EntityFrameworkCore/issues/1679)

Dieses Feature ist nicht in der aktuellen Vorschauversion enthalten.

[Abfragetypen](xref:core/modeling/query-types) wurden in EF Core 2.1 eingeführt und werden in EF Core 3.0 als Entitätstypen ohne Schlüssel betrachtet. Sie stellen Daten dar, die zwar aus einer Datenbank gelesen, aber nicht aktualisiert werden können.
Dank dieser Eigenschaft eignen sich diese Typen in den meisten Szenarios hervorragend für Datenbanksichten. Für das Reverse Engineering von Datenbanksichten ist daher geplant, dass die Erstellung von Entitätstypen ohne Schlüssel künftig automatisiert wird.

## <a name="property-bag-entities"></a>Entitäten mit Eigenschaftenbehältern 

[Issue #13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) und [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)

Dieses Feature befindet sich noch in der Entwicklungsphase und ist nicht in der aktuellen Vorschauversion enthalten. 

Bei diesem Feature geht es darum, Entitäten zu aktivieren, die Daten in indizierten Eigenschaften anstelle von regulären Eigenschaften speichern, und auch darum, Instanzen derselben .NET-Klasse (potenziell etwas so Einfaches wie `Dictionary<string, object>`) verwenden zu können, um verschiedene Entitätstypen im selben EF Core-Modell darzustellen.
Dieses Feature ist ein Sprungbrett, um m:n-Beziehungen ohne eine Joinentität zu unterstützen – eine der gefragtesten Verbesserungen für EF Core.

## <a name="ef-63-on-net-core"></a>EF 6.3 unter .NET Core 

[Issue EF6#271](https://github.com/aspnet/EntityFramework6/issues/271)

Dieses Feature befindet sich noch in der Entwicklungsphase und ist nicht in der aktuellen Vorschauversion enthalten. 

Uns ist bewusst, dass viele bestehende Anwendungen frühere Versionen von EF verwenden und dass die Portierung auf EF Core nur zur Nutzung von .NET Core unter Umständen mit einem erheblichen Aufwand verbunden ist.
Aus diesem Grund werden wir die nächste Version von EF 6 zur Ausführung auf .NET Core 3.0 anpassen.
Dies soll die Portierung vorhandener Anwendungen mit minimalen Änderungen erleichtern.
Es wird jedoch auch einige Einschränkungen geben. Beispiel:
- Neue Anbieter werden unter .NET Core nicht nur mit den bereits unterstützten SQL Server-Datenbanken, sondern auch mit anderen arbeiten müssen.
- Die Unterstützung räumlicher Daten für SQL Server wird nicht aktiviert.

Beachten Sie außerdem, dass aktuell für EF 6 keine weiteren Features geplant sind.
