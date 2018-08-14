---
title: Erste Schritte in ASP.NET Core – Neue Datenbank – EF Core
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
ms.date: 08/03/2018
ms.topic: get-started-article
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 9e86bc9cff028ad9791f23cbb45f0a93110c0064
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614349"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a>Erste Schritte mit EF Core in ASP.NET Core mit einer neuen Datenbank

In diesem Tutorial entwickeln Sie eine ASP.NET Core MVC-Anwendung, die einen grundlegenden Datenzugriff mit Entity Framework Core durchführt. Sie verwenden Migrationen, um die Datenbank aus Ihrem EF Core-Modell zu erstellen.

[Sehen Sie sich das Beispiel aus diesem Artikel auf GitHub an](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).

## <a name="prerequisites"></a>Erforderliche Komponenten

Installieren Sie die folgenden Softwarekomponenten:

* [Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) mit diesen Workloads:
  * **ASP.NET- und Webentwicklung** (unter **Web & Cloud**)
  * **Plattformübergreifende .NET Core-Entwicklung** (unter **Andere Toolsets**)
* [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core)

## <a name="create-a-new-project-in-visual-studio-2017"></a>Erstellen eines neuen Projekts in Visual Studio 2017

* Öffnen Sie Visual Studio 2017.
* Klicken Sie auf **Datei > Neu > Projekt**.
* Klicken Sie im Menü links auf **Installiert > Visual C# > .NET Core**.
* Wählen Sie **ASP.NET Core-Webanwendung** aus.
* Geben Sie als Name **EFGetStarted.AspNetCore.NewDb** ein, und klicken Sie auf **OK**.
* Gehen Sie im Dialogfeld **ASP.NET Core-Webanwendung** folgendermaßen vor:
  * Stellen Sie sicher, dass die Optionen **.NET Core** und **ASP.NET Core 2.1** in den Dropdownlisten ausgewählt sind.
  * Wählen Sie die Projektvorlage **Webanwendung (Model-View-Controller)** aus.
  * Stellen Sie sicher, dass **Authentifizierung** auf **Keine Authentifizierung** festgelegt ist.
  * Klicken Sie auf **OK**.

Warnung: Wenn Sie für **Authentifizierung** anstelle von **Keine Authentifizierung** die Option **Einzelne Benutzerkonten** verwenden, wird Ihrem Projekt in `Models\IdentityModel.cs` ein Entity Framework Core-Modell hinzugefügt. Anhand der Techniken, die Sie in diesem Tutorial kennenlernen, können Sie ein zweites Modell hinzufügen oder das vorhandene Modell um Ihre Entitätsklassen erweitern.

## <a name="install-entity-framework-core"></a>Installieren von Entity Framework Core

Installieren Sie das Paket für den (oder die) gewünschten EF Core-Datenbankanbieter, um EF Core zu installieren. Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md). 

Für dieses Tutorial müssen Sie kein Anbieterpaket installieren, da das Tutorial SQL Server verwendet. Das SQL Server-Anbieterpaket ist im Metapaket [Microsoft.AspnetCore.App](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1) enthalten.

## <a name="create-the-model"></a>Erstellen des Modells

Definieren Sie eine Kontextklasse und Entitätsklassen für das Modell:

* Klicken Sie mit der rechten Maustaste auf den Ordner **Modelle**, und wählen Sie **Hinzufügen > Klasse** aus.
* Geben Sie als Name **Model.cs** ein, und klicken Sie auf **OK**.
* Ersetzen Sie den Inhalt der Datei durch den folgenden Code:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

In einer echten Anwendung würden Sie jede Klasse Ihres Modells typischerweise in einer separaten Datei platzieren. Der Einfachheit halber werden in diesem Tutorial alle Klassen in einer Datei zusammengefasst.

## <a name="register-your-context-with-dependency-injection"></a>Registrieren Ihres Kontexts durch Abhängigkeitsinjektion

Dienste (z.B. `BloggingContext`) werden per [Abhängigkeitsinjektion](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) während des Anwendungsstarts registriert. Komponenten, die diese Dienste benötigen (z.B. Ihre MVC-Controller), werden die Dienste anschließend über Konstruktorparameter oder Eigenschaften bereitgestellt.

Um eine `BloggingContext`-Klasse für MVC-Controller verfügbar zu machen, registrieren Sie sie als Dienst.

* Öffnen Sie **Startup.cs**.
* Fügen Sie die folgenden `using` -Anweisungen hinzu:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

Rufen Sie die `AddDbContext`-Methode auf, um den Kontext als Dienst zu registrieren.

* Fügen Sie der `ConfigureServices`-Methode den folgenden hervorgehobenen Code zu:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=13-14)]

Hinweis: In einer echten Anwendung wird die Verbindungszeichenfolge im Allgemeinen in eine Konfigurationsdatei oder Umgebungsvariable eingefügt. Der Einfachheit halber wird sie in diesem Tutorial im Code definiert. Weitere Informationen finden Sie in [Verbindungszeichenfolgen](../../miscellaneous/connection-strings.md).

## <a name="create-the-database"></a>Erstellen der Datenbank

Da Sie jetzt über ein Modell verfügen, können Sie mithilfe von [Migrationen](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations) eine Datenbank erstellen.

* Wählen Sie **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.
* Führen Sie `Add-Migration InitialCreate` aus, um per Gerüstbau eine Migration zum Erstellen des anfänglichen Tabellensatzes für Ihr Modell einzurichten. Wenn Sie den Fehler `The term 'add-migration' is not recognized as the name of a cmdlet` erhalten, schließen Sie Visual Studio, und öffnen Sie es erneut.
* Führen Sie `Update-Database` aus, um die neue Migration auf die Datenbank anzuwenden. Mit diesem Befehl wird die Datenbank erstellt, bevor Migrationen angewendet werden.

## <a name="create-a-controller"></a>Erstellen eines Controllers

Erstellen Sie ein Gerüst für einen Controller und Ansichten für die `Blog`-Entität.

* Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Controller**, und wählen Sie **Hinzufügen > Controller** aus.
* Klicken Sie auf **MVC-Controller mit Ansichten unter Verwendung von Entity Framework** und danach auf **Hinzufügen**.
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
