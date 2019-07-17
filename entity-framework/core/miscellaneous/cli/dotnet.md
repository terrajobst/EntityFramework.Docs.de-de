---
title: EF Core-Tools-Verweis (.NET CLI) – EF Core
author: bricelam
ms.author: bricelam
ms.date: 07/11/2019
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 05c5f89fc79556e72a7e629c147aa817fe7d1a6b
ms.sourcegitcommit: e90d6cfa3e96f10b8b5275430759a66a0c714ed1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2019
ms.locfileid: "68286465"
---
# <a name="entity-framework-core-tools-reference---net-cli"></a>Referenz zu Entity Framework Core Tools - .NET CLI

Die Befehlszeilenschnittstelle (CLI)-Tools für Entity Framework Core führen Sie während der Entwurfszeit-Entwicklungsaufgaben. Angenommen, sie erstellen [Migrationen](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), Anwenden von Migrationen und Code für ein Modell basierend auf einer vorhandenen Datenbank generieren. Die Befehle sind eine Erweiterung für die plattformübergreifende [Dotnet](/dotnet/core/tools) Befehl, der Teil von der [.NET Core SDK](https://www.microsoft.com/net/core). Diese Tools funktionieren mit .NET Core-Projekten.

Wenn Sie Visual Studio verwenden, empfehlen wir die [-Paket-Manager-Konsole Tools](powershell.md) stattdessen:
* Sie automatisch mit dem aktuellen Projekt, das in ausgewählten funktionieren die **-Paket-Manager-Konsole** ohne, dass Sie manuell zwischen Verzeichnissen zu wechseln.
* Öffnen Sie automatisch Dateien, die von einem Befehl generiert wird, nachdem der Befehl abgeschlossen ist.

## <a name="installing-the-tools"></a>Installieren der Tools

Der Installationsvorgang zu starten, hängt von Projekttyp und der Version ab:

* EF Core 3.x
* ASP.NET Core Version 2.1 und höher
* EF Core 2.x
* EF Core 1.x

### <a name="ef-core-3x"></a>EF Core 3.x

* `dotnet ef` muss als globale oder lokale Tool installiert werden. Die meisten Entwickler installiert `dotnet ef` als globale Tool mit dem folgenden Befehl:

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

  Sie können auch `dotnet ef` als lokale Tool. Um es als lokale Tool verwenden, Wiederherstellen der Abhängigkeiten eines Projekts, das ihn, als Tools Abhängigkeit mithilfe deklariert einer [Tool-Manifestdatei](https://github.com/dotnet/cli/issues/10288).

* Installieren Sie die [.NET Core SDK 3.0](https://dotnet.microsoft.com/download/dotnet-core/3.0)). Das SDK muss installiert sein, auch wenn Sie die neueste Version von Visual Studio verfügen.

* Installieren Sie das neueste `Microsoft.EntityFrameworkCore.Design` Paket.

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

### <a name="aspnet-core-21"></a>ASP.NET Core 2.1 +

* Installieren Sie die aktuelle [.NET Core SDK](https://www.microsoft.com/net/download/core). Das SDK muss installiert werden, auch wenn Sie über die neueste Version von Visual Studio 2017 verfügen.

  Dies ist alles, was für ASP.NET Core 2.1 + erforderlich ist, da die `Microsoft.EntityFrameworkCore.Design` Paket befindet sich auf die [Microsoft.AspNetCore.App metapaket](/aspnet/core/fundamentals/metapackage-app).

### <a name="ef-core-2x-not-aspnet-core"></a>EF Core 2.x (nicht in ASP.NET Core)

Die `dotnet ef` Befehle sind in .NET Core SDK enthalten, aber so aktivieren Sie die Befehle müssen Sie installieren die `Microsoft.EntityFrameworkCore.Design` Paket.

* Installieren Sie die aktuelle [.NET Core SDK](https://www.microsoft.com/net/download/core). Das SDK muss installiert sein, auch wenn Sie die neueste Version von Visual Studio verfügen.

* Installieren Sie die neueste stabile `Microsoft.EntityFrameworkCore.Design` Paket.

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

### <a name="ef-core-1x"></a>EF Core 1.x

* Installieren Sie .NET Core SDK, Version 2.1.200. Höhere Versionen sind nicht kompatibel mit CLI-Tools für EF Core 1.0 und 1.1.

* Konfigurieren Sie die Anwendung auf Version des SDKS verwenden die 2.1.200 durch Ändern der ["Global.JSON"](/dotnet/core/tools/global-json) Datei. Diese Datei befindet sich normalerweise in dem Projektmappenverzeichnis (eine über das Projekt).

* Bearbeiten die Projektdatei und fügen `Microsoft.EntityFrameworkCore.Tools.DotNet` als eine `DotNetCliToolReference` Element. Geben Sie die neueste Version der 1.x, z.B.: 1.1.6. Finden Sie in der Beispielprojektdatei wird am Ende dieses Abschnitts.

* Installieren Sie die neueste Version 1.x der der `Microsoft.EntityFrameworkCore.Design` Verpacken, z.B.:

  ```console
  dotnet add package Microsoft.EntityFrameworkCore.Design -v 1.1.6
  ```

  Mit beiden Paketverweise hinzugefügt wird sieht die Projektdatei etwa folgendermaßen aus:

  ``` xml
  <Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
      <OutputType>Exe</OutputType>
      <TargetFramework>netcoreapp1.1</TargetFramework>
    </PropertyGroup>
    <ItemGroup>
      <PackageReference Include="Microsoft.EntityFrameworkCore.Design"
                        Version="1.1.6"
                         PrivateAssets="All" />
    </ItemGroup>
    <ItemGroup>
       <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
                              Version="1.1.6" />
    </ItemGroup>
  </Project>
  ```

  Einen Paketverweis mit `PrivateAssets="All"` ist nicht verfügbar gemacht werden, für Projekte, die dieses Projekt verweisen. Diese Einschränkung ist besonders nützlich für Pakete, die in der Regel nur während der Entwicklung verwendet werden.

### <a name="verify-installation"></a>Überprüfen der installation

Führen Sie die folgenden Befehle aus, um sicherzustellen, dass EF Core-CLI-Tools ordnungsgemäß installiert sind:

  ``` Console
  dotnet restore
  dotnet ef
  ```

Die Ausgabe des Befehls identifiziert die Version der Tools verwendet:

```console

                     _/\__
               ---==/    \\
         ___  ___   |.    \|\
        | __|| __|  |  )   \\\
        | _| | _|   \_/ |  //|\\
        |___||_|       /   \\\/\\

Entity Framework Core .NET Command-line Tools 2.1.3-rtm-32065

<Usage documentation follows, not shown.>
```

## <a name="using-the-tools"></a>Mithilfe der Tools

Bevor Sie mit den Tools müssen Sie möglicherweise ein Startprojekt erstellen, oder legen Sie die Umgebung.

### <a name="target-project-and-startup-project"></a>Zielprojekt und Startup-Projekt

Die Befehle finden Sie in einem *Projekt* und ein *Startprojekt*.

* Die *Projekt* ist auch bekannt als die *Zielprojekt* da es sich handelt, in dem Sie die Befehle zum Hinzufügen oder Entfernen von Dateien. Standardmäßig ist das Projekt im aktuellen Verzeichnis das Zielprojekt. Sie können ein anderes Projekt als Zielprojekt angeben, mit der <nobr> `--project` </nobr> Option.

* Die *Startprojekt* ist diejenige, die die Tools erstellen und ausführen. Die Tools werden zum Ausführen der Anwendungscode zur Entwurfszeit zum Abrufen von Informationen über das Projekt, z. B. die Datenbank-Verbindungszeichenfolge und die Konfiguration des Modells. Standardmäßig ist das Projekt im aktuellen Verzeichnis das Startprojekt an. Sie können ein anderes Projekt als Startprojekt angeben, mit der <nobr> `--startup-project` </nobr> Option.

Das Startprojekt und ein Zielprojekt sind häufig die gleichen Projekt. Ein typisches Szenario, in dem sie separate Projekte sind, ist:

* Die EF Core-Kontext und Entitätsklassen-Klassen sind in einer .NET Core-Klassenbibliothek.
* Ein .NET Core-Konsolen-app oder Web-app verweist auf die Klassenbibliothek.

Es ist auch möglich, [platzieren migrationscode in eine Klassenbibliothek, die getrennt von den Entity Framework Core-Kontext](xref:core/managing-schemas/migrations/projects).

### <a name="other-target-frameworks"></a>Andere Zielframeworks

Die CLI-Tools funktionieren mit .NET Core-Projekten und .NET Framework-Projekten. Apps, die EF Core-Modell in einer .NET Standard-Klassenbibliothek verfügen, möglicherweise keiner .NET Core- oder .NET Framework-Projekt. Dies ist z. B. "true" mit Xamarin und die universelle Windows-Plattform-apps. In solchen Fällen können Sie ein Konsolenanwendungsprojekt für .NET Core erstellen, dessen einziger Zweck ist es, als Startprojekt für die Tools zu fungieren. Das Projekt kann ein dummyprojekt ohne realen Code sein &mdash; ist nur erforderlich, ein Ziel für die Tools bereitzustellen.

Warum ist ein dummyprojekt erforderlich? Wie bereits erwähnt, haben die Tools zum Ausführen der Anwendungscode zur Entwurfszeit an. Zu diesem Zweck müssen sie die .NET Core-Runtime zu verwenden. Wenn die EF Core-Modell in einem Projekt, die auf .NET Core oder .NET Framework abzielt ist, nutzen die EF Core-Tools die Laufzeit aus dem Projekt. Sie können nicht dies tun, wenn das EF Core-Modell in einer .NET Standard-Klassenbibliothek ist. .NET Standard ist keine tatsächliche Implementierung von .NET. Es ist eine Spezifikation eines Satzes von APIs, die .NET-Implementierungen unterstützt wird müssen. Aus diesem Grund ist .NET Standard nicht ausreichend für die EF Core-Tools, um Anwendungscode auszuführen. Dem dummyprojekt, die Sie erstellen, für die Verwendung als Startup-Projekt enthält eine konkrete Zielplattform, die in der die Tools die .NET Standard-Klassenbibliothek laden können.

### <a name="aspnet-core-environment"></a>ASP.NET Core-Umgebung

Legen Sie zum Angeben der Umgebung für ASP.NET Core-Projekten die **"aspnetcore_environment"** Umgebungsvariablen vor dem Ausführen der Befehle.

## <a name="common-options"></a>Allgemeine Optionen

|                   | Option                            | Beschreibung                                                                                                                                                                                                                                                   |
|:------------------|:----------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                   | `--json`                          | Zeigen Sie die JSON-Ausgabe.                                                                                                                                                                                                                                             |
| <nobr>`-c`</nobr> | `--context <DBCONTEXT>`           | Die `DbContext`-Klasse, die verwendet werden soll. Name der Klasse nur oder mit Namespaces vollqualifiziert.  Wenn diese Option ausgelassen wird, findet das EF Core die Context-Klasse. Diese Option ist erforderlich, wenn es mehrere Kontextklassen sind.                                            |
| `-p`              | `--project <PROJECT>`             | Relativer Pfad in den Projektordner des Zielprojekts.  Standardwert ist der aktuelle Ordner an.                                                                                                                                                              |
| `-s`              | `--startup-project <PROJECT>`     | Relativer Pfad in den Projektordner für das Startprojekt. Standardwert ist der aktuelle Ordner an.                                                                                                                                                              |
|                   | `--framework <FRAMEWORK>`         | Die [Zielframework-Moniker](/dotnet/standard/frameworks#supported-target-framework-versions) für die [Zielframework](/dotnet/standard/frameworks).  Wird verwendet, wenn die Projektdatei mehrere Zielframeworks gibt, und Sie eine dieser Optionen auswählen möchten. |
|                   | `--configuration <CONFIGURATION>` | Die Build-Konfiguration, z. B.: `Debug` oder `Release`.                                                                                                                                                                                                   |
|                   | `--runtime <IDENTIFIER>`          | Der Bezeichner der Ziellaufzeit zum Wiederherstellen von Paketen für. Eine Liste der Runtime-IDs (RIDs) finden Sie unter [RID-Katalog](/dotnet/core/rid-catalog).                                                                                                      |
| `-h`              | `--help`                          | Anzeigen von Hilfeinformationen.                                                                                                                                                                                                                                        |
| `-v`              | `--verbose`                       | Zeigen Sie ausführlichen Ausgabe.                                                                                                                                                                                                                                          |
|                   | `--no-color`                      | Nicht farbig anzuzeigende Ausgabe aus.                                                                                                                                                                                                                                        |
|                   | `--prefix-output`                 | Das Präfix auf Ausgabe.                                                                                                                                                                                                                                     |

## <a name="dotnet-ef-database-drop"></a>dotnet ef database drop

Löscht die Datenbank.

Optionen:

|                   | Option                   | Beschreibung                                              |
|:------------------|:-------------------------|:---------------------------------------------------------|
| <nobr>`-f`</nobr> | <nobr>`--force`</nobr>   | Bestätigen Sie nicht.                                           |
|                   | <nobr>`--dry-run`</nobr> | Anzeigen der Datenbank gelöscht werden würde, dies jedoch noch nicht. |

## <a name="dotnet-ef-database-update"></a>dotnet ef database update

Aktualisiert die Datenbank aus, um die letzte Migration oder für eine angegebene Migration.

Argumente:

| Argument      | Beschreibung                                                                                                                                                                                                                                                     |
|:--------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<MIGRATION>` | Die Ziel-Migration. Migrationen können nach Name oder ID identifiziert werden Die Zahl 0 ist ein Sonderfall, das bedeutet, dass *vor der ersten Migration* und bewirkt, dass alle Migrationen zu rückgängig gemacht werden. Wenn keine Migration angegeben ist, verwendet der Befehl auf die letzte Migration. |

In den folgenden Beispielen wird die Datenbank für eine angegebene Migration aktualisieren. Das erste verwendet den Migrationsnamen aus, und die zweite verwendet den Migrations-ID:

```console
dotnet ef database update InitialCreate
dotnet ef database update 20180904195021_InitialCreate
```

## <a name="dotnet-ef-dbcontext-info"></a>dotnet ef dbcontext info

Ruft Informationen über eine `DbContext` Typ.

## <a name="dotnet-ef-dbcontext-list"></a>dotnet ef dbcontext list

Listet verfügbare `DbContext` Typen.

## <a name="dotnet-ef-dbcontext-scaffold"></a>dotnet ef dbcontext scaffold

Generiert Code für eine `DbContext` und Entitätstypen für eine Datenbank. Damit für diesen Befehl zum Generieren eines Entitätstyps muss in der Datenbanktabelle einen Primärschlüssel verfügen.

Argumente:

| Argument       | Beschreibung                                                                                                                                                                                                             |
|:---------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<CONNECTION>` | Die Verbindungszeichenfolge in der Datenbank. Der Wert kann für ASP.NET Core 2.x-Projekte sein *Name =\<Name der Verbindungszeichenfolge >* . In diesem Fall stammt der Name der Konfigurationsquellen, die für das Projekt eingerichtet sind. |
| `<PROVIDER>`   | Der zu verwendende Anbieter. Normalerweise hat der Name des NuGet-Pakets, z. B.: `Microsoft.EntityFrameworkCore.SqlServer`.                                                                                           |

Optionen:

|                 | Option                                   | Beschreibung                                                                                                                                                                    |
|:----------------|:-----------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr> | `--data-annotations`                     | Verwenden Sie Attribute, um das Modell (sofern möglich) zu konfigurieren. Wenn diese Option ausgelassen wird, wird nur die fluent-API verwendet.                                                                |
| `-c`            | `--context <NAME>`                       | Der Name des der `DbContext` Klasse generiert.                                                                                                                                 |
|                 | `--context-dir <PATH>`                   | Verzeichnis zum Ablegen der `DbContext` Klassendatei in. Pfade sind relativ zum Projektverzeichnis. Namespaces werden von den Ordnernamen abgeleitet.                                 |
| `-f`            | `--force`                                | Überschreiben Sie vorhandene Dateien.                                                                                                                                                      |
| `-o`            | `--output-dir <PATH>`                    | Das Verzeichnis einzufügenden Entität Klassendateien. Pfade sind relativ zum Projektverzeichnis.                                                                                       |
|                 | <nobr>`--schema <SCHEMA_NAME>...`</nobr> | Die Schemas der Tabellen zur Generierung von Entitätstypen für. Um mehrere Schemas anzugeben, wiederholen Sie die `--schema` jeweils. Wenn diese Option ausgelassen wird, sind alle Schemas enthalten.          |
| `-t`            | `--table <TABLE_NAME>`...                | Die Tabellen zur Generierung von Entitätstypen für werden soll. Um mehrere Tabellen anzugeben, wiederholen Sie die `-t` oder `--table` jeweils. Wenn diese Option ausgelassen wird, sind alle Tabellen enthalten.                |
|                 | `--use-database-names`                   | Verwenden Sie Tabellen- und Spaltennamen, genau wie in der Datenbank. Wenn diese Option ausgelassen wird, werden die Datenbanknamen geändert, um genauer zu Konventionen für C#-Namen entsprechen. |

Das folgende Beispiel erstellt das Gerüst für alle Schemas und Tabellen und fügt die neuen Dateien in die *Modelle* Ordner.

```console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -o Models
```

Das folgende Beispiel erstellt das Gerüst für nur ausgewählte Tabellen und den Kontext in einem separaten Ordner mit dem angegebenen Namen erstellt:

```console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -o Models -t Blog -t Post --context-dir Context -c BlogContext
```

## <a name="dotnet-ef-migrations-add"></a>dotnet ef migrations add

Fügt eine neue Migration hinzu.

Argumente:

| Argument | Beschreibung                |
|:---------|:---------------------------|
| `<NAME>` | Der Name der Migration. |

Optionen:

|                   | Option                             | Beschreibung                                                                                                      |
|:------------------|:-----------------------------------|:-----------------------------------------------------------------------------------------------------------------|
| <nobr>`-o`</nobr> | <nobr>`--output-dir <PATH>`</nobr> | Das Verzeichnis (und Sub-Namespace) verwenden. Pfade sind relativ zum Projektverzeichnis. Der Standardwert ist "Migrations". |

## <a name="dotnet-ef-migrations-list"></a>dotnet ef migrations list

Listet die verfügbaren Migrationen.

## <a name="dotnet-ef-migrations-remove"></a>dotnet ef migrations remove

Entfernt die letzte Migration (Rollback die codeänderungen, die für die Migration durchgeführt wurden).

Optionen:

|                   | Option    | Beschreibung                                                                     |
|:------------------|:----------|:--------------------------------------------------------------------------------|
| <nobr>`-f`</nobr> | `--force` | Zurücksetzen die Migration (ein Rollback die Änderungen, die auf die Datenbank angewendet wurden). |

## <a name="dotnet-ef-migrations-script"></a>dotnet ef migrations script

Generiert ein SQL-Skript von Migrationen.

Argumente:

| Argument | Beschreibung                                                                                                                                                   |
|:---------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<FROM>` | Die Migration der ab. Migrationen können nach Name oder ID identifiziert werden Die Zahl 0 ist ein Sonderfall, das bedeutet, dass *vor der ersten Migration*. Der Standardwert ist 0. |
| `<TO>`   | Der Endpunkt-Migration. Ist standardmäßig auf die letzte Migration ein.                                                                                                         |

Optionen:

|                   | Option            | Beschreibung                                                        |
|:------------------|:------------------|:-------------------------------------------------------------------|
| <nobr>`-o`</nobr> | `--output <FILE>` | Die Datei, die das Skript zu schreiben.                                   |
| `-i`              | `--idempotent`    | Generieren Sie ein Skript, das für eine Datenbank auf eine Migration verwendet werden kann. |

Das folgende Beispiel erstellt ein Skript für die Migration InitialCreate:

```console
dotnet ef migrations script 0 InitialCreate
```

Das folgende Beispiel erstellt ein Skript für alle Migrationen nach der Migration InitialCreate.

```console
dotnet ef migrations script 20180904195021_InitialCreate
```

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Migrationen](xref:core/managing-schemas/migrations/index)
* [Reverse Engineering](xref:core/managing-schemas/scaffolding)
