---
title: Breaking Changes in EF Core 3.0
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: c73663412efcd93c04892f193d4f5a2485724e22
ms.sourcegitcommit: 755a15a789631cc4ea581e2262a2dcc49c219eef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2019
ms.locfileid: "68497526"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="a0f81-102">Breaking Changes in EF Core 3.0 (aktuell in der Vorschauversion)</span><span class="sxs-lookup"><span data-stu-id="a0f81-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a0f81-103">Bitte beachten Sie, dass die Featuregruppen und Zeitpläne für künftige Releases jederzeit geändert werden können. Obwohl diese Seite bestmöglich aktualisiert wird, entspricht sie nicht immer den neuesten Plänen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="a0f81-104">Die folgenden API-Änderungen und Behavior Changes können dazu führen, dass Anwendungen, die für EF Core 2.2.x entwickelt wurden, beim Upgrade auf 3.0.0 nicht mehr funktionieren.</span><span class="sxs-lookup"><span data-stu-id="a0f81-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="a0f81-105">Änderungen, die sich voraussichtlich nur auf Datenbankanbieter auswirken, sind unter [Änderungen mit Auswirkungen auf den Anbieter](../../providers/provider-log.md) dokumentiert.</span><span class="sxs-lookup"><span data-stu-id="a0f81-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="a0f81-106">Werden Breaking Changes zwischen zwei Vorschauversionen von EF Core 3.0 in neuen Features eingeführt, werden diese hier nicht dokumentiert.</span><span class="sxs-lookup"><span data-stu-id="a0f81-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="summary"></a><span data-ttu-id="a0f81-107">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="a0f81-107">Summary</span></span>

