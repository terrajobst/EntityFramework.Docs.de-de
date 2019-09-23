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
# <a name="whats-new-in-ef6"></a><span data-ttu-id="3e646-102">Neuigkeiten in EF6</span><span class="sxs-lookup"><span data-stu-id="3e646-102">What's New in EF6</span></span>

<span data-ttu-id="3e646-103">Es wird dringend empfohlen, dass Sie die neueste veröffentlichte Version von Entity Framework verwenden. So wird sichergestellt, dass Sie die neuesten Features nutzen und die höchste Stabilität erhalten.</span><span class="sxs-lookup"><span data-stu-id="3e646-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="3e646-104">Uns ist jedoch klar, dass Sie möglicherweise eine frühere Version verwenden müssen oder die neuen Verbesserungen für die aktuelle Vorabversion ausprobieren möchten.</span><span class="sxs-lookup"><span data-stu-id="3e646-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="3e646-105">Wie Sie bestimmte Versionen von EF installieren, erfahren Sie unter [Get Entity Framework (Beziehen von Entity Framework)](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="3e646-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-630"></a><span data-ttu-id="3e646-106">EF 6.3.0</span><span class="sxs-lookup"><span data-stu-id="3e646-106">EF 6.3.0</span></span>

<span data-ttu-id="3e646-107">Die EF 6.3.0-Runtime wurde im September 2019 für NuGet veröffentlicht.</span><span class="sxs-lookup"><span data-stu-id="3e646-107">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="3e646-108">Das Hauptziel dieses Release bestand darin, die Migration vorhandener Anwendungen, die EF 6 verwenden, zu . NET Core 3.0 zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="3e646-108">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="3e646-109">Die Community hat außerdem verschiedene Fehlerbehebungen und Verbesserungen beigetragen.</span><span class="sxs-lookup"><span data-stu-id="3e646-109">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="3e646-110">Einzelheiten zu den gelösten Problemen finden Sie in den [Meilensteinen](https://github.com/aspnet/EntityFramework6/milestones?state=closed) zu Version 6.3.0.</span><span class="sxs-lookup"><span data-stu-id="3e646-110">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="3e646-111">In Folgenden werden einige der wichtigsten aufgeführt:</span><span class="sxs-lookup"><span data-stu-id="3e646-111">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="3e646-112">Unterstützung für .NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="3e646-112">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="3e646-113">Das EntityFramework-Paket verwendet nun .NET Standard 2.1 zusätzlich zu .NET Framework 4.x als Ziel.</span><span class="sxs-lookup"><span data-stu-id="3e646-113">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x</span></span>
  - <span data-ttu-id="3e646-114">Die Migrationsbefehle wurden so umgeschrieben, dass Sie außerhalb des Prozesses ausgeführt werden und mit Projekten im SDK-Stil funktionieren.</span><span class="sxs-lookup"><span data-stu-id="3e646-114">The migrations commands have been rewritten to execute out of process and work with SDK-style projects</span></span>
- <span data-ttu-id="3e646-115">Unterstützung für HierarchyId von SQL Server</span><span class="sxs-lookup"><span data-stu-id="3e646-115">Support for SQL Server HierarchyId</span></span>
- <span data-ttu-id="3e646-116">Verbesserte Kompatibilität mit PackageReference von Roslyn und NuGet</span><span class="sxs-lookup"><span data-stu-id="3e646-116">Improved compatibility with Roslyn and NuGet PackageReference</span></span>
- <span data-ttu-id="3e646-117">„ef6. exe“ wurde zum Aktivieren, Hinzufügen, Skripten und Anwenden von Migrationen aus Assemblys hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="3e646-117">Added ef6.exe for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="3e646-118">Dies ersetzt „migrate.exe“</span><span class="sxs-lookup"><span data-stu-id="3e646-118">This replaces migrate.exe</span></span>

## <a name="past-releases"></a><span data-ttu-id="3e646-119">Frühere Releases</span><span class="sxs-lookup"><span data-stu-id="3e646-119">Past Releases</span></span>

<span data-ttu-id="3e646-120">Auf der Seite [Frühere Releases](past-releases.md) finden Sie ein Archiv aller früheren Versionen von EF und die wichtigsten Features, die in den einzelnen Releases eingeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="3e646-120">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
