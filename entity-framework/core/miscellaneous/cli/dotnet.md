---
title: .NET Core CLI – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 396d31c9d0c0f47d299f49e82e557ed29b8420e7
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="9ace9-102">EF Core .NET-Befehlszeilentools</span><span class="sxs-lookup"><span data-stu-id="9ace9-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="9ace9-103">Das Entity Framework Core .NET Befehlszeilentools sind eine Erweiterung für die plattformübergreifende **Dotnet** Befehl, die Teil von der [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="9ace9-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="9ace9-104">Wenn Sie Visual Studio verwenden, empfiehlt es sich [PMC Tools] [ 1] seitdem sie stattdessen eine weitere integrierte Lösung bieten.</span><span class="sxs-lookup"><span data-stu-id="9ace9-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="9ace9-105">Installieren der Tools</span><span class="sxs-lookup"><span data-stu-id="9ace9-105">Installing the tools</span></span>
--------------------
<span data-ttu-id="9ace9-106">Installieren Sie die EF Core .NET-Befehlszeilentools mithilfe der folgenden Schritte:</span><span class="sxs-lookup"><span data-stu-id="9ace9-106">Install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="9ace9-107">Bearbeiten der Projektdatei, und fügen Sie Microsoft.EntityFrameworkCore.Tools.DotNet als DotNetCliToolReference-Element (siehe unten)</span><span class="sxs-lookup"><span data-stu-id="9ace9-107">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="9ace9-108">Führen Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="9ace9-108">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore


<span data-ttu-id="9ace9-109">Das daraus resultierende Projekt sollte etwa wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="9ace9-109">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="9ace9-110">Ein Verweis Paket mit `PrivateAssets="All"` bedeutet, er ist nicht verfügbar gemacht, um Projekte, die auf dieses Projekt verweisen, insbesondere für die Pakete, die in der Regel nur während der Entwicklung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="9ace9-110">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="9ace9-111">Wenn Sie alles richtig haben, sollten Sie den folgenden Befehl an einer Eingabeaufforderung erfolgreich ausführen können.</span><span class="sxs-lookup"><span data-stu-id="9ace9-111">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="9ace9-112">Mithilfe der tools</span><span class="sxs-lookup"><span data-stu-id="9ace9-112">Using the tools</span></span>
---------------
<span data-ttu-id="9ace9-113">Bei jedem eines Befehls aufrufen umfasst zwei Projekte:</span><span class="sxs-lookup"><span data-stu-id="9ace9-113">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="9ace9-114">Dem Zielprojekt werden Dateien hinzugefügt (oder sie werden in einigen Fällen aus diesem entfernt).</span><span class="sxs-lookup"><span data-stu-id="9ace9-114">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="9ace9-115">Das Zielprojekt wird standardmäßig auf das Projekt im aktuellen Verzeichnis, jedoch können geändert werden, mithilfe der <nobr> **– Projekt** </nobr> Option.</span><span class="sxs-lookup"><span data-stu-id="9ace9-115">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="9ace9-116">Das Startprojekt wird bei Ausführung des Projektcodes von den Tools emuliert.</span><span class="sxs-lookup"><span data-stu-id="9ace9-116">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="9ace9-117">Auch wird standardmäßig auf das Projekt im aktuellen Verzeichnis, aber kann geändert werden, mithilfe der **--Startprojekt** Option.</span><span class="sxs-lookup"><span data-stu-id="9ace9-117">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

> [!NOTE]
> <span data-ttu-id="9ace9-118">Aktualisieren der Datenbank Ihrer Web-Anwendung, die EF-Core, die in einem anderen Projekt installiert hat z. B. wie folgt aussehen würde: `dotnet ef database update --project {project-path}` (aus Ihrem Verzeichnis für den Web-app)</span><span class="sxs-lookup"><span data-stu-id="9ace9-118">For instance, updating the database of your web application that has EF Core installed in a different project would look like this: `dotnet ef database update --project {project-path}` (from your web app directory)</span></span>

