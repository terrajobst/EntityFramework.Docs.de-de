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
# <a name="installing-ef-core"></a>Installieren von EF Core

## <a name="prerequisites"></a>Erforderliche Komponenten

Um .NET Core 2.1-Anwendungen (einschließlich ASP.NET Core 2.1-Anwendungen, deren Zielversionen für .NET Core festgelegt sind) zu entwickeln, müssen Sie eine für Ihre Plattform entsprechende Version des [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core) herunterladen und installieren. **Dies gilt auch dann, wenn Sie Visual Studio 2017 Version 15.7 installiert haben.**

Um neben .NET Core 2.1 auch EF Core 2.0- oder andere .NET Standard 2.1-Bibliotheken mit .NET-Plattform zu verwenden (z.B. mit .NET Framework 4.6.1 oder höher), benötigen Sie eine NuGet-Version, die auf .NET Standard 2.0 und den zugehörigen kompatiblen Frameworks basiert. Im Folgenden werden einige Möglichkeiten zum Abrufen dieser Version beschrieben:

* Installieren von Visual Studio 2017 Version 15.7
* Wenn Sie Visual Studio 2015 verwenden, [laden Sie den NuGet-Client herunter, und führen Sie ein Upgrade auf Version 3.6.0 durch](https://www.nuget.org/downloads).

Projekte, die mit früheren Versionen von Visual Studio erstellt wurden und deren Zielversionen für das .NET Framework festgelegt sind, erfordern möglicherweise zusätzliche Änderungen, damit Kompatibilität mit .NET Standard 2.0-Bibliotheken besteht:

* Bearbeiten Sie die Projektdatei, und stellen Sie sicher, dass der folgende Eintrag in der initialen Eigenschaftengruppe angezeigt wird:
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* Achten Sie bei Testprojekten auch darauf, dass der folgende Eintrag vorhanden ist:
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a>Abrufen von Bits
Es wird empfohlen, EF Core-Runtimebibliotheken durch Installation eines EF Core-Datenbankanbieters von NuGet in eine Anwendung einzubinden.

Neben den Runtimebibliotheken können Sie Tools installieren, die Ihnen die Durchführung mehrerer EF Core-bezogener Aufgaben in Ihrem Projekt bei der Entwurfszeit vereinfachen, wie etwa das Erstellen und Durchführen von Migrationen sowie das Erstellen eines Modells basierend auf einer bestehenden Datenbank.

> [!TIP]  
> Wenn Sie eine Anwendung aktualisieren müssen, die einen Datenbankanbieter eines Drittanbieters verwendet, sollten Sie immer nach einem Update des Anbieters suchen, der mit der von Ihnen gewünschten Version von EF Core kompatibel ist. Datenbankanbieter für vorherige Versionen sind nicht mit Version 2.1 der EF Core-Runtime kompatibel.  

> [!TIP]  
> Anwendungen, deren Zielversionen für ASP.NET Core 2.1 festgelegt sind, können neben Datenbankanbietern von Drittanbietern EF Core 2.1 ohne zusätzliche Abhängigkeiten nutzen. Bei Anwendungen, deren Zielversionen für vorherige ASP.NET Core-Versionen festgelegt sind, muss für die Nutzung von EF Core 2.1 ein Upgrade auf ASP.NET Core 2.1 durchgeführt werden.

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a>Plattformübergreifende Entwicklung unter Verwendung der .NET Core-CLI (Command Line Interface)

Um Anwendungen, deren Zielversionen für [.NET Core](https://www.microsoft.com/net/download/core) festgelegt sind, zu entwickeln, können Sie die [`dotnet`-CLI-Befehle](https://docs.microsoft.com/dotnet/core/tools/) in Kombination mit Ihrem bevorzugten Text-Editor oder einer integrierten Entwicklungsumgebung (IDE) wie Visual Studio, Visual Studio für Mac oder Visual Studio Code verwenden.

> [!IMPORTANT]  
> Anwendungen, deren Zielversionen für .NET Core festgelegt sind, erfordern bestimmte Versionen von Visual Studio. Die .NET Core 1.x-Entwicklung beispielsweise erfordert Visual Studio 2017, die .NET Core 2.1-Entwicklung dagegen erfordert Visual Studio 2017 Version 15.7.

Um den SQL Server-Anbieter in einer plattformübergreifenden .NET Core-Anwendung zu installieren oder ein Upgrade für diesen durchzuführen, wechseln Sie in das Verzeichnis der Anwendung und führen den folgenden Befehl in einer Befehlszeile aus:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Mit dem Modifizierer `-v` können Sie eine bestimmte Installationsversion im Befehl `dotnet add package` angeben. Wenn Sie beispielsweise EF Core 2.1-Pakete installieren möchten, fügen Sie `-v 2.1.0` an den Befehl an.

EF Core beinhaltet eine Reihe von [zusätzlichen Befehlen für die `dotnet`-CLI](../../miscellaneous/cli/dotnet.md), die mit `dotnet ef` beginnen. Für die .NET Core-CLI-Tools für EF Core ist ein Paket namens `Microsoft.EntityFrameworkCore.Design` erforderlich. Sie können es durch folgende Methoden zum Projekt hinzufügen:

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

> [!IMPORTANT]      
> Verwenden Sie immer die Toolpaketversion, die der Hauptversion der Runtimepakete entspricht.

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a>Visual Studio-Entwicklung

Sie können mit Visual Studio viele verschiedene Arten von Anwendungen entwickeln, deren Zielversionen für .NET Core, das .NET Framework oder andere von EF Core unterstützte Plattformen festgelegt sind.

Es gibt zwei Möglichkeiten, wie Sie über Visual Studio einen EF Core-Datenbankanbieter in Ihrer Anwendung installieren können:

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a>Verwenden der [Paket-Manager-Benutzeroberfläche](https://docs.microsoft.com/nuget/tools/package-manager-ui) von NuGet

* Wählen Sie im Menü **Projekt > NuGet-Pakete verwalten** aus.

* Klicken Sie auf **Durchsuchen** oder die Registerkarte **Updates**.

* Wählen Sie das Paket `Microsoft.EntityFrameworkCore.SqlServer` und die gewünschte Version aus, und bestätigen Sie Ihre Auswahl.

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a>Verwenden der [Paket-Manager-Konsole (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console) von NuGet

* Wählen Sie im Menü **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.

* Geben Sie den folgenden Befehl in die PMC ein, und führen Sie ihn aus:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* Sie können stattdessen den Befehl `Update-Package` verwenden, um ein bereits installiertes Paket auf eine neuere Version zu aktualisieren.

* Um eine bestimmte Version anzugeben, können Sie den Modifizierer `-Version` verwenden. Wenn Sie beispielsweise EF Core 2.1-Pakete installieren möchten, fügen Sie `-Version 2.1.0` an den Befehl an.

#### <a name="tools"></a>Tools

Es gibt auch eine PowerShell-Version der [EF-Core-Befehle mit ähnlichen Funktionen wie die `dotnet ef`-Befehle, die in der PMC in Visual Studio ausgeführt werden](../../miscellaneous/cli/powershell.md). 

> [!TIP]  
> Die `dotnet ef`-Befehle aus der PMC können zwar in Visual Studio verwendet werden, es ist jedoch weitaus komfortabler, die PowerShell-Version zu verwenden:
> * Sie werden automatisch mit dem aktuellen in der PMC ausgewählten Projekt ausgeführt, ohne dass manuell zwischen den Verzeichnissen gewechselt werden muss.  
> * Nach Abschluss des Befehls öffnen sie automatisch Dateien, die von den Befehlen in Visual Studio generiert wurden.

> [!IMPORTANT]  
> **Veraltete Pakete in EF Core 2.1:** Wenn Sie ein Upgrade für eine vorhandene Anwendung auf EF Core 2.1 durchführen, müssen einige Verweise auf ältere EF Core-Pakete eventuell manuell entfernt werden:
> * Entwurfspakete von Datenbankanbietern wie `Microsoft.EntityFrameworkCore.SqlServer.Design` werden nicht mehr von EF Core 2.1 benötigt oder von EF Core 2.1 unterstützt. Sie werden bei der Durchführung eines Upgrades für andere Pakete jedoch nicht automatisch entfernt.
> * .NET CLI-Tools sind jetzt im .NET SDK enthalten. Der Verweis auf das Paket kann also aus der *csproj*-Datei entfernt werden:
>   ```
>   <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
>   ```
