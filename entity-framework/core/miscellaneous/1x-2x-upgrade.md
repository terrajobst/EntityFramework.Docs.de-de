---
title: Aktualisieren von früheren Versionen auf EF Core 2-EF Core
author: divega
ms.date: 08/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: b27c09fdb6210dd7c6aa0c8bc912a8bd183c16b9
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414230"
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a>Aktualisieren von Anwendungen von früheren Versionen auf EF Core 2,0

Wir haben die Gelegenheit genommen, unsere vorhandenen APIs und Verhaltensweisen in 2,0 deutlich zu verfeinern. Es gibt einige Verbesserungen, die das Ändern von vorhandenem Anwendungscode erfordern, obwohl wir davon ausgehen, dass für die Mehrzahl der Anwendungen die Auswirkung gering ist, in den meisten Fällen nur eine Neukompilierung und minimale gesteuerte Änderungen, um veraltete APIs zu ersetzen.

Zum Aktualisieren einer vorhandenen Anwendung auf EF Core 2,0 ist möglicherweise Folgendes erforderlich:

1. Aktualisieren der .NET-Ziel Implementierung der Anwendung auf eine, die .NET Standard 2,0 unterstützt. Weitere Informationen finden Sie [unter Unterstützte .net-Implementierungen](xref:core/platforms/index) .

