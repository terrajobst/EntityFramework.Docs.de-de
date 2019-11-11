---
title: Neuigkeiten in EF Core 1.0 – EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: 20A25111-AEBE-4BC2-83A5-3F651952DF72
uid: core/what-is-new/ef-core-1.0
ms.openlocfilehash: 2cd2a54d75ed3f0caa8b674dfb56babcfcc13592
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655857"
---
# <a name="features-included-in-ef-core-10"></a><span data-ttu-id="90873-102">Features in EF Core 1.0</span><span class="sxs-lookup"><span data-stu-id="90873-102">Features included in EF Core 1.0</span></span>

## <a name="platforms"></a><span data-ttu-id="90873-103">Plattformen</span><span class="sxs-lookup"><span data-stu-id="90873-103">Platforms</span></span>

### <a name="net-framework-451"></a><span data-ttu-id="90873-104">.NET Framework 4.5.1</span><span class="sxs-lookup"><span data-stu-id="90873-104">.NET Framework 4.5.1</span></span>

<span data-ttu-id="90873-105">Dies schließt die Konsole, WPF, WinForms, ASP.NET 4 usw. ein.</span><span class="sxs-lookup"><span data-stu-id="90873-105">Includes Console, WPF, WinForms, ASP.NET 4, etc.</span></span>

### <a name="net-standard-13"></a><span data-ttu-id="90873-106">.NET Standard 1.3</span><span class="sxs-lookup"><span data-stu-id="90873-106">.NET Standard 1.3</span></span>

<span data-ttu-id="90873-107">Einschließlich ASP.NET Core für .NET Framework und .NET Core unter Windows, OSX und Linux.</span><span class="sxs-lookup"><span data-stu-id="90873-107">Including ASP.NET Core targeting both .NET Framework and .NET Core on Windows, OSX, and Linux.</span></span>

## <a name="modelling"></a><span data-ttu-id="90873-108">Modellierung</span><span class="sxs-lookup"><span data-stu-id="90873-108">Modelling</span></span>

### <a name="basic-modelling"></a><span data-ttu-id="90873-109">Grundlegende Modellierung</span><span class="sxs-lookup"><span data-stu-id="90873-109">Basic modelling</span></span>

<span data-ttu-id="90873-110">Basierend auf POCO-Entitäten mit get/set-Eigenschaften gängiger skalarer Typen (`int`, `string` usw.).</span><span class="sxs-lookup"><span data-stu-id="90873-110">Based on POCO entities with get/set properties of common scalar types (`int`, `string`, etc.).</span></span>

### <a name="relationships-and-navigation-properties"></a><span data-ttu-id="90873-111">Beziehungen und Navigationseigenschaften</span><span class="sxs-lookup"><span data-stu-id="90873-111">Relationships and navigation properties</span></span>

<span data-ttu-id="90873-112">Beziehungen vom Typ „1:n“ und „1:0 oder 1“ können im Modell basierend auf einem Fremdschlüssel angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="90873-112">One-to-many and One-to-zero-or-one relationships can be specified in the model based on a foreign key.</span></span> <span data-ttu-id="90873-113">Navigationseigenschaften einfacher Sammlungs- oder Verweistypen können diesen Beziehungen zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="90873-113">Navigation properties of simple collection or reference types can be associated with these relationships.</span></span>

### <a name="built-in-conventions"></a><span data-ttu-id="90873-114">Integrierte Konventionen</span><span class="sxs-lookup"><span data-stu-id="90873-114">Built-in conventions</span></span>

<span data-ttu-id="90873-115">Diese bilden ein erstes Modell, das auf der Form der Entitätsklassen basiert.</span><span class="sxs-lookup"><span data-stu-id="90873-115">These build an initial model based on the shape of the entity classes.</span></span>

### <a name="fluent-api"></a><span data-ttu-id="90873-116">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="90873-116">Fluent API</span></span>

