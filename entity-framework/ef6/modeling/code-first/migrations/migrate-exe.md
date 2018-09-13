---
title: Verwenden von migrate.exe - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 989ea862-e936-4c85-926a-8cfbef5df5b8
ms.openlocfilehash: 6e9880523bbcf2fe55390a447241e59723a0967f
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490215"
---
# <a name="using-migrateexe"></a>Verwenden von migrate.exe
Code First-Migrationen können verwendet werden, um die Aktualisierung einer Datenbank in visual Studio, aber Sie können auch über die Befehlszeile Tool migrate.exe ausgeführt werden. Auf dieser Seite erhalten einen Überblick zum migrate.exe zu verwenden, um Migrationen für eine Datenbank auszuführen.

> [!NOTE]
> In diesem Artikel wird davon ausgegangen, dass Sie wissen, wie Code First-Migrationen in grundlegenden Szenarien verwenden. Wenn Sie dies nicht tun, müssen Sie lesen [Code First-Migrationen](~/ef6/modeling/code-first/migrations/index.md) vor dem fortfahren.

## <a name="copy-migrateexe"></a>Kopieren Sie migrate.exe

Bei der Installation von Entity Framework werden mithilfe von NuGet migrate.exe im Ordner "Tools" des heruntergeladenen Pakets. In &lt;Projektordner&gt;\\Pakete\\EntityFramework.&lt; Version&gt;\\Tools

Nachdem Sie migrate.exe haben müssen Sie zum Kopieren auf den Speicherort der Assembly, die Ihre Migrationen enthält.

Wenn Ihre Anwendung auf .NET 4 ausgerichtet ist und nicht 4.5 Sie zum Kopieren müssen der **Redirect.config** an Ort und benennen Sie sie **migrate.exe.config**. Dies ist, damit migrate.exe Ruft ab, die richtigen bindungsumleitungen, um die Entity Framework-Assembly suchen zu können.

| .NET 4.5                                   | .NET 4.0                                   |
|:-------------------------------------------|:-------------------------------------------|
| ![.NET 4.5-Dateien](~/ef6/media/net45files.png)  | ![.NET 4.0-Dateien](~/ef6/media/net40files.png)  |

> [!NOTE]
> Migrate.exe X64 unterstützt keine Assemblys.

Sobald Sie migrate.exe in den richtigen Ordner verschoben haben, und Sie können Sie sie verwenden Migrationen, für die Datenbank ausgeführt werden soll. Das Hilfsprogramm dient Sie lediglich Migrationen auszuführen. Es kann nicht Migrationen zu generieren, oder erstellen Sie ein SQL­Skript.

## <a name="see-options"></a>Finden Sie unter Optionen

``` console
Migrate.exe /?
```

Oben zeigt die Hilfeseite verknüpft ist, mit diesem Hilfsprogramm, beachten Sie, die Sie benötigen, um EntityFramework.dll am gleichen Speicherort zu erhalten, die Sie migrate.exe, damit dies ausgeführt werden funktioniert.

## <a name="migrate-to-the-latest-migration"></a>Migrieren Sie zur aktuellen

``` console
Migrate.exe MyMvcApplication.dll /startupConfigurationFile=”..\\web.config”
```

Wenn ausführen migrate.exe des einzige obligatorischen Parameters wird die Assembly, die die Assembly mit den Migrationen, die Sie ausführen möchten ist, aber alle Konvention wird verwendet, die Einstellungen basieren, wenn Sie keine Konfigurationsdatei angeben.

## <a name="migrate-to-a-specific-migration"></a>Migrieren Sie zu einem GUID-migration

``` console
Migrate.exe MyApp.exe /startupConfigurationFile=”MyApp.exe.config” /targetMigration=”AddTitle”
```

Wenn Sie Migrationen bis zu einer bestimmten Migration ausführen möchten, können Sie den Namen der Migration angeben. Dies führt bei allen vorherige Migrationen nach Bedarf bis zum Abrufen der Migration angegeben.

## <a name="specify-working-directory"></a>Arbeitsverzeichnis angeben

``` console
Migrate.exe MyApp.exe /startupConfigurationFile=”MyApp.exe.config” /startupDirectory=”c:\\MyApp”
```

Wenn Sie Assembly verfügt über Abhängigkeiten oder Dateien relativ zum Arbeitsverzeichnis liest, müssen Sie StartupDirectory festgelegt.

## <a name="specify-migration-configuration-to-use"></a>Geben Sie die zu verwendende migrationskonfiguration

``` console
Migrate.exe MyAssembly CustomConfig /startupConfigurationFile=”..\\web.config”
```

Wenn Sie über mehrere Konfigurationsklassen für die Migration verfügen, müssen Klassen erben von "dbmigrationconfiguration", klicken Sie dann angeben, die für diese Ausführung verwendet werden. Dies wird durch Bereitstellen des optionalen zweiten Parameters ohne einen Switch als oben angegeben.

## <a name="provide-connection-string"></a>Geben Sie die Verbindungszeichenfolge

``` console
Migrate.exe BlogDemo.dll /connectionString=”Data Source=localhost;Initial Catalog=BlogDemo;Integrated Security=SSPI” /connectionProviderName=”System.Data.SqlClient”
```

