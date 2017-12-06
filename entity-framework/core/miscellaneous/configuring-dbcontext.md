---
title: Konfigurieren ein ' DbContext ' - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 96abf3b94be3e1d19f833644f1c2f6f13fe0e730
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2017
---
# <a name="configuring-a-dbcontext"></a>Konfigurieren ein ' DbContext '

In diesem Artikel werden die unzulässigen Muster für die Konfiguration einer `DbContext` mit `DbContextOptions`. Optionen werden in erster Linie zum auswählen und konfigurieren den Datenspeicher.

## <a name="configuring-dbcontextoptions"></a>Konfigurieren von DbContextOptions

`DbContext`eine Instanz des `DbContextOptions` um auszuführen. Dies kann durch Außerkraftsetzen konfiguriert werden `OnConfiguring`, oder extern über einen Konstruktorargument angegeben.

Wenn beide verwendet werden, `OnConfiguring` ausgeführt wird, auf den angegebenen Optionen, d. h., es ist Additive und kann für Konstruktorarguments angegebenen Optionen für das überschreiben.

### <a name="constructor-argument"></a>Konstruktorargument

Kontextcode mit-Konstruktor

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

> [!TIP]  
> Die Basis DbContext-Konstruktor akzeptiert auch die nicht generische Version der `DbContextOptions`. Verwenden die nicht generische Version wird nicht für Anwendungen mit mehreren Kontexttypen empfohlen.

Anwendungscode Konstruktorargument Initialisierung

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>OnConfiguring

> [!WARNING]  
> `OnConfiguring`Tritt auf, zuletzt und Optionen, die aus abgerufen, DI oder der Konstruktor überschreiben können. Dieser Ansatz ist nicht verleiten testen (es sei denn, Sie die vollständige Datenbank ausgerichtet sein).

Kontextcode mit `OnConfiguring`:

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlite("Data Source=blog.db");
    }
}
```

Anwendungscode bei `OnConfiguring`:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

## <a name="using-dbcontext-with-dependency-injection"></a>Verwenden von ' DbContext ' mit Abhängigkeitsinjektion

EF unterstützt die Verwendung von `DbContext` mit einen abhängigkeitseinschleusungscontainer. Der DbContext-Typ kann dem Dienstcontainer hinzugefügt werden, mithilfe von `AddDbContext<TContext>`.

`AddDbContext`veranlasst, dass sowohl die DbContext-Typ `TContext`, und `DbContextOptions<TContext>` für Injection aus dem Dienstcontainer verfügbar.

Finden Sie unter [Weitere lesen](#more-reading) unten Informationen über die Abhängigkeitsinjektion.

Abhängigkeitsinjektion Dbcontext hinzugefügt

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Dies erfordert, Hinzufügen einer [Konstruktorargument](#constructor-argument) in den DbContext-Typ, das akzeptiert `DbContextOptions`.

Kontextcode:

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

Der Anwendungscode (in ASP.NET Core):

``` csharp
public MyController(BloggingContext context)
```

Der Anwendungscode (mithilfe von ServiceProvider direkt, weniger übliche):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="using-idesigntimedbcontextfactorytcontext"></a>Mithilfe von`IDesignTimeDbContextFactory<TContext>`

Als Alternative zu den oben genannten Optionen können Sie auch eine Implementierung bereitstellen `IDesignTimeDbContextFactory<TContext>`. EF-Tools können diese Factory zum Erstellen einer Instanz von Ihrem DbContext. Dies möglicherweise erforderlich, um bestimmte zur Entwurfszeit Erfahrungen z. B. Migrationen zu erzielen.

Implementieren Sie diese Schnittstelle, um Entwurfszeitdienste für Kontexttypen zu aktivieren, die nicht über einen öffentlichen Standardkonstruktor verfügen. Entwurfszeitdienste erkennt automatisch Implementierungen dieser Schnittstelle, die sich in derselben Assembly wie den abgeleiteten Kontext befinden.

Beispiel:

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

## <a name="more-reading"></a>Weitere lesen

* Lesen [Einstieg auf ASP.NET Core](../get-started/aspnetcore/index.md) für Weitere Informationen zur Verwendung von EF mit ASP.NET Core.
* Lesen [Abhängigkeitsinjektion](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) Weitere Informationen zum mithilfe der Abhängigkeitsinjektion.
* Lesen [Test](testing/index.md) für Weitere Informationen.