<span data-ttu-id="90873-117">Ermöglicht es Ihnen, die `OnModelCreating`-Methode für Ihren Kontext zu überschreiben, um das per Konvention erkannte Modell weiter zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="90873-117">Allows you to override the `OnModelCreating` method on your context to further configure the model that was discovered by convention.</span></span>

### <a name="data-annotations"></a><span data-ttu-id="90873-118">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="90873-118">Data annotations</span></span>

<span data-ttu-id="90873-119">Dies sind Attribute, die zu Ihren Entitätsklassen/Eigenschaften hinzugefügt werden können und das EF-Modell beeinflussen.</span><span class="sxs-lookup"><span data-stu-id="90873-119">Are attributes that can be added to your entity classes/properties and will influence the EF model.</span></span> <span data-ttu-id="90873-120">Wenn Sie z.B. `[Required]` hinzufügen, informiert dies EF darüber, dass eine Eigenschaft erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="90873-120">For example, adding `[Required]` will let EF know that a property is required.</span></span>

### <a name="relational-table-mapping"></a><span data-ttu-id="90873-121">Zuordnung in relationalen Tabellen</span><span class="sxs-lookup"><span data-stu-id="90873-121">Relational Table mapping</span></span>

<span data-ttu-id="90873-122">Ermöglicht das Zuordnen von Entitäten zu Tabellen/Spalten.</span><span class="sxs-lookup"><span data-stu-id="90873-122">Allows entities to be mapped to tables/columns.</span></span>

### <a name="key-value-generation"></a><span data-ttu-id="90873-123">Generieren von Schlüsselwerten</span><span class="sxs-lookup"><span data-stu-id="90873-123">Key value generation</span></span>

<span data-ttu-id="90873-124">Dies schließt das clientseitige Generieren und die Datenbankgenerierung ein.</span><span class="sxs-lookup"><span data-stu-id="90873-124">Including client-side generation and database generation.</span></span>

### <a name="database-generated-values"></a><span data-ttu-id="90873-125">Von der Datenbank generierte Werte</span><span class="sxs-lookup"><span data-stu-id="90873-125">Database generated values</span></span>

<span data-ttu-id="90873-126">Ermöglicht es der Datenbank, beim Einfügen (Standardwerte) oder beim Aktualisieren (berechnete Spalten) Werte zu generieren.</span><span class="sxs-lookup"><span data-stu-id="90873-126">Allows for values to be generated by the database on insert (default values) or update (computed columns).</span></span>

### <a name="sequences-in-sql-server"></a><span data-ttu-id="90873-127">Sequenzen in SQL Server</span><span class="sxs-lookup"><span data-stu-id="90873-127">Sequences in SQL Server</span></span>

<span data-ttu-id="90873-128">Ermöglicht das Definieren von Sequenzobjekten im Modell.</span><span class="sxs-lookup"><span data-stu-id="90873-128">Allows for sequence objects to be defined in the model.</span></span>

### <a name="unique-constraints"></a><span data-ttu-id="90873-129">UNIQUE-Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="90873-129">Unique constraints</span></span>

<span data-ttu-id="90873-130">Ermöglicht das Definieren alternativer Schlüssel sowie die Definition von Beziehungen, die diesen Schlüssel als Ziel verwenden.</span><span class="sxs-lookup"><span data-stu-id="90873-130">Allows for the definition of alternate keys and the ability to define relationships that target that key.</span></span>

### <a name="indexes"></a><span data-ttu-id="90873-131">Indizes</span><span class="sxs-lookup"><span data-stu-id="90873-131">Indexes</span></span>

<span data-ttu-id="90873-132">Durch das Definieren von Indizes im Modell werden automatisch Indizes in der Datenbank eingeführt.</span><span class="sxs-lookup"><span data-stu-id="90873-132">Defining indexes in the model automatically introduces indexes in the database.</span></span> <span data-ttu-id="90873-133">UNIQUE-Indizes werden ebenfalls unterstützt.</span><span class="sxs-lookup"><span data-stu-id="90873-133">Unique indexes are also supported.</span></span>

