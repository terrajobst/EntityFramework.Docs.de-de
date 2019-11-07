---
title: Konfigurieren eines dbcontext-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 3ab90d46b7a4476044e5ea38eaf04f995708e7bf
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655800"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="a5cdf-102">Konfigurieren einer DbContext-Instanz</span><span class="sxs-lookup"><span data-stu-id="a5cdf-102">Configuring a DbContext</span></span>

<span data-ttu-id="a5cdf-103">In diesem Artikel werden grundlegende Muster für die Konfiguration eines `DbContext` über einen `DbContextOptions` zum Herstellen einer Verbindung mit einer Datenbank mithilfe eines bestimmten EF Core Anbieters und optionaler Verhalten veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="a5cdf-104">Dbcontext-Konfiguration zur Entwurfszeit</span><span class="sxs-lookup"><span data-stu-id="a5cdf-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="a5cdf-105">EF Core Entwurfszeit Tools (z. b. [Migrationen](xref:core/managing-schemas/migrations/index) ) müssen in der Lage sein, eine funktionierende Instanz eines `DbContext`-Typs zu ermitteln und zu erstellen, um Details zu den Entitäts Typen der Anwendung und deren Zuordnung zu einem Datenbankschema zu erfassen.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="a5cdf-106">Dieser Prozess kann automatisch ausgeführt werden, solange das Tool das `DbContext` problemlos so erstellen kann, dass es so konfiguriert wird, wie es zur Laufzeit konfiguriert wird.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="a5cdf-107">Obwohl jedes Muster, das die erforderlichen Konfigurationsinformationen für den `DbContext` bereitstellt, zur Laufzeit funktionieren kann, können Tools, die die Verwendung einer `DbContext` zur Entwurfszeit erfordern, nur mit einer begrenzten Anzahl von Mustern funktionieren.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="a5cdf-108">Diese werden im Abschnitt [Entwurfszeit-Kontext Erstellung](xref:core/miscellaneous/cli/dbcontext-creation) ausführlicher behandelt.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="a5cdf-109">Konfigurieren von dbcontextoptions</span><span class="sxs-lookup"><span data-stu-id="a5cdf-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="a5cdf-110">`DbContext` muss über eine Instanz von `DbContextOptions` verfügen, um die Arbeit auszuführen.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="a5cdf-111">Die Instanz `DbContextOptions` enthält Konfigurationsinformationen wie z. b.:</span><span class="sxs-lookup"><span data-stu-id="a5cdf-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="a5cdf-112">Der zu verwendende Datenbankanbieter, der in der Regel durch Aufrufen einer Methode wie `UseSqlServer` oder `UseSqlite` ausgewählt wird.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`.</span></span> <span data-ttu-id="a5cdf-113">Diese Erweiterungs Methoden erfordern das entsprechende Anbieter Paket, z. b. `Microsoft.EntityFrameworkCore.SqlServer` oder `Microsoft.EntityFrameworkCore.Sqlite`.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-113">These extension methods require the corresponding provider package, such as `Microsoft.EntityFrameworkCore.SqlServer` or `Microsoft.EntityFrameworkCore.Sqlite`.</span></span> <span data-ttu-id="a5cdf-114">Die-Methoden werden im `Microsoft.EntityFrameworkCore`-Namespace definiert.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-114">The methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span>
- <span data-ttu-id="a5cdf-115">Alle erforderlichen Verbindungs Zeichenfolgen oder Bezeichner der Daten Bank Instanz, die in der Regel als Argument an die oben genannte Anbieter Auswahlmethode weitergegeben werden.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-115">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="a5cdf-116">Beliebige optionale verhaltenselektoren auf Anbieter Ebene, die in der Regel auch innerhalb des Aufrufes der Anbieter Auswahlmethode verkettet sind</span><span class="sxs-lookup"><span data-stu-id="a5cdf-116">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="a5cdf-117">Alle allgemeinen EF Core verhaltenselektoren, die normalerweise nach oder vor der Anbieter Auswahlmethode verkettet sind</span><span class="sxs-lookup"><span data-stu-id="a5cdf-117">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="a5cdf-118">Im folgenden Beispiel wird der `DbContextOptions` für die Verwendung des SQL Server Anbieters konfiguriert, eine in der `connectionString`-Variable enthaltene Verbindung, ein Befehls Timeout auf Anbieter Ebene und ein EF Core verhaltensselektor, der alle Abfragen ausführt, die in der `DbContext` [No-Tracking](xref:core/querying/tracking#no-tracking-queries) -Anweisung ausgeführt werden. Standardmäßig:</span><span class="sxs-lookup"><span data-stu-id="a5cdf-118">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="a5cdf-119">Anbieter Auswahlmethoden und andere Verhaltens Auswahlmethoden, die oben erwähnt werden, sind Erweiterungs Methoden für `DbContextOptions` oder anbieterspezifische Options Klassen.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-119">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="a5cdf-120">Um Zugriff auf diese Erweiterungs Methoden zu erhalten, benötigen Sie möglicherweise einen Namespace (in der Regel `Microsoft.EntityFrameworkCore`) im Gültigkeitsbereich, und fügen Sie zusätzliche Paketabhängigkeiten in das Projekt ein.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-120">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="a5cdf-121">Der `DbContextOptions` kann für die `DbContext` bereitgestellt werden, indem die `OnConfiguring`-Methode überschrieben wird, oder extern über ein Konstruktorargument.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-121">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="a5cdf-122">Wenn beide verwendet werden, wird "`OnConfiguring`" zuletzt angewendet und kann für das Konstruktorargument bereitgestellte Optionen überschreiben.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-122">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="a5cdf-123">Konstruktorargument</span><span class="sxs-lookup"><span data-stu-id="a5cdf-123">Constructor argument</span></span>

