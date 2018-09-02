---
title: Erste Schritte in ASP.NET Core – Vorhandene Datenbank – EF Core
author: rowanmiller
ms.date: 08/02/2018
ms.assetid: 2bc68bea-ff77-4860-bf0b-cf00db6712a0
uid: core/get-started/aspnetcore/existing-db
ms.openlocfilehash: 79a73e38fdc9c4268c21de66571d6272f33e9457
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997035"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-an-existing-database"></a>Erste Schritte mit EF Core in ASP.NET Core mit einer vorhandenen Datenbank

In diesem Tutorial entwickeln Sie eine ASP.NET Core MVC-Anwendung, die einen grundlegenden Datenzugriff mit Entity Framework Core durchführt. Sie erstellen ein Entity Framework-Modell, indem Sie ein Reverse Engineering für eine vorhandene Datenbank durchführen.

[Sehen Sie sich das Beispiel aus diesem Artikel auf GitHub an](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb).

## <a name="prerequisites"></a>Erforderliche Komponenten

Installieren Sie die folgenden Softwarekomponenten:

* [Visual Studio 2017 15.7](https://www.visualstudio.com/downloads/) mit diesen Workloads:
  * **ASP.NET- und Webentwicklung** (unter **Web & Cloud**)
  * **Plattformübergreifende .NET Core-Entwicklung** (unter **Andere Toolsets**)
* [.NET Core 2.1 SDK](https://www.microsoft.com/net/download/core)

## <a name="create-blogging-database"></a>Erstellen einer Blogging-Datenbank

In diesem Tutorial wird als vorhandene Datenbank eine **Blogging**-Datenbank in Ihrer LocalDb-Instanz verwendet. Wenn Sie die **Blogging**-Datenbank bereits im Rahmen eines anderen Tutorials erstellt haben, können Sie diese Schritte überspringen.

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
* Klicken Sie auf **Datei > Neu > Projekt...**.
* Klicken Sie im linken Bereich auf **Installiert > Visual C# > Web**.
* Wählen Sie die Projektvorlage **ASP.NET Core-Webanwendung** aus.
* Geben Sie als Name **EFGetStarted.AspNetCore.ExistingDb** ein, und klicken Sie auf **OK**.
* Warten Sie, bis das Dialogfeld **Neue ASP.NET Core-Webanwendung** angezeigt wird.
* Stellen Sie sicher, dass das Dropdownmenü für das Zielframework auf **.NET Core** und für die Version auf **ASP.NET Core 2.1** festgelegt ist.
* Wählen Sie die Vorlage **Webanwendung (Model-View-Controller)** aus.
* Stellen Sie sicher, dass **Authentifizierung** auf **Keine Authentifizierung** festgelegt ist.
* Klicken Sie auf **OK**.

## <a name="install-entity-framework-core"></a>Installieren von Entity Framework Core

Installieren Sie das Paket für den (oder die) gewünschten EF Core-Datenbankanbieter, um EF Core zu installieren. Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md). 

Für dieses Tutorial müssen Sie kein Anbieterpaket installieren, da das Tutorial SQL Server verwendet. Das SQL Server-Anbieterpaket ist im Metapaket [Microsoft.AspnetCore.App](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1) enthalten.

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

 Die Entitätsklassen sind einfache C#-Objekte zum Repräsentieren der Daten, die Sie abfragen und speichern werden. Hier sind die Entitätsklassen `Blog` und `Post`:

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Blog.cs)]

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Models/Post.cs)]

> [!TIP]  
> Sie können Navigationseigenschaften auf `virtual` festlegen (Blog.Post und Post.Blog), um Lazy Loading zu aktivieren.

 Der Kontext repräsentiert eine Sitzung mit der Datenbank und ermöglicht Ihnen das Abfragen und Speichern von Instanzen der Entitätsklassen.

<!-- Static code listing, rather than a linked file, because the tutorial modifies the context file heavily -->
 ``` csharp
public partial class BloggingContext : DbContext
{
    public BloggingContext()
    {
    }

    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    {
    }

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

### <a name="register-and-configure-your-context-in-startupcs"></a>Registrieren und Konfigurieren Ihres Kontexts in „Startup.cs“

Um eine `BloggingContext`-Klasse für MVC-Controller verfügbar zu machen, registrieren Sie sie als Dienst.

* Öffnen Sie **Startup.cs**.
* Fügen Sie am Anfang der Datei die folgenden `using`-Anweisungen ein.

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs#AddedUsings)]

Jetzt können Sie die `AddDbContext(...)`-Methode verwenden, um sie als Dienst zu registrieren.
* Suchen Sie nach der `ConfigureServices(...)`-Methode.
* Fügen Sie den folgenden hervorgehobenen Code hinzu, um den Kontext als Dienst zu registrieren.

[!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.ExistingDb/Startup.cs?name=ConfigureServices&highlight=14-15)]

> [!TIP]  
> In einer echten Anwendung würden Sie die Verbindungszeichenfolge typischerweise in einer Konfigurationsdatei oder Umgebungsvariable platzieren. Der Einfachheit halber definieren Sie diese in diesem Tutorial im Code. Weitere Informationen finden Sie in [Verbindungszeichenfolgen](../../miscellaneous/connection-strings.md).

## <a name="create-a-controller-and-views"></a>Erstellen von einem Controller und Ansichten

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
