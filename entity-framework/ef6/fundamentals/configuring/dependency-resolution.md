---
title: Abhängigkeitsauflösung-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
ms.openlocfilehash: 6082124481f5795bbcb62fff2bb6a58ecdcb48e4
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414794"
---
# <a name="dependency-resolution"></a><span data-ttu-id="df458-102">Abhängigkeitsauflösung</span><span class="sxs-lookup"><span data-stu-id="df458-102">Dependency resolution</span></span>
> [!NOTE]
> <span data-ttu-id="df458-103">**Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="df458-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="df458-104">Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.</span><span class="sxs-lookup"><span data-stu-id="df458-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="df458-105">Ab EF6 enthält Entity Framework einen allgemeinen Mechanismus zum Abrufen von erforderlichen Implementierungen von Diensten.</span><span class="sxs-lookup"><span data-stu-id="df458-105">Starting with EF6, Entity Framework contains a general-purpose mechanism for obtaining implementations of services that it requires.</span></span> <span data-ttu-id="df458-106">Das heißt, wenn EF eine Instanz einiger Schnittstellen oder Basisklassen verwendet, wird eine konkrete Implementierung der-Schnittstelle oder der Basisklasse angefordert, die verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="df458-106">That is, when EF uses an instance of some interfaces or base classes it will ask for a concrete implementation of the interface or base class to use.</span></span> <span data-ttu-id="df458-107">Dies wird durch die Verwendung der idbdependencyresolver-Schnittstelle erreicht:</span><span class="sxs-lookup"><span data-stu-id="df458-107">This is achieved through use of the IDbDependencyResolver interface:</span></span>  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

<span data-ttu-id="df458-108">Die GetService-Methode wird in der Regel von EF aufgerufen und wird von einer Implementierung von idbdependencyresolver behandelt, die entweder von EF oder der Anwendung bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="df458-108">The GetService method is typically called by EF and is handled by an implementation of IDbDependencyResolver provided either by EF or by the application.</span></span> <span data-ttu-id="df458-109">Beim Aufruf ist das Typargument die Schnittstelle oder der Basis Klassentyp des angeforderten Dienstanbieter, das Schlüsselobjekt ist entweder NULL oder ein Objekt, das Kontextinformationen zum angeforderten Dienst bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="df458-109">When called, the type argument is the interface or base class type of the service being requested, and the key object is either null or an object providing contextual information about the requested service.</span></span>  

<span data-ttu-id="df458-110">Sofern nicht anders angegeben, muss jedes zurückgegebene Objekt Thread sicher sein, da es als Singleton verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="df458-110">Unless otherwise stated any object returned must be thread-safe since it can be used as a singleton.</span></span> <span data-ttu-id="df458-111">In vielen Fällen ist das zurückgegebene Objekt eine Factory. in diesem Fall muss die Factory selbst Thread sicher sein, aber das von der Factory zurückgegebene Objekt muss nicht Thread sicher sein, da eine neue Instanz für jede Verwendung von der Factory angefordert wird.</span><span class="sxs-lookup"><span data-stu-id="df458-111">In many cases the object returned is a factory in which case the factory itself must be thread-safe but the object returned from the factory does not need to be thread-safe since a new instance is requested from the factory for each use.</span></span>

<span data-ttu-id="df458-112">Dieser Artikel enthält keine vollständigen Details zur Implementierung von idbdependencyresolver, sondern fungiert als Referenz für die Dienst Typen (d. h. die Schnittstelle und Basisklassen Typen), für die EF GetService aufruft, und die Semantik des Schlüssel Objekts für jede dieser auf.</span><span class="sxs-lookup"><span data-stu-id="df458-112">This article does not contain full details on how to implement IDbDependencyResolver, but instead acts as a reference for the service types (that is, the interface and base class types) for which EF calls GetService and the semantics of the key object for each of these calls.</span></span>

