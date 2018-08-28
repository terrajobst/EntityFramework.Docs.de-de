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
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="38e3a-102">Konfigurieren einen "DbContext"</span><span class="sxs-lookup"><span data-stu-id="38e3a-102">Configuring a DbContext</span></span>

<span data-ttu-id="38e3a-103">In diesem Artikel wird die grundlegende Muster für die Konfiguration einer `DbContext` über eine `DbContextOptions` zur Verbindung mit einer Datenbank mithilfe eines bestimmten Anbieters von EF Core und optionales Verhalten.</span><span class="sxs-lookup"><span data-stu-id="38e3a-103">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="38e3a-104">Während der Entwurfszeit "DbContext"-Konfiguration</span><span class="sxs-lookup"><span data-stu-id="38e3a-104">Design-time DbContext configuration</span></span>

<span data-ttu-id="38e3a-105">EF Core Entwurfszeit-tools, z. B. [Migrationen](xref:core/managing-schemas/migrations/index) müssen in der Lage, um zu ermitteln, und erstellen eine Arbeitsinstanz von einem `DbContext` Typ sammeln Informationen zu der Anwendung Entitätstypen und wie sie ein Datenbankschema zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="38e3a-105">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="38e3a-106">Dieser Prozess kann automatisch erfolgen, solange das Tool leicht erstellen kann die `DbContext` so, dass sie auf ähnliche Weise konfiguriert werden soll, wie es zur Laufzeit konfiguriert werden sollen.</span><span class="sxs-lookup"><span data-stu-id="38e3a-106">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="38e3a-107">Während jedes Muster, das die erforderlichen Konfigurationsinformationen, bietet der `DbContext` können arbeiten, zur Laufzeit, Tools, die mit erfordern eine `DbContext` zur Entwurfszeit funktioniert nur mit einer begrenzten Anzahl von Mustern.</span><span class="sxs-lookup"><span data-stu-id="38e3a-107">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="38e3a-108">Diese werden in ausführlicher behandelt die [während der Entwurfszeit beim Erstellen des Serverkontexts](xref:core/miscellaneous/cli/dbcontext-creation) Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="38e3a-108">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="38e3a-109">Konfigurieren der "dbcontextoptions"</span><span class="sxs-lookup"><span data-stu-id="38e3a-109">Configuring DbContextOptions</span></span>

<span data-ttu-id="38e3a-110">`DbContext` eine Instanz des `DbContextOptions` um ausführen jeglicher arbeiten.</span><span class="sxs-lookup"><span data-stu-id="38e3a-110">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="38e3a-111">Die `DbContextOptions` Instanz enthält Konfigurationsinformationen, z.B.:</span><span class="sxs-lookup"><span data-stu-id="38e3a-111">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="38e3a-112">Die zu verwendenden Datenbankanbieter, in der Regel ausgewählt wird, durch Aufrufen einer Methode wie z. B. `UseSqlServer` oder `UseSqlite`</span><span class="sxs-lookup"><span data-stu-id="38e3a-112">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`</span></span>
- <span data-ttu-id="38e3a-113">Eine erforderliche Verbindungszeichenfolge oder die ID der Datenbankinstanz, in der Regel als Argument übergeben, der oben genannten Anbieter Selection-Methode</span><span class="sxs-lookup"><span data-stu-id="38e3a-113">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="38e3a-114">Alle auf Anbieterebene optionales Verhalten-Selektoren, die in der Regel auch verkettet innerhalb des Aufrufs an die Anbieter-Auswahl-Methode</span><span class="sxs-lookup"><span data-stu-id="38e3a-114">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="38e3a-115">Alle allgemeinen Selektoren zur EF Core-Verhalten, verkettet in der Regel hinter oder vor der Anbieter-Selektor-Methode</span><span class="sxs-lookup"><span data-stu-id="38e3a-115">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="38e3a-116">Im folgenden Beispiel wird die `DbContextOptions` für die Verwendung den SQL Server-Anbieter eine Verbindung enthalten sind, der `connectionString` Variable, ein auf Anbieterebene Befehlstimeout und eine Auswahl der EF Core-Verhalten, die alle Abfragen ausgeführt werden die `DbContext` [ohne nachverfolgung](xref:core/querying/tracking#no-tracking-queries) standardmäßig:</span><span class="sxs-lookup"><span data-stu-id="38e3a-116">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="38e3a-117">Anbieter-Selektor-Methoden und andere Verhalten Selector-Methoden, die oben genannten Erweiterungsmethoden befinden sich auf `DbContextOptions` oder Klassen von Anbieter-spezifische Option.</span><span class="sxs-lookup"><span data-stu-id="38e3a-117">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="38e3a-118">Um den Zugriff auf diese besonderen Erweiterungsmethoden zu erhalten, Sie möglicherweise haben einen Namespace müssen (in der Regel `Microsoft.EntityFrameworkCore`) im Bereich und die Abhängigkeiten von zusätzlichen Paketen im Projekt enthalten.</span><span class="sxs-lookup"><span data-stu-id="38e3a-118">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="38e3a-119">Die `DbContextOptions` bereitgestellt werden kann die `DbContext` durch Überschreiben der `OnConfiguring` Methode oder extern über ein Konstruktorargument.</span><span class="sxs-lookup"><span data-stu-id="38e3a-119">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="38e3a-120">Wenn beide verwendet werden, `OnConfiguring` zuletzt angewendet wird, und können überschreiben Optionen, die das Konstruktorargument angegeben.</span><span class="sxs-lookup"><span data-stu-id="38e3a-120">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="38e3a-121">Konstruktorargument</span><span class="sxs-lookup"><span data-stu-id="38e3a-121">Constructor argument</span></span>

<span data-ttu-id="38e3a-122">Kontextcode mit Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="38e3a-122">Context code with constructor:</span></span>

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
> <span data-ttu-id="38e3a-123">Basiskonstruktor der "DbContext" akzeptiert auch die nicht generische Version von `DbContextOptions`, die nicht generische Version wird jedoch nicht empfohlen für Anwendungen mit mehreren Arten von Kontext.</span><span class="sxs-lookup"><span data-stu-id="38e3a-123">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="38e3a-124">Der Anwendungscode Konstruktorargument nicht initialisieren:</span><span class="sxs-lookup"><span data-stu-id="38e3a-124">Application code to initialize from constructor argument:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="38e3a-125">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="38e3a-125">OnConfiguring</span></span>

<span data-ttu-id="38e3a-126">Kontextcode mit `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="38e3a-126">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="38e3a-127">Anwendungscode zum Initialisieren einer `DbContext` verwendet `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="38e3a-127">Application code to initialize a `DbContext` that uses `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="38e3a-128">Dieser Ansatz ist nicht selbst für Tests geeignet sind, wenn die Tests die vollständige Datenbank als Ziel.</span><span class="sxs-lookup"><span data-stu-id="38e3a-128">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="38e3a-129">Mithilfe von "DbContext" über Dependency injection</span><span class="sxs-lookup"><span data-stu-id="38e3a-129">Using DbContext with dependency injection</span></span>

