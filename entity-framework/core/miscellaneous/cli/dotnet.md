---
title: ".NET Core CLI – EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 26b5fb326d20575ed2f3c6955c699e0c3757bf57
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
<a name="ef-core-net-command-line-tools"></a>EF Core .NET-Befehlszeilentools
===============================
Das Entity Framework Core .NET Befehlszeilentools sind eine Erweiterung für die plattformübergreifende **Dotnet** Befehl, die Teil von der [.NET Core SDK][2].

> [!TIP]
> Wenn Sie Visual Studio verwenden, empfiehlt es sich [PMC Tools] [ 1] seitdem sie stattdessen eine weitere integrierte Lösung bieten.

<a name="installing-the-tools"></a>Installieren der Tools
--------------------
Installieren Sie die EF Core .NET-Befehlszeilentools mithilfe der folgenden Schritte:

1. Bearbeiten der Projektdatei, und fügen Sie Microsoft.EntityFrameworkCore.Tools.DotNet als DotNetCliToolReference-Element (siehe unten)
2. Führen Sie die folgenden Befehle ein:

       dotnet add package Microsoft.EntityFrameworkCore.Design
       dotnet restore


Das daraus resultierende Projekt sollte etwa wie folgt aussehen:

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
> Ein Verweis Paket mit `PrivateAssets="All"` bedeutet, er ist nicht verfügbar gemacht, um Projekte, die auf dieses Projekt verweisen, insbesondere für die Pakete, die in der Regel nur während der Entwicklung verwendet werden.

Wenn Sie alles richtig haben, sollten Sie den folgenden Befehl an einer Eingabeaufforderung erfolgreich ausführen können.

``` Console
dotnet ef
```

<a name="using-the-tools"></a>Mithilfe der tools
---------------
Bei jedem eines Befehls aufrufen umfasst zwei Projekte:

Das Zielprojekt ist keine Dateien hinzugefügt werden (oder in einigen Fällen entfernt). Das Zielprojekt wird standardmäßig auf das Projekt im aktuellen Verzeichnis, jedoch können geändert werden, mithilfe der <nobr> **– Projekt** </nobr> Option.

Das Startprojekt wird von den Tools emuliert wird, wenn der Code des Projekts ausführen. Auch wird standardmäßig auf das Projekt im aktuellen Verzeichnis, aber kann geändert werden, mithilfe der **--Startprojekt** Option.

Allgemeine Optionen:

|    |                                  |                             |
| -- | -------------------------------- | --------------------------- |
|    | --json                           | JSON-Ausgabe anzeigen.           |
| -c | --Kontext \<DBCONTEXT >           | Die DbContext verwenden.       |
| -P | --Projekt \<Projekt >             | Das Projekt verwendet werden soll.         |
| -s | --Startprojekt \<Projekt >     | Das Startup-Projekt verwenden. |
|    | --Framework \<FRAMEWORK >         | Das Zielframework.       |
|    | --Konfiguration \<CONFIGURATION > | Die zu verwendende Konfiguration.   |
|    | --Runtime \<BEZEICHNER >          | Die Laufzeit verwendet werden soll.         |
| -h | – Hilfe                           | Anzeigen von Hilfeinformationen.      |
| -v | -verbose                        | Zeigen Sie eine ausführlichen Ausgabe.        |
|    | --keine Farbe                       | Nicht zur farblichen Kennzeichnung der Ausgabe.      |
|    | --Präfix-Ausgabe                  | Präfix Ausgabe auf.   |


> [!TIP]
> Legen Sie zum Angeben der ASP.NET Core-Umgebung die **ASPNETCORE_ENVIRONMENT** Umgebungsvariablen vor der Ausführung.

<a name="commands"></a>Befehle
--------

### <a name="dotnet-ef-database-drop"></a>Dotnet Ef das Löschen einer Datenbank

Löscht die Datenbank.

Optionen:

|    |           |                                                          |
| -- | --------- | -------------------------------------------------------- |
| -f | --erzwingen   | Bestätigen Sie nicht.                                           |
|    | --Probelauf | Anzeigen, welche Datenbank verworfen werden, jedoch nicht, legen Sie sie. |

### <a name="dotnet-ef-database-update"></a>Dotnet Ef-Datenbankupdate

Aktualisiert die Datenbank in eine angegebene Migration.

