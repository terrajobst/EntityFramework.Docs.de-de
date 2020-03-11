---
title: Referenz zu EF Core Tools (Paket-Manager-Konsole)-EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: a9ce6d5b5f36a72e3715a9de787f1f00e989a58c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414200"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a><span data-ttu-id="17a54-102">Referenz zur Entity Framework Core Tools-Paket-Manager-Konsole in Visual Studio</span><span class="sxs-lookup"><span data-stu-id="17a54-102">Entity Framework Core tools reference - Package Manager Console in Visual Studio</span></span>

<span data-ttu-id="17a54-103">Die Paket-Manager-Konsole (PMC)-Tools für Entity Framework Core die Entwicklungsaufgaben zur Entwurfszeit ausführen.</span><span class="sxs-lookup"><span data-stu-id="17a54-103">The Package Manager Console (PMC) tools for Entity Framework Core perform design-time development tasks.</span></span> <span data-ttu-id="17a54-104">Beispielsweise erstellen Sie [Migrationen](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0), wenden Migrationen an und generieren Code für ein Modell, das auf einer vorhandenen Datenbank basiert.</span><span class="sxs-lookup"><span data-stu-id="17a54-104">For example, they create [migrations](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0), apply migrations, and generate code for a model based on an existing database.</span></span> <span data-ttu-id="17a54-105">Die Befehle werden innerhalb von Visual Studio mithilfe der [Paket-Manager-Konsole](/nuget/tools/package-manager-console)ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="17a54-105">The commands run inside of Visual Studio using the [Package Manager Console](/nuget/tools/package-manager-console).</span></span> <span data-ttu-id="17a54-106">Diese Tools funktionieren sowohl mit .NET Framework- als auch mit .NET Core-Projekten.</span><span class="sxs-lookup"><span data-stu-id="17a54-106">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="17a54-107">Wenn Sie Visual Studio nicht verwenden, empfiehlt es sich, stattdessen die [EF Core Befehlszeilen Tools](dotnet.md) zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="17a54-107">If you aren't using Visual Studio, we recommend the [EF Core Command-line Tools](dotnet.md) instead.</span></span> <span data-ttu-id="17a54-108">Die CLI-Tools sind plattformübergreifend und werden innerhalb einer Eingabeaufforderung ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="17a54-108">The CLI tools are cross-platform and run inside a command prompt.</span></span>

## <a name="installing-the-tools"></a><span data-ttu-id="17a54-109">Installieren der Tools</span><span class="sxs-lookup"><span data-stu-id="17a54-109">Installing the tools</span></span>

<span data-ttu-id="17a54-110">Die Verfahren zum Installieren und Aktualisieren der Tools unterscheiden sich zwischen ASP.net Core 2.1 und früheren Versionen oder anderen Projekttypen.</span><span class="sxs-lookup"><span data-stu-id="17a54-110">The procedures for installing and updating the tools differ between ASP.NET Core 2.1+ and earlier versions or other project types.</span></span>

### <a name="aspnet-core-version-21-and-later"></a><span data-ttu-id="17a54-111">ASP.net Core Version 2,1 und höher</span><span class="sxs-lookup"><span data-stu-id="17a54-111">ASP.NET Core version 2.1 and later</span></span>

<span data-ttu-id="17a54-112">Die Tools werden automatisch in ein ASP.net Core 2.1 +-Projekt eingeschlossen, da das `Microsoft.EntityFrameworkCore.Tools`-Paket im [Microsoft. aspnetcore. app-Metapaket](/aspnet/core/fundamentals/metapackage-app)enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="17a54-112">The tools are automatically included in an ASP.NET Core 2.1+ project because the `Microsoft.EntityFrameworkCore.Tools` package is included in the [Microsoft.AspNetCore.App metapackage](/aspnet/core/fundamentals/metapackage-app).</span></span>

<span data-ttu-id="17a54-113">Daher müssen Sie keine weiteren Schritte ausführen, um die Tools zu installieren, aber Sie müssen folgende Schritte ausführen:</span><span class="sxs-lookup"><span data-stu-id="17a54-113">Therefore, you don't have to do anything to install the tools, but you do have to:</span></span>

* <span data-ttu-id="17a54-114">Stellen Sie Pakete wieder her, bevor Sie die Tools in einem neuen Projekt verwenden.</span><span class="sxs-lookup"><span data-stu-id="17a54-114">Restore packages before using the tools in a new project.</span></span>
* <span data-ttu-id="17a54-115">Installieren Sie ein Paket, um die Tools auf eine neuere Version zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="17a54-115">Install a package to update the tools to a newer version.</span></span>

<span data-ttu-id="17a54-116">Um sicherzustellen, dass Sie die neueste Version der Tools erhalten, empfiehlt es sich, auch den folgenden Schritt durchzuführen:</span><span class="sxs-lookup"><span data-stu-id="17a54-116">To make sure that you're getting the latest version of the tools, we recommend that you also do the following step:</span></span>

* <span data-ttu-id="17a54-117">Bearbeiten Sie die *csproj* -Datei, und fügen Sie eine Zeile hinzu, die die neueste Version des [Microsoft. entityframeworkcore. Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) -Pakets angibt.</span><span class="sxs-lookup"><span data-stu-id="17a54-117">Edit your *.csproj* file and add a line specifying the latest version of the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) package.</span></span> <span data-ttu-id="17a54-118">Die CSPROJ-Datei kann z *. b.* ein `ItemGroup` enthalten, das wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="17a54-118">For example, the *.csproj* file might include an `ItemGroup` that looks like this:</span></span>

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

