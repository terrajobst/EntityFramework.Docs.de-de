---
title: Verbindungs Zeichenfolgen-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: ed89d6d09b15b0dea7fd8bc3ff3e3f631495ecb7
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149106"
---
# <a name="connection-strings"></a><span data-ttu-id="29875-102">Verbindungszeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="29875-102">Connection Strings</span></span>

<span data-ttu-id="29875-103">Die meisten Datenbankanbieter erfordern eine Form der Verbindungszeichenfolge zum Herstellen einer Datenbankverbindung.</span><span class="sxs-lookup"><span data-stu-id="29875-103">Most database providers require some form of connection string to connect to the database.</span></span> <span data-ttu-id="29875-104">Diese Verbindungszeichenfolge enthält vertrauliche Informationen, die geschützt werden müssen.</span><span class="sxs-lookup"><span data-stu-id="29875-104">Sometimes this connection string contains sensitive information that needs to be protected.</span></span> <span data-ttu-id="29875-105">Außerdem muss sie geändert werden, wenn Sie die Umgebung, wie z.B. Entwicklung, Test und Produktion, wechseln.</span><span class="sxs-lookup"><span data-stu-id="29875-105">You may also need to change the connection string as you move your application between environments, such as development, testing, and production.</span></span>

## <a name="winforms--wpf-applications"></a><span data-ttu-id="29875-106">WinForms-& WPF-Anwendungen</span><span class="sxs-lookup"><span data-stu-id="29875-106">WinForms & WPF Applications</span></span>

<span data-ttu-id="29875-107">WinForms-, WPF-und ASP.NET 4-Anwendungen haben ein bewährtes und getestetes Verbindungs Zeichen folgen Muster.</span><span class="sxs-lookup"><span data-stu-id="29875-107">WinForms, WPF, and ASP.NET 4 applications have a tried and tested connection string pattern.</span></span> <span data-ttu-id="29875-108">Die Verbindungs Zeichenfolge sollte der app. config-Datei Ihrer Anwendung hinzugefügt werden (Web. config, wenn Sie ASP.NET verwenden).</span><span class="sxs-lookup"><span data-stu-id="29875-108">The connection string should be added to your application's App.config file (Web.config if you are using ASP.NET).</span></span> <span data-ttu-id="29875-109">Wenn Ihre Verbindungs Zeichenfolge vertrauliche Informationen (z. b. Benutzername und Kennwort) enthält, können Sie den Inhalt der Konfigurationsdatei mit dem [Geheimnis-Manager-Tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)schützen.</span><span class="sxs-lookup"><span data-stu-id="29875-109">If your connection string contains sensitive information, such as username and password, you can protect the contents of the configuration file using the [Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager).</span></span>

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
> <span data-ttu-id="29875-110">Die `providerName`-Einstellung in EF Core-Verbindungszeichenfolgen muss nicht in der Datei "App.config" gespeichert werden, weil der Datenbankanbieter über den Code konfiguriert wird.</span><span class="sxs-lookup"><span data-stu-id="29875-110">The `providerName` setting is not required on EF Core connection strings stored in App.config because the database provider is configured via code.</span></span>

<span data-ttu-id="29875-111">Anschließend können Sie die Verbindungszeichenfolge mithilfe der `ConfigurationManager`-API im Kontext Ihrer `OnConfiguring`-Methode auslesen.</span><span class="sxs-lookup"><span data-stu-id="29875-111">You can then read the connection string using the `ConfigurationManager` API in your context's `OnConfiguring` method.</span></span> <span data-ttu-id="29875-112">Sie müssen möglicherweise auf die Framework-Assembly `System.Configuration` verweisen, um diese API verwenden zu können.</span><span class="sxs-lookup"><span data-stu-id="29875-112">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="universal-windows-platform-uwp"></a><span data-ttu-id="29875-113">Universelle Windows-Plattform (UWP)</span><span class="sxs-lookup"><span data-stu-id="29875-113">Universal Windows Platform (UWP)</span></span>

<span data-ttu-id="29875-114">Verbindungszeichenfolgen in einer UWP-Anwendung sind in der Regel SQLite-Verbindungen, die nur einen lokalen Dateinamen angeben.</span><span class="sxs-lookup"><span data-stu-id="29875-114">Connection strings in a UWP application are typically a SQLite connection that just specifies a local filename.</span></span> <span data-ttu-id="29875-115">Sie enthalten typischerweise keine vertraulichen Informationen und müssen nicht geändert werden, wenn eine Anwendung bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="29875-115">They typically do not contain sensitive information, and do not need to be changed as an application is deployed.</span></span> <span data-ttu-id="29875-116">Daher ist es in Ordnung, wenn die Verbindungszeichenfolge im Code verbleibt, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="29875-116">As such, these connection strings are usually fine to be left in code, as shown below.</span></span> <span data-ttu-id="29875-117">Wenn Sie die Verbindungszeichenfolge nicht im Code beibehalten möchten, unterstützt UWP das Konzept der Einstellungen. Weitere Informationen hierzu finden Sie im [Abschnitt zu App-Einstellungen in der UWP-Dokumentation](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data).</span><span class="sxs-lookup"><span data-stu-id="29875-117">If you wish to move them out of code then UWP supports the concept of settings, see the [App Settings section of the UWP documentation](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) for details.</span></span>

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

## <a name="aspnet-core"></a><span data-ttu-id="29875-118">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="29875-118">ASP.NET Core</span></span>

<span data-ttu-id="29875-119">Das Konfigurationssystem in ASP.NET Core ist sehr flexibel, und die Verbindungszeichenfolge kann in `appsettings.json`, einer Umgebungsvariable, dem Speicher für Benutzergeheimnisse oder einer anderen Konfigurationsquelle gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="29875-119">In ASP.NET Core the configuration system is very flexible, and the connection string could be stored in `appsettings.json`, an environment variable, the user secret store, or another configuration source.</span></span> <span data-ttu-id="29875-120">Weitere Informationen finden Sie im [Abschnitt zur Konfiguration in der ASP.NET Core-Dokumentation](https://docs.asp.net/en/latest/fundamentals/configuration.html).</span><span class="sxs-lookup"><span data-stu-id="29875-120">See the [Configuration section of the ASP.NET Core documentation](https://docs.asp.net/en/latest/fundamentals/configuration.html) for more details.</span></span> <span data-ttu-id="29875-121">Das folgende Beispiel zeigt die Verbindungszeichenfolge in der Datei `appsettings.json`.</span><span class="sxs-lookup"><span data-stu-id="29875-121">The following example shows the connection string stored in `appsettings.json`.</span></span>

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

<span data-ttu-id="29875-122">Der Kontext wird in der Regel in der Datei `Startup.cs` konfiguriert. Die Verbindungszeichenfolge wird dabei aus der Konfiguration gelesen.</span><span class="sxs-lookup"><span data-stu-id="29875-122">The context is typically configured in `Startup.cs` with the connection string being read from configuration.</span></span> <span data-ttu-id="29875-123">Im folgenden Beispiel sucht die `GetConnectionString()`-Methode nach einem Konfigurationswert, dessen Schlüssel `ConnectionStrings:<connection string name>` lautet.</span><span class="sxs-lookup"><span data-stu-id="29875-123">Note the `GetConnectionString()` method looks for a configuration value whose key is `ConnectionStrings:<connection string name>`.</span></span> <span data-ttu-id="29875-124">Sie müssen den [Microsoft. Extensions. Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) -Namespace importieren, um diese Erweiterungsmethode zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="29875-124">You need to import the [Microsoft.Extensions.Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) namespace to use this extension method.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
