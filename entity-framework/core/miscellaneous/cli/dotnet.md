---
title: .NET Core CLI – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 721235b07e695efd8df43294e1f4e90c28ae83d7
ms.sourcegitcommit: 72e59e6af86b568653e1b29727529dfd7f65d312
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34754495"
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="b6693-102">EF Core .NET-Befehlszeilentools</span><span class="sxs-lookup"><span data-stu-id="b6693-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="b6693-103">Das Entity Framework Core .NET Befehlszeilentools sind eine Erweiterung für die plattformübergreifende **Dotnet** Befehl, die Teil von der [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="b6693-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="b6693-104">Wenn Sie Visual Studio verwenden, empfiehlt es sich [PMC Tools] [ 1] seitdem sie stattdessen eine weitere integrierte Lösung bieten.</span><span class="sxs-lookup"><span data-stu-id="b6693-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="b6693-105">Installieren der Tools</span><span class="sxs-lookup"><span data-stu-id="b6693-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="b6693-106">.NET Core SDK, Version 2.1.300 und enthält neuere **Dotnet Ef** Befehle, die mit EF Core 2.0 und höheren Versionen kompatibel sind.</span><span class="sxs-lookup"><span data-stu-id="b6693-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="b6693-107">Daher wird bei Verwendung von neueren Versionen von .NET Core SDK und die EF-Core-Laufzeit ist keine Installation erforderlich und können Sie die restlichen Teil dieses Abschnitts ignorieren.</span><span class="sxs-lookup"><span data-stu-id="b6693-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="b6693-108">Auf der anderen Seite der **Dotnet Ef** Tool in .NET Core SDK-Version 2.1.300 enthalten und höher ist nicht kompatibel mit EF-Core-Version 1.0 und 1.1.</span><span class="sxs-lookup"><span data-stu-id="b6693-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="b6693-109">Bevor Sie kann für ein Projekt, das dieser früheren Versionen von EF Core auf einem Computer verwendet, die .NET Core SDK 2.1.300 verfügt oder höher installiert ist, müssen Sie auch Version 2.1.200 installieren oder eine ältere des SDK und konfigurieren Sie die Anwendung zur Verwendung dieser älteren Version durch Ändern der  [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) Datei.</span><span class="sxs-lookup"><span data-stu-id="b6693-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="b6693-110">Diese Datei ist normalerweise in dem Projektmappenverzeichnis (eine oberhalb des Projekts) enthalten.</span><span class="sxs-lookup"><span data-stu-id="b6693-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="b6693-111">Anschließend können Sie mit der Installlation-Anweisung unten fortfahren.</span><span class="sxs-lookup"><span data-stu-id="b6693-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="b6693-112">Für frühere Versionen von .NET Core SDK können Sie die EF Core .NET-Befehlszeilentools mithilfe der folgenden Schritte installieren:</span><span class="sxs-lookup"><span data-stu-id="b6693-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="b6693-113">Bearbeiten der Projektdatei, und fügen Sie Microsoft.EntityFrameworkCore.Tools.DotNet als DotNetCliToolReference-Element (siehe unten)</span><span class="sxs-lookup"><span data-stu-id="b6693-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="b6693-114">Führen Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="b6693-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="b6693-115">Das daraus resultierende Projekt sollte etwa wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="b6693-115">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="b6693-116">Ein Verweis Paket mit `PrivateAssets="All"` bedeutet, er ist nicht verfügbar gemacht, um Projekte, die auf dieses Projekt verweisen, insbesondere für die Pakete, die in der Regel nur während der Entwicklung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="b6693-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="b6693-117">Wenn Sie alles richtig haben, sollten Sie den folgenden Befehl an einer Eingabeaufforderung erfolgreich ausführen können.</span><span class="sxs-lookup"><span data-stu-id="b6693-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="b6693-118">Mithilfe der tools</span><span class="sxs-lookup"><span data-stu-id="b6693-118">Using the tools</span></span>
---------------
<span data-ttu-id="b6693-119">Bei jedem eines Befehls aufrufen umfasst zwei Projekte:</span><span class="sxs-lookup"><span data-stu-id="b6693-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="b6693-120">Dem Zielprojekt werden Dateien hinzugefügt (oder sie werden in einigen Fällen aus diesem entfernt).</span><span class="sxs-lookup"><span data-stu-id="b6693-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="b6693-121">Das Zielprojekt wird standardmäßig auf das Projekt im aktuellen Verzeichnis, jedoch können geändert werden, mithilfe der <nobr> **– Projekt** </nobr> Option.</span><span class="sxs-lookup"><span data-stu-id="b6693-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="b6693-122">Das Startprojekt wird bei Ausführung des Projektcodes von den Tools emuliert.</span><span class="sxs-lookup"><span data-stu-id="b6693-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="b6693-123">Auch wird standardmäßig auf das Projekt im aktuellen Verzeichnis, aber kann geändert werden, mithilfe der **--Startprojekt** Option.</span><span class="sxs-lookup"><span data-stu-id="b6693-123">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="b6693-124">Aktualisieren der Datenbank Ihrer Web-Anwendung, die EF-Core, die in einem anderen Projekt installiert hat z. B. wie folgt aussehen würde: `dotnet ef database update --project {project-path}` (aus Ihrem Verzeichnis für den Web-app)</span><span class="sxs-lookup"><span data-stu-id="b6693-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="b6693-125">Allgemeine Optionen:</span><span class="sxs-lookup"><span data-stu-id="b6693-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="b6693-126">--json</span><span class="sxs-lookup"><span data-stu-id="b6693-126">--json</span></span>                           | <span data-ttu-id="b6693-127">JSON-Ausgabe anzeigen.</span><span class="sxs-lookup"><span data-stu-id="b6693-127">Show JSON output.</span></span>           |
| <span data-ttu-id="b6693-128">-c</span><span class="sxs-lookup"><span data-stu-id="b6693-128">-c</span></span> | <span data-ttu-id="b6693-129">--Kontext \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="b6693-129">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="b6693-130">Die DbContext verwenden.</span><span class="sxs-lookup"><span data-stu-id="b6693-130">The DbContext to use.</span></span>       |
| <span data-ttu-id="b6693-131">-p</span><span class="sxs-lookup"><span data-stu-id="b6693-131">-p</span></span> | <span data-ttu-id="b6693-132">--Projekt \<Projekt ></span><span class="sxs-lookup"><span data-stu-id="b6693-132">--project \<PROJECT></span></span>             | <span data-ttu-id="b6693-133">Das Projekt verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="b6693-133">The project to use.</span></span>         |
| <span data-ttu-id="b6693-134">-s</span><span class="sxs-lookup"><span data-stu-id="b6693-134">-s</span></span> | <span data-ttu-id="b6693-135">--Startprojekt \<Projekt ></span><span class="sxs-lookup"><span data-stu-id="b6693-135">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="b6693-136">Das Startup-Projekt verwenden.</span><span class="sxs-lookup"><span data-stu-id="b6693-136">The startup project to use.</span></span> |
|    | <span data-ttu-id="b6693-137">--Framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="b6693-137">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="b6693-138">Das Zielframework.</span><span class="sxs-lookup"><span data-stu-id="b6693-138">The target framework.</span></span>       |
|    | <span data-ttu-id="b6693-139">--Konfiguration \<CONFIGURATION ></span><span class="sxs-lookup"><span data-stu-id="b6693-139">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="b6693-140">Die zu verwendende Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="b6693-140">The configuration to use.</span></span>   |
|    | <span data-ttu-id="b6693-141">--Runtime \<BEZEICHNER ></span><span class="sxs-lookup"><span data-stu-id="b6693-141">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="b6693-142">Die Laufzeit verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="b6693-142">The runtime to use.</span></span>         |
| <span data-ttu-id="b6693-143">-h</span><span class="sxs-lookup"><span data-stu-id="b6693-143">-h</span></span> | <span data-ttu-id="b6693-144">– Hilfe</span><span class="sxs-lookup"><span data-stu-id="b6693-144">--help</span></span>                           | <span data-ttu-id="b6693-145">Anzeigen von Hilfeinformationen.</span><span class="sxs-lookup"><span data-stu-id="b6693-145">Show help information.</span></span>      |
| <span data-ttu-id="b6693-146">-v</span><span class="sxs-lookup"><span data-stu-id="b6693-146">-v</span></span> | <span data-ttu-id="b6693-147">-verbose</span><span class="sxs-lookup"><span data-stu-id="b6693-147">--verbose</span></span>                        | <span data-ttu-id="b6693-148">Zeigen Sie eine ausführlichen Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="b6693-148">Show verbose output.</span></span>        |
|    | <span data-ttu-id="b6693-149">--keine Farbe</span><span class="sxs-lookup"><span data-stu-id="b6693-149">--no-color</span></span>                       | <span data-ttu-id="b6693-150">Nicht zur farblichen Kennzeichnung der Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="b6693-150">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="b6693-151">--Präfix-Ausgabe</span><span class="sxs-lookup"><span data-stu-id="b6693-151">--prefix-output</span></span>                  | <span data-ttu-id="b6693-152">Präfix Ausgabe auf.</span><span class="sxs-lookup"><span data-stu-id="b6693-152">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="b6693-153">Legen Sie zum Angeben der ASP.NET Core-Umgebung die **ASPNETCORE_ENVIRONMENT** Umgebungsvariablen vor der Ausführung.</span><span class="sxs-lookup"><span data-stu-id="b6693-153">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="b6693-154">Befehle</span><span class="sxs-lookup"><span data-stu-id="b6693-154">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="b6693-155">Dotnet Ef das Löschen einer Datenbank</span><span class="sxs-lookup"><span data-stu-id="b6693-155">dotnet ef database drop</span></span>