<span data-ttu-id="9ace9-119">Allgemeine Optionen:</span><span class="sxs-lookup"><span data-stu-id="9ace9-119">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="9ace9-120">--json</span><span class="sxs-lookup"><span data-stu-id="9ace9-120">--json</span></span>                           | <span data-ttu-id="9ace9-121">JSON-Ausgabe anzeigen.</span><span class="sxs-lookup"><span data-stu-id="9ace9-121">Show JSON output.</span></span>           |
| <span data-ttu-id="9ace9-122">-c</span><span class="sxs-lookup"><span data-stu-id="9ace9-122">-c</span></span> | <span data-ttu-id="9ace9-123">--Kontext \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="9ace9-123">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="9ace9-124">Die DbContext verwenden.</span><span class="sxs-lookup"><span data-stu-id="9ace9-124">The DbContext to use.</span></span>       |
| <span data-ttu-id="9ace9-125">-p</span><span class="sxs-lookup"><span data-stu-id="9ace9-125">-p</span></span> | <span data-ttu-id="9ace9-126">--Projekt \<Projekt ></span><span class="sxs-lookup"><span data-stu-id="9ace9-126">--project \<PROJECT></span></span>             | <span data-ttu-id="9ace9-127">Das Projekt verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="9ace9-127">The project to use.</span></span>         |
| <span data-ttu-id="9ace9-128">-s</span><span class="sxs-lookup"><span data-stu-id="9ace9-128">-s</span></span> | <span data-ttu-id="9ace9-129">--Startprojekt \<Projekt ></span><span class="sxs-lookup"><span data-stu-id="9ace9-129">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="9ace9-130">Das Startup-Projekt verwenden.</span><span class="sxs-lookup"><span data-stu-id="9ace9-130">The startup project to use.</span></span> |
|    | <span data-ttu-id="9ace9-131">--Framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="9ace9-131">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="9ace9-132">Das Zielframework.</span><span class="sxs-lookup"><span data-stu-id="9ace9-132">The target framework.</span></span>       |
|    | <span data-ttu-id="9ace9-133">--Konfiguration \<CONFIGURATION ></span><span class="sxs-lookup"><span data-stu-id="9ace9-133">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="9ace9-134">Die zu verwendende Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="9ace9-134">The configuration to use.</span></span>   |
|    | <span data-ttu-id="9ace9-135">--Runtime \<BEZEICHNER ></span><span class="sxs-lookup"><span data-stu-id="9ace9-135">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="9ace9-136">Die Laufzeit verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="9ace9-136">The runtime to use.</span></span>         |
| <span data-ttu-id="9ace9-137">-h</span><span class="sxs-lookup"><span data-stu-id="9ace9-137">-h</span></span> | <span data-ttu-id="9ace9-138">– Hilfe</span><span class="sxs-lookup"><span data-stu-id="9ace9-138">--help</span></span>                           | <span data-ttu-id="9ace9-139">Anzeigen von Hilfeinformationen.</span><span class="sxs-lookup"><span data-stu-id="9ace9-139">Show help information.</span></span>      |
| <span data-ttu-id="9ace9-140">-v</span><span class="sxs-lookup"><span data-stu-id="9ace9-140">-v</span></span> | <span data-ttu-id="9ace9-141">-verbose</span><span class="sxs-lookup"><span data-stu-id="9ace9-141">--verbose</span></span>                        | <span data-ttu-id="9ace9-142">Zeigen Sie eine ausführlichen Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="9ace9-142">Show verbose output.</span></span>        |
|    | <span data-ttu-id="9ace9-143">--keine Farbe</span><span class="sxs-lookup"><span data-stu-id="9ace9-143">--no-color</span></span>                       | <span data-ttu-id="9ace9-144">Nicht zur farblichen Kennzeichnung der Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="9ace9-144">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="9ace9-145">--Präfix-Ausgabe</span><span class="sxs-lookup"><span data-stu-id="9ace9-145">--prefix-output</span></span>                  | <span data-ttu-id="9ace9-146">Präfix Ausgabe auf.</span><span class="sxs-lookup"><span data-stu-id="9ace9-146">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="9ace9-147">Legen Sie zum Angeben der ASP.NET Core-Umgebung die **ASPNETCORE_ENVIRONMENT** Umgebungsvariablen vor der Ausführung.</span><span class="sxs-lookup"><span data-stu-id="9ace9-147">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="9ace9-148">Befehle</span><span class="sxs-lookup"><span data-stu-id="9ace9-148">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="9ace9-149">Dotnet Ef das Löschen einer Datenbank</span><span class="sxs-lookup"><span data-stu-id="9ace9-149">dotnet ef database drop</span></span>

<span data-ttu-id="9ace9-150">Löscht die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="9ace9-150">Drops the database.</span></span>

