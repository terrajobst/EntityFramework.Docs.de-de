---
title: Referenz zu EF Core Tools (.net CLI)-EF Core
author: bricelam
ms.author: bricelam
ms.date: 07/11/2019
uid: core/miscellaneous/cli/dotnet
ms.openlocfilehash: 0278353640ea242df9e6ee5278c9dda78bfd341b
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565265"
---
# <a name="entity-framework-core-tools-reference---net-cli"></a>Referenz zu Entity Framework Core Tools: .net CLI

Die CLI-Tools (Command-Line Interface, Befehlszeilenschnittstelle) für Entity Framework Core die Entwicklungsaufgaben zur Entwurfszeit ausführen. Beispielsweise erstellen Sie [Migrationen](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), wenden Migrationen an und generieren Code für ein Modell, das auf einer vorhandenen Datenbank basiert. Die Befehle sind eine Erweiterung des plattformübergreifenden [dotnet](/dotnet/core/tools) -Befehls, der Teil der [.net Core SDK](https://www.microsoft.com/net/core)ist. Diese Tools funktionieren mit .net Core-Projekten.

Wenn Sie Visual Studio verwenden, empfehlen wir stattdessen die [Tools der Paket-Manager-Konsole](powershell.md) :
* Sie arbeiten automatisch mit dem aktuellen Projekt, das in der **Paket-Manager-Konsole** ausgewählt ist, ohne dass Sie manuell Verzeichnisse wechseln müssten.
* Dateien, die von einem Befehl generiert werden, werden automatisch geöffnet, nachdem der Befehl abgeschlossen wurde.

## <a name="installing-the-tools"></a>Installieren der Tools

Das Installationsverfahren hängt vom Projekttyp und der Version ab:

* EF Core 3. x
* ASP.net Core Version 2,1 und höher
* EF Core 2. x
* EF Core 1. x

### <a name="ef-core-3x"></a>EF Core 3. x

* `dotnet ef`muss als globales oder lokales Tool installiert werden. Die meisten Entwickler werden `dotnet ef` mit dem folgenden Befehl als globales Tool installiert:

  ``` console
  dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

  Sie können auch als `dotnet ef` lokales Tool verwenden. Um es als lokales Tool zu verwenden, stellen Sie die Abhängigkeiten eines Projekts wieder her, das es mithilfe einer [Tool Manifest-Datei als Tool](https://github.com/dotnet/cli/issues/10288)Abhängigkeit deklariert.

* Installieren Sie den [.net Core SDK 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0)). Das SDK muss installiert werden, auch wenn Sie über die neueste Version von Visual Studio verfügen.

* Installieren Sie das `Microsoft.EntityFrameworkCore.Design` neueste Paket.

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

### <a name="aspnet-core-21"></a>ASP.net Core 2.1 und höher

* Installieren Sie die aktuelle [.net Core SDK](https://www.microsoft.com/net/download/core). Das SDK muss installiert werden, auch wenn Sie über die neueste Version von Visual Studio 2017 verfügen.

  Dies ist nur für ASP.net Core 2.1 und höher erforderlich, da das `Microsoft.EntityFrameworkCore.Design` Paket im [Metapaket Microsoft. aspnetcore. app](/aspnet/core/fundamentals/metapackage-app)enthalten ist.

### <a name="ef-core-2x-not-aspnet-core"></a>EF Core 2. x (nicht ASP.net Core)

Die `dotnet ef` Befehle sind im .net Core SDK enthalten, aber zum Aktivieren der Befehle, die Sie zum Installieren des `Microsoft.EntityFrameworkCore.Design` Pakets haben.

* Installieren Sie die aktuelle [.net Core SDK](https://www.microsoft.com/net/download/core). Das SDK muss installiert werden, auch wenn Sie über die neueste Version von Visual Studio verfügen.

* Installieren Sie das aktuellste stabile `Microsoft.EntityFrameworkCore.Design` Paket.

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

### <a name="ef-core-1x"></a>EF Core 1. x

* Installieren Sie die .net Core SDK Version 2.1.200. Spätere Versionen sind nicht mit den CLI-Tools für EF Core 1,0 und 1,1 kompatibel.

* Konfigurieren Sie die Anwendung für die Verwendung der 2.1.200 SDK-Version, indem Sie Ihre [Global. JSON](/dotnet/core/tools/global-json) -Datei ändern. Diese Datei ist normalerweise im Projektmappenverzeichnis enthalten (eines oberhalb des Projekts).

* Bearbeiten Sie die Projektdatei, `Microsoft.EntityFrameworkCore.Tools.DotNet` und fügen `DotNetCliToolReference` Sie Sie als-Element hinzu. Geben Sie die neueste Version von 1. x an, z. b.: 1.1.6. Weitere Informationen finden Sie im Beispiel für eine Projektdatei am Ende dieses Abschnitts.

* Installieren Sie die neueste Version von 1. x `Microsoft.EntityFrameworkCore.Design` des Pakets, z. b.:

  ```console
  dotnet add package Microsoft.EntityFrameworkCore.Design -v 1.1.6
  ```

  Wenn beide Paket Verweise hinzugefügt wurden, sieht die Projektdatei in etwa wie folgt aus:

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

  Ein Paket Verweis mit `PrivateAssets="All"` ist nicht für Projekte verfügbar, die auf dieses Projekt verweisen. Diese Einschränkung ist besonders nützlich für Pakete, die in der Regel nur während der Entwicklung verwendet werden.

### <a name="verify-installation"></a>Überprüfen der Installation

Führen Sie die folgenden Befehle aus, um sicherzustellen, dass EF Core CLI-Tools ordnungsgemäß installiert sind

  ``` Console
  dotnet restore
  dotnet ef
  ```

Die Ausgabe des Befehls identifiziert die Version der verwendeten Tools:

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

Vor der Verwendung der Tools müssen Sie möglicherweise ein Startprojekt erstellen oder die Umgebung festlegen.

### <a name="target-project-and-startup-project"></a>Ziel Projekt und Startprojekt

Die Befehle verweisen auf ein *Projekt* und ein *Startprojekt*.

* Das *Projekt* wird auch als *Ziel Projekt* bezeichnet, weil die Befehle Dateien hinzufügen oder entfernen. Standardmäßig ist das Projekt im aktuellen Verzeichnis das Ziel Projekt. Sie können ein anderes Projekt als Ziel Projekt angeben, indem Sie <nobr>`--project`</nobr> die Option verwenden.

* Das *Startprojekt ist das Startprojekt* , das von den Tools erstellt und ausgeführt wird. Die Tools müssen Anwendungscode zur Entwurfszeit ausführen, um Informationen zum Projekt zu erhalten, z. b. die Daten bankverbindungs Zeichenfolge und die Konfiguration des Modells. Standardmäßig ist das Projekt im aktuellen Verzeichnis das Startprojekt. Sie können ein anderes Projekt als Startprojekt angeben, indem Sie <nobr>`--startup-project`</nobr> die Option verwenden.

Das Startprojekt und das Ziel Projekt sind häufig das gleiche Projekt. Ein typisches Szenario, bei dem es sich um separate Projekte handelt, sind folgende:

* Die EF Core Kontext-und Entitäts Klassen befinden sich in einer .net Core-Klassenbibliothek.
* Eine .net Core-Konsolen-APP oder-Web-App verweist auf die Klassenbibliothek.

Es ist auch möglich, [Migrations Code in einer Klassenbibliothek zu platzieren, getrennt vom EF Core Kontext](xref:core/managing-schemas/migrations/projects).

### <a name="other-target-frameworks"></a>Andere Ziel-Frameworks

Die CLI-Tools funktionieren mit .net Core-Projekten und .NET Framework Projekten. Apps, die über das EF Core Modell in einer .NET Standard-Klassenbibliothek verfügen, verfügen möglicherweise nicht über ein .net Core-oder .NET Framework-Projekt. Dies gilt z. b. für xamarin-und universelle Windows-Plattform-apps. In solchen Fällen können Sie ein .net Core-Konsolen-App-Projekt erstellen, dessen einziger Zweck darin besteht, als Startprojekt für die Tools zu fungieren. Das Projekt kann ein Dummyprojekt ohne echten Code &mdash; sein. es ist nur erforderlich, um ein Ziel für die Tools bereitzustellen.

Warum ist ein Dummyprojekt erforderlich? Wie bereits erwähnt, müssen die Tools Anwendungscode zur Entwurfszeit ausführen. Hierzu müssen Sie die .net Core-Laufzeit verwenden. Wenn sich das EF Core Modell in einem Projekt befindet, das .net Core oder .NET Framework als Ziel hat, wird die Laufzeit von den EF Core Tools aus dem Projekt ausgeliehen. Dies ist nicht möglich, wenn sich das EF Core Modell in einer .NET Standard Klassenbibliothek befindet. Der .NET Standard ist keine tatsächliche .NET-Implementierung. Es handelt sich um eine Spezifikation für eine Reihe von APIs, die von .net-Implementierungen unterstützt werden müssen. Daher ist .NET Standard für die EF Core Tools nicht ausreichend, um Anwendungscode auszuführen. Das Dummyprojekt, das Sie als Startprojekt verwenden, stellt eine konkrete Zielplattform bereit, in die die Tools die .NET Standard Klassenbibliothek laden können.

### <a name="aspnet-core-environment"></a>ASP.net Core Umgebung

Um die Umgebung für ASP.net Core Projekte anzugeben, legen Sie die **ASPNETCORE_ENVIRONMENT** -Umgebungsvariable vor dem Ausführen von Befehlen fest.

## <a name="common-options"></a>Allgemeine Optionen

|                   | Option                            | Beschreibung                                                                                                                                                                                                                                                   |
|:------------------|:----------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                   | `--json`                          | JSON-Ausgabe anzeigen.                                                                                                                                                                                                                                             |
| <nobr>`-c`</nobr> | `--context <DBCONTEXT>`           | Die `DbContext`-Klasse, die verwendet werden soll. Der Klassenname oder voll qualifiziert mit Namespaces.  Wenn diese Option weggelassen wird, wird EF Core die Kontext Klasse finden. Wenn mehrere Kontext Klassen vorhanden sind, ist diese Option erforderlich.                                            |
| `-p`              | `--project <PROJECT>`             | Relativer Pfad zum Projektordner des Ziel Projekts.  Der Standardwert ist der aktuelle Ordner.                                                                                                                                                              |
| `-s`              | `--startup-project <PROJECT>`     | Relativer Pfad zum Projektordner des Start Projekts. Der Standardwert ist der aktuelle Ordner.                                                                                                                                                              |
|                   | `--framework <FRAMEWORK>`         | Der [zielframeworkmoniker](/dotnet/standard/frameworks#supported-target-framework-versions) für das [Ziel Framework](/dotnet/standard/frameworks).  Verwenden Sie, wenn die Projektdatei mehrere Ziel-Frameworks angibt, und wählen Sie eine davon aus. |
|                   | `--configuration <CONFIGURATION>` | Die Buildkonfiguration, z `Debug` . b. oder. `Release`                                                                                                                                                                                                   |
|                   | `--runtime <IDENTIFIER>`          | Der Bezeichner der Ziel Laufzeit, für die Pakete wieder hergestellt werden sollen. Eine Liste der Runtime-IDs (RIDs) finden Sie unter [RID-Katalog](/dotnet/core/rid-catalog).                                                                                                      |
| `-h`              | `--help`                          | Anzeigen von Hilfe Informationen.                                                                                                                                                                                                                                        |
| `-v`              | `--verbose`                       | Ausführliche Ausgabe anzeigen.                                                                                                                                                                                                                                          |
|                   | `--no-color`                      | Ausgabe nicht einfärben.                                                                                                                                                                                                                                        |
|                   | `--prefix-output`                 | Präfix Ausgabe mit Ebene.                                                                                                                                                                                                                                     |

## <a name="dotnet-ef-database-drop"></a>dotnet ef database drop

Löscht die Datenbank.

Optionen:

|                   | Option                   | Beschreibung                                              |
|:------------------|:-------------------------|:---------------------------------------------------------|
| <nobr>`-f`</nobr> | <nobr>`--force`</nobr>   | Nicht bestätigen.                                           |
|                   | <nobr>`--dry-run`</nobr> | Zeigen Sie an, welche Datenbank gelöscht werden soll, aber löschen Sie Sie nicht. |

## <a name="dotnet-ef-database-update"></a>dotnet ef database update

Aktualisiert die Datenbank auf die letzte Migration oder eine angegebene Migration.

Argumente:

| Argument      | Beschreibung                                                                                                                                                                                                                                                     |
|:--------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<MIGRATION>` | Die Ziel Migration. Migrationen können anhand des Namens oder der ID identifiziert werden. Die Zahl 0 (null) ist ein Sonderfall, der *vor der ersten Migration* steht und bewirkt, dass alle Migrationen rückgängig gemacht werden. Wenn keine Migration angegeben ist, wird für den Befehl standardmäßig die letzte Migration verwendet. |

In den folgenden Beispielen wird die-Datenbank auf eine angegebene Migration aktualisiert. Der erste verwendet den Migrations Namen, der zweite verwendet die Migrations-ID:

```console
dotnet ef database update InitialCreate
dotnet ef database update 20180904195021_InitialCreate
```

## <a name="dotnet-ef-dbcontext-info"></a>dotnet ef dbcontext info

Ruft Informationen zu einem `DbContext` Typ ab.

## <a name="dotnet-ef-dbcontext-list"></a>dotnet ef dbcontext list

Listet die `DbContext` verfügbaren Typen auf.

## <a name="dotnet-ef-dbcontext-scaffold"></a>dotnet ef dbcontext scaffold

Generiert Code für einen `DbContext` und Entitäts Typen für eine Datenbank. Damit dieser Befehl einen Entitätstyp generieren kann, muss die Datenbanktabelle über einen Primärschlüssel verfügen.

Argumente:

| Argument       | Beschreibung                                                                                                                                                                                                             |
|:---------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<CONNECTION>` | Die Verbindungs Zeichenfolge für die Datenbank. Bei ASP.net Core 2. x-Projekten kann der Wert *Name =\<Name der Verbindungs Zeichenfolge >* sein. In diesem Fall stammt der Name aus den Konfigurations Quellen, die für das Projekt eingerichtet sind. |
| `<PROVIDER>`   | Der zu verwendende Anbieter. In der Regel ist dies der Name des nuget-Pakets, z `Microsoft.EntityFrameworkCore.SqlServer`. b.:.                                                                                           |

Optionen:

|                 | Option                                   | Beschreibung                                                                                                                                                                    |
|:----------------|:-----------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>-d</nobr> | `--data-annotations`                     | Verwenden Sie Attribute, um das Modell zu konfigurieren (sofern möglich). Wenn diese Option weggelassen wird, wird nur die fließende API verwendet.                                                                |
| `-c`            | `--context <NAME>`                       | Der Name `DbContext` der zu generierenden Klasse.                                                                                                                                 |
|                 | `--context-dir <PATH>`                   | Das Verzeichnis, in das `DbContext` die Klassendatei eingefügt werden soll. Pfade sind relativ zum Projektverzeichnis. Namespaces werden aus den Ordnernamen abgeleitet.                                 |
| `-f`            | `--force`                                | Überschreibt vorhandene Dateien.                                                                                                                                                      |
| `-o`            | `--output-dir <PATH>`                    | Das Verzeichnis, in dem Entitäts Klassendateien abgelegt werden sollen. Pfade sind relativ zum Projektverzeichnis.                                                                                       |
|                 | <nobr>`--schema <SCHEMA_NAME>...`</nobr> | Die Schemas von Tabellen, für die Entitäts Typen generiert werden sollen. Wenn Sie mehrere Schemas angeben möchten `--schema` , wiederholen Sie diese für jeden Vorgang. Wenn diese Option weggelassen wird, werden alle Schemas eingeschlossen.          |
| `-t`            | `--table <TABLE_NAME>`...                | Die Tabellen, für die Entitäts Typen generiert werden sollen. Wenn Sie mehrere Tabellen angeben möchten `-t` , `--table` wiederholen Sie oder für jede einzelne Tabelle. Wenn diese Option weggelassen wird, werden alle Tabellen eingeschlossen.                |
|                 | `--use-database-names`                   | Verwenden Sie Tabellen-und Spaltennamen genau so, wie Sie in der Datenbank angezeigt werden. Wenn diese Option weggelassen wird, werden die Datenbanknamen entsprechend den C# Namensformat Konventionen genauer angepasst. |

Im folgenden Beispiel wird ein Gerüst für alle Schemas und Tabellen und die neuen Dateien im Ordner " *Models* " eingefügt.

```console
dotnet ef dbcontext scaffold "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -o Models
```

Im folgenden Beispiel wird nur ein Gerüst für ausgewählte Tabellen erstellt, und der Kontext wird in einem separaten Ordner mit einem angegebenen Namen erstellt:

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
| <nobr>`-o`</nobr> | <nobr>`--output-dir <PATH>`</nobr> | Das zu verwendende Verzeichnis (und der untergeordnete Namespace). Pfade sind relativ zum Projektverzeichnis. Der Standardwert ist "Migrationen". |

## <a name="dotnet-ef-migrations-list"></a>dotnet ef migrations list

Listet verfügbare Migrationen auf.

## <a name="dotnet-ef-migrations-remove"></a>dotnet ef migrations remove

Entfernt die letzte Migration (führt einen Rollback für die Codeänderungen aus, die für die Migration durchgeführt wurden).

Optionen:

|                   | Option    | Beschreibung                                                                     |
|:------------------|:----------|:--------------------------------------------------------------------------------|
| <nobr>`-f`</nobr> | `--force` | Setzen Sie die Migration zurück (führen Sie ein Rollback der Änderungen aus, die auf die Datenbank angewendet wurden). |

## <a name="dotnet-ef-migrations-script"></a>dotnet ef migrations script

Generiert ein SQL-Skript aus Migrationen.

Argumente:

| Argument | Beschreibung                                                                                                                                                   |
|:---------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `<FROM>` | Die Migration wird gestartet. Migrationen können anhand des Namens oder der ID identifiziert werden. Die Zahl 0 (null) ist ein Sonderfall, der *vor der ersten Migration*liegt. Der Standardwert ist 0. |
| `<TO>`   | Die Beendigung der Migration. Standardmäßig wird die letzte Migration verwendet.                                                                                                         |

Optionen:

|                   | Option            | Beschreibung                                                        |
|:------------------|:------------------|:-------------------------------------------------------------------|
| <nobr>`-o`</nobr> | `--output <FILE>` | Die Datei, in die das Skript geschrieben werden soll.                                   |
| `-i`              | `--idempotent`    | Generieren Sie ein Skript, das bei jeder Migration in einer Datenbank verwendet werden kann. |

Im folgenden Beispiel wird ein Skript für die InitialCreate-Migration erstellt:

```console
dotnet ef migrations script 0 InitialCreate
```

Im folgenden Beispiel wird nach der InitialCreate-Migration ein Skript für alle Migrationen erstellt.

```console
dotnet ef migrations script 20180904195021_InitialCreate
```

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Migrationen](xref:core/managing-schemas/migrations/index)
* [Reverse Engineering](xref:core/managing-schemas/scaffolding)