### <a name="shadow-state-properties"></a><span data-ttu-id="90873-134">Eigenschaften im Schattenzustand</span><span class="sxs-lookup"><span data-stu-id="90873-134">Shadow state properties</span></span>

<span data-ttu-id="90873-135">Ermöglicht das Definieren von Eigenschaften im Modell, die nicht deklariert und nicht in der .NET-Klasse gespeichert sind, aber durch EF Core nachverfolgt und aktualisiert werden können.</span><span class="sxs-lookup"><span data-stu-id="90873-135">Allows for properties to be defined in the model that are not declared and are not stored in the .NET class but can be tracked and updated by EF Core.</span></span> <span data-ttu-id="90873-136">Diese Eigenschaften werden häufig für Fremdschlüsseleigenschaften verwendet, wenn das Offenlegen dieser Eigenschaften im Objekt nicht gewünscht wird.</span><span class="sxs-lookup"><span data-stu-id="90873-136">Commonly used for foreign key properties when exposing these in the object is not desired.</span></span>

### <a name="table-per-hierarchy-inheritance-pattern"></a><span data-ttu-id="90873-137">TPH-Vererbungsmuster (Tabelle pro Hierarchie)</span><span class="sxs-lookup"><span data-stu-id="90873-137">Table-Per-Hierarchy inheritance pattern</span></span>

<span data-ttu-id="90873-138">Ermöglicht es, Entitäten in einer Vererbungshierarchie in einer einzigen Tabelle zu speichern. Hierbei wird eine Diskriminatorspalte verwendet, um den Entitätstyp für einen bestimmten Datensatz in der Datenbank zu identifizieren.</span><span class="sxs-lookup"><span data-stu-id="90873-138">Allows entities in an inheritance hierarchy to be saved to a single table using a discriminator column to identify they entity type for a given record in the database.</span></span>

### <a name="model-validation"></a><span data-ttu-id="90873-139">Modellvalidierung</span><span class="sxs-lookup"><span data-stu-id="90873-139">Model validation</span></span>

<span data-ttu-id="90873-140">Erkennt ungültige Muster im Modell und stellt nützliche Fehlermeldungen bereit.</span><span class="sxs-lookup"><span data-stu-id="90873-140">Detects invalid patterns in the model and provides helpful error messages.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="90873-141">Änderungsnachverfolgung</span><span class="sxs-lookup"><span data-stu-id="90873-141">Change tracking</span></span>

### <a name="snapshot-change-tracking"></a><span data-ttu-id="90873-142">Änderungsnachverfolgung für Momentaufnahmen</span><span class="sxs-lookup"><span data-stu-id="90873-142">Snapshot change tracking</span></span>

<span data-ttu-id="90873-143">Ermöglicht die automatische Erkennung von Änderungen in Entitäten durch den Vergleich des aktuellen Zustands mit einer Kopie (Momentaufnahme) des ursprünglichen Zustands.</span><span class="sxs-lookup"><span data-stu-id="90873-143">Allows changes in entities to be detected automatically by comparing current state against a copy (snapshot) of the original state.</span></span>

### <a name="notification-change-tracking"></a><span data-ttu-id="90873-144">Benachrichtigung der Änderungsnachverfolgung</span><span class="sxs-lookup"><span data-stu-id="90873-144">Notification change tracking</span></span>

<span data-ttu-id="90873-145">Ermöglicht es Ihren Entitäten, die Änderungsnachverfolgung zu benachrichtigen, wenn der Wert einer Eigenschaft geändert wurde.</span><span class="sxs-lookup"><span data-stu-id="90873-145">Allows your entities to notify the change tracker when property values are modified.</span></span>

