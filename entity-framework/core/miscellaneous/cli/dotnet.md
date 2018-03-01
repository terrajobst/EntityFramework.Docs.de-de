---
title: ".NET Core CLI – EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 8a52cb8259bb381729a33a8161aec4b73f69f45d
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="68893-102">EF Core .NET-Befehlszeilentools</span><span class="sxs-lookup"><span data-stu-id="68893-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="68893-103">Das Entity Framework Core .NET Befehlszeilentools sind eine Erweiterung für die plattformübergreifende **Dotnet** Befehl, die Teil von der [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="68893-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="68893-104">Wenn Sie Visual Studio verwenden, empfiehlt es sich [PMC Tools] [ 1] seitdem sie stattdessen eine weitere integrierte Lösung bieten.</span><span class="sxs-lookup"><span data-stu-id="68893-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="68893-105">Installieren der Tools</span><span class="sxs-lookup"><span data-stu-id="68893-105">Installing the tools</span></span>
--------------------
<span data-ttu-id="68893-106">Installieren Sie die EF Core .NET-Befehlszeilentools mithilfe der folgenden Schritte:</span><span class="sxs-lookup"><span data-stu-id="68893-106">Install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="68893-107">Bearbeiten der Projektdatei, und fügen Sie Microsoft.EntityFrameworkCore.Tools.DotNet als DotNetCliToolReference-Element (siehe unten)</span><span class="sxs-lookup"><span data-stu-id="68893-107">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="68893-108">Führen Sie die folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="68893-108">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore


<span data-ttu-id="68893-109">Das daraus resultierende Projekt sollte etwa wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="68893-109">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="68893-110">Ein Verweis Paket mit `PrivateAssets="All"` bedeutet, er ist nicht verfügbar gemacht, um Projekte, die auf dieses Projekt verweisen, insbesondere für die Pakete, die in der Regel nur während der Entwicklung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="68893-110">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="68893-111">Wenn Sie alles richtig haben, sollten Sie den folgenden Befehl an einer Eingabeaufforderung erfolgreich ausführen können.</span><span class="sxs-lookup"><span data-stu-id="68893-111">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="68893-112">Mithilfe der tools</span><span class="sxs-lookup"><span data-stu-id="68893-112">Using the tools</span></span>
---------------
<span data-ttu-id="68893-113">Bei jedem eines Befehls aufrufen umfasst zwei Projekte:</span><span class="sxs-lookup"><span data-stu-id="68893-113">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="68893-114">Dem Zielprojekt werden Dateien hinzugefügt (oder sie werden in einigen Fällen aus diesem entfernt).</span><span class="sxs-lookup"><span data-stu-id="68893-114">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="68893-115">Das Zielprojekt wird standardmäßig auf das Projekt im aktuellen Verzeichnis, jedoch können geändert werden, mithilfe der <nobr> **– Projekt** </nobr> Option.</span><span class="sxs-lookup"><span data-stu-id="68893-115">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="68893-116">Das Startprojekt wird bei Ausführung des Projektcodes von den Tools emuliert.</span><span class="sxs-lookup"><span data-stu-id="68893-116">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="68893-117">Auch wird standardmäßig auf das Projekt im aktuellen Verzeichnis, aber kann geändert werden, mithilfe der **--Startprojekt** Option.</span><span class="sxs-lookup"><span data-stu-id="68893-117">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

