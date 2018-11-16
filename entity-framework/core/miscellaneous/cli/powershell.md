---
title: EF Core-Tools-Verweis (Paket-Manager-Konsole) – EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: 468698d1bbd17d4ad10b1b1601bfbc315a01c1ff
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688705"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a>Toolreferenz Entity Framework Core - Paket-Manager-Konsole in Visual Studio

Die Paket-Manager-Konsole (PMC)-Tools für Entity Framework Core führen Sie während der Entwurfszeit-Entwicklungsaufgaben. Angenommen, sie erstellen [Migrationen](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), Anwenden von Migrationen und Code für ein Modell basierend auf einer vorhandenen Datenbank generieren. Ausführen der Befehle in Visual Studio mit der [-Paket-Manager-Konsole](/nuget/tools/package-manager-console). Diese Tools funktionieren sowohl mit .NET Framework- als auch mit .NET Core-Projekten.

Wenn Sie Visual Studio nicht verwenden, empfehlen wir die [EF Core-Befehlszeilentools](dotnet.md) stattdessen. Die CLI-Tools sind – plattformübergreifend und in einer Eingabeaufforderung ausführen.

## <a name="installing-the-tools"></a>Installieren der Tools

Die Verfahren zum Installieren und aktualisieren die Tools unterscheiden sich zwischen ASP.NET Core 2.1 + und früheren Versionen oder anderen Projekttypen.

### <a name="aspnet-core-version-21-and-later"></a>ASP.NET Core Version 2.1 und höher

Die Tools sind automatisch in einem Projekt auf ASP.NET Core 2.1 enthalten, da die `Microsoft.EntityFrameworkCore.Tools` Paket befindet sich auf die [Microsoft.AspNetCore.App metapaket](/aspnet/core/fundamentals/metapackage-app).

Aus diesem Grund, Sie müssen nichts tun, um die Tools installieren, jedoch müssen Sie:
* Stellen Sie Pakete wieder her, bevor Sie mit den Tools in einem neuen Projekt.
* Installieren Sie ein Paket, um die Tools auf eine neuere Version zu aktualisieren.

Um sicherzustellen, dass Sie die neueste Version der Tools erhalten, empfehlen wir, dass Sie auch die folgende Schritte durchführen:

* Bearbeiten Ihrer *csproj* -Datei und fügen Sie eine Zeile, in die neueste Version von der [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) Paket. Z. B. die *csproj* Datei sind zum Beispiel eine `ItemGroup` , sieht folgendermaßen aus:

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

Aktualisieren Sie die Tools aus, wenn eine Meldung wie im folgenden Beispiel angezeigt:

> Die EF Core-Tools-Version "2.1.1-rtm-30846" ist älter als der die Laufzeit "2.1.3-rtm-32065". Aktualisieren Sie die Tools für die neuesten Features und Fehlerbehebungen.

So aktualisieren Sie die Tools:
* Installieren Sie das neueste .NET Core SDK an.
* Visual Studio auf die neueste Version zu aktualisieren.
* Bearbeiten der *csproj* Datei, sodass sie einen Paketverweis auf die neuesten Tools-Paket enthält, wie oben beschrieben.

### <a name="other-versions-and-project-types"></a>Andere Versionen und Projekttypen

Installieren Sie die Tools-Paket-Manager-Konsole mithilfe des folgenden Befehls **-Paket-Manager-Konsole**:

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

Aktualisieren Sie die Tools, mithilfe des folgenden Befehls **-Paket-Manager-Konsole**.

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a>Überprüfen Sie die installation

Stellen Sie sicher, dass die Tools installiert sind, mit dem folgenden Befehl:

``` powershell
Get-Help about_EntityFrameworkCore
```

Die Ausgabe sieht folgendermaßen aus (nicht teilt es Ihnen die Version der Tools, die Sie verwenden):

```console

                     _/\__
               ---==/    \\
         ___  ___   |.    \|\
        | __|| __|  |  )   \\\
        | _| | _|   \_/ |  //|\\
        |___||_|       /   \\\/\\

TOPIC
    about_EntityFrameworkCore

SHORT DESCRIPTION
    Provides information about the Entity Framework Core Package Manager Console Tools.

<A list of available commands follows, omitted here.>
```

