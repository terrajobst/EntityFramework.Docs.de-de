---
title: Während der Entwurfszeit "DbContext"-Erstellung – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: 648ca990252fb32d8cf181a7ae672d07a81f56bb
ms.sourcegitcommit: 0935ff275ae739243297f5b97eb21414398125c6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2018
ms.locfileid: "39201918"
---
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="19f75-102">Während der Entwurfszeit DbContext-Instanzerstellung</span><span class="sxs-lookup"><span data-stu-id="19f75-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="19f75-103">Einige der Befehle EF Core-Tools (z. B. die [Migrationen] [ 1] Befehle) erfordern eine abgeleitete `DbContext` Instanz zur Entwurfszeit erstellt werden, um Details zu der Anwendung zu sammeln. Entitätstypen und wie sie ein Datenbankschema zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="19f75-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="19f75-104">In den meisten Fällen ist es wünschenswert, die die `DbContext` dadurch erstellten erfolgt auf ähnliche Weise, wie es wäre [zur Laufzeit konfiguriert][2].</span><span class="sxs-lookup"><span data-stu-id="19f75-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="19f75-105">Es gibt verschiedene Möglichkeiten, versuchen die Tools zum Erstellen, der `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="19f75-105">There are various ways the tools try to create the `DbContext`:</span></span>

<a name="from-application-services"></a><span data-ttu-id="19f75-106">Von Application-Dienste</span><span class="sxs-lookup"><span data-stu-id="19f75-106">From application services</span></span>
-------------------------
<span data-ttu-id="19f75-107">Wenn Startprojekt einer ASP.NET Core-app ist, werden diese Tools ausprobieren, um das DbContext-Objekt von der Anwendung Service-Anbieter zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="19f75-107">If your startup project is an ASP.NET Core app, the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="19f75-108">Das Tool zunächst durch Aufrufen den Dienstanbieter abzurufenden `Program.BuildWebHost()` und den Zugriff auf die `IWebHost.Services` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="19f75-108">The tool first try to obtain the service provider by invoking `Program.BuildWebHost()` and accessing the `IWebHost.Services` property.</span></span>

> [!NOTE]
> <span data-ttu-id="19f75-109">Wenn Sie eine neue ASP.NET Core 2.0-Anwendung erstellen, ist dieser Hook standardmäßig enthalten.</span><span class="sxs-lookup"><span data-stu-id="19f75-109">When you create a new ASP.NET Core 2.0 application, this hook is included by default.</span></span> <span data-ttu-id="19f75-110">In früheren Versionen von EF Core und ASP.NET Core, versuchen die Tools, aufzurufen `Startup.ConfigureServices` direkt um Dienstanbieter der Anwendung, aber dieses Muster erhalten nicht mehr ordnungsgemäß in ASP.NET Core 2.0-Anwendungen funktioniert.</span><span class="sxs-lookup"><span data-stu-id="19f75-110">In previous versions of EF Core and ASP.NET Core, the tools try to invoke `Startup.ConfigureServices` directly in order to obtain the application's service provider, but this pattern no longer works correctly in ASP.NET Core 2.0 applications.</span></span> <span data-ttu-id="19f75-111">Wenn Sie ASP.NET Core 1.x-Anwendungen auf 2.0 aktualisieren, können Sie [ändern Ihre `Program` Klasse, um dem neuen Muster folgen][3].</span><span class="sxs-lookup"><span data-stu-id="19f75-111">If you are upgrading an ASP.NET Core 1.x application to 2.0, you can [modify your `Program` class to follow the new pattern][3].</span></span>

<span data-ttu-id="19f75-112">Die `DbContext` selbst und alle Abhängigkeiten in seinem Konstruktor als Dienste in der Anwendung Dienstanbieter registriert werden müssen.</span><span class="sxs-lookup"><span data-stu-id="19f75-112">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="19f75-113">Ganz einfach dafür, dass [ein Konstruktor für die `DbContext` , akzeptiert eine Instanz von `DbContextOptions<TContext>` als Argument] [ 4] und Verwenden der [ `AddDbContext<TContext>` -Methode] [5].</span><span class="sxs-lookup"><span data-stu-id="19f75-113">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as an argument][4] and using the [`AddDbContext<TContext>` method][5].</span></span>

<a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="19f75-114">Mithilfe eines Konstruktors ohne Parameter</span><span class="sxs-lookup"><span data-stu-id="19f75-114">Using a constructor with no parameters</span></span>
--------------------------------------
<span data-ttu-id="19f75-115">Wenn "DbContext" aus der Dienstanbieter für die Anwendung abgerufen werden kann, suchen Sie die Tools für die abgeleitete `DbContext` Typ innerhalb des Projekts.</span><span class="sxs-lookup"><span data-stu-id="19f75-115">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="19f75-116">Versuchen sie, um eine Instanz mit einem Konstruktor ohne Parameter erstellen.</span><span class="sxs-lookup"><span data-stu-id="19f75-116">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="19f75-117">Der Standardkonstruktor kann sein, wenn die `DbContext` erfolgt über die [ `OnConfiguring` ] [ 6] Methode.</span><span class="sxs-lookup"><span data-stu-id="19f75-117">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][6] method.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="19f75-118">Aus einer Factory zur Entwurfszeit</span><span class="sxs-lookup"><span data-stu-id="19f75-118">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="19f75-119">Sie können auch erkennen die Tools wie Ihr "DbContext" Implementieren der `IDesignTimeDbContextFactory<TContext>` Schnittstelle: Wenn eine Klasse, die diese Schnittstelle implementiert, entweder in der gleichen Projekt wie das abgeleitete gefunden wird `DbContext` oder in der Anwendung Startprojekt, umgehen Sie die Tools Weitere Möglichkeiten erstellen Sie stattdessen die "DbContext" und die Verwendung der Entwurfszeit-Factory.</span><span class="sxs-lookup"><span data-stu-id="19f75-119">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Design;
using Microsoft.EntityFrameworkCore.Infrastructure;

namespace MyProject
{
    public class BloggingContextFactory : IDesignTimeDbContextFactory<BloggingContext>
    {
        public BloggingContext CreateDbContext(string[] args)
        {
            var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
            optionsBuilder.UseSqlite("Data Source=blog.db");

            return new BloggingContext(optionsBuilder.Options);
        }
    }
}
```

> [!NOTE]
> <span data-ttu-id="19f75-120">Die `args` Parameter wird derzeit nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="19f75-120">The `args` parameter is currently unused.</span></span> <span data-ttu-id="19f75-121">Es gibt [ein Problem] [ 7] verfolgen die Möglichkeit, die während der Entwurfszeit Argumente über die Tools angeben.</span><span class="sxs-lookup"><span data-stu-id="19f75-121">There is [an issue][7] tracking the ability to specify design-time arguments from the tools.</span></span>

<span data-ttu-id="19f75-122">Eine Entwurfszeit-Factory kann besonders hilfreich, wenn Sie die "DbContext" für die Entwurfszeit als zur Laufzeit unterschiedlich zu konfigurieren, wenn möchten sein der `DbContext` Attributkonstruktor verwendet, die zusätzliche Parameter sind nicht im DI registriert, wenn Sie DI nicht zu verwenden oder, wenn für einige Grund möchten nicht, dass eine `BuildWebHost` -Methode in Ihrer ASP.NET Core-Anwendung `Main` Klasse.</span><span class="sxs-lookup"><span data-stu-id="19f75-122">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's `Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