### <a name="accessing-tracked-state"></a><span data-ttu-id="90873-146">Zugriff auf den erfassten Zustand</span><span class="sxs-lookup"><span data-stu-id="90873-146">Accessing tracked state</span></span>

<span data-ttu-id="90873-147">Über `DbContext.Entry` und `DbContext.ChangeTracker`.</span><span class="sxs-lookup"><span data-stu-id="90873-147">Via `DbContext.Entry` and `DbContext.ChangeTracker`.</span></span>

### <a name="attaching-detached-entitiesgraphs"></a><span data-ttu-id="90873-148">Anfügen abgekoppelter Entitäten/Graphen</span><span class="sxs-lookup"><span data-stu-id="90873-148">Attaching detached entities/graphs</span></span>

<span data-ttu-id="90873-149">Die neue `DbContext.AttachGraph`-API ermöglicht es, Entitäten erneut an einen Kontext anzufügen, um neue/geänderte Entitäten zu speichern.</span><span class="sxs-lookup"><span data-stu-id="90873-149">The new `DbContext.AttachGraph` API helps re-attach entities to a context in order to save new/modified entities.</span></span>

## <a name="saving-data"></a><span data-ttu-id="90873-150">Speichern von Daten</span><span class="sxs-lookup"><span data-stu-id="90873-150">Saving data</span></span>

### <a name="basic-save-functionality"></a><span data-ttu-id="90873-151">Grundlegende Speicherfunktionalität</span><span class="sxs-lookup"><span data-stu-id="90873-151">Basic save functionality</span></span>

<span data-ttu-id="90873-152">Ermöglicht das Persistieren von Änderungen an Entitätsinstanzen in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="90873-152">Allows changes to entity instances to be persisted to the database.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="90873-153">Optimistische Nebenläufigkeit</span><span class="sxs-lookup"><span data-stu-id="90873-153">Optimistic Concurrency</span></span>

<span data-ttu-id="90873-154">Schützt vor dem Überschreiben von Änderungen, die seit dem Abruf von Daten aus der Datenbank von einem anderen Benutzer vorgenommen wurden.</span><span class="sxs-lookup"><span data-stu-id="90873-154">Protects against overwriting changes made by another user since data was fetched from the database.</span></span>

### <a name="async-savechanges"></a><span data-ttu-id="90873-155">Asynchrones SaveChanges</span><span class="sxs-lookup"><span data-stu-id="90873-155">Async SaveChanges</span></span>

<span data-ttu-id="90873-156">Der aktuelle Thread kann freigegeben werden, um andere Anforderungen zu verarbeiten, während die Datenbank die über `SaveChanges` ausgeführten Befehle verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="90873-156">Can free up the current thread to process other requests while the database processes the commands issued from `SaveChanges`.</span></span>

### <a name="database-transactions"></a><span data-ttu-id="90873-157">Datenbanktransaktionen</span><span class="sxs-lookup"><span data-stu-id="90873-157">Database Transactions</span></span>

<span data-ttu-id="90873-158">Bedeutet, dass `SaveChanges` immer unteilbar ist (entweder ist der Vorgang als Ganzes erfolgreich, oder es werden keine Änderungen an der Datenbank vorgenommen).</span><span class="sxs-lookup"><span data-stu-id="90873-158">Means that `SaveChanges` is always atomic (meaning it either completely succeeds, or no changes are made to the database).</span></span> <span data-ttu-id="90873-159">Es gibt auch transaktionsbezogene APIs, die eine gemeinsame Verwendung von Transaktionen durch Kontextinstanzen usw. ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="90873-159">There are also transaction related APIs to allow sharing transactions between context instances etc.</span></span>

### <a name="relational-batching-of-statements"></a><span data-ttu-id="90873-160">Relational: Anweisungsbatches</span><span class="sxs-lookup"><span data-stu-id="90873-160">Relational: Batching of statements</span></span>

