---
title: Breaking Changes in EF Core 3.0
author: ajcvickers
ms.date: 12/03/2019
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: d614103169837238810fabd0a8889043c851ef14
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824862"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="a5588-102">Breaking Changes in EF Core 3.0</span><span class="sxs-lookup"><span data-stu-id="a5588-102">Breaking changes included in EF Core 3.0</span></span>

<span data-ttu-id="a5588-103">Die folgenden API-Änderungen und Behavior Changes können dazu führen, dass vorhandene Anwendungen beim Upgrade auf 3.0.0 nicht mehr funktionieren.</span><span class="sxs-lookup"><span data-stu-id="a5588-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="a5588-104">Änderungen, die sich voraussichtlich nur auf Datenbankanbieter auswirken, sind unter [Änderungen mit Auswirkungen auf den Anbieter](xref:core/providers/provider-log) dokumentiert.</span><span class="sxs-lookup"><span data-stu-id="a5588-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>

## <a name="summary"></a><span data-ttu-id="a5588-105">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="a5588-105">Summary</span></span>

| <span data-ttu-id="a5588-106">**Wichtige Änderung**</span><span class="sxs-lookup"><span data-stu-id="a5588-106">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="a5588-107">**Auswirkung**</span><span class="sxs-lookup"><span data-stu-id="a5588-107">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="a5588-108">LINQ-Abfragen werden nicht mehr auf dem Client ausgewertet</span><span class="sxs-lookup"><span data-stu-id="a5588-108">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="a5588-109">Hoch</span><span class="sxs-lookup"><span data-stu-id="a5588-109">High</span></span>       |
| [<span data-ttu-id="a5588-110">EF Core 3.0 zielt auf .NET Standard 2.1 und nicht auf .NET Standard 2.0 ab</span><span class="sxs-lookup"><span data-stu-id="a5588-110">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="a5588-111">Hoch</span><span class="sxs-lookup"><span data-stu-id="a5588-111">High</span></span>      |
| [<span data-ttu-id="a5588-112">Das EF Core-Befehlszeilentool (dotnet ef) ist nicht mehr Bestandteil des .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="a5588-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="a5588-113">Hoch</span><span class="sxs-lookup"><span data-stu-id="a5588-113">High</span></span>      |
| [<span data-ttu-id="a5588-114">DetectChanges berücksichtigt vom Speicher generierte Schlüsselwerte</span><span class="sxs-lookup"><span data-stu-id="a5588-114">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="a5588-115">Hoch</span><span class="sxs-lookup"><span data-stu-id="a5588-115">High</span></span>      |
| [<span data-ttu-id="a5588-116">FromSql, ExecuteSql und ExecuteSqlAsync wurden umbenannt</span><span class="sxs-lookup"><span data-stu-id="a5588-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="a5588-117">Hoch</span><span class="sxs-lookup"><span data-stu-id="a5588-117">High</span></span>      |
| [<span data-ttu-id="a5588-118">Abfragetypen werden mit Entitätstypen zusammengeführt</span><span class="sxs-lookup"><span data-stu-id="a5588-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="a5588-119">Hoch</span><span class="sxs-lookup"><span data-stu-id="a5588-119">High</span></span>      |
| [<span data-ttu-id="a5588-120">Entity Framework Core ist nicht mehr Bestandteil des gemeinsam verwendeten ASP.NET Core-Frameworks</span><span class="sxs-lookup"><span data-stu-id="a5588-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="a5588-121">Mittel</span><span class="sxs-lookup"><span data-stu-id="a5588-121">Medium</span></span>      |
| [<span data-ttu-id="a5588-122">Kaskadierende DELETE-Anweisungen werden standardmäßig sofort ausgeführt</span><span class="sxs-lookup"><span data-stu-id="a5588-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="a5588-123">Mittel</span><span class="sxs-lookup"><span data-stu-id="a5588-123">Medium</span></span>      |
| [<span data-ttu-id="a5588-124">Eager Loading von verwandten Entitäten erfolgt nun in einer einzelnen Abfrage</span><span class="sxs-lookup"><span data-stu-id="a5588-124">Eager loading of related entities now happens in a single query</span></span>](#eager-loading-single-query) | <span data-ttu-id="a5588-125">Mittel</span><span class="sxs-lookup"><span data-stu-id="a5588-125">Medium</span></span>      |
| [<span data-ttu-id="a5588-126">DeleteBehavior.Restrict verfügt über eine übersichtlichere Semantik</span><span class="sxs-lookup"><span data-stu-id="a5588-126">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="a5588-127">Mittel</span><span class="sxs-lookup"><span data-stu-id="a5588-127">Medium</span></span>      |
| [<span data-ttu-id="a5588-128">Die Konfigurations-API für Beziehungen abhängiger (owned) Typen wurde geändert</span><span class="sxs-lookup"><span data-stu-id="a5588-128">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="a5588-129">Mittel</span><span class="sxs-lookup"><span data-stu-id="a5588-129">Medium</span></span>      |
| [<span data-ttu-id="a5588-130">Für jede Eigenschaft wird separat ein ganzzahliger speicherinterner Schlüssel generiert</span><span class="sxs-lookup"><span data-stu-id="a5588-130">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="a5588-131">Mittel</span><span class="sxs-lookup"><span data-stu-id="a5588-131">Medium</span></span>      |
| [<span data-ttu-id="a5588-132">Abfragen ohne Nachverfolgung führen keine Identitätsauflösung mehr durch</span><span class="sxs-lookup"><span data-stu-id="a5588-132">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="a5588-133">Mittel</span><span class="sxs-lookup"><span data-stu-id="a5588-133">Medium</span></span>      |
| [<span data-ttu-id="a5588-134">Metadaten-API-Änderungen</span><span class="sxs-lookup"><span data-stu-id="a5588-134">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="a5588-135">Mittel</span><span class="sxs-lookup"><span data-stu-id="a5588-135">Medium</span></span>      |
| [<span data-ttu-id="a5588-136">Anbieterspezifische Metadaten-API-Änderungen</span><span class="sxs-lookup"><span data-stu-id="a5588-136">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="a5588-137">Mittel</span><span class="sxs-lookup"><span data-stu-id="a5588-137">Medium</span></span>      |
| [<span data-ttu-id="a5588-138">UseRowNumberForPaging wurde entfernt</span><span class="sxs-lookup"><span data-stu-id="a5588-138">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="a5588-139">Mittel</span><span class="sxs-lookup"><span data-stu-id="a5588-139">Medium</span></span>      |
| [<span data-ttu-id="a5588-140">FromSql-Methode kann nicht zusammengesetzt werden, wenn sie mit einer gespeicherten Prozedur verwendet wird</span><span class="sxs-lookup"><span data-stu-id="a5588-140">FromSql method when used with stored procedure cannot be composed</span></span>](#fromsqlsproc) | <span data-ttu-id="a5588-141">Mittel</span><span class="sxs-lookup"><span data-stu-id="a5588-141">Medium</span></span>      |
| [<span data-ttu-id="a5588-142">FromSql-Methoden können nur für die Stammelemente der Abfrage angegeben werden</span><span class="sxs-lookup"><span data-stu-id="a5588-142">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="a5588-143">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-143">Low</span></span>      |
| [<span data-ttu-id="a5588-144">~~Die Abfrageausführung wird auf Debugebene protokolliert~~ – zurückgesetzt</span><span class="sxs-lookup"><span data-stu-id="a5588-144">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="a5588-145">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-145">Low</span></span>      |
| [<span data-ttu-id="a5588-146">Temporäre Schlüsselwerte werden nicht mehr Entitätsinstanzen zugewiesen</span><span class="sxs-lookup"><span data-stu-id="a5588-146">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="a5588-147">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-147">Low</span></span>      |
| [<span data-ttu-id="a5588-148">Abhängige Entitäten, die die Tabelle gemeinsam mit dem Prinzipal verwenden, sind jetzt optional</span><span class="sxs-lookup"><span data-stu-id="a5588-148">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="a5588-149">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-149">Low</span></span>      |
| [<span data-ttu-id="a5588-150">Alle Entitäten, die eine Tabelle mit einer Spalte für das Parallelitätstoken gemeinsam verwenden, müssen diese einer Eigenschaft zuordnen</span><span class="sxs-lookup"><span data-stu-id="a5588-150">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="a5588-151">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-151">Low</span></span>      |
| [<span data-ttu-id="a5588-152">Benutzereigene Entitäten können nicht abgefragt werden, ohne dass der Besitzer eine Nachverfolgungsabfrage verwendet</span><span class="sxs-lookup"><span data-stu-id="a5588-152">Owned entities cannot be queried without the owner using a tracking query</span></span>](#owned-query) | <span data-ttu-id="a5588-153">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-153">Low</span></span>      |
| [<span data-ttu-id="a5588-154">Geerbte Eigenschaften von nicht zugeordneten Typen sind nun einer einzelnen Spalte für alle abgeleiteten Typen zugeordnet</span><span class="sxs-lookup"><span data-stu-id="a5588-154">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="a5588-155">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-155">Low</span></span>      |
| [<span data-ttu-id="a5588-156">Die Konvention zur Fremdschlüsseleigenschaft entspricht nicht mehr dem Namen der Prinzipaleigenschaft</span><span class="sxs-lookup"><span data-stu-id="a5588-156">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="a5588-157">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-157">Low</span></span>      |
| [<span data-ttu-id="a5588-158">Die Datenbankverbindung wird jetzt getrennt, wenn sie nicht mehr verwendet wird, bevor TransactionScope abgeschlossen wurde</span><span class="sxs-lookup"><span data-stu-id="a5588-158">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="a5588-159">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-159">Low</span></span>      |
| [<span data-ttu-id="a5588-160">Unterstützungsfelder werden standardmäßig verwendet</span><span class="sxs-lookup"><span data-stu-id="a5588-160">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="a5588-161">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-161">Low</span></span>      |
| [<span data-ttu-id="a5588-162">Eine Ausnahme wird ausgelöst, wenn mehrere kompatible Unterstützungsfelder gefunden werden</span><span class="sxs-lookup"><span data-stu-id="a5588-162">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="a5588-163">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-163">Low</span></span>      |
| [<span data-ttu-id="a5588-164">„Nur-Feld“-Eigenschaftennamen sollten dem Feldnamen entsprechen</span><span class="sxs-lookup"><span data-stu-id="a5588-164">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="a5588-165">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-165">Low</span></span>      |
| [<span data-ttu-id="a5588-166">AddLogging und AddMemoryCache werden nicht mehr von AddDbContext/AddDbContextPool aufgerufen</span><span class="sxs-lookup"><span data-stu-id="a5588-166">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="a5588-167">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-167">Low</span></span>      |
| [<span data-ttu-id="a5588-168">AddEntityFramework\* fügt IMemoryCache mit einer Größenbeschränkung hinzu</span><span class="sxs-lookup"><span data-stu-id="a5588-168">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>](#addentityframework-adds-imemorycache-with-a-size-limit) | <span data-ttu-id="a5588-169">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-169">Low</span></span>      |
| [<span data-ttu-id="a5588-170">DbContext.Entry erkennt nun mit DetectChanges lokale Änderungen</span><span class="sxs-lookup"><span data-stu-id="a5588-170">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="a5588-171">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-171">Low</span></span>      |
| [<span data-ttu-id="a5588-172">Schlüssel aus Zeichenfolgen und Bytearrays werden standardmäßig nicht vom Client generiert</span><span class="sxs-lookup"><span data-stu-id="a5588-172">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="a5588-173">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-173">Low</span></span>      |
| [<span data-ttu-id="a5588-174">ILoggerFactory ist nun ein bereichsbezogener Dienst</span><span class="sxs-lookup"><span data-stu-id="a5588-174">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="a5588-175">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-175">Low</span></span>      |
| [<span data-ttu-id="a5588-176">Für Proxys mit Lazy Loading (trägem Laden) gilt nicht mehr die Annahme, dass Navigationseigenschaften vollständig geladen sind</span><span class="sxs-lookup"><span data-stu-id="a5588-176">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="a5588-177">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-177">Low</span></span>      |
| [<span data-ttu-id="a5588-178">Die Erstellung zu vieler interner Dienstanbieter führt standardmäßig zu einem Fehler</span><span class="sxs-lookup"><span data-stu-id="a5588-178">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="a5588-179">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-179">Low</span></span>      |
| [<span data-ttu-id="a5588-180">Neues Verhalten für HasOne/HasMany bei Aufruf mit einer einzelnen Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="a5588-180">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="a5588-181">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-181">Low</span></span>      |
| [<span data-ttu-id="a5588-182">Der Rückgabetyp für mehrere asynchrone Methoden wurde von Task in ValueTask geändert</span><span class="sxs-lookup"><span data-stu-id="a5588-182">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="a5588-183">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-183">Low</span></span>      |
| [<span data-ttu-id="a5588-184">Die Anmerkung Relational:TypeMapping heißt nun nur noch TypeMapping</span><span class="sxs-lookup"><span data-stu-id="a5588-184">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="a5588-185">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-185">Low</span></span>      |
| [<span data-ttu-id="a5588-186">ToTable löst für einen abgeleiteten Typ eine Ausnahme aus</span><span class="sxs-lookup"><span data-stu-id="a5588-186">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="a5588-187">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-187">Low</span></span>      |
| [<span data-ttu-id="a5588-188">EF Core sendet keine PRAGMA-Anweisungen mehr, um Fremdschlüssel in SQLite zu erzwingen</span><span class="sxs-lookup"><span data-stu-id="a5588-188">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="a5588-189">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-189">Low</span></span>      |
| [<span data-ttu-id="a5588-190">SQLitePCLRaw.bundle_e_sqlite3 ist nun eine Abhängigkeit von Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="a5588-190">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="a5588-191">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-191">Low</span></span>      |
| [<span data-ttu-id="a5588-192">GUID-Werte werden jetzt als TEXT in SQLite gespeichert</span><span class="sxs-lookup"><span data-stu-id="a5588-192">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="a5588-193">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-193">Low</span></span>      |
| [<span data-ttu-id="a5588-194">Char-Werte werden jetzt als TEXT in SQLite gespeichert</span><span class="sxs-lookup"><span data-stu-id="a5588-194">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="a5588-195">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-195">Low</span></span>      |
| [<span data-ttu-id="a5588-196">Migrations-IDs werden jetzt mit dem Kalender der invarianten Kultur generiert</span><span class="sxs-lookup"><span data-stu-id="a5588-196">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="a5588-197">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-197">Low</span></span>      |
| [<span data-ttu-id="a5588-198">Erweiterungsinformationen und -metadaten wurden aus IDbContextOptionsExtension entfernt</span><span class="sxs-lookup"><span data-stu-id="a5588-198">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="a5588-199">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-199">Low</span></span>      |
| [<span data-ttu-id="a5588-200">LogQueryPossibleExceptionWithAggregateOperator wurde umbenannt</span><span class="sxs-lookup"><span data-stu-id="a5588-200">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="a5588-201">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-201">Low</span></span>      |
| [<span data-ttu-id="a5588-202">Übersichtlichere API für Namen von Fremdschlüsselconstraints</span><span class="sxs-lookup"><span data-stu-id="a5588-202">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="a5588-203">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-203">Low</span></span>      |
| [<span data-ttu-id="a5588-204">IRelationalDatabaseCreator.HasTables/HasTablesAsync ist jetzt öffentlich</span><span class="sxs-lookup"><span data-stu-id="a5588-204">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="a5588-205">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-205">Low</span></span>      |
| [<span data-ttu-id="a5588-206">Microsoft.EntityFrameworkCore.Design ist jetzt ein DevelopmentDependency-Paket</span><span class="sxs-lookup"><span data-stu-id="a5588-206">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="a5588-207">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-207">Low</span></span>      |
| [<span data-ttu-id="a5588-208">SQLitePCL.raw wurde auf Version 2.0.0 aktualisiert</span><span class="sxs-lookup"><span data-stu-id="a5588-208">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="a5588-209">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-209">Low</span></span>      |
| [<span data-ttu-id="a5588-210">NetTopologySuite wurde auf Version 2.0.0 aktualisiert</span><span class="sxs-lookup"><span data-stu-id="a5588-210">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="a5588-211">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-211">Low</span></span>      |
| [<span data-ttu-id="a5588-212">Microsoft.Data.SqlClient wird statt System.Data.SqlClient verwendet</span><span class="sxs-lookup"><span data-stu-id="a5588-212">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>](#SqlClient) | <span data-ttu-id="a5588-213">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-213">Low</span></span>      |
| [<span data-ttu-id="a5588-214">Beziehungen mit mehreren mehrdeutigen Selbstverweisen müssen nun konfiguriert werden</span><span class="sxs-lookup"><span data-stu-id="a5588-214">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="a5588-215">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-215">Low</span></span>      |
| [<span data-ttu-id="a5588-216">DbFunction.Schema ist NULL oder eine leere Zeichenfolge und ist deshalb so konfiguriert, dass es im Standardschema des Modells ist</span><span class="sxs-lookup"><span data-stu-id="a5588-216">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="a5588-217">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a5588-217">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="a5588-218">LINQ-Abfragen werden nicht mehr auf dem Client ausgewertet</span><span class="sxs-lookup"><span data-stu-id="a5588-218">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="a5588-219">[Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Siehe auch Issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="a5588-219">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="a5588-220">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-220">**Old behavior**</span></span>

<span data-ttu-id="a5588-221">Vor Version 3.0 wurden in EF Core automatisch Ausdrücke auf dem Client ausgewertet, wenn diese Teil einer Abfrage waren und nicht in SQL oder in einen Parameter konvertiert werden konnten.</span><span class="sxs-lookup"><span data-stu-id="a5588-221">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="a5588-222">Wenn potenziell aufwendige Ausdrücke auf dem Client ausgewertet werden sollten, wurde standardmäßig nur eine Warnung ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="a5588-222">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="a5588-223">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-223">**New behavior**</span></span>

<span data-ttu-id="a5588-224">Ab Version 3.0 können in EF Core nur Ausdrücke der obersten Projektion (dem letzten `Select()`-Aufruf in der Abfrage) auf dem Client ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="a5588-224">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="a5588-225">Wenn Ausdrücke in einem anderen Teil der Abfrage nicht in SQL oder in einen Parameter konvertiert werden können, wird eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="a5588-225">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="a5588-226">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-226">**Why**</span></span>

<span data-ttu-id="a5588-227">Die automatische Auswertung von Abfragen auf Clients hat zur Folge, dass viele Abfragen auch dann ausgeführt werden, wenn wichtige Teile nicht übersetzt werden können.</span><span class="sxs-lookup"><span data-stu-id="a5588-227">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="a5588-228">Dieses Verhalten kann zu unerwartetem und potenziell schädlichem Verhalten führen, das möglicherweise erst in der Produktionsphase erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="a5588-228">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="a5588-229">Eine nicht übersetzbare Bedingung in einem `Where()`-Aufruf kann sich beispielsweise so auswirken, dass alle Zeilen einer Tabelle vom Datenbankserver übertragen werden und der Filter auf dem Client angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="a5588-229">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="a5588-230">Wenn die Tabelle in der Entwicklungsphase nur wenige Zeilen enthält, ist es gut möglich, dass diese Situation unerkannt bleibt. Wenn die Anwendung jedoch in die Produktionsumgebung überführt wird, enthält die Tabelle möglicherweise Millionen von Zeilen, was zu schwerwiegenden Konsequenzen führen kann.</span><span class="sxs-lookup"><span data-stu-id="a5588-230">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="a5588-231">Wie die Erfahrung in der Entwicklungsphase gezeigt hat, können auch Warnungen, die bei Auswertungen auf dem Client ausgelöst werden, dieses Problem nicht lösen, da sie sich leicht ignorieren lassen.</span><span class="sxs-lookup"><span data-stu-id="a5588-231">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="a5588-232">Die automatische Auswertung auf Clients kann darüber hinaus zu Problemen führen, bei denen die Verbesserung der Abfrageübersetzung für bestimmte Ausdrücke zu unbeabsichtigten Breaking Changes zwischen Releases führt.</span><span class="sxs-lookup"><span data-stu-id="a5588-232">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="a5588-233">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-233">**Mitigations**</span></span>

<span data-ttu-id="a5588-234">Wenn sich eine Abfrage nicht vollständig übersetzen lässt, habe Sie zwei Möglichkeiten: Schreiben Sie sie entweder um, oder setzen Sie alternativ `AsEnumerable()`, `ToList()` oder ähnliche Methoden ein, um Daten wieder zurück an den Client zu übertragen, wo sie mit LINQ to Objects weiterverarbeitet werden können.</span><span class="sxs-lookup"><span data-stu-id="a5588-234">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="a5588-235">EF Core 3.0 zielt auf .NET Standard 2.1 und nicht auf .NET Standard 2.0 ab</span><span class="sxs-lookup"><span data-stu-id="a5588-235">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="a5588-236">Issue #15498</span><span class="sxs-lookup"><span data-stu-id="a5588-236">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="a5588-237">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-237">**Old behavior**</span></span>

<span data-ttu-id="a5588-238">Vor 3.0 zielte EF Core auf .NET Standard 2.0 ab und wurde auf allen Plattformen ausgeführt, die diesen Standard unterstützen, einschließlich .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="a5588-238">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="a5588-239">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-239">**New behavior**</span></span>

<span data-ttu-id="a5588-240">Ab 3.0 zielt EF Core auf .NET Standard 2.1 ab und wird auf allen Plattformen ausgeführt, die diesen Standard unterstützen.</span><span class="sxs-lookup"><span data-stu-id="a5588-240">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="a5588-241">Dies schließt .NET Framework nicht ein.</span><span class="sxs-lookup"><span data-stu-id="a5588-241">This does not include .NET Framework.</span></span>

<span data-ttu-id="a5588-242">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-242">**Why**</span></span>

<span data-ttu-id="a5588-243">Dies ist ein Teil der strategischen Entscheidung bezüglich .NET-Technologien, um den Fokus auf .NET Core und andere moderne .NET-Plattformen (z. B. Xamarin) zu legen.</span><span class="sxs-lookup"><span data-stu-id="a5588-243">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="a5588-244">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-244">**Mitigations**</span></span>

<span data-ttu-id="a5588-245">Erwägen Sie, zu einer moderneren .NET-Plattform zu wechseln.</span><span class="sxs-lookup"><span data-stu-id="a5588-245">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="a5588-246">Falls dies nicht möglich ist, verwenden Sie weiterhin EF Core 2.1 oder EF Core 2.2, die beide .NET Framework unterstützen.</span><span class="sxs-lookup"><span data-stu-id="a5588-246">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="a5588-247">Entity Framework Core ist nicht mehr Teil des gemeinsam verwendeten ASP.NET Core-Frameworks</span><span class="sxs-lookup"><span data-stu-id="a5588-247">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="a5588-248">Issueankündigungen #325</span><span class="sxs-lookup"><span data-stu-id="a5588-248">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="a5588-249">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-249">**Old behavior**</span></span>

<span data-ttu-id="a5588-250">Vor Version 3.0 von ASP.NET Core wurden EF Core und einige der zugehörigen Datenanbieter wie SQL Server eingeschlossen, wenn `Microsoft.AspNetCore.App` oder `Microsoft.AspNetCore.All` ein Paketverweis hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="a5588-250">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="a5588-251">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-251">**New behavior**</span></span>

<span data-ttu-id="a5588-252">Ab Version 3.0 enthält das gemeinsam verwendete ASP.NET Core-Framework weder EF Core noch zugehörige Datenanbieter.</span><span class="sxs-lookup"><span data-stu-id="a5588-252">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="a5588-253">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-253">**Why**</span></span>

<span data-ttu-id="a5588-254">Vor dieser Änderung konnte EF Core auf unterschiedliche Arten bezogen werden. Diese richteten sich danach, ob ASP.NET Core und SQL Server oder ein anderes Zielframework für die Anwendung vorgesehen war.</span><span class="sxs-lookup"><span data-stu-id="a5588-254">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="a5588-255">Bei einem Upgrade von ASP.NET Core wurde zudem ein Upgrade von EF Core und des SQL Server-Anbieters erzwungen, was nicht in allen Fällen gewünscht war.</span><span class="sxs-lookup"><span data-stu-id="a5588-255">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="a5588-256">Durch die eingeführte Änderung wird EF Core unter allen Anbietern, unterstützten .NET-Implementierungen und Anwendungstypen auf dieselbe Weise bezogen.</span><span class="sxs-lookup"><span data-stu-id="a5588-256">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="a5588-257">Entwickler können nun außerdem genau festlegen, wann für EF Core und zugehörige Datenanbieter ein Upgrade durchgeführt werden soll.</span><span class="sxs-lookup"><span data-stu-id="a5588-257">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="a5588-258">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-258">**Mitigations**</span></span>

<span data-ttu-id="a5588-259">Wenn Sie EF Core in einer Anwendung unter ASP.NET Core 3.0 oder in einer anderen unterstützten Anwendung verwenden möchten, müssen Sie dem EF Core-Datenbankanbieter, der für die Anwendung genutzt werden soll, explizit einen Paketverweis hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="a5588-259">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="a5588-260">Das EF Core-Befehlszeilentool (dotnet ef) ist nicht länger Bestandteil des .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="a5588-260">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="a5588-261">Issue #14016</span><span class="sxs-lookup"><span data-stu-id="a5588-261">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="a5588-262">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-262">**Old behavior**</span></span>

<span data-ttu-id="a5588-263">Vor Version 3.0 war das `dotnet ef`-Tool im .NET Core SDK enthalten und konnte von der Befehlszeile aus ohne zusätzliche Schritte in einem beliebigen Projekt verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="a5588-263">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="a5588-264">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-264">**New behavior**</span></span>

<span data-ttu-id="a5588-265">Ab Version 3.0 ist das `dotnet ef`-Tool nicht mehr im .NET SDK enthalten und muss deshalb vor der Verwendung explizit als lokales oder globales Tool installiert werden.</span><span class="sxs-lookup"><span data-stu-id="a5588-265">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="a5588-266">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-266">**Why**</span></span>

<span data-ttu-id="a5588-267">Durch diese Änderung kann `dotnet ef` als reguläres .NET-CLI-Tool für NuGet verteilt und aktualisiert werden und entspricht damit dem üblichen Verfahren, dass EF Core 3.0 ebenfalls immer als NuGet-Paket verteilt wird.</span><span class="sxs-lookup"><span data-stu-id="a5588-267">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="a5588-268">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-268">**Mitigations**</span></span>

<span data-ttu-id="a5588-269">Um Migrationen zu verwalten oder ein Gerüst für `DbContext` zu erstellen, installieren Sie `dotnet-ef` als globales Tool:</span><span class="sxs-lookup"><span data-stu-id="a5588-269">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="a5588-270">Eine Verwendung als lokales Tool ist ebenfalls möglich, wenn Sie die Abhängigkeiten eines Projekts wiederherstellen, das das Tool mithilfe einer [Toolmanifestdatei](https://github.com/dotnet/cli/issues/10288) als Toolabhängigkeit deklariert.</span><span class="sxs-lookup"><span data-stu-id="a5588-270">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="a5588-271">FromSql, ExecuteSql und ExecuteSqlAsync wurden umbenannt</span><span class="sxs-lookup"><span data-stu-id="a5588-271">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="a5588-272">Issue #10996</span><span class="sxs-lookup"><span data-stu-id="a5588-272">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="a5588-273">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-273">**Old behavior**</span></span>

<span data-ttu-id="a5588-274">Vor Version 3.0 wurden in EF Core diese Methodennamen überladen, um entweder mit einer normalen Zeichenfolge oder einer Zeichenfolge, die in SQL und Parameter interpoliert werden sollte, zu arbeiten.</span><span class="sxs-lookup"><span data-stu-id="a5588-274">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="a5588-275">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-275">**New behavior**</span></span>

<span data-ttu-id="a5588-276">Ab Version 3.0 verwenden Sie in EF Core `FromSqlRaw`, `ExecuteSqlRaw` und `ExecuteSqlRawAsync`, um eine parametrisierte Abfrage zu erstellen, bei der die Parameter einzeln aus der Abfragezeichenfolge übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="a5588-276">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="a5588-277">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a5588-277">For example:</span></span>

```csharp
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="a5588-278">Verwenden Sie `FromSqlInterpolated`, `ExecuteSqlInterpolated` und `ExecuteSqlInterpolatedAsync`, um eine parametrisierte Abfrage zu erstellen, bei der die Parameter als Teil einer interpolierten Abfragezeichenfolge übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="a5588-278">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="a5588-279">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a5588-279">For example:</span></span>

```csharp
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="a5588-280">Beachten Sie, dass die beiden oben genannten Abfragen dieselbe parametrisierte SQL mit denselben SQL-Parametern erzeugen.</span><span class="sxs-lookup"><span data-stu-id="a5588-280">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="a5588-281">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-281">**Why**</span></span>

<span data-ttu-id="a5588-282">Bei Methodenüberladungen wie dieser wird sehr leicht versehentlich die Methode für unformatierte Zeichenfolgen aufgerufen, wenn eigentlich beabsichtigt war, die Methode für interpolierte Zeichenfolgen aufzurufen (und umgekehrt).</span><span class="sxs-lookup"><span data-stu-id="a5588-282">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="a5588-283">Dadurch werden Abfragen möglicherweise nicht parametrisiert, obwohl dies der Fall sein sollte.</span><span class="sxs-lookup"><span data-stu-id="a5588-283">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="a5588-284">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-284">**Mitigations**</span></span>

<span data-ttu-id="a5588-285">Verwenden Sie ab sofort die neuen Methodennamen.</span><span class="sxs-lookup"><span data-stu-id="a5588-285">Switch to use the new method names.</span></span>

<a name="fromsqlsproc"></a>
### <a name="fromsql-method-when-used-with-stored-procedure-cannot-be-composed"></a><span data-ttu-id="a5588-286">FromSql-Methode kann nicht zusammengesetzt werden, wenn sie mit einer gespeicherten Prozedur verwendet wird</span><span class="sxs-lookup"><span data-stu-id="a5588-286">FromSql method when used with stored procedure cannot be composed</span></span>

[<span data-ttu-id="a5588-287">Issue #15392</span><span class="sxs-lookup"><span data-stu-id="a5588-287">Tracking Issue #15392</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15392)

<span data-ttu-id="a5588-288">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-288">**Old behavior**</span></span>

<span data-ttu-id="a5588-289">Vor EF Core 3.0 versuchte die FromSql-Methode, zu ermitteln, ob das übergebene SQL zusammengesetzt werden konnte.</span><span class="sxs-lookup"><span data-stu-id="a5588-289">Before EF Core 3.0, FromSql method tried to detect if the passed SQL can be composed upon.</span></span> <span data-ttu-id="a5588-290">Eine Clientauswertung wurde durchgeführt, wenn das SQL wie eine gespeicherte Prozedur nicht zusammensetzbar war.</span><span class="sxs-lookup"><span data-stu-id="a5588-290">It did client evaluation when the SQL was non-composable like a stored procedure.</span></span> <span data-ttu-id="a5588-291">Die folgende Abfrage funktionierte, indem die gespeicherte Prozedur auf dem Server und die FirstOrDefault-Methode auf dem Client ausgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="a5588-291">The following query worked by running the stored procedure on the server and doing FirstOrDefault on the client side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").FirstOrDefault();
```

<span data-ttu-id="a5588-292">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-292">**New behavior**</span></span>

<span data-ttu-id="a5588-293">Ab EF Core 3.0 versucht EF Core nicht mehr, das SQL zu analysieren.</span><span class="sxs-lookup"><span data-stu-id="a5588-293">Starting with EF Core 3.0, EF Core will not try to parse the SQL.</span></span> <span data-ttu-id="a5588-294">Wenn Sie also nach FromSqlRaw/FromSqlInterpolated zusammensetzen, setzt EF Core das SQL zusammen, indem eine Unterabfrage ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="a5588-294">So if you are composing after FromSqlRaw/FromSqlInterpolated, then EF Core will compose the SQL by causing sub query.</span></span> <span data-ttu-id="a5588-295">Wenn Sie eine gespeicherte Prozedur mit Zusammensetzung verwenden, wird eine Ausnahme für ungültige SQL-Syntax ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="a5588-295">So if you are using a stored procedure with composition then you will get an exception for invalid SQL syntax.</span></span>

<span data-ttu-id="a5588-296">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-296">**Why**</span></span>

<span data-ttu-id="a5588-297">EF Core 3.0 unterstützt die automatische Clientauswertung nicht, da diese wie [hier](#linq-queries-are-no-longer-evaluated-on-the-client) erläutert fehleranfällig war.</span><span class="sxs-lookup"><span data-stu-id="a5588-297">EF Core 3.0 does not support automatic client evaluation, since it was error prone as explained [here](#linq-queries-are-no-longer-evaluated-on-the-client).</span></span>

<span data-ttu-id="a5588-298">**Lösung**</span><span class="sxs-lookup"><span data-stu-id="a5588-298">**Mitigation**</span></span>

<span data-ttu-id="a5588-299">Wenn Sie eine gespeicherte Prozedur in FromSqlRaw/FromSqlInterpolated verwenden, wissen Sie, dass diese nicht zusammengesetzt werden kann. Daher können Sie __AsEnumerable/AsAsyncEnumerable__ direkt nach dem FromSql-Methodenaufruf hinzufügen, um die Zusammensetzung auf dem Server zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="a5588-299">If you are using a stored procedure in FromSqlRaw/FromSqlInterpolated, you know that it cannot be composed upon, so you can add __AsEnumerable/AsAsyncEnumerable__ right after the FromSql method call to avoid any composition on server side.</span></span>

```csharp
context.Products.FromSqlRaw("[dbo].[Ten Most Expensive Products]").AsEnumerable().FirstOrDefault();
```

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="a5588-300">FromSql-Methoden können nur für die Stammelemente der Abfrage angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="a5588-300">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="a5588-301">Issue #15704</span><span class="sxs-lookup"><span data-stu-id="a5588-301">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="a5588-302">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-302">**Old behavior**</span></span>

<span data-ttu-id="a5588-303">Vor EF Core 3.0 konnte die `FromSql`-Methode an einer beliebigen Stelle in der Abfrage angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="a5588-303">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="a5588-304">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-304">**New behavior**</span></span>

<span data-ttu-id="a5588-305">Ab EF Core 3.0 können die neuen Methoden `FromSqlRaw` und `FromSqlInterpolated` (durch die `FromSql` ersetzt wird) nur für Stammelemente von Abfragen angegeben werden, d.h. direkt für das `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="a5588-305">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="a5588-306">Der Versuch, sie an anderer Stelle anzugeben, führt zu einem Kompilierungsfehler.</span><span class="sxs-lookup"><span data-stu-id="a5588-306">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="a5588-307">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-307">**Why**</span></span>

<span data-ttu-id="a5588-308">Die Angabe von `FromSql` an einer anderen Stelle als für ein `DbSet` erbrachte keine zusätzliche Bedeutung und keinen Mehrwert, sondern konnte in bestimmten Szenarien zu Mehrdeutigkeiten führen.</span><span class="sxs-lookup"><span data-stu-id="a5588-308">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="a5588-309">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-309">**Mitigations**</span></span>

<span data-ttu-id="a5588-310">`FromSql` Aufrufe sollten so verschoben, dass sie direkt für das zugehörige `DbSet` gelten.</span><span class="sxs-lookup"><span data-stu-id="a5588-310">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="a5588-311">Abfragen ohne Nachverfolgung führen keine Identitätsauflösung mehr durch</span><span class="sxs-lookup"><span data-stu-id="a5588-311">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="a5588-312">Issue #13518</span><span class="sxs-lookup"><span data-stu-id="a5588-312">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="a5588-313">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-313">**Old behavior**</span></span>

<span data-ttu-id="a5588-314">Vor EF Core 3.0 wurde dieselbe Entitätsinstanz für jedes Vorkommen einer Entität mit einem bestimmten Typ und einer bestimmten ID verwendet.</span><span class="sxs-lookup"><span data-stu-id="a5588-314">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="a5588-315">Dies entspricht dem Verhalten von Abfragen zu Nachverfolgungen.</span><span class="sxs-lookup"><span data-stu-id="a5588-315">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="a5588-316">Die folgende Abfrage:</span><span class="sxs-lookup"><span data-stu-id="a5588-316">For example, this query:</span></span>

```csharp
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="a5588-317">gibt dieselbe `Category`-Instanz für jedes `Product` zurück, das der bestimmten Kategorie zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="a5588-317">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="a5588-318">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-318">**New behavior**</span></span>

<span data-ttu-id="a5588-319">Ab EF Core 3.0 werden unterschiedliche Entitätsinstanzen erstellt, wenn eine Entität mit einem bestimmten Typ und einer bestimmten ID an verschiedenen Stellen im zurückgegebenen Diagramm gefunden wird.</span><span class="sxs-lookup"><span data-stu-id="a5588-319">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="a5588-320">Die Abfrage oben gibt beispielsweise nun eine neue `Category`-Instanz für jedes `Product` zurück, auch wenn zwei Produkte derselben Kategorie zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="a5588-320">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="a5588-321">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-321">**Why**</span></span>

<span data-ttu-id="a5588-322">Die Identitätsauflösung (d. h. Feststellen, dass eine Entität über denselben Typ und die dieselbe ID wie die zuvor aufgetretene Entität verfügt) fügt zusätzlichen Aufwand für Leistung und Arbeitsspeicher hinzu.</span><span class="sxs-lookup"><span data-stu-id="a5588-322">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="a5588-323">Dies widerspricht normalerweise dem Grund, warum in erster Linie Abfragen ohne Nachverfolgung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="a5588-323">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="a5588-324">Obwohl die Identitätsauflösung manchmal nützlich sein kann, wird sie nicht benötigt, wenn die Entitäten serialisiert und an den Client gesendet werden, was manchmal für Abfragen ohne Nachverfolgung der Fall ist.</span><span class="sxs-lookup"><span data-stu-id="a5588-324">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="a5588-325">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-325">**Mitigations**</span></span>

<span data-ttu-id="a5588-326">Verwenden Sie eine Nachverfolgungsabfrage, falls die Auflösung erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="a5588-326">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="a5588-327">~~Die Abfrageausführung wird auf Debugebene protokolliert~~ – zurückgesetzt</span><span class="sxs-lookup"><span data-stu-id="a5588-327">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="a5588-328">Issue #14523</span><span class="sxs-lookup"><span data-stu-id="a5588-328">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="a5588-329">Wir haben diese Änderung zurückgesetzt, weil die neue Konfiguration in EF Core 3.0 die Angabe des Protokolliergrads für jedes Ereignis durch die Anwendung ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="a5588-329">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="a5588-330">Um z.B. die Protokollierung von SQL zu `Debug` zu ändern, konfigurieren Sie den Grad explizit in `OnConfiguring` oder `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="a5588-330">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="a5588-331">Temporäre Schlüsselwerte werden nicht mehr Entitätsinstanzen zugewiesen</span><span class="sxs-lookup"><span data-stu-id="a5588-331">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="a5588-332">Issue #12378</span><span class="sxs-lookup"><span data-stu-id="a5588-332">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="a5588-333">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-333">**Old behavior**</span></span>

<span data-ttu-id="a5588-334">Vor Version 3.0 wurden in EF Core allen Schlüsseleigenschaften temporäre Werte zugewiesen. Später wurden den Eigenschaften dann die tatsächlichen Werte zugewiesen, die von der Datenbank generiert wurden.</span><span class="sxs-lookup"><span data-stu-id="a5588-334">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="a5588-335">Die temporären Werte waren in der Regel große negative Zahlen.</span><span class="sxs-lookup"><span data-stu-id="a5588-335">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="a5588-336">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-336">**New behavior**</span></span>

<span data-ttu-id="a5588-337">Ab Version 3.0 wird in EF Core der temporäre Wert als Teil der Überwachungsinformationen der Entität gespeichert. Die Schlüsseleigenschaft selbst bleibt unverändert.</span><span class="sxs-lookup"><span data-stu-id="a5588-337">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="a5588-338">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-338">**Why**</span></span>

<span data-ttu-id="a5588-339">Diese Änderung wurde vorgenommen, damit temporäre Schlüsselwerte nicht fälschlicherweise dauerhaft gespeichert werden, wenn eine Entität, die vorher von einer `DbContext`-Instanz überwacht wurde, in eine andere `DbContext`-Instanz verschoben wird.</span><span class="sxs-lookup"><span data-stu-id="a5588-339">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="a5588-340">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-340">**Mitigations**</span></span>

<span data-ttu-id="a5588-341">Anwendungen, die Primärschlüsselwerte Fremdschlüsseln zuweisen, um Zuordnungen zwischen Entitäten zu erstellen, können vom alten Verhalten abhängig sein, wenn Primärschlüssel vom Speicher generiert werden und zu Entitäten im `Added`-Zustand gehören.</span><span class="sxs-lookup"><span data-stu-id="a5588-341">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="a5588-342">Sie können dies wie folgt vermeiden:</span><span class="sxs-lookup"><span data-stu-id="a5588-342">This can be avoided by:</span></span>
* <span data-ttu-id="a5588-343">Verwenden Sie keine vom Speicher generierten Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="a5588-343">Not using store-generated keys.</span></span>
* <span data-ttu-id="a5588-344">Legen Sie zum Erstellen von Zuordnungen keine Fremdschlüsselwerte, sondern Navigationseigenschaften fest.</span><span class="sxs-lookup"><span data-stu-id="a5588-344">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="a5588-345">Rufen Sie die tatsächlichen temporären Schlüsselwerte aus den Überwachungsinformationen der Entität ab.</span><span class="sxs-lookup"><span data-stu-id="a5588-345">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="a5588-346">`context.Entry(blog).Property(e => e.Id).CurrentValue` gibt beispielsweise den temporären Wert zurück, obwohl `blog.Id` noch nicht festgelegt wurde.</span><span class="sxs-lookup"><span data-stu-id="a5588-346">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="a5588-347">DetectChanges berücksichtigt vom Speicher generierte Schlüsselwerte</span><span class="sxs-lookup"><span data-stu-id="a5588-347">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="a5588-348">Issue #14616</span><span class="sxs-lookup"><span data-stu-id="a5588-348">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="a5588-349">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-349">**Old behavior**</span></span>

<span data-ttu-id="a5588-350">Vor Version 3.0 wurde in EF Core eine nicht überwachte Entität, die von `DetectChanges` gefunden wurde, im `Added`-Zustand überwacht und als neue Zeile eingefügt, sobald `SaveChanges` aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="a5588-350">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="a5588-351">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-351">**New behavior**</span></span>

<span data-ttu-id="a5588-352">Ab Version 3.0 wird in EF Core die Entität im `Modified`-Zustand überwacht, wenn für diese generierte Schlüsselwerte verwendet werden und ein Schlüsselwert festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="a5588-352">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="a5588-353">Es wird also davon ausgegangen, dass eine Zeile für die Entität vorhanden ist. Wenn `SaveChanges` aufgerufen wird, wird die Zeile außerdem aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="a5588-353">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="a5588-354">Falls kein Schlüsselwert festgelegt ist oder der Entitätstyp keine generierten Schlüssel verwendet, wird die neue Entität wie in früheren Version weiterhin im `Added`-Zustand überwacht.</span><span class="sxs-lookup"><span data-stu-id="a5588-354">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="a5588-355">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-355">**Why**</span></span>

<span data-ttu-id="a5588-356">Diese Änderung wurde vorgenommen, um bei der Verwendung von speichergenerierten Schlüsseln die Arbeit mit nicht verbundenen Entitätsgraphen einfacher und konsistenter zu gestalten.</span><span class="sxs-lookup"><span data-stu-id="a5588-356">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="a5588-357">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-357">**Mitigations**</span></span>

<span data-ttu-id="a5588-358">Diese Änderung kann dazu führen, dass eine Anwendung nicht mehr funktioniert. Dieser Fall tritt ein, wenn für Entitätstypen generierte Schlüssel vorgesehen sind, dann jedoch explizit Schlüsselwerte für neue Instanzen festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="a5588-358">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="a5588-359">Sie können dieses Problem vermeiden, indem Sie explizit festlegen, dass für Schlüsseleigenschaften keine generierten Werte verwendet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="a5588-359">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="a5588-360">Dies ist beispielsweise mit der Fluent-API möglich:</span><span class="sxs-lookup"><span data-stu-id="a5588-360">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="a5588-361">Alternativ bieten sich auch Datenanmerkungen an:</span><span class="sxs-lookup"><span data-stu-id="a5588-361">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="a5588-362">Kaskadierende Deletes werden standardmäßig sofort ausgeführt</span><span class="sxs-lookup"><span data-stu-id="a5588-362">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="a5588-363">Issue #10114</span><span class="sxs-lookup"><span data-stu-id="a5588-363">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="a5588-364">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-364">**Old behavior**</span></span>

<span data-ttu-id="a5588-365">Vor Version 3.0 wurden in EF Core kaskadierenden Aktionen (Löschen von abhängigen Entitäten nach dem Löschen eines erforderlichen Prinzipals oder nach dem Aufheben einer Beziehung zum erforderlichen Prinzipal) erst ausgeführt, wenn SaveChanges aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="a5588-365">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="a5588-366">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-366">**New behavior**</span></span>

<span data-ttu-id="a5588-367">Ab Version 3.0 werden in EF Core kaskadierende Aktionen angewendet, sobald die auslösende Bedingung erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="a5588-367">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="a5588-368">Wenn beispielsweise `context.Remove()` zum Löschen einer Prinzipalentität aufgerufen wird, führt das dazu, dass auch alle zugehörigen erforderlichen Entitäten, die überwacht werden, sofort auf `Deleted` festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="a5588-368">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="a5588-369">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-369">**Why**</span></span>

<span data-ttu-id="a5588-370">Diese Änderung wurde vorgenommen, um die Arbeit mit Datenbindungen und Überwachungsszenarios zu vereinfachen, in denen bekannt sein muss, welche Entitäten _vor_ dem Aufruf von `SaveChanges` gelöscht werden.</span><span class="sxs-lookup"><span data-stu-id="a5588-370">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="a5588-371">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-371">**Mitigations**</span></span>

<span data-ttu-id="a5588-372">Sie können das vorherige Verhalten wiederherstellen, indem Sie für `context.ChangedTracker` die entsprechenden Einstellungen vornehmen.</span><span class="sxs-lookup"><span data-stu-id="a5588-372">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="a5588-373">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a5588-373">For example:</span></span>

```csharp
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="eager-loading-single-query"></a>
### <a name="eager-loading-of-related-entities-now-happens-in-a-single-query"></a><span data-ttu-id="a5588-374">Eager Loading von verwandten Entitäten erfolgt nun in einer einzelnen Abfrage</span><span class="sxs-lookup"><span data-stu-id="a5588-374">Eager loading of related entities now happens in a single query</span></span>

[<span data-ttu-id="a5588-375">Issue #18022</span><span class="sxs-lookup"><span data-stu-id="a5588-375">Tracking issue #18022</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18022)

<span data-ttu-id="a5588-376">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-376">**Old behavior**</span></span>

<span data-ttu-id="a5588-377">Vor Version 3.0 löste Eager Loading von Sammlungsnavigationen über `Include`-Operatoren die Generierung mehrerer Abfragen auf der relationalen Datenbank aus. Eine Abfrage für jeden verwandten Entitätstyp.</span><span class="sxs-lookup"><span data-stu-id="a5588-377">Before 3.0, eagerly loading collection navigations via `Include` operators caused multiple queries to be generated on relational database, one for each related entity type.</span></span>

<span data-ttu-id="a5588-378">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-378">**New behavior**</span></span>

<span data-ttu-id="a5588-379">Ab Version 3.0 generiert EF Core eine einzelne Abfrage mit JOINs für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="a5588-379">Starting with 3.0, EF Core generates a single query with JOINs on relational databases.</span></span>

<span data-ttu-id="a5588-380">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-380">**Why**</span></span>

<span data-ttu-id="a5588-381">Das Ausgeben mehrerer Abfragen zum Implementieren einer einzelnen LINQ-Abfrage führte zu einer Vielzahl von Problemen, darunter Leistungsbeeinträchtigungen, da mehrere Datenbankroundtrips erforderlich waren, und Datenkohärenzprobleme, da jede Abfrage einen anderen Zustand der Datenbank beobachten könnte.</span><span class="sxs-lookup"><span data-stu-id="a5588-381">Issuing multiple queries to implement a single LINQ query caused numerous issues, including negative performance as multiple database roundtrips were necessary, and data coherency issues as each query could observe a different state of the database.</span></span>

<span data-ttu-id="a5588-382">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-382">**Mitigations**</span></span>

<span data-ttu-id="a5588-383">Zwar ist dies technisch gesehen kein Breaking Change, jedoch könnte es erhebliche Auswirkungen auf die Leistung der Anwendung haben, wenn eine einzelne Abfrage eine große Anzahl von `Include`-Operatoren für Sammlungsnavigationen enthält.</span><span class="sxs-lookup"><span data-stu-id="a5588-383">While technically this is not a breaking change, it could have a considerable effect on application performance when a single query contains a large number of `Include` operator on collection navigations.</span></span> <span data-ttu-id="a5588-384">Weitere Informationen sowie Informationen zum Umschreiben von Abfragen auf eine effizientere Weise finden Sie [in diesem Kommentar](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085).</span><span class="sxs-lookup"><span data-stu-id="a5588-384">[See this comment](https://github.com/aspnet/EntityFrameworkCore/issues/18022#issuecomment-542397085) for more information and for rewriting queries in a more efficient way.</span></span>

**

<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="a5588-385">DeleteBehavior.Restrict verfügt über eine übersichtlichere Semantik</span><span class="sxs-lookup"><span data-stu-id="a5588-385">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="a5588-386">Issue #12661</span><span class="sxs-lookup"><span data-stu-id="a5588-386">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="a5588-387">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-387">**Old behavior**</span></span>

<span data-ttu-id="a5588-388">Vor Version 3.0 wurden mit `DeleteBehavior.Restrict` Fremdschlüssel in der Datenbank mit `Restrict`-Semantik erstellt, es wurden aber auch unvorhergesehen interne Fixups geändert.</span><span class="sxs-lookup"><span data-stu-id="a5588-388">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="a5588-389">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-389">**New behavior**</span></span>

<span data-ttu-id="a5588-390">Ab Version 3.0 wird mit `DeleteBehavior.Restrict` sichergestellt, dass Fremdschlüssel mit `Restrict`-Semantik erstellt werden – d. h. ohne Überlappungen; ausgenommen bei Einschränkungsverletzungen – und ohne Auswirkungen auf EF-interne Fixups.</span><span class="sxs-lookup"><span data-stu-id="a5588-390">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="a5588-391">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-391">**Why**</span></span>

<span data-ttu-id="a5588-392">Diese Änderung wurde vorgenommen, um die Benutzerfreundlichkeit bei intuitiver Verwendung von `DeleteBehavior` ohne unerwartete Nebeneffekte zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="a5588-392">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="a5588-393">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-393">**Mitigations**</span></span>

<span data-ttu-id="a5588-394">Sie können das vorherige Verhalten wiederherstellen, indem Sie `DeleteBehavior.ClientNoAction` verwenden.</span><span class="sxs-lookup"><span data-stu-id="a5588-394">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="a5588-395">Abfragetypen werden mit Entitätstypen zusammengeführt</span><span class="sxs-lookup"><span data-stu-id="a5588-395">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="a5588-396">Issue #14194</span><span class="sxs-lookup"><span data-stu-id="a5588-396">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="a5588-397">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-397">**Old behavior**</span></span>

<span data-ttu-id="a5588-398">Vor Version 3.0 wurden in EF Core [Abfragetypen](xref:core/modeling/keyless-entity-types) dazu verwendet, Daten abzufragen, in denen kein Primärschlüssel auf strukturierte Weise festgelegt war.</span><span class="sxs-lookup"><span data-stu-id="a5588-398">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="a5588-399">Abfragetypen wurden also für die Zuordnung von Entitätstypen ohne Schlüssel verwendet (in der Regel in einer Sicht, in einigen Fällen aber auch in einer Tabelle), während reguläre Entitätstypen für einen verfügbaren Schlüssel verwendet wurden (in der Regel in einer Tabelle, in einigen Fällen aber auch in einer Sicht).</span><span class="sxs-lookup"><span data-stu-id="a5588-399">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="a5588-400">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-400">**New behavior**</span></span>

<span data-ttu-id="a5588-401">Aus einem Abfragetyp wird nun einfach ein Entitätstyp ohne Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="a5588-401">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="a5588-402">Entitätstypen ohne Schlüssel besitzen die gleiche Funktionalität wie Abfragetypen in früheren Versionen.</span><span class="sxs-lookup"><span data-stu-id="a5588-402">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="a5588-403">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-403">**Why**</span></span>

<span data-ttu-id="a5588-404">Diese Änderung wurde vorgenommen, um Unklarheiten bei der Verwendung von Abfragetypen zu beseitigen.</span><span class="sxs-lookup"><span data-stu-id="a5588-404">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="a5588-405">Abfragetypen sind schlüssellose Entitätstypen, die grundsätzlich schreibgeschützt sind. Sie sollten allerdings nicht allein wegen des Schreibschutzes verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="a5588-405">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="a5588-406">Des Weiteren werden sie oft Sichten zugeordnet, was aber nur daran liegt, dass für Letztere häufig keine Schlüssel definiert werden.</span><span class="sxs-lookup"><span data-stu-id="a5588-406">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="a5588-407">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-407">**Mitigations**</span></span>

<span data-ttu-id="a5588-408">Die folgenden Teile der API sind durch die Änderungen veraltet:</span><span class="sxs-lookup"><span data-stu-id="a5588-408">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="a5588-409">**`ModelBuilder.Query<>()`** : Rufen Sie stattdessen `ModelBuilder.Entity<>().HasNoKey()` auf, um einen schlüssellosen Entitätstyp festzulegen.</span><span class="sxs-lookup"><span data-stu-id="a5588-409">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="a5588-410">Dieses Verhalten wird nach wie vor nicht konventionsgemäß festgelegt, um Fehlkonfigurationen zu vermeiden, wenn ein Primärschlüssel erwartet wird, jedoch nicht mit der Konvention übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="a5588-410">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="a5588-411">**`DbQuery<>`** : Verwenden Sie stattdessen `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="a5588-411">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="a5588-412">**`DbContext.Query<>()`** : Verwenden Sie stattdessen `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="a5588-412">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="a5588-413">Die Konfigurations-API für Beziehungen abhängiger (owned) Typen wurde geändert</span><span class="sxs-lookup"><span data-stu-id="a5588-413">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="a5588-414">[Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="a5588-414">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="a5588-415">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-415">**Old behavior**</span></span>

<span data-ttu-id="a5588-416">Vor Version 3.0 wurde in EF Core die abhängige Beziehung direkt nach dem Aufruf von `OwnsOne` oder `OwnsMany` konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="a5588-416">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="a5588-417">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-417">**New behavior**</span></span>

<span data-ttu-id="a5588-418">Ab Version 3.0 von EF Core ist eine Fluent-API verfügbar, mit der über `WithOwner()` eine Navigationseigenschaft für den Besitzer konfiguriert werden kann.</span><span class="sxs-lookup"><span data-stu-id="a5588-418">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="a5588-419">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a5588-419">For example:</span></span>

```csharp
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="a5588-420">Die Konfiguration für die Beziehung zwischen Besitzertyp und abhängigem Typ sollte nun ähnlich wie bei der Konfiguration anderer Beziehungen nach `WithOwner()` verkettet werden.</span><span class="sxs-lookup"><span data-stu-id="a5588-420">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="a5588-421">Die Konfiguration für den abhängigen Typ wird jedoch weiterhin nach `OwnsOne()/OwnsMany()` verkettet.</span><span class="sxs-lookup"><span data-stu-id="a5588-421">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="a5588-422">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a5588-422">For example:</span></span>

```csharp
modelBuilder.Entity<Order>.OwnsOne(e => e.Details, eb =>
    {
        eb.WithOwner()
            .HasForeignKey(e => e.AlternateId)
            .HasConstraintName("FK_OrderDetails");
            
        eb.ToTable("OrderDetails");
        eb.HasKey(e => e.AlternateId);
        eb.HasIndex(e => e.Id);

        eb.HasOne(e => e.Customer).WithOne();

        eb.HasData(
            new OrderDetails
            {
                AlternateId = 1,
                Id = -1
            });
    });
```

<span data-ttu-id="a5588-423">Wenn Sie zusätzlich `Entity()`, `HasOne()`, oder `Set()` für das Ziel eines abhängigen Typs aufrufen, wird keine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="a5588-423">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="a5588-424">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-424">**Why**</span></span>

<span data-ttu-id="a5588-425">Diese Änderung wurde vorgenommen, um eine deutlichere Trennung zwischen der Konfiguration des abhängigen Typs und der _Beziehung zum abhängigen Typ_ zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="a5588-425">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="a5588-426">Dadurch werden für Methoden wie `HasForeignKey` Mehrdeutigkeiten und Unklarheiten beseitigt.</span><span class="sxs-lookup"><span data-stu-id="a5588-426">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="a5588-427">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-427">**Mitigations**</span></span>

<span data-ttu-id="a5588-428">Passen Sie für die Beziehungen von abhängigen Typen die Konfiguration so an, dass die neue Fluent-API wie im obigen Beispiel verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="a5588-428">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="a5588-429">Abhängige Entitäten, die die Tabelle gemeinsam mit dem Prinzipal verwenden, sind jetzt optional</span><span class="sxs-lookup"><span data-stu-id="a5588-429">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="a5588-430">Issue #9005</span><span class="sxs-lookup"><span data-stu-id="a5588-430">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="a5588-431">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-431">**Old behavior**</span></span>

<span data-ttu-id="a5588-432">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="a5588-432">Consider the following model:</span></span>
```csharp
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```
<span data-ttu-id="a5588-433">Wenn `OrderDetails` im Besitz von `Order` ist oder explizit derselben Tabelle zugeordnet ist, war vor Version 3.0 in EF Core immer eine `OrderDetails`-Instanz beim Hinzufügen eines neuen `Order`-Objekts erforderlich.</span><span class="sxs-lookup"><span data-stu-id="a5588-433">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="a5588-434">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-434">**New behavior**</span></span>

<span data-ttu-id="a5588-435">Ab Version 3.0 bietet EF Core die Möglichkeit, `Order` ohne `OrderDetails` hinzuzufügen, und alle `OrderDetails`-Eigenschaften, mit Ausnahme des primären Schlüssels, werden Spalten zugeordnet, die NULL-Werte zulassen.</span><span class="sxs-lookup"><span data-stu-id="a5588-435">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="a5588-436">Bei Abfragen von EF Core wird `OrderDetails` auf `null` festgelegt, wenn eine der erforderlichen Eigenschaften keinen Wert aufweist oder keine erforderlichen Eigenschaften außer dem primären Schlüssel vorhanden und alle Eigenschaften `null` sind.</span><span class="sxs-lookup"><span data-stu-id="a5588-436">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="a5588-437">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-437">**Mitigations**</span></span>

<span data-ttu-id="a5588-438">Wenn Ihr Modell von der gemeinsamen Nutzung einer Tabelle mit allen optionalen Spalten abhängig ist, aber die Navigation, die darauf zeigt, wahrscheinlich nicht `null` ist, sollte die Anwendung für die Handhabung von Fällen geändert werden, in denen die Navigation `null` ist.</span><span class="sxs-lookup"><span data-stu-id="a5588-438">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="a5588-439">Wenn das nicht möglich ist, sollte dem Entitätstyp eine erforderliche Eigenschaft hinzugefügt werden oder mindestens einer Eigenschaft sollte ein Wert ungleich `null` zugewiesen sein.</span><span class="sxs-lookup"><span data-stu-id="a5588-439">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="a5588-440">Alle Entitäten, die eine Tabelle mit einer Spalte für das Parallelitätstoken gemeinsam verwenden, müssen diese einer Eigenschaft zuordnen</span><span class="sxs-lookup"><span data-stu-id="a5588-440">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="a5588-441">Issue #14154</span><span class="sxs-lookup"><span data-stu-id="a5588-441">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="a5588-442">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-442">**Old behavior**</span></span>

<span data-ttu-id="a5588-443">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="a5588-443">Consider the following model:</span></span>
```csharp
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public byte[] Version { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Order>()
        .Property(o => o.Version).IsRowVersion().HasColumnName("Version");
}
```
<span data-ttu-id="a5588-444">Wenn `OrderDetails` im Besitz von `Order` ist oder explizit derselben Tabelle zugeordnet ist, wird vor Version 3.0 in EF Core durch das alleinige Aktualisieren von `OrderDetails` der `Version`-Wert auf dem Client nicht aktualisiert, und das nächste Update schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="a5588-444">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="a5588-445">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-445">**New behavior**</span></span>

<span data-ttu-id="a5588-446">Ab Version 3.0 gibt EF Core den neuen `Version`-Wert an `Order` weiter, wenn dies Besitzer von `OrderDetails` ist.</span><span class="sxs-lookup"><span data-stu-id="a5588-446">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="a5588-447">Andernfalls wird eine Ausnahme während der Modellvalidierung ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="a5588-447">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="a5588-448">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-448">**Why**</span></span>

<span data-ttu-id="a5588-449">Diese Änderung wurde vorgenommen, um einen veralteten Wert für ein Parallelitätstoken zu vermeiden, wenn nur eine der Entitäten, die derselben Tabelle zugeordnet sind, aktualisiert wird.</span><span class="sxs-lookup"><span data-stu-id="a5588-449">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="a5588-450">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-450">**Mitigations**</span></span>

<span data-ttu-id="a5588-451">Alle Entitäten, die die Tabelle gemeinsam nutzen, müssen eine Eigenschaft enthalten, die der Spalte für das Parallelitätstoken zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="a5588-451">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="a5588-452">Es ist möglich, eine solche im Schattenzustand zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="a5588-452">It's possible the create one in shadow-state:</span></span>
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="owned-query"></a>

### <a name="owned-entities-cannot-be-queried-without-the-owner-using-a-tracking-query"></a><span data-ttu-id="a5588-453">Benutzereigene Entitäten können nicht abgefragt werden, ohne dass der Besitzer eine Nachverfolgungsabfrage verwendet</span><span class="sxs-lookup"><span data-stu-id="a5588-453">Owned entities cannot be queried without the owner using a tracking query</span></span>

[<span data-ttu-id="a5588-454">Issue 18876</span><span class="sxs-lookup"><span data-stu-id="a5588-454">Tracking Issue #18876</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/18876)

<span data-ttu-id="a5588-455">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-455">**Old behavior**</span></span>

<span data-ttu-id="a5588-456">Vor EF Core 3.0 konnten die benutzereigenen Entitäten wie jedes andere Navigationselement abgefragt werden.</span><span class="sxs-lookup"><span data-stu-id="a5588-456">Before EF Core 3.0, the owned entities could be queried as any other navigation.</span></span>

```csharp
context.People.Select(p => p.Address);
```

<span data-ttu-id="a5588-457">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-457">**New behavior**</span></span>

<span data-ttu-id="a5588-458">Ab EF Core 3.0 wird eine Ausnahme ausgelöst, wenn eine Nachverfolgungsabfrage eine benutzereigene Entität ohne den Besitzer projiziert.</span><span class="sxs-lookup"><span data-stu-id="a5588-458">Starting with 3.0, EF Core will throw if a tracking query projects an owned entity without the owner.</span></span>

<span data-ttu-id="a5588-459">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-459">**Why**</span></span>

<span data-ttu-id="a5588-460">Benutzereigene Entitäten können ohne den Besitzer nicht bearbeitet werden, daher handelt es sich in den meisten Fällen um einen Fehler, wenn sie auf diese Weise abgefragt werden.</span><span class="sxs-lookup"><span data-stu-id="a5588-460">Owned entities cannot be manipulated without the owner, so in the vast majority of cases querying them in this way is an error.</span></span>

<span data-ttu-id="a5588-461">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-461">**Mitigations**</span></span>

<span data-ttu-id="a5588-462">Wenn die benutzereigene Entität nachverfolgt werden soll, damit sie später Änderungen an ihr vorgenommen werden können, sollte der Besitzer in der Abfrage enthalten sein.</span><span class="sxs-lookup"><span data-stu-id="a5588-462">If the owned entity should be tracked to be modified in any way later then the owner should be included in the query.</span></span>

<span data-ttu-id="a5588-463">Fügen Sie andernfalls einen `AsNoTracking()`-Aufruf hinzu:</span><span class="sxs-lookup"><span data-stu-id="a5588-463">Otherwise add an `AsNoTracking()` call:</span></span>

```csharp
context.People.Select(p => p.Address).AsNoTracking();
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="a5588-464">Geerbte Eigenschaften von nicht zugeordneten Typen sind nun einer einzelnen Spalte für alle abgeleiteten Typen zugeordnet</span><span class="sxs-lookup"><span data-stu-id="a5588-464">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="a5588-465">Issue #13998</span><span class="sxs-lookup"><span data-stu-id="a5588-465">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="a5588-466">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-466">**Old behavior**</span></span>

<span data-ttu-id="a5588-467">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="a5588-467">Consider the following model:</span></span>
```csharp
public abstract class EntityBase
{
    public int Id { get; set; }
}

public abstract class OrderBase : EntityBase
{
    public int ShippingAddress { get; set; }
}

public class BulkOrder : OrderBase
{
}

public class Order : OrderBase
{
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>();
    modelBuilder.Entity<Order>();
}
```

<span data-ttu-id="a5588-468">Vor Version 3.0 wurde in EF Core die Eigenschaft `ShippingAddress` standardmäßig separaten Spalten für `BulkOrder` und `Order` zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="a5588-468">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="a5588-469">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-469">**New behavior**</span></span>

<span data-ttu-id="a5588-470">Ab Version 3.0 erstellt EF Core nur eine Spalte für `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="a5588-470">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="a5588-471">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-471">**Why**</span></span>

<span data-ttu-id="a5588-472">Der alte Verhalten war unerwartet.</span><span class="sxs-lookup"><span data-stu-id="a5588-472">The old behavoir was unexpected.</span></span>

<span data-ttu-id="a5588-473">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-473">**Mitigations**</span></span>

<span data-ttu-id="a5588-474">Die Eigenschaft kann noch immer explizit einer separaten Spalte für die abgeleiteten Typen zugeordnet werden:</span><span class="sxs-lookup"><span data-stu-id="a5588-474">The property can still be explicitly mapped to separate column on the derived types:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>()
        .Property(o => o.ShippingAddress).HasColumnName("BulkShippingAddress");
    modelBuilder.Entity<Order>()
        .Property(o => o.ShippingAddress).HasColumnName("ShippingAddress");
}
```

<a name="fkp"></a>

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="a5588-475">Die Konvention zur Fremdschlüsseleigenschaft entspricht nicht mehr dem Namen der Prinzipaleigenschaft</span><span class="sxs-lookup"><span data-stu-id="a5588-475">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="a5588-476">Issue #13274</span><span class="sxs-lookup"><span data-stu-id="a5588-476">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="a5588-477">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-477">**Old behavior**</span></span>

<span data-ttu-id="a5588-478">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="a5588-478">Consider the following model:</span></span>
```csharp
public class Customer
{
    public int CustomerId { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```
<span data-ttu-id="a5588-479">Vor Version 3.0 wurde in EF Core gemäß der Konvention die `CustomerId`-Eigenschaft für den Fremdschlüssel verwendet.</span><span class="sxs-lookup"><span data-stu-id="a5588-479">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="a5588-480">Wenn `Order` jedoch ein abhängiger Typ ist, wäre `CustomerId` der Primärschlüssel, was normalerweise nicht der Erwartungshaltung entspricht.</span><span class="sxs-lookup"><span data-stu-id="a5588-480">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="a5588-481">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-481">**New behavior**</span></span>

<span data-ttu-id="a5588-482">Ab Version 3.0 werden in EF Core konventionsgemäß keine Eigenschaften mehr für Fremdschlüssel verwendet, wenn diese denselben Namen wie die Prinzipaleigenschaft besitzen.</span><span class="sxs-lookup"><span data-stu-id="a5588-482">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="a5588-483">Die Muster für den Namen des Prinzipaltyps, der mit dem Namen der Prinzipaleigenschaft verkettet wird, und für den Navigationsnamen, der mit dem Namen der Prinzipaleigenschaft verkettet wird, werden jedoch weiterhin abgeglichen.</span><span class="sxs-lookup"><span data-stu-id="a5588-483">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="a5588-484">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a5588-484">For example:</span></span>

```csharp
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```

```csharp
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int BuyerId { get; set; }
    public Customer Buyer { get; set; }
}
```

<span data-ttu-id="a5588-485">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-485">**Why**</span></span>

<span data-ttu-id="a5588-486">Diese Änderung wurde vorgenommen, um zu vermeiden, dass versehentlich eine Primärschlüsseleigenschaft für den abhängigen Typ definiert wird.</span><span class="sxs-lookup"><span data-stu-id="a5588-486">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="a5588-487">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-487">**Mitigations**</span></span>

<span data-ttu-id="a5588-488">Wenn die Eigenschaft als Fremdschlüssel und daher Teil des Primärschlüssels vorgesehen war, müssen Sie diese explizit als solche festlegen.</span><span class="sxs-lookup"><span data-stu-id="a5588-488">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="a5588-489">Die Datenbankverbindung wird jetzt geschlossen, wenn sie nicht mehr verwendet wird, bevor TransactionScope abgeschlossen wurde</span><span class="sxs-lookup"><span data-stu-id="a5588-489">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="a5588-490">Issue #14218</span><span class="sxs-lookup"><span data-stu-id="a5588-490">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="a5588-491">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-491">**Old behavior**</span></span>

<span data-ttu-id="a5588-492">Wenn vor Version 3.0 in EF Core der Kontext die Verbindung in einem `TransactionScope` öffnet, bleibt die Verbindung geöffnet, während der aktuelle `TransactionScope` aktiv ist.</span><span class="sxs-lookup"><span data-stu-id="a5588-492">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

```csharp
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();

        // Old behavior: Connection is still open at this point

        var categories = context.ProductCategories().ToList();
    }
}
```

<span data-ttu-id="a5588-493">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-493">**New behavior**</span></span>

<span data-ttu-id="a5588-494">Ab Version 3.0 schließt EF Core die Verbindung, sobald sie nicht mehr verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="a5588-494">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="a5588-495">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-495">**Why**</span></span>

<span data-ttu-id="a5588-496">Diese Änderung ermöglicht die Verwendung mehrerer Kontexte in demselben `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="a5588-496">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="a5588-497">Das neue Verhalten entspricht außerdem EF6.</span><span class="sxs-lookup"><span data-stu-id="a5588-497">The new behavior also matches EF6.</span></span>

<span data-ttu-id="a5588-498">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-498">**Mitigations**</span></span>

<span data-ttu-id="a5588-499">Wenn die Verbindung geöffnet bleiben muss, wird durch einen expliziten Aufruf von `OpenConnection()` sichergestellt, dass EF Core diese nicht vorzeitig schließt:</span><span class="sxs-lookup"><span data-stu-id="a5588-499">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

```csharp
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.Database.OpenConnection();
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();

        var categories = context.ProductCategories().ToList();
        context.Database.CloseConnection();
    }
}
```

<a name="each"></a>

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="a5588-500">Für jede Eigenschaft wird separat ein ganzzahliger speicherinterner Schlüssel generiert</span><span class="sxs-lookup"><span data-stu-id="a5588-500">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="a5588-501">Issue #6872</span><span class="sxs-lookup"><span data-stu-id="a5588-501">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="a5588-502">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-502">**Old behavior**</span></span>

<span data-ttu-id="a5588-503">Vor Version 3.0 wurde in EF Core ein gemeinsam verwendeter Wertegenerator für alle speicherinternen ganzzahligen Schlüsseleigenschaften verwendet.</span><span class="sxs-lookup"><span data-stu-id="a5588-503">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="a5588-504">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-504">**New behavior**</span></span>

<span data-ttu-id="a5588-505">Ab Version 3.0 von EF Core wird für jede ganzzahlige Schlüsseleigenschaft ein eigener Wertegenerator bei der Verwendung der In-Memory Database verwendet.</span><span class="sxs-lookup"><span data-stu-id="a5588-505">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="a5588-506">Wenn die Datenbank gelöscht wird, wird die Schlüsselgenerierung für alle Tabellen zurückgesetzt.</span><span class="sxs-lookup"><span data-stu-id="a5588-506">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="a5588-507">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-507">**Why**</span></span>

<span data-ttu-id="a5588-508">Diese Änderung wurde vorgenommen, um die speicherinterne Schlüsselgenerierung enger mit der Schlüsselgenerierung für echte Datenbanken abzustimmen. Außerdem sollen bei Verwendung der In-Memory Database Tests leichter voneinander isoliert werden können.</span><span class="sxs-lookup"><span data-stu-id="a5588-508">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="a5588-509">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-509">**Mitigations**</span></span>

<span data-ttu-id="a5588-510">Wenn Anwendungen von bestimmten speicherinternen Schlüsselwerten abhängig sind, funktionieren Erstere durch die eingeführte Änderung möglicherweise nicht mehr.</span><span class="sxs-lookup"><span data-stu-id="a5588-510">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="a5588-511">Versuchen Sie, Abhängigkeiten von bestimmten Schlüsselwerten zu vermeiden, oder passen Sie die Anwendung an das neue Verhalten an.</span><span class="sxs-lookup"><span data-stu-id="a5588-511">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="a5588-512">Unterstützungsfelder werden standardmäßig verwendet</span><span class="sxs-lookup"><span data-stu-id="a5588-512">Backing fields are used by default</span></span>

[<span data-ttu-id="a5588-513">Issue #12430</span><span class="sxs-lookup"><span data-stu-id="a5588-513">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="a5588-514">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-514">**Old behavior**</span></span>

<span data-ttu-id="a5588-515">Vor Version 3.0 wurde in EF Core mithilfe der Getter- und Settermethoden standardmäßig der Eigenschaftswert gelesen und geschrieben. Das war selbst dann der Fall, wenn das Unterstützungsfeld für eine Eigenschaft nicht bekannt war.</span><span class="sxs-lookup"><span data-stu-id="a5588-515">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="a5588-516">Eine Ausnahme war die Abfrageausführung, bei der das Unterstützungsfeld direkt festgelegt wurde, wenn es bekannt war.</span><span class="sxs-lookup"><span data-stu-id="a5588-516">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="a5588-517">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-517">**New behavior**</span></span>

<span data-ttu-id="a5588-518">Ab Version 3.0 wird in EF Core die Eigenschaft mithilfe des Unterstützungsfelds gelesen und geschrieben, wenn dieses für eine Eigenschaft bekannt ist.</span><span class="sxs-lookup"><span data-stu-id="a5588-518">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="a5588-519">Dies kann dazu führen, dass eine Anwendung nicht mehr funktioniert, wenn sie auf zusätzliches Verhalten angewiesen ist, das im Code der Getter- und Settermethoden definiert ist.</span><span class="sxs-lookup"><span data-stu-id="a5588-519">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="a5588-520">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-520">**Why**</span></span>

<span data-ttu-id="a5588-521">Diese Änderung wurde vorgenommen, um zu verhindern, dass in EF Core fälschlicherweise immer dann Geschäftslogik ausgelöst wird, wenn Datenbankvorgänge für Entitäten durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="a5588-521">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="a5588-522">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-522">**Mitigations**</span></span>

<span data-ttu-id="a5588-523">Sie können das Verhalten von vor Version 3.0 wiederherstellen, indem Sie in `ModelBuilder` den Zugriffsmodus für die Eigenschaft konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a5588-523">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="a5588-524">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a5588-524">For example:</span></span>

```csharp
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="a5588-525">Eine Ausnahme wird ausgelöst, wenn mehrere kompatible Unterstützungsfelder gefunden werden</span><span class="sxs-lookup"><span data-stu-id="a5588-525">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="a5588-526">Issue #12523</span><span class="sxs-lookup"><span data-stu-id="a5588-526">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="a5588-527">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-527">**Old behavior**</span></span>

<span data-ttu-id="a5588-528">Vor Version 3.0 wurde in EF Core ein Unterstützungsfeld auf Grundlage einer Rangfolge ausgewählt, wenn die Regeln zum Suchen eines Eigenschaftsfelds auf mehrere Felder zutrafen.</span><span class="sxs-lookup"><span data-stu-id="a5588-528">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="a5588-529">Dabei konnte es vorkommen, dass in mehrdeutigen Fällen das falsche Feld verwendet wurde.</span><span class="sxs-lookup"><span data-stu-id="a5588-529">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="a5588-530">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-530">**New behavior**</span></span>

<span data-ttu-id="a5588-531">Ab Version 3.0 wird in EF Core eine Ausnahme ausgelöst, wenn sich mehrere Felder auf dieselbe Eigenschaft beziehen.</span><span class="sxs-lookup"><span data-stu-id="a5588-531">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="a5588-532">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-532">**Why**</span></span>

<span data-ttu-id="a5588-533">Diese Änderung wurde vorgenommen, um zu vermeiden, dass automatisch ein falsches Feld verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="a5588-533">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="a5588-534">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-534">**Mitigations**</span></span>

<span data-ttu-id="a5588-535">Für Eigenschaften mit mehrdeutigen Unterstützungsfeldern muss das zu verwendende Feld explizit festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="a5588-535">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="a5588-536">Mit der Fluent-API ist dies beispielsweise wie folgt möglich:</span><span class="sxs-lookup"><span data-stu-id="a5588-536">For example, using the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="a5588-537">„Nur-Feld“-Eigenschaftsnamen sollten dem Feldnamen entsprechen</span><span class="sxs-lookup"><span data-stu-id="a5588-537">Field-only property names should match the field name</span></span>

<span data-ttu-id="a5588-538">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-538">**Old behavior**</span></span>

<span data-ttu-id="a5588-539">Vor Version 3.0 konnte in EF Core eine Eigenschaft durch einen Zeichenfolgenwert angegeben werden, und wenn keine Eigenschaft mit diesem Namen im .NET-Typ gefunden wurde, versuchte EF Core, mithilfe von Konventionsregeln ein übereinstimmendes Feld zu finden.</span><span class="sxs-lookup"><span data-stu-id="a5588-539">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>

```csharp
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

<span data-ttu-id="a5588-540">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-540">**New behavior**</span></span>

<span data-ttu-id="a5588-541">Ab Version 3.0 muss in EF Core eine „Nur-Feld“-Eigenschaft mit dem Namen des Felds genau übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="a5588-541">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="a5588-542">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-542">**Why**</span></span>

<span data-ttu-id="a5588-543">Diese Änderung wurde vorgenommen, um zu vermeiden, dass das gleiche Feld für zwei Eigenschaften mit ähnlichem Namen verwendet wird. Durch die Änderung entsprechen nun auch die Übereinstimmungsregeln für „Nur-Feld“-Eigenschaften den Regeln für Eigenschaften, die CLR-Eigenschaften zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="a5588-543">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="a5588-544">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-544">**Mitigations**</span></span>

<span data-ttu-id="a5588-545">„Nur-Feld“-Eigenschaften müssen so benannt werden wie das Feld, dem sie zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="a5588-545">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="a5588-546">In einer kommenden Version von EF Core nach 3.0 soll das explizite Konfigurieren eines Feldnamens, der sich vom Eigenschaftsnamen unterscheidet, erneut aktiviert werden (siehe Problem [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="a5588-546">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="a5588-547">Von AddDbContext/AddDbContextPool werden AddLogging und AddMemoryCache nicht mehr aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="a5588-547">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="a5588-548">Issue #14756</span><span class="sxs-lookup"><span data-stu-id="a5588-548">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="a5588-549">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-549">**Old behavior**</span></span>

<span data-ttu-id="a5588-550">Vor EF Core 3.0 wurden beim Aufruf von `AddDbContext` oder `AddDbContextPool` durch Aufrufe von [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) und [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache) auch Protokollierungs- und Arbeitsspeichercachingdienste bei DI registriert.</span><span class="sxs-lookup"><span data-stu-id="a5588-550">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with DI through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="a5588-551">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-551">**New behavior**</span></span>

<span data-ttu-id="a5588-552">Ab EF Core 3.0 werden diese Dienste durch `AddDbContext` und `AddDbContextPool` nicht mehr bei Dependency Injection (DI) registriert.</span><span class="sxs-lookup"><span data-stu-id="a5588-552">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="a5588-553">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-553">**Why**</span></span>

<span data-ttu-id="a5588-554">Für EF Core 3.0 ist es nicht erforderlich, dass diese Dienste im DI-Container der Anwendung enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="a5588-554">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="a5588-555">Wenn `ILoggerFactory` jedoch im DI-Container der Anwendung registriert ist, wird sie nach wie vor von EF Core verwendet.</span><span class="sxs-lookup"><span data-stu-id="a5588-555">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="a5588-556">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-556">**Mitigations**</span></span>

<span data-ttu-id="a5588-557">Wenn Ihre Anwendung diese Dienste benötigt, registrieren Sie sie explizit mit [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) oder [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache) beim DI-Container.</span><span class="sxs-lookup"><span data-stu-id="a5588-557">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

### <a name="addentityframework-adds-imemorycache-with-a-size-limit"></a><span data-ttu-id="a5588-558">AddEntityFramework\* fügt IMemoryCache mit einer Größenbeschränkung hinzu</span><span class="sxs-lookup"><span data-stu-id="a5588-558">AddEntityFramework\* adds IMemoryCache with a size limit</span></span>

[<span data-ttu-id="a5588-559">Issue 12905</span><span class="sxs-lookup"><span data-stu-id="a5588-559">Tracking Issue #12905</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12905)

<span data-ttu-id="a5588-560">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-560">**Old behavior**</span></span>

<span data-ttu-id="a5588-561">Vor EF Core 3.0 wurden bei Aufrufen von `AddEntityFramework*`-Methoden auch Arbeitsspeichercachingdienste ohne eine Größenbeschränkung bei DI registriert.</span><span class="sxs-lookup"><span data-stu-id="a5588-561">Before EF Core 3.0, calling `AddEntityFramework*` methods would also register memory caching services with DI without a size limit.</span></span>

<span data-ttu-id="a5588-562">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-562">**New behavior**</span></span>

<span data-ttu-id="a5588-563">Ab EF Core 3.0 registriert `AddEntityFramework*` einen IMemoryCache-Dienst mit einer Größenbeschränkung.</span><span class="sxs-lookup"><span data-stu-id="a5588-563">Starting with EF Core 3.0, `AddEntityFramework*` will register an IMemoryCache service with a size limit.</span></span> <span data-ttu-id="a5588-564">Wenn danach weitere Dienste hinzugefügt werden, die von IMemoryCache abhängig sind, können diese schnell den Standardgrenzwert erreichen, was zu Ausnahmen und Leistungsbeeinträchtigungen führt.</span><span class="sxs-lookup"><span data-stu-id="a5588-564">If any other services added afterwards depend on IMemoryCache they can quickly reach the default limit causing exceptions or degraded performance.</span></span>

<span data-ttu-id="a5588-565">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-565">**Why**</span></span>

<span data-ttu-id="a5588-566">Die Verwendung von IMemoryCache ohne Einschränkung kann zu unkontrollierter Arbeitsspeicherauslastung führen, wenn ein Fehler in die Abfragecachelogik auftritt oder wenn die Abfragen dynamisch generiert werden.</span><span class="sxs-lookup"><span data-stu-id="a5588-566">Using IMemoryCache without a limit could result in uncontrolled memory usage if there is a bug in query caching logic or the queries are generated dynamically.</span></span> <span data-ttu-id="a5588-567">Durch eine Standardeinschränkung können potenzielle DoS-Angriffe verhindert werden.</span><span class="sxs-lookup"><span data-stu-id="a5588-567">Having a default limit mitigates a potential DoS attack.</span></span>

<span data-ttu-id="a5588-568">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-568">**Mitigations**</span></span>

<span data-ttu-id="a5588-569">In den meisten Fällen ist das Aufrufen von `AddEntityFramework*` nicht erforderlich, wenn `AddDbContext` oder `AddDbContextPool` ebenfalls aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="a5588-569">In most cases calling `AddEntityFramework*` is not necessary if `AddDbContext` or `AddDbContextPool` is called as well.</span></span> <span data-ttu-id="a5588-570">Daher besteht die beste Entschärfung darin, den `AddEntityFramework*`-Aufruf zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="a5588-570">Therefore, the best mitigation is to remove the `AddEntityFramework*` call.</span></span>

<span data-ttu-id="a5588-571">Wenn Ihre Anwendung diese Dienste erfordert, registrieren Sie im Voraus eine IMemoryCache-Implementierung explizit mit dem DI-Container mithilfe von [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="a5588-571">If your application needs these services, then register a IMemoryCache implementation explicitly with the DI container beforehand using [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="a5588-572">DbContext.Entry erkennt nun mit DetectChanges lokale Änderungen</span><span class="sxs-lookup"><span data-stu-id="a5588-572">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="a5588-573">Issue #13552</span><span class="sxs-lookup"><span data-stu-id="a5588-573">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="a5588-574">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-574">**Old behavior**</span></span>

<span data-ttu-id="a5588-575">Vor Version 3.0 führte in EF Core ein Aufruf von `DbContext.Entry` dazu, dass Änderungen an allen überwachten Entitäten erkannt wurden.</span><span class="sxs-lookup"><span data-stu-id="a5588-575">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="a5588-576">Dadurch wurde sichergestellt, dass der Zustand in `EntityEntry` immer aktuell war.</span><span class="sxs-lookup"><span data-stu-id="a5588-576">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="a5588-577">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-577">**New behavior**</span></span>

<span data-ttu-id="a5588-578">Ab Version 3.0 werden in EF Core Änderungen bei einem Aufruf von `DbContext.Entry` nur in der angegebenen Entität und in allen überwachten Prinzipalentitäten erkannt.</span><span class="sxs-lookup"><span data-stu-id="a5588-578">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="a5588-579">Änderungen an anderer Stelle werden daher durch den Aufruf dieser Methode möglicherweise nicht erkannt, was sich auf den Anwendungszustand auswirken kann.</span><span class="sxs-lookup"><span data-stu-id="a5588-579">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="a5588-580">Beachten Sie, dass selbst die Erkennung lokaler Änderungen deaktiviert wird, wenn `ChangeTracker.AutoDetectChangesEnabled` auf `false` festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="a5588-580">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="a5588-581">Andere Methoden wie `ChangeTracker.Entries` und `SaveChanges`, die eine Änderungserkennung auslösen, führen nach wie vor zu einem vollständigen `DetectChanges`-Vorgang für alle überwachten Entitäten.</span><span class="sxs-lookup"><span data-stu-id="a5588-581">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="a5588-582">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-582">**Why**</span></span>

<span data-ttu-id="a5588-583">Diese Änderung wurde vorgenommen, um die Leistung bei der standardmäßigen Verwendung von `context.Entry` zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="a5588-583">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="a5588-584">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-584">**Mitigations**</span></span>

<span data-ttu-id="a5588-585">Rufen Sie `ChgangeTracker.DetectChanges()` explizit vor dem Aufruf von `Entry` auf, wenn das Verhalten vor Version 3.0 beibehalten werden soll.</span><span class="sxs-lookup"><span data-stu-id="a5588-585">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="a5588-586">Schlüssel aus Zeichenfolgen und Bytearrays werden standardmäßig nicht vom Client generiert</span><span class="sxs-lookup"><span data-stu-id="a5588-586">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="a5588-587">Issue #14617</span><span class="sxs-lookup"><span data-stu-id="a5588-587">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="a5588-588">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-588">**Old behavior**</span></span>

<span data-ttu-id="a5588-589">Vor Version 3.0 konnten in EF Core `string`- und `byte[]`-Schlüsseleigenschaften verwendet werden, ohne dass explizit ein anderer Wert als NULL festgelegt werden musste.</span><span class="sxs-lookup"><span data-stu-id="a5588-589">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="a5588-590">In diesem Fall wurde der Schlüsselwert auf dem Client als GUID generiert und für `byte[]` in Bytes serialisiert.</span><span class="sxs-lookup"><span data-stu-id="a5588-590">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="a5588-591">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-591">**New behavior**</span></span>

<span data-ttu-id="a5588-592">Ab Version 3.0 wird in EF Core eine Ausnahme ausgelöst, wenn kein Schlüsselwert festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="a5588-592">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="a5588-593">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-593">**Why**</span></span>

<span data-ttu-id="a5588-594">Diese Änderung wurde vorgenommen, da vom Client generierte `string`/`byte[]`-Werte in der Regel nicht sinnvoll sind. Das Standardverhalten führte außerdem zu kaum nachvollziehbaren generierten Schlüsselwerten.</span><span class="sxs-lookup"><span data-stu-id="a5588-594">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="a5588-595">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-595">**Mitigations**</span></span>

<span data-ttu-id="a5588-596">Wenn Sie das Verhalten vor Version 3.0 nutzen möchten, müssen Sie explizit festlegen, dass für die Schlüsseleigenschaften generierte Werte verwendet sollen, wenn kein anderer Nicht-NULL-Wert festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="a5588-596">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="a5588-597">Dies ist beispielsweise mit der Fluent-API möglich:</span><span class="sxs-lookup"><span data-stu-id="a5588-597">For example, with the fluent API:</span></span>

```csharp
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="a5588-598">Alternativ bieten sich auch Datenanmerkungen an:</span><span class="sxs-lookup"><span data-stu-id="a5588-598">Or with data annotations:</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="a5588-599">ILoggerFactory ist nun ein bereichsbezogener Dienst</span><span class="sxs-lookup"><span data-stu-id="a5588-599">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="a5588-600">Issue #14698</span><span class="sxs-lookup"><span data-stu-id="a5588-600">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="a5588-601">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-601">**Old behavior**</span></span>

<span data-ttu-id="a5588-602">Vor Version 3.0 wurde `ILoggerFactory` in EF Core als Singletondienst registriert.</span><span class="sxs-lookup"><span data-stu-id="a5588-602">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="a5588-603">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-603">**New behavior**</span></span>

<span data-ttu-id="a5588-604">Ab Version 3.0 wird in EF Core `ILoggerFactory` als bereichsbezogener Dienst registriert.</span><span class="sxs-lookup"><span data-stu-id="a5588-604">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="a5588-605">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-605">**Why**</span></span>

<span data-ttu-id="a5588-606">Diese Änderung wurde vorgenommen, um einer `DbContext`-Instanz eine Protokollierung zuordnen zu können. Dadurch werden weitere Funktionen bereitgestellt. In einigen Fällen wird außerdem schädliches Verhalten wie etwa die schnelle Zunahme von internen Dienstanbietern beseitigt.</span><span class="sxs-lookup"><span data-stu-id="a5588-606">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="a5588-607">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-607">**Mitigations**</span></span>

<span data-ttu-id="a5588-608">Diese Änderung wirkt sich nur dann auf den Anwendungscode aus, wenn benutzerdefinierte Dienste auf dem internen Dienstanbieter von EF Core registriert und verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="a5588-608">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="a5588-609">Ein solches Szenario ist aber unüblich.</span><span class="sxs-lookup"><span data-stu-id="a5588-609">This isn't common.</span></span>
<span data-ttu-id="a5588-610">In einem derartigen Fall funktioniert fast alles wie vorher. Singletondienste, die von `ILoggerFactory` abhängig sind, müssen jedoch so angepasst werden, dass `ILoggerFactory` auf andere Weise abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="a5588-610">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="a5588-611">Erstellen Sie in diesen Situationen bitte im [GitHub-Issuetracker für EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) ein Issue, in dem Sie beschreiben, wie Sie `ILoggerFactory` verwenden. So können wir in Zukunft erneute Breaking Changes vermeiden.</span><span class="sxs-lookup"><span data-stu-id="a5588-611">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="a5588-612">Für Proxys mit Lazy Loading (trägem Laden) gilt nicht mehr die Annahme, dass Navigationseigenschaften vollständig geladen sind</span><span class="sxs-lookup"><span data-stu-id="a5588-612">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="a5588-613">Issue #12780</span><span class="sxs-lookup"><span data-stu-id="a5588-613">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="a5588-614">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-614">**Old behavior**</span></span>

<span data-ttu-id="a5588-615">Vor Version 3.0 gab es in EF Core nach der Freigabe eines `DbContext`-Objekts keine Möglichkeit, zu ermitteln, ob eine aus dem Kontext abgerufene Navigationseigenschaft für eine Entität vollständig geladen war.</span><span class="sxs-lookup"><span data-stu-id="a5588-615">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="a5588-616">Stattdessen galt für Proxys die Annahme, dass eine Verweisnavigation geladen ist, falls für diese ein anderer Wert als NULL vorliegt. Außerdem wurde davon ausgegangen, dass eine Sammlungsnavigation geladen ist, wenn sie nicht leer ist.</span><span class="sxs-lookup"><span data-stu-id="a5588-616">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="a5588-617">In diesen Fällen entsprach das Lazy Loading einer Nulloperation („No-Op“).</span><span class="sxs-lookup"><span data-stu-id="a5588-617">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="a5588-618">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-618">**New behavior**</span></span>

<span data-ttu-id="a5588-619">Ab Version 3.0 überwachen in EF Core Proxys, ob eine Navigationseigenschaft geladen ist.</span><span class="sxs-lookup"><span data-stu-id="a5588-619">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="a5588-620">Wenn also auf eine Navigationseigenschaft zugegriffen werden soll, die nach der Freigabe des Kontexts geladen wird, ist dies immer eine No-Op-Operation. Das ist selbst dann der Fall, wenn die geladene Navigation leer oder NULL ist.</span><span class="sxs-lookup"><span data-stu-id="a5588-620">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="a5588-621">Wenn umgekehrt auf eine nicht geladene Navigationseigenschaft zugegriffen werden soll und der Kontext bereits freigegeben wurde, wird eine Ausnahme ausgelöst. Dieser Fall tritt auch dann ein, wenn die Navigationseigenschaft eine nicht leere Sammlung ist.</span><span class="sxs-lookup"><span data-stu-id="a5588-621">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="a5588-622">In dieser Situation wird also vom Anwendungscode versucht, zu einem ungültigen Zeitpunkt Lazy Loading durchzuführen. Die Anwendung sollte so angepasst werden, dass dieser Vorgang vermieden wird.</span><span class="sxs-lookup"><span data-stu-id="a5588-622">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="a5588-623">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-623">**Why**</span></span>

<span data-ttu-id="a5588-624">Diese Änderung wurde vorgenommen, um das Verhalten beim Lazy Loading einer freigegebenen `DbContext`-Instanz konsistent und korrekt zu gestalten.</span><span class="sxs-lookup"><span data-stu-id="a5588-624">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="a5588-625">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-625">**Mitigations**</span></span>

<span data-ttu-id="a5588-626">Aktualisieren Sie den Anwendungscode so, dass kein Lazy Loading durchgeführt wird, nachdem der Kontext freigegeben wurde. Alternativ können Sie auch, wie in der Ausnahmemeldung beschrieben wird, eine No-Op-Operation festlegen.</span><span class="sxs-lookup"><span data-stu-id="a5588-626">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="a5588-627">Die Erstellung zu vieler interner Dienstanbieter führt standardmäßig zu einem Fehler</span><span class="sxs-lookup"><span data-stu-id="a5588-627">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="a5588-628">Issue #10236</span><span class="sxs-lookup"><span data-stu-id="a5588-628">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="a5588-629">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-629">**Old behavior**</span></span>

<span data-ttu-id="a5588-630">Vor Version 3.0 wurde in EF Core eine Warnung für eine Anwendung protokolliert, die unverhältnismäßig viele interne Dienstanbieter erstellte.</span><span class="sxs-lookup"><span data-stu-id="a5588-630">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="a5588-631">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-631">**New behavior**</span></span>

<span data-ttu-id="a5588-632">Ab Version 3.0 wird diese Warnung in EF Core als Fehler betrachtet, und eine Ausnahme wird ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="a5588-632">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="a5588-633">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-633">**Why**</span></span>

<span data-ttu-id="a5588-634">Diese Änderung wurde vorgenommen, damit auf den oben beschriebenen Fall explizit hingewiesen wird. So soll die Entwicklung von besserem Anwendungscode erleichtert werden.</span><span class="sxs-lookup"><span data-stu-id="a5588-634">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="a5588-635">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-635">**Mitigations**</span></span>

<span data-ttu-id="a5588-636">Wenn dieser Fehler auftritt, müssen Sie die Grundursache ermitteln und die Erstellung vieler interner Dienstanbieter unterbinden.</span><span class="sxs-lookup"><span data-stu-id="a5588-636">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="a5588-637">Sie können den Fehler allerdings auch wieder in eine Warnung konvertierten (oder ihn ignorieren), indem Sie `DbContextOptionsBuilder` konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a5588-637">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="a5588-638">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a5588-638">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="a5588-639">Neues Verhalten für HasOne/HasMany bei Aufruf mit einer einzelnen Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="a5588-639">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="a5588-640">Issue #9171</span><span class="sxs-lookup"><span data-stu-id="a5588-640">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="a5588-641">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-641">**Old behavior**</span></span>

<span data-ttu-id="a5588-642">Vor EF Core 3.0 wurde Code, der `HasOne` oder `HasMany` mit einer einzelnen Zeichenfolge aufruft, auf irritierende Weise interpretiert.</span><span class="sxs-lookup"><span data-stu-id="a5588-642">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="a5588-643">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a5588-643">For example:</span></span>
```csharp
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="a5588-644">Der Code scheint `Samurai` mit einem anderen Entitätstyp über die `Entrance`-Navigationseigenschaft zu verknüpfen, die möglicherweise privat ist.</span><span class="sxs-lookup"><span data-stu-id="a5588-644">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="a5588-645">Tatsächlich versucht dieser Code, eine Beziehung zu einem Entitätstyp namens `Entrance` ohne Navigationseigenschaft herzustellen.</span><span class="sxs-lookup"><span data-stu-id="a5588-645">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="a5588-646">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-646">**New behavior**</span></span>

<span data-ttu-id="a5588-647">Ab EF Core 3.0 führt der obige Code jetzt die erwartete Aktion durch.</span><span class="sxs-lookup"><span data-stu-id="a5588-647">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="a5588-648">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-648">**Why**</span></span>

<span data-ttu-id="a5588-649">Das alte Verhalten war sehr verwirrend, vor allem beim Lesen des Konfigurationscodes und bei der Suche nach Fehlern.</span><span class="sxs-lookup"><span data-stu-id="a5588-649">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="a5588-650">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-650">**Mitigations**</span></span>

<span data-ttu-id="a5588-651">Dies führt nur zu Fehlern in Anwendungen, die Beziehungen explizit unter Verwendung von Zeichenfolgen für Typnamen und ohne explizite Angabe der Navigationseigenschaft konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a5588-651">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="a5588-652">Dies ist nicht üblich.</span><span class="sxs-lookup"><span data-stu-id="a5588-652">This is not common.</span></span>
<span data-ttu-id="a5588-653">Das vorherige Verhalten kann durch explizite Übergabe von `null` für den Namen der Navigationseigenschaft erzielt werden.</span><span class="sxs-lookup"><span data-stu-id="a5588-653">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="a5588-654">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a5588-654">For example:</span></span>

```csharp
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="a5588-655">Der Rückgabetyp für mehrere asynchrone Methoden wurde von Task in ValueTask geändert</span><span class="sxs-lookup"><span data-stu-id="a5588-655">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="a5588-656">Issue #15184</span><span class="sxs-lookup"><span data-stu-id="a5588-656">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="a5588-657">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-657">**Old behavior**</span></span>

<span data-ttu-id="a5588-658">Die folgenden asynchronen Methoden gaben zuvor `Task<T>` zurück:</span><span class="sxs-lookup"><span data-stu-id="a5588-658">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="a5588-659">`ValueGenerator.NextValueAsync()` (und abgeleitete Klassen)</span><span class="sxs-lookup"><span data-stu-id="a5588-659">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="a5588-660">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-660">**New behavior**</span></span>

<span data-ttu-id="a5588-661">Die oben genannten Methoden geben nun `ValueTask<T>` über dieselbe `T` wie zuvor zurück.</span><span class="sxs-lookup"><span data-stu-id="a5588-661">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="a5588-662">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-662">**Why**</span></span>

<span data-ttu-id="a5588-663">Durch diese Änderung verringert sich die Anzahl der Heapzuordnungen, die beim Aufrufen dieser Methoden entstehen, und dies führt zu einer Verbesserung der allgemeinen Leistung.</span><span class="sxs-lookup"><span data-stu-id="a5588-663">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="a5588-664">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-664">**Mitigations**</span></span>

<span data-ttu-id="a5588-665">Anwendungen, die einfach die oben genannten APIs erwarten, müssen nur neu kompiliert werden – Quelländerungen sind nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="a5588-665">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="a5588-666">Eine komplexere Verwendung (z.B. Übergeben der zurückgegebenen `Task` an `Task.WhenAny()`) erfordern in der Regel, dass die zurückgegebene `ValueTask<T>` durch einen Aufruf von `AsTask()` in `Task<T>` konvertiert wird.</span><span class="sxs-lookup"><span data-stu-id="a5588-666">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="a5588-667">Beachten Sie, dass dadurch die mit dieser Änderung verbundene Verringerung der Zuordnungen aufgehoben wird.</span><span class="sxs-lookup"><span data-stu-id="a5588-667">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="a5588-668">Die Anmerkung Relational:TypeMapping heißt nun nur noch TypeMapping</span><span class="sxs-lookup"><span data-stu-id="a5588-668">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="a5588-669">Issue #9913</span><span class="sxs-lookup"><span data-stu-id="a5588-669">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="a5588-670">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-670">**Old behavior**</span></span>

<span data-ttu-id="a5588-671">Der Anmerkungsname für Typzuordnungsanmerkungen war bisher Relational:TypeMapping.</span><span class="sxs-lookup"><span data-stu-id="a5588-671">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="a5588-672">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-672">**New behavior**</span></span>

<span data-ttu-id="a5588-673">Der Anmerkungsname für Typzuordnungsanmerkungen lautet nun TypeMapping.</span><span class="sxs-lookup"><span data-stu-id="a5588-673">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="a5588-674">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-674">**Why**</span></span>

<span data-ttu-id="a5588-675">Typzuordnungen werden nicht mehr ausschließlich für Anbieter relationaler Datenbanken verwendet.</span><span class="sxs-lookup"><span data-stu-id="a5588-675">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="a5588-676">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-676">**Mitigations**</span></span>

<span data-ttu-id="a5588-677">Diese Änderung ist nur dann ein Breaking Change, wenn Anwendungen direkt über eine Anmerkung auf die Typzuordnung zugreifen. Dieses Szenario ist jedoch unüblich.</span><span class="sxs-lookup"><span data-stu-id="a5588-677">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="a5588-678">Verzichten Sie daher darauf, die Anmerkung direkt zu verwenden, und greifen Sie stattdessen über die Fluent-API auf Typzuordnungen zu, um das Problem zu beheben.</span><span class="sxs-lookup"><span data-stu-id="a5588-678">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="a5588-679">ToTable löst für einen abgeleiteten Typ eine Ausnahme aus</span><span class="sxs-lookup"><span data-stu-id="a5588-679">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="a5588-680">Issue #11811</span><span class="sxs-lookup"><span data-stu-id="a5588-680">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="a5588-681">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-681">**Old behavior**</span></span>

<span data-ttu-id="a5588-682">Vor Version 3.0 wurde in EF Core der Aufruf von `ToTable()` für einen abgeleiteten Typ ignoriert, da TPH die einzige Strategie zur Vererbungszuordnung war und diese in unzulässigen Fällen angewendet wurde.</span><span class="sxs-lookup"><span data-stu-id="a5588-682">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="a5588-683">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-683">**New behavior**</span></span>

<span data-ttu-id="a5588-684">Ab Version 3.0 wird in EF Core eine Ausnahme ausgelöst, um später eine unerwartete Zuordnung zu vermeiden, wenn `ToTable()` für einen abgeleiteten Typ aufgerufen wird. Dieser Schritt dient auch als Vorbereitung für die TPT- und TPC-Unterstützung, die in einem künftigen Release folgen wird.</span><span class="sxs-lookup"><span data-stu-id="a5588-684">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="a5588-685">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-685">**Why**</span></span>

<span data-ttu-id="a5588-686">Derzeit ist es nicht zulässig, einen abgeleiteten Typ einer anderen Tabelle zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="a5588-686">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="a5588-687">Durch diese Änderung werden Breaking Changes vermieden, wenn der beschriebene Vorgang in Zukunft zulässig wird.</span><span class="sxs-lookup"><span data-stu-id="a5588-687">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="a5588-688">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-688">**Mitigations**</span></span>

<span data-ttu-id="a5588-689">Entfernen Sie alle Zuordnungen von abgeleiteten Typen zu anderen Tabellen.</span><span class="sxs-lookup"><span data-stu-id="a5588-689">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="a5588-690">ForSqlServerHasIndex wurde durch HasIndex ersetzt</span><span class="sxs-lookup"><span data-stu-id="a5588-690">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="a5588-691">Issue #12366</span><span class="sxs-lookup"><span data-stu-id="a5588-691">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="a5588-692">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-692">**Old behavior**</span></span>

<span data-ttu-id="a5588-693">Vor Version 3.0 konnten in EF Core mit `ForSqlServerHasIndex().ForSqlServerInclude()` Spalten konfiguriert werden, die mit `INCLUDE` verwendet wurden.</span><span class="sxs-lookup"><span data-stu-id="a5588-693">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="a5588-694">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-694">**New behavior**</span></span>

<span data-ttu-id="a5588-695">Ab Version 3.0 wird in EF Core die Nutzung von `Include` für einen Index auf der relationalen Ebene unterstützt.</span><span class="sxs-lookup"><span data-stu-id="a5588-695">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="a5588-696">Verwenden Sie `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="a5588-696">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="a5588-697">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-697">**Why**</span></span>

<span data-ttu-id="a5588-698">Diese Änderung wurde vorgenommen, um die API für Indizes mit `Include` an einer zentralen Stelle für alle Datenbankanbieter zusammenzuführen.</span><span class="sxs-lookup"><span data-stu-id="a5588-698">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="a5588-699">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-699">**Mitigations**</span></span>

<span data-ttu-id="a5588-700">Verwenden Sie wie oben beschrieben die neue API.</span><span class="sxs-lookup"><span data-stu-id="a5588-700">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="a5588-701">Metadaten-API-Änderungen</span><span class="sxs-lookup"><span data-stu-id="a5588-701">Metadata API changes</span></span>

[<span data-ttu-id="a5588-702">Issue #214</span><span class="sxs-lookup"><span data-stu-id="a5588-702">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="a5588-703">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-703">**New behavior**</span></span>

<span data-ttu-id="a5588-704">Die folgenden Eigenschaften wurden in Erweiterungsmethoden konvertiert:</span><span class="sxs-lookup"><span data-stu-id="a5588-704">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="a5588-705">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-705">**Why**</span></span>

<span data-ttu-id="a5588-706">Mit dieser Änderung wird die Implementierung der oben genannten Schnittstellen vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="a5588-706">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="a5588-707">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-707">**Mitigations**</span></span>

<span data-ttu-id="a5588-708">Verwenden Sie die neuen Erweiterungsmethoden.</span><span class="sxs-lookup"><span data-stu-id="a5588-708">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="a5588-709">Anbieterspezifische Metadaten-API-Änderungen</span><span class="sxs-lookup"><span data-stu-id="a5588-709">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="a5588-710">Issue #214</span><span class="sxs-lookup"><span data-stu-id="a5588-710">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="a5588-711">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-711">**New behavior**</span></span>

<span data-ttu-id="a5588-712">Die anbieterspezifischen Erweiterungsmethoden werden vereinfacht:</span><span class="sxs-lookup"><span data-stu-id="a5588-712">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="a5588-713">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-713">**Why**</span></span>

<span data-ttu-id="a5588-714">Mit dieser Änderung wird die Implementierung der oben genannten Erweiterungsmethoden vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="a5588-714">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="a5588-715">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-715">**Mitigations**</span></span>

<span data-ttu-id="a5588-716">Verwenden Sie die neuen Erweiterungsmethoden.</span><span class="sxs-lookup"><span data-stu-id="a5588-716">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="a5588-717">EF Core sendet keine PRAGMA-Anweisungen mehr, um Fremdschlüssel in SQLite zu erzwingen</span><span class="sxs-lookup"><span data-stu-id="a5588-717">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="a5588-718">Issue #12151</span><span class="sxs-lookup"><span data-stu-id="a5588-718">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="a5588-719">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-719">**Old behavior**</span></span>

<span data-ttu-id="a5588-720">Vor Version 3.0 wurden in EF Core `PRAGMA foreign_keys = 1`-Anweisungen gesendet, wenn eine Verbindung mit SQLite hergestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="a5588-720">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="a5588-721">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-721">**New behavior**</span></span>

<span data-ttu-id="a5588-722">Ab Version 3.0 werden in EF Core keine `PRAGMA foreign_keys = 1`-Anweisungen mehr gesendet, wenn eine Verbindung mit SQLite hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="a5588-722">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="a5588-723">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-723">**Why**</span></span>

<span data-ttu-id="a5588-724">Diese Änderung wurde vorgenommen, da von EF Core standardmäßig `SQLitePCLRaw.bundle_e_sqlite3` verwendet wird. Dadurch ist die Erzwingung von Fremdschlüsseln standardmäßig aktiviert und muss nicht jedes Mal explizit aktiviert werden, wenn eine Verbindung hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="a5588-724">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="a5588-725">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-725">**Mitigations**</span></span>

<span data-ttu-id="a5588-726">Fremdschlüssel sind in SQLitePCLRaw.bundle_e_sqlite3 standardmäßig aktiviert. Dieses Bundle wird standardmäßig von EF Core verwendet.</span><span class="sxs-lookup"><span data-stu-id="a5588-726">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="a5588-727">In anderen Fällen können Fremdschlüssel durch die Angabe `Foreign Keys=True` in der Verbindungszeichenfolge aktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="a5588-727">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="a5588-728">SQLitePCLRaw.bundle_e_sqlite3 ist nun eine Abhängigkeit von Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="a5588-728">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="a5588-729">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-729">**Old behavior**</span></span>

<span data-ttu-id="a5588-730">Vor Version 3.0 wurde von EF Core `SQLitePCLRaw.bundle_green` verwendet.</span><span class="sxs-lookup"><span data-stu-id="a5588-730">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="a5588-731">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-731">**New behavior**</span></span>

<span data-ttu-id="a5588-732">Ab Version 3.0 wird von EF Core `SQLitePCLRaw.bundle_e_sqlite3` verwendet.</span><span class="sxs-lookup"><span data-stu-id="a5588-732">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="a5588-733">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-733">**Why**</span></span>

<span data-ttu-id="a5588-734">Diese Änderung wurde vorgenommen, damit die unter iOS verwendete Version von SQLite sich konsistent zu Versionen auf anderen Plattformen verhält.</span><span class="sxs-lookup"><span data-stu-id="a5588-734">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="a5588-735">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-735">**Mitigations**</span></span>

<span data-ttu-id="a5588-736">Konfigurieren Sie `Microsoft.Data.Sqlite` so, dass ein anderes `SQLitePCLRaw`-Bundle verwendet wird, um die native SQLite-Version unter iOS zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="a5588-736">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="a5588-737">GUID-Werte werden jetzt als TEXT in SQLite gespeichert</span><span class="sxs-lookup"><span data-stu-id="a5588-737">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="a5588-738">Issue #15078</span><span class="sxs-lookup"><span data-stu-id="a5588-738">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="a5588-739">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-739">**Old behavior**</span></span>

<span data-ttu-id="a5588-740">GUID-Werte wurden in der Vergangenheit in SQLite als BLOB-Werte gespeichert.</span><span class="sxs-lookup"><span data-stu-id="a5588-740">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="a5588-741">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-741">**New behavior**</span></span>

<span data-ttu-id="a5588-742">GUID-Werte werden jetzt als TEXT gespeichert.</span><span class="sxs-lookup"><span data-stu-id="a5588-742">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="a5588-743">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-743">**Why**</span></span>

<span data-ttu-id="a5588-744">Das Binärformat der GUIDs ist nicht standardisiert.</span><span class="sxs-lookup"><span data-stu-id="a5588-744">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="a5588-745">Das Speichern der Werte als TEXT führt dazu, dass die Datenbank mit anderen Technologien eher kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="a5588-745">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="a5588-746">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-746">**Mitigations**</span></span>

<span data-ttu-id="a5588-747">Sie können vorhandene Datenbanken zum neuen Format migrieren, indem Sie SQL-Code wie den folgenden ausführen:</span><span class="sxs-lookup"><span data-stu-id="a5588-747">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET GuidColumn = hex(substr(GuidColumn, 4, 1)) ||
                 hex(substr(GuidColumn, 3, 1)) ||
                 hex(substr(GuidColumn, 2, 1)) ||
                 hex(substr(GuidColumn, 1, 1)) || '-' ||
                 hex(substr(GuidColumn, 6, 1)) ||
                 hex(substr(GuidColumn, 5, 1)) || '-' ||
                 hex(substr(GuidColumn, 8, 1)) ||
                 hex(substr(GuidColumn, 7, 1)) || '-' ||
                 hex(substr(GuidColumn, 9, 2)) || '-' ||
                 hex(substr(GuidColumn, 11, 6))
WHERE typeof(GuidColumn) == 'blob';
```

<span data-ttu-id="a5588-748">Sie können in EF Core auch weiterhin das alte Verhalten verwenden, indem Sie einen Wertkonverter für diese Eigenschaften konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a5588-748">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="a5588-749">Microsoft.Data.Sqlite kann weiterhin GUID-Werte aus BLOB- und TEXT-Spalten lesen. Da sich jedoch das Standardformat für Parameter und Konstanten geändert hat, müssen Sie wahrscheinlich Aktionen für die meisten GUID-Szenarios vornehmen.</span><span class="sxs-lookup"><span data-stu-id="a5588-749">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="a5588-750">Char-Werte werden jetzt als TEXT in SQLite gespeichert</span><span class="sxs-lookup"><span data-stu-id="a5588-750">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="a5588-751">Issue #15020</span><span class="sxs-lookup"><span data-stu-id="a5588-751">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="a5588-752">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-752">**Old behavior**</span></span>

<span data-ttu-id="a5588-753">Char-Werte wurden in der Vergangenheit in SQLite als INTEGER-Werte gespeichert.</span><span class="sxs-lookup"><span data-stu-id="a5588-753">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="a5588-754">Der Char-Wert *A* wurde z.B. als INTEGER-Wert 65 gespeichert.</span><span class="sxs-lookup"><span data-stu-id="a5588-754">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="a5588-755">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-755">**New behavior**</span></span>

<span data-ttu-id="a5588-756">Char-Werte werden jetzt als TEXT gespeichert.</span><span class="sxs-lookup"><span data-stu-id="a5588-756">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="a5588-757">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-757">**Why**</span></span>

<span data-ttu-id="a5588-758">Das Speichern der Werte als TEXT ist naheliegender und führt dazu, dass die Datenbank mit anderen Technologien eher kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="a5588-758">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="a5588-759">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-759">**Mitigations**</span></span>

<span data-ttu-id="a5588-760">Sie können vorhandene Datenbanken zum neuen Format migrieren, indem Sie SQL-Code wie den folgenden ausführen:</span><span class="sxs-lookup"><span data-stu-id="a5588-760">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="a5588-761">Sie können in EF Core auch weiterhin das alte Verhalten verwenden, indem Sie einen Wertkonverter für diese Eigenschaften konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a5588-761">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="a5588-762">Microsoft.Data.Sqlite kann auch weiterhin Zeichenwerte aus INTEGER- und TEXT-Spalten lesen, weshalb es sein kann, dass in einigen Szenarios keine weiteren Maßnahmen erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="a5588-762">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="a5588-763">Migrations-IDs werden jetzt mit dem Kalender der invarianten Kultur generiert</span><span class="sxs-lookup"><span data-stu-id="a5588-763">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="a5588-764">Issue #12978</span><span class="sxs-lookup"><span data-stu-id="a5588-764">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="a5588-765">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-765">**Old behavior**</span></span>

<span data-ttu-id="a5588-766">Migrations-IDs wurden versehentlich mit dem Kalender der aktuellen Kultur generiert.</span><span class="sxs-lookup"><span data-stu-id="a5588-766">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="a5588-767">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-767">**New behavior**</span></span>

<span data-ttu-id="a5588-768">Migrations-IDs werden jetzt immer mit dem Kalender der invarianten Kultur generiert (gregorianischer Kalender).</span><span class="sxs-lookup"><span data-stu-id="a5588-768">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="a5588-769">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-769">**Why**</span></span>

<span data-ttu-id="a5588-770">Die Reihenfolge der Migrationen ist beim Aktualisieren einer Datenbank oder Auflösen von Mergekonflikten wesentlich.</span><span class="sxs-lookup"><span data-stu-id="a5588-770">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="a5588-771">Wenn der invariante Kalender verwendet wird, werden Probleme bei der Reihenfolge vermieden, die entstehen, wenn Teammitglieder unterschiedliche Systemkalender verwenden.</span><span class="sxs-lookup"><span data-stu-id="a5588-771">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="a5588-772">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-772">**Mitigations**</span></span>

<span data-ttu-id="a5588-773">Diese Änderung betrifft jeden Benutzer, der einen Kalender verwendet, der nicht gregorianisch ist und bei dem die Jahreszahl höher als die des gregorianischen Kalenders ist (wie z.B. in der buddhistischen Zeitrechnung).</span><span class="sxs-lookup"><span data-stu-id="a5588-773">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="a5588-774">Vorhandene Migrations-IDs müssen aktualisiert werden, damit neue Migrationen in der Reihenfolge hinter vorhandenen Migrationen eingeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="a5588-774">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="a5588-775">Sie können die Migrations-ID im Migration-Attribut in der Designerdatei der Migration finden.</span><span class="sxs-lookup"><span data-stu-id="a5588-775">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="a5588-776">Die Tabelle mit dem Migrationsverlauf muss auch aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="a5588-776">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="a5588-777">UseRowNumberForPaging wurde entfernt.</span><span class="sxs-lookup"><span data-stu-id="a5588-777">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="a5588-778">Issue #16400</span><span class="sxs-lookup"><span data-stu-id="a5588-778">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="a5588-779">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-779">**Old behavior**</span></span>

<span data-ttu-id="a5588-780">Vor EF Core 3.0 konnte `UseRowNumberForPaging` zum Generieren von SQL-Code für die Paginierung verwendet werden, der mit SQL Server 2008 kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="a5588-780">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="a5588-781">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-781">**New behavior**</span></span>

<span data-ttu-id="a5588-782">Ab EF Core 3.0 generiert EF nur noch SQL-Code für die Paginierung, die mit höheren SQL Server-Versionen kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="a5588-782">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="a5588-783">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-783">**Why**</span></span>

<span data-ttu-id="a5588-784">Wir führen diese Änderung ein, weil [SQL Server 2008 kein unterstütztes Produkt mehr ist](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) und weil die Aktualisierung dieses Features auf die in EF Core 3.0 vorgenommenen Änderungen an Abfragen ein wichtiger Schritt ist.</span><span class="sxs-lookup"><span data-stu-id="a5588-784">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="a5588-785">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-785">**Mitigations**</span></span>

<span data-ttu-id="a5588-786">Es empfiehlt sich, eine Update auf eine neuere Version von SQL Server durchzuführen oder einen höheren Kompatibilitätsgrad zu verwenden, damit der generierte SQL-Code unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="a5588-786">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="a5588-787">Wenn dies bei Ihnen nicht möglich ist, fügen Sie einen [Kommentar zum Issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) ein, und geben Sie alle relevanten Informationen an.</span><span class="sxs-lookup"><span data-stu-id="a5588-787">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="a5588-788">Je nach Feedback der Benutzer wird diese Entscheidung möglicherweise noch einmal überprüft.</span><span class="sxs-lookup"><span data-stu-id="a5588-788">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="a5588-789">Erweiterungsinformationen und Metadaten aus IDbContextOptionsExtension entfernt</span><span class="sxs-lookup"><span data-stu-id="a5588-789">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="a5588-790">Issue #16119</span><span class="sxs-lookup"><span data-stu-id="a5588-790">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="a5588-791">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-791">**Old behavior**</span></span>

<span data-ttu-id="a5588-792">`IDbContextOptionsExtension` enthielt Methoden zum Bereitstellen von Metadaten zur Erweiterung.</span><span class="sxs-lookup"><span data-stu-id="a5588-792">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="a5588-793">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-793">**New behavior**</span></span>

<span data-ttu-id="a5588-794">Diese Methoden wurden in eine neue abstrakte `DbContextOptionsExtensionInfo`-Basisklasse verschoben, die von einer neuen `IDbContextOptionsExtension.Info`-Eigenschaft zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="a5588-794">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="a5588-795">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-795">**Why**</span></span>

<span data-ttu-id="a5588-796">In den Releases zwischen 2.0 und 3.0 mussten wir diese Methoden mehrmals ergänzen oder ändern.</span><span class="sxs-lookup"><span data-stu-id="a5588-796">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="a5588-797">Durch die Bereitstellung dieser Methoden in einer neuen abstrakten Basisklasse lassen sich solche Änderungen einfacher einführen, ohne dass es zu Konflikten mit vorhandenen Erweiterungen kommt.</span><span class="sxs-lookup"><span data-stu-id="a5588-797">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="a5588-798">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-798">**Mitigations**</span></span>

<span data-ttu-id="a5588-799">Erweiterungen wurden aktualisiert und verwenden das neue Muster.</span><span class="sxs-lookup"><span data-stu-id="a5588-799">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="a5588-800">Beispiele finden sich in den vielen Implementierungen von `IDbContextOptionsExtension` für verschiedene Arten von Erweiterungen im EF Core-Quellcode.</span><span class="sxs-lookup"><span data-stu-id="a5588-800">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="a5588-801">LogQueryPossibleExceptionWithAggregateOperator wurde umbenannt</span><span class="sxs-lookup"><span data-stu-id="a5588-801">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="a5588-802">Issue #10985</span><span class="sxs-lookup"><span data-stu-id="a5588-802">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="a5588-803">**Änderung**</span><span class="sxs-lookup"><span data-stu-id="a5588-803">**Change**</span></span>

<span data-ttu-id="a5588-804">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` wurde umbenannt in `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="a5588-804">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="a5588-805">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-805">**Why**</span></span>

<span data-ttu-id="a5588-806">Die Benennung dieses Warnereignisses wurde an allen anderen Warnereignissen ausgerichtet.</span><span class="sxs-lookup"><span data-stu-id="a5588-806">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="a5588-807">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-807">**Mitigations**</span></span>

<span data-ttu-id="a5588-808">Verwenden Sie den neuen Namen.</span><span class="sxs-lookup"><span data-stu-id="a5588-808">Use the new name.</span></span> <span data-ttu-id="a5588-809">(Beachten Sie, dass sich die Ereignis-ID-Nummer nicht geändert hat.)</span><span class="sxs-lookup"><span data-stu-id="a5588-809">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="a5588-810">Verdeutlichen der API für Einschränkungsnamen von Fremdschlüsseln</span><span class="sxs-lookup"><span data-stu-id="a5588-810">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="a5588-811">Issue #10730</span><span class="sxs-lookup"><span data-stu-id="a5588-811">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="a5588-812">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-812">**Old behavior**</span></span>

<span data-ttu-id="a5588-813">Vor EF Core 3.0 wurden Einschränkungsnamen von Fremdschlüsseln einfach als „Namen“ bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="a5588-813">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="a5588-814">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a5588-814">For example:</span></span>

```csharp
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="a5588-815">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-815">**New behavior**</span></span>

<span data-ttu-id="a5588-816">Ab EF Core 3.0 werden Einschränkungsnamen von Fremdschlüsseln nun als „Einschränkungsnamen“ bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="a5588-816">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="a5588-817">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a5588-817">For example:</span></span>

```csharp
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="a5588-818">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-818">**Why**</span></span>

<span data-ttu-id="a5588-819">Durch diese Änderung wird Konsistenz für Benennungen in diesem Bereich gewährleistet, und es wird verdeutlicht, dass es sich dabei um den Namen der Fremdschlüsseleinschränkung handelt und nicht um den Spalten- oder Eigenschaftennamen, auf deren Grundlage der Fremdschlüssel definiert ist.</span><span class="sxs-lookup"><span data-stu-id="a5588-819">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="a5588-820">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-820">**Mitigations**</span></span>

<span data-ttu-id="a5588-821">Verwenden Sie den neuen Namen.</span><span class="sxs-lookup"><span data-stu-id="a5588-821">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="a5588-822">„IRelationalDatabaseCreator.HasTables/HasTablesAsync“ ist jetzt öffentlich</span><span class="sxs-lookup"><span data-stu-id="a5588-822">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="a5588-823">Issue #15997</span><span class="sxs-lookup"><span data-stu-id="a5588-823">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="a5588-824">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-824">**Old behavior**</span></span>

<span data-ttu-id="a5588-825">Vor EF Core 3.0 waren diese Methoden geschützt.</span><span class="sxs-lookup"><span data-stu-id="a5588-825">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="a5588-826">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-826">**New behavior**</span></span>

<span data-ttu-id="a5588-827">Ab EF Core 3.0 sind diese Methoden öffentlich.</span><span class="sxs-lookup"><span data-stu-id="a5588-827">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="a5588-828">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-828">**Why**</span></span>

<span data-ttu-id="a5588-829">Diese Methoden werden von EF verwendet, um zu ermitteln, ob eine Datenbank erstellt wurde, aber leer ist.</span><span class="sxs-lookup"><span data-stu-id="a5588-829">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="a5588-830">Dies kann auch außerhalb von EF nützlich sein, um zu ermitteln, ob Migrationen angewendet werden sollen oder nicht.</span><span class="sxs-lookup"><span data-stu-id="a5588-830">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="a5588-831">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-831">**Mitigations**</span></span>

<span data-ttu-id="a5588-832">Der Zugriff auf Überschreibungen wurde geändert.</span><span class="sxs-lookup"><span data-stu-id="a5588-832">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="a5588-833">Microsoft.EntityFrameworkCore.Design ist jetzt ein DevelopmentDependency-Paket</span><span class="sxs-lookup"><span data-stu-id="a5588-833">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="a5588-834">Issue #11506</span><span class="sxs-lookup"><span data-stu-id="a5588-834">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="a5588-835">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-835">**Old behavior**</span></span>

<span data-ttu-id="a5588-836">Vor EF Core 3.0 war Microsoft.EntityFrameworkCore.Design ein reguläres NuGet-Paket, auf dessen Assembly von Projekten verwiesen werden kann, die davon abhängig sind.</span><span class="sxs-lookup"><span data-stu-id="a5588-836">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="a5588-837">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-837">**New behavior**</span></span>

<span data-ttu-id="a5588-838">Ab EF Core 3.0 ist es ein DevelopmentDependency-Paket.</span><span class="sxs-lookup"><span data-stu-id="a5588-838">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="a5588-839">Das bedeutet, dass die Abhängigkeit nicht transitiv auf andere Projekte übertragen wird und Sie nicht mehr standardmäßig auf die Assembly verweisen können.</span><span class="sxs-lookup"><span data-stu-id="a5588-839">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="a5588-840">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-840">**Why**</span></span>

<span data-ttu-id="a5588-841">Dieses Paket ist nur für die Verwendung zur Entwurfszeit konzipiert.</span><span class="sxs-lookup"><span data-stu-id="a5588-841">This package is only intended to be used at design time.</span></span> <span data-ttu-id="a5588-842">Bereitgestellte Anwendungen sollten nicht darauf verweisen.</span><span class="sxs-lookup"><span data-stu-id="a5588-842">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="a5588-843">Die Kennzeichnung des Pakets als DevelopmentDependency unterstreicht diese Empfehlung.</span><span class="sxs-lookup"><span data-stu-id="a5588-843">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="a5588-844">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-844">**Mitigations**</span></span>

<span data-ttu-id="a5588-845">Wenn Sie auf dieses Paket verweisen müssen, um das Verhalten von EF Core zur Entwurfszeit zu überschreiben, können Sie die Metadaten des PackageReference-Elements in Ihrem Projekt aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="a5588-845">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="a5588-846">Wenn transitiv über via Microsoft.EntityFrameworkCore.Tools auf das Paket verwiesen wird, müssen Sie dem Paket eine explizite PackageReference hinzufügen, um seine Metadaten zu ändern.</span><span class="sxs-lookup"><span data-stu-id="a5588-846">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="a5588-847">SQLitePCL.raw zu Version 2.0.0 aktualisiert</span><span class="sxs-lookup"><span data-stu-id="a5588-847">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="a5588-848">Issue #14824</span><span class="sxs-lookup"><span data-stu-id="a5588-848">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="a5588-849">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-849">**Old behavior**</span></span>

<span data-ttu-id="a5588-850">Microsoft.EntityFrameworkCore.Sqlite war früher von Version 1.1.12 von SQLitePCL.raw abhängig.</span><span class="sxs-lookup"><span data-stu-id="a5588-850">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="a5588-851">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-851">**New behavior**</span></span>

<span data-ttu-id="a5588-852">Wir haben unser Paket aktualisiert, so dass es jetzt von Version 2.0.0 abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="a5588-852">We've updated our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="a5588-853">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-853">**Why**</span></span>

<span data-ttu-id="a5588-854">Version 2.0.0 von SQLitePCL.raw zielt auf .NET Standard 2.0 ab.</span><span class="sxs-lookup"><span data-stu-id="a5588-854">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="a5588-855">Zuvor war .NET Standard 1.1 das Ziel, hierfür war für ein umfassender Funktionsabschluss von transitiven Paketen erforderlich.</span><span class="sxs-lookup"><span data-stu-id="a5588-855">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="a5588-856">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-856">**Mitigations**</span></span>

<span data-ttu-id="a5588-857">SQLitePCL.raw, Version 2.0.0, enthält einige Breaking Changes.</span><span class="sxs-lookup"><span data-stu-id="a5588-857">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="a5588-858">Weitere Informationen finden Sie in den [Versionshinweisen](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md).</span><span class="sxs-lookup"><span data-stu-id="a5588-858">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="a5588-859">NetTopologySuite auf Version 2.0.0 aktualisiert</span><span class="sxs-lookup"><span data-stu-id="a5588-859">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="a5588-860">Issue #14825</span><span class="sxs-lookup"><span data-stu-id="a5588-860">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="a5588-861">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-861">**Old behavior**</span></span>

<span data-ttu-id="a5588-862">Die Pakete mit räumlichen Daten waren bisher von Version 1.15.1 der NetTopologySuite abhängig.</span><span class="sxs-lookup"><span data-stu-id="a5588-862">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="a5588-863">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-863">**New behavior**</span></span>

<span data-ttu-id="a5588-864">Wir haben unser Paket aktualisiert, so dass es jetzt von Version 2.0.0 abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="a5588-864">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="a5588-865">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-865">**Why**</span></span>

<span data-ttu-id="a5588-866">Version 2.0.0 der NetTopologySuite behebt verschiedene Verwendungsprobleme, die von EF Core-Benutzern gemeldet wurden.</span><span class="sxs-lookup"><span data-stu-id="a5588-866">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="a5588-867">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-867">**Mitigations**</span></span>

<span data-ttu-id="a5588-868">NetTopologySuite 2.0.0 umfasst einige Breaking Changes.</span><span class="sxs-lookup"><span data-stu-id="a5588-868">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="a5588-869">Weitere Informationen finden Sie in den [Versionshinweisen](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001).</span><span class="sxs-lookup"><span data-stu-id="a5588-869">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="SqlClient"></a>

### <a name="microsoftdatasqlclient-is-used-instead-of-systemdatasqlclient"></a><span data-ttu-id="a5588-870">Microsoft.Data.SqlClient wird statt System.Data.SqlClient verwendet</span><span class="sxs-lookup"><span data-stu-id="a5588-870">Microsoft.Data.SqlClient is used instead of System.Data.SqlClient</span></span>

[<span data-ttu-id="a5588-871">Issue #15636</span><span class="sxs-lookup"><span data-stu-id="a5588-871">Tracking Issue #15636</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15636)

<span data-ttu-id="a5588-872">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-872">**Old behavior**</span></span>

<span data-ttu-id="a5588-873">Microsoft.EntityFrameworkCore.SqlServer war zuvor von System.Data.SqlClient abhängig.</span><span class="sxs-lookup"><span data-stu-id="a5588-873">Microsoft.EntityFrameworkCore.SqlServer previously depended on System.Data.SqlClient.</span></span>

<span data-ttu-id="a5588-874">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-874">**New behavior**</span></span>

<span data-ttu-id="a5588-875">Das Paket wurde so aktualisiert, dass es jetzt von Microsoft.Data.SqlClient abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="a5588-875">We've updated our package to depend on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="a5588-876">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-876">**Why**</span></span>

<span data-ttu-id="a5588-877">Microsoft SQL Server ist zukünftig der Haupttreiber für den Datenzugriff für SQL Server, und System.Data.SqlClient wird nicht länger der Fokus bei der Entwicklung sein.</span><span class="sxs-lookup"><span data-stu-id="a5588-877">Microsoft.Data.SqlClient is the flagship data access driver for SQL Server going forward, and System.Data.SqlClient no longer be the focus of development.</span></span>
<span data-ttu-id="a5588-878">Einige wichtige Features wie Always Encrypted sind nur in Microsoft.Data.SqlClient verfügbar.</span><span class="sxs-lookup"><span data-stu-id="a5588-878">Some important features, such as Always Encrypted, are only available on Microsoft.Data.SqlClient.</span></span>

<span data-ttu-id="a5588-879">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-879">**Mitigations**</span></span>

<span data-ttu-id="a5588-880">Wenn Ihr Code eine direkte Abhängigkeit von System.Data.SqlClient hat, müssen Sie diesen ändern, damit er stattdessen auf Microsoft.Data.SqlClient verweist. Da die beiden Pakete einen hohen Grad an API-Kompatibilität vorweisen, sollte dafür nur eine einfache Paket- und Namespaceänderung erforderlich sein.</span><span class="sxs-lookup"><span data-stu-id="a5588-880">If your code takes a direct dependency on System.Data.SqlClient, you must change it to reference Microsoft.Data.SqlClient instead; as the two packages maintain a very high degree of API compatibility, this should only be a simple package and namespace change.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="a5588-881">Beziehungen mit mehreren mehrdeutigen Selbstverweisen müssen nun konfiguriert werden</span><span class="sxs-lookup"><span data-stu-id="a5588-881">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="a5588-882">Issue #13573</span><span class="sxs-lookup"><span data-stu-id="a5588-882">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="a5588-883">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-883">**Old behavior**</span></span>

<span data-ttu-id="a5588-884">Ein Entitätstyp, der über unidirektionale Navigationseigenschaften mit mehreren Selbstverweisen und über übereinstimmende Fremdschlüssel verfügte, wurde bislang fälschlicherweise als einfache Beziehung konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="a5588-884">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="a5588-885">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a5588-885">For example:</span></span>

```csharp
public class User 
{
        public Guid Id { get; set; }
        public User CreatedBy { get; set; }
        public User UpdatedBy { get; set; }
        public Guid CreatedById { get; set; }
        public Guid? UpdatedById { get; set; }
}
```

<span data-ttu-id="a5588-886">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-886">**New behavior**</span></span>

<span data-ttu-id="a5588-887">Dieses Szenario wird nun beim Erstellen von Modellen erkannt. Außerdem wird eine Ausnahme ausgelöst, in der darauf hingewiesen wird, dass das Modell mehrdeutig ist.</span><span class="sxs-lookup"><span data-stu-id="a5588-887">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="a5588-888">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-888">**Why**</span></span>

<span data-ttu-id="a5588-889">Das resultierende Modell war mehrdeutig und führte in diesem Fall üblicherweise zu falschen Ergebnissen.</span><span class="sxs-lookup"><span data-stu-id="a5588-889">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="a5588-890">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-890">**Mitigations**</span></span>

<span data-ttu-id="a5588-891">Konfigurieren Sie die Beziehung vollständig.</span><span class="sxs-lookup"><span data-stu-id="a5588-891">Use full configuration of the relationship.</span></span> <span data-ttu-id="a5588-892">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a5588-892">For example:</span></span>

```csharp
modelBuilder
     .Entity<User>()
     .HasOne(e => e.CreatedBy)
     .WithMany();
 
 modelBuilder
     .Entity<User>()
     .HasOne(e => e.UpdatedBy)
     .WithMany();
```

<a name="udf-empty-string"></a>
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="a5588-893">DbFunction.Schema ist NULL oder eine leere Zeichenfolge und ist deshalb so konfiguriert, dass es im Standardschema des Modells ist</span><span class="sxs-lookup"><span data-stu-id="a5588-893">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="a5588-894">Issue #12757</span><span class="sxs-lookup"><span data-stu-id="a5588-894">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="a5588-895">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-895">**Old behavior**</span></span>

<span data-ttu-id="a5588-896">Eine mit dem Schema als leere Zeichenfolge konfigurierte DbFunction wurde als integrierte Funktion ohne Schema behandelt.</span><span class="sxs-lookup"><span data-stu-id="a5588-896">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="a5588-897">Der folgende Code ordnet z. B. die CLR-Funktion `DatePart` der integrierten Funktion `DATEPART` auf SqlServer zu.</span><span class="sxs-lookup"><span data-stu-id="a5588-897">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```csharp
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="a5588-898">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a5588-898">**New behavior**</span></span>

<span data-ttu-id="a5588-899">Alle DbFunction-Zuordnungen werden als einer benutzerdefinierten Funktion zugeordnet betrachtet.</span><span class="sxs-lookup"><span data-stu-id="a5588-899">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="a5588-900">Daher würde die Funktion durch einen leeren Zeichenfolgenwert in das Standardschema für das Modell eingefügt werden.</span><span class="sxs-lookup"><span data-stu-id="a5588-900">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="a5588-901">Dies könnte das Schema sein, das explizit über eine fließende `modelBuilder.HasDefaultSchema()`- oder anderweitig über eine `dbo`-API konfiguriert wurde.</span><span class="sxs-lookup"><span data-stu-id="a5588-901">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="a5588-902">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a5588-902">**Why**</span></span>

<span data-ttu-id="a5588-903">Weil das Schema vorher leer war, konnte man erreichen, dass die Funktion integriert, aber die Logik nur für SqlServer anwendbar war, wo integrierte Funktionen nicht zu einem Schema gehören.</span><span class="sxs-lookup"><span data-stu-id="a5588-903">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="a5588-904">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a5588-904">**Mitigations**</span></span>

<span data-ttu-id="a5588-905">Konfigurieren Sie die Übersetzung von DbFunction manuell, um Sie einer integrierten Funktion zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="a5588-905">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```csharp
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
