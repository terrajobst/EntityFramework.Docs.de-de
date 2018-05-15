---
title: .NET Core CLI – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: d053d53bd50d2e7d16223c5b4e4009c9bb2298bb
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2018
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="69d1b-102">EF Core .NET-Befehlszeilentools</span><span class="sxs-lookup"><span data-stu-id="69d1b-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="69d1b-103">Das Entity Framework Core .NET Befehlszeilentools sind eine Erweiterung für die plattformübergreifende **Dotnet** Befehl, die Teil von der [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="69d1b-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="69d1b-104">Wenn Sie Visual Studio verwenden, empfiehlt es sich [PMC Tools] [ 1] seitdem sie stattdessen eine weitere integrierte Lösung bieten.</span><span class="sxs-lookup"><span data-stu-id="69d1b-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="69d1b-105">Installieren der Tools</span><span class="sxs-lookup"><span data-stu-id="69d1b-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="69d1b-106">.NET Core SDK, Version 2.1.300 und enthält neuere **Dotnet Ef** Befehle, die mit EF Core 2.0 und höheren Versionen kompatibel sind.</span><span class="sxs-lookup"><span data-stu-id="69d1b-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="69d1b-107">Daher wird bei Verwendung von neueren Versionen von .NET Core SDK und die EF-Core-Laufzeit ist keine Installation erforderlich und können Sie die restlichen Teil dieses Abschnitts ignorieren.</span><span class="sxs-lookup"><span data-stu-id="69d1b-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="69d1b-108">Auf der anderen Seite der **Dotnet Ef** Tool in .NET Core SDK-Version 2.1.300 enthalten und höher ist nicht kompatibel mit EF-Core-Version 1.0 und 1.1.</span><span class="sxs-lookup"><span data-stu-id="69d1b-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="69d1b-109">Bevor Sie kann für ein Projekt, das dieser früheren Versionen von EF Core auf einem Computer verwendet, die .NET Core SDK 2.1.300 verfügt oder höher installiert ist, müssen Sie auch Version 2.1.200 installieren oder eine ältere des SDK und konfigurieren Sie die Anwendung zur Verwendung dieser älteren Version durch Ändern der  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) Datei.</span><span class="sxs-lookup"><span data-stu-id="69d1b-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="69d1b-110">Diese Datei ist normalerweise in dem Projektmappenverzeichnis (eine oberhalb des Projekts) enthalten.</span><span class="sxs-lookup"><span data-stu-id="69d1b-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="69d1b-111">Anschließend können Sie mit der Installlation-Anweisung unten fortfahren.</span><span class="sxs-lookup"><span data-stu-id="69d1b-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="69d1b-112">Für frühere Versionen von .NET Core SDK können Sie die EF Core .NET-Befehlszeilentools mithilfe der folgenden Schritte installieren:</span><span class="sxs-lookup"><span data-stu-id="69d1b-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="69d1b-113">Bearbeiten der Projektdatei, und fügen Sie Microsoft.EntityFrameworkCore.Tools.DotNet als DotNetCliToolReference-Element (siehe unten)</span><span class="sxs-lookup"><span data-stu-id="69d1b-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="69d1b-114">Führen Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="69d1b-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="69d1b-115">Das daraus resultierende Projekt sollte etwa wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="69d1b-115">The resulting project should look something like this:</span></span>

``` xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.EntityFrameworkCore.Design"
                      Version="2.0.0"
                      PrivateAssets="All" />
  </ItemGroup>
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
                            Version="2.0.0" />
  </ItemGroup>
</Project>
```

