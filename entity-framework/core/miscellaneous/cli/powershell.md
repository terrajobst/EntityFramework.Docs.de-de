---
title: "Paket-Manager-Konsole (Visual Studio) – EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: b4ecb27edf94e7b9ad6c7fe65a891dcbf1593309
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="77e3c-102">EF-Core-Paket-Manager-Konsole Tools</span><span class="sxs-lookup"><span data-stu-id="77e3c-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="77e3c-103">Die Tools EF Core Paket-Manager-Konsole (PMC) ausführen, in Visual Studio mithilfe von NuGet [Package Manager Console][2].</span><span class="sxs-lookup"><span data-stu-id="77e3c-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="77e3c-104">Diese Tools funktionieren mit .NET Framework und .NET Core-Projekten.</span><span class="sxs-lookup"><span data-stu-id="77e3c-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="77e3c-105">Verwenden Sie nicht Visual Studio?</span><span class="sxs-lookup"><span data-stu-id="77e3c-105">Not using Visual Studio?</span></span> <span data-ttu-id="77e3c-106">Die [EF Core-Befehlszeilentools] [ 1] über Plattformen hinweg und in einer Eingabeaufforderung ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="77e3c-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="77e3c-107">Installieren der Tools</span><span class="sxs-lookup"><span data-stu-id="77e3c-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="77e3c-108">Installieren von EF-Core-Paket-Manager-Konsole Tools durch das Microsoft.EntityFrameworkCore.Tools NuGet-Paket installieren.</span><span class="sxs-lookup"><span data-stu-id="77e3c-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="77e3c-109">Sie können es installieren, indem Sie in den folgenden Befehl ausführen [Package Manager Console][2].</span><span class="sxs-lookup"><span data-stu-id="77e3c-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="77e3c-110">Wenn alles ordnungsgemäß funktioniert, sollten Sie diesen Befehl ausführen können:</span><span class="sxs-lookup"><span data-stu-id="77e3c-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="77e3c-111">Wenn das Startprojekt .NET Standard verwendet [Cross-Zielframework eine unterstützte] [ 3] vor der Verwendung der Tools.</span><span class="sxs-lookup"><span data-stu-id="77e3c-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="77e3c-112">Bei Verwendung von **universelle Windows** oder **Xamarin**, Verschieben von EF Code auf eine .NET Standard-Klassenbibliothek und [Cross-Zielframework eine unterstützte] [ 3] vor der Verwendung der Tools.</span><span class="sxs-lookup"><span data-stu-id="77e3c-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="77e3c-113">Geben Sie die Klassenbibliothek als Startprojekt fest.</span><span class="sxs-lookup"><span data-stu-id="77e3c-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="77e3c-114">Mithilfe der tools</span><span class="sxs-lookup"><span data-stu-id="77e3c-114">Using the tools</span></span>
---------------
<span data-ttu-id="77e3c-115">Bei jedem eines Befehls aufrufen umfasst zwei Projekte:</span><span class="sxs-lookup"><span data-stu-id="77e3c-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="77e3c-116">Das Zielprojekt ist keine Dateien hinzugefügt werden (oder in einigen Fällen entfernt).</span><span class="sxs-lookup"><span data-stu-id="77e3c-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="77e3c-117">Das Zielprojekt wird standardmäßig auf die **Standardprojekt** in Paket-Manager-Konsole ausgewählt, aber kann auch mit angegeben werden-Projektparameter.</span><span class="sxs-lookup"><span data-stu-id="77e3c-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="77e3c-118">Das Startprojekt wird von den Tools emuliert wird, wenn der Code des Projekts ausführen.</span><span class="sxs-lookup"><span data-stu-id="77e3c-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="77e3c-119">Wird standardmäßig eine **als Startprojekt festlegen** im Projektmappen-Explorer.</span><span class="sxs-lookup"><span data-stu-id="77e3c-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="77e3c-120">Sie können auch mit dem StartupProject - Parameter angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="77e3c-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="77e3c-121">Allgemeine Parameter:</span><span class="sxs-lookup"><span data-stu-id="77e3c-121">Common parameters:</span></span>

