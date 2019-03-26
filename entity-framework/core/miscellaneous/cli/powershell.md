---
title: EF Core-Tools-Verweis (Paket-Manager-Konsole) – EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: cb05e3fb66adf96f8a6778711a76520d0be24c71
ms.sourcegitcommit: 645785187ae23ddf7d7b0642c7a4da5ffb0c7f30
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419769"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a><span data-ttu-id="021a8-102">Toolreferenz Entity Framework Core - Paket-Manager-Konsole in Visual Studio</span><span class="sxs-lookup"><span data-stu-id="021a8-102">Entity Framework Core tools reference - Package Manager Console in Visual Studio</span></span>

<span data-ttu-id="021a8-103">Die Paket-Manager-Konsole (PMC)-Tools für Entity Framework Core führen Sie während der Entwurfszeit-Entwicklungsaufgaben.</span><span class="sxs-lookup"><span data-stu-id="021a8-103">The Package Manager Console (PMC) tools for Entity Framework Core perform design-time development tasks.</span></span> <span data-ttu-id="021a8-104">Angenommen, sie erstellen [Migrationen](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), Anwenden von Migrationen und Code für ein Modell basierend auf einer vorhandenen Datenbank generieren.</span><span class="sxs-lookup"><span data-stu-id="021a8-104">For example, they create [migrations](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), apply migrations, and generate code for a model based on an existing database.</span></span> <span data-ttu-id="021a8-105">Ausführen der Befehle in Visual Studio mit der [-Paket-Manager-Konsole](/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="021a8-105">The commands run inside of Visual Studio using the [Package Manager Console](/nuget/tools/package-manager-console).</span></span> <span data-ttu-id="021a8-106">Diese Tools funktionieren sowohl mit .NET Framework- als auch mit .NET Core-Projekten.</span><span class="sxs-lookup"><span data-stu-id="021a8-106">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="021a8-107">Wenn Sie Visual Studio nicht verwenden, empfehlen wir die [EF Core-Befehlszeilentools](dotnet.md) stattdessen.</span><span class="sxs-lookup"><span data-stu-id="021a8-107">If you aren't using Visual Studio, we recommend the [EF Core Command-line Tools](dotnet.md) instead.</span></span> <span data-ttu-id="021a8-108">Die CLI-Tools sind – plattformübergreifend und in einer Eingabeaufforderung ausführen.</span><span class="sxs-lookup"><span data-stu-id="021a8-108">The CLI tools are cross-platform and run inside a command prompt.</span></span>

## <a name="installing-the-tools"></a><span data-ttu-id="021a8-109">Installieren der Tools</span><span class="sxs-lookup"><span data-stu-id="021a8-109">Installing the tools</span></span>

<span data-ttu-id="021a8-110">Die Verfahren zum Installieren und aktualisieren die Tools unterscheiden sich zwischen ASP.NET Core 2.1 + und früheren Versionen oder anderen Projekttypen.</span><span class="sxs-lookup"><span data-stu-id="021a8-110">The procedures for installing and updating the tools differ between ASP.NET Core 2.1+ and earlier versions or other project types.</span></span>

### <a name="aspnet-core-version-21-and-later"></a><span data-ttu-id="021a8-111">ASP.NET Core Version 2.1 und höher</span><span class="sxs-lookup"><span data-stu-id="021a8-111">ASP.NET Core version 2.1 and later</span></span>

<span data-ttu-id="021a8-112">Die Tools sind automatisch in einem Projekt auf ASP.NET Core 2.1 enthalten, da die `Microsoft.EntityFrameworkCore.Tools` Paket befindet sich auf die [Microsoft.AspNetCore.App metapaket](/aspnet/core/fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="021a8-112">The tools are automatically included in an ASP.NET Core 2.1+ project because the `Microsoft.EntityFrameworkCore.Tools` package is included in the [Microsoft.AspNetCore.App metapackage](/aspnet/core/fundamentals/metapackage-app).</span></span>

<span data-ttu-id="021a8-113">Aus diesem Grund, Sie müssen nichts tun, um die Tools installieren, jedoch müssen Sie:</span><span class="sxs-lookup"><span data-stu-id="021a8-113">Therefore, you don't have to do anything to install the tools, but you do have to:</span></span>
* <span data-ttu-id="021a8-114">Stellen Sie Pakete wieder her, bevor Sie mit den Tools in einem neuen Projekt.</span><span class="sxs-lookup"><span data-stu-id="021a8-114">Restore packages before using the tools in a new project.</span></span>
* <span data-ttu-id="021a8-115">Installieren Sie ein Paket, um die Tools auf eine neuere Version zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="021a8-115">Install a package to update the tools to a newer version.</span></span>

<span data-ttu-id="021a8-116">Um sicherzustellen, dass Sie die neueste Version der Tools erhalten, empfehlen wir, dass Sie auch die folgende Schritte durchführen:</span><span class="sxs-lookup"><span data-stu-id="021a8-116">To make sure that you're getting the latest version of the tools, we recommend that you also do the following step:</span></span>

* <span data-ttu-id="021a8-117">Bearbeiten Ihrer *csproj* -Datei und fügen Sie eine Zeile, in die neueste Version von der [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) Paket.</span><span class="sxs-lookup"><span data-stu-id="021a8-117">Edit your *.csproj* file and add a line specifying the latest version of the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) package.</span></span> <span data-ttu-id="021a8-118">Z. B. die *csproj* Datei sind zum Beispiel eine `ItemGroup` , sieht folgendermaßen aus:</span><span class="sxs-lookup"><span data-stu-id="021a8-118">For example, the *.csproj* file might include an `ItemGroup` that looks like this:</span></span>

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

<span data-ttu-id="021a8-119">Aktualisieren Sie die Tools aus, wenn eine Meldung wie im folgenden Beispiel angezeigt:</span><span class="sxs-lookup"><span data-stu-id="021a8-119">Update the tools when you get a message like the following example:</span></span>

> <span data-ttu-id="021a8-120">Die EF Core-Tools-Version "2.1.1-rtm-30846" ist älter als der die Laufzeit "2.1.3-rtm-32065".</span><span class="sxs-lookup"><span data-stu-id="021a8-120">The EF Core tools version '2.1.1-rtm-30846' is older than that of the runtime '2.1.3-rtm-32065'.</span></span> <span data-ttu-id="021a8-121">Aktualisieren Sie die Tools für die neuesten Features und Fehlerbehebungen.</span><span class="sxs-lookup"><span data-stu-id="021a8-121">Update the tools for the latest features and bug fixes.</span></span>

<span data-ttu-id="021a8-122">So aktualisieren Sie die Tools:</span><span class="sxs-lookup"><span data-stu-id="021a8-122">To update the tools:</span></span>
* <span data-ttu-id="021a8-123">Installieren Sie das neueste .NET Core SDK an.</span><span class="sxs-lookup"><span data-stu-id="021a8-123">Install the latest .NET Core SDK.</span></span>
* <span data-ttu-id="021a8-124">Visual Studio auf die neueste Version zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="021a8-124">Update Visual Studio to the latest version.</span></span>
* <span data-ttu-id="021a8-125">Bearbeiten der *csproj* Datei, sodass sie einen Paketverweis auf die neuesten Tools-Paket enthält, wie oben beschrieben.</span><span class="sxs-lookup"><span data-stu-id="021a8-125">Edit the *.csproj* file so that it includes a package reference to the latest tools package, as shown earlier.</span></span>

### <a name="other-versions-and-project-types"></a><span data-ttu-id="021a8-126">Andere Versionen und Projekttypen</span><span class="sxs-lookup"><span data-stu-id="021a8-126">Other versions and project types</span></span>

<span data-ttu-id="021a8-127">Installieren Sie die Tools-Paket-Manager-Konsole mithilfe des folgenden Befehls **-Paket-Manager-Konsole**:</span><span class="sxs-lookup"><span data-stu-id="021a8-127">Install the Package Manager Console tools by running the following command in **Package Manager Console**:</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="021a8-128">Aktualisieren Sie die Tools, mithilfe des folgenden Befehls **-Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="021a8-128">Update the tools by running the following command in **Package Manager Console**.</span></span>

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a><span data-ttu-id="021a8-129">Überprüfen Sie die installation</span><span class="sxs-lookup"><span data-stu-id="021a8-129">Verify the installation</span></span>

<span data-ttu-id="021a8-130">Stellen Sie sicher, dass die Tools installiert sind, mit dem folgenden Befehl:</span><span class="sxs-lookup"><span data-stu-id="021a8-130">Verify that the tools are installed by running this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```

<span data-ttu-id="021a8-131">Die Ausgabe sieht folgendermaßen aus (nicht teilt es Ihnen die Version der Tools, die Sie verwenden):</span><span class="sxs-lookup"><span data-stu-id="021a8-131">The output looks like this (it doesn't tell you which version of the tools you're using):</span></span>

```console

                     _/\__
               ---==/    \\
         ___  ___   |.    \|\
        | __|| __|  |  )   \\\
        | _| | _|   \_/ |  //|\\
        |___||_|       /   \\\/\\

