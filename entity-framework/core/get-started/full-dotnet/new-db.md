---
title: Erste Schritte in .NET Framework – Neue Datenbank – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: bd7054c6834ae11bfdc352d63654e4304771e432
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812520"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a>Erste Schritte mit EF Core in .NET Framework mit einer neuen Datenbank

In dieser exemplarischen Vorgehensweise entwickeln Sie eine Konsolenanwendung, die einen grundlegenden Datenzugriff auf eine Microsoft SQL Server-Datenbank mithilfe von Entity Framework durchführt. Sie verwenden Migrationen, um die Datenbank aus Ihrem Modell zu erstellen.

> [!TIP]  
> Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb) finden Sie auf GitHub.

## <a name="prerequisites"></a>Erforderliche Komponenten

Zum Ausführen dieser exemplarischen Vorgehensweise müssen folgende Voraussetzungen erfüllt sein:

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)

* [Neueste Version des NuGet-Paket-Managers](https://dist.nuget.org/index.html)

* [Neueste Version von Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

## <a name="create-a-new-project"></a>Erstellt ein neues Projekt

* Öffnen Sie Visual Studio.

* Klicken Sie auf „Datei“ > „Neu“ > „Projekt“.

* Wählen Sie im Menü links „Vorlagen“ > „Visual C#“ > „Klassischer Windows-Desktop“ aus.

* Wählen Sie die Projektvorlage **Konsolen-App (.NET Framework)** aus.

* Stellen Sie sicher, dass Sie als Ziel **.NET Framework 4.5.1** oder höher verwenden.

* Benennen Sie das Projekt, und klicken Sie auf **OK**.

## <a name="install-entity-framework"></a>Installieren von Entity Framework

Installieren Sie zur Verwendung von EF Core das Paket für den (oder die) gewünschten Datenbankanbieter. In dieser exemplarischen Vorgehensweise wird SQL Server verwendet. Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).

* Wählen Sie „Tools“ > „NuGet-Paket-Manager“ > „Paket-Manager-Konsole“ aus.

* Ausführen von `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Später in dieser exemplarischen Vorgehensweise werden einige Entity Framework-Tools zum Verwalten der Datenbank eingesetzt. Deshalb installieren wir auch das Toolpaket.

* Ausführen von `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="create-your-model"></a>Erstellen Ihres Modells

Jetzt ist es Zeit, einen Kontext und Entitätsklassen für Ihr Modell zu definieren.

* Klicken Sie auf „Projekt“ > „Klasse hinzufügen“.

* Geben Sie als Name *Model.cs* ein, und klicken Sie auf **OK**.

* Ersetzen Sie den Inhalt der Datei durch den folgenden Code.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }

        public List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
}
```

> [!TIP]  
> In einer echten Anwendung würden Sie jede Anwendung in einer separaten Datei platzieren und die Verbindungszeichenfolge in die Datei `App.Config` einfügen und sie mithilfe von `ConfigurationManager` auslesen. Der Einfachheit halber werden in diesem Tutorial alle Elemente in einer einzigen Codedatei zusammengefasst.

## <a name="create-your-database"></a>Erstellen Ihrer Datenbank

Nachdem Sie jetzt über ein Modell verfügen, können Sie mithilfe von Migrationen eine Datenbank erstellen.

* Wählen Sie „Tools“ > „NuGet-Paket-Manager“ > „Paket-Manager-Konsole“ aus.

* Führen Sie `Add-Migration MyFirstMigration` aus, um per Gerüstbau eine Migration zum Erstellen des anfänglichen Tabellensatzes für Ihr Modell einzurichten.

* Führen Sie `Update-Database` aus, um die neue Migration auf die Datenbank anzuwenden. Ihre Datenbank ist noch nicht vorhanden, deshalb wird sie vor dem Anwenden der Migration erstellt.

> [!TIP]  
> Wenn Sie später Änderungen an Ihrem Modell durchführen, können Sie mit dem Befehl `Add-Migration` per Gerüstbau eine neue Migration einrichten, um die entsprechenden Schemaänderungen an der Datenbank vorzunehmen. Nachdem Sie den Gerüstcode überprüft (und alle erforderlichen Änderungen vorgenommen haben) haben, können Sie mit dem Befehl `Update-Database` Änderungen auf die Datenbank anwenden.
>
>EF verwendet eine `__EFMigrationsHistory`-Tabelle in der Datenbank, um nachzuverfolgen, welche Migrationen bereits auf die Datenbank angewendet wurden.

## <a name="use-your-model"></a>Verwenden Ihres Modells

Jetzt können Sie Ihr Modell zum Zugreifen auf Daten verwenden.

* Öffnen Sie *Program.cs*.

* Ersetzen Sie den Inhalt der Datei durch den folgenden Code.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blogs)
                {
                    Console.WriteLine(" - {0}", blog.Url);
                }
            }
        }
    }
}
```

* Klicken Sie auf „Debuggen“ > „Ohne Debuggen starten“.

Sie werden sehen, dass ein Blog in der Datenbank gespeichert wird und dann die Details zu allen Blogs an die Konsole ausgegeben werden.

![Bild](_static/output-new-db.png)
