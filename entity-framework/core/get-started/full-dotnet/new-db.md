---
title: Erste Schritte in .NET Framework – Neue Datenbank – EF Core
author: rowanmiller
ms.author: divega
ms.date: 08/06/2018
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: 088ac915041489242eb8090e7bf3a2bdc8036534
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614427"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a>Erste Schritte mit EF Core in .NET Framework mit einer neuen Datenbank

In diesem Tutorial erstellen Sie eine Konsolenanwendung, die einen grundlegenden Datenzugriff auf eine Microsoft SQL Server-Datenbank mithilfe von Entity Framework durchführt. Sie verwenden Migrationen, um die Datenbank aus einem Modell zu erstellen.

[Sehen Sie sich das Beispiel aus diesem Artikel auf GitHub an](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).

## <a name="prerequisites"></a>Erforderliche Komponenten

* [Visual Studio 2017 Version 15.7 (oder höher)](https://www.visualstudio.com/downloads/)

## <a name="create-a-new-project"></a>Erstellt ein neues Projekt

* Öffnen Sie Visual Studio 2017.

* Klicken Sie auf **Datei > Neu > Projekt...**.

* Klicken Sie im Menü links auf **Installiert > Visual C# > Windows Desktop**.

* Wählen Sie die Projektvorlage **Konsolen-App (.NET Framework)** aus.

* Stellen Sie sicher, dass das Projekt **.NET Framework 4.6.1** (oder höher) anzielt.

* Benennen Sie das Projekt *ConsoleApp.NewDb*, und klicken Sie auf **OK**.

## <a name="install-entity-framework"></a>Installieren von Entity Framework

Installieren Sie zur Verwendung von EF Core das Paket für den (oder die) gewünschten Datenbankanbieter. In diesem Tutorial wird SQL Server verwendet. Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).

* Wählen Sie „Tools“ > „NuGet-Paket-Manager“ > „Paket-Manager-Konsole“ aus.

* Ausführen von `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Später in diesem Tutorial verwenden Sie Entity Framework Tools zum Verwalten der Datenbank. Installieren Sie deshalb auch das Toolpaket.

* Ausführen von `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="create-the-model"></a>Erstellen des Modells

Jetzt ist es Zeit, einen Kontext und Entitätsklassen für das Modell zu definieren.

* Klicken Sie auf **Projekt > Klasse hinzufügen**.

* Geben Sie als Name *Model.cs* ein, und klicken Sie auf **OK**.

* Ersetzen Sie den Inhalt der Datei durch den folgenden Code.

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] 

> [!TIP]  
> In einer echten Anwendung würden Sie jede Anwendung in einer separaten Datei platzieren und die Verbindungszeichenfolge in eine Konfigurationsdatei oder Umgebungsvariable einfügen. Der Einfachheit halber sind in diesem Tutorial alle Elemente in einer einzigen Codedatei zusammengefasst.

## <a name="create-the-database"></a>Erstellen der Datenbank

Nachdem Sie jetzt über ein Modell verfügen, können Sie mithilfe von Migrationen eine Datenbank erstellen.

* Wählen Sie **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.

* Führen Sie `Add-Migration InitialCreate` aus, um per Gerüstbau eine Migration einzurichten und den anfänglichen Tabellensatz für das Modell zu erstellen.

* Führen Sie `Update-Database` aus, um die neue Migration auf die Datenbank anzuwenden. Da die Datenbank noch nicht vorhanden ist, wird sie vor dem Anwenden der Migration erstellt.

> [!TIP]  
> Wenn Sie Änderungen am Modell durchführen, können Sie mit dem Befehl `Add-Migration` per Gerüstbau eine neue Migration einrichten, um die entsprechenden Schemaänderungen an der Datenbank vorzunehmen. Nachdem Sie den Gerüstcode überprüft (und alle erforderlichen Änderungen vorgenommen haben) haben, können Sie mit dem Befehl `Update-Database` Änderungen auf die Datenbank anwenden.
>
> EF verwendet eine `__EFMigrationsHistory`-Tabelle in der Datenbank, um nachzuverfolgen, welche Migrationen bereits auf die Datenbank angewendet wurden.

## <a name="use-the-model"></a>Verwenden des Modells

Jetzt können Sie das Modell zum Zugreifen auf Daten verwenden.

* Öffnen Sie *Program.cs*.

* Ersetzen Sie den Inhalt der Datei durch den folgenden Code.

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)]

* Klicken Sie auf **Debuggen > Ohne Debuggen starten**.

  Sie werden sehen, dass ein Blog in der Datenbank gespeichert wird und dann die Details zu allen Blogs an die Konsole ausgegeben werden.

  ![Bild](_static/output-new-db.png)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [EF Core in .NET Framework mit einer vorhandenen Datenbank](xref:core/get-started/full-dotnet/existing-db)
* [EF Core in .NET Core mit einer neuen Datenbank - SQLite](xref:core/get-started/netcore/new-db-sqlite) – Ein EF-Tutorial für beliebige Plattformkonsolen