## <a name="systemdataentityidatabaseinitializertcontext"></a><span data-ttu-id="df458-113">System. Data. Entity. idatabaseinitializer < tcontext\></span><span class="sxs-lookup"><span data-stu-id="df458-113">System.Data.Entity.IDatabaseInitializer<TContext\></span></span>  

<span data-ttu-id="df458-114">**Eingeführte Version**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="df458-114">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="df458-115">**Zurück**gegebenes Objekt: ein datenbankinitialisierer für den angegebenen Kontexttyp.</span><span class="sxs-lookup"><span data-stu-id="df458-115">**Object returned**: A database initializer for the given context type</span></span>  

<span data-ttu-id="df458-116">**Schlüssel**: nicht verwendet; wird NULL sein.</span><span class="sxs-lookup"><span data-stu-id="df458-116">**Key**: Not used; will be null</span></span>  

## <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a><span data-ttu-id="df458-117">Func < System. Data. Entity. Migrations. SQL. migrationsqlgenerator\></span><span class="sxs-lookup"><span data-stu-id="df458-117">Func<System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span></span>  

<span data-ttu-id="df458-118">**Eingeführte Version**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="df458-118">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="df458-119">**Zurück**gegebenes Objekt: eine Factory zum Erstellen eines SQL-Generators, der für Migrationen und andere Aktionen verwendet werden kann, die eine Datenbank erstellen, z. b. die Erstellung von Datenbanken mit datenbankinitialisierern.</span><span class="sxs-lookup"><span data-stu-id="df458-119">**Object returned**: A factory to create a SQL generator that can be used for Migrations and other actions that cause a database to be created, such as database creation with database initializers.</span></span>  

