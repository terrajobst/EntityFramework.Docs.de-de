---
title: ".NET Core CLI – EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 26b5fb326d20575ed2f3c6955c699e0c3757bf57
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
<a name="ef-core-net-command-line-tools"></a><span data-ttu-id="dcb53-102">EF Core .NET-Befehlszeilentools</span><span class="sxs-lookup"><span data-stu-id="dcb53-102">EF Core .NET Command-line Tools</span></span>
===============================
<span data-ttu-id="dcb53-103">Das Entity Framework Core .NET Befehlszeilentools sind eine Erweiterung für die plattformübergreifende **Dotnet** Befehl, die Teil von der [.NET Core SDK][2].</span><span class="sxs-lookup"><span data-stu-id="dcb53-103">The Entity Framework Core .NET Command-line Tools are an extension to the cross-platform **dotnet** command, which is part of the [.NET Core SDK][2].</span></span>

> [!TIP]
> <span data-ttu-id="dcb53-104">Wenn Sie Visual Studio verwenden, empfiehlt es sich [PMC Tools] [ 1] seitdem sie stattdessen eine weitere integrierte Lösung bieten.</span><span class="sxs-lookup"><span data-stu-id="dcb53-104">If you're using Visual Studio, we recommend [the PMC Tools][1] instead since they provide a more integrated experience.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="dcb53-105">Installieren der Tools</span><span class="sxs-lookup"><span data-stu-id="dcb53-105">Installing the tools</span></span>
--------------------
<span data-ttu-id="dcb53-106">Installieren Sie die EF Core .NET-Befehlszeilentools mithilfe der folgenden Schritte:</span><span class="sxs-lookup"><span data-stu-id="dcb53-106">Install the EF Core .NET Command-line Tools using these steps:</span></span>

1. <span data-ttu-id="dcb53-107">Bearbeiten der Projektdatei, und fügen Sie Microsoft.EntityFrameworkCore.Tools.DotNet als DotNetCliToolReference-Element (siehe unten)</span><span class="sxs-lookup"><span data-stu-id="dcb53-107">Edit the project file and add Microsoft.EntityFrameworkCore.Tools.DotNet as a DotNetCliToolReference item (See below)</span></span>
2. <span data-ttu-id="dcb53-108">Führen Sie die folgenden Befehle ein:</span><span class="sxs-lookup"><span data-stu-id="dcb53-108">Run the following commands:</span></span>

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore


<span data-ttu-id="dcb53-109">Das daraus resultierende Projekt sollte etwa wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="dcb53-109">The resulting project should look something like this:</span></span>

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
> <span data-ttu-id="dcb53-110">Ein Verweis Paket mit `PrivateAssets="All"` bedeutet, er ist nicht verfügbar gemacht, um Projekte, die auf dieses Projekt verweisen, insbesondere für die Pakete, die in der Regel nur während der Entwicklung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="dcb53-110">A package reference with `PrivateAssets="All"` means it isn't exposed to projects that reference this project, which is especially useful for packages that are typically only used during development.</span></span>

<span data-ttu-id="dcb53-111">Wenn Sie alles richtig haben, sollten Sie den folgenden Befehl an einer Eingabeaufforderung erfolgreich ausführen können.</span><span class="sxs-lookup"><span data-stu-id="dcb53-111">If you did everything right, you should be able to successfully run the following command in a command prompt.</span></span>

``` Console
dotnet ef
```

