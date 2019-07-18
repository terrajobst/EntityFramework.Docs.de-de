---
title: Konfigurieren eines dbcontext-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: ddabf825ef23c2ec07efcde390df7d0cf48db33c
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306507"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="81e97-102">Konfigurieren eines dbcontext</span><span class="sxs-lookup"><span data-stu-id="81e97-102">Configuring a DbContext</span></span>

<span data-ttu-id="81e97-103">In diesem Artikel werden grundlegende Muster für `DbContext` die Konfiguration `DbContextOptions` eines über eine veranschaulicht, um mithilfe eines bestimmten EF Core Anbieters und optionalem Verhalten eine Verbindung mit einer Datenbank herzustellen.</span><span class="sxs-lookup"><span data-stu-id="81e97-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="81e97-104">Dbcontext-Konfiguration zur Entwurfszeit</span><span class="sxs-lookup"><span data-stu-id="81e97-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="81e97-105">EF Core Entwurfszeit Tools (z. b. [Migrationen](xref:core/managing-schemas/migrations/index) ) müssen in der Lage sein, eine funktionierende Instanz `DbContext` eines Typs zu ermitteln und zu erstellen, um Details zu den Entitäts Typen der Anwendung und deren Zuordnung zu einem Datenbankschema zu erfassen.</span><span class="sxs-lookup"><span data-stu-id="81e97-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="81e97-106">Dieser Prozess kann automatisch ausgeführt werden, solange das Tool auf einfache Weise so `DbContext` erstellen kann, dass es so konfiguriert wird, wie es zur Laufzeit konfiguriert wird.</span><span class="sxs-lookup"><span data-stu-id="81e97-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="81e97-107">Obwohl jedes Muster, das die erforderlichen Konfigurationsinformationen für `DbContext` bereitstellt, zur Laufzeit funktionieren kann, können Tools, die die `DbContext` Verwendung von zur Entwurfszeit erfordern, nur mit einer begrenzten Anzahl von Mustern funktionieren.</span><span class="sxs-lookup"><span data-stu-id="81e97-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="81e97-108">Diese werden im Abschnitt [Entwurfszeit-Kontext Erstellung](xref:core/miscellaneous/cli/dbcontext-creation) ausführlicher behandelt.</span><span class="sxs-lookup"><span data-stu-id="81e97-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="81e97-109">Konfigurieren von dbcontextoptions</span><span class="sxs-lookup"><span data-stu-id="81e97-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="81e97-110">`DbContext`muss über eine Instanz von `DbContextOptions` verfügen, um die Arbeit auszuführen.</span><span class="sxs-lookup"><span data-stu-id="81e97-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="81e97-111">Die `DbContextOptions` -Instanz enthält Konfigurationsinformationen wie z. b.:</span><span class="sxs-lookup"><span data-stu-id="81e97-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="81e97-112">Der zu verwendende Datenbankanbieter, der in der Regel durch Aufrufen `UseSqlServer` einer `UseSqlite`Methode wie oder ausgewählt wird.</span><span class="sxs-lookup"><span data-stu-id="81e97-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`.</span></span> <span data-ttu-id="81e97-113">Diese Erweiterungs Methoden erfordern das entsprechende Anbieter Paket, z `Microsoft.EntityFrameworkCore.SqlServer` . b. oder. `Microsoft.EntityFrameworkCore.Sqlite`</span><span class="sxs-lookup"><span data-stu-id="81e97-113">These extension methods require the corresponding provider package, such as `Microsoft.EntityFrameworkCore.SqlServer` or `Microsoft.EntityFrameworkCore.Sqlite`.</span></span> <span data-ttu-id="81e97-114">Die-Methoden werden im `Microsoft.EntityFrameworkCore` -Namespace definiert.</span><span class="sxs-lookup"><span data-stu-id="81e97-114">The methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span>
- <span data-ttu-id="81e97-115">Alle erforderlichen Verbindungs Zeichenfolgen oder Bezeichner der Daten Bank Instanz, die in der Regel als Argument an die oben genannte Anbieter Auswahlmethode weitergegeben werden.</span><span class="sxs-lookup"><span data-stu-id="81e97-115">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="81e97-116">Beliebige optionale verhaltenselektoren auf Anbieter Ebene, die in der Regel auch innerhalb des Aufrufes der Anbieter Auswahlmethode verkettet sind</span><span class="sxs-lookup"><span data-stu-id="81e97-116">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="81e97-117">Alle allgemeinen EF Core verhaltenselektoren, die normalerweise nach oder vor der Anbieter Auswahlmethode verkettet sind</span><span class="sxs-lookup"><span data-stu-id="81e97-117">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="81e97-118">Im folgenden Beispiel wird konfiguriert `DbContextOptions` , um den SQL Server-Anbieter, eine in der `connectionString` -Variable enthaltene Verbindung, ein Befehls Timeout auf Anbieter Ebene und eine EF Core Verhaltens Auswahl zu verwenden, die `DbContext` alle Abfragen im standardmäßig [keine Nachverfolgung](xref:core/querying/tracking#no-tracking-queries) :</span><span class="sxs-lookup"><span data-stu-id="81e97-118">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="81e97-119">Anbieter Auswahlmethoden und andere Verhaltens Auswahlmethoden, die oben erwähnt werden, `DbContextOptions` sind Erweiterungs Methoden für oder anbieterspezifische Options Klassen.</span><span class="sxs-lookup"><span data-stu-id="81e97-119">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="81e97-120">Um Zugriff auf diese Erweiterungs Methoden zu erhalten, benötigen Sie möglicherweise einen Namespace (in der `Microsoft.EntityFrameworkCore`Regel) im Gültigkeitsbereich und fügen zusätzliche Paketabhängigkeiten in das Projekt ein.</span><span class="sxs-lookup"><span data-stu-id="81e97-120">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="81e97-121">Kann für das `DbContext` bereitgestellt werden, indem die `OnConfiguring` -Methode überschrieben wird, oder extern über ein Konstruktorargument. `DbContextOptions`</span><span class="sxs-lookup"><span data-stu-id="81e97-121">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="81e97-122">Wenn beide verwendet werden, `OnConfiguring` wird zuletzt angewendet, und die für das Konstruktorargument bereitgestellten Optionen können überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="81e97-122">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="81e97-123">Konstruktorargument</span><span class="sxs-lookup"><span data-stu-id="81e97-123">Constructor argument</span></span>

<span data-ttu-id="81e97-124">Kontext Code mit Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="81e97-124">Context code with constructor:</span></span>

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
> <span data-ttu-id="81e97-125">Der Basiskonstruktor von dbcontext akzeptiert auch die nicht generische Version von `DbContextOptions`, aber die Verwendung der nicht generischen Version wird nicht für Anwendungen mit mehreren Kontext Typen empfohlen.</span><span class="sxs-lookup"><span data-stu-id="81e97-125">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="81e97-126">Anwendungscode, der aus dem Konstruktorargument initialisiert werden soll:</span><span class="sxs-lookup"><span data-stu-id="81e97-126">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="81e97-127">Onkonfiguration</span><span class="sxs-lookup"><span data-stu-id="81e97-127">OnConfiguring</span></span>

<span data-ttu-id="81e97-128">Kontext Code mit `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="81e97-128">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="81e97-129">Anwendungscode zum Initialisieren einer `DbContext` , die `OnConfiguring`Folgendes verwendet:</span><span class="sxs-lookup"><span data-stu-id="81e97-129">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="81e97-130">Dieser Ansatz eignet sich nicht für Tests, es sei denn, die Tests sind auf die vollständige Datenbank ausgerichtet.</span><span class="sxs-lookup"><span data-stu-id="81e97-130">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="81e97-131">Verwenden von dbcontext mit Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="81e97-131">Using DbContext with dependency injection</span></span>

