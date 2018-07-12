---
title: Kurze Übersicht – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
ms.technology: entity-framework-core
uid: core/index
ms.openlocfilehash: 103e5e069687950a8411f2d92c7b5a191844e0ae
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37948989"
---
# <a name="entity-framework-core-quick-overview"></a><span data-ttu-id="803ae-102">Entity Framework Core – Kurze Übersicht</span><span class="sxs-lookup"><span data-stu-id="803ae-102">Entity Framework Core Quick Overview</span></span>

<span data-ttu-id="803ae-103">Entity Framework Core (EF Core) ist eine einfache, erweiterbare und plattformübergreifende Version der beliebten Entity Framework-Datenzugriffstechnologie.</span><span class="sxs-lookup"><span data-stu-id="803ae-103">Entity Framework (EF) Core is a lightweight, extensible, and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="803ae-104">EF Core kann als objektrelationaler Mapper (O/RM) eingesetzt werden und bietet .NET-Entwicklern so die Möglichkeit, unter Verwendung von .NET-Objekten mit einer Datenbank zu arbeiten. Auf diese Weise entfällt ein Großteil des Datenzugriffscodes, der üblicherweise geschrieben werden muss.</span><span class="sxs-lookup"><span data-stu-id="803ae-104">EF Core can serve as an object-relational mapper (O/RM), enabling .NET developers to work with a database using .NET objects, and eliminating the need for most of the data-access code they usually need to write.</span></span>