> [!NOTE]
> <span data-ttu-id="69d1b-116">Ein Verweis Paket mit `PrivateAssets="All"` bedeutet, er ist nicht verfügbar gemacht, um Projekte, die auf dieses Projekt verweisen, insbesondere für die Pakete, die in der Regel nur während der Entwicklung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="69d1b-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="69d1b-117">Wenn Sie alles richtig haben, sollten Sie den folgenden Befehl an einer Eingabeaufforderung erfolgreich ausführen können.</span><span class="sxs-lookup"><span data-stu-id="69d1b-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="69d1b-118">Mithilfe der tools</span><span class="sxs-lookup"><span data-stu-id="69d1b-118">Using the tools</span></span>
---------------
<span data-ttu-id="69d1b-119">Bei jedem eines Befehls aufrufen umfasst zwei Projekte:</span><span class="sxs-lookup"><span data-stu-id="69d1b-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="69d1b-120">Dem Zielprojekt werden Dateien hinzugefügt (oder sie werden in einigen Fällen aus diesem entfernt).</span><span class="sxs-lookup"><span data-stu-id="69d1b-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="69d1b-121">Das Zielprojekt wird standardmäßig auf das Projekt im aktuellen Verzeichnis, jedoch können geändert werden, mithilfe der <nobr> **– Projekt** </nobr> Option.</span><span class="sxs-lookup"><span data-stu-id="69d1b-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="69d1b-122">Das Startprojekt wird bei Ausführung des Projektcodes von den Tools emuliert.</span><span class="sxs-lookup"><span data-stu-id="69d1b-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="69d1b-123">Auch wird standardmäßig auf das Projekt im aktuellen Verzeichnis, aber kann geändert werden, mithilfe der **--Startprojekt** Option.</span><span class="sxs-lookup"><span data-stu-id="69d1b-123">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="69d1b-124">Aktualisieren der Datenbank Ihrer Web-Anwendung, die EF-Core, die in einem anderen Projekt installiert hat z. B. wie folgt aussehen würde: `dotnet ef database update --project {project-path}` (aus Ihrem Verzeichnis für den Web-app)</span><span class="sxs-lookup"><span data-stu-id="69d1b-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="69d1b-125">Allgemeine Optionen:</span><span class="sxs-lookup"><span data-stu-id="69d1b-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="69d1b-126">--json</span><span class="sxs-lookup"><span data-stu-id="69d1b-126">--json</span></span>                           | <span data-ttu-id="69d1b-127">JSON-Ausgabe anzeigen.</span><span class="sxs-lookup"><span data-stu-id="69d1b-127">Show JSON output.</span></span>           |
| <span data-ttu-id="69d1b-128">-c</span><span class="sxs-lookup"><span data-stu-id="69d1b-128">-c</span></span> | <span data-ttu-id="69d1b-129">--Kontext \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="69d1b-129">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="69d1b-130">Die DbContext verwenden.</span><span class="sxs-lookup"><span data-stu-id="69d1b-130">The DbContext to use.</span></span>       |
| <span data-ttu-id="69d1b-131">-p</span><span class="sxs-lookup"><span data-stu-id="69d1b-131">-p</span></span> | <span data-ttu-id="69d1b-132">--Projekt \<Projekt ></span><span class="sxs-lookup"><span data-stu-id="69d1b-132">--project \<PROJECT></span></span>             | <span data-ttu-id="69d1b-133">Das Projekt verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="69d1b-133">The project to use.</span></span>         |
| <span data-ttu-id="69d1b-134">-s</span><span class="sxs-lookup"><span data-stu-id="69d1b-134">-s</span></span> | <span data-ttu-id="69d1b-135">--Startprojekt \<Projekt ></span><span class="sxs-lookup"><span data-stu-id="69d1b-135">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="69d1b-136">Das Startup-Projekt verwenden.</span><span class="sxs-lookup"><span data-stu-id="69d1b-136">The startup project to use.</span></span> |
|    | <span data-ttu-id="69d1b-137">--Framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="69d1b-137">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="69d1b-138">Das Zielframework.</span><span class="sxs-lookup"><span data-stu-id="69d1b-138">The target framework.</span></span>       |
|    | <span data-ttu-id="69d1b-139">--Konfiguration \<CONFIGURATION ></span><span class="sxs-lookup"><span data-stu-id="69d1b-139">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="69d1b-140">Die zu verwendende Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="69d1b-140">The configuration to use.</span></span>   |
|    | <span data-ttu-id="69d1b-141">--Runtime \<BEZEICHNER ></span><span class="sxs-lookup"><span data-stu-id="69d1b-141">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="69d1b-142">Die Laufzeit verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="69d1b-142">The runtime to use.</span></span>         |
| <span data-ttu-id="69d1b-143">-h</span><span class="sxs-lookup"><span data-stu-id="69d1b-143">-h</span></span> | <span data-ttu-id="69d1b-144">– Hilfe</span><span class="sxs-lookup"><span data-stu-id="69d1b-144">--help</span></span>                           | <span data-ttu-id="69d1b-145">Anzeigen von Hilfeinformationen.</span><span class="sxs-lookup"><span data-stu-id="69d1b-145">Show help information.</span></span>      |
| <span data-ttu-id="69d1b-146">-v</span><span class="sxs-lookup"><span data-stu-id="69d1b-146">-v</span></span> | <span data-ttu-id="69d1b-147">-verbose</span><span class="sxs-lookup"><span data-stu-id="69d1b-147">--verbose</span></span>                        | <span data-ttu-id="69d1b-148">Zeigen Sie eine ausführlichen Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="69d1b-148">Show verbose output.</span></span>        |
|    | <span data-ttu-id="69d1b-149">--keine Farbe</span><span class="sxs-lookup"><span data-stu-id="69d1b-149">--no-color</span></span>                       | <span data-ttu-id="69d1b-150">Nicht zur farblichen Kennzeichnung der Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="69d1b-150">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="69d1b-151">--Präfix-Ausgabe</span><span class="sxs-lookup"><span data-stu-id="69d1b-151">--prefix-output</span></span>                  | <span data-ttu-id="69d1b-152">Präfix Ausgabe auf.</span><span class="sxs-lookup"><span data-stu-id="69d1b-152">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="69d1b-153">Legen Sie zum Angeben der ASP.NET Core-Umgebung die **ASPNETCORE_ENVIRONMENT** Umgebungsvariablen vor der Ausführung.</span><span class="sxs-lookup"><span data-stu-id="69d1b-153">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="69d1b-154">Befehle</span><span class="sxs-lookup"><span data-stu-id="69d1b-154">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="69d1b-155">Dotnet Ef das Löschen einer Datenbank</span><span class="sxs-lookup"><span data-stu-id="69d1b-155">dotnet ef database drop</span></span>

