---
title: 'Neuigkeiten: EF6'
author: divega
ms.date: 10/23/2016
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
ms.openlocfilehash: fcd6339f67a1512dd66220c59537d12cf0b22620
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490297"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="3882a-102">Neuigkeiten in EF6</span><span class="sxs-lookup"><span data-stu-id="3882a-102">What's New in EF6</span></span>

<span data-ttu-id="3882a-103">Es wird dringend empfohlen, dass Sie die neueste veröffentlichte Version von Entity Framework verwenden. So wird sichergestellt, dass Sie die neuesten Features nutzen und die höchste Stabilität erhalten.</span><span class="sxs-lookup"><span data-stu-id="3882a-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="3882a-104">Uns ist jedoch klar, dass Sie möglicherweise eine frühere Version verwenden müssen oder die neuen Verbesserungen für die aktuelle Vorabversion ausprobieren möchten.</span><span class="sxs-lookup"><span data-stu-id="3882a-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="3882a-105">Wie Sie bestimmte Versionen von EF installieren, erfahren Sie unter [Get Entity Framework (Beziehen von Entity Framework)](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="3882a-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

<span data-ttu-id="3882a-106">Auf dieser Seite werden die Funktionen beschrieben, die jeweils in neuen Releases enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="3882a-106">This page documents the features that are included on each new release.</span></span>

## <a name="recent-releases"></a><span data-ttu-id="3882a-107">Aktuelle Releases</span><span class="sxs-lookup"><span data-stu-id="3882a-107">Recent releases</span></span>

### <a name="ef-tools-update-in-visual-studio-2017-157"></a><span data-ttu-id="3882a-108">Update für die EF-Tools in Visual Studio 2017 15.7</span><span class="sxs-lookup"><span data-stu-id="3882a-108">EF Tools Update in Visual Studio 2017 15.7</span></span>

<span data-ttu-id="3882a-109">Im Mai 2018 haben wir eine aktualisierte Version der EF-Tools als Teil von Visual Studio 2017 15.7 veröffentlicht.</span><span class="sxs-lookup"><span data-stu-id="3882a-109">In May 2018, we released an updated version of the EF Tools as part of Visual Studio 2017 15.7.</span></span>
<span data-ttu-id="3882a-110">Sie enthält Verbesserungen für einige übliche Problempunkte:</span><span class="sxs-lookup"><span data-stu-id="3882a-110">It includes improvements for some common pain points:</span></span>

- <span data-ttu-id="3882a-111">Korrekturen für einige Fehler beim Benutzeroberflächenzugriff</span><span class="sxs-lookup"><span data-stu-id="3882a-111">Fixes for several user interface accessibility bugs</span></span>
- <span data-ttu-id="3882a-112">Problemumgehung für die SQL Server-Leistungsregression beim Generieren von Modellen aus vorhandenen Datenbanken [#4](https://github.com/aspnet/entityframework6/issues/4)</span><span class="sxs-lookup"><span data-stu-id="3882a-112">Workaround for SQL Server performance regression when generating models from existing databases [#4](https://github.com/aspnet/entityframework6/issues/4)</span></span>
- <span data-ttu-id="3882a-113">Unterstützung zum Aktualisieren von Modellen für größere Modelle in SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)</span><span class="sxs-lookup"><span data-stu-id="3882a-113">Support for updating models for larger models on SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185)</span></span>

<span data-ttu-id="3882a-114">Eine weitere Verbesserung bei dieser neuen Version der EF-Tools ist, dass die EF 6.2-Runtime installiert wird, wenn ein Modell in einem neuen Projekt erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="3882a-114">Another improvement in this this new version of EF Tools is that it installs the EF 6.2 runtime when creating a model in a new project.</span></span> <span data-ttu-id="3882a-115">Bei älteren Versionen von Visual Studio ist es möglich, die EF 6.2-Runtime (sowie alle früheren Version von Entity Framework) zu verwenden, indem Sie die entsprechende Version des NuGet-Pakets installieren.</span><span class="sxs-lookup"><span data-stu-id="3882a-115">With older versions of Visual Studio, it is possible to use the EF 6.2 runtime (as well as any past version of EF) by installing the corresponding version of the NuGet package.</span></span>

### <a name="ef-62-runtime"></a><span data-ttu-id="3882a-116">EF 6.2-Runtime</span><span class="sxs-lookup"><span data-stu-id="3882a-116">EF 6.2 Runtime</span></span>

<span data-ttu-id="3882a-117">Die EF 6.2-Runtime wurde im Januar 2017 für NuGet veröffentlicht.</span><span class="sxs-lookup"><span data-stu-id="3882a-117">The EF 6.2 runtime was released to NuGet in October of 2017.</span></span>
<span data-ttu-id="3882a-118">Es ist vor allem der Verdienst unserer Community von Open-Source-Mitwirkenden, dass EF 6.2 zahlreiche [Fehlerbehebungen](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) und [Produktverbesserungen](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20) enthält.</span><span class="sxs-lookup"><span data-stu-id="3882a-118">Thanks in great part to the efforts our community of open source contributors, EF 6.2 includes numerous [bugs fixes](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) and [product enhancements](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).</span></span>