<span data-ttu-id="68893-118">Allgemeine Optionen:</span><span class="sxs-lookup"><span data-stu-id="68893-118">Common options:</span></span>

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | <span data-ttu-id="68893-119">--json</span><span class="sxs-lookup"><span data-stu-id="68893-119">--json</span></span>                           | <span data-ttu-id="68893-120">JSON-Ausgabe anzeigen.</span><span class="sxs-lookup"><span data-stu-id="68893-120">Show JSON output.</span></span>           |
| <span data-ttu-id="68893-121">-c</span><span class="sxs-lookup"><span data-stu-id="68893-121">-c</span></span> | <span data-ttu-id="68893-122">--Kontext \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="68893-122">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="68893-123">Die DbContext verwenden.</span><span class="sxs-lookup"><span data-stu-id="68893-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="68893-124">-p</span><span class="sxs-lookup"><span data-stu-id="68893-124">-p</span></span> | <span data-ttu-id="68893-125">--Projekt \<Projekt ></span><span class="sxs-lookup"><span data-stu-id="68893-125">--project \<PROJECT></span></span>             | <span data-ttu-id="68893-126">Das Projekt verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="68893-126">The project to use.</span></span>         |
| <span data-ttu-id="68893-127">-s</span><span class="sxs-lookup"><span data-stu-id="68893-127">-s</span></span> | <span data-ttu-id="68893-128">--Startprojekt \<Projekt ></span><span class="sxs-lookup"><span data-stu-id="68893-128">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="68893-129">Das Startup-Projekt verwenden.</span><span class="sxs-lookup"><span data-stu-id="68893-129">The startup project to use.</span></span> |
|    | <span data-ttu-id="68893-130">--Framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="68893-130">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="68893-131">Das Zielframework.</span><span class="sxs-lookup"><span data-stu-id="68893-131">The target framework.</span></span>       |
|    | <span data-ttu-id="68893-132">--Konfiguration \<CONFIGURATION ></span><span class="sxs-lookup"><span data-stu-id="68893-132">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="68893-133">Die zu verwendende Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="68893-133">The configuration to use.</span></span>   |
|    | <span data-ttu-id="68893-134">--Runtime \<BEZEICHNER ></span><span class="sxs-lookup"><span data-stu-id="68893-134">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="68893-135">Die Laufzeit verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="68893-135">The runtime to use.</span></span>         |
| <span data-ttu-id="68893-136">-h</span><span class="sxs-lookup"><span data-stu-id="68893-136">-h</span></span> | <span data-ttu-id="68893-137">– Hilfe</span><span class="sxs-lookup"><span data-stu-id="68893-137">--help</span></span>                           | <span data-ttu-id="68893-138">Anzeigen von Hilfeinformationen.</span><span class="sxs-lookup"><span data-stu-id="68893-138">Show help information.</span></span>      |
| <span data-ttu-id="68893-139">-v</span><span class="sxs-lookup"><span data-stu-id="68893-139">-v</span></span> | <span data-ttu-id="68893-140">-verbose</span><span class="sxs-lookup"><span data-stu-id="68893-140">--verbose</span></span>                        | <span data-ttu-id="68893-141">Zeigen Sie eine ausführlichen Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="68893-141">Show verbose output.</span></span>        |
|    | <span data-ttu-id="68893-142">--no-color</span><span class="sxs-lookup"><span data-stu-id="68893-142">--no-color</span></span>                       | <span data-ttu-id="68893-143">Nicht zur farblichen Kennzeichnung der Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="68893-143">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="68893-144">--prefix-output</span><span class="sxs-lookup"><span data-stu-id="68893-144">--prefix-output</span></span>                  | <span data-ttu-id="68893-145">Präfix Ausgabe auf.</span><span class="sxs-lookup"><span data-stu-id="68893-145">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="68893-146">Legen Sie zum Angeben der ASP.NET Core-Umgebung die **ASPNETCORE_ENVIRONMENT** Umgebungsvariablen vor der Ausführung.</span><span class="sxs-lookup"><span data-stu-id="68893-146">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="68893-147">Befehle</span><span class="sxs-lookup"><span data-stu-id="68893-147">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="68893-148">dotnet ef database drop</span><span class="sxs-lookup"><span data-stu-id="68893-148">dotnet ef database drop</span></span>

<span data-ttu-id="68893-149">Löscht die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="68893-149">Drops the database.</span></span>