<a name="using-the-tools"></a><span data-ttu-id="dcb53-112">Mithilfe der tools</span><span class="sxs-lookup"><span data-stu-id="dcb53-112">Using the tools</span></span>
---------------
<span data-ttu-id="dcb53-113">Bei jedem eines Befehls aufrufen umfasst zwei Projekte:</span><span class="sxs-lookup"><span data-stu-id="dcb53-113">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="dcb53-114">Das Zielprojekt ist keine Dateien hinzugefügt werden (oder in einigen Fällen entfernt).</span><span class="sxs-lookup"><span data-stu-id="dcb53-114">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="dcb53-115">Das Zielprojekt wird standardmäßig auf das Projekt im aktuellen Verzeichnis, jedoch können geändert werden, mithilfe der <nobr> **– Projekt** </nobr> Option.</span><span class="sxs-lookup"><span data-stu-id="dcb53-115">The target project defaults to the project in the current directory, but can be changed using the <nobr>**--project**</nobr> option.</span></span>

<span data-ttu-id="dcb53-116">Das Startprojekt wird von den Tools emuliert wird, wenn der Code des Projekts ausführen.</span><span class="sxs-lookup"><span data-stu-id="dcb53-116">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="dcb53-117">Auch wird standardmäßig auf das Projekt im aktuellen Verzeichnis, aber kann geändert werden, mithilfe der **--Startprojekt** Option.</span><span class="sxs-lookup"><span data-stu-id="dcb53-117">It also defaults to the project in the current directory, but can be changed using the **--startup-project** option.</span></span>

<span data-ttu-id="dcb53-118">Allgemeine Optionen:</span><span class="sxs-lookup"><span data-stu-id="dcb53-118">Common options:</span></span>

|    |                                  |                             |
| -- | -------------------------------- | --------------------------- |
|    | <span data-ttu-id="dcb53-119">--json</span><span class="sxs-lookup"><span data-stu-id="dcb53-119">--json</span></span>                           | <span data-ttu-id="dcb53-120">JSON-Ausgabe anzeigen.</span><span class="sxs-lookup"><span data-stu-id="dcb53-120">Show JSON output.</span></span>           |
| <span data-ttu-id="dcb53-121">-c</span><span class="sxs-lookup"><span data-stu-id="dcb53-121">-c</span></span> | <span data-ttu-id="dcb53-122">--Kontext \<DBCONTEXT ></span><span class="sxs-lookup"><span data-stu-id="dcb53-122">--context \<DBCONTEXT></span></span>           | <span data-ttu-id="dcb53-123">Die DbContext verwenden.</span><span class="sxs-lookup"><span data-stu-id="dcb53-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="dcb53-124">-P</span><span class="sxs-lookup"><span data-stu-id="dcb53-124">-p</span></span> | <span data-ttu-id="dcb53-125">--Projekt \<Projekt ></span><span class="sxs-lookup"><span data-stu-id="dcb53-125">--project \<PROJECT></span></span>             | <span data-ttu-id="dcb53-126">Das Projekt verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="dcb53-126">The project to use.</span></span>         |
| <span data-ttu-id="dcb53-127">-s</span><span class="sxs-lookup"><span data-stu-id="dcb53-127">-s</span></span> | <span data-ttu-id="dcb53-128">--Startprojekt \<Projekt ></span><span class="sxs-lookup"><span data-stu-id="dcb53-128">--startup-project \<PROJECT></span></span>     | <span data-ttu-id="dcb53-129">Das Startup-Projekt verwenden.</span><span class="sxs-lookup"><span data-stu-id="dcb53-129">The startup project to use.</span></span> |
|    | <span data-ttu-id="dcb53-130">--Framework \<FRAMEWORK ></span><span class="sxs-lookup"><span data-stu-id="dcb53-130">--framework \<FRAMEWORK></span></span>         | <span data-ttu-id="dcb53-131">Das Zielframework.</span><span class="sxs-lookup"><span data-stu-id="dcb53-131">The target framework.</span></span>       |
|    | <span data-ttu-id="dcb53-132">--Konfiguration \<CONFIGURATION ></span><span class="sxs-lookup"><span data-stu-id="dcb53-132">--configuration \<CONFIGURATION></span></span> | <span data-ttu-id="dcb53-133">Die zu verwendende Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="dcb53-133">The configuration to use.</span></span>   |
|    | <span data-ttu-id="dcb53-134">--Runtime \<BEZEICHNER ></span><span class="sxs-lookup"><span data-stu-id="dcb53-134">--runtime \<IDENTIFIER></span></span>          | <span data-ttu-id="dcb53-135">Die Laufzeit verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="dcb53-135">The runtime to use.</span></span>         |
| <span data-ttu-id="dcb53-136">-h</span><span class="sxs-lookup"><span data-stu-id="dcb53-136">-h</span></span> | <span data-ttu-id="dcb53-137">– Hilfe</span><span class="sxs-lookup"><span data-stu-id="dcb53-137">--help</span></span>                           | <span data-ttu-id="dcb53-138">Anzeigen von Hilfeinformationen.</span><span class="sxs-lookup"><span data-stu-id="dcb53-138">Show help information.</span></span>      |
| <span data-ttu-id="dcb53-139">-v</span><span class="sxs-lookup"><span data-stu-id="dcb53-139">-v</span></span> | <span data-ttu-id="dcb53-140">-verbose</span><span class="sxs-lookup"><span data-stu-id="dcb53-140">--verbose</span></span>                        | <span data-ttu-id="dcb53-141">Zeigen Sie eine ausführlichen Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="dcb53-141">Show verbose output.</span></span>        |
|    | <span data-ttu-id="dcb53-142">--keine Farbe</span><span class="sxs-lookup"><span data-stu-id="dcb53-142">--no-color</span></span>                       | <span data-ttu-id="dcb53-143">Nicht zur farblichen Kennzeichnung der Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="dcb53-143">Don't colorize output.</span></span>      |
|    | <span data-ttu-id="dcb53-144">--Präfix-Ausgabe</span><span class="sxs-lookup"><span data-stu-id="dcb53-144">--prefix-output</span></span>                  | <span data-ttu-id="dcb53-145">Präfix Ausgabe auf.</span><span class="sxs-lookup"><span data-stu-id="dcb53-145">Prefix output with level.</span></span>   |


