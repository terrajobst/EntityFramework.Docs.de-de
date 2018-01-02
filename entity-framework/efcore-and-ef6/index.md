---
title: Vergleichen von EF Core und EF 6
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 4442e6931327f6a07d98aee47211ddbeed51a467
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="compare-ef-core--ef6"></a>Vergleichen von EF Core und EF 6

Es gibt zwei Versionen von Entity Framework: Entity Framework Core und Entity Framework 6.

## <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework 6 (EF 6) ist eine bewährte Datenzugriffstechnologie mit Features und Stabilisierungsmaßnahmen, die das Ergebnis jahrelanger Arbeit sind. Es wurde erstmals in 2008 im Rahmen von .NET Framework 3.5 SP1 und Visual Studio 2008 SP1 veröffentlicht. Ab Release EF 4.1 wurde es im Lieferumfang des [NuGet-Pakets „EntityFramework“](https://www.nuget.org/packages/EntityFramework/) bereitgestellt, das derzeit das am häufigsten verwendete Paket auf NuGet.org darstellt.

EF 6 ist weiterhin ein unterstütztes Produkt, für das einige Zeit lang weiterhin Support durch Fehlerbehebungen und kleinere Verbesserungen bereitgestellt wird.

## <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework Core (EF Core) ist eine einfache, erweiterbare und plattformübergreifende Version von Entity Framework. In EF Core werden zahlreiche Verbesserungen und neue Features im Vergleich zu EF 6 eingeführt. EF Core ist gleichzeitig eine neue Codebasis und nicht so ausgereift wie EF 6.

In EF Core wird die Entwickleroberfläche von EF 6 wie auch die meisten APIs der obersten Ebene beibehalten, sodass EF Core für Benutzer, die EF 6 verwendet haben, sehr vertraut wirken wird. Gleichzeitig wird EF Core über eine völlig neue Gruppe von Kernkomponenten erstellt. Das bedeutet, dass EF Core nicht automatisch alle Features von EF 6 erbt. Einige dieser Features (z.B. Lazy Loading und Verbindungsresilienz) werden in zukünftigen Releases vorkommen, während andere seltener verwendete Features nicht in EF Core implementiert werden.

In der neuen, erweiterbaren und einfachen EF Core-Version konnten außerdem einige Features zu EF Core hinzugefügt werden, die nicht in EF 6 implementiert werden (z.B. Alternativschlüssel und gemischte Client-/Datenbank-Auswertung in LINQ-Abfragen).
