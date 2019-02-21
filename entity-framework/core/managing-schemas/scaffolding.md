---
title: Reverse-Engineering - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: 6e61d2ebcf5ada365dcdb264bc371199574e12fa
ms.sourcegitcommit: 33b2e84dae96040f60a613186a24ff3c7b00b6db
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/21/2019
ms.locfileid: "56459184"
---
# <a name="reverse-engineering"></a>Reverse-Engineering

Reverse Engineering ist der Prozess der Gerüstbau, Entity-Typ-Klassen und eine DbContext-Klasse, die basierend auf einem Datenbankschema. Sie können mithilfe von ausgeführt werden die `Scaffold-DbContext` Befehl der Paket-Manager-Konsole (PMC) von EF Core-Tools oder `dotnet ef dbcontext scaffold` -Befehl von .NET Command-Line Interface (CLI)-Tools.

## <a name="installing"></a>Installation

Vor reverse Engineering, müssen Sie zum Installieren der [PMC-Tools](xref:core/miscellaneous/cli/powershell) (nur Visual Studio) oder die [CLI-Tools](xref:core/miscellaneous/cli/dotnet). Finden Sie Links Weitere Informationen.

Sie müssen auch eine entsprechende installieren [Datenbankanbieter](xref:core/providers/index) für das Datenbankschema für das reverse Engineering werden sollen.

## <a name="connection-string"></a>Verbindungszeichenfolge

Das erste Argument für den Befehl wird eine Verbindungszeichenfolge für die Datenbank. Die Tools werden diese Verbindungszeichenfolge verwenden, um das Datenbankschema zu lesen.

Wie Sie zitieren und Escapezeichen die Verbindungszeichenfolge hängt ab, in der Shell Sie zum Ausführen des Befehls verwenden. Finden Sie Einzelheiten in Ihrer Shell. Z. B. PowerShell müssen Sie mit Escapezeichen versehen die `$` Zeichen, aber nicht `\`.

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

``` Console
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a>Konfiguration und vertrauliche Informationen eines Benutzers

Wenn Sie eine ASP.NET Core-Projekt verfügen, können Sie die `Name=<connection-string>` Syntax, um die Verbindungszeichenfolge aus der Konfiguration gelesen.

Dies funktioniert gut mit den [Secret Manager-Tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) um Ihr Datenbankkennwort von Ihrer Codebasis zu trennen.

``` Console
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a>Name des Anbieters

Das zweite Argument ist der Name des Anbieters. Der Name des Anbieters entspricht in der Regel der Name des Anbieters NuGet-Paket.

## <a name="specifying-tables"></a>Festlegen von Tabellen

Alle Tabellen im Datenbankschema sind reverse Engineering in Entitätstypen in der Standardeinstellung. Sie können einschränken, welche Tabellen reverse sind so konzipiert, indem Sie Schemas und Tabellen angeben.

Die `-Schemas` Parameter in PMC und `--schema` Option in der CLI kann verwendet werden, um jede Tabelle in einem Schema enthalten.

`-Tables` (PMC) und `--table` (CLI) kann verwendet werden, können Sie bestimmte Tabellen einbeziehen.

Um mehrere Tabellen in der PMC einzuschließen, verwenden Sie ein Array.

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

Um mehrere Tabellen in der CLI einzuschließen, geben Sie die Option mehrfach.

``` Console
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a>Beibehalten von Namen

Tabellen- und Spaltennamen werden repariert, die .NET-Benennungskonventionen für Typen und Eigenschaften besser zu entsprechen standardmäßig. Angeben der `-UseDatabaseNames` in PMC wechseln oder die `--use-database-names` Option in der CLI wird dieses Verhalten, das die ursprünglichen Datenbanknamen so weit wie möglich beibehalten deaktiviert. Ungültige .NET Bezeichner werden immer noch behoben werden, und synthetische Namen wie z. B. Navigationseigenschaften werden weiterhin Namenskonventionen von .NET entsprechen.

## <a name="fluent-api-or-data-annotations"></a>Fluent-API oder Datenanmerkungen

Entitätstypen mithilfe der Fluent-API wird standardmäßig konfiguriert. Geben Sie `-DataAnnotations` (PMC) oder `--data-annotations` (CLI) zu verwenden. von datenanmerkungen, sofern möglich.

Beispielsweise wird mit der Fluent-API diese Gerüst:

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

Dies wird bei der Verwendung von Datenanmerkungen Gerüst:

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a>"DbContext"-name

Die erstellte "DbContext"-Klasse wird als Name der Name der Datenbank mit dem Suffix *Kontext* standardmäßig. Um einen anderen Wert anzugeben, verwenden `-Context` in PMC und `--context` in der CLI.

## <a name="directories-and-namespaces"></a>Verzeichnisse und namespaces

Die Entitätsklassen und eine DbContext-Klasse sind, die in das Stammverzeichnis des Projekts erstellt haben, und Verwenden von Standardnamespace des Projekts aus. Sie können das Verzeichnis angeben, das Gerüst für Klassen erstellt werden, mithilfe von `-OutputDir` (PMC) oder `--output-dir` (CLI). Der Namespace werden den Stamm-Namespace sowie die Namen der keine Unterverzeichnisse unter dem Stammverzeichnis des Projekts.