> [!TIP]
> <span data-ttu-id="dcb53-146">Legen Sie zum Angeben der ASP.NET Core-Umgebung die **ASPNETCORE_ENVIRONMENT** Umgebungsvariablen vor der Ausführung.</span><span class="sxs-lookup"><span data-stu-id="dcb53-146">To specify the ASP.NET Core environment, set the **ASPNETCORE_ENVIRONMENT** environment variable before running.</span></span>

<a name="commands"></a><span data-ttu-id="dcb53-147">Befehle</span><span class="sxs-lookup"><span data-stu-id="dcb53-147">Commands</span></span>
--------

### <a name="dotnet-ef-database-drop"></a><span data-ttu-id="dcb53-148">Dotnet Ef das Löschen einer Datenbank</span><span class="sxs-lookup"><span data-stu-id="dcb53-148">dotnet ef database drop</span></span>

<span data-ttu-id="dcb53-149">Löscht die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="dcb53-149">Drops the database.</span></span>

<span data-ttu-id="dcb53-150">Optionen:</span><span class="sxs-lookup"><span data-stu-id="dcb53-150">Options:</span></span>

|    |           |                                                          |
| -- | --------- | -------------------------------------------------------- |
| <span data-ttu-id="dcb53-151">-f</span><span class="sxs-lookup"><span data-stu-id="dcb53-151">-f</span></span> | <span data-ttu-id="dcb53-152">--erzwingen</span><span class="sxs-lookup"><span data-stu-id="dcb53-152">--force</span></span>   | <span data-ttu-id="dcb53-153">Bestätigen Sie nicht.</span><span class="sxs-lookup"><span data-stu-id="dcb53-153">Don't confirm.</span></span>                                           |
|    | <span data-ttu-id="dcb53-154">--Probelauf</span><span class="sxs-lookup"><span data-stu-id="dcb53-154">--dry-run</span></span> | <span data-ttu-id="dcb53-155">Anzeigen, welche Datenbank verworfen werden, jedoch nicht, legen Sie sie.</span><span class="sxs-lookup"><span data-stu-id="dcb53-155">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="dotnet-ef-database-update"></a><span data-ttu-id="dcb53-156">Dotnet Ef-Datenbankupdate</span><span class="sxs-lookup"><span data-stu-id="dcb53-156">dotnet ef database update</span></span>

