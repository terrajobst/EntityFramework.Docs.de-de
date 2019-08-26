---
title: Breaking Changes in EF Core 3.0
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 884cc6611b986fb213d99d3d2fc69d7bebe34aa2
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565303"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="78671-102">Breaking Changes in EF Core 3.0 (aktuell in der Vorschauversion)</span><span class="sxs-lookup"><span data-stu-id="78671-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="78671-103">Bitte beachten Sie, dass die Featuregruppen und Zeitpläne für künftige Releases jederzeit geändert werden können. Obwohl diese Seite bestmöglich aktualisiert wird, entspricht sie nicht immer den neuesten Plänen.</span><span class="sxs-lookup"><span data-stu-id="78671-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="78671-104">Die folgenden API-Änderungen und Behavior Changes können dazu führen, dass Anwendungen, die für EF Core 2.2.x entwickelt wurden, beim Upgrade auf 3.0.0 nicht mehr funktionieren.</span><span class="sxs-lookup"><span data-stu-id="78671-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="78671-105">Änderungen, die sich voraussichtlich nur auf Datenbankanbieter auswirken, sind unter [Änderungen mit Auswirkungen auf den Anbieter](../../providers/provider-log.md) dokumentiert.</span><span class="sxs-lookup"><span data-stu-id="78671-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="78671-106">Werden Breaking Changes zwischen zwei Vorschauversionen von EF Core 3.0 in neuen Features eingeführt, werden diese hier nicht dokumentiert.</span><span class="sxs-lookup"><span data-stu-id="78671-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="summary"></a><span data-ttu-id="78671-107">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="78671-107">Summary</span></span>