<span data-ttu-id="90873-161">Bietet bessere Leistung durch das Zusammenfassen mehrerer INSERT/UPDATE/DELETE-Befehle in einem einzigen Roundtrip an die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="90873-161">Provides better performance by batching up multiple INSERT/UPDATE/DELETE commands into a single roundtrip to the database.</span></span>

## <a name="query"></a><span data-ttu-id="90873-162">Abfrage</span><span class="sxs-lookup"><span data-stu-id="90873-162">Query</span></span>

### <a name="basic-linq-support"></a><span data-ttu-id="90873-163">Grundlegende LINQ-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="90873-163">Basic LINQ support</span></span>

<span data-ttu-id="90873-164">Bietet die Möglichkeit, LINQ zum Abrufen von Daten aus der Datenbank zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="90873-164">Provides the ability to use LINQ to retrieve data from the database.</span></span>

### <a name="mixed-clientserver-evaluation"></a><span data-ttu-id="90873-165">Gemischte Client-/Serverauswertung</span><span class="sxs-lookup"><span data-stu-id="90873-165">Mixed client/server evaluation</span></span>

<span data-ttu-id="90873-166">Abfragen können Logik enthalten, die nicht in der Datenbank ausgewertet werden kann und deshalb eine Auswertung der Daten nach dem Abruf in den Speicher erfordern.</span><span class="sxs-lookup"><span data-stu-id="90873-166">Enables queries to contain logic that cannot be evaluated in the database, and must therefore be evaluated after the data is retrieved into memory.</span></span>

### <a name="notracking"></a><span data-ttu-id="90873-167">NoTracking</span><span class="sxs-lookup"><span data-stu-id="90873-167">NoTracking</span></span>

<span data-ttu-id="90873-168">Abfragen können schneller ausgeführt werden, wenn der Kontext nicht überwachen muss, ob Änderungen an den Entitätsinstanzen durchgeführt wurden (das ist nützlich, wenn die Ergebnisse schreibgeschützt sind).</span><span class="sxs-lookup"><span data-stu-id="90873-168">Queries enables quicker query execution when the context does not need to monitor for changes to the entity instances (this is useful if the results are read-only).</span></span>

### <a name="eager-loading"></a><span data-ttu-id="90873-169">Eager Loading</span><span class="sxs-lookup"><span data-stu-id="90873-169">Eager loading</span></span>

<span data-ttu-id="90873-170">Stellt die Methoden `Include` und `ThenInclude` bereit, um bezogene Daten zu identifizieren, die bei der Abfrage ebenfalls abgerufen werden sollten.</span><span class="sxs-lookup"><span data-stu-id="90873-170">Provides the `Include` and `ThenInclude` methods to identify related data that should also be fetched when querying.</span></span>

### <a name="async-query"></a><span data-ttu-id="90873-171">Asynchrone Abfrage</span><span class="sxs-lookup"><span data-stu-id="90873-171">Async query</span></span>

