---
title: Verbindungszeichenfolgen - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: c306f9ca7a51fc9e3db18e883fd44f56dd1a3cb4
ms.sourcegitcommit: e90d6cfa3e96f10b8b5275430759a66a0c714ed1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2019
ms.locfileid: "68286446"
---
# <a name="connection-strings"></a>Verbindungszeichenfolgen

Die meisten Datenbankanbieter erfordern eine Form der Verbindungszeichenfolge zum Herstellen einer Datenbankverbindung. Diese Verbindungszeichenfolge enthält vertrauliche Informationen, die geschützt werden müssen. Außerdem muss sie geändert werden, wenn Sie die Umgebung, wie z.B. Entwicklung, Test und Produktion, wechseln.

## <a name="net-framework-applications"></a>.NET Framework-Anwendungen

.NET Framework-Anwendungen, z.B. Windows Forms, WPF, Konsole und ASP.NET 4, verwenden ein bewährtes Muster für Verbindungszeichenfolgen. App.config-Datei Ihrer Anwendung ("Web.config" bei Verwendung von ASP.NET), sollten die Verbindungszeichenfolge hinzugefügt werden. Wenn Ihre Verbindungszeichenfolge enthält vertrauliche Informationen wie Benutzername und Kennwort, enthält Sie können den Inhalt der Konfigurationsdatei mit schützen [der geschützten Konfiguration](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).

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
> Die `providerName`-Einstellung in EF Core-Verbindungszeichenfolgen muss nicht in der Datei "App.config" gespeichert werden, weil der Datenbankanbieter über den Code konfiguriert wird.

Anschließend können Sie die Verbindungszeichenfolge mithilfe der `ConfigurationManager`-API im Kontext Ihrer `OnConfiguring`-Methode auslesen. Sie müssen möglicherweise auf die Framework-Assembly `System.Configuration` verweisen, um diese API verwenden zu können.

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

Verbindungszeichenfolgen in einer UWP-Anwendung sind in der Regel SQLite-Verbindungen, die nur einen lokalen Dateinamen angeben. Sie enthalten typischerweise keine vertraulichen Informationen und müssen nicht geändert werden, wenn eine Anwendung bereitgestellt wird. Daher ist es in Ordnung, wenn die Verbindungszeichenfolge im Code verbleibt, wie unten dargestellt. Wenn Sie die Verbindungszeichenfolge nicht im Code beibehalten möchten, unterstützt UWP das Konzept der Einstellungen. Weitere Informationen hierzu finden Sie im [Abschnitt zu App-Einstellungen in der UWP-Dokumentation](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data).

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

Das Konfigurationssystem in ASP.NET Core ist sehr flexibel, und die Verbindungszeichenfolge kann in `appsettings.json`, einer Umgebungsvariable, dem Speicher für Benutzergeheimnisse oder einer anderen Konfigurationsquelle gespeichert werden. Weitere Informationen finden Sie im [Abschnitt zur Konfiguration in der ASP.NET Core-Dokumentation](https://docs.asp.net/en/latest/fundamentals/configuration.html). Das folgende Beispiel zeigt die Verbindungszeichenfolge in der Datei `appsettings.json`.

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

Der Kontext wird in der Regel in der Datei `Startup.cs` konfiguriert. Die Verbindungszeichenfolge wird dabei aus der Konfiguration gelesen. Im folgenden Beispiel sucht die `GetConnectionString()`-Methode nach einem Konfigurationswert, dessen Schlüssel `ConnectionStrings:<connection string name>` lautet. Sie müssen zum Importieren der ["Microsoft.Extensions.Configuration"](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) Namespace, um diese Erweiterungsmethode verwenden.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
