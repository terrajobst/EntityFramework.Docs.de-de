---
title: Abhängigkeitsauflösung – EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
caps.latest.revision: 3
ms.openlocfilehash: 88fd859b4f9a8069eeb08f32bb1d35ddcd21aec5
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "39121481"
---
# <a name="dependency-resolution"></a><span data-ttu-id="70b9a-102">Abhängigkeitsauflösung</span><span class="sxs-lookup"><span data-stu-id="70b9a-102">Dependency resolution</span></span>
> [!NOTE]
> <span data-ttu-id="70b9a-103">**Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="70b9a-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="70b9a-104">Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.</span><span class="sxs-lookup"><span data-stu-id="70b9a-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="70b9a-105">Ab EF6 enthält Entity Framework einen allgemeinen Mechanismus zum Abrufen von Implementierungen der Dienste, die es benötigt.</span><span class="sxs-lookup"><span data-stu-id="70b9a-105">Starting with EF6, Entity Framework contains a general-purpose mechanism for obtaining implementations of services that it requires.</span></span> <span data-ttu-id="70b9a-106">D. h. wenn EF eine Instanz der einige Schnittstellen oder Basisklassen verwendet fragt es eine konkrete Implementierung der Schnittstelle oder Base-Klasse verwenden.</span><span class="sxs-lookup"><span data-stu-id="70b9a-106">That is, when EF uses an instance of some interfaces or base classes it will ask for a concrete implementation of the interface or base class to use.</span></span> <span data-ttu-id="70b9a-107">Dies erfolgt durch die Verwendung der Schnittstelle "idbdependencyresolver":</span><span class="sxs-lookup"><span data-stu-id="70b9a-107">This is achieved through use of the IDbDependencyResolver interface:</span></span>  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

<span data-ttu-id="70b9a-108">Die Methode "GetService" heißt in der Regel von EF und erfolgt durch eine Implementierung von "idbdependencyresolver" von EF oder von der Anwendung bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="70b9a-108">The GetService method is typically called by EF and is handled by an implementation of IDbDependencyResolver provided either by EF or by the application.</span></span> <span data-ttu-id="70b9a-109">Wenn aufgerufen, wird das Typargument der Klassentyp-Schnittstelle oder in der der angeforderte Dienst, und das Objekt ist Null oder ein Objekt, das Kontextinformationen für den angeforderten Dienst bietet.</span><span class="sxs-lookup"><span data-stu-id="70b9a-109">When called, the type argument is the interface or base class type of the service being requested, and the key object is either null or an object providing contextual information about the requested service.</span></span>  

<span data-ttu-id="70b9a-110">Dieser Artikel enthält keine ausführliche Anleitung zum Implementieren von "idbdependencyresolver", aber stattdessen dient als Referenz für die Diensttypen (d. h. die Schnittstellen und Basisklassen Klassentypen) für die EF "GetService" und die Semantik des Schlüsselobjekts für jedes dieser Aufrufe wird aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="70b9a-110">This article does not contain full details on how to implement IDbDependencyResolver, but instead acts as a reference for the service types (that is, the interface and base class types) for which EF calls GetService and the semantics of the key object for each of these calls.</span></span> <span data-ttu-id="70b9a-111">In diesem Dokument wird auf dem neuesten Stand gehalten, wenn weitere Dienste hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="70b9a-111">This document will be kept up-to-date as additional services are added.</span></span>  

## <a name="services-resolved"></a><span data-ttu-id="70b9a-112">Dienste, die aufgelöst</span><span class="sxs-lookup"><span data-stu-id="70b9a-112">Services resolved</span></span>  

<span data-ttu-id="70b9a-113">Sofern nicht anders angegeben, alle zurückgegebene Objekt threadsicher sein muss, da er als Singleton verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="70b9a-113">Unless otherwise stated any object returned must be thread-safe since it can be used as a singleton.</span></span> <span data-ttu-id="70b9a-114">In vielen Fällen, die eine Factory ist in diesem Fall ist das zurückgegebene Objekt die Factory selbst threadsicher sein muss, aber das von der Factory zurückgegebene Objekt muss nicht threadsicher sein, da eine neue Instanz von der Factory für jede Verwendung angefordert wird.</span><span class="sxs-lookup"><span data-stu-id="70b9a-114">In many cases the object returned is a factory in which case the factory itself must be thread-safe but the object returned from the factory does not need to be thread-safe since a new instance is requested from the factory for each use.</span></span>  