|                           |                             |
| ------------------------- | --------------------------- |
| <span data-ttu-id="77e3c-122">-Kontext \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="77e3c-122">-Context \<String></span></span>        | <span data-ttu-id="77e3c-123">Die DbContext verwenden.</span><span class="sxs-lookup"><span data-stu-id="77e3c-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="77e3c-124">-Projekt \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="77e3c-124">-Project \<String></span></span>        | <span data-ttu-id="77e3c-125">Das Projekt verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="77e3c-125">The project to use.</span></span>         |
| <span data-ttu-id="77e3c-126">-StartupProject \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="77e3c-126">-StartupProject \<String></span></span> | <span data-ttu-id="77e3c-127">Das Startup-Projekt verwenden.</span><span class="sxs-lookup"><span data-stu-id="77e3c-127">The startup project to use.</span></span> |
| <span data-ttu-id="77e3c-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="77e3c-128">-Verbose</span></span>                  | <span data-ttu-id="77e3c-129">Zeigen Sie eine ausführlichen Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="77e3c-129">Show verbose output.</span></span>        |

<span data-ttu-id="77e3c-130">Um Hilfeinformationen zu einem Befehl anzuzeigen, verwenden Sie PowerShell `Get-Help` Befehl.</span><span class="sxs-lookup"><span data-stu-id="77e3c-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="77e3c-131">Der Kontext, Projekt- und StartupProject-Parameter unterstützen Tab-Taste.</span><span class="sxs-lookup"><span data-stu-id="77e3c-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="77e3c-132">Legen Sie **Env:ASPNETCORE_ENVIRONMENT** vor dem Ausführen, um anzugeben, die ASP.NET Core-Umgebung.</span><span class="sxs-lookup"><span data-stu-id="77e3c-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="77e3c-133">Befehle</span><span class="sxs-lookup"><span data-stu-id="77e3c-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="77e3c-134">Hinzufügen-Migration</span><span class="sxs-lookup"><span data-stu-id="77e3c-134">Add-Migration</span></span>

<span data-ttu-id="77e3c-135">Fügt eine neue Migration.</span><span class="sxs-lookup"><span data-stu-id="77e3c-135">Adds a new migration.</span></span>

<span data-ttu-id="77e3c-136">Parameter:</span><span class="sxs-lookup"><span data-stu-id="77e3c-136">Parameters:</span></span>

|                                    |                                                                                 |
| ---------------------------------- | ------------------------------------------------------------------------------- |
| <span data-ttu-id="77e3c-137">***-Namen*** \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="77e3c-137">***-Name*** \<String></span></span>              | <span data-ttu-id="77e3c-138">Der Name der Migration.</span><span class="sxs-lookup"><span data-stu-id="77e3c-138">The name of the migration.</span></span>                                                      |
| <span data-ttu-id="77e3c-139"><nobr>-OutputDir \<Zeichenfolge ></nobr></span><span class="sxs-lookup"><span data-stu-id="77e3c-139"><nobr>-OutputDir \<String></nobr></span></span>  | <span data-ttu-id="77e3c-140">Das Verzeichnis (und Sub-Namespace) zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="77e3c-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="77e3c-141">Pfade sind relativ zum Projektverzeichnis an.</span><span class="sxs-lookup"><span data-stu-id="77e3c-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="77e3c-142">Der Standardwert ist "Migration".</span><span class="sxs-lookup"><span data-stu-id="77e3c-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="77e3c-143">Parameter in **fett** sind erforderlich, und diejenigen *Kursiv* sind mit Feldern fester Breite.</span><span class="sxs-lookup"><span data-stu-id="77e3c-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="77e3c-144">Drop-Datenbank</span><span class="sxs-lookup"><span data-stu-id="77e3c-144">Drop-Database</span></span>

<span data-ttu-id="77e3c-145">Löscht die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="77e3c-145">Drops the database.</span></span>

<span data-ttu-id="77e3c-146">Parameter:</span><span class="sxs-lookup"><span data-stu-id="77e3c-146">Parameters:</span></span>

|          |                                                          |
| -------- | -------------------------------------------------------- |
| <span data-ttu-id="77e3c-147">"-WhatIf"</span><span class="sxs-lookup"><span data-stu-id="77e3c-147">-WhatIf</span></span>  | <span data-ttu-id="77e3c-148">Anzeigen, welche Datenbank verworfen werden, jedoch nicht, legen Sie sie.</span><span class="sxs-lookup"><span data-stu-id="77e3c-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="77e3c-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="77e3c-149">Get-DbContext</span></span>

