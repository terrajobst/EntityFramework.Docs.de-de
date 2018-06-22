---
title: Aktualisieren von EF Core 1.0 RC1 auf RC2 – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
ms.technology: entity-framework-core
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: e76886729248101ccac024568cf9abcd945fca33
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678626"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a>Aktualisieren von EF Core 1.0 RC1 auf RC2 1.0

Dieser Artikel enthält die Anleitung zum Verschieben einer Anwendung, die mit den Paketen RC1 auf RC2 erstellt.

## <a name="package-names-and-versions"></a>Paketnamen und Versionen

Zwischen RC1 und RC2 geändert von "Entity Framework 7", "Entity Framework Core". Erfahren Sie mehr über die Gründe für die Änderung der [diesen Beitrag von Scott Hanselman](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx). Aufgrund dieser Änderung unserer Paketnamen geändert `EntityFramework.*` auf `Microsoft.EntityFrameworkCore.*` und unsere-Versionen `7.0.0-rc1-final` auf `1.0.0-rc2-final` (oder `1.0.0-preview1-final` für Tools).

**Sie müssen die Pakete RC1 vollständig zu entfernen und installieren Sie die RC2 verfügen.** Hier ist die Zuordnung für einige allgemeine Pakete aus.

| RC1-Paket                                               | RC2-Entsprechung                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final      |
| EntityFramework.SQLite                    7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final      |
| EntityFramework7.Npgsql                   3.1.0-rc1-3     | NpgSql.EntityFrameworkCore.Postgres             <to be advised>      |
| EntityFramework.SqlServerCompact35        7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final      |
| EntityFramework.SqlServerCompact40        7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final      |
| EntityFramework.InMemory                  7.0.0-rc1-final | Microsoft.EntityFrameworkCore.InMemory 1.0.0-rc2-final      |
| EntityFramework.IBMDataServer             7.0.0-beta1     | Für RC2 noch nicht verfügbar                                            |
| EntityFramework.Commands                  7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Tools 1.0.0-preview1-final |
| EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final      |

## <a name="namespaces"></a>Namespaces

Zusammen mit den Paketnamen, Namespaces, die von geändert `Microsoft.Data.Entity.*` auf `Microsoft.EntityFrameworkCore.*`. Sie können diese Änderung mit einem Suchen/Ersetzen von behandeln `using Microsoft.Data.Entity` mit `using Microsoft.EntityFrameworkCore`.

## <a name="table-naming-convention-changes"></a>Tabelle Naming Convention-Änderungen