<span data-ttu-id="dcb53-157">Aktualisiert die Datenbank in eine angegebene Migration.</span><span class="sxs-lookup"><span data-stu-id="dcb53-157">Updates the database to a specified migration.</span></span>

<span data-ttu-id="dcb53-158">Argumente:</span><span class="sxs-lookup"><span data-stu-id="dcb53-158">Arguments:</span></span>

|              |                                                                                              |
| ------------ | ---------------------------------------------------------------------------------------------|
| <span data-ttu-id="dcb53-159">\<MIGRATION ></span><span class="sxs-lookup"><span data-stu-id="dcb53-159">\<MIGRATION></span></span> | <span data-ttu-id="dcb53-160">Der zielmigration.</span><span class="sxs-lookup"><span data-stu-id="dcb53-160">The target migration.</span></span> <span data-ttu-id="dcb53-161">Bei 0 werden bei allen Migrationen rückgängig gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="dcb53-161">If 0, all migrations will be reverted.</span></span> <span data-ttu-id="dcb53-162">Standardmäßig bis zum letzten Migration.</span><span class="sxs-lookup"><span data-stu-id="dcb53-162">Defaults to the last migration.</span></span> |

### <a name="dotnet-ef-dbcontext-info"></a><span data-ttu-id="dcb53-163">Dotnet Ef Dbcontext info</span><span class="sxs-lookup"><span data-stu-id="dcb53-163">dotnet ef dbcontext info</span></span>

<span data-ttu-id="dcb53-164">Ruft Informationen zu einem DbContext-Typ ab.</span><span class="sxs-lookup"><span data-stu-id="dcb53-164">Gets information about a DbContext type.</span></span>

### <a name="dotnet-ef-dbcontext-list"></a><span data-ttu-id="dcb53-165">Dotnet Ef Dbcontext-Liste</span><span class="sxs-lookup"><span data-stu-id="dcb53-165">dotnet ef dbcontext list</span></span>

<span data-ttu-id="dcb53-166">Listet die verfügbare DbContext-Typen.</span><span class="sxs-lookup"><span data-stu-id="dcb53-166">Lists available DbContext types.</span></span>

### <a name="dotnet-ef-dbcontext-scaffold"></a><span data-ttu-id="dcb53-167">Dotnet Ef Dbcontext Gerüst</span><span class="sxs-lookup"><span data-stu-id="dcb53-167">dotnet ef dbcontext scaffold</span></span>

<span data-ttu-id="dcb53-168">Gerüste ein ' DbContext ' und Entität Typen für eine Datenbank.</span><span class="sxs-lookup"><span data-stu-id="dcb53-168">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="dcb53-169">Argumente:</span><span class="sxs-lookup"><span data-stu-id="dcb53-169">Arguments:</span></span>

