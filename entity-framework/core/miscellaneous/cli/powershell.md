---
title: Paket-Manager-Konsole (Visual Studio) – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
ms.openlocfilehash: 3d57a1665da2c94c55981d17e041b0ef74282496
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490849"
---
<a name="ef-core-package-manager-console-tools"></a><span data-ttu-id="7a95d-102">Tools für EF Core-Paket-Manager-Konsole</span><span class="sxs-lookup"><span data-stu-id="7a95d-102">EF Core Package Manager Console Tools</span></span>
=====================================
<span data-ttu-id="7a95d-103">Die Paket-Manager-Konsole (PMC) von EF Core-Tools ausführen in Visual Studio mithilfe von NuGet [-Paket-Manager-Konsole][2].</span><span class="sxs-lookup"><span data-stu-id="7a95d-103">The EF Core Package Manager Console (PMC) Tools run inside of Visual Studio using NuGet's [Package Manager Console][2].</span></span>
<span data-ttu-id="7a95d-104">Diese Tools funktionieren sowohl mit .NET Framework- als auch mit .NET Core-Projekten.</span><span class="sxs-lookup"><span data-stu-id="7a95d-104">These tools work with both .NET Framework and .NET Core projects.</span></span>

> [!TIP]
> <span data-ttu-id="7a95d-105">Verwenden Sie nicht Visual Studio ein?</span><span class="sxs-lookup"><span data-stu-id="7a95d-105">Not using Visual Studio?</span></span> <span data-ttu-id="7a95d-106">Die [EF Core-Befehlszeilentools] [ 1] sind – plattformübergreifend und in einer Eingabeaufforderung ausführen.</span><span class="sxs-lookup"><span data-stu-id="7a95d-106">The [EF Core Command-line Tools][1] are cross-platform and run inside a command prompt.</span></span>

<a name="installing-the-tools"></a><span data-ttu-id="7a95d-107">Installieren der Tools</span><span class="sxs-lookup"><span data-stu-id="7a95d-107">Installing the tools</span></span>
--------------------
<span data-ttu-id="7a95d-108">Installieren Sie die Tools der EF Core-Paket-Manager-Konsole, indem das Microsoft.EntityFrameworkCore.Tools-NuGet-Paket installieren.</span><span class="sxs-lookup"><span data-stu-id="7a95d-108">Install the EF Core Package Manager Console Tools by installing the Microsoft.EntityFrameworkCore.Tools NuGet package.</span></span>
<span data-ttu-id="7a95d-109">Sie können es installieren, indem Sie in den folgenden Befehl ausführen [-Paket-Manager-Konsole][2].</span><span class="sxs-lookup"><span data-stu-id="7a95d-109">You can install it by executing the following command inside [Package Manager Console][2].</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="7a95d-110">Wenn alles ordnungsgemäß funktioniert, sollten Sie diesen Befehl ausführen können:</span><span class="sxs-lookup"><span data-stu-id="7a95d-110">If everything worked correctly, you should be able to run this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> <span data-ttu-id="7a95d-111">Wenn Startprojekt .NET Standard abzielt [plattformübergreifend eine Zielversionen ein unterstützten Framework] [ 3] vor der Verwendung der Tools.</span><span class="sxs-lookup"><span data-stu-id="7a95d-111">If your startup project targets .NET Standard, [cross-target a supported framework][3] before using the tools.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7a95d-112">Bei Verwendung von **Universal Windows** oder **Xamarin**, verschieben Sie den EF-Code in eine .NET Standard-Klassenbibliothek und [plattformübergreifend eine Zielversionen ein unterstützten Framework] [ 3] vor der Verwendung der Tools.</span><span class="sxs-lookup"><span data-stu-id="7a95d-112">If you're using **Universal Windows** or **Xamarin**, move your EF code to a .NET Standard class library and [cross-target a supported framework][3] before using the tools.</span></span> <span data-ttu-id="7a95d-113">Geben Sie die Klassenbibliothek als Startprojekt fest.</span><span class="sxs-lookup"><span data-stu-id="7a95d-113">Specify the class library as your startup project.</span></span>

