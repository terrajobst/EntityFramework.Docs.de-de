---
title: .NET Core-CLI - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 81427b010f63bdd591ffb9117c1556722313c3fa
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995296"
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="859fd-102">EF .NET Core-Befehlszeilentools</span><span class="sxs-lookup"><span data-stu-id="859fd-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="859fd-103">Die Entity Framework Core .NET Command-Line-Tools sind eine Erweiterung der plattformübergreifenden **Dotnet** Befehl, der Teil von der [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="859fd-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="859fd-104">Wenn Sie Visual Studio verwenden, empfehlen wir [PMC-Tools] [ 1] seit sie stattdessen ein besseres ganzheitliches Erlebnis bieten.</span><span class="sxs-lookup"><span data-stu-id="859fd-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="859fd-105">Installieren der Tools</span><span class="sxs-lookup"><span data-stu-id="859fd-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="859fd-106">Das .NET Core SDK 2.1.300-Version sowie neuere **Dotnet Ef** Befehle, die mit EF Core 2.0 und höheren Versionen kompatibel sind.</span><span class="sxs-lookup"><span data-stu-id="859fd-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="859fd-107">Aus diesem Grund wird, wenn Sie neuere Versionen von .NET Core SDK und EF Core-Runtime verwenden, ist keine Installation erforderlich und können Sie den restlichen Teil dieses Abschnitts ignorieren.</span><span class="sxs-lookup"><span data-stu-id="859fd-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="859fd-108">Auf der anderen Seite der **Dotnet Ef** Tool in .NET Core SDK 2.1.300-Version enthalten und höher ist nicht kompatibel mit EF Core-Version 1.0 und 1.1.</span><span class="sxs-lookup"><span data-stu-id="859fd-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="859fd-109">Bevor Sie ein Projekt, das dieser früheren Versionen von EF Core auf einem Computer verwendet, die .NET Core SDK 2.1.300 arbeiten können, oder höher installiert, müssen Sie auch Version 2.1.200 installieren oder eine frühere SDK und konfigurieren Sie die Anwendung zur Verwendung dieser älteren Version durch Ändern der  ["Global.JSON"](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) Datei.</span><span class="sxs-lookup"><span data-stu-id="859fd-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="859fd-110">Diese Datei befindet sich normalerweise in dem Projektmappenverzeichnis (eine über das Projekt).</span><span class="sxs-lookup"><span data-stu-id="859fd-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="859fd-111">Anschließend können Sie mit der Installlation Anweisung unten fortfahren.</span><span class="sxs-lookup"><span data-stu-id="859fd-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="859fd-112">Für frühere Versionen von .NET Core SDK können Sie die EF Core-.NET-Befehlszeilentools mithilfe der folgenden Schritte installieren:</span><span class="sxs-lookup"><span data-stu-id="859fd-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="859fd-113">Bearbeiten Sie die Projektdatei aus, und fügen Sie Microsoft.EntityFrameworkCore.Tools.DotNet als ein Element "dotnetclitoolreference" (siehe unten)</span><span class="sxs-lookup"><span data-stu-id="859fd-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="859fd-114">Führen Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="859fd-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="859fd-115">Das resultierende Projekt sollte etwa wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="859fd-115">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="859fd-116">Einen Paketverweis mit `PrivateAssets="All"` bedeutet, es ist nicht verfügbar gemacht, um Projekte, die dieses Projekt verweisen, handelt es sich besonders für Pakete, die in der Regel nur während der Entwicklung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="859fd-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="859fd-117">Wenn Sie alles richtig war, sollten Sie erfolgreich den folgenden Befehl an einer Eingabeaufforderung ausführen können.</span><span class="sxs-lookup"><span data-stu-id="859fd-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="859fd-118">Mithilfe der tools</span><span class="sxs-lookup"><span data-stu-id="859fd-118">Using the tools</span></span>
---------------
<span data-ttu-id="859fd-119">Wenn Sie einen Befehl aufrufen, sind zwei Projekte beteiligt:</span><span class="sxs-lookup"><span data-stu-id="859fd-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="859fd-120">Dem Zielprojekt werden Dateien hinzugefügt (oder sie werden in einigen Fällen aus diesem entfernt).</span><span class="sxs-lookup"><span data-stu-id="859fd-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="859fd-121">Das Zielprojekt standardmäßig auf das Projekt im aktuellen Verzeichnis, jedoch können geändert werden, mithilfe der <nobr> **– Projekt** </nobr> Option.</span><span class="sxs-lookup"><span data-stu-id="859fd-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="859fd-122">Das Startprojekt wird bei Ausführung des Projektcodes von den Tools emuliert.</span><span class="sxs-lookup"><span data-stu-id="859fd-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="859fd-123">Ebenfalls standardmäßig auf das Projekt im aktuellen Verzeichnis, sondern kann geändert werden, mithilfe der **--Startup-Projekt** Option.</span><span class="sxs-lookup"><span data-stu-id="859fd-123">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="859fd-124">Z. B. Aktualisieren der Datenbank der Webanwendung, die EF Core-Installation in einem anderen Projekt sieht wie folgt: `dotnet ef database update --project {project-path}` (von Ihrem Web-app-Verzeichnis)</span><span class="sxs-lookup"><span data-stu-id="859fd-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="859fd-125">Allgemeine Optionen:</span><span class="sxs-lookup"><span data-stu-id="859fd-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="859fd-126">--json</span><span class="sxs-lookup"><span data-stu-id="859fd-126">--json</span></span>                           | <span data-ttu-id="859fd-127">Zeigen Sie die JSON-Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="859fd-127">Show JSON output.</span></span>           |
| <span data-ttu-id="859fd-128">-c</span><span class="sxs-lookup"><span data-stu-id="859fd-128">-c</span></span> | <span data-ttu-id="859fd-129">--Kontext \<"DbContext" ></span><span class="sxs-lookup"><span data-stu-id="859fd-129">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="859fd-130">Die "DbContext" verwenden.</span><span class="sxs-lookup"><span data-stu-id="859fd-130">The DbContext to use.</span></span>       |
| <span data-ttu-id="859fd-131">-p</span><span class="sxs-lookup"><span data-stu-id="859fd-131">-p</span></span> | <span data-ttu-id="859fd-132">--Projekt \<Projekt ></span><span class="sxs-lookup"><span data-stu-id="859fd-132">--project \<PROJECT></span></span>             | <span data-ttu-id="859fd-133">Das Projekt zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="859fd-133">The project to use.</span></span>         |
| <span data-ttu-id="859fd-134">-s</span><span class="sxs-lookup"><span data-stu-id="859fd-134">-s</span></span> | <span data-ttu-id="859fd-135">--Startup-Projekt \<Projekt ></span><span class="sxs-lookup"><span data-stu-id="859fd-135">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="859fd-136">Das Startprojekt verwenden.</span><span class="sxs-lookup"><span data-stu-id="859fd-136">The startup project to use.</span></span> |
|    | <span data-ttu-id="859fd-137">--Framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="859fd-137">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="859fd-138">Das Zielframework.</span><span class="sxs-lookup"><span data-stu-id="859fd-138">The target framework.</span></span>       |
|    | <span data-ttu-id="859fd-139">– Konfiguration \<CONFIGURATION ></span><span class="sxs-lookup"><span data-stu-id="859fd-139">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="859fd-140">Die zu verwendende Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="859fd-140">The configuration to use.</span></span>   |
|    | <span data-ttu-id="859fd-141">--Runtime \<ID ></span><span class="sxs-lookup"><span data-stu-id="859fd-141">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="859fd-142">Die Laufzeit, die verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="859fd-142">The runtime to use.</span></span>         |
| <span data-ttu-id="859fd-143">-h</span><span class="sxs-lookup"><span data-stu-id="859fd-143">-h</span></span> | <span data-ttu-id="859fd-144">– Hilfe</span><span class="sxs-lookup"><span data-stu-id="859fd-144">--help</span></span>                           | <span data-ttu-id="859fd-145">Anzeigen von Hilfeinformationen.</span><span class="sxs-lookup"><span data-stu-id="859fd-145">Show help information.</span></span>      |
| <span data-ttu-id="859fd-146">-v</span><span class="sxs-lookup"><span data-stu-id="859fd-146">-v</span></span> | <span data-ttu-id="859fd-147">--verbose</span><span class="sxs-lookup"><span data-stu-id="859fd-147">--verbose</span></span>                        | <span data-ttu-id="859fd-148">Zeigen Sie ausführlichen Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="859fd-148">Show verbose output.</span></span>        |
|    | <span data-ttu-id="859fd-149">--ohne-Farben</span><span class="sxs-lookup"><span data-stu-id="859fd-149">--no-color</span></span>                       | <span data-ttu-id="859fd-150">Nicht farbig anzuzeigende Ausgabe aus.</span><span class="sxs-lookup"><span data-stu-id="859fd-150">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="859fd-151">--Präfix-Ausgabe</span><span class="sxs-lookup"><span data-stu-id="859fd-151">--prefix-output</span></span>                  | <span data-ttu-id="859fd-152">Das Präfix auf Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="859fd-152">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="859fd-153">Um die ASP.NET Core-Umgebung anzugeben, legen die **"aspnetcore_environment"** Umgebungsvariablen vor der Ausführung.</span><span class="sxs-lookup"><span data-stu-id="859fd-153">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="859fd-154">Befehle</span><span class="sxs-lookup"><span data-stu-id="859fd-154">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="859fd-155">Dotnet Ef Löschen einer Datenbank</span><span class="sxs-lookup"><span data-stu-id="859fd-155">dotnet ef database drop</span></span>

