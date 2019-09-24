---
title: Erstellung von dbcontext zur Entwurfszeit EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: f83d4b16227d114a1cac1514667484a908fea4ac
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197579"
---
<a name="design-time-dbcontext-creation"></a>DbContext-Instanzerstellung zur Entwurfszeit
==============================
Einige der EF Core Tools-Befehle (z. b. die [Migrations][1] Befehle) erfordern, `DbContext` dass eine abgeleitete Instanz zur Entwurfszeit erstellt wird, um Details zu den Entitäts Typen der Anwendung und deren Zuordnung zu einem Datenbankschema zu erfassen. In den meisten Fällen ist es wünschenswert, `DbContext` dass die, die erstellt wird, auf ähnliche Weise konfiguriert wird, wie Sie zur [Laufzeit konfiguriert][2]wird.

Es gibt verschiedene Möglichkeiten, die Tools zu erstellen `DbContext`:

<a name="from-application-services"></a>Aus Anwendungsdiensten
-------------------------
Wenn das Startprojekt den [ASP.net Core Webhost][3] oder den [generischen .net Core-Host][4]verwendet, versuchen die Tools, das dbcontext-Objekt vom Dienstanbieter der Anwendung abzurufen.

Die Tools versuchen zunächst, den Dienstanbieter abzurufen `Program.CreateHostBuilder()`, indem Sie aufrufen `Build()`und dann auf die `Services` -Eigenschaft zugreifen.

``` csharp
public class Program
{
    public static void Main(string[] args)
        => CreateHostBuilder(args).Build().Run();

    // EF Core uses this method at design time to access the DbContext
    public static IHostBuilder CreateHostBuilder(string[] args)
        => Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(
                webBuilder => webBuilder.UseStartup<Startup>());
}

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
        => services.AddDbContext<ApplicationDbContext>();
}

public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

> [!NOTE]
> Wenn Sie eine neue ASP.net Core Anwendung erstellen, ist dieser Hook standardmäßig enthalten.

Die `DbContext` selbst und alle Abhängigkeiten im Konstruktor müssen als Dienste im Dienstanbieter der Anwendung registriert werden. Dies kann problemlos erreicht werden, indem [ein Konstruktor für den `DbContext` verwendet wird, der eine `DbContextOptions<TContext>` Instanz von als Argument annimmt][5] und [ `AddDbContext<TContext>` ][6]die-Methode verwendet.

<a name="using-a-constructor-with-no-parameters"></a>Verwenden eines Konstruktors ohne Parameter
--------------------------------------
Wenn dbcontext vom Anwendungs Dienstanbieter nicht abgerufen werden kann, suchen die Tools im Projekt nach dem `DbContext` abgeleiteten Typ. Anschließend wird versucht, eine Instanz mit einem Konstruktor ohne Parameter zu erstellen. Dies kann der Standardkonstruktor sein, `DbContext` wenn mithilfe der [`OnConfiguring`][7] -Methode konfiguriert wird.

<a name="from-a-design-time-factory"></a>Aus einer Entwurfszeit Factory
--------------------------
Sie können die Tools auch darüber informieren, wie Sie dbcontext erstellen, indem `IDesignTimeDbContextFactory<TContext>` Sie die-Schnittstelle implementieren: Wenn eine Klasse, die diese Schnittstelle implementiert, sich im gleichen Projekt wie die `DbContext` abgeleitete oder das Startprojekt der Anwendung befindet, umgehen die Tools die anderen Methoden zum Erstellen von dbcontext und verwenden stattdessen die entwurfszeitfactory.

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
> Der `args` -Parameter wird derzeit nicht verwendet. Es gibt [ein Problem][8] bei der Nachverfolgung der Möglichkeit, Entwurfszeit Argumente aus den Tools anzugeben.

Eine Entwurfszeit Factory kann besonders nützlich sein, wenn Sie dbcontext zur Entwurfszeit anders konfigurieren müssen als zur Laufzeit, wenn der `DbContext` Konstruktor zusätzliche Parameter benötigt, die nicht in di registriert sind, wenn Sie di überhaupt nicht verwenden, oder wenn Sie für einige der Grund dafür ist, dass Sie `BuildWebHost` nicht in der `Main` Klasse der ASP.net Core Anwendung eine Methode haben möchten.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