<span data-ttu-id="17a54-119">Aktualisieren Sie die Tools, wenn Sie eine Meldung wie im folgenden Beispiel erhalten:</span><span class="sxs-lookup"><span data-stu-id="17a54-119">Update the tools when you get a message like the following example:</span></span>

> <span data-ttu-id="17a54-120">Die EF Core Tools-Version "2.1.1-RTM-30846" ist älter als die der Runtime "2.1.3-RTM-32065".</span><span class="sxs-lookup"><span data-stu-id="17a54-120">The EF Core tools version '2.1.1-rtm-30846' is older than that of the runtime '2.1.3-rtm-32065'.</span></span> <span data-ttu-id="17a54-121">Aktualisieren Sie die Tools für die neuesten Features und Fehlerbehebungen.</span><span class="sxs-lookup"><span data-stu-id="17a54-121">Update the tools for the latest features and bug fixes.</span></span>

<span data-ttu-id="17a54-122">So aktualisieren Sie die Tools:</span><span class="sxs-lookup"><span data-stu-id="17a54-122">To update the tools:</span></span>

* <span data-ttu-id="17a54-123">Installieren Sie die neueste .net Core SDK.</span><span class="sxs-lookup"><span data-stu-id="17a54-123">Install the latest .NET Core SDK.</span></span>
* <span data-ttu-id="17a54-124">Aktualisieren Sie Visual Studio auf die neueste Version.</span><span class="sxs-lookup"><span data-stu-id="17a54-124">Update Visual Studio to the latest version.</span></span>
* <span data-ttu-id="17a54-125">Bearbeiten Sie die *csproj* -Datei so, dass Sie einen Paket Verweis auf das aktuellste Tool Paket enthält, wie zuvor gezeigt.</span><span class="sxs-lookup"><span data-stu-id="17a54-125">Edit the *.csproj* file so that it includes a package reference to the latest tools package, as shown earlier.</span></span>

### <a name="other-versions-and-project-types"></a><span data-ttu-id="17a54-126">Andere Versionen und Projekttypen</span><span class="sxs-lookup"><span data-stu-id="17a54-126">Other versions and project types</span></span>

<span data-ttu-id="17a54-127">Installieren Sie die Paket-Manager-Konsole, indem Sie den folgenden Befehl in der **Paket-Manager-Konsole**ausführen:</span><span class="sxs-lookup"><span data-stu-id="17a54-127">Install the Package Manager Console tools by running the following command in **Package Manager Console**:</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="17a54-128">Aktualisieren Sie die Tools, indem Sie den folgenden Befehl in der **Paket-Manager-Konsole**ausführen.</span><span class="sxs-lookup"><span data-stu-id="17a54-128">Update the tools by running the following command in **Package Manager Console**.</span></span>

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a><span data-ttu-id="17a54-129">Überprüfen der Installation</span><span class="sxs-lookup"><span data-stu-id="17a54-129">Verify the installation</span></span>