<span data-ttu-id="859fd-156">Löscht die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="859fd-156">Drops the database.</span></span>

<span data-ttu-id="859fd-157">Optionen:</span><span class="sxs-lookup"><span data-stu-id="859fd-157">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="859fd-158">-f</span><span class="sxs-lookup"><span data-stu-id="859fd-158">-f</span></span> | <span data-ttu-id="859fd-159">--erzwingen</span><span class="sxs-lookup"><span data-stu-id="859fd-159">--force</span></span>   | <span data-ttu-id="859fd-160">Bestätigen Sie nicht.</span><span class="sxs-lookup"><span data-stu-id="859fd-160">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="859fd-161">--Dry-Run</span><span class="sxs-lookup"><span data-stu-id="859fd-161">--dry-run</span></span> | <span data-ttu-id="859fd-162">Anzeigen der Datenbank gelöscht werden würde, dies jedoch noch nicht.</span><span class="sxs-lookup"><span data-stu-id="859fd-162">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="859fd-163">Dotnet-Ef-Datenbankupdates</span><span class="sxs-lookup"><span data-stu-id="859fd-163">dotnet ef database update</span></span>

<span data-ttu-id="859fd-164">Aktualisiert die Datenbank für eine angegebene Migration.</span><span class="sxs-lookup"><span data-stu-id="859fd-164">Updates the database to a specified migration.</span></span>