| <span data-ttu-id="a0f81-108">**Wichtige Änderung**</span><span class="sxs-lookup"><span data-stu-id="a0f81-108">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="a0f81-109">**Auswirkung**</span><span class="sxs-lookup"><span data-stu-id="a0f81-109">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="a0f81-110">LINQ-Abfragen werden nicht mehr auf dem Client ausgewertet</span><span class="sxs-lookup"><span data-stu-id="a0f81-110">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="a0f81-111">Hoch</span><span class="sxs-lookup"><span data-stu-id="a0f81-111">High</span></span>       |
| [<span data-ttu-id="a0f81-112">Das EF Core-Befehlszeilentool (dotnet ef) ist nicht mehr Bestandteil des .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="a0f81-112">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="a0f81-113">Hoch</span><span class="sxs-lookup"><span data-stu-id="a0f81-113">High</span></span>      |
| [<span data-ttu-id="a0f81-114">FromSql, ExecuteSql und ExecuteSqlAsync wurden umbenannt</span><span class="sxs-lookup"><span data-stu-id="a0f81-114">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="a0f81-115">Hoch</span><span class="sxs-lookup"><span data-stu-id="a0f81-115">High</span></span>      |
| [<span data-ttu-id="a0f81-116">Abfragetypen werden mit Entitätstypen zusammengeführt</span><span class="sxs-lookup"><span data-stu-id="a0f81-116">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="a0f81-117">Hoch</span><span class="sxs-lookup"><span data-stu-id="a0f81-117">High</span></span>      |
| [<span data-ttu-id="a0f81-118">Entity Framework Core ist nicht mehr Bestandteil des gemeinsam verwendeten ASP.NET Core-Frameworks</span><span class="sxs-lookup"><span data-stu-id="a0f81-118">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="a0f81-119">Mittel</span><span class="sxs-lookup"><span data-stu-id="a0f81-119">Medium</span></span>      |
| [<span data-ttu-id="a0f81-120">Kaskadierende DELETE-Anweisungen werden standardmäßig sofort ausgeführt</span><span class="sxs-lookup"><span data-stu-id="a0f81-120">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="a0f81-121">Mittel</span><span class="sxs-lookup"><span data-stu-id="a0f81-121">Medium</span></span>      |
| [<span data-ttu-id="a0f81-122">DeleteBehavior.Restrict verfügt über eine übersichtlichere Semantik</span><span class="sxs-lookup"><span data-stu-id="a0f81-122">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="a0f81-123">Mittel</span><span class="sxs-lookup"><span data-stu-id="a0f81-123">Medium</span></span>      |
| [<span data-ttu-id="a0f81-124">Die Konfigurations-API für Beziehungen abhängiger (owned) Typen wurde geändert</span><span class="sxs-lookup"><span data-stu-id="a0f81-124">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="a0f81-125">Mittel</span><span class="sxs-lookup"><span data-stu-id="a0f81-125">Medium</span></span>      |
| [<span data-ttu-id="a0f81-126">Für jede Eigenschaft wird separat ein ganzzahliger speicherinterner Schlüssel generiert</span><span class="sxs-lookup"><span data-stu-id="a0f81-126">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="a0f81-127">Mittel</span><span class="sxs-lookup"><span data-stu-id="a0f81-127">Medium</span></span>      |
| [<span data-ttu-id="a0f81-128">Metadaten-API-Änderungen</span><span class="sxs-lookup"><span data-stu-id="a0f81-128">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="a0f81-129">Mittel</span><span class="sxs-lookup"><span data-stu-id="a0f81-129">Medium</span></span>      |
| [<span data-ttu-id="a0f81-130">Anbieterspezifische Metadaten-API-Änderungen</span><span class="sxs-lookup"><span data-stu-id="a0f81-130">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="a0f81-131">Mittel</span><span class="sxs-lookup"><span data-stu-id="a0f81-131">Medium</span></span>      |
| [<span data-ttu-id="a0f81-132">UseRowNumberForPaging wurde entfernt</span><span class="sxs-lookup"><span data-stu-id="a0f81-132">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="a0f81-133">Mittel</span><span class="sxs-lookup"><span data-stu-id="a0f81-133">Medium</span></span>      |
| [<span data-ttu-id="a0f81-134">FromSql-Methoden können nur für die Stammelemente der Abfrage angegeben werden</span><span class="sxs-lookup"><span data-stu-id="a0f81-134">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="a0f81-135">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-135">Low</span></span>      |
| [<span data-ttu-id="a0f81-136">~~Die Abfrageausführung wird auf Debugebene protokolliert~~ – zurückgesetzt</span><span class="sxs-lookup"><span data-stu-id="a0f81-136">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="a0f81-137">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-137">Low</span></span>      |
| [<span data-ttu-id="a0f81-138">Temporäre Schlüsselwerte werden nicht mehr Entitätsinstanzen zugewiesen</span><span class="sxs-lookup"><span data-stu-id="a0f81-138">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="a0f81-139">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-139">Low</span></span>      |
| [<span data-ttu-id="a0f81-140">DetectChanges berücksichtigt vom Speicher generierte Schlüsselwerte</span><span class="sxs-lookup"><span data-stu-id="a0f81-140">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="a0f81-141">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-141">Low</span></span>      |
| [<span data-ttu-id="a0f81-142">Abhängige Entitäten, die die Tabelle gemeinsam mit dem Prinzipal verwenden, sind jetzt optional</span><span class="sxs-lookup"><span data-stu-id="a0f81-142">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="a0f81-143">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-143">Low</span></span>      |
| [<span data-ttu-id="a0f81-144">Alle Entitäten, die eine Tabelle mit einer Spalte für das Parallelitätstoken gemeinsam verwenden, müssen diese einer Eigenschaft zuordnen</span><span class="sxs-lookup"><span data-stu-id="a0f81-144">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="a0f81-145">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-145">Low</span></span>      |
| [<span data-ttu-id="a0f81-146">Geerbte Eigenschaften von nicht zugeordneten Typen sind nun einer einzelnen Spalte für alle abgeleiteten Typen zugeordnet</span><span class="sxs-lookup"><span data-stu-id="a0f81-146">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="a0f81-147">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-147">Low</span></span>      |
| [<span data-ttu-id="a0f81-148">Die Konvention zur Fremdschlüsseleigenschaft entspricht nicht mehr dem Namen der Prinzipaleigenschaft</span><span class="sxs-lookup"><span data-stu-id="a0f81-148">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="a0f81-149">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-149">Low</span></span>      |
| [<span data-ttu-id="a0f81-150">Die Datenbankverbindung wird jetzt getrennt, wenn sie nicht mehr verwendet wird, bevor TransactionScope abgeschlossen wurde</span><span class="sxs-lookup"><span data-stu-id="a0f81-150">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="a0f81-151">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-151">Low</span></span>      |
| [<span data-ttu-id="a0f81-152">Unterstützungsfelder werden standardmäßig verwendet</span><span class="sxs-lookup"><span data-stu-id="a0f81-152">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="a0f81-153">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-153">Low</span></span>      |
| [<span data-ttu-id="a0f81-154">Eine Ausnahme wird ausgelöst, wenn mehrere kompatible Unterstützungsfelder gefunden werden</span><span class="sxs-lookup"><span data-stu-id="a0f81-154">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="a0f81-155">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-155">Low</span></span>      |
| [<span data-ttu-id="a0f81-156">„Nur-Feld“-Eigenschaftennamen sollten dem Feldnamen entsprechen</span><span class="sxs-lookup"><span data-stu-id="a0f81-156">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="a0f81-157">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-157">Low</span></span>      |
| [<span data-ttu-id="a0f81-158">AddLogging und AddMemoryCache werden nicht mehr von AddDbContext/AddDbContextPool aufgerufen</span><span class="sxs-lookup"><span data-stu-id="a0f81-158">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="a0f81-159">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-159">Low</span></span>      |
| [<span data-ttu-id="a0f81-160">DbContext.Entry erkennt nun mit DetectChanges lokale Änderungen</span><span class="sxs-lookup"><span data-stu-id="a0f81-160">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="a0f81-161">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-161">Low</span></span>      |
| [<span data-ttu-id="a0f81-162">Schlüssel aus Zeichenfolgen und Bytearrays werden standardmäßig nicht vom Client generiert</span><span class="sxs-lookup"><span data-stu-id="a0f81-162">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="a0f81-163">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-163">Low</span></span>      |
| [<span data-ttu-id="a0f81-164">ILoggerFactory ist nun ein bereichsbezogener Dienst</span><span class="sxs-lookup"><span data-stu-id="a0f81-164">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="a0f81-165">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-165">Low</span></span>      |
| [<span data-ttu-id="a0f81-166">Für Proxys mit Lazy Loading (trägem Laden) gilt nicht mehr die Annahme, dass Navigationseigenschaften vollständig geladen sind</span><span class="sxs-lookup"><span data-stu-id="a0f81-166">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="a0f81-167">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-167">Low</span></span>      |
| [<span data-ttu-id="a0f81-168">Die Erstellung zu vieler interner Dienstanbieter führt standardmäßig zu einem Fehler</span><span class="sxs-lookup"><span data-stu-id="a0f81-168">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="a0f81-169">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-169">Low</span></span>      |
| [<span data-ttu-id="a0f81-170">Neues Verhalten für HasOne/HasMany bei Aufruf mit einer einzelnen Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="a0f81-170">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="a0f81-171">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-171">Low</span></span>      |
| [<span data-ttu-id="a0f81-172">Der Rückgabetyp für mehrere asynchrone Methoden wurde von Task in ValueTask geändert</span><span class="sxs-lookup"><span data-stu-id="a0f81-172">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="a0f81-173">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-173">Low</span></span>      |
| [<span data-ttu-id="a0f81-174">Die Anmerkung Relational:TypeMapping heißt nun nur noch TypeMapping</span><span class="sxs-lookup"><span data-stu-id="a0f81-174">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="a0f81-175">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-175">Low</span></span>      |
| [<span data-ttu-id="a0f81-176">ToTable löst für einen abgeleiteten Typ eine Ausnahme aus</span><span class="sxs-lookup"><span data-stu-id="a0f81-176">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="a0f81-177">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-177">Low</span></span>      |
| [<span data-ttu-id="a0f81-178">EF Core sendet keine PRAGMA-Anweisungen mehr, um Fremdschlüssel in SQLite zu erzwingen</span><span class="sxs-lookup"><span data-stu-id="a0f81-178">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="a0f81-179">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-179">Low</span></span>      |
| [<span data-ttu-id="a0f81-180">SQLitePCLRaw.bundle_e_sqlite3 ist nun eine Abhängigkeit von Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="a0f81-180">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="a0f81-181">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-181">Low</span></span>      |
| [<span data-ttu-id="a0f81-182">GUID-Werte werden jetzt als TEXT in SQLite gespeichert</span><span class="sxs-lookup"><span data-stu-id="a0f81-182">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="a0f81-183">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-183">Low</span></span>      |
| [<span data-ttu-id="a0f81-184">Char-Werte werden jetzt als TEXT in SQLite gespeichert</span><span class="sxs-lookup"><span data-stu-id="a0f81-184">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="a0f81-185">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-185">Low</span></span>      |
| [<span data-ttu-id="a0f81-186">Migrations-IDs werden jetzt mit dem Kalender der invarianten Kultur generiert</span><span class="sxs-lookup"><span data-stu-id="a0f81-186">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="a0f81-187">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-187">Low</span></span>      |
| [<span data-ttu-id="a0f81-188">Erweiterungsinformationen und -metadaten wurden aus IDbContextOptionsExtension entfernt</span><span class="sxs-lookup"><span data-stu-id="a0f81-188">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="a0f81-189">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-189">Low</span></span>      |
| [<span data-ttu-id="a0f81-190">LogQueryPossibleExceptionWithAggregateOperator wurde umbenannt</span><span class="sxs-lookup"><span data-stu-id="a0f81-190">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="a0f81-191">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-191">Low</span></span>      |
| [<span data-ttu-id="a0f81-192">Übersichtlichere API für Namen von Fremdschlüsselconstraints</span><span class="sxs-lookup"><span data-stu-id="a0f81-192">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="a0f81-193">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-193">Low</span></span>      |
| [<span data-ttu-id="a0f81-194">IRelationalDatabaseCreator.HasTables/HasTablesAsync ist jetzt öffentlich</span><span class="sxs-lookup"><span data-stu-id="a0f81-194">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="a0f81-195">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-195">Low</span></span>      |
| [<span data-ttu-id="a0f81-196">Microsoft.EntityFrameworkCore.Design ist jetzt ein DevelopmentDependency-Paket</span><span class="sxs-lookup"><span data-stu-id="a0f81-196">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="a0f81-197">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-197">Low</span></span>      |
| [<span data-ttu-id="a0f81-198">SQLitePCL.raw wurde auf Version 2.0.0 aktualisiert</span><span class="sxs-lookup"><span data-stu-id="a0f81-198">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="a0f81-199">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-199">Low</span></span>      |
| [<span data-ttu-id="a0f81-200">NetTopologySuite wurde auf Version 2.0.0 aktualisiert</span><span class="sxs-lookup"><span data-stu-id="a0f81-200">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="a0f81-201">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-201">Low</span></span>      |
| [<span data-ttu-id="a0f81-202">Beziehungen mit mehreren mehrdeutigen Selbstverweisen müssen nun konfiguriert werden</span><span class="sxs-lookup"><span data-stu-id="a0f81-202">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="a0f81-203">Niedrig</span><span class="sxs-lookup"><span data-stu-id="a0f81-203">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="a0f81-204">LINQ-Abfragen werden nicht mehr auf dem Client ausgewertet</span><span class="sxs-lookup"><span data-stu-id="a0f81-204">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="a0f81-205">[Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Siehe auch Issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="a0f81-205">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="a0f81-206">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-206">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a0f81-207">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-207">**Old behavior**</span></span>

<span data-ttu-id="a0f81-208">Vor Version 3.0 wurden in EF Core automatisch Ausdrücke auf dem Client ausgewertet, wenn diese Teil einer Abfrage waren und nicht in SQL oder in einen Parameter konvertiert werden konnten.</span><span class="sxs-lookup"><span data-stu-id="a0f81-208">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="a0f81-209">Wenn potenziell aufwendige Ausdrücke auf dem Client ausgewertet werden sollten, wurde standardmäßig nur eine Warnung ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="a0f81-209">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="a0f81-210">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-210">**New behavior**</span></span>

<span data-ttu-id="a0f81-211">Ab Version 3.0 können in EF Core nur Ausdrücke der obersten Projektion (dem letzten `Select()`-Aufruf in der Abfrage) auf dem Client ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-211">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="a0f81-212">Wenn Ausdrücke in einem anderen Teil der Abfrage nicht in SQL oder in einen Parameter konvertiert werden können, wird eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="a0f81-212">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="a0f81-213">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-213">**Why**</span></span>

<span data-ttu-id="a0f81-214">Die automatische Auswertung von Abfragen auf Clients hat zur Folge, dass viele Abfragen auch dann ausgeführt werden, wenn wichtige Teile nicht übersetzt werden können.</span><span class="sxs-lookup"><span data-stu-id="a0f81-214">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="a0f81-215">Dieses Verhalten kann zu unerwartetem und potenziell schädlichem Verhalten führen, das möglicherweise erst in der Produktionsphase erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="a0f81-215">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="a0f81-216">Eine nicht übersetzbare Bedingung in einem `Where()`-Aufruf kann sich beispielsweise so auswirken, dass alle Zeilen einer Tabelle vom Datenbankserver übertragen werden und der Filter auf dem Client angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="a0f81-216">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="a0f81-217">Wenn die Tabelle in der Entwicklungsphase nur wenige Zeilen enthält, ist es gut möglich, dass diese Situation unerkannt bleibt. Wenn die Anwendung jedoch in die Produktionsumgebung überführt wird, enthält die Tabelle möglicherweise Millionen von Zeilen, was zu schwerwiegenden Konsequenzen führen kann.</span><span class="sxs-lookup"><span data-stu-id="a0f81-217">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="a0f81-218">Wie die Erfahrung in der Entwicklungsphase gezeigt hat, können auch Warnungen, die bei Auswertungen auf dem Client ausgelöst werden, dieses Problem nicht lösen, da sie sich leicht ignorieren lassen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-218">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="a0f81-219">Die automatische Auswertung auf Clients kann darüber hinaus zu Problemen führen, bei denen die Verbesserung der Abfrageübersetzung für bestimmte Ausdrücke zu unbeabsichtigten Breaking Changes zwischen Releases führt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-219">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="a0f81-220">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-220">**Mitigations**</span></span>

<span data-ttu-id="a0f81-221">Wenn sich eine Abfrage nicht vollständig übersetzen lässt, habe Sie zwei Möglichkeiten: Schreiben Sie sie entweder um, oder setzen Sie alternativ `AsEnumerable()`, `ToList()` oder ähnliche Methoden ein, um Daten wieder zurück an den Client zu übertragen, wo sie mit LINQ to Objects weiterverarbeitet werden können.</span><span class="sxs-lookup"><span data-stu-id="a0f81-221">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="a0f81-222">Entity Framework Core ist nicht mehr Teil des gemeinsam verwendeten ASP.NET Core-Frameworks</span><span class="sxs-lookup"><span data-stu-id="a0f81-222">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="a0f81-223">Issueankündigungen #325</span><span class="sxs-lookup"><span data-stu-id="a0f81-223">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="a0f81-224">Diese Änderung wird in Vorschauversion 1 von ASP.NET Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-224">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="a0f81-225">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-225">**Old behavior**</span></span>

<span data-ttu-id="a0f81-226">Vor Version 3.0 von ASP.NET Core wurden EF Core und einige der zugehörigen Datenanbieter wie SQL Server eingeschlossen, wenn `Microsoft.AspNetCore.App` oder `Microsoft.AspNetCore.All` ein Paketverweis hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="a0f81-226">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="a0f81-227">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-227">**New behavior**</span></span>

<span data-ttu-id="a0f81-228">Ab Version 3.0 enthält das gemeinsam verwendete ASP.NET Core-Framework weder EF Core noch zugehörige Datenanbieter.</span><span class="sxs-lookup"><span data-stu-id="a0f81-228">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="a0f81-229">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-229">**Why**</span></span>

<span data-ttu-id="a0f81-230">Vor dieser Änderung konnte EF Core auf unterschiedliche Arten bezogen werden. Diese richteten sich danach, ob ASP.NET Core und SQL Server oder ein anderes Zielframework für die Anwendung vorgesehen war.</span><span class="sxs-lookup"><span data-stu-id="a0f81-230">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="a0f81-231">Bei einem Upgrade von ASP.NET Core wurde zudem ein Upgrade von EF Core und des SQL Server-Anbieters erzwungen, was nicht in allen Fällen gewünscht war.</span><span class="sxs-lookup"><span data-stu-id="a0f81-231">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="a0f81-232">Durch die eingeführte Änderung wird EF Core unter allen Anbietern, unterstützten .NET-Implementierungen und Anwendungstypen auf dieselbe Weise bezogen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-232">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="a0f81-233">Entwickler können nun außerdem genau festlegen, wann für EF Core und zugehörige Datenanbieter ein Upgrade durchgeführt werden soll.</span><span class="sxs-lookup"><span data-stu-id="a0f81-233">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="a0f81-234">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-234">**Mitigations**</span></span>

<span data-ttu-id="a0f81-235">Wenn Sie EF Core in einer Anwendung unter ASP.NET Core 3.0 oder in einer anderen unterstützten Anwendung verwenden möchten, müssen Sie dem EF Core-Datenbankanbieter, der für die Anwendung genutzt werden soll, explizit einen Paketverweis hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-235">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="a0f81-236">Das EF Core-Befehlszeilentool (dotnet ef) ist nicht länger Bestandteil des .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="a0f81-236">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="a0f81-237">Issue #14016</span><span class="sxs-lookup"><span data-stu-id="a0f81-237">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="a0f81-238">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 und der zugehörigen Version des .NET Core SDK eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-238">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="a0f81-239">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-239">**Old behavior**</span></span>

<span data-ttu-id="a0f81-240">Vor Version 3.0 war das `dotnet ef`-Tool im .NET Core SDK enthalten und konnte von der Befehlszeile aus ohne zusätzliche Schritte in einem beliebigen Projekt verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-240">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="a0f81-241">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-241">**New behavior**</span></span>

<span data-ttu-id="a0f81-242">Ab Version 3.0 ist das `dotnet ef`-Tool nicht mehr im .NET SDK enthalten und muss deshalb vor der Verwendung explizit als lokales oder globales Tool installiert werden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-242">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="a0f81-243">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-243">**Why**</span></span>

<span data-ttu-id="a0f81-244">Durch diese Änderung kann `dotnet ef` als reguläres .NET-CLI-Tool für NuGet verteilt und aktualisiert werden und entspricht damit dem üblichen Verfahren, dass EF Core 3.0 ebenfalls immer als NuGet-Paket verteilt wird.</span><span class="sxs-lookup"><span data-stu-id="a0f81-244">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="a0f81-245">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-245">**Mitigations**</span></span>

<span data-ttu-id="a0f81-246">Um Migrationen zu verwalten oder ein Gerüst für `DbContext` zu erstellen, installieren Sie `dotnet-ef` als globales Tool:</span><span class="sxs-lookup"><span data-stu-id="a0f81-246">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

<span data-ttu-id="a0f81-247">Eine Verwendung als lokales Tool ist ebenfalls möglich, wenn Sie die Abhängigkeiten eines Projekts wiederherstellen, das das Tool mithilfe einer [Toolmanifestdatei](https://github.com/dotnet/cli/issues/10288) als Toolabhängigkeit deklariert.</span><span class="sxs-lookup"><span data-stu-id="a0f81-247">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="a0f81-248">FromSql, ExecuteSql und ExecuteSqlAsync wurden umbenannt</span><span class="sxs-lookup"><span data-stu-id="a0f81-248">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="a0f81-249">Issue #10996</span><span class="sxs-lookup"><span data-stu-id="a0f81-249">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="a0f81-250">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-250">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a0f81-251">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-251">**Old behavior**</span></span>

<span data-ttu-id="a0f81-252">Vor Version 3.0 wurden in EF Core diese Methodennamen überladen, um entweder mit einer normalen Zeichenfolge oder einer Zeichenfolge, die in SQL und Parameter interpoliert werden sollte, zu arbeiten.</span><span class="sxs-lookup"><span data-stu-id="a0f81-252">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="a0f81-253">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-253">**New behavior**</span></span>

<span data-ttu-id="a0f81-254">Ab Version 3.0 verwenden Sie in EF Core `FromSqlRaw`, `ExecuteSqlRaw` und `ExecuteSqlRawAsync`, um eine parametrisierte Abfrage zu erstellen, bei der die Parameter einzeln aus der Abfragezeichenfolge übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-254">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="a0f81-255">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a0f81-255">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="a0f81-256">Verwenden Sie `FromSqlInterpolated`, `ExecuteSqlInterpolated` und `ExecuteSqlInterpolatedAsync`, um eine parametrisierte Abfrage zu erstellen, bei der die Parameter als Teil einer interpolierten Abfragezeichenfolge übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-256">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="a0f81-257">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a0f81-257">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="a0f81-258">Beachten Sie, dass die beiden oben genannten Abfragen dieselbe parametrisierte SQL mit denselben SQL-Parametern erzeugen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-258">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="a0f81-259">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-259">**Why**</span></span>

<span data-ttu-id="a0f81-260">Bei Methodenüberladungen wie dieser wird sehr leicht versehentlich die Methode für unformatierte Zeichenfolgen aufgerufen, wenn eigentlich beabsichtigt war, die Methode für interpolierte Zeichenfolgen aufzurufen (und umgekehrt).</span><span class="sxs-lookup"><span data-stu-id="a0f81-260">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="a0f81-261">Dadurch werden Abfragen möglicherweise nicht parametrisiert, obwohl dies der Fall sein sollte.</span><span class="sxs-lookup"><span data-stu-id="a0f81-261">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="a0f81-262">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-262">**Mitigations**</span></span>

<span data-ttu-id="a0f81-263">Verwenden Sie ab sofort die neuen Methodennamen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-263">Switch to use the new method names.</span></span>

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="a0f81-264">FromSql-Methoden können nur für die Stammelemente der Abfrage angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-264">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="a0f81-265">Issue #15704</span><span class="sxs-lookup"><span data-stu-id="a0f81-265">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="a0f81-266">Diese Änderung wird in Vorschauversion 6 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-266">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="a0f81-267">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-267">**Old behavior**</span></span>

<span data-ttu-id="a0f81-268">Vor EF Core 3.0 konnte die `FromSql`-Methode an einer beliebigen Stelle in der Abfrage angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-268">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="a0f81-269">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-269">**New behavior**</span></span>

<span data-ttu-id="a0f81-270">Ab EF Core 3.0 können die neuen Methoden `FromSqlRaw` und `FromSqlInterpolated` (durch die `FromSql` ersetzt wird) nur für Stammelemente von Abfragen angegeben werden, d.h. direkt für das `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="a0f81-270">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="a0f81-271">Der Versuch, sie an anderer Stelle anzugeben, führt zu einem Kompilierungsfehler.</span><span class="sxs-lookup"><span data-stu-id="a0f81-271">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="a0f81-272">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-272">**Why**</span></span>

<span data-ttu-id="a0f81-273">Die Angabe von `FromSql` an einer anderen Stelle als für ein `DbSet` erbrachte keine zusätzliche Bedeutung und keinen Mehrwert, sondern konnte in bestimmten Szenarien zu Mehrdeutigkeiten führen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-273">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="a0f81-274">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-274">**Mitigations**</span></span>

<span data-ttu-id="a0f81-275">`FromSql` Aufrufe sollten so verschoben, dass sie direkt für das zugehörige `DbSet` gelten.</span><span class="sxs-lookup"><span data-stu-id="a0f81-275">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="a0f81-276">~~Die Abfrageausführung wird auf Debugebene protokolliert~~ – zurückgesetzt</span><span class="sxs-lookup"><span data-stu-id="a0f81-276">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="a0f81-277">Issue #14523</span><span class="sxs-lookup"><span data-stu-id="a0f81-277">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="a0f81-278">Diese Änderung wird in Vorschauversion 7 von EF Core 3.0 zurückgesetzt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-278">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="a0f81-279">Wir haben diese Änderung zurückgesetzt, weil die neue Konfiguration in EF Core 3.0 die Angabe des Protokolliergrads für jedes Ereignis durch die Anwendung ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="a0f81-279">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="a0f81-280">Um z.B. die Protokollierung von SQL zu `Debug` zu ändern, konfigurieren Sie den Grad explizit in `OnConfiguring` oder `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="a0f81-280">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="a0f81-281">Temporäre Schlüsselwerte werden nicht mehr Entitätsinstanzen zugewiesen</span><span class="sxs-lookup"><span data-stu-id="a0f81-281">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="a0f81-282">Issue #12378</span><span class="sxs-lookup"><span data-stu-id="a0f81-282">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="a0f81-283">Diese Änderung wird in Vorschauversion 2 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-283">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="a0f81-284">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-284">**Old behavior**</span></span>

<span data-ttu-id="a0f81-285">Vor Version 3.0 wurden in EF Core allen Schlüsseleigenschaften temporäre Werte zugewiesen. Später wurden den Eigenschaften dann die tatsächlichen Werte zugewiesen, die von der Datenbank generiert wurden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-285">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="a0f81-286">Die temporären Werte waren in der Regel große negative Zahlen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-286">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="a0f81-287">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-287">**New behavior**</span></span>

<span data-ttu-id="a0f81-288">Ab Version 3.0 wird in EF Core der temporäre Wert als Teil der Überwachungsinformationen der Entität gespeichert. Die Schlüsseleigenschaft selbst bleibt unverändert.</span><span class="sxs-lookup"><span data-stu-id="a0f81-288">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="a0f81-289">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-289">**Why**</span></span>

<span data-ttu-id="a0f81-290">Diese Änderung wurde vorgenommen, damit temporäre Schlüsselwerte nicht fälschlicherweise dauerhaft gespeichert werden, wenn eine Entität, die vorher von einer `DbContext`-Instanz überwacht wurde, in eine andere `DbContext`-Instanz verschoben wird.</span><span class="sxs-lookup"><span data-stu-id="a0f81-290">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="a0f81-291">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-291">**Mitigations**</span></span>

<span data-ttu-id="a0f81-292">Anwendungen, die Primärschlüsselwerte Fremdschlüsseln zuweisen, um Zuordnungen zwischen Entitäten zu erstellen, können vom alten Verhalten abhängig sein, wenn Primärschlüssel vom Speicher generiert werden und zu Entitäten im `Added`-Zustand gehören.</span><span class="sxs-lookup"><span data-stu-id="a0f81-292">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="a0f81-293">Sie können dies wie folgt vermeiden:</span><span class="sxs-lookup"><span data-stu-id="a0f81-293">This can be avoided by:</span></span>
* <span data-ttu-id="a0f81-294">Verwenden Sie keine vom Speicher generierten Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="a0f81-294">Not using store-generated keys.</span></span>
* <span data-ttu-id="a0f81-295">Legen Sie zum Erstellen von Zuordnungen keine Fremdschlüsselwerte, sondern Navigationseigenschaften fest.</span><span class="sxs-lookup"><span data-stu-id="a0f81-295">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="a0f81-296">Rufen Sie die tatsächlichen temporären Schlüsselwerte aus den Überwachungsinformationen der Entität ab.</span><span class="sxs-lookup"><span data-stu-id="a0f81-296">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="a0f81-297">`context.Entry(blog).Property(e => e.Id).CurrentValue` gibt beispielsweise den temporären Wert zurück, obwohl `blog.Id` noch nicht festgelegt wurde.</span><span class="sxs-lookup"><span data-stu-id="a0f81-297">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="a0f81-298">DetectChanges berücksichtigt vom Speicher generierte Schlüsselwerte</span><span class="sxs-lookup"><span data-stu-id="a0f81-298">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="a0f81-299">Issue #14616</span><span class="sxs-lookup"><span data-stu-id="a0f81-299">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="a0f81-300">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-300">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a0f81-301">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-301">**Old behavior**</span></span>

<span data-ttu-id="a0f81-302">Vor Version 3.0 wurde in EF Core eine nicht überwachte Entität, die von `DetectChanges` gefunden wurde, im `Added`-Zustand überwacht und als neue Zeile eingefügt, sobald `SaveChanges` aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="a0f81-302">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="a0f81-303">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-303">**New behavior**</span></span>

<span data-ttu-id="a0f81-304">Ab Version 3.0 wird in EF Core die Entität im `Modified`-Zustand überwacht, wenn für diese generierte Schlüsselwerte verwendet werden und ein Schlüsselwert festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="a0f81-304">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="a0f81-305">Es wird also davon ausgegangen, dass eine Zeile für die Entität vorhanden ist. Wenn `SaveChanges` aufgerufen wird, wird die Zeile außerdem aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="a0f81-305">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="a0f81-306">Falls kein Schlüsselwert festgelegt ist oder der Entitätstyp keine generierten Schlüssel verwendet, wird die neue Entität wie in früheren Version weiterhin im `Added`-Zustand überwacht.</span><span class="sxs-lookup"><span data-stu-id="a0f81-306">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="a0f81-307">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-307">**Why**</span></span>

<span data-ttu-id="a0f81-308">Diese Änderung wurde vorgenommen, um bei der Verwendung von speichergenerierten Schlüsseln die Arbeit mit nicht verbundenen Entitätsgraphen einfacher und konsistenter zu gestalten.</span><span class="sxs-lookup"><span data-stu-id="a0f81-308">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="a0f81-309">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-309">**Mitigations**</span></span>

<span data-ttu-id="a0f81-310">Diese Änderung kann dazu führen, dass eine Anwendung nicht mehr funktioniert. Dieser Fall tritt ein, wenn für Entitätstypen generierte Schlüssel vorgesehen sind, dann jedoch explizit Schlüsselwerte für neue Instanzen festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-310">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="a0f81-311">Sie können dieses Problem vermeiden, indem Sie explizit festlegen, dass für Schlüsseleigenschaften keine generierten Werte verwendet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-311">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="a0f81-312">Dies ist beispielsweise mit der Fluent-API möglich:</span><span class="sxs-lookup"><span data-stu-id="a0f81-312">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="a0f81-313">Alternativ bieten sich auch Datenanmerkungen an:</span><span class="sxs-lookup"><span data-stu-id="a0f81-313">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="a0f81-314">Kaskadierende Deletes werden standardmäßig sofort ausgeführt</span><span class="sxs-lookup"><span data-stu-id="a0f81-314">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="a0f81-315">Issue #10114</span><span class="sxs-lookup"><span data-stu-id="a0f81-315">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="a0f81-316">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-316">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a0f81-317">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-317">**Old behavior**</span></span>

<span data-ttu-id="a0f81-318">Vor Version 3.0 wurden in EF Core kaskadierenden Aktionen (Löschen von abhängigen Entitäten nach dem Löschen eines erforderlichen Prinzipals oder nach dem Aufheben einer Beziehung zum erforderlichen Prinzipal) erst ausgeführt, wenn SaveChanges aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="a0f81-318">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="a0f81-319">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-319">**New behavior**</span></span>

<span data-ttu-id="a0f81-320">Ab Version 3.0 werden in EF Core kaskadierende Aktionen angewendet, sobald die auslösende Bedingung erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="a0f81-320">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="a0f81-321">Wenn beispielsweise `context.Remove()` zum Löschen einer Prinzipalentität aufgerufen wird, führt das dazu, dass auch alle zugehörigen erforderlichen Entitäten, die überwacht werden, sofort auf `Deleted` festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-321">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="a0f81-322">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-322">**Why**</span></span>

<span data-ttu-id="a0f81-323">Diese Änderung wurde vorgenommen, um die Arbeit mit Datenbindungen und Überwachungsszenarios zu vereinfachen, in denen bekannt sein muss, welche Entitäten _vor_ dem Aufruf von `SaveChanges` gelöscht werden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-323">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="a0f81-324">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-324">**Mitigations**</span></span>

<span data-ttu-id="a0f81-325">Sie können das vorherige Verhalten wiederherstellen, indem Sie für `context.ChangedTracker` die entsprechenden Einstellungen vornehmen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-325">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="a0f81-326">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a0f81-326">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="a0f81-327">DeleteBehavior.Restrict verfügt über eine übersichtlichere Semantik</span><span class="sxs-lookup"><span data-stu-id="a0f81-327">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="a0f81-328">Issue #12661</span><span class="sxs-lookup"><span data-stu-id="a0f81-328">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="a0f81-329">Diese Änderung wird in Vorschauversion 5 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-329">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="a0f81-330">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-330">**Old behavior**</span></span>

<span data-ttu-id="a0f81-331">Vor Version 3.0 wurden mit `DeleteBehavior.Restrict` Fremdschlüssel in der Datenbank mit `Restrict`-Semantik erstellt, es wurden aber auch unvorhergesehen interne Fixups geändert.</span><span class="sxs-lookup"><span data-stu-id="a0f81-331">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="a0f81-332">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-332">**New behavior**</span></span>

<span data-ttu-id="a0f81-333">Ab Version 3.0 wird mit `DeleteBehavior.Restrict` sichergestellt, dass Fremdschlüssel mit `Restrict`-Semantik erstellt werden – d. h. ohne Überlappungen; ausgenommen bei Einschränkungsverletzungen – und ohne Auswirkungen auf EF-interne Fixups.</span><span class="sxs-lookup"><span data-stu-id="a0f81-333">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="a0f81-334">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-334">**Why**</span></span>

<span data-ttu-id="a0f81-335">Diese Änderung wurde vorgenommen, um die Benutzerfreundlichkeit bei intuitiver Verwendung von `DeleteBehavior` ohne unerwartete Nebeneffekte zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="a0f81-335">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="a0f81-336">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-336">**Mitigations**</span></span>

<span data-ttu-id="a0f81-337">Sie können das vorherige Verhalten wiederherstellen, indem Sie `DeleteBehavior.ClientNoAction` verwenden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-337">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="a0f81-338">Abfragetypen werden mit Entitätstypen zusammengeführt</span><span class="sxs-lookup"><span data-stu-id="a0f81-338">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="a0f81-339">Issue #14194</span><span class="sxs-lookup"><span data-stu-id="a0f81-339">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="a0f81-340">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-340">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a0f81-341">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-341">**Old behavior**</span></span>

<span data-ttu-id="a0f81-342">Vor Version 3.0 wurden in EF Core [Abfragetypen](xref:core/modeling/query-types) dazu verwendet, Daten abzufragen, in denen kein Primärschlüssel auf strukturierte Weise festgelegt war.</span><span class="sxs-lookup"><span data-stu-id="a0f81-342">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="a0f81-343">Abfragetypen wurden also für die Zuordnung von Entitätstypen ohne Schlüssel verwendet (in der Regel in einer Sicht, in einigen Fällen aber auch in einer Tabelle), während reguläre Entitätstypen für einen verfügbaren Schlüssel verwendet wurden (in der Regel in einer Tabelle, in einigen Fällen aber auch in einer Sicht).</span><span class="sxs-lookup"><span data-stu-id="a0f81-343">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="a0f81-344">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-344">**New behavior**</span></span>

<span data-ttu-id="a0f81-345">Aus einem Abfragetyp wird nun einfach ein Entitätstyp ohne Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="a0f81-345">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="a0f81-346">Entitätstypen ohne Schlüssel besitzen die gleiche Funktionalität wie Abfragetypen in früheren Versionen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-346">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="a0f81-347">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-347">**Why**</span></span>

<span data-ttu-id="a0f81-348">Diese Änderung wurde vorgenommen, um Unklarheiten bei der Verwendung von Abfragetypen zu beseitigen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-348">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="a0f81-349">Abfragetypen sind schlüssellose Entitätstypen, die grundsätzlich schreibgeschützt sind. Sie sollten allerdings nicht allein wegen des Schreibschutzes verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-349">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="a0f81-350">Des Weiteren werden sie oft Sichten zugeordnet, was aber nur daran liegt, dass für Letztere häufig keine Schlüssel definiert werden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-350">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="a0f81-351">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-351">**Mitigations**</span></span>

<span data-ttu-id="a0f81-352">Die folgenden Teile der API sind durch die Änderungen veraltet:</span><span class="sxs-lookup"><span data-stu-id="a0f81-352">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="a0f81-353">**`ModelBuilder.Query<>()`** : Rufen Sie stattdessen `ModelBuilder.Entity<>().HasNoKey()` auf, um einen schlüssellosen Entitätstyp festzulegen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-353">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="a0f81-354">Dieses Verhalten wird nach wie vor nicht konventionsgemäß festgelegt, um Fehlkonfigurationen zu vermeiden, wenn ein Primärschlüssel erwartet wird, jedoch nicht mit der Konvention übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-354">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="a0f81-355">**`DbQuery<>`** : Verwenden Sie stattdessen `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="a0f81-355">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="a0f81-356">**`DbContext.Query<>()`** : Verwenden Sie stattdessen `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="a0f81-356">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="a0f81-357">Die Konfigurations-API für Beziehungen abhängiger (owned) Typen wurde geändert</span><span class="sxs-lookup"><span data-stu-id="a0f81-357">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="a0f81-358">[Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="a0f81-358">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="a0f81-359">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-359">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a0f81-360">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-360">**Old behavior**</span></span>

<span data-ttu-id="a0f81-361">Vor Version 3.0 wurde in EF Core die abhängige Beziehung direkt nach dem Aufruf von `OwnsOne` oder `OwnsMany` konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="a0f81-361">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="a0f81-362">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-362">**New behavior**</span></span>

<span data-ttu-id="a0f81-363">Ab Version 3.0 von EF Core ist eine Fluent-API verfügbar, mit der über `WithOwner()` eine Navigationseigenschaft für den Besitzer konfiguriert werden kann.</span><span class="sxs-lookup"><span data-stu-id="a0f81-363">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="a0f81-364">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a0f81-364">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="a0f81-365">Die Konfiguration für die Beziehung zwischen Besitzertyp und abhängigem Typ sollte nun ähnlich wie bei der Konfiguration anderer Beziehungen nach `WithOwner()` verkettet werden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-365">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="a0f81-366">Die Konfiguration für den abhängigen Typ wird jedoch weiterhin nach `OwnsOne()/OwnsMany()` verkettet.</span><span class="sxs-lookup"><span data-stu-id="a0f81-366">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="a0f81-367">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a0f81-367">For example:</span></span>

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

<span data-ttu-id="a0f81-368">Wenn Sie zusätzlich `Entity()`, `HasOne()`, oder `Set()` für das Ziel eines abhängigen Typs aufrufen, wird keine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="a0f81-368">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="a0f81-369">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-369">**Why**</span></span>

<span data-ttu-id="a0f81-370">Diese Änderung wurde vorgenommen, um eine deutlichere Trennung zwischen der Konfiguration des abhängigen Typs und der _Beziehung zum abhängigen Typ_ zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-370">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="a0f81-371">Dadurch werden für Methoden wie `HasForeignKey` Mehrdeutigkeiten und Unklarheiten beseitigt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-371">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="a0f81-372">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-372">**Mitigations**</span></span>

<span data-ttu-id="a0f81-373">Passen Sie für die Beziehungen von abhängigen Typen die Konfiguration so an, dass die neue Fluent-API wie im obigen Beispiel verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="a0f81-373">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="a0f81-374">Abhängige Entitäten, die die Tabelle gemeinsam mit dem Prinzipal verwenden, sind jetzt optional</span><span class="sxs-lookup"><span data-stu-id="a0f81-374">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="a0f81-375">Issue #9005</span><span class="sxs-lookup"><span data-stu-id="a0f81-375">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="a0f81-376">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-376">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a0f81-377">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-377">**Old behavior**</span></span>

<span data-ttu-id="a0f81-378">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="a0f81-378">Consider the following model:</span></span>
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
<span data-ttu-id="a0f81-379">Wenn `OrderDetails` im Besitz von `Order` ist oder explizit derselben Tabelle zugeordnet ist, war vor Version 3.0 in EF Core immer eine `OrderDetails`-Instanz beim Hinzufügen eines neuen `Order`-Objekts erforderlich.</span><span class="sxs-lookup"><span data-stu-id="a0f81-379">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="a0f81-380">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-380">**New behavior**</span></span>

<span data-ttu-id="a0f81-381">Ab Version 3.0 bietet EF Core die Möglichkeit, `Order` ohne `OrderDetails` hinzuzufügen, und alle `OrderDetails`-Eigenschaften, mit Ausnahme des primären Schlüssels, werden Spalten zugeordnet, die NULL-Werte zulassen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-381">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="a0f81-382">Bei Abfragen von EF Core wird `OrderDetails` auf `null` festgelegt, wenn eine der erforderlichen Eigenschaften keinen Wert aufweist oder keine erforderlichen Eigenschaften außer dem primären Schlüssel vorhanden und alle Eigenschaften `null` sind.</span><span class="sxs-lookup"><span data-stu-id="a0f81-382">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="a0f81-383">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-383">**Mitigations**</span></span>

<span data-ttu-id="a0f81-384">Wenn Ihr Modell von der gemeinsamen Nutzung einer Tabelle mit allen optionalen Spalten abhängig ist, aber die Navigation, die darauf zeigt, wahrscheinlich nicht `null` ist, sollte die Anwendung für die Handhabung von Fällen geändert werden, in denen die Navigation `null` ist.</span><span class="sxs-lookup"><span data-stu-id="a0f81-384">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="a0f81-385">Wenn das nicht möglich ist, sollte dem Entitätstyp eine erforderliche Eigenschaft hinzugefügt werden oder mindestens einer Eigenschaft sollte ein Wert ungleich `null` zugewiesen sein.</span><span class="sxs-lookup"><span data-stu-id="a0f81-385">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="a0f81-386">Alle Entitäten, die eine Tabelle mit einer Spalte für das Parallelitätstoken gemeinsam verwenden, müssen diese einer Eigenschaft zuordnen</span><span class="sxs-lookup"><span data-stu-id="a0f81-386">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="a0f81-387">Issue #14154</span><span class="sxs-lookup"><span data-stu-id="a0f81-387">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="a0f81-388">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-388">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a0f81-389">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-389">**Old behavior**</span></span>

<span data-ttu-id="a0f81-390">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="a0f81-390">Consider the following model:</span></span>
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
<span data-ttu-id="a0f81-391">Wenn `OrderDetails` im Besitz von `Order` ist oder explizit derselben Tabelle zugeordnet ist, wird vor Version 3.0 in EF Core durch das alleinige Aktualisieren von `OrderDetails` der `Version`-Wert auf dem Client nicht aktualisiert, und das nächste Update schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="a0f81-391">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="a0f81-392">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-392">**New behavior**</span></span>

<span data-ttu-id="a0f81-393">Ab Version 3.0 gibt EF Core den neuen `Version`-Wert an `Order` weiter, wenn dies Besitzer von `OrderDetails` ist.</span><span class="sxs-lookup"><span data-stu-id="a0f81-393">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="a0f81-394">Andernfalls wird eine Ausnahme während der Modellvalidierung ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="a0f81-394">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="a0f81-395">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-395">**Why**</span></span>

<span data-ttu-id="a0f81-396">Diese Änderung wurde vorgenommen, um einen veralteten Wert für ein Parallelitätstoken zu vermeiden, wenn nur eine der Entitäten, die derselben Tabelle zugeordnet sind, aktualisiert wird.</span><span class="sxs-lookup"><span data-stu-id="a0f81-396">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="a0f81-397">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-397">**Mitigations**</span></span>

<span data-ttu-id="a0f81-398">Alle Entitäten, die die Tabelle gemeinsam nutzen, müssen eine Eigenschaft enthalten, die der Spalte für das Parallelitätstoken zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="a0f81-398">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="a0f81-399">Es ist möglich, eine solche im Schattenzustand zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="a0f81-399">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="a0f81-400">Geerbte Eigenschaften von nicht zugeordneten Typen sind nun einer einzelnen Spalte für alle abgeleiteten Typen zugeordnet</span><span class="sxs-lookup"><span data-stu-id="a0f81-400">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="a0f81-401">Issue #13998</span><span class="sxs-lookup"><span data-stu-id="a0f81-401">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="a0f81-402">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-402">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a0f81-403">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-403">**Old behavior**</span></span>

<span data-ttu-id="a0f81-404">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="a0f81-404">Consider the following model:</span></span>
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

<span data-ttu-id="a0f81-405">Vor Version 3.0 wurde in EF Core die Eigenschaft `ShippingAddress` standardmäßig separaten Spalten für `BulkOrder` und `Order` zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="a0f81-405">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="a0f81-406">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-406">**New behavior**</span></span>

<span data-ttu-id="a0f81-407">Ab Version 3.0 erstellt EF Core nur eine Spalte für `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="a0f81-407">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="a0f81-408">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-408">**Why**</span></span>

<span data-ttu-id="a0f81-409">Der alte Verhalten war unerwartet.</span><span class="sxs-lookup"><span data-stu-id="a0f81-409">The old behavoir was unexpected.</span></span>

<span data-ttu-id="a0f81-410">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-410">**Mitigations**</span></span>

<span data-ttu-id="a0f81-411">Die Eigenschaft kann noch immer explizit einer separaten Spalte für die abgeleiteten Typen zugeordnet werden:</span><span class="sxs-lookup"><span data-stu-id="a0f81-411">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="a0f81-412">Die Konvention zur Fremdschlüsseleigenschaft entspricht nicht mehr dem Namen der Prinzipaleigenschaft</span><span class="sxs-lookup"><span data-stu-id="a0f81-412">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="a0f81-413">Issue #13274</span><span class="sxs-lookup"><span data-stu-id="a0f81-413">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="a0f81-414">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-414">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a0f81-415">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-415">**Old behavior**</span></span>

<span data-ttu-id="a0f81-416">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="a0f81-416">Consider the following model:</span></span>
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
<span data-ttu-id="a0f81-417">Vor Version 3.0 wurde in EF Core gemäß der Konvention die `CustomerId`-Eigenschaft für den Fremdschlüssel verwendet.</span><span class="sxs-lookup"><span data-stu-id="a0f81-417">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="a0f81-418">Wenn `Order` jedoch ein abhängiger Typ ist, wäre `CustomerId` der Primärschlüssel, was normalerweise nicht der Erwartungshaltung entspricht.</span><span class="sxs-lookup"><span data-stu-id="a0f81-418">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="a0f81-419">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-419">**New behavior**</span></span>

<span data-ttu-id="a0f81-420">Ab Version 3.0 werden in EF Core konventionsgemäß keine Eigenschaften mehr für Fremdschlüssel verwendet, wenn diese denselben Namen wie die Prinzipaleigenschaft besitzen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-420">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="a0f81-421">Die Muster für den Namen des Prinzipaltyps, der mit dem Namen der Prinzipaleigenschaft verkettet wird, und für den Navigationsnamen, der mit dem Namen der Prinzipaleigenschaft verkettet wird, werden jedoch weiterhin abgeglichen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-421">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="a0f81-422">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a0f81-422">For example:</span></span>

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

<span data-ttu-id="a0f81-423">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-423">**Why**</span></span>

<span data-ttu-id="a0f81-424">Diese Änderung wurde vorgenommen, um zu vermeiden, dass versehentlich eine Primärschlüsseleigenschaft für den abhängigen Typ definiert wird.</span><span class="sxs-lookup"><span data-stu-id="a0f81-424">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="a0f81-425">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-425">**Mitigations**</span></span>

<span data-ttu-id="a0f81-426">Wenn die Eigenschaft als Fremdschlüssel und daher Teil des Primärschlüssels vorgesehen war, müssen Sie diese explizit als solche festlegen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-426">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="a0f81-427">Die Datenbankverbindung wird jetzt geschlossen, wenn sie nicht mehr verwendet wird, bevor TransactionScope abgeschlossen wurde</span><span class="sxs-lookup"><span data-stu-id="a0f81-427">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="a0f81-428">Issue #14218</span><span class="sxs-lookup"><span data-stu-id="a0f81-428">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="a0f81-429">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-429">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a0f81-430">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-430">**Old behavior**</span></span>

<span data-ttu-id="a0f81-431">Wenn vor Version 3.0 in EF Core der Kontext die Verbindung in einem `TransactionScope` öffnet, bleibt die Verbindung geöffnet, während der aktuelle `TransactionScope` aktiv ist.</span><span class="sxs-lookup"><span data-stu-id="a0f81-431">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="a0f81-432">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-432">**New behavior**</span></span>

<span data-ttu-id="a0f81-433">Ab Version 3.0 schließt EF Core die Verbindung, sobald sie nicht mehr verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="a0f81-433">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="a0f81-434">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-434">**Why**</span></span>

<span data-ttu-id="a0f81-435">Diese Änderung ermöglicht die Verwendung mehrerer Kontexte in demselben `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="a0f81-435">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="a0f81-436">Das neue Verhalten entspricht außerdem EF6.</span><span class="sxs-lookup"><span data-stu-id="a0f81-436">The new behavior also matches EF6.</span></span>

<span data-ttu-id="a0f81-437">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-437">**Mitigations**</span></span>

<span data-ttu-id="a0f81-438">Wenn die Verbindung geöffnet bleiben muss, wird durch einen expliziten Aufruf von `OpenConnection()` sichergestellt, dass EF Core diese nicht vorzeitig schließt:</span><span class="sxs-lookup"><span data-stu-id="a0f81-438">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="a0f81-439">Für jede Eigenschaft wird separat ein ganzzahliger speicherinterner Schlüssel generiert</span><span class="sxs-lookup"><span data-stu-id="a0f81-439">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="a0f81-440">Issue #6872</span><span class="sxs-lookup"><span data-stu-id="a0f81-440">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="a0f81-441">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-441">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a0f81-442">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-442">**Old behavior**</span></span>

<span data-ttu-id="a0f81-443">Vor Version 3.0 wurde in EF Core ein gemeinsam verwendeter Wertegenerator für alle speicherinternen ganzzahligen Schlüsseleigenschaften verwendet.</span><span class="sxs-lookup"><span data-stu-id="a0f81-443">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="a0f81-444">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-444">**New behavior**</span></span>

<span data-ttu-id="a0f81-445">Ab Version 3.0 von EF Core wird für jede ganzzahlige Schlüsseleigenschaft ein eigener Wertegenerator bei der Verwendung der In-Memory Database verwendet.</span><span class="sxs-lookup"><span data-stu-id="a0f81-445">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="a0f81-446">Wenn die Datenbank gelöscht wird, wird die Schlüsselgenerierung für alle Tabellen zurückgesetzt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-446">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="a0f81-447">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-447">**Why**</span></span>

<span data-ttu-id="a0f81-448">Diese Änderung wurde vorgenommen, um die speicherinterne Schlüsselgenerierung enger mit der Schlüsselgenerierung für echte Datenbanken abzustimmen. Außerdem sollen bei Verwendung der In-Memory Database Tests leichter voneinander isoliert werden können.</span><span class="sxs-lookup"><span data-stu-id="a0f81-448">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="a0f81-449">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-449">**Mitigations**</span></span>

<span data-ttu-id="a0f81-450">Wenn Anwendungen von bestimmten speicherinternen Schlüsselwerten abhängig sind, funktionieren Erstere durch die eingeführte Änderung möglicherweise nicht mehr.</span><span class="sxs-lookup"><span data-stu-id="a0f81-450">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="a0f81-451">Versuchen Sie, Abhängigkeiten von bestimmten Schlüsselwerten zu vermeiden, oder passen Sie die Anwendung an das neue Verhalten an.</span><span class="sxs-lookup"><span data-stu-id="a0f81-451">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="a0f81-452">Unterstützungsfelder werden standardmäßig verwendet</span><span class="sxs-lookup"><span data-stu-id="a0f81-452">Backing fields are used by default</span></span>

[<span data-ttu-id="a0f81-453">Issue #12430</span><span class="sxs-lookup"><span data-stu-id="a0f81-453">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="a0f81-454">Diese Änderung wird in Vorschauversion 2 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-454">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="a0f81-455">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-455">**Old behavior**</span></span>

<span data-ttu-id="a0f81-456">Vor Version 3.0 wurde in EF Core mithilfe der Getter- und Settermethoden standardmäßig der Eigenschaftswert gelesen und geschrieben. Das war selbst dann der Fall, wenn das Unterstützungsfeld für eine Eigenschaft nicht bekannt war.</span><span class="sxs-lookup"><span data-stu-id="a0f81-456">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="a0f81-457">Eine Ausnahme war die Abfrageausführung, bei der das Unterstützungsfeld direkt festgelegt wurde, wenn es bekannt war.</span><span class="sxs-lookup"><span data-stu-id="a0f81-457">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="a0f81-458">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-458">**New behavior**</span></span>

<span data-ttu-id="a0f81-459">Ab Version 3.0 wird in EF Core die Eigenschaft mithilfe des Unterstützungsfelds gelesen und geschrieben, wenn dieses für eine Eigenschaft bekannt ist.</span><span class="sxs-lookup"><span data-stu-id="a0f81-459">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="a0f81-460">Dies kann dazu führen, dass eine Anwendung nicht mehr funktioniert, wenn sie auf zusätzliches Verhalten angewiesen ist, das im Code der Getter- und Settermethoden definiert ist.</span><span class="sxs-lookup"><span data-stu-id="a0f81-460">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="a0f81-461">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-461">**Why**</span></span>

<span data-ttu-id="a0f81-462">Diese Änderung wurde vorgenommen, um zu verhindern, dass in EF Core fälschlicherweise immer dann Geschäftslogik ausgelöst wird, wenn Datenbankvorgänge für Entitäten durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-462">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="a0f81-463">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-463">**Mitigations**</span></span>

<span data-ttu-id="a0f81-464">Sie können das Verhalten von vor Version 3.0 wiederherstellen, indem Sie in `ModelBuilder` den Zugriffsmodus für die Eigenschaft konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a0f81-464">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="a0f81-465">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a0f81-465">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="a0f81-466">Eine Ausnahme wird ausgelöst, wenn mehrere kompatible Unterstützungsfelder gefunden werden</span><span class="sxs-lookup"><span data-stu-id="a0f81-466">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="a0f81-467">Issue #12523</span><span class="sxs-lookup"><span data-stu-id="a0f81-467">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="a0f81-468">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-468">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a0f81-469">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-469">**Old behavior**</span></span>

<span data-ttu-id="a0f81-470">Vor Version 3.0 wurde in EF Core ein Unterstützungsfeld auf Grundlage einer Rangfolge ausgewählt, wenn die Regeln zum Suchen eines Eigenschaftsfelds auf mehrere Felder zutrafen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-470">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="a0f81-471">Dabei konnte es vorkommen, dass in mehrdeutigen Fällen das falsche Feld verwendet wurde.</span><span class="sxs-lookup"><span data-stu-id="a0f81-471">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="a0f81-472">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-472">**New behavior**</span></span>

<span data-ttu-id="a0f81-473">Ab Version 3.0 wird in EF Core eine Ausnahme ausgelöst, wenn sich mehrere Felder auf dieselbe Eigenschaft beziehen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-473">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="a0f81-474">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-474">**Why**</span></span>

<span data-ttu-id="a0f81-475">Diese Änderung wurde vorgenommen, um zu vermeiden, dass automatisch ein falsches Feld verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="a0f81-475">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="a0f81-476">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-476">**Mitigations**</span></span>

<span data-ttu-id="a0f81-477">Für Eigenschaften mit mehrdeutigen Unterstützungsfeldern muss das zu verwendende Feld explizit festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-477">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="a0f81-478">Mit der Fluent-API ist dies beispielsweise wie folgt möglich:</span><span class="sxs-lookup"><span data-stu-id="a0f81-478">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="a0f81-479">„Nur-Feld“-Eigenschaftsnamen sollten dem Feldnamen entsprechen</span><span class="sxs-lookup"><span data-stu-id="a0f81-479">Field-only property names should match the field name</span></span>

<span data-ttu-id="a0f81-480">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-480">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a0f81-481">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-481">**Old behavior**</span></span>

<span data-ttu-id="a0f81-482">Vor Version 3.0 konnte in EF Core eine Eigenschaft durch einen Zeichenfolgenwert angegeben werden, und wenn keine Eigenschaft mit diesem Namen im CLR-Typ gefunden wurde, versuchte EF Core, mithilfe von Konventionsregeln ein übereinstimmendes Feld zu finden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-482">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="a0f81-483">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-483">**New behavior**</span></span>

<span data-ttu-id="a0f81-484">Ab Version 3.0 muss in EF Core eine „Nur-Feld“-Eigenschaft mit dem Namen des Felds genau übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-484">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="a0f81-485">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-485">**Why**</span></span>

<span data-ttu-id="a0f81-486">Diese Änderung wurde vorgenommen, um zu vermeiden, dass das gleiche Feld für zwei Eigenschaften mit ähnlichem Namen verwendet wird. Durch die Änderung entsprechen nun auch die Übereinstimmungsregeln für „Nur-Feld“-Eigenschaften den Regeln für Eigenschaften, die CLR-Eigenschaften zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="a0f81-486">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="a0f81-487">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-487">**Mitigations**</span></span>

<span data-ttu-id="a0f81-488">„Nur-Feld“-Eigenschaften müssen so benannt werden wie das Feld, dem sie zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="a0f81-488">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="a0f81-489">In einer späteren Vorschauversion von EF Core 3.0 soll das explizite Konfigurieren eines Feldnamens, der sich vom Eigenschaftsnamen unterscheidet, erneut aktiviert werden:</span><span class="sxs-lookup"><span data-stu-id="a0f81-489">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="a0f81-490">Von AddDbContext/AddDbContextPool werden AddLogging und AddMemoryCache nicht mehr aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-490">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="a0f81-491">Issue #14756</span><span class="sxs-lookup"><span data-stu-id="a0f81-491">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="a0f81-492">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-492">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a0f81-493">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-493">**Old behavior**</span></span>

<span data-ttu-id="a0f81-494">Vor EF Core 3.0 wurden beim Aufruf von `AddDbContext` oder `AddDbContextPool` durch Aufrufe von [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) und [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache) auch Protokollierungs- und Arbeitsspeichercaching-Dienste bei DI registriert.</span><span class="sxs-lookup"><span data-stu-id="a0f81-494">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="a0f81-495">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-495">**New behavior**</span></span>

<span data-ttu-id="a0f81-496">Ab EF Core 3.0 werden diese Dienste durch `AddDbContext` und `AddDbContextPool` nicht mehr bei Dependency Injection (DI) registriert.</span><span class="sxs-lookup"><span data-stu-id="a0f81-496">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="a0f81-497">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-497">**Why**</span></span>

<span data-ttu-id="a0f81-498">Für EF Core 3.0 ist es nicht erforderlich, dass diese Dienste im DI-Container der Anwendung enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="a0f81-498">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="a0f81-499">Wenn `ILoggerFactory` jedoch im DI-Container der Anwendung registriert ist, wird sie nach wie vor von EF Core verwendet.</span><span class="sxs-lookup"><span data-stu-id="a0f81-499">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="a0f81-500">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-500">**Mitigations**</span></span>

<span data-ttu-id="a0f81-501">Wenn Ihre Anwendung diese Dienste benötigt, registrieren Sie sie explizit mit [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) oder [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache) beim DI-Container.</span><span class="sxs-lookup"><span data-stu-id="a0f81-501">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="a0f81-502">DbContext.Entry erkennt nun mit DetectChanges lokale Änderungen</span><span class="sxs-lookup"><span data-stu-id="a0f81-502">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="a0f81-503">Issue #13552</span><span class="sxs-lookup"><span data-stu-id="a0f81-503">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="a0f81-504">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-504">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a0f81-505">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-505">**Old behavior**</span></span>

<span data-ttu-id="a0f81-506">Vor Version 3.0 führte in EF Core ein Aufruf von `DbContext.Entry` dazu, dass Änderungen an allen überwachten Entitäten erkannt wurden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-506">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="a0f81-507">Dadurch wurde sichergestellt, dass der Zustand in `EntityEntry` immer aktuell war.</span><span class="sxs-lookup"><span data-stu-id="a0f81-507">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="a0f81-508">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-508">**New behavior**</span></span>

<span data-ttu-id="a0f81-509">Ab Version 3.0 werden in EF Core Änderungen bei einem Aufruf von `DbContext.Entry` nur in der angegebenen Entität und in allen überwachten Prinzipalentitäten erkannt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-509">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="a0f81-510">Änderungen an anderer Stelle werden daher durch den Aufruf dieser Methode möglicherweise nicht erkannt, was sich auf den Anwendungszustand auswirken kann.</span><span class="sxs-lookup"><span data-stu-id="a0f81-510">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="a0f81-511">Beachten Sie, dass selbst die Erkennung lokaler Änderungen deaktiviert wird, wenn `ChangeTracker.AutoDetectChangesEnabled` auf `false` festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="a0f81-511">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="a0f81-512">Andere Methoden wie `ChangeTracker.Entries` und `SaveChanges`, die eine Änderungserkennung auslösen, führen nach wie vor zu einem vollständigen `DetectChanges`-Vorgang für alle überwachten Entitäten.</span><span class="sxs-lookup"><span data-stu-id="a0f81-512">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="a0f81-513">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-513">**Why**</span></span>

<span data-ttu-id="a0f81-514">Diese Änderung wurde vorgenommen, um die Leistung bei der standardmäßigen Verwendung von `context.Entry` zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="a0f81-514">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="a0f81-515">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-515">**Mitigations**</span></span>

<span data-ttu-id="a0f81-516">Rufen Sie `ChgangeTracker.DetectChanges()` explizit vor dem Aufruf von `Entry` auf, wenn das Verhalten vor Version 3.0 beibehalten werden soll.</span><span class="sxs-lookup"><span data-stu-id="a0f81-516">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="a0f81-517">Schlüssel aus Zeichenfolgen und Bytearrays werden standardmäßig nicht vom Client generiert</span><span class="sxs-lookup"><span data-stu-id="a0f81-517">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="a0f81-518">Issue #14617</span><span class="sxs-lookup"><span data-stu-id="a0f81-518">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="a0f81-519">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-519">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a0f81-520">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-520">**Old behavior**</span></span>

<span data-ttu-id="a0f81-521">Vor Version 3.0 konnten in EF Core `string`- und `byte[]`-Schlüsseleigenschaften verwendet werden, ohne dass explizit ein anderer Wert als NULL festgelegt werden musste.</span><span class="sxs-lookup"><span data-stu-id="a0f81-521">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="a0f81-522">In diesem Fall wurde der Schlüsselwert auf dem Client als GUID generiert und für `byte[]` in Bytes serialisiert.</span><span class="sxs-lookup"><span data-stu-id="a0f81-522">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="a0f81-523">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-523">**New behavior**</span></span>

<span data-ttu-id="a0f81-524">Ab Version 3.0 wird in EF Core eine Ausnahme ausgelöst, wenn kein Schlüsselwert festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="a0f81-524">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="a0f81-525">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-525">**Why**</span></span>

<span data-ttu-id="a0f81-526">Diese Änderung wurde vorgenommen, da vom Client generierte `string`/`byte[]`-Werte in der Regel nicht sinnvoll sind. Das Standardverhalten führte außerdem zu kaum nachvollziehbaren generierten Schlüsselwerten.</span><span class="sxs-lookup"><span data-stu-id="a0f81-526">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="a0f81-527">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-527">**Mitigations**</span></span>

<span data-ttu-id="a0f81-528">Wenn Sie das Verhalten vor Version 3.0 nutzen möchten, müssen Sie explizit festlegen, dass für die Schlüsseleigenschaften generierte Werte verwendet sollen, wenn kein anderer Nicht-NULL-Wert festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="a0f81-528">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="a0f81-529">Dies ist beispielsweise mit der Fluent-API möglich:</span><span class="sxs-lookup"><span data-stu-id="a0f81-529">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="a0f81-530">Alternativ bieten sich auch Datenanmerkungen an:</span><span class="sxs-lookup"><span data-stu-id="a0f81-530">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="a0f81-531">ILoggerFactory ist nun ein bereichsbezogener Dienst</span><span class="sxs-lookup"><span data-stu-id="a0f81-531">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="a0f81-532">Issue #14698</span><span class="sxs-lookup"><span data-stu-id="a0f81-532">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="a0f81-533">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-533">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a0f81-534">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-534">**Old behavior**</span></span>

<span data-ttu-id="a0f81-535">Vor Version 3.0 wurde `ILoggerFactory` in EF Core als Singletondienst registriert.</span><span class="sxs-lookup"><span data-stu-id="a0f81-535">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="a0f81-536">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-536">**New behavior**</span></span>

<span data-ttu-id="a0f81-537">Ab Version 3.0 wird in EF Core `ILoggerFactory` als bereichsbezogener Dienst registriert.</span><span class="sxs-lookup"><span data-stu-id="a0f81-537">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="a0f81-538">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-538">**Why**</span></span>

<span data-ttu-id="a0f81-539">Diese Änderung wurde vorgenommen, um einer `DbContext`-Instanz eine Protokollierung zuordnen zu können. Dadurch werden weitere Funktionen bereitgestellt. In einigen Fällen wird außerdem schädliches Verhalten wie etwa die schnelle Zunahme von internen Dienstanbietern beseitigt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-539">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="a0f81-540">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-540">**Mitigations**</span></span>

<span data-ttu-id="a0f81-541">Diese Änderung wirkt sich nur dann auf den Anwendungscode aus, wenn benutzerdefinierte Dienste auf dem internen Dienstanbieter von EF Core registriert und verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-541">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="a0f81-542">Ein solches Szenario ist aber unüblich.</span><span class="sxs-lookup"><span data-stu-id="a0f81-542">This isn't common.</span></span>
<span data-ttu-id="a0f81-543">In einem derartigen Fall funktioniert fast alles wie vorher. Singletondienste, die von `ILoggerFactory` abhängig sind, müssen jedoch so angepasst werden, dass `ILoggerFactory` auf andere Weise abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="a0f81-543">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="a0f81-544">Erstellen Sie in diesen Situationen bitte im [GitHub-Issuetracker für EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) ein Issue, in dem Sie beschreiben, wie Sie `ILoggerFactory` verwenden. So können wir in Zukunft erneute Breaking Changes vermeiden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-544">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="a0f81-545">Für Proxys mit Lazy Loading (trägem Laden) gilt nicht mehr die Annahme, dass Navigationseigenschaften vollständig geladen sind</span><span class="sxs-lookup"><span data-stu-id="a0f81-545">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="a0f81-546">Issue #12780</span><span class="sxs-lookup"><span data-stu-id="a0f81-546">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="a0f81-547">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-547">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a0f81-548">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-548">**Old behavior**</span></span>

<span data-ttu-id="a0f81-549">Vor Version 3.0 gab es in EF Core nach der Freigabe eines `DbContext`-Objekts keine Möglichkeit, zu ermitteln, ob eine aus dem Kontext abgerufene Navigationseigenschaft für eine Entität vollständig geladen war.</span><span class="sxs-lookup"><span data-stu-id="a0f81-549">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="a0f81-550">Stattdessen galt für Proxys die Annahme, dass eine Verweisnavigation geladen ist, falls für diese ein anderer Wert als NULL vorliegt. Außerdem wurde davon ausgegangen, dass eine Sammlungsnavigation geladen ist, wenn sie nicht leer ist.</span><span class="sxs-lookup"><span data-stu-id="a0f81-550">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="a0f81-551">In diesen Fällen entsprach das Lazy Loading einer Nulloperation („No-Op“).</span><span class="sxs-lookup"><span data-stu-id="a0f81-551">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="a0f81-552">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-552">**New behavior**</span></span>

<span data-ttu-id="a0f81-553">Ab Version 3.0 überwachen in EF Core Proxys, ob eine Navigationseigenschaft geladen ist.</span><span class="sxs-lookup"><span data-stu-id="a0f81-553">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="a0f81-554">Wenn also auf eine Navigationseigenschaft zugegriffen werden soll, die nach der Freigabe des Kontexts geladen wird, ist dies immer eine No-Op-Operation. Das ist selbst dann der Fall, wenn die geladene Navigation leer oder NULL ist.</span><span class="sxs-lookup"><span data-stu-id="a0f81-554">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="a0f81-555">Wenn umgekehrt auf eine nicht geladene Navigationseigenschaft zugegriffen werden soll und der Kontext bereits freigegeben wurde, wird eine Ausnahme ausgelöst. Dieser Fall tritt auch dann ein, wenn die Navigationseigenschaft eine nicht leere Sammlung ist.</span><span class="sxs-lookup"><span data-stu-id="a0f81-555">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="a0f81-556">In dieser Situation wird also vom Anwendungscode versucht, zu einem ungültigen Zeitpunkt Lazy Loading durchzuführen. Die Anwendung sollte so angepasst werden, dass dieser Vorgang vermieden wird.</span><span class="sxs-lookup"><span data-stu-id="a0f81-556">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="a0f81-557">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-557">**Why**</span></span>

<span data-ttu-id="a0f81-558">Diese Änderung wurde vorgenommen, um das Verhalten beim Lazy Loading einer freigegebenen `DbContext`-Instanz konsistent und korrekt zu gestalten.</span><span class="sxs-lookup"><span data-stu-id="a0f81-558">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="a0f81-559">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-559">**Mitigations**</span></span>

<span data-ttu-id="a0f81-560">Aktualisieren Sie den Anwendungscode so, dass kein Lazy Loading durchgeführt wird, nachdem der Kontext freigegeben wurde. Alternativ können Sie auch, wie in der Ausnahmemeldung beschrieben wird, eine No-Op-Operation festlegen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-560">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="a0f81-561">Die Erstellung zu vieler interner Dienstanbieter führt standardmäßig zu einem Fehler</span><span class="sxs-lookup"><span data-stu-id="a0f81-561">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="a0f81-562">Issue #10236</span><span class="sxs-lookup"><span data-stu-id="a0f81-562">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="a0f81-563">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-563">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a0f81-564">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-564">**Old behavior**</span></span>

<span data-ttu-id="a0f81-565">Vor Version 3.0 wurde in EF Core eine Warnung für eine Anwendung protokolliert, die unverhältnismäßig viele interne Dienstanbieter erstellte.</span><span class="sxs-lookup"><span data-stu-id="a0f81-565">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="a0f81-566">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-566">**New behavior**</span></span>

<span data-ttu-id="a0f81-567">Ab Version 3.0 wird diese Warnung in EF Core als Fehler betrachtet, und eine Ausnahme wird ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="a0f81-567">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="a0f81-568">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-568">**Why**</span></span>

<span data-ttu-id="a0f81-569">Diese Änderung wurde vorgenommen, damit auf den oben beschriebenen Fall explizit hingewiesen wird. So soll die Entwicklung von besserem Anwendungscode erleichtert werden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-569">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="a0f81-570">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-570">**Mitigations**</span></span>

<span data-ttu-id="a0f81-571">Wenn dieser Fehler auftritt, müssen Sie die Grundursache ermitteln und die Erstellung vieler interner Dienstanbieter unterbinden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-571">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="a0f81-572">Sie können den Fehler allerdings auch wieder in eine Warnung konvertierten (oder ihn ignorieren), indem Sie `DbContextOptionsBuilder` konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a0f81-572">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="a0f81-573">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a0f81-573">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="a0f81-574">Neues Verhalten für HasOne/HasMany bei Aufruf mit einer einzelnen Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="a0f81-574">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="a0f81-575">Issue #9171</span><span class="sxs-lookup"><span data-stu-id="a0f81-575">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="a0f81-576">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-576">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a0f81-577">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-577">**Old behavior**</span></span>

<span data-ttu-id="a0f81-578">Vor EF Core 3.0 wurde Code, der `HasOne` oder `HasMany` mit einer einzelnen Zeichenfolge aufruft, auf irritierende Weise interpretiert.</span><span class="sxs-lookup"><span data-stu-id="a0f81-578">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="a0f81-579">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a0f81-579">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="a0f81-580">Der Code scheint `Samurai` mit einem anderen Entitätstyp über die `Entrance`-Navigationseigenschaft zu verknüpfen, die möglicherweise privat ist.</span><span class="sxs-lookup"><span data-stu-id="a0f81-580">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="a0f81-581">Tatsächlich versucht dieser Code, eine Beziehung zu einem Entitätstyp namens `Entrance` ohne Navigationseigenschaft herzustellen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-581">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="a0f81-582">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-582">**New behavior**</span></span>

<span data-ttu-id="a0f81-583">Ab EF Core 3.0 führt der obige Code jetzt die erwartete Aktion durch.</span><span class="sxs-lookup"><span data-stu-id="a0f81-583">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="a0f81-584">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-584">**Why**</span></span>

<span data-ttu-id="a0f81-585">Das alte Verhalten war sehr verwirrend, vor allem beim Lesen des Konfigurationscodes und bei der Suche nach Fehlern.</span><span class="sxs-lookup"><span data-stu-id="a0f81-585">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="a0f81-586">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-586">**Mitigations**</span></span>

<span data-ttu-id="a0f81-587">Dies führt nur zu Fehlern in Anwendungen, die Beziehungen explizit unter Verwendung von Zeichenfolgen für Typnamen und ohne explizite Angabe der Navigationseigenschaft konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a0f81-587">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="a0f81-588">Dies ist nicht üblich.</span><span class="sxs-lookup"><span data-stu-id="a0f81-588">This is not common.</span></span>
<span data-ttu-id="a0f81-589">Das vorherige Verhalten kann durch explizite Übergabe von `null` für den Namen der Navigationseigenschaft erzielt werden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-589">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="a0f81-590">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a0f81-590">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="a0f81-591">Der Rückgabetyp für mehrere asynchrone Methoden wurde von Task in ValueTask geändert</span><span class="sxs-lookup"><span data-stu-id="a0f81-591">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="a0f81-592">Issue #15184</span><span class="sxs-lookup"><span data-stu-id="a0f81-592">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="a0f81-593">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-593">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a0f81-594">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-594">**Old behavior**</span></span>

<span data-ttu-id="a0f81-595">Die folgenden asynchronen Methoden gaben zuvor `Task<T>` zurück:</span><span class="sxs-lookup"><span data-stu-id="a0f81-595">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="a0f81-596">`ValueGenerator.NextValueAsync()` (und abgeleitete Klassen)</span><span class="sxs-lookup"><span data-stu-id="a0f81-596">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="a0f81-597">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-597">**New behavior**</span></span>

<span data-ttu-id="a0f81-598">Die oben genannten Methoden geben nun `ValueTask<T>` über dieselbe `T` wie zuvor zurück.</span><span class="sxs-lookup"><span data-stu-id="a0f81-598">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="a0f81-599">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-599">**Why**</span></span>

<span data-ttu-id="a0f81-600">Durch diese Änderung verringert sich die Anzahl der Heapzuordnungen, die beim Aufrufen dieser Methoden entstehen, und dies führt zu einer Verbesserung der allgemeinen Leistung.</span><span class="sxs-lookup"><span data-stu-id="a0f81-600">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="a0f81-601">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-601">**Mitigations**</span></span>

<span data-ttu-id="a0f81-602">Anwendungen, die einfach die oben genannten APIs erwarten, müssen nur neu kompiliert werden – Quelländerungen sind nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="a0f81-602">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="a0f81-603">Eine komplexere Verwendung (z.B. Übergeben der zurückgegebenen `Task` an `Task.WhenAny()`) erfordern in der Regel, dass die zurückgegebene `ValueTask<T>` durch einen Aufruf von `AsTask()` in `Task<T>` konvertiert wird.</span><span class="sxs-lookup"><span data-stu-id="a0f81-603">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="a0f81-604">Beachten Sie, dass dadurch die mit dieser Änderung verbundene Verringerung der Zuordnungen aufgehoben wird.</span><span class="sxs-lookup"><span data-stu-id="a0f81-604">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="a0f81-605">Die Anmerkung Relational:TypeMapping heißt nun nur noch TypeMapping</span><span class="sxs-lookup"><span data-stu-id="a0f81-605">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="a0f81-606">Issue #9913</span><span class="sxs-lookup"><span data-stu-id="a0f81-606">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="a0f81-607">Diese Änderung wird in Vorschauversion 2 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-607">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="a0f81-608">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-608">**Old behavior**</span></span>

<span data-ttu-id="a0f81-609">Der Anmerkungsname für Typzuordnungsanmerkungen war bisher Relational:TypeMapping.</span><span class="sxs-lookup"><span data-stu-id="a0f81-609">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="a0f81-610">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-610">**New behavior**</span></span>

<span data-ttu-id="a0f81-611">Der Anmerkungsname für Typzuordnungsanmerkungen lautet nun TypeMapping.</span><span class="sxs-lookup"><span data-stu-id="a0f81-611">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="a0f81-612">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-612">**Why**</span></span>

<span data-ttu-id="a0f81-613">Typzuordnungen werden nicht mehr ausschließlich für Anbieter relationaler Datenbanken verwendet.</span><span class="sxs-lookup"><span data-stu-id="a0f81-613">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="a0f81-614">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-614">**Mitigations**</span></span>

<span data-ttu-id="a0f81-615">Diese Änderung ist nur dann ein Breaking Change, wenn Anwendungen direkt über eine Anmerkung auf die Typzuordnung zugreifen. Dieses Szenario ist jedoch unüblich.</span><span class="sxs-lookup"><span data-stu-id="a0f81-615">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="a0f81-616">Verzichten Sie daher darauf, die Anmerkung direkt zu verwenden, und greifen Sie stattdessen über die Fluent-API auf Typzuordnungen zu, um das Problem zu beheben.</span><span class="sxs-lookup"><span data-stu-id="a0f81-616">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="a0f81-617">ToTable löst für einen abgeleiteten Typ eine Ausnahme aus</span><span class="sxs-lookup"><span data-stu-id="a0f81-617">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="a0f81-618">Issue #11811</span><span class="sxs-lookup"><span data-stu-id="a0f81-618">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="a0f81-619">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-619">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a0f81-620">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-620">**Old behavior**</span></span>

<span data-ttu-id="a0f81-621">Vor Version 3.0 wurde in EF Core der Aufruf von `ToTable()` für einen abgeleiteten Typ ignoriert, da TPH die einzige Strategie zur Vererbungszuordnung war und diese in unzulässigen Fällen angewendet wurde.</span><span class="sxs-lookup"><span data-stu-id="a0f81-621">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="a0f81-622">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-622">**New behavior**</span></span>

<span data-ttu-id="a0f81-623">Ab Version 3.0 wird in EF Core eine Ausnahme ausgelöst, um später eine unerwartete Zuordnung zu vermeiden, wenn `ToTable()` für einen abgeleiteten Typ aufgerufen wird. Dieser Schritt dient auch als Vorbereitung für die TPT- und TPC-Unterstützung, die in einem künftigen Release folgen wird.</span><span class="sxs-lookup"><span data-stu-id="a0f81-623">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="a0f81-624">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-624">**Why**</span></span>

<span data-ttu-id="a0f81-625">Derzeit ist es nicht zulässig, einen abgeleiteten Typ einer anderen Tabelle zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-625">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="a0f81-626">Durch diese Änderung werden Breaking Changes vermieden, wenn der beschriebene Vorgang in Zukunft zulässig wird.</span><span class="sxs-lookup"><span data-stu-id="a0f81-626">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="a0f81-627">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-627">**Mitigations**</span></span>

<span data-ttu-id="a0f81-628">Entfernen Sie alle Zuordnungen von abgeleiteten Typen zu anderen Tabellen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-628">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="a0f81-629">ForSqlServerHasIndex wurde durch HasIndex ersetzt</span><span class="sxs-lookup"><span data-stu-id="a0f81-629">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="a0f81-630">Issue #12366</span><span class="sxs-lookup"><span data-stu-id="a0f81-630">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="a0f81-631">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-631">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a0f81-632">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-632">**Old behavior**</span></span>

<span data-ttu-id="a0f81-633">Vor Version 3.0 konnten in EF Core mit `ForSqlServerHasIndex().ForSqlServerInclude()` Spalten konfiguriert werden, die mit `INCLUDE` verwendet wurden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-633">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="a0f81-634">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-634">**New behavior**</span></span>

<span data-ttu-id="a0f81-635">Ab Version 3.0 wird in EF Core die Nutzung von `Include` für einen Index auf der relationalen Ebene unterstützt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-635">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="a0f81-636">Verwenden Sie `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="a0f81-636">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="a0f81-637">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-637">**Why**</span></span>

<span data-ttu-id="a0f81-638">Diese Änderung wurde vorgenommen, um die API für Indizes mit `Include` an einer zentralen Stelle für alle Datenbankanbieter zusammenzuführen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-638">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="a0f81-639">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-639">**Mitigations**</span></span>

<span data-ttu-id="a0f81-640">Verwenden Sie wie oben beschrieben die neue API.</span><span class="sxs-lookup"><span data-stu-id="a0f81-640">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="a0f81-641">Metadaten-API-Änderungen</span><span class="sxs-lookup"><span data-stu-id="a0f81-641">Metadata API changes</span></span>

[<span data-ttu-id="a0f81-642">Issue #214</span><span class="sxs-lookup"><span data-stu-id="a0f81-642">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="a0f81-643">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-643">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a0f81-644">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-644">**New behavior**</span></span>

<span data-ttu-id="a0f81-645">Die folgenden Eigenschaften wurden in Erweiterungsmethoden konvertiert:</span><span class="sxs-lookup"><span data-stu-id="a0f81-645">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="a0f81-646">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-646">**Why**</span></span>

<span data-ttu-id="a0f81-647">Mit dieser Änderung wird die Implementierung der oben genannten Schnittstellen vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="a0f81-647">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="a0f81-648">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-648">**Mitigations**</span></span>

<span data-ttu-id="a0f81-649">Verwenden Sie die neuen Erweiterungsmethoden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-649">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="a0f81-650">Anbieterspezifische Metadaten-API-Änderungen</span><span class="sxs-lookup"><span data-stu-id="a0f81-650">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="a0f81-651">Issue #214</span><span class="sxs-lookup"><span data-stu-id="a0f81-651">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="a0f81-652">Diese Änderung wird in Vorschauversion 6 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-652">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="a0f81-653">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-653">**New behavior**</span></span>

<span data-ttu-id="a0f81-654">Die anbieterspezifischen Erweiterungsmethoden werden vereinfacht:</span><span class="sxs-lookup"><span data-stu-id="a0f81-654">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.GetSqlServerIsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.ForSqlServerUseIdentityColumn()`

<span data-ttu-id="a0f81-655">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-655">**Why**</span></span>

<span data-ttu-id="a0f81-656">Mit dieser Änderung wird die Implementierung der oben genannten Erweiterungsmethoden vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="a0f81-656">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="a0f81-657">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-657">**Mitigations**</span></span>

<span data-ttu-id="a0f81-658">Verwenden Sie die neuen Erweiterungsmethoden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-658">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="a0f81-659">EF Core sendet keine PRAGMA-Anweisungen mehr, um Fremdschlüssel in SQLite zu erzwingen</span><span class="sxs-lookup"><span data-stu-id="a0f81-659">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="a0f81-660">Issue #12151</span><span class="sxs-lookup"><span data-stu-id="a0f81-660">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="a0f81-661">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-661">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="a0f81-662">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-662">**Old behavior**</span></span>

<span data-ttu-id="a0f81-663">Vor Version 3.0 wurden in EF Core `PRAGMA foreign_keys = 1`-Anweisungen gesendet, wenn eine Verbindung mit SQLite hergestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="a0f81-663">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="a0f81-664">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-664">**New behavior**</span></span>

<span data-ttu-id="a0f81-665">Ab Version 3.0 werden in EF Core keine `PRAGMA foreign_keys = 1`-Anweisungen mehr gesendet, wenn eine Verbindung mit SQLite hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="a0f81-665">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="a0f81-666">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-666">**Why**</span></span>

<span data-ttu-id="a0f81-667">Diese Änderung wurde vorgenommen, da von EF Core standardmäßig `SQLitePCLRaw.bundle_e_sqlite3` verwendet wird. Dadurch ist die Erzwingung von Fremdschlüsseln standardmäßig aktiviert und muss nicht jedes Mal explizit aktiviert werden, wenn eine Verbindung hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="a0f81-667">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="a0f81-668">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-668">**Mitigations**</span></span>

<span data-ttu-id="a0f81-669">Fremdschlüssel sind in SQLitePCLRaw.bundle_e_sqlite3 standardmäßig aktiviert. Dieses Bundle wird standardmäßig von EF Core verwendet.</span><span class="sxs-lookup"><span data-stu-id="a0f81-669">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="a0f81-670">In anderen Fällen können Fremdschlüssel durch die Angabe `Foreign Keys=True` in der Verbindungszeichenfolge aktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-670">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="a0f81-671">SQLitePCLRaw.bundle_e_sqlite3 ist nun eine Abhängigkeit von Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="a0f81-671">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="a0f81-672">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-672">**Old behavior**</span></span>

<span data-ttu-id="a0f81-673">Vor Version 3.0 wurde von EF Core `SQLitePCLRaw.bundle_green` verwendet.</span><span class="sxs-lookup"><span data-stu-id="a0f81-673">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="a0f81-674">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-674">**New behavior**</span></span>

<span data-ttu-id="a0f81-675">Ab Version 3.0 wird von EF Core `SQLitePCLRaw.bundle_e_sqlite3` verwendet.</span><span class="sxs-lookup"><span data-stu-id="a0f81-675">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="a0f81-676">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-676">**Why**</span></span>

<span data-ttu-id="a0f81-677">Diese Änderung wurde vorgenommen, damit die unter iOS verwendete Version von SQLite sich konsistent zu Versionen auf anderen Plattformen verhält.</span><span class="sxs-lookup"><span data-stu-id="a0f81-677">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="a0f81-678">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-678">**Mitigations**</span></span>

<span data-ttu-id="a0f81-679">Konfigurieren Sie `Microsoft.Data.Sqlite` so, dass ein anderes `SQLitePCLRaw`-Bundle verwendet wird, um die native SQLite-Version unter iOS zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-679">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="a0f81-680">GUID-Werte werden jetzt als TEXT in SQLite gespeichert</span><span class="sxs-lookup"><span data-stu-id="a0f81-680">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="a0f81-681">Issue #15078</span><span class="sxs-lookup"><span data-stu-id="a0f81-681">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="a0f81-682">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-682">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a0f81-683">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-683">**Old behavior**</span></span>

<span data-ttu-id="a0f81-684">GUID-Werte wurden in der Vergangenheit in SQLite als BLOB-Werte gespeichert.</span><span class="sxs-lookup"><span data-stu-id="a0f81-684">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="a0f81-685">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-685">**New behavior**</span></span>

<span data-ttu-id="a0f81-686">GUID-Werte werden jetzt als TEXT gespeichert.</span><span class="sxs-lookup"><span data-stu-id="a0f81-686">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="a0f81-687">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-687">**Why**</span></span>

<span data-ttu-id="a0f81-688">Das Binärformat der GUIDs ist nicht standardisiert.</span><span class="sxs-lookup"><span data-stu-id="a0f81-688">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="a0f81-689">Das Speichern der Werte als TEXT führt dazu, dass die Datenbank mit anderen Technologien eher kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="a0f81-689">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="a0f81-690">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-690">**Mitigations**</span></span>

<span data-ttu-id="a0f81-691">Sie können vorhandene Datenbanken zum neuen Format migrieren, indem Sie SQL-Code wie den folgenden ausführen:</span><span class="sxs-lookup"><span data-stu-id="a0f81-691">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="a0f81-692">Sie können in EF Core auch weiterhin das alte Verhalten verwenden, indem Sie einen Wertkonverter für diese Eigenschaften konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a0f81-692">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="a0f81-693">Microsoft.Data.Sqlite kann weiterhin GUID-Werte aus BLOB- und TEXT-Spalten lesen. Da sich jedoch das Standardformat für Parameter und Konstanten geändert hat, müssen Sie wahrscheinlich Aktionen für die meisten GUID-Szenarios vornehmen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-693">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="a0f81-694">Char-Werte werden jetzt als TEXT in SQLite gespeichert</span><span class="sxs-lookup"><span data-stu-id="a0f81-694">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="a0f81-695">Issue #15020</span><span class="sxs-lookup"><span data-stu-id="a0f81-695">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="a0f81-696">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-696">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a0f81-697">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-697">**Old behavior**</span></span>

<span data-ttu-id="a0f81-698">Char-Werte wurden in der Vergangenheit in SQLite als INTEGER-Werte gespeichert.</span><span class="sxs-lookup"><span data-stu-id="a0f81-698">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="a0f81-699">Der Char-Wert *A* wurde z.B. als INTEGER-Wert 65 gespeichert.</span><span class="sxs-lookup"><span data-stu-id="a0f81-699">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="a0f81-700">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-700">**New behavior**</span></span>

<span data-ttu-id="a0f81-701">Char-Werte werden jetzt als TEXT gespeichert.</span><span class="sxs-lookup"><span data-stu-id="a0f81-701">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="a0f81-702">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-702">**Why**</span></span>

<span data-ttu-id="a0f81-703">Das Speichern der Werte als TEXT ist naheliegender und führt dazu, dass die Datenbank mit anderen Technologien eher kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="a0f81-703">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="a0f81-704">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-704">**Mitigations**</span></span>

<span data-ttu-id="a0f81-705">Sie können vorhandene Datenbanken zum neuen Format migrieren, indem Sie SQL-Code wie den folgenden ausführen:</span><span class="sxs-lookup"><span data-stu-id="a0f81-705">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="a0f81-706">Sie können in EF Core auch weiterhin das alte Verhalten verwenden, indem Sie einen Wertkonverter für diese Eigenschaften konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a0f81-706">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="a0f81-707">Microsoft.Data.Sqlite kann auch weiterhin Zeichenwerte aus INTEGER- und TEXT-Spalten lesen, weshalb es sein kann, dass in einigen Szenarios keine weiteren Maßnahmen erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="a0f81-707">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="a0f81-708">Migrations-IDs werden jetzt mit dem Kalender der invarianten Kultur generiert</span><span class="sxs-lookup"><span data-stu-id="a0f81-708">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="a0f81-709">Issue #12978</span><span class="sxs-lookup"><span data-stu-id="a0f81-709">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="a0f81-710">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-710">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a0f81-711">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-711">**Old behavior**</span></span>

<span data-ttu-id="a0f81-712">Migrations-IDs wurden versehentlich mit dem Kalender der aktuellen Kultur generiert.</span><span class="sxs-lookup"><span data-stu-id="a0f81-712">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="a0f81-713">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-713">**New behavior**</span></span>

<span data-ttu-id="a0f81-714">Migrations-IDs werden jetzt immer mit dem Kalender der invarianten Kultur generiert (gregorianischer Kalender).</span><span class="sxs-lookup"><span data-stu-id="a0f81-714">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="a0f81-715">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-715">**Why**</span></span>

<span data-ttu-id="a0f81-716">Die Reihenfolge der Migrationen ist beim Aktualisieren einer Datenbank oder Auflösen von Mergekonflikten wesentlich.</span><span class="sxs-lookup"><span data-stu-id="a0f81-716">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="a0f81-717">Wenn der invariante Kalender verwendet wird, werden Probleme bei der Reihenfolge vermieden, die entstehen, wenn Teammitglieder unterschiedliche Systemkalender verwenden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-717">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="a0f81-718">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-718">**Mitigations**</span></span>

<span data-ttu-id="a0f81-719">Diese Änderung betrifft jeden Benutzer, der einen Kalender verwendet, der nicht gregorianisch ist und bei dem die Jahreszahl höher als die des gregorianischen Kalenders ist (wie z.B. in der buddhistischen Zeitrechnung).</span><span class="sxs-lookup"><span data-stu-id="a0f81-719">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="a0f81-720">Vorhandene Migrations-IDs müssen aktualisiert werden, damit neue Migrationen in der Reihenfolge hinter vorhandenen Migrationen eingeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-720">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="a0f81-721">Sie können die Migrations-ID im Migration-Attribut in der Designerdatei der Migration finden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-721">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="a0f81-722">Die Tabelle mit dem Migrationsverlauf muss auch aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-722">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="a0f81-723">UseRowNumberForPaging wurde entfernt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-723">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="a0f81-724">Issue #16400</span><span class="sxs-lookup"><span data-stu-id="a0f81-724">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="a0f81-725">Diese Änderung wird in Vorschauversion 6 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-725">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="a0f81-726">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-726">**Old behavior**</span></span>

<span data-ttu-id="a0f81-727">Vor EF Core 3.0 konnte `UseRowNumberForPaging` zum Generieren von SQL-Code für die Paginierung verwendet werden, der mit SQL Server 2008 kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="a0f81-727">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="a0f81-728">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-728">**New behavior**</span></span>

<span data-ttu-id="a0f81-729">Ab EF Core 3.0 generiert EF nur noch SQL-Code für die Paginierung, die mit höheren SQL Server-Versionen kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="a0f81-729">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="a0f81-730">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-730">**Why**</span></span>

<span data-ttu-id="a0f81-731">Wir führen diese Änderung ein, weil [SQL Server 2008 kein unterstütztes Produkt mehr ist](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) und weil die Aktualisierung dieses Features auf die in EF Core 3.0 vorgenommenen Änderungen an Abfragen ein wichtiger Schritt ist.</span><span class="sxs-lookup"><span data-stu-id="a0f81-731">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="a0f81-732">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-732">**Mitigations**</span></span>

<span data-ttu-id="a0f81-733">Es empfiehlt sich, eine Update auf eine neuere Version von SQL Server durchzuführen oder einen höheren Kompatibilitätsgrad zu verwenden, damit der generierte SQL-Code unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="a0f81-733">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="a0f81-734">Wenn dies bei Ihnen nicht möglich ist, fügen Sie einen [Kommentar zum Issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) ein, und geben Sie alle relevanten Informationen an.</span><span class="sxs-lookup"><span data-stu-id="a0f81-734">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="a0f81-735">Je nach Feedback der Benutzer wird diese Entscheidung möglicherweise noch einmal überprüft.</span><span class="sxs-lookup"><span data-stu-id="a0f81-735">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="a0f81-736">Erweiterungsinformationen und Metadaten aus IDbContextOptionsExtension entfernt</span><span class="sxs-lookup"><span data-stu-id="a0f81-736">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="a0f81-737">Issue #16119</span><span class="sxs-lookup"><span data-stu-id="a0f81-737">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="a0f81-738">Diese Änderung wird in Vorschauversion 7 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-738">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="a0f81-739">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-739">**Old behavior**</span></span>

<span data-ttu-id="a0f81-740">`IDbContextOptionsExtension` enthielt Methoden zum Bereitstellen von Metadaten zur Erweiterung.</span><span class="sxs-lookup"><span data-stu-id="a0f81-740">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="a0f81-741">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-741">**New behavior**</span></span>

<span data-ttu-id="a0f81-742">Diese Methoden wurden in eine neue abstrakte `DbContextOptionsExtensionInfo`-Basisklasse verschoben, die von einer neuen `IDbContextOptionsExtension.Info`-Eigenschaft zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-742">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="a0f81-743">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-743">**Why**</span></span>

<span data-ttu-id="a0f81-744">In den Releases zwischen 2.0 und 3.0 mussten wir diese Methoden mehrmals ergänzen oder ändern.</span><span class="sxs-lookup"><span data-stu-id="a0f81-744">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="a0f81-745">Durch die Bereitstellung dieser Methoden in einer neuen abstrakten Basisklasse lassen sich solche Änderungen einfacher einführen, ohne dass es zu Konflikten mit vorhandenen Erweiterungen kommt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-745">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="a0f81-746">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-746">**Mitigations**</span></span>

<span data-ttu-id="a0f81-747">Erweiterungen wurden aktualisiert und verwenden das neue Muster.</span><span class="sxs-lookup"><span data-stu-id="a0f81-747">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="a0f81-748">Beispiele finden sich in den vielen Implementierungen von `IDbContextOptionsExtension` für verschiedene Arten von Erweiterungen im EF Core-Quellcode.</span><span class="sxs-lookup"><span data-stu-id="a0f81-748">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="a0f81-749">LogQueryPossibleExceptionWithAggregateOperator wurde umbenannt</span><span class="sxs-lookup"><span data-stu-id="a0f81-749">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="a0f81-750">Issue #10985</span><span class="sxs-lookup"><span data-stu-id="a0f81-750">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="a0f81-751">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-751">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a0f81-752">**Änderung**</span><span class="sxs-lookup"><span data-stu-id="a0f81-752">**Change**</span></span>

<span data-ttu-id="a0f81-753">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` wurde umbenannt in `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="a0f81-753">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="a0f81-754">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-754">**Why**</span></span>

<span data-ttu-id="a0f81-755">Die Benennung dieses Warnereignisses wurde an allen anderen Warnereignissen ausgerichtet.</span><span class="sxs-lookup"><span data-stu-id="a0f81-755">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="a0f81-756">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-756">**Mitigations**</span></span>

<span data-ttu-id="a0f81-757">Verwenden Sie den neuen Namen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-757">Use the new name.</span></span> <span data-ttu-id="a0f81-758">(Beachten Sie, dass sich die Ereignis-ID-Nummer nicht geändert hat.)</span><span class="sxs-lookup"><span data-stu-id="a0f81-758">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="a0f81-759">Verdeutlichen der API für Einschränkungsnamen von Fremdschlüsseln</span><span class="sxs-lookup"><span data-stu-id="a0f81-759">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="a0f81-760">Issue #10730</span><span class="sxs-lookup"><span data-stu-id="a0f81-760">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="a0f81-761">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-761">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a0f81-762">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-762">**Old behavior**</span></span>

<span data-ttu-id="a0f81-763">Vor EF Core 3.0 wurden Einschränkungsnamen von Fremdschlüsseln einfach als „Namen“ bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="a0f81-763">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="a0f81-764">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a0f81-764">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="a0f81-765">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-765">**New behavior**</span></span>

<span data-ttu-id="a0f81-766">Ab EF Core 3.0 werden Einschränkungsnamen von Fremdschlüsseln nun als „Einschränkungsnamen“ bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="a0f81-766">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="a0f81-767">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a0f81-767">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="a0f81-768">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-768">**Why**</span></span>

<span data-ttu-id="a0f81-769">Durch diese Änderung wird Konsistenz für Benennungen in diesem Bereich gewährleistet, und es wird verdeutlicht, dass es sich dabei um den Namen der Fremdschlüsseleinschränkung handelt und nicht um den Spalten- oder Eigenschaftennamen, auf deren Grundlage der Fremdschlüssel definiert ist.</span><span class="sxs-lookup"><span data-stu-id="a0f81-769">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="a0f81-770">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-770">**Mitigations**</span></span>

<span data-ttu-id="a0f81-771">Verwenden Sie den neuen Namen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-771">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="a0f81-772">„IRelationalDatabaseCreator.HasTables/HasTablesAsync“ ist jetzt öffentlich</span><span class="sxs-lookup"><span data-stu-id="a0f81-772">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="a0f81-773">Issue #15997</span><span class="sxs-lookup"><span data-stu-id="a0f81-773">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="a0f81-774">Diese Änderung wird in Vorschauversion 7 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-774">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="a0f81-775">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-775">**Old behavior**</span></span>

<span data-ttu-id="a0f81-776">Vor EF Core 3.0 waren diese Methoden geschützt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-776">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="a0f81-777">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-777">**New behavior**</span></span>

<span data-ttu-id="a0f81-778">Ab EF Core 3.0 sind diese Methoden öffentlich.</span><span class="sxs-lookup"><span data-stu-id="a0f81-778">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="a0f81-779">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-779">**Why**</span></span>

<span data-ttu-id="a0f81-780">Diese Methoden werden von EF verwendet, um zu ermitteln, ob eine Datenbank erstellt wurde, aber leer ist.</span><span class="sxs-lookup"><span data-stu-id="a0f81-780">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="a0f81-781">Dies kann auch außerhalb von EF nützlich sein, um zu ermitteln, ob Migrationen angewendet werden sollen oder nicht.</span><span class="sxs-lookup"><span data-stu-id="a0f81-781">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="a0f81-782">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-782">**Mitigations**</span></span>

<span data-ttu-id="a0f81-783">Der Zugriff auf Überschreibungen wurde geändert.</span><span class="sxs-lookup"><span data-stu-id="a0f81-783">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="a0f81-784">Microsoft.EntityFrameworkCore.Design ist jetzt ein DevelopmentDependency-Paket</span><span class="sxs-lookup"><span data-stu-id="a0f81-784">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="a0f81-785">Issue #11506</span><span class="sxs-lookup"><span data-stu-id="a0f81-785">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="a0f81-786">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-786">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="a0f81-787">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-787">**Old behavior**</span></span>

<span data-ttu-id="a0f81-788">Vor EF Core 3.0 war Microsoft.EntityFrameworkCore.Design ein reguläres NuGet-Paket, auf dessen Assembly von Projekten verwiesen werden kann, die davon abhängig sind.</span><span class="sxs-lookup"><span data-stu-id="a0f81-788">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="a0f81-789">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-789">**New behavior**</span></span>

<span data-ttu-id="a0f81-790">Ab EF Core 3.0 ist es ein DevelopmentDependency-Paket.</span><span class="sxs-lookup"><span data-stu-id="a0f81-790">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="a0f81-791">Das bedeutet, dass die Abhängigkeit nicht transitiv auf andere Projekte übertragen wird und Sie nicht mehr standardmäßig auf die Assembly verweisen können.</span><span class="sxs-lookup"><span data-stu-id="a0f81-791">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="a0f81-792">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-792">**Why**</span></span>

<span data-ttu-id="a0f81-793">Dieses Paket ist nur für die Verwendung zur Entwurfszeit konzipiert.</span><span class="sxs-lookup"><span data-stu-id="a0f81-793">This package is only intended to be used at design time.</span></span> <span data-ttu-id="a0f81-794">Bereitgestellte Anwendungen sollten nicht darauf verweisen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-794">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="a0f81-795">Die Kennzeichnung des Pakets als DevelopmentDependency unterstreicht diese Empfehlung.</span><span class="sxs-lookup"><span data-stu-id="a0f81-795">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="a0f81-796">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-796">**Mitigations**</span></span>

<span data-ttu-id="a0f81-797">Wenn Sie auf dieses Paket verweisen müssen, um das Verhalten von EF Core zur Entwurfszeit zu überschreiben, können Sie die Metadaten des PackageReference-Elements in Ihrem Projekt aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="a0f81-797">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="a0f81-798">Wenn transitiv über via Microsoft.EntityFrameworkCore.Tools auf das Paket verwiesen wird, müssen Sie dem Paket eine explizite PackageReference hinzufügen, um seine Metadaten zu ändern.</span><span class="sxs-lookup"><span data-stu-id="a0f81-798">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="a0f81-799">SQLitePCL.raw zu Version 2.0.0 aktualisiert</span><span class="sxs-lookup"><span data-stu-id="a0f81-799">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="a0f81-800">Issue #14824</span><span class="sxs-lookup"><span data-stu-id="a0f81-800">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="a0f81-801">Diese Änderung wird in Vorschauversion 7 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-801">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="a0f81-802">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-802">**Old behavior**</span></span>

<span data-ttu-id="a0f81-803">Microsoft.EntityFrameworkCore.Sqlite war früher von Version 1.1.12 von SQLitePCL.raw abhängig.</span><span class="sxs-lookup"><span data-stu-id="a0f81-803">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="a0f81-804">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-804">**New behavior**</span></span>

<span data-ttu-id="a0f81-805">Wir haben unser Paket aktualisiert, so dass es jetzt von Version 2.0.0 abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="a0f81-805">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="a0f81-806">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-806">**Why**</span></span>

<span data-ttu-id="a0f81-807">Version 2.0.0 von SQLitePCL.raw zielt auf .NET Standard 2.0 ab.</span><span class="sxs-lookup"><span data-stu-id="a0f81-807">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="a0f81-808">Zuvor war .NET Standard 1.1 das Ziel, hierfür war für ein umfassender Funktionsabschluss von transitiven Paketen erforderlich.</span><span class="sxs-lookup"><span data-stu-id="a0f81-808">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="a0f81-809">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-809">**Mitigations**</span></span>

<span data-ttu-id="a0f81-810">SQLitePCL.raw, Version 2.0.0, enthält einige Breaking Changes.</span><span class="sxs-lookup"><span data-stu-id="a0f81-810">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="a0f81-811">Weitere Informationen finden Sie in den [Versionshinweisen](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md).</span><span class="sxs-lookup"><span data-stu-id="a0f81-811">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="a0f81-812">NetTopologySuite auf Version 2.0.0 aktualisiert</span><span class="sxs-lookup"><span data-stu-id="a0f81-812">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="a0f81-813">Issue #14825</span><span class="sxs-lookup"><span data-stu-id="a0f81-813">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="a0f81-814">Diese Änderung wird in Vorschauversion 7 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-814">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="a0f81-815">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-815">**Old behavior**</span></span>

<span data-ttu-id="a0f81-816">Die Pakete mit räumlichen Daten waren bisher von Version 1.15.1 der NetTopologySuite abhängig.</span><span class="sxs-lookup"><span data-stu-id="a0f81-816">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="a0f81-817">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-817">**New behavior**</span></span>

<span data-ttu-id="a0f81-818">Wir haben unser Paket aktualisiert, so dass es jetzt von Version 2.0.0 abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="a0f81-818">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="a0f81-819">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-819">**Why**</span></span>

<span data-ttu-id="a0f81-820">Version 2.0.0 der NetTopologySuite behebt verschiedene Verwendungsprobleme, die von EF Core-Benutzern gemeldet wurden.</span><span class="sxs-lookup"><span data-stu-id="a0f81-820">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="a0f81-821">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-821">**Mitigations**</span></span>

<span data-ttu-id="a0f81-822">NetTopologySuite 2.0.0 umfasst einige Breaking Changes.</span><span class="sxs-lookup"><span data-stu-id="a0f81-822">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="a0f81-823">Weitere Informationen finden Sie in den [Versionshinweisen](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001).</span><span class="sxs-lookup"><span data-stu-id="a0f81-823">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="a0f81-824">Beziehungen mit mehreren mehrdeutigen Selbstverweisen müssen nun konfiguriert werden</span><span class="sxs-lookup"><span data-stu-id="a0f81-824">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="a0f81-825">Issue #13573</span><span class="sxs-lookup"><span data-stu-id="a0f81-825">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="a0f81-826">Diese Änderung wird in Vorschauversion 6 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="a0f81-826">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="a0f81-827">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-827">**Old behavior**</span></span>

<span data-ttu-id="a0f81-828">Ein Entitätstyp, der über unidirektionale Navigationseigenschaften mit mehreren Selbstverweisen und über übereinstimmende Fremdschlüssel verfügte, wurde bislang fälschlicherweise als einfache Beziehung konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="a0f81-828">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="a0f81-829">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a0f81-829">For example:</span></span>

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

<span data-ttu-id="a0f81-830">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="a0f81-830">**New behavior**</span></span>

<span data-ttu-id="a0f81-831">Dieses Szenario wird nun beim Erstellen von Modellen erkannt. Außerdem wird eine Ausnahme ausgelöst, in der darauf hingewiesen wird, dass das Modell mehrdeutig ist.</span><span class="sxs-lookup"><span data-stu-id="a0f81-831">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="a0f81-832">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="a0f81-832">**Why**</span></span>

<span data-ttu-id="a0f81-833">Das resultierende Modell war mehrdeutig und führte in diesem Fall üblicherweise zu falschen Ergebnissen.</span><span class="sxs-lookup"><span data-stu-id="a0f81-833">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="a0f81-834">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="a0f81-834">**Mitigations**</span></span>

<span data-ttu-id="a0f81-835">Konfigurieren Sie die Beziehung vollständig.</span><span class="sxs-lookup"><span data-stu-id="a0f81-835">Use full configuration of the relationship.</span></span> <span data-ttu-id="a0f81-836">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a0f81-836">For example:</span></span>

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
