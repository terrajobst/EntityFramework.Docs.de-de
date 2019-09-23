---
title: 'Neuigkeiten: EF6'
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: 568790d9c9bb7dd2213907bef8fa090710cd3ba0
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149129"
---
# <a name="whats-new-in-ef6"></a>Neuigkeiten in EF6

Es wird dringend empfohlen, dass Sie die neueste veröffentlichte Version von Entity Framework verwenden. So wird sichergestellt, dass Sie die neuesten Features nutzen und die höchste Stabilität erhalten.
Uns ist jedoch klar, dass Sie möglicherweise eine frühere Version verwenden müssen oder die neuen Verbesserungen für die aktuelle Vorabversion ausprobieren möchten.
Wie Sie bestimmte Versionen von EF installieren, erfahren Sie unter [Get Entity Framework (Beziehen von Entity Framework)](~/ef6/fundamentals/install.md).

## <a name="ef-630"></a>EF 6.3.0

Die EF 6.3.0-Runtime wurde im September 2019 für NuGet veröffentlicht. Das Hauptziel dieses Release bestand darin, die Migration vorhandener Anwendungen, die EF 6 verwenden, zu . NET Core 3.0 zu ermöglichen. Die Community hat außerdem verschiedene Fehlerbehebungen und Verbesserungen beigetragen. Einzelheiten zu den gelösten Problemen finden Sie in den [Meilensteinen](https://github.com/aspnet/EntityFramework6/milestones?state=closed) zu Version 6.3.0. In Folgenden werden einige der wichtigsten aufgeführt:

- Unterstützung für .NET Core 3.0
  - Das EntityFramework-Paket verwendet nun .NET Standard 2.1 zusätzlich zu .NET Framework 4.x als Ziel.
  - Die Migrationsbefehle wurden so umgeschrieben, dass Sie außerhalb des Prozesses ausgeführt werden und mit Projekten im SDK-Stil funktionieren.
- Unterstützung für HierarchyId von SQL Server
- Verbesserte Kompatibilität mit PackageReference von Roslyn und NuGet
- „ef6. exe“ wurde zum Aktivieren, Hinzufügen, Skripten und Anwenden von Migrationen aus Assemblys hinzugefügt. Dies ersetzt „migrate.exe“

## <a name="past-releases"></a>Frühere Releases

Auf der Seite [Frühere Releases](past-releases.md) finden Sie ein Archiv aller früheren Versionen von EF und die wichtigsten Features, die in den einzelnen Releases eingeführt wurden.
