---
title: Breaking Changes in EF Core 3.0
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: f7c241159c689d4648b2778b53e50c22f580deb0
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197920"
---
# <a name="breaking-changes-included-in-ef-core-30"></a><span data-ttu-id="fd29b-102">Breaking Changes in EF Core 3.0</span><span class="sxs-lookup"><span data-stu-id="fd29b-102">Breaking changes included in EF Core 3.0</span></span>
<span data-ttu-id="fd29b-103">Die folgenden API-Änderungen und Behavior Changes können dazu führen, dass vorhandene Anwendungen beim Upgrade auf 3.0.0 nicht mehr funktionieren.</span><span class="sxs-lookup"><span data-stu-id="fd29b-103">The following API and behavior changes have the potential to break existing applications when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="fd29b-104">Änderungen, die sich voraussichtlich nur auf Datenbankanbieter auswirken, sind unter [Änderungen mit Auswirkungen auf den Anbieter](xref:core/providers/provider-log) dokumentiert.</span><span class="sxs-lookup"><span data-stu-id="fd29b-104">Changes that we expect to only impact database providers are documented under [provider changes](xref:core/providers/provider-log).</span></span>
<span data-ttu-id="fd29b-105">Breaking Changes, die zwischen zwei Vorschauversionen von EF Core 3.0 eingeführt wurden, sind hier nicht dokumentiert.</span><span class="sxs-lookup"><span data-stu-id="fd29b-105">Breaks from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="summary"></a><span data-ttu-id="fd29b-106">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="fd29b-106">Summary</span></span>

