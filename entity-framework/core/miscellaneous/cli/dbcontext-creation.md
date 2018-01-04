---
title: Zur Entwurfszeit DbContext Erstellung - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: a899c474cc45437bff7c82ce5bddeb915b15c3b0
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/22/2017
---
<a name="design-time-dbcontext-creation"></a>Zur Entwurfszeit DbContext-Erstellung
==============================
Einige der Befehle EF-Core-Tools (z. B. die [Migrationen] [ 1] Befehle) erfordern ein abgeleiteter `DbContext` Instanz zur Entwurfszeit erstellt werden, um Informationen über die Anwendung sammeln Entitätstypen und deren Zuordnung zum eines Datenbankschemas. In den meisten Fällen ist es wünschenswert, die `DbContext` dadurch erstellten konfiguriert ist, auf ähnliche Weise, wie es wäre [zur Laufzeit konfiguriert][2].

Es gibt verschiedene Möglichkeiten, versuchen die Tools zum Erstellen, der `DbContext`:

<a name="from-application-services"></a>Von Application-Dienste
-------------------------
Ist Ihr Startprojekt eine ASP.NET Core-app, versuchen Sie die Tools, den DbContext-Objekt von der Anwendung Service-Anbieter abzurufen.

Das Tool zunächst durch Aufrufen den Dienstanbieter abrufen `Program.BuildWebHost()` und den Zugriff auf die `IWebHost.Services` Eigenschaft.

> [!NOTE]
> Wenn Sie eine neue ASP.NET Core 2.0-Anwendung erstellen, ist dieser Hook standardmäßig enthalten. In früheren Versionen von EF Core und ASP.NET Core, versuchen die Tools, aufzurufen `Startup.ConfigureServices` direkt um Dienstanbieter für die Anwendung, aber dieses Muster nicht mehr erhalten ordnungsgemäß in ASP.NET Core 2.0-Anwendungen funktioniert. Wenn Sie eine Anwendung ASP.NET Core 1.x 2.0 aktualisieren, können Sie [Ändern Ihrer `Program` Klasse, um dem neuen Muster folgen][3].

Die `DbContext` müssen sich selbst und alle Abhängigkeiten in seinem Konstruktor als Dienste in der Dienstanbieter für die Anwendung registriert werden. Dies einfach erreichen, indem mit [ein Konstruktor für die `DbContext` , akzeptiert eine Instanz von `DbContextOptions<TContext>` als Argument] [ 4] und mithilfe der [ `AddDbContext<TContext>` -Methode] [5].

<a name="using-a-constructor-with-no-parameters"></a>Verwenden einen Konstruktor ohne Parameter
--------------------------------------
Wenn von der Anwendung-Service-Anbieter ' DbContext ' abgerufen werden kann, suchen Sie die Tools für die abgeleitete `DbContext` Typ innerhalb des Projekts. Wiederholen sie zum Erstellen einer Instanz mit einem Konstruktor ohne Parameter. Der Standardkonstruktor kann sein, wenn die `DbContext` konfiguriert ist, mit der [ `OnConfiguring` ] [ 6] Methode.

<a name="from-a-design-time-factory"></a>Aus einer Factory zur Entwurfszeit
--------------------------
Können die Tools zum Erstellen Ihrer DbContext implementieren auch erkennen die `IDesignTimeDbContextFactory<TContext>` Schnittstelle: Wenn eine Klasse, die diese Schnittstelle implementiert, entweder in der gleichen Projekt wie die abgeleitete gefunden wird `DbContext` oder in der Anwendung Startprojekt fest, die Tools umgehen Weitere Möglichkeiten erstellen Sie stattdessen die DbContext und die Verwendung der Factory zur Entwurfszeit.

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
> Die `args` Parameter wird derzeit nicht verwendet. Es ist [ein Problem] [ 7] verfolgen die Möglichkeit, Argumente in den Tools anzugeben.

Eine Factory zur Entwurfszeit kann besonders hilfreich, wenn Sie zu ' DbContext ' für die Entwurfszeit als zur Laufzeit unterschiedlich konfigurieren, wenn müssen die `DbContext` Konstruktor nimmt zusätzliche Parameter sind nicht in DI, registriert, wenn nicht gar DI verwenden werden, oder, wenn für einige Grund, dass nicht gewünscht eine `BuildWebHost` Methode in der ASP.NET Core-Anwendung  
`Main`-Klasse.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
