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
<a name="design-time-dbcontext-creation"></a>Zur Entwurfszeit DbContext-Erstellung
==============================
Einige der Befehle eine DbContext-Instanz zu erstellenden Daten am Entwurf erfordern EF Tools Zeit (z. B. bei der Migrationen Befehle ausführen zu können). Es gibt verschiedene Möglichkeiten, die die Tools versuchen, ihn zu erstellen.

<a name="from-application-services"></a>Von Application-Dienste
-------------------------
Ist Ihr Startprojekt eine ASP.NET Core-app, versuchen Sie die Tools, den DbContext-Objekt von der Anwendung Service-Anbieter abzurufen. Sie erhalten sie durch den Aufruf `Program.BuildWebHost()` und den Zugriff auf die `IWebHost.Services` Eigenschaft. Mit jeder DbContext registriert `IServiceCollection.AddDbContext<TContext>()` gefunden und auf diese Weise erstellt werden kann. Dieses Muster wurde [in ASP.NET Core 2.0 eingeführt][1]

<a name="using-the-default-constructor"></a>Mithilfe des Standardkonstruktors
-----------------------------
Wenn von der Anwendung-Service-Anbieter ' DbContext ' abgerufen werden kann, suchen Sie die Tools für den DbContext-Typ innerhalb des Projekts ein. Sie versuchen, ihn mit seinem Standardkonstruktor erstellen.

<a name="from-a-design-time-factory"></a>Aus einer Factory zur Entwurfszeit
--------------------------
Können die Tools zum Erstellen Ihrer DbContext implementieren auch erkennen `IDesignTimeDbContextFactory`. Wenn eine Klasse, die diese Schnittstelle implementiert, innerhalb des Projekts gefunden wird, umgehen die Tools weitere Möglichkeiten zum Erstellen von ' DbContext ' aus.
Zur Entwurfszeit wird stets der Factory verwendet. Eine Factory ist besonders nützlich, wenn Sie ' DbContext ' für die Entwurfszeit als zur Laufzeit unterschiedlich konfigurieren müssen.

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
> Die `args` Parameter wird derzeit nicht verwendet. Es ist [ein Problem] [ 2] verfolgen die Möglichkeit, Argumente in den Tools anzugeben.

  [1]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [2]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