<span data-ttu-id="17a54-130">Stellen Sie sicher, dass die Tools installiert sind, indem Sie folgenden Befehl ausführen:</span><span class="sxs-lookup"><span data-stu-id="17a54-130">Verify that the tools are installed by running this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```

<span data-ttu-id="17a54-131">Die Ausgabe sieht wie folgt aus (Sie gibt nicht an, welche Version der Tools Sie verwenden):</span><span class="sxs-lookup"><span data-stu-id="17a54-131">The output looks like this (it doesn't tell you which version of the tools you're using):</span></span>

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

## <a name="using-the-tools"></a><span data-ttu-id="17a54-132">Mithilfe der Tools</span><span class="sxs-lookup"><span data-stu-id="17a54-132">Using the tools</span></span>

<span data-ttu-id="17a54-133">Vor der Verwendung der Tools:</span><span class="sxs-lookup"><span data-stu-id="17a54-133">Before using the tools:</span></span>

* <span data-ttu-id="17a54-134">Verstehen Sie den Unterschied Zwischenziel und Startprojekt.</span><span class="sxs-lookup"><span data-stu-id="17a54-134">Understand the difference between target and startup project.</span></span>
* <span data-ttu-id="17a54-135">Erfahren Sie, wie Sie die Tools mit .NET Standard-Klassenbibliotheken verwenden.</span><span class="sxs-lookup"><span data-stu-id="17a54-135">Learn how to use the tools with .NET Standard class libraries.</span></span>
* <span data-ttu-id="17a54-136">Legen Sie für ASP.net Core Projekte die Umgebung fest.</span><span class="sxs-lookup"><span data-stu-id="17a54-136">For ASP.NET Core projects, set the environment.</span></span>

### <a name="target-and-startup-project"></a><span data-ttu-id="17a54-137">Ziel-und Startprojekt</span><span class="sxs-lookup"><span data-stu-id="17a54-137">Target and startup project</span></span>

<span data-ttu-id="17a54-138">Die Befehle verweisen auf ein *Projekt* und ein *Startprojekt*.</span><span class="sxs-lookup"><span data-stu-id="17a54-138">The commands refer to a *project* and a *startup project*.</span></span>

* <span data-ttu-id="17a54-139">Das *Projekt* wird auch als *Ziel Projekt* bezeichnet, weil die Befehle Dateien hinzufügen oder entfernen.</span><span class="sxs-lookup"><span data-stu-id="17a54-139">The *project* is also known as the *target project* because it's where the commands add or remove files.</span></span> <span data-ttu-id="17a54-140">Standardmäßig ist das **Standard Projekt** , das in der **Paket-Manager-Konsole** ausgewählt ist, das Ziel Projekt.</span><span class="sxs-lookup"><span data-stu-id="17a54-140">By default, the **Default project** selected in **Package Manager Console** is the target project.</span></span> <span data-ttu-id="17a54-141">Sie können ein anderes Projekt als Ziel Projekt angeben, indem Sie die Option <nobr>`--project`</nobr> verwenden.</span><span class="sxs-lookup"><span data-stu-id="17a54-141">You can specify a different project as target project by using the <nobr>`--project`</nobr> option.</span></span>

* <span data-ttu-id="17a54-142">Das *Startprojekt ist das Startprojekt* , das von den Tools erstellt und ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="17a54-142">The *startup project* is the one that the tools build and run.</span></span> <span data-ttu-id="17a54-143">Die Tools müssen Anwendungscode zur Entwurfszeit ausführen, um Informationen zum Projekt zu erhalten, z. b. die Daten bankverbindungs Zeichenfolge und die Konfiguration des Modells.</span><span class="sxs-lookup"><span data-stu-id="17a54-143">The tools have to execute application code at design time to get information about the project, such as the database connection string and the configuration of the model.</span></span> <span data-ttu-id="17a54-144">Standardmäßig ist das **Startprojekt** in **Projektmappen-Explorer** das Startprojekt.</span><span class="sxs-lookup"><span data-stu-id="17a54-144">By default, the **Startup Project** in **Solution Explorer** is the startup project.</span></span> <span data-ttu-id="17a54-145">Sie können ein anderes Projekt als Startprojekt angeben, indem Sie die Option <nobr>`--startup-project`</nobr> verwenden.</span><span class="sxs-lookup"><span data-stu-id="17a54-145">You can specify a different project as startup project by using the <nobr>`--startup-project`</nobr> option.</span></span>

<span data-ttu-id="17a54-146">Das Startprojekt und das Ziel Projekt sind häufig das gleiche Projekt.</span><span class="sxs-lookup"><span data-stu-id="17a54-146">The startup project and target project are often the same project.</span></span> <span data-ttu-id="17a54-147">Ein typisches Szenario, bei dem es sich um separate Projekte handelt, sind folgende:</span><span class="sxs-lookup"><span data-stu-id="17a54-147">A typical scenario where they are separate projects is when:</span></span>

* <span data-ttu-id="17a54-148">Die EF Core Kontext-und Entitäts Klassen befinden sich in einer .net Core-Klassenbibliothek.</span><span class="sxs-lookup"><span data-stu-id="17a54-148">The EF Core context and entity classes are in a .NET Core class library.</span></span>
* <span data-ttu-id="17a54-149">Eine .net Core-Konsolen-APP oder-Web-App verweist auf die Klassenbibliothek.</span><span class="sxs-lookup"><span data-stu-id="17a54-149">A .NET Core console app or web app references the class library.</span></span>

<span data-ttu-id="17a54-150">Es ist auch möglich, [Migrations Code in einer Klassenbibliothek zu platzieren, getrennt vom EF Core Kontext](xref:core/managing-schemas/migrations/projects).</span><span class="sxs-lookup"><span data-stu-id="17a54-150">It's also possible to [put migrations code in a class library separate from the EF Core context](xref:core/managing-schemas/migrations/projects).</span></span>

### <a name="other-target-frameworks"></a><span data-ttu-id="17a54-151">Andere Ziel-Frameworks</span><span class="sxs-lookup"><span data-stu-id="17a54-151">Other target frameworks</span></span>

<span data-ttu-id="17a54-152">Die Paket-Manager-Konsolen Tools können mit .net Core-oder .NET Framework-Projekten verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="17a54-152">The Package Manager Console tools work with .NET Core or .NET Framework projects.</span></span> <span data-ttu-id="17a54-153">Apps, die über das EF Core Modell in einer .NET Standard-Klassenbibliothek verfügen, verfügen möglicherweise nicht über ein .net Core-oder .NET Framework-Projekt.</span><span class="sxs-lookup"><span data-stu-id="17a54-153">Apps that have the EF Core model in a .NET Standard class library might not have a .NET Core or .NET Framework project.</span></span> <span data-ttu-id="17a54-154">Dies gilt z. b. für xamarin-und universelle Windows-Plattform-apps.</span><span class="sxs-lookup"><span data-stu-id="17a54-154">For example, this is true of Xamarin and Universal Windows Platform apps.</span></span> <span data-ttu-id="17a54-155">In solchen Fällen können Sie ein .net Core-oder .NET Framework Konsolen-App-Projekt erstellen, dessen einziger Zweck darin besteht, als Startprojekt für die Tools zu fungieren.</span><span class="sxs-lookup"><span data-stu-id="17a54-155">In such cases, you can create a .NET Core or .NET Framework console app project whose only purpose is to act as startup project for the tools.</span></span> <span data-ttu-id="17a54-156">Das Projekt kann ein Dummyprojekt ohne echten Code sein &mdash; es ist nur erforderlich, um ein Ziel für die Tools bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="17a54-156">The project can be a dummy project with no real code &mdash; it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="17a54-157">Warum ist ein Dummyprojekt erforderlich?</span><span class="sxs-lookup"><span data-stu-id="17a54-157">Why is a dummy project required?</span></span> <span data-ttu-id="17a54-158">Wie bereits erwähnt, müssen die Tools Anwendungscode zur Entwurfszeit ausführen.</span><span class="sxs-lookup"><span data-stu-id="17a54-158">As mentioned earlier, the tools have to execute application code at design time.</span></span> <span data-ttu-id="17a54-159">Hierzu müssen Sie die .net Core-oder .NET Framework-Laufzeit verwenden.</span><span class="sxs-lookup"><span data-stu-id="17a54-159">To do that, they need to use the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="17a54-160">Wenn sich das EF Core Modell in einem Projekt befindet, das .net Core oder .NET Framework als Ziel hat, wird die Laufzeit von den EF Core Tools aus dem Projekt ausgeliehen.</span><span class="sxs-lookup"><span data-stu-id="17a54-160">When the EF Core model is in a project that targets .NET Core or .NET Framework, the EF Core tools borrow the runtime from the project.</span></span> <span data-ttu-id="17a54-161">Dies ist nicht möglich, wenn sich das EF Core Modell in einer .NET Standard Klassenbibliothek befindet.</span><span class="sxs-lookup"><span data-stu-id="17a54-161">They can't do that if the EF Core model is in a .NET Standard class library.</span></span> <span data-ttu-id="17a54-162">Der .NET Standard ist keine tatsächliche .NET-Implementierung. Es handelt sich um eine Spezifikation für eine Reihe von APIs, die von .net-Implementierungen unterstützt werden müssen.</span><span class="sxs-lookup"><span data-stu-id="17a54-162">The .NET Standard is not an actual .NET implementation; it's a specification of a set of APIs that .NET implementations must support.</span></span> <span data-ttu-id="17a54-163">Daher ist .NET Standard für die EF Core Tools nicht ausreichend, um Anwendungscode auszuführen.</span><span class="sxs-lookup"><span data-stu-id="17a54-163">Therefore .NET Standard is not sufficient for the EF Core tools to execute application code.</span></span> <span data-ttu-id="17a54-164">Das Dummyprojekt, das Sie als Startprojekt verwenden, stellt eine konkrete Zielplattform bereit, in die die Tools die .NET Standard Klassenbibliothek laden können.</span><span class="sxs-lookup"><span data-stu-id="17a54-164">The dummy project you create to use as startup project provides a concrete target platform into which the tools can load the .NET Standard class library.</span></span>

### <a name="aspnet-core-environment"></a><span data-ttu-id="17a54-165">ASP.net Core Umgebung</span><span class="sxs-lookup"><span data-stu-id="17a54-165">ASP.NET Core environment</span></span>

<span data-ttu-id="17a54-166">Wenn Sie die Umgebung für ASP.net Core Projekte angeben möchten, legen Sie **ENV: ASPNETCORE_ENVIRONMENT** vor dem Ausführen von Befehlen fest.</span><span class="sxs-lookup"><span data-stu-id="17a54-166">To specify the environment for ASP.NET Core projects, set **env:ASPNETCORE_ENVIRONMENT** before running commands.</span></span>

## <a name="common-parameters"></a><span data-ttu-id="17a54-167">Allgemeine Parameter</span><span class="sxs-lookup"><span data-stu-id="17a54-167">Common parameters</span></span>

<span data-ttu-id="17a54-168">In der folgenden Tabelle sind die Parameter aufgeführt, die für alle EF Core-Befehle gelten:</span><span class="sxs-lookup"><span data-stu-id="17a54-168">The following table shows parameters that are common to all of the EF Core commands:</span></span>

| <span data-ttu-id="17a54-169">Parameter</span><span class="sxs-lookup"><span data-stu-id="17a54-169">Parameter</span></span>                 | <span data-ttu-id="17a54-170">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="17a54-170">Description</span></span>                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="17a54-171">-Kontext \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="17a54-171">-Context \<String></span></span>        | <span data-ttu-id="17a54-172">Die `DbContext`-Klasse, die verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="17a54-172">The `DbContext` class to use.</span></span> <span data-ttu-id="17a54-173">Der Klassenname oder voll qualifiziert mit Namespaces.</span><span class="sxs-lookup"><span data-stu-id="17a54-173">Class name only or fully qualified with namespaces.</span></span>  <span data-ttu-id="17a54-174">Wenn dieser Parameter ausgelassen wird, wird EF Core die Kontext Klasse findet.</span><span class="sxs-lookup"><span data-stu-id="17a54-174">If this parameter is omitted, EF Core finds the context class.</span></span> <span data-ttu-id="17a54-175">Wenn mehrere Kontext Klassen vorhanden sind, ist dieser Parameter erforderlich.</span><span class="sxs-lookup"><span data-stu-id="17a54-175">If there are multiple context classes, this parameter is required.</span></span> |
| <span data-ttu-id="17a54-176">-Projekt \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="17a54-176">-Project \<String></span></span>        | <span data-ttu-id="17a54-177">Das Ziel Projekt.</span><span class="sxs-lookup"><span data-stu-id="17a54-177">The target project.</span></span> <span data-ttu-id="17a54-178">Wenn dieser Parameter ausgelassen wird, wird das **Standard Projekt** für die **Paket-Manager-Konsole** als Ziel Projekt verwendet.</span><span class="sxs-lookup"><span data-stu-id="17a54-178">If this parameter is omitted, the **Default project** for **Package Manager Console** is used as the target project.</span></span>                                                                             |
| <span data-ttu-id="17a54-179">-StartupProject \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="17a54-179">-StartupProject \<String></span></span> | <span data-ttu-id="17a54-180">Das Startprojekt.</span><span class="sxs-lookup"><span data-stu-id="17a54-180">The startup project.</span></span> <span data-ttu-id="17a54-181">Wenn dieser Parameter ausgelassen wird, wird das **Startprojekt** in Projektmappeneigenschaften als Ziel Projekt verwendet.</span><span class="sxs-lookup"><span data-stu-id="17a54-181">If this parameter is omitted, the **Startup project** in **Solution properties** is used as the target project.</span></span>                                                                                 |
| <span data-ttu-id="17a54-182">-Verbose</span><span class="sxs-lookup"><span data-stu-id="17a54-182">-Verbose</span></span>                  | <span data-ttu-id="17a54-183">Zeigt eine ausführliche Ausgabe an.</span><span class="sxs-lookup"><span data-stu-id="17a54-183">Show verbose output.</span></span>                                                                                                                                                                                                 |

<span data-ttu-id="17a54-184">Verwenden Sie den `Get-Help` Befehl von PowerShell, um Hilfe Informationen zu einem Befehl anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="17a54-184">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="17a54-185">Die Parameter "Context", "Project" und "StartupProject" unterstützen die Tab-Erweiterung.</span><span class="sxs-lookup"><span data-stu-id="17a54-185">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

## <a name="add-migration"></a><span data-ttu-id="17a54-186">Migration hinzufügen</span><span class="sxs-lookup"><span data-stu-id="17a54-186">Add-Migration</span></span>

<span data-ttu-id="17a54-187">Fügt eine neue Migration hinzu.</span><span class="sxs-lookup"><span data-stu-id="17a54-187">Adds a new migration.</span></span>

<span data-ttu-id="17a54-188">Parameter:</span><span class="sxs-lookup"><span data-stu-id="17a54-188">Parameters:</span></span>

| <span data-ttu-id="17a54-189">Parameter</span><span class="sxs-lookup"><span data-stu-id="17a54-189">Parameter</span></span>                         | <span data-ttu-id="17a54-190">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="17a54-190">Description</span></span>                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="17a54-191"><nobr>Name \<Zeichenfolge ><nobr></span><span class="sxs-lookup"><span data-stu-id="17a54-191"><nobr>-Name \<String><nobr></span></span>       | <span data-ttu-id="17a54-192">Der Name der Migration.</span><span class="sxs-lookup"><span data-stu-id="17a54-192">The name of the migration.</span></span> <span data-ttu-id="17a54-193">Dies ist ein Positions Parameter, der erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="17a54-193">This is a positional parameter and is required.</span></span>                                              |
| <span data-ttu-id="17a54-194"><nobr>-OutputDir \<Zeichenfolge ></nobr></span><span class="sxs-lookup"><span data-stu-id="17a54-194"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="17a54-195">Das zu verwendende Verzeichnis (und der untergeordnete Namespace).</span><span class="sxs-lookup"><span data-stu-id="17a54-195">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="17a54-196">Pfade sind relativ zum Ziel Projektverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="17a54-196">Paths are relative to the target project directory.</span></span> <span data-ttu-id="17a54-197">Der Standardwert ist "Migrationen".</span><span class="sxs-lookup"><span data-stu-id="17a54-197">Defaults to "Migrations".</span></span> |

## <a name="drop-database"></a><span data-ttu-id="17a54-198">Drop-Database</span><span class="sxs-lookup"><span data-stu-id="17a54-198">Drop-Database</span></span>

<span data-ttu-id="17a54-199">Löscht die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="17a54-199">Drops the database.</span></span>

<span data-ttu-id="17a54-200">Parameter:</span><span class="sxs-lookup"><span data-stu-id="17a54-200">Parameters:</span></span>

| <span data-ttu-id="17a54-201">Parameter</span><span class="sxs-lookup"><span data-stu-id="17a54-201">Parameter</span></span> | <span data-ttu-id="17a54-202">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="17a54-202">Description</span></span>                                              |
|:----------|:---------------------------------------------------------|
| <span data-ttu-id="17a54-203">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="17a54-203">-WhatIf</span></span>   | <span data-ttu-id="17a54-204">Zeigen Sie an, welche Datenbank gelöscht werden soll, aber löschen Sie Sie nicht.</span><span class="sxs-lookup"><span data-stu-id="17a54-204">Show which database would be dropped, but don't drop it.</span></span> |

## <a name="get-dbcontext"></a><span data-ttu-id="17a54-205">Get-dbcontext</span><span class="sxs-lookup"><span data-stu-id="17a54-205">Get-DbContext</span></span>

<span data-ttu-id="17a54-206">Ruft Informationen zu einem `DbContext` Typs ab.</span><span class="sxs-lookup"><span data-stu-id="17a54-206">Gets information about a `DbContext` type.</span></span>

## <a name="remove-migration"></a><span data-ttu-id="17a54-207">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="17a54-207">Remove-Migration</span></span>

<span data-ttu-id="17a54-208">Entfernt die letzte Migration (führt einen Rollback für die Codeänderungen aus, die für die Migration durchgeführt wurden).</span><span class="sxs-lookup"><span data-stu-id="17a54-208">Removes the last migration (rolls back the code changes that were done for the migration).</span></span>

<span data-ttu-id="17a54-209">Parameter:</span><span class="sxs-lookup"><span data-stu-id="17a54-209">Parameters:</span></span>

| <span data-ttu-id="17a54-210">Parameter</span><span class="sxs-lookup"><span data-stu-id="17a54-210">Parameter</span></span> | <span data-ttu-id="17a54-211">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="17a54-211">Description</span></span>                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| <span data-ttu-id="17a54-212">-Force</span><span class="sxs-lookup"><span data-stu-id="17a54-212">-Force</span></span>    | <span data-ttu-id="17a54-213">Setzen Sie die Migration zurück (führen Sie ein Rollback der Änderungen aus, die auf die Datenbank angewendet wurden).</span><span class="sxs-lookup"><span data-stu-id="17a54-213">Revert the migration (roll back the changes that were applied to the database).</span></span> |

## <a name="scaffold-dbcontext"></a><span data-ttu-id="17a54-214">Scaffold-DbContext</span><span class="sxs-lookup"><span data-stu-id="17a54-214">Scaffold-DbContext</span></span>

<span data-ttu-id="17a54-215">Generiert Code für eine `DbContext` und Entitäts Typen für eine Datenbank.</span><span class="sxs-lookup"><span data-stu-id="17a54-215">Generates code for a `DbContext` and entity types for a database.</span></span> <span data-ttu-id="17a54-216">Damit `Scaffold-DbContext` einen Entitätstyp generieren kann, muss die Datenbanktabelle über einen Primärschlüssel verfügen.</span><span class="sxs-lookup"><span data-stu-id="17a54-216">In order for `Scaffold-DbContext` to generate an entity type, the database table must have a primary key.</span></span>

<span data-ttu-id="17a54-217">Parameter:</span><span class="sxs-lookup"><span data-stu-id="17a54-217">Parameters:</span></span>

| <span data-ttu-id="17a54-218">Parameter</span><span class="sxs-lookup"><span data-stu-id="17a54-218">Parameter</span></span>                          | <span data-ttu-id="17a54-219">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="17a54-219">Description</span></span>                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="17a54-220"><nobr>-Verbindungs \<Zeichenfolge ></nobr></span><span class="sxs-lookup"><span data-stu-id="17a54-220"><nobr>-Connection \<String></nobr></span></span> | <span data-ttu-id="17a54-221">Die Verbindungszeichenfolge für die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="17a54-221">The connection string to the database.</span></span> <span data-ttu-id="17a54-222">Bei ASP.net Core 2. x-Projekten kann der Wert *Name =\<Name der Verbindungs Zeichenfolge >* sein.</span><span class="sxs-lookup"><span data-stu-id="17a54-222">For ASP.NET Core 2.x projects, the value can be *name=\<name of connection string>*.</span></span> <span data-ttu-id="17a54-223">In diesem Fall stammt der Name aus den Konfigurations Quellen, die für das Projekt eingerichtet sind.</span><span class="sxs-lookup"><span data-stu-id="17a54-223">In that case the name comes from the configuration sources that are set up for the project.</span></span> <span data-ttu-id="17a54-224">Dies ist ein Positions Parameter, der erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="17a54-224">This is a positional parameter and is required.</span></span> |
| <span data-ttu-id="17a54-225"><nobr>-Anbieter \<Zeichenfolge ></nobr></span><span class="sxs-lookup"><span data-stu-id="17a54-225"><nobr>-Provider \<String></nobr></span></span>   | <span data-ttu-id="17a54-226">Der zu verwendende Anbieter.</span><span class="sxs-lookup"><span data-stu-id="17a54-226">The provider to use.</span></span> <span data-ttu-id="17a54-227">In der Regel ist dies der Name des nuget-Pakets, z. b. `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="17a54-227">Typically this is the name of the NuGet package, for example: `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="17a54-228">Dies ist ein Positions Parameter, der erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="17a54-228">This is a positional parameter and is required.</span></span>                                                                                           |
| <span data-ttu-id="17a54-229">-OutputDir \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="17a54-229">-OutputDir \<String></span></span>               | <span data-ttu-id="17a54-230">Das Verzeichnis, in dem Dateien abgelegt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="17a54-230">The directory to put files in.</span></span> <span data-ttu-id="17a54-231">Pfade sind relativ zum Projektverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="17a54-231">Paths are relative to the project directory.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="17a54-232">-Contextdir \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="17a54-232">-ContextDir \<String></span></span>              | <span data-ttu-id="17a54-233">Das Verzeichnis, in das die `DbContext` Datei eingefügt werden soll.</span><span class="sxs-lookup"><span data-stu-id="17a54-233">The directory to put the `DbContext` file in.</span></span> <span data-ttu-id="17a54-234">Pfade sind relativ zum Projektverzeichnis.</span><span class="sxs-lookup"><span data-stu-id="17a54-234">Paths are relative to the project directory.</span></span>                                                                                                                                                                              |
| <span data-ttu-id="17a54-235">-Kontext \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="17a54-235">-Context \<String></span></span>                 | <span data-ttu-id="17a54-236">Der Name der zu generierenden `DbContext`-Klasse.</span><span class="sxs-lookup"><span data-stu-id="17a54-236">The name of the `DbContext` class to generate.</span></span>                                                                                                                                                                                                                          |
| <span data-ttu-id="17a54-237">-Schemas \<Zeichenfolge [] ></span><span class="sxs-lookup"><span data-stu-id="17a54-237">-Schemas \<String[]></span></span>               | <span data-ttu-id="17a54-238">Die Schemas von Tabellen, für die Entitäts Typen generiert werden sollen.</span><span class="sxs-lookup"><span data-stu-id="17a54-238">The schemas of tables to generate entity types for.</span></span> <span data-ttu-id="17a54-239">Wenn dieser Parameter ausgelassen wird, werden alle Schemas eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="17a54-239">If this parameter is omitted, all schemas are included.</span></span>                                                                                                                                                             |
| <span data-ttu-id="17a54-240">-Tabellen \<Zeichenfolge [] ></span><span class="sxs-lookup"><span data-stu-id="17a54-240">-Tables \<String[]></span></span>                | <span data-ttu-id="17a54-241">Die Tabellen, für die Entitäts Typen generiert werden sollen.</span><span class="sxs-lookup"><span data-stu-id="17a54-241">The tables to generate entity types for.</span></span> <span data-ttu-id="17a54-242">Wenn dieser Parameter ausgelassen wird, werden alle Tabellen eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="17a54-242">If this parameter is omitted, all tables are included.</span></span>                                                                                                                                                                         |
| <span data-ttu-id="17a54-243">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="17a54-243">-DataAnnotations</span></span>                   | <span data-ttu-id="17a54-244">Verwenden Sie Attribute, um das Modell zu konfigurieren (sofern möglich).</span><span class="sxs-lookup"><span data-stu-id="17a54-244">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="17a54-245">Wenn dieser Parameter ausgelassen wird, wird nur die fließende API verwendet.</span><span class="sxs-lookup"><span data-stu-id="17a54-245">If this parameter is omitted, only the fluent API is used.</span></span>                                                                                                                                                      |
| <span data-ttu-id="17a54-246">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="17a54-246">-UseDatabaseNames</span></span>                  | <span data-ttu-id="17a54-247">Verwenden Sie Tabellen-und Spaltennamen genau so, wie Sie in der Datenbank angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="17a54-247">Use table and column names exactly as they appear in the database.</span></span> <span data-ttu-id="17a54-248">Wenn dieser Parameter ausgelassen wird, werden die Datenbanknamen entsprechend den C# Namensformat Konventionen genauer angepasst.</span><span class="sxs-lookup"><span data-stu-id="17a54-248">If this parameter is omitted, database names are changed to more closely conform to C# name style conventions.</span></span>                                                                                       |
| <span data-ttu-id="17a54-249">-Force</span><span class="sxs-lookup"><span data-stu-id="17a54-249">-Force</span></span>                             | <span data-ttu-id="17a54-250">Überschreibt vorhandene Dateien.</span><span class="sxs-lookup"><span data-stu-id="17a54-250">Overwrite existing files.</span></span>                                                                                                                                                                                                                                               |

<span data-ttu-id="17a54-251">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="17a54-251">Example:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="17a54-252">Beispiel, bei dem nur ausgewählte Tabellen gerüziert werden und der Kontext in einem separaten Ordner mit einem angegebenen Namen erstellt wird:</span><span class="sxs-lookup"><span data-stu-id="17a54-252">Example that scaffolds only selected tables and creates the context in a separate folder with a specified name:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a><span data-ttu-id="17a54-253">Skript-Migration</span><span class="sxs-lookup"><span data-stu-id="17a54-253">Script-Migration</span></span>

<span data-ttu-id="17a54-254">Generiert ein SQL-Skript, mit dem alle Änderungen von einer ausgewählten Migration zu einer anderen ausgewählten Migration übernommen werden.</span><span class="sxs-lookup"><span data-stu-id="17a54-254">Generates a SQL script that applies all of the changes from one selected migration to another selected migration.</span></span>

<span data-ttu-id="17a54-255">Parameter:</span><span class="sxs-lookup"><span data-stu-id="17a54-255">Parameters:</span></span>

| <span data-ttu-id="17a54-256">Parameter</span><span class="sxs-lookup"><span data-stu-id="17a54-256">Parameter</span></span>                | <span data-ttu-id="17a54-257">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="17a54-257">Description</span></span>                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="17a54-258">*-From* \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="17a54-258">*-From* \<String></span></span>        | <span data-ttu-id="17a54-259">Die Migration wird gestartet.</span><span class="sxs-lookup"><span data-stu-id="17a54-259">The starting migration.</span></span> <span data-ttu-id="17a54-260">Migrationen können anhand des Namens oder der ID identifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="17a54-260">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="17a54-261">Die Zahl 0 (null) ist ein Sonderfall, der *vor der ersten Migration*liegt.</span><span class="sxs-lookup"><span data-stu-id="17a54-261">The number 0 is a special case that means *before the first migration*.</span></span> <span data-ttu-id="17a54-262">Der Standardwert ist 0.</span><span class="sxs-lookup"><span data-stu-id="17a54-262">Defaults to 0.</span></span>                                                              |
| <span data-ttu-id="17a54-263">*-To* \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="17a54-263">*-To* \<String></span></span>          | <span data-ttu-id="17a54-264">Die Beendigung der Migration.</span><span class="sxs-lookup"><span data-stu-id="17a54-264">The ending migration.</span></span> <span data-ttu-id="17a54-265">Standardmäßig wird die letzte Migration verwendet.</span><span class="sxs-lookup"><span data-stu-id="17a54-265">Defaults to the last migration.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="17a54-266"><nobr>-Idempotent</nobr></span><span class="sxs-lookup"><span data-stu-id="17a54-266"><nobr>-Idempotent</nobr></span></span> | <span data-ttu-id="17a54-267">Generieren Sie ein Skript, das bei jeder Migration in einer Datenbank verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="17a54-267">Generate a script that can be used on a database at any migration.</span></span>                                                                                                                                                         |
| <span data-ttu-id="17a54-268">-Ausgabe \<Zeichenfolge ></span><span class="sxs-lookup"><span data-stu-id="17a54-268">-Output \<String></span></span>        | <span data-ttu-id="17a54-269">Die Datei, in die das Ergebnis geschrieben werden soll.</span><span class="sxs-lookup"><span data-stu-id="17a54-269">The file to write the result to.</span></span> <span data-ttu-id="17a54-270">Wenn dieser Parameter ausgelassen wird, wird die Datei mit einem generierten Namen im selben Ordner erstellt, in dem die Laufzeitdateien der App erstellt werden, z. b.: */obj/Debug/netcoreapp2.1/ghbkztfz.SQL/* .</span><span class="sxs-lookup"><span data-stu-id="17a54-270">IF this parameter is omitted, the file is created with a generated name in the same folder as the app's runtime files are created, for example: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span></span> |

> [!TIP]
> <span data-ttu-id="17a54-271">Die Parameter to, from und Output unterstützen die Tab-Erweiterung.</span><span class="sxs-lookup"><span data-stu-id="17a54-271">The To, From, and Output parameters support tab-expansion.</span></span>

<span data-ttu-id="17a54-272">Im folgenden Beispiel wird ein Skript für die InitialCreate-Migration mit dem Namen der Migration erstellt.</span><span class="sxs-lookup"><span data-stu-id="17a54-272">The following example creates a script for the InitialCreate migration, using the migration name.</span></span>

```powershell
Script-Migration -To InitialCreate
```

<span data-ttu-id="17a54-273">Im folgenden Beispiel wird ein Skript für alle Migrationen nach der InitialCreate-Migration mit der Migrations-ID erstellt.</span><span class="sxs-lookup"><span data-stu-id="17a54-273">The following example creates a script for all migrations after the InitialCreate migration, using the migration ID.</span></span>

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a><span data-ttu-id="17a54-274">Update-Database</span><span class="sxs-lookup"><span data-stu-id="17a54-274">Update-Database</span></span>

<span data-ttu-id="17a54-275">Aktualisiert die Datenbank auf die letzte Migration oder eine angegebene Migration.</span><span class="sxs-lookup"><span data-stu-id="17a54-275">Updates the database to the last migration or to a specified migration.</span></span>

| <span data-ttu-id="17a54-276">Parameter</span><span class="sxs-lookup"><span data-stu-id="17a54-276">Parameter</span></span>                           | <span data-ttu-id="17a54-277">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="17a54-277">Description</span></span>                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="17a54-278"><nobr> *-Migrations* \<Zeichenfolge ></nobr></span><span class="sxs-lookup"><span data-stu-id="17a54-278"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="17a54-279">Die Ziel Migration.</span><span class="sxs-lookup"><span data-stu-id="17a54-279">The target migration.</span></span> <span data-ttu-id="17a54-280">Migrationen können anhand des Namens oder der ID identifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="17a54-280">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="17a54-281">Die Zahl 0 (null) ist ein Sonderfall, der *vor der ersten Migration* steht und bewirkt, dass alle Migrationen rückgängig gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="17a54-281">The number 0 is a special case that means *before the first migration* and causes all migrations to be reverted.</span></span> <span data-ttu-id="17a54-282">Wenn keine Migration angegeben ist, wird für den Befehl standardmäßig die letzte Migration verwendet.</span><span class="sxs-lookup"><span data-stu-id="17a54-282">If no migration is specified, the command defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="17a54-283">Der Migrations Parameter unterstützt die Erweiterung der Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="17a54-283">The Migration parameter supports tab-expansion.</span></span>

<span data-ttu-id="17a54-284">Im folgenden Beispiel werden alle Migrationen wieder hergestellt.</span><span class="sxs-lookup"><span data-stu-id="17a54-284">The following example reverts all migrations.</span></span>

```powershell
Update-Database -Migration 0
```

<span data-ttu-id="17a54-285">In den folgenden Beispielen wird die-Datenbank auf eine angegebene Migration aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="17a54-285">The following examples update the database to a specified migration.</span></span> <span data-ttu-id="17a54-286">Der erste verwendet den Migrations Namen, der zweite verwendet die Migrations-ID:</span><span class="sxs-lookup"><span data-stu-id="17a54-286">The first uses the migration name and the second uses the migration ID:</span></span>

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```

## <a name="additional-resources"></a><span data-ttu-id="17a54-287">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="17a54-287">Additional resources</span></span>

* [<span data-ttu-id="17a54-288">Migrationen</span><span class="sxs-lookup"><span data-stu-id="17a54-288">Migrations</span></span>](xref:core/managing-schemas/migrations/index)
* [<span data-ttu-id="17a54-289">Reverse Engineering</span><span class="sxs-lookup"><span data-stu-id="17a54-289">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