<span data-ttu-id="9ace9-151">Optionen:</span><span class="sxs-lookup"><span data-stu-id="9ace9-151">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="9ace9-152">-f</span><span class="sxs-lookup"><span data-stu-id="9ace9-152">-f</span></span> | <span data-ttu-id="9ace9-153">--erzwingen</span><span class="sxs-lookup"><span data-stu-id="9ace9-153">--force</span></span>   | <span data-ttu-id="9ace9-154">Bestätigen Sie nicht.</span><span class="sxs-lookup"><span data-stu-id="9ace9-154">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="9ace9-155">--Probelauf</span><span class="sxs-lookup"><span data-stu-id="9ace9-155">--dry-run</span></span> | <span data-ttu-id="9ace9-156">Anzeigen, welche Datenbank verworfen werden, jedoch nicht, legen Sie sie.</span><span class="sxs-lookup"><span data-stu-id="9ace9-156">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="9ace9-157">Dotnet Ef-Datenbankupdate</span><span class="sxs-lookup"><span data-stu-id="9ace9-157">dotnet ef database update</span></span>

<span data-ttu-id="9ace9-158">Aktualisiert die Datenbank in eine angegebene Migration.</span><span class="sxs-lookup"><span data-stu-id="9ace9-158">Updates the database to a specified migration.</span></span>

<span data-ttu-id="9ace9-159">Argumente:</span><span class="sxs-lookup"><span data-stu-id="9ace9-159">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="9ace9-160">\<MIGRATION &GT;</span><span class="sxs-lookup"><span data-stu-id="9ace9-160">\<MIGRATION></span></span> | <span data-ttu-id="9ace9-161">Der zielmigration.</span><span class="sxs-lookup"><span data-stu-id="9ace9-161">The target migration.</span></span> <span data-ttu-id="9ace9-162">Bei 0 werden bei allen Migrationen rückgängig gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="9ace9-162">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="9ace9-163">Standardmäßig bis zum letzten Migration.</span><span class="sxs-lookup"><span data-stu-id="9ace9-163">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="9ace9-164">Dotnet Ef Dbcontext info</span><span class="sxs-lookup"><span data-stu-id="9ace9-164">dotnet ef dbcontext info</span></span>

<span data-ttu-id="9ace9-165">Ruft Informationen zu einem DbContext-Typ ab.</span><span class="sxs-lookup"><span data-stu-id="9ace9-165">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="9ace9-166">Dotnet Ef Dbcontext-Liste</span><span class="sxs-lookup"><span data-stu-id="9ace9-166">dotnet ef dbcontext list</span></span>

<span data-ttu-id="9ace9-167">Listet die verfügbare DbContext-Typen.</span><span class="sxs-lookup"><span data-stu-id="9ace9-167">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="9ace9-168">Dotnet Ef Dbcontext Gerüst</span><span class="sxs-lookup"><span data-stu-id="9ace9-168">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="9ace9-169">Gerüste ein ' DbContext ' und Entität Typen für eine Datenbank.</span><span class="sxs-lookup"><span data-stu-id="9ace9-169">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="9ace9-170">Argumente:</span><span class="sxs-lookup"><span data-stu-id="9ace9-170">Arguments:</span></span>