<span data-ttu-id="b6693-156">Löscht die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="b6693-156">Drops the database.</span></span>

<span data-ttu-id="b6693-157">Optionen:</span><span class="sxs-lookup"><span data-stu-id="b6693-157">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="b6693-158">-f</span><span class="sxs-lookup"><span data-stu-id="b6693-158">-f</span></span> | <span data-ttu-id="b6693-159">--erzwingen</span><span class="sxs-lookup"><span data-stu-id="b6693-159">--force</span></span>   | <span data-ttu-id="b6693-160">Bestätigen Sie nicht.</span><span class="sxs-lookup"><span data-stu-id="b6693-160">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="b6693-161">--Probelauf</span><span class="sxs-lookup"><span data-stu-id="b6693-161">--dry-run</span></span> | <span data-ttu-id="b6693-162">Anzeigen, welche Datenbank verworfen werden, jedoch nicht, legen Sie sie.</span><span class="sxs-lookup"><span data-stu-id="b6693-162">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="b6693-163">Dotnet Ef-Datenbankupdate</span><span class="sxs-lookup"><span data-stu-id="b6693-163">dotnet ef database update</span></span>

<span data-ttu-id="b6693-164">Aktualisiert die Datenbank in eine angegebene Migration.</span><span class="sxs-lookup"><span data-stu-id="b6693-164">Updates the database to a specified migration.</span></span>