### <a name="systemdataentityidatabaseinitializertcontext"></a><span data-ttu-id="70b9a-115">System.Data.Entity.IDatabaseInitializer < TContext\></span><span class="sxs-lookup"><span data-stu-id="70b9a-115">System.Data.Entity.IDatabaseInitializer<TContext\></span></span>  

<span data-ttu-id="70b9a-116">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="70b9a-116">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="70b9a-117">**Zurückgegebene Objekt**: einen Datenbankinitialisierer, für den angegebenen Kontext-Typ</span><span class="sxs-lookup"><span data-stu-id="70b9a-117">**Object returned**: A database initializer for the given context type</span></span>  

<span data-ttu-id="70b9a-118">**Schlüssel**: nicht verwendet wird, wird null sein.</span><span class="sxs-lookup"><span data-stu-id="70b9a-118">**Key**: Not used; will be null</span></span>  

### <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a><span data-ttu-id="70b9a-119">Func < System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span><span class="sxs-lookup"><span data-stu-id="70b9a-119">Func<System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span></span>  

<span data-ttu-id="70b9a-120">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="70b9a-120">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="70b9a-121">**Zurückgegebene Objekt**: eine Factory, einen SQL-Generator zu erstellen, die für Migrationen und weiteren Aktionen, die dazu führen, eine Datenbank dass erstellt werden, wie z. B. die Erstellung der Datenbank mit der Datenbankinitialisierer verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="70b9a-121">**Object returned**: A factory to create a SQL generator that can be used for Migrations and other actions that cause a database to be created, such as database creation with database initializers.</span></span>  

<span data-ttu-id="70b9a-122">**Schlüssel**: eine Zeichenfolge mit ADO.NET invariante Name des Anbieters, die den Typ der Datenbank, die für die SQL generiert werden.</span><span class="sxs-lookup"><span data-stu-id="70b9a-122">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which SQL will be generated.</span></span> <span data-ttu-id="70b9a-123">Beispielsweise wird der SQL Server SQL-Generator für den Schlüssel "System.Data.SqlClient" zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="70b9a-123">For example, the SQL Server SQL generator is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="70b9a-124">Weitere Informationen zum Anbieter-bezogene Dienste in EF 6 finden Sie die [EF6-Anbietermodell](~/ef6/fundamentals/providers/provider-model.md) Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="70b9a-124">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentitycorecommondbproviderservices"></a><span data-ttu-id="70b9a-125">System.Data.Entity.Core.Common.DbProviderServices</span><span class="sxs-lookup"><span data-stu-id="70b9a-125">System.Data.Entity.Core.Common.DbProviderServices</span></span>  

<span data-ttu-id="70b9a-126">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="70b9a-126">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="70b9a-127">**Zurückgegebene Objekt**: des EF-Anbieter für einen angegebenen invarianten Anbieternamen verwenden</span><span class="sxs-lookup"><span data-stu-id="70b9a-127">**Object returned**: The EF provider to use for a given provider invariant name</span></span>  

<span data-ttu-id="70b9a-128">**Schlüssel**: eine Zeichenfolge mit ADO.NET invariante Name des Anbieters, die den Typ der Datenbank, die für den ein Anbieter erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="70b9a-128">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which a provider is needed.</span></span> <span data-ttu-id="70b9a-129">Beispielsweise wird der SQL Server-Anbieter für den Schlüssel "System.Data.SqlClient" zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="70b9a-129">For example, the SQL Server provider is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="70b9a-130">Weitere Informationen zum Anbieter-bezogene Dienste in EF 6 finden Sie die [EF6-Anbietermodell](~/ef6/fundamentals/providers/provider-model.md) Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="70b9a-130">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureidbconnectionfactory"></a><span data-ttu-id="70b9a-131">System.Data.Entity.Infrastructure.IDbConnectionFactory</span><span class="sxs-lookup"><span data-stu-id="70b9a-131">System.Data.Entity.Infrastructure.IDbConnectionFactory</span></span>  