<a name="using-the-tools"></a><span data-ttu-id="7a95d-114">Mithilfe der tools</span><span class="sxs-lookup"><span data-stu-id="7a95d-114">Using the tools</span></span>
---------------
<span data-ttu-id="7a95d-115">Wenn Sie einen Befehl aufrufen, sind zwei Projekte beteiligt:</span><span class="sxs-lookup"><span data-stu-id="7a95d-115">Whenever you invoke a command, there are two projects involved:</span></span>

<span data-ttu-id="7a95d-116">Dem Zielprojekt werden Dateien hinzugefügt (oder sie werden in einigen Fällen aus diesem entfernt).</span><span class="sxs-lookup"><span data-stu-id="7a95d-116">The target project is where any files are added (or in some cases removed).</span></span> <span data-ttu-id="7a95d-117">Das Zielprojekt standardmäßig die **Standardprojekt** in Paket-Manager-Konsole ausgewählt, können aber auch festgelegt werden mithilfe von-Projektparameter.</span><span class="sxs-lookup"><span data-stu-id="7a95d-117">The target project defaults to the **Default project** selected in Package Manager Console, but can also be specified using the -Project parameter.</span></span>

<span data-ttu-id="7a95d-118">Das Startprojekt wird bei Ausführung des Projektcodes von den Tools emuliert.</span><span class="sxs-lookup"><span data-stu-id="7a95d-118">The startup project is the one emulated by the tools when executing your project's code.</span></span> <span data-ttu-id="7a95d-119">Es ist standardmäßig auf eine **als Startprojekt festlegen** im Projektmappen-Explorer.</span><span class="sxs-lookup"><span data-stu-id="7a95d-119">It defaults to one **Set as StartUp Project** in Solution Explorer.</span></span> <span data-ttu-id="7a95d-120">Sie können auch mit dem StartupProject - Parameter angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="7a95d-120">It can also be specified using the -StartupProject parameter.</span></span>

<span data-ttu-id="7a95d-121">Allgemeine Parameter:</span><span class="sxs-lookup"><span data-stu-id="7a95d-121">Common parameters:</span></span>

|                           |                             |
|:--------------------------|:----------------------------|
| <span data-ttu-id="7a95d-122">-Kontext \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="7a95d-122">-Context \<String></span></span>        | <span data-ttu-id="7a95d-123">Die "DbContext" verwenden.</span><span class="sxs-lookup"><span data-stu-id="7a95d-123">The DbContext to use.</span></span>       |
| <span data-ttu-id="7a95d-124">-Projekt \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="7a95d-124">-Project \<String></span></span>        | <span data-ttu-id="7a95d-125">Das Projekt zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="7a95d-125">The project to use.</span></span>         |
| <span data-ttu-id="7a95d-126">-StartupProject \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="7a95d-126">-StartupProject \<String></span></span> | <span data-ttu-id="7a95d-127">Das Startprojekt verwenden.</span><span class="sxs-lookup"><span data-stu-id="7a95d-127">The startup project to use.</span></span> |
| <span data-ttu-id="7a95d-128">-Verbose</span><span class="sxs-lookup"><span data-stu-id="7a95d-128">-Verbose</span></span>                  | <span data-ttu-id="7a95d-129">Zeigen Sie ausführlichen Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="7a95d-129">Show verbose output.</span></span>        |

<span data-ttu-id="7a95d-130">Um Hilfeinformationen zu einem Befehl zu anzuzeigen, verwenden Sie PowerShell `Get-Help` Befehl.</span><span class="sxs-lookup"><span data-stu-id="7a95d-130">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="7a95d-131">Der Kontext, Projekt- und StartupProject parameterunterstützung Tab-Taste.</span><span class="sxs-lookup"><span data-stu-id="7a95d-131">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

> [!TIP]
> <span data-ttu-id="7a95d-132">Legen Sie **Env:ASPNETCORE_ENVIRONMENT** vor dem Ausführen, um die ASP.NET Core-Umgebung anzugeben.</span><span class="sxs-lookup"><span data-stu-id="7a95d-132">Set **env:ASPNETCORE_ENVIRONMENT** before running to specify the ASP.NET Core environment.</span></span>

<a name="commands"></a><span data-ttu-id="7a95d-133">Befehle</span><span class="sxs-lookup"><span data-stu-id="7a95d-133">Commands</span></span>
--------

