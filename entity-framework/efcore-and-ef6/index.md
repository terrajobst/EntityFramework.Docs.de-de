---
title: Vergleichen von EF Core und EF 6
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 09ffd8408ea8575ea367eaf2bdab4002db5c619e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997123"
---
# <a name="compare-ef-core--ef6"></a>Vergleichen von EF Core und EF 6

Es gibt zwei unterschiedliche Versionen von Entity Framework: Entity Framework Core und Entity Framework 6.

## <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework 6 (EF 6) ist eine bewährte Datenzugriffstechnologie mit Features und Stabilisierungsmaßnahmen, die das Ergebnis jahrelanger Arbeit sind. Es wurde erstmals in 2008 im Rahmen von .NET Framework 3.5 SP1 und Visual Studio 2008 SP1 veröffentlicht. Ab Release EF 4.1 wurde es im Lieferumfang des [NuGet-Pakets „EntityFramework“](https://www.nuget.org/packages/EntityFramework/) ausgeliefert, derzeit eines der am häufigsten verwendeten Pakete auf NuGet.org.

EF 6 ist weiterhin ein unterstütztes Produkt, für das einige Zeit lang weiterhin Support durch Fehlerbehebungen und kleinere Verbesserungen bereitgestellt wird.

## <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework Core (EF Core) ist eine einfache, erweiterbare und plattformübergreifende Version von Entity Framework. In EF Core werden zahlreiche Verbesserungen und neue Features im Vergleich zu EF 6 eingeführt. EF Core ist gleichzeitig eine neue Codebasis und nicht so ausgereift wie EF 6.

In EF Core wird die Entwickleroberfläche von EF 6 wie auch die meisten APIs der obersten Ebene beibehalten, sodass EF Core für Benutzer, die EF 6 verwendet haben, sehr vertraut wirken wird. Gleichzeitig wird EF Core über eine völlig neue Gruppe von Kernkomponenten erstellt. Das bedeutet, dass EF Core nicht automatisch alle Features von EF 6 erbt. Einige dieser Features werden in zukünftigen Releases vorkommen, während andere seltener verwendete Features nicht in EF Core implementiert werden.

In der neuen, erweiterbaren und einfachen EF Core-Version konnten außerdem einige Features zu EF Core hinzugefügt werden, die nicht in EF 6 implementiert werden (z.B. Alternativschlüssel, Batchupdates und gemischte Client-/Datenbank-Auswertung in LINQ-Abfragen).
