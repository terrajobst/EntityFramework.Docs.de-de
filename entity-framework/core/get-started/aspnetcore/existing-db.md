---
title: Erste Schritte in ASP.NET Core – Vorhandene Datenbank – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: e28149346ccd7531449ea696505588317471e6dd
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949152"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a>Erste Schritte mit EF Core in ASP.NET Core mit einer vorhandenen Datenbank

In dieser exemplarischen Vorgehensweise entwickeln Sie eine ASP.NET Core MVC-Anwendung, die einen grundlegenden Datenzugriff mit Entity Framework durchführt. Sie erstellen per Reverse Engineering ein Entity Framework-Modell, das auf einer vorhandenen Datenbank basiert.

> [!TIP]  
> Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb) finden Sie auf GitHub.

## <a name="prerequisites"></a>Erforderliche Komponenten

Zum Ausführen dieser exemplarischen Vorgehensweise müssen folgende Voraussetzungen erfüllt sein:

* [Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) mit diesen Workloads:
  * **ASP.NET- und Webentwicklung** (unter **Web & Cloud**)
  * **Plattformübergreifende .NET Core-Entwicklung** (unter **Andere Toolsets**)
* [.NET Core 2.0 SDK](https://www.microsoft.com/net/download/core).
* [Blogging-Datenbank](#blogging-database)

### <a name="blogging-database"></a>Blogging-Datenbank

In diesem Tutorial wird als vorhandene Datenbank eine **Blogging**-Datenbank in Ihrer LocalDb-Instanz verwendet.

> [!TIP]  
> Wenn Sie die **Blogging**-Datenbank bereits im Rahmen eines anderen Tutorials erstellt haben, können Sie diese Schritte überspringen.

* Öffnen Sie Visual Studio.
* Klicken Sie auf **Tools > Verbindung mit Datenbank herstellen**.
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
* Klicken Sie auf **Datei > Neu > Projekt**.
* Wählen Sie im Menü links **Installiert > Vorlagen > Visual C# > Web** aus.
* Wählen Sie die Projektvorlage **ASP.NET Core-Webanwendung (.NET Core)** aus.
* Geben Sie als Name **EFGetStarted.AspNetCore.ExistingDb** ein, und klicken Sie auf **OK**.
* Warten Sie, bis das Dialogfeld **Neue ASP.NET Core-Webanwendung** angezeigt wird.
* Wählen Sie unter **ASP.NET Core Templates 2.0** die Vorlage **Webanwendung (Model-View-Controller)** aus.
* Stellen Sie sicher, dass **Authentifizierung** auf **Keine Authentifizierung** festgelegt ist.
* Klicken Sie auf **OK**.

## <a name="install-entity-framework"></a>Installieren von Entity Framework

Installieren Sie zur Verwendung von EF Core das Paket für den (oder die) gewünschten Datenbankanbieter. In dieser exemplarischen Vorgehensweise wird SQL Server verwendet. Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).

* Wählen Sie **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.

* Ausführen von `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Wir werden einige Entity Framework-Tools verwenden, um ein Modell aus der Datenbank zu erstellen. Deshalb installieren wir auch das Toolpaket:

* Ausführen von `Install-Package Microsoft.EntityFrameworkCore.Tools`

Später verwenden wir einige ASP.NET Core-Gerüsttools, um Controller und Ansichten zu erstellen. Deshalb installieren wird auch dieses Entwurfspaket:

* Ausführen von `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`

## <a name="reverse-engineer-your-model"></a>Zurückentwickeln (Reverse Engineering) Ihres Modells

Jetzt ist es Zeit, basierend auf Ihrer vorhandenen Datenbank das EF-Modell zu erstellen.

* Wählen Sie **Tools > NuGet-Paket-Manager > Paket-Manager-Konsole** aus.
* Führen Sie den folgenden Befehl aus, um ein Modell aus der vorhandenen Datenbank zu erstellen:

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

Wenn Sie den Fehler `The term 'Scaffold-DbContext' is not recognized as the name of a cmdlet` erhalten, schließen Sie Visual Studio, und öffnen Sie es erneut.

> [!TIP]  
> Indem Sie das `-Tables`-Argument zum obigen Befehl hinzufügen, können Sie angeben, für welche Tabellen Sie Entitäten generieren möchten. Beispielsweise `-Tables Blog,Post`.

Der Reverse Engineering-Prozess hat Entitätsklassen (`Blog.cs` & `Post.cs`) und einen abgeleiteten Kontext (`BloggingContext.cs`) basierend auf dem Schema der vorhandenen Datenbank erstellt.

 Die Entitätsklassen sind einfache C#-Objekte zum Repräsentieren der Daten, die Sie abfragen und speichern werden.

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

 Der Kontext repräsentiert eine Sitzung mit der Datenbank und ermöglicht Ihnen das Abfragen und Speichern von Instanzen der Entitätsklassen.

<!-- Static code listing, rather than a linked file, because the walkthrough modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
    public virtual DbSet<Blog> Blog { get; set; }
    public virtual DbSet<Post> Post { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        if (!optionsBuilder.IsConfigured)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>(entity =>
        {
            entity.Property(e => e.Url).IsRequired();
        });

        modelBuilder.Entity<Post>(entity =>
        {
            entity.HasOne(d => d.Blog)
                .WithMany(p => p.Post)
                .HasForeignKey(d => d.BlogId);
        });
    }
}
```

## <a name="register-your-context-with-dependency-injection"></a>Registrieren Ihres Kontexts durch Abhängigkeitsinjektion

Die Abhängigkeitsinjektion ist ein zentrales Konzept von ASP.NET Core. Dienste (z.B. `BloggingContext`) werden per Abhängigkeitsinjektion während des Anwendungsstarts registriert. Komponenten, die diese Dienste benötigen (z.B. Ihre MVC-Controller), werden die Dienste anschließend über Konstruktorparameter oder Eigenschaften bereitgestellt. Weitere Informationen zur Abhängigkeitsinjektion finden Sie im Artikel [Abhängigkeitsinjektion](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) auf der ASP.NET-Website.

### <a name="remove-inline-context-configuration"></a>Entfernen der Inlinekontextkonfiguration

In ASP.NET Core wird die Konfiguration im Allgemeinen in **Startup.cs** durchgeführt. Um diesem Muster zu entsprechen, verschieben wir die Konfiguration des Datenbankanbieters in **Startup.cs**.

* Öffnen Sie `Models\BloggingContext.cs`.
* Löschen Sie die `OnConfiguring(...)`-Methode.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
    optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
}
```

* Fügen Sie den folgenden Konstruktor hinzu, der eine Übergabe der Konfiguration in den Kontext per Abhängigkeitsinjektion zulässt.

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/BloggingContext.cs#Constructor)]

### <a name="register-and-configure-your-context-in-startupcs"></a>Registrieren und Konfigurieren Ihres Kontexts in „Startup.cs“

Damit unsere MVC-Controller `BloggingContext` nutzen können, werden wir sie als Dienst registrieren.

* Öffnen Sie **Startup.cs**.
* Fügen Sie am Anfang der Datei die folgenden `using`-Anweisungen ein.

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

Jetzt können wir die `AddDbContext(...)`-Methode verwenden, um sie als Dienst zu registrieren.
* Suchen Sie nach der `ConfigureServices(...)`-Methode.
* Fügen Sie den folgenden Code hinzu, um den Kontext als Dienst zu registrieren.

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

> [!TIP]  
> In einer echten Anwendung würden Sie die Verbindungszeichenfolge typischerweise in einer Konfigurationsdatei platzieren. Der Einfachheit halber definieren wir sie hier im Code. Weitere Informationen finden Sie in [Verbindungszeichenfolgen](../../miscellaneous/connection-strings.md).

## <a name="create-a-controller"></a>Erstellen eines Controllers

Als Nächstes aktivieren wir den Gerüstbau in unserem Projekt.

* Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Controller**, und wählen Sie **Hinzufügen > Controller** aus.
* Wählen Sie **Vollständige Abhängigkeiten** aus, und klicken Sie auf **Hinzufügen**.
* Sie können die Anweisungen in der jetzt geöffneten Datei `ScaffoldingReadMe.txt` ignorieren.

Nach der Aktivierung des Gerüstbaus können Sie einen Controller für die `Blog`-Entität einrichten.

* Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner **Controller**, und wählen Sie **Hinzufügen > Controller** aus.
* Wählen Sie **MVC-Controller mit Ansichten unter Verwendung von Entity Framework** aus, und klicken Sie auf **OK**.
* Legen Sie **Modellklasse** auf **Blog** und **Datenkontextklasse** auf **BloggingContext** fest.
* Klicken Sie auf **Hinzufügen**.

## <a name="run-the-application"></a>Ausführen der Anwendung

Sie können die Anwendung jetzt ausführen, um sie in Aktion zu sehen.

* Klicken Sie auf **Debuggen > Ohne Debuggen starten**.
* Die Anwendung wird erstellt und in einem Webbrowser geöffnet.
* Navigieren Sie zu `/Blogs`.
* Klicken Sie auf **Neu erstellen**.
* Geben Sie eine **URL** für den neuen Blog ein, und klicken Sie auf **Erstellen**.

![Bild](_static/create.png)

![Bild](_static/index-existing-db.png)