2. Identifizieren Sie einen Anbieter für die Zieldatenbank, der mit EF Core 2,0 kompatibel ist. Weitere Informationen finden Sie unter [EF Core 2,0 erfordert einen 2,0-Datenbankanbieter](#ef-core-20-requires-a-20-database-provider) .

3. Aktualisieren aller EF Core Pakete (Runtime und Tools) auf 2,0. Weitere Informationen finden Sie unter [Installieren von EF Core](xref:core/get-started/install/index) .

4. Nehmen Sie erforderliche Codeänderungen vor, um die im restlichen Dokument beschriebenen wichtigen Änderungen auszugleichen.

## <a name="aspnet-core-now-includes-ef-core"></a>ASP.net Core jetzt enthalten EF Core

Anwendungen, deren Zielversionen für ASP.NET Core 2.0 festgelegt sind, können neben Datenbankanbietern von Drittanbietern EF Core 2.0 ohne zusätzliche Abhängigkeiten nutzen. Anwendungen, die auf frühere Versionen ASP.net Core von abzielen, müssen jedoch auf ASP.net Core 2,0 aktualisiert werden, damit EF Core 2,0 verwendet werden kann. Weitere Informationen zum Aktualisieren von ASP.net Core Anwendungen auf 2,0 finden Sie in [der ASP.net Core Dokumentation zu diesem Thema](/aspnet/core/migration/1x-to-2x/).

## <a name="new-way-of-getting-application-services-in-aspnet-core"></a>Neue Methode zum erhalten von Anwendungsdiensten in ASP.net Core

Das empfohlene Muster für ASP.net Core Webanwendungen wurde für 2,0 auf eine Weise aktualisiert, die die Entwurfszeit Logik EF Core, die in 1. x verwendet wurde. Zuvor wurde zur Entwurfszeit EF Core versucht, `Startup.ConfigureServices` direkt aufzurufen, um auf den Dienstanbieter der Anwendung zuzugreifen. In ASP.net Core 2,0 wird die Konfiguration außerhalb der `Startup`-Klasse initialisiert. Anwendungen, die EF Core verwenden, greifen in der Regel auf Ihre Verbindungs Zeichenfolge aus der Konfiguration zu, sodass `Startup` allein nicht mehr ausreicht. Wenn Sie ein Upgrade für eine ASP.net Core 1. x-Anwendung durchführen, erhalten Sie möglicherweise den folgenden Fehler, wenn Sie die EF Core Tools verwenden.

> In "ApplicationContext" wurde kein Parameter loser Konstruktor gefunden. Fügen Sie "ApplicationContext" einen Parameter losen Konstruktor hinzu, oder fügen Sie eine Implementierung von "idesigntimedbcontextfactory&lt;ApplicationContext&gt;" in derselben Assembly wie "ApplicationContext" hinzu.

In ASP.net Core 2.0-Standardvorlage wurde ein neuer Entwurfszeit Hook hinzugefügt. Die statische `Program.BuildWebHost`-Methode ermöglicht EF Core den Zugriff auf den Dienstanbieter der Anwendung zur Entwurfszeit. Wenn Sie eine ASP.net Core 1. x-Anwendung aktualisieren, müssen Sie die `Program`-Klasse so aktualisieren, dass Sie der folgenden ähnelt.

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

Die Annahme dieses neuen Musters beim Aktualisieren von Anwendungen auf 2,0 wird dringend empfohlen und ist erforderlich, damit Produkt Features wie Entity Framework Core Migrationen funktionieren. Die andere gängige Alternative ist die [Implementierung von *idesigntimedbcontextfactory\<tcontext >* ](xref:core/miscellaneous/cli/dbcontext-creation#from-a-design-time-factory).

## <a name="idbcontextfactory-renamed"></a>Idbcontextfactory umbenannt

Um verschiedene Anwendungs Muster zu unterstützen und Benutzern mehr Kontrolle darüber zu bieten, wie Ihre `DbContext` zur Entwurfszeit verwendet werden, haben wir in der Vergangenheit die `IDbContextFactory<TContext>` Schnittstelle bereitgestellt. Zur Entwurfszeit werden die EF Core Tools Implementierungen dieser Schnittstelle in Ihrem Projekt ermitteln und zum Erstellen von `DbContext` Objekten verwenden.

Diese Schnittstelle enthielt einen allgemeinen Namen, der einige Benutzer irre, um Sie für andere `DbContext`erstellende Szenarien wiederzuverwenden. Sie wurden von Guard abgefangen, als die EF-Tools anschließend versuchten, ihre Implementierung zur Entwurfszeit zu verwenden, und Befehle wie `Update-Database` oder `dotnet ef database update` zu einem Fehler führten.

Um die starke Entwurfszeit Semantik dieser Schnittstelle zu kommunizieren, haben wir Sie in "`IDesignTimeDbContextFactory<TContext>`" umbenannt.

Bei Version 2,0 ist die `IDbContextFactory<TContext>` weiterhin vorhanden, aber als veraltet markiert.

## <a name="dbcontextfactoryoptions-removed"></a>Dbcontextfactor yoptions entfernt

Aufgrund der oben beschriebenen ASP.net Core 2,0-Änderungen stellten wir fest, dass `DbContextFactoryOptions` in der neuen `IDesignTimeDbContextFactory<TContext>` Schnittstelle nicht mehr benötigt wurde. Sie sollten stattdessen die Alternativen verwenden, die Sie stattdessen verwenden sollten.

| DbContextFactoryOptions | Alternative                                                  |
|:------------------------|:-------------------------------------------------------------|
| ApplicationBasePath     | AppContext.BaseDirectory                                     |
| ContentRootPath         | Directory. GetCurrentDirectory ()                              |
| EnvironmentName         | Environment. GetEnvironmentVariable ("ASPNETCORE_ENVIRONMENT") |

## <a name="design-time-working-directory-changed"></a>Entwurfszeit-Arbeitsverzeichnis geändert

Die ASP.net Core 2,0-Änderungen erforderten außerdem, dass das Arbeitsverzeichnis, das von `dotnet ef` verwendet wird, dem Arbeitsverzeichnis entspricht, das von Visual Studio beim Ausführen der Anwendung verwendet wird. Ein beobachtbarer Nebeneffekt besteht darin, dass SQLite-Dateinamen jetzt relativ zum Projektverzeichnis und nicht zum Ausgabeverzeichnis sind, wie Sie es verwendet haben.

## <a name="ef-core-20-requires-a-20-database-provider"></a>EF Core 2,0 erfordert einen 2,0-Datenbankanbieter.

Bei EF Core 2,0 haben wir viele Vereinfachungen und Verbesserungen bei der Funktionsweise von Datenbankanbietern vorgenommen. Dies bedeutet, dass die Anbieter 1.0. x und 1.1. x nicht mit EF Core 2,0 funktionieren.

Die SQL Server-und SQLite-Anbieter werden vom EF-Team geliefert, und 2,0-Versionen werden als Teil der 2,0-Version verfügbar sein. Die Open Source-Anbieter von Drittanbietern für [SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL)und [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) werden für 2,0 aktualisiert. Wenden Sie sich für alle anderen Anbieter an den Anbieter-Writer.

## <a name="logging-and-diagnostics-events-have-changed"></a>Protokollierungs-und Diagnose Ereignisse wurden geändert.

Hinweis: diese Änderungen sollten sich nicht auf den meisten Anwendungscode auswirken.

Die Ereignis-IDs für an eine [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) gesendete Nachrichten wurden in 2,0 geändert. Die Ereignis-IDs sind nun für alle EF Core-Codes eindeutig. Diese Nachrichten entsprechen nun auch dem Standardmuster für die strukturierte Protokollierung, das z.B. von MVC eingesetzt wird.

Die Protokollierungskategorien haben sich ebenfalls geändert. Nun gibt es eine bekannte Gruppe von Kategorien, auf die über [DbLoggerCategory](https://github.com/aspnet/EntityFrameworkCore/blob/rel/2.0.0/src/EFCore/DbLoggerCategory.cs) zugegriffen werden kann.

[Diagnosticsource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) -Ereignisse verwenden nun die gleichen Ereignis-ID-Namen wie die entsprechenden `ILogger` Nachrichten. Die Ereignis Nutzlasten sind alle nominalen Typen, die von [EventData](/dotnet/api/microsoft.entityframeworkcore.diagnostics.eventdata)abgeleitet werden.

Ereignis-IDs, Nutz Last Typen und Kategorien sind in den Klassen [coreeventid](/dotnet/api/microsoft.entityframeworkcore.diagnostics.coreeventid) und [relationaleventid](/dotnet/api/microsoft.entityframeworkcore.diagnostics.relationaleventid) dokumentiert.

IDs wurden auch aus Microsoft. entityframeworkcore. Infrastructure in den neuen Microsoft. entityframeworkcore. Diagnostics-Namespace verschoben.

## <a name="ef-core-relational-metadata-api-changes"></a>EF Core relationale Metadaten-API-Änderungen

EF Core 2.0 erstellt nun eine andere [IModel](/dotnet/api/microsoft.entityframeworkcore.metadata.imodel)-Klasse für die verschiedenen verwendeten Anbieter. Dies geschieht in der Regel auf eine für die Anwendung transparente Weise. Dies hat eine Vereinfachung von Metadaten auf niedrigerer Ebene ermöglicht, sodass jeglicher Zugriff auf _Allgemeine relationale Metadatenkonzepte_ immer durch einen Aufrufen von `.Relational` statt `.SqlServer`, `.Sqlite`usw. erfolgt. Beispielsweise ist der Code 1.1. x wie folgt:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

Sollte nun wie folgt geschrieben werden:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

Anstatt Methoden wie `ForSqlServerToTable`zu verwenden, stehen nun Erweiterungs Methoden zum Schreiben von bedingtem Code zur Verfügung, der auf dem verwendeten aktuellen Anbieter basiert. Beispiel:

```csharp
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

Beachten Sie, dass diese Änderung nur für APIs/Metadaten gilt, die für _alle_ relationalen Anbieter definiert sind. Die API und die Metadaten bleiben unverändert, wenn Sie nur für einen einzelnen Anbieter spezifisch sind. Gruppierte Indizes sind z. b. für SQL Server spezifisch, sodass `ForSqlServerIsClustered` und `.SqlServer().IsClustered()` weiterhin verwendet werden müssen.

## <a name="dont-take-control-of-the-ef-service-provider"></a>Übernehmen Sie keine Kontrolle über den EF-Dienstanbieter

EF Core verwendet einen internen `IServiceProvider` (einen Container für die Abhängigkeitsinjektion) für die interne Implementierung. Anwendungen sollten es EF Core ermöglichen, diesen Anbieter zu erstellen und zu verwalten, außer in besonderen Fällen. Ziehen Sie in Erwägung, alle Aufrufe von `UseInternalServiceProvider`zu entfernen. Wenn eine Anwendung `UseInternalServiceProvider`aufrufen muss, sollten Sie erwägen, [ein Problem](https://github.com/aspnet/EntityFramework/Issues) zu melden, damit wir andere Möglichkeiten zur Handhabung Ihres Szenarios untersuchen können.

Das Aufrufen von `AddEntityFramework`, `AddEntityFrameworkSqlServer`usw. ist für den Anwendungscode nur erforderlich, wenn `UseInternalServiceProvider` auch aufgerufen wird. Entfernen Sie alle vorhandenen Aufrufe von `AddEntityFramework` oder `AddEntityFrameworkSqlServer`, usw. `AddDbContext` sollte weiterhin auf dieselbe Weise wie zuvor verwendet werden.

## <a name="in-memory-databases-must-be-named"></a>In-Memory-Datenbanken müssen benannt werden

Die globale unbenannte in-Memory Database wurde entfernt, und stattdessen müssen alle in-Memory-Datenbanken benannt werden. Beispiel:

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

Dadurch wird eine Datenbank mit dem Namen "MyDatabase" erstellt oder verwendet. Wenn `UseInMemoryDatabase` erneut mit dem gleichen Namen aufgerufen wird, wird derselbe in-Memory Database verwendet, sodass er von mehreren Kontext Instanzen gemeinsam genutzt werden kann.

## <a name="read-only-api-changes"></a>Schreibgeschützte API-Änderungen

`IsReadOnlyBeforeSave`, `IsReadOnlyAfterSave`und `IsStoreGeneratedAlways` sind veraltet und wurden durch " [beforesavebehavior](/dotnet/api/microsoft.entityframeworkcore.metadata.iproperty.beforesavebehavior) " und " [aftersavebehavior](/dotnet/api/microsoft.entityframeworkcore.metadata.iproperty.aftersavebehavior)" ersetzt. Diese Verhalten gelten für jede Eigenschaft (nicht nur vom Speicher generierte Eigenschaften) und bestimmen, wie der Wert der Eigenschaft beim Einfügen in eine Datenbankzeile (`BeforeSaveBehavior`) oder beim Aktualisieren einer vorhandenen Datenbankzeile (`AfterSaveBehavior`) verwendet werden soll.

Eigenschaften, die als [valuegenerated. onaddorupdate](/dotnet/api/microsoft.entityframeworkcore.metadata.valuegenerated) gekennzeichnet sind (z. b. für berechnete Spalten), ignorieren standardmäßig alle Werte, die derzeit für die Eigenschaft festgelegt sind. Dies bedeutet, dass ein vom Speicher generierter Wert immer abgerufen wird, unabhängig davon, ob ein Wert für die nach verfolgte Entität festgelegt oder geändert wurde. Dies kann geändert werden, indem eine andere `Before\AfterSaveBehavior`festgelegt wird.

## <a name="new-clientsetnull-delete-behavior"></a>Neues clientsetnull-Lösch Verhalten

In früheren Versionen hatte [deleteBehavior. Limit](/dotnet/api/microsoft.entityframeworkcore.deletebehavior) ein Verhalten für Entitäten, die vom Kontext nachverfolgt wurden, der die übereinstimmende Übereinstimmung `SetNull` Semantik aufwies. In EF Core 2,0 wurde ein neues `ClientSetNull` Verhalten als Standardwert für optionale Beziehungen eingeführt. Dieses Verhalten hat `SetNull` Semantik für nach verfolgte Entitäten und `Restrict` Verhalten für Datenbanken, die mit EF Core erstellt wurden. In unserer Darstellung sind dies die am häufigsten erwarteten/nützlichen Verhalten für nach verfolgte Entitäten und die Datenbank. `DeleteBehavior.Restrict` wird jetzt für nach verfolgte Entitäten berücksichtigt, wenn Sie für optionale Beziehungen festgelegt wird.

## <a name="provider-design-time-packages-removed"></a>Anbieter Entwurfszeit Pakete entfernt

Das `Microsoft.EntityFrameworkCore.Relational.Design`-Paket wurde entfernt. Der Inhalt wurde in `Microsoft.EntityFrameworkCore.Relational` und `Microsoft.EntityFrameworkCore.Design`konsolidiert.

Dies wird in die Anbieter-Entwurfszeit Pakete weitergegeben. Diese Pakete (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`usw.) wurden entfernt, und ihre Inhalte wurden in den Hauptanbieter Paketen konsolidiert.

Um `Scaffold-DbContext` oder `dotnet ef dbcontext scaffold` in EF Core 2,0 zu aktivieren, müssen Sie nur auf das einzelne Anbieter Paket verweisen:

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```
