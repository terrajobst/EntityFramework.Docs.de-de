---
title: Installieren von EF Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 30ca81a0ede65506a6684d2322d31332115b1ed3
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996927"
---
# <a name="installing-ef-core"></a><span data-ttu-id="45c5c-102">Installieren von EF Core</span><span class="sxs-lookup"><span data-stu-id="45c5c-102">Installing EF Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="45c5c-103">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="45c5c-103">Prerequisites</span></span>

<span data-ttu-id="45c5c-104">Um .NET Core 2.1-Anwendungen (einschließlich ASP.NET Core 2.1-Anwendungen, deren Zielversionen für .NET Core festgelegt sind) zu entwickeln, müssen Sie eine für Ihre Plattform entsprechende Version des [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core) herunterladen und installieren.</span><span class="sxs-lookup"><span data-stu-id="45c5c-104">In order to develop .NET Core 2.1 applications (including ASP.NET Core 2.1 applications that target .NET Core) you will need to download and install a version of the [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core) that is appropriate to your platform.</span></span> <span data-ttu-id="45c5c-105">**Dies gilt auch dann, wenn Sie Visual Studio 2017 Version 15.7 installiert haben.**</span><span class="sxs-lookup"><span data-stu-id="45c5c-105">**This is true even if you have installed Visual Studio 2017 version 15.7.**</span></span>

<span data-ttu-id="45c5c-106">Um neben .NET Core 2.1 auch EF Core 2.0- oder andere .NET Standard 2.1-Bibliotheken mit .NET-Plattform zu verwenden (z.B. mit .NET Framework 4.6.1 oder höher), benötigen Sie eine NuGet-Version, die auf .NET Standard 2.0 und den zugehörigen kompatiblen Frameworks basiert.</span><span class="sxs-lookup"><span data-stu-id="45c5c-106">In order to use EF Core 2.1 or any other .NET Standard 2.0 library with a .NET platform besides .NET Core 2.1 (for example, with .NET Framework 4.6.1 or greater) you will need a version of NuGet that is aware of the .NET Standard 2.0 and its compatible frameworks.</span></span> <span data-ttu-id="45c5c-107">Im Folgenden werden einige Möglichkeiten zum Abrufen dieser Version beschrieben:</span><span class="sxs-lookup"><span data-stu-id="45c5c-107">Here are a few ways you can obtain this:</span></span>