## <a name="using-the-tools"></a>Mithilfe der Tools

Bevor Sie mit den Tools:
* Erfahren Sie, den Unterschied zwischen Ziel- und Startup-Projekt.
* Erfahren Sie, wie Sie die Tools mit .NET Standard-Klassenbibliotheken verwenden.
* Legen Sie die Umgebung für ASP.NET Core-Projekte.

### <a name="target-and-startup-project"></a>Ziel und Startup-Projekt

Die Befehle finden Sie in einem *Projekt* und ein *Startprojekt*.

* Die *Projekt* ist auch bekannt als die *Zielprojekt* da es sich handelt, in dem Sie die Befehle zum Hinzufügen oder Entfernen von Dateien. In der Standardeinstellung die **Standardprojekt** im **-Paket-Manager-Konsole** ist das Zielprojekt. Sie können ein anderes Projekt als Zielprojekt angeben, mit der <nobr> `--project` </nobr> Option.

* Die *Startprojekt* ist diejenige, die die Tools erstellen und ausführen. Die Tools werden zum Ausführen der Anwendungscode zur Entwurfszeit zum Abrufen von Informationen über das Projekt, z. B. die Datenbank-Verbindungszeichenfolge und die Konfiguration des Modells. In der Standardeinstellung die **Startprojekt** in **Projektmappen-Explorer** das Startprojekt ist. Sie können ein anderes Projekt als Startprojekt angeben, mit der <nobr> `--startup-project` </nobr> Option.

Das Startprojekt und ein Zielprojekt sind häufig die gleichen Projekt. Ein typisches Szenario, in dem sie separate Projekte sind, ist:

* Die EF Core-Kontext und Entitätsklassen-Klassen sind in einer .NET Core-Klassenbibliothek.
* Ein .NET Core-Konsolen-app oder Web-app verweist auf die Klassenbibliothek.

Es ist auch möglich, [platzieren migrationscode in eine Klassenbibliothek, die getrennt von den Entity Framework Core-Kontext](xref:core/managing-schemas/migrations/projects).

### <a name="other-target-frameworks"></a>Andere Zielframeworks

Die Paket-Manager-Konsole-Tools funktionieren mit .NET Core oder .NET Framework-Projekte. Apps, die EF Core-Modell in einer .NET Standard-Klassenbibliothek verfügen, möglicherweise keiner .NET Core- oder .NET Framework-Projekt. Dies ist z. B. "true" mit Xamarin und die universelle Windows-Plattform-apps. In solchen Fällen können Sie ein .NET Core oder .NET Framework-Konsolenanwendungsprojekt erstellen, dessen einziger Zweck ist es, als Startprojekt für die Tools zu fungieren. Das Projekt kann ein dummyprojekt ohne realen Code sein &mdash; ist nur erforderlich, ein Ziel für die Tools bereitzustellen.

Warum ist ein dummyprojekt erforderlich? Wie bereits erwähnt, haben die Tools zum Ausführen der Anwendungscode zur Entwurfszeit an. Zu diesem Zweck müssen sie die .NET Core oder .NET Framework-Laufzeit verwenden. Wenn die EF Core-Modell in einem Projekt, die auf .NET Core oder .NET Framework abzielt ist, nutzen die EF Core-Tools die Laufzeit aus dem Projekt. Sie können nicht dies tun, wenn das EF Core-Modell in einer .NET Standard-Klassenbibliothek ist. .NET Standard ist keine tatsächliche Implementierung von .NET. Es ist eine Spezifikation eines Satzes von APIs, die .NET-Implementierungen unterstützt wird müssen. Aus diesem Grund ist .NET Standard nicht ausreichend für die EF Core-Tools, um Anwendungscode auszuführen. Dem dummyprojekt, die Sie erstellen, für die Verwendung als Startup-Projekt enthält eine konkrete Zielplattform, die in der die Tools die .NET Standard-Klassenbibliothek laden können.

### <a name="aspnet-core-environment"></a>ASP.NET Core-Umgebung

Legen Sie zum Angeben der Umgebung für ASP.NET Core-Projekten **Env:ASPNETCORE_ENVIRONMENT** vor dem Ausführen der Befehle.

