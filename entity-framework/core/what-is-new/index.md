---
title: EF Core-Releases und Planung
author: ajcvickers
ms.date: 03/03/2020
ms.assetid: C21F89EE-FB08-4ED9-A2A0-76CB7656E6E4
uid: core/what-is-new/index
ms.openlocfilehash: 2c41f65d1fead8430a39c6230a0f22506686504e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413205"
---
# <a name="ef-core-releases-and-planning"></a>EF Core-Releases und Planung

## <a name="stable-releases"></a>Stabile Releases

| Freigabe | Zielframework | Support bis zum | Links
|:--------|------------------|-----------------|------
| [EF Core 3.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.1.2) | .NET-Standard 2.0 | 3\. Dezember 2022 (LTS) | [Ankündigung](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-3-1-and-entity-framework-6-4/)
| ~~[EF Core 3.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.0.3)~~ | .NET Standard 2.1 | Abgelaufen: 3. März 2020 | [Ankündigung](https://devblogs.microsoft.com/dotnet/announcing-ef-core-3-0-and-ef-6-3-general-availability/) / [Wichtige Änderungen](ef-core-3.0/breaking-changes.md)
| ~~[EF Core 2.2](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.2.6)~~ | .NET-Standard 2.0 | Am 23. Dezember 2019 abgelaufen | [Ankündigung](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-2/)
| [EF Core 2.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.1.14) | .NET-Standard 2.0 | 21. August 2021 (LTS) | [Ankündigung](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-1/)
| ~~[EF Core 2.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.0.3)~~ | .NET-Standard 2.0 | Am 1. Oktober 2018 abgelaufen | [Ankündigung](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-0/)
| ~~[EF Core 1.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/1.1.6)~~ | .NET Standard 1.3 | Am 27. Juni 2019 abgelaufen | [Ankündigung](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-1-1/)
| ~~[EF Core 1.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/1.0.6)~~ | .NET Standard 1.3 | Am 27. Juni 2019 abgelaufen | [Ankündigung](https://devblogs.microsoft.com/dotnet/entity-framework-core-1-0-0-available/)

Weitere Informationen über die speziellen Plattformen, die von den jeweiligen EF Core-Releases unterstützt werden, finden Sie unter [Unterstützte Plattformen](../platforms/index.md).

Weitere Informationen zum Ablaufdatum des Supports und zu Releases mit langfristigem Support (long-term support, LTS) finden Sie unter [Supportrichtlinie für .NET](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).

## <a name="guidance-on-updating-to-new-releases"></a>Anleitung zum Aktualisieren auf neue Releases

* Unterstützte Releases werden auf Sicherheits- und andere kritische Fehler gepatcht. Verwenden Sie immer den aktuellen Patch eines vorhandenen Release. Bei EF Core 2.1 sollten Sie z. B. die Version 2.1.14 verwenden.
* Aktualisierungen von Hauptversionen (z. B. EF Core 2 auf EF Core 3) beinhalten häufig wichtige Änderungen. Bei der Aktualisierung von Hauptversionen werden gründliche Tests empfohlen. Anleitungen zum Umgang mit wichtigen Änderungen finden Sie unter den Links „Wichtige Änderungen“.
* Kleinere Versionsaktualisierungen enthalten in der Regel keine wichtigen Änderungen. Dennoch wird ein gründlicher Test empfohlen, da neue Funktionen zu Regressionen führen können.

## <a name="release-planning-and-schedules"></a>Releaseplanung und Zeitpläne

Die EF Core-Releases werden mit dem [Veröffentlichungsplan von .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md) abgestimmt.

Patchreleases werden normalerweise monatlich ausgeliefert, haben jedoch eine lange Vorlaufzeit.
Wir arbeiten daran, dies zu verbessern.

Weitere Informationen zur Entscheidung, was in den einzelnen Releases ausgeliefert werden soll, finden Sie unter [Die Releaseplanung](release-planning.md).
Unsere detaillierte Planung geht in der Regel nicht über die nächste Haupt- oder Nebenversion hinaus.

## <a name="ef-core-50"></a>EF Core 5.0

Der nächste stabile Release auf **EF Core 5.0** ist für November 2020 geplant.

Mithilfe der dokumentierten [Releaseplanung](release-planning.md) wurde ein [allgemeiner Plan für EF Core 5.0](ef-core-5.0/plan.md) erstellt.

Ihr Feedback zur Planung ist wichtig.
Sie können für ein Problem auf GitHub abstimmen (Daumen hoch 👍) und so angeben, dass dieses Problem wichtig ist.
Diese Daten werden dann in den Planungsprozess für das nächste Release aufgenommen.

### <a name="get-it-now"></a>Jetzt herunterladen

Die Pakete von EF Core 5.0 sind **jetzt** als [tägliche Builds](https://github.com/aspnet/AspNetCore/blob/master/docs/DailyBuilds.md) verfügbar. 

Mithilfe der täglichen Builds können Sie Probleme ausfindig machen und so früh wie möglich Feedback geben.
Je früher wir solches Feedback erhalten, desto wahrscheinlicher ist eine Umsetzung vor dem nächsten Release.
Wir bemühen uns sehr, bei den täglichen Builds eine gute Qualität aufrechtzuerhalten, indem wir für die jeweiligen Builds über 56.000 Tests pro Plattform ausführen.

Im Laufe des Jahres werden die Vorschaupakete für NuGet veröffentlicht.