### <a name="add-migration"></a><span data-ttu-id="7a95d-134">Add-Migration</span><span class="sxs-lookup"><span data-stu-id="7a95d-134">Add-Migration</span></span>

<span data-ttu-id="7a95d-135">Fügt eine neue Migration hinzu.</span><span class="sxs-lookup"><span data-stu-id="7a95d-135">Adds a new migration.</span></span>

<span data-ttu-id="7a95d-136">Parameter:</span><span class="sxs-lookup"><span data-stu-id="7a95d-136">Parameters:</span></span>

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="7a95d-137">***--Name*** \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="7a95d-137">***-Name*** \<String></span></span>             | <span data-ttu-id="7a95d-138">Der Name der Migration.</span><span class="sxs-lookup"><span data-stu-id="7a95d-138">The name of the migration.</span></span>                                                                                       |
| <span data-ttu-id="7a95d-139"><nobr>-OutputDir \<Zeichenfolge ></nobr></span><span class="sxs-lookup"><span data-stu-id="7a95d-139"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="7a95d-140">Das Verzeichnis (und Sub-Namespace) verwenden.</span><span class="sxs-lookup"><span data-stu-id="7a95d-140">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="7a95d-141">Pfade sind relativ zum Projektverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="7a95d-141">Paths are relative to the project directory.</span></span> <span data-ttu-id="7a95d-142">Der Standardwert ist "Migrations".</span><span class="sxs-lookup"><span data-stu-id="7a95d-142">Defaults to "Migrations".</span></span> |

> [!NOTE]
> <span data-ttu-id="7a95d-143">Parameter in **fett** sind erforderlich, und diejenigen *Kursiv* sind mit Feldern fester Breite.</span><span class="sxs-lookup"><span data-stu-id="7a95d-143">Parameters in **bold** are required, and ones in *italics* are positional.</span></span>

### <a name="drop-database"></a><span data-ttu-id="7a95d-144">Drop-Datenbank</span><span class="sxs-lookup"><span data-stu-id="7a95d-144">Drop-Database</span></span>

<span data-ttu-id="7a95d-145">Löscht die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="7a95d-145">Drops the database.</span></span>

<span data-ttu-id="7a95d-146">Parameter:</span><span class="sxs-lookup"><span data-stu-id="7a95d-146">Parameters:</span></span>

|         |                                                          |
|:--------|:---------------------------------------------------------|
| <span data-ttu-id="7a95d-147">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="7a95d-147">-WhatIf</span></span> | <span data-ttu-id="7a95d-148">Anzeigen der Datenbank gelöscht werden würde, dies jedoch noch nicht.</span><span class="sxs-lookup"><span data-stu-id="7a95d-148">Show which database would be dropped, but don't drop it.</span></span> |

### <a name="get-dbcontext"></a><span data-ttu-id="7a95d-149">"Get-DbContext"</span><span class="sxs-lookup"><span data-stu-id="7a95d-149">Get-DbContext</span></span>

<span data-ttu-id="7a95d-150">Ruft Informationen über einen "DbContext"-Typ ab.</span><span class="sxs-lookup"><span data-stu-id="7a95d-150">Gets information about a DbContext type.</span></span>

### <a name="remove-migration"></a><span data-ttu-id="7a95d-151">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="7a95d-151">Remove-Migration</span></span>

<span data-ttu-id="7a95d-152">Entfernt die letzte Migration.</span><span class="sxs-lookup"><span data-stu-id="7a95d-152">Removes the last migration.</span></span>

<span data-ttu-id="7a95d-153">Parameter:</span><span class="sxs-lookup"><span data-stu-id="7a95d-153">Parameters:</span></span>

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| <span data-ttu-id="7a95d-154">-Force</span><span class="sxs-lookup"><span data-stu-id="7a95d-154">-Force</span></span> | <span data-ttu-id="7a95d-155">Die Migration zurückgesetzt, wenn es auf die Datenbank angewendet wurde.</span><span class="sxs-lookup"><span data-stu-id="7a95d-155">Revert the migration if it has been applied to the database.</span></span> |

### <a name="scaffold-dbcontext"></a><span data-ttu-id="7a95d-156">"Scaffold-DbContext"</span><span class="sxs-lookup"><span data-stu-id="7a95d-156">Scaffold-DbContext</span></span>

