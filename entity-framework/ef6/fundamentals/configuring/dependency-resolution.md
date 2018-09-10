---
title: Abhängigkeitsauflösung – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
ms.openlocfilehash: c6c56c3048e17a5c888ffe564e7606abf8b0c4ed
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251244"
---
# <a name="dependency-resolution"></a>Abhängigkeitsauflösung
> [!NOTE]
> **Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt. Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.  

Ab EF6 enthält Entity Framework einen allgemeinen Mechanismus zum Abrufen von Implementierungen der Dienste, die es benötigt. D. h. wenn EF eine Instanz der einige Schnittstellen oder Basisklassen verwendet fragt es eine konkrete Implementierung der Schnittstelle oder Base-Klasse verwenden. Dies erfolgt durch die Verwendung der Schnittstelle "idbdependencyresolver":  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

Die Methode "GetService" heißt in der Regel von EF und erfolgt durch eine Implementierung von "idbdependencyresolver" von EF oder von der Anwendung bereitgestellt wird. Wenn aufgerufen, wird das Typargument der Klassentyp-Schnittstelle oder in der der angeforderte Dienst, und das Objekt ist Null oder ein Objekt, das Kontextinformationen für den angeforderten Dienst bietet.  

Sofern nicht anders angegeben, alle zurückgegebene Objekt threadsicher sein muss, da er als Singleton verwendet werden kann. In vielen Fällen, die eine Factory ist in diesem Fall ist das zurückgegebene Objekt die Factory selbst threadsicher sein muss, aber das von der Factory zurückgegebene Objekt muss nicht threadsicher sein, da eine neue Instanz von der Factory für jede Verwendung angefordert wird.

Dieser Artikel enthält keine ausführliche Anleitung zum Implementieren von "idbdependencyresolver", aber stattdessen dient als Referenz für die Diensttypen (d. h. die Schnittstellen und Basisklassen Klassentypen) für die EF "GetService" und die Semantik des Schlüsselobjekts für jedes dieser Aufrufe wird aufgerufen.

## <a name="systemdataentityidatabaseinitializertcontext"></a>System.Data.Entity.IDatabaseInitializer < TContext\>  

**Eingeführt in Version**: EF6.0.0  

**Zurückgegebene Objekt**: einen Datenbankinitialisierer, für den angegebenen Kontext-Typ  

**Schlüssel**: nicht verwendet wird, wird null sein.  

## <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a>Func < System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\>  

**Eingeführt in Version**: EF6.0.0

**Zurückgegebene Objekt**: eine Factory, einen SQL-Generator zu erstellen, die für Migrationen und weiteren Aktionen, die dazu führen, eine Datenbank dass erstellt werden, wie z. B. die Erstellung der Datenbank mit der Datenbankinitialisierer verwendet werden kann.  

**Schlüssel**: eine Zeichenfolge mit ADO.NET invariante Name des Anbieters, die den Typ der Datenbank, die für die SQL generiert werden. Beispielsweise wird der SQL Server SQL-Generator für den Schlüssel "System.Data.SqlClient" zurückgegeben.  

>[!NOTE]
> Weitere Informationen zum Anbieter-bezogene Dienste in EF 6 finden Sie die [EF6-Anbietermodell](~/ef6/fundamentals/providers/provider-model.md) Abschnitt.  

## <a name="systemdataentitycorecommondbproviderservices"></a>System.Data.Entity.Core.Common.DbProviderServices  

**Eingeführt in Version**: EF6.0.0  

**Zurückgegebene Objekt**: des EF-Anbieter für einen angegebenen invarianten Anbieternamen verwenden  

**Schlüssel**: eine Zeichenfolge mit ADO.NET invariante Name des Anbieters, die den Typ der Datenbank, die für den ein Anbieter erforderlich ist. Beispielsweise wird der SQL Server-Anbieter für den Schlüssel "System.Data.SqlClient" zurückgegeben.  

>[!NOTE]
> Weitere Informationen zum Anbieter-bezogene Dienste in EF 6 finden Sie die [EF6-Anbietermodell](~/ef6/fundamentals/providers/provider-model.md) Abschnitt.  