<span data-ttu-id="68893-150">Optionen:</span><span class="sxs-lookup"><span data-stu-id="68893-150">Options:</span></span>

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| <span data-ttu-id="68893-151">-f</span><span class="sxs-lookup"><span data-stu-id="68893-151">-f</span></span> | <span data-ttu-id="68893-152">--erzwingen</span><span class="sxs-lookup"><span data-stu-id="68893-152">--force</span></span>   | <span data-ttu-id="68893-153">Bestätigen Sie nicht.</span><span class="sxs-lookup"><span data-stu-id="68893-153">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="68893-154">--Probelauf</span><span class="sxs-lookup"><span data-stu-id="68893-154">--dry-run</span></span> | <span data-ttu-id="68893-155">Anzeigen, welche Datenbank verworfen werden, jedoch nicht, legen Sie sie.</span><span class="sxs-lookup"><span data-stu-id="68893-155">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="68893-156">Dotnet Ef-Datenbankupdate</span><span class="sxs-lookup"><span data-stu-id="68893-156">dotnet ef database update</span></span>

<span data-ttu-id="68893-157">Aktualisiert die Datenbank in eine angegebene Migration.</span><span class="sxs-lookup"><span data-stu-id="68893-157">Updates the database to a specified migration.</span></span>

<span data-ttu-id="68893-158">Argumente:</span><span class="sxs-lookup"><span data-stu-id="68893-158">Arguments:</span></span>

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| <span data-ttu-id="68893-159">\<MIGRATION></span><span class="sxs-lookup"><span data-stu-id="68893-159">\<MIGRATION></span></span> | <span data-ttu-id="68893-160">Der zielmigration.</span><span class="sxs-lookup"><span data-stu-id="68893-160">The target migration.</span></span> <span data-ttu-id="68893-161">Bei 0 werden bei allen Migrationen rückgängig gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="68893-161">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="68893-162">Standardmäßig bis zum letzten Migration.</span><span class="sxs-lookup"><span data-stu-id="68893-162">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="68893-163">Dotnet Ef Dbcontext info</span><span class="sxs-lookup"><span data-stu-id="68893-163">dotnet ef dbcontext info</span></span>

<span data-ttu-id="68893-164">Ruft Informationen zu einem DbContext-Typ ab.</span><span class="sxs-lookup"><span data-stu-id="68893-164">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="68893-165">dotnet ef dbcontext list</span><span class="sxs-lookup"><span data-stu-id="68893-165">dotnet ef dbcontext list</span></span>

<span data-ttu-id="68893-166">Listet die verfügbare DbContext-Typen.</span><span class="sxs-lookup"><span data-stu-id="68893-166">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="68893-167">dotnet ef dbcontext scaffold</span><span class="sxs-lookup"><span data-stu-id="68893-167">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="68893-168">Gerüste ein ' DbContext ' und Entität Typen für eine Datenbank.</span><span class="sxs-lookup"><span data-stu-id="68893-168">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="68893-169">Argumente:</span><span class="sxs-lookup"><span data-stu-id="68893-169">Arguments:</span></span>

