---
title: .NET Core-CLI - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 3534336f1caeed96079b35c739d694a536919020
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489608"
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="230d6-102">EF .NET Core-Befehlszeilentools</span><span class="sxs-lookup"><span data-stu-id="230d6-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="230d6-103">Die Entity Framework Core .NET Command-Line-Tools sind eine Erweiterung der plattformübergreifenden **Dotnet** Befehl, der Teil von der [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="230d6-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="230d6-104">Wenn Sie Visual Studio verwenden, empfehlen wir [PMC-Tools] [ 1] seit sie stattdessen ein besseres ganzheitliches Erlebnis bieten.</span><span class="sxs-lookup"><span data-stu-id="230d6-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="230d6-105">Installieren der Tools</span><span class="sxs-lookup"><span data-stu-id="230d6-105">Installing the tools</span></span>
--------------------
> [!NOTE]
> <span data-ttu-id="230d6-106">Das .NET Core SDK 2.1.300-Version sowie neuere **Dotnet Ef** Befehle, die mit EF Core 2.0 und höheren Versionen kompatibel sind.</span><span class="sxs-lookup"><span data-stu-id="230d6-106">The .NET Core SDK version 2.1.300 and newer includes **dotnet ef** commands that are compatible with EF Core 2.0 and later versions.</span></span> <span data-ttu-id="230d6-107">Aus diesem Grund wird, wenn Sie neuere Versionen von .NET Core SDK und EF Core-Runtime verwenden, ist keine Installation erforderlich und können Sie den restlichen Teil dieses Abschnitts ignorieren.</span><span class="sxs-lookup"><span data-stu-id="230d6-107">Therefore if you are using recent versions of the .NET Core SDK and the EF Core runtime, no installation is required and you can ignore the rest of this section.</span></span>
>
> <span data-ttu-id="230d6-108">Auf der anderen Seite der **Dotnet Ef** Tool in .NET Core SDK 2.1.300-Version enthalten und höher ist nicht kompatibel mit EF Core-Version 1.0 und 1.1.</span><span class="sxs-lookup"><span data-stu-id="230d6-108">On the other hand, the **dotnet ef** tool contained in .NET Core SDK version 2.1.300 and newer is not compatible with EF Core version 1.0 and 1.1.</span></span> <span data-ttu-id="230d6-109">Bevor Sie ein Projekt, das dieser früheren Versionen von EF Core auf einem Computer verwendet, die .NET Core SDK 2.1.300 arbeiten können, oder höher installiert, müssen Sie auch Version 2.1.200 installieren oder eine frühere SDK und konfigurieren Sie die Anwendung zur Verwendung dieser älteren Version durch Ändern der  ["Global.JSON"](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) Datei.</span><span class="sxs-lookup"><span data-stu-id="230d6-109">Before you can work with a project that uses these earlier versions of EF Core on a computer that has .NET Core SDK 2.1.300 or newer installed, you must also install version 2.1.200 or older of the SDK and configure the application to use that older version by modifying its [global.json](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) file.</span></span> <span data-ttu-id="230d6-110">Diese Datei befindet sich normalerweise in dem Projektmappenverzeichnis (eine über das Projekt).</span><span class="sxs-lookup"><span data-stu-id="230d6-110">This file is normally included in the solution directory (one above the project).</span></span> <span data-ttu-id="230d6-111">Anschließend können Sie mit der Installlation Anweisung unten fortfahren.</span><span class="sxs-lookup"><span data-stu-id="230d6-111">Then you can proceed with the installlation instruction below.</span></span>

<span data-ttu-id="230d6-112">Für frühere Versionen von .NET Core SDK können Sie die EF Core-.NET-Befehlszeilentools mithilfe der folgenden Schritte installieren:</span><span class="sxs-lookup"><span data-stu-id="230d6-112">For previous versions of the .NET Core SDK, you can install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="230d6-113">Bearbeiten Sie die Projektdatei aus, und fügen Sie Microsoft.EntityFrameworkCore.Tools.DotNet als ein Element "dotnetclitoolreference" (siehe unten)</span><span class="sxs-lookup"><span data-stu-id="230d6-113">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="230d6-114">Führen Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="230d6-114">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

