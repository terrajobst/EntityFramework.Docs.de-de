---
title: Konfigurieren eines dbcontext-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 734acad86e364abbfd1522fe79d4a847b1acfb52
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149031"
---
# <a name="configuring-a-dbcontext"></a>Konfigurieren eines dbcontext

In diesem Artikel werden grundlegende Muster für `DbContext` die Konfiguration `DbContextOptions` eines über eine veranschaulicht, um mithilfe eines bestimmten EF Core Anbieters und optionalem Verhalten eine Verbindung mit einer Datenbank herzustellen.

## <a name="design-time-dbcontext-configuration"></a>Dbcontext-Konfiguration zur Entwurfszeit

EF Core Entwurfszeit Tools (z. b. [Migrationen](xref:core/managing-schemas/migrations/index) ) müssen in der Lage sein, eine funktionierende Instanz `DbContext` eines Typs zu ermitteln und zu erstellen, um Details zu den Entitäts Typen der Anwendung und deren Zuordnung zu einem Datenbankschema zu erfassen. Dieser Prozess kann automatisch ausgeführt werden, solange das Tool auf einfache Weise so `DbContext` erstellen kann, dass es so konfiguriert wird, wie es zur Laufzeit konfiguriert wird.

Obwohl jedes Muster, das die erforderlichen Konfigurationsinformationen für `DbContext` bereitstellt, zur Laufzeit funktionieren kann, können Tools, die die `DbContext` Verwendung von zur Entwurfszeit erfordern, nur mit einer begrenzten Anzahl von Mustern funktionieren. Diese werden im Abschnitt [Entwurfszeit-Kontext Erstellung](xref:core/miscellaneous/cli/dbcontext-creation) ausführlicher behandelt.

## <a name="configuring-dbcontextoptions"></a>Konfigurieren von dbcontextoptions

`DbContext`muss über eine Instanz von `DbContextOptions` verfügen, um die Arbeit auszuführen. Die `DbContextOptions` -Instanz enthält Konfigurationsinformationen wie z. b.:

- Der zu verwendende Datenbankanbieter, der in der Regel durch Aufrufen `UseSqlServer` einer `UseSqlite`Methode wie oder ausgewählt wird. Diese Erweiterungs Methoden erfordern das entsprechende Anbieter Paket, z `Microsoft.EntityFrameworkCore.SqlServer` . b. oder. `Microsoft.EntityFrameworkCore.Sqlite` Die-Methoden werden im `Microsoft.EntityFrameworkCore` -Namespace definiert.
- Alle erforderlichen Verbindungs Zeichenfolgen oder Bezeichner der Daten Bank Instanz, die in der Regel als Argument an die oben genannte Anbieter Auswahlmethode weitergegeben werden.
- Beliebige optionale verhaltenselektoren auf Anbieter Ebene, die in der Regel auch innerhalb des Aufrufes der Anbieter Auswahlmethode verkettet sind
- Alle allgemeinen EF Core verhaltenselektoren, die normalerweise nach oder vor der Anbieter Auswahlmethode verkettet sind

