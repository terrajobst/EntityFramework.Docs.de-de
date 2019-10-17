---
title: Konfigurieren eines dbcontext-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: e04c1b65df96819f3493e0ed34ccf26609511f6a
ms.sourcegitcommit: 37d0e0fd1703467918665a64837dc54ad2ec7484
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2019
ms.locfileid: "72445910"
---
# <a name="configuring-a-dbcontext"></a>Konfigurieren einer DbContext-Instanz

In diesem Artikel werden grundlegende Muster für die Konfiguration eines `DbContext` über einen `DbContextOptions` zum Herstellen einer Verbindung mit einer Datenbank mithilfe eines bestimmten EF Core Anbieters und optionaler Verhalten veranschaulicht.

## <a name="design-time-dbcontext-configuration"></a>Dbcontext-Konfiguration zur Entwurfszeit

EF Core Entwurfszeit Tools (z. b. [Migrationen](xref:core/managing-schemas/migrations/index) ) müssen in der Lage sein, eine funktionierende Instanz eines `DbContext`-Typs zu ermitteln und zu erstellen, um Details zu den Entitäts Typen der Anwendung und deren Zuordnung zu einem Datenbankschema zu erfassen. Dieser Prozess kann automatisch ausgeführt werden, solange das Tool das `DbContext` problemlos so erstellen kann, dass es so konfiguriert wird, wie es zur Laufzeit konfiguriert wird.

Obwohl jedes Muster, das die erforderlichen Konfigurationsinformationen für den `DbContext` bereitstellt, zur Laufzeit funktionieren kann, können Tools, die die Verwendung einer `DbContext` zur Entwurfszeit erfordern, nur mit einer begrenzten Anzahl von Mustern funktionieren. Diese werden im Abschnitt [Entwurfszeit-Kontext Erstellung](xref:core/miscellaneous/cli/dbcontext-creation) ausführlicher behandelt.

## <a name="configuring-dbcontextoptions"></a>Konfigurieren von dbcontextoptions

`DbContext` muss über eine Instanz von `DbContextOptions` verfügen, um die Arbeit auszuführen. Die Instanz `DbContextOptions` enthält Konfigurationsinformationen wie z. b.:

- Der zu verwendende Datenbankanbieter, der in der Regel durch Aufrufen einer Methode wie `UseSqlServer` oder `UseSqlite` ausgewählt wird. Diese Erweiterungs Methoden erfordern das entsprechende Anbieter Paket, z. b. `Microsoft.EntityFrameworkCore.SqlServer` oder `Microsoft.EntityFrameworkCore.Sqlite`. Die-Methoden werden im `Microsoft.EntityFrameworkCore`-Namespace definiert.
- Alle erforderlichen Verbindungs Zeichenfolgen oder Bezeichner der Daten Bank Instanz, die in der Regel als Argument an die oben genannte Anbieter Auswahlmethode weitergegeben werden.
- Beliebige optionale verhaltenselektoren auf Anbieter Ebene, die in der Regel auch innerhalb des Aufrufes der Anbieter Auswahlmethode verkettet sind
- Alle allgemeinen EF Core verhaltenselektoren, die normalerweise nach oder vor der Anbieter Auswahlmethode verkettet sind

