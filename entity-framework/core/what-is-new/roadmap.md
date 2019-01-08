---
title: Wegweiser für Entity Framework Core
author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
uid: core/what-is-new/roadmap
ms.openlocfilehash: f18de8e8cb4fbe81bb2f983a00c9dd2f46be6073
ms.sourcegitcommit: a6082a2caee62029f101eb1000656966195cd6ee
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/10/2018
ms.locfileid: "53182019"
---
# <a name="entity-framework-core-roadmap"></a>Wegweiser für Entity Framework Core

> [!IMPORTANT]
> Bitte beachten Sie, dass die Featuregruppen und Zeitpläne für künftige Releases jederzeit geändert werden können. Obwohl diese Seite bestmöglich aktualisiert wird, entspricht sie nicht immer den neuesten Plänen.

### <a name="ef-core-30"></a>EF Core 3.0

Nach der Veröffentlichung von EF Core 2.2 liegt unser Hauptaugenmerk nun auf EF Core 3.0, das auf die Releases .NET Core 3.0 und ASP.NET 3.0 abgestimmt wird.

Es wurden noch keine neuen Features fertiggestellt, sodass die  [EF Core 3.0 Preview 1-Pakete, die im Dezember 2018 im NuGet-Katalog](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.0.0-preview.18572.1)  veröffentlicht wurden, ausschließlich [Bugfixes, kleinere Verbesserungen und Änderungen enthalten, die wir in Vorbereitung auf die 3.0-Version](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aissue+milestone%3A3.0.0+is%3Aclosed+label%3Aclosed-fixed) vorgenommen haben.