|               |                                                                     |
| ------------- | ------------------------------------------------------------------- |
| <span data-ttu-id="dcb53-170">\<VERBINDUNG ></span><span class="sxs-lookup"><span data-stu-id="dcb53-170">\<CONNECTION></span></span> | <span data-ttu-id="dcb53-171">Die Verbindungszeichenfolge zur Datenbank.</span><span class="sxs-lookup"><span data-stu-id="dcb53-171">The connection string to the database.</span></span>                              |
| <span data-ttu-id="dcb53-172">\<ANBIETER ></span><span class="sxs-lookup"><span data-stu-id="dcb53-172">\<PROVIDER></span></span>   | <span data-ttu-id="dcb53-173">Die zu verwendenden Anbieter an.</span><span class="sxs-lookup"><span data-stu-id="dcb53-173">The provider to use.</span></span> <span data-ttu-id="dcb53-174">(D. h.</span><span class="sxs-lookup"><span data-stu-id="dcb53-174">(E.g.</span></span> <span data-ttu-id="dcb53-175">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="dcb53-175">Microsoft.EntityFrameworkCore.SqlServer)</span></span> |

<span data-ttu-id="dcb53-176">Optionen:</span><span class="sxs-lookup"><span data-stu-id="dcb53-176">Options:</span></span>

|                 |                                         |                                                          |
| --------------- | --------------------------------------- | -------------------------------------------------------- |
| <span data-ttu-id="dcb53-177"><nobr>-d:</nobr></span><span class="sxs-lookup"><span data-stu-id="dcb53-177"><nobr>-d</nobr></span></span> |       <span data-ttu-id="dcb53-178">--datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="dcb53-178">--data-annotations</span></span>                | <span data-ttu-id="dcb53-179">Verwenden Sie Attribute, um das Modell (sofern möglich) konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="dcb53-179">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="dcb53-180">Wenn nicht angegeben, wird nur die fluent-API verwendet.</span><span class="sxs-lookup"><span data-stu-id="dcb53-180">If omitted, only the fluent API is used.</span></span> |
|       <span data-ttu-id="dcb53-181">-c</span><span class="sxs-lookup"><span data-stu-id="dcb53-181">-c</span></span>        |       <span data-ttu-id="dcb53-182">--Kontext \<NAME ></span><span class="sxs-lookup"><span data-stu-id="dcb53-182">--context \<NAME></span></span>                 | <span data-ttu-id="dcb53-183">Der Name von ' DbContext '.</span><span class="sxs-lookup"><span data-stu-id="dcb53-183">The name of the DbContext.</span></span>                               |
|       <span data-ttu-id="dcb53-184">-f</span><span class="sxs-lookup"><span data-stu-id="dcb53-184">-f</span></span>        |       <span data-ttu-id="dcb53-185">--erzwingen</span><span class="sxs-lookup"><span data-stu-id="dcb53-185">--force</span></span>                           | <span data-ttu-id="dcb53-186">Überschreiben Sie vorhandene Dateien.</span><span class="sxs-lookup"><span data-stu-id="dcb53-186">Overwrite existing files.</span></span>                                |
|       <span data-ttu-id="dcb53-187">-o</span><span class="sxs-lookup"><span data-stu-id="dcb53-187">-o</span></span>        |       <span data-ttu-id="dcb53-188">--Ausgabe Dir \<Pfad ></span><span class="sxs-lookup"><span data-stu-id="dcb53-188">--output-dir \<PATH></span></span>              | <span data-ttu-id="dcb53-189">Das Verzeichnis in den Dateien versetzt.</span><span class="sxs-lookup"><span data-stu-id="dcb53-189">The directory to put files in.</span></span> <span data-ttu-id="dcb53-190">Pfade sind relativ zum Projektverzeichnis an.</span><span class="sxs-lookup"><span data-stu-id="dcb53-190">Paths are relative to the project directory.</span></span> |
|                 | <span data-ttu-id="dcb53-191"><nobr>--Schema \<SCHEMA_NAME >...</nobr></span><span class="sxs-lookup"><span data-stu-id="dcb53-191"><nobr>--schema \<SCHEMA_NAME>...</nobr></span></span> | <span data-ttu-id="dcb53-192">Die Schemas der Tabellen zur Generierung von Entitätstypen für.</span><span class="sxs-lookup"><span data-stu-id="dcb53-192">The schemas of tables to generate entity types for.</span></span>      |
|       <span data-ttu-id="dcb53-193">-t</span><span class="sxs-lookup"><span data-stu-id="dcb53-193">-t</span></span>        |       <span data-ttu-id="dcb53-194">--Tabelle \<TABLE_NAME >...</span><span class="sxs-lookup"><span data-stu-id="dcb53-194">--table \<TABLE_NAME>...</span></span>          | <span data-ttu-id="dcb53-195">Die Tabellen für Entitätstypen generieren.</span><span class="sxs-lookup"><span data-stu-id="dcb53-195">The tables to generate entity types for.</span></span>                 |
|                 |       <span data-ttu-id="dcb53-196">– Verwenden der Datenbanknamen</span><span class="sxs-lookup"><span data-stu-id="dcb53-196">--use-database-names</span></span>              | <span data-ttu-id="dcb53-197">Verwenden Sie die Tabellen- und Spaltennamen direkt aus der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="dcb53-197">Use table and column names directly from the database.</span></span>   |

### <a name="dotnet-ef-migrations-add"></a><span data-ttu-id="dcb53-198">Dotnet Ef Migrationen hinzufügen</span><span class="sxs-lookup"><span data-stu-id="dcb53-198">dotnet ef migrations add</span></span>

<span data-ttu-id="dcb53-199">Fügt eine neue Migration.</span><span class="sxs-lookup"><span data-stu-id="dcb53-199">Adds a new migration.</span></span>

<span data-ttu-id="dcb53-200">Argumente:</span><span class="sxs-lookup"><span data-stu-id="dcb53-200">Arguments:</span></span>

|         |                            |
| ------- | -------------------------- |
| <span data-ttu-id="dcb53-201">\<NAME ></span><span class="sxs-lookup"><span data-stu-id="dcb53-201">\<NAME></span></span> | <span data-ttu-id="dcb53-202">Der Name der Migration.</span><span class="sxs-lookup"><span data-stu-id="dcb53-202">The name of the migration.</span></span> |

<span data-ttu-id="dcb53-203">Optionen:</span><span class="sxs-lookup"><span data-stu-id="dcb53-203">Options:</span></span>

|                 |                                   |                                                                |
| --------------- |---------------------------------- | -------------------------------------------------------------- |
| <span data-ttu-id="dcb53-204"><nobr>-o</nobr></span><span class="sxs-lookup"><span data-stu-id="dcb53-204"><nobr>-o</nobr></span></span> | <span data-ttu-id="dcb53-205"><nobr>--Ausgabe Dir \<Pfad ></nobr></span><span class="sxs-lookup"><span data-stu-id="dcb53-205"><nobr>--output-dir \<PATH></nobr></span></span> | <span data-ttu-id="dcb53-206">Das Verzeichnis (und Sub-Namespace) zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="dcb53-206">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="dcb53-207">Pfade sind relativ zum Projektverzeichnis an.</span><span class="sxs-lookup"><span data-stu-id="dcb53-207">Paths are relative to the project directory.</span></span> <span data-ttu-id="dcb53-208">Der Standardwert ist "Migration".</span><span class="sxs-lookup"><span data-stu-id="dcb53-208">Defaults to "Migrations".</span></span> |

### <a name="dotnet-ef-migrations-list"></a><span data-ttu-id="dcb53-209">Dotnet Ef Migrationen Liste</span><span class="sxs-lookup"><span data-stu-id="dcb53-209">dotnet ef migrations list</span></span>

<span data-ttu-id="dcb53-210">Listet die verfügbaren Migrationen.</span><span class="sxs-lookup"><span data-stu-id="dcb53-210">Lists available migrations.</span></span>

### <a name="dotnet-ef-migrations-remove"></a><span data-ttu-id="dcb53-211">Entfernen Sie Dotnet Ef Migrationen</span><span class="sxs-lookup"><span data-stu-id="dcb53-211">dotnet ef migrations remove</span></span>

<span data-ttu-id="dcb53-212">Entfernt die letzte Migration.</span><span class="sxs-lookup"><span data-stu-id="dcb53-212">Removes the last migration.</span></span>

<span data-ttu-id="dcb53-213">Optionen:</span><span class="sxs-lookup"><span data-stu-id="dcb53-213">Options:</span></span>

|    |         |                                                                       |
| -- | ------- | --------------------------------------------------------------------- |
| <span data-ttu-id="dcb53-214">-f</span><span class="sxs-lookup"><span data-stu-id="dcb53-214">-f</span></span> | <span data-ttu-id="dcb53-215">--erzwingen</span><span class="sxs-lookup"><span data-stu-id="dcb53-215">--force</span></span> | <span data-ttu-id="dcb53-216">Überprüfen Sie nicht, um festzustellen, ob die Migration der Datenbank angewendet wurde.</span><span class="sxs-lookup"><span data-stu-id="dcb53-216">Don't check to see if the migration has been applied to the database.</span></span> |

### <a name="dotnet-ef-migrations-script"></a><span data-ttu-id="dcb53-217">Dotnet Ef Migrationen Skript</span><span class="sxs-lookup"><span data-stu-id="dcb53-217">dotnet ef migrations script</span></span>

<span data-ttu-id="dcb53-218">Generiert ein SQL-Skript von Migrationen.</span><span class="sxs-lookup"><span data-stu-id="dcb53-218">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="dcb53-219">Argumente:</span><span class="sxs-lookup"><span data-stu-id="dcb53-219">Arguments:</span></span>

|         |                                                               |
| ------- | ------------------------------------------------------------- |
| <span data-ttu-id="dcb53-220">\<AUS ></span><span class="sxs-lookup"><span data-stu-id="dcb53-220">\<FROM></span></span> | <span data-ttu-id="dcb53-221">Der Start Migration.</span><span class="sxs-lookup"><span data-stu-id="dcb53-221">The starting migration.</span></span> <span data-ttu-id="dcb53-222">Der Standardwert ist 0 (die ursprüngliche Datenbank).</span><span class="sxs-lookup"><span data-stu-id="dcb53-222">Defaults to 0 (the initial database).</span></span> |
| <span data-ttu-id="dcb53-223">\<UM ></span><span class="sxs-lookup"><span data-stu-id="dcb53-223">\<TO></span></span>   | <span data-ttu-id="dcb53-224">Der Endwert Migration.</span><span class="sxs-lookup"><span data-stu-id="dcb53-224">The ending migration.</span></span> <span data-ttu-id="dcb53-225">Standardmäßig bis zum letzten Migration.</span><span class="sxs-lookup"><span data-stu-id="dcb53-225">Defaults to the last migration.</span></span>         |

<span data-ttu-id="dcb53-226">Optionen:</span><span class="sxs-lookup"><span data-stu-id="dcb53-226">Options:</span></span>

|    |                  |                                                                    |
| -- | ---------------- | ------------------------------------------------------------------ |
| <span data-ttu-id="dcb53-227">-o</span><span class="sxs-lookup"><span data-stu-id="dcb53-227">-o</span></span> | <span data-ttu-id="dcb53-228">--Ausgabe \<Datei ></span><span class="sxs-lookup"><span data-stu-id="dcb53-228">--output \<FILE></span></span> | <span data-ttu-id="dcb53-229">Die Datei, schreibt das Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="dcb53-229">The file to write the result to.</span></span>                                   |
| <span data-ttu-id="dcb53-230">-i</span><span class="sxs-lookup"><span data-stu-id="dcb53-230">-i</span></span> | <span data-ttu-id="dcb53-231">--idempotent</span><span class="sxs-lookup"><span data-stu-id="dcb53-231">--idempotent</span></span>     | <span data-ttu-id="dcb53-232">Generiert ein Skript, das für eine Datenbank bei jeder Migration verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="dcb53-232">Generate a script that can be used on a database at any migration.</span></span> |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