<span data-ttu-id="77e3c-150">Ruft Informationen zu einem DbContext-Typ ab.</span><span class="sxs-lookup"><span data-stu-id="77e3c-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="77e3c-151">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="77e3c-151">Remove-Migration</span></span>

<span data-ttu-id="77e3c-152">Entfernt die letzte Migration.</span><span class="sxs-lookup"><span data-stu-id="77e3c-152">Removes the last migration.</span></span>

<span data-ttu-id="77e3c-153">Parameter:</span><span class="sxs-lookup"><span data-stu-id="77e3c-153">Parameters:</span></span>

|        |                                                                       |
| ------ | --------------------------------------------------------------------- |
| <span data-ttu-id="77e3c-154">-Force</span><span class="sxs-lookup"><span data-stu-id="77e3c-154">-Force</span></span> | <span data-ttu-id="77e3c-155">Überprüfen Sie nicht, um festzustellen, ob die Migration der Datenbank angewendet wurde.</span><span class="sxs-lookup"><span data-stu-id="77e3c-155">Don't check to see if the migration has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="77e3c-156">Gerüst DbContext</span><span class="sxs-lookup"><span data-stu-id="77e3c-156">Scaffold-DbContext</span></span>

<span data-ttu-id="77e3c-157">Gerüste ein ' DbContext ' und Entität Typen für eine Datenbank.</span><span class="sxs-lookup"><span data-stu-id="77e3c-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="77e3c-158">Parameter:</span><span class="sxs-lookup"><span data-stu-id="77e3c-158">Parameters:</span></span>

|                                          |                                                                           |
| ---------------------------------------- | ------------------------------------------------------------------------- |
| <span data-ttu-id="77e3c-159"><nobr>***-Verbindung*** \<Zeichenfolge ></nobr></span><span class="sxs-lookup"><span data-stu-id="77e3c-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="77e3c-160">Die Verbindungszeichenfolge zur Datenbank.</span><span class="sxs-lookup"><span data-stu-id="77e3c-160">The connection string to the database.</span></span>                                    |
| <span data-ttu-id="77e3c-161">***-Anbieter*** \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="77e3c-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="77e3c-162">Die zu verwendenden Anbieter an.</span><span class="sxs-lookup"><span data-stu-id="77e3c-162">The provider to use.</span></span> <span data-ttu-id="77e3c-163">(D. h.</span><span class="sxs-lookup"><span data-stu-id="77e3c-163">(E.g.</span></span> <span data-ttu-id="77e3c-164">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="77e3c-164">Microsoft.EntityFrameworkCore.SqlServer)</span></span>       |
| <span data-ttu-id="77e3c-165">-OutputDir \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="77e3c-165">-OutputDir \<String></span></span>                     | <span data-ttu-id="77e3c-166">Das Verzeichnis in den Dateien versetzt.</span><span class="sxs-lookup"><span data-stu-id="77e3c-166">The directory to put files in.</span></span> <span data-ttu-id="77e3c-167">Pfade sind relativ zum Projektverzeichnis an.</span><span class="sxs-lookup"><span data-stu-id="77e3c-167">Paths are relative to the project directory.</span></span> |
| <span data-ttu-id="77e3c-168">-Kontext \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="77e3c-168">-Context \<String></span></span>                       | <span data-ttu-id="77e3c-169">Der Name von ' DbContext ' zu generieren.</span><span class="sxs-lookup"><span data-stu-id="77e3c-169">The name of the DbContext to generate.</span></span>                                    |
| <span data-ttu-id="77e3c-170">-Schemas \<String [] ></span><span class="sxs-lookup"><span data-stu-id="77e3c-170">-Schemas \<String[]></span></span>                     | <span data-ttu-id="77e3c-171">Die Schemas der Tabellen zur Generierung von Entitätstypen für.</span><span class="sxs-lookup"><span data-stu-id="77e3c-171">The schemas of tables to generate entity types for.</span></span>                       |
| <span data-ttu-id="77e3c-172">-Tabellen \<String [] ></span><span class="sxs-lookup"><span data-stu-id="77e3c-172">-Tables \<String[]></span></span>                      | <span data-ttu-id="77e3c-173">Die Tabellen für Entitätstypen generieren.</span><span class="sxs-lookup"><span data-stu-id="77e3c-173">The tables to generate entity types for.</span></span>                                  |
| <span data-ttu-id="77e3c-174">DataAnnotations-</span><span class="sxs-lookup"><span data-stu-id="77e3c-174">-DataAnnotations</span></span>                         | <span data-ttu-id="77e3c-175">Verwenden Sie Attribute, um das Modell (sofern möglich) konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="77e3c-175">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="77e3c-176">Wenn nicht angegeben, wird nur die fluent-API verwendet.</span><span class="sxs-lookup"><span data-stu-id="77e3c-176">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="77e3c-177">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="77e3c-177">-UseDatabaseNames</span></span>                        | <span data-ttu-id="77e3c-178">Verwenden Sie die Tabellen- und Spaltennamen direkt aus der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="77e3c-178">Use table and column names directly from the database.</span></span>                    |
| <span data-ttu-id="77e3c-179">-Force</span><span class="sxs-lookup"><span data-stu-id="77e3c-179">-Force</span></span>                                   | <span data-ttu-id="77e3c-180">Überschreiben Sie vorhandene Dateien.</span><span class="sxs-lookup"><span data-stu-id="77e3c-180">Overwrite existing files.</span></span>                                                 |

