---
title: Das Entity Framework 6-Anbietermodell - EF6
author: divega
ms.date: 2018-06-27
ms.assetid: 066832F0-D51B-4655-8BE7-C983C557E0E4
ms.openlocfilehash: ebe9b426b164f619b716ac221d1d94354f8b1fe5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997735"
---
# <a name="the-entity-framework-6-provider-model"></a>Das Entity Framework 6-Anbietermodell

Das Entity Framework-Anbieter ermöglicht Entity Framework, mit verschiedenen Typen von Datenbank-Server verwendet werden. Beispielsweise kann ein Anbieter angeschlossen werden, können Sie EF für Microsoft SQL Server verwendet werden, während ein anderer Anbieter in integriert werden kann, um EF für Microsoft SQL Server Compact Edition verwendet werden können. Die Anbieter für EF 6, die wir kennen finden Sie auf die [Entity Framework-Anbieter](~/ef6/fundamentals/providers/index.md) Seite.

Bestimmte Änderungen waren notwendig, um die Möglichkeit, die EF interagiert, Anbieter von EF unter einer open-Source-Lizenz freigegeben werden können. Diese Änderungen erfordern die Neuerstellung des EF-Anbieter für die EF6-Assemblys sowie neue Mechanismen für die Registrierung des Anbieters.

## <a name="rebuilding"></a>Neu erstellen

Mit EF6 ist die Core-Code, der zuvor Teil von .NET Framework war jetzt als Out-of-Band (OOB) Assemblys versendet. Informationen zum Erstellen von Anwendungen für EF6 finden Sie auf die [Aktualisieren von Anwendungen für EF6](~/ef6/what-is-new/upgrading-to-ef6.md) Seite. Anbieter müssen auch neu erstellt werden, die mithilfe der Anweisungen.

## <a name="provider-types-overview"></a>Übersicht über Steuerelementtypen für Anbieter

Ein EF-Anbieter ist wirklich eine Sammlung von anbieterspezifischen-Diensten, die definiert, die von CLR-Typen, die diese Dienste erweitern aus (für eine Basisklasse) oder (für eine Schnittstelle) implementieren. Zwei dieser Dienste sind grundlegende und erforderlich für EF funktionsfähig. Andere sind optional und müssen nur implementiert werden, wenn bestimmte Funktionalität erforderlich ist, und/oder die standardimplementierungen dieser Dienste funktioniert nicht für den bestimmten Datenbankserver abgezielt wird.

### <a name="fundamental-provider-types"></a>Grundlegende Anbietertypen

#### <a name="dbproviderfactory"></a>"DbProviderFactory"

