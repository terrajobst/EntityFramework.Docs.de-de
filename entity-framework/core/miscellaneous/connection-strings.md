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
# <a name="connection-strings"></a>Verbindungszeichenfolgen

Die meisten Datenbankanbieter erfordern eine Form der Verbindungszeichenfolge zum Herstellen einer Datenbankverbindung. Diese Verbindungszeichenfolge enthält vertrauliche Informationen, die geschützt werden müssen. Außerdem muss sie geändert werden, wenn Sie die Umgebung, wie z.B. Entwicklung, Test und Produktion, wechseln.

## <a name="net-framework-applications"></a>.NET Framework-Anwendungen

.NET Framework-Anwendungen, z. B. Windows Forms, WPF, Konsole und ASP.NET 4 haben ein bewährtes Muster für die Verbindungszeichenfolge. Die Verbindungszeichenfolge sollte in Ihrer Anwendungs-Datei "App.config" ("Web.config" bei Verwendung von ASP.NET) hinzugefügt werden. Wenn Ihre Verbindungszeichenfolge vertrauliche Informationen wie Benutzername und Kennwort enthält, können Sie den Inhalt der Konfigurationsdatei mit Hilfe [der geschützten Konfiguration](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration) schützen.

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
> Die `providerName` Einstellung in EF Core-Verbindungszeichenfolgen muss nicht in der "App.config"-Datei gespeichert werden, da der Datenbankanbieter über Code konfiguriert wird.

Anschließend können Sie die Verbindungszeichenfolge mithilfe der `ConfigurationManager` API im Kontexts Ihrer `OnConfiguring` Methode auslesen. Sie müssen möglicherweise auf die `System.Configuration` Framework-Assembly verweisen, um diese API verwenden zu können.

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

Verbindungszeichenfolgen in einer UWP-Anwendung sind in der Regel eine SQLite-Verbindung, die nur einen lokalen Dateinamen angibt. Sie enthalten in der Regel keine vertraulichen Informationen und müssen nicht geändert werden, wenn eine Anwendung bereitgestellt wird. Daher ist es in Ordnung, wenn die Verbindungszeichenfolge im Code verbleibt, wie unten dargestellt. Wenn Sie die Verbindungszeichenfolge nicht im Code halten möchten, unterstützt UWP das Konzept der Einstellungen. Mehr Informationen darüber finden Sie im [Abschnitt App-Einstellungen der UWP-Dokumentation](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data).

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

Das ASP.NET Core-Konfigurationssystem ist sehr flexibel, und die Verbindungszeichenfolge könnte in der `appsettings.json`, einer Umgebungsvariable, dem Geheimnis-Speicher oder einer anderen Konfigurationsquelle gespeichert werden. Im [Konfigurationsabschnitt der Dokumentation zu ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) finden Sie weitere Details. Das folgende Beispiel zeigt die Verbindungszeichenfolge in der `appsettings.json`-Datei.

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

Der Kontext wird in der Regel in der `Startup.cs`-Datei konfiguriert. Die Verbindungszeichenfolge wird dabei aus der Konfiguration gelesen. Im folgenden Beispiel sucht die `GetConnectionString()` Methode einen Konfigurationswert, dessen Schlüssel `ConnectionStrings:<connection string name>` ist.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
