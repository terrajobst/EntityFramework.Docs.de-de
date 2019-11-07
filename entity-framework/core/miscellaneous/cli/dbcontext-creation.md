---
title: Erstellung von dbcontext zur Entwurfszeit EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: f44f0648678af5a70e5171d69692bde1c1d5e0eb
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655528"
---
# <a name="design-time-dbcontext-creation"></a><span data-ttu-id="f564a-102">DbContext-Instanzerstellung zur Entwurfszeit</span><span class="sxs-lookup"><span data-stu-id="f564a-102">Design-time DbContext Creation</span></span>

<span data-ttu-id="f564a-103">Einige der EF Core Tools-Befehle (z. b. die [Migrations][1] Befehle) erfordern, dass eine abgeleitete `DbContext` Instanz zur Entwurfszeit erstellt wird, um Details zu den Entitäts Typen der Anwendung und deren Zuordnung zu einem Datenbankschema zu erfassen.</span><span class="sxs-lookup"><span data-stu-id="f564a-103">Some of the EF Core Tools commands (for example, the [Migrations][1] commands) require a derived `DbContext` instance to be created at design time in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="f564a-104">In den meisten Fällen ist es wünschenswert, dass die `DbContext`, die auf diese Weise erstellt wird, auf ähnliche Weise wie Sie zur [Laufzeit][2]konfiguriert wird.</span><span class="sxs-lookup"><span data-stu-id="f564a-104">In most cases, it is desirable that the `DbContext` thereby created is configured in a similar way to how it would be [configured at run time][2].</span></span>

<span data-ttu-id="f564a-105">Es gibt verschiedene Möglichkeiten, wie die Tools versuchen, den `DbContext`zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="f564a-105">There are various ways the tools try to create the `DbContext`:</span></span>

## <a name="from-application-services"></a><span data-ttu-id="f564a-106">Aus Anwendungsdiensten</span><span class="sxs-lookup"><span data-stu-id="f564a-106">From application services</span></span>

<span data-ttu-id="f564a-107">Wenn das Startprojekt den [ASP.net Core Webhost][3] oder den [generischen .net Core-Host][4]verwendet, versuchen die Tools, das dbcontext-Objekt vom Dienstanbieter der Anwendung abzurufen.</span><span class="sxs-lookup"><span data-stu-id="f564a-107">If your startup project uses the [ASP.NET Core Web Host][3] or [.NET Core Generic Host][4], the tools try to obtain the DbContext object from the application's service provider.</span></span>

<span data-ttu-id="f564a-108">Die Tools versuchen zunächst, den Dienstanbieter abzurufen, indem Sie `Program.CreateHostBuilder()`aufrufen, `Build()`aufrufen und dann auf die `Services`-Eigenschaft zugreifen.</span><span class="sxs-lookup"><span data-stu-id="f564a-108">The tools first try to obtain the service provider by invoking `Program.CreateHostBuilder()`, calling `Build()`, then accessing the `Services` property.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/ApplicationService.cs)]

> [!NOTE]
> <span data-ttu-id="f564a-109">Wenn Sie eine neue ASP.net Core Anwendung erstellen, ist dieser Hook standardmäßig enthalten.</span><span class="sxs-lookup"><span data-stu-id="f564a-109">When you create a new ASP.NET Core application, this hook is included by default.</span></span>

<span data-ttu-id="f564a-110">Die `DbContext` selbst und alle Abhängigkeiten im Konstruktor müssen als Dienste im Dienstanbieter der Anwendung registriert werden.</span><span class="sxs-lookup"><span data-stu-id="f564a-110">The `DbContext` itself and any dependencies in its constructor need to be registered as services in the application's service provider.</span></span> <span data-ttu-id="f564a-111">Dies kann problemlos erreicht werden, indem [ein Konstruktor für den `DbContext` verwendet wird, der eine Instanz von `DbContextOptions<TContext>` als Argument annimmt][5] und die [`AddDbContext<TContext>`-Methode][6]verwendet.</span><span class="sxs-lookup"><span data-stu-id="f564a-111">This can be easily achieved by having [a constructor on the `DbContext` that takes an instance of `DbContextOptions<TContext>` as an argument][5] and using the [`AddDbContext<TContext>` method][6].</span></span>

