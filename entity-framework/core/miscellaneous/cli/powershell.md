---
title: Paket-Manager-Konsole (Visual Studio) – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: a53455a78db4bc504c45abafdacf9a15381f608e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812559"
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="bd7c5-102">EF-Core-Paket-Manager-Konsole Tools</span><span class="sxs-lookup"><span data-stu-id="bd7c5-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="bd7c5-103">Die Tools EF Core Paket-Manager-Konsole (PMC) ausführen, in Visual Studio mithilfe von NuGet [Package Manager Console][2].</span><span class="sxs-lookup"><span data-stu-id="bd7c5-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="bd7c5-104">Diese Tools funktionieren sowohl mit .NET Framework- als auch mit .NET Core-Projekten.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="bd7c5-105">Verwenden Sie nicht Visual Studio?</span><span class="sxs-lookup"><span data-stu-id="bd7c5-105">Not using Visual Studio?</span></span> <span data-ttu-id="bd7c5-106">Die [EF Core-Befehlszeilentools] [ 1] über Plattformen hinweg und in einer Eingabeaufforderung ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="bd7c5-107">Installieren der Tools</span><span class="sxs-lookup"><span data-stu-id="bd7c5-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="bd7c5-108">Installieren von EF-Core-Paket-Manager-Konsole Tools durch das Microsoft.EntityFrameworkCore.Tools NuGet-Paket installieren.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="bd7c5-109">Sie können es installieren, indem Sie in den folgenden Befehl ausführen [Package Manager Console][2].</span><span class="sxs-lookup"><span data-stu-id="bd7c5-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="bd7c5-110">Wenn alles ordnungsgemäß funktioniert, sollten Sie diesen Befehl ausführen können:</span><span class="sxs-lookup"><span data-stu-id="bd7c5-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="bd7c5-111">Wenn das Startprojekt .NET Standard verwendet [Cross-Zielframework eine unterstützte] [ 3] vor der Verwendung der Tools.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bd7c5-112">Bei Verwendung von **universelle Windows** oder **Xamarin**, Verschieben von EF Code auf eine .NET Standard-Klassenbibliothek und [Cross-Zielframework eine unterstützte] [ 3] vor der Verwendung der Tools.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="bd7c5-113">Geben Sie die Klassenbibliothek als Startprojekt fest.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="bd7c5-114">Mithilfe der tools</span><span class="sxs-lookup"><span data-stu-id="bd7c5-114">Using the tools</span></span>
---------------
<span data-ttu-id="bd7c5-115">Bei jedem eines Befehls aufrufen umfasst zwei Projekte:</span><span class="sxs-lookup"><span data-stu-id="bd7c5-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="bd7c5-116">Dem Zielprojekt werden Dateien hinzugefügt (oder sie werden in einigen Fällen aus diesem entfernt).</span><span class="sxs-lookup"><span data-stu-id="bd7c5-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="bd7c5-117">Das Zielprojekt wird standardmäßig auf die **Standardprojekt** in Paket-Manager-Konsole ausgewählt, aber kann auch mit angegeben werden-Projektparameter.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="bd7c5-118">Das Startprojekt wird bei Ausführung des Projektcodes von den Tools emuliert.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="bd7c5-119">Wird standardmäßig eine **als Startprojekt festlegen** im Projektmappen-Explorer.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="bd7c5-120">Sie können auch mit dem StartupProject - Parameter angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="bd7c5-121">Allgemeine Parameter:</span><span class="sxs-lookup"><span data-stu-id="bd7c5-121">Common parameters:</span></span>

|                           |                             |
|:--------------------------|:----------------------------|
| <span data-ttu-id="bd7c5-122">-Kontext \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="bd7c5-122">-Context \<String></span></span>        | <span data-ttu-id="bd7c5-123">Die DbContext verwenden.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="bd7c5-124">-Projekt \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="bd7c5-124">-Project \<String></span></span>        | <span data-ttu-id="bd7c5-125">Das Projekt verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-125">The project to use.</span></span>         |
| <span data-ttu-id="bd7c5-126">-StartupProject \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="bd7c5-126">-StartupProject \<String></span></span> | <span data-ttu-id="bd7c5-127">Das Startup-Projekt verwenden.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-127">The startup project to use.</span></span> |
| <span data-ttu-id="bd7c5-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="bd7c5-128">-Verbose</span></span>                  | <span data-ttu-id="bd7c5-129">Zeigen Sie eine ausführlichen Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-129">Show verbose output.</span></span>        |

