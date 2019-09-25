---
title: Neuerungen – EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: C21F89EE-FB08-4ED9-A2A0-76CB7656E6E4
uid: core/what-is-new/index
ms.openlocfilehash: 2ca4915fca515b4bdbfeb77bc7b02f15ce1704b6
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197729"
---
# <a name="whats-new-in-ef-core"></a>Neuerungen in EF Core

## <a name="recent-releases"></a>Aktuelle Releases

- **EF Core 3.0** (neuestes stabiles Release) 
  - [Neue Features](xref:core/what-is-new/ef-core-3.0/index) 
  - [Breaking Changes](xref:core/what-is-new/ef-core-3.0/breaking-changes), die Sie beim Upgrade beachten sollten
- [EF Core 2.2](xref:core/what-is-new/ef-core-2.2)
- [EF Core 2.1](xref:core/what-is-new/ef-core-2.1) (neueste Version mit langfristigem Support)

## <a name="product-roadmap"></a>Produktroadmap

> [!IMPORTANT]
> Bitte beachten Sie, dass die Featuregruppen und Zeitpläne für künftige Releases jederzeit geändert werden können. Obwohl diese Seite bestmöglich aktualisiert wird, entspricht sie nicht immer den neuesten Plänen.

### <a name="future-releases"></a>Zukünftige Releases

- **EF Core 3.1**  
  - In der aktiven Entwicklungsphase
  - Wird [kleinere Verbesserungen bei Leistung, Qualität und Stabilität](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.1.0+sort%3Areactions-desc) enthalten
  - Als Version mit Long-Term Support (LTS) geplant
  - Derzeit für Dezember 2019 geplant
- **EF Core "vNext"**   
  - Nächste Hauptversion von EF Core, die zusammen mit .NET 5 ausgeliefert werden soll
  - Die Planungsphase für diese Version hat noch nicht begonnen, und es wurden noch keine Features angekündigt.  

### <a name="schedule"></a>Zeitplan

Der Releasezeitplan für EF Core ist mit dem für [.NET Core](https://github.com/dotnet/core/blob/master/roadmap.md) abgestimmt.

### <a name="backlog"></a>Backlog

Der [Backlogmeilenstein](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog+sort%3Areactions-%2B1-desc) in der Problemverfolgung enthält Probleme, die wir beheben möchten oder die unserer Meinung nach Communitymitglieder lösen können.
Wir freuen uns darüber, wenn Benutzer Kommentare und Feedback zu diesen Problemen hinterlassen.
Mitwirkenden, die eines dieser Probleme behandeln möchten, wird empfohlen, zunächst eine Diskussion über den richtigen Ansatz anzustoßen.

Es gibt keine Garantie, dass bestimmte Features einer bestimmten EF Core-Version bearbeitet werden.
Wie bei allen Softwareprojekten können sich Prioritäten, Releasezeitpläne und verfügbare Ressourcen jederzeit ändern.
Wenn wir ein Problem jedoch in einem bestimmten Zeitfenster lösen möchten, ordnen wir es einem Releasemeilenstein statt dem Backlogmeilenstein zu.
Wir verschieben Probleme regelmäßig zwischen Backlog- und Releasemeilensteinen im Rahmen der [Releaseplanung](#release-planning-process).

Wenn die Behandlung eines bestimmten Problems nicht geplant ist, wird dieses voraussichtlich geschlossen.
Allerdings können wir uns einem geschlossenen Problem erneut widmen, wenn uns neue Informationen vorliegen.

### <a name="release-planning-process"></a>Die Releaseplanung

Benutzer fragen oftmals, wie bestimmte Features für bestimmte Releases ausgewählt werden.
Das Backlog kann nicht automatisch in Releasepläne übersetzt werden.
Die Features von EF6 werden auch nicht automatisch in EF Core implementiert.

Es ist schwierig, den gesamten Prozess der Releaseplanung im Detail darzustellen.
Einen Großteil macht die Diskussion über spezifische Funktionen, Möglichkeiten und Prioritäten aus. Der Prozess selbst entwickelt sich mit jedem Release weiter.
Das ist eine Zusammenfassung der Fragen, die wir uns bei der Entscheidung stellen, womit die Arbeit weitergehen soll:

1. **Wie viele Entwickler werden das Feature zukünftig verwenden, und welche Verbesserungen für die Anwendungen oder Benutzererfahrung bietet es?** Um diese Frage zu beantworten, sammeln wir Feedback aus verschiedensten Quellen, zu denen auch Kommentare und abgegebene Stimmen zählen.

2. **Welche Problemumgehungen können genutzt werden, wenn dieses Feature noch nicht implementiert wird?** Viele Entwickler können z.B. eine Verknüpfungstabelle zuordnen, um die fehlende Unterstützung für native m:n-Unterstützung zu umgehen. Natürlich wollen nicht alle Entwickler diese Option nutzen, doch ausschlaggebend ist, dass viele es können.

3. **Wird durch die Implementierung dieses Features die Architektur von EF Core weiterentwickelt, sodass die Implementierung anderer Features erleichtert wird?** In der Regel werden Features bevorzugt, die als Bausteine für andere Features dienen können. Beispielsweise können Eigenschaftenbehälterentitäten beim Entwickeln der m:n-Unterstützung helfen, und Entitätskonstruktoren haben unsere Lazy Loading-Unterstützung ermöglicht.

4. **Ist dieses Feature ein Erweiterungspunkt?** In der Regel werden Erweiterungspunkte regulären Features vorgezogen, da Entwickler damit eigene Verhaltensweisen verknüpfen und so einen Teil der fehlenden Funktionalität ersetzen können.

5. **Welche Synergien erzeugt das Feature in Kombination mit anderen Produkten?** Normalerweise werden Features bevorzugt, die die Verwendbarkeit von EF Core mit anderen Produkten wie .NET Core, der aktuellen Version von Visual Studio, Microsoft Azure usw. deutlich verbessern.

6. **Welche Qualifikationen haben die Personen, die am Feature arbeiten, und wie lassen sich diese Ressourcen am besten einsetzen?** Alle Mitglieder des EF-Teams und alle Mitwirkenden aus der Community können auf unterschiedliche Erfahrungsschätze in verschiedenen Bereichen zurückgreifen. Dementsprechend muss geplant werden. Selbst wenn „alle mit anpacken“ und an einem bestimmten Feature wie GroupBy-Übersetzungen oder m:n arbeiten möchten, ist das nicht immer der beste Ansatz.

Wie bereits erwähnt, entwickelt sich der Prozess mit jedem Release weiter.
In Zukunft möchten wir für Communitymitglieder weitere Möglichkeiten hinzufügen, zu den Releaseplänen beitragen zu können.
So möchten wir beispielsweise die Überprüfung von Featureentwürfen und des Releaseplans selbst erleichtern.