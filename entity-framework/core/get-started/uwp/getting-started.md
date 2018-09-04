---
title: Erste Schritte in UWP – Neue Datenbank – EF Core
author: rowanmiller
ms.date: 08/08/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: c243ef2a1940af9bf4f4b32f17acfcce7f972862
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996909"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a>Erste Schritte mit EF Core in UWP (Universelle Windows-Plattform) mit einer neuen Datenbank

In diesem Tutorial erstellen Sie eine UWP-Anwendung, die grundlegenden Datenzugriff auf eine lokale SQLite-Datenbank mithilfe von Entity Framework Core ausführt.

[Sehen Sie sich das Beispiel aus diesem Artikel auf GitHub an](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).

## <a name="prerequisites"></a>Erforderliche Komponenten

* [Windows 10 Fall Creators Update (10.0, Build 16299) oder höher](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).

* [Visual Studio 2017](https://www.visualstudio.com/downloads/), Version 15.7 oder höher, mit der Workload für die **UWP-Entwicklung**.

* [.NET Core 2.1 SDK oder höher](https://www.microsoft.com/net/core).

## <a name="create-a-model-project"></a>Erstellen eines Modellprojekts

> [!IMPORTANT]
> Aufgrund von Einschränkungen in der Interaktion zwischen .NET Core-Tools mit UWP-Projekten muss das Modell in einem Nicht-UWP-Projekt platziert werden, um Migrationsbefehle in der **Paket-Manager-Konsole** (PMC) ausführen zu können.

* Öffnen Sie Visual Studio.

* Klicken Sie auf **Datei > Neu > Projekt**.

* Wählen Sie im Menü auf der linken Seite **Installiert > Visual C# > .NET Standard** aus.

* Wählen Sie die Vorlage **Klassenbibliothek (.NET Standard)** aus.

* Nennen Sie das Projekt *Blogging.Model*.

* Nennen Sie die Projektmappe *Blogging*.

* Klicken Sie auf **OK**.

## <a name="install-entity-framework-core"></a>Installieren von Entity Framework Core

Installieren Sie zur Verwendung von EF Core das Paket für den (oder die) gewünschten Datenbankanbieter. In diesem Tutorial wird SQLite verwendet. Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).

* **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole**.

* Ausführen von `Install-Package Microsoft.EntityFrameworkCore.Sqlite`

Später in diesem Tutorial verwenden Sie Entity Framework Core-Tools zum Verwalten der Datenbank. Installieren Sie deshalb auch das Toolpaket.

* Ausführen von `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="create-the-model"></a>Erstellen des Modells

Jetzt ist es Zeit, einen Kontext und Entitätsklassen für das Modell zu definieren.

* Löschen Sie *Class1.cs*.

* Erstellen Sie *Model.cs* mit dem folgenden Code:

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a>Erstellen eines neuen UWP-Projekts

* Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Projektmappe, und wählen Sie dann **Hinzufügen > Neues Projekt** aus.

* Wählen Sie im Menü links **Installiert > Visual C# > Universelles Windows** aus.

* Wählen Sie die Projektvorlage **Leere App (universelles Windows)** aus.

* Nennen Sie das Projekt *Blogging.UWP*, und klicken Sie auf **OK**

* Legen Sie die Ziel- und Mindestversionen auf mindestens **Windows 10 Fall Creators Update (10.0, Build 16299.0)** fest.

## <a name="create-the-initial-migration"></a>Erstellen der ursprünglichen Migration

Da Sie nun über ein Modell verfügen, richten Sie die App so ein, dass sie beim ersten Start zunächst eine Datenbank erstellt. In diesem Abschnitt erstellen Sie die ursprüngliche Migration. Im folgenden Abschnitt fügen Sie Code hinzu, der diese Migration anwendet, wenn die App gestartet wird.

Migrationstools erfordern ein Nicht-UWP-Startprojekt. Erstellen Sie dieses also zuerst.

* Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Projektmappe, und wählen Sie dann **Hinzufügen > Neues Projekt** aus.

* Klicken Sie im Menü links auf **Installiert > Visual C# > .NET Core**.

* Wählen Sie die Projektvorlage **Konsolen-App (.NET Core)** aus.

* Nennen Sie das Projekt *Blogging.Migrations.Startup*, und klicken Sie auf **OK**.

* Fügen Sie dem *Blogging.Model*-Projekt einen Projektverweis aus dem *Blogging.Migrations.Startup*-Projekt hinzu.

Jetzt können Sie Ihre ursprüngliche Migration erstellen.

* Wählen Sie **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.

* Wählen Sie das *Blogging.Model*-Projekt als **Standardprojekt** aus.

* Legen Sie im **Projektmappen-Explorer** das Projekt *Blogging.Migrations.Startup* als Startprojekt fest.

* Führen Sie `Add-Migration InitialCreate` aus.

  Dieser Befehl richtet über Gerüstbau eine Migration zum Erstellen des anfänglichen Tabellensatzes für Ihr Modell ein.

## <a name="create-the-database-on-app-startup"></a>Erstellen der Datenbank beim Start der App

Da die Datenbank auf dem Gerät erstellt werden soll, auf dem die App ausgeführt wird, fügen Sie Code hinzu, mit dem beim Anwendungsstart ausstehende Migrationen auf die lokale Datenbank angewendet werden. Bei der ersten App-Ausführung wird hierbei die lokale Datenbank erstellt.

* Fügen Sie dem *Blogging.Model*-Projekt einen Projektverweis aus dem *Blogging.UWP*-Projekt hinzu.

* Öffnen Sie *App.Xaml.cs*.

* Fügen Sie den hervorgehobenen Code hinzu, um ausstehende Migrationen anzuwenden.

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> Wenn Sie Änderungen an Ihrem Modell vornehmen, können Sie mit dem Befehl `Add-Migration` per Gerüstbau eine neue Migration einrichten, um die entsprechenden Änderungen auf die Datenbank anzuwenden. Alle ausstehenden Migrationen werden beim Start der Anwendung auf die lokale Datenbank auf jedem Gerät angewendet.
>
>EF verwendet eine `__EFMigrationsHistory`-Tabelle in der Datenbank, um nachzuverfolgen, welche Migrationen bereits auf die Datenbank angewendet wurden.

## <a name="use-the-model"></a>Verwenden des Modells

Jetzt können Sie das Modell zum Zugreifen auf Daten verwenden.

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