<span data-ttu-id="7a95d-157">Erstellt das Gerüst für einen DbContext und Entitätstypen Typen für eine Datenbank.</span><span class="sxs-lookup"><span data-stu-id="7a95d-157">Scaffolds a DbContext and entity types for a database.</span></span>

<span data-ttu-id="7a95d-158">Parameter:</span><span class="sxs-lookup"><span data-stu-id="7a95d-158">Parameters:</span></span>

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="7a95d-159"><nobr>***-Connection*** \<Zeichenfolge ></nobr></span><span class="sxs-lookup"><span data-stu-id="7a95d-159"><nobr>***-Connection*** \<String></nobr></span></span> | <span data-ttu-id="7a95d-160">Die Verbindungszeichenfolge in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="7a95d-160">The connection string to the database.</span></span>                                                           |
| <span data-ttu-id="7a95d-161">***-Anbieter*** \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="7a95d-161">***-Provider*** \<String></span></span>                | <span data-ttu-id="7a95d-162">Die zu verwendende Anbieter.</span><span class="sxs-lookup"><span data-stu-id="7a95d-162">The provider to use.</span></span> <span data-ttu-id="7a95d-163">(z. B. "Microsoft.entityframeworkcore.SqlServer")</span><span class="sxs-lookup"><span data-stu-id="7a95d-163">(for example, Microsoft.EntityFrameworkCore.SqlServer)</span></span>                      |
| <span data-ttu-id="7a95d-164">-OutputDir \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="7a95d-164">-OutputDir \<String></span></span>                     | <span data-ttu-id="7a95d-165">Zum Einfügen von Dateien im Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="7a95d-165">The directory to put files in.</span></span> <span data-ttu-id="7a95d-166">Pfade sind relativ zum Projektverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="7a95d-166">Paths are relative to the project directory.</span></span>                      |
| <span data-ttu-id="7a95d-167">-ContextDir \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="7a95d-167">-ContextDir \<String></span></span>                    | <span data-ttu-id="7a95d-168">Das Verzeichnis, fügen Sie in "DbContext"-Datei.</span><span class="sxs-lookup"><span data-stu-id="7a95d-168">The directory to put DbContext file in.</span></span> <span data-ttu-id="7a95d-169">Pfade sind relativ zum Projektverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="7a95d-169">Paths are relative to the project directory.</span></span>             |
| <span data-ttu-id="7a95d-170">-Kontext \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="7a95d-170">-Context \<String></span></span>                       | <span data-ttu-id="7a95d-171">Der Name von "DbContext", um zu generieren.</span><span class="sxs-lookup"><span data-stu-id="7a95d-171">The name of the DbContext to generate.</span></span>                                                           |
| <span data-ttu-id="7a95d-172">-Schemas \<String [] ></span><span class="sxs-lookup"><span data-stu-id="7a95d-172">-Schemas \<String[]></span></span>                     | <span data-ttu-id="7a95d-173">Die Schemas der Tabellen zur Generierung von Entitätstypen für.</span><span class="sxs-lookup"><span data-stu-id="7a95d-173">The schemas of tables to generate entity types for.</span></span>                                              |
| <span data-ttu-id="7a95d-174">-Tabellen \<String [] ></span><span class="sxs-lookup"><span data-stu-id="7a95d-174">-Tables \<String[]></span></span>                      | <span data-ttu-id="7a95d-175">Die Tabellen zur Generierung von Entitätstypen für werden soll.</span><span class="sxs-lookup"><span data-stu-id="7a95d-175">The tables to generate entity types for.</span></span>                                                         |
| <span data-ttu-id="7a95d-176">-"DataAnnotations"</span><span class="sxs-lookup"><span data-stu-id="7a95d-176">-DataAnnotations</span></span>                         | <span data-ttu-id="7a95d-177">Verwenden Sie Attribute, um das Modell (sofern möglich) zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="7a95d-177">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="7a95d-178">Wenn nicht angegeben, wird nur die fluent-API verwendet.</span><span class="sxs-lookup"><span data-stu-id="7a95d-178">If omitted, only the fluent API is used.</span></span> |
| <span data-ttu-id="7a95d-179">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="7a95d-179">-UseDatabaseNames</span></span>                        | <span data-ttu-id="7a95d-180">Verwenden Sie Tabellen- und Spaltennamen direkt aus der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="7a95d-180">Use table and column names directly from the database.</span></span>                                           |
| <span data-ttu-id="7a95d-181">-Force</span><span class="sxs-lookup"><span data-stu-id="7a95d-181">-Force</span></span>                                   | <span data-ttu-id="7a95d-182">Überschreiben Sie vorhandene Dateien.</span><span class="sxs-lookup"><span data-stu-id="7a95d-182">Overwrite existing files.</span></span>                                                                        |