Im folgenden Beispiel wird der `DbContextOptions` für die Verwendung des SQL Server Anbieters konfiguriert, eine in der `connectionString`-Variable enthaltene Verbindung, ein Befehls Timeout auf Anbieter Ebene und ein EF Core verhaltensselektor, der alle Abfragen ausführt, die in der `DbContext` [No-Tracking](xref:core/querying/tracking#no-tracking-queries) -Anweisung ausgeführt werden. Standardmäßig:

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Anbieter Auswahlmethoden und andere Verhaltens Auswahlmethoden, die oben erwähnt werden, sind Erweiterungs Methoden für `DbContextOptions` oder anbieterspezifische Options Klassen. Um Zugriff auf diese Erweiterungs Methoden zu erhalten, benötigen Sie möglicherweise einen Namespace (in der Regel `Microsoft.EntityFrameworkCore`) im Gültigkeitsbereich, und fügen Sie zusätzliche Paketabhängigkeiten in das Projekt ein.

Der `DbContextOptions` kann für die `DbContext` bereitgestellt werden, indem die `OnConfiguring`-Methode überschrieben wird, oder extern über ein Konstruktorargument.

Wenn beide verwendet werden, wird "`OnConfiguring`" zuletzt angewendet und kann für das Konstruktorargument bereitgestellte Optionen überschreiben.

### <a name="constructor-argument"></a>Konstruktorargument

Der Konstruktor kann einfach wie folgt eine `DbContextOptions` akzeptieren:

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
> Der Basiskonstruktor von dbcontext akzeptiert auch die nicht generische Version von `DbContextOptions`, aber die Verwendung der nicht generischen Version ist für Anwendungen mit mehreren Kontext Typen nicht empfehlenswert.

Die Anwendung kann jetzt den `DbContextOptions` übergeben, wenn ein Kontext instanziiert wird, wie im folgenden dargestellt:

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>Onkonfiguration

Sie können die `DbContextOptions` auch innerhalb des Kontexts selbst initialisieren. Obwohl Sie dieses Verfahren für die Basiskonfiguration verwenden können, müssen Sie in der Regel weiterhin bestimmte Konfigurationsdetails von außen (z. b. eine Daten bankverbindungs Zeichenfolge) erhalten. Dies kann mit einer Konfigurations-API oder mit einer beliebigen anderen Möglichkeit erreicht werden.

Um `DbContextOptions` innerhalb des Kontexts zu initialisieren, überschreiben Sie die `OnConfiguring`-Methode, und wenden Sie die Methoden auf dem bereitgestellten `DbContextOptionsBuilder` an:

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

Eine Anwendung kann einen solchen Kontext einfach instanziieren, ohne etwas an den Konstruktor zu übergeben:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Dieser Ansatz eignet sich nicht für Tests, es sei denn, die Tests sind auf die vollständige Datenbank ausgerichtet.

### <a name="using-dbcontext-with-dependency-injection"></a>Verwenden von dbcontext mit Abhängigkeitsinjektion

EF Core unterstützt die Verwendung von `DbContext` mit einem Container für die Abhängigkeitsinjektion. Der dbcontext-Typ kann dem Dienst Container hinzugefügt werden, indem die `AddDbContext<TContext>`-Methode verwendet wird.

`AddDbContext<TContext>` macht sowohl den dbcontext-Typ als auch `TContext` und den entsprechenden `DbContextOptions<TContext>` für die Injektion aus dem Dienst Container verfügbar.

Weitere Informationen zur Abhängigkeitsinjektion finden Sie unten unter [Weitere](#more-reading) Informationen.

Hinzufügen der `DbContext` zur Abhängigkeitsinjektion:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Dies erfordert das Hinzufügen eines [Konstruktorarguments](#constructor-argument) zu Ihrem dbcontext-Typ, der `DbContextOptions<TContext>` akzeptiert.

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

Entity Framework Core unterstützt nicht mehrere parallele Vorgänge, die auf derselben `DbContext`-Instanz ausgeführt werden. Dies schließt die parallele Ausführung von asynchronen Abfragen und jede explizite gleichzeitige Verwendung mehrerer Threads ein. Aus diesem Grund werden asynchrone Aufrufe von immer `await` ausgeführt, oder es werden separate `DbContext`-Instanzen für Vorgänge verwendet, die parallel ausgeführt werden.

Wenn EF Core den Versuch erkennt, eine `DbContext`-Instanz gleichzeitig zu verwenden, wird eine `InvalidOperationException` mit einer Meldung wie die folgende angezeigt: 

> Ein zweiter Vorgang, der in diesem Kontext gestartet wurde, bevor ein vorheriger Vorgang abgeschlossen wurde. Dies wird in der Regel durch unterschiedliche Threads verursacht, die dieselbe Instanz von dbcontext verwenden, aber es ist nicht garantiert, dass Instanzmember Thread sicher sind.

Wenn der gleichzeitige Zugriff nicht erkannt wird, kann dies zu nicht definiertem Verhalten, Anwendungs abstürzen und Daten Beschädigung führen.

Es gibt häufige Fehler, die versehentlich den gleichzeitigen Zugriff auf dieselbe `DbContext`-Instanz verursachen können:

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a>Vergessen Sie nicht, auf den Abschluss eines asynchronen Vorgangs zu warten, bevor Sie einen anderen Vorgang im gleichen dbcontext starten.

Asynchrone Methoden ermöglichen EF Core das Initiieren von Vorgängen, die auf eine nicht blockierende Weise auf die Datenbank zugreifen. Wenn ein Aufrufer jedoch nicht auf den Abschluss einer dieser Methoden wartet und weiterhin andere Vorgänge für die `DbContext` ausführt, kann der Status des `DbContext` sein, (und sehr wahrscheinlich sein) beschädigt. 

Asynchrone Methoden sollten immer sofort EF Core werden.  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a>Implizites Freigeben von dbcontext-Instanzen über mehrere Threads über Abhängigkeitsinjektion

Die [`AddDbContext`-](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) Erweiterungsmethode registriert `DbContext`-Typen standardmäßig mit einer Bereichs bezogenen Gültigkeits [Dauer](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) . 

Dies ist vor gleichzeitigen Zugriffsproblemen in ASP.net Core Anwendungen sicher, weil nur ein Thread zu einem bestimmten Zeitpunkt jede Client Anforderung ausführt, und weil jede Anforderung einen separaten Abhängigkeits einschleusungs Bereich erhält (und somit eine separate `DbContext`-Instanz).

Jeder Code, der explizit mehrere Threads parallel ausführt, sollte jedoch sicherstellen, dass auf `DbContext`-Instanzen nicht gleichzeitig zugegriffen wird.

Mithilfe der Abhängigkeitsinjektion kann dies erreicht werden, indem der Kontext entweder als Bereichs bezogen registriert und Bereiche (mit `IServiceScopeFactory`) für jeden Thread registriert werden, oder indem die `DbContext` als vorübergehender Wert registriert wird (mithilfe der Überladung von `AddDbContext`, der einen `ServiceLifetime`-Parameter annimmt).

## <a name="more-reading"></a>Weitere Informationen

* Weitere Informationen zur Verwendung von di finden Sie unter [Abhängigkeitsinjektion](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) .
* Weitere Informationen finden Sie unter [Testen](testing/index.md) .