EF ist das Vorhandensein von abgeleiteten Typs [System.Data.Common.DbProviderFactory](http://msdn.microsoft.com/en-us/library/system.data.common.dbproviderfactory.aspx) für die Durchführung aller Low-Level-Datenbankzugriff. "DbProviderFactory" ist nicht Bestandteil von EF, sondern ist stattdessen eine Klasse in .NET Framework, die einem Einstiegspunkt für ADO.NET-Anbieter dient kann verwendet werden von EF, andere O/RMs oder direkt durch eine Anwendung zum Abrufen von Instanzen von Verbindungen, Befehle und Parameter und andere ADO.NET Abstraktionen in einem Anbieter hostagnostische Weise. Weitere Informationen zu "DbProviderFactory" eine finden Sie in der [MSDN-Dokumentation für ADO.NET](http://msdn.microsoft.com/en-us/library/a6cd7c08.aspx).

#### <a name="dbproviderservices"></a>DbProviderServices

EF wird, hängt davon ab, dass DbProviderServices abgeleitet sind, für die Bereitstellung von zusätzliche Funktionen, die von EF auf die bereits von der ADO NET-Anbieter bereitgestellte Funktionalität benötigt. In früheren Versionen von EF die DbProviderServices-Klasse Teil von .NET Framework war und in den System.Data.Common-Namespace gefunden wurde. Ab EF6 diese Klasse ist jetzt Teil von EntityFramework.dll und befindet sich im Namespace System.Data.Entity.Core.Common.

Weitere Informationen zu grundlegenden Funktionen einer DbProviderServices-Implementierung finden Sie auf [MSDN](http://msdn.microsoft.com/en-us/library/ee789835.aspx). Beachten Sie jedoch, dass zum Zeitpunkt der Erstellung des Schreibens von diese Informationen nicht für EF6 aktualisiert wird, obwohl die meisten Konzepte noch gültig sind. Die SQL Server und SQL Server Compact-Implementierungen der DbProviderServices werden auch in überprüft, um die [Open-Source-Codebasis](https://gihtub.com/aspnet/EntityFramework6/) und dienen als nützliche Verweise bei anderen Implementierungen.

In früheren Versionen von EF wurde die DbProviderServices-Implementierung zu verwenden, direkt einem ADO.NET-Anbieter abgerufen. Dies erfolgte durch Umwandeln der "DbProviderFactory" in IServiceProvider umwandeln und Aufrufen der GetService-Methode. Eng gekoppelter dies den EF-Anbieter, der "DbProviderFactory". Diese Kopplung wird blockiert, dass EF aus .NET Framework verschoben werden und daher für EF6 diese enge Kopplung wurde entfernt und eine Implementierung von DbProviderServices ist nun registriert, direkt in der Konfigurationsdatei der Anwendung oder in Code-basierten Konfiguration, wie im Detail beschrieben die _registrieren DbProviderServices_ Abschnitt weiter unten.

### <a name="additional-services"></a>Zusätzliche Dienste

Zusätzlich zu den grundlegenden Dienste, die oben beschriebenen es gibt auch viele andere Dienste, die von EF verwendet die anbieterspezifische entweder ständig oder gelegentlich sind. Anbieterspezifische standardimplementierungen dieser Dienste können durch eine DbProviderServices-Implementierung angegeben werden. Anwendungen können auch die Implementierungen dieser Dienste zu überschreiben, oder Implementierungen bereitstellen, wenn ein DbProviderServices keinen Standardwert bereitstellt. Dies wird ausführlicher beschrieben die _zum Auflösen von zusätzlichen Diensten_ Abschnitt weiter unten.

Die zusätzliche Diensttypen, die ein Anbieter für einen Anbieter von Interesse sein können, sind unten aufgeführt. Weitere Informationen über jeden dieser Diensttypen finden Sie in der API-Dokumentation.

#### <a name="idbexecutionstrategy"></a>IDbExecutionStrategy

Dies ist ein optionaler Dienst, der ermöglicht, dass einen Anbieter Wiederholungen oder anderem Verhalten implementieren, wenn Abfragen und Befehlen für die Datenbank ausgeführt werden. Wenn keine Implementierung bereitgestellt wird, wird Klicken Sie dann EF einfach führen Sie die Befehle und weitergegeben werden alle ausgelösten Ausnahmen. Für SQL Server ist dieser Dienst verwendet, um eine wiederholungsrichtlinie bereitzustellen, die was besonders hilfreich ist, bei der Ausführung für Cloud-basierten Datenbankserver, z. B. SQL Azure.

#### <a name="idbconnectionfactory"></a>IDbConnectionFactory

Dies ist ein optionaler Dienst, der einen Anbieter zum Erstellen von "DbConnection"-Objekten gemäß der Konvention, wenn nur der Name einer Datenbank ermöglicht. Beachten Sie, dass dieser Dienst von einem DbProviderServices-Implementierung aufgelöst werden kann, die es wurde seit EF 4.1 vorhanden und kann auch explizit festgelegt werden in der Config-Datei oder im Code. Der Anbieter erhalten nur die Möglichkeit, diesen Dienst zu beheben, wenn es als Standardanbieter registriert (finden Sie unter _der Standardanbieter_ unten) und eine standardverbindungsfactory nicht an anderer Stelle festgelegt wurde.

#### <a name="dbspatialservices"></a>DbSpatialServices

Dies ist eine optionale Dienste, die einen Anbieter zum Hinzufügen der Unterstützung für Geography und Geometry räumliche Typen ermöglicht. Eine Implementierung dieses Diensts muss in der Reihenfolge für eine Anwendung für die Verwendung von EF mit räumlichen Typen angegeben werden. DbSptialServices wird auf zwei Arten zur aufgefordert. Erste, anbieterspezifische räumlichen Dienste mithilfe eines Objekts DbProviderInfo angefordert werden (die invariante enthält Namen und das manifesttoken) als Schlüssel. Zweitens können DbSpatialServices ohne Schlüssel aufgefordert. Dies wird zum Auflösen der "räumlichen Anbieter", die beim Erstellen von eigenständigen DbGeography oder DbGeometry-Typen verwendet wird.

#### <a name="migrationsqlgenerator"></a>MigrationSqlGenerator

Dies ist ein optionaler Dienst, mit dem EF-Migrationen durch Code First zum Generieren von SQL zum Erstellen und Ändern von Datenbankschemas verwendet werden kann. Eine Implementierung ist erforderlich, um Migrationen zu unterstützen. Wenn eine Implementierung bereitgestellt wird wird Klicken Sie dann es auch verwendet werden, wenn Datenbanken mit Datenbankinitialisierer oder der Database.Create-Methode erstellt werden.

#### <a name="funcdbconnection-string-historycontextfactory"></a>Func < "DbConnection", String, HistoryContextFactory >

Dies ist ein optionaler Dienst, der einen Anbieter so konfigurieren Sie die "historycontext" für die Zuordnung ermöglicht die `__MigrationHistory` Tabelle, die von EF-Migrationen verwendet. Die "historycontext" ist der Code für einen ersten "DbContext" und kann mithilfe der normalen fluent-API so ändern Sie z.B. den Namen der Tabelle und den Mappingspezifikationen Spalte konfiguriert werden. Die standardmäßige Implementierung des Diensts zurückgegeben, die von EF für alle Anbieter funktioniert möglicherweise für eine bestimmte Datenbank-Server, wenn alle die Tabelle und Spalte standardzuordnungen von diesem Anbieter unterstützt werden. In diesem Fall muss der Anbieter nicht auf eine Implementierung dieses Diensts bereitstellen.

#### <a name="idbproviderfactoryresolver"></a>IDbProviderFactoryResolver

Dies ist ein optionaler Dienst zum Abrufen der richtigen "DbProviderFactory" aus einem angegebenen DbConnection-Objekt. Die standardmäßige Implementierung des Diensts, die für alle Anbieter von EF zurückgegeben soll für alle Anbieter funktionieren. Bei der Ausführung in .NET 4 der "DbProviderFactory" ist jedoch nicht über einen öffentlich zugänglichen seine DbConnections. Aus diesem Grund verwendet EF Teil der Heuristik, um die registrierten Anbieter um eine Übereinstimmung zu finden. zu durchsuchen. Es ist möglich, dass für einige Anbieter diese Heuristiken fehl, und in solchen Situationen sollte der Anbieter eine neue Implementierung bereitstellen.

## <a name="registering-dbproviderservices"></a>Registrieren von DbProviderServices

Die DbProviderServices-Implementierung verwenden kann entweder in der Anwendung ("App.config" oder "Web.config") oder über die codebasierte Konfiguration registriert werden. In beiden Fällen wird durch die Registrierung des Anbieters "invarianten Namen" als Schlüssel verwendet. Dadurch können mehrere Anbieter registriert und in einer einzelnen Anwendung verwendet werden. Der invariante Name des EF-Registrierungen zum ist identisch mit der invariante Name des für die Registrierung und verbindungsherstellung anbieterzeichenfolgen des ADO.NET verwendet. Beispielsweise wird für SQL Server den invarianten Namen "System.Data.SqlClient" verwendet.

### <a name="config-file-registration"></a>Registrierung über die Konfigurationsdatei

Der zu verwendende DbProviderServices-Typ wird als ein Anbieterelement in der Anbieterliste der EntityFramework-Abschnitt der Konfigurationsdatei der Anwendung registriert. Zum Beispiel:

``` xml
<entityFramework>
  <providers>
    <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
  </providers>
</entityFramework>
```

Die _Typ_ Zeichenfolge muss die Assembly qualifizierten Typnamen der DbProviderServices-Implementierung verwendet werden.

### <a name="code-based-registration"></a>Codebasierte Registrierung

Beginnend mit der EF6-Anbieter kann auch registriert werden mithilfe von Code. Dies ermöglicht einen EF-Anbieter, ohne jede Änderung an der Konfigurationsdatei der Anwendung verwendet werden soll. Codebasierte Konfiguration verwenden sollte eine Anwendung eine "dbconfiguration"-Klasse erstellen, wie beschrieben in der [codebasierte Konfigurationsdokumentation](http://msdn.com/data/jj680699). Der Konstruktor der Klasse "dbconfiguration" sollten SetProviderServices zum Registrieren des EF-Anbieters aufrufen. Zum Beispiel:

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetProviderServices("My.New.Provider", new MyProviderServices());
    }
}
```

## <a name="resolving-additional-services"></a>Zum Auflösen von zusätzlichen Diensten

Wie bereits erwähnt in der _Anbieter Typen – Übersicht_ Abschnitt eine DbProviderServices Klasse kann auch verwendet werden, um zusätzliche Dienste aufzulösen. Dies ist möglich, da DbProviderServices "idbdependencyresolver implementiert", und jede registrierter DbProviderServices-Typ wird als ein "standardmäßiger Resolver" hinzugefügt. Die IDbDpendencyResolver-Mechanismus wird ausführlicher beschrieben [Abhängigkeitsauflösung](~/ef6/fundamentals/configuring/dependency-resolution.md). Jedoch ist es nicht erforderlich, um alle Konzepte in dieser Spezifikation zum Auflösen von zusätzlicher Diensten in einem Anbieter zu verstehen.

Die gängigste Methode für einen Anbieter zum Auflösen von zusätzlicher Diensten wird DbProviderServices.AddDependencyResolver für jeden Dienst in den Konstruktor der Klasse DbProviderServices aufzurufen. SqlProviderServices (der EF-Anbieter für SQL Server) hat beispielsweise Code wie diesen für die Initialisierung:

``` csharp
private SqlProviderServices()
{
    AddDependencyResolver(new SingletonDependencyResolver<IDbConnectionFactory>(
        new SqlConnectionFactory()));

    AddDependencyResolver(new ExecutionStrategyResolver<DefaultSqlExecutionStrategy>(
        "System.data.SqlClient", null, () => new DefaultSqlExecutionStrategy()));

    AddDependencyResolver(new SingletonDependencyResolver<Func<MigrationSqlGenerator>>(
        () => new SqlServerMigrationSqlGenerator(), "System.data.SqlClient"));

    AddDependencyResolver(new SingletonDependencyResolver<DbSpatialServices>(
        SqlSpatialServices.Instance,
        k =>
        {
            var asSpatialKey = k as DbProviderInfo;
            return asSpatialKey == null
                || asSpatialKey.ProviderInvariantName == ProviderInvariantName;
        }));
}
```

Dieser Konstruktor verwendet die folgenden Hilfsklassen:

*   SingletonDependencyResolver: bietet eine einfache Möglichkeit zum Auflösen der Singleton-Dienste –, also Dienste, die für die die gleiche Instanz zurückgegeben jedes Mal, dass "GetService" aufgerufen wird. Kurzlebige Dienste werden häufig als eine Singletonfactory registriert, die zum Erstellen temporärer Instanzen bei Bedarf verwendet werden.
*   ExecutionStrategyResolver: ein Resolver für IExecutionStrategy Implementierungen zurückgegeben.

Anstelle von DbProviderServices.AddDependencyResolver ist es auch möglich, außer Kraft setzen DbProviderServices.GetService und zusätzliche Dienste direkt zu beheben. Diese Methode wird aufgerufen werden, wenn EF ein Dienst, der definiert, von einem bestimmten Typ und in einigen Fällen für einen bestimmten Schlüssel benötigt. Die Methode muss der Dienst zurückgeben, wenn kann, oder null zurückgeben, um die Rückgabe des Diensts teilnehmen und eine andere Klasse zur Behebung lassen. Beispielsweise kann die standardverbindungsfactory der Behebung der Code in "GetService" wie folgt aussehen:

``` csharp
public override object GetService(Type type, object key)
{
    if (type == typeof(IDbConnectionFactory))
    {
        return new SqlConnectionFactory();
    }
    return null;
}
```

### <a name="registration-order"></a>Registrierungsreihenfolge

Wenn mehrere DbProviderServices-Implementierungen in der Konfigurationsdatei der Anwendung registriert sind werden sie als sekundären Resolver in der Reihenfolge hinzugefügt werden, dass sie aufgeführt sind. Da Konfliktlöser immer am Anfang der Kette sekundären Konfliktlöser, dass dies bedeutet hinzugefügt werden, dass der Anbieter am Ende der Liste zum Auflösen von Abhängigkeiten, bevor Sie die anderen werden wird. (Mag zunächst ein wenig widersinnig, aber es ist sinnvoll, wenn Sie sich vorstellen, nehmen jeder Anbieter aus der Liste, und es zusätzlich zu den vorhandenen Anbietern Stapeln.)

Diese Sortierung ist in der Regel unerheblich, da die meisten Anbieterdienste-spezifischen und vom invarianten Anbieternamen schlüsselgebundene sind. Allerdings werden für Dienste, die nicht nach Argumentnamen geordnet invariante Anbietername oder einige andere anbieterspezifische Schlüssel wird der Dienst nicht aufgelöst werden basierend auf dieser Reihenfolge. Z. B. Wenn sie nicht explizit anders an einer beliebigen Stelle andernfalls festgelegt ist, ist die standardverbindungsfactory vom obersten Anbieter in der Kette festgelegt.

## <a name="additional-config-file-registrations"></a>Zusätzliche Config-Datei-Registrierungen

Es ist möglich, einige der oben beschriebenen, direkt in der Konfigurationsdatei der Anwendung zusätzliche Anbieterdienste explizit zu registrieren. Anschließend wird die Registrierung in der Config-Datei werden keine zurückgegeben, die von der GetService-Methode, der die DbProviderServices-Implementierung verwendet werden.

### <a name="registering-the-default-connection-factory"></a>Die standardverbindungsfactory registrieren

Mit EF5 EntityFramework NuGet-Paket automatisch gestartet, werden entweder die verbindungsfactory SQL Express oder der LocalDb-verbindungsfactory in der Konfigurationsdatei registriert.

Zum Beispiel:

``` xml
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework" >
</entityFramework>
```

Die _Typ_ wird die Assembly qualifizierten Typnamen für die standardverbindungsfactory, die IDbConnectionFactory implementieren muss.

Es wird empfohlen, dass ein Anbieter-NuGet-Paket die standardverbindungsfactory auf diese Weise bei der Installation festgelegt. Finden Sie unter _NuGet-Pakete für Anbieter_ unten.

## <a name="additional-ef6-provider-changes"></a>Zusätzliche Änderungen der EF6-Anbieter

### <a name="spatial-provider-changes"></a>Räumliche Anbieter-Änderungen

Anbieter, die räumliche Typen zu unterstützen, müssen nun einige zusätzlichen Methoden für abgeleitete Klassen von DbSpatialDataReader implementieren:

*   `public abstract bool IsGeographyColumn(int ordinal)`
*   `public abstract bool IsGeometryColumn(int ordinal)`

Es gibt auch neue asynchrone Versionen der vorhandenen Methoden, die außer Kraft gesetzt werden, da die standardimplementierungen der synchronen Methoden delegieren und aus diesem Grund nicht asynchron führen sollten:

*   `public virtual Task<DbGeography> GetGeographyAsync(int ordinal, CancellationToken cancellationToken)`
*   `public virtual Task<DbGeometry> GetGeometryAsync(int ordinal, CancellationToken cancellationToken)`

### <a name="native-support-for-enumerablecontains"></a>Systemeigene Unterstützung für Enumerable.Contains

EF6 führt es sich um einen neuen Ausdruck ein, DbInExpression, die Leistungsprobleme für die Verwendung der Enumerable.Contains in LINQ-Abfragen hinzugefügt wurde. Die DbProviderManifest-Klasse verfügt über eine neue virtuelle Methode SupportsInExpression, die aufgerufen wird, von EF zu bestimmen, ob ein Anbieter den Typ des neuer Ausdrucks verarbeitet. Für die Kompatibilität mit vorhandenen Implementierungen für Anbieter gibt die Methode "false" zurück. Um diese Verbesserung nutzen, kann ein Anbieter für EF 6 Code hinzufügen, zum Verarbeiten von DbInExpression und außer Kraft setzen SupportsInExpression true zurück. Eine Instanz von DbInExpression kann durch Aufrufen der Methode DbExpressionBuilder.In erstellt werden. Eine Instanz DbInExpression besteht ein "DbExpression", in der Regel eine Spalte und eine Liste der DbConstantExpression auf Übereinstimmung zu prüfende darstellt.

## <a name="nuget-packages-for-providers"></a>NuGet-Pakete für Anbieter

Eine Möglichkeit, einen Anbieter für EF 6 verfügbar machen werden als NuGet-Paket freigeben. Verwenden ein NuGet-Paket bietet folgende Vorteile:

*   Es ist einfach zu verwenden NuGet Config-Datei der Anwendung die Registrierung des Anbieters hinzu
*   Weitere Änderungen können an der Config-Datei vorgenommen werden, die standardverbindungsfactory so festlegen, dass Verbindungen, die gemäß der Konvention das registrierte Anbieter verwenden
*   NuGet behandelt bindungsumleitungen hinzufügen, damit an, dass der EF6-Anbieter weiterhin funktionieren, auch nach der Veröffentlichung eines neuen EF-Pakets sollte

Ein Beispiel hierfür ist das EntityFramework.SqlServerCompact-Paket der in enthalten ist das [open-Source-Codebasis](http://github.com/aspnet/entityframework6). Dieses Paket enthält eine gute Vorlage zum Erstellen von NuGet-Pakete für die EF-Anbieter.

### <a name="powershell-commands"></a>PowerShell-Befehle

Wenn EntityFramework NuGet-Paket installiert wird, registriert sie ein PowerShell-Modul, das zwei Befehle enthält, die für die anbieterpakete sehr nützlich sind:

*   Hinzufügen EFProvider Fügt eine neue Entität, für den Anbieter in der Konfigurationsdatei für das Zielprojekt und stellt sicher, dass es am Ende der Liste der registrierten Anbieter ist.
*   Hinzufügen EFDefaultConnectionFactory entweder hinzugefügt oder aktualisiert die DefaultConnectionFactory-Registrierung in das Zielprojekt-Konfigurationsdatei.

Diese beiden Befehle übernehmen einen EntityFramework-Abschnitt der Config-Datei hinzufügen und eine Sammlung der Anbieter hinzufügen, falls erforderlich.

Es ist vorgesehen, dass diese Befehle aus dem NuGet-Skript Install. ps1 aufgerufen werden. Beispielsweise sieht Install. ps1 für den SQL Compact-Anbieter wie folgt aus:

``` powershell
param($installPath, $toolsPath, $package, $project)
Add-EFDefaultConnectionFactory $project 'System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework' -ConstructorArguments 'System.Data.SqlServerCe.4.0'
Add-EFProvider $project 'System.Data.SqlServerCe.4.0' 'System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact'</pre>
```

Weitere Informationen zu diesen Befehlen kann mithilfe von Get-Help im Fenster Paket-Manager-Konsole abgerufen werden.

## <a name="wrapping-providers"></a>Umschließen von Anbietern

Ein Umbruch-Anbieter ist ein EF und/oder ADO.NET-Anbieter, der einen vorhandenen Anbieter so erweitern, mit anderen Funktionen wie z. B. die profilerstellung oder Ablaufverfolgungsfunktionen umschließt. Umschließen die Anbieter auf die übliche Weise registriert werden kann, aber häufig ist es sinnvoll, den Anbieter "Wrapping" zur Laufzeit durch Abfangen der Auflösung des Anbieter-bezogene Dienste einrichten. Das statische Ereignis OnLockingConfiguration für die DbConfiguration-Klasse kann dazu verwendet werden.

OnLockingConfiguration wird aufgerufen, nachdem EF ermittelt hat, in dem die gesamte EF-Konfiguration für die Anwendungsdomäne von erhalten wird, aber bevor es für die Verwendung gesperrt wird. Beim Starten der app (bevor EF verwendet wird), sollten die app einen Ereignishandler für dieses Ereignis registrieren. (Wird derzeit die Unterstützung für die Registrierung dieser Handler in der Konfigurationsdatei hinzufügen, aber dies ist noch nicht unterstützt.) Der Ereignishandler muss dann ReplaceService für jeden Dienst aufrufen, die eingeschlossen werden muss.  

Beispielsweise sollte ein Handler könnte folgendermaßen aussehen IDbConnectionFactory und DbProviderService Wrapper, registriert werden:

``` csharp
DbConfiguration.OnLockingConfiguration +=
    (_, a) =>
    {
        a.ReplaceService<DbProviderServices>(
            (s, k) => new MyWrappedProviderServices(s));

        a.ReplaceService<IDbConnectionFactory>(
            (s, k) => new MyWrappedConnectionFactory(s));
    };
```

Der Dienst, der wurde behoben und nun zusammen mit den Schlüssel, der mit den Dienst aufgelöst wurde umschlossen werden, an den Handler übergeben. Der Handler kann dann umschließen diesen Dienst und den zurückgegebenen Dienst durch die umschlossene Version ersetzen.

## <a name="resolving-a-dbproviderfactory-with-ef"></a>Auflösen von einer "DbProviderFactory" in EF

"DbProviderFactory" ist eines der grundlegenden Anbietertypen von EF benötigt werden, wie beschrieben in der _Anbieter Typen – Übersicht_ weiter oben. Wie bereits erwähnt, kann kein EF-Typ und Registrierung ist in der Regel nicht Teil der EF-Konfiguration, aber es wird stattdessen der normale ADO.NET anbieterregistrierung in der Datei "Machine.config" und/oder der Konfigurationsdatei der Anwendung.

Trotz dieser EF verwendet immer noch der Standardmechanismus für die normale Abhängigkeit bei der Suche nach einer "DbProviderFactory" verwenden. Der Standardresolver verwendet die normale Registrierung für ADO.NET in die Konfigurationsdateien und in der Regel transparent ist. Aufgrund der normalen abhängigkeitsauflösung Mechanismus verwendet wird, aber dies bedeutet, dass eine "idbdependencyresolver" verwendet werden kann, um einer "DbProviderFactory" zu beheben, selbst wenn die normalen ADO.NET-Registrierung nicht ausgeführt wurde.

Auflösung von DbProviderFactory. auf diese Weise hat mehrere Auswirkungen:

*   Eine Anwendung mit codebasierte Konfiguration kann Aufrufe in die DbConfiguration-Klasse, zum Registrieren der entsprechenden "DbProviderFactory" hinzufügen. Dies ist besonders nützlich für Anwendungen, die nicht möchten (oder nicht) stellen eine dateibasierte Konfiguration zu verwenden.
*   Der Dienst kann eingebunden werden oder mithilfe von ReplaceService Siehe ersetzt die _Wrapping Anbieter_ weiter oben
*   Theoretisch könnte eine DbProviderServices-Implementierung eine "DbProviderFactory" aufgelöst werden.

Wichtig zu beachten, die über eine der folgenden Aufgaben ausführen, ist, dass sie nur die Suche nach "DbProviderFactory" von EF auswirkt. Andere nicht EF-Code möglicherweise dennoch erwarten, dass der ADO NET-Anbieter auf die übliche Weise registriert werden und kann fehlschlagen, wenn die Registrierung nicht gefunden wird. Aus diesem Grund ist es normalerweise besser, für die einer "DbProviderFactory" wie gewohnt ADO.NET registriert werden.

### <a name="related-services"></a>Verwandte Dienste

Wenn EF zum Auflösen einer "DbProviderFactory" verwendet wird, sollten auch die IProviderInvariantName und IDbProviderFactoryResolver-Dienste auflösen.

IProviderInvariantName ist ein Dienst, der verwendet wird, um zu bestimmen, einen invarianten Anbieternamen für einen bestimmten Typ ' DbProviderFactory '. Die standardmäßige Implementierung dieses Diensts wird die Registrierung des Anbieters ADO.NET verwendet. Dies bedeutet, dass wenn der ADO NET-Anbieter nicht auf die übliche Weise registriert ist, da der "DbProviderFactory" von EF aufgelöst wird, klicken Sie dann es außerdem erforderlich, um diesen Dienst zu beheben kann. Beachten Sie, dass ein Konfliktlöser für diesen Dienst automatisch hinzugefügt wird, wenn die DbConfiguration.SetProviderFactory-Methode verwendet.

Siehe die _Anbieter Typen – Übersicht_ Abschnitt, der IDbProviderFactoryResolver verwendet, um die richtige "DbProviderFactory" aus einem angegebenen DbConnection-Objekt abzurufen. Die standardmäßige Implementierung des Diensts bei der Ausführung von .NET 4 wird die Registrierung des Anbieters ADO.NET verwendet. Dies bedeutet, dass wenn der ADO NET-Anbieter nicht auf die übliche Weise registriert ist, da der "DbProviderFactory" von EF aufgelöst wird, klicken Sie dann es außerdem erforderlich, um diesen Dienst zu beheben kann.
