---
title: Erste Schritte in ASP.NET Core – Neue Datenbank – EF Core
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 878478099878e4a0bc65c44fef0609d28f39f2b8
ms.sourcegitcommit: 7a7da65404c9338e1e3df42576a13be536a6f95f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2018
ms.locfileid: "48834772"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a>Erste Schritte mit EF Core in ASP.NET Core mit einer neuen Datenbank

In diesem Tutorial entwickeln Sie eine ASP.NET Core MVC-Anwendung, die einen grundlegenden Datenzugriff mit Entity Framework Core durchführt. Dieses Tutorial verwendet Migrationen, um die Datenbank aus dem Datenmodell zu erstellen.

Sie können das Tutorial mit Visual Studio 2017 für Windows oder mithilfe von .NET Core-CLI unter Windows, MacOS oder Linux nachvollziehen.

Beispiel aus diesem Artikel auf GitHub anzeigen:
* [Visual Studio 2017 mit SQLServer](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb)
* [.NET Core-CLI mit SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).

## <a name="prerequisites"></a>Erforderliche Komponenten

Installieren Sie die folgenden Softwarekomponenten:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2017, Version 15.7 oder höher](https://www.visualstudio.com/downloads/), mit folgenden Workloads:
  * **ASP.NET- und Webentwicklung** (unter **Web & Cloud**)
  * **Plattformübergreifende .NET Core-Entwicklung** (unter **Andere Toolsets**)
* [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core)

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core-CLI](#tab/netcore-cli)

* [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core)

---

## <a name="create-a-new-project"></a>Erstellt ein neues Projekt

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Öffnen Sie Visual Studio 2017.
* Klicken Sie auf **Datei > Neu > Projekt**.
* Wählen Sie im Menü links **Installiert > Visual C# > .NET Core** aus.
* Wählen Sie **ASP.NET Core-Webanwendung** aus.
* Geben Sie als Name **EFGetStarted.AspNetCore.NewDb** ein, und klicken Sie auf **OK**.
* Gehen Sie im Dialogfeld **ASP.NET Core-Webanwendung** folgendermaßen vor:
  * Achten Sie darauf, dass **.NET Core** und **ASP.NET Core 2.1** in den Dropdownlisten ausgewählt sind.
  * Wählen Sie die Projektvorlage **Webanwendung (Model-View-Controller)** aus.
  * Vergewissern Sie sich, dass **Authentifizierung** auf **Keine Authentifizierung** festgelegt ist.
  * Klicken Sie auf **OK**.

Warnung: Wenn Sie für **Authentifizierung** anstelle von **Keine Authentifizierung** die Option **Einzelne Benutzerkonten** verwenden, wird dem Projekt in `Models\IdentityModel.cs` ein Entity Framework Core-Modell hinzugefügt. Anhand der Techniken, die Sie in diesem Tutorial kennenlernen, können Sie ein zweites Modell hinzufügen oder das vorhandene Modell um Ihre Entitätsklassen erweitern.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core-CLI](#tab/netcore-cli)

* Führen Sie den folgenden Befehl aus, um ein MVC-Projekt zu erstellen:

   ```cli
   dotnet new mvc -n EFGetStarted.AspNetCore.NewDb
   ```
* Wechseln Sie in das Projektverzeichnis. Die nächsten Befehle, die Sie eingeben, müssen für das neue Projekt ausgeführt werden.

   ```cli
   cd EFGetStarted.AspNetCore.NewDb
   ```
---

## <a name="install-entity-framework-core"></a>Installieren von Entity Framework Core

Installieren Sie das Paket für den (oder die) gewünschten EF Core-Datenbankanbieter, um EF Core zu installieren. Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Für dieses Tutorial müssen Sie kein Anbieterpaket installieren, da das Tutorial SQL Server verwendet. Das SQL Server-Anbieterpaket ist im Metapaket [Microsoft.AspnetCore.App](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1) enthalten.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core-CLI](#tab/netcore-cli)

In diesem Tutorial wird SQLite verwendet, da es auf allen Plattformen ausgeführt werden kann, die .NET Core unterstützt.

* Führen Sie den folgenden Befehl aus, um den SQLite-Anbieter zu installieren:

   ```cli
   dotnet add package Microsoft.EntityFrameworkCore.Sqlite
   ```

---

## <a name="create-the-model"></a>Erstellen des Modells

Definieren Sie eine Kontextklasse und Entitätsklassen für das Modell.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Klicken Sie mit der rechten Maustaste auf den Ordner **Modelle**, und wählen Sie **Hinzufügen > Klasse** aus.
* Geben Sie als Name **Model.cs** ein, und klicken Sie auf **OK**.
* Ersetzen Sie den Inhalt der Datei durch den folgenden Code:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core-CLI](#tab/netcore-cli)

* Erstellen Sie mit dem folgenden Code im Ordner **Models** (Modelle) eine **Model.cs**-Datei:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Models/Model.cs)]

---

