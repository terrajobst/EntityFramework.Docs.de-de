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
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="ea080-102">Konfigurieren ein ' DbContext '</span><span class="sxs-lookup"><span data-stu-id="ea080-102">Configuring a DbContext</span></span>

<span data-ttu-id="ea080-103">In diesem Artikel werden die unzulässigen Muster für die Konfiguration einer `DbContext` mit `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="ea080-103">This article shows patterns for configuring a `DbContext` with `DbContextOptions`.</span></span> <span data-ttu-id="ea080-104">Optionen werden in erster Linie zum auswählen und konfigurieren den Datenspeicher.</span><span class="sxs-lookup"><span data-stu-id="ea080-104">Options are primarily used to select and configure the data store.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="ea080-105">Konfigurieren von DbContextOptions</span><span class="sxs-lookup"><span data-stu-id="ea080-105">Configuring DbContextOptions</span></span>

<span data-ttu-id="ea080-106">`DbContext`eine Instanz des `DbContextOptions` um auszuführen.</span><span class="sxs-lookup"><span data-stu-id="ea080-106">`DbContext` must have an instance of `DbContextOptions` in order to execute.</span></span> <span data-ttu-id="ea080-107">Dies kann durch Außerkraftsetzen konfiguriert werden `OnConfiguring`, oder extern über einen Konstruktorargument angegeben.</span><span class="sxs-lookup"><span data-stu-id="ea080-107">This can be configured by overriding `OnConfiguring`, or supplied externally via a constructor argument.</span></span>

<span data-ttu-id="ea080-108">Wenn beide verwendet werden, `OnConfiguring` ausgeführt wird, auf den angegebenen Optionen, d. h., es ist Additive und kann für Konstruktorarguments angegebenen Optionen für das überschreiben.</span><span class="sxs-lookup"><span data-stu-id="ea080-108">If both are used, `OnConfiguring` is executed on the supplied options, meaning it is additive and can overwrite  options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="ea080-109">Konstruktorargument</span><span class="sxs-lookup"><span data-stu-id="ea080-109">Constructor argument</span></span>

<span data-ttu-id="ea080-110">Kontextcode mit-Konstruktor</span><span class="sxs-lookup"><span data-stu-id="ea080-110">Context code with constructor</span></span>

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
> <span data-ttu-id="ea080-111">Die Basis DbContext-Konstruktor akzeptiert auch die nicht generische Version der `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="ea080-111">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`.</span></span> <span data-ttu-id="ea080-112">Verwenden die nicht generische Version wird nicht für Anwendungen mit mehreren Kontexttypen empfohlen.</span><span class="sxs-lookup"><span data-stu-id="ea080-112">Using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="ea080-113">Anwendungscode Konstruktorargument Initialisierung</span><span class="sxs-lookup"><span data-stu-id="ea080-113">Application code to initialize from constructor argument</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="ea080-114">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="ea080-114">OnConfiguring</span></span>

> [!WARNING]  
> <span data-ttu-id="ea080-115">`OnConfiguring`Tritt auf, zuletzt und Optionen, die aus abgerufen, DI oder der Konstruktor überschreiben können.</span><span class="sxs-lookup"><span data-stu-id="ea080-115">`OnConfiguring` occurs last and can overwrite options obtained from DI or the constructor.</span></span> <span data-ttu-id="ea080-116">Dieser Ansatz ist nicht verleiten testen (es sei denn, Sie die vollständige Datenbank ausgerichtet sein).</span><span class="sxs-lookup"><span data-stu-id="ea080-116">This approach does not lend itself to testing (unless you target the full database).</span></span>

<span data-ttu-id="ea080-117">Kontextcode mit `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="ea080-117">Context code with `OnConfiguring`:</span></span>

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

<span data-ttu-id="ea080-118">Anwendungscode bei `OnConfiguring`:</span><span class="sxs-lookup"><span data-stu-id="ea080-118">Application code to initialize with `OnConfiguring`:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

## <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="ea080-119">Verwenden von ' DbContext ' mit Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="ea080-119">Using DbContext with dependency injection</span></span>

