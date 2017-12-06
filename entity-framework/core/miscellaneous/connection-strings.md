---
title: Verbindungszeichenfolgen - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: b4ed01f0452d74ac49d3fde780caa5f1b25a6e97
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="connection-strings"></a>Verbindungszeichenfolgen

Die meisten Datenbankanbieter erfordern eine Form der Verbindungszeichenfolge für die Verbindung mit der Datenbank. In einigen Fällen enthält diese Verbindungszeichenfolge vertraulichen Informationen, der geschützt werden muss. Sie müssen möglicherweise auch die Verbindungszeichenfolge ändern, wie Sie Ihre Anwendung zwischen Umgebungen, wie z. B. Entwicklungs-, Test- und Produktionszwecke verschieben.

## <a name="net-framework-applications"></a>.NET Framework-Anwendungen

.NET Framework-Anwendungen, z. B. WinForms, WPF-Konsole und ASP.NET 4 haben ein Zeichenfolgenmuster wiederholt und getestete Verbindung. Die Verbindungszeichenfolge sollte auf die Datei "App.config" der Anwendung ("Web.config" bei Verwendung von ASP.NET) hinzugefügt werden. Die Verbindungszeichenfolge enthält vertraulichen Informationen wie Benutzername und Kennwort können Sie den Inhalt der Konfigurationsdatei mit schützen [geschützte Konfiguration](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).

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
> Die `providerName` Einstellung ist nicht erforderlich, EF Core Verbindungszeichenfolgen in App.config gespeichert werden, da der Datenbankanbieter über Code konfiguriert ist.

Sie können dann gelesen, die Verbindungszeichenfolge mithilfe der `ConfigurationManager` -API in des Kontexts `OnConfiguring` Methode. Sie müssen möglicherweise einen Verweis auf Hinzufügen der `System.Configuration` Framework-Assembly, um diese API verwenden zu können.

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

## <a name="universal-windows-platform-uwp"></a>Universelle Windows-Plattform (UWP)

Verbindungszeichenfolgen in einer uwp-Anwendung sind in der Regel eine SQLite-Verbindung, die nur einen lokalen Dateinamen angibt. Sie in der Regel nicht vertraulichen Informationen enthalten und müssen nicht geändert werden, da eine Anwendung bereitgestellt wird. Daher sind diese Verbindungszeichenfolgen in der Regel gut, verbleiben soll, im Code, wie unten dargestellt. Wenn Sie sie aus Code heraus zu verschieben möchten. Klicken Sie dann universelle Windows-Plattform unterstützt das Konzept der Einstellungen, finden Sie unter der [App-Einstellungen-Abschnitt der Dokumentation UWP](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) Details.

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

## <a name="aspnet-core"></a>ASP.NET Core

Das Konfigurationssystem in ASP.NET Core ist sehr flexibel und die Verbindungszeichenfolge gespeichert werden `appsettings.json`, eine Umgebungsvariable, den geheimen Speicher des Benutzers oder einer anderen Konfigurationsquelle angibt. Finden Sie unter der [Konfigurationsabschnitt der Dokumentation zu ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) Weitere Details. Das folgende Beispiel zeigt die Verbindungszeichenfolge, die in gespeicherten `appsettings.json`.

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

Der Kontext wird in der Regel in konfiguriert `Startup.cs` mit der Verbindungszeichenfolge aus der Konfiguration gelesen wird. Beachten Sie die `GetConnectionString()` Methode sucht für einen Konfigurationswert fest, dessen Schlüssel `ConnectionStrings:<connection string name>`.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
