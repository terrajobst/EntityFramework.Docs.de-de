---
title: Wegweiser für Entity Framework Core
author: divega
ms.date: 02/20/2018
ms.assetid: 834C9729-7F6E-4355-917D-DE3EE9FE149E
uid: core/what-is-new/roadmap
ms.openlocfilehash: e9d1a707c643036fa15b1e0e46730eadefd65518
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562467"
---
# <a name="entity-framework-core-roadmap"></a>Wegweiser für Entity Framework Core

> [!IMPORTANT]
> Bitte beachten Sie, dass die Featuregruppen und Zeitpläne für künftige Releases jederzeit geändert werden können. Obwohl diese Seite bestmöglich aktualisiert wird, entspricht sie nicht immer den neuesten Plänen.

## <a name="ef-core-30"></a>EF Core 3.0

Nach der Veröffentlichung von EF Core 2.2 liegt der Hauptfokus nun auf EF Core 3.0.
Informationen zu den geplanten [neuen Features](xref:core/what-is-new/ef-core-3.0/features) und beabsichtigten [Breaking Changes](xref:core/what-is-new/ef-core-3.0/breaking-changes), die in diesem Release enthalten sind, finden Sie unter [Neue Features in EF Core 3.0](xref:core/what-is-new/ef-core-3.0/index).

## <a name="schedule"></a>Zeitplan

Der Releasezeitplan für EF Core ist mit dem für [.NET Core](https://github.com/dotnet/core/blob/master/roadmap.md) abgestimmt.

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
