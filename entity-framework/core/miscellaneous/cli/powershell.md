---
title: "Paket-Manager-Konsole (Visual Studio) – EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: aacf8c8564a3966db6202c9ff1c1c02a19a10814
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
<a name="ef-core-package-manager-console-tools"></a>EF-Core-Paket-Manager-Konsole Tools
=====================================
Die Tools EF Core Paket-Manager-Konsole (PMC) ausführen, in Visual Studio mithilfe von NuGet [Package Manager Console][2].
Diese Tools funktionieren sowohl mit .NET Framework- als auch mit .NET Core-Projekten.

> [!TIP]
> Verwenden Sie nicht Visual Studio? Die [EF Core-Befehlszeilentools] [ 1] über Plattformen hinweg und in einer Eingabeaufforderung ausgeführt werden.

<a name="installing-the-tools"></a>Installieren der Tools
--------------------
Installieren von EF-Core-Paket-Manager-Konsole Tools durch das Microsoft.EntityFrameworkCore.Tools NuGet-Paket installieren.
Sie können es installieren, indem Sie in den folgenden Befehl ausführen [Package Manager Console][2].

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

Wenn alles ordnungsgemäß funktioniert, sollten Sie diesen Befehl ausführen können:

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> Wenn das Startprojekt .NET Standard verwendet [Cross-Zielframework eine unterstützte] [ 3] vor der Verwendung der Tools.

> [!IMPORTANT]
> Bei Verwendung von **universelle Windows** oder **Xamarin**, Verschieben von EF Code auf eine .NET Standard-Klassenbibliothek und [Cross-Zielframework eine unterstützte] [ 3] vor der Verwendung der Tools. Geben Sie die Klassenbibliothek als Startprojekt fest.

<a name="using-the-tools"></a>Mithilfe der tools
---------------
Bei jedem eines Befehls aufrufen umfasst zwei Projekte:

Dem Zielprojekt werden Dateien hinzugefügt (oder sie werden in einigen Fällen aus diesem entfernt). Das Zielprojekt wird standardmäßig auf die **Standardprojekt** in Paket-Manager-Konsole ausgewählt, aber kann auch mit angegeben werden-Projektparameter.

Das Startprojekt wird bei Ausführung des Projektcodes von den Tools emuliert. Wird standardmäßig eine **als Startprojekt festlegen** im Projektmappen-Explorer. Sie können auch mit dem StartupProject - Parameter angegeben werden.

Allgemeine Parameter:

|                           |                             |
|:--------------------------|:----------------------------|
| -Kontext \<Zeichenfolge >        | Die DbContext verwenden.       |
| -Projekt \<Zeichenfolge >        | Das Projekt verwendet werden soll.         |
| -StartupProject \<Zeichenfolge > | Das Startup-Projekt verwenden. |
| -Verbose                  | Zeigen Sie eine ausführlichen Ausgabe.        |

Um Hilfeinformationen zu einem Befehl anzuzeigen, verwenden Sie PowerShell `Get-Help` Befehl.

> [!TIP]
> Der Kontext, Projekt- und StartupProject-Parameter unterstützen Tab-Taste.

> [!TIP]
> Legen Sie **Env:ASPNETCORE_ENVIRONMENT** vor dem Ausführen, um anzugeben, die ASP.NET Core-Umgebung.

<a name="commands"></a>Befehle
--------

### <a name="add-migration"></a>Hinzufügen-Migration

Fügt eine neue Migration.

Parameter:

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| ***-Name*** \<String>             | Der Name der Migration.                                                                                       |
| <nobr>-OutputDir \<String></nobr> | Das Verzeichnis (und Sub-Namespace) zu verwenden. Pfade sind relativ zum Projektverzeichnis an. Der Standardwert ist "Migration". |

> [!NOTE]
> Parameter in **fett** sind erforderlich, und diejenigen *Kursiv* sind mit Feldern fester Breite.

### <a name="drop-database"></a>Drop-Database

Löscht die Datenbank.

Parameter:

|         |                                                          |
|:--------|:---------------------------------------------------------|
| -WhatIf | Anzeigen, welche Datenbank verworfen werden, jedoch nicht, legen Sie sie. |

### <a name="get-dbcontext"></a>Get-DbContext

Ruft Informationen zu einem DbContext-Typ ab.

### <a name="remove-migration"></a>Remove-Migration

Entfernt die letzte Migration.

Parameter:

|        |                                                                       |
|:-------|:----------------------------------------------------------------------|
| -Force | Überprüfen Sie nicht, um festzustellen, ob die Migration der Datenbank angewendet wurde. |

### <a name="scaffold-dbcontext"></a>Scaffold-DbContext

Gerüste ein ' DbContext ' und Entität Typen für eine Datenbank.

Parameter:

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>***-Verbindung*** \<Zeichenfolge ></nobr> | Die Verbindungszeichenfolge zur Datenbank.                                                           |
| ***-Provider*** \<String>                | Die zu verwendenden Anbieter an. (Z. B. Microsoft.EntityFrameworkCore.SqlServer)                              |
| -OutputDir \<Zeichenfolge >                     | Das Verzeichnis in den Dateien versetzt. Pfade sind relativ zum Projektverzeichnis an.                      |
| -Kontext \<Zeichenfolge >                       | Der Name von ' DbContext ' zu generieren.                                                           |
| -Schemas \<String[]>                     | Die Schemas der Tabellen zur Generierung von Entitätstypen für.                                              |
| -Tables \<String[]>                      | Die Tabellen für Entitätstypen generieren.                                                         |
| -DataAnnotations                         | Verwenden Sie Attribute, um das Modell (sofern möglich) konfigurieren. Wenn nicht angegeben, wird nur die fluent-API verwendet. |
| -UseDatabaseNames                        | Verwenden Sie die Tabellen- und Spaltennamen direkt aus der Datenbank.                                           |
| -Force                                   | Überschreiben Sie vorhandene Dateien.                                                                        |

### <a name="script-migration"></a>Skript-Migration

Generiert ein SQL-Skript von Migrationen.

Parameter:

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| *-From* \<String> | Der Start Migration. Der Standardwert ist 0 (die ursprüngliche Datenbank).      |
| *-Zu* \<Zeichenfolge >   | Der Endwert Migration. Standardmäßig bis zum letzten Migration.              |
| -Idempotent       | Generiert ein Skript, das für eine Datenbank bei jeder Migration verwendet werden kann. |
| -Output \<Zeichenfolge > | Die Datei, schreibt das Ergebnis.                                   |

> [!TIP]
> To, From, und Output-Parameter unterstützen Tab-Taste.

### <a name="update-database"></a>Datenbank aktualisieren

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <nobr>*-Migration* \<Zeichenfolge ></nobr> | Der zielmigration. Bei "0" werden bei allen Migrationen rückgängig gemacht werden. Standardmäßig bis zum letzten Migration. |

> [!TIP]
> Der Parameter für die Migration unterstützt Tab-Taste.


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
