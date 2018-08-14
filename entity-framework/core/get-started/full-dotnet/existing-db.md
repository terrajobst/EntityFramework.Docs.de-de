---
title: Erste Schritte in .NET Framework – Vorhandene Datenbank – EF Core
author: rowanmiller
ms.author: divega
ms.date: 08/06/2018
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: d5c548927b736199c7d6fddc9c74139ca5f6614e
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614414"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a>Erste Schritte mit EF Core in .NET Framework mit einer vorhandenen Datenbank

In diesem Tutorial erstellen Sie eine Konsolenanwendung, die einen grundlegenden Datenzugriff auf eine Microsoft SQL Server-Datenbank mithilfe von Entity Framework durchführt. Sie erstellen ein Entity Framework-Modell, indem Sie ein Reverse Engineering für eine vorhandene Datenbank durchführen.

[Sehen Sie sich das Beispiel aus diesem Artikel auf GitHub an](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb).

## <a name="prerequisites"></a>Erforderliche Komponenten

* [Visual Studio 2017 Version 15.7 (oder höher)](https://www.visualstudio.com/downloads/)

## <a name="create-blogging-database"></a>Erstellen einer Blogging-Datenbank

In diesem Tutorial wird als vorhandene Datenbank eine **Blogging**-Datenbank in der LocalDb-Instanz verwendet. Wenn Sie die **Blogging**-Datenbank bereits im Rahmen eines anderen Tutorials erstellt haben, können Sie diese Schritte überspringen.

* Öffnen Sie Visual Studio.

* Klicken Sie auf **Extras > Verbindung mit Datenbank herstellen**.

* Wählen Sie **Microsoft SQL Server** aus, und klicken Sie auf **Weiter**.

* Geben Sie **(localdb)\mssqllocaldb** als **Servername** ein.

* Geben Sie als **Datenbankname** den Wert **master** ein, und klicken Sie auf **OK**.

* Die master-Datenbank wird jetzt unter **Datenverbindungen** im **Server-Explorer** angezeigt.

* Klicken Sie im **Server-Explorer** mit der rechten Maustaste auf die Datenbank, und wählen Sie **Neue Abfrage** aus.

* Kopieren Sie das nachstehende Skript in den Abfrage-Editor.

* Klicken Sie mit der rechten Maustaste auf den Abfrage-Editor, und wählen Sie **Ausführen** aus.

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Erstellt ein neues Projekt

* Öffnen Sie Visual Studio 2017.

* Klicken Sie auf **Datei > Neu > Projekt...**.

* Klicken Sie im Menü links auf **Installiert > Visual C# > Windows Desktop**.

* Wählen Sie die Projektvorlage **Konsolen-App (.NET Framework)** aus.

* Stellen Sie sicher, dass das Projekt **.NET Framework 4.6.1** (oder höher) anzielt.

* Benennen Sie das Projekt *ConsoleApp.ExistingDb*, und klicken Sie auf **OK**.

## <a name="install-entity-framework"></a>Installieren von Entity Framework

Installieren Sie zur Verwendung von EF Core das Paket für den (oder die) gewünschten Datenbankanbieter. In diesem Tutorial wird SQL Server verwendet. Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).

* Wählen Sie **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.

* Ausführen von `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Im nächsten Schritt verwenden Sie einige Entity Framework Tools, um ein Reverse Engineering für die Datenbank durchzuführen. Installieren Sie deshalb auch das Toolpaket.

* Ausführen von `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="reverse-engineer-the-model"></a>Reverse Engineering des Modells

Jetzt ist es Zeit, das EF-Modell basierend auf einer vorhandenen Datenbank zu erstellen.

* Wählen Sie **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.

* Führen Sie den folgenden Befehl aus, um ein Modell aus der vorhandenen Datenbank zu erstellen.

  ``` powershell
  Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
  ```

> [!TIP]  
> Indem Sie das `-Tables`-Argument zum obigen Befehl hinzufügen, können Sie angeben, für welche Tabellen Sie Entitäten generieren möchten. Beispielsweise `-Tables Blog,Post`.

Der Reverse Engineering-Prozess hat Entitätsklassen (`Blog` und `Post`) und einen abgeleiteten Kontext (`BloggingContext`) basierend auf dem Schema der vorhandenen Datenbank erstellt.

Die Entitätsklassen sind einfache C#-Objekte zum Repräsentieren der Daten, die Sie abfragen und speichern werden. Hier sind die Entitätsklassen `Blog` und `Post`:

 [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Post.cs)]

> [!TIP]  
> Sie können Navigationseigenschaften auf `virtual` festlegen (Blog.Post und Post.Blog), um Lazy Loading zu aktivieren.

Der Kontext stellt eine Sitzung mit der Datenbank dar. Sie verfügt über Methoden, die Sie zum Abfragen und Speichern von Instanzen der Entitätsklassen verwenden können.

[!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)]

## <a name="use-the-model"></a>Verwenden des Modells

Jetzt können Sie das Modell zum Zugreifen auf Daten verwenden.

* Öffnen Sie *Program.cs*.

* Ersetzen Sie den Inhalt der Datei durch den folgenden Code.

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] 

* Klicken Sie auf „Debuggen“ > „Ohne Debuggen starten“.

  Sie werden sehen, dass ein Blog in der Datenbank gespeichert wird und dann die Details zu allen Blogs an die Konsole ausgegeben werden.

  ![Bild](_static/output-existing-db.png)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [EF Core in .NET Framework mit einer neuen Datenbank](xref:core/get-started/full-dotnet/new-db)
* [EF Core in .NET Core mit einer neuen Datenbank - SQLite](xref:core/get-started/netcore/new-db-sqlite) – Ein EF-Tutorial für beliebige Plattformkonsolen
