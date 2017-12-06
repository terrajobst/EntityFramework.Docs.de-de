---
title: "Aktualisieren von EF Core 1.0 RC1 auf RC2 – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
ms.technology: entity-framework-core
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: ae5077c30642e3f40f51adee429821978f194460
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a><span data-ttu-id="860c3-102">Aktualisieren von EF Core 1.0 RC1 auf RC2 1.0</span><span class="sxs-lookup"><span data-stu-id="860c3-102">Upgrading from EF Core 1.0 RC1 to 1.0 RC2</span></span>

<span data-ttu-id="860c3-103">Dieser Artikel enthält die Anleitung zum Verschieben einer Anwendung, die mit den Paketen RC1 auf RC2 erstellt.</span><span class="sxs-lookup"><span data-stu-id="860c3-103">This article provides guidance for moving an application built with the RC1 packages to RC2.</span></span>

## <a name="package-names-and-versions"></a><span data-ttu-id="860c3-104">Paketnamen und Versionen</span><span class="sxs-lookup"><span data-stu-id="860c3-104">Package Names and Versions</span></span>

<span data-ttu-id="860c3-105">Zwischen RC1 und RC2 geändert von "Entity Framework 7", "Entity Framework Core".</span><span class="sxs-lookup"><span data-stu-id="860c3-105">Between RC1 and RC2, we changed from "Entity Framework 7" to "Entity Framework Core".</span></span> <span data-ttu-id="860c3-106">Erfahren Sie mehr über die Gründe für die Änderung der [diesen Beitrag von Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span><span class="sxs-lookup"><span data-stu-id="860c3-106">You can read more about the reasons for the change in [this post by Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span></span> <span data-ttu-id="860c3-107">Aufgrund dieser Änderung unserer Paketnamen geändert `EntityFramework.*` auf `Microsoft.EntityFrameworkCore.*` und unsere-Versionen `7.0.0-rc1-final` auf `1.0.0-rc2-final` (oder `1.0.0-preview1-final` für Tools).</span><span class="sxs-lookup"><span data-stu-id="860c3-107">Because of this change, our package names changed from `EntityFramework.*` to `Microsoft.EntityFrameworkCore.*` and our versions from `7.0.0-rc1-final` to `1.0.0-rc2-final` (or `1.0.0-preview1-final` for tooling).</span></span>

<span data-ttu-id="860c3-108">**Sie müssen die Pakete RC1 vollständig zu entfernen und installieren Sie die RC2 verfügen.**</span><span class="sxs-lookup"><span data-stu-id="860c3-108">**You will need to completely remove the RC1 packages and then install the RC2 ones.**</span></span> <span data-ttu-id="860c3-109">Hier ist die Zuordnung für einige allgemeine Pakete aus.</span><span class="sxs-lookup"><span data-stu-id="860c3-109">Here is the mapping for some common packages.</span></span>

| <span data-ttu-id="860c3-110">RC1-Paket</span><span class="sxs-lookup"><span data-stu-id="860c3-110">RC1 Package</span></span>                                               | <span data-ttu-id="860c3-111">RC2-Entsprechung</span><span class="sxs-lookup"><span data-stu-id="860c3-111">RC2 Equivalent</span></span>                                                       |
| --------------------------------------------------------- | -------------------------------------------------------------------- |
| <span data-ttu-id="860c3-112">EntityFramework.MicrosoftSqlServer 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="860c3-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span></span> | <span data-ttu-id="860c3-113">Microsoft.EntityFrameworkCore.SqlServer 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="860c3-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="860c3-114">EntityFramework.SQLite 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="860c3-114">EntityFramework.SQLite                    7.0.0-rc1-final</span></span> | <span data-ttu-id="860c3-115">Microsoft.EntityFrameworkCore.Sqlite 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="860c3-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="860c3-116">EntityFramework7.Npgsql 3.1.0-rc1-3</span><span class="sxs-lookup"><span data-stu-id="860c3-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span></span>     | <span data-ttu-id="860c3-117">NpgSql.EntityFrameworkCore.Postgres<to be advised></span><span class="sxs-lookup"><span data-stu-id="860c3-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span></span>      |
| <span data-ttu-id="860c3-118">EntityFramework.SqlServerCompact35 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="860c3-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span></span> | <span data-ttu-id="860c3-119">EntityFrameworkCore.SqlServerCompact35 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="860c3-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="860c3-120">EntityFramework.SqlServerCompact40 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="860c3-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span></span> | <span data-ttu-id="860c3-121">EntityFrameworkCore.SqlServerCompact40 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="860c3-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="860c3-122">EntityFramework.InMemory 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="860c3-122">EntityFramework.InMemory                  7.0.0-rc1-final</span></span> | <span data-ttu-id="860c3-123">Microsoft.EntityFrameworkCore.InMemory 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="860c3-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="860c3-124">EntityFramework.IBMDataServer 7.0.0-beta1</span><span class="sxs-lookup"><span data-stu-id="860c3-124">EntityFramework.IBMDataServer             7.0.0-beta1</span></span>     | <span data-ttu-id="860c3-125">Für RC2 noch nicht verfügbar</span><span class="sxs-lookup"><span data-stu-id="860c3-125">Not yet available for RC2</span></span>                                            |
| <span data-ttu-id="860c3-126">EntityFramework.Commands 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="860c3-126">EntityFramework.Commands                  7.0.0-rc1-final</span></span> | <span data-ttu-id="860c3-127">Microsoft.EntityFrameworkCore.Tools 1.0.0-preview1-final</span><span class="sxs-lookup"><span data-stu-id="860c3-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span></span> |
| <span data-ttu-id="860c3-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="860c3-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span></span> | <span data-ttu-id="860c3-129">Microsoft.EntityFrameworkCore.SqlServer.Design 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="860c3-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span></span>      |

## <a name="namespaces"></a><span data-ttu-id="860c3-130">Namespaces</span><span class="sxs-lookup"><span data-stu-id="860c3-130">Namespaces</span></span>

<span data-ttu-id="860c3-131">Zusammen mit den Paketnamen, Namespaces, die von geändert `Microsoft.Data.Entity.*` auf `Microsoft.EntityFrameworkCore.*`.</span><span class="sxs-lookup"><span data-stu-id="860c3-131">Along with package names, namespaces changed from `Microsoft.Data.Entity.*` to `Microsoft.EntityFrameworkCore.*`.</span></span> <span data-ttu-id="860c3-132">Sie können diese Änderung mit einem Suchen/Ersetzen von behandeln `using Microsoft.Data.Entity` mit `using Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="860c3-132">You can handle this change with a find/replace of `using Microsoft.Data.Entity` with `using Microsoft.EntityFrameworkCore`.</span></span>

## <a name="table-naming-convention-changes"></a><span data-ttu-id="860c3-133">Tabelle Naming Convention-Änderungen</span><span class="sxs-lookup"><span data-stu-id="860c3-133">Table Naming Convention Changes</span></span>

<span data-ttu-id="860c3-134">Eine wesentliche funktionale Änderung, die wir in RC2 dauerte wurde auf den Namen der `DbSet<TEntity>` -Eigenschaft für eine bestimmte Entität als Namen der Tabelle zugeordnet, anstatt nur der Klassenname.</span><span class="sxs-lookup"><span data-stu-id="860c3-134">A significant functional change we took in RC2 was to use the name of the `DbSet<TEntity>` property for a given entity as the table name it maps to, rather than just the class name.</span></span> <span data-ttu-id="860c3-135">Erfahren Sie mehr über diese Änderung in [das verwandte Ankündigung Problem](https://github.com/aspnet/Announcements/issues/167).</span><span class="sxs-lookup"><span data-stu-id="860c3-135">You can read more about this change in [the related announcement issue](https://github.com/aspnet/Announcements/issues/167).</span></span>

<span data-ttu-id="860c3-136">Bei vorhandenen RC1-Anwendungen sollten den folgenden Code am Anfang des Hinzufügen Ihrer `OnModelCreating` Methode, die RC1 Benennungsstrategie beizubehalten:</span><span class="sxs-lookup"><span data-stu-id="860c3-136">For existing RC1 applications, we recommend adding the following code to the start of your `OnModelCreating` method to keep the RC1 naming strategy:</span></span>

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

<span data-ttu-id="860c3-137">Wenn Sie die neue Benennungsstrategie übernehmen möchten, würden erfolgreich empfohlen benennt Abschließen der Rest der Upgradeschritte und klicken Sie dann den Code entfernt und erstellen eine Migration aus, um die Tabelle anwenden.</span><span class="sxs-lookup"><span data-stu-id="860c3-137">If you want to adopt the new naming strategy, we would recommend successfully completing the rest of the upgrade steps and then removing the code and creating a migration to apply the table renames.</span></span>

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a><span data-ttu-id="860c3-138">AddDbContext / Startup.cs ändert (gilt nur für ASP.NET Core-Projekte)</span><span class="sxs-lookup"><span data-stu-id="860c3-138">AddDbContext / Startup.cs Changes (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="860c3-139">In RC1, mussten Sie Entity Framework-Dienste in der Anwendung-Dienstanbieter - hinzufügen `Startup.ConfigureServices(...)`:</span><span class="sxs-lookup"><span data-stu-id="860c3-139">In RC1, you had to add Entity Framework services to the application service provider - in `Startup.ConfigureServices(...)`:</span></span>

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="860c3-140">In RC2, entfernen Sie die Aufrufe von `AddEntityFramework()`, `AddSqlServer()`usw..:</span><span class="sxs-lookup"><span data-stu-id="860c3-140">In RC2, you can remove the calls to `AddEntityFramework()`, `AddSqlServer()`, etc.:</span></span>

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="860c3-141">Sie müssen auch einen Konstruktor den abgeleiteten Kontext hinzufügen, nimmt die Kontextoptionen und übergibt sie an den Basiskonstruktor.</span><span class="sxs-lookup"><span data-stu-id="860c3-141">You also need to add a constructor, to your derived context, that takes context options and passes them to the base constructor.</span></span> <span data-ttu-id="860c3-142">Dies ist erforderlich, da wir einige furchterregend Magic entfernt, die sie in den Hintergrund eingeschmuggelt habe:</span><span class="sxs-lookup"><span data-stu-id="860c3-142">This is needed because we removed some of the scary magic that snuck them in behind the scenes:</span></span>

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a><span data-ttu-id="860c3-143">Übergabe eines IServiceProvider</span><span class="sxs-lookup"><span data-stu-id="860c3-143">Passing in an IServiceProvider</span></span>

<span data-ttu-id="860c3-144">Wenn Sie RC1-Code verfügen, der übergibt ein `IServiceProvider` an den Kontext, dies jetzt verschoben wurde `DbContextOptions`, anstatt eine separate Konstruktorparameter.</span><span class="sxs-lookup"><span data-stu-id="860c3-144">If you have RC1 code that passes an `IServiceProvider` to the context, this has now moved to `DbContextOptions`, rather than being a separate constructor parameter.</span></span> <span data-ttu-id="860c3-145">Verwendung `DbContextOptionsBuilder.UseInternalServiceProvider(...)` den Dienstanbieter festgelegt.</span><span class="sxs-lookup"><span data-stu-id="860c3-145">Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` to set the service provider.</span></span>

### <a name="testing"></a><span data-ttu-id="860c3-146">Test</span><span class="sxs-lookup"><span data-stu-id="860c3-146">Testing</span></span>

<span data-ttu-id="860c3-147">Das häufigste Szenario für diese Vorgehensweise wurde um den Bereich einer InMemory-Datenbank steuern, beim Testen.</span><span class="sxs-lookup"><span data-stu-id="860c3-147">The most common scenario for doing this was to control the scope of an InMemory database when testing.</span></span> <span data-ttu-id="860c3-148">Finden Sie unter den aktualisierten [Test](testing/index.md) Artikel ein Beispiel hierfür RC2.</span><span class="sxs-lookup"><span data-stu-id="860c3-148">See the updated [Testing](testing/index.md) article for an example of doing this with RC2.</span></span>

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a><span data-ttu-id="860c3-149">Zum Auflösen von internen Diensten aus Application Service Provider (gilt nur für ASP.NET Core-Projekte)</span><span class="sxs-lookup"><span data-stu-id="860c3-149">Resolving Internal Services from Application Service Provider (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="860c3-150">Wenn Sie eine ASP.NET Core-Anwendung und EF zum Auflösen der internen Dienste vom Anwendungsdienstanbieter angezeigt werden sollen, ist eine Überladung der `AddDbContext` , mit der Sie zum Konfigurieren dieser:</span><span class="sxs-lookup"><span data-stu-id="860c3-150">If you have an ASP.NET Core application and you want EF to resolve internal services from the application service provider, there is an overload of `AddDbContext` that allows you to configure this:</span></span>

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> <span data-ttu-id="860c3-151">Es wird empfohlen, EF zur Verwaltung von intern einen eigenen Diensten ermöglichen, es sei denn, Sie einen Grund haben für die interne EF-Dienste in Ihrer Anwendungsdienstanbieter kombinieren.</span><span class="sxs-lookup"><span data-stu-id="860c3-151">We recommend allowing EF to internally manage its own services, unless you have a reason to combine the internal EF services into your application service provider.</span></span> <span data-ttu-id="860c3-152">Die Hauptgründe dafür, dass Sie dies tun möchten wird an Ihren Dienstanbieter, Anwendung verwenden, um Dienste zu ersetzen, die EF intern verwendet</span><span class="sxs-lookup"><span data-stu-id="860c3-152">The main reason you may want to do this is to use your application service provider to replace services that EF uses internally</span></span>

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a><span data-ttu-id="860c3-153">DNX-Befehle = > .NET CLI (gilt nur für ASP.NET Core-Projekte)</span><span class="sxs-lookup"><span data-stu-id="860c3-153">DNX Commands => .NET CLI (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="860c3-154">Wenn Sie zuvor verwendet die `dnx ef` -Befehle für ASP.NET 5-Projekte, diese haben jetzt verschoben `dotnet ef` Befehle.</span><span class="sxs-lookup"><span data-stu-id="860c3-154">If you previously used the `dnx ef` commands for ASP.NET 5 projects, these have now moved to `dotnet ef` commands.</span></span> <span data-ttu-id="860c3-155">Die gleichen Befehlssyntax gilt weiterhin.</span><span class="sxs-lookup"><span data-stu-id="860c3-155">The same command syntax still applies.</span></span> <span data-ttu-id="860c3-156">Sie können `dotnet ef --help` Informationen zur Syntax.</span><span class="sxs-lookup"><span data-stu-id="860c3-156">You can use `dotnet ef --help` for syntax information.</span></span>

<span data-ttu-id="860c3-157">Die Möglichkeit, die Befehle registriert sind, wurde in RC2 aufgrund von DNX mit .NET CLI ersetzt wird geändert.</span><span class="sxs-lookup"><span data-stu-id="860c3-157">The way commands are registered has changed in RC2, due to DNX being replaced by .NET CLI.</span></span> <span data-ttu-id="860c3-158">Befehle sind nun registriert, einem `tools` im Abschnitt `project.json`:</span><span class="sxs-lookup"><span data-stu-id="860c3-158">Commands are now registered in a `tools` section in `project.json`:</span></span>

``` json
"tools": {
  "Microsoft.EntityFrameworkCore.Tools": {
    "version": "1.0.0-preview1-final",
    "imports": [
      "portable-net45+win8+dnxcore50",
      "portable-net45+win8"
    ]
  }
}
```

> [!TIP]  
> <span data-ttu-id="860c3-159">Wenn Sie Visual Studio verwenden, können Sie jetzt Paket-Manager-Konsole verwenden, EF-Befehle für ASP.NET Core Projekte ausführen (Dies wurde nicht in RC1 unterstützt).</span><span class="sxs-lookup"><span data-stu-id="860c3-159">If you use Visual Studio, you can now use Package Manager Console to run EF commands for ASP.NET Core projects (this was not supported in RC1).</span></span> <span data-ttu-id="860c3-160">Müssen Sie immer noch die Befehle in Registrieren der `tools` Abschnitt `project.json` dazu.</span><span class="sxs-lookup"><span data-stu-id="860c3-160">You still need to register the commands in the `tools` section of `project.json` to do this.</span></span>

## <a name="package-manager-commands-require-powershell-5"></a><span data-ttu-id="860c3-161">Paket-Manager-Befehle erfordern PowerShell 5</span><span class="sxs-lookup"><span data-stu-id="860c3-161">Package Manager Commands Require PowerShell 5</span></span>

<span data-ttu-id="860c3-162">Wenn Sie die Entity Framework-Befehle in der Paket-Manager-Konsole in Visual Studio verwenden, müssen Sie sicherstellen, dass Sie PowerShell 5 installiert ist.</span><span class="sxs-lookup"><span data-stu-id="860c3-162">If you use the Entity Framework commands in Package Manager Console in Visual Studio, then you will need to ensure you have PowerShell 5 installed.</span></span> <span data-ttu-id="860c3-163">Dies ist eine temporäre Anforderung, die in der nächsten Version entfernt wird (finden Sie unter [ausstellen #5327](https://github.com/aspnet/EntityFramework/issues/5327) Weitere Details).</span><span class="sxs-lookup"><span data-stu-id="860c3-163">This is a temporary requirement that will be removed in the next release (see [issue #5327](https://github.com/aspnet/EntityFramework/issues/5327) for more details).</span></span>

## <a name="using-imports-in-projectjson"></a><span data-ttu-id="860c3-164">Verwenden von "Imports" in "Project.JSON"</span><span class="sxs-lookup"><span data-stu-id="860c3-164">Using "imports" in project.json</span></span>

<span data-ttu-id="860c3-165">Einige der EF kernabhängigkeiten unterstützt .NET Standard noch nicht.</span><span class="sxs-lookup"><span data-stu-id="860c3-165">Some of EF Core's dependencies do not support .NET Standard yet.</span></span> <span data-ttu-id="860c3-166">Hinzufügen von "imports" auf "Project.JSON" als vorübergehende problemumgehung ist EF Core in standardmäßigen .NET und .NET Core-Projekte erfordern.</span><span class="sxs-lookup"><span data-stu-id="860c3-166">EF Core in .NET Standard and .NET Core projects may require adding "imports" to project.json as a temporary workaround.</span></span>

<span data-ttu-id="860c3-167">Beim Hinzufügen von EF wird NuGet Restore diese Fehlermeldung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="860c3-167">When adding EF, NuGet restore will display this error message:</span></span>

``` Console
Package Ix-Async 1.2.5 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Ix-Async 1.2.5 supports:
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8 (.NETPortable,Version=v0.0,Profile=Profile78)
Package Remotion.Linq 2.0.2 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Remotion.Linq 2.0.2 supports:
  - net35 (.NETFramework,Version=v3.5)
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8+wpa81 (.NETPortable,Version=v0.0,Profile=Profile259)
```

<span data-ttu-id="860c3-168">Die problemumgehung besteht darin, die portable Profil "Portable net451 + win8" manuell importieren.</span><span class="sxs-lookup"><span data-stu-id="860c3-168">The workaround is to manually import the portable profile "portable-net451+win8".</span></span> <span data-ttu-id="860c3-169">Diese erzwingt NuGet, diese Binärdateien behandelt werden sollen, die dies entsprechen bieten als kompatibel mit dem standardmäßigen .NET Framework, obwohl sie nicht sind.</span><span class="sxs-lookup"><span data-stu-id="860c3-169">This forces NuGet to treat this binaries that match this provide as a compatible framework with .NET Standard, even though they are not.</span></span> <span data-ttu-id="860c3-170">Obwohl "Portable net451 + win8" nicht 100 % mit .NET Standard kompatibel ist, ist es kompatibel genug für den Übergang vom PCL .NET-Standard.</span><span class="sxs-lookup"><span data-stu-id="860c3-170">Although "portable-net451+win8" is not 100% compatible with .NET Standard, it is compatible enough for the transition from PCL to .NET Standard.</span></span> <span data-ttu-id="860c3-171">Importe können entfernt werden, wenn EF Abhängigkeiten schließlich auf .NET Standard aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="860c3-171">Imports can be removed when EF's dependencies eventually upgrade to .NET Standard.</span></span>

<span data-ttu-id="860c3-172">Mehrere Frameworks können "Imports" in der Arraysyntax hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="860c3-172">Multiple frameworks can be added to "imports" in array syntax.</span></span> <span data-ttu-id="860c3-173">Andere Importe ist möglicherweise erforderlich, wenn Sie zusätzliche Bibliotheken zu Ihrem Projekt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="860c3-173">Other imports may be necessary if you add additional libraries to your project.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

<span data-ttu-id="860c3-174">Finden Sie unter [ausstellen #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span><span class="sxs-lookup"><span data-stu-id="860c3-174">See [Issue #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span></span>