Im folgenden Beispiel wird konfiguriert `DbContextOptions` , um den SQL Server-Anbieter, eine in der `connectionString` -Variable enthaltene Verbindung, ein Befehls Timeout auf Anbieter Ebene und eine EF Core Verhaltens Auswahl zu verwenden, die `DbContext` alle Abfragen im standardmäßig [keine Nachverfolgung](xref:core/querying/tracking#no-tracking-queries) :

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Anbieter Auswahlmethoden und andere Verhaltens Auswahlmethoden, die oben erwähnt werden, `DbContextOptions` sind Erweiterungs Methoden für oder anbieterspezifische Options Klassen. Um Zugriff auf diese Erweiterungs Methoden zu erhalten, benötigen Sie möglicherweise einen Namespace (in der `Microsoft.EntityFrameworkCore`Regel) im Gültigkeitsbereich und fügen zusätzliche Paketabhängigkeiten in das Projekt ein.

Kann für das `DbContext` bereitgestellt werden, indem die `OnConfiguring` -Methode überschrieben wird, oder extern über ein Konstruktorargument. `DbContextOptions`

Wenn beide verwendet werden, `OnConfiguring` wird zuletzt angewendet, und die für das Konstruktorargument bereitgestellten Optionen können überschrieben werden.

### <a name="constructor-argument"></a>Konstruktorargument

Kontext Code mit Konstruktor:

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
> Der Basiskonstruktor von dbcontext akzeptiert auch die nicht generische Version von `DbContextOptions`, aber die Verwendung der nicht generischen Version wird nicht für Anwendungen mit mehreren Kontext Typen empfohlen.

Anwendungscode, der aus dem Konstruktorargument initialisiert werden soll:

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>Onkonfiguration

Kontext Code mit `OnConfiguring`:

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

Anwendungscode zum Initialisieren einer `DbContext` , die `OnConfiguring`Folgendes verwendet:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Dieser Ansatz eignet sich nicht für Tests, es sei denn, die Tests sind auf die vollständige Datenbank ausgerichtet.

### <a name="using-dbcontext-with-dependency-injection"></a>Verwenden von dbcontext mit Abhängigkeitsinjektion

EF Core unterstützt `DbContext` die Verwendung von mit einem Container für Abhängigkeitsinjektion. Der dbcontext-Typ kann dem Dienst Container mithilfe der `AddDbContext<TContext>` -Methode hinzugefügt werden.

`AddDbContext<TContext>`von werden sowohl der dbcontext-Typ `TContext`als auch der entsprechende `DbContextOptions<TContext>` für Injection aus dem Dienst Container verfügbar gemacht.

Weitere Informationen zur Abhängigkeitsinjektion finden Sie unten unter [Weitere](#more-reading) Informationen.

Hinzufügen von zur Abhängigkeitsinjektion: `DbContext`

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Dies erfordert das Hinzufügen eines [Konstruktorarguments](#constructor-argument) zu Ihrem dbcontext- `DbContextOptions<TContext>`Typ, der akzeptiert.

Kontext Code:

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

Anwendungscode (in ASP.net Core):

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

Anwendungscode (Verwenden von Service Provider direkt, weniger häufig):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```
## <a name="avoiding-dbcontext-threading-issues"></a>Vermeiden von dbcontext-Threading Problemen

Entity Framework Core unterstützt nicht mehrere parallele Vorgänge, die auf derselben `DbContext` Instanz ausgeführt werden. Dies schließt die parallele Ausführung von asynchronen Abfragen und jede explizite gleichzeitige Verwendung mehrerer Threads ein. Daher wird immer `await` Async sofort aufgerufen, oder es werden `DbContext` separate Instanzen für Vorgänge verwendet, die parallel ausgeführt werden.

Wenn EF Core den Versuch erkennt, eine `DbContext` -Instanz gleichzeitig zu verwenden, wird eine mit einer Meldung wie die `InvalidOperationException` folgende angezeigt: 

> Ein zweiter Vorgang, der in diesem Kontext gestartet wurde, bevor ein vorheriger Vorgang abgeschlossen wurde. Dies wird in der Regel durch unterschiedliche Threads verursacht, die dieselbe Instanz von dbcontext verwenden, aber es ist nicht garantiert, dass Instanzmember Thread sicher sind.

Wenn der gleichzeitige Zugriff nicht erkannt wird, kann dies zu nicht definiertem Verhalten, Anwendungs abstürzen und Daten Beschädigung führen.

Es gibt häufige Fehler, die versehentlich den gleichzeitigen Zugriff auf dieselbe `DbContext` Instanz verursachen können:

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a>Vergessen Sie nicht, auf den Abschluss eines asynchronen Vorgangs zu warten, bevor Sie einen anderen Vorgang im gleichen dbcontext starten.

Asynchrone Methoden ermöglichen EF Core das Initiieren von Vorgängen, die auf eine nicht blockierende Weise auf die Datenbank zugreifen. Wenn ein Aufrufer jedoch nicht auf den Abschluss einer dieser Methoden wartet und weiterhin andere Vorgänge auf dem `DbContext`ausführt, `DbContext` kann der Zustand von (und wahrscheinlich) beschädigt sein. 

Asynchrone Methoden sollten immer sofort EF Core werden.  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a>Implizites Freigeben von dbcontext-Instanzen über mehrere Threads über Abhängigkeitsinjektion

Die [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) Erweiterungsmethode registriert `DbContext` Typen standardmäßig mit einer Gültigkeits [Dauer](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) . 

Dies ist vor gleichzeitigen Zugriffsproblemen in ASP.net Core Anwendungen sicher, weil nur ein Thread zu einem bestimmten Zeitpunkt jede Client Anforderung ausführt, und weil jede Anforderung einen separaten Abhängigkeits einschleusungs Bereich erhält ( `DbContext` und somit eine separate Instanz).

Jeder Code, der explizit mehrere Threads parallel ausführt, sollte jedoch sicher `DbContext` stellen, dass auf Instanzen nicht gleichzeitig zugegriffen wird.

Mithilfe der Abhängigkeitsinjektion kann dies erreicht werden, indem der Kontext entweder als Bereichs bezogen registriert wird und Bereiche `IServiceScopeFactory`(mit) für die einzelnen Threads erstellt werden `DbContext` , oder indem der als vorübergehender `AddDbContext` registriert wird (mithilfe der Überladung von, die einen `ServiceLifetime` -Parameter).

## <a name="more-reading"></a>Weitere Informationen

* Weitere Informationen zur Verwendung von di finden Sie unter [Abhängigkeitsinjektion](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) .
* Weitere Informationen finden Sie unter [Testen](testing/index.md) .