<span data-ttu-id="69d1b-156">Löscht die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="69d1b-156">Drops the database.</span></span>

<span data-ttu-id="69d1b-157">Optionen:</span><span class="sxs-lookup"><span data-stu-id="69d1b-157">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="69d1b-158">-f</span><span class="sxs-lookup"><span data-stu-id="69d1b-158">-f</span></span> | <span data-ttu-id="69d1b-159">--erzwingen</span><span class="sxs-lookup"><span data-stu-id="69d1b-159">--force</span></span>   | <span data-ttu-id="69d1b-160">Bestätigen Sie nicht.</span><span class="sxs-lookup"><span data-stu-id="69d1b-160">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="69d1b-161">--Probelauf</span><span class="sxs-lookup"><span data-stu-id="69d1b-161">--dry-run</span></span> | <span data-ttu-id="69d1b-162">Anzeigen, welche Datenbank verworfen werden, jedoch nicht, legen Sie sie.</span><span class="sxs-lookup"><span data-stu-id="69d1b-162">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="69d1b-163">Dotnet Ef-Datenbankupdate</span><span class="sxs-lookup"><span data-stu-id="69d1b-163">dotnet ef database update</span></span>

<span data-ttu-id="69d1b-164">Aktualisiert die Datenbank in eine angegebene Migration.</span><span class="sxs-lookup"><span data-stu-id="69d1b-164">Updates the database to a specified migration.</span></span>