* <span data-ttu-id="45c5c-108">Installieren von Visual Studio 2017 Version 15.7</span><span class="sxs-lookup"><span data-stu-id="45c5c-108">Install Visual Studio 2017 version 15.7</span></span>
* <span data-ttu-id="45c5c-109">Wenn Sie Visual Studio 2015 verwenden, [laden Sie den NuGet-Client herunter, und führen Sie ein Upgrade auf Version 3.6.0 durch](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="45c5c-109">If you are using Visual Studio 2015, [download and upgrade NuGet client to version 3.6.0](https://www.nuget.org/downloads)</span></span>

<span data-ttu-id="45c5c-110">Projekte, die mit früheren Versionen von Visual Studio erstellt wurden und deren Zielversionen für das .NET Framework festgelegt sind, erfordern möglicherweise zusätzliche Änderungen, damit Kompatibilität mit .NET Standard 2.0-Bibliotheken besteht:</span><span class="sxs-lookup"><span data-stu-id="45c5c-110">Projects created with previous versions of Visual Studio and targeting .NET Framework may need additional modifications in order to be compatible with .NET Standard 2.0 libraries:</span></span>

* <span data-ttu-id="45c5c-111">Bearbeiten Sie die Projektdatei, und stellen Sie sicher, dass der folgende Eintrag in der initialen Eigenschaftengruppe angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="45c5c-111">Edit the project file and make sure the following entry appears in the initial property group:</span></span>
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* <span data-ttu-id="45c5c-112">Achten Sie bei Testprojekten auch darauf, dass der folgende Eintrag vorhanden ist:</span><span class="sxs-lookup"><span data-stu-id="45c5c-112">For test projects, also make sure the following entry is present:</span></span>
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a><span data-ttu-id="45c5c-113">Abrufen von Bits</span><span class="sxs-lookup"><span data-stu-id="45c5c-113">Getting the bits</span></span>
<span data-ttu-id="45c5c-114">Es wird empfohlen, EF Core-Runtimebibliotheken durch Installation eines EF Core-Datenbankanbieters von NuGet in eine Anwendung einzubinden.</span><span class="sxs-lookup"><span data-stu-id="45c5c-114">The recommended way to add EF Core runtime libraries into an application is to install an EF Core database provider from NuGet.</span></span>

<span data-ttu-id="45c5c-115">Neben den Runtimebibliotheken können Sie Tools installieren, die Ihnen die Durchführung mehrerer EF Core-bezogener Aufgaben in Ihrem Projekt bei der Entwurfszeit vereinfachen, wie etwa das Erstellen und Durchführen von Migrationen sowie das Erstellen eines Modells basierend auf einer bestehenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="45c5c-115">Besides the runtime libraries, you can install tools which make it easier to perform several EF Core-related tasks in your project at design time, such as creating and applying migrations, and creating a model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="45c5c-116">Wenn Sie eine Anwendung aktualisieren müssen, die einen Datenbankanbieter eines Drittanbieters verwendet, sollten Sie immer nach einem Update des Anbieters suchen, der mit der von Ihnen gewünschten Version von EF Core kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="45c5c-116">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="45c5c-117">Datenbankanbieter für vorherige Versionen sind nicht mit Version 2.1 der EF Core-Runtime kompatibel.</span><span class="sxs-lookup"><span data-stu-id="45c5c-117">For example, database providers for previous versions are not compatible with version 2.1 of the EF Core runtime.</span></span>  

> [!TIP]  
> <span data-ttu-id="45c5c-118">Anwendungen, deren Zielversionen für ASP.NET Core 2.1 festgelegt sind, können neben Datenbankanbietern von Drittanbietern EF Core 2.1 ohne zusätzliche Abhängigkeiten nutzen.</span><span class="sxs-lookup"><span data-stu-id="45c5c-118">Applications targeting ASP.NET Core 2.1 can use EF Core 2.1 without additional dependencies besides third party database providers.</span></span> <span data-ttu-id="45c5c-119">Bei Anwendungen, deren Zielversionen für vorherige ASP.NET Core-Versionen festgelegt sind, muss für die Nutzung von EF Core 2.1 ein Upgrade auf ASP.NET Core 2.1 durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="45c5c-119">Applications targeting previous versions of ASP.NET Core need to upgrade to ASP.NET Core 2.1 in order to use EF Core 2.1.</span></span>

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a><span data-ttu-id="45c5c-120">Plattformübergreifende Entwicklung unter Verwendung der .NET Core-CLI (Command Line Interface)</span><span class="sxs-lookup"><span data-stu-id="45c5c-120">Cross-platform development using the .NET Core Command Line Interface (CLI)</span></span>

<span data-ttu-id="45c5c-121">Um Anwendungen, deren Zielversionen für [.NET Core](https://www.microsoft.com/net/download/core) festgelegt sind, zu entwickeln, können Sie die [`dotnet`-CLI-Befehle](https://docs.microsoft.com/dotnet/core/tools/) in Kombination mit Ihrem bevorzugten Text-Editor oder einer integrierten Entwicklungsumgebung (IDE) wie Visual Studio, Visual Studio für Mac oder Visual Studio Code verwenden.</span><span class="sxs-lookup"><span data-stu-id="45c5c-121">To develop applications that target [.NET Core](https://www.microsoft.com/net/download/core) you can choose to use the [`dotnet` CLI commands](https://docs.microsoft.com/dotnet/core/tools/) in combination with your favorite text editor, or an Integrated Development Environment (IDE) such as Visual Studio, Visual Studio for Mac, or Visual Studio Code.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="45c5c-122">Anwendungen, deren Zielversionen für .NET Core festgelegt sind, erfordern bestimmte Versionen von Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="45c5c-122">Applications that target .NET Core require specific versions of Visual Studio.</span></span> <span data-ttu-id="45c5c-123">Die .NET Core 1.x-Entwicklung beispielsweise erfordert Visual Studio 2017, die .NET Core 2.1-Entwicklung dagegen erfordert Visual Studio 2017 Version 15.7.</span><span class="sxs-lookup"><span data-stu-id="45c5c-123">For example, .NET Core 1.x development requires Visual Studio 2017, while .NET Core 2.1 development requires Visual Studio 2017 version 15.7.</span></span>

<span data-ttu-id="45c5c-124">Um den SQL Server-Anbieter in einer plattformübergreifenden .NET Core-Anwendung zu installieren oder ein Upgrade für diesen durchzuführen, wechseln Sie in das Verzeichnis der Anwendung und führen den folgenden Befehl in einer Befehlszeile aus:</span><span class="sxs-lookup"><span data-stu-id="45c5c-124">To install or upgrade the SQL Server provider in a cross-platform .NET Core application, switch to the application's directory and run the following in a command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="45c5c-125">Mit dem Modifizierer `-v` können Sie eine bestimmte Installationsversion im Befehl `dotnet add package` angeben.</span><span class="sxs-lookup"><span data-stu-id="45c5c-125">You can indicate a specific version install in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="45c5c-126">Wenn Sie beispielsweise EF Core 2.1-Pakete installieren möchten, fügen Sie `-v 2.1.0` an den Befehl an.</span><span class="sxs-lookup"><span data-stu-id="45c5c-126">For example, to install EF Core 2.1 packages, append `-v 2.1.0` to the command.</span></span>

<span data-ttu-id="45c5c-127">EF Core beinhaltet eine Reihe von [zusätzlichen Befehlen für die `dotnet`-CLI](../../miscellaneous/cli/dotnet.md), die mit `dotnet ef` beginnen.</span><span class="sxs-lookup"><span data-stu-id="45c5c-127">EF Core includes a set of [additional commands for the `dotnet` CLI](../../miscellaneous/cli/dotnet.md), starting with `dotnet ef`.</span></span> <span data-ttu-id="45c5c-128">Für die .NET Core-CLI-Tools für EF Core ist ein Paket namens `Microsoft.EntityFrameworkCore.Design` erforderlich.</span><span class="sxs-lookup"><span data-stu-id="45c5c-128">The .NET Core CLI tools for EF Core require a package called `Microsoft.EntityFrameworkCore.Design`.</span></span> <span data-ttu-id="45c5c-129">Sie können es durch folgende Methoden zum Projekt hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="45c5c-129">You can add it to the project using:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

> [!IMPORTANT]      
> <span data-ttu-id="45c5c-130">Verwenden Sie immer die Toolpaketversion, die der Hauptversion der Runtimepakete entspricht.</span><span class="sxs-lookup"><span data-stu-id="45c5c-130">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a><span data-ttu-id="45c5c-131">Visual Studio-Entwicklung</span><span class="sxs-lookup"><span data-stu-id="45c5c-131">Visual Studio development</span></span>

<span data-ttu-id="45c5c-132">Sie können mit Visual Studio viele verschiedene Arten von Anwendungen entwickeln, deren Zielversionen für .NET Core, das .NET Framework oder andere von EF Core unterstützte Plattformen festgelegt sind.</span><span class="sxs-lookup"><span data-stu-id="45c5c-132">You can develop many different types of applications that target .NET Core, .NET Framework, or other platforms supported by EF Core using Visual Studio.</span></span>

<span data-ttu-id="45c5c-133">Es gibt zwei Möglichkeiten, wie Sie über Visual Studio einen EF Core-Datenbankanbieter in Ihrer Anwendung installieren können:</span><span class="sxs-lookup"><span data-stu-id="45c5c-133">There are two ways you can install an EF Core database provider in your application from Visual Studio:</span></span>

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a><span data-ttu-id="45c5c-134">Verwenden der [Paket-Manager-Benutzeroberfläche](https://docs.microsoft.com/nuget/tools/package-manager-ui) von NuGet</span><span class="sxs-lookup"><span data-stu-id="45c5c-134">Using NuGet's [Package Manager User Interface](https://docs.microsoft.com/nuget/tools/package-manager-ui)</span></span>

* <span data-ttu-id="45c5c-135">Wählen Sie im Menü **Projekt > NuGet-Pakete verwalten** aus.</span><span class="sxs-lookup"><span data-stu-id="45c5c-135">Select on the menu **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="45c5c-136">Klicken Sie auf **Durchsuchen** oder die Registerkarte **Updates**.</span><span class="sxs-lookup"><span data-stu-id="45c5c-136">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="45c5c-137">Wählen Sie das Paket `Microsoft.EntityFrameworkCore.SqlServer` und die gewünschte Version aus, und bestätigen Sie Ihre Auswahl.</span><span class="sxs-lookup"><span data-stu-id="45c5c-137">Select the `Microsoft.EntityFrameworkCore.SqlServer` package and the desired version and confirm</span></span>

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a><span data-ttu-id="45c5c-138">Verwenden der [Paket-Manager-Konsole (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console) von NuGet</span><span class="sxs-lookup"><span data-stu-id="45c5c-138">Using NuGet's [Package Manager Console (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console)</span></span>

* <span data-ttu-id="45c5c-139">Wählen Sie im Menü **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="45c5c-139">Select on the menu **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="45c5c-140">Geben Sie den folgenden Befehl in die PMC ein, und führen Sie ihn aus:</span><span class="sxs-lookup"><span data-stu-id="45c5c-140">Type and run the following command in the PMC:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="45c5c-141">Sie können stattdessen den Befehl `Update-Package` verwenden, um ein bereits installiertes Paket auf eine neuere Version zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="45c5c-141">You can use the `Update-Package` command instead to update a package that is already installed to a more recent  version</span></span>

* <span data-ttu-id="45c5c-142">Um eine bestimmte Version anzugeben, können Sie den Modifizierer `-Version` verwenden.</span><span class="sxs-lookup"><span data-stu-id="45c5c-142">To specify a specific version, you can use the `-Version` modifier.</span></span> <span data-ttu-id="45c5c-143">Wenn Sie beispielsweise EF Core 2.1-Pakete installieren möchten, fügen Sie `-Version 2.1.0` an den Befehl an.</span><span class="sxs-lookup"><span data-stu-id="45c5c-143">For example, to install EF Core 2.1 packages, append `-Version 2.1.0` to the commands</span></span>

#### <a name="tools"></a><span data-ttu-id="45c5c-144">Tools</span><span class="sxs-lookup"><span data-stu-id="45c5c-144">Tools</span></span>

<span data-ttu-id="45c5c-145">Es gibt auch eine PowerShell-Version der [EF-Core-Befehle mit ähnlichen Funktionen wie die `dotnet ef`-Befehle, die in der PMC in Visual Studio ausgeführt werden](../../miscellaneous/cli/powershell.md).</span><span class="sxs-lookup"><span data-stu-id="45c5c-145">There is also a PowerShell version of the [EF Core commands which run inside the PMC](../../miscellaneous/cli/powershell.md) in Visual Studio, with similar capabilities to the `dotnet ef` commands.</span></span> 

> [!TIP]  
> <span data-ttu-id="45c5c-146">Die `dotnet ef`-Befehle aus der PMC können zwar in Visual Studio verwendet werden, es ist jedoch weitaus komfortabler, die PowerShell-Version zu verwenden:</span><span class="sxs-lookup"><span data-stu-id="45c5c-146">Although it is possible to use the `dotnet ef` commands from the PMC in Visual Studio, it is far more convenient to use the PowerShell version:</span></span>
> * <span data-ttu-id="45c5c-147">Sie werden automatisch mit dem aktuellen in der PMC ausgewählten Projekt ausgeführt, ohne dass manuell zwischen den Verzeichnissen gewechselt werden muss.</span><span class="sxs-lookup"><span data-stu-id="45c5c-147">They automatically work with the current project selected in the PMC without requiring manually switching directories.</span></span>  
> * <span data-ttu-id="45c5c-148">Nach Abschluss des Befehls öffnen sie automatisch Dateien, die von den Befehlen in Visual Studio generiert wurden.</span><span class="sxs-lookup"><span data-stu-id="45c5c-148">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="45c5c-149">**Veraltete Pakete in EF Core 2.1:** Wenn Sie ein Upgrade für eine vorhandene Anwendung auf EF Core 2.1 durchführen, müssen einige Verweise auf ältere EF Core-Pakete eventuell manuell entfernt werden:</span><span class="sxs-lookup"><span data-stu-id="45c5c-149">**Deprecated packages in EF Core 2.1:** If you're upgrading an existing application to EF Core 2.1, some references to older EF Core packages may need to be removed manually:</span></span>
> * <span data-ttu-id="45c5c-150">Entwurfspakete von Datenbankanbietern wie `Microsoft.EntityFrameworkCore.SqlServer.Design` werden nicht mehr von EF Core 2.1 benötigt oder von EF Core 2.1 unterstützt. Sie werden bei der Durchführung eines Upgrades für andere Pakete jedoch nicht automatisch entfernt.</span><span class="sxs-lookup"><span data-stu-id="45c5c-150">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported in EF Core 2.1, but will not be automatically removed when upgrading the other packages.</span></span>
> * <span data-ttu-id="45c5c-151">.NET CLI-Tools sind jetzt im .NET SDK enthalten. Der Verweis auf das Paket kann also aus der *csproj*-Datei entfernt werden:</span><span class="sxs-lookup"><span data-stu-id="45c5c-151">The .NET CLI tools are now included in the .NET SDK, so the reference to that package can be removed from the *.csproj* file:</span></span>
>   ```
>   <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
>   ```