<span data-ttu-id="a5cdf-124">Der Konstruktor kann einfach wie folgt eine `DbContextOptions` akzeptieren:</span><span class="sxs-lookup"><span data-stu-id="a5cdf-124">Your constructor can simply accept a `DbContextOptions` as follows:</span></span>

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
> <span data-ttu-id="a5cdf-125">Der Basiskonstruktor von dbcontext akzeptiert auch die nicht generische Version von `DbContextOptions`, aber die Verwendung der nicht generischen Version ist für Anwendungen mit mehreren Kontext Typen nicht empfehlenswert.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-125">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="a5cdf-126">Die Anwendung kann jetzt den `DbContextOptions` übergeben, wenn ein Kontext instanziiert wird, wie im folgenden dargestellt:</span><span class="sxs-lookup"><span data-stu-id="a5cdf-126">Your application can now pass the `DbContextOptions` when instantiating a context, as follows:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="a5cdf-127">Onkonfiguration</span><span class="sxs-lookup"><span data-stu-id="a5cdf-127">OnConfiguring</span></span>

<span data-ttu-id="a5cdf-128">Sie können die `DbContextOptions` auch innerhalb des Kontexts selbst initialisieren.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-128">You can also initialize the `DbContextOptions` within the context itself.</span></span> <span data-ttu-id="a5cdf-129">Obwohl Sie dieses Verfahren für die Basiskonfiguration verwenden können, müssen Sie in der Regel weiterhin bestimmte Konfigurationsdetails von außen (z. b. eine Daten bankverbindungs Zeichenfolge) erhalten.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-129">While you can use this technique for basic configuration, you will typically still need to get certain configuration details from the outside, e.g. a database connection string.</span></span> <span data-ttu-id="a5cdf-130">Dies kann mit einer Konfigurations-API oder mit einer beliebigen anderen Möglichkeit erreicht werden.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-130">This can be done with a configuration API or any other means.</span></span>

<span data-ttu-id="a5cdf-131">Um `DbContextOptions` innerhalb des Kontexts zu initialisieren, überschreiben Sie die `OnConfiguring`-Methode, und wenden Sie die Methoden auf dem bereitgestellten `DbContextOptionsBuilder` an:</span><span class="sxs-lookup"><span data-stu-id="a5cdf-131">To initialize `DbContextOptions` within the context, override the `OnConfiguring` method and call the methods on the provided `DbContextOptionsBuilder`:</span></span>

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

<span data-ttu-id="a5cdf-132">Eine Anwendung kann einen solchen Kontext einfach instanziieren, ohne etwas an den Konstruktor zu übergeben:</span><span class="sxs-lookup"><span data-stu-id="a5cdf-132">An application can simply instantiate such a context without passing anything to its constructor:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="a5cdf-133">Dieser Ansatz eignet sich nicht für Tests, es sei denn, die Tests sind auf die vollständige Datenbank ausgerichtet.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-133">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="a5cdf-134">Verwenden von dbcontext mit Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="a5cdf-134">Using DbContext with dependency injection</span></span>

