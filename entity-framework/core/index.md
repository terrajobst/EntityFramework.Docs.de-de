---
title: "Kurze Übersicht – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
ms.technology: entity-framework-core
uid: core/index
ms.openlocfilehash: 13de9cf98111b8e253e073c591fcec04206b4c4f
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="entity-framework-core-quick-overview"></a><span data-ttu-id="f9c66-102">Entity Framework Core – Kurze Übersicht</span><span class="sxs-lookup"><span data-stu-id="f9c66-102">Entity Framework Core Quick Overview</span></span>

<span data-ttu-id="f9c66-103">Entity Framework Core (EF Core) ist eine einfache, erweiterbare und plattformübergreifende Version der beliebten Entity Framework-Datenzugriffstechnologie.</span><span class="sxs-lookup"><span data-stu-id="f9c66-103">Entity Framework (EF) Core is a lightweight, extensible, and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="f9c66-104">EF Core ist eine objektrelationale Zuordnung (Object-Relational Mapper, O/RM), die .NET-Entwicklern mithilfe von .NET-Objekten das Arbeiten mit einer Datenbank ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="f9c66-104">EF Core is an object-relational mapper (O/RM) that enables .NET developers to work with a database using .NET objects.</span></span> <span data-ttu-id="f9c66-105">In EF Core ist der Großteil des Datenzugriffscodes, den Entwickler in der Regel schreiben müssen, nicht mehr erforderlich.</span><span class="sxs-lookup"><span data-stu-id="f9c66-105">It eliminates the need for most of the data-access code that developers usually need to write.</span></span> <span data-ttu-id="f9c66-106">Einzelheiten zu den von EF Core unterstützten Datenbank-Engines finden Sie unter [Datenbankanbieter](providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="f9c66-106">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

<span data-ttu-id="f9c66-107">Wenn Sie das Schreiben von Code erlernen möchten, empfehlen wir Ihnen für den Einstieg in EF Core einen unserer Leitfäden mit den [Ersten Schritten](get-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="f9c66-107">If you like to learn by writing code, we'd recommend one of our [Getting Started](get-started/index.md) guides to get you started with EF Core.</span></span>

## <a name="latest-version-ef-core-20"></a><span data-ttu-id="f9c66-108">Neueste Version: EF Core 2.0</span><span class="sxs-lookup"><span data-stu-id="f9c66-108">Latest version: EF Core 2.0</span></span>

<span data-ttu-id="f9c66-109">Wenn Sie mit EF Core vertraut sind und sich direkt mit den Einzelheiten zur neuen Version auseinandersetzen möchten, helfen Ihnen folgende Ressourcen weiter:</span><span class="sxs-lookup"><span data-stu-id="f9c66-109">If you are familiar with EF Core and want to jump straight into the details of the new version:</span></span>

- <span data-ttu-id="f9c66-110">**[Neue Features in EF Core 2.0](what-is-new/index.md)**</span><span class="sxs-lookup"><span data-stu-id="f9c66-110">**[New features in EF Core 2.0](what-is-new/index.md)**</span></span>
- <span data-ttu-id="f9c66-111">**[Durchführen eines Upgrades für vorhandene Anwendungen auf EF Core 2.0](miscellaneous/1x-2x-upgrade.md)**</span><span class="sxs-lookup"><span data-stu-id="f9c66-111">**[Upgrading existing applications to EF Core 2.0](miscellaneous/1x-2x-upgrade.md)**</span></span>

## <a name="get-entity-framework-core"></a><span data-ttu-id="f9c66-112">Beziehen von Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="f9c66-112">Get Entity Framework Core</span></span>

<span data-ttu-id="f9c66-113">[Installieren Sie das NuGet-Paket](https://docs.nuget.org/ndocs/quickstart/use-a-package) für den gewünschten Datenbankanbieter.</span><span class="sxs-lookup"><span data-stu-id="f9c66-113">[Install the NuGet package](https://docs.nuget.org/ndocs/quickstart/use-a-package) for the database provider you want to use.</span></span> <span data-ttu-id="f9c66-114">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f9c66-114">E.g.</span></span> <span data-ttu-id="f9c66-115">Installieren Sie mit dem `dotnet`-Tool in der Befehlszeile den SQL Server-Anbieter in der plattformübergreifenden Entwicklung:</span><span class="sxs-lookup"><span data-stu-id="f9c66-115">to install the SQL Server provider in cross-platform development using `dotnet` tool in the command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="f9c66-116">Alternativ können Sie die Paket-Manager-Konsole in Visual Studio verwenden:</span><span class="sxs-lookup"><span data-stu-id="f9c66-116">Or in Visual Studio, using the Package Manager Console:</span></span>

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
<span data-ttu-id="f9c66-117">Informationen zu verfügbaren Anbietern finden Sie unter [Datenbankanbieter](providers/index.md) und ausführlichere Beschreibungen der Installationsschritte unter [Installieren von EF Core](get-started/install/index.md).</span><span class="sxs-lookup"><span data-stu-id="f9c66-117">See [Database Providers](providers/index.md) for information on available providers and [Installing EF Core](get-started/install/index.md) for more detailed installation steps.</span></span>

## <a name="the-model"></a><span data-ttu-id="f9c66-118">Das Modell</span><span class="sxs-lookup"><span data-stu-id="f9c66-118">The Model</span></span>

<span data-ttu-id="f9c66-119">Bei EF Core erfolgt der Datenzugriff über ein Modell.</span><span class="sxs-lookup"><span data-stu-id="f9c66-119">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="f9c66-120">Ein Modell setzt sich aus Entitätsklassen und einem abgeleiteten Kontext zusammen, der eine Sitzung bei der Datenbank darstellt und Ihnen das Abfragen und Speichern von Daten ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="f9c66-120">A model is made up of entity classes and a derived context that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="f9c66-121">Weitere Informationen finden Sie unter [Erstellen eines Modells](modeling/index.md).</span><span class="sxs-lookup"><span data-stu-id="f9c66-121">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="f9c66-122">Sie können ein Modell aus einer vorhandenen Datenbank generieren, ein Modell manuell entsprechend Ihrer Datenbank codieren oder mithilfe von EF-Migrationen eine Datenbank anhand Ihres Modells erstellen (und es im Laufe der Zeit entsprechend Ihres Modells weiterentwickeln).</span><span class="sxs-lookup"><span data-stu-id="f9c66-122">You can generate a model from an existing database, hand code a model to match your database, or use EF Migrations to create a database from your model (and evolve it as your model changes over time).</span></span>

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

## <a name="querying"></a><span data-ttu-id="f9c66-123">Abfragen</span><span class="sxs-lookup"><span data-stu-id="f9c66-123">Querying</span></span>

<span data-ttu-id="f9c66-124">Instanzen Ihrer Entitätsklassen werden mit Language Integrated Query (LINQ) von der Datenbank abgerufen.</span><span class="sxs-lookup"><span data-stu-id="f9c66-124">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="f9c66-125">Weitere Informationen finden Sie unter [Abfragen von Daten](querying/index.md).</span><span class="sxs-lookup"><span data-stu-id="f9c66-125">See [Querying Data](querying/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a><span data-ttu-id="f9c66-126">Speichern von Daten</span><span class="sxs-lookup"><span data-stu-id="f9c66-126">Saving Data</span></span>

<span data-ttu-id="f9c66-127">Daten werden in der Datenbank mithilfe von Instanzen Ihrer Entitätsklassen erstellt, gelöscht und geändert.</span><span class="sxs-lookup"><span data-stu-id="f9c66-127">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="f9c66-128">Weitere Informationen finden Sie unter [Speichern von Daten](saving/index.md).</span><span class="sxs-lookup"><span data-stu-id="f9c66-128">See [Saving Data](saving/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