| <span data-ttu-id="78671-108">**Wichtige Änderung**</span><span class="sxs-lookup"><span data-stu-id="78671-108">**Breaking change**</span></span>                                                                                               | <span data-ttu-id="78671-109">**Auswirkung**</span><span class="sxs-lookup"><span data-stu-id="78671-109">**Impact**</span></span> |
|:------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="78671-110">LINQ-Abfragen werden nicht mehr auf dem Client ausgewertet</span><span class="sxs-lookup"><span data-stu-id="78671-110">LINQ queries are no longer evaluated on the client</span></span>](#linq-queries-are-no-longer-evaluated-on-the-client)         | <span data-ttu-id="78671-111">Hoch</span><span class="sxs-lookup"><span data-stu-id="78671-111">High</span></span>       |
| [<span data-ttu-id="78671-112">EF Core 3.0 zielt auf .NET Standard 2.1 und nicht auf .NET Standard 2.0 ab</span><span class="sxs-lookup"><span data-stu-id="78671-112">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>](#netstandard21) | <span data-ttu-id="78671-113">Hoch</span><span class="sxs-lookup"><span data-stu-id="78671-113">High</span></span>      |
| [<span data-ttu-id="78671-114">Das EF Core-Befehlszeilentool (dotnet ef) ist nicht mehr Bestandteil des .NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="78671-114">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>](#dotnet-ef) | <span data-ttu-id="78671-115">Hoch</span><span class="sxs-lookup"><span data-stu-id="78671-115">High</span></span>      |
| [<span data-ttu-id="78671-116">FromSql, ExecuteSql und ExecuteSqlAsync wurden umbenannt</span><span class="sxs-lookup"><span data-stu-id="78671-116">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>](#fromsql) | <span data-ttu-id="78671-117">Hoch</span><span class="sxs-lookup"><span data-stu-id="78671-117">High</span></span>      |
| [<span data-ttu-id="78671-118">Abfragetypen werden mit Entitätstypen zusammengeführt</span><span class="sxs-lookup"><span data-stu-id="78671-118">Query types are consolidated with entity types</span></span>](#qt) | <span data-ttu-id="78671-119">Hoch</span><span class="sxs-lookup"><span data-stu-id="78671-119">High</span></span>      |
| [<span data-ttu-id="78671-120">Entity Framework Core ist nicht mehr Bestandteil des gemeinsam verwendeten ASP.NET Core-Frameworks</span><span class="sxs-lookup"><span data-stu-id="78671-120">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>](#no-longer) | <span data-ttu-id="78671-121">Mittel</span><span class="sxs-lookup"><span data-stu-id="78671-121">Medium</span></span>      |
| [<span data-ttu-id="78671-122">Kaskadierende DELETE-Anweisungen werden standardmäßig sofort ausgeführt</span><span class="sxs-lookup"><span data-stu-id="78671-122">Cascade deletions now happen immediately by default</span></span>](#cascade) | <span data-ttu-id="78671-123">Mittel</span><span class="sxs-lookup"><span data-stu-id="78671-123">Medium</span></span>      |
| [<span data-ttu-id="78671-124">DeleteBehavior.Restrict verfügt über eine übersichtlichere Semantik</span><span class="sxs-lookup"><span data-stu-id="78671-124">DeleteBehavior.Restrict has cleaner semantics</span></span>](#deletebehavior) | <span data-ttu-id="78671-125">Mittel</span><span class="sxs-lookup"><span data-stu-id="78671-125">Medium</span></span>      |
| [<span data-ttu-id="78671-126">Die Konfigurations-API für Beziehungen abhängiger (owned) Typen wurde geändert</span><span class="sxs-lookup"><span data-stu-id="78671-126">Configuration API for owned type relationships has changed</span></span>](#config) | <span data-ttu-id="78671-127">Mittel</span><span class="sxs-lookup"><span data-stu-id="78671-127">Medium</span></span>      |
| [<span data-ttu-id="78671-128">Für jede Eigenschaft wird separat ein ganzzahliger speicherinterner Schlüssel generiert</span><span class="sxs-lookup"><span data-stu-id="78671-128">Each property uses independent in-memory integer key generation</span></span>](#each) | <span data-ttu-id="78671-129">Mittel</span><span class="sxs-lookup"><span data-stu-id="78671-129">Medium</span></span>      |
| [<span data-ttu-id="78671-130">Abfragen ohne Nachverfolgung führen keine Identitätsauflösung mehr durch</span><span class="sxs-lookup"><span data-stu-id="78671-130">No-tracking queries no longer perform identity resolution</span></span>](#notrackingresolution) | <span data-ttu-id="78671-131">Mittel</span><span class="sxs-lookup"><span data-stu-id="78671-131">Medium</span></span>      |
| [<span data-ttu-id="78671-132">Metadaten-API-Änderungen</span><span class="sxs-lookup"><span data-stu-id="78671-132">Metadata API changes</span></span>](#metadata-api-changes) | <span data-ttu-id="78671-133">Mittel</span><span class="sxs-lookup"><span data-stu-id="78671-133">Medium</span></span>      |
| [<span data-ttu-id="78671-134">Anbieterspezifische Metadaten-API-Änderungen</span><span class="sxs-lookup"><span data-stu-id="78671-134">Provider-specific Metadata API changes</span></span>](#provider) | <span data-ttu-id="78671-135">Mittel</span><span class="sxs-lookup"><span data-stu-id="78671-135">Medium</span></span>      |
| [<span data-ttu-id="78671-136">UseRowNumberForPaging wurde entfernt</span><span class="sxs-lookup"><span data-stu-id="78671-136">UseRowNumberForPaging has been removed</span></span>](#urn) | <span data-ttu-id="78671-137">Mittel</span><span class="sxs-lookup"><span data-stu-id="78671-137">Medium</span></span>      |
| [<span data-ttu-id="78671-138">FromSql-Methoden können nur für die Stammelemente der Abfrage angegeben werden</span><span class="sxs-lookup"><span data-stu-id="78671-138">FromSql methods can only be specified on query roots</span></span>](#fromsql) | <span data-ttu-id="78671-139">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-139">Low</span></span>      |
| [<span data-ttu-id="78671-140">~~Die Abfrageausführung wird auf Debugebene protokolliert~~ – zurückgesetzt</span><span class="sxs-lookup"><span data-stu-id="78671-140">~~Query execution is logged at Debug level~~ Reverted</span></span>](#qe) | <span data-ttu-id="78671-141">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-141">Low</span></span>      |
| [<span data-ttu-id="78671-142">Temporäre Schlüsselwerte werden nicht mehr Entitätsinstanzen zugewiesen</span><span class="sxs-lookup"><span data-stu-id="78671-142">Temporary key values are no longer set onto entity instances</span></span>](#tkv) | <span data-ttu-id="78671-143">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-143">Low</span></span>      |
| [<span data-ttu-id="78671-144">DetectChanges berücksichtigt vom Speicher generierte Schlüsselwerte</span><span class="sxs-lookup"><span data-stu-id="78671-144">DetectChanges honors store-generated key values</span></span>](#dc) | <span data-ttu-id="78671-145">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-145">Low</span></span>      |
| [<span data-ttu-id="78671-146">Abhängige Entitäten, die die Tabelle gemeinsam mit dem Prinzipal verwenden, sind jetzt optional</span><span class="sxs-lookup"><span data-stu-id="78671-146">Dependent entities sharing the table with the principal are now optional</span></span>](#de) | <span data-ttu-id="78671-147">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-147">Low</span></span>      |
| [<span data-ttu-id="78671-148">Alle Entitäten, die eine Tabelle mit einer Spalte für das Parallelitätstoken gemeinsam verwenden, müssen diese einer Eigenschaft zuordnen</span><span class="sxs-lookup"><span data-stu-id="78671-148">All entities sharing a table with a concurrency token column have to map it to a property</span></span>](#aes) | <span data-ttu-id="78671-149">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-149">Low</span></span>      |
| [<span data-ttu-id="78671-150">Geerbte Eigenschaften von nicht zugeordneten Typen sind nun einer einzelnen Spalte für alle abgeleiteten Typen zugeordnet</span><span class="sxs-lookup"><span data-stu-id="78671-150">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>](#ip) | <span data-ttu-id="78671-151">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-151">Low</span></span>      |
| [<span data-ttu-id="78671-152">Die Konvention zur Fremdschlüsseleigenschaft entspricht nicht mehr dem Namen der Prinzipaleigenschaft</span><span class="sxs-lookup"><span data-stu-id="78671-152">The foreign key property convention no longer matches same name as the principal property</span></span>](#fkp) | <span data-ttu-id="78671-153">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-153">Low</span></span>      |
| [<span data-ttu-id="78671-154">Die Datenbankverbindung wird jetzt getrennt, wenn sie nicht mehr verwendet wird, bevor TransactionScope abgeschlossen wurde</span><span class="sxs-lookup"><span data-stu-id="78671-154">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>](#dbc) | <span data-ttu-id="78671-155">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-155">Low</span></span>      |
| [<span data-ttu-id="78671-156">Unterstützungsfelder werden standardmäßig verwendet</span><span class="sxs-lookup"><span data-stu-id="78671-156">Backing fields are used by default</span></span>](#backing-fields-are-used-by-default) | <span data-ttu-id="78671-157">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-157">Low</span></span>      |
| [<span data-ttu-id="78671-158">Eine Ausnahme wird ausgelöst, wenn mehrere kompatible Unterstützungsfelder gefunden werden</span><span class="sxs-lookup"><span data-stu-id="78671-158">Throw if multiple compatible backing fields are found</span></span>](#throw-if-multiple-compatible-backing-fields-are-found) | <span data-ttu-id="78671-159">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-159">Low</span></span>      |
| [<span data-ttu-id="78671-160">„Nur-Feld“-Eigenschaftennamen sollten dem Feldnamen entsprechen</span><span class="sxs-lookup"><span data-stu-id="78671-160">Field-only property names should match the field name</span></span>](#field-only-property-names-should-match-the-field-name) | <span data-ttu-id="78671-161">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-161">Low</span></span>      |
| [<span data-ttu-id="78671-162">AddLogging und AddMemoryCache werden nicht mehr von AddDbContext/AddDbContextPool aufgerufen</span><span class="sxs-lookup"><span data-stu-id="78671-162">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>](#adddbc) | <span data-ttu-id="78671-163">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-163">Low</span></span>      |
| [<span data-ttu-id="78671-164">DbContext.Entry erkennt nun mit DetectChanges lokale Änderungen</span><span class="sxs-lookup"><span data-stu-id="78671-164">DbContext.Entry now performs a local DetectChanges</span></span>](#dbe) | <span data-ttu-id="78671-165">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-165">Low</span></span>      |
| [<span data-ttu-id="78671-166">Schlüssel aus Zeichenfolgen und Bytearrays werden standardmäßig nicht vom Client generiert</span><span class="sxs-lookup"><span data-stu-id="78671-166">String and byte array keys are not client-generated by default</span></span>](#string-and-byte-array-keys-are-not-client-generated-by-default) | <span data-ttu-id="78671-167">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-167">Low</span></span>      |
| [<span data-ttu-id="78671-168">ILoggerFactory ist nun ein bereichsbezogener Dienst</span><span class="sxs-lookup"><span data-stu-id="78671-168">ILoggerFactory is now a scoped service</span></span>](#ilf) | <span data-ttu-id="78671-169">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-169">Low</span></span>      |
| [<span data-ttu-id="78671-170">Für Proxys mit Lazy Loading (trägem Laden) gilt nicht mehr die Annahme, dass Navigationseigenschaften vollständig geladen sind</span><span class="sxs-lookup"><span data-stu-id="78671-170">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>](#lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded) | <span data-ttu-id="78671-171">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-171">Low</span></span>      |
| [<span data-ttu-id="78671-172">Die Erstellung zu vieler interner Dienstanbieter führt standardmäßig zu einem Fehler</span><span class="sxs-lookup"><span data-stu-id="78671-172">Excessive creation of internal service providers is now an error by default</span></span>](#excessive-creation-of-internal-service-providers-is-now-an-error-by-default) | <span data-ttu-id="78671-173">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-173">Low</span></span>      |
| [<span data-ttu-id="78671-174">Neues Verhalten für HasOne/HasMany bei Aufruf mit einer einzelnen Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="78671-174">New behavior for HasOne/HasMany called with a single string</span></span>](#nbh) | <span data-ttu-id="78671-175">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-175">Low</span></span>      |
| [<span data-ttu-id="78671-176">Der Rückgabetyp für mehrere asynchrone Methoden wurde von Task in ValueTask geändert</span><span class="sxs-lookup"><span data-stu-id="78671-176">The return type for several async methods has been changed from Task to ValueTask</span></span>](#rtnt) | <span data-ttu-id="78671-177">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-177">Low</span></span>      |
| [<span data-ttu-id="78671-178">Die Anmerkung Relational:TypeMapping heißt nun nur noch TypeMapping</span><span class="sxs-lookup"><span data-stu-id="78671-178">The Relational:TypeMapping annotation is now just TypeMapping</span></span>](#rtt) | <span data-ttu-id="78671-179">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-179">Low</span></span>      |
| [<span data-ttu-id="78671-180">ToTable löst für einen abgeleiteten Typ eine Ausnahme aus</span><span class="sxs-lookup"><span data-stu-id="78671-180">ToTable on a derived type throws an exception</span></span>](#totable-on-a-derived-type-throws-an-exception) | <span data-ttu-id="78671-181">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-181">Low</span></span>      |
| [<span data-ttu-id="78671-182">EF Core sendet keine PRAGMA-Anweisungen mehr, um Fremdschlüssel in SQLite zu erzwingen</span><span class="sxs-lookup"><span data-stu-id="78671-182">EF Core no longer sends pragma for SQLite FK enforcement</span></span>](#pragma) | <span data-ttu-id="78671-183">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-183">Low</span></span>      |
| [<span data-ttu-id="78671-184">SQLitePCLRaw.bundle_e_sqlite3 ist nun eine Abhängigkeit von Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="78671-184">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>](#sqlite3) | <span data-ttu-id="78671-185">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-185">Low</span></span>      |
| [<span data-ttu-id="78671-186">GUID-Werte werden jetzt als TEXT in SQLite gespeichert</span><span class="sxs-lookup"><span data-stu-id="78671-186">Guid values are now stored as TEXT on SQLite</span></span>](#guid) | <span data-ttu-id="78671-187">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-187">Low</span></span>      |
| [<span data-ttu-id="78671-188">Char-Werte werden jetzt als TEXT in SQLite gespeichert</span><span class="sxs-lookup"><span data-stu-id="78671-188">Char values are now stored as TEXT on SQLite</span></span>](#char) | <span data-ttu-id="78671-189">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-189">Low</span></span>      |
| [<span data-ttu-id="78671-190">Migrations-IDs werden jetzt mit dem Kalender der invarianten Kultur generiert</span><span class="sxs-lookup"><span data-stu-id="78671-190">Migration IDs are now generated using the invariant culture's calendar</span></span>](#migid) | <span data-ttu-id="78671-191">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-191">Low</span></span>      |
| [<span data-ttu-id="78671-192">Erweiterungsinformationen und -metadaten wurden aus IDbContextOptionsExtension entfernt</span><span class="sxs-lookup"><span data-stu-id="78671-192">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>](#xinfo) | <span data-ttu-id="78671-193">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-193">Low</span></span>      |
| [<span data-ttu-id="78671-194">LogQueryPossibleExceptionWithAggregateOperator wurde umbenannt</span><span class="sxs-lookup"><span data-stu-id="78671-194">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>](#lqpe) | <span data-ttu-id="78671-195">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-195">Low</span></span>      |
| [<span data-ttu-id="78671-196">Übersichtlichere API für Namen von Fremdschlüsselconstraints</span><span class="sxs-lookup"><span data-stu-id="78671-196">Clarify API for foreign key constraint names</span></span>](#clarify) | <span data-ttu-id="78671-197">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-197">Low</span></span>      |
| [<span data-ttu-id="78671-198">IRelationalDatabaseCreator.HasTables/HasTablesAsync ist jetzt öffentlich</span><span class="sxs-lookup"><span data-stu-id="78671-198">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>](#irdc2) | <span data-ttu-id="78671-199">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-199">Low</span></span>      |
| [<span data-ttu-id="78671-200">Microsoft.EntityFrameworkCore.Design ist jetzt ein DevelopmentDependency-Paket</span><span class="sxs-lookup"><span data-stu-id="78671-200">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>](#dip) | <span data-ttu-id="78671-201">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-201">Low</span></span>      |
| [<span data-ttu-id="78671-202">SQLitePCL.raw wurde auf Version 2.0.0 aktualisiert</span><span class="sxs-lookup"><span data-stu-id="78671-202">SQLitePCL.raw updated to version 2.0.0</span></span>](#SQLitePCL) | <span data-ttu-id="78671-203">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-203">Low</span></span>      |
| [<span data-ttu-id="78671-204">NetTopologySuite wurde auf Version 2.0.0 aktualisiert</span><span class="sxs-lookup"><span data-stu-id="78671-204">NetTopologySuite updated to version 2.0.0</span></span>](#NetTopologySuite) | <span data-ttu-id="78671-205">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-205">Low</span></span>      |
| [<span data-ttu-id="78671-206">Beziehungen mit mehreren mehrdeutigen Selbstverweisen müssen nun konfiguriert werden</span><span class="sxs-lookup"><span data-stu-id="78671-206">Multiple ambiguous self-referencing relationships must be configured</span></span>](#mersa) | <span data-ttu-id="78671-207">Niedrig</span><span class="sxs-lookup"><span data-stu-id="78671-207">Low</span></span>      |

### <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="78671-208">LINQ-Abfragen werden nicht mehr auf dem Client ausgewertet</span><span class="sxs-lookup"><span data-stu-id="78671-208">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="78671-209">[Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Siehe auch Issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="78671-209">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="78671-210">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-210">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="78671-211">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-211">**Old behavior**</span></span>

<span data-ttu-id="78671-212">Vor Version 3.0 wurden in EF Core automatisch Ausdrücke auf dem Client ausgewertet, wenn diese Teil einer Abfrage waren und nicht in SQL oder in einen Parameter konvertiert werden konnten.</span><span class="sxs-lookup"><span data-stu-id="78671-212">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="78671-213">Wenn potenziell aufwendige Ausdrücke auf dem Client ausgewertet werden sollten, wurde standardmäßig nur eine Warnung ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="78671-213">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="78671-214">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-214">**New behavior**</span></span>

<span data-ttu-id="78671-215">Ab Version 3.0 können in EF Core nur Ausdrücke der obersten Projektion (dem letzten `Select()`-Aufruf in der Abfrage) auf dem Client ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="78671-215">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="78671-216">Wenn Ausdrücke in einem anderen Teil der Abfrage nicht in SQL oder in einen Parameter konvertiert werden können, wird eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="78671-216">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="78671-217">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-217">**Why**</span></span>

<span data-ttu-id="78671-218">Die automatische Auswertung von Abfragen auf Clients hat zur Folge, dass viele Abfragen auch dann ausgeführt werden, wenn wichtige Teile nicht übersetzt werden können.</span><span class="sxs-lookup"><span data-stu-id="78671-218">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="78671-219">Dieses Verhalten kann zu unerwartetem und potenziell schädlichem Verhalten führen, das möglicherweise erst in der Produktionsphase erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="78671-219">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="78671-220">Eine nicht übersetzbare Bedingung in einem `Where()`-Aufruf kann sich beispielsweise so auswirken, dass alle Zeilen einer Tabelle vom Datenbankserver übertragen werden und der Filter auf dem Client angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="78671-220">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="78671-221">Wenn die Tabelle in der Entwicklungsphase nur wenige Zeilen enthält, ist es gut möglich, dass diese Situation unerkannt bleibt. Wenn die Anwendung jedoch in die Produktionsumgebung überführt wird, enthält die Tabelle möglicherweise Millionen von Zeilen, was zu schwerwiegenden Konsequenzen führen kann.</span><span class="sxs-lookup"><span data-stu-id="78671-221">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="78671-222">Wie die Erfahrung in der Entwicklungsphase gezeigt hat, können auch Warnungen, die bei Auswertungen auf dem Client ausgelöst werden, dieses Problem nicht lösen, da sie sich leicht ignorieren lassen.</span><span class="sxs-lookup"><span data-stu-id="78671-222">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="78671-223">Die automatische Auswertung auf Clients kann darüber hinaus zu Problemen führen, bei denen die Verbesserung der Abfrageübersetzung für bestimmte Ausdrücke zu unbeabsichtigten Breaking Changes zwischen Releases führt.</span><span class="sxs-lookup"><span data-stu-id="78671-223">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="78671-224">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-224">**Mitigations**</span></span>

<span data-ttu-id="78671-225">Wenn sich eine Abfrage nicht vollständig übersetzen lässt, habe Sie zwei Möglichkeiten: Schreiben Sie sie entweder um, oder setzen Sie alternativ `AsEnumerable()`, `ToList()` oder ähnliche Methoden ein, um Daten wieder zurück an den Client zu übertragen, wo sie mit LINQ to Objects weiterverarbeitet werden können.</span><span class="sxs-lookup"><span data-stu-id="78671-225">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

<a name="netstandard21"></a>
### <a name="ef-core-30-targets-net-standard-21-rather-than-net-standard-20"></a><span data-ttu-id="78671-226">EF Core 3.0 zielt auf .NET Standard 2.1 und nicht auf .NET Standard 2.0 ab</span><span class="sxs-lookup"><span data-stu-id="78671-226">EF Core 3.0 targets .NET Standard 2.1 rather than .NET Standard 2.0</span></span>

[<span data-ttu-id="78671-227">Issue #15498</span><span class="sxs-lookup"><span data-stu-id="78671-227">Tracking Issue #15498</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15498)

<span data-ttu-id="78671-228">Diese Änderung wird in Vorschauversion 7 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-228">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="78671-229">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-229">**Old behavior**</span></span>

<span data-ttu-id="78671-230">Vor 3.0 zielte EF Core auf .NET Standard 2.0 ab und wurde auf allen Plattformen ausgeführt, die diesen Standard unterstützen, einschließlich .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="78671-230">Before 3.0, EF Core targeted .NET Standard 2.0 and would run on all platforms that support that standard, including .NET Framework.</span></span>

<span data-ttu-id="78671-231">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-231">**New behavior**</span></span>

<span data-ttu-id="78671-232">Ab 3.0 zielt EF Core auf .NET Standard 2.1 ab und wird auf allen Plattformen ausgeführt, die diesen Standard unterstützen.</span><span class="sxs-lookup"><span data-stu-id="78671-232">Starting with 3.0, EF Core targets .NET Standard 2.1 and will run on all platforms that support this standard.</span></span> <span data-ttu-id="78671-233">Dies schließt .NET Framework nicht ein.</span><span class="sxs-lookup"><span data-stu-id="78671-233">This does not include .NET Framework.</span></span>

<span data-ttu-id="78671-234">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-234">**Why**</span></span>

<span data-ttu-id="78671-235">Dies ist ein Teil der strategischen Entscheidung bezüglich .NET-Technologien, um den Fokus auf .NET Core und andere moderne .NET-Plattformen (z. B. Xamarin) zu legen.</span><span class="sxs-lookup"><span data-stu-id="78671-235">This is part of a strategic decision across .NET technologies to focus energy on .NET Core and other modern .NET platforms, such as Xamarin.</span></span>

<span data-ttu-id="78671-236">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-236">**Mitigations**</span></span>

<span data-ttu-id="78671-237">Erwägen Sie, zu einer moderneren .NET-Plattform zu wechseln.</span><span class="sxs-lookup"><span data-stu-id="78671-237">Consider moving to a modern .NET platform.</span></span> <span data-ttu-id="78671-238">Falls dies nicht möglich ist, verwenden Sie weiterhin EF Core 2.1 oder EF Core 2.2, die beide .NET Framework unterstützen.</span><span class="sxs-lookup"><span data-stu-id="78671-238">If this is not possible, then continue to use EF Core 2.1 or EF Core 2.2, both of which support .NET Framework.</span></span>

<a name="no-longer"></a>
### <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="78671-239">Entity Framework Core ist nicht mehr Teil des gemeinsam verwendeten ASP.NET Core-Frameworks</span><span class="sxs-lookup"><span data-stu-id="78671-239">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="78671-240">Issueankündigungen #325</span><span class="sxs-lookup"><span data-stu-id="78671-240">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="78671-241">Diese Änderung wird in Vorschauversion 1 von ASP.NET Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-241">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="78671-242">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-242">**Old behavior**</span></span>

<span data-ttu-id="78671-243">Vor Version 3.0 von ASP.NET Core wurden EF Core und einige der zugehörigen Datenanbieter wie SQL Server eingeschlossen, wenn `Microsoft.AspNetCore.App` oder `Microsoft.AspNetCore.All` ein Paketverweis hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="78671-243">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="78671-244">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-244">**New behavior**</span></span>

<span data-ttu-id="78671-245">Ab Version 3.0 enthält das gemeinsam verwendete ASP.NET Core-Framework weder EF Core noch zugehörige Datenanbieter.</span><span class="sxs-lookup"><span data-stu-id="78671-245">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="78671-246">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-246">**Why**</span></span>

<span data-ttu-id="78671-247">Vor dieser Änderung konnte EF Core auf unterschiedliche Arten bezogen werden. Diese richteten sich danach, ob ASP.NET Core und SQL Server oder ein anderes Zielframework für die Anwendung vorgesehen war.</span><span class="sxs-lookup"><span data-stu-id="78671-247">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="78671-248">Bei einem Upgrade von ASP.NET Core wurde zudem ein Upgrade von EF Core und des SQL Server-Anbieters erzwungen, was nicht in allen Fällen gewünscht war.</span><span class="sxs-lookup"><span data-stu-id="78671-248">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="78671-249">Durch die eingeführte Änderung wird EF Core unter allen Anbietern, unterstützten .NET-Implementierungen und Anwendungstypen auf dieselbe Weise bezogen.</span><span class="sxs-lookup"><span data-stu-id="78671-249">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="78671-250">Entwickler können nun außerdem genau festlegen, wann für EF Core und zugehörige Datenanbieter ein Upgrade durchgeführt werden soll.</span><span class="sxs-lookup"><span data-stu-id="78671-250">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="78671-251">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-251">**Mitigations**</span></span>

<span data-ttu-id="78671-252">Wenn Sie EF Core in einer Anwendung unter ASP.NET Core 3.0 oder in einer anderen unterstützten Anwendung verwenden möchten, müssen Sie dem EF Core-Datenbankanbieter, der für die Anwendung genutzt werden soll, explizit einen Paketverweis hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="78671-252">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

<a name="dotnet-ef"></a>
### <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="78671-253">Das EF Core-Befehlszeilentool (dotnet ef) ist nicht länger Bestandteil des .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="78671-253">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="78671-254">Issue #14016</span><span class="sxs-lookup"><span data-stu-id="78671-254">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="78671-255">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 und der zugehörigen Version des .NET Core SDK eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-255">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="78671-256">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-256">**Old behavior**</span></span>

<span data-ttu-id="78671-257">Vor Version 3.0 war das `dotnet ef`-Tool im .NET Core SDK enthalten und konnte von der Befehlszeile aus ohne zusätzliche Schritte in einem beliebigen Projekt verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="78671-257">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="78671-258">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-258">**New behavior**</span></span>

<span data-ttu-id="78671-259">Ab Version 3.0 ist das `dotnet ef`-Tool nicht mehr im .NET SDK enthalten und muss deshalb vor der Verwendung explizit als lokales oder globales Tool installiert werden.</span><span class="sxs-lookup"><span data-stu-id="78671-259">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="78671-260">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-260">**Why**</span></span>

<span data-ttu-id="78671-261">Durch diese Änderung kann `dotnet ef` als reguläres .NET-CLI-Tool für NuGet verteilt und aktualisiert werden und entspricht damit dem üblichen Verfahren, dass EF Core 3.0 ebenfalls immer als NuGet-Paket verteilt wird.</span><span class="sxs-lookup"><span data-stu-id="78671-261">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="78671-262">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-262">**Mitigations**</span></span>

<span data-ttu-id="78671-263">Um Migrationen zu verwalten oder ein Gerüst für `DbContext` zu erstellen, installieren Sie `dotnet-ef` als globales Tool:</span><span class="sxs-lookup"><span data-stu-id="78671-263">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

<span data-ttu-id="78671-264">Eine Verwendung als lokales Tool ist ebenfalls möglich, wenn Sie die Abhängigkeiten eines Projekts wiederherstellen, das das Tool mithilfe einer [Toolmanifestdatei](https://github.com/dotnet/cli/issues/10288) als Toolabhängigkeit deklariert.</span><span class="sxs-lookup"><span data-stu-id="78671-264">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

<a name="fromsql"></a>
### <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="78671-265">FromSql, ExecuteSql und ExecuteSqlAsync wurden umbenannt</span><span class="sxs-lookup"><span data-stu-id="78671-265">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="78671-266">Issue #10996</span><span class="sxs-lookup"><span data-stu-id="78671-266">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="78671-267">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-267">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="78671-268">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-268">**Old behavior**</span></span>

<span data-ttu-id="78671-269">Vor Version 3.0 wurden in EF Core diese Methodennamen überladen, um entweder mit einer normalen Zeichenfolge oder einer Zeichenfolge, die in SQL und Parameter interpoliert werden sollte, zu arbeiten.</span><span class="sxs-lookup"><span data-stu-id="78671-269">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="78671-270">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-270">**New behavior**</span></span>

<span data-ttu-id="78671-271">Ab Version 3.0 verwenden Sie in EF Core `FromSqlRaw`, `ExecuteSqlRaw` und `ExecuteSqlRawAsync`, um eine parametrisierte Abfrage zu erstellen, bei der die Parameter einzeln aus der Abfragezeichenfolge übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="78671-271">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="78671-272">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="78671-272">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="78671-273">Verwenden Sie `FromSqlInterpolated`, `ExecuteSqlInterpolated` und `ExecuteSqlInterpolatedAsync`, um eine parametrisierte Abfrage zu erstellen, bei der die Parameter als Teil einer interpolierten Abfragezeichenfolge übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="78671-273">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="78671-274">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="78671-274">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="78671-275">Beachten Sie, dass die beiden oben genannten Abfragen dieselbe parametrisierte SQL mit denselben SQL-Parametern erzeugen.</span><span class="sxs-lookup"><span data-stu-id="78671-275">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="78671-276">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-276">**Why**</span></span>

<span data-ttu-id="78671-277">Bei Methodenüberladungen wie dieser wird sehr leicht versehentlich die Methode für unformatierte Zeichenfolgen aufgerufen, wenn eigentlich beabsichtigt war, die Methode für interpolierte Zeichenfolgen aufzurufen (und umgekehrt).</span><span class="sxs-lookup"><span data-stu-id="78671-277">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="78671-278">Dadurch werden Abfragen möglicherweise nicht parametrisiert, obwohl dies der Fall sein sollte.</span><span class="sxs-lookup"><span data-stu-id="78671-278">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="78671-279">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-279">**Mitigations**</span></span>

<span data-ttu-id="78671-280">Verwenden Sie ab sofort die neuen Methodennamen.</span><span class="sxs-lookup"><span data-stu-id="78671-280">Switch to use the new method names.</span></span>

<a name="fromsql"></a>

### <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="78671-281">FromSql-Methoden können nur für die Stammelemente der Abfrage angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="78671-281">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="78671-282">Issue #15704</span><span class="sxs-lookup"><span data-stu-id="78671-282">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="78671-283">Diese Änderung wird in Vorschauversion 6 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-283">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="78671-284">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-284">**Old behavior**</span></span>

<span data-ttu-id="78671-285">Vor EF Core 3.0 konnte die `FromSql`-Methode an einer beliebigen Stelle in der Abfrage angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="78671-285">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="78671-286">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-286">**New behavior**</span></span>

<span data-ttu-id="78671-287">Ab EF Core 3.0 können die neuen Methoden `FromSqlRaw` und `FromSqlInterpolated` (durch die `FromSql` ersetzt wird) nur für Stammelemente von Abfragen angegeben werden, d.h. direkt für das `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="78671-287">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="78671-288">Der Versuch, sie an anderer Stelle anzugeben, führt zu einem Kompilierungsfehler.</span><span class="sxs-lookup"><span data-stu-id="78671-288">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="78671-289">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-289">**Why**</span></span>

<span data-ttu-id="78671-290">Die Angabe von `FromSql` an einer anderen Stelle als für ein `DbSet` erbrachte keine zusätzliche Bedeutung und keinen Mehrwert, sondern konnte in bestimmten Szenarien zu Mehrdeutigkeiten führen.</span><span class="sxs-lookup"><span data-stu-id="78671-290">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="78671-291">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-291">**Mitigations**</span></span>

<span data-ttu-id="78671-292">`FromSql` Aufrufe sollten so verschoben, dass sie direkt für das zugehörige `DbSet` gelten.</span><span class="sxs-lookup"><span data-stu-id="78671-292">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

<a name="notrackingresolution"></a>
### <a name="no-tracking-queries-no-longer-perform-identity-resolution"></a><span data-ttu-id="78671-293">Abfragen ohne Nachverfolgung führen keine Identitätsauflösung mehr durch</span><span class="sxs-lookup"><span data-stu-id="78671-293">No-tracking queries no longer perform identity resolution</span></span>

[<span data-ttu-id="78671-294">Issue #13518</span><span class="sxs-lookup"><span data-stu-id="78671-294">Tracking Issue #13518</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13518)

<span data-ttu-id="78671-295">Diese Änderung wird in Vorschauversion 6 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-295">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="78671-296">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-296">**Old behavior**</span></span>

<span data-ttu-id="78671-297">Vor EF Core 3.0 wurde dieselbe Entitätsinstanz für jedes Vorkommen einer Entität mit einem bestimmten Typ und einer bestimmten ID verwendet.</span><span class="sxs-lookup"><span data-stu-id="78671-297">Before EF Core 3.0, the same entity instance would be used for every occurrence of an entity with a given type and ID.</span></span> <span data-ttu-id="78671-298">Dies entspricht dem Verhalten von Abfragen zu Nachverfolgungen.</span><span class="sxs-lookup"><span data-stu-id="78671-298">This matches the behavior of tracking queries.</span></span> <span data-ttu-id="78671-299">Die folgende Abfrage:</span><span class="sxs-lookup"><span data-stu-id="78671-299">For example, this query:</span></span>

```C#
var results = context.Products.Include(e => e.Category).AsNoTracking().ToList();
```
<span data-ttu-id="78671-300">gibt dieselbe `Category`-Instanz für jedes `Product` zurück, das der bestimmten Kategorie zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="78671-300">would return the same `Category` instance for each `Product` that is associated with the given category.</span></span>

<span data-ttu-id="78671-301">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-301">**New behavior**</span></span>

<span data-ttu-id="78671-302">Ab EF Core 3.0 werden unterschiedliche Entitätsinstanzen erstellt, wenn eine Entität mit einem bestimmten Typ und einer bestimmten ID an verschiedenen Stellen im zurückgegebenen Diagramm gefunden wird.</span><span class="sxs-lookup"><span data-stu-id="78671-302">Starting with EF Core 3.0, different entity instances will be created when an entity with a given type and ID is encountered at different places in the returned graph.</span></span> <span data-ttu-id="78671-303">Die Abfrage oben gibt beispielsweise nun eine neue `Category`-Instanz für jedes `Product` zurück, auch wenn zwei Produkte derselben Kategorie zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="78671-303">For example, the query above will now return a new `Category` instance for each `Product` even when two products are associated with the same category.</span></span>

<span data-ttu-id="78671-304">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-304">**Why**</span></span>

<span data-ttu-id="78671-305">Die Identitätsauflösung (d. h. Feststellen, dass eine Entität über denselben Typ und die dieselbe ID wie die zuvor aufgetretene Entität verfügt) fügt zusätzlichen Aufwand für Leistung und Arbeitsspeicher hinzu.</span><span class="sxs-lookup"><span data-stu-id="78671-305">Identity resolution (that is, determining that an entity has the same type and ID as a previously encountered entity) adds additional performance and memory overhead.</span></span> <span data-ttu-id="78671-306">Dies widerspricht normalerweise dem Grund, warum in erster Linie Abfragen ohne Nachverfolgung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="78671-306">This usually runs counter to why no-tracking queries are used in the first place.</span></span> <span data-ttu-id="78671-307">Obwohl die Identitätsauflösung manchmal nützlich sein kann, wird sie nicht benötigt, wenn die Entitäten serialisiert und an den Client gesendet werden, was manchmal für Abfragen ohne Nachverfolgung der Fall ist.</span><span class="sxs-lookup"><span data-stu-id="78671-307">Also, while identity resolution can sometimes be useful, it is not needed if the entities are to be serialized and sent to a client, which is common for no-tracking queries.</span></span>

<span data-ttu-id="78671-308">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-308">**Mitigations**</span></span>

<span data-ttu-id="78671-309">Verwenden Sie eine Nachverfolgungsabfrage, falls die Auflösung erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="78671-309">Use a tracking query if identity resolution is required.</span></span>

<a name="qe"></a>

### <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="78671-310">~~Die Abfrageausführung wird auf Debugebene protokolliert~~ – zurückgesetzt</span><span class="sxs-lookup"><span data-stu-id="78671-310">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="78671-311">Issue #14523</span><span class="sxs-lookup"><span data-stu-id="78671-311">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="78671-312">Diese Änderung wird in Vorschauversion 7 von EF Core 3.0 zurückgesetzt.</span><span class="sxs-lookup"><span data-stu-id="78671-312">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="78671-313">Wir haben diese Änderung zurückgesetzt, weil die neue Konfiguration in EF Core 3.0 die Angabe des Protokolliergrads für jedes Ereignis durch die Anwendung ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="78671-313">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="78671-314">Um z.B. die Protokollierung von SQL zu `Debug` zu ändern, konfigurieren Sie den Grad explizit in `OnConfiguring` oder `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="78671-314">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

<a name="tkv"></a>

### <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="78671-315">Temporäre Schlüsselwerte werden nicht mehr Entitätsinstanzen zugewiesen</span><span class="sxs-lookup"><span data-stu-id="78671-315">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="78671-316">Issue #12378</span><span class="sxs-lookup"><span data-stu-id="78671-316">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="78671-317">Diese Änderung wird in Vorschauversion 2 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-317">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="78671-318">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-318">**Old behavior**</span></span>

<span data-ttu-id="78671-319">Vor Version 3.0 wurden in EF Core allen Schlüsseleigenschaften temporäre Werte zugewiesen. Später wurden den Eigenschaften dann die tatsächlichen Werte zugewiesen, die von der Datenbank generiert wurden.</span><span class="sxs-lookup"><span data-stu-id="78671-319">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="78671-320">Die temporären Werte waren in der Regel große negative Zahlen.</span><span class="sxs-lookup"><span data-stu-id="78671-320">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="78671-321">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-321">**New behavior**</span></span>

<span data-ttu-id="78671-322">Ab Version 3.0 wird in EF Core der temporäre Wert als Teil der Überwachungsinformationen der Entität gespeichert. Die Schlüsseleigenschaft selbst bleibt unverändert.</span><span class="sxs-lookup"><span data-stu-id="78671-322">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="78671-323">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-323">**Why**</span></span>

<span data-ttu-id="78671-324">Diese Änderung wurde vorgenommen, damit temporäre Schlüsselwerte nicht fälschlicherweise dauerhaft gespeichert werden, wenn eine Entität, die vorher von einer `DbContext`-Instanz überwacht wurde, in eine andere `DbContext`-Instanz verschoben wird.</span><span class="sxs-lookup"><span data-stu-id="78671-324">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="78671-325">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-325">**Mitigations**</span></span>

<span data-ttu-id="78671-326">Anwendungen, die Primärschlüsselwerte Fremdschlüsseln zuweisen, um Zuordnungen zwischen Entitäten zu erstellen, können vom alten Verhalten abhängig sein, wenn Primärschlüssel vom Speicher generiert werden und zu Entitäten im `Added`-Zustand gehören.</span><span class="sxs-lookup"><span data-stu-id="78671-326">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="78671-327">Sie können dies wie folgt vermeiden:</span><span class="sxs-lookup"><span data-stu-id="78671-327">This can be avoided by:</span></span>
* <span data-ttu-id="78671-328">Verwenden Sie keine vom Speicher generierten Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="78671-328">Not using store-generated keys.</span></span>
* <span data-ttu-id="78671-329">Legen Sie zum Erstellen von Zuordnungen keine Fremdschlüsselwerte, sondern Navigationseigenschaften fest.</span><span class="sxs-lookup"><span data-stu-id="78671-329">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="78671-330">Rufen Sie die tatsächlichen temporären Schlüsselwerte aus den Überwachungsinformationen der Entität ab.</span><span class="sxs-lookup"><span data-stu-id="78671-330">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="78671-331">`context.Entry(blog).Property(e => e.Id).CurrentValue` gibt beispielsweise den temporären Wert zurück, obwohl `blog.Id` noch nicht festgelegt wurde.</span><span class="sxs-lookup"><span data-stu-id="78671-331">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

<a name="dc"></a>

### <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="78671-332">DetectChanges berücksichtigt vom Speicher generierte Schlüsselwerte</span><span class="sxs-lookup"><span data-stu-id="78671-332">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="78671-333">Issue #14616</span><span class="sxs-lookup"><span data-stu-id="78671-333">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="78671-334">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-334">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="78671-335">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-335">**Old behavior**</span></span>

<span data-ttu-id="78671-336">Vor Version 3.0 wurde in EF Core eine nicht überwachte Entität, die von `DetectChanges` gefunden wurde, im `Added`-Zustand überwacht und als neue Zeile eingefügt, sobald `SaveChanges` aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="78671-336">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="78671-337">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-337">**New behavior**</span></span>

<span data-ttu-id="78671-338">Ab Version 3.0 wird in EF Core die Entität im `Modified`-Zustand überwacht, wenn für diese generierte Schlüsselwerte verwendet werden und ein Schlüsselwert festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="78671-338">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="78671-339">Es wird also davon ausgegangen, dass eine Zeile für die Entität vorhanden ist. Wenn `SaveChanges` aufgerufen wird, wird die Zeile außerdem aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="78671-339">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="78671-340">Falls kein Schlüsselwert festgelegt ist oder der Entitätstyp keine generierten Schlüssel verwendet, wird die neue Entität wie in früheren Version weiterhin im `Added`-Zustand überwacht.</span><span class="sxs-lookup"><span data-stu-id="78671-340">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="78671-341">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-341">**Why**</span></span>

<span data-ttu-id="78671-342">Diese Änderung wurde vorgenommen, um bei der Verwendung von speichergenerierten Schlüsseln die Arbeit mit nicht verbundenen Entitätsgraphen einfacher und konsistenter zu gestalten.</span><span class="sxs-lookup"><span data-stu-id="78671-342">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="78671-343">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-343">**Mitigations**</span></span>

<span data-ttu-id="78671-344">Diese Änderung kann dazu führen, dass eine Anwendung nicht mehr funktioniert. Dieser Fall tritt ein, wenn für Entitätstypen generierte Schlüssel vorgesehen sind, dann jedoch explizit Schlüsselwerte für neue Instanzen festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="78671-344">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="78671-345">Sie können dieses Problem vermeiden, indem Sie explizit festlegen, dass für Schlüsseleigenschaften keine generierten Werte verwendet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="78671-345">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="78671-346">Dies ist beispielsweise mit der Fluent-API möglich:</span><span class="sxs-lookup"><span data-stu-id="78671-346">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="78671-347">Alternativ bieten sich auch Datenanmerkungen an:</span><span class="sxs-lookup"><span data-stu-id="78671-347">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```
<a name="cascade"></a>
### <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="78671-348">Kaskadierende Deletes werden standardmäßig sofort ausgeführt</span><span class="sxs-lookup"><span data-stu-id="78671-348">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="78671-349">Issue #10114</span><span class="sxs-lookup"><span data-stu-id="78671-349">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="78671-350">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-350">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="78671-351">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-351">**Old behavior**</span></span>

<span data-ttu-id="78671-352">Vor Version 3.0 wurden in EF Core kaskadierenden Aktionen (Löschen von abhängigen Entitäten nach dem Löschen eines erforderlichen Prinzipals oder nach dem Aufheben einer Beziehung zum erforderlichen Prinzipal) erst ausgeführt, wenn SaveChanges aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="78671-352">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="78671-353">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-353">**New behavior**</span></span>

<span data-ttu-id="78671-354">Ab Version 3.0 werden in EF Core kaskadierende Aktionen angewendet, sobald die auslösende Bedingung erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="78671-354">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="78671-355">Wenn beispielsweise `context.Remove()` zum Löschen einer Prinzipalentität aufgerufen wird, führt das dazu, dass auch alle zugehörigen erforderlichen Entitäten, die überwacht werden, sofort auf `Deleted` festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="78671-355">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="78671-356">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-356">**Why**</span></span>

<span data-ttu-id="78671-357">Diese Änderung wurde vorgenommen, um die Arbeit mit Datenbindungen und Überwachungsszenarios zu vereinfachen, in denen bekannt sein muss, welche Entitäten _vor_ dem Aufruf von `SaveChanges` gelöscht werden.</span><span class="sxs-lookup"><span data-stu-id="78671-357">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="78671-358">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-358">**Mitigations**</span></span>

<span data-ttu-id="78671-359">Sie können das vorherige Verhalten wiederherstellen, indem Sie für `context.ChangedTracker` die entsprechenden Einstellungen vornehmen.</span><span class="sxs-lookup"><span data-stu-id="78671-359">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="78671-360">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="78671-360">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```
<a name="deletebehavior"></a>
### <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="78671-361">DeleteBehavior.Restrict verfügt über eine übersichtlichere Semantik</span><span class="sxs-lookup"><span data-stu-id="78671-361">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="78671-362">Issue #12661</span><span class="sxs-lookup"><span data-stu-id="78671-362">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="78671-363">Diese Änderung wird in Vorschauversion 5 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-363">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="78671-364">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-364">**Old behavior**</span></span>

<span data-ttu-id="78671-365">Vor Version 3.0 wurden mit `DeleteBehavior.Restrict` Fremdschlüssel in der Datenbank mit `Restrict`-Semantik erstellt, es wurden aber auch unvorhergesehen interne Fixups geändert.</span><span class="sxs-lookup"><span data-stu-id="78671-365">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="78671-366">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-366">**New behavior**</span></span>

<span data-ttu-id="78671-367">Ab Version 3.0 wird mit `DeleteBehavior.Restrict` sichergestellt, dass Fremdschlüssel mit `Restrict`-Semantik erstellt werden – d. h. ohne Überlappungen; ausgenommen bei Einschränkungsverletzungen – und ohne Auswirkungen auf EF-interne Fixups.</span><span class="sxs-lookup"><span data-stu-id="78671-367">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="78671-368">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-368">**Why**</span></span>

<span data-ttu-id="78671-369">Diese Änderung wurde vorgenommen, um die Benutzerfreundlichkeit bei intuitiver Verwendung von `DeleteBehavior` ohne unerwartete Nebeneffekte zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="78671-369">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="78671-370">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-370">**Mitigations**</span></span>

<span data-ttu-id="78671-371">Sie können das vorherige Verhalten wiederherstellen, indem Sie `DeleteBehavior.ClientNoAction` verwenden.</span><span class="sxs-lookup"><span data-stu-id="78671-371">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

<a name="qt"></a>
### <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="78671-372">Abfragetypen werden mit Entitätstypen zusammengeführt</span><span class="sxs-lookup"><span data-stu-id="78671-372">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="78671-373">Issue #14194</span><span class="sxs-lookup"><span data-stu-id="78671-373">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="78671-374">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-374">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="78671-375">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-375">**Old behavior**</span></span>

<span data-ttu-id="78671-376">Vor Version 3.0 wurden in EF Core [Abfragetypen](xref:core/modeling/query-types) dazu verwendet, Daten abzufragen, in denen kein Primärschlüssel auf strukturierte Weise festgelegt war.</span><span class="sxs-lookup"><span data-stu-id="78671-376">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="78671-377">Abfragetypen wurden also für die Zuordnung von Entitätstypen ohne Schlüssel verwendet (in der Regel in einer Sicht, in einigen Fällen aber auch in einer Tabelle), während reguläre Entitätstypen für einen verfügbaren Schlüssel verwendet wurden (in der Regel in einer Tabelle, in einigen Fällen aber auch in einer Sicht).</span><span class="sxs-lookup"><span data-stu-id="78671-377">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="78671-378">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-378">**New behavior**</span></span>

<span data-ttu-id="78671-379">Aus einem Abfragetyp wird nun einfach ein Entitätstyp ohne Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="78671-379">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="78671-380">Entitätstypen ohne Schlüssel besitzen die gleiche Funktionalität wie Abfragetypen in früheren Versionen.</span><span class="sxs-lookup"><span data-stu-id="78671-380">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="78671-381">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-381">**Why**</span></span>

<span data-ttu-id="78671-382">Diese Änderung wurde vorgenommen, um Unklarheiten bei der Verwendung von Abfragetypen zu beseitigen.</span><span class="sxs-lookup"><span data-stu-id="78671-382">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="78671-383">Abfragetypen sind schlüssellose Entitätstypen, die grundsätzlich schreibgeschützt sind. Sie sollten allerdings nicht allein wegen des Schreibschutzes verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="78671-383">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="78671-384">Des Weiteren werden sie oft Sichten zugeordnet, was aber nur daran liegt, dass für Letztere häufig keine Schlüssel definiert werden.</span><span class="sxs-lookup"><span data-stu-id="78671-384">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="78671-385">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-385">**Mitigations**</span></span>

<span data-ttu-id="78671-386">Die folgenden Teile der API sind durch die Änderungen veraltet:</span><span class="sxs-lookup"><span data-stu-id="78671-386">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="78671-387">**`ModelBuilder.Query<>()`**: Rufen Sie stattdessen `ModelBuilder.Entity<>().HasNoKey()` auf, um einen schlüssellosen Entitätstyp festzulegen.</span><span class="sxs-lookup"><span data-stu-id="78671-387">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="78671-388">Dieses Verhalten wird nach wie vor nicht konventionsgemäß festgelegt, um Fehlkonfigurationen zu vermeiden, wenn ein Primärschlüssel erwartet wird, jedoch nicht mit der Konvention übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="78671-388">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="78671-389">**`DbQuery<>`**: Verwenden Sie stattdessen `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="78671-389">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="78671-390">**`DbContext.Query<>()`**: Verwenden Sie stattdessen `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="78671-390">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

<a name="config"></a>
### <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="78671-391">Die Konfigurations-API für Beziehungen abhängiger (owned) Typen wurde geändert</span><span class="sxs-lookup"><span data-stu-id="78671-391">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="78671-392">[Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="78671-392">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="78671-393">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-393">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="78671-394">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-394">**Old behavior**</span></span>

<span data-ttu-id="78671-395">Vor Version 3.0 wurde in EF Core die abhängige Beziehung direkt nach dem Aufruf von `OwnsOne` oder `OwnsMany` konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="78671-395">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="78671-396">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-396">**New behavior**</span></span>

<span data-ttu-id="78671-397">Ab Version 3.0 von EF Core ist eine Fluent-API verfügbar, mit der über `WithOwner()` eine Navigationseigenschaft für den Besitzer konfiguriert werden kann.</span><span class="sxs-lookup"><span data-stu-id="78671-397">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="78671-398">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="78671-398">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="78671-399">Die Konfiguration für die Beziehung zwischen Besitzertyp und abhängigem Typ sollte nun ähnlich wie bei der Konfiguration anderer Beziehungen nach `WithOwner()` verkettet werden.</span><span class="sxs-lookup"><span data-stu-id="78671-399">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="78671-400">Die Konfiguration für den abhängigen Typ wird jedoch weiterhin nach `OwnsOne()/OwnsMany()` verkettet.</span><span class="sxs-lookup"><span data-stu-id="78671-400">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="78671-401">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="78671-401">For example:</span></span>

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

<span data-ttu-id="78671-402">Wenn Sie zusätzlich `Entity()`, `HasOne()`, oder `Set()` für das Ziel eines abhängigen Typs aufrufen, wird keine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="78671-402">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="78671-403">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-403">**Why**</span></span>

<span data-ttu-id="78671-404">Diese Änderung wurde vorgenommen, um eine deutlichere Trennung zwischen der Konfiguration des abhängigen Typs und der _Beziehung zum abhängigen Typ_ zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="78671-404">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="78671-405">Dadurch werden für Methoden wie `HasForeignKey` Mehrdeutigkeiten und Unklarheiten beseitigt.</span><span class="sxs-lookup"><span data-stu-id="78671-405">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="78671-406">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-406">**Mitigations**</span></span>

<span data-ttu-id="78671-407">Passen Sie für die Beziehungen von abhängigen Typen die Konfiguration so an, dass die neue Fluent-API wie im obigen Beispiel verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="78671-407">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

<a name="de"></a>

### <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="78671-408">Abhängige Entitäten, die die Tabelle gemeinsam mit dem Prinzipal verwenden, sind jetzt optional</span><span class="sxs-lookup"><span data-stu-id="78671-408">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="78671-409">Issue #9005</span><span class="sxs-lookup"><span data-stu-id="78671-409">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="78671-410">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-410">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="78671-411">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-411">**Old behavior**</span></span>

<span data-ttu-id="78671-412">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="78671-412">Consider the following model:</span></span>
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
<span data-ttu-id="78671-413">Wenn `OrderDetails` im Besitz von `Order` ist oder explizit derselben Tabelle zugeordnet ist, war vor Version 3.0 in EF Core immer eine `OrderDetails`-Instanz beim Hinzufügen eines neuen `Order`-Objekts erforderlich.</span><span class="sxs-lookup"><span data-stu-id="78671-413">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="78671-414">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-414">**New behavior**</span></span>

<span data-ttu-id="78671-415">Ab Version 3.0 bietet EF Core die Möglichkeit, `Order` ohne `OrderDetails` hinzuzufügen, und alle `OrderDetails`-Eigenschaften, mit Ausnahme des primären Schlüssels, werden Spalten zugeordnet, die NULL-Werte zulassen.</span><span class="sxs-lookup"><span data-stu-id="78671-415">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="78671-416">Bei Abfragen von EF Core wird `OrderDetails` auf `null` festgelegt, wenn eine der erforderlichen Eigenschaften keinen Wert aufweist oder keine erforderlichen Eigenschaften außer dem primären Schlüssel vorhanden und alle Eigenschaften `null` sind.</span><span class="sxs-lookup"><span data-stu-id="78671-416">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="78671-417">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-417">**Mitigations**</span></span>

<span data-ttu-id="78671-418">Wenn Ihr Modell von der gemeinsamen Nutzung einer Tabelle mit allen optionalen Spalten abhängig ist, aber die Navigation, die darauf zeigt, wahrscheinlich nicht `null` ist, sollte die Anwendung für die Handhabung von Fällen geändert werden, in denen die Navigation `null` ist.</span><span class="sxs-lookup"><span data-stu-id="78671-418">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="78671-419">Wenn das nicht möglich ist, sollte dem Entitätstyp eine erforderliche Eigenschaft hinzugefügt werden oder mindestens einer Eigenschaft sollte ein Wert ungleich `null` zugewiesen sein.</span><span class="sxs-lookup"><span data-stu-id="78671-419">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

<a name="aes"></a>

### <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="78671-420">Alle Entitäten, die eine Tabelle mit einer Spalte für das Parallelitätstoken gemeinsam verwenden, müssen diese einer Eigenschaft zuordnen</span><span class="sxs-lookup"><span data-stu-id="78671-420">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="78671-421">Issue #14154</span><span class="sxs-lookup"><span data-stu-id="78671-421">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="78671-422">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-422">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="78671-423">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-423">**Old behavior**</span></span>

<span data-ttu-id="78671-424">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="78671-424">Consider the following model:</span></span>
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
<span data-ttu-id="78671-425">Wenn `OrderDetails` im Besitz von `Order` ist oder explizit derselben Tabelle zugeordnet ist, wird vor Version 3.0 in EF Core durch das alleinige Aktualisieren von `OrderDetails` der `Version`-Wert auf dem Client nicht aktualisiert, und das nächste Update schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="78671-425">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="78671-426">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-426">**New behavior**</span></span>

<span data-ttu-id="78671-427">Ab Version 3.0 gibt EF Core den neuen `Version`-Wert an `Order` weiter, wenn dies Besitzer von `OrderDetails` ist.</span><span class="sxs-lookup"><span data-stu-id="78671-427">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="78671-428">Andernfalls wird eine Ausnahme während der Modellvalidierung ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="78671-428">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="78671-429">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-429">**Why**</span></span>

<span data-ttu-id="78671-430">Diese Änderung wurde vorgenommen, um einen veralteten Wert für ein Parallelitätstoken zu vermeiden, wenn nur eine der Entitäten, die derselben Tabelle zugeordnet sind, aktualisiert wird.</span><span class="sxs-lookup"><span data-stu-id="78671-430">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="78671-431">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-431">**Mitigations**</span></span>

<span data-ttu-id="78671-432">Alle Entitäten, die die Tabelle gemeinsam nutzen, müssen eine Eigenschaft enthalten, die der Spalte für das Parallelitätstoken zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="78671-432">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="78671-433">Es ist möglich, eine solche im Schattenzustand zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="78671-433">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

<a name="ip"></a>

### <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="78671-434">Geerbte Eigenschaften von nicht zugeordneten Typen sind nun einer einzelnen Spalte für alle abgeleiteten Typen zugeordnet</span><span class="sxs-lookup"><span data-stu-id="78671-434">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="78671-435">Issue #13998</span><span class="sxs-lookup"><span data-stu-id="78671-435">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="78671-436">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-436">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="78671-437">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-437">**Old behavior**</span></span>

<span data-ttu-id="78671-438">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="78671-438">Consider the following model:</span></span>
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

<span data-ttu-id="78671-439">Vor Version 3.0 wurde in EF Core die Eigenschaft `ShippingAddress` standardmäßig separaten Spalten für `BulkOrder` und `Order` zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="78671-439">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="78671-440">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-440">**New behavior**</span></span>

<span data-ttu-id="78671-441">Ab Version 3.0 erstellt EF Core nur eine Spalte für `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="78671-441">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="78671-442">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-442">**Why**</span></span>

<span data-ttu-id="78671-443">Der alte Verhalten war unerwartet.</span><span class="sxs-lookup"><span data-stu-id="78671-443">The old behavoir was unexpected.</span></span>

<span data-ttu-id="78671-444">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-444">**Mitigations**</span></span>

<span data-ttu-id="78671-445">Die Eigenschaft kann noch immer explizit einer separaten Spalte für die abgeleiteten Typen zugeordnet werden:</span><span class="sxs-lookup"><span data-stu-id="78671-445">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

### <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="78671-446">Die Konvention zur Fremdschlüsseleigenschaft entspricht nicht mehr dem Namen der Prinzipaleigenschaft</span><span class="sxs-lookup"><span data-stu-id="78671-446">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="78671-447">Issue #13274</span><span class="sxs-lookup"><span data-stu-id="78671-447">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="78671-448">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-448">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="78671-449">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-449">**Old behavior**</span></span>

<span data-ttu-id="78671-450">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="78671-450">Consider the following model:</span></span>
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
<span data-ttu-id="78671-451">Vor Version 3.0 wurde in EF Core gemäß der Konvention die `CustomerId`-Eigenschaft für den Fremdschlüssel verwendet.</span><span class="sxs-lookup"><span data-stu-id="78671-451">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="78671-452">Wenn `Order` jedoch ein abhängiger Typ ist, wäre `CustomerId` der Primärschlüssel, was normalerweise nicht der Erwartungshaltung entspricht.</span><span class="sxs-lookup"><span data-stu-id="78671-452">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="78671-453">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-453">**New behavior**</span></span>

<span data-ttu-id="78671-454">Ab Version 3.0 werden in EF Core konventionsgemäß keine Eigenschaften mehr für Fremdschlüssel verwendet, wenn diese denselben Namen wie die Prinzipaleigenschaft besitzen.</span><span class="sxs-lookup"><span data-stu-id="78671-454">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="78671-455">Die Muster für den Namen des Prinzipaltyps, der mit dem Namen der Prinzipaleigenschaft verkettet wird, und für den Navigationsnamen, der mit dem Namen der Prinzipaleigenschaft verkettet wird, werden jedoch weiterhin abgeglichen.</span><span class="sxs-lookup"><span data-stu-id="78671-455">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="78671-456">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="78671-456">For example:</span></span>

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

<span data-ttu-id="78671-457">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-457">**Why**</span></span>

<span data-ttu-id="78671-458">Diese Änderung wurde vorgenommen, um zu vermeiden, dass versehentlich eine Primärschlüsseleigenschaft für den abhängigen Typ definiert wird.</span><span class="sxs-lookup"><span data-stu-id="78671-458">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="78671-459">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-459">**Mitigations**</span></span>

<span data-ttu-id="78671-460">Wenn die Eigenschaft als Fremdschlüssel und daher Teil des Primärschlüssels vorgesehen war, müssen Sie diese explizit als solche festlegen.</span><span class="sxs-lookup"><span data-stu-id="78671-460">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

<a name="dbc"></a>

### <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="78671-461">Die Datenbankverbindung wird jetzt geschlossen, wenn sie nicht mehr verwendet wird, bevor TransactionScope abgeschlossen wurde</span><span class="sxs-lookup"><span data-stu-id="78671-461">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="78671-462">Issue #14218</span><span class="sxs-lookup"><span data-stu-id="78671-462">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="78671-463">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-463">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="78671-464">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-464">**Old behavior**</span></span>

<span data-ttu-id="78671-465">Wenn vor Version 3.0 in EF Core der Kontext die Verbindung in einem `TransactionScope` öffnet, bleibt die Verbindung geöffnet, während der aktuelle `TransactionScope` aktiv ist.</span><span class="sxs-lookup"><span data-stu-id="78671-465">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="78671-466">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-466">**New behavior**</span></span>

<span data-ttu-id="78671-467">Ab Version 3.0 schließt EF Core die Verbindung, sobald sie nicht mehr verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="78671-467">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="78671-468">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-468">**Why**</span></span>

<span data-ttu-id="78671-469">Diese Änderung ermöglicht die Verwendung mehrerer Kontexte in demselben `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="78671-469">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="78671-470">Das neue Verhalten entspricht außerdem EF6.</span><span class="sxs-lookup"><span data-stu-id="78671-470">The new behavior also matches EF6.</span></span>

<span data-ttu-id="78671-471">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-471">**Mitigations**</span></span>

<span data-ttu-id="78671-472">Wenn die Verbindung geöffnet bleiben muss, wird durch einen expliziten Aufruf von `OpenConnection()` sichergestellt, dass EF Core diese nicht vorzeitig schließt:</span><span class="sxs-lookup"><span data-stu-id="78671-472">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

### <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="78671-473">Für jede Eigenschaft wird separat ein ganzzahliger speicherinterner Schlüssel generiert</span><span class="sxs-lookup"><span data-stu-id="78671-473">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="78671-474">Issue #6872</span><span class="sxs-lookup"><span data-stu-id="78671-474">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="78671-475">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-475">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="78671-476">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-476">**Old behavior**</span></span>

<span data-ttu-id="78671-477">Vor Version 3.0 wurde in EF Core ein gemeinsam verwendeter Wertegenerator für alle speicherinternen ganzzahligen Schlüsseleigenschaften verwendet.</span><span class="sxs-lookup"><span data-stu-id="78671-477">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="78671-478">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-478">**New behavior**</span></span>

<span data-ttu-id="78671-479">Ab Version 3.0 von EF Core wird für jede ganzzahlige Schlüsseleigenschaft ein eigener Wertegenerator bei der Verwendung der In-Memory Database verwendet.</span><span class="sxs-lookup"><span data-stu-id="78671-479">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="78671-480">Wenn die Datenbank gelöscht wird, wird die Schlüsselgenerierung für alle Tabellen zurückgesetzt.</span><span class="sxs-lookup"><span data-stu-id="78671-480">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="78671-481">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-481">**Why**</span></span>

<span data-ttu-id="78671-482">Diese Änderung wurde vorgenommen, um die speicherinterne Schlüsselgenerierung enger mit der Schlüsselgenerierung für echte Datenbanken abzustimmen. Außerdem sollen bei Verwendung der In-Memory Database Tests leichter voneinander isoliert werden können.</span><span class="sxs-lookup"><span data-stu-id="78671-482">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="78671-483">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-483">**Mitigations**</span></span>

<span data-ttu-id="78671-484">Wenn Anwendungen von bestimmten speicherinternen Schlüsselwerten abhängig sind, funktionieren Erstere durch die eingeführte Änderung möglicherweise nicht mehr.</span><span class="sxs-lookup"><span data-stu-id="78671-484">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="78671-485">Versuchen Sie, Abhängigkeiten von bestimmten Schlüsselwerten zu vermeiden, oder passen Sie die Anwendung an das neue Verhalten an.</span><span class="sxs-lookup"><span data-stu-id="78671-485">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

### <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="78671-486">Unterstützungsfelder werden standardmäßig verwendet</span><span class="sxs-lookup"><span data-stu-id="78671-486">Backing fields are used by default</span></span>

[<span data-ttu-id="78671-487">Issue #12430</span><span class="sxs-lookup"><span data-stu-id="78671-487">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="78671-488">Diese Änderung wird in Vorschauversion 2 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-488">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="78671-489">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-489">**Old behavior**</span></span>

<span data-ttu-id="78671-490">Vor Version 3.0 wurde in EF Core mithilfe der Getter- und Settermethoden standardmäßig der Eigenschaftswert gelesen und geschrieben. Das war selbst dann der Fall, wenn das Unterstützungsfeld für eine Eigenschaft nicht bekannt war.</span><span class="sxs-lookup"><span data-stu-id="78671-490">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="78671-491">Eine Ausnahme war die Abfrageausführung, bei der das Unterstützungsfeld direkt festgelegt wurde, wenn es bekannt war.</span><span class="sxs-lookup"><span data-stu-id="78671-491">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="78671-492">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-492">**New behavior**</span></span>

<span data-ttu-id="78671-493">Ab Version 3.0 wird in EF Core die Eigenschaft mithilfe des Unterstützungsfelds gelesen und geschrieben, wenn dieses für eine Eigenschaft bekannt ist.</span><span class="sxs-lookup"><span data-stu-id="78671-493">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="78671-494">Dies kann dazu führen, dass eine Anwendung nicht mehr funktioniert, wenn sie auf zusätzliches Verhalten angewiesen ist, das im Code der Getter- und Settermethoden definiert ist.</span><span class="sxs-lookup"><span data-stu-id="78671-494">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="78671-495">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-495">**Why**</span></span>

<span data-ttu-id="78671-496">Diese Änderung wurde vorgenommen, um zu verhindern, dass in EF Core fälschlicherweise immer dann Geschäftslogik ausgelöst wird, wenn Datenbankvorgänge für Entitäten durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="78671-496">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="78671-497">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-497">**Mitigations**</span></span>

<span data-ttu-id="78671-498">Sie können das Verhalten von vor Version 3.0 wiederherstellen, indem Sie in `ModelBuilder` den Zugriffsmodus für die Eigenschaft konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="78671-498">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="78671-499">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="78671-499">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

### <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="78671-500">Eine Ausnahme wird ausgelöst, wenn mehrere kompatible Unterstützungsfelder gefunden werden</span><span class="sxs-lookup"><span data-stu-id="78671-500">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="78671-501">Issue #12523</span><span class="sxs-lookup"><span data-stu-id="78671-501">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="78671-502">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-502">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="78671-503">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-503">**Old behavior**</span></span>

<span data-ttu-id="78671-504">Vor Version 3.0 wurde in EF Core ein Unterstützungsfeld auf Grundlage einer Rangfolge ausgewählt, wenn die Regeln zum Suchen eines Eigenschaftsfelds auf mehrere Felder zutrafen.</span><span class="sxs-lookup"><span data-stu-id="78671-504">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="78671-505">Dabei konnte es vorkommen, dass in mehrdeutigen Fällen das falsche Feld verwendet wurde.</span><span class="sxs-lookup"><span data-stu-id="78671-505">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="78671-506">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-506">**New behavior**</span></span>

<span data-ttu-id="78671-507">Ab Version 3.0 wird in EF Core eine Ausnahme ausgelöst, wenn sich mehrere Felder auf dieselbe Eigenschaft beziehen.</span><span class="sxs-lookup"><span data-stu-id="78671-507">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="78671-508">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-508">**Why**</span></span>

<span data-ttu-id="78671-509">Diese Änderung wurde vorgenommen, um zu vermeiden, dass automatisch ein falsches Feld verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="78671-509">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="78671-510">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-510">**Mitigations**</span></span>

<span data-ttu-id="78671-511">Für Eigenschaften mit mehrdeutigen Unterstützungsfeldern muss das zu verwendende Feld explizit festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="78671-511">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="78671-512">Mit der Fluent-API ist dies beispielsweise wie folgt möglich:</span><span class="sxs-lookup"><span data-stu-id="78671-512">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

### <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="78671-513">„Nur-Feld“-Eigenschaftsnamen sollten dem Feldnamen entsprechen</span><span class="sxs-lookup"><span data-stu-id="78671-513">Field-only property names should match the field name</span></span>

<span data-ttu-id="78671-514">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-514">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="78671-515">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-515">**Old behavior**</span></span>

<span data-ttu-id="78671-516">Vor Version 3.0 konnte in EF Core eine Eigenschaft durch einen Zeichenfolgenwert angegeben werden, und wenn keine Eigenschaft mit diesem Namen im CLR-Typ gefunden wurde, versuchte EF Core, mithilfe von Konventionsregeln ein übereinstimmendes Feld zu finden.</span><span class="sxs-lookup"><span data-stu-id="78671-516">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="78671-517">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-517">**New behavior**</span></span>

<span data-ttu-id="78671-518">Ab Version 3.0 muss in EF Core eine „Nur-Feld“-Eigenschaft mit dem Namen des Felds genau übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="78671-518">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="78671-519">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-519">**Why**</span></span>

<span data-ttu-id="78671-520">Diese Änderung wurde vorgenommen, um zu vermeiden, dass das gleiche Feld für zwei Eigenschaften mit ähnlichem Namen verwendet wird. Durch die Änderung entsprechen nun auch die Übereinstimmungsregeln für „Nur-Feld“-Eigenschaften den Regeln für Eigenschaften, die CLR-Eigenschaften zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="78671-520">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="78671-521">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-521">**Mitigations**</span></span>

<span data-ttu-id="78671-522">„Nur-Feld“-Eigenschaften müssen so benannt werden wie das Feld, dem sie zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="78671-522">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="78671-523">In einer späteren Vorschauversion von EF Core 3.0 soll das explizite Konfigurieren eines Feldnamens, der sich vom Eigenschaftsnamen unterscheidet, erneut aktiviert werden:</span><span class="sxs-lookup"><span data-stu-id="78671-523">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

<a name="adddbc"></a>

### <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="78671-524">Von AddDbContext/AddDbContextPool werden AddLogging und AddMemoryCache nicht mehr aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="78671-524">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="78671-525">Issue #14756</span><span class="sxs-lookup"><span data-stu-id="78671-525">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="78671-526">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-526">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="78671-527">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-527">**Old behavior**</span></span>

<span data-ttu-id="78671-528">Vor EF Core 3.0 wurden beim Aufruf von `AddDbContext` oder `AddDbContextPool` durch Aufrufe von [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) und [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache) auch Protokollierungs- und Arbeitsspeichercaching-Dienste bei DI registriert.</span><span class="sxs-lookup"><span data-stu-id="78671-528">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="78671-529">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-529">**New behavior**</span></span>

<span data-ttu-id="78671-530">Ab EF Core 3.0 werden diese Dienste durch `AddDbContext` und `AddDbContextPool` nicht mehr bei Dependency Injection (DI) registriert.</span><span class="sxs-lookup"><span data-stu-id="78671-530">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="78671-531">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-531">**Why**</span></span>

<span data-ttu-id="78671-532">Für EF Core 3.0 ist es nicht erforderlich, dass diese Dienste im DI-Container der Anwendung enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="78671-532">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="78671-533">Wenn `ILoggerFactory` jedoch im DI-Container der Anwendung registriert ist, wird sie nach wie vor von EF Core verwendet.</span><span class="sxs-lookup"><span data-stu-id="78671-533">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="78671-534">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-534">**Mitigations**</span></span>

<span data-ttu-id="78671-535">Wenn Ihre Anwendung diese Dienste benötigt, registrieren Sie sie explizit mit [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) oder [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache) beim DI-Container.</span><span class="sxs-lookup"><span data-stu-id="78671-535">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<a name="dbe"></a>

### <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="78671-536">DbContext.Entry erkennt nun mit DetectChanges lokale Änderungen</span><span class="sxs-lookup"><span data-stu-id="78671-536">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="78671-537">Issue #13552</span><span class="sxs-lookup"><span data-stu-id="78671-537">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="78671-538">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-538">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="78671-539">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-539">**Old behavior**</span></span>

<span data-ttu-id="78671-540">Vor Version 3.0 führte in EF Core ein Aufruf von `DbContext.Entry` dazu, dass Änderungen an allen überwachten Entitäten erkannt wurden.</span><span class="sxs-lookup"><span data-stu-id="78671-540">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="78671-541">Dadurch wurde sichergestellt, dass der Zustand in `EntityEntry` immer aktuell war.</span><span class="sxs-lookup"><span data-stu-id="78671-541">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="78671-542">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-542">**New behavior**</span></span>

<span data-ttu-id="78671-543">Ab Version 3.0 werden in EF Core Änderungen bei einem Aufruf von `DbContext.Entry` nur in der angegebenen Entität und in allen überwachten Prinzipalentitäten erkannt.</span><span class="sxs-lookup"><span data-stu-id="78671-543">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="78671-544">Änderungen an anderer Stelle werden daher durch den Aufruf dieser Methode möglicherweise nicht erkannt, was sich auf den Anwendungszustand auswirken kann.</span><span class="sxs-lookup"><span data-stu-id="78671-544">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="78671-545">Beachten Sie, dass selbst die Erkennung lokaler Änderungen deaktiviert wird, wenn `ChangeTracker.AutoDetectChangesEnabled` auf `false` festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="78671-545">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="78671-546">Andere Methoden wie `ChangeTracker.Entries` und `SaveChanges`, die eine Änderungserkennung auslösen, führen nach wie vor zu einem vollständigen `DetectChanges`-Vorgang für alle überwachten Entitäten.</span><span class="sxs-lookup"><span data-stu-id="78671-546">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="78671-547">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-547">**Why**</span></span>

<span data-ttu-id="78671-548">Diese Änderung wurde vorgenommen, um die Leistung bei der standardmäßigen Verwendung von `context.Entry` zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="78671-548">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="78671-549">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-549">**Mitigations**</span></span>

<span data-ttu-id="78671-550">Rufen Sie `ChgangeTracker.DetectChanges()` explizit vor dem Aufruf von `Entry` auf, wenn das Verhalten vor Version 3.0 beibehalten werden soll.</span><span class="sxs-lookup"><span data-stu-id="78671-550">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

### <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="78671-551">Schlüssel aus Zeichenfolgen und Bytearrays werden standardmäßig nicht vom Client generiert</span><span class="sxs-lookup"><span data-stu-id="78671-551">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="78671-552">Issue #14617</span><span class="sxs-lookup"><span data-stu-id="78671-552">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="78671-553">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-553">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="78671-554">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-554">**Old behavior**</span></span>

<span data-ttu-id="78671-555">Vor Version 3.0 konnten in EF Core `string`- und `byte[]`-Schlüsseleigenschaften verwendet werden, ohne dass explizit ein anderer Wert als NULL festgelegt werden musste.</span><span class="sxs-lookup"><span data-stu-id="78671-555">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="78671-556">In diesem Fall wurde der Schlüsselwert auf dem Client als GUID generiert und für `byte[]` in Bytes serialisiert.</span><span class="sxs-lookup"><span data-stu-id="78671-556">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="78671-557">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-557">**New behavior**</span></span>

<span data-ttu-id="78671-558">Ab Version 3.0 wird in EF Core eine Ausnahme ausgelöst, wenn kein Schlüsselwert festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="78671-558">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="78671-559">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-559">**Why**</span></span>

<span data-ttu-id="78671-560">Diese Änderung wurde vorgenommen, da vom Client generierte `string`/`byte[]`-Werte in der Regel nicht sinnvoll sind. Das Standardverhalten führte außerdem zu kaum nachvollziehbaren generierten Schlüsselwerten.</span><span class="sxs-lookup"><span data-stu-id="78671-560">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="78671-561">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-561">**Mitigations**</span></span>

<span data-ttu-id="78671-562">Wenn Sie das Verhalten vor Version 3.0 nutzen möchten, müssen Sie explizit festlegen, dass für die Schlüsseleigenschaften generierte Werte verwendet sollen, wenn kein anderer Nicht-NULL-Wert festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="78671-562">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="78671-563">Dies ist beispielsweise mit der Fluent-API möglich:</span><span class="sxs-lookup"><span data-stu-id="78671-563">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="78671-564">Alternativ bieten sich auch Datenanmerkungen an:</span><span class="sxs-lookup"><span data-stu-id="78671-564">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

<a name="ilf"></a>

### <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="78671-565">ILoggerFactory ist nun ein bereichsbezogener Dienst</span><span class="sxs-lookup"><span data-stu-id="78671-565">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="78671-566">Issue #14698</span><span class="sxs-lookup"><span data-stu-id="78671-566">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="78671-567">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-567">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="78671-568">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-568">**Old behavior**</span></span>

<span data-ttu-id="78671-569">Vor Version 3.0 wurde `ILoggerFactory` in EF Core als Singletondienst registriert.</span><span class="sxs-lookup"><span data-stu-id="78671-569">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="78671-570">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-570">**New behavior**</span></span>

<span data-ttu-id="78671-571">Ab Version 3.0 wird in EF Core `ILoggerFactory` als bereichsbezogener Dienst registriert.</span><span class="sxs-lookup"><span data-stu-id="78671-571">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="78671-572">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-572">**Why**</span></span>

<span data-ttu-id="78671-573">Diese Änderung wurde vorgenommen, um einer `DbContext`-Instanz eine Protokollierung zuordnen zu können. Dadurch werden weitere Funktionen bereitgestellt. In einigen Fällen wird außerdem schädliches Verhalten wie etwa die schnelle Zunahme von internen Dienstanbietern beseitigt.</span><span class="sxs-lookup"><span data-stu-id="78671-573">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="78671-574">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-574">**Mitigations**</span></span>

<span data-ttu-id="78671-575">Diese Änderung wirkt sich nur dann auf den Anwendungscode aus, wenn benutzerdefinierte Dienste auf dem internen Dienstanbieter von EF Core registriert und verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="78671-575">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="78671-576">Ein solches Szenario ist aber unüblich.</span><span class="sxs-lookup"><span data-stu-id="78671-576">This isn't common.</span></span>
<span data-ttu-id="78671-577">In einem derartigen Fall funktioniert fast alles wie vorher. Singletondienste, die von `ILoggerFactory` abhängig sind, müssen jedoch so angepasst werden, dass `ILoggerFactory` auf andere Weise abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="78671-577">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="78671-578">Erstellen Sie in diesen Situationen bitte im [GitHub-Issuetracker für EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) ein Issue, in dem Sie beschreiben, wie Sie `ILoggerFactory` verwenden. So können wir in Zukunft erneute Breaking Changes vermeiden.</span><span class="sxs-lookup"><span data-stu-id="78671-578">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

### <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="78671-579">Für Proxys mit Lazy Loading (trägem Laden) gilt nicht mehr die Annahme, dass Navigationseigenschaften vollständig geladen sind</span><span class="sxs-lookup"><span data-stu-id="78671-579">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="78671-580">Issue #12780</span><span class="sxs-lookup"><span data-stu-id="78671-580">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="78671-581">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-581">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="78671-582">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-582">**Old behavior**</span></span>

<span data-ttu-id="78671-583">Vor Version 3.0 gab es in EF Core nach der Freigabe eines `DbContext`-Objekts keine Möglichkeit, zu ermitteln, ob eine aus dem Kontext abgerufene Navigationseigenschaft für eine Entität vollständig geladen war.</span><span class="sxs-lookup"><span data-stu-id="78671-583">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="78671-584">Stattdessen galt für Proxys die Annahme, dass eine Verweisnavigation geladen ist, falls für diese ein anderer Wert als NULL vorliegt. Außerdem wurde davon ausgegangen, dass eine Sammlungsnavigation geladen ist, wenn sie nicht leer ist.</span><span class="sxs-lookup"><span data-stu-id="78671-584">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="78671-585">In diesen Fällen entsprach das Lazy Loading einer Nulloperation („No-Op“).</span><span class="sxs-lookup"><span data-stu-id="78671-585">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="78671-586">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-586">**New behavior**</span></span>

<span data-ttu-id="78671-587">Ab Version 3.0 überwachen in EF Core Proxys, ob eine Navigationseigenschaft geladen ist.</span><span class="sxs-lookup"><span data-stu-id="78671-587">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="78671-588">Wenn also auf eine Navigationseigenschaft zugegriffen werden soll, die nach der Freigabe des Kontexts geladen wird, ist dies immer eine No-Op-Operation. Das ist selbst dann der Fall, wenn die geladene Navigation leer oder NULL ist.</span><span class="sxs-lookup"><span data-stu-id="78671-588">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="78671-589">Wenn umgekehrt auf eine nicht geladene Navigationseigenschaft zugegriffen werden soll und der Kontext bereits freigegeben wurde, wird eine Ausnahme ausgelöst. Dieser Fall tritt auch dann ein, wenn die Navigationseigenschaft eine nicht leere Sammlung ist.</span><span class="sxs-lookup"><span data-stu-id="78671-589">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="78671-590">In dieser Situation wird also vom Anwendungscode versucht, zu einem ungültigen Zeitpunkt Lazy Loading durchzuführen. Die Anwendung sollte so angepasst werden, dass dieser Vorgang vermieden wird.</span><span class="sxs-lookup"><span data-stu-id="78671-590">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="78671-591">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-591">**Why**</span></span>

<span data-ttu-id="78671-592">Diese Änderung wurde vorgenommen, um das Verhalten beim Lazy Loading einer freigegebenen `DbContext`-Instanz konsistent und korrekt zu gestalten.</span><span class="sxs-lookup"><span data-stu-id="78671-592">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="78671-593">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-593">**Mitigations**</span></span>

<span data-ttu-id="78671-594">Aktualisieren Sie den Anwendungscode so, dass kein Lazy Loading durchgeführt wird, nachdem der Kontext freigegeben wurde. Alternativ können Sie auch, wie in der Ausnahmemeldung beschrieben wird, eine No-Op-Operation festlegen.</span><span class="sxs-lookup"><span data-stu-id="78671-594">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

### <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="78671-595">Die Erstellung zu vieler interner Dienstanbieter führt standardmäßig zu einem Fehler</span><span class="sxs-lookup"><span data-stu-id="78671-595">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="78671-596">Issue #10236</span><span class="sxs-lookup"><span data-stu-id="78671-596">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="78671-597">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-597">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="78671-598">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-598">**Old behavior**</span></span>

<span data-ttu-id="78671-599">Vor Version 3.0 wurde in EF Core eine Warnung für eine Anwendung protokolliert, die unverhältnismäßig viele interne Dienstanbieter erstellte.</span><span class="sxs-lookup"><span data-stu-id="78671-599">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="78671-600">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-600">**New behavior**</span></span>

<span data-ttu-id="78671-601">Ab Version 3.0 wird diese Warnung in EF Core als Fehler betrachtet, und eine Ausnahme wird ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="78671-601">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="78671-602">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-602">**Why**</span></span>

<span data-ttu-id="78671-603">Diese Änderung wurde vorgenommen, damit auf den oben beschriebenen Fall explizit hingewiesen wird. So soll die Entwicklung von besserem Anwendungscode erleichtert werden.</span><span class="sxs-lookup"><span data-stu-id="78671-603">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="78671-604">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-604">**Mitigations**</span></span>

<span data-ttu-id="78671-605">Wenn dieser Fehler auftritt, müssen Sie die Grundursache ermitteln und die Erstellung vieler interner Dienstanbieter unterbinden.</span><span class="sxs-lookup"><span data-stu-id="78671-605">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="78671-606">Sie können den Fehler allerdings auch wieder in eine Warnung konvertierten (oder ihn ignorieren), indem Sie `DbContextOptionsBuilder` konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="78671-606">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="78671-607">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="78671-607">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

<a name="nbh"></a>

### <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="78671-608">Neues Verhalten für HasOne/HasMany bei Aufruf mit einer einzelnen Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="78671-608">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="78671-609">Issue #9171</span><span class="sxs-lookup"><span data-stu-id="78671-609">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="78671-610">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-610">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="78671-611">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-611">**Old behavior**</span></span>

<span data-ttu-id="78671-612">Vor EF Core 3.0 wurde Code, der `HasOne` oder `HasMany` mit einer einzelnen Zeichenfolge aufruft, auf irritierende Weise interpretiert.</span><span class="sxs-lookup"><span data-stu-id="78671-612">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="78671-613">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="78671-613">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="78671-614">Der Code scheint `Samurai` mit einem anderen Entitätstyp über die `Entrance`-Navigationseigenschaft zu verknüpfen, die möglicherweise privat ist.</span><span class="sxs-lookup"><span data-stu-id="78671-614">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="78671-615">Tatsächlich versucht dieser Code, eine Beziehung zu einem Entitätstyp namens `Entrance` ohne Navigationseigenschaft herzustellen.</span><span class="sxs-lookup"><span data-stu-id="78671-615">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="78671-616">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-616">**New behavior**</span></span>

<span data-ttu-id="78671-617">Ab EF Core 3.0 führt der obige Code jetzt die erwartete Aktion durch.</span><span class="sxs-lookup"><span data-stu-id="78671-617">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="78671-618">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-618">**Why**</span></span>

<span data-ttu-id="78671-619">Das alte Verhalten war sehr verwirrend, vor allem beim Lesen des Konfigurationscodes und bei der Suche nach Fehlern.</span><span class="sxs-lookup"><span data-stu-id="78671-619">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="78671-620">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-620">**Mitigations**</span></span>

<span data-ttu-id="78671-621">Dies führt nur zu Fehlern in Anwendungen, die Beziehungen explizit unter Verwendung von Zeichenfolgen für Typnamen und ohne explizite Angabe der Navigationseigenschaft konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="78671-621">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="78671-622">Dies ist nicht üblich.</span><span class="sxs-lookup"><span data-stu-id="78671-622">This is not common.</span></span>
<span data-ttu-id="78671-623">Das vorherige Verhalten kann durch explizite Übergabe von `null` für den Namen der Navigationseigenschaft erzielt werden.</span><span class="sxs-lookup"><span data-stu-id="78671-623">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="78671-624">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="78671-624">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

<a name="rtnt"></a>

### <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="78671-625">Der Rückgabetyp für mehrere asynchrone Methoden wurde von Task in ValueTask geändert</span><span class="sxs-lookup"><span data-stu-id="78671-625">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="78671-626">Issue #15184</span><span class="sxs-lookup"><span data-stu-id="78671-626">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="78671-627">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-627">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="78671-628">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-628">**Old behavior**</span></span>

<span data-ttu-id="78671-629">Die folgenden asynchronen Methoden gaben zuvor `Task<T>` zurück:</span><span class="sxs-lookup"><span data-stu-id="78671-629">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="78671-630">`ValueGenerator.NextValueAsync()` (und abgeleitete Klassen)</span><span class="sxs-lookup"><span data-stu-id="78671-630">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="78671-631">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-631">**New behavior**</span></span>

<span data-ttu-id="78671-632">Die oben genannten Methoden geben nun `ValueTask<T>` über dieselbe `T` wie zuvor zurück.</span><span class="sxs-lookup"><span data-stu-id="78671-632">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="78671-633">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-633">**Why**</span></span>

<span data-ttu-id="78671-634">Durch diese Änderung verringert sich die Anzahl der Heapzuordnungen, die beim Aufrufen dieser Methoden entstehen, und dies führt zu einer Verbesserung der allgemeinen Leistung.</span><span class="sxs-lookup"><span data-stu-id="78671-634">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="78671-635">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-635">**Mitigations**</span></span>

<span data-ttu-id="78671-636">Anwendungen, die einfach die oben genannten APIs erwarten, müssen nur neu kompiliert werden – Quelländerungen sind nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="78671-636">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="78671-637">Eine komplexere Verwendung (z.B. Übergeben der zurückgegebenen `Task` an `Task.WhenAny()`) erfordern in der Regel, dass die zurückgegebene `ValueTask<T>` durch einen Aufruf von `AsTask()` in `Task<T>` konvertiert wird.</span><span class="sxs-lookup"><span data-stu-id="78671-637">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="78671-638">Beachten Sie, dass dadurch die mit dieser Änderung verbundene Verringerung der Zuordnungen aufgehoben wird.</span><span class="sxs-lookup"><span data-stu-id="78671-638">Note that this negates the allocation reduction that this change brings.</span></span>

<a name="rtt"></a>

### <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="78671-639">Die Anmerkung Relational:TypeMapping heißt nun nur noch TypeMapping</span><span class="sxs-lookup"><span data-stu-id="78671-639">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="78671-640">Issue #9913</span><span class="sxs-lookup"><span data-stu-id="78671-640">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="78671-641">Diese Änderung wird in Vorschauversion 2 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-641">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="78671-642">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-642">**Old behavior**</span></span>

<span data-ttu-id="78671-643">Der Anmerkungsname für Typzuordnungsanmerkungen war bisher Relational:TypeMapping.</span><span class="sxs-lookup"><span data-stu-id="78671-643">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="78671-644">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-644">**New behavior**</span></span>

<span data-ttu-id="78671-645">Der Anmerkungsname für Typzuordnungsanmerkungen lautet nun TypeMapping.</span><span class="sxs-lookup"><span data-stu-id="78671-645">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="78671-646">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-646">**Why**</span></span>

<span data-ttu-id="78671-647">Typzuordnungen werden nicht mehr ausschließlich für Anbieter relationaler Datenbanken verwendet.</span><span class="sxs-lookup"><span data-stu-id="78671-647">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="78671-648">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-648">**Mitigations**</span></span>

<span data-ttu-id="78671-649">Diese Änderung ist nur dann ein Breaking Change, wenn Anwendungen direkt über eine Anmerkung auf die Typzuordnung zugreifen. Dieses Szenario ist jedoch unüblich.</span><span class="sxs-lookup"><span data-stu-id="78671-649">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="78671-650">Verzichten Sie daher darauf, die Anmerkung direkt zu verwenden, und greifen Sie stattdessen über die Fluent-API auf Typzuordnungen zu, um das Problem zu beheben.</span><span class="sxs-lookup"><span data-stu-id="78671-650">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

### <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="78671-651">ToTable löst für einen abgeleiteten Typ eine Ausnahme aus</span><span class="sxs-lookup"><span data-stu-id="78671-651">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="78671-652">Issue #11811</span><span class="sxs-lookup"><span data-stu-id="78671-652">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="78671-653">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-653">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="78671-654">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-654">**Old behavior**</span></span>

<span data-ttu-id="78671-655">Vor Version 3.0 wurde in EF Core der Aufruf von `ToTable()` für einen abgeleiteten Typ ignoriert, da TPH die einzige Strategie zur Vererbungszuordnung war und diese in unzulässigen Fällen angewendet wurde.</span><span class="sxs-lookup"><span data-stu-id="78671-655">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="78671-656">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-656">**New behavior**</span></span>

<span data-ttu-id="78671-657">Ab Version 3.0 wird in EF Core eine Ausnahme ausgelöst, um später eine unerwartete Zuordnung zu vermeiden, wenn `ToTable()` für einen abgeleiteten Typ aufgerufen wird. Dieser Schritt dient auch als Vorbereitung für die TPT- und TPC-Unterstützung, die in einem künftigen Release folgen wird.</span><span class="sxs-lookup"><span data-stu-id="78671-657">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="78671-658">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-658">**Why**</span></span>

<span data-ttu-id="78671-659">Derzeit ist es nicht zulässig, einen abgeleiteten Typ einer anderen Tabelle zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="78671-659">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="78671-660">Durch diese Änderung werden Breaking Changes vermieden, wenn der beschriebene Vorgang in Zukunft zulässig wird.</span><span class="sxs-lookup"><span data-stu-id="78671-660">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="78671-661">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-661">**Mitigations**</span></span>

<span data-ttu-id="78671-662">Entfernen Sie alle Zuordnungen von abgeleiteten Typen zu anderen Tabellen.</span><span class="sxs-lookup"><span data-stu-id="78671-662">Remove any attempts to map derived types to other tables.</span></span>

### <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="78671-663">ForSqlServerHasIndex wurde durch HasIndex ersetzt</span><span class="sxs-lookup"><span data-stu-id="78671-663">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="78671-664">Issue #12366</span><span class="sxs-lookup"><span data-stu-id="78671-664">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="78671-665">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-665">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="78671-666">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-666">**Old behavior**</span></span>

<span data-ttu-id="78671-667">Vor Version 3.0 konnten in EF Core mit `ForSqlServerHasIndex().ForSqlServerInclude()` Spalten konfiguriert werden, die mit `INCLUDE` verwendet wurden.</span><span class="sxs-lookup"><span data-stu-id="78671-667">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="78671-668">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-668">**New behavior**</span></span>

<span data-ttu-id="78671-669">Ab Version 3.0 wird in EF Core die Nutzung von `Include` für einen Index auf der relationalen Ebene unterstützt.</span><span class="sxs-lookup"><span data-stu-id="78671-669">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="78671-670">Verwenden Sie `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="78671-670">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="78671-671">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-671">**Why**</span></span>

<span data-ttu-id="78671-672">Diese Änderung wurde vorgenommen, um die API für Indizes mit `Include` an einer zentralen Stelle für alle Datenbankanbieter zusammenzuführen.</span><span class="sxs-lookup"><span data-stu-id="78671-672">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="78671-673">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-673">**Mitigations**</span></span>

<span data-ttu-id="78671-674">Verwenden Sie wie oben beschrieben die neue API.</span><span class="sxs-lookup"><span data-stu-id="78671-674">Use the new API, as shown above.</span></span>

### <a name="metadata-api-changes"></a><span data-ttu-id="78671-675">Metadaten-API-Änderungen</span><span class="sxs-lookup"><span data-stu-id="78671-675">Metadata API changes</span></span>

[<span data-ttu-id="78671-676">Issue #214</span><span class="sxs-lookup"><span data-stu-id="78671-676">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="78671-677">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-677">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="78671-678">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-678">**New behavior**</span></span>

<span data-ttu-id="78671-679">Die folgenden Eigenschaften wurden in Erweiterungsmethoden konvertiert:</span><span class="sxs-lookup"><span data-stu-id="78671-679">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="78671-680">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-680">**Why**</span></span>

<span data-ttu-id="78671-681">Mit dieser Änderung wird die Implementierung der oben genannten Schnittstellen vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="78671-681">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="78671-682">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-682">**Mitigations**</span></span>

<span data-ttu-id="78671-683">Verwenden Sie die neuen Erweiterungsmethoden.</span><span class="sxs-lookup"><span data-stu-id="78671-683">Use the new extension methods.</span></span>

<a name="provider"></a>

### <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="78671-684">Anbieterspezifische Metadaten-API-Änderungen</span><span class="sxs-lookup"><span data-stu-id="78671-684">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="78671-685">Issue #214</span><span class="sxs-lookup"><span data-stu-id="78671-685">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="78671-686">Diese Änderung wird in Vorschauversion 6 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-686">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="78671-687">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-687">**New behavior**</span></span>

<span data-ttu-id="78671-688">Die anbieterspezifischen Erweiterungsmethoden werden vereinfacht:</span><span class="sxs-lookup"><span data-stu-id="78671-688">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.IsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.UseIdentityColumn()`

<span data-ttu-id="78671-689">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-689">**Why**</span></span>

<span data-ttu-id="78671-690">Mit dieser Änderung wird die Implementierung der oben genannten Erweiterungsmethoden vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="78671-690">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="78671-691">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-691">**Mitigations**</span></span>

<span data-ttu-id="78671-692">Verwenden Sie die neuen Erweiterungsmethoden.</span><span class="sxs-lookup"><span data-stu-id="78671-692">Use the new extension methods.</span></span>

<a name="pragma"></a>

### <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="78671-693">EF Core sendet keine PRAGMA-Anweisungen mehr, um Fremdschlüssel in SQLite zu erzwingen</span><span class="sxs-lookup"><span data-stu-id="78671-693">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="78671-694">Issue #12151</span><span class="sxs-lookup"><span data-stu-id="78671-694">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="78671-695">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-695">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="78671-696">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-696">**Old behavior**</span></span>

<span data-ttu-id="78671-697">Vor Version 3.0 wurden in EF Core `PRAGMA foreign_keys = 1`-Anweisungen gesendet, wenn eine Verbindung mit SQLite hergestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="78671-697">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="78671-698">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-698">**New behavior**</span></span>

<span data-ttu-id="78671-699">Ab Version 3.0 werden in EF Core keine `PRAGMA foreign_keys = 1`-Anweisungen mehr gesendet, wenn eine Verbindung mit SQLite hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="78671-699">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="78671-700">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-700">**Why**</span></span>

<span data-ttu-id="78671-701">Diese Änderung wurde vorgenommen, da von EF Core standardmäßig `SQLitePCLRaw.bundle_e_sqlite3` verwendet wird. Dadurch ist die Erzwingung von Fremdschlüsseln standardmäßig aktiviert und muss nicht jedes Mal explizit aktiviert werden, wenn eine Verbindung hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="78671-701">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="78671-702">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-702">**Mitigations**</span></span>

<span data-ttu-id="78671-703">Fremdschlüssel sind in SQLitePCLRaw.bundle_e_sqlite3 standardmäßig aktiviert. Dieses Bundle wird standardmäßig von EF Core verwendet.</span><span class="sxs-lookup"><span data-stu-id="78671-703">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="78671-704">In anderen Fällen können Fremdschlüssel durch die Angabe `Foreign Keys=True` in der Verbindungszeichenfolge aktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="78671-704">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

<a name="sqlite3"></a>

### <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundle_e_sqlite3"></a><span data-ttu-id="78671-705">SQLitePCLRaw.bundle_e_sqlite3 ist nun eine Abhängigkeit von Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="78671-705">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="78671-706">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-706">**Old behavior**</span></span>

<span data-ttu-id="78671-707">Vor Version 3.0 wurde von EF Core `SQLitePCLRaw.bundle_green` verwendet.</span><span class="sxs-lookup"><span data-stu-id="78671-707">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="78671-708">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-708">**New behavior**</span></span>

<span data-ttu-id="78671-709">Ab Version 3.0 wird von EF Core `SQLitePCLRaw.bundle_e_sqlite3` verwendet.</span><span class="sxs-lookup"><span data-stu-id="78671-709">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="78671-710">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-710">**Why**</span></span>

<span data-ttu-id="78671-711">Diese Änderung wurde vorgenommen, damit die unter iOS verwendete Version von SQLite sich konsistent zu Versionen auf anderen Plattformen verhält.</span><span class="sxs-lookup"><span data-stu-id="78671-711">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="78671-712">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-712">**Mitigations**</span></span>

<span data-ttu-id="78671-713">Konfigurieren Sie `Microsoft.Data.Sqlite` so, dass ein anderes `SQLitePCLRaw`-Bundle verwendet wird, um die native SQLite-Version unter iOS zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="78671-713">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

<a name="guid"></a>

### <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="78671-714">GUID-Werte werden jetzt als TEXT in SQLite gespeichert</span><span class="sxs-lookup"><span data-stu-id="78671-714">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="78671-715">Issue #15078</span><span class="sxs-lookup"><span data-stu-id="78671-715">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="78671-716">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-716">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="78671-717">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-717">**Old behavior**</span></span>

<span data-ttu-id="78671-718">GUID-Werte wurden in der Vergangenheit in SQLite als BLOB-Werte gespeichert.</span><span class="sxs-lookup"><span data-stu-id="78671-718">Guid values were previously stored as BLOB values on SQLite.</span></span>

<span data-ttu-id="78671-719">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-719">**New behavior**</span></span>

<span data-ttu-id="78671-720">GUID-Werte werden jetzt als TEXT gespeichert.</span><span class="sxs-lookup"><span data-stu-id="78671-720">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="78671-721">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-721">**Why**</span></span>

<span data-ttu-id="78671-722">Das Binärformat der GUIDs ist nicht standardisiert.</span><span class="sxs-lookup"><span data-stu-id="78671-722">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="78671-723">Das Speichern der Werte als TEXT führt dazu, dass die Datenbank mit anderen Technologien eher kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="78671-723">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="78671-724">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-724">**Mitigations**</span></span>

<span data-ttu-id="78671-725">Sie können vorhandene Datenbanken zum neuen Format migrieren, indem Sie SQL-Code wie den folgenden ausführen:</span><span class="sxs-lookup"><span data-stu-id="78671-725">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="78671-726">Sie können in EF Core auch weiterhin das alte Verhalten verwenden, indem Sie einen Wertkonverter für diese Eigenschaften konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="78671-726">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="78671-727">Microsoft.Data.Sqlite kann weiterhin GUID-Werte aus BLOB- und TEXT-Spalten lesen. Da sich jedoch das Standardformat für Parameter und Konstanten geändert hat, müssen Sie wahrscheinlich Aktionen für die meisten GUID-Szenarios vornehmen.</span><span class="sxs-lookup"><span data-stu-id="78671-727">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

<a name="char"></a>

### <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="78671-728">Char-Werte werden jetzt als TEXT in SQLite gespeichert</span><span class="sxs-lookup"><span data-stu-id="78671-728">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="78671-729">Issue #15020</span><span class="sxs-lookup"><span data-stu-id="78671-729">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="78671-730">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-730">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="78671-731">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-731">**Old behavior**</span></span>

<span data-ttu-id="78671-732">Char-Werte wurden in der Vergangenheit in SQLite als INTEGER-Werte gespeichert.</span><span class="sxs-lookup"><span data-stu-id="78671-732">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="78671-733">Der Char-Wert *A* wurde z.B. als INTEGER-Wert 65 gespeichert.</span><span class="sxs-lookup"><span data-stu-id="78671-733">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="78671-734">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-734">**New behavior**</span></span>

<span data-ttu-id="78671-735">Char-Werte werden jetzt als TEXT gespeichert.</span><span class="sxs-lookup"><span data-stu-id="78671-735">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="78671-736">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-736">**Why**</span></span>

<span data-ttu-id="78671-737">Das Speichern der Werte als TEXT ist naheliegender und führt dazu, dass die Datenbank mit anderen Technologien eher kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="78671-737">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="78671-738">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-738">**Mitigations**</span></span>

<span data-ttu-id="78671-739">Sie können vorhandene Datenbanken zum neuen Format migrieren, indem Sie SQL-Code wie den folgenden ausführen:</span><span class="sxs-lookup"><span data-stu-id="78671-739">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="78671-740">Sie können in EF Core auch weiterhin das alte Verhalten verwenden, indem Sie einen Wertkonverter für diese Eigenschaften konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="78671-740">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="78671-741">Microsoft.Data.Sqlite kann auch weiterhin Zeichenwerte aus INTEGER- und TEXT-Spalten lesen, weshalb es sein kann, dass in einigen Szenarios keine weiteren Maßnahmen erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="78671-741">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

<a name="migid"></a>

### <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="78671-742">Migrations-IDs werden jetzt mit dem Kalender der invarianten Kultur generiert</span><span class="sxs-lookup"><span data-stu-id="78671-742">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="78671-743">Issue #12978</span><span class="sxs-lookup"><span data-stu-id="78671-743">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="78671-744">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-744">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="78671-745">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-745">**Old behavior**</span></span>

<span data-ttu-id="78671-746">Migrations-IDs wurden versehentlich mit dem Kalender der aktuellen Kultur generiert.</span><span class="sxs-lookup"><span data-stu-id="78671-746">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="78671-747">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-747">**New behavior**</span></span>

<span data-ttu-id="78671-748">Migrations-IDs werden jetzt immer mit dem Kalender der invarianten Kultur generiert (gregorianischer Kalender).</span><span class="sxs-lookup"><span data-stu-id="78671-748">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="78671-749">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-749">**Why**</span></span>

<span data-ttu-id="78671-750">Die Reihenfolge der Migrationen ist beim Aktualisieren einer Datenbank oder Auflösen von Mergekonflikten wesentlich.</span><span class="sxs-lookup"><span data-stu-id="78671-750">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="78671-751">Wenn der invariante Kalender verwendet wird, werden Probleme bei der Reihenfolge vermieden, die entstehen, wenn Teammitglieder unterschiedliche Systemkalender verwenden.</span><span class="sxs-lookup"><span data-stu-id="78671-751">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="78671-752">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-752">**Mitigations**</span></span>

<span data-ttu-id="78671-753">Diese Änderung betrifft jeden Benutzer, der einen Kalender verwendet, der nicht gregorianisch ist und bei dem die Jahreszahl höher als die des gregorianischen Kalenders ist (wie z.B. in der buddhistischen Zeitrechnung).</span><span class="sxs-lookup"><span data-stu-id="78671-753">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="78671-754">Vorhandene Migrations-IDs müssen aktualisiert werden, damit neue Migrationen in der Reihenfolge hinter vorhandenen Migrationen eingeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="78671-754">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="78671-755">Sie können die Migrations-ID im Migration-Attribut in der Designerdatei der Migration finden.</span><span class="sxs-lookup"><span data-stu-id="78671-755">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="78671-756">Die Tabelle mit dem Migrationsverlauf muss auch aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="78671-756">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

<a name="urn"></a>

### <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="78671-757">UseRowNumberForPaging wurde entfernt.</span><span class="sxs-lookup"><span data-stu-id="78671-757">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="78671-758">Issue #16400</span><span class="sxs-lookup"><span data-stu-id="78671-758">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="78671-759">Diese Änderung wird in Vorschauversion 6 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-759">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="78671-760">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-760">**Old behavior**</span></span>

<span data-ttu-id="78671-761">Vor EF Core 3.0 konnte `UseRowNumberForPaging` zum Generieren von SQL-Code für die Paginierung verwendet werden, der mit SQL Server 2008 kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="78671-761">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="78671-762">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-762">**New behavior**</span></span>

<span data-ttu-id="78671-763">Ab EF Core 3.0 generiert EF nur noch SQL-Code für die Paginierung, die mit höheren SQL Server-Versionen kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="78671-763">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="78671-764">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-764">**Why**</span></span>

<span data-ttu-id="78671-765">Wir führen diese Änderung ein, weil [SQL Server 2008 kein unterstütztes Produkt mehr ist](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) und weil die Aktualisierung dieses Features auf die in EF Core 3.0 vorgenommenen Änderungen an Abfragen ein wichtiger Schritt ist.</span><span class="sxs-lookup"><span data-stu-id="78671-765">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="78671-766">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-766">**Mitigations**</span></span>

<span data-ttu-id="78671-767">Es empfiehlt sich, eine Update auf eine neuere Version von SQL Server durchzuführen oder einen höheren Kompatibilitätsgrad zu verwenden, damit der generierte SQL-Code unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="78671-767">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="78671-768">Wenn dies bei Ihnen nicht möglich ist, fügen Sie einen [Kommentar zum Issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) ein, und geben Sie alle relevanten Informationen an.</span><span class="sxs-lookup"><span data-stu-id="78671-768">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="78671-769">Je nach Feedback der Benutzer wird diese Entscheidung möglicherweise noch einmal überprüft.</span><span class="sxs-lookup"><span data-stu-id="78671-769">We may revisit this decision based on feedback.</span></span>

<a name="xinfo"></a>

### <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="78671-770">Erweiterungsinformationen und Metadaten aus IDbContextOptionsExtension entfernt</span><span class="sxs-lookup"><span data-stu-id="78671-770">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="78671-771">Issue #16119</span><span class="sxs-lookup"><span data-stu-id="78671-771">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="78671-772">Diese Änderung wird in Vorschauversion 7 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-772">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="78671-773">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-773">**Old behavior**</span></span>

<span data-ttu-id="78671-774">`IDbContextOptionsExtension` enthielt Methoden zum Bereitstellen von Metadaten zur Erweiterung.</span><span class="sxs-lookup"><span data-stu-id="78671-774">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="78671-775">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-775">**New behavior**</span></span>

<span data-ttu-id="78671-776">Diese Methoden wurden in eine neue abstrakte `DbContextOptionsExtensionInfo`-Basisklasse verschoben, die von einer neuen `IDbContextOptionsExtension.Info`-Eigenschaft zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="78671-776">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="78671-777">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-777">**Why**</span></span>

<span data-ttu-id="78671-778">In den Releases zwischen 2.0 und 3.0 mussten wir diese Methoden mehrmals ergänzen oder ändern.</span><span class="sxs-lookup"><span data-stu-id="78671-778">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="78671-779">Durch die Bereitstellung dieser Methoden in einer neuen abstrakten Basisklasse lassen sich solche Änderungen einfacher einführen, ohne dass es zu Konflikten mit vorhandenen Erweiterungen kommt.</span><span class="sxs-lookup"><span data-stu-id="78671-779">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="78671-780">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-780">**Mitigations**</span></span>

<span data-ttu-id="78671-781">Erweiterungen wurden aktualisiert und verwenden das neue Muster.</span><span class="sxs-lookup"><span data-stu-id="78671-781">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="78671-782">Beispiele finden sich in den vielen Implementierungen von `IDbContextOptionsExtension` für verschiedene Arten von Erweiterungen im EF Core-Quellcode.</span><span class="sxs-lookup"><span data-stu-id="78671-782">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

<a name="lqpe"></a>

### <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="78671-783">LogQueryPossibleExceptionWithAggregateOperator wurde umbenannt</span><span class="sxs-lookup"><span data-stu-id="78671-783">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="78671-784">Issue #10985</span><span class="sxs-lookup"><span data-stu-id="78671-784">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="78671-785">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-785">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="78671-786">**Änderung**</span><span class="sxs-lookup"><span data-stu-id="78671-786">**Change**</span></span>

<span data-ttu-id="78671-787">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` wurde umbenannt in `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="78671-787">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="78671-788">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-788">**Why**</span></span>

<span data-ttu-id="78671-789">Die Benennung dieses Warnereignisses wurde an allen anderen Warnereignissen ausgerichtet.</span><span class="sxs-lookup"><span data-stu-id="78671-789">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="78671-790">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-790">**Mitigations**</span></span>

<span data-ttu-id="78671-791">Verwenden Sie den neuen Namen.</span><span class="sxs-lookup"><span data-stu-id="78671-791">Use the new name.</span></span> <span data-ttu-id="78671-792">(Beachten Sie, dass sich die Ereignis-ID-Nummer nicht geändert hat.)</span><span class="sxs-lookup"><span data-stu-id="78671-792">(Note that the event ID number has not changed.)</span></span>

<a name="clarify"></a>

### <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="78671-793">Verdeutlichen der API für Einschränkungsnamen von Fremdschlüsseln</span><span class="sxs-lookup"><span data-stu-id="78671-793">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="78671-794">Issue #10730</span><span class="sxs-lookup"><span data-stu-id="78671-794">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="78671-795">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-795">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="78671-796">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-796">**Old behavior**</span></span>

<span data-ttu-id="78671-797">Vor EF Core 3.0 wurden Einschränkungsnamen von Fremdschlüsseln einfach als „Namen“ bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="78671-797">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="78671-798">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="78671-798">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="78671-799">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-799">**New behavior**</span></span>

<span data-ttu-id="78671-800">Ab EF Core 3.0 werden Einschränkungsnamen von Fremdschlüsseln nun als „Einschränkungsnamen“ bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="78671-800">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="78671-801">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="78671-801">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="78671-802">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-802">**Why**</span></span>

<span data-ttu-id="78671-803">Durch diese Änderung wird Konsistenz für Benennungen in diesem Bereich gewährleistet, und es wird verdeutlicht, dass es sich dabei um den Namen der Fremdschlüsseleinschränkung handelt und nicht um den Spalten- oder Eigenschaftennamen, auf deren Grundlage der Fremdschlüssel definiert ist.</span><span class="sxs-lookup"><span data-stu-id="78671-803">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="78671-804">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-804">**Mitigations**</span></span>

<span data-ttu-id="78671-805">Verwenden Sie den neuen Namen.</span><span class="sxs-lookup"><span data-stu-id="78671-805">Use the new name.</span></span>

<a name="irdc2"></a>

### <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="78671-806">„IRelationalDatabaseCreator.HasTables/HasTablesAsync“ ist jetzt öffentlich</span><span class="sxs-lookup"><span data-stu-id="78671-806">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="78671-807">Issue #15997</span><span class="sxs-lookup"><span data-stu-id="78671-807">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="78671-808">Diese Änderung wird in Vorschauversion 7 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-808">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="78671-809">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-809">**Old behavior**</span></span>

<span data-ttu-id="78671-810">Vor EF Core 3.0 waren diese Methoden geschützt.</span><span class="sxs-lookup"><span data-stu-id="78671-810">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="78671-811">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-811">**New behavior**</span></span>

<span data-ttu-id="78671-812">Ab EF Core 3.0 sind diese Methoden öffentlich.</span><span class="sxs-lookup"><span data-stu-id="78671-812">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="78671-813">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-813">**Why**</span></span>

<span data-ttu-id="78671-814">Diese Methoden werden von EF verwendet, um zu ermitteln, ob eine Datenbank erstellt wurde, aber leer ist.</span><span class="sxs-lookup"><span data-stu-id="78671-814">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="78671-815">Dies kann auch außerhalb von EF nützlich sein, um zu ermitteln, ob Migrationen angewendet werden sollen oder nicht.</span><span class="sxs-lookup"><span data-stu-id="78671-815">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="78671-816">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-816">**Mitigations**</span></span>

<span data-ttu-id="78671-817">Der Zugriff auf Überschreibungen wurde geändert.</span><span class="sxs-lookup"><span data-stu-id="78671-817">Change the accessibility of any overrides.</span></span>

<a name="dip"></a>

### <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="78671-818">Microsoft.EntityFrameworkCore.Design ist jetzt ein DevelopmentDependency-Paket</span><span class="sxs-lookup"><span data-stu-id="78671-818">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="78671-819">Issue #11506</span><span class="sxs-lookup"><span data-stu-id="78671-819">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="78671-820">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-820">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="78671-821">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-821">**Old behavior**</span></span>

<span data-ttu-id="78671-822">Vor EF Core 3.0 war Microsoft.EntityFrameworkCore.Design ein reguläres NuGet-Paket, auf dessen Assembly von Projekten verwiesen werden kann, die davon abhängig sind.</span><span class="sxs-lookup"><span data-stu-id="78671-822">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="78671-823">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-823">**New behavior**</span></span>

<span data-ttu-id="78671-824">Ab EF Core 3.0 ist es ein DevelopmentDependency-Paket.</span><span class="sxs-lookup"><span data-stu-id="78671-824">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="78671-825">Das bedeutet, dass die Abhängigkeit nicht transitiv auf andere Projekte übertragen wird und Sie nicht mehr standardmäßig auf die Assembly verweisen können.</span><span class="sxs-lookup"><span data-stu-id="78671-825">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="78671-826">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-826">**Why**</span></span>

<span data-ttu-id="78671-827">Dieses Paket ist nur für die Verwendung zur Entwurfszeit konzipiert.</span><span class="sxs-lookup"><span data-stu-id="78671-827">This package is only intended to be used at design time.</span></span> <span data-ttu-id="78671-828">Bereitgestellte Anwendungen sollten nicht darauf verweisen.</span><span class="sxs-lookup"><span data-stu-id="78671-828">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="78671-829">Die Kennzeichnung des Pakets als DevelopmentDependency unterstreicht diese Empfehlung.</span><span class="sxs-lookup"><span data-stu-id="78671-829">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="78671-830">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-830">**Mitigations**</span></span>

<span data-ttu-id="78671-831">Wenn Sie auf dieses Paket verweisen müssen, um das Verhalten von EF Core zur Entwurfszeit zu überschreiben, können Sie die Metadaten des PackageReference-Elements in Ihrem Projekt aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="78671-831">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="78671-832">Wenn transitiv über via Microsoft.EntityFrameworkCore.Tools auf das Paket verwiesen wird, müssen Sie dem Paket eine explizite PackageReference hinzufügen, um seine Metadaten zu ändern.</span><span class="sxs-lookup"><span data-stu-id="78671-832">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<a name="SQLitePCL"></a>

### <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="78671-833">SQLitePCL.raw zu Version 2.0.0 aktualisiert</span><span class="sxs-lookup"><span data-stu-id="78671-833">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="78671-834">Issue #14824</span><span class="sxs-lookup"><span data-stu-id="78671-834">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="78671-835">Diese Änderung wird in Vorschauversion 7 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-835">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="78671-836">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-836">**Old behavior**</span></span>

<span data-ttu-id="78671-837">Microsoft.EntityFrameworkCore.Sqlite war früher von Version 1.1.12 von SQLitePCL.raw abhängig.</span><span class="sxs-lookup"><span data-stu-id="78671-837">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="78671-838">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-838">**New behavior**</span></span>

<span data-ttu-id="78671-839">Wir haben unser Paket aktualisiert, so dass es jetzt von Version 2.0.0 abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="78671-839">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="78671-840">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-840">**Why**</span></span>

<span data-ttu-id="78671-841">Version 2.0.0 von SQLitePCL.raw zielt auf .NET Standard 2.0 ab.</span><span class="sxs-lookup"><span data-stu-id="78671-841">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="78671-842">Zuvor war .NET Standard 1.1 das Ziel, hierfür war für ein umfassender Funktionsabschluss von transitiven Paketen erforderlich.</span><span class="sxs-lookup"><span data-stu-id="78671-842">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="78671-843">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-843">**Mitigations**</span></span>

<span data-ttu-id="78671-844">SQLitePCL.raw, Version 2.0.0, enthält einige Breaking Changes.</span><span class="sxs-lookup"><span data-stu-id="78671-844">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="78671-845">Weitere Informationen finden Sie in den [Versionshinweisen](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md).</span><span class="sxs-lookup"><span data-stu-id="78671-845">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>

<a name="NetTopologySuite"></a>

### <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="78671-846">NetTopologySuite auf Version 2.0.0 aktualisiert</span><span class="sxs-lookup"><span data-stu-id="78671-846">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="78671-847">Issue #14825</span><span class="sxs-lookup"><span data-stu-id="78671-847">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="78671-848">Diese Änderung wird in Vorschauversion 7 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-848">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="78671-849">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-849">**Old behavior**</span></span>

<span data-ttu-id="78671-850">Die Pakete mit räumlichen Daten waren bisher von Version 1.15.1 der NetTopologySuite abhängig.</span><span class="sxs-lookup"><span data-stu-id="78671-850">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="78671-851">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-851">**New behavior**</span></span>

<span data-ttu-id="78671-852">Wir haben unser Paket aktualisiert, so dass es jetzt von Version 2.0.0 abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="78671-852">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="78671-853">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-853">**Why**</span></span>

<span data-ttu-id="78671-854">Version 2.0.0 der NetTopologySuite behebt verschiedene Verwendungsprobleme, die von EF Core-Benutzern gemeldet wurden.</span><span class="sxs-lookup"><span data-stu-id="78671-854">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="78671-855">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-855">**Mitigations**</span></span>

<span data-ttu-id="78671-856">NetTopologySuite 2.0.0 umfasst einige Breaking Changes.</span><span class="sxs-lookup"><span data-stu-id="78671-856">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="78671-857">Weitere Informationen finden Sie in den [Versionshinweisen](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001).</span><span class="sxs-lookup"><span data-stu-id="78671-857">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>

<a name="mersa"></a>

### <a name="multiple-ambiguous-self-referencing-relationships-must-be-configured"></a><span data-ttu-id="78671-858">Beziehungen mit mehreren mehrdeutigen Selbstverweisen müssen nun konfiguriert werden</span><span class="sxs-lookup"><span data-stu-id="78671-858">Multiple ambiguous self-referencing relationships must be configured</span></span> 

[<span data-ttu-id="78671-859">Issue #13573</span><span class="sxs-lookup"><span data-stu-id="78671-859">Tracking Issue #13573</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13573)

<span data-ttu-id="78671-860">Diese Änderung wird in Vorschauversion 6 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="78671-860">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="78671-861">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-861">**Old behavior**</span></span>

<span data-ttu-id="78671-862">Ein Entitätstyp, der über unidirektionale Navigationseigenschaften mit mehreren Selbstverweisen und über übereinstimmende Fremdschlüssel verfügte, wurde bislang fälschlicherweise als einfache Beziehung konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="78671-862">An entity type with multiple self-referencing uni-directional navigation properties and matching FKs was incorrectly configured as a single relationship.</span></span> <span data-ttu-id="78671-863">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="78671-863">For example:</span></span>

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

<span data-ttu-id="78671-864">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="78671-864">**New behavior**</span></span>

<span data-ttu-id="78671-865">Dieses Szenario wird nun beim Erstellen von Modellen erkannt. Außerdem wird eine Ausnahme ausgelöst, in der darauf hingewiesen wird, dass das Modell mehrdeutig ist.</span><span class="sxs-lookup"><span data-stu-id="78671-865">This scenario is now detected in model building and an exception is thrown indicating that the model is ambiguous.</span></span>

<span data-ttu-id="78671-866">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="78671-866">**Why**</span></span>

<span data-ttu-id="78671-867">Das resultierende Modell war mehrdeutig und führte in diesem Fall üblicherweise zu falschen Ergebnissen.</span><span class="sxs-lookup"><span data-stu-id="78671-867">The resultant model was ambiguous and will likely usually be wrong for this case.</span></span>

<span data-ttu-id="78671-868">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="78671-868">**Mitigations**</span></span>

<span data-ttu-id="78671-869">Konfigurieren Sie die Beziehung vollständig.</span><span class="sxs-lookup"><span data-stu-id="78671-869">Use full configuration of the relationship.</span></span> <span data-ttu-id="78671-870">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="78671-870">For example:</span></span>

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
