---
title: Zur Entwurfszeit DbContext Erstellung - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 5fcd9e362d76127e7acadd9e552ef3ac90967a37
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="7de11-102">Zur Entwurfszeit DbContext-Erstellung</span><span class="sxs-lookup"><span data-stu-id="7de11-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="7de11-103">Einige der Befehle eine DbContext-Instanz zu erstellenden Daten am Entwurf erfordern EF Tools Zeit (z. B. bei der Migrationen Befehle ausführen zu können).</span><span class="sxs-lookup"><span data-stu-id="7de11-103">Some of the EF Tools commands require a DbContext instance to be created at design time (for example, when running Migrations commands).</span></span> <span data-ttu-id="7de11-104">Es gibt verschiedene Möglichkeiten, die die Tools versuchen, ihn zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="7de11-104">There are various ways the tools try to create it.</span></span>

<a name="from-application-services"></a><span data-ttu-id="7de11-105">Von Application-Dienste</span><span class="sxs-lookup"><span data-stu-id="7de11-105">From application services</span></span>
-------------------------
<span data-ttu-id="7de11-106">Ist Ihr Startprojekt eine ASP.NET Core-app, versuchen Sie die Tools, den DbContext-Objekt von der Anwendung Service-Anbieter abzurufen.</span><span class="sxs-lookup"><span data-stu-id="7de11-106">If your startup project is an ASP.NET Core app, the tools try to obtain the DbContext object from the application's service provider.</span></span> <span data-ttu-id="7de11-107">Sie erhalten sie durch den Aufruf `Program.BuildWebHost()` und den Zugriff auf die `IWebHost.Services` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="7de11-107">They obtain it by invoking `Program.BuildWebHost()` and accessing the `IWebHost.Services` property.</span></span> <span data-ttu-id="7de11-108">Mit jeder DbContext registriert `IServiceCollection.AddDbContext<TContext>()` gefunden und auf diese Weise erstellt werden kann.</span><span class="sxs-lookup"><span data-stu-id="7de11-108">Any DbContext registered using `IServiceCollection.AddDbContext<TContext>()` can be found and created this way.</span></span> <span data-ttu-id="7de11-109">Dieses Muster wurde [in ASP.NET Core 2.0 eingeführt][1]</span><span class="sxs-lookup"><span data-stu-id="7de11-109">This pattern was [introduced in ASP.NET Core 2.0][1]</span></span>

<a name="using-the-default-constructor"></a><span data-ttu-id="7de11-110">Mithilfe des Standardkonstruktors</span><span class="sxs-lookup"><span data-stu-id="7de11-110">Using the default constructor</span></span>
-----------------------------
<span data-ttu-id="7de11-111">Wenn von der Anwendung-Service-Anbieter ' DbContext ' abgerufen werden kann, suchen Sie die Tools für den DbContext-Typ innerhalb des Projekts ein.</span><span class="sxs-lookup"><span data-stu-id="7de11-111">If the DbContext can't be obtained from the application service provider, the tools look for the DbContext type inside the project.</span></span> <span data-ttu-id="7de11-112">Sie versuchen, ihn mit seinem Standardkonstruktor erstellen.</span><span class="sxs-lookup"><span data-stu-id="7de11-112">They try to create it using its default constructor.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="7de11-113">Aus einer Factory zur Entwurfszeit</span><span class="sxs-lookup"><span data-stu-id="7de11-113">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="7de11-114">Können die Tools zum Erstellen Ihrer DbContext implementieren auch erkennen `IDesignTimeDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="7de11-114">You can also tell the tools how to create your DbContext by implementing `IDesignTimeDbContextFactory`.</span></span> <span data-ttu-id="7de11-115">Wenn eine Klasse, die diese Schnittstelle implementiert, innerhalb des Projekts gefunden wird, umgehen die Tools weitere Möglichkeiten zum Erstellen von ' DbContext ' aus.</span><span class="sxs-lookup"><span data-stu-id="7de11-115">If a class implementing this interface is found inside your project, the tools bypass the other ways of creating the DbContext.</span></span>
<span data-ttu-id="7de11-116">Zur Entwurfszeit wird stets der Factory verwendet.</span><span class="sxs-lookup"><span data-stu-id="7de11-116">They always use the factory at design time.</span></span> <span data-ttu-id="7de11-117">Eine Factory ist besonders nützlich, wenn Sie ' DbContext ' für die Entwurfszeit als zur Laufzeit unterschiedlich konfigurieren müssen.</span><span class="sxs-lookup"><span data-stu-id="7de11-117">A factory is especially useful if you need to configure the DbContext differently for design time than at runtime.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
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
> <span data-ttu-id="7de11-118">Die `args` Parameter wird derzeit nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="7de11-118">The `args` parameter is currently unused.</span></span> <span data-ttu-id="7de11-119">Es ist [ein Problem] [ 2] verfolgen die Möglichkeit, Argumente in den Tools anzugeben.</span><span class="sxs-lookup"><span data-stu-id="7de11-119">There is [an issue][2] tracking the ability to specify design-time arguments from the tools.</span></span>

  [1]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [2]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
