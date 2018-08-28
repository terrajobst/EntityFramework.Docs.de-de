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

Die meisten Datenbankanbieter erfordern eine Form der Verbindungszeichenfolge zum Herstellen einer datenbankverbindung. Manchmal: Diese Verbindungszeichenfolge enthält vertrauliche Informationen, die geschützt werden muss. Sie müssen auch die Verbindungszeichenfolge zu ändern, wie Sie Ihre Anwendung zwischen Umgebungen, wie z. B. Entwicklung, Tests und Produktion wechseln.

## <a name="net-framework-applications"></a>.NET Framework-Anwendungen

.NET Framework-Anwendungen, z. B. Windows Forms, WPF, Konsole und ASP.NET 4 haben ein Zeichenfolgenmuster bewährte Verbindung. Die Verbindungszeichenfolge sollten Ihre Anwendungen-Datei "App.config" ("Web.config" bei Verwendung von ASP.NET) hinzugefügt werden. Wenn Ihre Verbindungszeichenfolge enthält vertrauliche Informationen wie Benutzername und Kennwort, enthält Sie können den Inhalt der Konfigurationsdatei mit schützen [der geschützten Konfiguration](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).

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
> Die `providerName` Einstellung muss nicht auf EF Core-Verbindungszeichenfolgen, die in "App.config" gespeichert werden, da der Datenbankanbieter über Code konfiguriert wird.

Anschließend können Sie lesen, die Verbindungszeichenfolge mithilfe der `ConfigurationManager` API im Kontexts Ihrer `OnConfiguring` Methode. Sie müssen möglicherweise einen Verweis auf Hinzufügen der `System.Configuration` Framework-Assembly, um diese API verwenden zu können.

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

Verbindungszeichenfolgen in einer UWP-Anwendung sind in der Regel eine SQLite-Verbindung, die nur einen lokalen Dateinamen angibt. Sie in der Regel enthalten keine vertraulichen Informationen und müssen nicht geändert werden, da eine Anwendung bereitgestellt wird. Daher werden diese Verbindungszeichenfolgen in der Regel in Ordnung, die im Code verbleiben, wie unten dargestellt. Wenn Sie sie aus Code heraus zu verschieben möchten. Klicken Sie dann UWP unterstützt das Konzept der Einstellungen, finden Sie unter den [App-Einstellungen im Abschnitt der Dokumentation zur UWP](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) Details.

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

In ASP.NET Core-Konfigurationssystem ist sehr flexibel, und die Verbindungszeichenfolge konnte gespeichert werden, `appsettings.json`, eine Umgebungsvariable, die User-secret-Speicher oder einer anderen Konfigurationsquelle. Finden Sie unter den [Konfigurationsabschnitt der Dokumentation zu ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) Weitere Details. Das folgende Beispiel zeigt die Verbindungszeichenfolge, die in gespeicherten `appsettings.json`.

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

Der Kontext wird in der Regel im konfiguriert `Startup.cs` durch die Verbindungszeichenfolge, die aus der Konfiguration gelesen. Beachten Sie die `GetConnectionString()` Methode sucht einen Konfigurationswert, dessen Schlüssel `ConnectionStrings:<connection string name>`.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
