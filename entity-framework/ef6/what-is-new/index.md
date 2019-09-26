---
title: Neuerungen – EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: c49f4cba0066d1e218f11c3959d96f9cafa913f4
ms.sourcegitcommit: 7bc43f21e7bdd64926314ea949aae689f1911956
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "71266784"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="b49a3-102">Neuerungen in EF6</span><span class="sxs-lookup"><span data-stu-id="b49a3-102">What's new in EF6</span></span>

<span data-ttu-id="b49a3-103">Es wird dringend empfohlen, dass Sie die neueste veröffentlichte Version von Entity Framework verwenden. So wird sichergestellt, dass Sie die neuesten Features nutzen und die höchste Stabilität erhalten.</span><span class="sxs-lookup"><span data-stu-id="b49a3-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="b49a3-104">Uns ist jedoch klar, dass Sie möglicherweise eine frühere Version verwenden müssen oder die neuen Verbesserungen für die aktuelle Vorabversion ausprobieren möchten.</span><span class="sxs-lookup"><span data-stu-id="b49a3-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="b49a3-105">Wie Sie bestimmte Versionen von EF installieren, erfahren Sie unter [Get Entity Framework (Beziehen von Entity Framework)](~/ef6/fundamentals/install.md).</span><span class="sxs-lookup"><span data-stu-id="b49a3-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-630"></a><span data-ttu-id="b49a3-106">EF 6.3.0</span><span class="sxs-lookup"><span data-stu-id="b49a3-106">EF 6.3.0</span></span>