## <a name="using-a-constructor-with-no-parameters"></a><span data-ttu-id="f564a-112">Verwenden eines Konstruktors ohne Parameter</span><span class="sxs-lookup"><span data-stu-id="f564a-112">Using a constructor with no parameters</span></span>

<span data-ttu-id="f564a-113">Wenn dbcontext vom Anwendungs Dienstanbieter nicht abgerufen werden kann, suchen die Tools im Projekt nach dem abgeleiteten `DbContext` Typ.</span><span class="sxs-lookup"><span data-stu-id="f564a-113">If the DbContext can't be obtained from the application service provider, the tools look for the derived `DbContext` type inside the project.</span></span> <span data-ttu-id="f564a-114">Anschließend wird versucht, eine Instanz mit einem Konstruktor ohne Parameter zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="f564a-114">Then they try to create an instance using a constructor with no parameters.</span></span> <span data-ttu-id="f564a-115">Dies kann der Standardkonstruktor sein, wenn die `DbContext` mit der [`OnConfiguring`][7] -Methode konfiguriert wird.</span><span class="sxs-lookup"><span data-stu-id="f564a-115">This can be the default constructor if the `DbContext` is configured using the [`OnConfiguring`][7] method.</span></span>

## <a name="from-a-design-time-factory"></a><span data-ttu-id="f564a-116">Aus einer Entwurfszeit Factory</span><span class="sxs-lookup"><span data-stu-id="f564a-116">From a design-time factory</span></span>

<span data-ttu-id="f564a-117">Sie können den Tools auch mitteilen, wie Sie dbcontext erstellen, indem Sie die `IDesignTimeDbContextFactory<TContext>`-Schnittstelle implementieren: Wenn eine Klasse, die diese Schnittstelle implementiert, sich im selben Projekt befindet wie die abgeleitete `DbContext` oder im Startprojekt der Anwendung, umgehen die Tools die andere Möglichkeiten zum Erstellen von dbcontext und Verwenden der entwurfszeitfactory.</span><span class="sxs-lookup"><span data-stu-id="f564a-117">You can also tell the tools how to create your DbContext by implementing the `IDesignTimeDbContextFactory<TContext>` interface: If a class implementing this interface is found in either the same project as the derived `DbContext` or in the application's startup project, the tools bypass the other ways of creating the DbContext and use the design-time factory instead.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/BloggingContextFactory.cs)]

> [!NOTE]
> <span data-ttu-id="f564a-118">Der `args`-Parameter wird derzeit nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="f564a-118">The `args` parameter is currently unused.</span></span> <span data-ttu-id="f564a-119">Es gibt [ein Problem][8] bei der Nachverfolgung der Möglichkeit, Entwurfszeit Argumente aus den Tools anzugeben.</span><span class="sxs-lookup"><span data-stu-id="f564a-119">There is [an issue][8] tracking the ability to specify design-time arguments from the tools.</span></span>

<span data-ttu-id="f564a-120">Eine Entwurfszeit Factory kann besonders nützlich sein, wenn Sie dbcontext zur Entwurfszeit anders konfigurieren müssen als zur Laufzeit, wenn der `DbContext`-Konstruktor zusätzliche Parameter nicht in di registriert ist, wenn Sie di überhaupt nicht verwenden oder wenn Sie aus irgendeinem Grund Es empfiehlt sich, keine `BuildWebHost`-Methode in der `Main` Klasse Ihrer ASP.net Core Anwendung zu haben.</span><span class="sxs-lookup"><span data-stu-id="f564a-120">A design-time factory can be especially useful if you need to configure the DbContext differently for design time than at run time, if the `DbContext` constructor takes additional parameters are not registered in DI, if you are not using DI at all, or if for some reason you prefer not to have a `BuildWebHost` method in your ASP.NET Core application's `Main` class.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