<span data-ttu-id="a5cdf-135">EF Core unterstützt die Verwendung von `DbContext` mit einem Container für die Abhängigkeitsinjektion.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-135">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="a5cdf-136">Der dbcontext-Typ kann dem Dienst Container hinzugefügt werden, indem die `AddDbContext<TContext>`-Methode verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-136">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="a5cdf-137">`AddDbContext<TContext>` macht sowohl den dbcontext-Typ als auch `TContext` und den entsprechenden `DbContextOptions<TContext>` für die Injektion aus dem Dienst Container verfügbar.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-137">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="a5cdf-138">Weitere Informationen zur Abhängigkeitsinjektion finden Sie unten unter [Weitere](#more-reading) Informationen.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-138">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="a5cdf-139">Hinzufügen der `DbContext` zur Abhängigkeitsinjektion:</span><span class="sxs-lookup"><span data-stu-id="a5cdf-139">Adding the `DbContext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="a5cdf-140">Dies erfordert das Hinzufügen eines [Konstruktorarguments](#constructor-argument) zu Ihrem dbcontext-Typ, der `DbContextOptions<TContext>` akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-140">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="a5cdf-141">Kontext Code:</span><span class="sxs-lookup"><span data-stu-id="a5cdf-141">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="a5cdf-142">Anwendungscode (in ASP.net Core):</span><span class="sxs-lookup"><span data-stu-id="a5cdf-142">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="a5cdf-143">Anwendungscode (Verwenden von Service Provider direkt, weniger häufig):</span><span class="sxs-lookup"><span data-stu-id="a5cdf-143">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="avoiding-dbcontext-threading-issues"></a><span data-ttu-id="a5cdf-144">Vermeiden von dbcontext-Threading Problemen</span><span class="sxs-lookup"><span data-stu-id="a5cdf-144">Avoiding DbContext threading issues</span></span>

<span data-ttu-id="a5cdf-145">Entity Framework Core unterstützt nicht mehrere parallele Vorgänge, die auf derselben `DbContext`-Instanz ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-145">Entity Framework Core does not support multiple parallel operations being run on the same `DbContext` instance.</span></span> <span data-ttu-id="a5cdf-146">Dies schließt die parallele Ausführung von asynchronen Abfragen und jede explizite gleichzeitige Verwendung mehrerer Threads ein.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-146">This includes both parallel execution of async queries and any explicit concurrent use from multiple threads.</span></span> <span data-ttu-id="a5cdf-147">Aus diesem Grund werden asynchrone Aufrufe von immer `await` ausgeführt, oder es werden separate `DbContext`-Instanzen für Vorgänge verwendet, die parallel ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-147">Therefore, always `await` async calls immediately, or use separate `DbContext` instances for operations that execute in parallel.</span></span>

<span data-ttu-id="a5cdf-148">Wenn EF Core den Versuch erkennt, eine `DbContext`-Instanz gleichzeitig zu verwenden, wird eine `InvalidOperationException` mit einer Meldung wie die folgende angezeigt:</span><span class="sxs-lookup"><span data-stu-id="a5cdf-148">When EF Core detects an attempt to use a `DbContext` instance concurrently, you'll see an `InvalidOperationException` with a message like this:</span></span>

> <span data-ttu-id="a5cdf-149">Ein zweiter Vorgang, der in diesem Kontext gestartet wurde, bevor ein vorheriger Vorgang abgeschlossen wurde.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-149">A second operation started on this context before a previous operation completed.</span></span> <span data-ttu-id="a5cdf-150">Dies wird in der Regel durch unterschiedliche Threads verursacht, die dieselbe Instanz von dbcontext verwenden, aber es ist nicht garantiert, dass Instanzmember Thread sicher sind.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-150">This is usually caused by different threads using the same instance of DbContext, however instance members are not guaranteed to be thread safe.</span></span>

<span data-ttu-id="a5cdf-151">Wenn der gleichzeitige Zugriff nicht erkannt wird, kann dies zu nicht definiertem Verhalten, Anwendungs abstürzen und Daten Beschädigung führen.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-151">When concurrent access goes undetected, it can result in undefined behavior, application crashes and data corruption.</span></span>

<span data-ttu-id="a5cdf-152">Es gibt häufige Fehler, die versehentlich den gleichzeitigen Zugriff auf dieselbe `DbContext`-Instanz verursachen können:</span><span class="sxs-lookup"><span data-stu-id="a5cdf-152">There are common mistakes that can inadvertently cause concurrent access on the same `DbContext` instance:</span></span>

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a><span data-ttu-id="a5cdf-153">Vergessen Sie nicht, auf den Abschluss eines asynchronen Vorgangs zu warten, bevor Sie einen anderen Vorgang im gleichen dbcontext starten.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-153">Forgetting to await the completion of an asynchronous operation before starting any other operation on the same DbContext</span></span>

<span data-ttu-id="a5cdf-154">Asynchrone Methoden ermöglichen EF Core das Initiieren von Vorgängen, die auf eine nicht blockierende Weise auf die Datenbank zugreifen.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-154">Asynchronous methods enable EF Core to initiate operations that access the database in a non-blocking way.</span></span> <span data-ttu-id="a5cdf-155">Wenn ein Aufrufer jedoch nicht auf den Abschluss einer dieser Methoden wartet und weiterhin andere Vorgänge für die `DbContext` ausführt, kann der Status des `DbContext` sein, (und sehr wahrscheinlich sein) beschädigt.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-155">But if a caller does not await the completion of one of these methods, and proceeds to perform other operations on the `DbContext`, the state of the `DbContext` can be, (and very likely will be) corrupted.</span></span>

<span data-ttu-id="a5cdf-156">Asynchrone Methoden sollten immer sofort EF Core werden.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-156">Always await EF Core asynchronous methods immediately.</span></span>  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a><span data-ttu-id="a5cdf-157">Implizites Freigeben von dbcontext-Instanzen über mehrere Threads über Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="a5cdf-157">Implicitly sharing DbContext instances across multiple threads via dependency injection</span></span>

<span data-ttu-id="a5cdf-158">Die [`AddDbContext`-](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) Erweiterungsmethode registriert `DbContext`-Typen standardmäßig mit einer Bereichs bezogenen Gültigkeits [Dauer](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) .</span><span class="sxs-lookup"><span data-stu-id="a5cdf-158">The [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) extension method registers `DbContext` types with a [scoped lifetime](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) by default.</span></span>

<span data-ttu-id="a5cdf-159">Dies ist vor gleichzeitigen Zugriffsproblemen in ASP.net Core Anwendungen sicher, weil nur ein Thread zu einem bestimmten Zeitpunkt jede Client Anforderung ausführt, und weil jede Anforderung einen separaten Abhängigkeits einschleusungs Bereich erhält (und somit eine separate `DbContext`-Instanz).</span><span class="sxs-lookup"><span data-stu-id="a5cdf-159">This is safe from concurrent access issues in ASP.NET Core applications because there is only one thread executing each client request at a given time, and because each request gets a separate dependency injection scope (and therefore a separate `DbContext` instance).</span></span>

<span data-ttu-id="a5cdf-160">Jeder Code, der explizit mehrere Threads parallel ausführt, sollte jedoch sicherstellen, dass auf `DbContext`-Instanzen nicht gleichzeitig zugegriffen wird.</span><span class="sxs-lookup"><span data-stu-id="a5cdf-160">However any code that explicitly executes multiple threads in parallel should ensure that `DbContext` instances aren't ever accessed concurrently.</span></span>

<span data-ttu-id="a5cdf-161">Mithilfe der Abhängigkeitsinjektion kann dies erreicht werden, indem der Kontext entweder als Bereichs bezogen registriert und Bereiche (mit `IServiceScopeFactory`) für jeden Thread registriert werden, oder indem die `DbContext` als vorübergehender Wert registriert wird (mithilfe der Überladung von `AddDbContext`, der einen `ServiceLifetime`-Parameter annimmt).</span><span class="sxs-lookup"><span data-stu-id="a5cdf-161">Using dependency injection, this can be achieved by either registering the context as scoped and creating scopes (using `IServiceScopeFactory`) for each thread, or by registering the `DbContext` as transient (using the overload of `AddDbContext` which takes a `ServiceLifetime` parameter).</span></span>

## <a name="more-reading"></a><span data-ttu-id="a5cdf-162">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="a5cdf-162">More reading</span></span>

- <span data-ttu-id="a5cdf-163">Weitere Informationen zur Verwendung von di finden Sie unter [Abhängigkeitsinjektion](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) .</span><span class="sxs-lookup"><span data-stu-id="a5cdf-163">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
- <span data-ttu-id="a5cdf-164">Weitere Informationen finden Sie unter [Testen](testing/index.md) .</span><span class="sxs-lookup"><span data-stu-id="a5cdf-164">Read [Testing](testing/index.md) for more information.</span></span>