<span data-ttu-id="b6693-165">Argumente:</span><span class="sxs-lookup"><span data-stu-id="b6693-165">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="b6693-166">\<MIGRATION &GT;</span><span class="sxs-lookup"><span data-stu-id="b6693-166">\<MIGRATION></span></span> | <span data-ttu-id="b6693-167">Der zielmigration.</span><span class="sxs-lookup"><span data-stu-id="b6693-167">The target migration.</span></span> <span data-ttu-id="b6693-168">Bei 0 werden bei allen Migrationen rückgängig gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="b6693-168">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="b6693-169">Standardmäßig bis zum letzten Migration.</span><span class="sxs-lookup"><span data-stu-id="b6693-169">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="b6693-170">Dotnet Ef Dbcontext info</span><span class="sxs-lookup"><span data-stu-id="b6693-170">dotnet ef dbcontext info</span></span>

<span data-ttu-id="b6693-171">Ruft Informationen zu einem DbContext-Typ ab.</span><span class="sxs-lookup"><span data-stu-id="b6693-171">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="b6693-172">Dotnet Ef Dbcontext-Liste</span><span class="sxs-lookup"><span data-stu-id="b6693-172">dotnet ef dbcontext list</span></span>

<span data-ttu-id="b6693-173">Listet die verfügbare DbContext-Typen.</span><span class="sxs-lookup"><span data-stu-id="b6693-173">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="b6693-174">Dotnet Ef Dbcontext Gerüst</span><span class="sxs-lookup"><span data-stu-id="b6693-174">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="b6693-175">Gerüste ein ' DbContext ' und Entität Typen für eine Datenbank.</span><span class="sxs-lookup"><span data-stu-id="b6693-175">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="b6693-176">Argumente:</span><span class="sxs-lookup"><span data-stu-id="b6693-176">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| <span data-ttu-id="b6693-177">\<VERBINDUNG &GT;</span><span class="sxs-lookup"><span data-stu-id="b6693-177">\<CONNECTION></span></span> | <span data-ttu-id="b6693-178">Die Verbindungszeichenfolge zur Datenbank.</span><span class="sxs-lookup"><span data-stu-id="b6693-178">The connection string to the database.</span></span>                                      |
| <span data-ttu-id="b6693-179">\<ANBIETER &GT;</span><span class="sxs-lookup"><span data-stu-id="b6693-179">\<PROVIDER></span></span>   | <span data-ttu-id="b6693-180">Die zu verwendenden Anbieter an.</span><span class="sxs-lookup"><span data-stu-id="b6693-180">The provider to use.</span></span> <span data-ttu-id="b6693-181">(z. B. Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="b6693-181">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="b6693-182">Optionen:</span><span class="sxs-lookup"><span data-stu-id="b6693-182">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b6693-183"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="b6693-183"><nobr>-d</nobr></span></span> | <span data-ttu-id="b6693-184">--datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="b6693-184">--data-annotations</span></span>                      | <span data-ttu-id="b6693-185">Verwenden Sie Attribute, um das Modell (sofern möglich) konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="b6693-185">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="b6693-186">Wenn nicht angegeben, wird nur die fluent-API verwendet.</span><span class="sxs-lookup"><span data-stu-id="b6693-186">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="b6693-187">-c</span><span class="sxs-lookup"><span data-stu-id="b6693-187">-c</span></span>              | <span data-ttu-id="b6693-188">--Kontext \<NAME ></span><span class="sxs-lookup"><span data-stu-id="b6693-188">--context \<NAME></span></span>                       | <span data-ttu-id="b6693-189">Der Name von ' DbContext '.</span><span class="sxs-lookup"><span data-stu-id="b6693-189">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="b6693-190">--Kontext Dir \<Pfad ></span><span class="sxs-lookup"><span data-stu-id="b6693-190">--context-dir \<PATH></span></span>                   | <span data-ttu-id="b6693-191">Das Verzeichnis DbContext-Datei in den versetzt.</span><span class="sxs-lookup"><span data-stu-id="b6693-191">The directory to put DbContext file in.</span></span> <span data-ttu-id="b6693-192">Pfade sind relativ zum Projektverzeichnis an.</span><span class="sxs-lookup"><span data-stu-id="b6693-192">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="b6693-193">-f</span><span class="sxs-lookup"><span data-stu-id="b6693-193">-f</span></span>              | <span data-ttu-id="b6693-194">--erzwingen</span><span class="sxs-lookup"><span data-stu-id="b6693-194">--force</span></span>                                 | <span data-ttu-id="b6693-195">Überschreiben Sie vorhandene Dateien.</span><span class="sxs-lookup"><span data-stu-id="b6693-195">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="b6693-196">-o</span><span class="sxs-lookup"><span data-stu-id="b6693-196">-o</span></span>              | <span data-ttu-id="b6693-197">--Ausgabe Dir \<Pfad ></span><span class="sxs-lookup"><span data-stu-id="b6693-197">--output-dir \<PATH></span></span>                    | <span data-ttu-id="b6693-198">Das Verzeichnis in den Dateien versetzt.</span><span class="sxs-lookup"><span data-stu-id="b6693-198">The directory to put files in.</span></span> <span data-ttu-id="b6693-199">Pfade sind relativ zum Projektverzeichnis an.</span><span class="sxs-lookup"><span data-stu-id="b6693-199">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="b6693-200"><nobr>--Schema \<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="b6693-200"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="b6693-201">Die Schemas der Tabellen zur Generierung von Entitätstypen für.</span><span class="sxs-lookup"><span data-stu-id="b6693-201">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="b6693-202">-t</span><span class="sxs-lookup"><span data-stu-id="b6693-202">-t</span></span>              | <span data-ttu-id="b6693-203">--Tabelle \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="b6693-203">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="b6693-204">Die Tabellen für Entitätstypen generieren.</span><span class="sxs-lookup"><span data-stu-id="b6693-204">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="b6693-205">– Verwenden der Datenbanknamen</span><span class="sxs-lookup"><span data-stu-id="b6693-205">--use-database-names</span></span>                    | <span data-ttu-id="b6693-206">Verwenden Sie die Tabellen- und Spaltennamen direkt aus der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="b6693-206">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="b6693-207">Dotnet Ef Migrationen hinzufügen</span><span class="sxs-lookup"><span data-stu-id="b6693-207">dotnet ef migrations add</span></span>

<span data-ttu-id="b6693-208">Fügt eine neue Migration.</span><span class="sxs-lookup"><span data-stu-id="b6693-208">Adds a new migration.</span></span>

<span data-ttu-id="b6693-209">Argumente:</span><span class="sxs-lookup"><span data-stu-id="b6693-209">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="b6693-210">\<NAME></span><span class="sxs-lookup"><span data-stu-id="b6693-210">\<NAME></span></span> | <span data-ttu-id="b6693-211">Der Name der Migration.</span><span class="sxs-lookup"><span data-stu-id="b6693-211">The name of the migration.</span></span> |

<span data-ttu-id="b6693-212">Optionen:</span><span class="sxs-lookup"><span data-stu-id="b6693-212">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b6693-213"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="b6693-213"><nobr>-o</nobr></span></span> | <span data-ttu-id="b6693-214"><nobr>--Ausgabe Dir \<Pfad ></nobr></span><span class="sxs-lookup"><span data-stu-id="b6693-214"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="b6693-215">Das Verzeichnis (und Sub-Namespace) zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="b6693-215">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="b6693-216">Pfade sind relativ zum Projektverzeichnis an.</span><span class="sxs-lookup"><span data-stu-id="b6693-216">Paths are relative to the project directory.</span></span> <span data-ttu-id="b6693-217">Der Standardwert ist "Migration".</span><span class="sxs-lookup"><span data-stu-id="b6693-217">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="b6693-218">Dotnet Ef Migrationen Liste</span><span class="sxs-lookup"><span data-stu-id="b6693-218">dotnet ef migrations list</span></span>

<span data-ttu-id="b6693-219">Listet die verfügbaren Migrationen.</span><span class="sxs-lookup"><span data-stu-id="b6693-219">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="b6693-220">Entfernen Sie Dotnet Ef Migrationen</span><span class="sxs-lookup"><span data-stu-id="b6693-220">dotnet ef migrations remove</span></span>

<span data-ttu-id="b6693-221">Entfernt die letzte Migration.</span><span class="sxs-lookup"><span data-stu-id="b6693-221">Removes the last migration.</span></span>

<span data-ttu-id="b6693-222">Optionen:</span><span class="sxs-lookup"><span data-stu-id="b6693-222">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="b6693-223">-f</span><span class="sxs-lookup"><span data-stu-id="b6693-223">-f</span></span> | <span data-ttu-id="b6693-224">--erzwingen</span><span class="sxs-lookup"><span data-stu-id="b6693-224">--force</span></span> | <span data-ttu-id="b6693-225">Die Migration zurückgesetzt werden, wenn es auf die Datenbank angewendet wurde.</span><span class="sxs-lookup"><span data-stu-id="b6693-225">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="b6693-226">Dotnet Ef Migrationen Skript</span><span class="sxs-lookup"><span data-stu-id="b6693-226">dotnet ef migrations script</span></span>

<span data-ttu-id="b6693-227">Generiert ein SQL-Skript von Migrationen.</span><span class="sxs-lookup"><span data-stu-id="b6693-227">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="b6693-228">Argumente:</span><span class="sxs-lookup"><span data-stu-id="b6693-228">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="b6693-229">\<FROM></span><span class="sxs-lookup"><span data-stu-id="b6693-229">\<FROM></span></span> | <span data-ttu-id="b6693-230">Der Start Migration.</span><span class="sxs-lookup"><span data-stu-id="b6693-230">The starting migration.</span></span> <span data-ttu-id="b6693-231">Der Standardwert ist 0 (die ursprüngliche Datenbank).</span><span class="sxs-lookup"><span data-stu-id="b6693-231">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="b6693-232">\<TO></span><span class="sxs-lookup"><span data-stu-id="b6693-232">\<TO></span></span>   | <span data-ttu-id="b6693-233">Der Endwert Migration.</span><span class="sxs-lookup"><span data-stu-id="b6693-233">The ending migration.</span></span> <span data-ttu-id="b6693-234">Standardmäßig bis zum letzten Migration.</span><span class="sxs-lookup"><span data-stu-id="b6693-234">Defaults to the last migration.</span></span>         |

<span data-ttu-id="b6693-235">Optionen:</span><span class="sxs-lookup"><span data-stu-id="b6693-235">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="b6693-236">-o</span><span class="sxs-lookup"><span data-stu-id="b6693-236">-o</span></span> | <span data-ttu-id="b6693-237">--Ausgabe \<Datei ></span><span class="sxs-lookup"><span data-stu-id="b6693-237">--output \<FILE></span></span> | <span data-ttu-id="b6693-238">Die Datei, schreibt das Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="b6693-238">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="b6693-239">-i</span><span class="sxs-lookup"><span data-stu-id="b6693-239">-i</span></span> | <span data-ttu-id="b6693-240">--idempotent</span><span class="sxs-lookup"><span data-stu-id="b6693-240">--idempotent</span></span>     | <span data-ttu-id="b6693-241">Generiert ein Skript, das für eine Datenbank bei jeder Migration verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="b6693-241">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
