---
title: Erste Schritte in ASP.NET Core – Neue Datenbank – EF Core
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 80477ca57b8b3df6de8ba3595c9056c6b8412040
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812572"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a>Erste Schritte mit EF Core in ASP.NET Core mit einer neuen Datenbank

In dieser exemplarischen Vorgehensweise entwickeln Sie eine ASP.NET Core MVC-Anwendung, die einen grundlegenden Datenzugriff mit Entity Framework Core durchführt. Sie verwenden Migrationen, um die Datenbank aus Ihrem EF Core-Modell zu erstellen. Weitere Entity Framework Core-Tutorials finden Sie unter [Zusätzliche Ressourcen](#additional-resources).

Für dieses Tutorial gelten die folgenden Voraussetzungen:
* [Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) mit diesen Workloads:
  * **ASP.NET- und Webentwicklung** (unter **Web & Cloud**)
  * **Plattformübergreifende .NET Core-Entwicklung** (unter **Andere Toolsets**)
* [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).

> [!TIP]  
> Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb) finden Sie auf GitHub.

## <a name="create-a-new-project-in-visual-studio-2017"></a>Erstellen eines neuen Projekts in Visual Studio 2017

* Klicken Sie auf **Datei > Neu > Projekt**.
* Wählen Sie im Menü links **Installiert > Vorlagen > Visual C# > .NET Core** aus.
* Wählen Sie **ASP.NET Core-Webanwendung** aus.
* Geben Sie als Name **EFGetStarted.AspNetCore.NewDb** ein, und klicken Sie auf **OK**.
* Gehen Sie im Dialogfeld **ASP.NET Core-Webanwendung** folgendermaßen vor:
  * Stellen Sie sicher, dass die Optionen **.NET Core** und **ASP.NET Core 2.0** in den Dropdownlisten ausgewählt sind.
  * Wählen Sie die Projektvorlage **Webanwendung (Model-View-Controller)** aus.
  * Stellen Sie sicher, dass **Authentifizierung** auf **Keine Authentifizierung** festgelegt ist.
  * Klicken Sie auf **OK**.

Warnung: Wenn Sie für **Authentifizierung** anstelle von **Keine Authentifizierung** die Option **Einzelne Benutzerkonten** verwenden, wird Ihrem Projekt in `Models\IdentityModel.cs` ein Entity Framework Core-Modell hinzugefügt. Anhand der Techniken, die Sie in dieser exemplarischen Vorgehensweise kennenlernen, können Sie ein zweites Model hinzufügen oder das vorhandene Modell um Ihre Entitätsklassen erweitern.

## <a name="install-entity-framework-core"></a>Installieren von Entity Framework Core

Installieren Sie das Paket für den (oder die) gewünschten EF Core-Datenbankanbieter. In dieser exemplarischen Vorgehensweise wird SQL Server verwendet. Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).

* Wählen Sie **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.