Sie können auch `-ContextDir` (PMC) und `--context-dir` (CLI) zum Erstellen des Gerüsts für der DbContext-Klasse in einem separaten Verzeichnis über die entitätstypklassen.

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

``` Console
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a>So funktioniert es

Reverse Engineering beginnt mit der das Schema der Datenbank lesen. Sie liest die Informationen zu Tabellen, Spalten, Einschränkungen und Indizes.

Als Nächstes verwendet er die Schemainformationen zum Erstellen eines EF Core-Modells. Tabellen werden verwendet, um Entitätstypen zu erstellen. Spalten werden verwendet, um Eigenschaften zu erstellen. und Fremdschlüssel werden verwendet, um Beziehungen zu erstellen.

Schließlich wird das Modell verwendet, um Code zu generieren. Die entsprechenden Entität Klassen, Fluent-API und Daten typanmerkungen sind erstellt haben, um das gleiche Modell aus Ihrer app neu zu erstellen.

## <a name="what-doesnt-work"></a>Was nicht funktioniert

Nicht alles, was zu einem Modell kann mit einem Datenbankschema dargestellt werden. Z. B. Informationen zu **Vererbungshierarchien**, **eigene Typen**, und **Tabelle aufteilen** nicht in das Schema der Datenbank vorhanden sind. Aus diesem Grund wird diese Konstrukte nie zurückentwickelt werden.

Darüber hinaus **Einige Spaltentypen** möglicherweise nicht vom Entity Framework Core-Anbieter unterstützt werden. Diese Spalten nicht im Modell berücksichtigt.

EF Core erfordert jeder Entitätstyp, der einen Schlüssel verfügen. Tabellen werden nicht allerdings erforderlich, um einen primären Schlüssel anzugeben. **Tabellen ohne Primärschlüssel** sind derzeit nicht mit Reverse Engineering.

Sie können definieren, **parallelitätstoken** in ein EF Core-Modell, um zu verhindern, dass zwei Benutzer die gleiche Entität zur selben Zeit aktualisiert. Einige Datenbanken haben eine besondere Art dieser Art von Spalte (z. B. "Rowversion" in SQL Server) darstellen, in diesem Fall kehren wir können diese Informationen zu entwickeln. Allerdings wird andere parallelitätstoken nicht zurückentwickelt werden.

## <a name="customizing-the-model"></a>Anpassen des Modells

Der von EF Core generierte Code ist der Code. Sie ändern können. Es wird nur neu generiert werden, wenn Sie Engineering desselben Modells erneut Reverse. Der eingerüstete Code stellt *eine* Modell, das verwendet werden kann, auf die Datenbank, aber es ist sicherlich nicht der *nur* Modell, das verwendet werden kann.

Passen Sie die entitätstypklassen und DbContext-Klasse, die Ihren Bedürfnissen an. Sie könnten z. B. das Umbenennen von Typen und Eigenschaften, Vererbungshierarchien einführen oder Teilen einer Tabelle in mehreren Entitäten. Sie können auch nicht eindeutige Indizes, nicht verwendete Sequenzen und Navigationseigenschaften, optionale skalare Eigenschaften und Einschränkungsnamen, die aus dem Modell entfernen.

Sie können auch zusätzliche Konstruktoren, Methoden, Eigenschaften usw., hinzufügen. verwenden eine andere partielle Klasse in einer separaten Datei an. Dieser Ansatz funktioniert auch, wenn Sie beabsichtigen, reverse-Engineering-das Modell erneut aus.

## <a name="updating-the-model"></a>Aktualisieren des Modells

Nach dem vornehmen von Änderungen in der Datenbank, müssen Sie das EF Core-Modell entsprechend aktualisieren. Wenn die Änderungen in der Datenbank auf einfach festgelegt sind, kann es am einfachsten, nur um die Änderungen an Ihrem EF Core-Modell manuell zu vornehmen sein. Beispielsweise werden durch das Umbenennen einer Tabelle oder Spalte, eine Spalte zu entfernen oder Aktualisieren einer Spalte des Typs banale Änderungen im Code vornehmen.

Bei wichtigeren Änderungen sind jedoch nicht einfach stellen manuell. Ein gängiger Workflow ist Reverse Engineering für das Modell aus der Datenbank erneut mit `-Force` (PMC) oder `--force` (CLI), um das vorhandene Modell durch eine aktualisierte zu überschreiben.

Ein weiteres häufig angeforderte Feature ist die Möglichkeit, das Modell aus der Datenbank zu aktualisieren, Beibehaltung von Anpassungen wie umbenennungen, Typhierarchien usw. an. Verwenden von Problem [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) zum Nachverfolgen des Status der dieses Feature.

> [!WARNING]
> Wenn reverse das Modell aus der Datenbank erneut Engineering verloren zu den Dateien vorgenommenen Änderungen.