<span data-ttu-id="70b9a-132">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="70b9a-132">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="70b9a-133">**Zurückgegebene Objekt**: der verbindungsfactory, das verwendet wird, wenn EF eine datenbankverbindung erstellt gemäß der Konvention.</span><span class="sxs-lookup"><span data-stu-id="70b9a-133">**Object returned**: The connection factory that will be used when EF creates a database connection by convention.</span></span> <span data-ttu-id="70b9a-134">D. h. wenn keine Verbindung oder eine Verbindungszeichenfolge mit EF erhält und keine Verbindungszeichenfolge finden Sie in der Datei "App.config" oder "Web.config", klicken Sie dann diesen Dienst dient zum Erstellen einer Verbindung gemäß der Konvention.</span><span class="sxs-lookup"><span data-stu-id="70b9a-134">That is, when no connection or connection string is given to EF, and no connection string can be found in the app.config or web.config, then this service is used to create a connection by convention.</span></span> <span data-ttu-id="70b9a-135">Ändern die verbindungsfactory können EF verwendet eine andere Art von Datenbank (z. B. SQL Server Compact Edition) in der Standardeinstellung.</span><span class="sxs-lookup"><span data-stu-id="70b9a-135">Changing the connection factory can allow EF to use a different type of database (for example, SQL Server Compact Edition) by default.</span></span>  

<span data-ttu-id="70b9a-136">**Schlüssel**: nicht verwendet wird, wird null sein.</span><span class="sxs-lookup"><span data-stu-id="70b9a-136">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="70b9a-137">Weitere Informationen zum Anbieter-bezogene Dienste in EF 6 finden Sie die [EF6-Anbietermodell](~/ef6/fundamentals/providers/provider-model.md) Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="70b9a-137">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureimanifesttokenservice"></a><span data-ttu-id="70b9a-138">System.Data.Entity.Infrastructure.IManifestTokenService</span><span class="sxs-lookup"><span data-stu-id="70b9a-138">System.Data.Entity.Infrastructure.IManifestTokenService</span></span>  

<span data-ttu-id="70b9a-139">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="70b9a-139">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="70b9a-140">**Zurückgegebene Objekt**: ein Dienst, der von einer Verbindung ein anbietermanifesttokens generieren kann.</span><span class="sxs-lookup"><span data-stu-id="70b9a-140">**Object returned**: A service that can generate a provider manifest token from a connection.</span></span> <span data-ttu-id="70b9a-141">Dieser Dienst wird in der Regel auf zwei Arten verwendet.</span><span class="sxs-lookup"><span data-stu-id="70b9a-141">This service is typically used in two ways.</span></span> <span data-ttu-id="70b9a-142">Zuerst kann verwendet werden, um Code First, die Verbindung zur Datenbank beim Erstellen eines Modells zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="70b9a-142">First, it can be used to avoid Code First connecting to the database when building a model.</span></span> <span data-ttu-id="70b9a-143">Zweitens kann sie verwendet werden zu erzwingen, dass Code First ein Modell für eine bestimmte Datenbank-Version – z. B. zu erstellen, um ein Modell für SQL Server 2005 zu erzwingen, auch wenn manchmal SQL Server 2008 verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="70b9a-143">Second, it can be used to force Code First to build a model for a specific database version -- for example, to force a model for SQL Server 2005 even if sometimes SQL Server 2008 is used.</span></span>  

<span data-ttu-id="70b9a-144">**Objektlebensdauer**: Singleton – in das gleiche Objekt möglicherweise verwendet mehrere Wiederherstellungszeiten und gleichzeitig von unterschiedlichen Threads</span><span class="sxs-lookup"><span data-stu-id="70b9a-144">**Object lifetime**: Singleton -- the same object may be used multiple times and concurrently by different threads</span></span>  

