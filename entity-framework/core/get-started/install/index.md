---
title: Installieren von Entiy Framework Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 7831e6a54e885cf0b89ef3eef2cd81a9292df606
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250321"
---
# <a name="installing-entity-framework-core"></a><span data-ttu-id="9b1fc-102">Installieren von Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="9b1fc-102">Installing Entity Framework Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9b1fc-103">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="9b1fc-103">Prerequisites</span></span>

* <span data-ttu-id="9b1fc-104">Installieren Sie das [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core), um Apps entwickeln zu können, die .NET Core 2.1 anzielen.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-104">To develop apps that target .NET Core 2.1, install [the .NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span> <span data-ttu-id="9b1fc-105">Das SDK muss installiert werden, auch wenn Sie über die neueste Version von Visual Studio 2017 verfügen.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-105">The SDK has to be installed even if you have the latest version of Visual Studio 2017.</span></span>

* <span data-ttu-id="9b1fc-106">Installieren Sie Visual Studio 2017 Version 15.7 oder höher, um Visual Studio für die Entwicklung von Apps zu verwenden, die .NET Core 2.1 anzielen.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-106">To use Visual Studio for development of apps that target .NET Core 2.1, install Visual Studio 2017 version 15.7 or later.</span></span>

* <span data-ttu-id="9b1fc-107">Damit Sie Entity Framework 2.1 in ASP.NET Core-Anwendungen verwenden können, verwenden Sie ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-107">To use Entity Framework 2.1 in ASP.NET Core applications, use ASP.NET Core 2.1.</span></span> <span data-ttu-id="9b1fc-108">Anwendungen, die frühere ASP.NET Core-Versionen verwenden, müssen auf Version 2.1 aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-108">Applications that use earlier versions of ASP.NET Core must be updated to 2.1.</span></span>