<span data-ttu-id="38e3a-130">EF Core unterstützt die Verwendung von `DbContext` mit DI-Containern.</span><span class="sxs-lookup"><span data-stu-id="38e3a-130">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="38e3a-131">Ihr "DbContext"-Typ kann zum Dienstcontainer hinzugefügt werden, mithilfe der `AddDbContext<TContext>` Methode.</span><span class="sxs-lookup"><span data-stu-id="38e3a-131">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="38e3a-132">`AddDbContext<TContext>` veranlasst, dass sowohl Ihr "DbContext"-Typ `TContext`, und die entsprechende `DbContextOptions<TContext>` zur Einschleusung von Befehlen aus dem Dienstcontainer.</span><span class="sxs-lookup"><span data-stu-id="38e3a-132">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="38e3a-133">Finden Sie unter [Weitere lesen](#more-reading) unten Weitere Informationen zur Abhängigkeitsinjektion.</span><span class="sxs-lookup"><span data-stu-id="38e3a-133">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="38e3a-134">Hinzufügen der `Dbcontext` zur Dependency Injection:</span><span class="sxs-lookup"><span data-stu-id="38e3a-134">Adding the `Dbcontext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="38e3a-135">Dies erfordert das Hinzufügen einer [Konstruktorargument](#constructor-argument) in den "DbContext"-Typ, der akzeptiert `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="38e3a-135">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="38e3a-136">Kontextcode:</span><span class="sxs-lookup"><span data-stu-id="38e3a-136">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="38e3a-137">Der Anwendungscode (in ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="38e3a-137">Application code (in ASP.NET Core):</span></span>

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

<span data-ttu-id="38e3a-138">Anwendungscode (mithilfe von ServiceProvider direkt nicht so üblich):</span><span class="sxs-lookup"><span data-stu-id="38e3a-138">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a><span data-ttu-id="38e3a-139">Weitere Informationen</span><span class="sxs-lookup"><span data-stu-id="38e3a-139">More reading</span></span>

* <span data-ttu-id="38e3a-140">Lesen [erste Schritte in ASP.NET Core](../get-started/aspnetcore/index.md) für Weitere Informationen zur Verwendung von EF mit ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="38e3a-140">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="38e3a-141">Lesen [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) Informationen mithilfe der Abhängigkeitsinjektion.</span><span class="sxs-lookup"><span data-stu-id="38e3a-141">Read [Dependency Injection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
* <span data-ttu-id="38e3a-142">Lesen [Tests](testing/index.md) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="38e3a-142">Read [Testing](testing/index.md) for more information.</span></span>
