---
title: "Erste Schritte in .NET Core – Neue Datenbank – EF Core"
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: Erste Schritte mit .NET Core unter Verwendung von Entity Framework Core
keywords: .NET Core, Entity Framework Core, VS-Code, Visual Studio-Code, Mac, Linux
ms.date: 04/05/2017
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 22fc0446dee71dd0d2402b47d76cc8b7307fbe5f
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a>Erste Schritte mit EF Core in einer .NET Core-Konsolen-App mit einer neuen Datenbank

In dieser exemplarischen Vorgehensweise entwickeln Sie eine .NET Core-Konsolen-App, die einen grundlegenden Datenzugriff auf eine SQLite-Datenbank mithilfe von Entity Framework Core durchführt. Sie verwenden Migrationen, um die Datenbank aus Ihrem Modell zu erstellen. Siehe [ASP.NET Core – Neue Datenbank](xref:core/get-started/aspnetcore/new-db) für eine Visual Studio-Version mit Verwendung von ASP.NET Core MVC.

> [!NOTE]  
> `project.json` oder Visual Studio 2015 werden vom [.NET Core SDK](https://www.microsoft.com/net/download/core) nicht mehr unterstützt. Es wird empfohlen, [eine Migration von „project.json“ zu „csproj“ durchzuführen](https://docs.microsoft.com/dotnet/articles/core/migration/). Wenn Sie Visual Studio verwenden, wird eine Migration zu [Visual Studio 2017](https://www.visualstudio.com/downloads/) empfohlen.

> [!TIP]  
> Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) finden Sie auf GitHub.

## <a name="prerequisites"></a>Erforderliche Komponenten

Zum Ausführen dieser exemplarischen Vorgehensweise müssen folgende Voraussetzungen erfüllt sein:
* Ein Betriebssystem mit Unterstützung von .NET Core.
* [.NET Core SDK 2.0](https://www.microsoft.com/net/core) (wenngleich zum Verwenden einer Anwendung mit einer vorherigen Version die Anweisungen nur leicht geändert werden müssen).

## <a name="create-a-new-project"></a>Erstellen eines neuen Projekts

* Erstellen Sie einen neuen Ordner `ConsoleApp.SQLite` für Ihr Projekt, und verwenden Sie den Befehl `dotnet`, um ihn mit einer .NET Core-App aufzufüllen.

``` Console
mkdir ConsoleApp.SQLite
cd ConsoleApp.SQLite/
dotnet new console
```

## <a name="install-entity-framework-core"></a>Installieren von Entity Framework Core

Installieren Sie zur Verwendung von EF Core das Paket für den (oder die) gewünschten Datenbankanbieter. In dieser exemplarischen Vorgehensweise wird SQLite verwendet. Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).

* Installieren Sie „Microsoft.EntityFrameworkCore.Sqlite“ und „Microsoft.EntityFrameworkCore.Design“.

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* Führen Sie eine manuelle Bearbeitung von `ConsoleApp.SQLite.csproj` durch, um „DotNetCliToolReference“ zu „Microsoft.EntityFrameworkCore.Tools.DotNet“ hinzuzufügen:

  ``` xml
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  </ItemGroup>
  ```

 Hinweis: In einer zukünftigen Version von `dotnet` wird „DotNetCliToolReferences“ über `dotnet add tool` unterstützt.

`ConsoleApp.SQLite.csproj` sollte jetzt Folgendes enthalten:

[!code[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/ConsoleApp.SQLite.csproj)]

 Hinweis: Die oben verwendeten Versionsnummer waren zum Zeitpunkt der Veröffentlichung korrekt.

*  Führen Sie `dotnet restore` aus, um die neuen Pakete zu installieren.

## <a name="create-the-model"></a>Erstellen des Modells

Definieren Sie einen Kontext und Entitätsklassen für Ihr Modell.

* Erstellen Sie eine neue Datei *Model.cs* mit den folgenden Inhalten.

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

Tipp: In einer echten Anwendung würden Sie jede Anwendung in einer separaten Datei platzieren und die Verbindungszeichenfolge eine Konfigurationsdatei einfügen. Der Einfachheit halber werden in diesem Tutorial alle Elemente in einer Datei zusammengefasst.

## <a name="create-the-database"></a>Erstellen der Datenbank

Da Sie jetzt über ein Modell verfügen, können Sie mithilfe von [Migrationen](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) eine Datenbank erstellen.

* Führen Sie `dotnet ef migrations add InitialCreate` aus, um per Gerüstbau eine Migration einzurichten und den anfänglichen Tabellensatz für das Modell zu erstellen.
* Führen Sie `dotnet ef database update` aus, um die neue Migration auf die Datenbank anzuwenden. Mit diesem Befehl wird die Datenbank erstellt, bevor Migrationen angewendet werden.

> [!NOTE]  
> Wenn Sie mit SQLite relative Pfade verwenden, sind diese Pfade relativ zur Hauptassembly der Anwendung. In diesem Beispiel ist `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll` die Hauptbinärdatei, deshalb befindet sich die SQLite-Datenbank in `bin/Debug/netcoreapp2.0/blogging.db`.

## <a name="use-your-model"></a>Verwenden Ihres Modells

* Öffnen Sie die Datei *Program.cs*, und ersetzen Sie den Inhalt durch den folgenden Code:

 [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* Testen der App:

 `dotnet run`

 Ein Blog wird in der Datenbank gespeichert, und die Details zu allen Blogs werden an die Konsole ausgegeben.

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a>Ändern des Modells:

- Wenn Sie Änderungen an Ihrem Modell durchführen, können Sie mit dem Befehl `dotnet ef migrations add` per Gerüstbau eine neue [Migration](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) einrichten, um die entsprechenden Schemaänderungen an der Datenbank vorzunehmen. Nachdem Sie den Gerüstcode überprüft (und alle erforderlichen Änderungen vorgenommen) haben, können Sie mit dem Befehl `dotnet ef database update` die Änderungen auf die Datenbank anwenden.
- EF verwendet eine `__EFMigrationsHistory`-Tabelle in der Datenbank, um nachzuverfolgen, welche Migrationen bereits auf die Datenbank angewendet wurden.
- SQLite unterstützt aufgrund von Einschränkungen in SQLite nicht alle Migrationen (Schemaänderungen). Siehe [SQLite-Einschränkungen](../../providers/sqlite/limitations.md). Für eine Neuentwicklung können Sie erwägen, bei Modelländerungen die Datenbank zu verwerfen und eine neue Datenbank zu erstellen, statt Migrationen zu verwenden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [.NET Core – Neue Datenbank mit SQLite](xref:core/get-started/netcore/new-db-sqlite): Ein EF-Tutorial für beliebige Plattformkonsolen.
* [Einführung in ASP.NET Core MVC unter Mac oder Linux](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Einführung in ASP.NET Core MVC mit Visual Studio](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Erste Schritte mit ASP.NET Core und Entity Framework Core mithilfe von Visual Studio](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