### <a name="script-migration"></a><span data-ttu-id="77e3c-181">Skript-Migration</span><span class="sxs-lookup"><span data-stu-id="77e3c-181">Script-Migration</span></span>

<span data-ttu-id="77e3c-182">Generiert ein SQL-Skript von Migrationen.</span><span class="sxs-lookup"><span data-stu-id="77e3c-182">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="77e3c-183">Parameter:</span><span class="sxs-lookup"><span data-stu-id="77e3c-183">Parameters:</span></span>

|                   |                                                                    |
| ----------------- | ------------------------------------------------------------------ |
| <span data-ttu-id="77e3c-184">*-From* \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="77e3c-184">*-From* \<String></span></span> | <span data-ttu-id="77e3c-185">Der Start Migration.</span><span class="sxs-lookup"><span data-stu-id="77e3c-185">The starting migration.</span></span> <span data-ttu-id="77e3c-186">Der Standardwert ist 0 (die ursprüngliche Datenbank).</span><span class="sxs-lookup"><span data-stu-id="77e3c-186">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="77e3c-187">*-Zu* \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="77e3c-187">*-To* \<String></span></span>   | <span data-ttu-id="77e3c-188">Der Endwert Migration.</span><span class="sxs-lookup"><span data-stu-id="77e3c-188">The ending migration.</span></span> <span data-ttu-id="77e3c-189">Standardmäßig bis zum letzten Migration.</span><span class="sxs-lookup"><span data-stu-id="77e3c-189">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="77e3c-190">-Idempotent</span><span class="sxs-lookup"><span data-stu-id="77e3c-190">-Idempotent</span></span>       | <span data-ttu-id="77e3c-191">Generiert ein Skript, das für eine Datenbank bei jeder Migration verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="77e3c-191">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="77e3c-192">-Output \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="77e3c-192">-Output \<String></span></span> | <span data-ttu-id="77e3c-193">Die Datei, schreibt das Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="77e3c-193">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="77e3c-194">To, From, und Output-Parameter unterstützen Tab-Taste.</span><span class="sxs-lookup"><span data-stu-id="77e3c-194">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="77e3c-195">Datenbank aktualisieren</span><span class="sxs-lookup"><span data-stu-id="77e3c-195">Update-Database</span></span>

|                                     |                                                                                |
| ----------------------------------- | ------------------------------------------------------------------------------ |
| <span data-ttu-id="77e3c-196"><nobr>*-Migration* \<Zeichenfolge ></nobr></span><span class="sxs-lookup"><span data-stu-id="77e3c-196"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="77e3c-197">Der zielmigration.</span><span class="sxs-lookup"><span data-stu-id="77e3c-197">The target migration.</span></span> <span data-ttu-id="77e3c-198">Bei "0" werden bei allen Migrationen rückgängig gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="77e3c-198">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="77e3c-199">Standardmäßig bis zum letzten Migration.</span><span class="sxs-lookup"><span data-stu-id="77e3c-199">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="77e3c-200">Der Parameter für die Migration unterstützt Tab-Taste.</span><span class="sxs-lookup"><span data-stu-id="77e3c-200">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks