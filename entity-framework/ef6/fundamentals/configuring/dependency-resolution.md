---
title: Abhängigkeitsauflösung – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
ms.openlocfilehash: 6082124481f5795bbcb62fff2bb6a58ecdcb48e4
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490960"
---
# <a name="dependency-resolution"></a><span data-ttu-id="539a4-102">Abhängigkeitsauflösung</span><span class="sxs-lookup"><span data-stu-id="539a4-102">Dependency resolution</span></span>
> [!NOTE]
> <span data-ttu-id="539a4-103">**Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="539a4-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="539a4-104">Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.</span><span class="sxs-lookup"><span data-stu-id="539a4-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="539a4-105">Ab EF6 enthält Entity Framework einen allgemeinen Mechanismus zum Abrufen von Implementierungen der Dienste, die es benötigt.</span><span class="sxs-lookup"><span data-stu-id="539a4-105">Starting with EF6, Entity Framework contains a general-purpose mechanism for obtaining implementations of services that it requires.</span></span> <span data-ttu-id="539a4-106">D. h. wenn EF eine Instanz der einige Schnittstellen oder Basisklassen verwendet fragt es eine konkrete Implementierung der Schnittstelle oder Base-Klasse verwenden.</span><span class="sxs-lookup"><span data-stu-id="539a4-106">That is, when EF uses an instance of some interfaces or base classes it will ask for a concrete implementation of the interface or base class to use.</span></span> <span data-ttu-id="539a4-107">Dies erfolgt durch die Verwendung der Schnittstelle "idbdependencyresolver":</span><span class="sxs-lookup"><span data-stu-id="539a4-107">This is achieved through use of the IDbDependencyResolver interface:</span></span>  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

<span data-ttu-id="539a4-108">Die Methode "GetService" heißt in der Regel von EF und erfolgt durch eine Implementierung von "idbdependencyresolver" von EF oder von der Anwendung bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="539a4-108">The GetService method is typically called by EF and is handled by an implementation of IDbDependencyResolver provided either by EF or by the application.</span></span> <span data-ttu-id="539a4-109">Wenn aufgerufen, wird das Typargument der Klassentyp-Schnittstelle oder in der der angeforderte Dienst, und das Objekt ist Null oder ein Objekt, das Kontextinformationen für den angeforderten Dienst bietet.</span><span class="sxs-lookup"><span data-stu-id="539a4-109">When called, the type argument is the interface or base class type of the service being requested, and the key object is either null or an object providing contextual information about the requested service.</span></span>  

<span data-ttu-id="539a4-110">Sofern nicht anders angegeben, alle zurückgegebene Objekt threadsicher sein muss, da er als Singleton verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="539a4-110">Unless otherwise stated any object returned must be thread-safe since it can be used as a singleton.</span></span> <span data-ttu-id="539a4-111">In vielen Fällen, die eine Factory ist in diesem Fall ist das zurückgegebene Objekt die Factory selbst threadsicher sein muss, aber das von der Factory zurückgegebene Objekt muss nicht threadsicher sein, da eine neue Instanz von der Factory für jede Verwendung angefordert wird.</span><span class="sxs-lookup"><span data-stu-id="539a4-111">In many cases the object returned is a factory in which case the factory itself must be thread-safe but the object returned from the factory does not need to be thread-safe since a new instance is requested from the factory for each use.</span></span>

<span data-ttu-id="539a4-112">Dieser Artikel enthält keine ausführliche Anleitung zum Implementieren von "idbdependencyresolver", aber stattdessen dient als Referenz für die Diensttypen (d. h. die Schnittstellen und Basisklassen Klassentypen) für die EF "GetService" und die Semantik des Schlüsselobjekts für jedes dieser Aufrufe wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="539a4-112">This article does not contain full details on how to implement IDbDependencyResolver, but instead acts as a reference for the service types (that is, the interface and base class types) for which EF calls GetService and the semantics of the key object for each of these calls.</span></span>

## <a name="systemdataentityidatabaseinitializertcontext"></a><span data-ttu-id="539a4-113">System.Data.Entity.IDatabaseInitializer < TContext\></span><span class="sxs-lookup"><span data-stu-id="539a4-113">System.Data.Entity.IDatabaseInitializer<TContext\></span></span>  

<span data-ttu-id="539a4-114">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="539a4-114">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="539a4-115">**Zurückgegebene Objekt**: einen Datenbankinitialisierer, für den angegebenen Kontext-Typ</span><span class="sxs-lookup"><span data-stu-id="539a4-115">**Object returned**: A database initializer for the given context type</span></span>  

<span data-ttu-id="539a4-116">**Schlüssel**: nicht verwendet wird, wird null sein.</span><span class="sxs-lookup"><span data-stu-id="539a4-116">**Key**: Not used; will be null</span></span>  

## <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a><span data-ttu-id="539a4-117">Func < System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span><span class="sxs-lookup"><span data-stu-id="539a4-117">Func<System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span></span>  

<span data-ttu-id="539a4-118">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="539a4-118">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="539a4-119">**Zurückgegebene Objekt**: eine Factory, einen SQL-Generator zu erstellen, die für Migrationen und weiteren Aktionen, die dazu führen, eine Datenbank dass erstellt werden, wie z. B. die Erstellung der Datenbank mit der Datenbankinitialisierer verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="539a4-119">**Object returned**: A factory to create a SQL generator that can be used for Migrations and other actions that cause a database to be created, such as database creation with database initializers.</span></span>  

<span data-ttu-id="539a4-120">**Schlüssel**: eine Zeichenfolge mit ADO.NET invariante Name des Anbieters, die den Typ der Datenbank, die für die SQL generiert werden.</span><span class="sxs-lookup"><span data-stu-id="539a4-120">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which SQL will be generated.</span></span> <span data-ttu-id="539a4-121">Beispielsweise wird der SQL Server SQL-Generator für den Schlüssel "System.Data.SqlClient" zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="539a4-121">For example, the SQL Server SQL generator is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="539a4-122">Weitere Informationen zum Anbieter-bezogene Dienste in EF 6 finden Sie die [EF6-Anbietermodell](~/ef6/fundamentals/providers/provider-model.md) Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="539a4-122">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycorecommondbproviderservices"></a><span data-ttu-id="539a4-123">System.Data.Entity.Core.Common.DbProviderServices</span><span class="sxs-lookup"><span data-stu-id="539a4-123">System.Data.Entity.Core.Common.DbProviderServices</span></span>  

<span data-ttu-id="539a4-124">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="539a4-124">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="539a4-125">**Zurückgegebene Objekt**: des EF-Anbieter für einen angegebenen invarianten Anbieternamen verwenden</span><span class="sxs-lookup"><span data-stu-id="539a4-125">**Object returned**: The EF provider to use for a given provider invariant name</span></span>  

<span data-ttu-id="539a4-126">**Schlüssel**: eine Zeichenfolge mit ADO.NET invariante Name des Anbieters, die den Typ der Datenbank, die für den ein Anbieter erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="539a4-126">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which a provider is needed.</span></span> <span data-ttu-id="539a4-127">Beispielsweise wird der SQL Server-Anbieter für den Schlüssel "System.Data.SqlClient" zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="539a4-127">For example, the SQL Server provider is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="539a4-128">Weitere Informationen zum Anbieter-bezogene Dienste in EF 6 finden Sie die [EF6-Anbietermodell](~/ef6/fundamentals/providers/provider-model.md) Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="539a4-128">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureidbconnectionfactory"></a><span data-ttu-id="539a4-129">System.Data.Entity.Infrastructure.IDbConnectionFactory</span><span class="sxs-lookup"><span data-stu-id="539a4-129">System.Data.Entity.Infrastructure.IDbConnectionFactory</span></span>  

<span data-ttu-id="539a4-130">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="539a4-130">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="539a4-131">**Zurückgegebene Objekt**: der verbindungsfactory, das verwendet wird, wenn EF eine datenbankverbindung erstellt gemäß der Konvention.</span><span class="sxs-lookup"><span data-stu-id="539a4-131">**Object returned**: The connection factory that will be used when EF creates a database connection by convention.</span></span> <span data-ttu-id="539a4-132">D. h. wenn keine Verbindung oder eine Verbindungszeichenfolge mit EF erhält und keine Verbindungszeichenfolge finden Sie in der Datei "App.config" oder "Web.config", klicken Sie dann diesen Dienst dient zum Erstellen einer Verbindung gemäß der Konvention.</span><span class="sxs-lookup"><span data-stu-id="539a4-132">That is, when no connection or connection string is given to EF, and no connection string can be found in the app.config or web.config, then this service is used to create a connection by convention.</span></span> <span data-ttu-id="539a4-133">Ändern die verbindungsfactory können EF verwendet eine andere Art von Datenbank (z. B. SQL Server Compact Edition) in der Standardeinstellung.</span><span class="sxs-lookup"><span data-stu-id="539a4-133">Changing the connection factory can allow EF to use a different type of database (for example, SQL Server Compact Edition) by default.</span></span>  

<span data-ttu-id="539a4-134">**Schlüssel**: nicht verwendet wird, wird null sein.</span><span class="sxs-lookup"><span data-stu-id="539a4-134">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="539a4-135">Weitere Informationen zum Anbieter-bezogene Dienste in EF 6 finden Sie die [EF6-Anbietermodell](~/ef6/fundamentals/providers/provider-model.md) Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="539a4-135">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureimanifesttokenservice"></a><span data-ttu-id="539a4-136">System.Data.Entity.Infrastructure.IManifestTokenService</span><span class="sxs-lookup"><span data-stu-id="539a4-136">System.Data.Entity.Infrastructure.IManifestTokenService</span></span>  

<span data-ttu-id="539a4-137">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="539a4-137">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="539a4-138">**Zurückgegebene Objekt**: ein Dienst, der von einer Verbindung ein anbietermanifesttokens generieren kann.</span><span class="sxs-lookup"><span data-stu-id="539a4-138">**Object returned**: A service that can generate a provider manifest token from a connection.</span></span> <span data-ttu-id="539a4-139">Dieser Dienst wird in der Regel auf zwei Arten verwendet.</span><span class="sxs-lookup"><span data-stu-id="539a4-139">This service is typically used in two ways.</span></span> <span data-ttu-id="539a4-140">Zuerst kann verwendet werden, um Code First, die Verbindung zur Datenbank beim Erstellen eines Modells zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="539a4-140">First, it can be used to avoid Code First connecting to the database when building a model.</span></span> <span data-ttu-id="539a4-141">Zweitens kann sie verwendet werden zu erzwingen, dass Code First ein Modell für eine bestimmte Datenbank-Version – z. B. zu erstellen, um ein Modell für SQL Server 2005 zu erzwingen, auch wenn manchmal SQL Server 2008 verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="539a4-141">Second, it can be used to force Code First to build a model for a specific database version -- for example, to force a model for SQL Server 2005 even if sometimes SQL Server 2008 is used.</span></span>  

<span data-ttu-id="539a4-142">**Objektlebensdauer**: Singleton – in das gleiche Objekt möglicherweise verwendet mehrere Wiederherstellungszeiten und gleichzeitig von unterschiedlichen Threads</span><span class="sxs-lookup"><span data-stu-id="539a4-142">**Object lifetime**: Singleton -- the same object may be used multiple times and concurrently by different threads</span></span>  

<span data-ttu-id="539a4-143">**Schlüssel**: nicht verwendet wird, wird null sein.</span><span class="sxs-lookup"><span data-stu-id="539a4-143">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a><span data-ttu-id="539a4-144">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span><span class="sxs-lookup"><span data-stu-id="539a4-144">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span></span>  

<span data-ttu-id="539a4-145">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="539a4-145">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="539a4-146">**Zurückgegebene Objekt**: ein Dienst, der eine bestimmte Verbindung eine Anbieterfactory abrufen kann.</span><span class="sxs-lookup"><span data-stu-id="539a4-146">**Object returned**: A service that can obtain a provider factory from a given connection.</span></span> <span data-ttu-id="539a4-147">In .NET 4.5 ist der Anbieter von der Verbindung öffentlich zugänglich.</span><span class="sxs-lookup"><span data-stu-id="539a4-147">On .NET 4.5 the provider is publicly accessible from the connection.</span></span> <span data-ttu-id="539a4-148">Von .NET 4 verwendet die standardmäßige Implementierung von diesem Dienst Teil der Heuristik, um den entsprechenden Anbieter zu finden.</span><span class="sxs-lookup"><span data-stu-id="539a4-148">On .NET 4 the default implementation of this service uses some heuristics to find the matching provider.</span></span> <span data-ttu-id="539a4-149">Wenn dann, eine neue Implementierung dieses Diensts fehlschlagen können registriert werden, um eine geeignete Lösung zu bieten.</span><span class="sxs-lookup"><span data-stu-id="539a4-149">If these fail then a new implementation of this service can be registered to provide an appropriate resolution.</span></span>  

<span data-ttu-id="539a4-150">**Schlüssel**: nicht verwendet wird, wird null sein.</span><span class="sxs-lookup"><span data-stu-id="539a4-150">**Key**: Not used; will be null</span></span>  

## <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a><span data-ttu-id="539a4-151">Func < DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span><span class="sxs-lookup"><span data-stu-id="539a4-151">Func<DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span></span>  

<span data-ttu-id="539a4-152">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="539a4-152">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="539a4-153">**Zurückgegebene Objekt**: eine Factory, die einen Modell Cacheschlüssel für einen angegebenen Kontext generiert wird.</span><span class="sxs-lookup"><span data-stu-id="539a4-153">**Object returned**: A factory that will generate a model cache key for a given context.</span></span> <span data-ttu-id="539a4-154">Standardmäßig speichert EF ein Modell pro DbContext-Typ pro Anbieter.</span><span class="sxs-lookup"><span data-stu-id="539a4-154">By default, EF caches one model per DbContext type per provider.</span></span> <span data-ttu-id="539a4-155">Eine andere Implementierung dieses Diensts kann verwendet werden, andere Informationen, z. B. Schemaname, der Cacheschlüssel hinzu.</span><span class="sxs-lookup"><span data-stu-id="539a4-155">A different implementation of this service can be used to add other information, such as schema name, to the cache key.</span></span>  

<span data-ttu-id="539a4-156">**Schlüssel**: nicht verwendet wird, wird null sein.</span><span class="sxs-lookup"><span data-stu-id="539a4-156">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityspatialdbspatialservices"></a><span data-ttu-id="539a4-157">System.Data.Entity.Spatial.DbSpatialServices</span><span class="sxs-lookup"><span data-stu-id="539a4-157">System.Data.Entity.Spatial.DbSpatialServices</span></span>  

<span data-ttu-id="539a4-158">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="539a4-158">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="539a4-159">**Zurückgegebene Objekt**: räumliche ein EF-Anbieter, das Unterstützung für den grundlegenden EF-Anbieter für Geography und Geometry räumliche Typen.</span><span class="sxs-lookup"><span data-stu-id="539a4-159">**Object returned**: An EF spatial provider that adds support to the basic EF provider for geography and geometry spatial types.</span></span>  

<span data-ttu-id="539a4-160">**Schlüssel**: DbSptialServices für aufgefordert wird, gibt es zwei Möglichkeiten.</span><span class="sxs-lookup"><span data-stu-id="539a4-160">**Key**: DbSptialServices is asked for in two ways.</span></span> <span data-ttu-id="539a4-161">Erste, anbieterspezifische räumlichen Dienste mithilfe eines Objekts DbProviderInfo angefordert werden (die invariante enthält Namen und das manifesttoken) als Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="539a4-161">First, provider-specific spatial services are requested using a DbProviderInfo object (which contains invariant name and manifest token) as the key.</span></span> <span data-ttu-id="539a4-162">Zweitens können DbSpatialServices ohne Schlüssel aufgefordert.</span><span class="sxs-lookup"><span data-stu-id="539a4-162">Second, DbSpatialServices can be asked for with no key.</span></span> <span data-ttu-id="539a4-163">Dies wird zum Auflösen des "räumlichen Anbieters" die beim Erstellen von eigenständigen DbGeography oder DbGeometry-Typen verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="539a4-163">This is used to resolve the "global spatial provider" which is used when creating stand-alone DbGeography or DbGeometry types.</span></span>  

>[!NOTE]
> <span data-ttu-id="539a4-164">Weitere Informationen zum Anbieter-bezogene Dienste in EF 6 finden Sie die [EF6-Anbietermodell](~/ef6/fundamentals/providers/provider-model.md) Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="539a4-164">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a><span data-ttu-id="539a4-165">Func < System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span><span class="sxs-lookup"><span data-stu-id="539a4-165">Func<System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span></span>  

<span data-ttu-id="539a4-166">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="539a4-166">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="539a4-167">**Zurückgegebene Objekt**: eine Factory zum Erstellen eines Diensts, der ermöglicht, dass einen Anbieter Wiederholungen oder anderem Verhalten implementieren, wenn Abfragen und Befehlen für die Datenbank ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="539a4-167">**Object returned**: A factory to create a service that allows a provider to implement retries or other behavior when queries and commands are executed against the database.</span></span> <span data-ttu-id="539a4-168">Wenn keine Implementierung bereitgestellt wird, wird Klicken Sie dann EF einfach führen Sie die Befehle und weitergegeben werden alle ausgelösten Ausnahmen.</span><span class="sxs-lookup"><span data-stu-id="539a4-168">If no implementation is provided, then EF will simply execute the commands and propagate any exceptions thrown.</span></span> <span data-ttu-id="539a4-169">Für SQL Server ist dieser Dienst verwendet, um eine wiederholungsrichtlinie bereitzustellen, die was besonders hilfreich ist, bei der Ausführung für Cloud-basierten Datenbankserver, z. B. SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="539a4-169">For SQL Server this service is used to provide a retry policy which is especially useful when running against cloud-based database servers such as SQL Azure.</span></span>  

<span data-ttu-id="539a4-170">**Schlüssel**: ein ExecutionStrategyKey-Objekt, das enthält, der invariante Anbietername und optional einen Servernamen ein, die für die Ausführungsstrategie verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="539a4-170">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the execution strategy will be used.</span></span>  

>[!NOTE]
> <span data-ttu-id="539a4-171">Weitere Informationen zum Anbieter-bezogene Dienste in EF 6 finden Sie die [EF6-Anbietermodell](~/ef6/fundamentals/providers/provider-model.md) Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="539a4-171">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a><span data-ttu-id="539a4-172">Func < "DbConnection", "String", "System.Data.Entity.Migrations.History.HistoryContext\></span><span class="sxs-lookup"><span data-stu-id="539a4-172">Func<DbConnection, string, System.Data.Entity.Migrations.History.HistoryContext\></span></span>  

<span data-ttu-id="539a4-173">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="539a4-173">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="539a4-174">**Zurückgegebene Objekt**: eine Factory, die einen Anbieter so konfigurieren Sie die Zuordnung von der "historycontext" für ermöglicht die `__MigrationHistory` Tabelle, die von EF-Migrationen verwendet.</span><span class="sxs-lookup"><span data-stu-id="539a4-174">**Object returned**: A factory that allows a provider to configure the mapping of the HistoryContext to the `__MigrationHistory` table used by EF Migrations.</span></span> <span data-ttu-id="539a4-175">Die "historycontext" ist der Code für einen ersten "DbContext" und kann mithilfe der normalen fluent-API so ändern Sie z.B. den Namen der Tabelle und den Mappingspezifikationen Spalte konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="539a4-175">The HistoryContext is a Code First DbContext and can be configured using the normal fluent API to change things like the name of the table and the column mapping specifications.</span></span>  

<span data-ttu-id="539a4-176">**Schlüssel**: nicht verwendet wird, wird null sein.</span><span class="sxs-lookup"><span data-stu-id="539a4-176">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="539a4-177">Weitere Informationen zum Anbieter-bezogene Dienste in EF 6 finden Sie die [EF6-Anbietermodell](~/ef6/fundamentals/providers/provider-model.md) Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="539a4-177">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdatacommondbproviderfactory"></a><span data-ttu-id="539a4-178">System.Data.Common.DbProviderFactory</span><span class="sxs-lookup"><span data-stu-id="539a4-178">System.Data.Common.DbProviderFactory</span></span>  

<span data-ttu-id="539a4-179">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="539a4-179">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="539a4-180">**Zurückgegebene Objekt**: der ADO NET-Anbieter für einen angegebenen invarianten Anbieternamen verwenden.</span><span class="sxs-lookup"><span data-stu-id="539a4-180">**Object returned**: The ADO.NET provider to use for a given provider invariant name.</span></span>  

<span data-ttu-id="539a4-181">**Schlüssel**: eine Zeichenfolge, enthält der invariante Anbietername für ADO.NET</span><span class="sxs-lookup"><span data-stu-id="539a4-181">**Key**: A string containing the ADO.NET provider invariant name</span></span>  

>[!NOTE]
> <span data-ttu-id="539a4-182">Dieser Dienst wird in der Regel nicht direkt geändert, da die Standardimplementierung der normalen anbieterregistrierung ADO.NET verwendet.</span><span class="sxs-lookup"><span data-stu-id="539a4-182">This service is not usually changed directly since the default implementation uses the normal ADO.NET provider registration.</span></span> <span data-ttu-id="539a4-183">Weitere Informationen zum Anbieter-bezogene Dienste in EF 6 finden Sie die [EF6-Anbietermodell](~/ef6/fundamentals/providers/provider-model.md) Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="539a4-183">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureiproviderinvariantname"></a><span data-ttu-id="539a4-184">System.Data.Entity.Infrastructure.IProviderInvariantName</span><span class="sxs-lookup"><span data-stu-id="539a4-184">System.Data.Entity.Infrastructure.IProviderInvariantName</span></span>  

<span data-ttu-id="539a4-185">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="539a4-185">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="539a4-186">**Zurückgegebene Objekt**: ein Dienst, der verwendet wird, um zu bestimmen, einen invarianten Anbieternamen für einen bestimmten Typ ' DbProviderFactory '.</span><span class="sxs-lookup"><span data-stu-id="539a4-186">**Object returned**: a service that is used to determine a provider invariant name for a given type of DbProviderFactory.</span></span> <span data-ttu-id="539a4-187">Die standardmäßige Implementierung dieses Diensts wird die Registrierung des Anbieters ADO.NET verwendet.</span><span class="sxs-lookup"><span data-stu-id="539a4-187">The default implementation of this service uses the ADO.NET provider registration.</span></span> <span data-ttu-id="539a4-188">Dies bedeutet, dass wenn der ADO NET-Anbieter nicht auf die übliche Weise registriert ist, da der "DbProviderFactory" von EF aufgelöst wird, klicken Sie dann es außerdem erforderlich, um diesen Dienst zu beheben kann.</span><span class="sxs-lookup"><span data-stu-id="539a4-188">This means that if the ADO.NET provider is not registered in the normal way because DbProviderFactory is being resolved by EF, then it will also be necessary to resolve this service.</span></span>  

<span data-ttu-id="539a4-189">**Schlüssel**: der "DbProviderFactory"-Instanz, die für das invarianter Name erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="539a4-189">**Key**: The DbProviderFactory instance for which an invariant name is required.</span></span>  

>[!NOTE]
> <span data-ttu-id="539a4-190">Weitere Informationen zum Anbieter-bezogene Dienste in EF 6 finden Sie die [EF6-Anbietermodell](~/ef6/fundamentals/providers/provider-model.md) Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="539a4-190">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a><span data-ttu-id="539a4-191">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span><span class="sxs-lookup"><span data-stu-id="539a4-191">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span></span>  

<span data-ttu-id="539a4-192">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="539a4-192">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="539a4-193">**Zurückgegebene Objekt**: einen Cache von Assemblys, die vorgenerierte Sichten enthalten.</span><span class="sxs-lookup"><span data-stu-id="539a4-193">**Object returned**: a cache of the assemblies that contain pre-generated views.</span></span> <span data-ttu-id="539a4-194">Eine Ersetzung wird normalerweise verwendet, um EF darüber informiert, welche Assemblys vorgenerierte Sichten enthalten, ohne alle Discovery zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="539a4-194">A replacement is typically used to let EF know which assemblies contain pre-generated views without doing any discovery.</span></span>  

<span data-ttu-id="539a4-195">**Schlüssel**: nicht verwendet wird, wird null sein.</span><span class="sxs-lookup"><span data-stu-id="539a4-195">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a><span data-ttu-id="539a4-196">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span><span class="sxs-lookup"><span data-stu-id="539a4-196">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span></span>

<span data-ttu-id="539a4-197">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="539a4-197">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="539a4-198">**Zurückgegebene Objekt**: ein Dienst, der von EF zu pluralize singularize Namen verwendet.</span><span class="sxs-lookup"><span data-stu-id="539a4-198">**Object returned**: a service used by EF to pluralize and singularize names.</span></span> <span data-ttu-id="539a4-199">Standardmäßig wird eine englische pluralisierungsdienst verwendet.</span><span class="sxs-lookup"><span data-stu-id="539a4-199">By default an English pluralization service is used.</span></span>  

<span data-ttu-id="539a4-200">**Schlüssel**: nicht verwendet wird, wird null sein.</span><span class="sxs-lookup"><span data-stu-id="539a4-200">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a><span data-ttu-id="539a4-201">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span><span class="sxs-lookup"><span data-stu-id="539a4-201">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span></span>  

<span data-ttu-id="539a4-202">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="539a4-202">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="539a4-203">**Zurückgegebene Objekte**: alle Interceptors, die beim Starten der Anwendung registriert werden soll.</span><span class="sxs-lookup"><span data-stu-id="539a4-203">**Objects returned**: Any interceptors that should be registered when the application starts.</span></span> <span data-ttu-id="539a4-204">Beachten Sie, dass diese Objekte werden durch den Aufruf GetServices angefordert, alle Dienste, die von jedem Abhängigkeitskonfliktlöser zurückgegeben werden registriert.</span><span class="sxs-lookup"><span data-stu-id="539a4-204">Note that these objects are requested using the GetServices call and all interceptors returned by any dependency resolver will registered.</span></span>

<span data-ttu-id="539a4-205">**Schlüssel**: nicht verwendet wird, wird null sein.</span><span class="sxs-lookup"><span data-stu-id="539a4-205">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a><span data-ttu-id="539a4-206">Func < System.Data.Entity.DbContext, Aktion < Zeichenfolge\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span><span class="sxs-lookup"><span data-stu-id="539a4-206">Func<System.Data.Entity.DbContext, Action<string\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span></span>  

<span data-ttu-id="539a4-207">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="539a4-207">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="539a4-208">**Zurückgegebene Objekt**: eine Factory, die mit den Datenbank-Log-Formatierer erstellen, die verwendet wird, wenn der Kontext. "Database.log"-Eigenschaft wird für den angegebenen Kontext festgelegt.</span><span class="sxs-lookup"><span data-stu-id="539a4-208">**Object returned**: A factory that will be used to create the database log formatter that will be used when the context.Database.Log property is set on the given context.</span></span>  

<span data-ttu-id="539a4-209">**Schlüssel**: nicht verwendet wird, wird null sein.</span><span class="sxs-lookup"><span data-stu-id="539a4-209">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext"></a><span data-ttu-id="539a4-210">Func < System.Data.Entity.DbContext\></span><span class="sxs-lookup"><span data-stu-id="539a4-210">Func<System.Data.Entity.DbContext\></span></span>  

<span data-ttu-id="539a4-211">**Eingeführt in Version**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="539a4-211">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="539a4-212">**Zurückgegebene Objekt**: eine Factory, die verwendet wird, um kontextinstanzen für Migrationen zu erstellen, wenn der Kontext nicht über einen zugänglichen parameterlosen Konstruktor verfügt.</span><span class="sxs-lookup"><span data-stu-id="539a4-212">**Object returned**: A factory that will be used to create context instances for Migrations when the context does not have an accessible parameterless constructor.</span></span>  

<span data-ttu-id="539a4-213">**Schlüssel**: das Typobjekt für den Typ des abgeleiteten "DbContext" für den eine Factory erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="539a4-213">**Key**: The Type object for the type of the derived DbContext for which a factory is needed.</span></span>  

## <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a><span data-ttu-id="539a4-214">Func < System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span><span class="sxs-lookup"><span data-stu-id="539a4-214">Func<System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span></span>  

<span data-ttu-id="539a4-215">**Eingeführt in Version**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="539a4-215">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="539a4-216">**Zurückgegebene Objekt**: eine Factory, die Serialisierungsprogramme für die Serialisierung von benutzerdefinierten Anmerkungen mit fester typbindung erstellen, sie können serialisiert und in XML-Code für die Verwendung in Code First-Migrationen desterilized verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="539a4-216">**Object returned**: A factory that will be used to create serializers for serialization of strongly-typed custom annotations such that they can be serialized and desterilized into XML for use in Code First Migrations.</span></span>  

<span data-ttu-id="539a4-217">**Schlüssel**: der Name der Anmerkung, die serialisiert oder deserialisiert.</span><span class="sxs-lookup"><span data-stu-id="539a4-217">**Key**: The name of the annotation that is being serialized or deserialized.</span></span>  

## <a name="funcsystemdataentityinfrastructuretransactionhandler"></a><span data-ttu-id="539a4-218">Func < System.Data.Entity.Infrastructure.TransactionHandler\></span><span class="sxs-lookup"><span data-stu-id="539a4-218">Func<System.Data.Entity.Infrastructure.TransactionHandler\></span></span>  

<span data-ttu-id="539a4-219">**Eingeführt in Version**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="539a4-219">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="539a4-220">**Zurückgegebene Objekt**: eine Factory, die verwendet wird, um Handler für Transaktionen zu erstellen, sodass zum Beispiel Behandlung commitfehlern sinnvoll, spezielle Behandlung angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="539a4-220">**Object returned**: A factory that will be used to create handlers for transactions so that special handling can be applied for situations such as handling commit failures.</span></span>  

<span data-ttu-id="539a4-221">**Schlüssel**: ein ExecutionStrategyKey-Objekt, das enthält, der invariante Anbietername und optional einen Servernamen ein, die für die der Handler für die Transaktion verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="539a4-221">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the transaction handler will be used.</span></span>  
