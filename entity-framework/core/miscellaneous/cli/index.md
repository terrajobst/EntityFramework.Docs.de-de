---
title: Befehlszeilenreferenz – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
ms.openlocfilehash: d43b01fc61bb1c9b678e12e41c27d7efe9a59fa5
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490362"
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

Wenn Sie eine Klassenbibliothek verwenden möchten, sollten Sie gegebenenfalls die Verwendung der Klassenbibliothek von .NET Core oder .NET Framework in Erwägung ziehen. Dies führt zu den geringsten Problemen mit .NET-Tools. Wenn Sie stattdessen eine .NET Standard-Klassenbibliothek verwenden möchten, müssen Sie ein Startprojekt verwenden, das auf .NET Framework oder .NET Core ausgerichtet ist, sodass das Tool eine Zielplattform hat, in die es Ihre Klassenbibliothek laden kann. Dieses Startprojekt kann ein Dummyprojekt ohne realen Code sein. Es wird nur zur Bereitstellung eines Ziels für Tools benötigt.

Wenn Ihr Projekt ein anderes Framework, wie z.B. Universal Windows oder Xamarin, verwendet, müssen Sie eine separate .NET Standard-Klassenbibliothek erstellen. In diesem Fall folgen Sie dem oben angeführten Leitfaden zum Erstellen eines Startprojekts, das von Tools verwendet werden kann.

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
