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
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="a5644-102">Konfigurieren ein ' DbContext '</span><span class="sxs-lookup"><span data-stu-id="a5644-102">Configuring a DbContext</span></span>

<span data-ttu-id="a5644-103">In diesem Artikel erfahren Grundmuster für die Konfiguration einer `DbContext` über eine `DbContextOptions` zur Verbindung mit einer Datenbank mithilfe einer bestimmten EF-Core-Anbieter und Optionaler Verhaltensweisen.</span><span class="sxs-lookup"><span data-stu-id="a5644-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="a5644-104">Zur Entwurfszeit DbContext-Konfiguration</span><span class="sxs-lookup"><span data-stu-id="a5644-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="a5644-105">EF Core Entwurfszeit-tools, z. B. [Migrationen](xref:core/managing-schemas/migrations/index) müssen in der Lage zu ermitteln, und erstellen Sie eine funktionierende Instanz von einer `DbContext` Typ, um Details über der Anwendungsverzeichnis Entitätstypen und deren Zuordnung zum Schema einer sammeln.</span><span class="sxs-lookup"><span data-stu-id="a5644-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="a5644-106">Dieser Vorgang kann automatische sein, solange das Tool leicht erstellen kann die `DbContext` so, dass sie auf ähnliche Weise konfiguriert werden soll, wie es zur Laufzeit konfiguriert werden würde.</span><span class="sxs-lookup"><span data-stu-id="a5644-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="a5644-107">Jedes Muster, die die erforderlichen Informationen bereitstellt, während die `DbContext` können zur Laufzeit, Tools, die für die Verwendung erforderlich arbeiten eine `DbContext` zur Entwurfszeit funktionieren nur mit einer begrenzten Anzahl von Mustern.</span><span class="sxs-lookup"><span data-stu-id="a5644-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="a5644-108">Diese werden in ausführlicher behandelt die [zur Entwurfszeit Kontext Erstellung](xref:core/miscellaneous/cli/dbcontext-creation) Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="a5644-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="a5644-109">Konfigurieren von DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="a5644-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="a5644-110">`DbContext` eine Instanz des `DbContextOptions` um alle Tasks auszuführen.</span><span class="sxs-lookup"><span data-stu-id="a5644-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="a5644-111">Die `DbContextOptions` Instanz führt Konfigurationsinformationen wie z. B.:</span><span class="sxs-lookup"><span data-stu-id="a5644-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="a5644-112">Der Datenbankanbieter verwendet, in der Regel durch Aufrufen einer Methode wie z. B. ausgewählt `UseSqlServer` oder `UseSqlite`</span><span class="sxs-lookup"><span data-stu-id="a5644-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`</span></span>
- <span data-ttu-id="a5644-113">Eine erforderliche Verbindungszeichenfolge oder die ID der Datenbankinstanz, in der Regel als Argument übergeben, die oben genannten Anbieter Auswahlmethode</span><span class="sxs-lookup"><span data-stu-id="a5644-113">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="a5644-114">Alle auf Anbieterebene optionales Verhalten Selektoren, die in der Regel auch innerhalb des Aufrufs an den Anbieter Auswahlmethode verkettet</span><span class="sxs-lookup"><span data-stu-id="a5644-114">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="a5644-115">Alle allgemeinen Selektoren zur EF-Core-Verhalten, verkettet in der Regel nach oder vor der Anbieter-Selektor-Methode</span><span class="sxs-lookup"><span data-stu-id="a5644-115">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="a5644-116">Das folgende Beispiel konfiguriert die `DbContextOptions` für die Verwendung den SQL Server-Anbieter eine Verbindung enthalten sind, der `connectionString` Variable, ein Befehlstimeout auf Anbieterebene und eine EF Core Verhalten Selektor, die alle im ausgeführten Abfragen stellt der `DbContext` [keine Überwachung](xref:core/querying/tracking#no-tracking-queries) standardmäßig:</span><span class="sxs-lookup"><span data-stu-id="a5644-116">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="a5644-117">Anbieter-Selektor-Methoden und andere Verhalten Auswahlzeiger-Methoden, die oben genannten Erweiterungsmethoden, sind auf `DbContextOptions` oder Anbieter-spezifische Klassen.</span><span class="sxs-lookup"><span data-stu-id="a5644-117">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="a5644-118">Um Zugriff auf diese Erweiterungsmethoden haben, Sie möglicherweise haben einen Namespace müssen (in der Regel `Microsoft.EntityFrameworkCore`) im Bereich und zusätzliche paketabhängigkeiten in das Projekt einfügen.</span><span class="sxs-lookup"><span data-stu-id="a5644-118">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="a5644-119">Die `DbContextOptions` bereitgestellt werden kann die `DbContext` durch Überschreiben der `OnConfiguring` Methode oder extern über einen Konstruktorargument.</span><span class="sxs-lookup"><span data-stu-id="a5644-119">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="a5644-120">Wenn beide verwendet werden, `OnConfiguring` zuletzt angewendet wird, und können Überschreiboptionen Konstruktorarguments bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="a5644-120">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="a5644-121">Konstruktorargument</span><span class="sxs-lookup"><span data-stu-id="a5644-121">Constructor argument</span></span>

<span data-ttu-id="a5644-122">Kontextcode mit dem Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="a5644-122">Context code with constructor:</span></span>

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
> <span data-ttu-id="a5644-123">Die Basis DbContext-Konstruktor akzeptiert auch die nicht generische Version der `DbContextOptions`, die nicht generische Version wird jedoch nicht empfohlen für Anwendungen mit mehreren Kontexttypen.</span><span class="sxs-lookup"><span data-stu-id="a5644-123">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="a5644-124">Der Anwendungscode Konstruktorargument Initialisierung:</span><span class="sxs-lookup"><span data-stu-id="a5644-124">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="a5644-125">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="a5644-125">OnConfiguring</span></span>

<span data-ttu-id="a5644-126">Kontextcode mit `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="a5644-126">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="a5644-127">Anwendungscode zum Initialisieren einer `DbContext` , verwendet `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="a5644-127">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="a5644-128">Dieser Ansatz wird selbst für testen, nicht geeignet, es sei denn, die Tests die vollständige Datenbank als Ziel.</span><span class="sxs-lookup"><span data-stu-id="a5644-128">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="a5644-129">Verwenden von ' DbContext ' mit Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="a5644-129">Using DbContext with dependency injection</span></span>

<span data-ttu-id="a5644-130">EF Core unterstützt die Verwendung von `DbContext` mit einen abhängigkeitseinschleusungscontainer.</span><span class="sxs-lookup"><span data-stu-id="a5644-130">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="a5644-131">Die DbContext-Typ kann dem Dienstcontainer hinzugefügt werden, mithilfe der `AddDbContext<TContext>` Methode.</span><span class="sxs-lookup"><span data-stu-id="a5644-131">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="a5644-132">`AddDbContext<TContext>` veranlasst, dass sowohl die DbContext-Typ `TContext`, und die entsprechende `DbContextOptions<TContext>` für Injection aus dem Dienstcontainer verfügbar.</span><span class="sxs-lookup"><span data-stu-id="a5644-132">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="a5644-133">Finden Sie unter [Weitere lesen](#more-reading) unten für Weitere Informationen zu Abhängigkeitsinjektion.</span><span class="sxs-lookup"><span data-stu-id="a5644-133">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="a5644-134">Hinzufügen der `Dbcontext` zum Abhängigkeitsinjektion:</span><span class="sxs-lookup"><span data-stu-id="a5644-134">Adding the `Dbcontext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="a5644-135">Dies erfordert, Hinzufügen einer [Konstruktorargument](#constructor-argument) in den DbContext-Typ, das akzeptiert `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="a5644-135">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="a5644-136">Kontextcode:</span><span class="sxs-lookup"><span data-stu-id="a5644-136">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="a5644-137">Der Anwendungscode (in ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="a5644-137">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="a5644-138">Der Anwendungscode (mithilfe von ServiceProvider direkt, weniger übliche):</span><span class="sxs-lookup"><span data-stu-id="a5644-138">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a><span data-ttu-id="a5644-139">Weitere lesen</span><span class="sxs-lookup"><span data-stu-id="a5644-139">More reading</span></span>

* <span data-ttu-id="a5644-140">Lesen [Einstieg auf ASP.NET Core](../get-started/aspnetcore/index.md) für Weitere Informationen zur Verwendung von EF mit ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a5644-140">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="a5644-141">Lesen [Abhängigkeitsinjektion](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) Weitere Informationen zum mithilfe der Abhängigkeitsinjektion.</span><span class="sxs-lookup"><span data-stu-id="a5644-141">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="a5644-142">Lesen [Test](testing/index.md) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="a5644-142">Read [Testing](testing/index.md) for more information.</span></span>