<span data-ttu-id="69d1b-165">Argumente:</span><span class="sxs-lookup"><span data-stu-id="69d1b-165">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="69d1b-166">\<MIGRATION &GT;</span><span class="sxs-lookup"><span data-stu-id="69d1b-166">\<MIGRATION></span></span> | <span data-ttu-id="69d1b-167">Der zielmigration.</span><span class="sxs-lookup"><span data-stu-id="69d1b-167">The target migration.</span></span> <span data-ttu-id="69d1b-168">Bei 0 werden bei allen Migrationen rückgängig gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="69d1b-168">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="69d1b-169">Standardmäßig bis zum letzten Migration.</span><span class="sxs-lookup"><span data-stu-id="69d1b-169">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="69d1b-170">Dotnet Ef Dbcontext info</span><span class="sxs-lookup"><span data-stu-id="69d1b-170">dotnet ef dbcontext info</span></span>

<span data-ttu-id="69d1b-171">Ruft Informationen zu einem DbContext-Typ ab.</span><span class="sxs-lookup"><span data-stu-id="69d1b-171">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="69d1b-172">Dotnet Ef Dbcontext-Liste</span><span class="sxs-lookup"><span data-stu-id="69d1b-172">dotnet ef dbcontext list</span></span>

<span data-ttu-id="69d1b-173">Listet die verfügbare DbContext-Typen.</span><span class="sxs-lookup"><span data-stu-id="69d1b-173">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="69d1b-174">Dotnet Ef Dbcontext Gerüst</span><span class="sxs-lookup"><span data-stu-id="69d1b-174">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="69d1b-175">Gerüste ein ' DbContext ' und Entität Typen für eine Datenbank.</span><span class="sxs-lookup"><span data-stu-id="69d1b-175">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="69d1b-176">Argumente:</span><span class="sxs-lookup"><span data-stu-id="69d1b-176">Arguments:</span></span>