<span data-ttu-id="859fd-165">Argumente:</span><span class="sxs-lookup"><span data-stu-id="859fd-165">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="859fd-166">\<MIGRATION &GT;</span><span class="sxs-lookup"><span data-stu-id="859fd-166">\<MIGRATION></span></span> | <span data-ttu-id="859fd-167">Die Ziel-Migration.</span><span class="sxs-lookup"><span data-stu-id="859fd-167">The target migration.</span></span> <span data-ttu-id="859fd-168">Wenn der Wert 0 ist, werden bei allen Migrationen rückgängig gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="859fd-168">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="859fd-169">Ist standardmäßig auf die letzte Migration ein.</span><span class="sxs-lookup"><span data-stu-id="859fd-169">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="859fd-170">Dotnet Ef-Dbcontext-Informationen</span><span class="sxs-lookup"><span data-stu-id="859fd-170">dotnet ef dbcontext info</span></span>

<span data-ttu-id="859fd-171">Ruft Informationen über einen "DbContext"-Typ ab.</span><span class="sxs-lookup"><span data-stu-id="859fd-171">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="859fd-172">Dotnet Ef-Dbcontext-Liste</span><span class="sxs-lookup"><span data-stu-id="859fd-172">dotnet ef dbcontext list</span></span>

<span data-ttu-id="859fd-173">Listet die verfügbare Typen von "DbContext".</span><span class="sxs-lookup"><span data-stu-id="859fd-173">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="859fd-174">Dotnet Ef-Dbcontext-scaffold</span><span class="sxs-lookup"><span data-stu-id="859fd-174">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="859fd-175">Erstellt das Gerüst für einen DbContext und Entitätstypen Typen für eine Datenbank.</span><span class="sxs-lookup"><span data-stu-id="859fd-175">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="859fd-176">Argumente:</span><span class="sxs-lookup"><span data-stu-id="859fd-176">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| <span data-ttu-id="859fd-177">\<VERBINDUNG &GT;</span><span class="sxs-lookup"><span data-stu-id="859fd-177">\<CONNECTION></span></span> | <span data-ttu-id="859fd-178">Die Verbindungszeichenfolge in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="859fd-178">The connection string to the database.</span></span>                                      |
| <span data-ttu-id="859fd-179">\<ANBIETER &GT;</span><span class="sxs-lookup"><span data-stu-id="859fd-179">\<PROVIDER></span></span>   | <span data-ttu-id="859fd-180">Die zu verwendende Anbieter.</span><span class="sxs-lookup"><span data-stu-id="859fd-180">The provider to use.</span></span> <span data-ttu-id="859fd-181">(z. B. "Microsoft.entityframeworkcore.SqlServer")</span><span class="sxs-lookup"><span data-stu-id="859fd-181">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="859fd-182">Optionen:</span><span class="sxs-lookup"><span data-stu-id="859fd-182">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="859fd-183"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="859fd-183"><nobr>-d</nobr></span></span> | <span data-ttu-id="859fd-184">--datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="859fd-184">--data-annotations</span></span>                      | <span data-ttu-id="859fd-185">Verwenden Sie Attribute, um das Modell (sofern möglich) zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="859fd-185">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="859fd-186">Wenn nicht angegeben, wird nur die fluent-API verwendet.</span><span class="sxs-lookup"><span data-stu-id="859fd-186">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="859fd-187">-c</span><span class="sxs-lookup"><span data-stu-id="859fd-187">-c</span></span>              | <span data-ttu-id="859fd-188">--Kontext \<NAME ></span><span class="sxs-lookup"><span data-stu-id="859fd-188">--context \<NAME></span></span>                       | <span data-ttu-id="859fd-189">Der Name von "DbContext".</span><span class="sxs-lookup"><span data-stu-id="859fd-189">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="859fd-190">--Kontext-Dir \<Pfad ></span><span class="sxs-lookup"><span data-stu-id="859fd-190">--context-dir \<PATH></span></span>                   | <span data-ttu-id="859fd-191">Das Verzeichnis, fügen Sie in "DbContext"-Datei.</span><span class="sxs-lookup"><span data-stu-id="859fd-191">The directory to put DbContext file in.</span></span> <span data-ttu-id="859fd-192">Pfade sind relativ zum Projektverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="859fd-192">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="859fd-193">-f</span><span class="sxs-lookup"><span data-stu-id="859fd-193">-f</span></span>              | <span data-ttu-id="859fd-194">--erzwingen</span><span class="sxs-lookup"><span data-stu-id="859fd-194">--force</span></span>                                 | <span data-ttu-id="859fd-195">Überschreiben Sie vorhandene Dateien.</span><span class="sxs-lookup"><span data-stu-id="859fd-195">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="859fd-196">-o</span><span class="sxs-lookup"><span data-stu-id="859fd-196">-o</span></span>              | <span data-ttu-id="859fd-197">--Ausgabe-Dir \<Pfad ></span><span class="sxs-lookup"><span data-stu-id="859fd-197">--output-dir \<PATH></span></span>                    | <span data-ttu-id="859fd-198">Zum Einfügen von Dateien im Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="859fd-198">The directory to put files in.</span></span> <span data-ttu-id="859fd-199">Pfade sind relativ zum Projektverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="859fd-199">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="859fd-200"><nobr>--Schema \<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="859fd-200"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="859fd-201">Die Schemas der Tabellen zur Generierung von Entitätstypen für.</span><span class="sxs-lookup"><span data-stu-id="859fd-201">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="859fd-202">-t</span><span class="sxs-lookup"><span data-stu-id="859fd-202">-t</span></span>              | <span data-ttu-id="859fd-203">--Tabelle \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="859fd-203">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="859fd-204">Die Tabellen zur Generierung von Entitätstypen für werden soll.</span><span class="sxs-lookup"><span data-stu-id="859fd-204">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="859fd-205">– Verwenden der Datenbanknamen</span><span class="sxs-lookup"><span data-stu-id="859fd-205">--use-database-names</span></span>                    | <span data-ttu-id="859fd-206">Verwenden Sie Tabellen- und Spaltennamen direkt aus der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="859fd-206">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="859fd-207">Fügen Dotnet Ef-Migrationen</span><span class="sxs-lookup"><span data-stu-id="859fd-207">dotnet ef migrations add</span></span>

