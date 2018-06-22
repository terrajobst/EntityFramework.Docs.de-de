---
title: Befehlszeilenreferenz – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: db25ed55e3724ee71743e563f39a6e4b16c17589
ms.sourcegitcommit: fc68321c211aca38f7b9dc3a75677c6ca1b2524b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2018
ms.locfileid: "29769409"
---
<a name="entity-framework-core-tools"></a><span data-ttu-id="1be04-102">Entity Framework Core Tools</span><span class="sxs-lookup"><span data-stu-id="1be04-102">Entity Framework Core Tools</span></span>
===========================
<span data-ttu-id="1be04-103">Entity Framework Core Tools unterstützen Sie bei der Entwicklung von EF Core-Apps.</span><span class="sxs-lookup"><span data-stu-id="1be04-103">The Entity Framework Core Tools help you during the development of EF Core apps.</span></span> <span data-ttu-id="1be04-104">Diese werden hauptsächlich verwendet, um ein Gerüst für DbContext und Entitätstypen durch Reverse Engineering eines Datenbankschemas zu erstellen und Migrationen zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="1be04-104">They're primarily used to scaffold a DbContext and entity types by reverse engineering the schema of a database, and to manage Migrations.</span></span>

<span data-ttu-id="1be04-105">Die [Tools der EF Core-Paket-Manager-Konsole (PMC)][1] gewährleisten ein hervorragendes Benutzererlebnis in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1be04-105">The [EF Core Package Manager Console (PMC) Tools][1] provide a superior experience inside Visual Studio.</span></span> <span data-ttu-id="1be04-106">Führen Sie sie mithilfe der [Paket-Manager-Konsole][2] von NuGet aus.</span><span class="sxs-lookup"><span data-stu-id="1be04-106">Run them using NuGet's [Package Manager Console][2].</span></span> <span data-ttu-id="1be04-107">Diese Tools funktionieren sowohl mit .NET Framework- als auch mit .NET Core-Projekten.</span><span class="sxs-lookup"><span data-stu-id="1be04-107">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="1be04-108">Die [EF Core-.NET-Befehlszeilentools][3] sind eine Erweiterung der [.NET Core-CLI-Tools (Command-Line Interface)][4], die plattformübergreifend verfügbar sind und außerhalb von Visual Studio ausgeführt werden können.</span><span class="sxs-lookup"><span data-stu-id="1be04-108">The [EF Core .NET Command-line Tools][3] are an extension to the [.NET Core command-line interface (CLI) tools][4] that are cross-platform and can run outside of Visual Studio.</span></span> <span data-ttu-id="1be04-109">Für diese Tools ist ein .NET Core-SDK-Projekt (mit `Sdk="Microsoft.NET.Sdk"` oder einem ähnlichen SDK in der Projektdatei) erforderlich.</span><span class="sxs-lookup"><span data-stu-id="1be04-109">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="1be04-110">Beide Tools bieten dieselbe Funktionalität.</span><span class="sxs-lookup"><span data-stu-id="1be04-110">Both tools expose the same functionality.</span></span> <span data-ttu-id="1be04-111">Für die Entwicklung in Visual Studio empfehlen wir die Verwendung von PMC-Tools, da diese ein besseres ganzheitliches Erlebnis bieten.</span><span class="sxs-lookup"><span data-stu-id="1be04-111">If you're developing in Visual Studio, we recommend using the PMC Tools since they provide a more integrated experience.</span></span>

<a name="frameworks"></a><span data-ttu-id="1be04-112">Frameworks</span><span class="sxs-lookup"><span data-stu-id="1be04-112">Frameworks</span></span>
----------
<span data-ttu-id="1be04-113">Die Tools unterstützen Projekte, deren Zielversionen für das .NET Framework oder .NET Core festgelegt sind.</span><span class="sxs-lookup"><span data-stu-id="1be04-113">The tools support projects targeting .NET Framework or .NET Core.</span></span>

<span data-ttu-id="1be04-114">Wenn Sie eine Klassenbibliothek verwenden möchten, sollten Sie gegebenenfalls die Verwendung der Klassenbibliothek von .NET Core oder .NET Framework in Erwägung ziehen.</span><span class="sxs-lookup"><span data-stu-id="1be04-114">If you want to use a class library, then consider using a .NET Core or .NET Framework class library if possible.</span></span> <span data-ttu-id="1be04-115">Dies führt zu den geringsten Problemen mit .NET-Tools.</span><span class="sxs-lookup"><span data-stu-id="1be04-115">This will result in the least issues with .NET tooling.</span></span> <span data-ttu-id="1be04-116">Wenn Sie stattdessen eine .NET Standard-Klassenbibliothek verwenden möchten, müssen Sie ein Startprojekt verwenden, das auf .NET Framework oder .NET Core ausgerichtet ist, sodass das Tool eine Zielplattform hat, in die es Ihre Klassenbibliothek laden kann.</span><span class="sxs-lookup"><span data-stu-id="1be04-116">If instead you wish to use a .NET Standard class library, then you will need to use a startup project that targets .NET Framework or .NET Core so that the tooling has a conrete target platform into which it can load your class library.</span></span> <span data-ttu-id="1be04-117">Dieses Startprojekt kann ein Dummyprojekt ohne realen Code sein. Es wird nur zur Bereitstellung eines Ziels für Tools benötigt.</span><span class="sxs-lookup"><span data-stu-id="1be04-117">This startup project can be a dummy project with no real code--it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="1be04-118">Wenn Ihr Projekt ein anderes Framework, wie z.B. Universal Windows oder Xamarin, verwendet, müssen Sie eine separate .NET Standard-Klassenbibliothek erstellen.</span><span class="sxs-lookup"><span data-stu-id="1be04-118">If your project targets another framework (for example, Universal Windows or Xamarin), then you will need to create a separate .NET Standard class library.</span></span> <span data-ttu-id="1be04-119">In diesem Fall folgen Sie dem oben angeführten Leitfaden zum Erstellen eines Startprojekts, das von Tools verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="1be04-119">In this case, follow the guidance above to also create a startup project that can be used by the tooling.</span></span>

<a name="startup-and-target-projects"></a><span data-ttu-id="1be04-120">Start- und Zielprojekte</span><span class="sxs-lookup"><span data-stu-id="1be04-120">Startup and Target Projects</span></span>
---------------------------
<span data-ttu-id="1be04-121">Beim Aufrufen von Befehlen sind zwei Projekte beteiligt: das Zielprojekt und das Startprojekt.</span><span class="sxs-lookup"><span data-stu-id="1be04-121">Whenever you invoke a command, there are two projects involved: the target project and the startup project.</span></span>

<span data-ttu-id="1be04-122">Dem Zielprojekt werden Dateien hinzugefügt (oder sie werden in einigen Fällen aus diesem entfernt).</span><span class="sxs-lookup"><span data-stu-id="1be04-122">The target project is where any files are added (or in some cases removed).</span></span>

<span data-ttu-id="1be04-123">Das Startprojekt wird bei Ausführung des Projektcodes von den Tools emuliert.</span><span class="sxs-lookup"><span data-stu-id="1be04-123">The startup project is the one emulated by the tools when executing your project's code.</span></span>

<span data-ttu-id="1be04-124">Sowohl das Zielprojekt als auch das Startprojekt können identisch sein.</span><span class="sxs-lookup"><span data-stu-id="1be04-124">Both the target project and the startup project can be the same.</span></span>


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