<span data-ttu-id="70b9a-145">**Schlüssel**: nicht verwendet wird, wird null sein.</span><span class="sxs-lookup"><span data-stu-id="70b9a-145">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a><span data-ttu-id="70b9a-146">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span><span class="sxs-lookup"><span data-stu-id="70b9a-146">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span></span>  

<span data-ttu-id="70b9a-147">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="70b9a-147">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="70b9a-148">**Zurückgegebene Objekt**: ein Dienst, der eine bestimmte Verbindung eine Anbieterfactory abrufen kann.</span><span class="sxs-lookup"><span data-stu-id="70b9a-148">**Object returned**: A service that can obtain a provider factory from a given connection.</span></span> <span data-ttu-id="70b9a-149">In .NET 4.5 ist der Anbieter von der Verbindung öffentlich zugänglich.</span><span class="sxs-lookup"><span data-stu-id="70b9a-149">On .NET 4.5 the provider is publicly accessible from the connection.</span></span> <span data-ttu-id="70b9a-150">Von .NET 4 verwendet die standardmäßige Implementierung von diesem Dienst Teil der Heuristik, um den entsprechenden Anbieter zu finden.</span><span class="sxs-lookup"><span data-stu-id="70b9a-150">On .NET 4 the default implementation of this service uses some heuristics to find the matching provider.</span></span> <span data-ttu-id="70b9a-151">Wenn dann, eine neue Implementierung dieses Diensts fehlschlagen können registriert werden, um eine geeignete Lösung zu bieten.</span><span class="sxs-lookup"><span data-stu-id="70b9a-151">If these fail then a new implementation of this service can be registered to provide an appropriate resolution.</span></span>  

<span data-ttu-id="70b9a-152">**Schlüssel**: nicht verwendet wird, wird null sein.</span><span class="sxs-lookup"><span data-stu-id="70b9a-152">**Key**: Not used; will be null</span></span>  

### <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a><span data-ttu-id="70b9a-153">Func < DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span><span class="sxs-lookup"><span data-stu-id="70b9a-153">Func<DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span></span>  

<span data-ttu-id="70b9a-154">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="70b9a-154">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="70b9a-155">**Zurückgegebene Objekt**: eine Factory, die einen Modell Cacheschlüssel für einen angegebenen Kontext generiert wird.</span><span class="sxs-lookup"><span data-stu-id="70b9a-155">**Object returned**: A factory that will generate a model cache key for a given context.</span></span> <span data-ttu-id="70b9a-156">Standardmäßig speichert EF ein Modell pro DbContext-Typ pro Anbieter.</span><span class="sxs-lookup"><span data-stu-id="70b9a-156">By default, EF caches one model per DbContext type per provider.</span></span> <span data-ttu-id="70b9a-157">Eine andere Implementierung dieses Diensts kann verwendet werden, andere Informationen, z. B. Schemaname, der Cacheschlüssel hinzu.</span><span class="sxs-lookup"><span data-stu-id="70b9a-157">A different implementation of this service can be used to add other information, such as schema name, to the cache key.</span></span>  

<span data-ttu-id="70b9a-158">**Schlüssel**: nicht verwendet wird, wird null sein.</span><span class="sxs-lookup"><span data-stu-id="70b9a-158">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityspatialdbspatialservices"></a><span data-ttu-id="70b9a-159">System.Data.Entity.Spatial.DbSpatialServices</span><span class="sxs-lookup"><span data-stu-id="70b9a-159">System.Data.Entity.Spatial.DbSpatialServices</span></span>  

<span data-ttu-id="70b9a-160">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="70b9a-160">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="70b9a-161">**Zurückgegebene Objekt**: räumliche ein EF-Anbieter, das Unterstützung für den grundlegenden EF-Anbieter für Geography und Geometry räumliche Typen.</span><span class="sxs-lookup"><span data-stu-id="70b9a-161">**Object returned**: An EF spatial provider that adds support to the basic EF provider for geography and geometry spatial types.</span></span>  

