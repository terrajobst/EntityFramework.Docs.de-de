---
title: Breaking Changes in EF Core 3.0
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: a7e1a03bf1131cd53123f5cc39b07bed94619b44
ms.sourcegitcommit: a013e243a14f384999ceccaf9c779b8c1ae3b936
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57463370"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="0ccf3-102">Breaking Changes in EF Core 3.0 (aktuell in der Vorschauversion)</span><span class="sxs-lookup"><span data-stu-id="0ccf3-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0ccf3-103">Bitte beachten Sie, dass die Featuregruppen und Zeitpläne für künftige Releases jederzeit geändert werden können. Obwohl diese Seite bestmöglich aktualisiert wird, entspricht sie nicht immer den neuesten Plänen.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="0ccf3-104">Die folgenden API-Änderungen und Behavior Changes können dazu führen, dass Anwendungen, die für EF Core 2.2.x entwickelt wurden, beim Upgrade auf 3.0.0 nicht mehr funktionieren.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="0ccf3-105">Änderungen, die sich voraussichtlich nur auf Datenbankanbieter auswirken, sind unter [Änderungen mit Auswirkungen auf den Anbieter](../../providers/provider-log.md) dokumentiert.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="0ccf3-106">Werden Breaking Changes zwischen zwei Vorschauversionen von EF Core 3.0 in neuen Features eingeführt, werden diese hier nicht dokumentiert.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="0ccf3-107">LINQ-Abfragen werden nicht mehr auf dem Client ausgewertet</span><span class="sxs-lookup"><span data-stu-id="0ccf3-107">LINQ queries are no longer evaluated on the client</span></span>