TOPIC
    about_EntityFrameworkCore

SHORT DESCRIPTION
    Provides information about the Entity Framework Core Package Manager Console Tools.

<A list of available commands follows, omitted here.>
```

## <a name="using-the-tools"></a><span data-ttu-id="021a8-132">Mithilfe der Tools</span><span class="sxs-lookup"><span data-stu-id="021a8-132">Using the tools</span></span>

<span data-ttu-id="021a8-133">Bevor Sie mit den Tools:</span><span class="sxs-lookup"><span data-stu-id="021a8-133">Before using the tools:</span></span>
* <span data-ttu-id="021a8-134">Erfahren Sie, den Unterschied zwischen Ziel- und Startup-Projekt.</span><span class="sxs-lookup"><span data-stu-id="021a8-134">Understand the difference between target and startup project.</span></span>
* <span data-ttu-id="021a8-135">Erfahren Sie, wie Sie die Tools mit .NET Standard-Klassenbibliotheken verwenden.</span><span class="sxs-lookup"><span data-stu-id="021a8-135">Learn how to use the tools with .NET Standard class libraries.</span></span>
* <span data-ttu-id="021a8-136">Legen Sie die Umgebung für ASP.NET Core-Projekte.</span><span class="sxs-lookup"><span data-stu-id="021a8-136">For ASP.NET Core projects, set the environment.</span></span>

### <a name="target-and-startup-project"></a><span data-ttu-id="021a8-137">Ziel und Startup-Projekt</span><span class="sxs-lookup"><span data-stu-id="021a8-137">Target and startup project</span></span>

<span data-ttu-id="021a8-138">Die Befehle finden Sie in einem *Projekt* und ein *Startprojekt*.</span><span class="sxs-lookup"><span data-stu-id="021a8-138">The commands refer to a *project* and a *startup project*.</span></span>

* <span data-ttu-id="021a8-139">Die *Projekt* ist auch bekannt als die *Zielprojekt* da es sich handelt, in dem Sie die Befehle zum Hinzufügen oder Entfernen von Dateien.</span><span class="sxs-lookup"><span data-stu-id="021a8-139">The *project* is also known as the *target project* because it's where the commands add or remove files.</span></span> <span data-ttu-id="021a8-140">In der Standardeinstellung die **Standardprojekt** im **-Paket-Manager-Konsole** ist das Zielprojekt.</span><span class="sxs-lookup"><span data-stu-id="021a8-140">By default, the **Default project** selected in **Package Manager Console** is the target project.</span></span> <span data-ttu-id="021a8-141">Sie können ein anderes Projekt als Zielprojekt angeben, mit der <nobr> `--project` </nobr> Option.</span><span class="sxs-lookup"><span data-stu-id="021a8-141">You can specify a different project as target project by using the <nobr>`--project`</nobr> option.</span></span>

* <span data-ttu-id="021a8-142">Die *Startprojekt* ist diejenige, die die Tools erstellen und ausführen.</span><span class="sxs-lookup"><span data-stu-id="021a8-142">The *startup project* is the one that the tools build and run.</span></span> <span data-ttu-id="021a8-143">Die Tools werden zum Ausführen der Anwendungscode zur Entwurfszeit zum Abrufen von Informationen über das Projekt, z. B. die Datenbank-Verbindungszeichenfolge und die Konfiguration des Modells.</span><span class="sxs-lookup"><span data-stu-id="021a8-143">The tools have to execute application code at design time to get information about the project, such as the database connection string and the configuration of the model.</span></span> <span data-ttu-id="021a8-144">In der Standardeinstellung die **Startprojekt** in **Projektmappen-Explorer** das Startprojekt ist.</span><span class="sxs-lookup"><span data-stu-id="021a8-144">By default, the **Startup Project** in **Solution Explorer** is the startup project.</span></span> <span data-ttu-id="021a8-145">Sie können ein anderes Projekt als Startprojekt angeben, mit der <nobr> `--startup-project` </nobr> Option.</span><span class="sxs-lookup"><span data-stu-id="021a8-145">You can specify a different project as startup project by using the <nobr>`--startup-project`</nobr> option.</span></span>

<span data-ttu-id="021a8-146">Das Startprojekt und ein Zielprojekt sind häufig die gleichen Projekt.</span><span class="sxs-lookup"><span data-stu-id="021a8-146">The startup project and target project are often the same project.</span></span> <span data-ttu-id="021a8-147">Ein typisches Szenario, in dem sie separate Projekte sind, ist:</span><span class="sxs-lookup"><span data-stu-id="021a8-147">A typical scenario where they are separate projects is when:</span></span>

* <span data-ttu-id="021a8-148">Die EF Core-Kontext und Entitätsklassen-Klassen sind in einer .NET Core-Klassenbibliothek.</span><span class="sxs-lookup"><span data-stu-id="021a8-148">The EF Core context and entity classes are in a .NET Core class library.</span></span>
* <span data-ttu-id="021a8-149">Ein .NET Core-Konsolen-app oder Web-app verweist auf die Klassenbibliothek.</span><span class="sxs-lookup"><span data-stu-id="021a8-149">A .NET Core console app or web app references the class library.</span></span>

<span data-ttu-id="021a8-150">Es ist auch möglich, [platzieren migrationscode in eine Klassenbibliothek, die getrennt von den Entity Framework Core-Kontext](xref:core/managing-schemas/migrations/projects).</span><span class="sxs-lookup"><span data-stu-id="021a8-150">It's also possible to [put migrations code in a class library separate from the EF Core context](xref:core/managing-schemas/migrations/projects).</span></span>

### <a name="other-target-frameworks"></a><span data-ttu-id="021a8-151">Andere Zielframeworks</span><span class="sxs-lookup"><span data-stu-id="021a8-151">Other target frameworks</span></span>

<span data-ttu-id="021a8-152">Die Paket-Manager-Konsole-Tools funktionieren mit .NET Core oder .NET Framework-Projekte.</span><span class="sxs-lookup"><span data-stu-id="021a8-152">The Package Manager Console tools work with .NET Core or .NET Framework projects.</span></span> <span data-ttu-id="021a8-153">Apps, die EF Core-Modell in einer .NET Standard-Klassenbibliothek verfügen, möglicherweise keiner .NET Core- oder .NET Framework-Projekt.</span><span class="sxs-lookup"><span data-stu-id="021a8-153">Apps that have the EF Core model in a .NET Standard class library might not have a .NET Core or .NET Framework project.</span></span> <span data-ttu-id="021a8-154">Dies ist z. B. "true" mit Xamarin und die universelle Windows-Plattform-apps.</span><span class="sxs-lookup"><span data-stu-id="021a8-154">For example, this is true of Xamarin and Universal Windows Platform apps.</span></span> <span data-ttu-id="021a8-155">In solchen Fällen können Sie ein .NET Core oder .NET Framework-Konsolenanwendungsprojekt erstellen, dessen einziger Zweck ist es, als Startprojekt für die Tools zu fungieren.</span><span class="sxs-lookup"><span data-stu-id="021a8-155">In such cases, you can create a .NET Core or .NET Framework console app project whose only purpose is to act as startup project for the tools.</span></span> <span data-ttu-id="021a8-156">Das Projekt kann ein dummyprojekt ohne realen Code sein &mdash; ist nur erforderlich, ein Ziel für die Tools bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="021a8-156">The project can be a dummy project with no real code &mdash; it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="021a8-157">Warum ist ein dummyprojekt erforderlich?</span><span class="sxs-lookup"><span data-stu-id="021a8-157">Why is a dummy project required?</span></span> <span data-ttu-id="021a8-158">Wie bereits erwähnt, haben die Tools zum Ausführen der Anwendungscode zur Entwurfszeit an.</span><span class="sxs-lookup"><span data-stu-id="021a8-158">As mentioned earlier, the tools have to execute application code at design time.</span></span> <span data-ttu-id="021a8-159">Zu diesem Zweck müssen sie die .NET Core oder .NET Framework-Laufzeit verwenden.</span><span class="sxs-lookup"><span data-stu-id="021a8-159">To do that, they need to use the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="021a8-160">Wenn die EF Core-Modell in einem Projekt, die auf .NET Core oder .NET Framework abzielt ist, nutzen die EF Core-Tools die Laufzeit aus dem Projekt.</span><span class="sxs-lookup"><span data-stu-id="021a8-160">When the EF Core model is in a project that targets .NET Core or .NET Framework, the EF Core tools borrow the runtime from the project.</span></span> <span data-ttu-id="021a8-161">Sie können nicht dies tun, wenn das EF Core-Modell in einer .NET Standard-Klassenbibliothek ist.</span><span class="sxs-lookup"><span data-stu-id="021a8-161">They can't do that if the EF Core model is in a .NET Standard class library.</span></span> <span data-ttu-id="021a8-162">.NET Standard ist keine tatsächliche Implementierung von .NET. Es ist eine Spezifikation eines Satzes von APIs, die .NET-Implementierungen unterstützt wird müssen.</span><span class="sxs-lookup"><span data-stu-id="021a8-162">The .NET Standard is not an actual .NET implementation; it's a specification of a set of APIs that .NET implementations must support.</span></span> <span data-ttu-id="021a8-163">Aus diesem Grund ist .NET Standard nicht ausreichend für die EF Core-Tools, um Anwendungscode auszuführen.</span><span class="sxs-lookup"><span data-stu-id="021a8-163">Therefore .NET Standard is not sufficient for the EF Core tools to execute application code.</span></span> <span data-ttu-id="021a8-164">Dem dummyprojekt, die Sie erstellen, für die Verwendung als Startup-Projekt enthält eine konkrete Zielplattform, die in der die Tools die .NET Standard-Klassenbibliothek laden können.</span><span class="sxs-lookup"><span data-stu-id="021a8-164">The dummy project you create to use as startup project provides a concrete target platform into which the tools can load the .NET Standard class library.</span></span>

### <a name="aspnet-core-environment"></a><span data-ttu-id="021a8-165">ASP.NET Core-Umgebung</span><span class="sxs-lookup"><span data-stu-id="021a8-165">ASP.NET Core environment</span></span>

<span data-ttu-id="021a8-166">Legen Sie zum Angeben der Umgebung für ASP.NET Core-Projekten **Env:ASPNETCORE_ENVIRONMENT** vor dem Ausführen der Befehle.</span><span class="sxs-lookup"><span data-stu-id="021a8-166">To specify the environment for ASP.NET Core projects, set **env:ASPNETCORE_ENVIRONMENT** before running commands.</span></span>

## <a name="common-parameters"></a><span data-ttu-id="021a8-167">Allgemeine Parameter</span><span class="sxs-lookup"><span data-stu-id="021a8-167">Common parameters</span></span>

<span data-ttu-id="021a8-168">Die folgende Tabelle zeigt die Parameter, die für alle EF Core-Befehle gelten:</span><span class="sxs-lookup"><span data-stu-id="021a8-168">The following table shows parameters that are common to all of the EF Core commands:</span></span>

| <span data-ttu-id="021a8-169">Parameter</span><span class="sxs-lookup"><span data-stu-id="021a8-169">Parameter</span></span>                 | <span data-ttu-id="021a8-170">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="021a8-170">Description</span></span>                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="021a8-171">-Kontext \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="021a8-171">-Context \<String></span></span>        | <span data-ttu-id="021a8-172">Die `DbContext`-Klasse, die verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="021a8-172">The `DbContext` class to use.</span></span> <span data-ttu-id="021a8-173">Name der Klasse nur oder mit Namespaces vollqualifiziert.</span><span class="sxs-lookup"><span data-stu-id="021a8-173">Class name only or fully qualified with namespaces.</span></span>  <span data-ttu-id="021a8-174">Wenn dieser Parameter ausgelassen wird, sucht Entity Framework Core die Context-Klasse.</span><span class="sxs-lookup"><span data-stu-id="021a8-174">If this parameter is omitted, EF Core finds the context class.</span></span> <span data-ttu-id="021a8-175">Dieser Parameter ist erforderlich, wenn es mehrere Kontextklassen sind.</span><span class="sxs-lookup"><span data-stu-id="021a8-175">If there are multiple context classes, this parameter is required.</span></span> |
| <span data-ttu-id="021a8-176">-Project \<String></span><span class="sxs-lookup"><span data-stu-id="021a8-176">-Project \<String></span></span>        | <span data-ttu-id="021a8-177">Das Zielprojekt.</span><span class="sxs-lookup"><span data-stu-id="021a8-177">The target project.</span></span> <span data-ttu-id="021a8-178">Wenn dieser Parameter ausgelassen wird, die **Standardprojekt** für **-Paket-Manager-Konsole** als das Zielprojekt verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="021a8-178">If this parameter is omitted, the **Default project** for **Package Manager Console** is used as the target project.</span></span>                                                                             |
| <span data-ttu-id="021a8-179">-StartupProject \<String></span><span class="sxs-lookup"><span data-stu-id="021a8-179">-StartupProject \<String></span></span> | <span data-ttu-id="021a8-180">Das Startprojekt.</span><span class="sxs-lookup"><span data-stu-id="021a8-180">The startup project.</span></span> <span data-ttu-id="021a8-181">Wenn dieser Parameter ausgelassen wird, die **Startprojekt** in **Projektmappeneigenschaften** als das Zielprojekt verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="021a8-181">If this parameter is omitted, the **Startup project** in **Solution properties** is used as the target project.</span></span>                                                                                 |
| <span data-ttu-id="021a8-182">-Verbose</span><span class="sxs-lookup"><span data-stu-id="021a8-182">-Verbose</span></span>                  | <span data-ttu-id="021a8-183">Zeigen Sie ausführlichen Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="021a8-183">Show verbose output.</span></span>                                                                                                                                                                                                 |

<span data-ttu-id="021a8-184">Um Hilfeinformationen zu einem Befehl zu anzuzeigen, verwenden Sie PowerShell `Get-Help` Befehl.</span><span class="sxs-lookup"><span data-stu-id="021a8-184">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="021a8-185">Der Kontext, Projekt- und StartupProject parameterunterstützung Tab-Taste.</span><span class="sxs-lookup"><span data-stu-id="021a8-185">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

## <a name="add-migration"></a><span data-ttu-id="021a8-186">Add-Migration</span><span class="sxs-lookup"><span data-stu-id="021a8-186">Add-Migration</span></span>

<span data-ttu-id="021a8-187">Fügt eine neue Migration hinzu.</span><span class="sxs-lookup"><span data-stu-id="021a8-187">Adds a new migration.</span></span>

<span data-ttu-id="021a8-188">Parameter:</span><span class="sxs-lookup"><span data-stu-id="021a8-188">Parameters:</span></span>

| <span data-ttu-id="021a8-189">Parameter</span><span class="sxs-lookup"><span data-stu-id="021a8-189">Parameter</span></span>                         | <span data-ttu-id="021a8-190">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="021a8-190">Description</span></span>                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="021a8-191"><nobr>-Name \<String><nobr></span><span class="sxs-lookup"><span data-stu-id="021a8-191"><nobr>-Name \<String><nobr></span></span>       | <span data-ttu-id="021a8-192">Der Name der Migration.</span><span class="sxs-lookup"><span data-stu-id="021a8-192">The name of the migration.</span></span> <span data-ttu-id="021a8-193">Dies ist ein Positionsparameter und ist erforderlich.</span><span class="sxs-lookup"><span data-stu-id="021a8-193">This is a positional parameter and is required.</span></span>                                              |
| <span data-ttu-id="021a8-194"><nobr>-OutputDir \<String></nobr></span><span class="sxs-lookup"><span data-stu-id="021a8-194"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="021a8-195">Das Verzeichnis (und Sub-Namespace) verwenden.</span><span class="sxs-lookup"><span data-stu-id="021a8-195">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="021a8-196">Pfade sind relativ zum Zielprojektverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="021a8-196">Paths are relative to the target project directory.</span></span> <span data-ttu-id="021a8-197">Der Standardwert ist "Migrations".</span><span class="sxs-lookup"><span data-stu-id="021a8-197">Defaults to "Migrations".</span></span> |

## <a name="drop-database"></a><span data-ttu-id="021a8-198">Drop-Database</span><span class="sxs-lookup"><span data-stu-id="021a8-198">Drop-Database</span></span>

<span data-ttu-id="021a8-199">Löscht die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="021a8-199">Drops the database.</span></span>

<span data-ttu-id="021a8-200">Parameter:</span><span class="sxs-lookup"><span data-stu-id="021a8-200">Parameters:</span></span>

| <span data-ttu-id="021a8-201">Parameter</span><span class="sxs-lookup"><span data-stu-id="021a8-201">Parameter</span></span> | <span data-ttu-id="021a8-202">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="021a8-202">Description</span></span>                                              |
|:----------|:---------------------------------------------------------|
| <span data-ttu-id="021a8-203">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="021a8-203">-WhatIf</span></span>   | <span data-ttu-id="021a8-204">Anzeigen der Datenbank gelöscht werden würde, dies jedoch noch nicht.</span><span class="sxs-lookup"><span data-stu-id="021a8-204">Show which database would be dropped, but don't drop it.</span></span> |

## <a name="get-dbcontext"></a><span data-ttu-id="021a8-205">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="021a8-205">Get-DbContext</span></span>

<span data-ttu-id="021a8-206">Ruft Informationen über eine `DbContext` Typ.</span><span class="sxs-lookup"><span data-stu-id="021a8-206">Gets information about a `DbContext` type.</span></span>

## <a name="remove-migration"></a><span data-ttu-id="021a8-207">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="021a8-207">Remove-Migration</span></span>

<span data-ttu-id="021a8-208">Entfernt die letzte Migration (Rollback die codeänderungen, die für die Migration durchgeführt wurden).</span><span class="sxs-lookup"><span data-stu-id="021a8-208">Removes the last migration (rolls back the code changes that were done for the migration).</span></span>

<span data-ttu-id="021a8-209">Parameter:</span><span class="sxs-lookup"><span data-stu-id="021a8-209">Parameters:</span></span>

| <span data-ttu-id="021a8-210">Parameter</span><span class="sxs-lookup"><span data-stu-id="021a8-210">Parameter</span></span> | <span data-ttu-id="021a8-211">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="021a8-211">Description</span></span>                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| <span data-ttu-id="021a8-212">-Force</span><span class="sxs-lookup"><span data-stu-id="021a8-212">-Force</span></span>    | <span data-ttu-id="021a8-213">Zurücksetzen die Migration (ein Rollback die Änderungen, die auf die Datenbank angewendet wurden).</span><span class="sxs-lookup"><span data-stu-id="021a8-213">Revert the migration (roll back the changes that were applied to the database).</span></span> |

## <a name="scaffold-dbcontext"></a><span data-ttu-id="021a8-214">Scaffold-DbContext</span><span class="sxs-lookup"><span data-stu-id="021a8-214">Scaffold-DbContext</span></span>

<span data-ttu-id="021a8-215">Generiert Code für eine `DbContext` und Entitätstypen für eine Datenbank.</span><span class="sxs-lookup"><span data-stu-id="021a8-215">Generates code for a `DbContext` and entity types for a database.</span></span> <span data-ttu-id="021a8-216">In der Reihenfolge für `Scaffold-DbContext` zum Generieren eines Entitätstyps muss in der Datenbanktabelle über einen Primärschlüssel verfügen.</span><span class="sxs-lookup"><span data-stu-id="021a8-216">In order for `Scaffold-DbContext` to generate an entity type, the database table must have a primary key.</span></span>

<span data-ttu-id="021a8-217">Parameter:</span><span class="sxs-lookup"><span data-stu-id="021a8-217">Parameters:</span></span>

| <span data-ttu-id="021a8-218">Parameter</span><span class="sxs-lookup"><span data-stu-id="021a8-218">Parameter</span></span>                          | <span data-ttu-id="021a8-219">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="021a8-219">Description</span></span>                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="021a8-220"><nobr>-Connection \<Zeichenfolge ></nobr></span><span class="sxs-lookup"><span data-stu-id="021a8-220"><nobr>-Connection \<String></nobr></span></span> | <span data-ttu-id="021a8-221">Die Verbindungszeichenfolge in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="021a8-221">The connection string to the database.</span></span> <span data-ttu-id="021a8-222">Der Wert kann für ASP.NET Core 2.x-Projekte sein *Name =\<Name der Verbindungszeichenfolge >*.</span><span class="sxs-lookup"><span data-stu-id="021a8-222">For ASP.NET Core 2.x projects, the value can be *name=\<name of connection string>*.</span></span> <span data-ttu-id="021a8-223">In diesem Fall stammt der Name der Konfigurationsquellen, die für das Projekt eingerichtet sind.</span><span class="sxs-lookup"><span data-stu-id="021a8-223">In that case the name comes from the configuration sources that are set up for the project.</span></span> <span data-ttu-id="021a8-224">Dies ist ein Positionsparameter und ist erforderlich.</span><span class="sxs-lookup"><span data-stu-id="021a8-224">This is a positional parameter and is required.</span></span> |
| <span data-ttu-id="021a8-225"><nobr>-Anbieter \<Zeichenfolge ></nobr></span><span class="sxs-lookup"><span data-stu-id="021a8-225"><nobr>-Provider \<String></nobr></span></span>   | <span data-ttu-id="021a8-226">Die zu verwendende Anbieter.</span><span class="sxs-lookup"><span data-stu-id="021a8-226">The provider to use.</span></span> <span data-ttu-id="021a8-227">Normalerweise hat der Name des NuGet-Pakets, z. B.: `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="021a8-227">Typically this is the name of the NuGet package, for example: `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="021a8-228">Dies ist ein Positionsparameter und ist erforderlich.</span><span class="sxs-lookup"><span data-stu-id="021a8-228">This is a positional parameter and is required.</span></span>                                                                                           |
| <span data-ttu-id="021a8-229">-OutputDir \<String></span><span class="sxs-lookup"><span data-stu-id="021a8-229">-OutputDir \<String></span></span>               | <span data-ttu-id="021a8-230">Zum Einfügen von Dateien im Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="021a8-230">The directory to put files in.</span></span> <span data-ttu-id="021a8-231">Pfade sind relativ zum Projektverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="021a8-231">Paths are relative to the project directory.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="021a8-232">-ContextDir \<String></span><span class="sxs-lookup"><span data-stu-id="021a8-232">-ContextDir \<String></span></span>              | <span data-ttu-id="021a8-233">Verzeichnis zum Ablegen der `DbContext` Datei.</span><span class="sxs-lookup"><span data-stu-id="021a8-233">The directory to put the `DbContext` file in.</span></span> <span data-ttu-id="021a8-234">Pfade sind relativ zum Projektverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="021a8-234">Paths are relative to the project directory.</span></span>                                                                                                                                                                              |
| <span data-ttu-id="021a8-235">-Kontext \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="021a8-235">-Context \<String></span></span>                 | <span data-ttu-id="021a8-236">Der Name des der `DbContext` Klasse generiert.</span><span class="sxs-lookup"><span data-stu-id="021a8-236">The name of the `DbContext` class to generate.</span></span>                                                                                                                                                                                                                          |
| <span data-ttu-id="021a8-237">-Schemas \<String [] ></span><span class="sxs-lookup"><span data-stu-id="021a8-237">-Schemas \<String[]></span></span>               | <span data-ttu-id="021a8-238">Die Schemas der Tabellen zur Generierung von Entitätstypen für.</span><span class="sxs-lookup"><span data-stu-id="021a8-238">The schemas of tables to generate entity types for.</span></span> <span data-ttu-id="021a8-239">Wenn dieser Parameter ausgelassen wird, sind alle Schemas enthalten.</span><span class="sxs-lookup"><span data-stu-id="021a8-239">If this parameter is omitted, all schemas are included.</span></span>                                                                                                                                                             |
| <span data-ttu-id="021a8-240">-Tabellen \<String [] ></span><span class="sxs-lookup"><span data-stu-id="021a8-240">-Tables \<String[]></span></span>                | <span data-ttu-id="021a8-241">Die Tabellen zur Generierung von Entitätstypen für werden soll.</span><span class="sxs-lookup"><span data-stu-id="021a8-241">The tables to generate entity types for.</span></span> <span data-ttu-id="021a8-242">Wenn dieser Parameter ausgelassen wird, sind alle Tabellen enthalten.</span><span class="sxs-lookup"><span data-stu-id="021a8-242">If this parameter is omitted, all tables are included.</span></span>                                                                                                                                                                         |
| <span data-ttu-id="021a8-243">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="021a8-243">-DataAnnotations</span></span>                   | <span data-ttu-id="021a8-244">Verwenden Sie Attribute, um das Modell (sofern möglich) zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="021a8-244">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="021a8-245">Wenn dieser Parameter ausgelassen wird, wird nur die fluent-API verwendet.</span><span class="sxs-lookup"><span data-stu-id="021a8-245">If this parameter is omitted, only the fluent API is used.</span></span>                                                                                                                                                      |
| <span data-ttu-id="021a8-246">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="021a8-246">-UseDatabaseNames</span></span>                  | <span data-ttu-id="021a8-247">Verwenden Sie Tabellen- und Spaltennamen, genau wie in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="021a8-247">Use table and column names exactly as they appear in the database.</span></span> <span data-ttu-id="021a8-248">Wenn dieser Parameter ausgelassen wird, werden die Datenbanknamen geändert, um genauer zu Konventionen für C#-Namen entsprechen.</span><span class="sxs-lookup"><span data-stu-id="021a8-248">If this parameter is omitted, database names are changed to more closely conform to C# name style conventions.</span></span>                                                                                       |
| <span data-ttu-id="021a8-249">-Force</span><span class="sxs-lookup"><span data-stu-id="021a8-249">-Force</span></span>                             | <span data-ttu-id="021a8-250">Überschreiben Sie vorhandene Dateien.</span><span class="sxs-lookup"><span data-stu-id="021a8-250">Overwrite existing files.</span></span>                                                                                                                                                                                                                                               |

<span data-ttu-id="021a8-251">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="021a8-251">Example:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="021a8-252">Ein Beispiel, die nur ausgewählte Tabellen erstellt das Gerüst für den Kontext in einem separaten Ordner mit dem angegebenen Namen erstellt:</span><span class="sxs-lookup"><span data-stu-id="021a8-252">Example that scaffolds only selected tables and creates the context in a separate folder with a specified name:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a><span data-ttu-id="021a8-253">Script-Migration</span><span class="sxs-lookup"><span data-stu-id="021a8-253">Script-Migration</span></span>

<span data-ttu-id="021a8-254">Generiert ein SQL­Skript, das alle Änderungen von einem ausgewählten Migration auf die Migration zu einem anderen ausgewählten angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="021a8-254">Generates a SQL script that applies all of the changes from one selected migration to another selected migration.</span></span>

<span data-ttu-id="021a8-255">Parameter:</span><span class="sxs-lookup"><span data-stu-id="021a8-255">Parameters:</span></span>

| <span data-ttu-id="021a8-256">Parameter</span><span class="sxs-lookup"><span data-stu-id="021a8-256">Parameter</span></span>                | <span data-ttu-id="021a8-257">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="021a8-257">Description</span></span>                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="021a8-258">*-From* \<String></span><span class="sxs-lookup"><span data-stu-id="021a8-258">*-From* \<String></span></span>        | <span data-ttu-id="021a8-259">Die Migration der ab.</span><span class="sxs-lookup"><span data-stu-id="021a8-259">The starting migration.</span></span> <span data-ttu-id="021a8-260">Migrationen können nach Name oder ID identifiziert werden</span><span class="sxs-lookup"><span data-stu-id="021a8-260">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="021a8-261">Die Zahl 0 ist ein Sonderfall, das bedeutet, dass *vor der ersten Migration*.</span><span class="sxs-lookup"><span data-stu-id="021a8-261">The number 0 is a special case that means *before the first migration*.</span></span> <span data-ttu-id="021a8-262">Der Standardwert ist 0.</span><span class="sxs-lookup"><span data-stu-id="021a8-262">Defaults to 0.</span></span>                                                              |
| <span data-ttu-id="021a8-263">*-To* \<String></span><span class="sxs-lookup"><span data-stu-id="021a8-263">*-To* \<String></span></span>          | <span data-ttu-id="021a8-264">Der Endpunkt-Migration.</span><span class="sxs-lookup"><span data-stu-id="021a8-264">The ending migration.</span></span> <span data-ttu-id="021a8-265">Ist standardmäßig auf die letzte Migration ein.</span><span class="sxs-lookup"><span data-stu-id="021a8-265">Defaults to the last migration.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="021a8-266"><nobr>-Idempotent</nobr></span><span class="sxs-lookup"><span data-stu-id="021a8-266"><nobr>-Idempotent</nobr></span></span> | <span data-ttu-id="021a8-267">Generieren Sie ein Skript, das für eine Datenbank auf eine Migration verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="021a8-267">Generate a script that can be used on a database at any migration.</span></span>                                                                                                                                                         |
| <span data-ttu-id="021a8-268">-Ausgabe \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="021a8-268">-Output \<String></span></span>        | <span data-ttu-id="021a8-269">Die Datei, schreibt das Ergebnis.</span><span class="sxs-lookup"><span data-stu-id="021a8-269">The file to write the result to.</span></span> <span data-ttu-id="021a8-270">Wenn dieser Parameter ausgelassen wird, wird die Datei mit einem generierten Namen im gleichen Ordner erstellt, wie die app Runtime-Dateien, z. B. erstellt werden: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span><span class="sxs-lookup"><span data-stu-id="021a8-270">IF this parameter is omitted, the file is created with a generated name in the same folder as the app's runtime files are created, for example: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span></span> |

> [!TIP]
> <span data-ttu-id="021a8-271">To, From, und Output-Parameter unterstützen die Tab-Taste.</span><span class="sxs-lookup"><span data-stu-id="021a8-271">The To, From, and Output parameters support tab-expansion.</span></span>

<span data-ttu-id="021a8-272">Das folgende Beispiel erstellt ein Skript für die InitialCreate-Migration, die mit dem Migrationsnamen.</span><span class="sxs-lookup"><span data-stu-id="021a8-272">The following example creates a script for the InitialCreate migration, using the migration name.</span></span>

```powershell
Script-Migration -To InitialCreate
```

<span data-ttu-id="021a8-273">Das folgende Beispiel erstellt ein Skript für alle Migrationen nach der Migration InitialCreate mithilfe der Migration-ID.</span><span class="sxs-lookup"><span data-stu-id="021a8-273">The following example creates a script for all migrations after the InitialCreate migration, using the migration ID.</span></span>

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a><span data-ttu-id="021a8-274">Update-Database</span><span class="sxs-lookup"><span data-stu-id="021a8-274">Update-Database</span></span>

<span data-ttu-id="021a8-275">Aktualisiert die Datenbank aus, um die letzte Migration oder für eine angegebene Migration.</span><span class="sxs-lookup"><span data-stu-id="021a8-275">Updates the database to the last migration or to a specified migration.</span></span>

| <span data-ttu-id="021a8-276">Parameter</span><span class="sxs-lookup"><span data-stu-id="021a8-276">Parameter</span></span>                           | <span data-ttu-id="021a8-277">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="021a8-277">Description</span></span>                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="021a8-278"><nobr>*-Migration* \<String></nobr></span><span class="sxs-lookup"><span data-stu-id="021a8-278"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="021a8-279">Die Ziel-Migration.</span><span class="sxs-lookup"><span data-stu-id="021a8-279">The target migration.</span></span> <span data-ttu-id="021a8-280">Migrationen können nach Name oder ID identifiziert werden</span><span class="sxs-lookup"><span data-stu-id="021a8-280">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="021a8-281">Die Zahl 0 ist ein Sonderfall, das bedeutet, dass *vor der ersten Migration* und bewirkt, dass alle Migrationen zu rückgängig gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="021a8-281">The number 0 is a special case that means *before the first migration* and causes all migrations to be reverted.</span></span> <span data-ttu-id="021a8-282">Wenn keine Migration angegeben ist, verwendet der Befehl auf die letzte Migration.</span><span class="sxs-lookup"><span data-stu-id="021a8-282">If no migration is specified, the command defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="021a8-283">Der Parameter für die Migration unterstützt die Tab-Taste.</span><span class="sxs-lookup"><span data-stu-id="021a8-283">The Migration parameter supports tab-expansion.</span></span>

<span data-ttu-id="021a8-284">Im folgende Beispiel wird bei allen Migrationen zurückgesetzt.</span><span class="sxs-lookup"><span data-stu-id="021a8-284">The following example reverts all migrations.</span></span>

```powershell
Update-Database -Migration 0
```

<span data-ttu-id="021a8-285">In den folgenden Beispielen wird die Datenbank für eine angegebene Migration aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="021a8-285">The following examples update the database to a specified migration.</span></span> <span data-ttu-id="021a8-286">Das erste verwendet den Migrationsnamen aus, und die zweite verwendet den Migrations-ID:</span><span class="sxs-lookup"><span data-stu-id="021a8-286">The first uses the migration name and the second uses the migration ID:</span></span>

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```

## <a name="additional-resources"></a><span data-ttu-id="021a8-287">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="021a8-287">Additional resources</span></span>

* [<span data-ttu-id="021a8-288">Migrationen</span><span class="sxs-lookup"><span data-stu-id="021a8-288">Migrations</span></span>](xref:core/managing-schemas/migrations/index)
* [<span data-ttu-id="021a8-289">Reverse Engineering</span><span class="sxs-lookup"><span data-stu-id="021a8-289">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
