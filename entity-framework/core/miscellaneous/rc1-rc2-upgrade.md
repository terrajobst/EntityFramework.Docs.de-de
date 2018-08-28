---
title: Aktualisieren von EF Core 1.0 RC1 auf RC2 – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: 83b98fda5ac9491994b5b3fb333c9951ec01188a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996896"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a>Aktualisieren von EF Core 1.0 RC1 auf 1.0 RC2

Dieser Artikel enthält Anleitungen zum Verschieben von einer Anwendung, die mit den Paketen RC1 auf RC2 erstellt.

## <a name="package-names-and-versions"></a>Die Paketnamen und Versionen

Zwischen RC1 und RC2 wurde von "Entitätsframework 7", "Entity Framework Core". Erfahren Sie mehr über die Gründe für die Änderung in [diesen Beitrag von Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx). Aufgrund dieser Änderung unsere Paketnamen geändert `EntityFramework.*` zu `Microsoft.EntityFrameworkCore.*` und unsere Versionen von `7.0.0-rc1-final` zu `1.0.0-rc2-final` (oder `1.0.0-preview1-final` Tools).

**Sie müssen die Pakete RC1 vollständig zu entfernen und installieren Sie dann den RC2 solche.** Hier ist die Zuordnung für einige gängige Pakete.

| RC1-Paket                                               | RC2-äquivalent                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| EntityFramework.MicrosoftSqlServer 7.0.0-rc1-final | "Microsoft.entityframeworkcore.SqlServer" 1.0.0-rc2-final      |
| EntityFramework.SQLite 7.0.0-rc1-final | "Microsoft.entityframeworkcore.SQLite" 1.0.0-rc2-final      |
| EntityFramework7.Npgsql 3.1.0-rc1-3     | NpgSql.EntityFrameworkCore.Postgres             <to be advised>      |
| EntityFramework.SqlServerCompact35 7.0.0-rc1-final | Sqlservercompact35 1.0.0-rc2-final      |
| EntityFramework.SqlServerCompact40 7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact40 1.0.0-rc2-final      |
| "EntityFramework.InMemory" 7.0.0-rc1-final | "Microsoft.entityframeworkcore.InMemory" 1.0.0-rc2-final      |
| EntityFramework.IBMDataServer 7.0.0-beta1     | Noch nicht für RC2 verfügbar                                            |
| "EntityFramework.Commands" 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Tools 1.0.0-preview1-final |
| EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer.Design 1.0.0-rc2-final      |

## <a name="namespaces"></a>Namespaces

Zusammen mit den Paketnamen, Namespaces, die von geändert `Microsoft.Data.Entity.*` zu `Microsoft.EntityFrameworkCore.*`. Sie können diese Änderung mit einer Suchen/Ersetzen von behandeln `using Microsoft.Data.Entity` mit `using Microsoft.EntityFrameworkCore`.

## <a name="table-naming-convention-changes"></a>Tabelle, die Benennung von Änderungen

