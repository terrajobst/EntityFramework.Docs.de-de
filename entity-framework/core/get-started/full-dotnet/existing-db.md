---
title: Erste Schritte in .NET Framework – Vorhandene Datenbank – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: 3cd69109e3cf8dbc103f9eea6e2553df17f29a98
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/26/2018
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a>Erste Schritte mit EF Core in .NET Framework mit einer vorhandenen Datenbank

In dieser exemplarischen Vorgehensweise entwickeln Sie eine Konsolenanwendung, die einen grundlegenden Datenzugriff auf eine Microsoft SQL Server-Datenbank mithilfe von Entity Framework durchführt. Sie erstellen per Reverse Engineering ein Entity Framework-Modell, das auf einer vorhandenen Datenbank basiert.

> [!TIP]  
> Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) finden Sie auf GitHub.

## <a name="prerequisites"></a>Erforderliche Komponenten

Zum Ausführen dieser exemplarischen Vorgehensweise müssen folgende Voraussetzungen erfüllt sein:

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)

* [Neueste Version des NuGet-Paket-Managers](https://dist.nuget.org/index.html)

* [Neueste Version von Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)

* [Blogging-Datenbank](#blogging-database)

### <a name="blogging-database"></a>Blogging-Datenbank

In diesem Tutorial wird als vorhandene Datenbank eine **Blogging**-Datenbank in Ihrer LocalDb-Instanz verwendet.

> [!TIP]  
> Wenn Sie die **Blogging**-Datenbank bereits im Rahmen eines anderen Tutorials erstellt haben, können Sie diese Schritte überspringen.

* Öffnen Sie Visual Studio.

* Klicken Sie auf „Tools“ > „Verbindung mit Datenbank herstellen“.

* Wählen Sie **Microsoft SQL Server** aus, und klicken Sie auf **Weiter**.

* Geben Sie **(localdb)\mssqllocaldb** als **Servername** ein.

* Geben Sie als **Datenbankname** den Wert **master** ein, und klicken Sie auf **OK**.

* Die master-Datenbank wird jetzt unter **Datenverbindungen** im **Server-Explorer** angezeigt.

* Klicken Sie im **Server-Explorer** mit der rechten Maustaste auf die Datenbank, und wählen Sie **Neue Abfrage** aus.

* Kopieren Sie das nachstehende Skript in den Abfrage-Editor.

* Klicken Sie mit der rechten Maustaste auf den Abfrage-Editor, und wählen Sie **Ausführen** aus.

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Erstellt ein neues Projekt

* Öffnen Sie Visual Studio.

* Klicken Sie auf „Datei“ > „Neu“ > „Projekt“.

* Wählen Sie im Menü links „Vorlagen“ > „Visual C#“ > „Windows“ aus.

* Wählen Sie die Projektvorlage **Konsolenanwendung** aus.

* Stellen Sie sicher, dass Sie als Ziel **.NET Framework 4.5.1** oder höher verwenden.

* Benennen Sie das Projekt, und klicken Sie auf **OK**.

## <a name="install-entity-framework"></a>Installieren von Entity Framework

Installieren Sie zur Verwendung von EF Core das Paket für den (oder die) gewünschten Datenbankanbieter. In dieser exemplarischen Vorgehensweise wird SQL Server verwendet. Eine Liste der verfügbaren Anbieter finden Sie unter [Datenbankanbieter](../../providers/index.md).

* Wählen Sie „Tools“ > „NuGet-Paket-Manager“ > „Paket-Manager-Konsole“ aus.

* Ausführen von `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Um das Reverse Engineering aus einer vorhandenen Datenbank zu aktivieren, müssen einige weitere Pakete installiert werden.

* Ausführen von `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="reverse-engineer-your-model"></a>Zurückentwickeln (Reverse Engineering) Ihres Modells

Jetzt ist es Zeit, basierend auf Ihrer vorhandenen Datenbank das EF-Modell zu erstellen.

* Wählen Sie „Tools“ > „NuGet-Paket-Manager“ > „Paket-Manager-Konsole“ aus.

* Führen Sie den folgenden Befehl aus, um ein Modell aus der vorhandenen Datenbank zu erstellen.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

Der Reverse Engineering-Prozess hat Entitätsklassen und einen abgeleiteten Kontext basierend auf dem Schema der vorhandenen Datenbank erstellt. Die Entitätsklassen sind einfache C#-Objekte zum Repräsentieren der Daten, die Sie abfragen und speichern werden.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)] -->
``` csharp
using System;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class Blog
    {
        public Blog()
        {
            Post = new HashSet<Post>();
        }

        public int BlogId { get; set; }
        public string Url { get; set; }

        public virtual ICollection<Post> Post { get; set; }
    }
}
```

Der Kontext repräsentiert eine Sitzung mit der Datenbank und ermöglicht Ihnen das Abfragen und Speichern von Instanzen der Entitätsklassen.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Metadata;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class BloggingContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
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

        public virtual DbSet<Blog> Blog { get; set; }
        public virtual DbSet<Post> Post { get; set; }
    }
}
```

## <a name="use-your-model"></a>Verwenden Ihres Modells

Jetzt können Sie Ihr Modell zum Zugreifen auf Daten verwenden.

* Öffnen Sie *Program.cs*.

* Ersetzen Sie den Inhalt der Datei durch den folgenden Code.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blog.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blog)
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

![Bild](_static/output-existing-db.png)
