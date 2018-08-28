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
# <a name="compare-ef-core--ef6"></a><span data-ttu-id="d3726-102">Vergleichen von EF Core und EF 6</span><span class="sxs-lookup"><span data-stu-id="d3726-102">Compare EF Core & EF6</span></span>

<span data-ttu-id="d3726-103">Es gibt zwei unterschiedliche Versionen von Entity Framework: Entity Framework Core und Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="d3726-103">There are two different versions of Entity Framework, Entity Framework Core and Entity Framework 6.</span></span>

## <a name="entity-framework-6"></a><span data-ttu-id="d3726-104">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="d3726-104">Entity Framework 6</span></span>

<span data-ttu-id="d3726-105">Entity Framework 6 (EF 6) ist eine bewährte Datenzugriffstechnologie mit Features und Stabilisierungsmaßnahmen, die das Ergebnis jahrelanger Arbeit sind.</span><span class="sxs-lookup"><span data-stu-id="d3726-105">Entity Framework 6 (EF6) is a tried and tested data access technology with many years of features and stabilization.</span></span> <span data-ttu-id="d3726-106">Es wurde erstmals in 2008 im Rahmen von .NET Framework 3.5 SP1 und Visual Studio 2008 SP1 veröffentlicht.</span><span class="sxs-lookup"><span data-stu-id="d3726-106">It first released in 2008, as part of .NET Framework 3.5 SP1 and Visual Studio 2008 SP1.</span></span> <span data-ttu-id="d3726-107">Ab Release EF 4.1 wurde es im Lieferumfang des [NuGet-Pakets „EntityFramework“](https://www.nuget.org/packages/EntityFramework/) ausgeliefert, derzeit eines der am häufigsten verwendeten Pakete auf NuGet.org.</span><span class="sxs-lookup"><span data-stu-id="d3726-107">Starting with the EF4.1 release it has shipped as the [EntityFramework NuGet package](https://www.nuget.org/packages/EntityFramework/) - currently one of the most popular packages on NuGet.org.</span></span>

<span data-ttu-id="d3726-108">EF 6 ist weiterhin ein unterstütztes Produkt, für das einige Zeit lang weiterhin Support durch Fehlerbehebungen und kleinere Verbesserungen bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="d3726-108">EF6 continues to be a supported product, and will continue to see bug fixes and minor improvements for some time to come.</span></span>

## <a name="entity-framework-core"></a><span data-ttu-id="d3726-109">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="d3726-109">Entity Framework Core</span></span>

<span data-ttu-id="d3726-110">Entity Framework Core (EF Core) ist eine einfache, erweiterbare und plattformübergreifende Version von Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d3726-110">Entity Framework Core (EF Core) is a lightweight, extensible, and cross-platform version of Entity Framework.</span></span> <span data-ttu-id="d3726-111">In EF Core werden zahlreiche Verbesserungen und neue Features im Vergleich zu EF 6 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="d3726-111">EF Core introduces many improvements and new features when compared with EF6.</span></span> <span data-ttu-id="d3726-112">EF Core ist gleichzeitig eine neue Codebasis und nicht so ausgereift wie EF 6.</span><span class="sxs-lookup"><span data-stu-id="d3726-112">At the same time, EF Core is a new code base and not as mature as EF6.</span></span>

<span data-ttu-id="d3726-113">In EF Core wird die Entwickleroberfläche von EF 6 wie auch die meisten APIs der obersten Ebene beibehalten, sodass EF Core für Benutzer, die EF 6 verwendet haben, sehr vertraut wirken wird.</span><span class="sxs-lookup"><span data-stu-id="d3726-113">EF Core keeps the developer experience from EF6, and most of the top-level APIs remain the same too, so EF Core will feel very familiar to folks who have used EF6.</span></span> <span data-ttu-id="d3726-114">Gleichzeitig wird EF Core über eine völlig neue Gruppe von Kernkomponenten erstellt.</span><span class="sxs-lookup"><span data-stu-id="d3726-114">At the same time, EF Core is built over a completely new set of core components.</span></span> <span data-ttu-id="d3726-115">Das bedeutet, dass EF Core nicht automatisch alle Features von EF 6 erbt.</span><span class="sxs-lookup"><span data-stu-id="d3726-115">This means EF Core doesn't automatically inherit all the features from EF6.</span></span> <span data-ttu-id="d3726-116">Einige dieser Features werden in zukünftigen Releases vorkommen, während andere seltener verwendete Features nicht in EF Core implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="d3726-116">While some of these features will show up in future releases, other less commonly used features will not be implemented in EF Core.</span></span>

<span data-ttu-id="d3726-117">In der neuen, erweiterbaren und einfachen EF Core-Version konnten außerdem einige Features zu EF Core hinzugefügt werden, die nicht in EF 6 implementiert werden (z.B. Alternativschlüssel, Batchupdates und gemischte Client-/Datenbank-Auswertung in LINQ-Abfragen).</span><span class="sxs-lookup"><span data-stu-id="d3726-117">The new, extensible, and lightweight core has also allowed us to add some features to EF Core that will not be implemented in EF6 (such as alternate keys, batch updates, and mixed client/database evaluation in LINQ queries).</span></span>