## <a name="systemdataentityinfrastructureidbconnectionfactory"></a>System.Data.Entity.Infrastructure.IDbConnectionFactory  

**Eingeführt in Version**: EF6.0.0  

**Zurückgegebene Objekt**: der verbindungsfactory, das verwendet wird, wenn EF eine datenbankverbindung erstellt gemäß der Konvention. D. h. wenn keine Verbindung oder eine Verbindungszeichenfolge mit EF erhält und keine Verbindungszeichenfolge finden Sie in der Datei "App.config" oder "Web.config", klicken Sie dann diesen Dienst dient zum Erstellen einer Verbindung gemäß der Konvention. Ändern die verbindungsfactory können EF verwendet eine andere Art von Datenbank (z. B. SQL Server Compact Edition) in der Standardeinstellung.  

**Schlüssel**: nicht verwendet wird, wird null sein.  

>[!NOTE]
> Weitere Informationen zum Anbieter-bezogene Dienste in EF 6 finden Sie die [EF6-Anbietermodell](~/ef6/fundamentals/providers/provider-model.md) Abschnitt.  

## <a name="systemdataentityinfrastructureimanifesttokenservice"></a>System.Data.Entity.Infrastructure.IManifestTokenService  

**Eingeführt in Version**: EF6.0.0  

**Zurückgegebene Objekt**: ein Dienst, der von einer Verbindung ein anbietermanifesttokens generieren kann. Dieser Dienst wird in der Regel auf zwei Arten verwendet. Zuerst kann verwendet werden, um Code First, die Verbindung zur Datenbank beim Erstellen eines Modells zu vermeiden. Zweitens kann sie verwendet werden zu erzwingen, dass Code First ein Modell für eine bestimmte Datenbank-Version – z. B. zu erstellen, um ein Modell für SQL Server 2005 zu erzwingen, auch wenn manchmal SQL Server 2008 verwendet wird.  

**Objektlebensdauer**: Singleton – in das gleiche Objekt möglicherweise verwendet mehrere Wiederherstellungszeiten und gleichzeitig von unterschiedlichen Threads  

**Schlüssel**: nicht verwendet wird, wird null sein.  

## <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a>System.Data.Entity.Infrastructure.IDbProviderFactoryService  

**Eingeführt in Version**: EF6.0.0  

**Zurückgegebene Objekt**: ein Dienst, der eine bestimmte Verbindung eine Anbieterfactory abrufen kann. In .NET 4.5 ist der Anbieter von der Verbindung öffentlich zugänglich. Von .NET 4 verwendet die standardmäßige Implementierung von diesem Dienst Teil der Heuristik, um den entsprechenden Anbieter zu finden. Wenn dann, eine neue Implementierung dieses Diensts fehlschlagen können registriert werden, um eine geeignete Lösung zu bieten.  

**Schlüssel**: nicht verwendet wird, wird null sein.  

## <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a>Func < DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\>  

**Eingeführt in Version**: EF6.0.0  

**Zurückgegebene Objekt**: eine Factory, die einen Modell Cacheschlüssel für einen angegebenen Kontext generiert wird. Standardmäßig speichert EF ein Modell pro DbContext-Typ pro Anbieter. Eine andere Implementierung dieses Diensts kann verwendet werden, andere Informationen, z. B. Schemaname, der Cacheschlüssel hinzu.  

**Schlüssel**: nicht verwendet wird, wird null sein.  

## <a name="systemdataentityspatialdbspatialservices"></a>System.Data.Entity.Spatial.DbSpatialServices  

**Eingeführt in Version**: EF6.0.0  

**Zurückgegebene Objekt**: räumliche ein EF-Anbieter, das Unterstützung für den grundlegenden EF-Anbieter für Geography und Geometry räumliche Typen.  

