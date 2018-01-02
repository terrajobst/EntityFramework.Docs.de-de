---
title: "Befehlszeilenreferenz – EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 076e9251850ba10df323cd25922aa8b95b3a5491
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
<a name="entity-framework-core-tools"></a><span data-ttu-id="1ce2d-102">Entity Framework Core Tools</span><span class="sxs-lookup"><span data-stu-id="1ce2d-102">Entity Framework Core Tools</span></span>
===========================
<span data-ttu-id="1ce2d-103">Entity Framework Core Tools unterstützen Sie bei der Entwicklung von EF Core-Apps.</span><span class="sxs-lookup"><span data-stu-id="1ce2d-103">The Entity Framework Core Tools help you during the development of EF Core apps.</span></span> <span data-ttu-id="1ce2d-104">Diese werden hauptsächlich verwendet, um ein Gerüst für DbContext und Entitätstypen durch Reverse Engineering eines Datenbankschemas zu erstellen und Migrationen zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="1ce2d-104">They're primarily used to scaffold a DbContext and entity types by reverse engineering the schema of a database, and to manage Migrations.</span></span>

<span data-ttu-id="1ce2d-105">Die [Tools der EF Core-Paket-Manager-Konsole (PMC)][1] gewährleisten ein hervorragendes Benutzererlebnis in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1ce2d-105">The [EF Core Package Manager Console (PMC) Tools][1] provide a superior experience inside Visual Studio.</span></span> <span data-ttu-id="1ce2d-106">Führen Sie sie mithilfe der [Paket-Manager-Konsole][2] von NuGet aus.</span><span class="sxs-lookup"><span data-stu-id="1ce2d-106">Run them using NuGet's [Package Manager Console][2].</span></span> <span data-ttu-id="1ce2d-107">Diese Tools funktionieren sowohl mit .NET Framework- als auch mit .NET Core-Projekten.</span><span class="sxs-lookup"><span data-stu-id="1ce2d-107">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="1ce2d-108">Die [EF Core-.NET-Befehlszeilentools][3] sind eine Erweiterung der [.NET Core-CLI-Tools (Command-Line Interface)][4], die plattformübergreifend verfügbar sind und außerhalb von Visual Studio ausgeführt werden können.</span><span class="sxs-lookup"><span data-stu-id="1ce2d-108">The [EF Core .NET Command-line Tools][3] are an extension to the [.NET Core command-line interface (CLI) tools][4] that are cross-platform and can run outside of Visual Studio.</span></span> <span data-ttu-id="1ce2d-109">Für diese Tools ist ein .NET Core-SDK-Projekt (mit `Sdk="Microsoft.NET.Sdk"` oder einem ähnlichen SDK in der Projektdatei) erforderlich.</span><span class="sxs-lookup"><span data-stu-id="1ce2d-109">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="1ce2d-110">Beide Tools bieten dieselbe Funktionalität.</span><span class="sxs-lookup"><span data-stu-id="1ce2d-110">Both tools expose the same functionality.</span></span> <span data-ttu-id="1ce2d-111">Für die Entwicklung in Visual Studio empfehlen wir die Verwendung von PMC-Tools, da diese ein besseres ganzheitliches Erlebnis bieten.</span><span class="sxs-lookup"><span data-stu-id="1ce2d-111">If you're developing in Visual Studio, we recommend using the PMC Tools since they provide a more integrated experience.</span></span>

<a name="frameworks"></a><span data-ttu-id="1ce2d-112">Frameworks</span><span class="sxs-lookup"><span data-stu-id="1ce2d-112">Frameworks</span></span>
----------
<span data-ttu-id="1ce2d-113">Die Tools unterstützen Projekte, deren Zielversionen für das .NET Framework oder .NET Core festgelegt sind.</span><span class="sxs-lookup"><span data-stu-id="1ce2d-113">The tools support projects targeting .NET Framework or .NET Core.</span></span>

<span data-ttu-id="1ce2d-114">Wenn für Ihr Projekt eine Zielversion für ein anderes Framework (z.B. das universelle Windows- oder Xamarin-Framework) festgelegt ist, empfehlen wir Ihnen, ein separates .NET Standard-Projekt zu erstellen und plattformübergreifend eine Zielversion für eines der unterstützten Frameworks festzulegen.</span><span class="sxs-lookup"><span data-stu-id="1ce2d-114">If your project targets another framework (for example, Universal Windows or Xamarin), we recommend creating a separate .NET Standard project and cross-targeting one of the supported frameworks.</span></span>

<span data-ttu-id="1ce2d-115">Um z.B. plattformübergreifend die Zielversion für .NET Core festzulegen, klicken Sie mit der rechten Maustaste auf das Projekt und wählen **\*.csproj bearbeiten**.</span><span class="sxs-lookup"><span data-stu-id="1ce2d-115">To cross-target .NET Core, for example, right-click on the project and select **Edit \*.csproj**.</span></span> <span data-ttu-id="1ce2d-116">Aktualisieren Sie im Folgenden die Eigenschaft `TargetFramework`.</span><span class="sxs-lookup"><span data-stu-id="1ce2d-116">Update the `TargetFramework` property as follows.</span></span> <span data-ttu-id="1ce2d-117">(Beachten Sie, dass der Eigenschaftenname zum Plural wird.)</span><span class="sxs-lookup"><span data-stu-id="1ce2d-117">(Note, the property name becomes plural.)</span></span>

``` xml
<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>
```

<span data-ttu-id="1ce2d-118">Wenn Sie eine .NET Standard-Klassenbibliothek verwenden und die Zielversionen für das .NET Framework oder .NET Core für Ihr Startprojekt festgelegt sind, müssen Sie nicht plattformübergreifend eine Zielversionen festlegen.</span><span class="sxs-lookup"><span data-stu-id="1ce2d-118">If you're using a .NET Standard class library, you don't need to cross-target if your startup project targets .NET Framework or .NET Core.</span></span>

<a name="startup-and-target-projects"></a><span data-ttu-id="1ce2d-119">Start- und Zielprojekte</span><span class="sxs-lookup"><span data-stu-id="1ce2d-119">Startup and Target Projects</span></span>
---------------------------
<span data-ttu-id="1ce2d-120">Beim Aufrufen von Befehlen sind zwei Projekte beteiligt: das Zielprojekt und das Startprojekt.</span><span class="sxs-lookup"><span data-stu-id="1ce2d-120">Whenever you invoke a command, there are two projects involved: the target project and the startup project.</span></span>

<span data-ttu-id="1ce2d-121">Dem Zielprojekt werden Dateien hinzugefügt (oder sie werden in einigen Fällen aus diesem entfernt).</span><span class="sxs-lookup"><span data-stu-id="1ce2d-121">The target project is where any files are added (or in some cases removed).</span></span>

<span data-ttu-id="1ce2d-122">Das Startprojekt wird bei Ausführung des Projektcodes von den Tools emuliert.</span><span class="sxs-lookup"><span data-stu-id="1ce2d-122">The startup project is the one emulated by the tools when executing your project's code.</span></span>

<span data-ttu-id="1ce2d-123">Sowohl das Zielprojekt als auch das Startprojekt können identisch sein.</span><span class="sxs-lookup"><span data-stu-id="1ce2d-123">Both the target project and the startup project can be the same.</span></span>


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