Tatsächlich müssen wir unsere [Releaseplanung](#release-planning-process) für 3.0 noch verfeinern, um sicherzustellen, dass wir über den richtigen Satz an Features verfügen, die in der vorgegebenen Zeit fertiggestellt werden können.
Wir werden weitere Informationen bekanntgeben, sobald wir mehr Klarheit haben. Die nachfolgende Liste zeigt einige allgemeine Designs und Features, an denen wir arbeiten möchten:

- **LINQ-Verbesserungen ([#12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795))**: LINQ ermöglicht es Ihnen, Datenbankabfragen zu schreiben, ohne die Sprache Ihrer Wahl zu verlassen. Gleichzeitig werden Rich-Type-Informationen verwendet, um von IntelliSense und der Typprüfung zur Kompilierungszeit zu profitieren.
  Aber mit LINQ können Sie auch eine unbegrenzte Anzahl komplizierter Abfragen schreiben, und dies war schon immer eine große Herausforderung für LINQ-Anbieter.
  In den ersten Versionen von EF Core wurde dies teilweise gelöst, indem wir herausgefunden haben, welche Teile einer Abfrage in SQL übersetzt werden konnten, und dann den Rest der Abfrage im Speicher des Clients ausführen ließen.
  Diese clientseitige Ausführung kann in einigen Situationen wünschenswert sein, aber in vielen anderen Fällen kann sie zu ineffizienten Abfragen führen, die möglicherweise erst nach der Bereitstellung einer Anwendung in der Produktion identifiziert werden.
  In EF Core 3.0 planen wir, tiefgreifende Änderungen an der Funktionsweise unserer LINQ-Implementierung und der Art ihrer Überprüfung vorzunehmen.
  Unsere Ziele bestehen darin, die Implementierung robuster zu machen (z.B. um Breaking Changes in Patchreleases zu vermeiden), mehr Ausdrücke korrekt in SQL übersetzen zu können, in mehr Fällen effiziente Abfragen zu erzeugen und zu verhindern, dass ineffiziente Abfragen unentdeckt bleiben.

- **Cosmos DB-Unterstützung ([#8443](https://github.com/aspnet/EntityFrameworkCore/issues/8443))**: Wir arbeiten an einem Cosmos DB-Anbieter für EF Core, damit Entwickler, die mit dem EF-Programmiermodell vertraut sind, Azure Cosmos DB problemlos als Anwendungsdatenbank einsetzen können.
  Unser Ziel ist es, einige der Vorteile von Cosmos DB – beispielsweise die globale Verteilung, Always On-Verfügbarkeit, elastische Skalierbarkeit und geringe Latenz – noch besser für .NET-Entwickler zugänglich zu machen.
  Der Anbieter wird die meisten EF Core-Features unterstützen, darunter die automatische Änderungsnachverfolgung, LINQ und Wertekonvertierungen anhand der SQL-API in Cosmos DB. Diese Bemühungen begannen vor EF Core 2.2, und [wir haben einige Vorschauversionen des Anbieters zur Verfügung gestellt](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).
  Der neue Plan sieht vor, den Anbieter neben EF Core 3.0 weiterzuentwickeln.   

- **C# 8.0-Unterstützung ([#12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047))**: Wir möchten, dass unsere Kunden bei der Verwendung von EF Core einige der [neuen Features in C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) nutzen können, wie z.B. asynchrone Streams (einschließlich „await foreach“) und NULL-fähige Verweistypen.

- **Reverse Engineering von Datenbankansichten in Abfragetypen ([#1679](https://github.com/aspnet/EntityFrameworkCore/issues/1679))**: In EF Core 2.1 wurde die Unterstützung für Abfragetypen hinzugefügt, die Daten darstellen können, die aus der Datenbank gelesen, aber nicht aktualisiert werden können.
  Abfragetypen eignen sich hervorragend für die Zuordnung von Datenbanksichten, deshalb möchten wir in EF Core 3.0 die Erstellung von Abfragetypen für Datenbanksichten automatisieren.

- **Eigenschaftenbehälterentitäten ([#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) und [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914))**: Bei diesem Feature geht es darum, Entitäten zu aktivieren, die Daten in indizierten Eigenschaften anstelle von regulären Eigenschaften speichern, und auch darum, Instanzen derselben .NET-Klasse (potenziell etwas so Einfaches wie `Dictionary<string, object>`) verwenden zu können, um verschiedene Entitätstypen im selben EF Core-Modell darzustellen.
  Dieses Feature ist ein Sprungbrett, um m:n-Beziehungen ohne eine Joinentität zu unterstützen – eine der gefragtesten Verbesserungen für EF Core.

- **EF 6.3 auf .NET Core ([EF6 #271](https://github.com/aspnet/EntityFramework6/issues/271))**: Uns ist bewusst, dass viele bestehende Anwendungen frühere Versionen von EF verwenden und dass die Portierung auf EF Core nur zur Nutzung von .NET Core unter Umständen mit einem erheblichen Aufwand verbunden ist.
  Aus diesem Grund werden wir die nächste Version von EF 6 zur Ausführung auf .NET Core 3.0 anpassen.
  Dies soll die Portierung vorhandener Anwendungen mit minimalen Änderungen erleichtern.
  Es wird einige Einschränkungen geben (z.B. sind neue Anbieter erforderlich, die Unterstützung räumlicher Daten mit SQL Server wird nicht aktiviert), und es sind keine neuen Features für EF 6 geplant.

In der Zwischenzeit können Sie mit [dieser Abfrage in der Problemverfolgung](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc) Arbeitselemente anzeigen, die vorläufig Release 3.0 zugeordnet wurden.

## <a name="schedule"></a>Zeitplan

Der Zeitplan für EF Core entspricht dem Zeitplan für [.NET Core](https://github.com/dotnet/core/blob/master/roadmap.md) und [ASP.NET Core](https://github.com/aspnet/Home/wiki/Roadmap).

## <a name="backlog"></a>Backlog

Der [Backlogmeilenstein](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) in der Problemverfolgung enthält Probleme, die wir beheben möchten oder die unserer Meinung nach Communitymitglieder lösen können.
Wir freuen uns darüber, wenn Benutzer Kommentare und Feedback zu diesen Problemen hinterlassen.
Mitwirkenden, die eines dieser Probleme behandeln möchten, wird empfohlen, zunächst eine Diskussion über den richtigen Ansatz anzustoßen.

Es gibt keine Garantie, dass bestimmte Features einer bestimmten EF Core-Version bearbeitet werden.
Wie bei allen Softwareprojekten können sich Prioritäten, Releasezeitpläne und verfügbare Ressourcen jederzeit ändern.
Wenn wir ein Problem jedoch in einem bestimmten Zeitfenster lösen möchten, ordnen wir es einem Releasemeilenstein statt dem Backlogmeilenstein zu.
Wir verschieben Probleme regelmäßig zwischen Backlog- und Releasemeilensteinen im Rahmen der [Releaseplanung](#release-planning-process).

Wenn die Behandlung eines bestimmten Problems nicht geplant ist, wird dieses voraussichtlich geschlossen.
Allerdings können wir uns einem geschlossenen Problem erneut widmen, wenn uns neue Informationen vorliegen.

## <a name="release-planning-process"></a>Die Releaseplanung

Benutzer fragen oftmals, wie bestimmte Features für bestimmte Releases ausgewählt werden.
Das Backlog kann nicht automatisch in Releasepläne übersetzt werden.
Die Features von EF6 werden auch nicht automatisch in EF Core implementiert.

Es ist schwierig, den gesamten Prozess der Releaseplanung im Detail darzustellen.
Einen Großteil macht die Diskussion über spezifische Funktionen, Möglichkeiten und Prioritäten aus. Der Prozess selbst entwickelt sich mit jedem Release weiter.
Das ist eine Zusammenfassung der Fragen, die wir uns bei der Entscheidung stellen, womit die Arbeit weitergehen soll:

1. **Wie viele Entwickler werden das Feature zukünftig verwenden, und welche Verbesserungen für die Anwendungen/Benutzererfahrung wird es bringen?** Um das zu beantworten, sammeln wir viel Feedback aus verschiedenen Quellen, zu denen auch Kommentare und die abgegebenen Stimmen zählen.

2. **Welche Problemumgehungen können genutzt werden, wenn dieses Feature noch nicht implementiert wird?** Viele Entwickler können z.B. eine Verknüpfungstabelle zuordnen, um die fehlende Unterstützung für native m:n-Unterstützung zu umgehen. Natürlich wollen nicht alle Entwickler diese Option nutzen, doch ausschlaggebend ist, dass viele es können.

3. **Wird durch die Implementierung dieses Features die Architektur von EF Core weiterentwickelt, sodass die Implementierung anderer Features erleichtert wird?** In der Regel werden Features bevorzugt, die als Bausteine für andere Features dienen können. Beispielsweise können Eigenschaftenbehälterentitäten beim Entwickeln der m:n-Unterstützung helfen, und Entitätskonstruktoren haben unsere Lazy Loading-Unterstützung aktiviert. 

4. **Ist dieses Feature ein Erweiterungspunkt?** In der Regel werden Erweiterungspunkte regulären Features vorgezogen, da Entwickler damit eigene Verhaltensweisen verknüpfen und so einen Teil der fehlenden Funktionalität ersetzen können. 

5. **Welche Synergien erzeugt das Feature in Kombination mit anderen Produkten?** Normalerweise werden Features bevorzugt, die die Verwendbarkeit von EF Core mit anderen Produkten wie .NET Core, der neuesten Version von Visual Studio, Microsoft Azure deutlich verbessern.

6. **Welche Qualifikationen haben die Personen, die am Feature arbeiten, und wie lassen sich diese Ressourcen am besten einsetzen?** Alle Mitglieder des EF-Teams und alle Mitwirkenden aus der Community können auf unterschiedliche Erfahrungsschätze in verschiedenen Bereichen zurückgreifen. Dementsprechend muss geplant werden. Selbst wenn „alle mit anpacken“ und an einem bestimmten Feature wie GroupBy-Übersetzungen oder m:n arbeiten möchten, ist das nicht immer der beste Ansatz.

Wie bereits erwähnt, entwickelt sich der Prozess mit jedem Release weiter.
In Zukunft möchten wir weitere Möglichkeiten für Communitymitglieder hinzufügen, damit sie zu den Releaseplänen beitragen können.
So möchten wir beispielsweise die Überprüfung von Featureentwürfen und des Releaseplans selbst erleichtern.