<span data-ttu-id="70b9a-162">**Schlüssel**: DbSptialServices für aufgefordert wird, gibt es zwei Möglichkeiten.</span><span class="sxs-lookup"><span data-stu-id="70b9a-162">**Key**: DbSptialServices is asked for in two ways.</span></span> <span data-ttu-id="70b9a-163">Erste, anbieterspezifische räumlichen Dienste mithilfe eines Objekts DbProviderInfo angefordert werden (die invariante enthält Namen und das manifesttoken) als Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="70b9a-163">First, provider-specific spatial services are requested using a DbProviderInfo object (which contains invariant name and manifest token) as the key.</span></span> <span data-ttu-id="70b9a-164">Zweitens können DbSpatialServices ohne Schlüssel aufgefordert.</span><span class="sxs-lookup"><span data-stu-id="70b9a-164">Second, DbSpatialServices can be asked for with no key.</span></span> <span data-ttu-id="70b9a-165">Dies wird zum Auflösen des "räumlichen Anbieters" die beim Erstellen von eigenständigen DbGeography oder DbGeometry-Typen verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="70b9a-165">This is used to resolve the "global spatial provider" which is used when creating stand-alone DbGeography or DbGeometry types.</span></span>  

>[!NOTE]
> <span data-ttu-id="70b9a-166">Weitere Informationen zum Anbieter-bezogene Dienste in EF 6 finden Sie die [EF6-Anbietermodell](~/ef6/fundamentals/providers/provider-model.md) Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="70b9a-166">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a><span data-ttu-id="70b9a-167">Func < System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span><span class="sxs-lookup"><span data-stu-id="70b9a-167">Func<System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span></span>  

<span data-ttu-id="70b9a-168">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="70b9a-168">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="70b9a-169">**Zurückgegebene Objekt**: eine Factory zum Erstellen eines Diensts, der ermöglicht, dass einen Anbieter Wiederholungen oder anderem Verhalten implementieren, wenn Abfragen und Befehlen für die Datenbank ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="70b9a-169">**Object returned**: A factory to create a service that allows a provider to implement retries or other behavior when queries and commands are executed against the database.</span></span> <span data-ttu-id="70b9a-170">Wenn keine Implementierung bereitgestellt wird, wird Klicken Sie dann EF einfach führen Sie die Befehle und weitergegeben werden alle ausgelösten Ausnahmen.</span><span class="sxs-lookup"><span data-stu-id="70b9a-170">If no implementation is provided, then EF will simply execute the commands and propagate any exceptions thrown.</span></span> <span data-ttu-id="70b9a-171">Für SQL Server ist dieser Dienst verwendet, um eine wiederholungsrichtlinie bereitzustellen, die was besonders hilfreich ist, bei der Ausführung für Cloud-basierten Datenbankserver, z. B. SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="70b9a-171">For SQL Server this service is used to provide a retry policy which is especially useful when running against cloud-based database servers such as SQL Azure.</span></span>  

<span data-ttu-id="70b9a-172">**Schlüssel**: ein ExecutionStrategyKey-Objekt, das enthält, der invariante Anbietername und optional einen Servernamen ein, die für die Ausführungsstrategie verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="70b9a-172">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the execution strategy will be used.</span></span>  

>[!NOTE]
> <span data-ttu-id="70b9a-173">Weitere Informationen zum Anbieter-bezogene Dienste in EF 6 finden Sie die [EF6-Anbietermodell](~/ef6/fundamentals/providers/provider-model.md) Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="70b9a-173">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a><span data-ttu-id="70b9a-174">Func < "DbConnection", "String", "System.Data.Entity.Migrations.History.HistoryContext\></span><span class="sxs-lookup"><span data-stu-id="70b9a-174">Func<DbConnection, string, System.Data.Entity.Migrations.History.HistoryContext\></span></span>  