|               |                                                                     |
|:--------------|:--------------------------------------------------------------------|
| <span data-ttu-id="69d1b-177">\<VERBINDUNG &GT;</span><span class="sxs-lookup"><span data-stu-id="69d1b-177">\<CONNECTION></span></span> | <span data-ttu-id="69d1b-178">Die Verbindungszeichenfolge zur Datenbank.</span><span class="sxs-lookup"><span data-stu-id="69d1b-178">The connection string to the database.</span></span>                              |
| <span data-ttu-id="69d1b-179">\<ANBIETER &GT;</span><span class="sxs-lookup"><span data-stu-id="69d1b-179">\<PROVIDER></span></span>   | <span data-ttu-id="69d1b-180">Die zu verwendenden Anbieter an.</span><span class="sxs-lookup"><span data-stu-id="69d1b-180">The provider to use.</span></span> <span data-ttu-id="69d1b-181">(z. B.</span><span class="sxs-lookup"><span data-stu-id="69d1b-181">(E.g.</span></span> <span data-ttu-id="69d1b-182">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="69d1b-182">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="69d1b-183">Optionen:</span><span class="sxs-lookup"><span data-stu-id="69d1b-183">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="69d1b-184"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="69d1b-184"><nobr>-d</nobr></span></span> | <span data-ttu-id="69d1b-185">--datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="69d1b-185">--data-annotations</span></span>                      | <span data-ttu-id="69d1b-186">Verwenden Sie Attribute, um das Modell (sofern möglich) konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="69d1b-186">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="69d1b-187">Wenn nicht angegeben, wird nur die fluent-API verwendet.</span><span class="sxs-lookup"><span data-stu-id="69d1b-187">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="69d1b-188">-c</span><span class="sxs-lookup"><span data-stu-id="69d1b-188">-c</span></span>              | <span data-ttu-id="69d1b-189">--Kontext \<NAME ></span><span class="sxs-lookup"><span data-stu-id="69d1b-189">--context \<NAME></span></span>                       | <span data-ttu-id="69d1b-190">Der Name von ' DbContext '.</span><span class="sxs-lookup"><span data-stu-id="69d1b-190">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="69d1b-191">--Kontext Dir \<Pfad ></span><span class="sxs-lookup"><span data-stu-id="69d1b-191">--context-dir \<PATH></span></span>                   | <span data-ttu-id="69d1b-192">Das Verzeichnis DbContext-Datei in den versetzt.</span><span class="sxs-lookup"><span data-stu-id="69d1b-192">The directory to put DbContext file in.</span></span> <span data-ttu-id="69d1b-193">Pfade sind relativ zum Projektverzeichnis an.</span><span class="sxs-lookup"><span data-stu-id="69d1b-193">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="69d1b-194">-f</span><span class="sxs-lookup"><span data-stu-id="69d1b-194">-f</span></span>              | <span data-ttu-id="69d1b-195">--erzwingen</span><span class="sxs-lookup"><span data-stu-id="69d1b-195">--force</span></span>                                 | <span data-ttu-id="69d1b-196">Überschreiben Sie vorhandene Dateien.</span><span class="sxs-lookup"><span data-stu-id="69d1b-196">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="69d1b-197">-o</span><span class="sxs-lookup"><span data-stu-id="69d1b-197">-o</span></span>              | <span data-ttu-id="69d1b-198">--Ausgabe Dir \<Pfad ></span><span class="sxs-lookup"><span data-stu-id="69d1b-198">--output-dir \<PATH></span></span>                    | <span data-ttu-id="69d1b-199">Das Verzeichnis in den Dateien versetzt.</span><span class="sxs-lookup"><span data-stu-id="69d1b-199">The directory to put files in.</span></span> <span data-ttu-id="69d1b-200">Pfade sind relativ zum Projektverzeichnis an.</span><span class="sxs-lookup"><span data-stu-id="69d1b-200">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="69d1b-201"><nobr>--Schema \<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="69d1b-201"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="69d1b-202">Die Schemas der Tabellen zur Generierung von Entitätstypen für.</span><span class="sxs-lookup"><span data-stu-id="69d1b-202">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="69d1b-203">-t</span><span class="sxs-lookup"><span data-stu-id="69d1b-203">-t</span></span>              | <span data-ttu-id="69d1b-204">--Tabelle \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="69d1b-204">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="69d1b-205">Die Tabellen für Entitätstypen generieren.</span><span class="sxs-lookup"><span data-stu-id="69d1b-205">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="69d1b-206">– Verwenden der Datenbanknamen</span><span class="sxs-lookup"><span data-stu-id="69d1b-206">--use-database-names</span></span>                    | <span data-ttu-id="69d1b-207">Verwenden Sie die Tabellen- und Spaltennamen direkt aus der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="69d1b-207">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="69d1b-208">Dotnet Ef Migrationen hinzufügen</span><span class="sxs-lookup"><span data-stu-id="69d1b-208">dotnet ef migrations add</span></span>

<span data-ttu-id="69d1b-209">Fügt eine neue Migration.</span><span class="sxs-lookup"><span data-stu-id="69d1b-209">Adds a new migration.</span></span>

<span data-ttu-id="69d1b-210">Argumente:</span><span class="sxs-lookup"><span data-stu-id="69d1b-210">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="69d1b-211">\<NAME></span><span class="sxs-lookup"><span data-stu-id="69d1b-211">\<NAME></span></span> | <span data-ttu-id="69d1b-212">Der Name der Migration.</span><span class="sxs-lookup"><span data-stu-id="69d1b-212">The name of the migration.</span></span> |

<span data-ttu-id="69d1b-213">Optionen:</span><span class="sxs-lookup"><span data-stu-id="69d1b-213">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="69d1b-214"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="69d1b-214"><nobr>-o</nobr></span></span> | <span data-ttu-id="69d1b-215"><nobr>--Ausgabe Dir \<Pfad ></nobr></span><span class="sxs-lookup"><span data-stu-id="69d1b-215"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="69d1b-216">Das Verzeichnis (und Sub-Namespace) zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="69d1b-216">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="69d1b-217">Pfade sind relativ zum Projektverzeichnis an.</span><span class="sxs-lookup"><span data-stu-id="69d1b-217">Paths are relative to the project directory.</span></span> <span data-ttu-id="69d1b-218">Der Standardwert ist "Migration".</span><span class="sxs-lookup"><span data-stu-id="69d1b-218">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="69d1b-219">Dotnet Ef Migrationen Liste</span><span class="sxs-lookup"><span data-stu-id="69d1b-219">dotnet ef migrations list</span></span>

<span data-ttu-id="69d1b-220">Listet die verfügbaren Migrationen.</span><span class="sxs-lookup"><span data-stu-id="69d1b-220">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="69d1b-221">Entfernen Sie Dotnet Ef Migrationen</span><span class="sxs-lookup"><span data-stu-id="69d1b-221">dotnet ef migrations remove</span></span>

<span data-ttu-id="69d1b-222">Entfernt die letzte Migration.</span><span class="sxs-lookup"><span data-stu-id="69d1b-222">Removes the last migration.</span></span>

<span data-ttu-id="69d1b-223">Optionen:</span><span class="sxs-lookup"><span data-stu-id="69d1b-223">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="69d1b-224">-f</span><span class="sxs-lookup"><span data-stu-id="69d1b-224">-f</span></span> | <span data-ttu-id="69d1b-225">--erzwingen</span><span class="sxs-lookup"><span data-stu-id="69d1b-225">--force</span></span> | <span data-ttu-id="69d1b-226">Die Migration zurückgesetzt werden, wenn es auf die Datenbank angewendet wurde.</span><span class="sxs-lookup"><span data-stu-id="69d1b-226">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="69d1b-227">Dotnet Ef Migrationen Skript</span><span class="sxs-lookup"><span data-stu-id="69d1b-227">dotnet ef migrations script</span></span>

<span data-ttu-id="69d1b-228">Generiert ein SQL-Skript von Migrationen.</span><span class="sxs-lookup"><span data-stu-id="69d1b-228">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="69d1b-229">Argumente:</span><span class="sxs-lookup"><span data-stu-id="69d1b-229">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="69d1b-230">\<FROM></span><span class="sxs-lookup"><span data-stu-id="69d1b-230">\<FROM></span></span> | <span data-ttu-id="69d1b-231">Der Start Migration.</span><span class="sxs-lookup"><span data-stu-id="69d1b-231">The starting migration.</span></span> <span data-ttu-id="69d1b-232">Der Standardwert ist 0 (die ursprüngliche Datenbank).</span><span class="sxs-lookup"><span data-stu-id="69d1b-232">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="69d1b-233">\<TO></span><span class="sxs-lookup"><span data-stu-id="69d1b-233">\<TO></span></span>   | <span data-ttu-id="69d1b-234">Der Endwert Migration.</span><span class="sxs-lookup"><span data-stu-id="69d1b-234">The ending migration.</span></span> <span data-ttu-id="69d1b-235">Standardmäßig bis zum letzten Migration.</span><span class="sxs-lookup"><span data-stu-id="69d1b-235">Defaults to the last migration.</span></span>         |

<span data-ttu-id="69d1b-236">Optionen:</span><span class="sxs-lookup"><span data-stu-id="69d1b-236">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="69d1b-237">-o</span><span class="sxs-lookup"><span data-stu-id="69d1b-237">-o</span></span> | <span data-ttu-id="69d1b-238">--Ausgabe \<Datei ></span><span class="sxs-lookup"><span data-stu-id="69d1b-238">--output \<FILE></span></span> | <span data-ttu-id="69d1b-239">Die Datei, schreibt das Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="69d1b-239">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="69d1b-240">-i</span><span class="sxs-lookup"><span data-stu-id="69d1b-240">-i</span></span> | <span data-ttu-id="69d1b-241">--idempotent</span><span class="sxs-lookup"><span data-stu-id="69d1b-241">--idempotent</span></span>     | <span data-ttu-id="69d1b-242">Generiert ein Skript, das für eine Datenbank bei jeder Migration verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="69d1b-242">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
