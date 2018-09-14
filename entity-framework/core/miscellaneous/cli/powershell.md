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
<a name="ef-core-package-manager-console-tools"></a>Tools für EF Core-Paket-Manager-Konsole
=====================================
Die Paket-Manager-Konsole (PMC) von EF Core-Tools ausführen in Visual Studio mithilfe von NuGet [-Paket-Manager-Konsole][2].
Diese Tools funktionieren sowohl mit .NET Framework- als auch mit .NET Core-Projekten.

> [!TIP]
> Verwenden Sie nicht Visual Studio ein? Die [EF Core-Befehlszeilentools] [ 1] sind – plattformübergreifend und in einer Eingabeaufforderung ausführen.

<a name="installing-the-tools"></a>Installieren der Tools
--------------------
Installieren Sie die Tools der EF Core-Paket-Manager-Konsole, indem das Microsoft.EntityFrameworkCore.Tools-NuGet-Paket installieren.
Sie können es installieren, indem Sie in den folgenden Befehl ausführen [-Paket-Manager-Konsole][2].

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

Wenn alles ordnungsgemäß funktioniert, sollten Sie diesen Befehl ausführen können:

``` powershell
Get-Help about_EntityFrameworkCore
```
> [!TIP]
> Wenn Startprojekt .NET Standard abzielt [plattformübergreifend eine Zielversionen ein unterstützten Framework] [ 3] vor der Verwendung der Tools.

> [!IMPORTANT]
> Bei Verwendung von **Universal Windows** oder **Xamarin**, verschieben Sie den EF-Code in eine .NET Standard-Klassenbibliothek und [plattformübergreifend eine Zielversionen ein unterstützten Framework] [ 3] vor der Verwendung der Tools. Geben Sie die Klassenbibliothek als Startprojekt fest.

<a name="using-the-tools"></a>Mithilfe der tools
---------------
Wenn Sie einen Befehl aufrufen, sind zwei Projekte beteiligt:

Dem Zielprojekt werden Dateien hinzugefügt (oder sie werden in einigen Fällen aus diesem entfernt). Das Zielprojekt standardmäßig die **Standardprojekt** in Paket-Manager-Konsole ausgewählt, können aber auch festgelegt werden mithilfe von-Projektparameter.

Das Startprojekt wird bei Ausführung des Projektcodes von den Tools emuliert. Es ist standardmäßig auf eine **als Startprojekt festlegen** im Projektmappen-Explorer. Sie können auch mit dem StartupProject - Parameter angegeben werden.

Allgemeine Parameter:

|                           |                             |
|:--------------------------|:----------------------------|
| -Kontext \<Zeichenfolge >        | Die "DbContext" verwenden.       |
| -Projekt \<Zeichenfolge >        | Das Projekt zu verwenden.         |
| -StartupProject \<Zeichenfolge > | Das Startprojekt verwenden. |
| -Verbose                  | Zeigen Sie ausführlichen Ausgabe.        |

Um Hilfeinformationen zu einem Befehl zu anzuzeigen, verwenden Sie PowerShell `Get-Help` Befehl.

> [!TIP]
> Der Kontext, Projekt- und StartupProject parameterunterstützung Tab-Taste.

> [!TIP]
> Legen Sie **Env:ASPNETCORE_ENVIRONMENT** vor dem Ausführen, um die ASP.NET Core-Umgebung anzugeben.

<a name="commands"></a>Befehle
--------

### <a name="add-migration"></a>Add-Migration

Fügt eine neue Migration hinzu.

Parameter:

|                                   |                                                                                                                  |
|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| ***--Name*** \<Zeichenfolge >             | Der Name der Migration.                                                                                       |
| <nobr>-OutputDir \<Zeichenfolge ></nobr> | Das Verzeichnis (und Sub-Namespace) verwenden. Pfade sind relativ zum Projektverzeichnis. Der Standardwert ist "Migrations". |

> [!NOTE]
> Parameter in **fett** sind erforderlich, und diejenigen *Kursiv* sind mit Feldern fester Breite.

### <a name="drop-database"></a>Drop-Database

Löscht die Datenbank.

Parameter:

|         |                                                          |
|:--------|:---------------------------------------------------------|
| -WhatIf | Anzeigen der Datenbank gelöscht werden würde, dies jedoch noch nicht. |

### <a name="get-dbcontext"></a>"Get-DbContext"

Ruft Informationen über einen "DbContext"-Typ ab.

### <a name="remove-migration"></a>Remove-Migration

Entfernt die letzte Migration.

Parameter:

|        |                                                              |
|:-------|:-------------------------------------------------------------|
| -Force | Die Migration zurückgesetzt, wenn es auf die Datenbank angewendet wurde. |

### <a name="scaffold-dbcontext"></a>"Scaffold-DbContext"

Erstellt das Gerüst für einen DbContext und Entitätstypen Typen für eine Datenbank.

Parameter:

|                                          |                                                                                                  |
|:-----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>***-Connection*** \<Zeichenfolge ></nobr> | Die Verbindungszeichenfolge in der Datenbank.                                                           |
| ***-Anbieter*** \<Zeichenfolge >                | Die zu verwendende Anbieter. (z. B. "Microsoft.entityframeworkcore.SqlServer")                      |
| -OutputDir \<Zeichenfolge >                     | Zum Einfügen von Dateien im Verzeichnis. Pfade sind relativ zum Projektverzeichnis.                      |
| -ContextDir \<Zeichenfolge >                    | Das Verzeichnis, fügen Sie in "DbContext"-Datei. Pfade sind relativ zum Projektverzeichnis.             |
| -Kontext \<Zeichenfolge >                       | Der Name von "DbContext", um zu generieren.                                                           |
| -Schemas \<String [] >                     | Die Schemas der Tabellen zur Generierung von Entitätstypen für.                                              |
| -Tabellen \<String [] >                      | Die Tabellen zur Generierung von Entitätstypen für werden soll.                                                         |
| -"DataAnnotations"                         | Verwenden Sie Attribute, um das Modell (sofern möglich) zu konfigurieren. Wenn nicht angegeben, wird nur die fluent-API verwendet. |
| -UseDatabaseNames                        | Verwenden Sie Tabellen- und Spaltennamen direkt aus der Datenbank.                                           |
| -Force                                   | Überschreiben Sie vorhandene Dateien.                                                                        |

### <a name="script-migration"></a>Script-Migration

Generiert ein SQL-Skript von Migrationen.

Parameter:

|                   |                                                                    |
|:------------------|:-------------------------------------------------------------------|
| *– Von* \<Zeichenfolge > | Die Migration der ab. Der Standardwert ist 0 (die ursprüngliche Datenbank).      |
| *– Bis* \<Zeichenfolge >   | Der Endpunkt-Migration. Ist standardmäßig auf die letzte Migration ein.              |
| -Idempotent       | Generieren Sie ein Skript, das für eine Datenbank auf eine Migration verwendet werden kann. |
| -Ausgabe \<Zeichenfolge > | Die Datei, schreibt das Ergebnis.                                   |

> [!TIP]
> To, From, und Output-Parameter unterstützen die Tab-Taste.

### <a name="update-database"></a>Update-Database

|                                     |                                                                                                |
|:------------------------------------|:-----------------------------------------------------------------------------------------------|
| <nobr>*-Migration* \<Zeichenfolge ></nobr> | Die Ziel-Migration. Wenn Sie '0' werden bei allen Migrationen rückgängig gemacht werden. Ist standardmäßig auf die letzte Migration ein. |

> [!TIP]
> Der Parameter für die Migration unterstützt die Tab-Taste.


  [1]: dotnet.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: index.md#frameworks
