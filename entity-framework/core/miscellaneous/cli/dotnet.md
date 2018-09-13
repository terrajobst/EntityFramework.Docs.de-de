---
title: .NET Core-CLI - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 3534336f1caeed96079b35c739d694a536919020
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489608"
---
<a name="ef-core-net-command-line-tools"></a>EF .NET Core-Befehlszeilentools
===============================
Die Entity Framework Core .NET Command-Line-Tools sind eine Erweiterung der plattformübergreifenden **Dotnet** Befehl, der Teil von der [.NET Core SDK][2].

> [!TIP]
> Wenn Sie Visual Studio verwenden, empfehlen wir [PMC-Tools] [ 1] seit sie stattdessen ein besseres ganzheitliches Erlebnis bieten.

<a name="installing-the-tools"></a>Installieren der Tools
--------------------
> [!NOTE]
> Das .NET Core SDK 2.1.300-Version sowie neuere **Dotnet Ef** Befehle, die mit EF Core 2.0 und höheren Versionen kompatibel sind. Aus diesem Grund wird, wenn Sie neuere Versionen von .NET Core SDK und EF Core-Runtime verwenden, ist keine Installation erforderlich und können Sie den restlichen Teil dieses Abschnitts ignorieren.
>
> Auf der anderen Seite der **Dotnet Ef** Tool in .NET Core SDK 2.1.300-Version enthalten und höher ist nicht kompatibel mit EF Core-Version 1.0 und 1.1. Bevor Sie ein Projekt, das dieser früheren Versionen von EF Core auf einem Computer verwendet, die .NET Core SDK 2.1.300 arbeiten können, oder höher installiert, müssen Sie auch Version 2.1.200 installieren oder eine frühere SDK und konfigurieren Sie die Anwendung zur Verwendung dieser älteren Version durch Ändern der  ["Global.JSON"](https://docs.microsoft.com/en-us/dotnet/core/tools/global-json) Datei. Diese Datei befindet sich normalerweise in dem Projektmappenverzeichnis (eine über das Projekt). Anschließend können Sie mit der Installlation Anweisung unten fortfahren.

Für frühere Versionen von .NET Core SDK können Sie die EF Core-.NET-Befehlszeilentools mithilfe der folgenden Schritte installieren:

1. Bearbeiten Sie die Projektdatei aus, und fügen Sie Microsoft.EntityFrameworkCore.Tools.DotNet als ein Element "dotnetclitoolreference" (siehe unten)
2. Führen Sie die folgenden Befehle aus:

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore

Das resultierende Projekt sollte etwa wie folgt aussehen:

``` xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.EntityFrameworkCore.Design"
                      Version="2.0.0"
                      PrivateAssets="All" />
  </ItemGroup>
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
                            Version="2.0.0" />
  </ItemGroup>
</Project>
```

> [!NOTE]
> Einen Paketverweis mit `PrivateAssets="All"` bedeutet, es ist nicht verfügbar gemacht, um Projekte, die dieses Projekt verweisen, handelt es sich besonders für Pakete, die in der Regel nur während der Entwicklung verwendet werden.

Wenn Sie alles richtig war, sollten Sie erfolgreich den folgenden Befehl an einer Eingabeaufforderung ausführen können.

``` Console
dotnet ef
```

<a name="using-the-tools"></a>Mithilfe der tools
---------------
Wenn Sie einen Befehl aufrufen, sind zwei Projekte beteiligt:

Dem Zielprojekt werden Dateien hinzugefügt (oder sie werden in einigen Fällen aus diesem entfernt). Das Zielprojekt standardmäßig auf das Projekt im aktuellen Verzeichnis, jedoch können geändert werden, mithilfe der <nobr> **`--project`** </nobr> Option.

Das Startprojekt wird bei Ausführung des Projektcodes von den Tools emuliert. Ebenfalls standardmäßig auf das Projekt im aktuellen Verzeichnis, sondern kann geändert werden, mithilfe der **`--startup-project`** Option.

> [!NOTE]
> Z. B. Aktualisieren der Datenbank der Webanwendung, die EF Core-Installation in einem anderen Projekt sieht wie folgt: `dotnet ef database update --project {project-path}` (von Ihrem Web-app-Verzeichnis)

Allgemeine Optionen:

|    |                                  |                             |
|:---|:---------------------------------|:----------------------------|
|    | `--json`                           | Zeigen Sie die JSON-Ausgabe.           |
| -c | `--context <DBCONTEXT>`           | Die "DbContext" verwenden.       |
| -p | `--project <PROJECT>`             | Das Projekt zu verwenden.         |
| -s | `--startup-project <PROJECT>`     | Das Startprojekt verwenden. |
|    | `--framework <FRAMEWORK>`         | Das Zielframework.       |
|    | `--configuration <CONFIGURATION>` | Die zu verwendende Konfiguration.   |
|    | `--runtime <IDENTIFIER>`          | Die Laufzeit, die verwendet werden soll.         |
| -h | `--help`                           | Anzeigen von Hilfeinformationen.      |
| -v | `--verbose`                        | Zeigen Sie ausführlichen Ausgabe.        |
|    | `--no-color`                       | Nicht farbig anzuzeigende Ausgabe aus.      |
|    | `--prefix-output`                  | Das Präfix auf Ausgabe.   |


> [!TIP]
> Um die ASP.NET Core-Umgebung anzugeben, legen die **"aspnetcore_environment"** Umgebungsvariablen vor der Ausführung.

<a name="commands"></a>Befehle
--------

### <a name="dotnet-ef-database-drop"></a>Dotnet Ef Löschen einer Datenbank

Löscht die Datenbank.

Optionen:

|    |           |                                                          |
|:---|:----------|:---------------------------------------------------------|
| -f | `--force`   | Bestätigen Sie nicht.                                           |
|    | `--dry-run` | Anzeigen der Datenbank gelöscht werden würde, dies jedoch noch nicht. |

### <a name="dotnet-ef-database-update"></a>Dotnet-Ef-Datenbankupdates

Aktualisiert die Datenbank für eine angegebene Migration.

Argumente:

|              |                                                                                              |
|:-------------|:---------------------------------------------------------------------------------------------|
| `<MIGRATION>` | Die Ziel-Migration. Wenn der Wert 0 ist, werden bei allen Migrationen rückgängig gemacht werden. Ist standardmäßig auf die letzte Migration ein. |

### <a name="dotnet-ef-dbcontext-info"></a>Dotnet Ef-Dbcontext-Informationen

Ruft Informationen über einen "DbContext"-Typ ab.

### <a name="dotnet-ef-dbcontext-list"></a>Dotnet Ef-Dbcontext-Liste

Listet die verfügbare Typen von "DbContext".

### <a name="dotnet-ef-dbcontext-scaffold"></a>Dotnet Ef-Dbcontext-scaffold

Erstellt das Gerüst für einen DbContext und Entitätstypen Typen für eine Datenbank.

Argumente:

|               |                                                                             |
|:--------------|:----------------------------------------------------------------------------|
| `<CONNECTION>` | Die Verbindungszeichenfolge in der Datenbank.                                      |
| `<PROVIDER>`   | Die zu verwendende Anbieter. (z. B. "Microsoft.entityframeworkcore.SqlServer") |

Optionen:

|                 |                                         |                                                                                                  |
|:----------------|:----------------------------------------|:-------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr> | `--data-annotations`                      | Verwenden Sie Attribute, um das Modell (sofern möglich) zu konfigurieren. Wenn nicht angegeben, wird nur die fluent-API verwendet. |
| -c              | `--context <NAME>`                       | Der Name von "DbContext".                                                                       |
|                 | `--context-dir <PATH>`                   | Das Verzeichnis, fügen Sie in "DbContext"-Datei. Pfade sind relativ zum Projektverzeichnis.             |
| -f              | `--force`                                 | Überschreiben Sie vorhandene Dateien.                                                                        |
| -o              | `--output-dir <PATH>`                    | Zum Einfügen von Dateien im Verzeichnis. Pfade sind relativ zum Projektverzeichnis.                      |
|                 | <nobr>`--schema <SCHEMA_NAME>...`</nobr> | Die Schemas der Tabellen zur Generierung von Entitätstypen für.                                              |
| -t              | `--table <TABLE_NAME>`...                | Die Tabellen zur Generierung von Entitätstypen für werden soll.                                                         |
|                 | `--use-database-names`                    | Verwenden Sie Tabellen- und Spaltennamen direkt aus der Datenbank.                                           |

### <a name="dotnet-ef-migrations-add"></a>Fügen Dotnet Ef-Migrationen

Fügt eine neue Migration hinzu.

Argumente:

|         |                            |
|:--------|:---------------------------|
| `<NAME>` | Der Name der Migration. |

Optionen:

|                 |                                   |                                                                                                                  |
|:----------------|:----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <nobr>-o</nobr> | <nobr> `--output-dir <PATH>` </nobr> | Das Verzeichnis (und Sub-Namespace) verwenden. Pfade sind relativ zum Projektverzeichnis. Der Standardwert ist "Migrations". |

### <a name="dotnet-ef-migrations-list"></a>Dotnet Ef Migrations-Liste

Listet die verfügbaren Migrationen.

### <a name="dotnet-ef-migrations-remove"></a>Dotnet-Ef-Migrationen zu entfernen.

Entfernt die letzte Migration.

Optionen:

|    |         |                                                                       |
|:---|:--------|:----------------------------------------------------------------------|
| -f | `--force` | Die Migration zurückgesetzt, wenn es auf die Datenbank angewendet wurde. |

### <a name="dotnet-ef-migrations-script"></a>Dotnet Ef Migrations-Skript

Generiert ein SQL-Skript von Migrationen.

Argumente:

|         |                                                               |
|:--------|:--------------------------------------------------------------|
| `<FROM>` | Die Migration der ab. Der Standardwert ist 0 (die ursprüngliche Datenbank). |
| `<TO>`   | Der Endpunkt-Migration. Ist standardmäßig auf die letzte Migration ein.         |

Optionen:

|    |                  |                                                                    |
|:---|:-----------------|:-------------------------------------------------------------------|
| -o | `--output <FILE>` | Die Datei, schreibt das Ergebnis.                                   |
| -i | `--idempotent`     | Generieren Sie ein Skript, das für eine Datenbank auf eine Migration verwendet werden kann. |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