Eine wesentliche funktionale Änderung wir in RC2 haben wurde, verwenden Sie den Namen der `DbSet<TEntity>` -Eigenschaft für eine bestimmte Entität als den Namen der Tabelle zugeordnet, anstatt nur den Klassennamen. Erfahren Sie mehr über diese Änderung [das Problem der entsprechenden Ankündigung](https://github.com/aspnet/Announcements/issues/167).

Für vorhandene RC1-Anwendungen, empfehlen wir den folgenden Code hinzufügen, am Anfang Ihrer `OnModelCreating` Methode, die Strategie zum Benennen von RC1 beizubehalten:

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

Wenn Sie die neue Benennungskonvention Strategie einführen möchten, würden erfolgreich empfohlen Abschluss den Rest der Upgradeschritte und klicken Sie dann den Code zu entfernen und erstellen eine Migration zum in der Tabelle angewendet wird.

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a>AddDbContext / "Startup.cs" geändert wird (gilt nur für ASP.NET Core-Projekte)

In RC1, mussten Sie Entity Framework-Dienste an den Application Service Provider - in hinzufügen `Startup.ConfigureServices(...)`:

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

In RC2, können Sie die Aufrufe an entfernen `AddEntityFramework()`, `AddSqlServer()`usw..:

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

Sie müssen auch einen Konstruktor, Ihrem abgeleiteten Kontext hinzufügen, die Optionen aus, und übergibt sie an den Basiskonstruktor. Dies ist erforderlich, da wir einige problematische eindrucksvollen Prozessen von entfernt haben, die sie in hinter den Kulissen eingeschmuggelt habe:

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a>Übergabe eines IServiceProvider

Wenn Sie RC1-Code verfügen, die übergibt eine `IServiceProvider` an den Kontext und dies jetzt wurde zur `DbContextOptions`, anstatt einen separaten Konstruktorparameter. Verwendung `DbContextOptionsBuilder.UseInternalServiceProvider(...)` den Dienstanbieter festgelegt.

### <a name="testing"></a>Test

Das häufigste Szenario hierfür bestand darin, den Umfang einer InMemory-Datenbank zu steuern, beim Testen. Finden Sie in der aktualisierten [Tests](testing/index.md) Artikel ein Beispiel hierfür mit RC2.

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a>Zum Auflösen von internen Diensten von Service Provider für die Anwendung (gilt nur für ASP.NET Core-Projekte)

Wenn Sie eine ASP.NET Core-Anwendung und EF interne Dienste vom Anwendungsdienstanbieter aufgelöst werden sollen, ist es eine Überladung der `AddDbContext` , mit der Sie zum Konfigurieren dieser:

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> Es wird empfohlen, EF die eigenen Diensten, intern verwalten können, es sei denn, Sie einen Grund haben für die internen EF-Dienste in Ihrem Dienstanbieter für die Anwendung zu kombinieren. Der Hauptgrund, möchten Sie möglicherweise dazu wird auf Ihrem Dienstanbieter für die Anwendung zu verwenden, um Dienste zu ersetzen, die EF intern verwendet

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a>DNX-Befehlen = > .NET CLI (gilt nur für ASP.NET Core-Projekte)

Wenn Sie zuvor die `dnx ef` -Befehle für ASP.NET 5-Projekten, diese wurden jetzt verschoben `dotnet ef` Befehle. Die gleichen Befehlssyntax gilt immer noch aus. Sie können `dotnet ef --help` Informationen zur Syntax.

Die Möglichkeit, die Befehle registriert sind, wurde in RC2, aufgrund von DNX, die von .NET CLI ersetzt wird geändert. Befehle sind jetzt registriert, einem `tools` im Abschnitt `project.json`:

``` json
"tools": {
  "Microsoft.EntityFrameworkCore.Tools": {
    "version": "1.0.0-preview1-final",
    "imports": [
      "portable-net45+win8+dnxcore50",
      "portable-net45+win8"
    ]
  }
}
```

> [!TIP]  
> Wenn Sie Visual Studio verwenden, können Sie jetzt-Paket-Manager-Konsole verwenden, zum Ausführen von EF-Befehlen für ASP.NET Core-Projekte (Dies war nicht in RC1 unterstützt). Müssen Sie immer noch die Befehle im Registrieren der `tools` Abschnitt `project.json` dazu.

## <a name="package-manager-commands-require-powershell-5"></a>Paket-Manager-Befehle erfordern PowerShell 5

Wenn Sie die Entity Framework-Befehle in der Paket-Manager-Konsole in Visual Studio verwenden, müssen Sie sicherstellen, dass Sie PowerShell 5 installiert haben. Dies ist eine temporäre Anforderung, die in der nächsten Version entfernt werden (finden Sie unter [Problem #5327](https://github.com/aspnet/EntityFramework/issues/5327) Weitere Details).

## <a name="using-imports-in-projectjson"></a>Verwenden von "Imports" in "Project.JSON"

Einige der EF Core Abhängigkeiten unterstützen .NET Standard noch keine. Hinzufügen von "imports" Datei "Project.JSON" als vorübergehende problemumgehung ist EF Core in .NET Standard und .NET Core-Projekten erfordern.

Beim Hinzufügen von EF wird NuGet-Wiederherstellung die folgende Fehlermeldung angezeigt:

``` Console
Package Ix-Async 1.2.5 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Ix-Async 1.2.5 supports:
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8 (.NETPortable,Version=v0.0,Profile=Profile78)
Package Remotion.Linq 2.0.2 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Remotion.Linq 2.0.2 supports:
  - net35 (.NETFramework,Version=v3.5)
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8+wpa81 (.NETPortable,Version=v0.0,Profile=Profile259)
```

Die problemumgehung besteht darin, das portable Profil "Portable-net451 + win8" manuell importieren. Diese erzwingt, dass NuGet, um diese Binärdateien zu behandeln, die dieser entsprechen bieten als kompatibel mit .NET Standard-Framework, obwohl dies nicht der Fall. Obwohl "Portable-net451 + win8" nicht 100 % kompatibel mit .NET Standard ist, ist es kompatibel genug ist, für den Übergang von PCL auf .NET Standard. Importe können entfernt werden, wenn Abhängigkeiten von EF schließlich ein Upgrade auf .NET Standard durchführen.

Mehrere Frameworks können enthaltene "Imports" in der Arraysyntax hinzugefügt werden. Andere Importvorgänge ist möglicherweise erforderlich, wenn Sie zusätzliche Bibliotheken zu Ihrem Projekt hinzufügen.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

Finden Sie unter [Problem #5176](https://github.com/aspnet/EntityFramework/issues/5176).