**Schlüssel**: DbSptialServices für aufgefordert wird, gibt es zwei Möglichkeiten. Erste, anbieterspezifische räumlichen Dienste mithilfe eines Objekts DbProviderInfo angefordert werden (die invariante enthält Namen und das manifesttoken) als Schlüssel. Zweitens können DbSpatialServices ohne Schlüssel aufgefordert. Dies wird zum Auflösen des "räumlichen Anbieters" die beim Erstellen von eigenständigen DbGeography oder DbGeometry-Typen verwendet wird.  

>[!NOTE]
> Weitere Informationen zum Anbieter-bezogene Dienste in EF 6 finden Sie die [EF6-Anbietermodell](~/ef6/fundamentals/providers/provider-model.md) Abschnitt.  

## <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a>Func < System.Data.Entity.Infrastructure.IDbExecutionStrategy\>  

**Eingeführt in Version**: EF6.0.0  

**Zurückgegebene Objekt**: eine Factory zum Erstellen eines Diensts, der ermöglicht, dass einen Anbieter Wiederholungen oder anderem Verhalten implementieren, wenn Abfragen und Befehlen für die Datenbank ausgeführt werden. Wenn keine Implementierung bereitgestellt wird, wird Klicken Sie dann EF einfach führen Sie die Befehle und weitergegeben werden alle ausgelösten Ausnahmen. Für SQL Server ist dieser Dienst verwendet, um eine wiederholungsrichtlinie bereitzustellen, die was besonders hilfreich ist, bei der Ausführung für Cloud-basierten Datenbankserver, z. B. SQL Azure.  

**Schlüssel**: ein ExecutionStrategyKey-Objekt, das enthält, der invariante Anbietername und optional einen Servernamen ein, die für die Ausführungsstrategie verwendet werden.  

>[!NOTE]
> Weitere Informationen zum Anbieter-bezogene Dienste in EF 6 finden Sie die [EF6-Anbietermodell](~/ef6/fundamentals/providers/provider-model.md) Abschnitt.  

## <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a>Func < "DbConnection", "String", "System.Data.Entity.Migrations.History.HistoryContext\>  

**Eingeführt in Version**: EF6.0.0  

**Zurückgegebene Objekt**: eine Factory, die einen Anbieter so konfigurieren Sie die Zuordnung von der "historycontext" für ermöglicht die `__MigrationHistory` Tabelle, die von EF-Migrationen verwendet. Die "historycontext" ist der Code für einen ersten "DbContext" und kann mithilfe der normalen fluent-API so ändern Sie z.B. den Namen der Tabelle und den Mappingspezifikationen Spalte konfiguriert werden.  

**Schlüssel**: nicht verwendet wird, wird null sein.  

>[!NOTE]
> Weitere Informationen zum Anbieter-bezogene Dienste in EF 6 finden Sie die [EF6-Anbietermodell](~/ef6/fundamentals/providers/provider-model.md) Abschnitt.  

## <a name="systemdatacommondbproviderfactory"></a>System.Data.Common.DbProviderFactory  

**Eingeführt in Version**: EF6.0.0  

**Zurückgegebene Objekt**: der ADO NET-Anbieter für einen angegebenen invarianten Anbieternamen verwenden.  

**Schlüssel**: eine Zeichenfolge, enthält der invariante Anbietername für ADO.NET  

>[!NOTE]
> Dieser Dienst wird in der Regel nicht direkt geändert, da die Standardimplementierung der normalen anbieterregistrierung ADO.NET verwendet. Weitere Informationen zum Anbieter-bezogene Dienste in EF 6 finden Sie die [EF6-Anbietermodell](~/ef6/fundamentals/providers/provider-model.md) Abschnitt.  

## <a name="systemdataentityinfrastructureiproviderinvariantname"></a>System.Data.Entity.Infrastructure.IProviderInvariantName  

**Eingeführt in Version**: EF6.0.0  

**Zurückgegebene Objekt**: ein Dienst, der verwendet wird, um zu bestimmen, einen invarianten Anbieternamen für einen bestimmten Typ ' DbProviderFactory '. Die standardmäßige Implementierung dieses Diensts wird die Registrierung des Anbieters ADO.NET verwendet. Dies bedeutet, dass wenn der ADO NET-Anbieter nicht auf die übliche Weise registriert ist, da der "DbProviderFactory" von EF aufgelöst wird, klicken Sie dann es außerdem erforderlich, um diesen Dienst zu beheben kann.  

