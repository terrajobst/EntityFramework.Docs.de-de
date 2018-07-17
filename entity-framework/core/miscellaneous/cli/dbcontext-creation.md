---
title: Während der Entwurfszeit "DbContext"-Erstellung – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: 7c16017d3b97d115841050fe6ac0fdbeb5e71d94
ms.sourcegitcommit: 00cb52625b57c1ea339ded1454179fe89b6bcfea
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2018
ms.locfileid: "39067530"
---
<a name="design-time-dbcontext-creation"></a>Während der Entwurfszeit DbContext-Instanzerstellung
==============================
Einige der Befehle EF Core-Tools (z. B. die [Migrationen] [ 1] Befehle) erfordern eine abgeleitete `DbContext` Instanz zur Entwurfszeit erstellt werden, um Details zu der Anwendung zu sammeln. Entitätstypen und wie sie ein Datenbankschema zugeordnet. In den meisten Fällen ist es wünschenswert, die die `DbContext` dadurch erstellten erfolgt auf ähnliche Weise, wie es wäre [zur Laufzeit konfiguriert][2].

Es gibt verschiedene Möglichkeiten, versuchen die Tools zum Erstellen, der `DbContext`:

<a name="from-application-services"></a>Von Application-Dienste
-------------------------
Wenn Startprojekt einer ASP.NET Core-app ist, werden diese Tools ausprobieren, um das DbContext-Objekt von der Anwendung Service-Anbieter zu erhalten.

Das Tool zunächst durch Aufrufen den Dienstanbieter abzurufenden `Program.BuildWebHost()` und den Zugriff auf die `IWebHost.Services` Eigenschaft.

> [!NOTE]
> Wenn Sie eine neue ASP.NET Core 2.0-Anwendung erstellen, ist dieser Hook standardmäßig enthalten. In früheren Versionen von EF Core und ASP.NET Core, versuchen die Tools, aufzurufen `Startup.ConfigureServices` direkt um Dienstanbieter der Anwendung, aber dieses Muster erhalten nicht mehr ordnungsgemäß in ASP.NET Core 2.0-Anwendungen funktioniert. Wenn Sie ASP.NET Core 1.x-Anwendungen auf 2.0 aktualisieren, können Sie [ändern Ihre `Program` Klasse, um dem neuen Muster folgen][3].

Die `DbContext` selbst und alle Abhängigkeiten in seinem Konstruktor als Dienste in der Anwendung Dienstanbieter registriert werden müssen. Ganz einfach dafür, dass [ein Konstruktor für die `DbContext` , akzeptiert eine Instanz von `DbContextOptions<TContext>` als Argument] [ 4] und Verwenden der [ `AddDbContext<TContext>` -Methode] [5].

<a name="using-a-constructor-with-no-parameters"></a>Mithilfe eines Konstruktors ohne Parameter
--------------------------------------
Wenn "DbContext" aus der Dienstanbieter für die Anwendung abgerufen werden kann, suchen Sie die Tools für die abgeleitete `DbContext` Typ innerhalb des Projekts. Versuchen sie, um eine Instanz mit einem Konstruktor ohne Parameter erstellen. Der Standardkonstruktor kann sein, wenn die `DbContext` erfolgt über die [ `OnConfiguring` ] [ 6] Methode.

<a name="from-a-design-time-factory"></a>Aus einer Factory zur Entwurfszeit
--------------------------
Sie können auch erkennen die Tools wie Ihr "DbContext" Implementieren der `IDesignTimeDbContextFactory<TContext>` Schnittstelle: Wenn eine Klasse, die diese Schnittstelle implementiert, entweder in der gleichen Projekt wie das abgeleitete gefunden wird `DbContext` oder in der Anwendung Startprojekt, umgehen Sie die Tools Weitere Möglichkeiten erstellen Sie stattdessen die "DbContext" und die Verwendung der Entwurfszeit-Factory.

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
> Die `args` Parameter wird derzeit nicht verwendet. Es gibt [ein Problem] [ 7] verfolgen die Möglichkeit, die während der Entwurfszeit Argumente über die Tools angeben.

Eine Entwurfszeit-Factory kann besonders hilfreich, wenn Sie die "DbContext" für die Entwurfszeit als zur Laufzeit unterschiedlich zu konfigurieren, wenn möchten sein der `DbContext` Attributkonstruktor verwendet, die zusätzliche Parameter sind nicht im DI registriert, wenn Sie DI nicht zu verwenden oder, wenn für einige Grund möchten nicht, dass eine `BuildWebHost` -Methode in Ihrer ASP.NET Core-Anwendung `Main` Klasse.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
