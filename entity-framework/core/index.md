---
title: Kurze Übersicht – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
ms.technology: entity-framework-core
uid: core/index
ms.openlocfilehash: 3befcbd3ff3da5dd159e6e6cb5fe7140c81317c2
ms.sourcegitcommit: a2b38dedc88ca3ccbfe7b1db9602ca02da8294cd
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/31/2018
ms.locfileid: "34686661"
---
# <a name="entity-framework-core-quick-overview"></a>Entity Framework Core – Kurze Übersicht

Entity Framework Core (EF Core) ist eine einfache, erweiterbare und plattformübergreifende Version der beliebten Entity Framework-Datenzugriffstechnologie.

EF Core kann als objektrelationaler Mapper (O/RM) eingesetzt werden und bietet .NET-Entwicklern so die Möglichkeit, unter Verwendung von .NET-Objekten mit einer Datenbank zu arbeiten. Auf diese Weise entfällt ein Großteil des Datenzugriffscodes, der üblicherweise geschrieben werden muss. 

Einzelheiten zu den von EF Core unterstützten Datenbank-Engines finden Sie unter [Datenbankanbieter](providers/index.md).

Wenn Sie das Schreiben von Code erlernen möchten, empfehlen wir Ihnen für den Einstieg in EF Core einen unserer Leitfäden mit den [Ersten Schritten](get-started/index.md).

## <a name="what-is-new-in-ef-core"></a>Neue Features in EF Core

Wenn Sie mit EF Core vertraut sind und sich direkt mit den Einzelheiten zu den neuesten Releases auseinandersetzen möchten, helfen Ihnen folgende Ressourcen weiter:

- **[Neue Features in EF Core 2.1](xref:core/what-is-new/ef-core-2.1)**
- **[Durchführen eines Upgrades für vorhandene Anwendungen auf EF Core 2.x](xref:core/miscellaneous/1x-2x-upgrade)**


## <a name="get-entity-framework-core"></a>Beziehen von Entity Framework Core

[Installieren Sie das NuGet-Paket](https://docs.nuget.org/ndocs/quickstart/use-a-package) für den gewünschten Datenbankanbieter. Beispiel: Installieren Sie mit dem `dotnet`-Tool in der Befehlszeile den SQL Server-Anbieter in der plattformübergreifenden Entwicklung:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Alternativ können Sie die Paket-Manager-Konsole in Visual Studio verwenden:

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
Informationen zu verfügbaren Anbietern finden Sie unter [Datenbankanbieter](providers/index.md) und ausführlichere Beschreibungen der Installationsschritte unter [Installieren von EF Core](get-started/install/index.md).

## <a name="the-model"></a>Das Modell

Bei EF Core erfolgt der Datenzugriff über ein Modell. Ein Modell setzt sich aus Entitätsklassen und einem abgeleiteten Kontext zusammen, der eine Sitzung bei der Datenbank darstellt und Ihnen das Abfragen und Speichern von Daten ermöglicht. Weitere Informationen finden Sie unter [Erstellen eines Modells](modeling/index.md).

Sie können ein Modell aus einer vorhandenen Datenbank generieren, ein Modell manuell entsprechend Ihrer Datenbank codieren oder mithilfe von EF-Migrationen eine Datenbank anhand Ihres Modells erstellen (und es im Laufe der Zeit entsprechend Ihres Modells weiterentwickeln).

``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace Intro
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=MyDatabase;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }
        public int Rating { get; set; }
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

## <a name="querying"></a>Abfragen

Instanzen Ihrer Entitätsklassen werden mit Language Integrated Query (LINQ) von der Datenbank abgerufen. Weitere Informationen finden Sie unter [Abfragen von Daten](querying/index.md).

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a>Speichern von Daten

Daten werden in der Datenbank mithilfe von Instanzen Ihrer Entitätsklassen erstellt, gelöscht und geändert. Weitere Informationen finden Sie unter [Speichern von Daten](saving/index.md).

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