## <a name="common-parameters"></a>Allgemeine Parameter

Die folgende Tabelle zeigt die Parameter, die für alle EF Core-Befehle gelten:

| Parameter                 | Beschreibung                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -Kontext \<Zeichenfolge >        | Die `DbContext` zu verwendende Klasse an. Name der Klasse nur oder mit Namespaces vollqualifiziert.  Wenn dieser Parameter ausgelassen wird, sucht Entity Framework Core die Context-Klasse. Dieser Parameter ist erforderlich, wenn es mehrere Kontextklassen sind. |
| -Projekt \<Zeichenfolge >        | Das Zielprojekt. Wenn dieser Parameter ausgelassen wird, die **Standardprojekt** für **-Paket-Manager-Konsole** als das Zielprojekt verwendet wird.                                                                             |
| -StartupProject \<Zeichenfolge > | Das Startprojekt. Wenn dieser Parameter ausgelassen wird, die **Startprojekt** in **Projektmappeneigenschaften** als das Zielprojekt verwendet wird.                                                                                 |
| -Verbose                  | Zeigen Sie ausführlichen Ausgabe.                                                                                                                                                                                                 |

Um Hilfeinformationen zu einem Befehl zu anzuzeigen, verwenden Sie PowerShell `Get-Help` Befehl.

> [!TIP]
> Der Kontext, Projekt- und StartupProject parameterunterstützung Tab-Taste.

## <a name="add-migration"></a>Add-Migration

Fügt eine neue Migration hinzu.

Parameter:

| Parameter                         | Beschreibung                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <nobr>--Name \<Zeichenfolge ><nobr>       | Der Name der Migration. Dies ist ein Positionsparameter und ist erforderlich.                                              |
| <nobr>-OutputDir \<Zeichenfolge ></nobr> | Das Verzeichnis (und Sub-Namespace) verwenden. Pfade sind relativ zum Zielprojektverzeichnis. Der Standardwert ist "Migrations". |

## <a name="drop-database"></a>Drop-Database

Löscht die Datenbank.

Parameter:

| Parameter | Beschreibung                                              |
|:----------|:---------------------------------------------------------|
| -WhatIf   | Anzeigen der Datenbank gelöscht werden würde, dies jedoch noch nicht. |

## <a name="get-dbcontext"></a>"Get-DbContext"

Listet verfügbare `DbContext` Typen.

## <a name="remove-migration"></a>Remove-Migration

Entfernt die letzte Migration (Rollback die codeänderungen, die für die Migration durchgeführt wurden).

Parameter:

| Parameter | Beschreibung                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| -Force    | Zurücksetzen die Migration (ein Rollback die Änderungen, die auf die Datenbank angewendet wurden). |

## <a name="scaffold-dbcontext"></a>"Scaffold-DbContext"

Generiert Code für eine `DbContext` und Entitätstypen für eine Datenbank. In der Reihenfolge für `Scaffold-DbContext` zum Generieren eines Entitätstyps muss in der Datenbanktabelle über einen Primärschlüssel verfügen.

Parameter:

| Parameter                          | Beschreibung                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>-Connection \<Zeichenfolge ></nobr> | Die Verbindungszeichenfolge in der Datenbank. Der Wert kann für ASP.NET Core 2.x-Projekte sein *Name =\<Name der Verbindungszeichenfolge >*. In diesem Fall stammt der Name der Konfigurationsquellen, die für das Projekt eingerichtet sind. Dies ist ein Positionsparameter und ist erforderlich. |
| <nobr>-Anbieter \<Zeichenfolge ></nobr>   | Die zu verwendende Anbieter. Normalerweise hat der Name des NuGet-Pakets, z. B.: `Microsoft.EntityFrameworkCore.SqlServer`. Dies ist ein Positionsparameter und ist erforderlich.                                                                                           |
| -OutputDir \<Zeichenfolge >               | Zum Einfügen von Dateien im Verzeichnis. Pfade sind relativ zum Projektverzeichnis.                                                                                                                                                                                             |
| -ContextDir \<Zeichenfolge >              | Verzeichnis zum Ablegen der `DbContext` Datei. Pfade sind relativ zum Projektverzeichnis.                                                                                                                                                                              |
| -Kontext \<Zeichenfolge >                 | Der Name des der `DbContext` Klasse generiert.                                                                                                                                                                                                                          |
| -Schemas \<String [] >               | Die Schemas der Tabellen zur Generierung von Entitätstypen für. Wenn dieser Parameter ausgelassen wird, sind alle Schemas enthalten.                                                                                                                                                             |
| -Tabellen \<String [] >                | Die Tabellen zur Generierung von Entitätstypen für werden soll. Wenn dieser Parameter ausgelassen wird, sind alle Tabellen enthalten.                                                                                                                                                                         |
| -"DataAnnotations"                   | Verwenden Sie Attribute, um das Modell (sofern möglich) zu konfigurieren. Wenn dieser Parameter ausgelassen wird, wird nur die fluent-API verwendet.                                                                                                                                                      |
| -UseDatabaseNames                  | Verwenden Sie Tabellen- und Spaltennamen, genau wie in der Datenbank. Wenn dieser Parameter ausgelassen wird, werden die Datenbanknamen geändert, um genauer zu Konventionen für C#-Namen entsprechen.                                                                                       |
| -Force                             | Überschreiben Sie vorhandene Dateien.                                                                                                                                                                                                                                               |

Beispiel:

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

Ein Beispiel, die nur ausgewählte Tabellen erstellt das Gerüst für den Kontext in einem separaten Ordner mit dem angegebenen Namen erstellt:

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a>Script-Migration

Generiert ein SQL­Skript, das alle Änderungen von einem ausgewählten Migration auf die Migration zu einem anderen ausgewählten angewendet wird.

Parameter:

| Parameter                | Beschreibung                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *– Von* \<Zeichenfolge >        | Die Migration der ab. Migrationen können nach Name oder ID identifiziert werden Die Zahl 0 ist ein Sonderfall, das bedeutet, dass *vor der ersten Migration*. Der Standardwert ist 0.                                                              |
| *– Bis* \<Zeichenfolge >          | Der Endpunkt-Migration. Ist standardmäßig auf die letzte Migration ein.                                                                                                                                                                      |
| <nobr>-Idempotent</nobr> | Generieren Sie ein Skript, das für eine Datenbank auf eine Migration verwendet werden kann.                                                                                                                                                         |
| -Ausgabe \<Zeichenfolge >        | Die Datei, schreibt das Ergebnis. Wenn dieser Parameter ausgelassen wird, wird die Datei mit einem generierten Namen im gleichen Ordner erstellt, wie die app Runtime-Dateien, z. B. erstellt werden: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*. |

> [!TIP]
> To, From, und Output-Parameter unterstützen die Tab-Taste.

Das folgende Beispiel erstellt ein Skript für die InitialCreate-Migration, die mit dem Migrationsnamen.

```powershell
Script-Migration -To InitialCreate
```

Das folgende Beispiel erstellt ein Skript für alle Migrationen nach der Migration InitialCreate mithilfe der Migration-ID.

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a>Update-Database

Aktualisiert die Datenbank aus, um die letzte Migration oder für eine angegebene Migration.

| Parameter                           | Beschreibung                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>*-Migration* \<Zeichenfolge ></nobr> | Die Ziel-Migration. Migrationen können nach Name oder ID identifiziert werden Die Zahl 0 ist ein Sonderfall, das bedeutet, dass *vor der ersten Migration* und bewirkt, dass alle Migrationen zu rückgängig gemacht werden. Wenn keine Migration angegeben ist, verwendet der Befehl auf die letzte Migration. |

> [!TIP]
> Der Parameter für die Migration unterstützt die Tab-Taste.

Im folgende Beispiel wird bei allen Migrationen zurückgesetzt.

```powershell
Update-Database -Migration 0
```

In den folgenden Beispielen wird die Datenbank für eine angegebene Migration aktualisieren. Das erste verwendet den Migrationsnamen aus, und die zweite verwendet den Migrations-ID:

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Migrationen](xref:core/managing-schemas/migrations/index)
* [Reverse-Engineering](xref:core/managing-schemas/scaffolding)