<span data-ttu-id="70b9a-175">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="70b9a-175">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="70b9a-176">**Zurückgegebene Objekt**: eine Factory, die einen Anbieter so konfigurieren Sie die Zuordnung von der "historycontext" für ermöglicht die `__MigrationHistory` Tabelle, die von EF-Migrationen verwendet.</span><span class="sxs-lookup"><span data-stu-id="70b9a-176">**Object returned**: A factory that allows a provider to configure the mapping of the HistoryContext to the `__MigrationHistory` table used by EF Migrations.</span></span> <span data-ttu-id="70b9a-177">Die "historycontext" ist der Code für einen ersten "DbContext" und kann mithilfe der normalen fluent-API so ändern Sie z.B. den Namen der Tabelle und den Mappingspezifikationen Spalte konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="70b9a-177">The HistoryContext is a Code First DbContext and can be configured using the normal fluent API to change things like the name of the table and the column mapping specifications.</span></span>  

<span data-ttu-id="70b9a-178">**Schlüssel**: nicht verwendet wird, wird null sein.</span><span class="sxs-lookup"><span data-stu-id="70b9a-178">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="70b9a-179">Weitere Informationen zum Anbieter-bezogene Dienste in EF 6 finden Sie die [EF6-Anbietermodell](~/ef6/fundamentals/providers/provider-model.md) Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="70b9a-179">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdatacommondbproviderfactory"></a><span data-ttu-id="70b9a-180">System.Data.Common.DbProviderFactory</span><span class="sxs-lookup"><span data-stu-id="70b9a-180">System.Data.Common.DbProviderFactory</span></span>  

<span data-ttu-id="70b9a-181">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="70b9a-181">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="70b9a-182">**Zurückgegebene Objekt**: der ADO NET-Anbieter für einen angegebenen invarianten Anbieternamen verwenden.</span><span class="sxs-lookup"><span data-stu-id="70b9a-182">**Object returned**: The ADO.NET provider to use for a given provider invariant name.</span></span>  

<span data-ttu-id="70b9a-183">**Schlüssel**: eine Zeichenfolge, enthält der invariante Anbietername für ADO.NET</span><span class="sxs-lookup"><span data-stu-id="70b9a-183">**Key**: A string containing the ADO.NET provider invariant name</span></span>  

>[!NOTE]
> <span data-ttu-id="70b9a-184">Dieser Dienst wird in der Regel nicht direkt geändert, da die Standardimplementierung der normalen anbieterregistrierung ADO.NET verwendet.</span><span class="sxs-lookup"><span data-stu-id="70b9a-184">This service is not usually changed directly since the default implementation uses the normal ADO.NET provider registration.</span></span> <span data-ttu-id="70b9a-185">Weitere Informationen zum Anbieter-bezogene Dienste in EF 6 finden Sie die [EF6-Anbietermodell](~/ef6/fundamentals/providers/provider-model.md) Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="70b9a-185">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureiproviderinvariantname"></a><span data-ttu-id="70b9a-186">System.Data.Entity.Infrastructure.IProviderInvariantName</span><span class="sxs-lookup"><span data-stu-id="70b9a-186">System.Data.Entity.Infrastructure.IProviderInvariantName</span></span>  

<span data-ttu-id="70b9a-187">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="70b9a-187">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="70b9a-188">**Zurückgegebene Objekt**: ein Dienst, der verwendet wird, um zu bestimmen, einen invarianten Anbieternamen für einen bestimmten Typ ' DbProviderFactory '.</span><span class="sxs-lookup"><span data-stu-id="70b9a-188">**Object returned**: a service that is used to determine a provider invariant name for a given type of DbProviderFactory.</span></span> <span data-ttu-id="70b9a-189">Die standardmäßige Implementierung dieses Diensts wird die Registrierung des Anbieters ADO.NET verwendet.</span><span class="sxs-lookup"><span data-stu-id="70b9a-189">The default implementation of this service uses the ADO.NET provider registration.</span></span> <span data-ttu-id="70b9a-190">Dies bedeutet, dass wenn der ADO NET-Anbieter nicht auf die übliche Weise registriert ist, da der "DbProviderFactory" von EF aufgelöst wird, klicken Sie dann es außerdem erforderlich, um diesen Dienst zu beheben kann.</span><span class="sxs-lookup"><span data-stu-id="70b9a-190">This means that if the ADO.NET provider is not registered in the normal way because DbProviderFactory is being resolved by EF, then it will also be necessary to resolve this service.</span></span>  