<span data-ttu-id="b49a3-107">Die EF 6.3.0-Runtime wurde im September 2019 für NuGet veröffentlicht.</span><span class="sxs-lookup"><span data-stu-id="b49a3-107">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="b49a3-108">Das Hauptziel dieses Release bestand darin, die Migration vorhandener Anwendungen, die EF 6 verwenden, zu . NET Core 3.0 zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="b49a3-108">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="b49a3-109">Die Community hat außerdem verschiedene Fehlerbehebungen und Verbesserungen beigetragen.</span><span class="sxs-lookup"><span data-stu-id="b49a3-109">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="b49a3-110">Einzelheiten zu den gelösten Problemen finden Sie in den [Meilensteinen](https://github.com/aspnet/EntityFramework6/milestones?state=closed) zu Version 6.3.0.</span><span class="sxs-lookup"><span data-stu-id="b49a3-110">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="b49a3-111">In Folgenden werden einige der wichtigsten aufgeführt:</span><span class="sxs-lookup"><span data-stu-id="b49a3-111">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="b49a3-112">Unterstützung für .NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="b49a3-112">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="b49a3-113">Das EntityFramework-Paket verwendet nun .NET Standard 2.1 zusätzlich zu .NET Framework 4.x als Ziel.</span><span class="sxs-lookup"><span data-stu-id="b49a3-113">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x.</span></span>
  - <span data-ttu-id="b49a3-114">Dies bedeutet, dass EF 6.3 plattformübergreifend ist und auf anderen Betriebssystemen als Windows unterstützt wird, wie z. B. Linux und macOS.</span><span class="sxs-lookup"><span data-stu-id="b49a3-114">This means that EF 6.3 is cross-platform and supported on other operating systems besides Windows, like Linux and macOS.</span></span>
  - <span data-ttu-id="b49a3-115">Die Migrationsbefehle wurden so umgeschrieben, dass Sie außerhalb des Prozesses ausgeführt werden und mit Projekten im SDK-Stil funktionieren.</span><span class="sxs-lookup"><span data-stu-id="b49a3-115">The migrations commands have been rewritten to execute out of process and work with SDK-style projects.</span></span>
- <span data-ttu-id="b49a3-116">Unterstützung für HierarchyId von SQL Server</span><span class="sxs-lookup"><span data-stu-id="b49a3-116">Support for SQL Server HierarchyId</span></span>
- <span data-ttu-id="b49a3-117">Verbesserte Kompatibilität mit PackageReference von Roslyn und NuGet</span><span class="sxs-lookup"><span data-stu-id="b49a3-117">Improved compatibility with Roslyn and NuGet PackageReference</span></span>
- <span data-ttu-id="b49a3-118">Das `ef6.exe`-Dienstprogramm wurde zum Aktivieren, Hinzufügen, Skripten und Anwenden von Migrationen aus Assemblys hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="b49a3-118">Added `ef6.exe` utility for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="b49a3-119">Hierdurch wird `migrate.exe` ersetzt.</span><span class="sxs-lookup"><span data-stu-id="b49a3-119">This replaces `migrate.exe`</span></span>

### <a name="ef-designer-support"></a><span data-ttu-id="b49a3-120">Unterstützung des EF-Designers</span><span class="sxs-lookup"><span data-stu-id="b49a3-120">EF designer support</span></span>

<span data-ttu-id="b49a3-121">Es gibt derzeit keine Unterstützung für die direkte Verwendung des EF-Designers in .NET Core- oder .NET Standard-Projekten.</span><span class="sxs-lookup"><span data-stu-id="b49a3-121">There's currently no support for using the EF designer directly on .NET Core or .NET Standard projects.</span></span> 

<span data-ttu-id="b49a3-122">Sie können diese Einschränkung umgehen, indem Sie die EDMX-Datei und die generierten Klassen für die Entitäten und DbContext als verknüpfte Dateien zu einem .NET Core 3.0- oder .NET Standard 2.1-Projekt in derselben Projektmappe hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="b49a3-122">You can work around this limitation by adding the EDMX file and the generated classes for the entities and the DbContext as linked files to a .NET Core 3.0 or .NET Standard 2.1 project in the same solution.</span></span>

<span data-ttu-id="b49a3-123">Die verknüpften Dateien sehen in der Projektdatei wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="b49a3-123">The linked files will look like this in the project file:</span></span>

``` csproj 
<ItemGroup>
  <EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" />
  <Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" />
  <Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" />
  <Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" />
</ItemGroup>
```

<span data-ttu-id="b49a3-124">Beachten Sie, dass die EDMX-Datei mit dem EntityDeploy-Buildvorgang verknüpft ist.</span><span class="sxs-lookup"><span data-stu-id="b49a3-124">Note that the EDMX file is linked with the EntityDeploy build action.</span></span> <span data-ttu-id="b49a3-125">Dabei handelt es sich um eine spezielle MSBuild-Aufgabe (die jetzt im EF 6.3-Paket enthalten ist), mit der das EF-Modell in der Zielassembly als eingebettete Ressourcen hinzugefügt wird (oder als Dateien in den Ausgabeordner kopiert wird, abhängig von der Einstellung für die Verarbeitung von Metadatenartefakten in der EDMX-Datei).</span><span class="sxs-lookup"><span data-stu-id="b49a3-125">This is a special MSBuild task (now included in the EF 6.3 package) that takes care of adding the EF model into the target assembly as embedded resources (or copying it as files in the output folder, depending on the Metadata Artifact Processing setting in the EDMX).</span></span> <span data-ttu-id="b49a3-126">Weitere Informationen zur Einrichtung finden Sie im [EDMX-Beispiel zu .NET Core](https://aka.ms/EdmxDotNetCoreSample).</span><span class="sxs-lookup"><span data-stu-id="b49a3-126">For more details on how to get this set up, see our [EDMX .NET Core sample](https://aka.ms/EdmxDotNetCoreSample).</span></span>

## <a name="past-releases"></a><span data-ttu-id="b49a3-127">Frühere Releases</span><span class="sxs-lookup"><span data-stu-id="b49a3-127">Past Releases</span></span>

<span data-ttu-id="b49a3-128">Auf der Seite [Frühere Releases](past-releases.md) finden Sie ein Archiv aller früheren Versionen von EF und die wichtigsten Features, die in den einzelnen Releases eingeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="b49a3-128">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
