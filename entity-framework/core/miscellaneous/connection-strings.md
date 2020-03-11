---
title: Verbindungs Zeichenfolgen-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: ed89d6d09b15b0dea7fd8bc3ff3e3f631495ecb7
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414092"
---
# <a name="connection-strings"></a>Verbindungszeichenfolgen

Die meisten Datenbankanbieter erfordern eine Form der Verbindungszeichenfolge zum Herstellen einer Datenbankverbindung. Diese Verbindungszeichenfolge enthält vertrauliche Informationen, die geschützt werden müssen. Außerdem muss sie geändert werden, wenn Sie die Umgebung, wie z.B. Entwicklung, Test und Produktion, wechseln.

## <a name="winforms--wpf-applications"></a>WinForms-& WPF-Anwendungen

WinForms-, WPF-und ASP.NET 4-Anwendungen haben ein bewährtes und getestetes Verbindungs Zeichen folgen Muster. Die Verbindungs Zeichenfolge sollte der app. config-Datei Ihrer Anwendung hinzugefügt werden (Web. config, wenn Sie ASP.NET verwenden). Wenn Ihre Verbindungs Zeichenfolge vertrauliche Informationen (z. b. Benutzername und Kennwort) enthält, können Sie den Inhalt der Konfigurationsdatei mit dem [Geheimnis-Manager-Tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)schützen.

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
> Die `providerName` Einstellung ist für EF Core Verbindungs Zeichenfolgen, die in app. config gespeichert sind, nicht erforderlich, da der Datenbankanbieter über Code konfiguriert ist.

Anschließend können Sie die Verbindungs Zeichenfolge mit der `ConfigurationManager`-API in der `OnConfiguring`-Methode Ihres Kontexts lesen. Möglicherweise müssen Sie einen Verweis auf die `System.Configuration` Framework-Assembly hinzufügen, damit diese API verwendet werden kann.

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

Verbindungszeichenfolgen in einer UWP-Anwendung sind in der Regel SQLite-Verbindungen, die nur einen lokalen Dateinamen angeben. Sie enthalten typischerweise keine vertraulichen Informationen und müssen nicht geändert werden, wenn eine Anwendung bereitgestellt wird. Daher ist es in Ordnung, wenn die Verbindungszeichenfolge im Code verbleibt, wie unten dargestellt. Wenn Sie Sie aus dem Code heraus verschieben möchten, unterstützt UWP das Konzept der Einstellungen. Weitere Informationen finden Sie im [Abschnitt App-Einstellungen der UWP-Dokumentation](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) .

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

In ASP.net Core das Konfigurationssystem sehr flexibel, und die Verbindungs Zeichenfolge kann in `appsettings.json`, einer Umgebungsvariablen, dem geheimen Benutzerspeicher oder einer anderen Konfigurations Quelle gespeichert werden. Weitere Informationen finden Sie im [Abschnitt zur Konfiguration der ASP.net Core-Dokumentation](https://docs.asp.net/en/latest/fundamentals/configuration.html) . Das folgende Beispiel zeigt die in `appsettings.json`gespeicherte Verbindungs Zeichenfolge.

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

Der Kontext wird in der Regel in `Startup.cs` konfiguriert, wobei die Verbindungs Zeichenfolge aus der Konfiguration gelesen wird. Beachten Sie, dass die `GetConnectionString()`-Methode nach einem Konfigurations Wert sucht, dessen Schlüssel `ConnectionStrings:<connection string name>`ist. Sie müssen den [Microsoft. Extensions. Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) -Namespace importieren, um diese Erweiterungsmethode zu verwenden.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