<span data-ttu-id="70b9a-191">**Schlüssel**: der "DbProviderFactory"-Instanz, die für das invarianter Name erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="70b9a-191">**Key**: The DbProviderFactory instance for which an invariant name is required.</span></span>  

>[!NOTE]
> <span data-ttu-id="70b9a-192">Weitere Informationen zum Anbieter-bezogene Dienste in EF 6 finden Sie die [EF6-Anbietermodell](~/ef6/fundamentals/providers/provider-model.md) Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="70b9a-192">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a><span data-ttu-id="70b9a-193">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span><span class="sxs-lookup"><span data-stu-id="70b9a-193">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span></span>  

<span data-ttu-id="70b9a-194">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="70b9a-194">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="70b9a-195">**Zurückgegebene Objekt**: einen Cache von Assemblys, die vorgenerierte Sichten enthalten.</span><span class="sxs-lookup"><span data-stu-id="70b9a-195">**Object returned**: a cache of the assemblies that contain pre-generated views.</span></span> <span data-ttu-id="70b9a-196">Eine Ersetzung wird normalerweise verwendet, um EF darüber informiert, welche Assemblys vorgenerierte Sichten enthalten, ohne alle Discovery zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="70b9a-196">A replacement is typically used to let EF know which assemblies contain pre-generated views without doing any discovery.</span></span>  

<span data-ttu-id="70b9a-197">**Schlüssel**: nicht verwendet wird, wird null sein.</span><span class="sxs-lookup"><span data-stu-id="70b9a-197">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a><span data-ttu-id="70b9a-198">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span><span class="sxs-lookup"><span data-stu-id="70b9a-198">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span></span>

<span data-ttu-id="70b9a-199">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="70b9a-199">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="70b9a-200">**Zurückgegebene Objekt**: ein Dienst, der von EF zu pluralize singularize Namen verwendet.</span><span class="sxs-lookup"><span data-stu-id="70b9a-200">**Object returned**: a service used by EF to pluralize and singularize names.</span></span> <span data-ttu-id="70b9a-201">Standardmäßig wird eine englische pluralisierungsdienst verwendet.</span><span class="sxs-lookup"><span data-stu-id="70b9a-201">By default an English pluralization service is used.</span></span>  

<span data-ttu-id="70b9a-202">**Schlüssel**: nicht verwendet wird, wird null sein.</span><span class="sxs-lookup"><span data-stu-id="70b9a-202">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a><span data-ttu-id="70b9a-203">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span><span class="sxs-lookup"><span data-stu-id="70b9a-203">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span></span>  

<span data-ttu-id="70b9a-204">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="70b9a-204">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="70b9a-205">**Zurückgegebene Objekte**: alle Interceptors, die beim Starten der Anwendung registriert werden soll.</span><span class="sxs-lookup"><span data-stu-id="70b9a-205">**Objects returned**: Any interceptors that should be registered when the application starts.</span></span> <span data-ttu-id="70b9a-206">Beachten Sie, dass diese Objekte werden durch den Aufruf GetServices angefordert, alle Dienste, die von jedem Abhängigkeitskonfliktlöser zurückgegeben werden registriert.</span><span class="sxs-lookup"><span data-stu-id="70b9a-206">Note that these objects are requested using the GetServices call and all interceptors returned by any dependency resolver will registered.</span></span>

<span data-ttu-id="70b9a-207">**Schlüssel**: nicht verwendet wird, wird null sein.</span><span class="sxs-lookup"><span data-stu-id="70b9a-207">**Key**: Not used; will be null.</span></span>  

### <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a><span data-ttu-id="70b9a-208">Func < System.Data.Entity.DbContext, Aktion < Zeichenfolge\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span><span class="sxs-lookup"><span data-stu-id="70b9a-208">Func<System.Data.Entity.DbContext, Action<string\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span></span>  