* <span data-ttu-id="9b1fc-109">Sie können Visual Studio 2015 für Apps verwenden, die .NET Framework 4.6.1 oder höher anzielen.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-109">You can use Visual Studio 2015 for apps that target the .NET Framework 4.6.1 or later.</span></span> <span data-ttu-id="9b1fc-110">Sie benötigen jedoch eine NuGet-Version, die .NET Standard 2.0 sowie die dafür kompatiblen Frameworks berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-110">But you need a version of NuGet that is aware of the .NET Standard 2.0 and its compatible frameworks.</span></span> <span data-ttu-id="9b1fc-111">Damit Sie diese in Visual Studio 2015 abrufen können, [führen Sie ein Upgrade des NuGet-Clients auf Version 3.6.0 durch](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="9b1fc-111">To get that in Visual Studio 2015, [upgrade the NuGet client to version 3.6.0](https://www.nuget.org/downloads).</span></span>

## <a name="get-the-entity-framework-core-runtime"></a><span data-ttu-id="9b1fc-112">Abrufen der Entity Framework Core-Runtime</span><span class="sxs-lookup"><span data-stu-id="9b1fc-112">Get the Entity Framework Core runtime</span></span>

<span data-ttu-id="9b1fc-113">Um einer Anwendung EF Core-Laufzeitbibliotheken hinzuzufügen, installieren Sie das NuGet-Paket für den Datenbankanbieter, den Sie verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-113">To add EF Core runtime libraries to an application, install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="9b1fc-114">Eine Liste der unterstützten Anbieter und deren NuGet-Paketnamen finden Sie unter [Datenbankanbieter](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="9b1fc-114">For a list of supported providers and their NuGet package names, see [Database providers](../../providers/index.md).</span></span>

<span data-ttu-id="9b1fc-115">Verwenden Sie die .NET Core-CLI, das Dialogfeld für den Visual Studio-Paket-Manager oder die Visual Studio-Paket-Manager-Konsole, um NuGet-Pakete zu installieren oder zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-115">To install or update NuGet packages, use the .NET Core CLI, the Visual Studio Package Manager Dialog, or the Visual Studio Package Manager Console.</span></span>

<span data-ttu-id="9b1fc-116">Für ASP.NET Core 2.1-Anwendungen sind die speicherinternen Anbieter und SQL Server-Anbieter automatisch enthalten, Sie müssen diese also nicht separat installieren.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-116">For ASP.NET Core 2.1 applications, the in-memory and SQL Server providers are automatically included, so there's no need to install them separately.</span></span>

> [!TIP]  
> <span data-ttu-id="9b1fc-117">Wenn Sie eine Anwendung aktualisieren müssen, die einen Datenbankanbieter eines Drittanbieters verwendet, sollten Sie immer nach einem Update des Anbieters suchen, der mit der von Ihnen gewünschten Version von EF Core kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-117">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="9b1fc-118">Datenbankanbieter für vorherige Versionen sind nicht mit Version 2.1 der EF Core-Runtime kompatibel.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-118">For example, database providers for previous versions are not compatible with version 2.1 of the EF Core runtime.</span></span>  

### <a name="net-core-cli"></a><span data-ttu-id="9b1fc-119">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="9b1fc-119">.NET Core CLI</span></span>

<span data-ttu-id="9b1fc-120">Der folgende .NET Core-CLI-Befehl installiert oder aktualisiert den SQL Server-Anbieter:</span><span class="sxs-lookup"><span data-stu-id="9b1fc-120">The following .NET Core CLI command installs or updates the SQL Server provider:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="9b1fc-121">Mit dem Modifizierer `-v` können Sie eine bestimmte Version im Befehl `dotnet add package` angeben.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-121">You can indicate a specific version in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="9b1fc-122">Wenn Sie beispielsweise EF Core 2.1.0-Pakete installieren möchten, fügen Sie `-v 2.1.0` an den Befehl an.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-122">For example, to install EF Core 2.1.0 packages, append `-v 2.1.0` to the command.</span></span>

### <a name="visual-studio-nuget-package-manager-dialog"></a><span data-ttu-id="9b1fc-123">Dialogfeld für den NuGet-Paket-Manager in Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9b1fc-123">Visual Studio NuGet Package Manager Dialog</span></span>

* <span data-ttu-id="9b1fc-124">Klicken Sie im Menü auf **Projekt > NuGet-Pakete verwalten**.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-124">From the menu, select **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="9b1fc-125">Klicken Sie auf **Durchsuchen** oder die Registerkarte **Updates**.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-125">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="9b1fc-126">Wählen Sie das `Microsoft.EntityFrameworkCore.SqlServer`-Paket aus, und bestätigen Sie, um den SQL Server-Anbieter zu installieren oder zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-126">To install or update the SQL Server provider, select the `Microsoft.EntityFrameworkCore.SqlServer` package, and confirm.</span></span>

<span data-ttu-id="9b1fc-127">Weitere Informationen finden Sie im [Dialogfeld des NuGet-Paket-Managers](https://docs.microsoft.com/nuget/tools/package-manager-ui).</span><span class="sxs-lookup"><span data-stu-id="9b1fc-127">For more information, see [NuGet Package Manager Dialog](https://docs.microsoft.com/nuget/tools/package-manager-ui).</span></span>

### <a name="visual-studio-nuget-package-manager-console"></a><span data-ttu-id="9b1fc-128">NuGet-Paket-Manager-Konsole (Visual Studio)</span><span class="sxs-lookup"><span data-stu-id="9b1fc-128">Visual Studio NuGet Package Manager Console</span></span>

* <span data-ttu-id="9b1fc-129">Wählen Sie im Menü **Tools die Option „NuGet-Paket-Manager“ und dann „Paket-Manager-Konsole“** aus.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-129">From the menu, select **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="9b1fc-130">Führen Sie den folgenden Befehl in der Paket-Manager-Konsole aus, um den SQL Server-Anbieter zu installieren:</span><span class="sxs-lookup"><span data-stu-id="9b1fc-130">To install the SQL Server provider, run the following command in the Package Manager Console:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="9b1fc-131">Verwenden Sie den `Update-Package`-Befehl, um den Anbieter zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-131">To update the provider, use the `Update-Package` command.</span></span>

* <span data-ttu-id="9b1fc-132">Um eine bestimmte Version anzugeben, können Sie den Modifizierer `-Version` verwenden.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-132">To specify a specific version, use the `-Version` modifier.</span></span> <span data-ttu-id="9b1fc-133">Wenn Sie beispielsweise EF Core 2.1.0-Pakete installieren möchten, fügen Sie `-Version 2.1.0` an den Befehl an.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-133">For example, to install EF Core 2.1.0 packages, append `-Version 2.1.0` to the commands</span></span>

<span data-ttu-id="9b1fc-134">Weitere Informationen finden Sie im Artikel zur [Paket-Manager-Konsole](https://docs.microsoft.com/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="9b1fc-134">For more information, see [Package Manager Console](https://docs.microsoft.com/nuget/tools/package-manager-console).</span></span>

## <a name="get-entity-framework-core-tools"></a><span data-ttu-id="9b1fc-135">Abrufen von Entity Framework Core-Tools</span><span class="sxs-lookup"><span data-stu-id="9b1fc-135">Get Entity Framework Core tools</span></span>

<span data-ttu-id="9b1fc-136">Zusätzlich zu Laufzeitbibliotheken können Sie Tools installieren, die einige Aufgaben in Ihrem Projekt zur Entwurfszeit ausführen können, die mit EF Core in Zusammenhang stehen.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-136">Besides the runtime libraries, you can install tools that can perform some EF Core-related tasks in your project at design time.</span></span> <span data-ttu-id="9b1fc-137">Sie können beispielsweise Migrationen erstellen, Migrationen anwenden und ein Modell basierend auf einer vorhandenen Datenbank erstellen.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-137">For example, you can create migrations, apply migrations, and create a model based on an existing database.</span></span>

<span data-ttu-id="9b1fc-138">Es sind zwei Sätze von Tools verfügbar:</span><span class="sxs-lookup"><span data-stu-id="9b1fc-138">Two sets of tools are available:</span></span>
* <span data-ttu-id="9b1fc-139">Die [Befehlszeilenschnittstellentools](../../miscellaneous/cli/dotnet.md) für .NET Core können unter Windows, Linux und macOS verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-139">The .NET Core [command-line interface (CLI) tools](../../miscellaneous/cli/dotnet.md) can be used on Windows, Linux, or macOS.</span></span> <span data-ttu-id="9b1fc-140">Diese Befehle beginnen mit `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-140">These commands begin with `dotnet ef`.</span></span> 
* <span data-ttu-id="9b1fc-141">Die [Tools der Paket-Manager-Konsole](../../miscellaneous/cli/powershell.md) werden in Visual Studio 2017 unter Windows ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-141">The [Package Manager Console tools](../../miscellaneous/cli/powershell.md)  run in Visual Studio 2017 on Windows.</span></span> <span data-ttu-id="9b1fc-142">Diese Befehlen beginnen mit einem Verb, z.B. `Add-Migration`, `Update-Database`.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-142">These commands start with a verb, for example `Add-Migration`, `Update-Database`.</span></span>

<span data-ttu-id="9b1fc-143">Obwohl Sie die `dotnet ef`-Befehle von der Paket-Manager-Konsole verwenden können, ist es viel einfacher, die Tools der Paket-Manager-Konsole zu verwenden, wenn Sie Visual Studio nutzen.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-143">Although you can use the `dotnet ef` commands from the Package Manager Console, it's more convenient to use the Package Manager Console tools when you're using Visual Studio:</span></span>
* <span data-ttu-id="9b1fc-144">Sie werden automatisch mit dem aktuell in der Paket-Manager-Konsole ausgewählten Projekt ausgeführt, ohne dass manuell zwischen den Verzeichnissen gewechselt werden muss.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-144">They automatically work with the current project selected in the Package Manager Console without requiring manually switching directories.</span></span>  
* <span data-ttu-id="9b1fc-145">Nach Abschluss des Befehls öffnen sie automatisch Dateien, die von den Befehlen in Visual Studio generiert wurden.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-145">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

<a name="cli"></a>

### <a name="get-the-cli-tools"></a><span data-ttu-id="9b1fc-146">Abrufen der CLI-Tools</span><span class="sxs-lookup"><span data-stu-id="9b1fc-146">Get the CLI tools</span></span>

<span data-ttu-id="9b1fc-147">Die `dotnet ef`-Befehle sind im .NET Core SDK enthalten. Um diese Befehle jedoch installieren zu können, müssen Sie das `Microsoft.EntityFrameworkCore.Design`-Paket installieren:</span><span class="sxs-lookup"><span data-stu-id="9b1fc-147">The `dotnet ef` commands are included in the .NET Core SDK, but to enable the commands you have to install the `Microsoft.EntityFrameworkCore.Design` package:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

<span data-ttu-id="9b1fc-148">Für Apps von ASP.NET Core 2.1 ist dieses Paket automatisch enthalten.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-148">For ASP.NET Core 2.1 apps, this package is included automatically.</span></span>

<span data-ttu-id="9b1fc-149">Wie zuvor in den [Voraussetzungen](#prerequisites) erläutert, müssen Sie auch das .NET Core 2.1 SDK erstellen.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-149">As explained earlier in [Prerequisites](#prerequisites), you also need to install the .NET Core 2.1 SDK.</span></span>

> [!IMPORTANT]      
> <span data-ttu-id="9b1fc-150">Verwenden Sie immer die Toolpaketversion, die der Hauptversion der Runtimepakete entspricht.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-150">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

### <a name="get-the-package-manager-console-tools"></a><span data-ttu-id="9b1fc-151">Abrufen der Tools für die Paket-Manager-Konsole</span><span class="sxs-lookup"><span data-stu-id="9b1fc-151">Get the Package Manager Console tools</span></span>

<span data-ttu-id="9b1fc-152">Installieren Sie das `Microsoft.EntityFrameworkCore.Tools`-Paket, um die Tools für die Paket-Manager-Konsole für EF Core zu installieren:</span><span class="sxs-lookup"><span data-stu-id="9b1fc-152">To get the Package Manager Console tools for EF Core, install the `Microsoft.EntityFrameworkCore.Tools` package:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Tools
``` 

<span data-ttu-id="9b1fc-153">Für Apps von ASP.NET Core 2.1 ist dieses Paket automatisch enthalten.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-153">For ASP.NET Core 2.1 apps, this package is included automatically.</span></span>

## <a name="upgrading-to-ef-core-21"></a><span data-ttu-id="9b1fc-154">Durchführen eines Upgrades auf EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="9b1fc-154">Upgrading to EF Core 2.1</span></span>

<span data-ttu-id="9b1fc-155">Wenn Sie ein Upgrade für eine vorhandene Anwendung auf EF Core 2.1 durchführen, müssen einige Verweise auf ältere EF Core-Pakete eventuell manuell entfernt werden:</span><span class="sxs-lookup"><span data-stu-id="9b1fc-155">If you're upgrading an existing application to EF Core 2.1, some references to older EF Core packages may need to be removed manually:</span></span>

* <span data-ttu-id="9b1fc-156">Entwurfspakete von Datenbankanbietern wie `Microsoft.EntityFrameworkCore.SqlServer.Design` werden nicht mehr von EF Core 2.1 benötigt oder von EF Core 2.1 unterstützt. Sie werden bei der Durchführung eines Upgrades für andere Pakete jedoch nicht automatisch entfernt.</span><span class="sxs-lookup"><span data-stu-id="9b1fc-156">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported in EF Core 2.1, but aren't automatically removed when upgrading the other packages.</span></span>

* <span data-ttu-id="9b1fc-157">.NET CLI-Tools sind jetzt im .NET SDK enthalten. Der Verweis auf das Paket kann also aus der *csproj*-Datei entfernt werden:</span><span class="sxs-lookup"><span data-stu-id="9b1fc-157">The .NET CLI tools are now included in the .NET SDK, so the reference to that package can be removed from the *.csproj* file:</span></span>

  ```
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  ```

<span data-ttu-id="9b1fc-158">Stellen Sie bei Anwendungen, die .NET Framework anzielen und von früheren Visual Studio-Versionen erstellt wurden, sicher, dass diese mit den .NET Standard 2.0-Bibliotheken kompatibel sind:</span><span class="sxs-lookup"><span data-stu-id="9b1fc-158">For applications that target the .NET Framework and were created by earlier versions of Visual Studio, make sure that they are compatible with .NET Standard 2.0 libraries:</span></span>

  * <span data-ttu-id="9b1fc-159">Bearbeiten Sie die Projektdatei, und stellen Sie sicher, dass der folgende Eintrag in der initialen Eigenschaftengruppe angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="9b1fc-159">Edit the project file and make sure the following entry appears in the initial property group:</span></span>

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * <span data-ttu-id="9b1fc-160">Achten Sie bei Testprojekten auch darauf, dass der folgende Eintrag vorhanden ist:</span><span class="sxs-lookup"><span data-stu-id="9b1fc-160">For test projects, also make sure the following entry is present:</span></span>

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