<span data-ttu-id="81e97-132">EF Core unterstützt `DbContext` die Verwendung von mit einem Container für Abhängigkeitsinjektion.</span><span class="sxs-lookup"><span data-stu-id="81e97-132">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="81e97-133">Der dbcontext-Typ kann dem Dienst Container mithilfe der `AddDbContext<TContext>` -Methode hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="81e97-133">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="81e97-134">`AddDbContext<TContext>`von werden sowohl der dbcontext-Typ `TContext`als auch der entsprechende `DbContextOptions<TContext>` für Injection aus dem Dienst Container verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="81e97-134">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="81e97-135">Weitere Informationen zur Abhängigkeitsinjektion finden Sie unten unter [Weitere](#more-reading) Informationen.</span><span class="sxs-lookup"><span data-stu-id="81e97-135">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="81e97-136">Hinzufügen von zur Abhängigkeitsinjektion: `DbContext`</span><span class="sxs-lookup"><span data-stu-id="81e97-136">Adding the `DbContext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="81e97-137">Dies erfordert das Hinzufügen eines [Konstruktorarguments](#constructor-argument) zu Ihrem dbcontext- `DbContextOptions<TContext>`Typ, der akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="81e97-137">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="81e97-138">Kontext Code:</span><span class="sxs-lookup"><span data-stu-id="81e97-138">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="81e97-139">Anwendungscode (in ASP.net Core):</span><span class="sxs-lookup"><span data-stu-id="81e97-139">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="81e97-140">Anwendungscode (Verwenden von Service Provider direkt, weniger häufig):</span><span class="sxs-lookup"><span data-stu-id="81e97-140">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```
## <a name="avoiding-dbcontext-threading-issues"></a><span data-ttu-id="81e97-141">Vermeiden von dbcontext-Threading Problemen</span><span class="sxs-lookup"><span data-stu-id="81e97-141">Avoiding DbContext threading issues</span></span>

<span data-ttu-id="81e97-142">Entity Framework Core unterstützt nicht mehrere parallele Vorgänge, die auf derselben `DbContext` Instanz ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="81e97-142">Entity Framework Core does not support multiple parallel operations being run on the same `DbContext` instance.</span></span> <span data-ttu-id="81e97-143">Dies schließt die parallele Ausführung von asynchronen Abfragen und jede explizite gleichzeitige Verwendung mehrerer Threads ein.</span><span class="sxs-lookup"><span data-stu-id="81e97-143">This includes both parallel execution of async queries and any explicit concurrent use from multiple threads.</span></span> <span data-ttu-id="81e97-144">Daher wird immer `await` Async sofort aufgerufen, oder es werden `DbContext` separate Instanzen für Vorgänge verwendet, die parallel ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="81e97-144">Therefore, always `await` async calls immediately, or use separate `DbContext` instances for operations that execute in parallel.</span></span>

<span data-ttu-id="81e97-145">Wenn EF Core den Versuch erkennt, eine `DbContext` -Instanz gleichzeitig zu verwenden, wird eine mit einer Meldung wie die `InvalidOperationException` folgende angezeigt:</span><span class="sxs-lookup"><span data-stu-id="81e97-145">When EF Core detects an attempt to use a `DbContext` instance concurrently, you'll see an `InvalidOperationException` with a message like this:</span></span> 

> <span data-ttu-id="81e97-146">Ein zweiter Vorgang, der in diesem Kontext gestartet wurde, bevor ein vorheriger Vorgang abgeschlossen wurde.</span><span class="sxs-lookup"><span data-stu-id="81e97-146">A second operation started on this context before a previous operation completed.</span></span> <span data-ttu-id="81e97-147">Dies wird in der Regel durch unterschiedliche Threads verursacht, die dieselbe Instanz von dbcontext verwenden, aber es ist nicht garantiert, dass Instanzmember Thread sicher sind.</span><span class="sxs-lookup"><span data-stu-id="81e97-147">This is usually caused by different threads using the same instance of DbContext, however instance members are not guaranteed to be thread safe.</span></span>

<span data-ttu-id="81e97-148">Wenn der gleichzeitige Zugriff nicht erkannt wird, kann dies zu nicht definiertem Verhalten, Anwendungs abstürzen und Daten Beschädigung führen.</span><span class="sxs-lookup"><span data-stu-id="81e97-148">When concurrent access goes undetected, it can result in undefined behavior, application crashes and data corruption.</span></span>

<span data-ttu-id="81e97-149">Es gibt häufige Fehler, die versehentlich den gleichzeitigen Zugriff auf dieselbe `DbContext` Instanz verursachen können:</span><span class="sxs-lookup"><span data-stu-id="81e97-149">There are common mistakes that can inadvertently cause concurrent access on the same `DbContext` instance:</span></span>

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a><span data-ttu-id="81e97-150">Vergessen Sie nicht, auf den Abschluss eines asynchronen Vorgangs zu warten, bevor Sie einen anderen Vorgang im gleichen dbcontext starten.</span><span class="sxs-lookup"><span data-stu-id="81e97-150">Forgetting to await the completion of an asynchronous operation before starting any other operation on the same DbContext</span></span>

<span data-ttu-id="81e97-151">Asynchrone Methoden ermöglichen EF Core das Initiieren von Vorgängen, die auf eine nicht blockierende Weise auf die Datenbank zugreifen.</span><span class="sxs-lookup"><span data-stu-id="81e97-151">Asynchronous methods enable EF Core to initiate operations that access the database in a non-blocking way.</span></span> <span data-ttu-id="81e97-152">Wenn ein Aufrufer jedoch nicht auf den Abschluss einer dieser Methoden wartet und weiterhin andere Vorgänge auf dem `DbContext`ausführt, `DbContext` kann der Zustand von (und wahrscheinlich) beschädigt sein.</span><span class="sxs-lookup"><span data-stu-id="81e97-152">But if a caller does not await the completion of one of these methods, and proceeds to perform other operations on the `DbContext`, the state of the `DbContext` can be, (and very likely will be) corrupted.</span></span> 

<span data-ttu-id="81e97-153">Asynchrone Methoden sollten immer sofort EF Core werden.</span><span class="sxs-lookup"><span data-stu-id="81e97-153">Always await EF Core asynchronous methods immediately.</span></span>  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a><span data-ttu-id="81e97-154">Implizites Freigeben von dbcontext-Instanzen über mehrere Threads über Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="81e97-154">Implicitly sharing DbContext instances across multiple threads via dependency injection</span></span>

<span data-ttu-id="81e97-155">Die [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) Erweiterungsmethode registriert `DbContext` Typen standardmäßig mit einer Gültigkeits [Dauer](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) .</span><span class="sxs-lookup"><span data-stu-id="81e97-155">The [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) extension method registers `DbContext` types with a [scoped lifetime](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) by default.</span></span> 

<span data-ttu-id="81e97-156">Dies ist vor gleichzeitigen Zugriffsproblemen in ASP.net Core Anwendungen sicher, weil nur ein Thread zu einem bestimmten Zeitpunkt jede Client Anforderung ausführt, und weil jede Anforderung einen separaten Abhängigkeits einschleusungs Bereich erhält ( `DbContext` und somit eine separate Instanz).</span><span class="sxs-lookup"><span data-stu-id="81e97-156">This is safe from concurrent access issues in ASP.NET Core applications because there is only one thread executing each client request at a given time, and because each request gets a separate dependency injection scope (and therefore a separate `DbContext` instance).</span></span>

<span data-ttu-id="81e97-157">Jeder Code, der explizit mehrere Threads parallel ausführt, sollte jedoch sicher `DbContext` stellen, dass auf Instanzen nicht gleichzeitig zugegriffen wird.</span><span class="sxs-lookup"><span data-stu-id="81e97-157">However any code that explicitly executes multiple threads in parallel should ensure that `DbContext` instances aren't ever accessed concurrently.</span></span>

<span data-ttu-id="81e97-158">Mithilfe der Abhängigkeitsinjektion kann dies erreicht werden, indem der Kontext entweder als Bereichs bezogen registriert wird und Bereiche `IServiceScopeFactory`(mit) für die einzelnen Threads erstellt werden `DbContext` , oder indem der als vorübergehender `AddDbContext` registriert wird (mithilfe der Überladung von, die einen `ServiceLifetime` -Parameter).</span><span class="sxs-lookup"><span data-stu-id="81e97-158">Using dependency injection, this can be achieved by either registering the context as scoped and creating scopes (using `IServiceScopeFactory`) for each thread, or by registering the `DbContext` as transient (using the overload of `AddDbContext` which takes a `ServiceLifetime` parameter).</span></span>

## <a name="more-reading"></a><span data-ttu-id="81e97-159">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="81e97-159">More reading</span></span>

* <span data-ttu-id="81e97-160">Weitere Informationen zur Verwendung von EF mit ASP.net Core finden Sie [unter Getting Started on ASP.net Core](../get-started/aspnetcore/index.md) .</span><span class="sxs-lookup"><span data-stu-id="81e97-160">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="81e97-161">Weitere Informationen zur Verwendung von di finden Sie unter [Abhängigkeitsinjektion](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) .</span><span class="sxs-lookup"><span data-stu-id="81e97-161">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="81e97-162">Weitere Informationen finden Sie unter [Testen](testing/index.md) .</span><span class="sxs-lookup"><span data-stu-id="81e97-162">Read [Testing](testing/index.md) for more information.</span></span>