<span data-ttu-id="230d6-115">Das resultierende Projekt sollte etwa wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="230d6-115">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="230d6-116">Einen Paketverweis mit `PrivateAssets="All"` bedeutet, es ist nicht verfügbar gemacht, um Projekte, die dieses Projekt verweisen, handelt es sich besonders für Pakete, die in der Regel nur während der Entwicklung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="230d6-116">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="230d6-117">Wenn Sie alles richtig war, sollten Sie erfolgreich den folgenden Befehl an einer Eingabeaufforderung ausführen können.</span><span class="sxs-lookup"><span data-stu-id="230d6-117">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="230d6-118">Mithilfe der tools</span><span class="sxs-lookup"><span data-stu-id="230d6-118">Using the tools</span></span>
---------------
<span data-ttu-id="230d6-119">Wenn Sie einen Befehl aufrufen, sind zwei Projekte beteiligt:</span><span class="sxs-lookup"><span data-stu-id="230d6-119">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="230d6-120">Dem Zielprojekt werden Dateien hinzugefügt (oder sie werden in einigen Fällen aus diesem entfernt).</span><span class="sxs-lookup"><span data-stu-id="230d6-120">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="230d6-121">Das Zielprojekt standardmäßig auf das Projekt im aktuellen Verzeichnis, jedoch können geändert werden, mithilfe der <nobr> **`--project`** </nobr> Option.</span><span class="sxs-lookup"><span data-stu-id="230d6-121">The target project defaults to the project in the current directory, but can be changed using the <nobr>**`--project`**</nobr> option.</span></span>

<span data-ttu-id="230d6-122">Das Startprojekt wird bei Ausführung des Projektcodes von den Tools emuliert.</span><span class="sxs-lookup"><span data-stu-id="230d6-122">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="230d6-123">Ebenfalls standardmäßig auf das Projekt im aktuellen Verzeichnis, sondern kann geändert werden, mithilfe der **`--startup-project`** Option.</span><span class="sxs-lookup"><span data-stu-id="230d6-123">It also defaults to the project in the current directory, but can be changed using the **`--startup-project`** option.</span></span>

> [!NOTE]
> <span data-ttu-id="230d6-124">Z. B. Aktualisieren der Datenbank der Webanwendung, die EF Core-Installation in einem anderen Projekt sieht wie folgt: `dotnet ef database update --project {project-path}` (von Ihrem Web-app-Verzeichnis)</span><span class="sxs-lookup"><span data-stu-id="230d6-124">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="230d6-125">Allgemeine Optionen:</span><span class="sxs-lookup"><span data-stu-id="230d6-125">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | `--json`                           | <span data-ttu-id="230d6-126">Zeigen Sie die JSON-Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="230d6-126">Show JSON output.</span></span>           |
| <span data-ttu-id="230d6-127">-c</span><span class="sxs-lookup"><span data-stu-id="230d6-127">-c</span></span> | `--context <DBCONTEXT>`           | <span data-ttu-id="230d6-128">Die "DbContext" verwenden.</span><span class="sxs-lookup"><span data-stu-id="230d6-128">The DbContext to use.</span></span>       |
| <span data-ttu-id="230d6-129">-p</span><span class="sxs-lookup"><span data-stu-id="230d6-129">-p</span></span> | `--project <PROJECT>`             | <span data-ttu-id="230d6-130">Das Projekt zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="230d6-130">The project to use.</span></span>         |
| <span data-ttu-id="230d6-131">-s</span><span class="sxs-lookup"><span data-stu-id="230d6-131">-s</span></span> | `--startup-project <PROJECT>`     | <span data-ttu-id="230d6-132">Das Startprojekt verwenden.</span><span class="sxs-lookup"><span data-stu-id="230d6-132">The startup project to use.</span></span> |
|    | `--framework <FRAMEWORK>`         | <span data-ttu-id="230d6-133">Das Zielframework.</span><span class="sxs-lookup"><span data-stu-id="230d6-133">The target framework.</span></span>       |
|    | `--configuration <CONFIGURATION>` | <span data-ttu-id="230d6-134">Die zu verwendende Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="230d6-134">The configuration to use.</span></span>   |
|    | `--runtime <IDENTIFIER>`          | <span data-ttu-id="230d6-135">Die Laufzeit, die verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="230d6-135">The runtime to use.</span></span>         |
| <span data-ttu-id="230d6-136">-h</span><span class="sxs-lookup"><span data-stu-id="230d6-136">-h</span></span> | `--help`                           | <span data-ttu-id="230d6-137">Anzeigen von Hilfeinformationen.</span><span class="sxs-lookup"><span data-stu-id="230d6-137">Show help information.</span></span>      |
| <span data-ttu-id="230d6-138">-v</span><span class="sxs-lookup"><span data-stu-id="230d6-138">-v</span></span> | `--verbose`                        | <span data-ttu-id="230d6-139">Zeigen Sie ausführlichen Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="230d6-139">Show verbose output.</span></span>        |
|    | `--no-color`                       | <span data-ttu-id="230d6-140">Nicht farbig anzuzeigende Ausgabe aus.</span><span class="sxs-lookup"><span data-stu-id="230d6-140">Don't colorize output.</span></span>      |
|    | `--prefix-output`                  | <span data-ttu-id="230d6-141">Das Präfix auf Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="230d6-141">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="230d6-142">Um die ASP.NET Core-Umgebung anzugeben, legen die **"aspnetcore_environment"** Umgebungsvariablen vor der Ausführung.</span><span class="sxs-lookup"><span data-stu-id="230d6-142">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="230d6-143">Befehle</span><span class="sxs-lookup"><span data-stu-id="230d6-143">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="230d6-144">Dotnet Ef Löschen einer Datenbank</span><span class="sxs-lookup"><span data-stu-id="230d6-144">dotnet ef database drop</span></span>

