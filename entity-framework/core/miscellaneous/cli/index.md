---
title: "Befehlszeilenreferenz – EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 076e9251850ba10df323cd25922aa8b95b3a5491
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
<a name="entity-framework-core-tools"></a>Entity Framework Core Tools
===========================
Entity Framework Core Tools unterstützen Sie bei der Entwicklung von EF Core-Apps. Diese werden hauptsächlich verwendet, um ein Gerüst für DbContext und Entitätstypen durch Reverse Engineering eines Datenbankschemas zu erstellen und Migrationen zu verwalten.

Die [Tools der EF Core-Paket-Manager-Konsole (PMC)][1] gewährleisten ein hervorragendes Benutzererlebnis in Visual Studio. Führen Sie sie mithilfe der [Paket-Manager-Konsole][2] von NuGet aus. Diese Tools funktionieren sowohl mit .NET Framework- als auch mit .NET Core-Projekten.

Die [EF Core-.NET-Befehlszeilentools][3] sind eine Erweiterung der [.NET Core-CLI-Tools (Command-Line Interface)][4], die plattformübergreifend verfügbar sind und außerhalb von Visual Studio ausgeführt werden können. Für diese Tools ist ein .NET Core-SDK-Projekt (mit `Sdk="Microsoft.NET.Sdk"` oder einem ähnlichen SDK in der Projektdatei) erforderlich.

Beide Tools bieten dieselbe Funktionalität. Für die Entwicklung in Visual Studio empfehlen wir die Verwendung von PMC-Tools, da diese ein besseres ganzheitliches Erlebnis bieten.

<a name="frameworks"></a>Frameworks
----------
Die Tools unterstützen Projekte, deren Zielversionen für das .NET Framework oder .NET Core festgelegt sind.

Wenn für Ihr Projekt eine Zielversion für ein anderes Framework (z.B. das universelle Windows- oder Xamarin-Framework) festgelegt ist, empfehlen wir Ihnen, ein separates .NET Standard-Projekt zu erstellen und plattformübergreifend eine Zielversion für eines der unterstützten Frameworks festzulegen.

Um z.B. plattformübergreifend die Zielversion für .NET Core festzulegen, klicken Sie mit der rechten Maustaste auf das Projekt und wählen **\*.csproj bearbeiten**. Aktualisieren Sie im Folgenden die Eigenschaft `TargetFramework`. (Beachten Sie, dass der Eigenschaftenname zum Plural wird.)

``` xml
<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>
```

Wenn Sie eine .NET Standard-Klassenbibliothek verwenden und die Zielversionen für das .NET Framework oder .NET Core für Ihr Startprojekt festgelegt sind, müssen Sie nicht plattformübergreifend eine Zielversionen festlegen.

<a name="startup-and-target-projects"></a>Start- und Zielprojekte
---------------------------
Beim Aufrufen von Befehlen sind zwei Projekte beteiligt: das Zielprojekt und das Startprojekt.

Dem Zielprojekt werden Dateien hinzugefügt (oder sie werden in einigen Fällen aus diesem entfernt).

Das Startprojekt wird bei Ausführung des Projektcodes von den Tools emuliert.

Sowohl das Zielprojekt als auch das Startprojekt können identisch sein.


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