<span data-ttu-id="70b9a-209">**Eingeführt in Version**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="70b9a-209">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="70b9a-210">**Zurückgegebene Objekt**: eine Factory, die mit den Datenbank-Log-Formatierer erstellen, die verwendet wird, wenn der Kontext. "Database.log"-Eigenschaft wird für den angegebenen Kontext festgelegt.</span><span class="sxs-lookup"><span data-stu-id="70b9a-210">**Object returned**: A factory that will be used to create the database log formatter that will be used when the context.Database.Log property is set on the given context.</span></span>  

<span data-ttu-id="70b9a-211">**Schlüssel**: nicht verwendet wird, wird null sein.</span><span class="sxs-lookup"><span data-stu-id="70b9a-211">**Key**: Not used; will be null.</span></span>  

### <a name="funcsystemdataentitydbcontext"></a><span data-ttu-id="70b9a-212">Func < System.Data.Entity.DbContext\></span><span class="sxs-lookup"><span data-stu-id="70b9a-212">Func<System.Data.Entity.DbContext\></span></span>  

<span data-ttu-id="70b9a-213">**Eingeführt in Version**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="70b9a-213">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="70b9a-214">**Zurückgegebene Objekt**: eine Factory, die verwendet wird, um kontextinstanzen für Migrationen zu erstellen, wenn der Kontext nicht über einen zugänglichen parameterlosen Konstruktor verfügt.</span><span class="sxs-lookup"><span data-stu-id="70b9a-214">**Object returned**: A factory that will be used to create context instances for Migrations when the context does not have an accessible parameterless constructor.</span></span>  

<span data-ttu-id="70b9a-215">**Schlüssel**: das Typobjekt für den Typ des abgeleiteten "DbContext" für den eine Factory erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="70b9a-215">**Key**: The Type object for the type of the derived DbContext for which a factory is needed.</span></span>  

### <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a><span data-ttu-id="70b9a-216">Func < System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span><span class="sxs-lookup"><span data-stu-id="70b9a-216">Func<System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span></span>  

<span data-ttu-id="70b9a-217">**Eingeführt in Version**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="70b9a-217">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="70b9a-218">**Zurückgegebene Objekt**: eine Factory, die Serialisierungsprogramme für die Serialisierung von benutzerdefinierten Anmerkungen mit fester typbindung erstellen, sie können serialisiert und in XML-Code für die Verwendung in Code First-Migrationen desterilized verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="70b9a-218">**Object returned**: A factory that will be used to create serializers for serialization of strongly-typed custom annotations such that they can be serialized and desterilized into XML for use in Code First Migrations.</span></span>  

<span data-ttu-id="70b9a-219">**Schlüssel**: der Name der Anmerkung, die serialisiert oder deserialisiert.</span><span class="sxs-lookup"><span data-stu-id="70b9a-219">**Key**: The name of the annotation that is being serialized or deserialized.</span></span>  

### <a name="funcsystemdataentityinfrastructuretransactionhandler"></a><span data-ttu-id="70b9a-220">Func < System.Data.Entity.Infrastructure.TransactionHandler\></span><span class="sxs-lookup"><span data-stu-id="70b9a-220">Func<System.Data.Entity.Infrastructure.TransactionHandler\></span></span>  

<span data-ttu-id="70b9a-221">**Eingeführt in Version**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="70b9a-221">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="70b9a-222">**Zurückgegebene Objekt**: eine Factory, die verwendet wird, um Handler für Transaktionen zu erstellen, sodass zum Beispiel Behandlung commitfehlern sinnvoll, spezielle Behandlung angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="70b9a-222">**Object returned**: A factory that will be used to create handlers for transactions so that special handling can be applied for situations such as handling commit failures.</span></span>  

<span data-ttu-id="70b9a-223">**Schlüssel**: ein ExecutionStrategyKey-Objekt, das enthält, der invariante Anbietername und optional einen Servernamen ein, die für die der Handler für die Transaktion verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="70b9a-223">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the transaction handler will be used.</span></span>  
