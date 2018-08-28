---
title: Konfigurieren einen "DbContext" - Entity Framework Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 393349c05ffaf42c6d2520e73abce23def6becc0
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995937"
---
# <a name="configuring-a-dbcontext"></a>Konfigurieren einen "DbContext"

In diesem Artikel wird die grundlegende Muster für die Konfiguration einer `DbContext` über eine `DbContextOptions` zur Verbindung mit einer Datenbank mithilfe eines bestimmten Anbieters von EF Core und optionales Verhalten.

## <a name="design-time-dbcontext-configuration"></a>Während der Entwurfszeit "DbContext"-Konfiguration

EF Core Entwurfszeit-tools, z. B. [Migrationen](xref:core/managing-schemas/migrations/index) müssen in der Lage, um zu ermitteln, und erstellen eine Arbeitsinstanz von einem `DbContext` Typ sammeln Informationen zu der Anwendung Entitätstypen und wie sie ein Datenbankschema zugeordnet. Dieser Prozess kann automatisch erfolgen, solange das Tool leicht erstellen kann die `DbContext` so, dass sie auf ähnliche Weise konfiguriert werden soll, wie es zur Laufzeit konfiguriert werden sollen.

Während jedes Muster, das die erforderlichen Konfigurationsinformationen, bietet der `DbContext` können arbeiten, zur Laufzeit, Tools, die mit erfordern eine `DbContext` zur Entwurfszeit funktioniert nur mit einer begrenzten Anzahl von Mustern. Diese werden in ausführlicher behandelt die [während der Entwurfszeit beim Erstellen des Serverkontexts](xref:core/miscellaneous/cli/dbcontext-creation) Abschnitt.

## <a name="configuring-dbcontextoptions"></a>Konfigurieren der "dbcontextoptions"

`DbContext` eine Instanz des `DbContextOptions` um ausführen jeglicher arbeiten. Die `DbContextOptions` Instanz enthält Konfigurationsinformationen, z.B.:

- Die zu verwendenden Datenbankanbieter, in der Regel ausgewählt wird, durch Aufrufen einer Methode wie z. B. `UseSqlServer` oder `UseSqlite`
- Eine erforderliche Verbindungszeichenfolge oder die ID der Datenbankinstanz, in der Regel als Argument übergeben, der oben genannten Anbieter Selection-Methode
- Alle auf Anbieterebene optionales Verhalten-Selektoren, die in der Regel auch verkettet innerhalb des Aufrufs an die Anbieter-Auswahl-Methode
- Alle allgemeinen Selektoren zur EF Core-Verhalten, verkettet in der Regel hinter oder vor der Anbieter-Selektor-Methode

Im folgenden Beispiel wird die `DbContextOptions` für die Verwendung den SQL Server-Anbieter eine Verbindung enthalten sind, der `connectionString` Variable, ein auf Anbieterebene Befehlstimeout und eine Auswahl der EF Core-Verhalten, die alle Abfragen ausgeführt werden die `DbContext` [ohne nachverfolgung](xref:core/querying/tracking#no-tracking-queries) standardmäßig:

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Anbieter-Selektor-Methoden und andere Verhalten Selector-Methoden, die oben genannten Erweiterungsmethoden befinden sich auf `DbContextOptions` oder Klassen von Anbieter-spezifische Option. Um den Zugriff auf diese besonderen Erweiterungsmethoden zu erhalten, Sie möglicherweise haben einen Namespace müssen (in der Regel `Microsoft.EntityFrameworkCore`) im Bereich und die Abhängigkeiten von zusätzlichen Paketen im Projekt enthalten.

Die `DbContextOptions` bereitgestellt werden kann die `DbContext` durch Überschreiben der `OnConfiguring` Methode oder extern über ein Konstruktorargument.

Wenn beide verwendet werden, `OnConfiguring` zuletzt angewendet wird, und können überschreiben Optionen, die das Konstruktorargument angegeben.

### <a name="constructor-argument"></a>Konstruktorargument

Kontextcode mit Konstruktor:

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
> Basiskonstruktor der "DbContext" akzeptiert auch die nicht generische Version von `DbContextOptions`, die nicht generische Version wird jedoch nicht empfohlen für Anwendungen mit mehreren Arten von Kontext.

Der Anwendungscode Konstruktorargument nicht initialisieren:

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

Anwendungscode zum Initialisieren einer `DbContext` verwendet `OnConfiguring`:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Dieser Ansatz ist nicht selbst für Tests geeignet sind, wenn die Tests die vollständige Datenbank als Ziel.

### <a name="using-dbcontext-with-dependency-injection"></a>Mithilfe von "DbContext" über Dependency injection

EF Core unterstützt die Verwendung von `DbContext` mit DI-Containern. Ihr "DbContext"-Typ kann zum Dienstcontainer hinzugefügt werden, mithilfe der `AddDbContext<TContext>` Methode.

`AddDbContext<TContext>` veranlasst, dass sowohl Ihr "DbContext"-Typ `TContext`, und die entsprechende `DbContextOptions<TContext>` zur Einschleusung von Befehlen aus dem Dienstcontainer.

Finden Sie unter [Weitere lesen](#more-reading) unten Weitere Informationen zur Abhängigkeitsinjektion.

Hinzufügen der `Dbcontext` zur Dependency Injection:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Dies erfordert das Hinzufügen einer [Konstruktorargument](#constructor-argument) in den "DbContext"-Typ, der akzeptiert `DbContextOptions<TContext>`.

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

Anwendungscode (mithilfe von ServiceProvider direkt nicht so üblich):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a>Weitere Informationen

* Lesen [erste Schritte in ASP.NET Core](../get-started/aspnetcore/index.md) für Weitere Informationen zur Verwendung von EF mit ASP.NET Core.
* Lesen [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) Informationen mithilfe der Abhängigkeitsinjektion.
* Lesen [Tests](testing/index.md) für Weitere Informationen.
