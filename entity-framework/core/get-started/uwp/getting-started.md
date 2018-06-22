---
title: Erste Schritte in UWP – Neue Datenbank – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.topic: get-started-article
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
ms.technology: entity-framework-core
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: f743ff5392d1f30283a13d2e7fb8029be88387aa
ms.sourcegitcommit: 96324e58c02b97277395ed43173bf13ac80d2012
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/01/2017
ms.locfileid: "26049833"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a>Erste Schritte mit EF Core in UWP (Universelle Windows-Plattform) mit einer neuen Datenbank

> [!NOTE]
> In diesem Tutorial wird EF Core 2.0.1 verwendet (zusammen mit ASP.NET Core und .NET Core SDK 2.0.3 veröffentlicht). In EF Core 2.0.0 fehlen einige wichtige Bugfixes, die für eine optimale UWP-Nutzung nötig sind.

In dieser exemplarischen Vorgehensweise entwickeln Sie eine UWP-Anwendung, die einen grundlegenden Datenzugriff auf eine lokale SQLite-Datenbank mithilfe von Entity Framework durchführt.

> [!IMPORTANT]
> **Vermeiden Sie die Verwendung anonymer Typen in LINQ-Abfragen in UWP**. Das Bereitstellen einer UWP-Anwendung im App Store erfordert eine Kompilierung Ihrer Anwendung mit .NET Native. Abfragen mit anonymen Typen zeigen in .NET Native eine unzureichende Leistung.

> [!TIP]
> Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite) finden Sie auf GitHub.

## <a name="prerequisites"></a>Erforderliche Komponenten

Für diese exemplarische Vorgehensweise wird Folgendes benötigt:

* [Windows 10 Fall Creators Update](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0)

* [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) oder höher

* [Visual Studio 2017](https://www.visualstudio.com/downloads/), Version 15.4 oder höher, mit der Workload für die **UWP-Entwicklung**.

## <a name="create-a-new-model-project"></a>Erstellen eines neuen Modellprojekts

> [!WARNING]
> Aufgrund von Einschränkungen in der Interaktion zwischen .NET Core-Tools mit UWP-Projekten muss das Modell in einem Nicht-UWP-Projekt platziert werden, um Migrationsbefehle in der Paket-Manager-Konsole ausführen zu können.

* Öffnen Sie Visual Studio.

* Klicken Sie auf „Datei“ > „Neu“ > „Projekt“.

* Wählen Sie im Menü links „Vorlagen“ > „Visual C#“ aus.

* Wählen Sie die Vorlage **Klassenbibliothek (.NET Standard)** aus.

* Benennen Sie das Projekt, und klicken Sie auf **OK**.

## <a name="install-entity-framework"></a>Installieren von Entity Framework

Installieren Sie zur Verwendung von EF Core das Paket für den (oder die) gewünschten Datenbankanbieter. In dieser exemplarischen Vorgehensweise wird SQLite verwendet. Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).

* Wählen Sie „Tools“ > „NuGet-Paket-Manager“ > „Paket-Manager-Konsole“ aus.

* Führen Sie `Install-Package Microsoft.EntityFrameworkCore.Sqlite` aus.

Später in dieser exemplarischen Vorgehensweise werden einige Entity Framework-Tools zum Verwalten der Datenbank eingesetzt. Deshalb installieren wir auch das Toolpaket.

* Führen Sie `Install-Package Microsoft.EntityFrameworkCore.Tools` aus.

* Bearbeiten Sie die CSPROJ-Datei, und ersetzen Sie `<TargetFramework>netstandard2.0</TargetFramework>` durch `<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`.

## <a name="create-your-model"></a>Erstellen Ihres Modells

Jetzt ist es Zeit, einen Kontext und Entitätsklassen für Ihr Modell zu definieren.

* Klicken Sie auf „Projekt“ > „Klasse hinzufügen“.

* Geben Sie als Name *Model.cs* ein, und klicken Sie auf **OK**.

* Ersetzen Sie den Inhalt der Datei durch den folgenden Code.

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a>Erstellen eines neuen UWP-Projekts

* Öffnen Sie Visual Studio.

