---
title: Zur Entwurfszeit DbContext Erstellung - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: 8b38d300d31038bdf5cd877aa3c42b7f5f97eabc
ms.sourcegitcommit: 7113e8675f26cbb546200824512078bf360225df
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
ms.locfileid: "30202483"
---
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="38ebb-102">Zur Entwurfszeit DbContext-Erstellung</span><span class="sxs-lookup"><span data-stu-id="38ebb-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="38ebb-103">Einige der Befehle EF-Core-Tools (z. B. die [Migrationen] [ 1] Befehle) erfordern ein abgeleiteter `DbContext` Instanz zur Entwurfszeit erstellt werden, um Informationen über die Anwendung sammeln Entitätstypen und deren Zuordnung zum eines Datenbankschemas.</span><span class="sxs-lookup"><span data-stu-id="38ebb-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="38ebb-104">In den meisten Fällen ist es wünschenswert, die `DbContext` dadurch erstellten konfiguriert ist, auf ähnliche Weise, wie es wäre [zur Laufzeit konfiguriert][2].</span><span class="sxs-lookup"><span data-stu-id="38ebb-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="38ebb-105">Es gibt verschiedene Möglichkeiten, versuchen die Tools zum Erstellen, der `DbContext`:</span><span class="sxs-lookup"><span data-stu-id="38ebb-105">There are various ways the tools try to create the `DbContext`:</span></span>

<a name="from-application-services"></a><span data-ttu-id="38ebb-106">Von Application-Dienste</span><span class="sxs-lookup"><span data-stu-id="38ebb-106">From application services</span></span>
-------------------------
<span data-ttu-id="38ebb-107">Ist Ihr Startprojekt eine ASP.NET Core-app, versuchen Sie die Tools, den DbContext-Objekt von der Anwendung Service-Anbieter abzurufen.</span><span class="sxs-lookup"><span data-stu-id="38ebb-107">If your startup project is an ASP.NET Core app, the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="38ebb-108">Das Tool zunächst durch Aufrufen den Dienstanbieter abrufen `Program.BuildWebHost()` und den Zugriff auf die `IWebHost.Services` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="38ebb-108">The tool first try to obtain the service provider by invoking `Program.BuildWebHost()` and accessing the `IWebHost.Services` property.</span></span>

> [!NOTE]
> <span data-ttu-id="38ebb-109">Wenn Sie eine neue ASP.NET Core 2.0-Anwendung erstellen, ist dieser Hook standardmäßig enthalten.</span><span class="sxs-lookup"><span data-stu-id="38ebb-109">When you create a new ASP.NET Core 2.0 application, this hook is included by default.</span></span> <span data-ttu-id="38ebb-110">In früheren Versionen von EF Core und ASP.NET Core, versuchen die Tools, aufzurufen `Startup.ConfigureServices` direkt um Dienstanbieter für die Anwendung, aber dieses Muster nicht mehr erhalten ordnungsgemäß in ASP.NET Core 2.0-Anwendungen funktioniert.</span><span class="sxs-lookup"><span data-stu-id="38ebb-110">In previous versions of EF Core and ASP.NET Core, the tools try to invoke `Startup.ConfigureServices` directly in order to obtain the application's service provider, but this pattern no longer works correctly in ASP.NET Core 2.0 applications.</span></span> <span data-ttu-id="38ebb-111">Wenn Sie eine Anwendung ASP.NET Core 1.x 2.0 aktualisieren, können Sie [Ändern Ihrer `Program` Klasse, um dem neuen Muster folgen][3].</span><span class="sxs-lookup"><span data-stu-id="38ebb-111">If you are upgrading an ASP.NET Core 1.x application to 2.0, you can [modify your `Program` class to follow the new pattern][3].</span></span>

<span data-ttu-id="38ebb-112">Die `DbContext` müssen sich selbst und alle Abhängigkeiten in seinem Konstruktor als Dienste in der Dienstanbieter für die Anwendung registriert werden.</span><span class="sxs-lookup"><span data-stu-id="38ebb-112">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="38ebb-113">Dies einfach erreichen, indem mit [ein Konstruktor für die `DbContext` , akzeptiert eine Instanz von `DbContextOptions<TContext>` als Argument] [ 4] und mithilfe der [ `AddDbContext<TContext>` -Methode] [5].</span><span class="sxs-lookup"><span data-stu-id="38ebb-113">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as a argument][4] and using the [`AddDbContext<TContext>` method][5].</span></span>

<a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="38ebb-114">Verwenden einen Konstruktor ohne Parameter</span><span class="sxs-lookup"><span data-stu-id="38ebb-114">Using a constructor with no parameters</span></span>
--------------------------------------
<span data-ttu-id="38ebb-115">Wenn von der Anwendung-Service-Anbieter ' DbContext ' abgerufen werden kann, suchen Sie die Tools für die abgeleitete `DbContext` Typ innerhalb des Projekts.</span><span class="sxs-lookup"><span data-stu-id="38ebb-115">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="38ebb-116">Wiederholen sie zum Erstellen einer Instanz mit einem Konstruktor ohne Parameter.</span><span class="sxs-lookup"><span data-stu-id="38ebb-116">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="38ebb-117">Der Standardkonstruktor kann sein, wenn die `DbContext` konfiguriert ist, mit der [ `OnConfiguring` ] [ 6] Methode.</span><span class="sxs-lookup"><span data-stu-id="38ebb-117">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][6] method.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="38ebb-118">Aus einer Factory zur Entwurfszeit</span><span class="sxs-lookup"><span data-stu-id="38ebb-118">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="38ebb-119">Können die Tools zum Erstellen Ihrer DbContext implementieren auch erkennen die `IDesignTimeDbContextFactory<TContext>` Schnittstelle: Wenn eine Klasse, die diese Schnittstelle implementiert, entweder in der gleichen Projekt wie die abgeleitete gefunden wird `DbContext` oder in der Anwendung Startprojekt fest, die Tools umgehen Weitere Möglichkeiten erstellen Sie stattdessen die DbContext und die Verwendung der Factory zur Entwurfszeit.</span><span class="sxs-lookup"><span data-stu-id="38ebb-119">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

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
> <span data-ttu-id="38ebb-120">Die `args` Parameter wird derzeit nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="38ebb-120">The `args` parameter is currently unused.</span></span> <span data-ttu-id="38ebb-121">Es ist [ein Problem] [ 7] verfolgen die Möglichkeit, Argumente in den Tools anzugeben.</span><span class="sxs-lookup"><span data-stu-id="38ebb-121">There is [an issue][7] tracking the ability to specify design-time arguments from the tools.</span></span>

<span data-ttu-id="38ebb-122">Eine Factory zur Entwurfszeit kann besonders hilfreich, wenn Sie zu ' DbContext ' für die Entwurfszeit als zur Laufzeit unterschiedlich konfigurieren, wenn müssen die `DbContext` Konstruktor nimmt zusätzliche Parameter sind nicht in DI, registriert, wenn nicht gar DI verwenden werden, oder, wenn für einige Grund, dass nicht gewünscht eine `BuildWebHost` Methode in der ASP.NET Core-Anwendung</span><span class="sxs-lookup"><span data-stu-id="38ebb-122">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's</span></span>  
<span data-ttu-id="38ebb-123">`Main`-Klasse.</span><span class="sxs-lookup"><span data-stu-id="38ebb-123">`Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
