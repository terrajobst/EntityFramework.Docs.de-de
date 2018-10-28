---
title: Erste Schritte in UWP – Neue Datenbank – EF Core
author: rowanmiller
ms.date: 10/13/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: 456967a0dc981053919064a19cc9c98bf7309865
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022375"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a>Erste Schritte mit EF Core in UWP (Universelle Windows-Plattform) mit einer neuen Datenbank

In diesem Tutorial erstellen Sie eine UWP-Anwendung, die grundlegenden Datenzugriff auf eine lokale SQLite-Datenbank mithilfe von Entity Framework Core ausführt.

[Sehen Sie sich das Beispiel aus diesem Artikel auf GitHub an](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).

## <a name="prerequisites"></a>Erforderliche Komponenten

* [Windows 10 Fall Creators Update (10.0, Build 16299) oder höher](https://support.microsoft.com/help/4027667/windows-update-windows-10).

* [Visual Studio 2017](https://www.visualstudio.com/downloads/), Version 15.7 oder höher, mit der Workload für die **UWP-Entwicklung**.

* [.NET Core 2.1 SDK oder höher](https://www.microsoft.com/net/core).

> [!IMPORTANT]
> Dieses Tutorial verwendet [Migrations](xref:core/managing-schemas/migrations/index)befehle aus Entity Framework Core, um das Datenbankschema zu erstellen und zu aktualisieren.
> Diese Befehle funktionieren nicht direkt in UWP-Projekten.
> Daher befindet sich das Datenmodell der Anwendung in einem freigegebenen Bibliotheksprojekt, und eine separate .NET Core-Konsolenanwendung wird verwendet, um die Befehle auszuführen.

## <a name="create-a-library-project-to-hold-the-data-model"></a>Erstellen eines Bibliotheksprojekts für das Datenmodell

* Öffnen Sie Visual Studio.

* Klicken Sie auf **Datei > Neu > Projekt**.

* Wählen Sie im Menü auf der linken Seite **Installiert > Visual C# > .NET Standard** aus.

* Wählen Sie die Vorlage **Klassenbibliothek (.NET Standard)** aus.

* Nennen Sie das Projekt *Blogging.Model*.

* Nennen Sie die Projektmappe *Blogging*.

* Klicken Sie auf **OK**.

## <a name="install-entity-framework-core-runtime-in-the-data-model-project"></a>Installieren von Entity Framework Core-Runtime im Datenmodell-Projekt

Installieren Sie zur Verwendung von EF Core das Paket für den (oder die) gewünschten Datenbankanbieter. In diesem Tutorial wird SQLite verwendet. Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).

* **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole**.

* Stellen Sie sicher, dass das Bibliotheksprojekt *Blogging.Model* in der Paket-Manager-Konsole als **Default Project** (Standardprojekt) ausgewählt ist.

* Ausführen von `Install-Package Microsoft.EntityFrameworkCore.Sqlite`

## <a name="create-the-data-model"></a>Erstellen des Datenmodells

Jetzt ist es Zeit, den *DbContext* und Entitätsklassen für das Modell zu definieren.

* Löschen Sie *Class1.cs*.

* Erstellen Sie *Model.cs* mit dem folgenden Code:

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-console-project-to-run-migrations-commands"></a>Erstellen eines neuen Konsolenprojekts zum Ausführen von Migrationsbefehlen

* Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Projektmappe, und wählen Sie dann **Hinzufügen > Neues Projekt** aus.

* Klicken Sie im Menü links auf **Installiert > Visual C# > .NET Core**.

* Wählen Sie die Projektvorlage **Konsolen-App (.NET Core)** aus.

* Nennen Sie das Projekt *Blogging.Migrations.Startup*, und klicken Sie auf **OK**.

* Fügen Sie dem *Blogging.Model*-Projekt einen Projektverweis aus dem *Blogging.Migrations.Startup*-Projekt hinzu.

## <a name="install-entity-framework-core-tools-in-the-migrations-startup-project"></a>Installieren von Entity Framework Core-Tools im Migrations-Startprojekt

Um EF Core-Migrationsbefehle in der Paket-Manager-Konsole zu aktivieren, installieren Sie das EF Core-Toolpaket in der Konsolenanwendung.

* Wählen Sie **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.

* Ausführen von `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`

## <a name="create-the-initial-migration"></a>Erstellen der ursprünglichen Migration

 Erstellen Sie die ursprüngliche Migration, indem Sie die Konsolenanwendung als Startprojekt angeben.

* Ausführen von `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`

Dieser Befehl richtet über Gerüstbau eine Migration zum Erstellen des anfänglichen Datenbanktabellensatzes für Ihr Datenmodell ein.

## <a name="create-the-uwp-project"></a>Erstellen eines UWP-Projekts

* Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Projektmappe, und wählen Sie dann **Hinzufügen > Neues Projekt** aus.

* Wählen Sie im Menü links **Installiert > Visual C# > Universelles Windows** aus.

* Wählen Sie die Projektvorlage **Leere App (universelles Windows)** aus.

* Nennen Sie das Projekt *Blogging.UWP*, und klicken Sie auf **OK**

> [!IMPORTANT]
> Legen Sie die Ziel- und Mindestversionen auf mindestens **Windows 10 Fall Creators Update (10.0, Build 16299.0)** fest.
> .NET Standard 2.0 ist für Entity Framework Core erforderlich, wird aber von älteren Windows 10-Versionen nicht unterstützt.

## <a name="add-code-to-create-the-database-on-application-startup"></a>Hinzufügen eines Codes zum Erstellen der Datenbank beim Anwendungsstart

Da die Datenbank auf dem Gerät erstellt werden soll, auf dem die App ausgeführt wird, fügen Sie Code hinzu, mit dem beim Anwendungsstart ausstehende Migrationen auf die lokale Datenbank angewendet werden. Bei der ersten App-Ausführung wird hierbei die lokale Datenbank erstellt.

* Fügen Sie dem *Blogging.Model*-Projekt einen Projektverweis aus dem *Blogging.UWP*-Projekt hinzu.

* Öffnen Sie *App.Xaml.cs*.

* Fügen Sie den hervorgehobenen Code hinzu, um ausstehende Migrationen anzuwenden.

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> Wenn Sie Änderungen an Ihrem Modell vornehmen, können Sie mit dem Befehl `Add-Migration` per Gerüstbau eine neue Migration einrichten, um die entsprechenden Änderungen auf die Datenbank anzuwenden. Alle ausstehenden Migrationen werden beim Start der Anwendung auf die lokale Datenbank auf jedem Gerät angewendet.
>
>EF Core verwendet eine `__EFMigrationsHistory`-Tabelle in der Datenbank, um nachzuverfolgen, welche Migrationen bereits auf die Datenbank angewendet wurden.

## <a name="use-the-data-model"></a>Verwenden des Datenmodells

Jetzt können Sie EF Core zum Zugreifen auf Daten verwenden.

* Öffnen Sie *MainPage.xaml*.

* Fügen Sie den nachstehend markierten Handler zum Laden der Seite und den Benutzerschnittstelleninhalt ein.

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

Fügen Sie jetzt Code zum Verknüpfen der Benutzeroberfläche mit der Datenbank hinzu.

* Öffnen Sie *MainPage.xaml.cs*.

* Fügen Sie den hervorgehobenen Code aus der folgenden Liste hinzu:

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

Sie können die Anwendung jetzt ausführen, um sie in Aktion zu sehen.

* Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das *Blogging.UWP*-Projekt, und wählen Sie dann **Bereitstellen** aus.

* Legen Sie *Blogging.UWP* als Startprojekt fest.

* Klicken Sie auf **Debuggen > Ohne Debuggen starten**.

  Die App wird erstellt und ausgeführt.

* Geben Sie eine URL ein, und klicken Sie auf die Schaltfläche **Hinzufügen**.

  ![Bild](_static/create.png)

  ![Bild](_static/list.png)

  Voilá! Sie verfügen jetzt über eine einfache UWP-App, die Entity Framework Core ausführt.

## <a name="next-steps"></a>Nächste Schritte

Informationen zur Kompatibilität und Leistung, die Sie bei der Verwendung von EF Core mit UWP kennen sollten, finden Sie unter [.NET-Implementierungen, die von EF Core unterstützt werden](../../platforms/index.md#universal-windows-platform).

Sehen Sie sich auch die weiteren Artikel in dieser Dokumentation an, um mehr über die Features von Entity Framework Core zu erfahren.