<span data-ttu-id="3882a-119">Hier ist eine kurze Liste der wichtigsten Änderungen, die die EF 6.2-Runtime betreffen:</span><span class="sxs-lookup"><span data-stu-id="3882a-119">Here is a brief list of the most important changes affecting the EF 6.2 runtime:</span></span>

- <span data-ttu-id="3882a-120">Startzeit reduzieren, indem abgeschlossene Code First-Modelle aus einem beständigen Cache geladen werden [#275](https://github.com/aspnet/EntityFramework6/issues/275)</span><span class="sxs-lookup"><span data-stu-id="3882a-120">Reduce start up time by loading finished code first models from a persistent cache [#275](https://github.com/aspnet/EntityFramework6/issues/275)</span></span>
- <span data-ttu-id="3882a-121">Fluent-API zum Definieren von Indizes [#274](https://github.com/aspnet/EntityFramework6/issues/274)</span><span class="sxs-lookup"><span data-stu-id="3882a-121">Fluent API to define indexes [#274](https://github.com/aspnet/EntityFramework6/issues/274)</span></span>
- <span data-ttu-id="3882a-122">DbFunctions.Like() hinzufügen, um das Schreiben von LINQ-Abfragen zu aktivieren, die in SQL als LIKE übersetzt werden [#241](https://github.com/aspnet/EntityFramework6/issues/241)</span><span class="sxs-lookup"><span data-stu-id="3882a-122">DbFunctions.Like() to enable writing LINQ queries that translate to LIKE in SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241)</span></span>
- <span data-ttu-id="3882a-123">Migrate.exe unterstützt jetzt die Option „-script“ [#240](https://github.com/aspnet/EntityFramework6/issues/240)</span><span class="sxs-lookup"><span data-stu-id="3882a-123">Migrate.exe now supports -script option [#240](https://github.com/aspnet/EntityFramework6/issues/240)</span></span>
- <span data-ttu-id="3882a-124">EF6 funktioniert jetzt mit Schlüsselwerten, die von einer Sequenz in SQL Server generiert wurden [#165](https://github.com/aspnet/EntityFramework6/issues/165)</span><span class="sxs-lookup"><span data-stu-id="3882a-124">EF6 can now work with key values generated by a sequence in SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165)</span></span>
- <span data-ttu-id="3882a-125">Updateliste der vorübergehenden Fehler für die SQL Azure-Ausführungsstrategie [#83](https://github.com/aspnet/EntityFramework6/issues/83)</span><span class="sxs-lookup"><span data-stu-id="3882a-125">Update list of transient errors for SQL Azure Execution Strategy [#83](https://github.com/aspnet/EntityFramework6/issues/83)</span></span>
- <span data-ttu-id="3882a-126">Fehler: Wiederholen von Abfragen oder SQL-Befehlen schlägt mit folgendem Hinweis fehl: „The SqlParameter is already contained by another SqlParameterCollection“ (Der SqlParameter ist bereits in einer anderen SqlParameterCollection enthalten) [#81](https://github.com/aspnet/EntityFramework6/issues/81)</span><span class="sxs-lookup"><span data-stu-id="3882a-126">Bug: Retrying queries or SQL commands fails with "The SqlParameter is already contained by another SqlParameterCollection" [#81](https://github.com/aspnet/EntityFramework6/issues/81)</span></span>
- <span data-ttu-id="3882a-127">Fehler: Bei der Auswertung von DbQuery.ToString() tritt häufig ein Timeout im Debugger auf [#73](https://github.com/aspnet/EntityFramework6/issues/73)</span><span class="sxs-lookup"><span data-stu-id="3882a-127">Bug: Evaluation of DbQuery.ToString() frequently times out in the debugger [#73](https://github.com/aspnet/EntityFramework6/issues/73)</span></span>

## <a name="future-releases"></a><span data-ttu-id="3882a-128">Zukünftige Releases</span><span class="sxs-lookup"><span data-stu-id="3882a-128">Future Releases</span></span>

<span data-ttu-id="3882a-129">Informationen zu zukünftigen Versionen von EF6, finden Sie in unserer [Roadmap](roadmap.md).</span><span class="sxs-lookup"><span data-stu-id="3882a-129">For information on future version of EF6, please look at our [Roadmap](roadmap.md).</span></span>

## <a name="past-releases"></a><span data-ttu-id="3882a-130">Frühere Releases</span><span class="sxs-lookup"><span data-stu-id="3882a-130">Past Releases</span></span>

<span data-ttu-id="3882a-131">Auf der Seite [Frühere Releases](past-releases.md) finden Sie ein Archiv aller früheren Versionen von EF und die wichtigsten Features, die in den einzelnen Releases eingeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="3882a-131">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