|               |                                                                     |
|:--------------|:--------------------------------------------------------------------|
| <span data-ttu-id="68893-170">\<VERBINDUNG ></span><span class="sxs-lookup"><span data-stu-id="68893-170">\<CONNECTION></span></span> | <span data-ttu-id="68893-171">Die Verbindungszeichenfolge zur Datenbank.</span><span class="sxs-lookup"><span data-stu-id="68893-171">The connection string to the database.</span></span>                              |
| <span data-ttu-id="68893-172">\<ANBIETER ></span><span class="sxs-lookup"><span data-stu-id="68893-172">\<PROVIDER></span></span>   | <span data-ttu-id="68893-173">Die zu verwendenden Anbieter an.</span><span class="sxs-lookup"><span data-stu-id="68893-173">The provider to use.</span></span> <span data-ttu-id="68893-174">(Z. B.</span><span class="sxs-lookup"><span data-stu-id="68893-174">(E.g.</span></span> <span data-ttu-id="68893-175">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="68893-175">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="68893-176">Optionen:</span><span class="sxs-lookup"><span data-stu-id="68893-176">Options:</span></span>

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="68893-177"><nobr>-d</nobr></span><span class="sxs-lookup"><span data-stu-id="68893-177"><nobr>-d</nobr></span></span> | <span data-ttu-id="68893-178">--datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="68893-178">--data-annotations</span></span>                      | <span data-ttu-id="68893-179">Verwenden Sie Attribute, um das Modell (sofern möglich) konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="68893-179">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="68893-180">Wenn nicht angegeben, wird nur die fluent-API verwendet.</span><span class="sxs-lookup"><span data-stu-id="68893-180">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="68893-181">-c</span><span class="sxs-lookup"><span data-stu-id="68893-181">-c</span></span>              | <span data-ttu-id="68893-182">--context \<NAME></span><span class="sxs-lookup"><span data-stu-id="68893-182">--context \<NAME></span></span>                       | <span data-ttu-id="68893-183">Der Name von ' DbContext '.</span><span class="sxs-lookup"><span data-stu-id="68893-183">The name of the DbContext.</span></span>                                                                       |
| <span data-ttu-id="68893-184">-f</span><span class="sxs-lookup"><span data-stu-id="68893-184">-f</span></span>              | <span data-ttu-id="68893-185">--erzwingen</span><span class="sxs-lookup"><span data-stu-id="68893-185">--force</span></span>                                 | <span data-ttu-id="68893-186">Überschreiben Sie vorhandene Dateien.</span><span class="sxs-lookup"><span data-stu-id="68893-186">Overwrite existing files.</span></span>                                                                        |
| <span data-ttu-id="68893-187">-o</span><span class="sxs-lookup"><span data-stu-id="68893-187">-o</span></span>              | <span data-ttu-id="68893-188">--Ausgabe Dir \<Pfad ></span><span class="sxs-lookup"><span data-stu-id="68893-188">--output-dir \<PATH></span></span>                    | <span data-ttu-id="68893-189">Das Verzeichnis in den Dateien versetzt.</span><span class="sxs-lookup"><span data-stu-id="68893-189">The directory to put files in.</span></span> <span data-ttu-id="68893-190">Pfade sind relativ zum Projektverzeichnis an.</span><span class="sxs-lookup"><span data-stu-id="68893-190">Paths are relative to the project directory.</span></span>                      |
|                 | <span data-ttu-id="68893-191"><nobr>--schema \<SCHEMA_NAME>...</nobr></span><span class="sxs-lookup"><span data-stu-id="68893-191"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="68893-192">Die Schemas der Tabellen zur Generierung von Entitätstypen für.</span><span class="sxs-lookup"><span data-stu-id="68893-192">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="68893-193">-t</span><span class="sxs-lookup"><span data-stu-id="68893-193">-t</span></span>              | <span data-ttu-id="68893-194">--Tabelle \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="68893-194">--table \<TABLE_NAME>...</span></span>                | <span data-ttu-id="68893-195">Die Tabellen für Entitätstypen generieren.</span><span class="sxs-lookup"><span data-stu-id="68893-195">The tables to generate entity types for.</span></span>                                                         |
|                 | <span data-ttu-id="68893-196">--use-database-names</span><span class="sxs-lookup"><span data-stu-id="68893-196">--use-database-names</span></span>                    | <span data-ttu-id="68893-197">Verwenden Sie die Tabellen- und Spaltennamen direkt aus der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="68893-197">Use table and column names directly from the database.</span></span>                                           |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="68893-198">dotnet ef migrations add</span><span class="sxs-lookup"><span data-stu-id="68893-198">dotnet ef migrations add</span></span>

<span data-ttu-id="68893-199">Fügt eine neue Migration.</span><span class="sxs-lookup"><span data-stu-id="68893-199">Adds a new migration.</span></span>

<span data-ttu-id="68893-200">Argumente:</span><span class="sxs-lookup"><span data-stu-id="68893-200">Arguments:</span></span>

|         |                            |
|:--------|:---------------------------|
| <span data-ttu-id="68893-201">\<NAME></span><span class="sxs-lookup"><span data-stu-id="68893-201">\<NAME></span></span> | <span data-ttu-id="68893-202">Der Name der Migration.</span><span class="sxs-lookup"><span data-stu-id="68893-202">The name of the migration.</span></span> |

<span data-ttu-id="68893-203">Optionen:</span><span class="sxs-lookup"><span data-stu-id="68893-203">Options:</span></span>

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="68893-204"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="68893-204"><nobr>-o</nobr></span></span> | <span data-ttu-id="68893-205"><nobr>--Ausgabe Dir \<Pfad ></nobr></span><span class="sxs-lookup"><span data-stu-id="68893-205"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="68893-206">Das Verzeichnis (und Sub-Namespace) zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="68893-206">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="68893-207">Pfade sind relativ zum Projektverzeichnis an.</span><span class="sxs-lookup"><span data-stu-id="68893-207">Paths are relative to the project directory.</span></span> <span data-ttu-id="68893-208">Der Standardwert ist "Migration".</span><span class="sxs-lookup"><span data-stu-id="68893-208">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="68893-209">dotnet ef migrations list</span><span class="sxs-lookup"><span data-stu-id="68893-209">dotnet ef migrations list</span></span>

<span data-ttu-id="68893-210">Listet die verfügbaren Migrationen.</span><span class="sxs-lookup"><span data-stu-id="68893-210">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="68893-211">dotnet ef migrations remove</span><span class="sxs-lookup"><span data-stu-id="68893-211">dotnet ef migrations remove</span></span>

<span data-ttu-id="68893-212">Entfernt die letzte Migration.</span><span class="sxs-lookup"><span data-stu-id="68893-212">Removes the last migration.</span></span>

<span data-ttu-id="68893-213">Optionen:</span><span class="sxs-lookup"><span data-stu-id="68893-213">Options:</span></span>

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| <span data-ttu-id="68893-214">-f</span><span class="sxs-lookup"><span data-stu-id="68893-214">-f</span></span> | <span data-ttu-id="68893-215">--erzwingen</span><span class="sxs-lookup"><span data-stu-id="68893-215">--force</span></span> | <span data-ttu-id="68893-216">Überprüfen Sie nicht, um festzustellen, ob die Migration der Datenbank angewendet wurde.</span><span class="sxs-lookup"><span data-stu-id="68893-216">Don't check to see if the migration has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="68893-217">dotnet ef migrations script</span><span class="sxs-lookup"><span data-stu-id="68893-217">dotnet ef migrations script</span></span>

<span data-ttu-id="68893-218">Generiert ein SQL-Skript von Migrationen.</span><span class="sxs-lookup"><span data-stu-id="68893-218">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="68893-219">Argumente:</span><span class="sxs-lookup"><span data-stu-id="68893-219">Arguments:</span></span>

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| <span data-ttu-id="68893-220">\<FROM></span><span class="sxs-lookup"><span data-stu-id="68893-220">\<FROM></span></span> | <span data-ttu-id="68893-221">Der Start Migration.</span><span class="sxs-lookup"><span data-stu-id="68893-221">The starting migration.</span></span> <span data-ttu-id="68893-222">Der Standardwert ist 0 (die ursprüngliche Datenbank).</span><span class="sxs-lookup"><span data-stu-id="68893-222">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="68893-223">\<TO></span><span class="sxs-lookup"><span data-stu-id="68893-223">\<TO></span></span>   | <span data-ttu-id="68893-224">Der Endwert Migration.</span><span class="sxs-lookup"><span data-stu-id="68893-224">The ending migration.</span></span> <span data-ttu-id="68893-225">Standardmäßig bis zum letzten Migration.</span><span class="sxs-lookup"><span data-stu-id="68893-225">Defaults to the last migration.</span></span>         |

<span data-ttu-id="68893-226">Optionen:</span><span class="sxs-lookup"><span data-stu-id="68893-226">Options:</span></span>

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| <span data-ttu-id="68893-227">-o</span><span class="sxs-lookup"><span data-stu-id="68893-227">-o</span></span> | <span data-ttu-id="68893-228">--Ausgabe \<Datei ></span><span class="sxs-lookup"><span data-stu-id="68893-228">--output \<FILE></span></span> | <span data-ttu-id="68893-229">Die Datei, schreibt das Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="68893-229">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="68893-230">-i</span><span class="sxs-lookup"><span data-stu-id="68893-230">-i</span></span> | <span data-ttu-id="68893-231">--idempotent</span><span class="sxs-lookup"><span data-stu-id="68893-231">--idempotent</span></span>     | <span data-ttu-id="68893-232">Generiert ein Skript, das für eine Datenbank bei jeder Migration verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="68893-232">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
