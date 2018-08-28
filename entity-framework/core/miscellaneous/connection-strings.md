---
title: Verbindungszeichenfolgen - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: 942865effba7b491dd950886ea30b69a86f1186c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997670"
---
# <a name="connection-strings"></a><span data-ttu-id="b344b-102">Verbindungszeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="b344b-102">Connection Strings</span></span>

<span data-ttu-id="b344b-103">Die meisten Datenbankanbieter erfordern eine Form der Verbindungszeichenfolge zum Herstellen einer datenbankverbindung.</span><span class="sxs-lookup"><span data-stu-id="b344b-103">Most database providers require some form of connection string to connect to the database.</span></span> <span data-ttu-id="b344b-104">Manchmal: Diese Verbindungszeichenfolge enthält vertrauliche Informationen, die geschützt werden muss.</span><span class="sxs-lookup"><span data-stu-id="b344b-104">Sometimes this connection string contains sensitive information that needs to be protected.</span></span> <span data-ttu-id="b344b-105">Sie müssen auch die Verbindungszeichenfolge zu ändern, wie Sie Ihre Anwendung zwischen Umgebungen, wie z. B. Entwicklung, Tests und Produktion wechseln.</span><span class="sxs-lookup"><span data-stu-id="b344b-105">You may also need to change the connection string as you move your application between environments, such as development, testing, and production.</span></span>

## <a name="net-framework-applications"></a><span data-ttu-id="b344b-106">.NET Framework-Anwendungen</span><span class="sxs-lookup"><span data-stu-id="b344b-106">.NET Framework Applications</span></span>

<span data-ttu-id="b344b-107">.NET Framework-Anwendungen, z. B. Windows Forms, WPF, Konsole und ASP.NET 4 haben ein Zeichenfolgenmuster bewährte Verbindung.</span><span class="sxs-lookup"><span data-stu-id="b344b-107">.NET Framework applications, such as WinForms, WPF, Console, and ASP.NET 4, have a tried and tested connection string pattern.</span></span> <span data-ttu-id="b344b-108">Die Verbindungszeichenfolge sollten Ihre Anwendungen-Datei "App.config" ("Web.config" bei Verwendung von ASP.NET) hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="b344b-108">The connection string should be added to your applications App.config file (Web.config if you are using ASP.NET).</span></span> <span data-ttu-id="b344b-109">Wenn Ihre Verbindungszeichenfolge enthält vertrauliche Informationen wie Benutzername und Kennwort, enthält Sie können den Inhalt der Konfigurationsdatei mit schützen [der geschützten Konfiguration](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).</span><span class="sxs-lookup"><span data-stu-id="b344b-109">If your connection string contains sensitive information, such as username and password, you can protect the contents of the configuration file using [Protected Configuration](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).</span></span>

``` xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>

  <connectionStrings>
    <add name="BloggingDatabase"
         connectionString="Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" />
  </connectionStrings>
</configuration>
```

> [!TIP]  
> <span data-ttu-id="b344b-110">Die `providerName` Einstellung muss nicht auf EF Core-Verbindungszeichenfolgen, die in "App.config" gespeichert werden, da der Datenbankanbieter über Code konfiguriert wird.</span><span class="sxs-lookup"><span data-stu-id="b344b-110">The `providerName` setting is not required on EF Core connection strings stored in App.config because the database provider is configured via code.</span></span>

<span data-ttu-id="b344b-111">Anschließend können Sie lesen, die Verbindungszeichenfolge mithilfe der `ConfigurationManager` API im Kontexts Ihrer `OnConfiguring` Methode.</span><span class="sxs-lookup"><span data-stu-id="b344b-111">You can then read the connection string using the `ConfigurationManager` API in your context's `OnConfiguring` method.</span></span> <span data-ttu-id="b344b-112">Sie müssen möglicherweise einen Verweis auf Hinzufügen der `System.Configuration` Framework-Assembly, um diese API verwenden zu können.</span><span class="sxs-lookup"><span data-stu-id="b344b-112">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="universal-windows-platform-uwp"></a><span data-ttu-id="b344b-113">Universelle Windows-Plattform (UWP)</span><span class="sxs-lookup"><span data-stu-id="b344b-113">Universal Windows Platform (UWP)</span></span>

<span data-ttu-id="b344b-114">Verbindungszeichenfolgen in einer UWP-Anwendung sind in der Regel eine SQLite-Verbindung, die nur einen lokalen Dateinamen angibt.</span><span class="sxs-lookup"><span data-stu-id="b344b-114">Connection strings in a UWP application are typically a SQLite connection that just specifies a local filename.</span></span> <span data-ttu-id="b344b-115">Sie in der Regel enthalten keine vertraulichen Informationen und müssen nicht geändert werden, da eine Anwendung bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="b344b-115">They typically do not contain sensitive information, and do not need to be changed as an application is deployed.</span></span> <span data-ttu-id="b344b-116">Daher werden diese Verbindungszeichenfolgen in der Regel in Ordnung, die im Code verbleiben, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="b344b-116">As such, these connection strings are usually fine to be left in code, as shown below.</span></span> <span data-ttu-id="b344b-117">Wenn Sie sie aus Code heraus zu verschieben möchten. Klicken Sie dann UWP unterstützt das Konzept der Einstellungen, finden Sie unter den [App-Einstellungen im Abschnitt der Dokumentation zur UWP](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) Details.</span><span class="sxs-lookup"><span data-stu-id="b344b-117">If you wish to move them out of code then UWP supports the concept of settings, see the [App Settings section of the UWP documentation](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) for details.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
            optionsBuilder.UseSqlite("Data Source=blogging.db");
    }
}
```

## <a name="aspnet-core"></a><span data-ttu-id="b344b-118">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b344b-118">ASP.NET Core</span></span>

<span data-ttu-id="b344b-119">In ASP.NET Core-Konfigurationssystem ist sehr flexibel, und die Verbindungszeichenfolge konnte gespeichert werden, `appsettings.json`, eine Umgebungsvariable, die User-secret-Speicher oder einer anderen Konfigurationsquelle.</span><span class="sxs-lookup"><span data-stu-id="b344b-119">In ASP.NET Core the configuration system is very flexible, and the connection string could be stored in `appsettings.json`, an environment variable, the user secret store, or another configuration source.</span></span> <span data-ttu-id="b344b-120">Finden Sie unter den [Konfigurationsabschnitt der Dokumentation zu ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) Weitere Details.</span><span class="sxs-lookup"><span data-stu-id="b344b-120">See the [Configuration section of the ASP.NET Core documentation](https://docs.asp.net/en/latest/fundamentals/configuration.html) for more details.</span></span> <span data-ttu-id="b344b-121">Das folgende Beispiel zeigt die Verbindungszeichenfolge, die in gespeicherten `appsettings.json`.</span><span class="sxs-lookup"><span data-stu-id="b344b-121">The following example shows the connection string stored in `appsettings.json`.</span></span>

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

<span data-ttu-id="b344b-122">Der Kontext wird in der Regel im konfiguriert `Startup.cs` durch die Verbindungszeichenfolge, die aus der Konfiguration gelesen.</span><span class="sxs-lookup"><span data-stu-id="b344b-122">The context is typically configured in `Startup.cs` with the connection string being read from configuration.</span></span> <span data-ttu-id="b344b-123">Beachten Sie die `GetConnectionString()` Methode sucht einen Konfigurationswert, dessen Schlüssel `ConnectionStrings:<connection string name>`.</span><span class="sxs-lookup"><span data-stu-id="b344b-123">Note the `GetConnectionString()` method looks for a configuration value whose key is `ConnectionStrings:<connection string name>`.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
