---
title: Wegweiser für Entity Framework Core
author: divega
ms.author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
ms.technology: entity-framework-core
uid: core/what-is-new/roadmap
ms.openlocfilehash: f319aabb53019e0f153c911dde5814328423081b
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949165"
---
# <a name="entity-framework-core-roadmap"></a>Wegweiser für Entity Framework Core

> [!IMPORTANT]
> Bitte beachten Sie, dass die Featuregruppen und Zeitpläne für künftige Releases jederzeit geändert werden können. Obwohl diese Seite bestmöglich aktualisiert wird, entspricht sie nicht immer den neuesten Plänen.

Die stabile Version von EF Core 2.1 wurde am 30. Mai 2018 veröffentlicht. Weitere Informationen zu diesem Release finden Sie unter [Neue Features in EF Core 2.1](xref:core/what-is-new/ef-core-2.1).

Die [Planung](#release-planning-process) für das Release nach 2.1 ist noch nicht abgeschlossen.

## <a name="schedule"></a>Zeitplan

Der Zeitplan für EF Core ist entspricht dem Zeitplan für [.NET Core](https://github.com/dotnet/core/blob/master/roadmap.md) und [ASP.NET Core](https://github.com/aspnet/Home/wiki/Roadmap).

## <a name="backlog"></a>Rückstand

Wir verwenden den [Backlog-Meilenstein](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) in unserer Problemverfolgung, um eine detaillierte Liste der Probleme und Funktionen zu pflegen. Kunden können diese kommentieren und darüber abstimmen.

Wenn wir davon ausgehen, dass Probleme zu einem späteren Zeitpunkt bearbeitet werden oder dass sich jemand aus der Community darum kümmert, lassen wir sie offen. Das bedeutet jedoch nicht, dass wir sie innerhalb eines bestimmten Zeitraums einem bestimmten Meilenstein des [Releaseplanungsprozesses](#release-planning-process) zuweisen.

Sollte die Implementierung eines Features zu keinem Zeitpunkt geplant sein, wird das Problem wahrscheinlich geschlossen. Ein geschlossenes Problem kann zu einem späteren Zeitpunkt wieder an Aktualität gewinnen, wenn wir neue Informationen dazu erhalten.

Alles in allem liegen nicht ausreichend Informationen vor, um zu gewährleisten, dass Problem X bis zum Datum oder Release Y gelöst wird. Wie bei allen Softwareprojekten können sich Prioritäten, Releasepläne und die verfügbaren Ressourcen jederzeit ändern.

## <a name="release-planning-process"></a>Die Releaseplanung

Benutzer fragen oftmals, wie bestimmte Features für bestimmte Releases ausgewählt werden. Das Backlog wird sicherlich natürlich nicht 1:1 in Releasepläne übersetzt. Die Features von EF6 werden auch nicht automatisch in EF Core implementiert.

Es ist schwierig, den gesamten Prozess der Releaseplanung hier darzulegen. Einerseits, weil er zu einem großen Teil aus der Besprechung der Features, Marktchancen und Prioritäten besteht und andererseits, weil er in der Regel bei jeder Version anders ist. Es ist jedoch relativ einfach, die Fragen zusammenzufassen, die wir uns bei der Entscheidung stellen, womit die Arbeit weitergehen soll:

1. **Wie viele Entwickler werden das Feature zukünftig verwenden, und welche Verbesserungen für die Anwendungen/Benutzererfahrung wird es bringen?** Wir sammeln viel Feedback aus verschiedenen Quellen, zu denen auch Kommentare und die abgegebenen Stimmen zählen.

2. **Welche Problemumgehungen können genutzt werden, wenn dieses Feature noch nicht implementiert werden soll?** Viele Entwickler können z.B. eine Jointabelle zuordnen, um die fehlende Unterstützung für nativen m:n-Support zu umgehen. Dies gilt zwar nicht für alle Entwickler, ist aber trotzdem eine wichtige Lösung.

3. **Wird durch die Implementierung dieses Features die Architektur von EF Core weiterentwickelt, sodass die Implementierung anderer Features erleichtert wird?** Features, die Bausteine für andere Features sind, werden üblicherweise bevorzugt. Die Tabellenaufteilung für eigene Typen bringt uns z.B. der Unterstützung von TPT einen Schritt näher.

4. **Ist dieses Feature ein Erweiterungspunkt?** In der Regel werden Erweiterungspunkte vorgezogen, da Entwickler damit einfacher eigene Verhaltensweisen verknüpfen und so einen Teil der fehlenden Funktionalität ersetzen können. Einiges davon ist als Ausgangspunkt für das verzögerte Laden gedacht.

5. **Welche Synergien erzeugt das Feature in Kombination mit anderen Produkten?** Normalerweise werden Features bevorzugt, die die Kompatibilität von EF Core mit anderen Produkten erhöhen oder die Benutzererfahrung beim Verwenden von Produkten wie .NET Core, der neuesten Version von Visual Studio, Microsoft Azure etc. deutlich verbessern.

6. **Welche Kompetenzen haben die Personen, die an dem Feature arbeiten, und wie lassen sich diese Ressourcen am besten einsetzen?** Alle Mitglieder des EF-Teams und alle Mitwirkenden aus der Community können auf unterschiedliche Erfahrungsschätze in verschiedenen Bereichen zurückgreifen. Dementsprechend muss geplant werden. Selbst wenn „alle mit anpacken“ und an einem bestimmten Feature wie z.B. GroupBy-Übersetzungen oder m:n arbeiten sollten, ist das nicht immer die beste Lösung.

Wie zuvor erläutert, entwickelt sich der Prozess bei jedem Release. In der Zukunft möchten wir den Mitgliedern der Entwickler-Community mehr Möglichkeiten geben, ihre Ideen in die Releaseplanung einzubringen, indem beispielsweise das Prüfen vorgeschlagener Features und des Releaseplans vereinfacht wird.