| <span data-ttu-id="fd29b-107">**Wichtige Änderung**</span><span class="sxs-lookup"><span data-stu-id="fd29b-107">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="fd29b-108">**Auswirkung**</span><span class="sxs-lookup"><span data-stu-id="fd29b-108">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="fd29b-109">LINQ-Abfragen werden nicht mehr auf dem Client ausgewertet</span><span class="sxs-lookup"><span data-stu-id="fd29b-109">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="fd29b-110">Hoch</span><span class="sxs-lookup"><span data-stu-id="fd29b-110">High</span></span>       |
| [<span data-ttu-id="fd29b-111">EF Core 3.0 zielt auf .NET Standard 2.1 und nicht auf .NET Standard 2.0 ab</span><span class="sxs-lookup"><span data-stu-id="fd29b-111">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="fd29b-112">Hoch</span><span class="sxs-lookup"><span data-stu-id="fd29b-112">High</span></span>      |
| [<span data-ttu-id="fd29b-113">Das EF Core-Befehlszeilentool (dotnet ef) ist nicht mehr Bestandteil des .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="fd29b-113">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="fd29b-114">Hoch</span><span class="sxs-lookup"><span data-stu-id="fd29b-114">High</span></span>      |
| [<span data-ttu-id="fd29b-115">DetectChanges berücksichtigt vom Speicher generierte Schlüsselwerte</span><span class="sxs-lookup"><span data-stu-id="fd29b-115">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="fd29b-116">Hoch</span><span class="sxs-lookup"><span data-stu-id="fd29b-116">High</span></span>      |
| [<span data-ttu-id="fd29b-117">FromSql, ExecuteSql und ExecuteSqlAsync wurden umbenannt</span><span class="sxs-lookup"><span data-stu-id="fd29b-117">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="fd29b-118">Hoch</span><span class="sxs-lookup"><span data-stu-id="fd29b-118">High</span></span>      |
| [<span data-ttu-id="fd29b-119">Abfragetypen werden mit Entitätstypen zusammengeführt</span><span class="sxs-lookup"><span data-stu-id="fd29b-119">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="fd29b-120">Hoch</span><span class="sxs-lookup"><span data-stu-id="fd29b-120">High</span></span>      |
| [<span data-ttu-id="fd29b-121">Entity Framework Core ist nicht mehr Bestandteil des gemeinsam verwendeten ASP.NET Core-Frameworks</span><span class="sxs-lookup"><span data-stu-id="fd29b-121">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="fd29b-122">Mittel</span><span class="sxs-lookup"><span data-stu-id="fd29b-122">Medium</span></span>      |
| [<span data-ttu-id="fd29b-123">Kaskadierende DELETE-Anweisungen werden standardmäßig sofort ausgeführt</span><span class="sxs-lookup"><span data-stu-id="fd29b-123">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="fd29b-124">Mittel</span><span class="sxs-lookup"><span data-stu-id="fd29b-124">Medium</span></span>      |
| [<span data-ttu-id="fd29b-125">DeleteBehavior.Restrict verfügt über eine übersichtlichere Semantik</span><span class="sxs-lookup"><span data-stu-id="fd29b-125">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="fd29b-126">Mittel</span><span class="sxs-lookup"><span data-stu-id="fd29b-126">Medium</span></span>      |
| [<span data-ttu-id="fd29b-127">Die Konfigurations-API für Beziehungen abhängiger (owned) Typen wurde geändert</span><span class="sxs-lookup"><span data-stu-id="fd29b-127">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="fd29b-128">Mittel</span><span class="sxs-lookup"><span data-stu-id="fd29b-128">Medium</span></span>      |
| [<span data-ttu-id="fd29b-129">Für jede Eigenschaft wird separat ein ganzzahliger speicherinterner Schlüssel generiert</span><span class="sxs-lookup"><span data-stu-id="fd29b-129">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="fd29b-130">Mittel</span><span class="sxs-lookup"><span data-stu-id="fd29b-130">Medium</span></span>      |
| [<span data-ttu-id="fd29b-131">Abfragen ohne Nachverfolgung führen keine Identitätsauflösung mehr durch</span><span class="sxs-lookup"><span data-stu-id="fd29b-131">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="fd29b-132">Mittel</span><span class="sxs-lookup"><span data-stu-id="fd29b-132">Medium</span></span>      |
| [<span data-ttu-id="fd29b-133">Metadaten-API-Änderungen</span><span class="sxs-lookup"><span data-stu-id="fd29b-133">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="fd29b-134">Mittel</span><span class="sxs-lookup"><span data-stu-id="fd29b-134">Medium</span></span>      |
| [<span data-ttu-id="fd29b-135">Anbieterspezifische Metadaten-API-Änderungen</span><span class="sxs-lookup"><span data-stu-id="fd29b-135">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="fd29b-136">Mittel</span><span class="sxs-lookup"><span data-stu-id="fd29b-136">Medium</span></span>      |
| [<span data-ttu-id="fd29b-137">UseRowNumberForPaging wurde entfernt</span><span class="sxs-lookup"><span data-stu-id="fd29b-137">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="fd29b-138">Mittel</span><span class="sxs-lookup"><span data-stu-id="fd29b-138">Medium</span></span>      |
| [<span data-ttu-id="fd29b-139">FromSql-Methoden können nur für die Stammelemente der Abfrage angegeben werden</span><span class="sxs-lookup"><span data-stu-id="fd29b-139">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="fd29b-140">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-140">Low</span></span>      |
| [<span data-ttu-id="fd29b-141">~~Die Abfrageausführung wird auf Debugebene protokolliert~~ – zurückgesetzt</span><span class="sxs-lookup"><span data-stu-id="fd29b-141">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="fd29b-142">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-142">Low</span></span>      |
| [<span data-ttu-id="fd29b-143">Temporäre Schlüsselwerte werden nicht mehr Entitätsinstanzen zugewiesen</span><span class="sxs-lookup"><span data-stu-id="fd29b-143">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="fd29b-144">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-144">Low</span></span>      |
| [<span data-ttu-id="fd29b-145">Abhängige Entitäten, die die Tabelle gemeinsam mit dem Prinzipal verwenden, sind jetzt optional</span><span class="sxs-lookup"><span data-stu-id="fd29b-145">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="fd29b-146">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-146">Low</span></span>      |
| [<span data-ttu-id="fd29b-147">Alle Entitäten, die eine Tabelle mit einer Spalte für das Parallelitätstoken gemeinsam verwenden, müssen diese einer Eigenschaft zuordnen</span><span class="sxs-lookup"><span data-stu-id="fd29b-147">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="fd29b-148">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-148">Low</span></span>      |
| [<span data-ttu-id="fd29b-149">Geerbte Eigenschaften von nicht zugeordneten Typen sind nun einer einzelnen Spalte für alle abgeleiteten Typen zugeordnet</span><span class="sxs-lookup"><span data-stu-id="fd29b-149">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="fd29b-150">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-150">Low</span></span>      |
| [<span data-ttu-id="fd29b-151">Die Konvention zur Fremdschlüsseleigenschaft entspricht nicht mehr dem Namen der Prinzipaleigenschaft</span><span class="sxs-lookup"><span data-stu-id="fd29b-151">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="fd29b-152">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-152">Low</span></span>      |
| [<span data-ttu-id="fd29b-153">Die Datenbankverbindung wird jetzt getrennt, wenn sie nicht mehr verwendet wird, bevor TransactionScope abgeschlossen wurde</span><span class="sxs-lookup"><span data-stu-id="fd29b-153">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="fd29b-154">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-154">Low</span></span>      |
| [<span data-ttu-id="fd29b-155">Unterstützungsfelder werden standardmäßig verwendet</span><span class="sxs-lookup"><span data-stu-id="fd29b-155">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="fd29b-156">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-156">Low</span></span>      |
| [<span data-ttu-id="fd29b-157">Eine Ausnahme wird ausgelöst, wenn mehrere kompatible Unterstützungsfelder gefunden werden</span><span class="sxs-lookup"><span data-stu-id="fd29b-157">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="fd29b-158">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-158">Low</span></span>      |
| [<span data-ttu-id="fd29b-159">„Nur-Feld“-Eigenschaftennamen sollten dem Feldnamen entsprechen</span><span class="sxs-lookup"><span data-stu-id="fd29b-159">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="fd29b-160">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-160">Low</span></span>      |
| [<span data-ttu-id="fd29b-161">AddLogging und AddMemoryCache werden nicht mehr von AddDbContext/AddDbContextPool aufgerufen</span><span class="sxs-lookup"><span data-stu-id="fd29b-161">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="fd29b-162">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-162">Low</span></span>      |
| [<span data-ttu-id="fd29b-163">DbContext.Entry erkennt nun mit DetectChanges lokale Änderungen</span><span class="sxs-lookup"><span data-stu-id="fd29b-163">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="fd29b-164">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-164">Low</span></span>      |
| [<span data-ttu-id="fd29b-165">Schlüssel aus Zeichenfolgen und Bytearrays werden standardmäßig nicht vom Client generiert</span><span class="sxs-lookup"><span data-stu-id="fd29b-165">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="fd29b-166">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-166">Low</span></span>      |
| [<span data-ttu-id="fd29b-167">ILoggerFactory ist nun ein bereichsbezogener Dienst</span><span class="sxs-lookup"><span data-stu-id="fd29b-167">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="fd29b-168">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-168">Low</span></span>      |
| [<span data-ttu-id="fd29b-169">Für Proxys mit Lazy Loading (trägem Laden) gilt nicht mehr die Annahme, dass Navigationseigenschaften vollständig geladen sind</span><span class="sxs-lookup"><span data-stu-id="fd29b-169">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="fd29b-170">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-170">Low</span></span>      |
| [<span data-ttu-id="fd29b-171">Die Erstellung zu vieler interner Dienstanbieter führt standardmäßig zu einem Fehler</span><span class="sxs-lookup"><span data-stu-id="fd29b-171">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="fd29b-172">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-172">Low</span></span>      |
| [<span data-ttu-id="fd29b-173">Neues Verhalten für HasOne/HasMany bei Aufruf mit einer einzelnen Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="fd29b-173">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="fd29b-174">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-174">Low</span></span>      |
| [<span data-ttu-id="fd29b-175">Der Rückgabetyp für mehrere asynchrone Methoden wurde von Task in ValueTask geändert</span><span class="sxs-lookup"><span data-stu-id="fd29b-175">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="fd29b-176">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-176">Low</span></span>      |
| [<span data-ttu-id="fd29b-177">Die Anmerkung Relational:TypeMapping heißt nun nur noch TypeMapping</span><span class="sxs-lookup"><span data-stu-id="fd29b-177">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="fd29b-178">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-178">Low</span></span>      |
| [<span data-ttu-id="fd29b-179">ToTable löst für einen abgeleiteten Typ eine Ausnahme aus</span><span class="sxs-lookup"><span data-stu-id="fd29b-179">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="fd29b-180">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-180">Low</span></span>      |
| [<span data-ttu-id="fd29b-181">EF Core sendet keine PRAGMA-Anweisungen mehr, um Fremdschlüssel in SQLite zu erzwingen</span><span class="sxs-lookup"><span data-stu-id="fd29b-181">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="fd29b-182">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-182">Low</span></span>      |
| [<span data-ttu-id="fd29b-183">SQLitePCLRaw.bundle_e_sqlite3 ist nun eine Abhängigkeit von Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="fd29b-183">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="fd29b-184">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-184">Low</span></span>      |
| [<span data-ttu-id="fd29b-185">GUID-Werte werden jetzt als TEXT in SQLite gespeichert</span><span class="sxs-lookup"><span data-stu-id="fd29b-185">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="fd29b-186">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-186">Low</span></span>      |
| [<span data-ttu-id="fd29b-187">Char-Werte werden jetzt als TEXT in SQLite gespeichert</span><span class="sxs-lookup"><span data-stu-id="fd29b-187">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="fd29b-188">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-188">Low</span></span>      |
| [<span data-ttu-id="fd29b-189">Migrations-IDs werden jetzt mit dem Kalender der invarianten Kultur generiert</span><span class="sxs-lookup"><span data-stu-id="fd29b-189">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="fd29b-190">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-190">Low</span></span>      |
| [<span data-ttu-id="fd29b-191">Erweiterungsinformationen und -metadaten wurden aus IDbContextOptionsExtension entfernt</span><span class="sxs-lookup"><span data-stu-id="fd29b-191">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="fd29b-192">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-192">Low</span></span>      |
| [<span data-ttu-id="fd29b-193">LogQueryPossibleExceptionWithAggregateOperator wurde umbenannt</span><span class="sxs-lookup"><span data-stu-id="fd29b-193">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="fd29b-194">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-194">Low</span></span>      |
| [<span data-ttu-id="fd29b-195">Übersichtlichere API für Namen von Fremdschlüsselconstraints</span><span class="sxs-lookup"><span data-stu-id="fd29b-195">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="fd29b-196">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-196">Low</span></span>      |
| [<span data-ttu-id="fd29b-197">IRelationalDatabaseCreator.HasTables/HasTablesAsync ist jetzt öffentlich</span><span class="sxs-lookup"><span data-stu-id="fd29b-197">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="fd29b-198">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-198">Low</span></span>      |
| [<span data-ttu-id="fd29b-199">Microsoft.EntityFrameworkCore.Design ist jetzt ein DevelopmentDependency-Paket</span><span class="sxs-lookup"><span data-stu-id="fd29b-199">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="fd29b-200">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-200">Low</span></span>      |
| [<span data-ttu-id="fd29b-201">SQLitePCL.raw wurde auf Version 2.0.0 aktualisiert</span><span class="sxs-lookup"><span data-stu-id="fd29b-201">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="fd29b-202">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-202">Low</span></span>      |
| [<span data-ttu-id="fd29b-203">NetTopologySuite wurde auf Version 2.0.0 aktualisiert</span><span class="sxs-lookup"><span data-stu-id="fd29b-203">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="fd29b-204">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-204">Low</span></span>      |
| [<span data-ttu-id="fd29b-205">Beziehungen mit mehreren mehrdeutigen Selbstverweisen müssen nun konfiguriert werden</span><span class="sxs-lookup"><span data-stu-id="fd29b-205">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="fd29b-206">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-206">Low</span></span>      |
| [<span data-ttu-id="fd29b-207">DbFunction.Schema ist NULL oder eine leere Zeichenfolge und ist deshalb so konfiguriert, dass es im Standardschema des Modells ist</span><span class="sxs-lookup"><span data-stu-id="fd29b-207">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>](#udf-empty-string) | <span data-ttu-id="fd29b-208">Niedrig</span><span class="sxs-lookup"><span data-stu-id="fd29b-208">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="fd29b-209">LINQ-Abfragen werden nicht mehr auf dem Client ausgewertet</span><span class="sxs-lookup"><span data-stu-id="fd29b-209">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="fd29b-210">[Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Siehe auch Issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="fd29b-210">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="fd29b-211">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-211">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fd29b-212">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-212">**Old behavior**</span></span>

<span data-ttu-id="fd29b-213">Vor Version 3.0 wurden in EF Core automatisch Ausdrücke auf dem Client ausgewertet, wenn diese Teil einer Abfrage waren und nicht in SQL oder in einen Parameter konvertiert werden konnten.</span><span class="sxs-lookup"><span data-stu-id="fd29b-213">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="fd29b-214">Wenn potenziell aufwendige Ausdrücke auf dem Client ausgewertet werden sollten, wurde standardmäßig nur eine Warnung ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="fd29b-214">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="fd29b-215">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-215">**New behavior**</span></span>

<span data-ttu-id="fd29b-216">Ab Version 3.0 können in EF Core nur Ausdrücke der obersten Projektion (dem letzten `Select()`-Aufruf in der Abfrage) auf dem Client ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-216">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="fd29b-217">Wenn Ausdrücke in einem anderen Teil der Abfrage nicht in SQL oder in einen Parameter konvertiert werden können, wird eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="fd29b-217">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="fd29b-218">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-218">**Why**</span></span>

<span data-ttu-id="fd29b-219">Die automatische Auswertung von Abfragen auf Clients hat zur Folge, dass viele Abfragen auch dann ausgeführt werden, wenn wichtige Teile nicht übersetzt werden können.</span><span class="sxs-lookup"><span data-stu-id="fd29b-219">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="fd29b-220">Dieses Verhalten kann zu unerwartetem und potenziell schädlichem Verhalten führen, das möglicherweise erst in der Produktionsphase erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="fd29b-220">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="fd29b-221">Eine nicht übersetzbare Bedingung in einem `Where()`-Aufruf kann sich beispielsweise so auswirken, dass alle Zeilen einer Tabelle vom Datenbankserver übertragen werden und der Filter auf dem Client angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="fd29b-221">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="fd29b-222">Wenn die Tabelle in der Entwicklungsphase nur wenige Zeilen enthält, ist es gut möglich, dass diese Situation unerkannt bleibt. Wenn die Anwendung jedoch in die Produktionsumgebung überführt wird, enthält die Tabelle möglicherweise Millionen von Zeilen, was zu schwerwiegenden Konsequenzen führen kann.</span><span class="sxs-lookup"><span data-stu-id="fd29b-222">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="fd29b-223">Wie die Erfahrung in der Entwicklungsphase gezeigt hat, können auch Warnungen, die bei Auswertungen auf dem Client ausgelöst werden, dieses Problem nicht lösen, da sie sich leicht ignorieren lassen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-223">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="fd29b-224">Die automatische Auswertung auf Clients kann darüber hinaus zu Problemen führen, bei denen die Verbesserung der Abfrageübersetzung für bestimmte Ausdrücke zu unbeabsichtigten Breaking Changes zwischen Releases führt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-224">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="fd29b-225">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-225">**Mitigations**</span></span>

<span data-ttu-id="fd29b-226">Wenn sich eine Abfrage nicht vollständig übersetzen lässt, habe Sie zwei Möglichkeiten: Schreiben Sie sie entweder um, oder setzen Sie alternativ `AsEnumerable()`, `ToList()` oder ähnliche Methoden ein, um Daten wieder zurück an den Client zu übertragen, wo sie mit LINQ to Objects weiterverarbeitet werden können.</span><span class="sxs-lookup"><span data-stu-id="fd29b-226">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="fd29b-227">EF Core 3.0 zielt auf .NET Standard 2.1 und nicht auf .NET Standard 2.0 ab</span><span class="sxs-lookup"><span data-stu-id="fd29b-227">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="fd29b-228">Issue #15498</span><span class="sxs-lookup"><span data-stu-id="fd29b-228">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="fd29b-229">Diese Änderung wird in Vorschauversion 7 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-229">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="fd29b-230">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-230">**Old behavior**</span></span>

<span data-ttu-id="fd29b-231">Vor 3.0 zielte EF Core auf .NET Standard 2.0 ab und wurde auf allen Plattformen ausgeführt, die diesen Standard unterstützen, einschließlich .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="fd29b-231">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="fd29b-232">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-232">**New behavior**</span></span>

<span data-ttu-id="fd29b-233">Ab 3.0 zielt EF Core auf .NET Standard 2.1 ab und wird auf allen Plattformen ausgeführt, die diesen Standard unterstützen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-233">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="fd29b-234">Dies schließt .NET Framework nicht ein.</span><span class="sxs-lookup"><span data-stu-id="fd29b-234">This does not include .NET Framework.</span></span>

<span data-ttu-id="fd29b-235">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-235">**Why**</span></span>

<span data-ttu-id="fd29b-236">Dies ist ein Teil der strategischen Entscheidung bezüglich .NET-Technologien, um den Fokus auf .NET Core und andere moderne .NET-Plattformen (z. B. Xamarin) zu legen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-236">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="fd29b-237">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-237">**Mitigations**</span></span>

<span data-ttu-id="fd29b-238">Erwägen Sie, zu einer moderneren .NET-Plattform zu wechseln.</span><span class="sxs-lookup"><span data-stu-id="fd29b-238">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="fd29b-239">Falls dies nicht möglich ist, verwenden Sie weiterhin EF Core 2.1 oder EF Core 2.2, die beide .NET Framework unterstützen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-239">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="fd29b-240">Entity Framework Core ist nicht mehr Teil des gemeinsam verwendeten ASP.NET Core-Frameworks</span><span class="sxs-lookup"><span data-stu-id="fd29b-240">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="fd29b-241">Issueankündigungen #325</span><span class="sxs-lookup"><span data-stu-id="fd29b-241">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="fd29b-242">Diese Änderung wird in Vorschauversion 1 von ASP.NET Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-242">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="fd29b-243">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-243">**Old behavior**</span></span>

<span data-ttu-id="fd29b-244">Vor Version 3.0 von ASP.NET Core wurden EF Core und einige der zugehörigen Datenanbieter wie SQL Server eingeschlossen, wenn `Microsoft.AspNetCore.App` oder `Microsoft.AspNetCore.All` ein Paketverweis hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="fd29b-244">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="fd29b-245">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-245">**New behavior**</span></span>

<span data-ttu-id="fd29b-246">Ab Version 3.0 enthält das gemeinsam verwendete ASP.NET Core-Framework weder EF Core noch zugehörige Datenanbieter.</span><span class="sxs-lookup"><span data-stu-id="fd29b-246">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="fd29b-247">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-247">**Why**</span></span>

<span data-ttu-id="fd29b-248">Vor dieser Änderung konnte EF Core auf unterschiedliche Arten bezogen werden. Diese richteten sich danach, ob ASP.NET Core und SQL Server oder ein anderes Zielframework für die Anwendung vorgesehen war.</span><span class="sxs-lookup"><span data-stu-id="fd29b-248">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="fd29b-249">Bei einem Upgrade von ASP.NET Core wurde zudem ein Upgrade von EF Core und des SQL Server-Anbieters erzwungen, was nicht in allen Fällen gewünscht war.</span><span class="sxs-lookup"><span data-stu-id="fd29b-249">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="fd29b-250">Durch die eingeführte Änderung wird EF Core unter allen Anbietern, unterstützten .NET-Implementierungen und Anwendungstypen auf dieselbe Weise bezogen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-250">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="fd29b-251">Entwickler können nun außerdem genau festlegen, wann für EF Core und zugehörige Datenanbieter ein Upgrade durchgeführt werden soll.</span><span class="sxs-lookup"><span data-stu-id="fd29b-251">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="fd29b-252">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-252">**Mitigations**</span></span>

<span data-ttu-id="fd29b-253">Wenn Sie EF Core in einer Anwendung unter ASP.NET Core 3.0 oder in einer anderen unterstützten Anwendung verwenden möchten, müssen Sie dem EF Core-Datenbankanbieter, der für die Anwendung genutzt werden soll, explizit einen Paketverweis hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-253">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="fd29b-254">Das EF Core-Befehlszeilentool (dotnet ef) ist nicht länger Bestandteil des .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="fd29b-254">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="fd29b-255">Issue #14016</span><span class="sxs-lookup"><span data-stu-id="fd29b-255">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="fd29b-256">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 und der zugehörigen Version des .NET Core SDK eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-256">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="fd29b-257">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-257">**Old behavior**</span></span>

<span data-ttu-id="fd29b-258">Vor Version 3.0 war das `dotnet ef`-Tool im .NET Core SDK enthalten und konnte von der Befehlszeile aus ohne zusätzliche Schritte in einem beliebigen Projekt verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-258">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="fd29b-259">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-259">**New behavior**</span></span>

<span data-ttu-id="fd29b-260">Ab Version 3.0 ist das `dotnet ef`-Tool nicht mehr im .NET SDK enthalten und muss deshalb vor der Verwendung explizit als lokales oder globales Tool installiert werden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-260">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="fd29b-261">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-261">**Why**</span></span>

<span data-ttu-id="fd29b-262">Durch diese Änderung kann `dotnet ef` als reguläres .NET-CLI-Tool für NuGet verteilt und aktualisiert werden und entspricht damit dem üblichen Verfahren, dass EF Core 3.0 ebenfalls immer als NuGet-Paket verteilt wird.</span><span class="sxs-lookup"><span data-stu-id="fd29b-262">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="fd29b-263">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-263">**Mitigations**</span></span>

<span data-ttu-id="fd29b-264">Um Migrationen zu verwalten oder ein Gerüst für `DbContext` zu erstellen, installieren Sie `dotnet-ef` als globales Tool:</span><span class="sxs-lookup"><span data-stu-id="fd29b-264">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef
  ```

<span data-ttu-id="fd29b-265">Eine Verwendung als lokales Tool ist ebenfalls möglich, wenn Sie die Abhängigkeiten eines Projekts wiederherstellen, das das Tool mithilfe einer [Toolmanifestdatei](https://github.com/dotnet/cli/issues/10288) als Toolabhängigkeit deklariert.</span><span class="sxs-lookup"><span data-stu-id="fd29b-265">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="fd29b-266">FromSql, ExecuteSql und ExecuteSqlAsync wurden umbenannt</span><span class="sxs-lookup"><span data-stu-id="fd29b-266">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="fd29b-267">Issue #10996</span><span class="sxs-lookup"><span data-stu-id="fd29b-267">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="fd29b-268">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-268">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fd29b-269">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-269">**Old behavior**</span></span>

<span data-ttu-id="fd29b-270">Vor Version 3.0 wurden in EF Core diese Methodennamen überladen, um entweder mit einer normalen Zeichenfolge oder einer Zeichenfolge, die in SQL und Parameter interpoliert werden sollte, zu arbeiten.</span><span class="sxs-lookup"><span data-stu-id="fd29b-270">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="fd29b-271">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-271">**New behavior**</span></span>

<span data-ttu-id="fd29b-272">Ab Version 3.0 verwenden Sie in EF Core `FromSqlRaw`, `ExecuteSqlRaw` und `ExecuteSqlRawAsync`, um eine parametrisierte Abfrage zu erstellen, bei der die Parameter einzeln aus der Abfragezeichenfolge übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-272">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="fd29b-273">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fd29b-273">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="fd29b-274">Verwenden Sie `FromSqlInterpolated`, `ExecuteSqlInterpolated` und `ExecuteSqlInterpolatedAsync`, um eine parametrisierte Abfrage zu erstellen, bei der die Parameter als Teil einer interpolierten Abfragezeichenfolge übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-274">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="fd29b-275">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fd29b-275">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="fd29b-276">Beachten Sie, dass die beiden oben genannten Abfragen dieselbe parametrisierte SQL mit denselben SQL-Parametern erzeugen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-276">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="fd29b-277">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-277">**Why**</span></span>

<span data-ttu-id="fd29b-278">Bei Methodenüberladungen wie dieser wird sehr leicht versehentlich die Methode für unformatierte Zeichenfolgen aufgerufen, wenn eigentlich beabsichtigt war, die Methode für interpolierte Zeichenfolgen aufzurufen (und umgekehrt).</span><span class="sxs-lookup"><span data-stu-id="fd29b-278">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="fd29b-279">Dadurch werden Abfragen möglicherweise nicht parametrisiert, obwohl dies der Fall sein sollte.</span><span class="sxs-lookup"><span data-stu-id="fd29b-279">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="fd29b-280">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-280">**Mitigations**</span></span>

<span data-ttu-id="fd29b-281">Verwenden Sie ab sofort die neuen Methodennamen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-281">Switch to use the new method names.</span></span>

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="fd29b-282">FromSql-Methoden können nur für die Stammelemente der Abfrage angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-282">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="fd29b-283">Issue #15704</span><span class="sxs-lookup"><span data-stu-id="fd29b-283">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="fd29b-284">Diese Änderung wird in Vorschauversion 6 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-284">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="fd29b-285">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-285">**Old behavior**</span></span>

<span data-ttu-id="fd29b-286">Vor EF Core 3.0 konnte die `FromSql`-Methode an einer beliebigen Stelle in der Abfrage angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-286">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="fd29b-287">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-287">**New behavior**</span></span>

<span data-ttu-id="fd29b-288">Ab EF Core 3.0 können die neuen Methoden `FromSqlRaw` und `FromSqlInterpolated` (durch die `FromSql` ersetzt wird) nur für Stammelemente von Abfragen angegeben werden, d.h. direkt für das `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="fd29b-288">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="fd29b-289">Der Versuch, sie an anderer Stelle anzugeben, führt zu einem Kompilierungsfehler.</span><span class="sxs-lookup"><span data-stu-id="fd29b-289">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="fd29b-290">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-290">**Why**</span></span>

<span data-ttu-id="fd29b-291">Die Angabe von `FromSql` an einer anderen Stelle als für ein `DbSet` erbrachte keine zusätzliche Bedeutung und keinen Mehrwert, sondern konnte in bestimmten Szenarien zu Mehrdeutigkeiten führen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-291">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="fd29b-292">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-292">**Mitigations**</span></span>

<span data-ttu-id="fd29b-293">`FromSql` Aufrufe sollten so verschoben, dass sie direkt für das zugehörige `DbSet` gelten.</span><span class="sxs-lookup"><span data-stu-id="fd29b-293">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="fd29b-294">Abfragen ohne Nachverfolgung führen keine Identitätsauflösung mehr durch</span><span class="sxs-lookup"><span data-stu-id="fd29b-294">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="fd29b-295">Issue #13518</span><span class="sxs-lookup"><span data-stu-id="fd29b-295">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="fd29b-296">Diese Änderung wird in Vorschauversion 6 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-296">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="fd29b-297">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-297">**Old behavior**</span></span>

<span data-ttu-id="fd29b-298">Vor EF Core 3.0 wurde dieselbe Entitätsinstanz für jedes Vorkommen einer Entität mit einem bestimmten Typ und einer bestimmten ID verwendet.</span><span class="sxs-lookup"><span data-stu-id="fd29b-298">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="fd29b-299">Dies entspricht dem Verhalten von Abfragen zu Nachverfolgungen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-299">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="fd29b-300">Die folgende Abfrage:</span><span class="sxs-lookup"><span data-stu-id="fd29b-300">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="fd29b-301">gibt dieselbe `Category`-Instanz für jedes `Product` zurück, das der bestimmten Kategorie zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="fd29b-301">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="fd29b-302">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-302">**New behavior**</span></span>

<span data-ttu-id="fd29b-303">Ab EF Core 3.0 werden unterschiedliche Entitätsinstanzen erstellt, wenn eine Entität mit einem bestimmten Typ und einer bestimmten ID an verschiedenen Stellen im zurückgegebenen Diagramm gefunden wird.</span><span class="sxs-lookup"><span data-stu-id="fd29b-303">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="fd29b-304">Die Abfrage oben gibt beispielsweise nun eine neue `Category`-Instanz für jedes `Product` zurück, auch wenn zwei Produkte derselben Kategorie zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="fd29b-304">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="fd29b-305">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-305">**Why**</span></span>

<span data-ttu-id="fd29b-306">Die Identitätsauflösung (d. h. Feststellen, dass eine Entität über denselben Typ und die dieselbe ID wie die zuvor aufgetretene Entität verfügt) fügt zusätzlichen Aufwand für Leistung und Arbeitsspeicher hinzu.</span><span class="sxs-lookup"><span data-stu-id="fd29b-306">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="fd29b-307">Dies widerspricht normalerweise dem Grund, warum in erster Linie Abfragen ohne Nachverfolgung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-307">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="fd29b-308">Obwohl die Identitätsauflösung manchmal nützlich sein kann, wird sie nicht benötigt, wenn die Entitäten serialisiert und an den Client gesendet werden, was manchmal für Abfragen ohne Nachverfolgung der Fall ist.</span><span class="sxs-lookup"><span data-stu-id="fd29b-308">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="fd29b-309">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-309">**Mitigations**</span></span>

<span data-ttu-id="fd29b-310">Verwenden Sie eine Nachverfolgungsabfrage, falls die Auflösung erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="fd29b-310">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="fd29b-311">~~Die Abfrageausführung wird auf Debugebene protokolliert~~ – zurückgesetzt</span><span class="sxs-lookup"><span data-stu-id="fd29b-311">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="fd29b-312">Issue #14523</span><span class="sxs-lookup"><span data-stu-id="fd29b-312">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="fd29b-313">Diese Änderung wird in Vorschauversion 7 von EF Core 3.0 zurückgesetzt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-313">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="fd29b-314">Wir haben diese Änderung zurückgesetzt, weil die neue Konfiguration in EF Core 3.0 die Angabe des Protokolliergrads für jedes Ereignis durch die Anwendung ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="fd29b-314">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="fd29b-315">Um z.B. die Protokollierung von SQL zu `Debug` zu ändern, konfigurieren Sie den Grad explizit in `OnConfiguring` oder `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="fd29b-315">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="fd29b-316">Temporäre Schlüsselwerte werden nicht mehr Entitätsinstanzen zugewiesen</span><span class="sxs-lookup"><span data-stu-id="fd29b-316">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="fd29b-317">Issue #12378</span><span class="sxs-lookup"><span data-stu-id="fd29b-317">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="fd29b-318">Diese Änderung wird in Vorschauversion 2 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-318">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="fd29b-319">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-319">**Old behavior**</span></span>

<span data-ttu-id="fd29b-320">Vor Version 3.0 wurden in EF Core allen Schlüsseleigenschaften temporäre Werte zugewiesen. Später wurden den Eigenschaften dann die tatsächlichen Werte zugewiesen, die von der Datenbank generiert wurden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-320">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="fd29b-321">Die temporären Werte waren in der Regel große negative Zahlen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-321">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="fd29b-322">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-322">**New behavior**</span></span>

<span data-ttu-id="fd29b-323">Ab Version 3.0 wird in EF Core der temporäre Wert als Teil der Überwachungsinformationen der Entität gespeichert. Die Schlüsseleigenschaft selbst bleibt unverändert.</span><span class="sxs-lookup"><span data-stu-id="fd29b-323">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="fd29b-324">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-324">**Why**</span></span>

<span data-ttu-id="fd29b-325">Diese Änderung wurde vorgenommen, damit temporäre Schlüsselwerte nicht fälschlicherweise dauerhaft gespeichert werden, wenn eine Entität, die vorher von einer `DbContext`-Instanz überwacht wurde, in eine andere `DbContext`-Instanz verschoben wird.</span><span class="sxs-lookup"><span data-stu-id="fd29b-325">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="fd29b-326">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-326">**Mitigations**</span></span>

<span data-ttu-id="fd29b-327">Anwendungen, die Primärschlüsselwerte Fremdschlüsseln zuweisen, um Zuordnungen zwischen Entitäten zu erstellen, können vom alten Verhalten abhängig sein, wenn Primärschlüssel vom Speicher generiert werden und zu Entitäten im `Added`-Zustand gehören.</span><span class="sxs-lookup"><span data-stu-id="fd29b-327">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="fd29b-328">Sie können dies wie folgt vermeiden:</span><span class="sxs-lookup"><span data-stu-id="fd29b-328">This can be avoided by:</span></span>
* <span data-ttu-id="fd29b-329">Verwenden Sie keine vom Speicher generierten Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="fd29b-329">Not using store-generated keys.</span></span>
* <span data-ttu-id="fd29b-330">Legen Sie zum Erstellen von Zuordnungen keine Fremdschlüsselwerte, sondern Navigationseigenschaften fest.</span><span class="sxs-lookup"><span data-stu-id="fd29b-330">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="fd29b-331">Rufen Sie die tatsächlichen temporären Schlüsselwerte aus den Überwachungsinformationen der Entität ab.</span><span class="sxs-lookup"><span data-stu-id="fd29b-331">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="fd29b-332">`context.Entry(blog).Property(e => e.Id).CurrentValue` gibt beispielsweise den temporären Wert zurück, obwohl `blog.Id` noch nicht festgelegt wurde.</span><span class="sxs-lookup"><span data-stu-id="fd29b-332">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="fd29b-333">DetectChanges berücksichtigt vom Speicher generierte Schlüsselwerte</span><span class="sxs-lookup"><span data-stu-id="fd29b-333">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="fd29b-334">Issue #14616</span><span class="sxs-lookup"><span data-stu-id="fd29b-334">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="fd29b-335">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-335">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="fd29b-336">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-336">**Old behavior**</span></span>

<span data-ttu-id="fd29b-337">Vor Version 3.0 wurde in EF Core eine nicht überwachte Entität, die von `DetectChanges` gefunden wurde, im `Added`-Zustand überwacht und als neue Zeile eingefügt, sobald `SaveChanges` aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="fd29b-337">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="fd29b-338">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-338">**New behavior**</span></span>

<span data-ttu-id="fd29b-339">Ab Version 3.0 wird in EF Core die Entität im `Modified`-Zustand überwacht, wenn für diese generierte Schlüsselwerte verwendet werden und ein Schlüsselwert festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="fd29b-339">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="fd29b-340">Es wird also davon ausgegangen, dass eine Zeile für die Entität vorhanden ist. Wenn `SaveChanges` aufgerufen wird, wird die Zeile außerdem aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="fd29b-340">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="fd29b-341">Falls kein Schlüsselwert festgelegt ist oder der Entitätstyp keine generierten Schlüssel verwendet, wird die neue Entität wie in früheren Version weiterhin im `Added`-Zustand überwacht.</span><span class="sxs-lookup"><span data-stu-id="fd29b-341">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="fd29b-342">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-342">**Why**</span></span>

<span data-ttu-id="fd29b-343">Diese Änderung wurde vorgenommen, um bei der Verwendung von speichergenerierten Schlüsseln die Arbeit mit nicht verbundenen Entitätsgraphen einfacher und konsistenter zu gestalten.</span><span class="sxs-lookup"><span data-stu-id="fd29b-343">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="fd29b-344">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-344">**Mitigations**</span></span>

<span data-ttu-id="fd29b-345">Diese Änderung kann dazu führen, dass eine Anwendung nicht mehr funktioniert. Dieser Fall tritt ein, wenn für Entitätstypen generierte Schlüssel vorgesehen sind, dann jedoch explizit Schlüsselwerte für neue Instanzen festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-345">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="fd29b-346">Sie können dieses Problem vermeiden, indem Sie explizit festlegen, dass für Schlüsseleigenschaften keine generierten Werte verwendet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-346">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="fd29b-347">Dies ist beispielsweise mit der Fluent-API möglich:</span><span class="sxs-lookup"><span data-stu-id="fd29b-347">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="fd29b-348">Alternativ bieten sich auch Datenanmerkungen an:</span><span class="sxs-lookup"><span data-stu-id="fd29b-348">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="fd29b-349">Kaskadierende Deletes werden standardmäßig sofort ausgeführt</span><span class="sxs-lookup"><span data-stu-id="fd29b-349">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="fd29b-350">Issue #10114</span><span class="sxs-lookup"><span data-stu-id="fd29b-350">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="fd29b-351">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-351">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="fd29b-352">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-352">**Old behavior**</span></span>

<span data-ttu-id="fd29b-353">Vor Version 3.0 wurden in EF Core kaskadierenden Aktionen (Löschen von abhängigen Entitäten nach dem Löschen eines erforderlichen Prinzipals oder nach dem Aufheben einer Beziehung zum erforderlichen Prinzipal) erst ausgeführt, wenn SaveChanges aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="fd29b-353">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="fd29b-354">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-354">**New behavior**</span></span>

<span data-ttu-id="fd29b-355">Ab Version 3.0 werden in EF Core kaskadierende Aktionen angewendet, sobald die auslösende Bedingung erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="fd29b-355">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="fd29b-356">Wenn beispielsweise `context.Remove()` zum Löschen einer Prinzipalentität aufgerufen wird, führt das dazu, dass auch alle zugehörigen erforderlichen Entitäten, die überwacht werden, sofort auf `Deleted` festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-356">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="fd29b-357">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-357">**Why**</span></span>

<span data-ttu-id="fd29b-358">Diese Änderung wurde vorgenommen, um die Arbeit mit Datenbindungen und Überwachungsszenarios zu vereinfachen, in denen bekannt sein muss, welche Entitäten _vor_ dem Aufruf von `SaveChanges` gelöscht werden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-358">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="fd29b-359">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-359">**Mitigations**</span></span>

<span data-ttu-id="fd29b-360">Sie können das vorherige Verhalten wiederherstellen, indem Sie für `context.ChangedTracker` die entsprechenden Einstellungen vornehmen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-360">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="fd29b-361">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fd29b-361">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="fd29b-362">DeleteBehavior.Restrict verfügt über eine übersichtlichere Semantik</span><span class="sxs-lookup"><span data-stu-id="fd29b-362">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="fd29b-363">Issue #12661</span><span class="sxs-lookup"><span data-stu-id="fd29b-363">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="fd29b-364">Diese Änderung wird in Vorschauversion 5 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-364">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="fd29b-365">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-365">**Old behavior**</span></span>

<span data-ttu-id="fd29b-366">Vor Version 3.0 wurden mit `DeleteBehavior.Restrict` Fremdschlüssel in der Datenbank mit `Restrict`-Semantik erstellt, es wurden aber auch unvorhergesehen interne Fixups geändert.</span><span class="sxs-lookup"><span data-stu-id="fd29b-366">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="fd29b-367">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-367">**New behavior**</span></span>

<span data-ttu-id="fd29b-368">Ab Version 3.0 wird mit `DeleteBehavior.Restrict` sichergestellt, dass Fremdschlüssel mit `Restrict`-Semantik erstellt werden – d. h. ohne Überlappungen; ausgenommen bei Einschränkungsverletzungen – und ohne Auswirkungen auf EF-interne Fixups.</span><span class="sxs-lookup"><span data-stu-id="fd29b-368">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="fd29b-369">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-369">**Why**</span></span>

<span data-ttu-id="fd29b-370">Diese Änderung wurde vorgenommen, um die Benutzerfreundlichkeit bei intuitiver Verwendung von `DeleteBehavior` ohne unerwartete Nebeneffekte zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="fd29b-370">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="fd29b-371">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-371">**Mitigations**</span></span>

<span data-ttu-id="fd29b-372">Sie können das vorherige Verhalten wiederherstellen, indem Sie `DeleteBehavior.ClientNoAction` verwenden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-372">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="fd29b-373">Abfragetypen werden mit Entitätstypen zusammengeführt</span><span class="sxs-lookup"><span data-stu-id="fd29b-373">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="fd29b-374">Issue #14194</span><span class="sxs-lookup"><span data-stu-id="fd29b-374">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="fd29b-375">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-375">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="fd29b-376">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-376">**Old behavior**</span></span>

<span data-ttu-id="fd29b-377">Vor Version 3.0 wurden in EF Core [Abfragetypen](xref:core/modeling/keyless-entity-types) dazu verwendet, Daten abzufragen, in denen kein Primärschlüssel auf strukturierte Weise festgelegt war.</span><span class="sxs-lookup"><span data-stu-id="fd29b-377">Before EF Core 3.0, [query types](xref:core/modeling/keyless-entity-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="fd29b-378">Abfragetypen wurden also für die Zuordnung von Entitätstypen ohne Schlüssel verwendet (in der Regel in einer Sicht, in einigen Fällen aber auch in einer Tabelle), während reguläre Entitätstypen für einen verfügbaren Schlüssel verwendet wurden (in der Regel in einer Tabelle, in einigen Fällen aber auch in einer Sicht).</span><span class="sxs-lookup"><span data-stu-id="fd29b-378">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="fd29b-379">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-379">**New behavior**</span></span>

<span data-ttu-id="fd29b-380">Aus einem Abfragetyp wird nun einfach ein Entitätstyp ohne Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="fd29b-380">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="fd29b-381">Entitätstypen ohne Schlüssel besitzen die gleiche Funktionalität wie Abfragetypen in früheren Versionen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-381">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="fd29b-382">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-382">**Why**</span></span>

<span data-ttu-id="fd29b-383">Diese Änderung wurde vorgenommen, um Unklarheiten bei der Verwendung von Abfragetypen zu beseitigen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-383">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="fd29b-384">Abfragetypen sind schlüssellose Entitätstypen, die grundsätzlich schreibgeschützt sind. Sie sollten allerdings nicht allein wegen des Schreibschutzes verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-384">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="fd29b-385">Des Weiteren werden sie oft Sichten zugeordnet, was aber nur daran liegt, dass für Letztere häufig keine Schlüssel definiert werden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-385">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="fd29b-386">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-386">**Mitigations**</span></span>

<span data-ttu-id="fd29b-387">Die folgenden Teile der API sind durch die Änderungen veraltet:</span><span class="sxs-lookup"><span data-stu-id="fd29b-387">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="fd29b-388">**`ModelBuilder.Query<>()`** : Rufen Sie stattdessen `ModelBuilder.Entity<>().HasNoKey()` auf, um einen schlüssellosen Entitätstyp festzulegen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-388">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="fd29b-389">Dieses Verhalten wird nach wie vor nicht konventionsgemäß festgelegt, um Fehlkonfigurationen zu vermeiden, wenn ein Primärschlüssel erwartet wird, jedoch nicht mit der Konvention übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-389">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="fd29b-390">**`DbQuery<>`** : Verwenden Sie stattdessen `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="fd29b-390">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="fd29b-391">**`DbContext.Query<>()`** : Verwenden Sie stattdessen `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="fd29b-391">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="fd29b-392">Die Konfigurations-API für Beziehungen abhängiger (owned) Typen wurde geändert</span><span class="sxs-lookup"><span data-stu-id="fd29b-392">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="fd29b-393">[Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="fd29b-393">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="fd29b-394">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-394">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="fd29b-395">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-395">**Old behavior**</span></span>

<span data-ttu-id="fd29b-396">Vor Version 3.0 wurde in EF Core die abhängige Beziehung direkt nach dem Aufruf von `OwnsOne` oder `OwnsMany` konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="fd29b-396">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="fd29b-397">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-397">**New behavior**</span></span>

<span data-ttu-id="fd29b-398">Ab Version 3.0 von EF Core ist eine Fluent-API verfügbar, mit der über `WithOwner()` eine Navigationseigenschaft für den Besitzer konfiguriert werden kann.</span><span class="sxs-lookup"><span data-stu-id="fd29b-398">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="fd29b-399">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fd29b-399">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="fd29b-400">Die Konfiguration für die Beziehung zwischen Besitzertyp und abhängigem Typ sollte nun ähnlich wie bei der Konfiguration anderer Beziehungen nach `WithOwner()` verkettet werden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-400">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="fd29b-401">Die Konfiguration für den abhängigen Typ wird jedoch weiterhin nach `OwnsOne()/OwnsMany()` verkettet.</span><span class="sxs-lookup"><span data-stu-id="fd29b-401">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="fd29b-402">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fd29b-402">For example:</span></span>

```C#
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

<span data-ttu-id="fd29b-403">Wenn Sie zusätzlich `Entity()`, `HasOne()`, oder `Set()` für das Ziel eines abhängigen Typs aufrufen, wird keine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="fd29b-403">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="fd29b-404">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-404">**Why**</span></span>

<span data-ttu-id="fd29b-405">Diese Änderung wurde vorgenommen, um eine deutlichere Trennung zwischen der Konfiguration des abhängigen Typs und der _Beziehung zum abhängigen Typ_ zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-405">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="fd29b-406">Dadurch werden für Methoden wie `HasForeignKey` Mehrdeutigkeiten und Unklarheiten beseitigt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-406">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="fd29b-407">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-407">**Mitigations**</span></span>

<span data-ttu-id="fd29b-408">Passen Sie für die Beziehungen von abhängigen Typen die Konfiguration so an, dass die neue Fluent-API wie im obigen Beispiel verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="fd29b-408">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="fd29b-409">Abhängige Entitäten, die die Tabelle gemeinsam mit dem Prinzipal verwenden, sind jetzt optional</span><span class="sxs-lookup"><span data-stu-id="fd29b-409">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="fd29b-410">Issue #9005</span><span class="sxs-lookup"><span data-stu-id="fd29b-410">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="fd29b-411">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-411">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fd29b-412">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-412">**Old behavior**</span></span>

<span data-ttu-id="fd29b-413">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="fd29b-413">Consider the following model:</span></span>
```C#
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
<span data-ttu-id="fd29b-414">Wenn `OrderDetails` im Besitz von `Order` ist oder explizit derselben Tabelle zugeordnet ist, war vor Version 3.0 in EF Core immer eine `OrderDetails`-Instanz beim Hinzufügen eines neuen `Order`-Objekts erforderlich.</span><span class="sxs-lookup"><span data-stu-id="fd29b-414">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="fd29b-415">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-415">**New behavior**</span></span>

<span data-ttu-id="fd29b-416">Ab Version 3.0 bietet EF Core die Möglichkeit, `Order` ohne `OrderDetails` hinzuzufügen, und alle `OrderDetails`-Eigenschaften, mit Ausnahme des primären Schlüssels, werden Spalten zugeordnet, die NULL-Werte zulassen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-416">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="fd29b-417">Bei Abfragen von EF Core wird `OrderDetails` auf `null` festgelegt, wenn eine der erforderlichen Eigenschaften keinen Wert aufweist oder keine erforderlichen Eigenschaften außer dem primären Schlüssel vorhanden und alle Eigenschaften `null` sind.</span><span class="sxs-lookup"><span data-stu-id="fd29b-417">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="fd29b-418">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-418">**Mitigations**</span></span>

<span data-ttu-id="fd29b-419">Wenn Ihr Modell von der gemeinsamen Nutzung einer Tabelle mit allen optionalen Spalten abhängig ist, aber die Navigation, die darauf zeigt, wahrscheinlich nicht `null` ist, sollte die Anwendung für die Handhabung von Fällen geändert werden, in denen die Navigation `null` ist.</span><span class="sxs-lookup"><span data-stu-id="fd29b-419">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="fd29b-420">Wenn das nicht möglich ist, sollte dem Entitätstyp eine erforderliche Eigenschaft hinzugefügt werden oder mindestens einer Eigenschaft sollte ein Wert ungleich `null` zugewiesen sein.</span><span class="sxs-lookup"><span data-stu-id="fd29b-420">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="fd29b-421">Alle Entitäten, die eine Tabelle mit einer Spalte für das Parallelitätstoken gemeinsam verwenden, müssen diese einer Eigenschaft zuordnen</span><span class="sxs-lookup"><span data-stu-id="fd29b-421">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="fd29b-422">Issue #14154</span><span class="sxs-lookup"><span data-stu-id="fd29b-422">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="fd29b-423">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-423">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fd29b-424">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-424">**Old behavior**</span></span>

<span data-ttu-id="fd29b-425">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="fd29b-425">Consider the following model:</span></span>
```C#
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
<span data-ttu-id="fd29b-426">Wenn `OrderDetails` im Besitz von `Order` ist oder explizit derselben Tabelle zugeordnet ist, wird vor Version 3.0 in EF Core durch das alleinige Aktualisieren von `OrderDetails` der `Version`-Wert auf dem Client nicht aktualisiert, und das nächste Update schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="fd29b-426">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="fd29b-427">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-427">**New behavior**</span></span>

<span data-ttu-id="fd29b-428">Ab Version 3.0 gibt EF Core den neuen `Version`-Wert an `Order` weiter, wenn dies Besitzer von `OrderDetails` ist.</span><span class="sxs-lookup"><span data-stu-id="fd29b-428">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="fd29b-429">Andernfalls wird eine Ausnahme während der Modellvalidierung ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="fd29b-429">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="fd29b-430">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-430">**Why**</span></span>

<span data-ttu-id="fd29b-431">Diese Änderung wurde vorgenommen, um einen veralteten Wert für ein Parallelitätstoken zu vermeiden, wenn nur eine der Entitäten, die derselben Tabelle zugeordnet sind, aktualisiert wird.</span><span class="sxs-lookup"><span data-stu-id="fd29b-431">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="fd29b-432">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-432">**Mitigations**</span></span>

<span data-ttu-id="fd29b-433">Alle Entitäten, die die Tabelle gemeinsam nutzen, müssen eine Eigenschaft enthalten, die der Spalte für das Parallelitätstoken zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="fd29b-433">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="fd29b-434">Es ist möglich, eine solche im Schattenzustand zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="fd29b-434">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="fd29b-435">Geerbte Eigenschaften von nicht zugeordneten Typen sind nun einer einzelnen Spalte für alle abgeleiteten Typen zugeordnet</span><span class="sxs-lookup"><span data-stu-id="fd29b-435">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="fd29b-436">Issue #13998</span><span class="sxs-lookup"><span data-stu-id="fd29b-436">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="fd29b-437">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-437">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fd29b-438">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-438">**Old behavior**</span></span>

<span data-ttu-id="fd29b-439">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="fd29b-439">Consider the following model:</span></span>
```C#
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

<span data-ttu-id="fd29b-440">Vor Version 3.0 wurde in EF Core die Eigenschaft `ShippingAddress` standardmäßig separaten Spalten für `BulkOrder` und `Order` zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="fd29b-440">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="fd29b-441">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-441">**New behavior**</span></span>

<span data-ttu-id="fd29b-442">Ab Version 3.0 erstellt EF Core nur eine Spalte für `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="fd29b-442">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="fd29b-443">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-443">**Why**</span></span>

<span data-ttu-id="fd29b-444">Der alte Verhalten war unerwartet.</span><span class="sxs-lookup"><span data-stu-id="fd29b-444">The old behavoir was unexpected.</span></span>

<span data-ttu-id="fd29b-445">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-445">**Mitigations**</span></span>

<span data-ttu-id="fd29b-446">Die Eigenschaft kann noch immer explizit einer separaten Spalte für die abgeleiteten Typen zugeordnet werden:</span><span class="sxs-lookup"><span data-stu-id="fd29b-446">The property can still be explicitly mapped to separate column on the derived types:</span></span>

```C#
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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="fd29b-447">Die Konvention zur Fremdschlüsseleigenschaft entspricht nicht mehr dem Namen der Prinzipaleigenschaft</span><span class="sxs-lookup"><span data-stu-id="fd29b-447">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="fd29b-448">Issue #13274</span><span class="sxs-lookup"><span data-stu-id="fd29b-448">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="fd29b-449">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-449">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="fd29b-450">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-450">**Old behavior**</span></span>

<span data-ttu-id="fd29b-451">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="fd29b-451">Consider the following model:</span></span>
```C#
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
<span data-ttu-id="fd29b-452">Vor Version 3.0 wurde in EF Core gemäß der Konvention die `CustomerId`-Eigenschaft für den Fremdschlüssel verwendet.</span><span class="sxs-lookup"><span data-stu-id="fd29b-452">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="fd29b-453">Wenn `Order` jedoch ein abhängiger Typ ist, wäre `CustomerId` der Primärschlüssel, was normalerweise nicht der Erwartungshaltung entspricht.</span><span class="sxs-lookup"><span data-stu-id="fd29b-453">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="fd29b-454">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-454">**New behavior**</span></span>

<span data-ttu-id="fd29b-455">Ab Version 3.0 werden in EF Core konventionsgemäß keine Eigenschaften mehr für Fremdschlüssel verwendet, wenn diese denselben Namen wie die Prinzipaleigenschaft besitzen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-455">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="fd29b-456">Die Muster für den Namen des Prinzipaltyps, der mit dem Namen der Prinzipaleigenschaft verkettet wird, und für den Navigationsnamen, der mit dem Namen der Prinzipaleigenschaft verkettet wird, werden jedoch weiterhin abgeglichen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-456">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="fd29b-457">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fd29b-457">For example:</span></span>

```C#
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

```C#
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

<span data-ttu-id="fd29b-458">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-458">**Why**</span></span>

<span data-ttu-id="fd29b-459">Diese Änderung wurde vorgenommen, um zu vermeiden, dass versehentlich eine Primärschlüsseleigenschaft für den abhängigen Typ definiert wird.</span><span class="sxs-lookup"><span data-stu-id="fd29b-459">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="fd29b-460">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-460">**Mitigations**</span></span>

<span data-ttu-id="fd29b-461">Wenn die Eigenschaft als Fremdschlüssel und daher Teil des Primärschlüssels vorgesehen war, müssen Sie diese explizit als solche festlegen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-461">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="fd29b-462">Die Datenbankverbindung wird jetzt geschlossen, wenn sie nicht mehr verwendet wird, bevor TransactionScope abgeschlossen wurde</span><span class="sxs-lookup"><span data-stu-id="fd29b-462">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="fd29b-463">Issue #14218</span><span class="sxs-lookup"><span data-stu-id="fd29b-463">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="fd29b-464">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-464">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fd29b-465">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-465">**Old behavior**</span></span>

<span data-ttu-id="fd29b-466">Wenn vor Version 3.0 in EF Core der Kontext die Verbindung in einem `TransactionScope` öffnet, bleibt die Verbindung geöffnet, während der aktuelle `TransactionScope` aktiv ist.</span><span class="sxs-lookup"><span data-stu-id="fd29b-466">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

```C#
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

<span data-ttu-id="fd29b-467">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-467">**New behavior**</span></span>

<span data-ttu-id="fd29b-468">Ab Version 3.0 schließt EF Core die Verbindung, sobald sie nicht mehr verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="fd29b-468">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="fd29b-469">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-469">**Why**</span></span>

<span data-ttu-id="fd29b-470">Diese Änderung ermöglicht die Verwendung mehrerer Kontexte in demselben `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="fd29b-470">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="fd29b-471">Das neue Verhalten entspricht außerdem EF6.</span><span class="sxs-lookup"><span data-stu-id="fd29b-471">The new behavior also matches EF6.</span></span>

<span data-ttu-id="fd29b-472">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-472">**Mitigations**</span></span>

<span data-ttu-id="fd29b-473">Wenn die Verbindung geöffnet bleiben muss, wird durch einen expliziten Aufruf von `OpenConnection()` sichergestellt, dass EF Core diese nicht vorzeitig schließt:</span><span class="sxs-lookup"><span data-stu-id="fd29b-473">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

```C#
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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="fd29b-474">Für jede Eigenschaft wird separat ein ganzzahliger speicherinterner Schlüssel generiert</span><span class="sxs-lookup"><span data-stu-id="fd29b-474">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="fd29b-475">Issue #6872</span><span class="sxs-lookup"><span data-stu-id="fd29b-475">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="fd29b-476">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-476">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fd29b-477">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-477">**Old behavior**</span></span>

<span data-ttu-id="fd29b-478">Vor Version 3.0 wurde in EF Core ein gemeinsam verwendeter Wertegenerator für alle speicherinternen ganzzahligen Schlüsseleigenschaften verwendet.</span><span class="sxs-lookup"><span data-stu-id="fd29b-478">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="fd29b-479">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-479">**New behavior**</span></span>

<span data-ttu-id="fd29b-480">Ab Version 3.0 von EF Core wird für jede ganzzahlige Schlüsseleigenschaft ein eigener Wertegenerator bei der Verwendung der In-Memory Database verwendet.</span><span class="sxs-lookup"><span data-stu-id="fd29b-480">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="fd29b-481">Wenn die Datenbank gelöscht wird, wird die Schlüsselgenerierung für alle Tabellen zurückgesetzt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-481">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="fd29b-482">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-482">**Why**</span></span>

<span data-ttu-id="fd29b-483">Diese Änderung wurde vorgenommen, um die speicherinterne Schlüsselgenerierung enger mit der Schlüsselgenerierung für echte Datenbanken abzustimmen. Außerdem sollen bei Verwendung der In-Memory Database Tests leichter voneinander isoliert werden können.</span><span class="sxs-lookup"><span data-stu-id="fd29b-483">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="fd29b-484">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-484">**Mitigations**</span></span>

<span data-ttu-id="fd29b-485">Wenn Anwendungen von bestimmten speicherinternen Schlüsselwerten abhängig sind, funktionieren Erstere durch die eingeführte Änderung möglicherweise nicht mehr.</span><span class="sxs-lookup"><span data-stu-id="fd29b-485">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="fd29b-486">Versuchen Sie, Abhängigkeiten von bestimmten Schlüsselwerten zu vermeiden, oder passen Sie die Anwendung an das neue Verhalten an.</span><span class="sxs-lookup"><span data-stu-id="fd29b-486">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="fd29b-487">Unterstützungsfelder werden standardmäßig verwendet</span><span class="sxs-lookup"><span data-stu-id="fd29b-487">Backing fields are used by default</span></span>

[<span data-ttu-id="fd29b-488">Issue #12430</span><span class="sxs-lookup"><span data-stu-id="fd29b-488">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="fd29b-489">Diese Änderung wird in Vorschauversion 2 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-489">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="fd29b-490">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-490">**Old behavior**</span></span>

<span data-ttu-id="fd29b-491">Vor Version 3.0 wurde in EF Core mithilfe der Getter- und Settermethoden standardmäßig der Eigenschaftswert gelesen und geschrieben. Das war selbst dann der Fall, wenn das Unterstützungsfeld für eine Eigenschaft nicht bekannt war.</span><span class="sxs-lookup"><span data-stu-id="fd29b-491">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="fd29b-492">Eine Ausnahme war die Abfrageausführung, bei der das Unterstützungsfeld direkt festgelegt wurde, wenn es bekannt war.</span><span class="sxs-lookup"><span data-stu-id="fd29b-492">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="fd29b-493">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-493">**New behavior**</span></span>

<span data-ttu-id="fd29b-494">Ab Version 3.0 wird in EF Core die Eigenschaft mithilfe des Unterstützungsfelds gelesen und geschrieben, wenn dieses für eine Eigenschaft bekannt ist.</span><span class="sxs-lookup"><span data-stu-id="fd29b-494">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="fd29b-495">Dies kann dazu führen, dass eine Anwendung nicht mehr funktioniert, wenn sie auf zusätzliches Verhalten angewiesen ist, das im Code der Getter- und Settermethoden definiert ist.</span><span class="sxs-lookup"><span data-stu-id="fd29b-495">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="fd29b-496">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-496">**Why**</span></span>

<span data-ttu-id="fd29b-497">Diese Änderung wurde vorgenommen, um zu verhindern, dass in EF Core fälschlicherweise immer dann Geschäftslogik ausgelöst wird, wenn Datenbankvorgänge für Entitäten durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-497">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="fd29b-498">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-498">**Mitigations**</span></span>

<span data-ttu-id="fd29b-499">Sie können das Verhalten von vor Version 3.0 wiederherstellen, indem Sie in `ModelBuilder` den Zugriffsmodus für die Eigenschaft konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="fd29b-499">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="fd29b-500">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fd29b-500">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="fd29b-501">Eine Ausnahme wird ausgelöst, wenn mehrere kompatible Unterstützungsfelder gefunden werden</span><span class="sxs-lookup"><span data-stu-id="fd29b-501">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="fd29b-502">Issue #12523</span><span class="sxs-lookup"><span data-stu-id="fd29b-502">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="fd29b-503">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-503">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fd29b-504">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-504">**Old behavior**</span></span>

<span data-ttu-id="fd29b-505">Vor Version 3.0 wurde in EF Core ein Unterstützungsfeld auf Grundlage einer Rangfolge ausgewählt, wenn die Regeln zum Suchen eines Eigenschaftsfelds auf mehrere Felder zutrafen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-505">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="fd29b-506">Dabei konnte es vorkommen, dass in mehrdeutigen Fällen das falsche Feld verwendet wurde.</span><span class="sxs-lookup"><span data-stu-id="fd29b-506">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="fd29b-507">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-507">**New behavior**</span></span>

<span data-ttu-id="fd29b-508">Ab Version 3.0 wird in EF Core eine Ausnahme ausgelöst, wenn sich mehrere Felder auf dieselbe Eigenschaft beziehen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-508">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="fd29b-509">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-509">**Why**</span></span>

<span data-ttu-id="fd29b-510">Diese Änderung wurde vorgenommen, um zu vermeiden, dass automatisch ein falsches Feld verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="fd29b-510">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="fd29b-511">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-511">**Mitigations**</span></span>

<span data-ttu-id="fd29b-512">Für Eigenschaften mit mehrdeutigen Unterstützungsfeldern muss das zu verwendende Feld explizit festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-512">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="fd29b-513">Mit der Fluent-API ist dies beispielsweise wie folgt möglich:</span><span class="sxs-lookup"><span data-stu-id="fd29b-513">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="fd29b-514">„Nur-Feld“-Eigenschaftsnamen sollten dem Feldnamen entsprechen</span><span class="sxs-lookup"><span data-stu-id="fd29b-514">Field-only property names should match the field name</span></span>

<span data-ttu-id="fd29b-515">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-515">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fd29b-516">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-516">**Old behavior**</span></span>

<span data-ttu-id="fd29b-517">Vor Version 3.0 konnte in EF Core eine Eigenschaft durch einen Zeichenfolgenwert angegeben werden, und wenn keine Eigenschaft mit diesem Namen im .NET-Typ gefunden wurde, versuchte EF Core, mithilfe von Konventionsregeln ein übereinstimmendes Feld zu finden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-517">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the .NET type then EF Core would try to match it to a field using convention rules.</span></span>
```C#
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```
```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

<span data-ttu-id="fd29b-518">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-518">**New behavior**</span></span>

<span data-ttu-id="fd29b-519">Ab Version 3.0 muss in EF Core eine „Nur-Feld“-Eigenschaft mit dem Namen des Felds genau übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-519">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="fd29b-520">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-520">**Why**</span></span>

<span data-ttu-id="fd29b-521">Diese Änderung wurde vorgenommen, um zu vermeiden, dass das gleiche Feld für zwei Eigenschaften mit ähnlichem Namen verwendet wird. Durch die Änderung entsprechen nun auch die Übereinstimmungsregeln für „Nur-Feld“-Eigenschaften den Regeln für Eigenschaften, die CLR-Eigenschaften zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="fd29b-521">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="fd29b-522">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-522">**Mitigations**</span></span>

<span data-ttu-id="fd29b-523">„Nur-Feld“-Eigenschaften müssen so benannt werden wie das Feld, dem sie zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="fd29b-523">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="fd29b-524">In einer kommenden Version von EF Core nach 3.0 soll das explizite Konfigurieren eines Feldnamens, der sich vom Eigenschaftsnamen unterscheidet, erneut aktiviert werden (siehe Problem [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span><span class="sxs-lookup"><span data-stu-id="fd29b-524">In a future release of EF Core after 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name (see issue [#15307](https://github.com/aspnet/EntityFrameworkCore/issues/15307)):</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="fd29b-525">Von AddDbContext/AddDbContextPool werden AddLogging und AddMemoryCache nicht mehr aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-525">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="fd29b-526">Issue #14756</span><span class="sxs-lookup"><span data-stu-id="fd29b-526">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="fd29b-527">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-527">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fd29b-528">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-528">**Old behavior**</span></span>

<span data-ttu-id="fd29b-529">Vor EF Core 3.0 wurden beim Aufruf von `AddDbContext` oder `AddDbContextPool` durch Aufrufe von [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) und [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache) auch Protokollierungs- und Arbeitsspeichercaching-Dienste bei DI registriert.</span><span class="sxs-lookup"><span data-stu-id="fd29b-529">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="fd29b-530">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-530">**New behavior**</span></span>

<span data-ttu-id="fd29b-531">Ab EF Core 3.0 werden diese Dienste durch `AddDbContext` und `AddDbContextPool` nicht mehr bei Dependency Injection (DI) registriert.</span><span class="sxs-lookup"><span data-stu-id="fd29b-531">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="fd29b-532">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-532">**Why**</span></span>

<span data-ttu-id="fd29b-533">Für EF Core 3.0 ist es nicht erforderlich, dass diese Dienste im DI-Container der Anwendung enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="fd29b-533">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="fd29b-534">Wenn `ILoggerFactory` jedoch im DI-Container der Anwendung registriert ist, wird sie nach wie vor von EF Core verwendet.</span><span class="sxs-lookup"><span data-stu-id="fd29b-534">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="fd29b-535">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-535">**Mitigations**</span></span>

<span data-ttu-id="fd29b-536">Wenn Ihre Anwendung diese Dienste benötigt, registrieren Sie sie explizit mit [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) oder [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache) beim DI-Container.</span><span class="sxs-lookup"><span data-stu-id="fd29b-536">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="fd29b-537">DbContext.Entry erkennt nun mit DetectChanges lokale Änderungen</span><span class="sxs-lookup"><span data-stu-id="fd29b-537">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="fd29b-538">Issue #13552</span><span class="sxs-lookup"><span data-stu-id="fd29b-538">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="fd29b-539">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-539">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="fd29b-540">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-540">**Old behavior**</span></span>

<span data-ttu-id="fd29b-541">Vor Version 3.0 führte in EF Core ein Aufruf von `DbContext.Entry` dazu, dass Änderungen an allen überwachten Entitäten erkannt wurden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-541">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="fd29b-542">Dadurch wurde sichergestellt, dass der Zustand in `EntityEntry` immer aktuell war.</span><span class="sxs-lookup"><span data-stu-id="fd29b-542">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="fd29b-543">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-543">**New behavior**</span></span>

<span data-ttu-id="fd29b-544">Ab Version 3.0 werden in EF Core Änderungen bei einem Aufruf von `DbContext.Entry` nur in der angegebenen Entität und in allen überwachten Prinzipalentitäten erkannt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-544">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="fd29b-545">Änderungen an anderer Stelle werden daher durch den Aufruf dieser Methode möglicherweise nicht erkannt, was sich auf den Anwendungszustand auswirken kann.</span><span class="sxs-lookup"><span data-stu-id="fd29b-545">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="fd29b-546">Beachten Sie, dass selbst die Erkennung lokaler Änderungen deaktiviert wird, wenn `ChangeTracker.AutoDetectChangesEnabled` auf `false` festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="fd29b-546">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="fd29b-547">Andere Methoden wie `ChangeTracker.Entries` und `SaveChanges`, die eine Änderungserkennung auslösen, führen nach wie vor zu einem vollständigen `DetectChanges`-Vorgang für alle überwachten Entitäten.</span><span class="sxs-lookup"><span data-stu-id="fd29b-547">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="fd29b-548">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-548">**Why**</span></span>

<span data-ttu-id="fd29b-549">Diese Änderung wurde vorgenommen, um die Leistung bei der standardmäßigen Verwendung von `context.Entry` zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="fd29b-549">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="fd29b-550">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-550">**Mitigations**</span></span>

<span data-ttu-id="fd29b-551">Rufen Sie `ChgangeTracker.DetectChanges()` explizit vor dem Aufruf von `Entry` auf, wenn das Verhalten vor Version 3.0 beibehalten werden soll.</span><span class="sxs-lookup"><span data-stu-id="fd29b-551">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="fd29b-552">Schlüssel aus Zeichenfolgen und Bytearrays werden standardmäßig nicht vom Client generiert</span><span class="sxs-lookup"><span data-stu-id="fd29b-552">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="fd29b-553">Issue #14617</span><span class="sxs-lookup"><span data-stu-id="fd29b-553">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="fd29b-554">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-554">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fd29b-555">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-555">**Old behavior**</span></span>

<span data-ttu-id="fd29b-556">Vor Version 3.0 konnten in EF Core `string`- und `byte[]`-Schlüsseleigenschaften verwendet werden, ohne dass explizit ein anderer Wert als NULL festgelegt werden musste.</span><span class="sxs-lookup"><span data-stu-id="fd29b-556">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="fd29b-557">In diesem Fall wurde der Schlüsselwert auf dem Client als GUID generiert und für `byte[]` in Bytes serialisiert.</span><span class="sxs-lookup"><span data-stu-id="fd29b-557">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="fd29b-558">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-558">**New behavior**</span></span>

<span data-ttu-id="fd29b-559">Ab Version 3.0 wird in EF Core eine Ausnahme ausgelöst, wenn kein Schlüsselwert festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="fd29b-559">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="fd29b-560">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-560">**Why**</span></span>

<span data-ttu-id="fd29b-561">Diese Änderung wurde vorgenommen, da vom Client generierte `string`/`byte[]`-Werte in der Regel nicht sinnvoll sind. Das Standardverhalten führte außerdem zu kaum nachvollziehbaren generierten Schlüsselwerten.</span><span class="sxs-lookup"><span data-stu-id="fd29b-561">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="fd29b-562">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-562">**Mitigations**</span></span>

<span data-ttu-id="fd29b-563">Wenn Sie das Verhalten vor Version 3.0 nutzen möchten, müssen Sie explizit festlegen, dass für die Schlüsseleigenschaften generierte Werte verwendet sollen, wenn kein anderer Nicht-NULL-Wert festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="fd29b-563">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="fd29b-564">Dies ist beispielsweise mit der Fluent-API möglich:</span><span class="sxs-lookup"><span data-stu-id="fd29b-564">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="fd29b-565">Alternativ bieten sich auch Datenanmerkungen an:</span><span class="sxs-lookup"><span data-stu-id="fd29b-565">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="fd29b-566">ILoggerFactory ist nun ein bereichsbezogener Dienst</span><span class="sxs-lookup"><span data-stu-id="fd29b-566">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="fd29b-567">Issue #14698</span><span class="sxs-lookup"><span data-stu-id="fd29b-567">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="fd29b-568">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-568">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="fd29b-569">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-569">**Old behavior**</span></span>

<span data-ttu-id="fd29b-570">Vor Version 3.0 wurde `ILoggerFactory` in EF Core als Singletondienst registriert.</span><span class="sxs-lookup"><span data-stu-id="fd29b-570">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="fd29b-571">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-571">**New behavior**</span></span>

<span data-ttu-id="fd29b-572">Ab Version 3.0 wird in EF Core `ILoggerFactory` als bereichsbezogener Dienst registriert.</span><span class="sxs-lookup"><span data-stu-id="fd29b-572">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="fd29b-573">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-573">**Why**</span></span>

<span data-ttu-id="fd29b-574">Diese Änderung wurde vorgenommen, um einer `DbContext`-Instanz eine Protokollierung zuordnen zu können. Dadurch werden weitere Funktionen bereitgestellt. In einigen Fällen wird außerdem schädliches Verhalten wie etwa die schnelle Zunahme von internen Dienstanbietern beseitigt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-574">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="fd29b-575">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-575">**Mitigations**</span></span>

<span data-ttu-id="fd29b-576">Diese Änderung wirkt sich nur dann auf den Anwendungscode aus, wenn benutzerdefinierte Dienste auf dem internen Dienstanbieter von EF Core registriert und verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-576">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="fd29b-577">Ein solches Szenario ist aber unüblich.</span><span class="sxs-lookup"><span data-stu-id="fd29b-577">This isn't common.</span></span>
<span data-ttu-id="fd29b-578">In einem derartigen Fall funktioniert fast alles wie vorher. Singletondienste, die von `ILoggerFactory` abhängig sind, müssen jedoch so angepasst werden, dass `ILoggerFactory` auf andere Weise abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="fd29b-578">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="fd29b-579">Erstellen Sie in diesen Situationen bitte im [GitHub-Issuetracker für EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) ein Issue, in dem Sie beschreiben, wie Sie `ILoggerFactory` verwenden. So können wir in Zukunft erneute Breaking Changes vermeiden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-579">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="fd29b-580">Für Proxys mit Lazy Loading (trägem Laden) gilt nicht mehr die Annahme, dass Navigationseigenschaften vollständig geladen sind</span><span class="sxs-lookup"><span data-stu-id="fd29b-580">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="fd29b-581">Issue #12780</span><span class="sxs-lookup"><span data-stu-id="fd29b-581">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="fd29b-582">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-582">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fd29b-583">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-583">**Old behavior**</span></span>

<span data-ttu-id="fd29b-584">Vor Version 3.0 gab es in EF Core nach der Freigabe eines `DbContext`-Objekts keine Möglichkeit, zu ermitteln, ob eine aus dem Kontext abgerufene Navigationseigenschaft für eine Entität vollständig geladen war.</span><span class="sxs-lookup"><span data-stu-id="fd29b-584">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="fd29b-585">Stattdessen galt für Proxys die Annahme, dass eine Verweisnavigation geladen ist, falls für diese ein anderer Wert als NULL vorliegt. Außerdem wurde davon ausgegangen, dass eine Sammlungsnavigation geladen ist, wenn sie nicht leer ist.</span><span class="sxs-lookup"><span data-stu-id="fd29b-585">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="fd29b-586">In diesen Fällen entsprach das Lazy Loading einer Nulloperation („No-Op“).</span><span class="sxs-lookup"><span data-stu-id="fd29b-586">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="fd29b-587">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-587">**New behavior**</span></span>

<span data-ttu-id="fd29b-588">Ab Version 3.0 überwachen in EF Core Proxys, ob eine Navigationseigenschaft geladen ist.</span><span class="sxs-lookup"><span data-stu-id="fd29b-588">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="fd29b-589">Wenn also auf eine Navigationseigenschaft zugegriffen werden soll, die nach der Freigabe des Kontexts geladen wird, ist dies immer eine No-Op-Operation. Das ist selbst dann der Fall, wenn die geladene Navigation leer oder NULL ist.</span><span class="sxs-lookup"><span data-stu-id="fd29b-589">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="fd29b-590">Wenn umgekehrt auf eine nicht geladene Navigationseigenschaft zugegriffen werden soll und der Kontext bereits freigegeben wurde, wird eine Ausnahme ausgelöst. Dieser Fall tritt auch dann ein, wenn die Navigationseigenschaft eine nicht leere Sammlung ist.</span><span class="sxs-lookup"><span data-stu-id="fd29b-590">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="fd29b-591">In dieser Situation wird also vom Anwendungscode versucht, zu einem ungültigen Zeitpunkt Lazy Loading durchzuführen. Die Anwendung sollte so angepasst werden, dass dieser Vorgang vermieden wird.</span><span class="sxs-lookup"><span data-stu-id="fd29b-591">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="fd29b-592">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-592">**Why**</span></span>

<span data-ttu-id="fd29b-593">Diese Änderung wurde vorgenommen, um das Verhalten beim Lazy Loading einer freigegebenen `DbContext`-Instanz konsistent und korrekt zu gestalten.</span><span class="sxs-lookup"><span data-stu-id="fd29b-593">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="fd29b-594">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-594">**Mitigations**</span></span>

<span data-ttu-id="fd29b-595">Aktualisieren Sie den Anwendungscode so, dass kein Lazy Loading durchgeführt wird, nachdem der Kontext freigegeben wurde. Alternativ können Sie auch, wie in der Ausnahmemeldung beschrieben wird, eine No-Op-Operation festlegen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-595">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="fd29b-596">Die Erstellung zu vieler interner Dienstanbieter führt standardmäßig zu einem Fehler</span><span class="sxs-lookup"><span data-stu-id="fd29b-596">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="fd29b-597">Issue #10236</span><span class="sxs-lookup"><span data-stu-id="fd29b-597">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="fd29b-598">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-598">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="fd29b-599">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-599">**Old behavior**</span></span>

<span data-ttu-id="fd29b-600">Vor Version 3.0 wurde in EF Core eine Warnung für eine Anwendung protokolliert, die unverhältnismäßig viele interne Dienstanbieter erstellte.</span><span class="sxs-lookup"><span data-stu-id="fd29b-600">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="fd29b-601">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-601">**New behavior**</span></span>

<span data-ttu-id="fd29b-602">Ab Version 3.0 wird diese Warnung in EF Core als Fehler betrachtet, und eine Ausnahme wird ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="fd29b-602">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="fd29b-603">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-603">**Why**</span></span>

<span data-ttu-id="fd29b-604">Diese Änderung wurde vorgenommen, damit auf den oben beschriebenen Fall explizit hingewiesen wird. So soll die Entwicklung von besserem Anwendungscode erleichtert werden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-604">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="fd29b-605">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-605">**Mitigations**</span></span>

<span data-ttu-id="fd29b-606">Wenn dieser Fehler auftritt, müssen Sie die Grundursache ermitteln und die Erstellung vieler interner Dienstanbieter unterbinden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-606">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="fd29b-607">Sie können den Fehler allerdings auch wieder in eine Warnung konvertierten (oder ihn ignorieren), indem Sie `DbContextOptionsBuilder` konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="fd29b-607">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="fd29b-608">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fd29b-608">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="fd29b-609">Neues Verhalten für HasOne/HasMany bei Aufruf mit einer einzelnen Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="fd29b-609">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="fd29b-610">Issue #9171</span><span class="sxs-lookup"><span data-stu-id="fd29b-610">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="fd29b-611">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-611">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fd29b-612">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-612">**Old behavior**</span></span>

<span data-ttu-id="fd29b-613">Vor EF Core 3.0 wurde Code, der `HasOne` oder `HasMany` mit einer einzelnen Zeichenfolge aufruft, auf irritierende Weise interpretiert.</span><span class="sxs-lookup"><span data-stu-id="fd29b-613">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="fd29b-614">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fd29b-614">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="fd29b-615">Der Code scheint `Samurai` mit einem anderen Entitätstyp über die `Entrance`-Navigationseigenschaft zu verknüpfen, die möglicherweise privat ist.</span><span class="sxs-lookup"><span data-stu-id="fd29b-615">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="fd29b-616">Tatsächlich versucht dieser Code, eine Beziehung zu einem Entitätstyp namens `Entrance` ohne Navigationseigenschaft herzustellen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-616">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="fd29b-617">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-617">**New behavior**</span></span>

<span data-ttu-id="fd29b-618">Ab EF Core 3.0 führt der obige Code jetzt die erwartete Aktion durch.</span><span class="sxs-lookup"><span data-stu-id="fd29b-618">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="fd29b-619">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-619">**Why**</span></span>

<span data-ttu-id="fd29b-620">Das alte Verhalten war sehr verwirrend, vor allem beim Lesen des Konfigurationscodes und bei der Suche nach Fehlern.</span><span class="sxs-lookup"><span data-stu-id="fd29b-620">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="fd29b-621">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-621">**Mitigations**</span></span>

<span data-ttu-id="fd29b-622">Dies führt nur zu Fehlern in Anwendungen, die Beziehungen explizit unter Verwendung von Zeichenfolgen für Typnamen und ohne explizite Angabe der Navigationseigenschaft konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="fd29b-622">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="fd29b-623">Dies ist nicht üblich.</span><span class="sxs-lookup"><span data-stu-id="fd29b-623">This is not common.</span></span>
<span data-ttu-id="fd29b-624">Das vorherige Verhalten kann durch explizite Übergabe von `null` für den Namen der Navigationseigenschaft erzielt werden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-624">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="fd29b-625">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fd29b-625">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="fd29b-626">Der Rückgabetyp für mehrere asynchrone Methoden wurde von Task in ValueTask geändert</span><span class="sxs-lookup"><span data-stu-id="fd29b-626">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="fd29b-627">Issue #15184</span><span class="sxs-lookup"><span data-stu-id="fd29b-627">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="fd29b-628">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-628">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fd29b-629">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-629">**Old behavior**</span></span>

<span data-ttu-id="fd29b-630">Die folgenden asynchronen Methoden gaben zuvor `Task<T>` zurück:</span><span class="sxs-lookup"><span data-stu-id="fd29b-630">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="fd29b-631">`ValueGenerator.NextValueAsync()` (und abgeleitete Klassen)</span><span class="sxs-lookup"><span data-stu-id="fd29b-631">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="fd29b-632">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-632">**New behavior**</span></span>

<span data-ttu-id="fd29b-633">Die oben genannten Methoden geben nun `ValueTask<T>` über dieselbe `T` wie zuvor zurück.</span><span class="sxs-lookup"><span data-stu-id="fd29b-633">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="fd29b-634">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-634">**Why**</span></span>

<span data-ttu-id="fd29b-635">Durch diese Änderung verringert sich die Anzahl der Heapzuordnungen, die beim Aufrufen dieser Methoden entstehen, und dies führt zu einer Verbesserung der allgemeinen Leistung.</span><span class="sxs-lookup"><span data-stu-id="fd29b-635">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="fd29b-636">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-636">**Mitigations**</span></span>

<span data-ttu-id="fd29b-637">Anwendungen, die einfach die oben genannten APIs erwarten, müssen nur neu kompiliert werden – Quelländerungen sind nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="fd29b-637">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="fd29b-638">Eine komplexere Verwendung (z.B. Übergeben der zurückgegebenen `Task` an `Task.WhenAny()`) erfordern in der Regel, dass die zurückgegebene `ValueTask<T>` durch einen Aufruf von `AsTask()` in `Task<T>` konvertiert wird.</span><span class="sxs-lookup"><span data-stu-id="fd29b-638">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="fd29b-639">Beachten Sie, dass dadurch die mit dieser Änderung verbundene Verringerung der Zuordnungen aufgehoben wird.</span><span class="sxs-lookup"><span data-stu-id="fd29b-639">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="fd29b-640">Die Anmerkung Relational:TypeMapping heißt nun nur noch TypeMapping</span><span class="sxs-lookup"><span data-stu-id="fd29b-640">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="fd29b-641">Issue #9913</span><span class="sxs-lookup"><span data-stu-id="fd29b-641">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="fd29b-642">Diese Änderung wird in Vorschauversion 2 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-642">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="fd29b-643">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-643">**Old behavior**</span></span>

<span data-ttu-id="fd29b-644">Der Anmerkungsname für Typzuordnungsanmerkungen war bisher Relational:TypeMapping.</span><span class="sxs-lookup"><span data-stu-id="fd29b-644">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="fd29b-645">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-645">**New behavior**</span></span>

<span data-ttu-id="fd29b-646">Der Anmerkungsname für Typzuordnungsanmerkungen lautet nun TypeMapping.</span><span class="sxs-lookup"><span data-stu-id="fd29b-646">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="fd29b-647">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-647">**Why**</span></span>

<span data-ttu-id="fd29b-648">Typzuordnungen werden nicht mehr ausschließlich für Anbieter relationaler Datenbanken verwendet.</span><span class="sxs-lookup"><span data-stu-id="fd29b-648">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="fd29b-649">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-649">**Mitigations**</span></span>

<span data-ttu-id="fd29b-650">Diese Änderung ist nur dann ein Breaking Change, wenn Anwendungen direkt über eine Anmerkung auf die Typzuordnung zugreifen. Dieses Szenario ist jedoch unüblich.</span><span class="sxs-lookup"><span data-stu-id="fd29b-650">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="fd29b-651">Verzichten Sie daher darauf, die Anmerkung direkt zu verwenden, und greifen Sie stattdessen über die Fluent-API auf Typzuordnungen zu, um das Problem zu beheben.</span><span class="sxs-lookup"><span data-stu-id="fd29b-651">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="fd29b-652">ToTable löst für einen abgeleiteten Typ eine Ausnahme aus</span><span class="sxs-lookup"><span data-stu-id="fd29b-652">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="fd29b-653">Issue #11811</span><span class="sxs-lookup"><span data-stu-id="fd29b-653">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="fd29b-654">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-654">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="fd29b-655">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-655">**Old behavior**</span></span>

<span data-ttu-id="fd29b-656">Vor Version 3.0 wurde in EF Core der Aufruf von `ToTable()` für einen abgeleiteten Typ ignoriert, da TPH die einzige Strategie zur Vererbungszuordnung war und diese in unzulässigen Fällen angewendet wurde.</span><span class="sxs-lookup"><span data-stu-id="fd29b-656">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="fd29b-657">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-657">**New behavior**</span></span>

<span data-ttu-id="fd29b-658">Ab Version 3.0 wird in EF Core eine Ausnahme ausgelöst, um später eine unerwartete Zuordnung zu vermeiden, wenn `ToTable()` für einen abgeleiteten Typ aufgerufen wird. Dieser Schritt dient auch als Vorbereitung für die TPT- und TPC-Unterstützung, die in einem künftigen Release folgen wird.</span><span class="sxs-lookup"><span data-stu-id="fd29b-658">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="fd29b-659">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-659">**Why**</span></span>

<span data-ttu-id="fd29b-660">Derzeit ist es nicht zulässig, einen abgeleiteten Typ einer anderen Tabelle zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-660">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="fd29b-661">Durch diese Änderung werden Breaking Changes vermieden, wenn der beschriebene Vorgang in Zukunft zulässig wird.</span><span class="sxs-lookup"><span data-stu-id="fd29b-661">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="fd29b-662">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-662">**Mitigations**</span></span>

<span data-ttu-id="fd29b-663">Entfernen Sie alle Zuordnungen von abgeleiteten Typen zu anderen Tabellen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-663">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="fd29b-664">ForSqlServerHasIndex wurde durch HasIndex ersetzt</span><span class="sxs-lookup"><span data-stu-id="fd29b-664">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="fd29b-665">Issue #12366</span><span class="sxs-lookup"><span data-stu-id="fd29b-665">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="fd29b-666">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-666">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="fd29b-667">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-667">**Old behavior**</span></span>

<span data-ttu-id="fd29b-668">Vor Version 3.0 konnten in EF Core mit `ForSqlServerHasIndex().ForSqlServerInclude()` Spalten konfiguriert werden, die mit `INCLUDE` verwendet wurden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-668">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="fd29b-669">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-669">**New behavior**</span></span>

<span data-ttu-id="fd29b-670">Ab Version 3.0 wird in EF Core die Nutzung von `Include` für einen Index auf der relationalen Ebene unterstützt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-670">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="fd29b-671">Verwenden Sie `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="fd29b-671">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="fd29b-672">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-672">**Why**</span></span>

<span data-ttu-id="fd29b-673">Diese Änderung wurde vorgenommen, um die API für Indizes mit `Include` an einer zentralen Stelle für alle Datenbankanbieter zusammenzuführen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-673">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="fd29b-674">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-674">**Mitigations**</span></span>

<span data-ttu-id="fd29b-675">Verwenden Sie wie oben beschrieben die neue API.</span><span class="sxs-lookup"><span data-stu-id="fd29b-675">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="fd29b-676">Metadaten-API-Änderungen</span><span class="sxs-lookup"><span data-stu-id="fd29b-676">Metadata API changes</span></span>

[<span data-ttu-id="fd29b-677">Issue #214</span><span class="sxs-lookup"><span data-stu-id="fd29b-677">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="fd29b-678">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-678">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fd29b-679">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-679">**New behavior**</span></span>

<span data-ttu-id="fd29b-680">Die folgenden Eigenschaften wurden in Erweiterungsmethoden konvertiert:</span><span class="sxs-lookup"><span data-stu-id="fd29b-680">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="fd29b-681">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-681">**Why**</span></span>

<span data-ttu-id="fd29b-682">Mit dieser Änderung wird die Implementierung der oben genannten Schnittstellen vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="fd29b-682">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="fd29b-683">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-683">**Mitigations**</span></span>

<span data-ttu-id="fd29b-684">Verwenden Sie die neuen Erweiterungsmethoden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-684">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="fd29b-685">Anbieterspezifische Metadaten-API-Änderungen</span><span class="sxs-lookup"><span data-stu-id="fd29b-685">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="fd29b-686">Issue #214</span><span class="sxs-lookup"><span data-stu-id="fd29b-686">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="fd29b-687">Diese Änderung wird in Vorschauversion 6 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-687">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="fd29b-688">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-688">**New behavior**</span></span>

<span data-ttu-id="fd29b-689">Die anbieterspezifischen Erweiterungsmethoden werden vereinfacht:</span><span class="sxs-lookup"><span data-stu-id="fd29b-689">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="fd29b-690">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-690">**Why**</span></span>

<span data-ttu-id="fd29b-691">Mit dieser Änderung wird die Implementierung der oben genannten Erweiterungsmethoden vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="fd29b-691">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="fd29b-692">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-692">**Mitigations**</span></span>

<span data-ttu-id="fd29b-693">Verwenden Sie die neuen Erweiterungsmethoden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-693">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="fd29b-694">EF Core sendet keine PRAGMA-Anweisungen mehr, um Fremdschlüssel in SQLite zu erzwingen</span><span class="sxs-lookup"><span data-stu-id="fd29b-694">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="fd29b-695">Issue #12151</span><span class="sxs-lookup"><span data-stu-id="fd29b-695">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="fd29b-696">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-696">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="fd29b-697">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-697">**Old behavior**</span></span>

<span data-ttu-id="fd29b-698">Vor Version 3.0 wurden in EF Core `PRAGMA foreign_keys = 1`-Anweisungen gesendet, wenn eine Verbindung mit SQLite hergestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="fd29b-698">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="fd29b-699">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-699">**New behavior**</span></span>

<span data-ttu-id="fd29b-700">Ab Version 3.0 werden in EF Core keine `PRAGMA foreign_keys = 1`-Anweisungen mehr gesendet, wenn eine Verbindung mit SQLite hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="fd29b-700">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="fd29b-701">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-701">**Why**</span></span>

<span data-ttu-id="fd29b-702">Diese Änderung wurde vorgenommen, da von EF Core standardmäßig `SQLitePCLRaw.bundle_e_sqlite3` verwendet wird. Dadurch ist die Erzwingung von Fremdschlüsseln standardmäßig aktiviert und muss nicht jedes Mal explizit aktiviert werden, wenn eine Verbindung hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="fd29b-702">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="fd29b-703">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-703">**Mitigations**</span></span>

<span data-ttu-id="fd29b-704">Fremdschlüssel sind in SQLitePCLRaw.bundle_e_sqlite3 standardmäßig aktiviert. Dieses Bundle wird standardmäßig von EF Core verwendet.</span><span class="sxs-lookup"><span data-stu-id="fd29b-704">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="fd29b-705">In anderen Fällen können Fremdschlüssel durch die Angabe `Foreign Keys=True` in der Verbindungszeichenfolge aktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-705">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="fd29b-706">SQLitePCLRaw.bundle_e_sqlite3 ist nun eine Abhängigkeit von Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="fd29b-706">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="fd29b-707">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-707">**Old behavior**</span></span>

<span data-ttu-id="fd29b-708">Vor Version 3.0 wurde von EF Core `SQLitePCLRaw.bundle_green` verwendet.</span><span class="sxs-lookup"><span data-stu-id="fd29b-708">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="fd29b-709">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-709">**New behavior**</span></span>

<span data-ttu-id="fd29b-710">Ab Version 3.0 wird von EF Core `SQLitePCLRaw.bundle_e_sqlite3` verwendet.</span><span class="sxs-lookup"><span data-stu-id="fd29b-710">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="fd29b-711">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-711">**Why**</span></span>

<span data-ttu-id="fd29b-712">Diese Änderung wurde vorgenommen, damit die unter iOS verwendete Version von SQLite sich konsistent zu Versionen auf anderen Plattformen verhält.</span><span class="sxs-lookup"><span data-stu-id="fd29b-712">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="fd29b-713">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-713">**Mitigations**</span></span>

<span data-ttu-id="fd29b-714">Konfigurieren Sie `Microsoft.Data.Sqlite` so, dass ein anderes `SQLitePCLRaw`-Bundle verwendet wird, um die native SQLite-Version unter iOS zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-714">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="fd29b-715">GUID-Werte werden jetzt als TEXT in SQLite gespeichert</span><span class="sxs-lookup"><span data-stu-id="fd29b-715">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="fd29b-716">Issue #15078</span><span class="sxs-lookup"><span data-stu-id="fd29b-716">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="fd29b-717">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-717">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fd29b-718">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-718">**Old behavior**</span></span>

<span data-ttu-id="fd29b-719">GUID-Werte wurden in der Vergangenheit in SQLite als BLOB-Werte gespeichert.</span><span class="sxs-lookup"><span data-stu-id="fd29b-719">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="fd29b-720">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-720">**New behavior**</span></span>

<span data-ttu-id="fd29b-721">GUID-Werte werden jetzt als TEXT gespeichert.</span><span class="sxs-lookup"><span data-stu-id="fd29b-721">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="fd29b-722">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-722">**Why**</span></span>

<span data-ttu-id="fd29b-723">Das Binärformat der GUIDs ist nicht standardisiert.</span><span class="sxs-lookup"><span data-stu-id="fd29b-723">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="fd29b-724">Das Speichern der Werte als TEXT führt dazu, dass die Datenbank mit anderen Technologien eher kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="fd29b-724">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="fd29b-725">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-725">**Mitigations**</span></span>

<span data-ttu-id="fd29b-726">Sie können vorhandene Datenbanken zum neuen Format migrieren, indem Sie SQL-Code wie den folgenden ausführen:</span><span class="sxs-lookup"><span data-stu-id="fd29b-726">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="fd29b-727">Sie können in EF Core auch weiterhin das alte Verhalten verwenden, indem Sie einen Wertkonverter für diese Eigenschaften konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="fd29b-727">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="fd29b-728">Microsoft.Data.Sqlite kann weiterhin GUID-Werte aus BLOB- und TEXT-Spalten lesen. Da sich jedoch das Standardformat für Parameter und Konstanten geändert hat, müssen Sie wahrscheinlich Aktionen für die meisten GUID-Szenarios vornehmen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-728">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="fd29b-729">Char-Werte werden jetzt als TEXT in SQLite gespeichert</span><span class="sxs-lookup"><span data-stu-id="fd29b-729">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="fd29b-730">Issue #15020</span><span class="sxs-lookup"><span data-stu-id="fd29b-730">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="fd29b-731">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-731">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fd29b-732">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-732">**Old behavior**</span></span>

<span data-ttu-id="fd29b-733">Char-Werte wurden in der Vergangenheit in SQLite als INTEGER-Werte gespeichert.</span><span class="sxs-lookup"><span data-stu-id="fd29b-733">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="fd29b-734">Der Char-Wert *A* wurde z.B. als INTEGER-Wert 65 gespeichert.</span><span class="sxs-lookup"><span data-stu-id="fd29b-734">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="fd29b-735">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-735">**New behavior**</span></span>

<span data-ttu-id="fd29b-736">Char-Werte werden jetzt als TEXT gespeichert.</span><span class="sxs-lookup"><span data-stu-id="fd29b-736">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="fd29b-737">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-737">**Why**</span></span>

<span data-ttu-id="fd29b-738">Das Speichern der Werte als TEXT ist naheliegender und führt dazu, dass die Datenbank mit anderen Technologien eher kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="fd29b-738">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="fd29b-739">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-739">**Mitigations**</span></span>

<span data-ttu-id="fd29b-740">Sie können vorhandene Datenbanken zum neuen Format migrieren, indem Sie SQL-Code wie den folgenden ausführen:</span><span class="sxs-lookup"><span data-stu-id="fd29b-740">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="fd29b-741">Sie können in EF Core auch weiterhin das alte Verhalten verwenden, indem Sie einen Wertkonverter für diese Eigenschaften konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="fd29b-741">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="fd29b-742">Microsoft.Data.Sqlite kann auch weiterhin Zeichenwerte aus INTEGER- und TEXT-Spalten lesen, weshalb es sein kann, dass in einigen Szenarios keine weiteren Maßnahmen erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="fd29b-742">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="fd29b-743">Migrations-IDs werden jetzt mit dem Kalender der invarianten Kultur generiert</span><span class="sxs-lookup"><span data-stu-id="fd29b-743">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="fd29b-744">Issue #12978</span><span class="sxs-lookup"><span data-stu-id="fd29b-744">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="fd29b-745">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-745">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fd29b-746">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-746">**Old behavior**</span></span>

<span data-ttu-id="fd29b-747">Migrations-IDs wurden versehentlich mit dem Kalender der aktuellen Kultur generiert.</span><span class="sxs-lookup"><span data-stu-id="fd29b-747">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="fd29b-748">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-748">**New behavior**</span></span>

<span data-ttu-id="fd29b-749">Migrations-IDs werden jetzt immer mit dem Kalender der invarianten Kultur generiert (gregorianischer Kalender).</span><span class="sxs-lookup"><span data-stu-id="fd29b-749">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="fd29b-750">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-750">**Why**</span></span>

<span data-ttu-id="fd29b-751">Die Reihenfolge der Migrationen ist beim Aktualisieren einer Datenbank oder Auflösen von Mergekonflikten wesentlich.</span><span class="sxs-lookup"><span data-stu-id="fd29b-751">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="fd29b-752">Wenn der invariante Kalender verwendet wird, werden Probleme bei der Reihenfolge vermieden, die entstehen, wenn Teammitglieder unterschiedliche Systemkalender verwenden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-752">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="fd29b-753">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-753">**Mitigations**</span></span>

<span data-ttu-id="fd29b-754">Diese Änderung betrifft jeden Benutzer, der einen Kalender verwendet, der nicht gregorianisch ist und bei dem die Jahreszahl höher als die des gregorianischen Kalenders ist (wie z.B. in der buddhistischen Zeitrechnung).</span><span class="sxs-lookup"><span data-stu-id="fd29b-754">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="fd29b-755">Vorhandene Migrations-IDs müssen aktualisiert werden, damit neue Migrationen in der Reihenfolge hinter vorhandenen Migrationen eingeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-755">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="fd29b-756">Sie können die Migrations-ID im Migration-Attribut in der Designerdatei der Migration finden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-756">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="fd29b-757">Die Tabelle mit dem Migrationsverlauf muss auch aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-757">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="fd29b-758">UseRowNumberForPaging wurde entfernt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-758">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="fd29b-759">Issue #16400</span><span class="sxs-lookup"><span data-stu-id="fd29b-759">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="fd29b-760">Diese Änderung wird in Vorschauversion 6 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-760">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="fd29b-761">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-761">**Old behavior**</span></span>

<span data-ttu-id="fd29b-762">Vor EF Core 3.0 konnte `UseRowNumberForPaging` zum Generieren von SQL-Code für die Paginierung verwendet werden, der mit SQL Server 2008 kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="fd29b-762">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="fd29b-763">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-763">**New behavior**</span></span>

<span data-ttu-id="fd29b-764">Ab EF Core 3.0 generiert EF nur noch SQL-Code für die Paginierung, die mit höheren SQL Server-Versionen kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="fd29b-764">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="fd29b-765">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-765">**Why**</span></span>

<span data-ttu-id="fd29b-766">Wir führen diese Änderung ein, weil [SQL Server 2008 kein unterstütztes Produkt mehr ist](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) und weil die Aktualisierung dieses Features auf die in EF Core 3.0 vorgenommenen Änderungen an Abfragen ein wichtiger Schritt ist.</span><span class="sxs-lookup"><span data-stu-id="fd29b-766">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="fd29b-767">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-767">**Mitigations**</span></span>

<span data-ttu-id="fd29b-768">Es empfiehlt sich, eine Update auf eine neuere Version von SQL Server durchzuführen oder einen höheren Kompatibilitätsgrad zu verwenden, damit der generierte SQL-Code unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="fd29b-768">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="fd29b-769">Wenn dies bei Ihnen nicht möglich ist, fügen Sie einen [Kommentar zum Issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) ein, und geben Sie alle relevanten Informationen an.</span><span class="sxs-lookup"><span data-stu-id="fd29b-769">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="fd29b-770">Je nach Feedback der Benutzer wird diese Entscheidung möglicherweise noch einmal überprüft.</span><span class="sxs-lookup"><span data-stu-id="fd29b-770">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="fd29b-771">Erweiterungsinformationen und Metadaten aus IDbContextOptionsExtension entfernt</span><span class="sxs-lookup"><span data-stu-id="fd29b-771">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="fd29b-772">Issue #16119</span><span class="sxs-lookup"><span data-stu-id="fd29b-772">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="fd29b-773">Diese Änderung wird in Vorschauversion 7 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-773">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="fd29b-774">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-774">**Old behavior**</span></span>

<span data-ttu-id="fd29b-775">`IDbContextOptionsExtension` enthielt Methoden zum Bereitstellen von Metadaten zur Erweiterung.</span><span class="sxs-lookup"><span data-stu-id="fd29b-775">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="fd29b-776">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-776">**New behavior**</span></span>

<span data-ttu-id="fd29b-777">Diese Methoden wurden in eine neue abstrakte `DbContextOptionsExtensionInfo`-Basisklasse verschoben, die von einer neuen `IDbContextOptionsExtension.Info`-Eigenschaft zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-777">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="fd29b-778">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-778">**Why**</span></span>

<span data-ttu-id="fd29b-779">In den Releases zwischen 2.0 und 3.0 mussten wir diese Methoden mehrmals ergänzen oder ändern.</span><span class="sxs-lookup"><span data-stu-id="fd29b-779">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="fd29b-780">Durch die Bereitstellung dieser Methoden in einer neuen abstrakten Basisklasse lassen sich solche Änderungen einfacher einführen, ohne dass es zu Konflikten mit vorhandenen Erweiterungen kommt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-780">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="fd29b-781">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-781">**Mitigations**</span></span>

<span data-ttu-id="fd29b-782">Erweiterungen wurden aktualisiert und verwenden das neue Muster.</span><span class="sxs-lookup"><span data-stu-id="fd29b-782">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="fd29b-783">Beispiele finden sich in den vielen Implementierungen von `IDbContextOptionsExtension` für verschiedene Arten von Erweiterungen im EF Core-Quellcode.</span><span class="sxs-lookup"><span data-stu-id="fd29b-783">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="fd29b-784">LogQueryPossibleExceptionWithAggregateOperator wurde umbenannt</span><span class="sxs-lookup"><span data-stu-id="fd29b-784">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="fd29b-785">Issue #10985</span><span class="sxs-lookup"><span data-stu-id="fd29b-785">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="fd29b-786">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-786">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fd29b-787">**Änderung**</span><span class="sxs-lookup"><span data-stu-id="fd29b-787">**Change**</span></span>

<span data-ttu-id="fd29b-788">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` wurde umbenannt in `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="fd29b-788">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="fd29b-789">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-789">**Why**</span></span>

<span data-ttu-id="fd29b-790">Die Benennung dieses Warnereignisses wurde an allen anderen Warnereignissen ausgerichtet.</span><span class="sxs-lookup"><span data-stu-id="fd29b-790">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="fd29b-791">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-791">**Mitigations**</span></span>

<span data-ttu-id="fd29b-792">Verwenden Sie den neuen Namen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-792">Use the new name.</span></span> <span data-ttu-id="fd29b-793">(Beachten Sie, dass sich die Ereignis-ID-Nummer nicht geändert hat.)</span><span class="sxs-lookup"><span data-stu-id="fd29b-793">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="fd29b-794">Verdeutlichen der API für Einschränkungsnamen von Fremdschlüsseln</span><span class="sxs-lookup"><span data-stu-id="fd29b-794">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="fd29b-795">Issue #10730</span><span class="sxs-lookup"><span data-stu-id="fd29b-795">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="fd29b-796">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-796">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fd29b-797">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-797">**Old behavior**</span></span>

<span data-ttu-id="fd29b-798">Vor EF Core 3.0 wurden Einschränkungsnamen von Fremdschlüsseln einfach als „Namen“ bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="fd29b-798">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="fd29b-799">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fd29b-799">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="fd29b-800">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-800">**New behavior**</span></span>

<span data-ttu-id="fd29b-801">Ab EF Core 3.0 werden Einschränkungsnamen von Fremdschlüsseln nun als „Einschränkungsnamen“ bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="fd29b-801">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="fd29b-802">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fd29b-802">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="fd29b-803">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-803">**Why**</span></span>

<span data-ttu-id="fd29b-804">Durch diese Änderung wird Konsistenz für Benennungen in diesem Bereich gewährleistet, und es wird verdeutlicht, dass es sich dabei um den Namen der Fremdschlüsseleinschränkung handelt und nicht um den Spalten- oder Eigenschaftennamen, auf deren Grundlage der Fremdschlüssel definiert ist.</span><span class="sxs-lookup"><span data-stu-id="fd29b-804">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="fd29b-805">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-805">**Mitigations**</span></span>

<span data-ttu-id="fd29b-806">Verwenden Sie den neuen Namen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-806">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="fd29b-807">„IRelationalDatabaseCreator.HasTables/HasTablesAsync“ ist jetzt öffentlich</span><span class="sxs-lookup"><span data-stu-id="fd29b-807">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="fd29b-808">Issue #15997</span><span class="sxs-lookup"><span data-stu-id="fd29b-808">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="fd29b-809">Diese Änderung wird in Vorschauversion 7 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-809">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="fd29b-810">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-810">**Old behavior**</span></span>

<span data-ttu-id="fd29b-811">Vor EF Core 3.0 waren diese Methoden geschützt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-811">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="fd29b-812">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-812">**New behavior**</span></span>

<span data-ttu-id="fd29b-813">Ab EF Core 3.0 sind diese Methoden öffentlich.</span><span class="sxs-lookup"><span data-stu-id="fd29b-813">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="fd29b-814">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-814">**Why**</span></span>

<span data-ttu-id="fd29b-815">Diese Methoden werden von EF verwendet, um zu ermitteln, ob eine Datenbank erstellt wurde, aber leer ist.</span><span class="sxs-lookup"><span data-stu-id="fd29b-815">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="fd29b-816">Dies kann auch außerhalb von EF nützlich sein, um zu ermitteln, ob Migrationen angewendet werden sollen oder nicht.</span><span class="sxs-lookup"><span data-stu-id="fd29b-816">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="fd29b-817">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-817">**Mitigations**</span></span>

<span data-ttu-id="fd29b-818">Der Zugriff auf Überschreibungen wurde geändert.</span><span class="sxs-lookup"><span data-stu-id="fd29b-818">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="fd29b-819">Microsoft.EntityFrameworkCore.Design ist jetzt ein DevelopmentDependency-Paket</span><span class="sxs-lookup"><span data-stu-id="fd29b-819">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="fd29b-820">Issue #11506</span><span class="sxs-lookup"><span data-stu-id="fd29b-820">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="fd29b-821">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-821">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="fd29b-822">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-822">**Old behavior**</span></span>

<span data-ttu-id="fd29b-823">Vor EF Core 3.0 war Microsoft.EntityFrameworkCore.Design ein reguläres NuGet-Paket, auf dessen Assembly von Projekten verwiesen werden kann, die davon abhängig sind.</span><span class="sxs-lookup"><span data-stu-id="fd29b-823">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="fd29b-824">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-824">**New behavior**</span></span>

<span data-ttu-id="fd29b-825">Ab EF Core 3.0 ist es ein DevelopmentDependency-Paket.</span><span class="sxs-lookup"><span data-stu-id="fd29b-825">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="fd29b-826">Das bedeutet, dass die Abhängigkeit nicht transitiv auf andere Projekte übertragen wird und Sie nicht mehr standardmäßig auf die Assembly verweisen können.</span><span class="sxs-lookup"><span data-stu-id="fd29b-826">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="fd29b-827">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-827">**Why**</span></span>

<span data-ttu-id="fd29b-828">Dieses Paket ist nur für die Verwendung zur Entwurfszeit konzipiert.</span><span class="sxs-lookup"><span data-stu-id="fd29b-828">This package is only intended to be used at design time.</span></span> <span data-ttu-id="fd29b-829">Bereitgestellte Anwendungen sollten nicht darauf verweisen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-829">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="fd29b-830">Die Kennzeichnung des Pakets als DevelopmentDependency unterstreicht diese Empfehlung.</span><span class="sxs-lookup"><span data-stu-id="fd29b-830">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="fd29b-831">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-831">**Mitigations**</span></span>

<span data-ttu-id="fd29b-832">Wenn Sie auf dieses Paket verweisen müssen, um das Verhalten von EF Core zur Entwurfszeit zu überschreiben, können Sie die Metadaten des PackageReference-Elements in Ihrem Projekt aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="fd29b-832">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="fd29b-833">Wenn transitiv über via Microsoft.EntityFrameworkCore.Tools auf das Paket verwiesen wird, müssen Sie dem Paket eine explizite PackageReference hinzufügen, um seine Metadaten zu ändern.</span><span class="sxs-lookup"><span data-stu-id="fd29b-833">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="fd29b-834">SQLitePCL.raw zu Version 2.0.0 aktualisiert</span><span class="sxs-lookup"><span data-stu-id="fd29b-834">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="fd29b-835">Issue #14824</span><span class="sxs-lookup"><span data-stu-id="fd29b-835">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="fd29b-836">Diese Änderung wird in Vorschauversion 7 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-836">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="fd29b-837">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-837">**Old behavior**</span></span>

<span data-ttu-id="fd29b-838">Microsoft.EntityFrameworkCore.Sqlite war früher von Version 1.1.12 von SQLitePCL.raw abhängig.</span><span class="sxs-lookup"><span data-stu-id="fd29b-838">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="fd29b-839">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-839">**New behavior**</span></span>

<span data-ttu-id="fd29b-840">Wir haben unser Paket aktualisiert, so dass es jetzt von Version 2.0.0 abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="fd29b-840">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="fd29b-841">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-841">**Why**</span></span>

<span data-ttu-id="fd29b-842">Version 2.0.0 von SQLitePCL.raw zielt auf .NET Standard 2.0 ab.</span><span class="sxs-lookup"><span data-stu-id="fd29b-842">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="fd29b-843">Zuvor war .NET Standard 1.1 das Ziel, hierfür war für ein umfassender Funktionsabschluss von transitiven Paketen erforderlich.</span><span class="sxs-lookup"><span data-stu-id="fd29b-843">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="fd29b-844">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-844">**Mitigations**</span></span>

<span data-ttu-id="fd29b-845">SQLitePCL.raw, Version 2.0.0, enthält einige Breaking Changes.</span><span class="sxs-lookup"><span data-stu-id="fd29b-845">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="fd29b-846">Weitere Informationen finden Sie in den [Versionshinweisen](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md).</span><span class="sxs-lookup"><span data-stu-id="fd29b-846">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="fd29b-847">NetTopologySuite auf Version 2.0.0 aktualisiert</span><span class="sxs-lookup"><span data-stu-id="fd29b-847">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="fd29b-848">Issue #14825</span><span class="sxs-lookup"><span data-stu-id="fd29b-848">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="fd29b-849">Diese Änderung wird in Vorschauversion 7 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-849">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="fd29b-850">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-850">**Old behavior**</span></span>

<span data-ttu-id="fd29b-851">Die Pakete mit räumlichen Daten waren bisher von Version 1.15.1 der NetTopologySuite abhängig.</span><span class="sxs-lookup"><span data-stu-id="fd29b-851">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="fd29b-852">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-852">**New behavior**</span></span>

<span data-ttu-id="fd29b-853">Wir haben unser Paket aktualisiert, so dass es jetzt von Version 2.0.0 abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="fd29b-853">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="fd29b-854">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-854">**Why**</span></span>

<span data-ttu-id="fd29b-855">Version 2.0.0 der NetTopologySuite behebt verschiedene Verwendungsprobleme, die von EF Core-Benutzern gemeldet wurden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-855">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="fd29b-856">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-856">**Mitigations**</span></span>

<span data-ttu-id="fd29b-857">NetTopologySuite 2.0.0 umfasst einige Breaking Changes.</span><span class="sxs-lookup"><span data-stu-id="fd29b-857">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="fd29b-858">Weitere Informationen finden Sie in den [Versionshinweisen](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001).</span><span class="sxs-lookup"><span data-stu-id="fd29b-858">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="fd29b-859">Beziehungen mit mehreren mehrdeutigen Selbstverweisen müssen nun konfiguriert werden</span><span class="sxs-lookup"><span data-stu-id="fd29b-859">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="fd29b-860">Issue #13573</span><span class="sxs-lookup"><span data-stu-id="fd29b-860">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="fd29b-861">Diese Änderung wird in Vorschauversion 6 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-861">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="fd29b-862">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-862">**Old behavior**</span></span>

<span data-ttu-id="fd29b-863">Ein Entitätstyp, der über unidirektionale Navigationseigenschaften mit mehreren Selbstverweisen und über übereinstimmende Fremdschlüssel verfügte, wurde bislang fälschlicherweise als einfache Beziehung konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="fd29b-863">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="fd29b-864">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fd29b-864">For example:</span></span>

```C#
public class User 
{
        public Guid Id { get; set; }
        public User CreatedBy { get; set; }
        public User UpdatedBy { get; set; }
        public Guid CreatedById { get; set; }
        public Guid? UpdatedById { get; set; }
}
```

<span data-ttu-id="fd29b-865">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-865">**New behavior**</span></span>

<span data-ttu-id="fd29b-866">Dieses Szenario wird nun beim Erstellen von Modellen erkannt. Außerdem wird eine Ausnahme ausgelöst, in der darauf hingewiesen wird, dass das Modell mehrdeutig ist.</span><span class="sxs-lookup"><span data-stu-id="fd29b-866">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="fd29b-867">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-867">**Why**</span></span>

<span data-ttu-id="fd29b-868">Das resultierende Modell war mehrdeutig und führte in diesem Fall üblicherweise zu falschen Ergebnissen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-868">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="fd29b-869">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-869">**Mitigations**</span></span>

<span data-ttu-id="fd29b-870">Konfigurieren Sie die Beziehung vollständig.</span><span class="sxs-lookup"><span data-stu-id="fd29b-870">Use full configuration of the relationship.</span></span> <span data-ttu-id="fd29b-871">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fd29b-871">For example:</span></span>

```C#
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
### <a name="dbfunctionschema-being-null-or-empty-string-configures-it-to-be-in-models-default-schema"></a><span data-ttu-id="fd29b-872">DbFunction.Schema ist NULL oder eine leere Zeichenfolge und ist deshalb so konfiguriert, dass es im Standardschema des Modells ist</span><span class="sxs-lookup"><span data-stu-id="fd29b-872">DbFunction.Schema being null or empty string configures it to be in model's default schema</span></span>

[<span data-ttu-id="fd29b-873">Issue #12757</span><span class="sxs-lookup"><span data-stu-id="fd29b-873">Tracking Issue #12757</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12757)

<span data-ttu-id="fd29b-874">Diese Änderung wird in Vorschauversion 7 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-874">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="fd29b-875">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-875">**Old behavior**</span></span>

<span data-ttu-id="fd29b-876">Eine mit dem Schema als leere Zeichenfolge konfigurierte DbFunction wurde als integrierte Funktion ohne Schema behandelt.</span><span class="sxs-lookup"><span data-stu-id="fd29b-876">A DbFunction configured with schema as an empty string was treated as built-in function without a schema.</span></span> <span data-ttu-id="fd29b-877">Der folgende Code ordnet z. B. die CLR-Funktion `DatePart` der integrierten Funktion `DATEPART` auf SqlServer zu.</span><span class="sxs-lookup"><span data-stu-id="fd29b-877">For example following code will map `DatePart` CLR function to `DATEPART` built-in function on SqlServer.</span></span>

```C#
[DbFunction("DATEPART", Schema = "")]
public static int? DatePart(string datePartArg, DateTime? date) => throw new Exception();

```

<span data-ttu-id="fd29b-878">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="fd29b-878">**New behavior**</span></span>

<span data-ttu-id="fd29b-879">Alle DbFunction-Zuordnungen werden als einer benutzerdefinierten Funktion zugeordnet betrachtet.</span><span class="sxs-lookup"><span data-stu-id="fd29b-879">All DbFunction mappings are considered to be mapped to user defined functions.</span></span> <span data-ttu-id="fd29b-880">Daher würde die Funktion durch einen leeren Zeichenfolgenwert in das Standardschema für das Modell eingefügt werden.</span><span class="sxs-lookup"><span data-stu-id="fd29b-880">Hence empty string value would put the function inside the default schema for the model.</span></span> <span data-ttu-id="fd29b-881">Dies könnte das Schema sein, das explizit über eine fließende `modelBuilder.HasDefaultSchema()`- oder anderweitig über eine `dbo`-API konfiguriert wurde.</span><span class="sxs-lookup"><span data-stu-id="fd29b-881">Which could be the schema configured explicitly via fluent API `modelBuilder.HasDefaultSchema()` or `dbo` otherwise.</span></span>

<span data-ttu-id="fd29b-882">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="fd29b-882">**Why**</span></span>

<span data-ttu-id="fd29b-883">Weil das Schema vorher leer war, konnte man erreichen, dass die Funktion integriert, aber die Logik nur für SqlServer anwendbar war, wo integrierte Funktionen nicht zu einem Schema gehören.</span><span class="sxs-lookup"><span data-stu-id="fd29b-883">Previously schema being empty was a way to treat that function is built-in but that logic is only applicable for SqlServer where built-in functions do not belong to any schema.</span></span>

<span data-ttu-id="fd29b-884">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="fd29b-884">**Mitigations**</span></span>

<span data-ttu-id="fd29b-885">Konfigurieren Sie die Übersetzung von DbFunction manuell, um Sie einer integrierten Funktion zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="fd29b-885">Configure DbFunction's translation manually to map it to a built-in function.</span></span>

```C#
modelBuilder
    .HasDbFunction(typeof(MyContext).GetMethod(nameof(MyContext.DatePart)))
    .HasTranslation(args => SqlFunctionExpression.Create("DatePart", args, typeof(int?), null));
```