<span data-ttu-id="bd7c5-130">Um Hilfeinformationen zu einem Befehl anzuzeigen, verwenden Sie PowerShell `Get-Help` Befehl.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="bd7c5-131">Der Kontext, Projekt- und StartupProject-Parameter unterstützen Tab-Taste.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="bd7c5-132">Legen Sie **Env:ASPNETCORE_ENVIRONMENT** vor dem Ausführen, um anzugeben, die ASP.NET Core-Umgebung.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="bd7c5-133">Befehle</span><span class="sxs-lookup"><span data-stu-id="bd7c5-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="bd7c5-134">Hinzufügen-Migration</span><span class="sxs-lookup"><span data-stu-id="bd7c5-134">Add-Migration</span></span>

<span data-ttu-id="bd7c5-135">Fügt eine neue Migration.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-135">Adds a new migration.</span></span>

<span data-ttu-id="bd7c5-136">Parameter:</span><span class="sxs-lookup"><span data-stu-id="bd7c5-136">Parameters:</span></span>

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="bd7c5-137">***-Namen*** \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="bd7c5-137">***-Name*** \<String></span></span>             | <span data-ttu-id="bd7c5-138">Der Name der Migration.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-138">The name of the migration.</span></span>                                                                                       |
| <span data-ttu-id="bd7c5-139"><nobr>-OutputDir \<Zeichenfolge ></nobr></span><span class="sxs-lookup"><span data-stu-id="bd7c5-139"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="bd7c5-140">Das Verzeichnis (und Sub-Namespace) zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="bd7c5-141">Pfade sind relativ zum Projektverzeichnis an.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="bd7c5-142">Der Standardwert ist "Migration".</span><span class="sxs-lookup"><span data-stu-id="bd7c5-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="bd7c5-143">Parameter in **fett** sind erforderlich, und diejenigen *Kursiv* sind mit Feldern fester Breite.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="bd7c5-144">Drop-Datenbank</span><span class="sxs-lookup"><span data-stu-id="bd7c5-144">Drop-Database</span></span>

<span data-ttu-id="bd7c5-145">Löscht die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-145">Drops the database.</span></span>

<span data-ttu-id="bd7c5-146">Parameter:</span><span class="sxs-lookup"><span data-stu-id="bd7c5-146">Parameters:</span></span>

|         |                                                          |
|:--------|:---------------------------------------------------------|
| <span data-ttu-id="bd7c5-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="bd7c5-147">-WhatIf</span></span> | <span data-ttu-id="bd7c5-148">Anzeigen, welche Datenbank verworfen werden, jedoch nicht, legen Sie sie.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="bd7c5-149">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="bd7c5-149">Get-DbContext</span></span>

<span data-ttu-id="bd7c5-150">Ruft Informationen zu einem DbContext-Typ ab.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="bd7c5-151">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="bd7c5-151">Remove-Migration</span></span>

<span data-ttu-id="bd7c5-152">Entfernt die letzte Migration.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-152">Removes the last migration.</span></span>

<span data-ttu-id="bd7c5-153">Parameter:</span><span class="sxs-lookup"><span data-stu-id="bd7c5-153">Parameters:</span></span>

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| <span data-ttu-id="bd7c5-154">-Force</span><span class="sxs-lookup"><span data-stu-id="bd7c5-154">-Force</span></span> | <span data-ttu-id="bd7c5-155">Die Migration zurückgesetzt werden, wenn es auf die Datenbank angewendet wurde.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-155">Revert the migration if it has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="bd7c5-156">Gerüst DbContext</span><span class="sxs-lookup"><span data-stu-id="bd7c5-156">Scaffold-DbContext</span></span>