<span data-ttu-id="859fd-208">Fügt eine neue Migration hinzu.</span><span class="sxs-lookup"><span data-stu-id="859fd-208">Adds a new migration.</span></span>

<span data-ttu-id="859fd-209">Argumente:</span><span class="sxs-lookup"><span data-stu-id="859fd-209">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="859fd-210">\<NAME></span><span class="sxs-lookup"><span data-stu-id="859fd-210">\<NAME></span></span> | <span data-ttu-id="859fd-211">Der Name der Migration.</span><span class="sxs-lookup"><span data-stu-id="859fd-211">The name of the migration.</span></span> |

<span data-ttu-id="859fd-212">Optionen:</span><span class="sxs-lookup"><span data-stu-id="859fd-212">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="859fd-213"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="859fd-213"><nobr>-o</nobr></span></span> | <span data-ttu-id="859fd-214"><nobr>--Ausgabe-Dir \<Pfad ></nobr></span><span class="sxs-lookup"><span data-stu-id="859fd-214"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="859fd-215">Das Verzeichnis (und Sub-Namespace) verwenden.</span><span class="sxs-lookup"><span data-stu-id="859fd-215">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="859fd-216">Pfade sind relativ zum Projektverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="859fd-216">Paths are relative to the project directory.</span></span> <span data-ttu-id="859fd-217">Der Standardwert ist "Migrations".</span><span class="sxs-lookup"><span data-stu-id="859fd-217">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="859fd-218">Dotnet Ef Migrations-Liste</span><span class="sxs-lookup"><span data-stu-id="859fd-218">dotnet ef migrations list</span></span>