* Klicken Sie auf „Datei“ > „Neu“ > „Projekt“.

* Wählen Sie im Menü links „Vorlagen“ > „Visual C#“ > „Windows Universal“ aus.

* Wählen Sie die Projektvorlage **Leere App (universelles Windows)** aus.

* Benennen Sie das Projekt, und klicken Sie auf **OK**.

* Legen Sie die Ziel- und Mindestversionen mindestens auf `Windows 10 Fall Creators Update (10.0; build 16299.0)` fest.

## <a name="create-your-database"></a>Erstellen Ihrer Datenbank

Nachdem Sie jetzt über ein Modell verfügen, können Sie mithilfe von Migrationen eine Datenbank erstellen.

* Wählen Sie „Tools“ > „NuGet-Paket-Manager“ > „Paket-Manager-Konsole“ aus.

* Wählen Sie das Modellprojekt als Standardprojekt aus, und legen Sie es als Startprojekt fest.

* Führen Sie `Add-Migration MyFirstMigration` aus, um per Gerüstbau eine Migration zum Erstellen des anfänglichen Tabellensatzes für Ihr Modell einzurichten.

Weil die Datenbank auf dem Gerät erstellt werden soll, auf der die App ausgeführt wird, fügen wir Code hinzu, mit dem beim Anwendungsstart ausstehende Migrationen auf die lokale Datenbank angewendet werden. Bei der ersten App-Ausführung wird hierbei die lokale Datenbank für uns erstellt.

* Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **App.xaml**, und wählen Sie **Code anzeigen** aus.

* Fügen Sie den hervorgehobenen using-Code am Anfang der Datei hinzu.

* Fügen Sie den hervorgehobenen Code hinzu, um ausstehende Migrationen anzuwenden.

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/App.xaml.cs?highlight=1,25-28)]

> [!TIP]  
> Wenn Sie später Änderungen an Ihrem Modell durchführen, können Sie mit dem Befehl `Add-Migration` per Gerüstbau eine neue Migration einrichten, um die entsprechenden Schemaänderungen auf die Datenbank anzuwenden. Alle ausstehenden Migrationen werden beim Start der Anwendung auf die lokale Datenbank auf jedem Gerät angewendet.
>
>EF verwendet eine `__EFMigrationsHistory`-Tabelle in der Datenbank, um nachzuverfolgen, welche Migrationen bereits auf die Datenbank angewendet wurden.

## <a name="use-your-model"></a>Verwenden Ihres Modells

Jetzt können Sie Ihr Modell zum Zugreifen auf Daten verwenden.

* Öffnen Sie *MainPage.xaml*.

* Fügen Sie den nachstehend markierten Handler zum Laden der Seite und den Benutzerschnittstelleninhalt ein.

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml?highlight=9,11-23)]

Jetzt fügen wir Code zum Verknüpfen der Benutzerschnittstelle mit der Datenbank ein.

* Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **MainPage.xaml**, und wählen Sie **Code anzeigen** aus.

* Fügen Sie den hervorgehobenen Code aus dem folgenden Listing hinzu.

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml.cs?highlight=30-48)]

Sie können die Anwendung jetzt ausführen, um sie in Aktion zu sehen.

* Klicken Sie auf „Debuggen“ > „Ohne Debuggen starten“.

* Die Anwendung wird erstellt und gestartet.

* Geben Sie eine URL ein, und klicken Sie auf die Schaltfläche **Hinzufügen**.

![image](_static/create.png)

![image](_static/list.png)

## <a name="next-steps"></a>Nächste Schritte

> [!TIP]
> Die Leistung von `SaveChanges()` kann durch das Implementieren von [`INotifyPropertyChanged`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [`INotifyPropertyChanging`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx), [`INotifyCollectionChanged`](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) in Ihren Entitätstypen und durch Verwendung von`ChangeTrackingStrategy.ChangingAndChangedNotifications` verbessert werden.

Voilá! Sie verfügen jetzt über eine einfache UWP-App mit Ausführung von Entity Framework.

Sehen Sie sich auch die weiteren Artikel in dieser Dokumentation an, um mehr über die Features von Entity Framework zu erfahren.