### <a name="script-migration"></a><span data-ttu-id="7a95d-183">Script-Migration</span><span class="sxs-lookup"><span data-stu-id="7a95d-183">Script-Migration</span></span>

<span data-ttu-id="7a95d-184">Generiert ein SQL-Skript von Migrationen.</span><span class="sxs-lookup"><span data-stu-id="7a95d-184">Generates a SQL script from migrations.</span></span>

<span data-ttu-id="7a95d-185">Parameter:</span><span class="sxs-lookup"><span data-stu-id="7a95d-185">Parameters:</span></span>

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| <span data-ttu-id="7a95d-186">*– Von* \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="7a95d-186">*-From* \<String></span></span> | <span data-ttu-id="7a95d-187">Die Migration der ab.</span><span class="sxs-lookup"><span data-stu-id="7a95d-187">The starting migration.</span></span> <span data-ttu-id="7a95d-188">Der Standardwert ist 0 (die ursprüngliche Datenbank).</span><span class="sxs-lookup"><span data-stu-id="7a95d-188">Defaults to 0 (the initial database).</span></span>      |
| <span data-ttu-id="7a95d-189">*– Bis* \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="7a95d-189">*-To* \<String></span></span>   | <span data-ttu-id="7a95d-190">Der Endpunkt-Migration.</span><span class="sxs-lookup"><span data-stu-id="7a95d-190">The ending migration.</span></span> <span data-ttu-id="7a95d-191">Ist standardmäßig auf die letzte Migration ein.</span><span class="sxs-lookup"><span data-stu-id="7a95d-191">Defaults to the last migration.</span></span>              |
| <span data-ttu-id="7a95d-192">-Idempotent</span><span class="sxs-lookup"><span data-stu-id="7a95d-192">-Idempotent</span></span>       | <span data-ttu-id="7a95d-193">Generieren Sie ein Skript, das für eine Datenbank auf eine Migration verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="7a95d-193">Generate a script that can be used on a database at any migration.</span></span> |
| <span data-ttu-id="7a95d-194">-Ausgabe \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="7a95d-194">-Output \<String></span></span> | <span data-ttu-id="7a95d-195">Die Datei, schreibt das Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="7a95d-195">The file to write the result to.</span></span>                                   |

> [!TIP]
> <span data-ttu-id="7a95d-196">To, From, und Output-Parameter unterstützen die Tab-Taste.</span><span class="sxs-lookup"><span data-stu-id="7a95d-196">The To, From, and Output parameters support tab-expansion.</span></span>

### <a name="update-database"></a><span data-ttu-id="7a95d-197">Datenbank aktualisieren</span><span class="sxs-lookup"><span data-stu-id="7a95d-197">Update-Database</span></span>

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <span data-ttu-id="7a95d-198"><nobr>*-Migration* \<Zeichenfolge ></nobr></span><span class="sxs-lookup"><span data-stu-id="7a95d-198"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="7a95d-199">Die Ziel-Migration.</span><span class="sxs-lookup"><span data-stu-id="7a95d-199">The target migration.</span></span> <span data-ttu-id="7a95d-200">Wenn Sie '0' werden bei allen Migrationen rückgängig gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="7a95d-200">If '0', all migrations will be reverted.</span></span> <span data-ttu-id="7a95d-201">Ist standardmäßig auf die letzte Migration ein.</span><span class="sxs-lookup"><span data-stu-id="7a95d-201">Defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="7a95d-202">Der Parameter für die Migration unterstützt die Tab-Taste.</span><span class="sxs-lookup"><span data-stu-id="7a95d-202">The Migration parameter supports tab-expansion.</span></span>


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