|               |                                                                     |
|:--------------|:--------------------------------------------------------------------|
| <span data-ttu-id="9ace9-171">\<VERBINDUNG &GT;</span><span class="sxs-lookup"><span data-stu-id="9ace9-171">\<CONNECTION></span></span> | <span data-ttu-id="9ace9-172">Die Verbindungszeichenfolge zur Datenbank.</span><span class="sxs-lookup"><span data-stu-id="9ace9-172">The connection string to the database.</span></span>                              |
| <span data-ttu-id="9ace9-173">\<ANBIETER &GT;</span><span class="sxs-lookup"><span data-stu-id="9ace9-173">\<PROVIDER></span></span>   | <span data-ttu-id="9ace9-174">Die zu verwendenden Anbieter an.</span><span class="sxs-lookup"><span data-stu-id="9ace9-174">The provider to use.</span></span> <span data-ttu-id="9ace9-175">(z. B.</span><span class="sxs-lookup"><span data-stu-id="9ace9-175">(E.g.</span></span> <span data-ttu-id="9ace9-176">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="9ace9-176">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="9ace9-177">Optionen:</span><span class="sxs-lookup"><span data-stu-id="9ace9-177">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9ace9-178"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="9ace9-178"><nobr>-d</nobr></span></span> | <span data-ttu-id="9ace9-179">--datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="9ace9-179">--data-annotations</span></span>                      | <span data-ttu-id="9ace9-180">Verwenden Sie Attribute, um das Modell (sofern möglich) konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="9ace9-180">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="9ace9-181">Wenn nicht angegeben, wird nur die fluent-API verwendet.</span><span class="sxs-lookup"><span data-stu-id="9ace9-181">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="9ace9-182">-c</span><span class="sxs-lookup"><span data-stu-id="9ace9-182">-c</span></span>              | <span data-ttu-id="9ace9-183">--Kontext \<NAME ></span><span class="sxs-lookup"><span data-stu-id="9ace9-183">--context \<NAME></span></span>                       | <span data-ttu-id="9ace9-184">Der Name von ' DbContext '.</span><span class="sxs-lookup"><span data-stu-id="9ace9-184">The name of the DbContext.</span></span>                                                                       |
|                 | <span data-ttu-id="9ace9-185">--Kontext Dir \<Pfad ></span><span class="sxs-lookup"><span data-stu-id="9ace9-185">--context-dir \<PATH></span></span>                   | <span data-ttu-id="9ace9-186">Das Verzeichnis DbContext-Datei in den versetzt.</span><span class="sxs-lookup"><span data-stu-id="9ace9-186">The directory to put DbContext file in.</span></span> <span data-ttu-id="9ace9-187">Pfade sind relativ zum Projektverzeichnis an.</span><span class="sxs-lookup"><span data-stu-id="9ace9-187">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="9ace9-188">-f</span><span class="sxs-lookup"><span data-stu-id="9ace9-188">-f</span></span>              | <span data-ttu-id="9ace9-189">--erzwingen</span><span class="sxs-lookup"><span data-stu-id="9ace9-189">--force</span></span>                                 | <span data-ttu-id="9ace9-190">Überschreiben Sie vorhandene Dateien.</span><span class="sxs-lookup"><span data-stu-id="9ace9-190">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="9ace9-191">-o</span><span class="sxs-lookup"><span data-stu-id="9ace9-191">-o</span></span>              | <span data-ttu-id="9ace9-192">--Ausgabe Dir \<Pfad ></span><span class="sxs-lookup"><span data-stu-id="9ace9-192">--output-dir \<PATH></span></span>                    | <span data-ttu-id="9ace9-193">Das Verzeichnis in den Dateien versetzt.</span><span class="sxs-lookup"><span data-stu-id="9ace9-193">The directory to put files in.</span></span> <span data-ttu-id="9ace9-194">Pfade sind relativ zum Projektverzeichnis an.</span><span class="sxs-lookup"><span data-stu-id="9ace9-194">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="9ace9-195"><nobr>--Schema \<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="9ace9-195"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="9ace9-196">Die Schemas der Tabellen zur Generierung von Entitätstypen für.</span><span class="sxs-lookup"><span data-stu-id="9ace9-196">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="9ace9-197">-t</span><span class="sxs-lookup"><span data-stu-id="9ace9-197">-t</span></span>              | <span data-ttu-id="9ace9-198">--Tabelle \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="9ace9-198">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="9ace9-199">Die Tabellen für Entitätstypen generieren.</span><span class="sxs-lookup"><span data-stu-id="9ace9-199">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="9ace9-200">– Verwenden der Datenbanknamen</span><span class="sxs-lookup"><span data-stu-id="9ace9-200">--use-database-names</span></span>                    | <span data-ttu-id="9ace9-201">Verwenden Sie die Tabellen- und Spaltennamen direkt aus der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="9ace9-201">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="9ace9-202">Dotnet Ef Migrationen hinzufügen</span><span class="sxs-lookup"><span data-stu-id="9ace9-202">dotnet ef migrations add</span></span>

<span data-ttu-id="9ace9-203">Fügt eine neue Migration.</span><span class="sxs-lookup"><span data-stu-id="9ace9-203">Adds a new migration.</span></span>

<span data-ttu-id="9ace9-204">Argumente:</span><span class="sxs-lookup"><span data-stu-id="9ace9-204">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="9ace9-205">\<NAME></span><span class="sxs-lookup"><span data-stu-id="9ace9-205">\<NAME></span></span> | <span data-ttu-id="9ace9-206">Der Name der Migration.</span><span class="sxs-lookup"><span data-stu-id="9ace9-206">The name of the migration.</span></span> |

<span data-ttu-id="9ace9-207">Optionen:</span><span class="sxs-lookup"><span data-stu-id="9ace9-207">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9ace9-208"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="9ace9-208"><nobr>-o</nobr></span></span> | <span data-ttu-id="9ace9-209"><nobr>--Ausgabe Dir \<Pfad ></nobr></span><span class="sxs-lookup"><span data-stu-id="9ace9-209"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="9ace9-210">Das Verzeichnis (und Sub-Namespace) zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="9ace9-210">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="9ace9-211">Pfade sind relativ zum Projektverzeichnis an.</span><span class="sxs-lookup"><span data-stu-id="9ace9-211">Paths are relative to the project directory.</span></span> <span data-ttu-id="9ace9-212">Der Standardwert ist "Migration".</span><span class="sxs-lookup"><span data-stu-id="9ace9-212">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="9ace9-213">Dotnet Ef Migrationen Liste</span><span class="sxs-lookup"><span data-stu-id="9ace9-213">dotnet ef migrations list</span></span>

<span data-ttu-id="9ace9-214">Listet die verfügbaren Migrationen.</span><span class="sxs-lookup"><span data-stu-id="9ace9-214">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="9ace9-215">Entfernen Sie Dotnet Ef Migrationen</span><span class="sxs-lookup"><span data-stu-id="9ace9-215">dotnet ef migrations remove</span></span>

<span data-ttu-id="9ace9-216">Entfernt die letzte Migration.</span><span class="sxs-lookup"><span data-stu-id="9ace9-216">Removes the last migration.</span></span>

<span data-ttu-id="9ace9-217">Optionen:</span><span class="sxs-lookup"><span data-stu-id="9ace9-217">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="9ace9-218">-f</span><span class="sxs-lookup"><span data-stu-id="9ace9-218">-f</span></span> | <span data-ttu-id="9ace9-219">--erzwingen</span><span class="sxs-lookup"><span data-stu-id="9ace9-219">--force</span></span> | <span data-ttu-id="9ace9-220">Die Migration zurückgesetzt werden, wenn es auf die Datenbank angewendet wurde.</span><span class="sxs-lookup"><span data-stu-id="9ace9-220">Revert the migration if it has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="9ace9-221">Dotnet Ef Migrationen Skript</span><span class="sxs-lookup"><span data-stu-id="9ace9-221">dotnet ef migrations script</span></span>

<span data-ttu-id="9ace9-222">Generiert ein SQL-Skript von Migrationen.</span><span class="sxs-lookup"><span data-stu-id="9ace9-222">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="9ace9-223">Argumente:</span><span class="sxs-lookup"><span data-stu-id="9ace9-223">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="9ace9-224">\<FROM></span><span class="sxs-lookup"><span data-stu-id="9ace9-224">\<FROM></span></span> | <span data-ttu-id="9ace9-225">Der Start Migration.</span><span class="sxs-lookup"><span data-stu-id="9ace9-225">The starting migration.</span></span> <span data-ttu-id="9ace9-226">Der Standardwert ist 0 (die ursprüngliche Datenbank).</span><span class="sxs-lookup"><span data-stu-id="9ace9-226">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="9ace9-227">\<TO></span><span class="sxs-lookup"><span data-stu-id="9ace9-227">\<TO></span></span>   | <span data-ttu-id="9ace9-228">Der Endwert Migration.</span><span class="sxs-lookup"><span data-stu-id="9ace9-228">The ending migration.</span></span> <span data-ttu-id="9ace9-229">Standardmäßig bis zum letzten Migration.</span><span class="sxs-lookup"><span data-stu-id="9ace9-229">Defaults to the last migration.</span></span>         |

<span data-ttu-id="9ace9-230">Optionen:</span><span class="sxs-lookup"><span data-stu-id="9ace9-230">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="9ace9-231">-o</span><span class="sxs-lookup"><span data-stu-id="9ace9-231">-o</span></span> | <span data-ttu-id="9ace9-232">--Ausgabe \<Datei ></span><span class="sxs-lookup"><span data-stu-id="9ace9-232">--output \<FILE></span></span> | <span data-ttu-id="9ace9-233">Die Datei, schreibt das Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="9ace9-233">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="9ace9-234">-i</span><span class="sxs-lookup"><span data-stu-id="9ace9-234">-i</span></span> | <span data-ttu-id="9ace9-235">--idempotent</span><span class="sxs-lookup"><span data-stu-id="9ace9-235">--idempotent</span></span>     | <span data-ttu-id="9ace9-236">Generiert ein Skript, das für eine Datenbank bei jeder Migration verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="9ace9-236">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