<span data-ttu-id="859fd-219">Listet die verfügbaren Migrationen.</span><span class="sxs-lookup"><span data-stu-id="859fd-219">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="859fd-220">Dotnet-Ef-Migrationen zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="859fd-220">dotnet ef migrations remove</span></span>

<span data-ttu-id="859fd-221">Entfernt die letzte Migration.</span><span class="sxs-lookup"><span data-stu-id="859fd-221">Removes the last migration.</span></span>

<span data-ttu-id="859fd-222">Optionen:</span><span class="sxs-lookup"><span data-stu-id="859fd-222">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="859fd-223">-f</span><span class="sxs-lookup"><span data-stu-id="859fd-223">-f</span></span> | <span data-ttu-id="859fd-224">--erzwingen</span><span class="sxs-lookup"><span data-stu-id="859fd-224">--force</span></span> | <span data-ttu-id="859fd-225">Die Migration zurückgesetzt, wenn es auf die Datenbank angewendet wurde.</span><span class="sxs-lookup"><span data-stu-id="859fd-225">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="859fd-226">Dotnet Ef Migrations-Skript</span><span class="sxs-lookup"><span data-stu-id="859fd-226">dotnet ef migrations script</span></span>

<span data-ttu-id="859fd-227">Generiert ein SQL-Skript von Migrationen.</span><span class="sxs-lookup"><span data-stu-id="859fd-227">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="859fd-228">Argumente:</span><span class="sxs-lookup"><span data-stu-id="859fd-228">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="859fd-229">\<FROM></span><span class="sxs-lookup"><span data-stu-id="859fd-229">\<FROM></span></span> | <span data-ttu-id="859fd-230">Die Migration der ab.</span><span class="sxs-lookup"><span data-stu-id="859fd-230">The starting migration.</span></span> <span data-ttu-id="859fd-231">Der Standardwert ist 0 (die ursprüngliche Datenbank).</span><span class="sxs-lookup"><span data-stu-id="859fd-231">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="859fd-232">\<TO></span><span class="sxs-lookup"><span data-stu-id="859fd-232">\<TO></span></span>   | <span data-ttu-id="859fd-233">Der Endpunkt-Migration.</span><span class="sxs-lookup"><span data-stu-id="859fd-233">The ending migration.</span></span> <span data-ttu-id="859fd-234">Ist standardmäßig auf die letzte Migration ein.</span><span class="sxs-lookup"><span data-stu-id="859fd-234">Defaults to the last migration.</span></span>         |

<span data-ttu-id="859fd-235">Optionen:</span><span class="sxs-lookup"><span data-stu-id="859fd-235">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="859fd-236">-o</span><span class="sxs-lookup"><span data-stu-id="859fd-236">-o</span></span> | <span data-ttu-id="859fd-237">--Ausgabe \<Datei ></span><span class="sxs-lookup"><span data-stu-id="859fd-237">--output \<FILE></span></span> | <span data-ttu-id="859fd-238">Die Datei, schreibt das Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="859fd-238">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="859fd-239">-i</span><span class="sxs-lookup"><span data-stu-id="859fd-239">-i</span></span> | <span data-ttu-id="859fd-240">--idempotent</span><span class="sxs-lookup"><span data-stu-id="859fd-240">--idempotent</span></span>     | <span data-ttu-id="859fd-241">Generieren Sie ein Skript, das für eine Datenbank auf eine Migration verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="859fd-241">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
