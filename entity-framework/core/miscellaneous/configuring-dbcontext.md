---
title: Konfigurieren ein ' DbContext ' - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 6980acd53b0a74055af7a1e04b476f4625c327c9
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2018
ms.locfileid: "29152389"
---
# <a name="configuring-a-dbcontext"></a>Konfigurieren ein ' DbContext '

In diesem Artikel erfahren Grundmuster für die Konfiguration einer `DbContext` über eine `DbContextOptions` zur Verbindung mit einer Datenbank mithilfe einer bestimmten EF-Core-Anbieter und Optionaler Verhaltensweisen.

## <a name="design-time-dbcontext-configuration"></a>Zur Entwurfszeit DbContext-Konfiguration

EF Core Entwurfszeit-tools, z. B. [Migrationen](xref:core/managing-schemas/migrations/index) müssen in der Lage zu ermitteln, und erstellen Sie eine funktionierende Instanz von einer `DbContext` Typ, um Details über der Anwendungsverzeichnis Entitätstypen und deren Zuordnung zum Schema einer sammeln. Dieser Vorgang kann automatische sein, solange das Tool leicht erstellen kann die `DbContext` so, dass sie auf ähnliche Weise konfiguriert werden soll, wie es zur Laufzeit konfiguriert werden würde.

Jedes Muster, die die erforderlichen Informationen bereitstellt, während die `DbContext` können zur Laufzeit, Tools, die für die Verwendung erforderlich arbeiten eine `DbContext` zur Entwurfszeit funktionieren nur mit einer begrenzten Anzahl von Mustern. Diese werden in ausführlicher behandelt die [zur Entwurfszeit Kontext Erstellung](xref:core/miscellaneous/cli/dbcontext-creation) Abschnitt.

## <a name="configuring-dbcontextoptions"></a>Konfigurieren von DbContextOptions

`DbContext` eine Instanz des `DbContextOptions` um alle Tasks auszuführen. Die `DbContextOptions` Instanz führt Konfigurationsinformationen wie z. B.:

- Der Datenbankanbieter verwendet, in der Regel durch Aufrufen einer Methode wie z. B. ausgewählt `UseSqlServer` oder `UseSqlite`
- Eine erforderliche Verbindungszeichenfolge oder die ID der Datenbankinstanz, in der Regel als Argument übergeben, die oben genannten Anbieter Auswahlmethode
- Alle auf Anbieterebene optionales Verhalten Selektoren, die in der Regel auch innerhalb des Aufrufs an den Anbieter Auswahlmethode verkettet
- Alle allgemeinen Selektoren zur EF-Core-Verhalten, verkettet in der Regel nach oder vor der Anbieter-Selektor-Methode

Das folgende Beispiel konfiguriert die `DbContextOptions` für die Verwendung den SQL Server-Anbieter eine Verbindung enthalten sind, der `connectionString` Variable, ein Befehlstimeout auf Anbieterebene und eine EF Core Verhalten Selektor, die alle im ausgeführten Abfragen stellt der `DbContext` [keine Überwachung](xref:core/querying/tracking#no-tracking-queries) standardmäßig:

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Anbieter-Selektor-Methoden und andere Verhalten Auswahlzeiger-Methoden, die oben genannten Erweiterungsmethoden, sind auf `DbContextOptions` oder Anbieter-spezifische Klassen. Um Zugriff auf diese Erweiterungsmethoden haben, Sie möglicherweise haben einen Namespace müssen (in der Regel `Microsoft.EntityFrameworkCore`) im Bereich und zusätzliche paketabhängigkeiten in das Projekt einfügen.

Die `DbContextOptions` bereitgestellt werden kann die `DbContext` durch Überschreiben der `OnConfiguring` Methode oder extern über einen Konstruktorargument.

Wenn beide verwendet werden, `OnConfiguring` zuletzt angewendet wird, und können Überschreiboptionen Konstruktorarguments bereitgestellt.

### <a name="constructor-argument"></a>Konstruktorargument

Kontextcode mit dem Konstruktor:

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
> Die Basis DbContext-Konstruktor akzeptiert auch die nicht generische Version der `DbContextOptions`, die nicht generische Version wird jedoch nicht empfohlen für Anwendungen mit mehreren Kontexttypen.

Der Anwendungscode Konstruktorargument Initialisierung:

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>OnConfiguring

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

Anwendungscode zum Initialisieren einer `DbContext` , verwendet `OnConfiguring`:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Dieser Ansatz wird selbst für testen, nicht geeignet, es sei denn, die Tests die vollständige Datenbank als Ziel.

### <a name="using-dbcontext-with-dependency-injection"></a>Verwenden von ' DbContext ' mit Abhängigkeitsinjektion

EF Core unterstützt die Verwendung von `DbContext` mit einen abhängigkeitseinschleusungscontainer. Die DbContext-Typ kann dem Dienstcontainer hinzugefügt werden, mithilfe der `AddDbContext<TContext>` Methode.

`AddDbContext<TContext>` veranlasst, dass sowohl die DbContext-Typ `TContext`, und die entsprechende `DbContextOptions<TContext>` für Injection aus dem Dienstcontainer verfügbar.

Finden Sie unter [Weitere lesen](#more-reading) unten für Weitere Informationen zu Abhängigkeitsinjektion.

Hinzufügen der `Dbcontext` zum Abhängigkeitsinjektion:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Dies erfordert, Hinzufügen einer [Konstruktorargument](#constructor-argument) in den DbContext-Typ, das akzeptiert `DbContextOptions<TContext>`.

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
public class MyController
{
    private readonly BloggingContext _context;

    public MyController(BloggingContext context)
    {
      _context = context;
    }

    ...
}
```

Der Anwendungscode (mithilfe von ServiceProvider direkt, weniger übliche):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a>Weitere lesen

* Lesen [Einstieg auf ASP.NET Core](../get-started/aspnetcore/index.md) für Weitere Informationen zur Verwendung von EF mit ASP.NET Core.
* Lesen [Abhängigkeitsinjektion](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) Weitere Informationen zum mithilfe der Abhängigkeitsinjektion.
* Lesen [Test](testing/index.md) für Weitere Informationen.