* Ausführen von `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Wir werden einige Entity Framework Core-Tools verwenden, um eine Datenbank aus Ihrem EF Core-Modell zu erstellen. Deshalb installieren wir auch das Toolpaket:

* Ausführen von `Install-Package Microsoft.EntityFrameworkCore.Tools`

Später verwenden wir einige ASP.NET Core-Gerüsttools, um Controller und Ansichten zu erstellen. Deshalb installieren wird auch dieses Entwurfspaket:

* Ausführen von `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`

## <a name="create-the-model"></a>Erstellen des Modells

Definieren Sie einen Kontext und Entitätsklassen für das Modell:

* Klicken Sie mit der rechten Maustaste auf den Ordner **Modelle**, und wählen Sie **Hinzufügen > Klasse** aus.
* Geben Sie als Name **Model.cs** ein, und klicken Sie auf **OK**.
* Ersetzen Sie den Inhalt der Datei durch den folgenden Code:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

Hinweis: In einer echten Anwendung würden Sie jede Klasse Ihres Modells typischerweise in einer separaten Datei platzieren. Der Einfachheit halber werden in diesem Tutorial alle Klassen in einer Datei zusammengefasst.

## <a name="register-your-context-with-dependency-injection"></a>Registrieren Ihres Kontexts durch Abhängigkeitsinjektion

Dienste (z.B. `BloggingContext`) werden per [Abhängigkeitsinjektion](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) während des Anwendungsstarts registriert. Komponenten, die diese Dienste benötigen (z.B. Ihre MVC-Controller), werden die Dienste anschließend über Konstruktorparameter oder Eigenschaften bereitgestellt.

Damit unsere MVC-Controller `BloggingContext` nutzen können, werden wir sie als Dienst registrieren.

* Öffnen Sie **Startup.cs**.
* Fügen Sie die folgenden `using` -Anweisungen hinzu:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

Fügen Sie die `AddDbContext`-Methode hinzu, um sie als Dienst zu registrieren:

* Fügen Sie der `ConfigureServices`-Methode den folgenden Code hinzu:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

Hinweis: In einer echten Anwendung wird die Verbindungszeichenfolge im Allgemeinen in eine Konfigurationsdatei eingefügt. Der Einfachheit halber definieren wir sie hier im Code. Weitere Informationen finden Sie in [Verbindungszeichenfolgen](../../miscellaneous/connection-strings.md).

## <a name="create-your-database"></a>Erstellen Ihrer Datenbank

Da Sie jetzt über ein Modell verfügen, können Sie mithilfe von [Migrationen](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) eine Datenbank erstellen.

* Öffnen Sie die Paket-Manager-Konsole:

  Wählen Sie **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.
* Führen Sie `Add-Migration InitialCreate` aus, um per Gerüstbau eine Migration zum Erstellen des anfänglichen Tabellensatzes für Ihr Modell einzurichten. Wenn Sie den Fehler `The term 'add-migration' is not recognized as the name of a cmdlet` erhalten, schließen Sie Visual Studio, und öffnen Sie es erneut.
* Führen Sie `Update-Database` aus, um die neue Migration auf die Datenbank anzuwenden. Mit diesem Befehl wird die Datenbank erstellt, bevor Migrationen angewendet werden.

## <a name="create-a-controller"></a>Erstellen eines Controllers

Aktivieren Sie den Gerüstbau im Projekt:

* Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Controller**, und wählen Sie **Hinzufügen > Controller** aus.
* Wählen Sie **Mindestens erforderliche Abhängigkeiten** aus, und klicken Sie auf **Hinzufügen**.
* Sie können die Datei *ScaffoldingReadMe.txt* ignorieren oder löschen.

Nach der Aktivierung des Gerüstbaus können Sie einen Controller für die `Blog`-Entität einrichten.

* Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Controller**, und wählen Sie **Hinzufügen > Controller** aus.
* Wählen Sie **MVC-Controller mit Ansichten unter Verwendung von Entity Framework** aus, und klicken Sie auf **OK**.
* Legen Sie **Modellklasse** auf **Blog** und **Datenkontextklasse** auf **BloggingContext** fest.
* Klicken Sie auf **Hinzufügen**.


## <a name="run-the-application"></a>Ausführen der Anwendung

Drücken Sie F5, um die App auszuführen und zu testen.

* Navigieren Sie zu `/Blogs`.
* Verwenden Sie den Link zum Erstellen, um einige Blogeinträge zu erstellen. Testen Sie die Links zum Anzeigen von Details und zum Löschen.

![Bild](_static/create.png)

![Bild](_static/index-new-db.png)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [EF – Neue Datenbank mit SQLite](xref:core/get-started/netcore/new-db-sqlite): Ein EF-Tutorial für beliebige Plattformkonsolen.
* [Einführung in ASP.NET Core MVC unter Mac oder Linux](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Einführung in ASP.NET Core MVC mit Visual Studio](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Erste Schritte mit ASP.NET Core und Entity Framework Core mithilfe von Visual Studio](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
