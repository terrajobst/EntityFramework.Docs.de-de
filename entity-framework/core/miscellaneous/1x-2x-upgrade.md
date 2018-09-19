---
title: Aktualisieren von früheren Versionen auf EF Core 2 – EF Core
author: divega
ms.date: 08/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: 5371c8f3b7c6102c621296bbae145d13779e0c6e
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283770"
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a>Aktualisieren von Anwendungen aus früheren Versionen auf EF Core 2.0

Habe ich die Möglichkeit, unsere vorhandenen APIs und Verhalten in 2.0 erheblich optimieren. Es gibt einige Verbesserungen, die erfordern, vorhandenen Code der Anwendung ändern können, obwohl wir, die für die meisten Anwendungen sind der Meinung die Auswirkungen niedrig ist, in den meisten Fällen erfordert nur eine Neukompilierung und nur minimale geführte Änderungen an die veralteten APIs zu ersetzen.

Aktualisieren einer vorhandenen Anwendung auf EF Core 2.0 erfordern:

1. Aktualisieren die Ziel .NET Implementierung der Anwendung auf einen ein, die .NET Standard 2.0 unterstützt. Finden Sie unter [unterstützt .NET Implementierungen](../platforms/index.md) Weitere Details.

2. Identifizieren Sie einen Anbieter für die Zieldatenbank, die mit EF Core 2.0 kompatibel ist. Finden Sie unter [EF Core 2.0 erfordert einen 2.0 Datenbankanbieter](#ef-core-20-requires-a-20-database-provider) unten.

3. Aktualisieren alle EF Core-Pakete ("Runtime" und "Tools") auf 2.0 an. Finden Sie unter [Installieren von EF Core](../get-started/install/index.md) Weitere Details.

4. Stellen Sie alle notwendige codeänderungen, um die Änderungen, die im weiteren Verlauf dieses Dokuments beschrieben zu kompensieren.

## <a name="aspnet-core-now-includes-ef-core"></a>ASP.NET Core enthält nun EF Core

Anwendungen, deren Zielversionen für ASP.NET Core 2.0 festgelegt sind, können neben Datenbankanbietern von Drittanbietern EF Core 2.0 ohne zusätzliche Abhängigkeiten nutzen. Allerdings müssen Anwendungen, die für frühere Versionen von ASP.NET Core auf ASP.NET Core 2.0 aktualisieren, um EF Core 2.0 verwenden. Weitere Informationen zum Aktualisieren von ASP.NET Core-Anwendungen auf 2.0 finden Sie [Dokumentation zu ASP.NET Core zu diesem Thema](https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/).

## <a name="new-way-of-getting-application-services-in-aspnet-core"></a>Neuen Weg, Dienste der Anwendung in ASP.NET Core

Das empfohlene Muster für ASP.NET Core-Webanwendungen wurde für 2.0 so aktualisiert, die die Logik zur Entwurfszeit unterbrochen wurde, die EF Core in 1.x verwendet. Zur Entwurfszeit verwendet wird, würde EF Core zuvor aufzurufende versuchen `Startup.ConfigureServices` direkt, um Zugriff auf den Dienstanbieter für der Anwendung. In ASP.NET Core 2.0-Konfiguration wird initialisiert, außerhalb von den `Startup` Klasse. Anwendungen, die Verwendung von EF Core in der Regel greifen Sie auf ihre Verbindungszeichenfolge aus der Konfiguration, also `Startup` allein reicht nicht mehr. Wenn Sie eine ASP.NET Core 1.x-Anwendung aktualisieren, erhalten Sie den folgenden Fehler, bei Verwendung der EF Core-Tools.

> Auf 'ApplicationContext' wurde kein parameterloser Konstruktor gefunden. "ApplicationContext" einen parameterlosen Konstruktor hinzu, oder fügen Sie eine Implementierung von "IDesignTimeDbContextFactory&lt;ApplicationContext&gt;" in der gleichen Assembly wie "ApplicationContext"

Ein neuer während der Entwurfszeit-Hook wurde in ASP.NET Core 2.0-Standardvorlage hinzugefügt. Die statische `Program.BuildWebHost` Methode können Sie EF Core zum Zugriff auf den Dienstanbieter für der Anwendung zur Entwurfszeit. Wenn Sie eine ASP.NET Core 1.x-Anwendung aktualisieren, müssen Sie zum Aktualisieren der `Program` Klasse, der wie folgt aussehen.

``` csharp
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;

namespace AspNetCoreDotNetCore2._0App
{
    public class Program
    {
        public static void Main(string[] args)
        {
            BuildWebHost(args).Run();
        }

        public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseStartup<Startup>()
                .Build();
    }
}
```

Die Einführung von diesem neuen Muster, wenn Anwendungen auf 2.0 aktualisieren, dringend empfohlen wird und, damit für Produktfunktionen wie Entity Framework Core-Migrationen erforderlich ist funktioniert. Die andere häufige Alternative ist die [implementieren *IDesignTimeDbContextFactory\<TContext >*](xref:core/miscellaneous/cli/dbcontext-creation#from-a-design-time-factory).

## <a name="idbcontextfactory-renamed"></a>IDbContextFactory umbenannt

Um verschiedene Anwendungsmuster zu unterstützen und Benutzern steuern, wie ihre `DbContext` dient zur Entwurfszeit, wir haben, in der Vergangenheit bereitgestellten der `IDbContextFactory<TContext>` Schnittstelle. Zur Entwurfszeit verwendet wird, die EF Core-Tools ermittelt Implementierungen dieser Schnittstelle in Ihrem Projekt und zum Erstellen von `DbContext` Objekte.

Diese Schnittstelle hatte einen sehr allgemeinen Namen, die einige versuchen erneut für andere Benutzer irreführen `DbContext`-Szenarien zu erstellen. Sie bei der EF-Tools versucht hat, verwenden Sie ihre Implementierung zur Entwurfszeit cloudsupport- und Befehle wie verursacht wurden `Update-Database` oder `dotnet ef database update` fehlschlägt.

Um die starke während der Entwurfszeit-Semantik dieser Schnittstelle kommunizieren zu können, haben wir es umbenannt `IDesignTimeDbContextFactory<TContext>`.

Version 2.0 der `IDbContextFactory<TContext>` weiterhin vorhanden, aber als veraltet markiert ist.

## <a name="dbcontextfactoryoptions-removed"></a>DbContextFactoryOptions entfernt.

Aufgrund der oben beschriebenen Änderungen ASP.NET Core 2.0 gefunden, die `DbContextFactoryOptions` war nicht mehr erforderlich, auf dem neuen `IDesignTimeDbContextFactory<TContext>` Schnittstelle. Hier sind die alternativen, die Sie stattdessen verwenden sollten.

| DbContextFactoryOptions | Alternative                                                  |
|:------------------------|:-------------------------------------------------------------|
| ApplicationBasePath     | AppContext.BaseDirectory                                     |
| ContentRootPath         | Directory.GetCurrentDirectory()                              |
| Umgebungsname         | Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") |

## <a name="design-time-working-directory-changed"></a>Während der Entwurfszeit-Arbeitsverzeichnis geändert

Die ASP.NET Core 2.0-Änderungen muss auch das Arbeitsverzeichnis ein, die `dotnet ef` an das mit dem Arbeitsverzeichnis, von Visual Studio verwendet werden, wenn die Anwendung ausgeführt. Ein Observable Nebeneffekt davon ist, SQLite, Dateinamen wird jetzt relativ zum Verzeichnis des Projekts und nicht das Ausgabeverzeichnis sind, wie sie verwendet werden.

## <a name="ef-core-20-requires-a-20-database-provider"></a>EF Core 2.0 erfordert einen 2.0-Anbieter

Für EF Core 2.0 haben wir viele vereinfachungen und Verbesserungen in den Anbietern Weise vorgenommen. Dies bedeutet, dass 1.0.x, und 1.1.x Anbieter nicht mit EF Core 2.0 funktioniert.

Die SQL Server- und SQLite-Anbieter werden vom EF-Team geliefert und 2.0-Versionen stehen als Teil der 2.0-Version. Die Open-Source-Drittanbieter-Anbieter für [SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), und [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) für 2.0 aktualisiert werden. Wenden Sie sich an den Anbieterwriter, für alle anderen Anbieter zu erhalten.

## <a name="logging-and-diagnostics-events-have-changed"></a>Protokollierung und Diagnose-Ereignissen haben sich geändert.

Hinweis: diese Änderungen sollten keine Auswirkungen auf den Großteil des Anwendungscodes.

Die Ereignis-IDs für Nachrichten an eine ["ILogger"](https://github.com/aspnet/Logging/blob/dev/src/Microsoft.Extensions.Logging.Abstractions/ILogger.cs) wurden in 2.0 geändert. Die Ereignis-IDs sind nun für alle EF Core-Codes eindeutig. Diese Nachrichten entsprechen nun auch dem Standardmuster für die strukturierte Protokollierung, das z.B. von MVC eingesetzt wird.

Die Protokollierungskategorien haben sich ebenfalls geändert. Nun gibt es eine bekannte Gruppe von Kategorien, auf die über [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs) zugegriffen werden kann.

[DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) Ereignisse verwenden nun dieselben Ereignis-ID-Namen wie die entsprechende `ILogger` Nachrichten. Die ereignisnutzlasten sind alle Nominale Typen, die von abgeleiteten [EventData](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/EventData.cs).

Ereignis-IDs, Nutzlasttypen und Kategorien sind dokumentiert, der [CoreEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/CoreEventId.cs) und [RelationalEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore.Relational/Diagnostics/RelationalEventId.cs) Klassen.

IDs wurden auch von Microsoft.EntityFrameworkCore.Infraestructure um den neuen Microsoft.EntityFrameworkCore.Diagnostics-Namespace verschoben.

## <a name="ef-core-relational-metadata-api-changes"></a>EF Core relationalen Metadaten-API-Änderungen

EF Core 2.0 erstellt nun eine andere [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs)-Klasse für die verschiedenen verwendeten Anbieter. Dies geschieht in der Regel auf eine für die Anwendung transparente Weise. Dies hat eine Vereinfachung der untergeordneten Metadaten-APIs ermöglicht, sodass Zugriffe auf _gemeinsame relationale Metadatenkonzepte_ immer durch Aufrufen von `.Relational` statt `.SqlServer`, `.Sqlite` usw. erfolgen. Z. B. 1.1.x Code wie folgt:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

Sollte jetzt wie folgt geschrieben werden:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

Statt Methoden wie `ForSqlServerToTable`, Erweiterungsmethoden sind jetzt verfügbar, bedingten basierten auf den aktuellen Anbieter verwendeten Code zu schreiben. Zum Beispiel:

```C#
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

Beachten Sie, die diese Änderung gilt nur für APIs/Metadaten, die für definiert ist _alle_ relationalen Anbieter. Die API und die Metadaten bleibt unverändert, wenn es nur einen einzelnen Anbieter spezifisch ist. Gruppierte Indizes sind z. B. für SQL Server, damit `ForSqlServerIsClustered` und `.SqlServer().IsClustered()` müssen immer noch verwendet werden.

## <a name="dont-take-control-of-the-ef-service-provider"></a>Nicht die Kontrolle über den EF-Dienstanbieter

EF Core verwendet ein internes `IServiceProvider` (DI-Containern) für die interne Implementierung. Anwendungen sollten die EF Core zum Erstellen und Verwalten von diesem Anbieter nur in besonderen Fällen ermöglichen. Sollten Sie alle Aufrufe von entfernen `UseInternalServiceProvider`. Wenn eine Anwendung aufrufen muss `UseInternalServiceProvider`, dann erwägen [erstellen ein Problem](https://github.com/aspnet/EntityFramework/Issues) , damit es andere Möglichkeiten zum Behandeln von Ihrem Szenarios untersuchen können.

Aufrufen von `AddEntityFramework`, `AddEntityFrameworkSqlServer`, usw. ist nicht vom Anwendungscode erforderlich, es sei denn, `UseInternalServiceProvider` wird auch als bezeichnet. Entfernen Sie alle vorhandenen Aufrufe `AddEntityFramework` oder `AddEntityFrameworkSqlServer`usw. `AddDbContext` sollte weiterhin verwendet werden auf die gleiche Weise wie zuvor.

## <a name="in-memory-databases-must-be-named"></a>In-Memory-Datenbanken müssen benannt werden

Globale unbenannte in-Memory-Datenbank wurde entfernt und stattdessen müssen alle in-Memory-Datenbanken den Namen. Zum Beispiel:

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

Dies erstellt/verwendet eine Datenbank mit dem Namen "MyDatabase". Wenn `UseInMemoryDatabase` erneut aufgerufen, mit dem gleichen Namen, die gleiche in-Memory-Datenbank verwendet werden wird, ermöglicht mehrere kontextinstanzen freigegeben werden.

## <a name="read-only-api-changes"></a>Nur-Lese API-Änderungen

`IsReadOnlyBeforeSave`, `IsReadOnlyAfterSave`, und `IsStoreGeneratedAlways` "ist veraltet und durch ersetzt wurden [BeforeSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L39) und [AfterSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L55). Dieses Verhalten gelten für jede Eigenschaft (nicht nur vom Speicher generierte Eigenschaften) und zu bestimmen, wie der Wert der Eigenschaft verwendet werden soll, beim Einfügen in die Zeile in einer Datenbank (`BeforeSaveBehavior`) oder beim Aktualisieren einer vorhandenen Zeile (`AfterSaveBehavior`).

Eigenschaften, die als markiert [ValueGenerated.OnAddOrUpdate](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/ValueGenerated.cs) (z. B. für berechnete Spalten) werden standardmäßig alle derzeit für die Eigenschaft festgelegten Wert ignoriert. Dies bedeutet, dass ein Wert vom Speicher generierte immer abgerufen werden wird, unabhängig davon, ob einen beliebigen Wert festgelegt oder auf die nachverfolgte Entität geändert wurde. Dies kann geändert werden, durch Festlegen einer anderen `Before\AfterSaveBehavior`.

## <a name="new-clientsetnull-delete-behavior"></a>Neue ClientSetNull Löschverhalten

In früheren Versionen [DeleteBehavior.Restrict](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/DeleteBehavior.cs) hatte ein Verhalten für Entitäten vom Kontext nachverfolgt, dass weitere übereinstimmende geschlossen `SetNull` Semantik. In EF Core 2.0 wird ein neues `ClientSetNull` Verhalten wurde als Standard für optionale Beziehungen eingeführt. Dieses Verhalten hat `SetNull` Semantik für nachverfolgte Entitäten und `Restrict` Verhalten für Datenbanken mit Entity Framework Core erstellt. Erfahrungsgemäß sind die erwarteten/nützlichsten Verhaltensweisen für nachverfolgte Entitäten und die Datenbank. `DeleteBehavior.Restrict` wird jetzt für nachverfolgte Entitäten, die bei Festlegung für optionale Beziehungen berücksichtigt werden.

## <a name="provider-design-time-packages-removed"></a>Während der Entwurfszeit anbieterpakete entfernt

Die `Microsoft.EntityFrameworkCore.Relational.Design` Paket entfernt wurde. Inhalten konsolidiert wurden `Microsoft.EntityFrameworkCore.Relational` und `Microsoft.EntityFrameworkCore.Design`.

Dies wird in der Entwurfszeit-anbieterpakete weitergegeben. Diese Pakete (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`usw.) entfernt wurden und deren Inhalt in die wichtigsten anbieterpakete konsolidiert.

So aktivieren Sie `Scaffold-DbContext` oder `dotnet ef dbcontext scaffold` in EF Core 2.0 müssen Sie nur das Paket für die einzelnen Anbieter zu verweisen:

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```