<span data-ttu-id="bd7c5-157">Gerüste ein ' DbContext ' und Entität Typen für eine Datenbank.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="bd7c5-158">Parameter:</span><span class="sxs-lookup"><span data-stu-id="bd7c5-158">Parameters:</span></span>

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="bd7c5-159"><nobr>***-Verbindung*** \<Zeichenfolge ></nobr></span><span class="sxs-lookup"><span data-stu-id="bd7c5-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="bd7c5-160">Die Verbindungszeichenfolge zur Datenbank.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-160">The connection string to the database.</span></span>                                                           |
| <span data-ttu-id="bd7c5-161">***-Anbieter*** \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="bd7c5-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="bd7c5-162">Die zu verwendenden Anbieter an.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-162">The provider to use.</span></span> <span data-ttu-id="bd7c5-163">(z. B.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-163">(E.g.</span></span> <span data-ttu-id="bd7c5-164">Microsoft.EntityFrameworkCore.SqlServer)</span><span class="sxs-lookup"><span data-stu-id="bd7c5-164">Microsoft.EntityFrameworkCore.SqlServer)</span></span>                              |
| <span data-ttu-id="bd7c5-165">-OutputDir \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="bd7c5-165">-OutputDir \<String></span></span>                     | <span data-ttu-id="bd7c5-166">Das Verzeichnis in den Dateien versetzt.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-166">The directory to put files in.</span></span> <span data-ttu-id="bd7c5-167">Pfade sind relativ zum Projektverzeichnis an.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-167">Paths are relative to the project directory.</span></span>                      |
| <span data-ttu-id="bd7c5-168">-ContextDir \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="bd7c5-168">-ContextDir \<String></span></span>                    | <span data-ttu-id="bd7c5-169">Das Verzeichnis DbContext-Datei in den versetzt.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-169">The directory to put DbContext file in.</span></span> <span data-ttu-id="bd7c5-170">Pfade sind relativ zum Projektverzeichnis an.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-170">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="bd7c5-171">-Kontext \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="bd7c5-171">-Context \<String></span></span>                       | <span data-ttu-id="bd7c5-172">Der Name von ' DbContext ' zu generieren.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-172">The name of the DbContext to generate.</span></span>                                                           |
| <span data-ttu-id="bd7c5-173">-Schemas \<String [] ></span><span class="sxs-lookup"><span data-stu-id="bd7c5-173">-Schemas \<String[]></span></span>                     | <span data-ttu-id="bd7c5-174">Die Schemas der Tabellen zur Generierung von Entitätstypen für.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-174">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="bd7c5-175">-Tabellen \<String [] ></span><span class="sxs-lookup"><span data-stu-id="bd7c5-175">-Tables \<String[]></span></span>                      | <span data-ttu-id="bd7c5-176">Die Tabellen für Entitätstypen generieren.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-176">The tables to generate entity types for.</span></span>                                                         |
| <span data-ttu-id="bd7c5-177">DataAnnotations-</span><span class="sxs-lookup"><span data-stu-id="bd7c5-177">-DataAnnotations</span></span>                         | <span data-ttu-id="bd7c5-178">Verwenden Sie Attribute, um das Modell (sofern möglich) konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-178">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="bd7c5-179">Wenn nicht angegeben, wird nur die fluent-API verwendet.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-179">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="bd7c5-180">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="bd7c5-180">-UseDatabaseNames</span></span>                        | <span data-ttu-id="bd7c5-181">Verwenden Sie die Tabellen- und Spaltennamen direkt aus der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-181">Use table and column names directly from the database.</span></span>                                           |
| <span data-ttu-id="bd7c5-182">-Force</span><span class="sxs-lookup"><span data-stu-id="bd7c5-182">-Force</span></span>                                   | <span data-ttu-id="bd7c5-183">Überschreiben Sie vorhandene Dateien.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-183">Overwrite existing files.</span></span>                                                                        |

### <a name="script-migration"></a><span data-ttu-id="bd7c5-184">Skript-Migration</span><span class="sxs-lookup"><span data-stu-id="bd7c5-184">Script-Migration</span></span>

<span data-ttu-id="bd7c5-185">Generiert ein SQL-Skript von Migrationen.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-185">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="bd7c5-186">Parameter:</span><span class="sxs-lookup"><span data-stu-id="bd7c5-186">Parameters:</span></span>

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| <span data-ttu-id="bd7c5-187">*-From* \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="bd7c5-187">*-From* \<String></span></span> | <span data-ttu-id="bd7c5-188">Der Start Migration.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-188">The starting migration.</span></span> <span data-ttu-id="bd7c5-189">Der Standardwert ist 0 (die ursprüngliche Datenbank).</span><span class="sxs-lookup"><span data-stu-id="bd7c5-189">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="bd7c5-190">*-Zu* \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="bd7c5-190">*-To* \<String></span></span>   | <span data-ttu-id="bd7c5-191">Der Endwert Migration.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-191">The ending migration.</span></span> <span data-ttu-id="bd7c5-192">Standardmäßig bis zum letzten Migration.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-192">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="bd7c5-193">-Idempotent</span><span class="sxs-lookup"><span data-stu-id="bd7c5-193">-Idempotent</span></span>       | <span data-ttu-id="bd7c5-194">Generiert ein Skript, das für eine Datenbank bei jeder Migration verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-194">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="bd7c5-195">-Output \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="bd7c5-195">-Output \<String></span></span> | <span data-ttu-id="bd7c5-196">Die Datei, schreibt das Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-196">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="bd7c5-197">To, From, und Output-Parameter unterstützen Tab-Taste.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-197">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="bd7c5-198">Datenbank aktualisieren</span><span class="sxs-lookup"><span data-stu-id="bd7c5-198">Update-Database</span></span>

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <span data-ttu-id="bd7c5-199"><nobr>*-Migration* \<Zeichenfolge ></nobr></span><span class="sxs-lookup"><span data-stu-id="bd7c5-199"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="bd7c5-200">Der zielmigration.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-200">The target migration.</span></span> <span data-ttu-id="bd7c5-201">Bei "0" werden bei allen Migrationen rückgängig gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-201">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="bd7c5-202">Standardmäßig bis zum letzten Migration.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-202">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="bd7c5-203">Der Parameter für die Migration unterstützt Tab-Taste.</span><span class="sxs-lookup"><span data-stu-id="bd7c5-203">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