<span data-ttu-id="230d6-145">Löscht die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="230d6-145">Drops the database.</span></span>

<span data-ttu-id="230d6-146">Optionen:</span><span class="sxs-lookup"><span data-stu-id="230d6-146">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="230d6-147">-f</span><span class="sxs-lookup"><span data-stu-id="230d6-147">-f</span></span> | `--force`   | <span data-ttu-id="230d6-148">Bestätigen Sie nicht.</span><span class="sxs-lookup"><span data-stu-id="230d6-148">Don't confirm.</span></span>                                           |
|    | `--dry-run` | <span data-ttu-id="230d6-149">Anzeigen der Datenbank gelöscht werden würde, dies jedoch noch nicht.</span><span class="sxs-lookup"><span data-stu-id="230d6-149">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="230d6-150">Dotnet-Ef-Datenbankupdates</span><span class="sxs-lookup"><span data-stu-id="230d6-150">dotnet ef database update</span></span>

<span data-ttu-id="230d6-151">Aktualisiert die Datenbank für eine angegebene Migration.</span><span class="sxs-lookup"><span data-stu-id="230d6-151">Updates the database to a specified migration.</span></span>

<span data-ttu-id="230d6-152">Argumente:</span><span class="sxs-lookup"><span data-stu-id="230d6-152">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| `<MIGRATION>` | <span data-ttu-id="230d6-153">Die Ziel-Migration.</span><span class="sxs-lookup"><span data-stu-id="230d6-153">The target migration.</span></span> <span data-ttu-id="230d6-154">Wenn der Wert 0 ist, werden bei allen Migrationen rückgängig gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="230d6-154">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="230d6-155">Ist standardmäßig auf die letzte Migration ein.</span><span class="sxs-lookup"><span data-stu-id="230d6-155">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="230d6-156">Dotnet Ef-Dbcontext-Informationen</span><span class="sxs-lookup"><span data-stu-id="230d6-156">dotnet ef dbcontext info</span></span>

<span data-ttu-id="230d6-157">Ruft Informationen über einen "DbContext"-Typ ab.</span><span class="sxs-lookup"><span data-stu-id="230d6-157">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="230d6-158">Dotnet Ef-Dbcontext-Liste</span><span class="sxs-lookup"><span data-stu-id="230d6-158">dotnet ef dbcontext list</span></span>

<span data-ttu-id="230d6-159">Listet die verfügbare Typen von "DbContext".</span><span class="sxs-lookup"><span data-stu-id="230d6-159">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="230d6-160">Dotnet Ef-Dbcontext-scaffold</span><span class="sxs-lookup"><span data-stu-id="230d6-160">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="230d6-161">Erstellt das Gerüst für einen DbContext und Entitätstypen Typen für eine Datenbank.</span><span class="sxs-lookup"><span data-stu-id="230d6-161">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="230d6-162">Argumente:</span><span class="sxs-lookup"><span data-stu-id="230d6-162">Arguments:</span></span>

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| `<CONNECTION>` | <span data-ttu-id="230d6-163">Die Verbindungszeichenfolge in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="230d6-163">The connection string to the database.</span></span>                                      |
| `<PROVIDER>`   | <span data-ttu-id="230d6-164">Die zu verwendende Anbieter.</span><span class="sxs-lookup"><span data-stu-id="230d6-164">The provider to use.</span></span> <span data-ttu-id="230d6-165">(z. B. "Microsoft.entityframeworkcore.SqlServer")</span><span class="sxs-lookup"><span data-stu-id="230d6-165">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="230d6-166">Optionen:</span><span class="sxs-lookup"><span data-stu-id="230d6-166">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="230d6-167"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="230d6-167"><nobr>-d</nobr></span></span> | `--data-annotations`                      | <span data-ttu-id="230d6-168">Verwenden Sie Attribute, um das Modell (sofern möglich) zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="230d6-168">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="230d6-169">Wenn nicht angegeben, wird nur die fluent-API verwendet.</span><span class="sxs-lookup"><span data-stu-id="230d6-169">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="230d6-170">-c</span><span class="sxs-lookup"><span data-stu-id="230d6-170">-c</span></span>              | `--context <NAME>`                       | <span data-ttu-id="230d6-171">Der Name von "DbContext".</span><span class="sxs-lookup"><span data-stu-id="230d6-171">The name of the DbContext.</span></span>                                                                       |
|                 | `--context-dir <PATH>`                   | <span data-ttu-id="230d6-172">Das Verzeichnis, fügen Sie in "DbContext"-Datei.</span><span class="sxs-lookup"><span data-stu-id="230d6-172">The directory to put DbContext file in.</span></span> <span data-ttu-id="230d6-173">Pfade sind relativ zum Projektverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="230d6-173">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="230d6-174">-f</span><span class="sxs-lookup"><span data-stu-id="230d6-174">-f</span></span>              | `--force`                                 | <span data-ttu-id="230d6-175">Überschreiben Sie vorhandene Dateien.</span><span class="sxs-lookup"><span data-stu-id="230d6-175">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="230d6-176">-o</span><span class="sxs-lookup"><span data-stu-id="230d6-176">-o</span></span>              | `--output-dir <PATH>`                    | <span data-ttu-id="230d6-177">Zum Einfügen von Dateien im Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="230d6-177">The directory to put files in.</span></span> <span data-ttu-id="230d6-178">Pfade sind relativ zum Projektverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="230d6-178">Paths are relative to the project directory.</span></span>                      |
|                 | <nobr>`--schema <SCHEMA_NAME>...`</nobr> | <span data-ttu-id="230d6-179">Die Schemas der Tabellen zur Generierung von Entitätstypen für.</span><span class="sxs-lookup"><span data-stu-id="230d6-179">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="230d6-180">-t</span><span class="sxs-lookup"><span data-stu-id="230d6-180">-t</span></span>              | <span data-ttu-id="230d6-181">`--table <TABLE_NAME>`...</span><span class="sxs-lookup"><span data-stu-id="230d6-181">`--table <TABLE_NAME>`...</span></span>                | <span data-ttu-id="230d6-182">Die Tabellen zur Generierung von Entitätstypen für werden soll.</span><span class="sxs-lookup"><span data-stu-id="230d6-182">The tables to generate entity types for.</span></span>                                                         |
|                 | `--use-database-names`                    | <span data-ttu-id="230d6-183">Verwenden Sie Tabellen- und Spaltennamen direkt aus der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="230d6-183">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="230d6-184">Fügen Dotnet Ef-Migrationen</span><span class="sxs-lookup"><span data-stu-id="230d6-184">dotnet ef migrations add</span></span>

<span data-ttu-id="230d6-185">Fügt eine neue Migration hinzu.</span><span class="sxs-lookup"><span data-stu-id="230d6-185">Adds a new migration.</span></span>

<span data-ttu-id="230d6-186">Argumente:</span><span class="sxs-lookup"><span data-stu-id="230d6-186">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| `<NAME>` | <span data-ttu-id="230d6-187">Der Name der Migration.</span><span class="sxs-lookup"><span data-stu-id="230d6-187">The name of the migration.</span></span> |

<span data-ttu-id="230d6-188">Optionen:</span><span class="sxs-lookup"><span data-stu-id="230d6-188">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="230d6-189"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="230d6-189"><nobr>-o</nobr></span></span> | <span data-ttu-id="230d6-190"><nobr> `--output-dir <PATH>` </nobr></span><span class="sxs-lookup"><span data-stu-id="230d6-190"><nobr> `--output-dir <PATH>` </nobr></span></span> | <span data-ttu-id="230d6-191">Das Verzeichnis (und Sub-Namespace) verwenden.</span><span class="sxs-lookup"><span data-stu-id="230d6-191">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="230d6-192">Pfade sind relativ zum Projektverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="230d6-192">Paths are relative to the project directory.</span></span> <span data-ttu-id="230d6-193">Der Standardwert ist "Migrations".</span><span class="sxs-lookup"><span data-stu-id="230d6-193">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="230d6-194">Dotnet Ef Migrations-Liste</span><span class="sxs-lookup"><span data-stu-id="230d6-194">dotnet ef migrations list</span></span>

<span data-ttu-id="230d6-195">Listet die verfügbaren Migrationen.</span><span class="sxs-lookup"><span data-stu-id="230d6-195">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="230d6-196">Dotnet-Ef-Migrationen zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="230d6-196">dotnet ef migrations remove</span></span>

<span data-ttu-id="230d6-197">Entfernt die letzte Migration.</span><span class="sxs-lookup"><span data-stu-id="230d6-197">Removes the last migration.</span></span>

<span data-ttu-id="230d6-198">Optionen:</span><span class="sxs-lookup"><span data-stu-id="230d6-198">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="230d6-199">-f</span><span class="sxs-lookup"><span data-stu-id="230d6-199">-f</span></span> | `--force` | <span data-ttu-id="230d6-200">Die Migration zurückgesetzt, wenn es auf die Datenbank angewendet wurde.</span><span class="sxs-lookup"><span data-stu-id="230d6-200">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="230d6-201">Dotnet Ef Migrations-Skript</span><span class="sxs-lookup"><span data-stu-id="230d6-201">dotnet ef migrations script</span></span>

<span data-ttu-id="230d6-202">Generiert ein SQL-Skript von Migrationen.</span><span class="sxs-lookup"><span data-stu-id="230d6-202">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="230d6-203">Argumente:</span><span class="sxs-lookup"><span data-stu-id="230d6-203">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| `<FROM>` | <span data-ttu-id="230d6-204">Die Migration der ab.</span><span class="sxs-lookup"><span data-stu-id="230d6-204">The starting migration.</span></span> <span data-ttu-id="230d6-205">Der Standardwert ist 0 (die ursprüngliche Datenbank).</span><span class="sxs-lookup"><span data-stu-id="230d6-205">Defaults to 0 (the initial database).</span></span> |
| `<TO>`   | <span data-ttu-id="230d6-206">Der Endpunkt-Migration.</span><span class="sxs-lookup"><span data-stu-id="230d6-206">The ending migration.</span></span> <span data-ttu-id="230d6-207">Ist standardmäßig auf die letzte Migration ein.</span><span class="sxs-lookup"><span data-stu-id="230d6-207">Defaults to the last migration.</span></span>         |

<span data-ttu-id="230d6-208">Optionen:</span><span class="sxs-lookup"><span data-stu-id="230d6-208">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="230d6-209">-o</span><span class="sxs-lookup"><span data-stu-id="230d6-209">-o</span></span> | `--output <FILE>` | <span data-ttu-id="230d6-210">Die Datei, schreibt das Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="230d6-210">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="230d6-211">-i</span><span class="sxs-lookup"><span data-stu-id="230d6-211">-i</span></span> | `--idempotent`     | <span data-ttu-id="230d6-212">Generieren Sie ein Skript, das für eine Datenbank auf eine Migration verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="230d6-212">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