<span data-ttu-id="df458-120">**Key**: eine Zeichenfolge, die den invarianten Namen des ADO.NET-Anbieters enthält, der den Typ der Datenbank angibt, für die SQL generiert wird.</span><span class="sxs-lookup"><span data-stu-id="df458-120">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which SQL will be generated.</span></span> <span data-ttu-id="df458-121">Beispielsweise wird der SQL Server SQL-Generator für den Schlüssel "System. Data. SqlClient" zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="df458-121">For example, the SQL Server SQL generator is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="df458-122">Weitere Informationen zu Anbieter bezogenen Diensten in EF6 finden Sie im Abschnitt [EF6 Provider Model](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="df458-122">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycorecommondbproviderservices"></a><span data-ttu-id="df458-123">System. Data. Entity. Core. Common. DbProviderServices</span><span class="sxs-lookup"><span data-stu-id="df458-123">System.Data.Entity.Core.Common.DbProviderServices</span></span>  

<span data-ttu-id="df458-124">**Eingeführte Version**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="df458-124">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="df458-125">**Zurück**gegebenes Objekt: der EF-Anbieter, der für den invarianten Namen eines angegebenen Anbieters verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="df458-125">**Object returned**: The EF provider to use for a given provider invariant name</span></span>  

<span data-ttu-id="df458-126">**Key**: eine Zeichenfolge, die den invarianten Namen des ADO.NET-Anbieters enthält, der den Typ der Datenbank angibt, für die ein Anbieter benötigt wird.</span><span class="sxs-lookup"><span data-stu-id="df458-126">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which a provider is needed.</span></span> <span data-ttu-id="df458-127">Beispielsweise wird der SQL Server Anbieter für den Schlüssel "System. Data. SqlClient" zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="df458-127">For example, the SQL Server provider is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="df458-128">Weitere Informationen zu Anbieter bezogenen Diensten in EF6 finden Sie im Abschnitt [EF6 Provider Model](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="df458-128">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureidbconnectionfactory"></a><span data-ttu-id="df458-129">System. Data. Entity. Infrastructure. idbconnectionfactory</span><span class="sxs-lookup"><span data-stu-id="df458-129">System.Data.Entity.Infrastructure.IDbConnectionFactory</span></span>  

<span data-ttu-id="df458-130">**Eingeführte Version**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="df458-130">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="df458-131">**Zurück**gegebenes Objekt: die Verbindungsfactory, die verwendet wird, wenn EF eine Datenbankverbindung gemäß der Konvention erstellt.</span><span class="sxs-lookup"><span data-stu-id="df458-131">**Object returned**: The connection factory that will be used when EF creates a database connection by convention.</span></span> <span data-ttu-id="df458-132">Das heißt, wenn EF keine Verbindung oder Verbindungs Zeichenfolge erhält und keine Verbindungs Zeichenfolge in der Datei "App. config" oder "Web. config" gefunden werden kann, wird dieser Dienst zum Erstellen einer Verbindung gemäß Konvention verwendet.</span><span class="sxs-lookup"><span data-stu-id="df458-132">That is, when no connection or connection string is given to EF, and no connection string can be found in the app.config or web.config, then this service is used to create a connection by convention.</span></span> <span data-ttu-id="df458-133">Wenn Sie die Verbindungsfactory ändern, kann EF standardmäßig einen anderen Typ von Datenbank (z. b. SQL Server Compact Edition) verwenden.</span><span class="sxs-lookup"><span data-stu-id="df458-133">Changing the connection factory can allow EF to use a different type of database (for example, SQL Server Compact Edition) by default.</span></span>  

<span data-ttu-id="df458-134">**Schlüssel**: nicht verwendet; wird NULL sein.</span><span class="sxs-lookup"><span data-stu-id="df458-134">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="df458-135">Weitere Informationen zu Anbieter bezogenen Diensten in EF6 finden Sie im Abschnitt [EF6 Provider Model](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="df458-135">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureimanifesttokenservice"></a><span data-ttu-id="df458-136">System. Data. Entity. Infrastructure. IManifestTokenService</span><span class="sxs-lookup"><span data-stu-id="df458-136">System.Data.Entity.Infrastructure.IManifestTokenService</span></span>  

<span data-ttu-id="df458-137">**Eingeführte Version**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="df458-137">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="df458-138">**Zurück**gegebenes Objekt: ein Dienst, der ein Anbieter Manifest-Token aus einer Verbindung generieren kann.</span><span class="sxs-lookup"><span data-stu-id="df458-138">**Object returned**: A service that can generate a provider manifest token from a connection.</span></span> <span data-ttu-id="df458-139">Dieser Dienst wird in der Regel auf zweierlei Weise verwendet.</span><span class="sxs-lookup"><span data-stu-id="df458-139">This service is typically used in two ways.</span></span> <span data-ttu-id="df458-140">Zuerst kann Sie verwendet werden, um zu vermeiden, dass beim Entwickeln eines Modells Code First eine Verbindung mit der Datenbank hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="df458-140">First, it can be used to avoid Code First connecting to the database when building a model.</span></span> <span data-ttu-id="df458-141">Zweitens kann Sie verwendet werden, um Code First zu erzwingen, ein Modell für eine bestimmte Datenbankversion zu erstellen, z. b. um ein Modell für SQL Server 2005 zu erzwingen, auch wenn manchmal SQL Server 2008 verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="df458-141">Second, it can be used to force Code First to build a model for a specific database version -- for example, to force a model for SQL Server 2005 even if sometimes SQL Server 2008 is used.</span></span>  

<span data-ttu-id="df458-142">**Objekt Lebensdauer**: Singleton--das gleiche Objekt kann mehrmals und gleichzeitig von verschiedenen Threads verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="df458-142">**Object lifetime**: Singleton -- the same object may be used multiple times and concurrently by different threads</span></span>  

<span data-ttu-id="df458-143">**Schlüssel**: nicht verwendet; wird NULL sein.</span><span class="sxs-lookup"><span data-stu-id="df458-143">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a><span data-ttu-id="df458-144">System. Data. Entity. Infrastructure. idbproviderfactoryservice</span><span class="sxs-lookup"><span data-stu-id="df458-144">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span></span>  

<span data-ttu-id="df458-145">**Eingeführte Version**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="df458-145">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="df458-146">**Zurück**gegebenes Objekt: ein Dienst, der eine Anbieterfactory von einer bestimmten Verbindung abrufen kann.</span><span class="sxs-lookup"><span data-stu-id="df458-146">**Object returned**: A service that can obtain a provider factory from a given connection.</span></span> <span data-ttu-id="df458-147">Unter .NET 4,5 ist der Anbieter öffentlich über die Verbindung zugänglich.</span><span class="sxs-lookup"><span data-stu-id="df458-147">On .NET 4.5 the provider is publicly accessible from the connection.</span></span> <span data-ttu-id="df458-148">In .NET 4 verwendet die Standard Implementierung dieses Dienstanbieters einige Heuristiken, um den passenden Anbieter zu suchen.</span><span class="sxs-lookup"><span data-stu-id="df458-148">On .NET 4 the default implementation of this service uses some heuristics to find the matching provider.</span></span> <span data-ttu-id="df458-149">Wenn diese Fehler auftreten, kann eine neue Implementierung dieses Dienstanbieter registriert werden, um eine geeignete Lösung bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="df458-149">If these fail then a new implementation of this service can be registered to provide an appropriate resolution.</span></span>  

<span data-ttu-id="df458-150">**Schlüssel**: nicht verwendet; wird NULL sein.</span><span class="sxs-lookup"><span data-stu-id="df458-150">**Key**: Not used; will be null</span></span>  

## <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a><span data-ttu-id="df458-151">Func < dbcontext, System. Data. Entity. Infrastructure. idbmodelcachekey\></span><span class="sxs-lookup"><span data-stu-id="df458-151">Func<DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span></span>  

<span data-ttu-id="df458-152">**Eingeführte Version**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="df458-152">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="df458-153">**Zurück**gegebenes Objekt: eine Factory, die einen Modell Cache Schlüssel für einen bestimmten Kontext generiert.</span><span class="sxs-lookup"><span data-stu-id="df458-153">**Object returned**: A factory that will generate a model cache key for a given context.</span></span> <span data-ttu-id="df458-154">Standardmäßig speichert EF ein Modell pro dbcontext-Typ pro Anbieter zwischen.</span><span class="sxs-lookup"><span data-stu-id="df458-154">By default, EF caches one model per DbContext type per provider.</span></span> <span data-ttu-id="df458-155">Eine andere Implementierung dieses Dienstanbieter kann verwendet werden, um weitere Informationen, wie z. b. den Schema Namen, zum Cache Schlüssel hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="df458-155">A different implementation of this service can be used to add other information, such as schema name, to the cache key.</span></span>  

<span data-ttu-id="df458-156">**Schlüssel**: nicht verwendet; wird NULL sein.</span><span class="sxs-lookup"><span data-stu-id="df458-156">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityspatialdbspatialservices"></a><span data-ttu-id="df458-157">System. Data. Entity. Spatial. dbspatialservices</span><span class="sxs-lookup"><span data-stu-id="df458-157">System.Data.Entity.Spatial.DbSpatialServices</span></span>  

<span data-ttu-id="df458-158">**Eingeführte Version**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="df458-158">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="df458-159">**Zurück**gegebenes Objekt: ein EF-Anbieter für räumliche Daten, der dem einfachen EF-Anbieter Unterstützung für räumliche Geography-und geometry-Typen hinzufügt</span><span class="sxs-lookup"><span data-stu-id="df458-159">**Object returned**: An EF spatial provider that adds support to the basic EF provider for geography and geometry spatial types.</span></span>  

<span data-ttu-id="df458-160">**Schlüssel**: dbsptialservices wird auf zwei Arten angefordert.</span><span class="sxs-lookup"><span data-stu-id="df458-160">**Key**: DbSptialServices is asked for in two ways.</span></span> <span data-ttu-id="df458-161">Zuerst werden anbieterspezifische räumliche Dienste mithilfe eines dbproviderinfo-Objekts (das den invarianten Namen und das Manifest-Token enthält) als Schlüssel angefordert.</span><span class="sxs-lookup"><span data-stu-id="df458-161">First, provider-specific spatial services are requested using a DbProviderInfo object (which contains invariant name and manifest token) as the key.</span></span> <span data-ttu-id="df458-162">Zweitens können dbspatialservices ohne Schlüssel angefordert werden.</span><span class="sxs-lookup"><span data-stu-id="df458-162">Second, DbSpatialServices can be asked for with no key.</span></span> <span data-ttu-id="df458-163">Dies wird zum Auflösen des "globalen räumlichen Anbieters" verwendet, der beim Erstellen eigenständiger dbgeography-oder dbgeometry-Typen verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="df458-163">This is used to resolve the "global spatial provider" which is used when creating stand-alone DbGeography or DbGeometry types.</span></span>  

>[!NOTE]
> <span data-ttu-id="df458-164">Weitere Informationen zu Anbieter bezogenen Diensten in EF6 finden Sie im Abschnitt [EF6 Provider Model](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="df458-164">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a><span data-ttu-id="df458-165">Func < System. Data. Entity. Infrastructure. idbexecutionstrategy\></span><span class="sxs-lookup"><span data-stu-id="df458-165">Func<System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span></span>  

<span data-ttu-id="df458-166">**Eingeführte Version**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="df458-166">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="df458-167">**Zurück**gegebenes Objekt: eine Factory zum Erstellen eines Dienstanbieters, der es einem Anbieter ermöglicht, Wiederholungs Versuche oder andere Verhalten zu implementieren, wenn Abfragen und Befehle für die Datenbank ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="df458-167">**Object returned**: A factory to create a service that allows a provider to implement retries or other behavior when queries and commands are executed against the database.</span></span> <span data-ttu-id="df458-168">Wenn keine Implementierung bereitgestellt wird, führt EF die Befehle einfach aus und verteilt alle ausgelösten Ausnahmen.</span><span class="sxs-lookup"><span data-stu-id="df458-168">If no implementation is provided, then EF will simply execute the commands and propagate any exceptions thrown.</span></span> <span data-ttu-id="df458-169">Bei SQL Server dieser Dienst zum Bereitstellen einer Wiederholungs Richtlinie verwendet, die besonders nützlich ist, wenn Sie auf cloudbasierten Datenbankservern wie SQL Azure ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="df458-169">For SQL Server this service is used to provide a retry policy which is especially useful when running against cloud-based database servers such as SQL Azure.</span></span>  

<span data-ttu-id="df458-170">**Key**: ein executionstrategykey-Objekt, das den invarianten Namen des Anbieters und optional einen Servernamen enthält, für den die Ausführungs Strategie verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="df458-170">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the execution strategy will be used.</span></span>  

>[!NOTE]
> <span data-ttu-id="df458-171">Weitere Informationen zu Anbieter bezogenen Diensten in EF6 finden Sie im Abschnitt [EF6 Provider Model](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="df458-171">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a><span data-ttu-id="df458-172">Func < DbConnection, String, System. Data. Entity. Migrationen. History. historycontext\></span><span class="sxs-lookup"><span data-stu-id="df458-172">Func<DbConnection, string, System.Data.Entity.Migrations.History.HistoryContext\></span></span>  

<span data-ttu-id="df458-173">**Eingeführte Version**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="df458-173">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="df458-174">**Zurück**gegebenes Objekt: eine Factory, mit der ein Anbieter die Zuordnung von historycontext zu der von EF-Migrationen verwendeten `__MigrationHistory` Tabelle konfigurieren kann.</span><span class="sxs-lookup"><span data-stu-id="df458-174">**Object returned**: A factory that allows a provider to configure the mapping of the HistoryContext to the `__MigrationHistory` table used by EF Migrations.</span></span> <span data-ttu-id="df458-175">Der historycontext ist eine Code First dbcontext und kann mit der normalen flüssigen API konfiguriert werden, um Dinge wie den Namen der Tabelle und die Spalten Mappingspezifikationen zu ändern.</span><span class="sxs-lookup"><span data-stu-id="df458-175">The HistoryContext is a Code First DbContext and can be configured using the normal fluent API to change things like the name of the table and the column mapping specifications.</span></span>  

<span data-ttu-id="df458-176">**Schlüssel**: nicht verwendet; wird NULL sein.</span><span class="sxs-lookup"><span data-stu-id="df458-176">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="df458-177">Weitere Informationen zu Anbieter bezogenen Diensten in EF6 finden Sie im Abschnitt [EF6 Provider Model](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="df458-177">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdatacommondbproviderfactory"></a><span data-ttu-id="df458-178">System. Data. Common. DbProviderFactory</span><span class="sxs-lookup"><span data-stu-id="df458-178">System.Data.Common.DbProviderFactory</span></span>  

<span data-ttu-id="df458-179">**Eingeführte Version**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="df458-179">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="df458-180">**Zurück**gegebenes Objekt: der ADO.NET-Anbieter, der für den invarianten Namen eines angegebenen Anbieters verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="df458-180">**Object returned**: The ADO.NET provider to use for a given provider invariant name.</span></span>  

<span data-ttu-id="df458-181">**Key**: eine Zeichenfolge, die den invarianten Namen des ADO.NET-Anbieters enthält</span><span class="sxs-lookup"><span data-stu-id="df458-181">**Key**: A string containing the ADO.NET provider invariant name</span></span>  

>[!NOTE]
> <span data-ttu-id="df458-182">Dieser Dienst wird in der Regel nicht direkt geändert, da die Standard Implementierung die normale ADO.NET-Anbieter Registrierung verwendet.</span><span class="sxs-lookup"><span data-stu-id="df458-182">This service is not usually changed directly since the default implementation uses the normal ADO.NET provider registration.</span></span> <span data-ttu-id="df458-183">Weitere Informationen zu Anbieter bezogenen Diensten in EF6 finden Sie im Abschnitt [EF6 Provider Model](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="df458-183">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureiproviderinvariantname"></a><span data-ttu-id="df458-184">System. Data. Entity. Infrastructure. iproviderinvariantname</span><span class="sxs-lookup"><span data-stu-id="df458-184">System.Data.Entity.Infrastructure.IProviderInvariantName</span></span>  

<span data-ttu-id="df458-185">**Eingeführte Version**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="df458-185">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="df458-186">**Zurück**gegebenes Objekt: ein Dienst, der verwendet wird, um einen invarianten Anbieter Namen für einen bestimmten Typ von "DbProviderFactory" zu ermitteln.</span><span class="sxs-lookup"><span data-stu-id="df458-186">**Object returned**: a service that is used to determine a provider invariant name for a given type of DbProviderFactory.</span></span> <span data-ttu-id="df458-187">Die Standard Implementierung dieses Dienstanbieters verwendet die ADO.NET-Anbieter Registrierung.</span><span class="sxs-lookup"><span data-stu-id="df458-187">The default implementation of this service uses the ADO.NET provider registration.</span></span> <span data-ttu-id="df458-188">Dies bedeutet Folgendes: Wenn der ADO.NET-Anbieter nicht auf die normale Weise registriert ist, weil DbProviderFactory von EF aufgelöst wird, muss dieser Dienst ebenfalls aufgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="df458-188">This means that if the ADO.NET provider is not registered in the normal way because DbProviderFactory is being resolved by EF, then it will also be necessary to resolve this service.</span></span>  

<span data-ttu-id="df458-189">**Key**: die DbProviderFactory-Instanz, für die ein invarianter Name erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="df458-189">**Key**: The DbProviderFactory instance for which an invariant name is required.</span></span>  

>[!NOTE]
> <span data-ttu-id="df458-190">Weitere Informationen zu Anbieter bezogenen Diensten in EF6 finden Sie im Abschnitt [EF6 Provider Model](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="df458-190">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a><span data-ttu-id="df458-191">System. Data. Entity. Core. Mapping. viewgene. iviewassemblycache</span><span class="sxs-lookup"><span data-stu-id="df458-191">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span></span>  

<span data-ttu-id="df458-192">**Eingeführte Version**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="df458-192">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="df458-193">**Zurück**gegebenes Objekt: ein Cache der Assemblys, die vorgenerierte Sichten enthalten.</span><span class="sxs-lookup"><span data-stu-id="df458-193">**Object returned**: a cache of the assemblies that contain pre-generated views.</span></span> <span data-ttu-id="df458-194">Ein Ersatz wird normalerweise verwendet, um EF darüber zu informieren, welche Assemblys vorab generierte Sichten enthalten, ohne dass eine Ermittlung durchgeführt wird</span><span class="sxs-lookup"><span data-stu-id="df458-194">A replacement is typically used to let EF know which assemblies contain pre-generated views without doing any discovery.</span></span>  

<span data-ttu-id="df458-195">**Schlüssel**: nicht verwendet; wird NULL sein.</span><span class="sxs-lookup"><span data-stu-id="df458-195">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a><span data-ttu-id="df458-196">System. Data. Entity. Infrastructure. Pluralization. ipluralizationservice</span><span class="sxs-lookup"><span data-stu-id="df458-196">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span></span>

<span data-ttu-id="df458-197">**Eingeführte Version**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="df458-197">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="df458-198">**Zurück**gegebenes Objekt: ein Dienst, der von EF zum pluralisieren und singularisieren von Namen verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="df458-198">**Object returned**: a service used by EF to pluralize and singularize names.</span></span> <span data-ttu-id="df458-199">Standardmäßig wird ein englischer pluralisierungs Dienst verwendet.</span><span class="sxs-lookup"><span data-stu-id="df458-199">By default an English pluralization service is used.</span></span>  

<span data-ttu-id="df458-200">**Schlüssel**: nicht verwendet; wird NULL sein.</span><span class="sxs-lookup"><span data-stu-id="df458-200">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a><span data-ttu-id="df458-201">System. Data. Entity. Infrastructure. intercep. idbinterceptor</span><span class="sxs-lookup"><span data-stu-id="df458-201">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span></span>  

<span data-ttu-id="df458-202">**Eingeführte Version**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="df458-202">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="df458-203">**Zurückgegebene Objekte**: alle Interceptors, die beim Starten der Anwendung registriert werden sollen.</span><span class="sxs-lookup"><span data-stu-id="df458-203">**Objects returned**: Any interceptors that should be registered when the application starts.</span></span> <span data-ttu-id="df458-204">Beachten Sie, dass diese Objekte mithilfe des GetServices-Aufrufes angefordert werden und alle von einem Abhängigkeits Konflikt Löser zurückgegebenen Interceptors registriert werden.</span><span class="sxs-lookup"><span data-stu-id="df458-204">Note that these objects are requested using the GetServices call and all interceptors returned by any dependency resolver will registered.</span></span>

<span data-ttu-id="df458-205">**Schlüssel**: nicht verwendet; wird NULL sein.</span><span class="sxs-lookup"><span data-stu-id="df458-205">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a><span data-ttu-id="df458-206">Func < System. Data. Entity. dbcontext, Action < String\>, System. Data. Entity. Infrastructure. intercep. databaselogformatter\></span><span class="sxs-lookup"><span data-stu-id="df458-206">Func<System.Data.Entity.DbContext, Action<string\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span></span>  

<span data-ttu-id="df458-207">**Eingeführte Version**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="df458-207">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="df458-208">**Zurück**gegebenes Objekt: eine Factory, die verwendet wird, um den Daten Bank Protokoll-Formatierer zu erstellen, der verwendet wird, wenn der Kontext verwendet wird. Die Database. Log-Eigenschaft wird für den angegebenen Kontext festgelegt.</span><span class="sxs-lookup"><span data-stu-id="df458-208">**Object returned**: A factory that will be used to create the database log formatter that will be used when the context.Database.Log property is set on the given context.</span></span>  

<span data-ttu-id="df458-209">**Schlüssel**: nicht verwendet; wird NULL sein.</span><span class="sxs-lookup"><span data-stu-id="df458-209">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext"></a><span data-ttu-id="df458-210">Func < System. Data. Entity. dbcontext\></span><span class="sxs-lookup"><span data-stu-id="df458-210">Func<System.Data.Entity.DbContext\></span></span>  

<span data-ttu-id="df458-211">**Eingeführte Version**: EF 6.1.0</span><span class="sxs-lookup"><span data-stu-id="df458-211">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="df458-212">**Zurück**gegebenes Objekt: eine Factory, die verwendet wird, um Kontext Instanzen für Migrationen zu erstellen, wenn der Kontext keinen zugänglichen Parameter losen Konstruktor hat.</span><span class="sxs-lookup"><span data-stu-id="df458-212">**Object returned**: A factory that will be used to create context instances for Migrations when the context does not have an accessible parameterless constructor.</span></span>  

<span data-ttu-id="df458-213">**Key**: das Type-Objekt für den Typ des abgeleiteten dbcontext, für den eine Factory benötigt wird.</span><span class="sxs-lookup"><span data-stu-id="df458-213">**Key**: The Type object for the type of the derived DbContext for which a factory is needed.</span></span>  

## <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a><span data-ttu-id="df458-214">Func < System. Data. Entity. Core. Metadata. Edm. IMetadataAnnotationSerializer\></span><span class="sxs-lookup"><span data-stu-id="df458-214">Func<System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span></span>  

<span data-ttu-id="df458-215">**Eingeführte Version**: EF 6.1.0</span><span class="sxs-lookup"><span data-stu-id="df458-215">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="df458-216">**Zurück**gegebenes Objekt: eine Factory, die verwendet wird, um Serialisierungsprogrammen für die Serialisierung von stark typisierten benutzerdefinierten Anmerkungen zu erstellen, sodass Sie serialisiert und in XML umgewandelt werden können, um Sie in Code First-Migrationen zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="df458-216">**Object returned**: A factory that will be used to create serializers for serialization of strongly-typed custom annotations such that they can be serialized and desterilized into XML for use in Code First Migrations.</span></span>  

<span data-ttu-id="df458-217">**Key**: der Name der Anmerkung, die serialisiert oder deserialisiert wird.</span><span class="sxs-lookup"><span data-stu-id="df458-217">**Key**: The name of the annotation that is being serialized or deserialized.</span></span>  

## <a name="funcsystemdataentityinfrastructuretransactionhandler"></a><span data-ttu-id="df458-218">Func < System. Data. Entity. Infrastructure. transaktionhandler\></span><span class="sxs-lookup"><span data-stu-id="df458-218">Func<System.Data.Entity.Infrastructure.TransactionHandler\></span></span>  

<span data-ttu-id="df458-219">**Eingeführte Version**: EF 6.1.0</span><span class="sxs-lookup"><span data-stu-id="df458-219">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="df458-220">**Zurück**gegebenes Objekt: eine Factory, die zum Erstellen von Handlern für Transaktionen verwendet wird, sodass eine spezielle Behandlung für Situationen wie das Behandeln von commitfehlern angewendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="df458-220">**Object returned**: A factory that will be used to create handlers for transactions so that special handling can be applied for situations such as handling commit failures.</span></span>  

<span data-ttu-id="df458-221">**Key**: ein executionstrategykey-Objekt, das den invarianten Namen des Anbieters und optional einen Servernamen enthält, für den der Transaktions Handler verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="df458-221">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the transaction handler will be used.</span></span>  