<span data-ttu-id="ea080-120">EF unterstützt die Verwendung von `DbContext` mit einen abhängigkeitseinschleusungscontainer.</span><span class="sxs-lookup"><span data-stu-id="ea080-120">EF supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="ea080-121">Der DbContext-Typ kann dem Dienstcontainer hinzugefügt werden, mithilfe von `AddDbContext<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="ea080-121">Your DbContext type can be added to the service container by using `AddDbContext<TContext>`.</span></span>

<span data-ttu-id="ea080-122">`AddDbContext`veranlasst, dass sowohl die DbContext-Typ `TContext`, und `DbContextOptions<TContext>` für Injection aus dem Dienstcontainer verfügbar.</span><span class="sxs-lookup"><span data-stu-id="ea080-122">`AddDbContext` will make both your DbContext type, `TContext`, and `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="ea080-123">Finden Sie unter [Weitere lesen](#more-reading) unten Informationen über die Abhängigkeitsinjektion.</span><span class="sxs-lookup"><span data-stu-id="ea080-123">See [more reading](#more-reading) below for information on dependency injection.</span></span>

<span data-ttu-id="ea080-124">Abhängigkeitsinjektion Dbcontext hinzugefügt</span><span class="sxs-lookup"><span data-stu-id="ea080-124">Adding dbcontext to dependency injection</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="ea080-125">Dies erfordert, Hinzufügen einer [Konstruktorargument](#constructor-argument) in den DbContext-Typ, das akzeptiert `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="ea080-125">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions`.</span></span>

<span data-ttu-id="ea080-126">Kontextcode:</span><span class="sxs-lookup"><span data-stu-id="ea080-126">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="ea080-127">Der Anwendungscode (in ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="ea080-127">Application code (in ASP.NET Core):</span></span>

``` csharp
public MyController(BloggingContext context)
```

<span data-ttu-id="ea080-128">Der Anwendungscode (mithilfe von ServiceProvider direkt, weniger übliche):</span><span class="sxs-lookup"><span data-stu-id="ea080-128">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="using-idesigntimedbcontextfactorytcontext"></a><span data-ttu-id="ea080-129">Mithilfe von`IDesignTimeDbContextFactory<TContext>`</span><span class="sxs-lookup"><span data-stu-id="ea080-129">Using `IDesignTimeDbContextFactory<TContext>`</span></span>

<span data-ttu-id="ea080-130">Als Alternative zu den oben genannten Optionen können Sie auch eine Implementierung bereitstellen `IDesignTimeDbContextFactory<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="ea080-130">As an alternative to the options above, you may also provide an implementation of `IDesignTimeDbContextFactory<TContext>`.</span></span> <span data-ttu-id="ea080-131">EF-Tools können diese Factory zum Erstellen einer Instanz von Ihrem DbContext.</span><span class="sxs-lookup"><span data-stu-id="ea080-131">EF tools can use this factory to create an instance of your DbContext.</span></span> <span data-ttu-id="ea080-132">Dies möglicherweise erforderlich, um bestimmte zur Entwurfszeit Erfahrungen z. B. Migrationen zu erzielen.</span><span class="sxs-lookup"><span data-stu-id="ea080-132">This may be required in order to enable specific design-time experiences such as migrations.</span></span>

<span data-ttu-id="ea080-133">Implementieren Sie diese Schnittstelle, um Entwurfszeitdienste für Kontexttypen zu aktivieren, die nicht über einen öffentlichen Standardkonstruktor verfügen.</span><span class="sxs-lookup"><span data-stu-id="ea080-133">Implement this interface to enable design-time services for context types that do not have a public default constructor.</span></span> <span data-ttu-id="ea080-134">Entwurfszeitdienste erkennt automatisch Implementierungen dieser Schnittstelle, die sich in derselben Assembly wie den abgeleiteten Kontext befinden.</span><span class="sxs-lookup"><span data-stu-id="ea080-134">Design-time services will automatically discover implementations of this interface that are in the same assembly as the derived context.</span></span>

<span data-ttu-id="ea080-135">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ea080-135">Example:</span></span>

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

## <a name="more-reading"></a><span data-ttu-id="ea080-136">Weitere lesen</span><span class="sxs-lookup"><span data-stu-id="ea080-136">More reading</span></span>

* <span data-ttu-id="ea080-137">Lesen [Einstieg auf ASP.NET Core](../get-started/aspnetcore/index.md) für Weitere Informationen zur Verwendung von EF mit ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ea080-137">Read [Getting Started on ASP.NET Core](../get-started/aspnetcore/index.md) for more information on using EF with ASP.NET Core.</span></span>
* <span data-ttu-id="ea080-138">Lesen [Abhängigkeitsinjektion](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) Weitere Informationen zum mithilfe der Abhängigkeitsinjektion.</span><span class="sxs-lookup"><span data-stu-id="ea080-138">Read [Dependency Injection](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) to learn more about using DI.</span></span>
* <span data-ttu-id="ea080-139">Lesen [Test](testing/index.md) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="ea080-139">Read [Testing](testing/index.md) for more information.</span></span>