Bei einer Produktionsanwendung würde normalerweise jede Klasse in einer separaten Datei platziert. Der Einfachheit halber werden in diesem Tutorial die Klassen in einer Datei zusammengefasst.

## <a name="register-the-context-with-dependency-injection"></a>Registrieren des Kontexts durch Dependency Injection

Dienste (z.B. `BloggingContext`) werden per [Abhängigkeitsinjektion](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) während des Anwendungsstarts registriert. Komponenten, die diese Dienste erfordern (z.B. MVC-Controller), werden über Konstruktorparameter oder Eigenschaften bereitgestellt.

Um eine `BloggingContext`-Klasse für MVC-Controller verfügbar zu machen, registrieren Sie sie als Dienst.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Fügen Sie in **Startup.cs** die folgenden `using`-Anweisungen hinzu:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

* Fügen Sie der `ConfigureServices`-Methode den folgenden hervorgehobenen Code zu:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=12-14)]

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core-CLI](#tab/netcore-cli)

* Fügen Sie in **Startup.cs** die folgenden `using`-Anweisungen hinzu:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs#AddedUsings)]

* Fügen Sie der `ConfigureServices`-Methode den folgenden hervorgehobenen Code zu:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs?name=ConfigureServices&highlight=12-14)]

---

In einer Produktionsanwendung wird die Verbindungszeichenfolge im Allgemeinen in eine Konfigurationsdatei oder Umgebungsvariable eingefügt. Der Einfachheit halber wird sie in diesem Tutorial im Code definiert. Weitere Informationen finden Sie in [Verbindungszeichenfolgen](../../miscellaneous/connection-strings.md).

## <a name="create-the-database"></a>Erstellen der Datenbank

Die folgenden Schritte verwenden [Migrationen](xref:core/managing-schemas/migrations/index), um eine Datenbank zu erstellen.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Wählen Sie **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.
* Führen Sie die folgenden Befehle aus:

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

  Wenn Sie den Fehler `The term 'add-migration' is not recognized as the name of a cmdlet` erhalten, schließen Sie Visual Studio, und öffnen Sie es erneut.

  Der Befehl `Add-Migration` richtet per Gerüstbau eine Migration ein und erstellt den anfänglichen Tabellensatz für das Modell. Der Befehl `Update-Database` erstellt die Datenbank und wendet die neue Migration auf sie an.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core-CLI](#tab/netcore-cli)

* Führen Sie die folgenden Befehle aus:

  ```cli
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  Der Befehl `migrations` richtet per Gerüstbau eine Migration ein und erstellt den anfänglichen Tabellensatz für das Modell. Der Befehl `database update` erstellt die Datenbank und wendet die neue Migration auf sie an.

---

## <a name="create-a-controller"></a>Erstellen eines Controllers

Erstellen Sie ein Gerüst für einen Controller und Ansichten für die `Blog`-Entität.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Controller**, und wählen Sie **Hinzufügen > Controller** aus.
* Klicken Sie auf **MVC-Controller mit Ansichten unter Verwendung von Entity Framework** und danach auf **Hinzufügen**.
* Legen Sie **Modellklasse** auf **Blog** und **Datenkontextklasse** auf **BloggingContext** fest.
* Klicken Sie auf **Hinzufügen**.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core-CLI](#tab/netcore-cli)

* Führen Sie die folgenden Befehle aus:

  ```cli
  dotnet tool install -g dotnet-aspnet-codegenerator
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet restore
  dotnet aspnet-codegenerator controller -name BlogsController -m Blog -dc BloggingContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  Die Befehle `tool install` und `add package` installieren die Tools, die für den Gerüstbau für Controller und Ansichten verwendet werden. Mit dem Befehl `restore` wird sichergestellt, dass alle Pakete des Projekts heruntergeladen werden, und der Befehl `aspnet-codegenerator` erledigt den Gerüstbau.
---

Die Gerüstbau-Engine erstellt die folgenden Dateien:

* Einen Controller (*Controllers/BlogsController.cs*)
* Razor-Ansichten für die Seiten „Erstellen“, „Löschen“, „Details“ „Bearbeiten“ und „Index“ (_Views/Movies/.cshtml_)

## <a name="run-the-application"></a>Ausführen der Anwendung

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Debuggen** > **Starten ohne Debuggen**

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core-CLI](#tab/netcore-cli)

```cli
dotnet run
```
---

* Navigieren Sie zu `/Blogs`.

* Verwenden Sie den Link **Neu erstellen**, um einige Blogeinträge zu erstellen.

  ![Seite „Create“](_static/create.png)

* Testen Sie die Links **Details**, **Bearbeiten** und **Löschen**.

  ![Indexseite](_static/index-new-db.png)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Tutorial: Erste Schritte mit EF Core in .NET Core mit einer neuen Datenbank mithilfe von SQLite](xref:core/get-started/netcore/new-db-sqlite)
* [Tutorial: Erste Schritte mit Razor Pages in ASP.NET Core](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)
* [Tutorial: Razor Pages mit Entity Framework Core in ASP.NET Core](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