Argumente:

|              |                                                                                              |
| ------------ | ---------------------------------------------------------------------------------------------|
| \<MIGRATION > | Der zielmigration. Bei 0 werden bei allen Migrationen rückgängig gemacht werden. Standardmäßig bis zum letzten Migration. |

### <a name="dotnet-ef-dbcontext-info"></a>Dotnet Ef Dbcontext info

Ruft Informationen zu einem DbContext-Typ ab.

### <a name="dotnet-ef-dbcontext-list"></a>Dotnet Ef Dbcontext-Liste

Listet die verfügbare DbContext-Typen.

### <a name="dotnet-ef-dbcontext-scaffold"></a>Dotnet Ef Dbcontext Gerüst

Gerüste ein ' DbContext ' und Entität Typen für eine Datenbank.

Argumente:

|               |                                                                     |
| ------------- | ------------------------------------------------------------------- |
| \<VERBINDUNG > | Die Verbindungszeichenfolge zur Datenbank.                              |
| \<ANBIETER >   | Die zu verwendenden Anbieter an. (D. h. Microsoft.EntityFrameworkCore.SqlServer) |

Optionen:

|                 |                                         |                                                          |
| --------------- | --------------------------------------- | -------------------------------------------------------- |
| <nobr>-d:</nobr> |       --datenanmerkungen                | Verwenden Sie Attribute, um das Modell (sofern möglich) konfigurieren. Wenn nicht angegeben, wird nur die fluent-API verwendet. |
|       -c        |       --Kontext \<NAME >                 | Der Name von ' DbContext '.                               |
|       -f        |       --erzwingen                           | Überschreiben Sie vorhandene Dateien.                                |
|       -o        |       --Ausgabe Dir \<Pfad >              | Das Verzeichnis in den Dateien versetzt. Pfade sind relativ zum Projektverzeichnis an. |
|                 | <nobr>--Schema \<SCHEMA_NAME >...</nobr> | Die Schemas der Tabellen zur Generierung von Entitätstypen für.      |
|       -t        |       --Tabelle \<TABLE_NAME >...          | Die Tabellen für Entitätstypen generieren.                 |
|                 |       – Verwenden der Datenbanknamen              | Verwenden Sie die Tabellen- und Spaltennamen direkt aus der Datenbank.   |

### <a name="dotnet-ef-migrations-add"></a>Dotnet Ef Migrationen hinzufügen

Fügt eine neue Migration.

Argumente:

|         |                            |
| ------- | -------------------------- |
| \<NAME > | Der Name der Migration. |

Optionen:

|                 |                                   |                                                                |
| --------------- |---------------------------------- | -------------------------------------------------------------- |
| <nobr>-o</nobr> | <nobr>--Ausgabe Dir \<Pfad ></nobr> | Das Verzeichnis (und Sub-Namespace) zu verwenden. Pfade sind relativ zum Projektverzeichnis an. Der Standardwert ist "Migration". |

### <a name="dotnet-ef-migrations-list"></a>Dotnet Ef Migrationen Liste

Listet die verfügbaren Migrationen.

### <a name="dotnet-ef-migrations-remove"></a>Entfernen Sie Dotnet Ef Migrationen

Entfernt die letzte Migration.

Optionen:

|    |         |                                                                       |
| -- | ------- | --------------------------------------------------------------------- |
| -f | --erzwingen | Überprüfen Sie nicht, um festzustellen, ob die Migration der Datenbank angewendet wurde. |

### <a name="dotnet-ef-migrations-script"></a>Dotnet Ef Migrationen Skript

Generiert ein SQL-Skript von Migrationen.

Argumente:

|         |                                                               |
| ------- | ------------------------------------------------------------- |
| \<AUS > | Der Start Migration. Der Standardwert ist 0 (die ursprüngliche Datenbank). |
| \<UM >   | Der Endwert Migration. Standardmäßig bis zum letzten Migration.         |

Optionen:

|    |                  |                                                                    |
| -- | ---------------- | ------------------------------------------------------------------ |
| -o | --Ausgabe \<Datei > | Die Datei, schreibt das Ergebnis.                                   |
| -i | --idempotent     | Generiert ein Skript, das für eine Datenbank bei jeder Migration verwendet werden kann. |


  [1]: powershell.md
  [2]: https://www.microsoft.com/net/core