**Schlüssel**: der "DbProviderFactory"-Instanz, die für das invarianter Name erforderlich ist.  

>[!NOTE]
> Weitere Informationen zum Anbieter-bezogene Dienste in EF 6 finden Sie die [EF6-Anbietermodell](~/ef6/fundamentals/providers/provider-model.md) Abschnitt.  

## <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a>System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache  

**Eingeführt in Version**: EF6.0.0  

**Zurückgegebene Objekt**: einen Cache von Assemblys, die vorgenerierte Sichten enthalten. Eine Ersetzung wird normalerweise verwendet, um EF darüber informiert, welche Assemblys vorgenerierte Sichten enthalten, ohne alle Discovery zu ermöglichen.  

**Schlüssel**: nicht verwendet wird, wird null sein.  

## <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a>System.Data.Entity.Infrastructure.Pluralization.IPluralizationService

**Eingeführt in Version**: EF6.0.0  

**Zurückgegebene Objekt**: ein Dienst, der von EF zu pluralize singularize Namen verwendet. Standardmäßig wird eine englische pluralisierungsdienst verwendet.  

**Schlüssel**: nicht verwendet wird, wird null sein.  

## <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a>System.Data.Entity.Infrastructure.Interception.IDbInterceptor  

**Eingeführt in Version**: EF6.0.0

**Zurückgegebene Objekte**: alle Interceptors, die beim Starten der Anwendung registriert werden soll. Beachten Sie, dass diese Objekte werden durch den Aufruf GetServices angefordert, alle Dienste, die von jedem Abhängigkeitskonfliktlöser zurückgegeben werden registriert.

**Schlüssel**: nicht verwendet wird, wird null sein.  

## <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a>Func < System.Data.Entity.DbContext, Aktion < Zeichenfolge\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\>  

**Eingeführt in Version**: EF6.0.0  

**Zurückgegebene Objekt**: eine Factory, die mit den Datenbank-Log-Formatierer erstellen, die verwendet wird, wenn der Kontext. "Database.log"-Eigenschaft wird für den angegebenen Kontext festgelegt.  

**Schlüssel**: nicht verwendet wird, wird null sein.  

## <a name="funcsystemdataentitydbcontext"></a>Func < System.Data.Entity.DbContext\>  

**Eingeführt in Version**: EF6.1.0  

**Zurückgegebene Objekt**: eine Factory, die verwendet wird, um kontextinstanzen für Migrationen zu erstellen, wenn der Kontext nicht über einen zugänglichen parameterlosen Konstruktor verfügt.  

**Schlüssel**: das Typobjekt für den Typ des abgeleiteten "DbContext" für den eine Factory erforderlich ist.  

## <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a>Func < System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\>  

**Eingeführt in Version**: EF6.1.0  

**Zurückgegebene Objekt**: eine Factory, die Serialisierungsprogramme für die Serialisierung von benutzerdefinierten Anmerkungen mit fester typbindung erstellen, sie können serialisiert und in XML-Code für die Verwendung in Code First-Migrationen desterilized verwendet werden.  

**Schlüssel**: der Name der Anmerkung, die serialisiert oder deserialisiert.  

## <a name="funcsystemdataentityinfrastructuretransactionhandler"></a>Func < System.Data.Entity.Infrastructure.TransactionHandler\>  

**Eingeführt in Version**: EF6.1.0  

**Zurückgegebene Objekt**: eine Factory, die verwendet wird, um Handler für Transaktionen zu erstellen, sodass zum Beispiel Behandlung commitfehlern sinnvoll, spezielle Behandlung angewendet werden können.  

**Schlüssel**: ein ExecutionStrategyKey-Objekt, das enthält, der invariante Anbietername und optional einen Servernamen ein, die für die der Handler für die Transaktion verwendet werden.  
