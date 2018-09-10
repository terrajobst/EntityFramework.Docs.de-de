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
# <a name="installing-entity-framework-core"></a>Installieren von Entity Framework Core

## <a name="prerequisites"></a>Erforderliche Komponenten

* Installieren Sie das [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core), um Apps entwickeln zu können, die .NET Core 2.1 anzielen. Das SDK muss installiert werden, auch wenn Sie über die neueste Version von Visual Studio 2017 verfügen.

* Installieren Sie Visual Studio 2017 Version 15.7 oder höher, um Visual Studio für die Entwicklung von Apps zu verwenden, die .NET Core 2.1 anzielen.

* Damit Sie Entity Framework 2.1 in ASP.NET Core-Anwendungen verwenden können, verwenden Sie ASP.NET Core 2.1. Anwendungen, die frühere ASP.NET Core-Versionen verwenden, müssen auf Version 2.1 aktualisiert werden.

* Sie können Visual Studio 2015 für Apps verwenden, die .NET Framework 4.6.1 oder höher anzielen. Sie benötigen jedoch eine NuGet-Version, die .NET Standard 2.0 sowie die dafür kompatiblen Frameworks berücksichtigt. Damit Sie diese in Visual Studio 2015 abrufen können, [führen Sie ein Upgrade des NuGet-Clients auf Version 3.6.0 durch](https://www.nuget.org/downloads).

## <a name="get-the-entity-framework-core-runtime"></a>Abrufen der Entity Framework Core-Runtime

Um einer Anwendung EF Core-Laufzeitbibliotheken hinzuzufügen, installieren Sie das NuGet-Paket für den Datenbankanbieter, den Sie verwenden möchten. Eine Liste der unterstützten Anbieter und deren NuGet-Paketnamen finden Sie unter [Datenbankanbieter](../../providers/index.md).

Verwenden Sie die .NET Core-CLI, das Dialogfeld für den Visual Studio-Paket-Manager oder die Visual Studio-Paket-Manager-Konsole, um NuGet-Pakete zu installieren oder zu aktualisieren.

Für ASP.NET Core 2.1-Anwendungen sind die speicherinternen Anbieter und SQL Server-Anbieter automatisch enthalten, Sie müssen diese also nicht separat installieren.

> [!TIP]  
> Wenn Sie eine Anwendung aktualisieren müssen, die einen Datenbankanbieter eines Drittanbieters verwendet, sollten Sie immer nach einem Update des Anbieters suchen, der mit der von Ihnen gewünschten Version von EF Core kompatibel ist. Datenbankanbieter für vorherige Versionen sind nicht mit Version 2.1 der EF Core-Runtime kompatibel.  

### <a name="net-core-cli"></a>.NET Core-CLI

Der folgende .NET Core-CLI-Befehl installiert oder aktualisiert den SQL Server-Anbieter:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Mit dem Modifizierer `-v` können Sie eine bestimmte Version im Befehl `dotnet add package` angeben. Wenn Sie beispielsweise EF Core 2.1.0-Pakete installieren möchten, fügen Sie `-v 2.1.0` an den Befehl an.

### <a name="visual-studio-nuget-package-manager-dialog"></a>Dialogfeld für den NuGet-Paket-Manager in Visual Studio

* Klicken Sie im Menü auf **Projekt > NuGet-Pakete verwalten**.

* Klicken Sie auf **Durchsuchen** oder die Registerkarte **Updates**.

* Wählen Sie das `Microsoft.EntityFrameworkCore.SqlServer`-Paket aus, und bestätigen Sie, um den SQL Server-Anbieter zu installieren oder zu aktualisieren.

Weitere Informationen finden Sie im [Dialogfeld des NuGet-Paket-Managers](https://docs.microsoft.com/nuget/tools/package-manager-ui).

### <a name="visual-studio-nuget-package-manager-console"></a>NuGet-Paket-Manager-Konsole (Visual Studio)

* Wählen Sie im Menü **Tools die Option „NuGet-Paket-Manager“ und dann „Paket-Manager-Konsole“** aus.

* Führen Sie den folgenden Befehl in der Paket-Manager-Konsole aus, um den SQL Server-Anbieter zu installieren:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* Verwenden Sie den `Update-Package`-Befehl, um den Anbieter zu aktualisieren.

* Um eine bestimmte Version anzugeben, können Sie den Modifizierer `-Version` verwenden. Wenn Sie beispielsweise EF Core 2.1.0-Pakete installieren möchten, fügen Sie `-Version 2.1.0` an den Befehl an.

Weitere Informationen finden Sie im Artikel zur [Paket-Manager-Konsole](https://docs.microsoft.com/nuget/tools/package-manager-console).

## <a name="get-entity-framework-core-tools"></a>Abrufen von Entity Framework Core-Tools

Zusätzlich zu Laufzeitbibliotheken können Sie Tools installieren, die einige Aufgaben in Ihrem Projekt zur Entwurfszeit ausführen können, die mit EF Core in Zusammenhang stehen. Sie können beispielsweise Migrationen erstellen, Migrationen anwenden und ein Modell basierend auf einer vorhandenen Datenbank erstellen.

Es sind zwei Sätze von Tools verfügbar:
* Die [Befehlszeilenschnittstellentools](../../miscellaneous/cli/dotnet.md) für .NET Core können unter Windows, Linux und macOS verwendet werden. Diese Befehle beginnen mit `dotnet ef`. 
* Die [Tools der Paket-Manager-Konsole](../../miscellaneous/cli/powershell.md) werden in Visual Studio 2017 unter Windows ausgeführt. Diese Befehlen beginnen mit einem Verb, z.B. `Add-Migration`, `Update-Database`.

Obwohl Sie die `dotnet ef`-Befehle von der Paket-Manager-Konsole verwenden können, ist es viel einfacher, die Tools der Paket-Manager-Konsole zu verwenden, wenn Sie Visual Studio nutzen.
* Sie werden automatisch mit dem aktuell in der Paket-Manager-Konsole ausgewählten Projekt ausgeführt, ohne dass manuell zwischen den Verzeichnissen gewechselt werden muss.  
* Nach Abschluss des Befehls öffnen sie automatisch Dateien, die von den Befehlen in Visual Studio generiert wurden.

<a name="cli"></a>

### <a name="get-the-cli-tools"></a>Abrufen der CLI-Tools

Die `dotnet ef`-Befehle sind im .NET Core SDK enthalten. Um diese Befehle jedoch installieren zu können, müssen Sie das `Microsoft.EntityFrameworkCore.Design`-Paket installieren:

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

Für Apps von ASP.NET Core 2.1 ist dieses Paket automatisch enthalten.

Wie zuvor in den [Voraussetzungen](#prerequisites) erläutert, müssen Sie auch das .NET Core 2.1 SDK erstellen.

> [!IMPORTANT]      
> Verwenden Sie immer die Toolpaketversion, die der Hauptversion der Runtimepakete entspricht.

### <a name="get-the-package-manager-console-tools"></a>Abrufen der Tools für die Paket-Manager-Konsole

Installieren Sie das `Microsoft.EntityFrameworkCore.Tools`-Paket, um die Tools für die Paket-Manager-Konsole für EF Core zu installieren:

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Tools
``` 

Für Apps von ASP.NET Core 2.1 ist dieses Paket automatisch enthalten.

## <a name="upgrading-to-ef-core-21"></a>Durchführen eines Upgrades auf EF Core 2.1

Wenn Sie ein Upgrade für eine vorhandene Anwendung auf EF Core 2.1 durchführen, müssen einige Verweise auf ältere EF Core-Pakete eventuell manuell entfernt werden:

* Entwurfspakete von Datenbankanbietern wie `Microsoft.EntityFrameworkCore.SqlServer.Design` werden nicht mehr von EF Core 2.1 benötigt oder von EF Core 2.1 unterstützt. Sie werden bei der Durchführung eines Upgrades für andere Pakete jedoch nicht automatisch entfernt.

* .NET CLI-Tools sind jetzt im .NET SDK enthalten. Der Verweis auf das Paket kann also aus der *csproj*-Datei entfernt werden:

  ```
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  ```

Stellen Sie bei Anwendungen, die .NET Framework anzielen und von früheren Visual Studio-Versionen erstellt wurden, sicher, dass diese mit den .NET Standard 2.0-Bibliotheken kompatibel sind:

  * Bearbeiten Sie die Projektdatei, und stellen Sie sicher, dass der folgende Eintrag in der initialen Eigenschaftengruppe angezeigt wird:

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * Achten Sie bei Testprojekten auch darauf, dass der folgende Eintrag vorhanden ist:

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