Eine wesentliche funktionale Änderung, die wir in RC2 dauerte wurde auf den Namen der `DbSet<TEntity>` -Eigenschaft für eine bestimmte Entität als Namen der Tabelle zugeordnet, anstatt nur der Klassenname. Erfahren Sie mehr über diese Änderung in [das verwandte Ankündigung Problem](https://github.com/aspnet/Announcements/issues/167).

Bei vorhandenen RC1-Anwendungen sollten den folgenden Code am Anfang des Hinzufügen Ihrer `OnModelCreating` Methode, die RC1 Benennungsstrategie beizubehalten:

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

Wenn Sie die neue Benennungsstrategie übernehmen möchten, würden erfolgreich empfohlen benennt Abschließen der Rest der Upgradeschritte und klicken Sie dann den Code entfernt und erstellen eine Migration aus, um die Tabelle anwenden.

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a>AddDbContext / Startup.cs ändert (gilt nur für ASP.NET Core-Projekte)

In RC1, mussten Sie Entity Framework-Dienste in der Anwendung-Dienstanbieter - hinzufügen `Startup.ConfigureServices(...)`:

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

In RC2, entfernen Sie die Aufrufe von `AddEntityFramework()`, `AddSqlServer()`usw..:

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

Sie müssen auch einen Konstruktor den abgeleiteten Kontext hinzufügen, nimmt die Kontextoptionen und übergibt sie an den Basiskonstruktor. Dies ist erforderlich, da wir einige furchterregend Magic entfernt, die sie in den Hintergrund eingeschmuggelt habe:

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a>Übergabe eines IServiceProvider

Wenn Sie RC1-Code verfügen, der übergibt ein `IServiceProvider` an den Kontext, dies jetzt verschoben wurde `DbContextOptions`, anstatt eine separate Konstruktorparameter. Verwendung `DbContextOptionsBuilder.UseInternalServiceProvider(...)` den Dienstanbieter festgelegt.

### <a name="testing"></a>Test

Das häufigste Szenario für diese Vorgehensweise wurde um den Bereich einer InMemory-Datenbank steuern, beim Testen. Finden Sie unter den aktualisierten [Test](testing/index.md) Artikel ein Beispiel hierfür RC2.

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a>Zum Auflösen von internen Diensten aus Application Service Provider (gilt nur für ASP.NET Core-Projekte)

Wenn Sie eine ASP.NET Core-Anwendung und EF zum Auflösen der internen Dienste vom Anwendungsdienstanbieter angezeigt werden sollen, ist eine Überladung der `AddDbContext` , mit der Sie zum Konfigurieren dieser:

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> Es wird empfohlen, EF zur Verwaltung von intern einen eigenen Diensten ermöglichen, es sei denn, Sie einen Grund haben für die interne EF-Dienste in Ihrer Anwendungsdienstanbieter kombinieren. Die Hauptgründe dafür, dass Sie dies tun möchten wird an Ihren Dienstanbieter, Anwendung verwenden, um Dienste zu ersetzen, die EF intern verwendet

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a>DNX-Befehle = > .NET CLI (gilt nur für ASP.NET Core-Projekte)

Wenn Sie zuvor verwendet die `dnx ef` -Befehle für ASP.NET 5-Projekte, diese haben jetzt verschoben `dotnet ef` Befehle. Die gleichen Befehlssyntax gilt weiterhin. Sie können `dotnet ef --help` Informationen zur Syntax.

Die Möglichkeit, die Befehle registriert sind, wurde in RC2 aufgrund von DNX mit .NET CLI ersetzt wird geändert. Befehle sind nun registriert, einem `tools` im Abschnitt `project.json`:

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
> Wenn Sie Visual Studio verwenden, können Sie jetzt Paket-Manager-Konsole verwenden, EF-Befehle für ASP.NET Core Projekte ausführen (Dies wurde nicht in RC1 unterstützt). Müssen Sie immer noch die Befehle in Registrieren der `tools` Abschnitt `project.json` dazu.

## <a name="package-manager-commands-require-powershell-5"></a>Paket-Manager-Befehle erfordern PowerShell 5

Wenn Sie die Entity Framework-Befehle in der Paket-Manager-Konsole in Visual Studio verwenden, müssen Sie sicherstellen, dass Sie PowerShell 5 installiert ist. Dies ist eine temporäre Anforderung, die in der nächsten Version entfernt wird (finden Sie unter [ausstellen #5327](https://github.com/aspnet/EntityFramework/issues/5327) Weitere Details).

## <a name="using-imports-in-projectjson"></a>Verwenden von "Imports" in "Project.JSON"

Einige der EF kernabhängigkeiten unterstützt .NET Standard noch nicht. Hinzufügen von "imports" auf "Project.JSON" als vorübergehende problemumgehung ist EF Core in standardmäßigen .NET und .NET Core-Projekte erfordern.

Beim Hinzufügen von EF wird NuGet Restore diese Fehlermeldung angezeigt:

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

Die problemumgehung besteht darin, die portable Profil "Portable net451 + win8" manuell importieren. Diese erzwingt NuGet, diese Binärdateien behandelt werden sollen, die dies entsprechen bieten als kompatibel mit dem standardmäßigen .NET Framework, obwohl sie nicht sind. Obwohl "Portable net451 + win8" nicht 100 % mit .NET Standard kompatibel ist, ist es kompatibel genug für den Übergang vom PCL .NET-Standard. Importe können entfernt werden, wenn EF Abhängigkeiten schließlich auf .NET Standard aktualisieren.

Mehrere Frameworks können "Imports" in der Arraysyntax hinzugefügt werden. Andere Importe ist möglicherweise erforderlich, wenn Sie zusätzliche Bibliotheken zu Ihrem Projekt hinzufügen.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

Finden Sie unter [ausstellen #5176](https://github.com/aspnet/EntityFramework/issues/5176).
