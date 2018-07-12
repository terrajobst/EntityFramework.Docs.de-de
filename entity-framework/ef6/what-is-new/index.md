---
title: 'Neuigkeiten: EF6'
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
caps.latest.revision: 3
ms.openlocfilehash: 0da2ce778a765037ecacd0726cbb7cda08b5683f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2018
ms.locfileid: "37911704"
---
# <a name="whats-new-in-ef6"></a>Neuigkeiten in EF6

Es wird dringend empfohlen, dass Sie die neueste veröffentlichte Version von Entity Framework verwenden. So wird sichergestellt, dass Sie die neuesten Features nutzen und die höchste Stabilität erhalten.
Uns ist jedoch klar, dass Sie möglicherweise eine frühere Version verwenden müssen oder die neuen Verbesserungen für die aktuelle Vorabversion ausprobieren möchten.
Wie Sie bestimmte Versionen von EF installieren, erfahren Sie unter [Get Entity Framework (Beziehen von Entity Framework)](~/ef6/fundamentals/install.md).

Auf dieser Seite werden die Funktionen beschrieben, die jeweils in neuen Releases enthalten sind.

## <a name="recent-releases"></a>Aktuelle Releases

### <a name="ef-tools-update-in-visual-studio-2017-157"></a>Update für die EF-Tools in Visual Studio 2017 15.7

Im Mai 2018 haben wir eine aktualisierte Version der EF6-Tools in Visual Studio 2017 15.7 veröffentlicht.
Sie enthält Verbesserungen in einigen üblichen Problembereichen:

- Überarbeitung der Barrierefreiheit der Benutzeroberfläche
- Problemumgehung für SQL Server-Leistungsverlust [#4](https://github.com/aspnet/entityframework6/issues/4)
- Updatemodell aus der Datenbankunterstützung für größere Modelle in SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)

Mit dieser neue Version der EF-Tools wird die EF 6.2-Runtime installiert, wenn ein Modell in einem neuen Projekt erstellt wird. Bei älteren Versionen von Visual Studio ist es möglich, die EF 6.2-Runtime (sowie alle früheren Version von Entity Framework) zu verwenden, indem Sie die entsprechende Version des NuGet-Pakets installieren.

### <a name="ef-62-runtime"></a>EF 6.2-Runtime

Die EF 6.2-Runtime wurde im Januar 2017 für NuGet veröffentlicht.
Es ist vor allem der Verdienst unserer Community von Open-Source-Mitwirkenden, dass EF 6.2 zahlreiche [Fehlerbehebungen](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) und [Produktverbesserungen](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20) enthält.

Hier ist eine kurze Liste der wichtigsten Änderungen, die die EF 6.2-Runtime betreffen:

- Startzeit reduzieren, indem abgeschlossene Code First-Modelle aus einem beständigen Cache geladen werden [#275](https://github.com/aspnet/EntityFramework6/issues/275)
- Fluent-API zum Definieren von Indizes [#274](https://github.com/aspnet/EntityFramework6/issues/274)
- DbFunctions.Like() hinzufügen, um das Schreiben von LINQ-Abfragen zu aktivieren, die in SQL als LIKE übersetzt werden [#241](https://github.com/aspnet/EntityFramework6/issues/241)
- Migrate.exe unterstützt jetzt die Option „-script“ [#240](https://github.com/aspnet/EntityFramework6/issues/240)
- EF6 funktioniert jetzt mit Schlüsselwerten, die von einer Sequenz in SQL Server generiert wurden [#165](https://github.com/aspnet/EntityFramework6/issues/165)
- Updateliste der vorübergehenden Fehler für die SQL Azure-Ausführungsstrategie [#83](https://github.com/aspnet/EntityFramework6/issues/83)
- Fehler: Wiederholen von Abfragen oder SQL-Befehlen schlägt mit folgendem Hinweis fehl: „The SqlParameter is already contained by another SqlParameterCollection“ (Der SqlParameter ist bereits in einer anderen SqlParameterCollection enthalten) [#81](https://github.com/aspnet/EntityFramework6/issues/81)
- Fehler: Bei der Auswertung von DbQuery.ToString() tritt häufig ein Timeout im Debugger auf [#73](https://github.com/aspnet/EntityFramework6/issues/73)

## <a name="future-releases"></a>Zukünftige Releases

Informationen zu zukünftigen Versionen von EF6, finden Sie in unserer [Roadmap](roadmap.md).

## <a name="past-releases"></a>Frühere Releases

Auf der Seite [Frühere Releases](past-releases.md) finden Sie ein Archiv aller früheren Versionen von EF und die wichtigsten Features, die in den einzelnen Releases eingeführt wurden. 