<span data-ttu-id="90873-172">Der aktuelle Thread (und zugeordnete Ressourcen) kann freigegeben werden, um andere Anforderungen zu verarbeiten, während die Datenbank die Abfrage verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="90873-172">Can free up the current thread (and it's associated resources) to process other requests while the database processes the query.</span></span>

### <a name="raw-sql-queries"></a><span data-ttu-id="90873-173">Unformatierte SQL-Abfragen</span><span class="sxs-lookup"><span data-stu-id="90873-173">Raw SQL queries</span></span>

<span data-ttu-id="90873-174">Stellt die `DbSet.FromSql`-Methode zur Verwendung von unformatierten SQL-Abfragen zum Abrufen von Daten bereit.</span><span class="sxs-lookup"><span data-stu-id="90873-174">Provides the `DbSet.FromSql` method to use raw SQL queries to fetch data.</span></span> <span data-ttu-id="90873-175">Diese Abfragen können auch mit LINQ erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="90873-175">These queries can also be composed on using LINQ.</span></span>

## <a name="database-schema-management"></a><span data-ttu-id="90873-176">Verwaltung des Datenbankschemas</span><span class="sxs-lookup"><span data-stu-id="90873-176">Database schema management</span></span>

### <a name="database-creationdeletion-apis"></a><span data-ttu-id="90873-177">APIs zum Erstellen/Löschen von Datenbanken</span><span class="sxs-lookup"><span data-stu-id="90873-177">Database creation/deletion APIs</span></span>

<span data-ttu-id="90873-178">Sind hauptsächlich für Tests gedacht, bei denen Sie die Datenbank schnell erstellen/löschen möchten, ohne Migrationen zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="90873-178">Are mostly designed for testing where you want to quickly create/delete the database without using migrations.</span></span>

### <a name="relational-database-migrations"></a><span data-ttu-id="90873-179">Migrationen relationaler Datenbanken</span><span class="sxs-lookup"><span data-stu-id="90873-179">Relational database migrations</span></span>

<span data-ttu-id="90873-180">Hiermit werden spätere Änderungen an einem relationalen Datenbankschema ermöglicht, wenn sich Ihr Modell ändert.</span><span class="sxs-lookup"><span data-stu-id="90873-180">Allow a relational database schema to evolve overtime as your model changes.</span></span>

### <a name="reverse-engineer-from-database"></a><span data-ttu-id="90873-181">Reverse Engineering aus der Datenbank</span><span class="sxs-lookup"><span data-stu-id="90873-181">Reverse engineer from database</span></span>

<span data-ttu-id="90873-182">Richtet per Gerüstbau ein EF-Modell basierend auf einem vorhandenen relationalen Datenbankschema ein.</span><span class="sxs-lookup"><span data-stu-id="90873-182">Scaffolds an EF model based on an existing relational database schema.</span></span>

## <a name="database-providers"></a><span data-ttu-id="90873-183">Datenbankanbieter</span><span class="sxs-lookup"><span data-stu-id="90873-183">Database providers</span></span>

### <a name="sql-server"></a><span data-ttu-id="90873-184">SQL Server</span><span class="sxs-lookup"><span data-stu-id="90873-184">SQL Server</span></span>

<span data-ttu-id="90873-185">Stellt eine Verbindung mit Microsoft SQL Server 2008 und höher her.</span><span class="sxs-lookup"><span data-stu-id="90873-185">Connects to Microsoft SQL Server 2008 onwards.</span></span>

### <a name="sqlite"></a><span data-ttu-id="90873-186">SQLite</span><span class="sxs-lookup"><span data-stu-id="90873-186">SQLite</span></span>

<span data-ttu-id="90873-187">Stellt eine Verbindung mit einer SQLite 3-Datenbank her.</span><span class="sxs-lookup"><span data-stu-id="90873-187">Connects to a SQLite 3 database.</span></span>

### <a name="in-memory"></a><span data-ttu-id="90873-188">Im Arbeitsspeicher</span><span class="sxs-lookup"><span data-stu-id="90873-188">In-Memory</span></span>

<span data-ttu-id="90873-189">Ist für das einfache Testen ohne Verbindung mit einer echten Datenbank konzipiert.</span><span class="sxs-lookup"><span data-stu-id="90873-189">Is designed to easily enable testing without connecting to a real database.</span></span>

### <a name="3rd-party-providers"></a><span data-ttu-id="90873-190">Datenbankanbieter von Drittanbietern</span><span class="sxs-lookup"><span data-stu-id="90873-190">3rd party providers</span></span>

<span data-ttu-id="90873-191">Es stehen verschiedene Anbieter für andere Datenbank-Engines zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="90873-191">Several providers are available for other database engines.</span></span> <span data-ttu-id="90873-192">Eine vollständige Liste finden Sie unter [Datenbankanbieter](../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="90873-192">See [Database Providers](../providers/index.md) for a complete list.</span></span>