<span data-ttu-id="803ae-105">Einzelheiten zu den von EF Core unterstützten Datenbank-Engines finden Sie unter [Datenbankanbieter](providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="803ae-105">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

<span data-ttu-id="803ae-106">Wenn Sie das Schreiben von Code erlernen möchten, empfehlen wir Ihnen für den Einstieg in EF Core einen unserer Leitfäden mit den [Ersten Schritten](get-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="803ae-106">If you like to learn by writing code, we'd recommend one of our [Getting Started](get-started/index.md) guides to get you started with EF Core.</span></span>

## <a name="what-is-new-in-ef-core"></a><span data-ttu-id="803ae-107">Neue Features in EF Core</span><span class="sxs-lookup"><span data-stu-id="803ae-107">What is new in EF Core</span></span>

<span data-ttu-id="803ae-108">Wenn Sie mit EF Core vertraut sind und sich direkt mit den Einzelheiten zu den neuesten Releases auseinandersetzen möchten, helfen Ihnen folgende Ressourcen weiter:</span><span class="sxs-lookup"><span data-stu-id="803ae-108">If you are familiar with EF Core and want to jump straight into the details of the latest releases:</span></span>

- <span data-ttu-id="803ae-109">**[Neue Features in EF Core 2.1](xref:core/what-is-new/ef-core-2.1)**</span><span class="sxs-lookup"><span data-stu-id="803ae-109">**[What is new in EF Core 2.1](xref:core/what-is-new/ef-core-2.1)**</span></span>
- <span data-ttu-id="803ae-110">**[Durchführen eines Upgrades für vorhandene Anwendungen auf EF Core 2.x](xref:core/miscellaneous/1x-2x-upgrade)**</span><span class="sxs-lookup"><span data-stu-id="803ae-110">**[Upgrading existing applications to EF Core 2.x](xref:core/miscellaneous/1x-2x-upgrade)**</span></span>


## <a name="get-entity-framework-core"></a><span data-ttu-id="803ae-111">Beziehen von Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="803ae-111">Get Entity Framework Core</span></span>

<span data-ttu-id="803ae-112">[Installieren Sie das NuGet-Paket](https://docs.nuget.org/ndocs/quickstart/use-a-package) für den gewünschten Datenbankanbieter.</span><span class="sxs-lookup"><span data-stu-id="803ae-112">[Install the NuGet package](https://docs.nuget.org/ndocs/quickstart/use-a-package) for the database provider you want to use.</span></span> <span data-ttu-id="803ae-113">Installieren Sie z.B. mit dem `dotnet`-Tool in der Befehlszeile den SQL Server-Anbieter in der plattformübergreifenden Entwicklung:</span><span class="sxs-lookup"><span data-stu-id="803ae-113">For example, to install the SQL Server provider in cross-platform development using `dotnet` tool in the command line:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="803ae-114">Alternativ können Sie die Paket-Manager-Konsole in Visual Studio verwenden:</span><span class="sxs-lookup"><span data-stu-id="803ae-114">Or in Visual Studio, using the Package Manager Console:</span></span>

``` PowerShell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```
<span data-ttu-id="803ae-115">Informationen zu verfügbaren Anbietern finden Sie unter [Datenbankanbieter](providers/index.md) und ausführlichere Beschreibungen der Installationsschritte unter [Installieren von EF Core](get-started/install/index.md).</span><span class="sxs-lookup"><span data-stu-id="803ae-115">See [Database Providers](providers/index.md) for information on available providers and [Installing EF Core](get-started/install/index.md) for more detailed installation steps.</span></span>

## <a name="the-model"></a><span data-ttu-id="803ae-116">Das Modell</span><span class="sxs-lookup"><span data-stu-id="803ae-116">The Model</span></span>

<span data-ttu-id="803ae-117">Bei EF Core erfolgt der Datenzugriff über ein Modell.</span><span class="sxs-lookup"><span data-stu-id="803ae-117">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="803ae-118">Ein Modell setzt sich aus Entitätsklassen und einem abgeleiteten Kontext zusammen, der eine Sitzung bei der Datenbank darstellt und Ihnen das Abfragen und Speichern von Daten ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="803ae-118">A model is made up of entity classes and a derived context that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="803ae-119">Weitere Informationen finden Sie unter [Erstellen eines Modells](modeling/index.md).</span><span class="sxs-lookup"><span data-stu-id="803ae-119">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="803ae-120">Sie können ein Modell aus einer vorhandenen Datenbank generieren, ein Modell manuell entsprechend Ihrer Datenbank codieren oder mithilfe von EF-Migrationen eine Datenbank anhand Ihres Modells erstellen (und es im Laufe der Zeit entsprechend Ihres Modells weiterentwickeln).</span><span class="sxs-lookup"><span data-stu-id="803ae-120">You can generate a model from an existing database, hand code a model to match your database, or use EF Migrations to create a database from your model (and evolve it as your model changes over time).</span></span>

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

## <a name="querying"></a><span data-ttu-id="803ae-121">Abfragen</span><span class="sxs-lookup"><span data-stu-id="803ae-121">Querying</span></span>

<span data-ttu-id="803ae-122">Instanzen Ihrer Entitätsklassen werden mit Language Integrated Query (LINQ) von der Datenbank abgerufen.</span><span class="sxs-lookup"><span data-stu-id="803ae-122">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="803ae-123">Weitere Informationen finden Sie unter [Abfragen von Daten](querying/index.md).</span><span class="sxs-lookup"><span data-stu-id="803ae-123">See [Querying Data](querying/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blogs = db.Blogs
        .Where(b => b.Rating > 3)
        .OrderBy(b => b.Url)
        .ToList();
}
```

## <a name="saving-data"></a><span data-ttu-id="803ae-124">Speichern von Daten</span><span class="sxs-lookup"><span data-stu-id="803ae-124">Saving Data</span></span>

<span data-ttu-id="803ae-125">Daten werden in der Datenbank mithilfe von Instanzen Ihrer Entitätsklassen erstellt, gelöscht und geändert.</span><span class="sxs-lookup"><span data-stu-id="803ae-125">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="803ae-126">Weitere Informationen finden Sie unter [Speichern von Daten](saving/index.md).</span><span class="sxs-lookup"><span data-stu-id="803ae-126">See [Saving Data](saving/index.md) to learn more.</span></span>

``` csharp
using (var db = new BloggingContext())
{
    var blog = new Blog { Url = "http://sample.com" };
    db.Blogs.Add(blog);
    db.SaveChanges();
}
```