Wenn Sie eine Verbindungszeichenfolge in der Befehlszeile angeben möchten müssen Sie auch den Anbieternamen bereitstellen. Der Name des Anbieters nicht angeben, wird eine Ausnahme ausgelöst.

## <a name="common-problems"></a>Häufige Probleme

| Fehlermeldung                                                                                                                                                                                                                                                                                                                      | Lösung                                                                                                                                                                                                                                                                                             |
|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nicht behandelte Ausnahme: System.IO.FileLoadException: konnte nicht geladen werden, Datei oder Assembly "EntityFramework", "Version = 5.0.0.0, Culture = Neutral, PublicKeyToken = b77a5c561934e089' oder eine ihrer Abhängigkeiten. Das manifest der sich Assemblydefinition entspricht nicht den der Assemblyverweis verweist. (Ausnahme von HRESULT: 0x80131040)         | Dies bedeutet normalerweise, dass Sie eine .NET 4-Anwendung ohne die Datei Redirect.config ausgeführt werden. Sie müssen die Redirect.config in am gleichen Speicherort wie migrate.exe kopieren aus, und benennen Sie sie in migrate.exe.config.                                                                                       |
| Nicht behandelte Ausnahme: System.IO.FileLoadException: konnte nicht geladen werden, Datei oder Assembly "EntityFramework, Version = 4.4.0.0, Kultur = Neutral, PublicKeyToken = b77a5c561934e089" oder eine ihrer Abhängigkeiten. Das manifest der sich Assemblydefinition entspricht nicht den der Assemblyverweis verweist. (Ausnahme von HRESULT: 0x80131040)          | Diese Ausnahme bedeutet, dass Sie .NET 4.5 ausgeführt werden, die Anwendung mit der Redirect.config an den migrate.exe-Speicherort kopiert. Wenn Ihre app auf .NET 4.5 ist, müssen Sie keine haben die Config-Datei mit die umleitungen in. Löschen Sie die migrate.exe.config-Datei.                                    |
| Fehler: Kann nicht zum Aktualisieren der Datenbank, um das aktuelle Modell übereinstimmen, da es ausstehende Änderungen gibt und die automatische Migration deaktiviert ist. Schreiben Sie die ausstehenden modelländerungen für eine Migration codebasierte oder aktivieren Sie die automatische Migration. Legen Sie DbMigrationsConfiguration.AutomaticMigrationsEnabled auf True, um die automatische Migration aktivieren. | Dieser Fehler tritt bei der Migration wird ausgeführt, wenn Sie noch nicht erstellt haben, eine Migration zum bewältigen der Änderungen am Modell vorgenommen, und die Datenbank nicht das Modell entspricht. Hinzufügen einer Eigenschaft auf eine Modellklasse, die dann migrate.exe ausführen, ohne eine Migration aus, um die Datenbank zu aktualisieren, ist ein Beispiel hierfür. |
| Fehler: Typ wird für Member nicht aufgelöst "System.Data.Entity.Migrations.Design.ToolingFacade+UpdateRunner,EntityFramework, Version = 5.0.0.0, Culture = Neutral, PublicKeyToken = b77a5c561934e089'.                                                                                                                                       | Dieser Fehler kann durch Angabe einer falschen Startverzeichnis verursacht werden. Dies muss der Speicherort der migrate.exe sein.                                                                                                                                                                                      |
| Nicht behandelte Ausnahme: System.NullReferenceException: der Objektverweis ist nicht auf eine Instanz eines Objekts festgelegt. <br/>   am System.Data.Entity.Migrations.Console.Program.Main (String [] Args)                                                                                                                                             | Dies kann verursacht werden, indem ein erforderlicher Parameter für ein Szenario, das Sie verwenden nicht angeben. Z. B. Angeben einer Verbindungszeichenfolge ohne Angabe der Name des Anbieters ein.                                                                                                                        |
| Fehler: mehrere Typen von Migrationen-Konfiguration wurde in der Assembly 'ClassLibrary1' gefunden. Geben Sie den Namen eines verwenden.                                                                                                                                                                                                  | Wie in der Fehlermeldung angegeben, ist es mehr als eine Konfigurationsklasse in der angegebenen Assembly an. Sie müssen die /configurationType-Schalter verwenden, um anzugeben, welche verwenden.                                                                                                                                           |
| Fehler: Konnte nicht geladen werden, Datei oder Assembly '&lt;AssemblyName&gt;' oder eine ihrer Abhängigkeiten. Die angegebene Assembly oder die Codebasis ist ungültig. (Ausnahme von HRESULT: 0x80131047)                                                                                                                                                    | Dies kann verursacht werden, durch Angeben von Namen einer Assembly falsch oder ohne                                                                                                                                                                                                                          |
| Fehler: Konnte nicht geladen werden, Datei oder Assembly '&lt;AssemblyName&gt;' oder eine ihrer Abhängigkeiten. Es wurde versucht, ein Programm mit einem falschen Format zu laden.                                                                                                                                                                          | Dies geschieht, wenn Sie versuchen, x X64 migrate.exe ausgeführt Anwendung. EF 5.0 und unten funktioniert nur auf X86.                                                                                                                                                                                |