[<span data-ttu-id="0ccf3-108">Issue #12795</span><span class="sxs-lookup"><span data-stu-id="0ccf3-108">Tracking Issue #12795</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

> [!IMPORTANT]
> <span data-ttu-id="0ccf3-109">Dieser Breaking Change wird bereits vorab angekündigt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-109">We are pre-announcing this break.</span></span>
<span data-ttu-id="0ccf3-110">Bisher ist er in keiner Vorschauversion von EF Core 3.0 implementiert.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-110">It has not yet shipped in any 3.0 preview.</span></span>

<span data-ttu-id="0ccf3-111">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-111">**Old behavior**</span></span>

<span data-ttu-id="0ccf3-112">Vor Version 3.0 wurden in EF Core automatisch Ausdrücke auf dem Client ausgewertet, wenn diese Teil einer Abfrage waren und nicht in SQL oder in einen Parameter konvertiert werden konnten.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-112">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="0ccf3-113">Wenn potenziell aufwendige Ausdrücke auf dem Client ausgewertet werden sollten, wurde standardmäßig nur eine Warnung ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-113">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="0ccf3-114">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-114">**New behavior**</span></span>

<span data-ttu-id="0ccf3-115">Ab Version 3.0 können in EF Core nur Ausdrücke der obersten Projektion (dem letzten `Select()`-Aufruf in der Abfrage) auf dem Client ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-115">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="0ccf3-116">Wenn Ausdrücke in einem anderen Teil der Abfrage nicht in SQL oder in einen Parameter konvertiert werden können, wird eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-116">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="0ccf3-117">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-117">**Why**</span></span>

<span data-ttu-id="0ccf3-118">Die automatische Auswertung von Abfragen auf Clients hat zur Folge, dass viele Abfragen auch dann ausgeführt werden, wenn wichtige Teile nicht übersetzt werden können.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-118">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="0ccf3-119">Dieses Verhalten kann zu unerwartetem und potenziell schädlichem Verhalten führen, das möglicherweise erst in der Produktionsphase erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-119">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="0ccf3-120">Eine nicht übersetzbare Bedingung in einem `Where()`-Aufruf kann sich beispielsweise so auswirken, dass alle Zeilen einer Tabelle vom Datenbankserver übertragen werden und der Filter auf dem Client angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-120">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="0ccf3-121">Wenn die Tabelle in der Entwicklungsphase nur wenige Zeilen enthält, ist es gut möglich, dass diese Situation unerkannt bleibt. Wenn die Anwendung jedoch in die Produktionsumgebung überführt wird, enthält die Tabelle möglicherweise Millionen von Zeilen, was zu schwerwiegenden Konsequenzen führen kann.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-121">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="0ccf3-122">Wie die Erfahrung in der Entwicklungsphase gezeigt hat, können auch Warnungen, die bei Auswertungen auf dem Client ausgelöst werden, dieses Problem nicht lösen, da sie sich leicht ignorieren lassen.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-122">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="0ccf3-123">Die automatische Auswertung auf Clients kann darüber hinaus zu Problemen führen, bei denen die Verbesserung der Abfrageübersetzung für bestimmte Ausdrücke zu unbeabsichtigten Breaking Changes zwischen Releases führt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-123">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="0ccf3-124">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-124">**Mitigations**</span></span>

<span data-ttu-id="0ccf3-125">Wenn sich eine Abfrage nicht vollständig übersetzen lässt, habe Sie zwei Möglichkeiten: Schreiben Sie sie entweder um, oder setzen Sie alternativ `AsEnumerable()`, `ToList()` oder ähnliche Methoden ein, um Daten wieder zurück an den Client zu übertragen, wo sie mit LINQ to Objects weiterverarbeitet werden können.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-125">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="0ccf3-126">Entity Framework Core ist nicht mehr Teil des gemeinsam verwendeten ASP.NET Core-Frameworks</span><span class="sxs-lookup"><span data-stu-id="0ccf3-126">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="0ccf3-127">Issueankündigungen #325</span><span class="sxs-lookup"><span data-stu-id="0ccf3-127">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="0ccf3-128">Diese Änderung wurde in Vorschauversion 1 von ASP.NET Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-128">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="0ccf3-129">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-129">**Old behavior**</span></span>

<span data-ttu-id="0ccf3-130">Vor Version 3.0 von ASP.NET Core wurden EF Core und einige der zugehörigen Datenanbieter wie SQL Server eingeschlossen, wenn `Microsoft.AspNetCore.App` oder `Microsoft.AspNetCore.All` ein Paketverweis hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-130">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="0ccf3-131">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-131">**New behavior**</span></span>

<span data-ttu-id="0ccf3-132">Ab Version 3.0 enthält das gemeinsam verwendete ASP.NET Core-Framework weder EF Core noch zugehörige Datenanbieter.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-132">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="0ccf3-133">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-133">**Why**</span></span>

<span data-ttu-id="0ccf3-134">Vor dieser Änderung konnte EF Core auf unterschiedliche Arten bezogen werden. Diese richteten sich danach, ob ASP.NET Core und SQL Server oder ein anderes Zielframework für die Anwendung vorgesehen war.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-134">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="0ccf3-135">Bei einem Upgrade von ASP.NET Core wurde zudem ein Upgrade von EF Core und des SQL Server-Anbieters erzwungen, was nicht in allen Fällen gewünscht war.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-135">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="0ccf3-136">Durch die eingeführte Änderung wird EF Core unter allen Anbietern, unterstützten .NET-Implementierungen und Anwendungstypen auf dieselbe Weise bezogen.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-136">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="0ccf3-137">Entwickler können nun außerdem genau festlegen, wann für EF Core und zugehörige Datenanbieter ein Upgrade durchgeführt werden soll.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-137">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="0ccf3-138">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-138">**Mitigations**</span></span>

<span data-ttu-id="0ccf3-139">Wenn Sie EF Core in einer Anwendung unter ASP.NET Core 3.0 oder in einer anderen unterstützten Anwendung verwenden möchten, müssen Sie dem EF Core-Datenbankanbieter, der für die Anwendung genutzt werden soll, explizit einen Paketverweis hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-139">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="0ccf3-140">Die Ausführung der Abfrage wird auf der Debugebene protokolliert</span><span class="sxs-lookup"><span data-stu-id="0ccf3-140">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="0ccf3-141">Issue #14523</span><span class="sxs-lookup"><span data-stu-id="0ccf3-141">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="0ccf3-142">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-142">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="0ccf3-143">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-143">**Old behavior**</span></span>

<span data-ttu-id="0ccf3-144">Vor Version 3.0 wurde in EF Core die Ausführung von Abfragen und anderen Befehlen auf der `Info`-Ebene protokolliert.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-144">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

<span data-ttu-id="0ccf3-145">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-145">**New behavior**</span></span>

<span data-ttu-id="0ccf3-146">Ab Version 3.0 wird in EF Core die Ausführung von Befehlen und SQL-Abfragen auf der `Debug`-Ebene protokolliert.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-146">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

<span data-ttu-id="0ccf3-147">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-147">**Why**</span></span>

<span data-ttu-id="0ccf3-148">Diese Änderung wurde vorgenommen, damit auf der `Info`-Ebene nur die wichtigsten Daten erfasst werden.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-148">This change was made to reduce the noise at the `Info` log level.</span></span>

<span data-ttu-id="0ccf3-149">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-149">**Mitigations**</span></span>

<span data-ttu-id="0ccf3-150">Dieses Protokollereignis wird durch `RelationalEventId.CommandExecuting` mit der Ereignis-ID 20100 definiert.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-150">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="0ccf3-151">Wenn Sie SQL-Abfragen wieder auf der `Info`-Ebene protokollieren möchten, aktivieren Sie die Protokollierung auf der `Debug`-Ebene, und filtern Sie nur nach diesem Ereignis.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-151">To log SQL at the `Info` level again, switch on logging at the `Debug` level and filter to just this event.</span></span>


## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="0ccf3-152">Temporäre Schlüsselwerte werden nicht mehr Entitätsinstanzen zugewiesen</span><span class="sxs-lookup"><span data-stu-id="0ccf3-152">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="0ccf3-153">Issue #12378</span><span class="sxs-lookup"><span data-stu-id="0ccf3-153">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="0ccf3-154">Diese Änderung wurde in Vorschauversion 2 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-154">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="0ccf3-155">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-155">**Old behavior**</span></span>

<span data-ttu-id="0ccf3-156">Vor Version 3.0 wurden in EF Core allen Schlüsseleigenschaften temporäre Werte zugewiesen. Später wurden den Eigenschaften dann die tatsächlichen Werte zugewiesen, die von der Datenbank generiert wurden.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-156">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="0ccf3-157">Die temporären Werte waren in der Regel große negative Zahlen.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-157">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="0ccf3-158">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-158">**New behavior**</span></span>

<span data-ttu-id="0ccf3-159">Ab Version 3.0 wird in EF Core der temporäre Wert als Teil der Überwachungsinformationen der Entität gespeichert. Die Schlüsseleigenschaft selbst bleibt unverändert.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-159">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="0ccf3-160">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-160">**Why**</span></span>

<span data-ttu-id="0ccf3-161">Diese Änderung wurde vorgenommen, damit temporäre Schlüsselwerte nicht fälschlicherweise dauerhaft gespeichert werden, wenn eine Entität, die vorher von einer `DbContext`-Instanz überwacht wurde, in eine andere `DbContext`-Instanz verschoben wird.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-161">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="0ccf3-162">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-162">**Mitigations**</span></span>

<span data-ttu-id="0ccf3-163">Anwendungen, die Primärschlüsselwerte Fremdschlüsseln zuweisen, um Zuordnungen zwischen Entitäten zu erstellen, können vom alten Verhalten abhängig sein, wenn Primärschlüssel vom Speicher generiert werden und zu Entitäten im `Added`-Zustand gehören.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-163">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="0ccf3-164">Sie können dies wie folgt vermeiden:</span><span class="sxs-lookup"><span data-stu-id="0ccf3-164">This can be avoided by:</span></span>
* <span data-ttu-id="0ccf3-165">Verwenden Sie keine vom Speicher generierten Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-165">Not using store-generated keys.</span></span>
* <span data-ttu-id="0ccf3-166">Legen Sie zum Erstellen von Zuordnungen keine Fremdschlüsselwerte, sondern Navigationseigenschaften fest.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-166">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="0ccf3-167">Rufen Sie die tatsächlichen temporären Schlüsselwerte aus den Überwachungsinformationen der Entität ab.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-167">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="0ccf3-168">`context.Entry(blog).Property(e => e.Id).CurrentValue` gibt beispielsweise den temporären Wert zurück, obwohl `blog.Id` noch nicht festgelegt wurde.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-168">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="0ccf3-169">DetectChanges berücksichtigt vom Speicher generierte Schlüsselwerte</span><span class="sxs-lookup"><span data-stu-id="0ccf3-169">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="0ccf3-170">Issue #14616</span><span class="sxs-lookup"><span data-stu-id="0ccf3-170">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="0ccf3-171">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-171">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="0ccf3-172">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-172">**Old behavior**</span></span>

<span data-ttu-id="0ccf3-173">Vor Version 3.0 wurde in EF Core eine nicht überwachte Entität, die von `DetectChanges` gefunden wurde, im `Added`-Zustand überwacht und als neue Zeile eingefügt, sobald `SaveChanges` aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-173">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="0ccf3-174">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-174">**New behavior**</span></span>

<span data-ttu-id="0ccf3-175">Ab Version 3.0 wird in EF Core die Entität im `Modified`-Zustand überwacht, wenn für diese generierte Schlüsselwerte verwendet werden und ein Schlüsselwert festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-175">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="0ccf3-176">Es wird also davon ausgegangen, dass eine Zeile für die Entität vorhanden ist. Wenn `SaveChanges` aufgerufen wird, wird die Zeile außerdem aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-176">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="0ccf3-177">Falls kein Schlüsselwert festgelegt ist oder der Entitätstyp keine generierten Schlüssel verwendet, wird die neue Entität wie in früheren Version weiterhin im `Added`-Zustand überwacht.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-177">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="0ccf3-178">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-178">**Why**</span></span>

<span data-ttu-id="0ccf3-179">Diese Änderung wurde vorgenommen, um bei der Verwendung von speichergenerierten Schlüsseln die Arbeit mit nicht verbundenen Entitätsgraphen einfacher und konsistenter zu gestalten.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-179">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="0ccf3-180">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-180">**Mitigations**</span></span>

<span data-ttu-id="0ccf3-181">Diese Änderung kann dazu führen, dass eine Anwendung nicht mehr funktioniert. Dieser Fall tritt ein, wenn für Entitätstypen generierte Schlüssel vorgesehen sind, dann jedoch explizit Schlüsselwerte für neue Instanzen festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-181">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="0ccf3-182">Sie können dieses Problem vermeiden, indem Sie explizit festlegen, dass für Schlüsseleigenschaften keine generierten Werte verwendet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-182">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="0ccf3-183">Dies ist beispielsweise mit der Fluent-API möglich:</span><span class="sxs-lookup"><span data-stu-id="0ccf3-183">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="0ccf3-184">Alternativ bieten sich auch Datenanmerkungen an:</span><span class="sxs-lookup"><span data-stu-id="0ccf3-184">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="0ccf3-185">Kaskadierende Deletes werden standardmäßig sofort ausgeführt</span><span class="sxs-lookup"><span data-stu-id="0ccf3-185">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="0ccf3-186">Issue #10114</span><span class="sxs-lookup"><span data-stu-id="0ccf3-186">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="0ccf3-187">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-187">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="0ccf3-188">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-188">**Old behavior**</span></span>

<span data-ttu-id="0ccf3-189">Vor Version 3.0 wurden in EF Core kaskadierenden Aktionen (Löschen von abhängigen Entitäten nach dem Löschen eines erforderlichen Prinzipals oder nach dem Aufheben einer Beziehung zum erforderlichen Prinzipal) erst ausgeführt, wenn SaveChanges aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-189">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="0ccf3-190">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-190">**New behavior**</span></span>

<span data-ttu-id="0ccf3-191">Ab Version 3.0 werden in EF Core kaskadierende Aktionen angewendet, sobald die auslösende Bedingung erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-191">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="0ccf3-192">Wenn beispielsweise `context.Remove()` zum Löschen einer Prinzipalentität aufgerufen wird, führt das dazu, dass auch alle zugehörigen erforderlichen Entitäten, die überwacht werden, sofort auf `Deleted` festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-192">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="0ccf3-193">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-193">**Why**</span></span>

<span data-ttu-id="0ccf3-194">Diese Änderung wurde vorgenommen, um die Arbeit mit Datenbindungen und Überwachungsszenarios zu vereinfachen, in denen bekannt sein muss, welche Entitäten _vor_ dem Aufruf von `SaveChanges` gelöscht werden.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-194">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="0ccf3-195">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-195">**Mitigations**</span></span>

<span data-ttu-id="0ccf3-196">Sie können das vorherige Verhalten wiederherstellen, indem Sie für `context.ChangedTracker` die entsprechenden Einstellungen vornehmen.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-196">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="0ccf3-197">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0ccf3-197">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="0ccf3-198">Abfragetypen werden mit Entitätstypen zusammengeführt</span><span class="sxs-lookup"><span data-stu-id="0ccf3-198">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="0ccf3-199">Issue #14194</span><span class="sxs-lookup"><span data-stu-id="0ccf3-199">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="0ccf3-200">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-200">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="0ccf3-201">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-201">**Old behavior**</span></span>

<span data-ttu-id="0ccf3-202">Vor Version 3.0 wurden in EF Core [Abfragetypen](xref:core/modeling/query-types) dazu verwendet, Daten abzufragen, in denen kein Primärschlüssel auf strukturierte Weise festgelegt war.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-202">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="0ccf3-203">Abfragetypen wurden also für die Zuordnung von Entitätstypen ohne Schlüssel verwendet (in der Regel in einer Sicht, in einigen Fällen aber auch in einer Tabelle), während reguläre Entitätstypen für einen verfügbaren Schlüssel verwendet wurden (in der Regel in einer Tabelle, in einigen Fällen aber auch in einer Sicht).</span><span class="sxs-lookup"><span data-stu-id="0ccf3-203">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="0ccf3-204">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-204">**New behavior**</span></span>

<span data-ttu-id="0ccf3-205">Aus einem Abfragetyp wird nun einfach ein Entitätstyp ohne Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-205">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="0ccf3-206">Entitätstypen ohne Schlüssel besitzen die gleiche Funktionalität wie Abfragetypen in früheren Versionen.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-206">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="0ccf3-207">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-207">**Why**</span></span>

<span data-ttu-id="0ccf3-208">Diese Änderung wurde vorgenommen, um Unklarheiten bei der Verwendung von Abfragetypen zu beseitigen.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-208">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="0ccf3-209">Abfragetypen sind schlüssellose Entitätstypen, die grundsätzlich schreibgeschützt sind. Sie sollten allerdings nicht allein wegen des Schreibschutzes verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-209">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="0ccf3-210">Des Weiteren werden sie oft Sichten zugeordnet, was aber nur daran liegt, dass für Letztere häufig keine Schlüssel definiert werden.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-210">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="0ccf3-211">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-211">**Mitigations**</span></span>

<span data-ttu-id="0ccf3-212">Die folgenden Teile der API sind durch die Änderungen veraltet:</span><span class="sxs-lookup"><span data-stu-id="0ccf3-212">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="0ccf3-213">**`ModelBuilder.Query<>()`**: Rufen Sie stattdessen `ModelBuilder.Entity<>().HasNoKey()` auf, um einen schlüssellosen Entitätstyp festzulegen.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-213">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="0ccf3-214">Dieses Verhalten wird nach wie vor nicht konventionsgemäß festgelegt, um Fehlkonfigurationen zu vermeiden, wenn ein Primärschlüssel erwartet wird, jedoch nicht mit der Konvention übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-214">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="0ccf3-215">**`DbQuery<>`**: Verwenden Sie stattdessen `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-215">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="0ccf3-216">**`DbContext.Query<>()`**: Verwenden Sie stattdessen `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-216">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="0ccf3-217">Die Konfigurations-API für Beziehungen abhängiger (owned) Typen wurde geändert</span><span class="sxs-lookup"><span data-stu-id="0ccf3-217">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="0ccf3-218">[Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="0ccf3-218">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="0ccf3-219">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-219">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="0ccf3-220">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-220">**Old behavior**</span></span>

<span data-ttu-id="0ccf3-221">Vor Version 3.0 wurde in EF Core die abhängige Beziehung direkt nach dem Aufruf von `OwnsOne` oder `OwnsMany` konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-221">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="0ccf3-222">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-222">**New behavior**</span></span>

<span data-ttu-id="0ccf3-223">Ab Version 3.0 von EF Core ist eine Fluent-API verfügbar, mit der über `WithOwner()` eine Navigationseigenschaft für den Besitzer konfiguriert werden kann.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-223">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="0ccf3-224">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0ccf3-224">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="0ccf3-225">Die Konfiguration für die Beziehung zwischen Besitzertyp und abhängigem Typ sollte nun ähnlich wie bei der Konfiguration anderer Beziehungen nach `WithOwner()` verkettet werden.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-225">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="0ccf3-226">Die Konfiguration für den abhängigen Typ wird jedoch weiterhin nach `OwnsOne()/OwnsMany()` verkettet.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-226">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="0ccf3-227">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0ccf3-227">For example:</span></span>

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

<span data-ttu-id="0ccf3-228">Wenn Sie zusätzlich `Entity()`, `HasOne()`, oder `Set()` für das Ziel eines abhängigen Typs aufrufen, wird keine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-228">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="0ccf3-229">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-229">**Why**</span></span>

<span data-ttu-id="0ccf3-230">Diese Änderung wurde vorgenommen, um eine deutlichere Trennung zwischen der Konfiguration des abhängigen Typs und der _Beziehung zum abhängigen Typ_ zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-230">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="0ccf3-231">Dadurch werden für Methoden wie `HasForeignKey` Mehrdeutigkeiten und Unklarheiten beseitigt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-231">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="0ccf3-232">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-232">**Mitigations**</span></span>

<span data-ttu-id="0ccf3-233">Passen Sie für die Beziehungen von abhängigen Typen die Konfiguration so an, dass die neue Fluent-API wie im obigen Beispiel verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-233">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="0ccf3-234">Die Konvention zur Fremdschlüsseleigenschaft entspricht nicht mehr dem Namen der Prinzipaleigenschaft</span><span class="sxs-lookup"><span data-stu-id="0ccf3-234">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="0ccf3-235">Issue #13274</span><span class="sxs-lookup"><span data-stu-id="0ccf3-235">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="0ccf3-236">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-236">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="0ccf3-237">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-237">**Old behavior**</span></span>

<span data-ttu-id="0ccf3-238">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="0ccf3-238">Consider the following model:</span></span>
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
<span data-ttu-id="0ccf3-239">Vor Version 3.0 wurde in EF Core gemäß der Konvention die `CustomerId`-Eigenschaft für den Fremdschlüssel verwendet.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-239">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="0ccf3-240">Wenn `Order` jedoch ein abhängiger Typ ist, wäre `CustomerId` der Primärschlüssel, was normalerweise nicht der Erwartungshaltung entspricht.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-240">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="0ccf3-241">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-241">**New behavior**</span></span>

<span data-ttu-id="0ccf3-242">Ab Version 3.0 werden in EF Core konventionsgemäß keine Eigenschaften mehr für Fremdschlüssel verwendet, wenn diese denselben Namen wie die Prinzipaleigenschaft besitzen.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-242">Starting with 3.0, EF Core won't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="0ccf3-243">Die Muster für den Namen des Prinzipaltyps, der mit dem Namen der Prinzipaleigenschaft verkettet wird, und für den Navigationsnamen, der mit dem Namen der Prinzipaleigenschaft verkettet wird, werden jedoch weiterhin abgeglichen.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-243">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="0ccf3-244">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0ccf3-244">For example:</span></span>

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

<span data-ttu-id="0ccf3-245">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-245">**Why**</span></span>

<span data-ttu-id="0ccf3-246">Diese Änderung wurde vorgenommen, um zu vermeiden, dass versehentlich eine Primärschlüsseleigenschaft für den abhängigen Typ definiert wird.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-246">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="0ccf3-247">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-247">**Mitigations**</span></span>

<span data-ttu-id="0ccf3-248">Wenn die Eigenschaft als Fremdschlüssel und daher Teil des Primärschlüssels vorgesehen war, müssen Sie diese explizit als solche festlegen.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-248">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="0ccf3-249">Für jede Eigenschaft wird separat ein ganzzahliger speicherinterner Schlüssel generiert</span><span class="sxs-lookup"><span data-stu-id="0ccf3-249">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="0ccf3-250">Issue #6872</span><span class="sxs-lookup"><span data-stu-id="0ccf3-250">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="0ccf3-251">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-251">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="0ccf3-252">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-252">**Old behavior**</span></span>

<span data-ttu-id="0ccf3-253">Vor Version 3.0 wurde in EF Core ein gemeinsam verwendeter Wertegenerator für alle speicherinternen ganzzahligen Schlüsseleigenschaften verwendet.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-253">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="0ccf3-254">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-254">**New behavior**</span></span>

<span data-ttu-id="0ccf3-255">Ab Version 3.0 von EF Core wird für jede ganzzahlige Schlüsseleigenschaft ein eigener Wertegenerator bei der Verwendung der In-Memory Database verwendet.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-255">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="0ccf3-256">Wenn die Datenbank gelöscht wird, wird die Schlüsselgenerierung für alle Tabellen zurückgesetzt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-256">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="0ccf3-257">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-257">**Why**</span></span>

<span data-ttu-id="0ccf3-258">Diese Änderung wurde vorgenommen, um die speicherinterne Schlüsselgenerierung enger mit der Schlüsselgenerierung für echte Datenbanken abzustimmen. Außerdem sollen bei Verwendung der In-Memory Database Tests leichter voneinander isoliert werden können.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-258">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="0ccf3-259">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-259">**Mitigations**</span></span>

<span data-ttu-id="0ccf3-260">Wenn Anwendungen von bestimmten speicherinternen Schlüsselwerten abhängig sind, funktionieren Erstere durch die eingeführte Änderung möglicherweise nicht mehr.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-260">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="0ccf3-261">Versuchen Sie, Abhängigkeiten von bestimmten Schlüsselwerten zu vermeiden, oder passen Sie die Anwendung an das neue Verhalten an.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-261">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="0ccf3-262">Unterstützungsfelder werden standardmäßig verwendet</span><span class="sxs-lookup"><span data-stu-id="0ccf3-262">Backing fields are used by default</span></span>

[<span data-ttu-id="0ccf3-263">Issue #12430</span><span class="sxs-lookup"><span data-stu-id="0ccf3-263">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="0ccf3-264">Diese Änderung wurde in Vorschauversion 2 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-264">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="0ccf3-265">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-265">**Old behavior**</span></span>

<span data-ttu-id="0ccf3-266">Vor Version 3.0 wurde in EF Core mithilfe der Getter- und Settermethoden standardmäßig der Eigenschaftswert gelesen und geschrieben. Das war selbst dann der Fall, wenn das Unterstützungsfeld für eine Eigenschaft nicht bekannt war.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-266">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="0ccf3-267">Eine Ausnahme war die Abfrageausführung, bei der das Unterstützungsfeld direkt festgelegt wurde, wenn es bekannt war.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-267">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="0ccf3-268">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-268">**New behavior**</span></span>

<span data-ttu-id="0ccf3-269">Ab Version 3.0 wird in EF Core die Eigenschaft mithilfe des Unterstützungsfelds gelesen und geschrieben, wenn dieses für eine Eigenschaft bekannt ist.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-269">Starting with EF Core 3.0, if the backing field for a property is known, then will always read and write that property using the backing field.</span></span>
<span data-ttu-id="0ccf3-270">Dies kann dazu führen, dass eine Anwendung nicht mehr funktioniert, wenn sie auf zusätzliches Verhalten angewiesen ist, das im Code der Getter- und Settermethoden definiert ist.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-270">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="0ccf3-271">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-271">**Why**</span></span>

<span data-ttu-id="0ccf3-272">Diese Änderung wurde vorgenommen, um zu verhindern, dass in EF Core fälschlicherweise immer dann Geschäftslogik ausgelöst wird, wenn Datenbankvorgänge für Entitäten durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-272">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="0ccf3-273">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-273">**Mitigations**</span></span>

<span data-ttu-id="0ccf3-274">Sie können das Verhalten vor Version 3.0 wiederherstellen, indem Sie über die modelBuilder-Fluent-API den Zugriffsmodus für die Eigenschaft konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-274">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="0ccf3-275">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0ccf3-275">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="0ccf3-276">Eine Ausnahme wird ausgelöst, wenn mehrere kompatible Unterstützungsfelder gefunden werden</span><span class="sxs-lookup"><span data-stu-id="0ccf3-276">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="0ccf3-277">Issue #12523</span><span class="sxs-lookup"><span data-stu-id="0ccf3-277">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="0ccf3-278">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-278">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="0ccf3-279">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-279">**Old behavior**</span></span>

<span data-ttu-id="0ccf3-280">Vor Version 3.0 wurde in EF Core ein Unterstützungsfeld auf Grundlage einer Rangfolge ausgewählt, wenn die Regeln zum Suchen eines Eigenschaftsfelds auf mehrere Felder zutrafen.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-280">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="0ccf3-281">Dabei konnte es vorkommen, dass in mehrdeutigen Fällen das falsche Feld verwendet wurde.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-281">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="0ccf3-282">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-282">**New behavior**</span></span>

<span data-ttu-id="0ccf3-283">Ab Version 3.0 wird in EF Core eine Ausnahme ausgelöst, wenn sich mehrere Felder auf dieselbe Eigenschaft beziehen.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-283">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="0ccf3-284">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-284">**Why**</span></span>

<span data-ttu-id="0ccf3-285">Diese Änderung wurde vorgenommen, um zu vermeiden, dass automatisch ein falsches Feld verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-285">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="0ccf3-286">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-286">**Mitigations**</span></span>

<span data-ttu-id="0ccf3-287">Für Eigenschaften mit mehrdeutigen Unterstützungsfeldern muss das zu verwendende Feld explizit festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-287">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="0ccf3-288">Mit der Fluent-API ist dies beispielsweise wie folgt möglich:</span><span class="sxs-lookup"><span data-stu-id="0ccf3-288">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="0ccf3-289">DbContext.Entry erkennt nun mit DetectChanges lokale Änderungen</span><span class="sxs-lookup"><span data-stu-id="0ccf3-289">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="0ccf3-290">Issue #13552</span><span class="sxs-lookup"><span data-stu-id="0ccf3-290">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="0ccf3-291">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-291">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="0ccf3-292">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-292">**Old behavior**</span></span>

<span data-ttu-id="0ccf3-293">Vor Version 3.0 führte in EF Core ein Aufruf von `DbContext.Entry` dazu, dass Änderungen an allen überwachten Entitäten erkannt wurden.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-293">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="0ccf3-294">Dadurch wurde sichergestellt, dass der Zustand in `EntityEntry` immer aktuell war.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-294">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="0ccf3-295">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-295">**New behavior**</span></span>

<span data-ttu-id="0ccf3-296">Ab Version 3.0 werden in EF Core Änderungen bei einem Aufruf von `DbContext.Entry` nur in der angegebenen Entität und in allen überwachten Prinzipalentitäten erkannt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-296">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="0ccf3-297">Änderungen an anderer Stelle werden daher durch den Aufruf dieser Methode möglicherweise nicht erkannt, was sich auf den Anwendungszustand auswirken kann.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-297">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="0ccf3-298">Beachten Sie, dass selbst die Erkennung lokaler Änderungen deaktiviert wird, wenn `ChangeTracker.AutoDetectChangesEnabled` auf `false` festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-298">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="0ccf3-299">Andere Methoden wie `ChangeTracker.Entries` und `SaveChanges`, die eine Änderungserkennung auslösen, führen nach wie vor zu einem vollständigen `DetectChanges`-Vorgang für alle überwachten Entitäten.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-299">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="0ccf3-300">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-300">**Why**</span></span>

<span data-ttu-id="0ccf3-301">Diese Änderung wurde vorgenommen, um die Leistung bei der standardmäßigen Verwendung von `context.Entry` zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-301">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="0ccf3-302">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-302">**Mitigations**</span></span>

<span data-ttu-id="0ccf3-303">Rufen Sie `ChgangeTracker.DetectChanges()` explizit vor dem Aufruf von `Entry` auf, wenn das Verhalten vor Version 3.0 beibehalten werden soll.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-303">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="0ccf3-304">Schlüssel aus Zeichenfolgen und Bytearrays werden standardmäßig nicht vom Client generiert</span><span class="sxs-lookup"><span data-stu-id="0ccf3-304">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="0ccf3-305">Issue #14617</span><span class="sxs-lookup"><span data-stu-id="0ccf3-305">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="0ccf3-306">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-306">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="0ccf3-307">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-307">**Old behavior**</span></span>

<span data-ttu-id="0ccf3-308">Vor Version 3.0 konnten in EF Core `string`- und `byte[]`-Schlüsseleigenschaften verwendet werden, ohne dass explizit ein anderer Wert als NULL festgelegt werden musste.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-308">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="0ccf3-309">In diesem Fall wurde der Schlüsselwert auf dem Client als GUID generiert und für `byte[]` in Bytes serialisiert.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-309">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="0ccf3-310">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-310">**New behavior**</span></span>

<span data-ttu-id="0ccf3-311">Ab Version 3.0 wird in EF Core eine Ausnahme ausgelöst, wenn kein Schlüsselwert festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-311">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="0ccf3-312">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-312">**Why**</span></span>

<span data-ttu-id="0ccf3-313">Diese Änderung wurde vorgenommen, da vom Client generierte `string`/`byte[]`-Werte in der Regel nicht sinnvoll sind. Das Standardverhalten führte außerdem zu kaum nachvollziehbaren generierten Schlüsselwerten.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-313">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="0ccf3-314">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-314">**Mitigations**</span></span>

<span data-ttu-id="0ccf3-315">Wenn Sie das Verhalten vor Version 3.0 nutzen möchten, müssen Sie explizit festlegen, dass für die Schlüsseleigenschaften generierte Werte verwendet sollen, wenn kein anderer Nicht-NULL-Wert festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-315">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="0ccf3-316">Dies ist beispielsweise mit der Fluent-API möglich:</span><span class="sxs-lookup"><span data-stu-id="0ccf3-316">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="0ccf3-317">Alternativ bieten sich auch Datenanmerkungen an:</span><span class="sxs-lookup"><span data-stu-id="0ccf3-317">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="0ccf3-318">ILoggerFactory ist nun ein bereichsbezogener Dienst</span><span class="sxs-lookup"><span data-stu-id="0ccf3-318">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="0ccf3-319">Issue #14698</span><span class="sxs-lookup"><span data-stu-id="0ccf3-319">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="0ccf3-320">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-320">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="0ccf3-321">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-321">**Old behavior**</span></span>

<span data-ttu-id="0ccf3-322">Vor Version 3.0 wurde `ILoggerFactory` in EF Core als Singletondienst registriert.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-322">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="0ccf3-323">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-323">**New behavior**</span></span>

<span data-ttu-id="0ccf3-324">Ab Version 3.0 wird in EF Core `ILoggerFactory` als bereichsbezogener Dienst registriert.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-324">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="0ccf3-325">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-325">**Why**</span></span>

<span data-ttu-id="0ccf3-326">Diese Änderung wurde vorgenommen, um einer `DbContext`-Instanz eine Protokollierung zuordnen zu können. Dadurch werden weitere Funktionen bereitgestellt. In einigen Fällen wird außerdem schädliches Verhalten wie etwa die schnelle Zunahme von internen Dienstanbietern beseitigt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-326">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="0ccf3-327">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-327">**Mitigations**</span></span>

<span data-ttu-id="0ccf3-328">Diese Änderung wirkt sich nur dann auf den Anwendungscode aus, wenn benutzerdefinierte Dienste auf dem internen Dienstanbieter von EF Core registriert und verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-328">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="0ccf3-329">Ein solches Szenario ist aber unüblich.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-329">This isn't common.</span></span>
<span data-ttu-id="0ccf3-330">In einem derartigen Fall funktioniert fast alles wie vorher. Singletondienste, die von `ILoggerFactory` abhängig sind, müssen jedoch so angepasst werden, dass `ILoggerFactory` auf andere Weise abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-330">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="0ccf3-331">Erstellen Sie in diesen Situationen bitte im [GitHub-Issuetracker für EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) ein Issue, in dem Sie beschreiben, wie Sie `ILoggerFactory` verwenden. So können wir in Zukunft erneute Breaking Changes vermeiden.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-331">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="0ccf3-332">IDbContextOptionsExtensionWithDebugInfo wurde mit IDbContextOptionsExtension zusammengeführt</span><span class="sxs-lookup"><span data-stu-id="0ccf3-332">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="0ccf3-333">Issue #13552</span><span class="sxs-lookup"><span data-stu-id="0ccf3-333">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="0ccf3-334">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-334">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="0ccf3-335">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-335">**Old behavior**</span></span>

<span data-ttu-id="0ccf3-336">`IDbContextOptionsExtensionWithDebugInfo` war eine zusätzliche optionale Schnittstelle, die von `IDbContextOptionsExtension` abgeleitet wurde, um während des 2.x-Releasezyklus Breaking Changes an der Schnittstelle zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-336">`IDbContextOptionsExtensionWithDebugInfo` was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

<span data-ttu-id="0ccf3-337">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-337">**New behavior**</span></span>

<span data-ttu-id="0ccf3-338">Die Schnittstellen wurden in `IDbContextOptionsExtension` zusammengeführt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-338">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

<span data-ttu-id="0ccf3-339">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-339">**Why**</span></span>

<span data-ttu-id="0ccf3-340">Diese Änderung wurde vorgenommen, da es sich im Prinzip um eine Schnittstelle handelt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-340">This change was made because the interfaces are conceptually one.</span></span>

<span data-ttu-id="0ccf3-341">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-341">**Mitigations**</span></span>

<span data-ttu-id="0ccf3-342">Alle Implementierungen von `IDbContextOptionsExtension` müssen aktualisiert werden, damit der neue Member unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-342">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="0ccf3-343">Für Proxys mit Lazy Loading (trägem Laden) gilt nicht mehr die Annahme, dass Navigationseigenschaften vollständig geladen sind</span><span class="sxs-lookup"><span data-stu-id="0ccf3-343">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="0ccf3-344">Issue #12780</span><span class="sxs-lookup"><span data-stu-id="0ccf3-344">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="0ccf3-345">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-345">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="0ccf3-346">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-346">**Old behavior**</span></span>

<span data-ttu-id="0ccf3-347">Vor Version 3.0 gab es in EF Core nach der Freigabe eines `DbContext`-Objekts keine Möglichkeit, zu ermitteln, ob eine aus dem Kontext abgerufene Navigationseigenschaft für eine Entität vollständig geladen war.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-347">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="0ccf3-348">Stattdessen galt für Proxys die Annahme, dass eine Verweisnavigation geladen ist, falls für diese ein anderer Wert als NULL vorliegt. Außerdem wurde davon ausgegangen, dass eine Sammlungsnavigation geladen ist, wenn sie nicht leer ist.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-348">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="0ccf3-349">In diesen Fällen entsprach das Lazy Loading einer Nulloperation („No-Op“).</span><span class="sxs-lookup"><span data-stu-id="0ccf3-349">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="0ccf3-350">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-350">**New behavior**</span></span>

<span data-ttu-id="0ccf3-351">Ab Version 3.0 überwachen in EF Core Proxys, ob eine Navigationseigenschaft geladen ist.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-351">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="0ccf3-352">Wenn also auf eine Navigationseigenschaft zugegriffen werden soll, die nach der Freigabe des Kontexts geladen wird, ist dies immer eine No-Op-Operation. Das ist selbst dann der Fall, wenn die geladene Navigation leer oder NULL ist.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-352">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="0ccf3-353">Wenn umgekehrt auf eine nicht geladene Navigationseigenschaft zugegriffen werden soll und der Kontext bereits freigegeben wurde, wird eine Ausnahme ausgelöst. Dieser Fall tritt auch dann ein, wenn die Navigationseigenschaft eine nicht leere Sammlung ist.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-353">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="0ccf3-354">In dieser Situation wird also vom Anwendungscode versucht, zu einem ungültigen Zeitpunkt Lazy Loading durchzuführen. Die Anwendung sollte so angepasst werden, dass dieser Vorgang vermieden wird.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-354">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="0ccf3-355">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-355">**Why**</span></span>

<span data-ttu-id="0ccf3-356">Diese Änderung wurde vorgenommen, um das Verhalten beim Lazy Loading einer freigegebenen `DbContext`-Instanz konsistent und korrekt zu gestalten.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-356">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="0ccf3-357">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-357">**Mitigations**</span></span>

<span data-ttu-id="0ccf3-358">Aktualisieren Sie den Anwendungscode so, dass kein Lazy Loading durchgeführt wird, nachdem der Kontext freigegeben wurde. Alternativ können Sie auch, wie in der Ausnahmemeldung beschrieben wird, eine No-Op-Operation festlegen.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-358">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="0ccf3-359">Die Erstellung zu vieler interner Dienstanbieter führt standardmäßig zu einem Fehler</span><span class="sxs-lookup"><span data-stu-id="0ccf3-359">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="0ccf3-360">Issue #10236</span><span class="sxs-lookup"><span data-stu-id="0ccf3-360">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="0ccf3-361">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-361">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="0ccf3-362">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-362">**Old behavior**</span></span>

<span data-ttu-id="0ccf3-363">Vor Version 3.0 wurde in EF Core eine Warnung für eine Anwendung protokolliert, die unverhältnismäßig viele interne Dienstanbieter erstellte.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-363">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="0ccf3-364">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-364">**New behavior**</span></span>

<span data-ttu-id="0ccf3-365">Ab Version 3.0 wird diese Warnung in EF Core als Fehler betrachtet, und eine Ausnahme wird ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-365">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="0ccf3-366">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-366">**Why**</span></span>

<span data-ttu-id="0ccf3-367">Diese Änderung wurde vorgenommen, damit auf den oben beschriebenen Fall explizit hingewiesen wird. So soll die Entwicklung von besserem Anwendungscode erleichtert werden.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-367">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="0ccf3-368">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-368">**Mitigations**</span></span>

<span data-ttu-id="0ccf3-369">Wenn dieser Fehler auftritt, müssen Sie die Grundursache ermitteln und die Erstellung vieler interner Dienstanbieter unterbinden.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-369">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="0ccf3-370">Sie können den Fehler allerdings auch wieder in eine Warnung konvertierten (oder ihn ignorieren), indem Sie `DbContextOptionsBuilder` konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-370">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="0ccf3-371">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="0ccf3-371">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="0ccf3-372">Die Anmerkung Relational:TypeMapping heißt nun nur noch TypeMapping</span><span class="sxs-lookup"><span data-stu-id="0ccf3-372">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="0ccf3-373">Issue #9913</span><span class="sxs-lookup"><span data-stu-id="0ccf3-373">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="0ccf3-374">Diese Änderung wurde in Vorschauversion 2 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-374">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="0ccf3-375">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-375">**Old behavior**</span></span>

<span data-ttu-id="0ccf3-376">Der Anmerkungsname für Typzuordnungsanmerkungen war bisher Relational:TypeMapping.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-376">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="0ccf3-377">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-377">**New behavior**</span></span>

<span data-ttu-id="0ccf3-378">Der Anmerkungsname für Typzuordnungsanmerkungen lautet nun TypeMapping.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-378">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="0ccf3-379">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-379">**Why**</span></span>

<span data-ttu-id="0ccf3-380">Typzuordnungen werden nicht mehr ausschließlich für Anbieter relationaler Datenbanken verwendet.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-380">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="0ccf3-381">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-381">**Mitigations**</span></span>

<span data-ttu-id="0ccf3-382">Diese Änderung ist nur dann ein Breaking Change, wenn Anwendungen direkt über eine Anmerkung auf die Typzuordnung zugreifen. Dieses Szenario ist jedoch unüblich.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-382">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="0ccf3-383">Verzichten Sie daher darauf, die Anmerkung direkt zu verwenden, und greifen Sie stattdessen über die Fluent-API auf Typzuordnungen zu, um das Problem zu beheben.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-383">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="0ccf3-384">ToTable löst für einen abgeleiteten Typ eine Ausnahme aus</span><span class="sxs-lookup"><span data-stu-id="0ccf3-384">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="0ccf3-385">Issue #11811</span><span class="sxs-lookup"><span data-stu-id="0ccf3-385">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="0ccf3-386">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-386">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="0ccf3-387">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-387">**Old behavior**</span></span>

<span data-ttu-id="0ccf3-388">Vor Version 3.0 wurde in EF Core der Aufruf von `ToTable()` für einen abgeleiteten Typ ignoriert, da TPH die einzige Strategie zur Vererbungszuordnung war und diese in unzulässigen Fällen angewendet wurde.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-388">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="0ccf3-389">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-389">**New behavior**</span></span>

<span data-ttu-id="0ccf3-390">Ab Version 3.0 wird in EF Core eine Ausnahme ausgelöst, um später eine unerwartete Zuordnung zu vermeiden, wenn `ToTable()` für einen abgeleiteten Typ aufgerufen wird. Dieser Schritt dient auch als Vorbereitung für die TPT- und TPC-Unterstützung, die in einem künftigen Release folgen wird.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-390">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="0ccf3-391">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-391">**Why**</span></span>

<span data-ttu-id="0ccf3-392">Derzeit ist es nicht zulässig, einen abgeleiteten Typ einer anderen Tabelle zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-392">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="0ccf3-393">Durch diese Änderung werden Breaking Changes vermieden, wenn der beschriebene Vorgang in Zukunft zulässig wird.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-393">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="0ccf3-394">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-394">**Mitigations**</span></span>

<span data-ttu-id="0ccf3-395">Entfernen Sie alle Zuordnungen von abgeleiteten Typen zu anderen Tabellen.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-395">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="0ccf3-396">ForSqlServerHasIndex wurde durch HasIndex ersetzt</span><span class="sxs-lookup"><span data-stu-id="0ccf3-396">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="0ccf3-397">Issue #12366</span><span class="sxs-lookup"><span data-stu-id="0ccf3-397">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="0ccf3-398">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-398">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="0ccf3-399">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-399">**Old behavior**</span></span>

<span data-ttu-id="0ccf3-400">Vor Version 3.0 konnten in EF Core mit `ForSqlServerHasIndex().ForSqlServerInclude()` Spalten konfiguriert werden, die mit `INCLUDE` verwendet wurden.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-400">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="0ccf3-401">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-401">**New behavior**</span></span>

<span data-ttu-id="0ccf3-402">Ab Version 3.0 wird in EF Core die Nutzung von `Include` für einen Index auf der relationalen Ebene unterstützt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-402">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="0ccf3-403">Verwenden Sie `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-403">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="0ccf3-404">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-404">**Why**</span></span>

<span data-ttu-id="0ccf3-405">Diese Änderung wurde vorgenommen, um die API für Indizes mit `Includes` an einer zentralen Stelle für alle Datenbankanbieter zusammenzuführen.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-405">This change was made to consolidate the API for indexes with `Includes` into one place for all database providers.</span></span>

<span data-ttu-id="0ccf3-406">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-406">**Mitigations**</span></span>

<span data-ttu-id="0ccf3-407">Verwenden Sie wie oben beschrieben die neue API.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-407">Use the new API, as shown above.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="0ccf3-408">EF Core sendet keine PRAGMA-Anweisungen mehr, um Fremdschlüssel in SQLite zu erzwingen</span><span class="sxs-lookup"><span data-stu-id="0ccf3-408">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="0ccf3-409">Issue #12151</span><span class="sxs-lookup"><span data-stu-id="0ccf3-409">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="0ccf3-410">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-410">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="0ccf3-411">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-411">**Old behavior**</span></span>

<span data-ttu-id="0ccf3-412">Vor Version 3.0 wurden in EF Core `PRAGMA foreign_keys = 1`-Anweisungen gesendet, wenn eine Verbindung mit SQLite hergestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-412">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="0ccf3-413">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-413">**New behavior**</span></span>

<span data-ttu-id="0ccf3-414">Ab Version 3.0 werden in EF Core keine `PRAGMA foreign_keys = 1`-Anweisungen mehr gesendet, wenn eine Verbindung mit SQLite hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-414">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="0ccf3-415">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-415">**Why**</span></span>

<span data-ttu-id="0ccf3-416">Diese Änderung wurde vorgenommen, da von EF Core standardmäßig `SQLitePCLRaw.bundle_e_sqlite3` verwendet wird. Dadurch ist die Erzwingung von Fremdschlüsseln standardmäßig aktiviert und muss nicht jedes Mal explizit aktiviert werden, wenn eine Verbindung hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-416">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="0ccf3-417">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-417">**Mitigations**</span></span>

<span data-ttu-id="0ccf3-418">Fremdschlüssel sind in SQLitePCLRaw.bundle_e_sqlite3 standardmäßig aktiviert. Dieses Bundle wird standardmäßig von EF Core verwendet.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-418">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="0ccf3-419">In anderen Fällen können Fremdschlüssel durch die Angabe `Foreign Keys=True` in der Verbindungszeichenfolge aktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-419">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="0ccf3-420">SQLitePCLRaw.bundle_e_sqlite3 ist nun eine Abhängigkeit von Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="0ccf3-420">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="0ccf3-421">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-421">**Old behavior**</span></span>

<span data-ttu-id="0ccf3-422">Vor Version 3.0 wurde von EF Core `SQLitePCLRaw.bundle_green` verwendet.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-422">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="0ccf3-423">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-423">**New behavior**</span></span>

<span data-ttu-id="0ccf3-424">Ab Version 3.0 wird von EF Core `SQLitePCLRaw.bundle_e_sqlite3` verwendet.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-424">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="0ccf3-425">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-425">**Why**</span></span>

<span data-ttu-id="0ccf3-426">Diese Änderung wurde vorgenommen, damit die unter iOS verwendete Version von SQLite sich konsistent zu Versionen auf anderen Plattformen verhält.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-426">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="0ccf3-427">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="0ccf3-427">**Mitigations**</span></span>

<span data-ttu-id="0ccf3-428">Konfigurieren Sie `Microsoft.Data.Sqlite` so, dass ein anderes `SQLitePCLRaw`-Bundle verwendet wird, um die native SQLite-Version unter iOS zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="0ccf3-428">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>
