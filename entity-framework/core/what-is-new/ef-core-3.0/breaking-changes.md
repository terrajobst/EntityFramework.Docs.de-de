---
title: Breaking Changes in EF Core 3.0
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 4b251638de43af6525f3e6faa0bd4113ab1714b9
ms.sourcegitcommit: 5280dcac4423acad8b440143433459b18886115b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2019
ms.locfileid: "59619258"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="e93bf-102">Breaking Changes in EF Core 3.0 (aktuell in der Vorschauversion)</span><span class="sxs-lookup"><span data-stu-id="e93bf-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e93bf-103">Bitte beachten Sie, dass die Featuregruppen und Zeitpläne für künftige Releases jederzeit geändert werden können. Obwohl diese Seite bestmöglich aktualisiert wird, entspricht sie nicht immer den neuesten Plänen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="e93bf-104">Die folgenden API-Änderungen und Behavior Changes können dazu führen, dass Anwendungen, die für EF Core 2.2.x entwickelt wurden, beim Upgrade auf 3.0.0 nicht mehr funktionieren.</span><span class="sxs-lookup"><span data-stu-id="e93bf-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="e93bf-105">Änderungen, die sich voraussichtlich nur auf Datenbankanbieter auswirken, sind unter [Änderungen mit Auswirkungen auf den Anbieter](../../providers/provider-log.md) dokumentiert.</span><span class="sxs-lookup"><span data-stu-id="e93bf-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="e93bf-106">Werden Breaking Changes zwischen zwei Vorschauversionen von EF Core 3.0 in neuen Features eingeführt, werden diese hier nicht dokumentiert.</span><span class="sxs-lookup"><span data-stu-id="e93bf-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="e93bf-107">LINQ-Abfragen werden nicht mehr auf dem Client ausgewertet</span><span class="sxs-lookup"><span data-stu-id="e93bf-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="e93bf-108">[Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Siehe auch Issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="e93bf-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="e93bf-109">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-109">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="e93bf-110">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-110">**Old behavior**</span></span>

<span data-ttu-id="e93bf-111">Vor Version 3.0 wurden in EF Core automatisch Ausdrücke auf dem Client ausgewertet, wenn diese Teil einer Abfrage waren und nicht in SQL oder in einen Parameter konvertiert werden konnten.</span><span class="sxs-lookup"><span data-stu-id="e93bf-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="e93bf-112">Wenn potenziell aufwendige Ausdrücke auf dem Client ausgewertet werden sollten, wurde standardmäßig nur eine Warnung ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="e93bf-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="e93bf-113">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-113">**New behavior**</span></span>

<span data-ttu-id="e93bf-114">Ab Version 3.0 können in EF Core nur Ausdrücke der obersten Projektion (dem letzten `Select()`-Aufruf in der Abfrage) auf dem Client ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="e93bf-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="e93bf-115">Wenn Ausdrücke in einem anderen Teil der Abfrage nicht in SQL oder in einen Parameter konvertiert werden können, wird eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="e93bf-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="e93bf-116">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-116">**Why**</span></span>

<span data-ttu-id="e93bf-117">Die automatische Auswertung von Abfragen auf Clients hat zur Folge, dass viele Abfragen auch dann ausgeführt werden, wenn wichtige Teile nicht übersetzt werden können.</span><span class="sxs-lookup"><span data-stu-id="e93bf-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="e93bf-118">Dieses Verhalten kann zu unerwartetem und potenziell schädlichem Verhalten führen, das möglicherweise erst in der Produktionsphase erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="e93bf-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="e93bf-119">Eine nicht übersetzbare Bedingung in einem `Where()`-Aufruf kann sich beispielsweise so auswirken, dass alle Zeilen einer Tabelle vom Datenbankserver übertragen werden und der Filter auf dem Client angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="e93bf-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="e93bf-120">Wenn die Tabelle in der Entwicklungsphase nur wenige Zeilen enthält, ist es gut möglich, dass diese Situation unerkannt bleibt. Wenn die Anwendung jedoch in die Produktionsumgebung überführt wird, enthält die Tabelle möglicherweise Millionen von Zeilen, was zu schwerwiegenden Konsequenzen führen kann.</span><span class="sxs-lookup"><span data-stu-id="e93bf-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="e93bf-121">Wie die Erfahrung in der Entwicklungsphase gezeigt hat, können auch Warnungen, die bei Auswertungen auf dem Client ausgelöst werden, dieses Problem nicht lösen, da sie sich leicht ignorieren lassen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="e93bf-122">Die automatische Auswertung auf Clients kann darüber hinaus zu Problemen führen, bei denen die Verbesserung der Abfrageübersetzung für bestimmte Ausdrücke zu unbeabsichtigten Breaking Changes zwischen Releases führt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="e93bf-123">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-123">**Mitigations**</span></span>

<span data-ttu-id="e93bf-124">Wenn sich eine Abfrage nicht vollständig übersetzen lässt, habe Sie zwei Möglichkeiten: Schreiben Sie sie entweder um, oder setzen Sie alternativ `AsEnumerable()`, `ToList()` oder ähnliche Methoden ein, um Daten wieder zurück an den Client zu übertragen, wo sie mit LINQ to Objects weiterverarbeitet werden können.</span><span class="sxs-lookup"><span data-stu-id="e93bf-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="e93bf-125">Entity Framework Core ist nicht mehr Teil des gemeinsam verwendeten ASP.NET Core-Frameworks</span><span class="sxs-lookup"><span data-stu-id="e93bf-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="e93bf-126">Issueankündigungen #325</span><span class="sxs-lookup"><span data-stu-id="e93bf-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="e93bf-127">Diese Änderung wurde in Vorschauversion 1 von ASP.NET Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-127">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="e93bf-128">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-128">**Old behavior**</span></span>

<span data-ttu-id="e93bf-129">Vor Version 3.0 von ASP.NET Core wurden EF Core und einige der zugehörigen Datenanbieter wie SQL Server eingeschlossen, wenn `Microsoft.AspNetCore.App` oder `Microsoft.AspNetCore.All` ein Paketverweis hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="e93bf-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="e93bf-130">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-130">**New behavior**</span></span>

<span data-ttu-id="e93bf-131">Ab Version 3.0 enthält das gemeinsam verwendete ASP.NET Core-Framework weder EF Core noch zugehörige Datenanbieter.</span><span class="sxs-lookup"><span data-stu-id="e93bf-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="e93bf-132">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-132">**Why**</span></span>

<span data-ttu-id="e93bf-133">Vor dieser Änderung konnte EF Core auf unterschiedliche Arten bezogen werden. Diese richteten sich danach, ob ASP.NET Core und SQL Server oder ein anderes Zielframework für die Anwendung vorgesehen war.</span><span class="sxs-lookup"><span data-stu-id="e93bf-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="e93bf-134">Bei einem Upgrade von ASP.NET Core wurde zudem ein Upgrade von EF Core und des SQL Server-Anbieters erzwungen, was nicht in allen Fällen gewünscht war.</span><span class="sxs-lookup"><span data-stu-id="e93bf-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="e93bf-135">Durch die eingeführte Änderung wird EF Core unter allen Anbietern, unterstützten .NET-Implementierungen und Anwendungstypen auf dieselbe Weise bezogen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="e93bf-136">Entwickler können nun außerdem genau festlegen, wann für EF Core und zugehörige Datenanbieter ein Upgrade durchgeführt werden soll.</span><span class="sxs-lookup"><span data-stu-id="e93bf-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="e93bf-137">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-137">**Mitigations**</span></span>

<span data-ttu-id="e93bf-138">Wenn Sie EF Core in einer Anwendung unter ASP.NET Core 3.0 oder in einer anderen unterstützten Anwendung verwenden möchten, müssen Sie dem EF Core-Datenbankanbieter, der für die Anwendung genutzt werden soll, explizit einen Paketverweis hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="e93bf-139">FromSql, ExecuteSql und ExecuteSqlAsync wurden umbenannt</span><span class="sxs-lookup"><span data-stu-id="e93bf-139">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="e93bf-140">Issue #10996</span><span class="sxs-lookup"><span data-stu-id="e93bf-140">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="e93bf-141">Diese Änderung wurde mit EF Core 3.0 Preview 4 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-141">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="e93bf-142">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-142">**Old behavior**</span></span>

<span data-ttu-id="e93bf-143">Vor Version 3.0 wurden in EF Core diese Methodennamen überladen, um entweder mit einer normalen Zeichenfolge oder einer Zeichenfolge, die in SQL und Parameter interpoliert werden sollte, zu arbeiten.</span><span class="sxs-lookup"><span data-stu-id="e93bf-143">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="e93bf-144">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-144">**New behavior**</span></span>

<span data-ttu-id="e93bf-145">Ab Version 3.0 verwenden Sie in EF Core `FromSqlRaw`, `ExecuteSqlRaw` und `ExecuteSqlRawAsync`, um eine parametrisierte Abfrage zu erstellen, bei der die Parameter einzeln aus der Abfragezeichenfolge übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="e93bf-145">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="e93bf-146">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="e93bf-146">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="e93bf-147">Verwenden Sie `FromSqlInterpolated`, `ExecuteSqlInterpolated` und `ExecuteSqlInterpolatedAsync`, um eine parametrisierte Abfrage zu erstellen, bei der die Parameter als Teil einer interpolierten Abfragezeichenfolge übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="e93bf-147">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="e93bf-148">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="e93bf-148">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="e93bf-149">Beachten Sie, dass die beiden oben genannten Abfragen dieselbe parametrisierte SQL mit denselben SQL-Parametern erzeugen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-149">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="e93bf-150">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-150">**Why**</span></span>

<span data-ttu-id="e93bf-151">Bei Methodenüberladungen wie dieser wird leicht versehentlich die Methode für unformatierte Zeichenfolgen aufgerufen, wenn eigentlich beabsichtigt war, die Methode für interpolierte Zeichenfolgen aufzurufen, und umgekehrt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-151">Method overloads like this make it very easy to accidentally call the raw srting method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="e93bf-152">Dadurch werden Abfragen möglicherweise nicht parametrisiert, obwohl dies der Fall sein sollte.</span><span class="sxs-lookup"><span data-stu-id="e93bf-152">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="e93bf-153">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-153">**Mitigations**</span></span>

<span data-ttu-id="e93bf-154">Verwenden Sie ab sofort die neuen Methodennamen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-154">Switch to use the new method names.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="e93bf-155">Die Ausführung der Abfrage wird auf der Debugebene protokolliert</span><span class="sxs-lookup"><span data-stu-id="e93bf-155">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="e93bf-156">Issue #14523</span><span class="sxs-lookup"><span data-stu-id="e93bf-156">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="e93bf-157">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-157">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="e93bf-158">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-158">**Old behavior**</span></span>

<span data-ttu-id="e93bf-159">Vor Version 3.0 wurde in EF Core die Ausführung von Abfragen und anderen Befehlen auf der `Info`-Ebene protokolliert.</span><span class="sxs-lookup"><span data-stu-id="e93bf-159">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

<span data-ttu-id="e93bf-160">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-160">**New behavior**</span></span>

<span data-ttu-id="e93bf-161">Ab Version 3.0 wird in EF Core die Ausführung von Befehlen und SQL-Abfragen auf der `Debug`-Ebene protokolliert.</span><span class="sxs-lookup"><span data-stu-id="e93bf-161">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

<span data-ttu-id="e93bf-162">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-162">**Why**</span></span>

<span data-ttu-id="e93bf-163">Diese Änderung wurde vorgenommen, damit auf der `Info`-Ebene nur die wichtigsten Daten erfasst werden.</span><span class="sxs-lookup"><span data-stu-id="e93bf-163">This change was made to reduce the noise at the `Info` log level.</span></span>

<span data-ttu-id="e93bf-164">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-164">**Mitigations**</span></span>

<span data-ttu-id="e93bf-165">Dieses Protokollereignis wird durch `RelationalEventId.CommandExecuting` mit der Ereignis-ID 20100 definiert.</span><span class="sxs-lookup"><span data-stu-id="e93bf-165">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="e93bf-166">Um SQL erneut auf der Ebene `Info` zu protokollieren, konfigurieren Sie die Ebene explizit in `OnConfiguring` oder `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="e93bf-166">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="e93bf-167">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="e93bf-167">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="e93bf-168">Temporäre Schlüsselwerte werden nicht mehr Entitätsinstanzen zugewiesen</span><span class="sxs-lookup"><span data-stu-id="e93bf-168">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="e93bf-169">Issue #12378</span><span class="sxs-lookup"><span data-stu-id="e93bf-169">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="e93bf-170">Diese Änderung wurde in Vorschauversion 2 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-170">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="e93bf-171">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-171">**Old behavior**</span></span>

<span data-ttu-id="e93bf-172">Vor Version 3.0 wurden in EF Core allen Schlüsseleigenschaften temporäre Werte zugewiesen. Später wurden den Eigenschaften dann die tatsächlichen Werte zugewiesen, die von der Datenbank generiert wurden.</span><span class="sxs-lookup"><span data-stu-id="e93bf-172">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="e93bf-173">Die temporären Werte waren in der Regel große negative Zahlen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-173">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="e93bf-174">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-174">**New behavior**</span></span>

<span data-ttu-id="e93bf-175">Ab Version 3.0 wird in EF Core der temporäre Wert als Teil der Überwachungsinformationen der Entität gespeichert. Die Schlüsseleigenschaft selbst bleibt unverändert.</span><span class="sxs-lookup"><span data-stu-id="e93bf-175">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="e93bf-176">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-176">**Why**</span></span>

<span data-ttu-id="e93bf-177">Diese Änderung wurde vorgenommen, damit temporäre Schlüsselwerte nicht fälschlicherweise dauerhaft gespeichert werden, wenn eine Entität, die vorher von einer `DbContext`-Instanz überwacht wurde, in eine andere `DbContext`-Instanz verschoben wird.</span><span class="sxs-lookup"><span data-stu-id="e93bf-177">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="e93bf-178">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-178">**Mitigations**</span></span>

<span data-ttu-id="e93bf-179">Anwendungen, die Primärschlüsselwerte Fremdschlüsseln zuweisen, um Zuordnungen zwischen Entitäten zu erstellen, können vom alten Verhalten abhängig sein, wenn Primärschlüssel vom Speicher generiert werden und zu Entitäten im `Added`-Zustand gehören.</span><span class="sxs-lookup"><span data-stu-id="e93bf-179">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="e93bf-180">Sie können dies wie folgt vermeiden:</span><span class="sxs-lookup"><span data-stu-id="e93bf-180">This can be avoided by:</span></span>
* <span data-ttu-id="e93bf-181">Verwenden Sie keine vom Speicher generierten Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="e93bf-181">Not using store-generated keys.</span></span>
* <span data-ttu-id="e93bf-182">Legen Sie zum Erstellen von Zuordnungen keine Fremdschlüsselwerte, sondern Navigationseigenschaften fest.</span><span class="sxs-lookup"><span data-stu-id="e93bf-182">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="e93bf-183">Rufen Sie die tatsächlichen temporären Schlüsselwerte aus den Überwachungsinformationen der Entität ab.</span><span class="sxs-lookup"><span data-stu-id="e93bf-183">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="e93bf-184">`context.Entry(blog).Property(e => e.Id).CurrentValue` gibt beispielsweise den temporären Wert zurück, obwohl `blog.Id` noch nicht festgelegt wurde.</span><span class="sxs-lookup"><span data-stu-id="e93bf-184">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="e93bf-185">DetectChanges berücksichtigt vom Speicher generierte Schlüsselwerte</span><span class="sxs-lookup"><span data-stu-id="e93bf-185">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="e93bf-186">Issue #14616</span><span class="sxs-lookup"><span data-stu-id="e93bf-186">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="e93bf-187">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-187">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="e93bf-188">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-188">**Old behavior**</span></span>

<span data-ttu-id="e93bf-189">Vor Version 3.0 wurde in EF Core eine nicht überwachte Entität, die von `DetectChanges` gefunden wurde, im `Added`-Zustand überwacht und als neue Zeile eingefügt, sobald `SaveChanges` aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="e93bf-189">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="e93bf-190">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-190">**New behavior**</span></span>

<span data-ttu-id="e93bf-191">Ab Version 3.0 wird in EF Core die Entität im `Modified`-Zustand überwacht, wenn für diese generierte Schlüsselwerte verwendet werden und ein Schlüsselwert festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="e93bf-191">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="e93bf-192">Es wird also davon ausgegangen, dass eine Zeile für die Entität vorhanden ist. Wenn `SaveChanges` aufgerufen wird, wird die Zeile außerdem aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="e93bf-192">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="e93bf-193">Falls kein Schlüsselwert festgelegt ist oder der Entitätstyp keine generierten Schlüssel verwendet, wird die neue Entität wie in früheren Version weiterhin im `Added`-Zustand überwacht.</span><span class="sxs-lookup"><span data-stu-id="e93bf-193">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="e93bf-194">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-194">**Why**</span></span>

<span data-ttu-id="e93bf-195">Diese Änderung wurde vorgenommen, um bei der Verwendung von speichergenerierten Schlüsseln die Arbeit mit nicht verbundenen Entitätsgraphen einfacher und konsistenter zu gestalten.</span><span class="sxs-lookup"><span data-stu-id="e93bf-195">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="e93bf-196">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-196">**Mitigations**</span></span>

<span data-ttu-id="e93bf-197">Diese Änderung kann dazu führen, dass eine Anwendung nicht mehr funktioniert. Dieser Fall tritt ein, wenn für Entitätstypen generierte Schlüssel vorgesehen sind, dann jedoch explizit Schlüsselwerte für neue Instanzen festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="e93bf-197">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="e93bf-198">Sie können dieses Problem vermeiden, indem Sie explizit festlegen, dass für Schlüsseleigenschaften keine generierten Werte verwendet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-198">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="e93bf-199">Dies ist beispielsweise mit der Fluent-API möglich:</span><span class="sxs-lookup"><span data-stu-id="e93bf-199">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="e93bf-200">Alternativ bieten sich auch Datenanmerkungen an:</span><span class="sxs-lookup"><span data-stu-id="e93bf-200">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="e93bf-201">Kaskadierende Deletes werden standardmäßig sofort ausgeführt</span><span class="sxs-lookup"><span data-stu-id="e93bf-201">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="e93bf-202">Issue #10114</span><span class="sxs-lookup"><span data-stu-id="e93bf-202">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="e93bf-203">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-203">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="e93bf-204">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-204">**Old behavior**</span></span>

<span data-ttu-id="e93bf-205">Vor Version 3.0 wurden in EF Core kaskadierenden Aktionen (Löschen von abhängigen Entitäten nach dem Löschen eines erforderlichen Prinzipals oder nach dem Aufheben einer Beziehung zum erforderlichen Prinzipal) erst ausgeführt, wenn SaveChanges aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="e93bf-205">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="e93bf-206">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-206">**New behavior**</span></span>

<span data-ttu-id="e93bf-207">Ab Version 3.0 werden in EF Core kaskadierende Aktionen angewendet, sobald die auslösende Bedingung erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="e93bf-207">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="e93bf-208">Wenn beispielsweise `context.Remove()` zum Löschen einer Prinzipalentität aufgerufen wird, führt das dazu, dass auch alle zugehörigen erforderlichen Entitäten, die überwacht werden, sofort auf `Deleted` festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="e93bf-208">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="e93bf-209">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-209">**Why**</span></span>

<span data-ttu-id="e93bf-210">Diese Änderung wurde vorgenommen, um die Arbeit mit Datenbindungen und Überwachungsszenarios zu vereinfachen, in denen bekannt sein muss, welche Entitäten _vor_ dem Aufruf von `SaveChanges` gelöscht werden.</span><span class="sxs-lookup"><span data-stu-id="e93bf-210">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="e93bf-211">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-211">**Mitigations**</span></span>

<span data-ttu-id="e93bf-212">Sie können das vorherige Verhalten wiederherstellen, indem Sie für `context.ChangedTracker` die entsprechenden Einstellungen vornehmen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-212">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="e93bf-213">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="e93bf-213">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="e93bf-214">DeleteBehavior.Restrict verfügt über eine übersichtlichere Semantik</span><span class="sxs-lookup"><span data-stu-id="e93bf-214">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="e93bf-215">Issue #12661</span><span class="sxs-lookup"><span data-stu-id="e93bf-215">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="e93bf-216">Diese Änderung wird in Vorschauversion 5 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-216">This change will be introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="e93bf-217">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-217">**Old behavior**</span></span>

<span data-ttu-id="e93bf-218">Vor Version 3.0 wurden mit `DeleteBehavior.Restrict` Fremdschlüssel in der Datenbank mit `Restrict`-Semantik erstellt, es wurden aber auch unvorhergesehen interne Fixups geändert.</span><span class="sxs-lookup"><span data-stu-id="e93bf-218">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="e93bf-219">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-219">**New behavior**</span></span>

<span data-ttu-id="e93bf-220">Ab Version 3.0 wird mit `DeleteBehavior.Restrict` sichergestellt, dass Fremdschlüssel mit `Restrict`-Semantik erstellt werden – d. h. ohne Überlappungen; ausgenommen bei Einschränkungsverletzungen – und ohne Auswirkungen auf EF-interne Fixups.</span><span class="sxs-lookup"><span data-stu-id="e93bf-220">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="e93bf-221">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-221">**Why**</span></span>

<span data-ttu-id="e93bf-222">Diese Änderung wurde vorgenommen, um die Benutzerfreundlichkeit bei intuitiver Verwendung von `DeleteBehavior` ohne unerwartete Nebeneffekte zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="e93bf-222">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="e93bf-223">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-223">**Mitigations**</span></span>

<span data-ttu-id="e93bf-224">Sie können das vorherige Verhalten wiederherstellen, indem Sie `DeleteBehavior.ClientNoAction` verwenden.</span><span class="sxs-lookup"><span data-stu-id="e93bf-224">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="e93bf-225">Abfragetypen werden mit Entitätstypen zusammengeführt</span><span class="sxs-lookup"><span data-stu-id="e93bf-225">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="e93bf-226">Issue #14194</span><span class="sxs-lookup"><span data-stu-id="e93bf-226">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="e93bf-227">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-227">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="e93bf-228">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-228">**Old behavior**</span></span>

<span data-ttu-id="e93bf-229">Vor Version 3.0 wurden in EF Core [Abfragetypen](xref:core/modeling/query-types) dazu verwendet, Daten abzufragen, in denen kein Primärschlüssel auf strukturierte Weise festgelegt war.</span><span class="sxs-lookup"><span data-stu-id="e93bf-229">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="e93bf-230">Abfragetypen wurden also für die Zuordnung von Entitätstypen ohne Schlüssel verwendet (in der Regel in einer Sicht, in einigen Fällen aber auch in einer Tabelle), während reguläre Entitätstypen für einen verfügbaren Schlüssel verwendet wurden (in der Regel in einer Tabelle, in einigen Fällen aber auch in einer Sicht).</span><span class="sxs-lookup"><span data-stu-id="e93bf-230">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="e93bf-231">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-231">**New behavior**</span></span>

<span data-ttu-id="e93bf-232">Aus einem Abfragetyp wird nun einfach ein Entitätstyp ohne Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="e93bf-232">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="e93bf-233">Entitätstypen ohne Schlüssel besitzen die gleiche Funktionalität wie Abfragetypen in früheren Versionen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-233">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="e93bf-234">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-234">**Why**</span></span>

<span data-ttu-id="e93bf-235">Diese Änderung wurde vorgenommen, um Unklarheiten bei der Verwendung von Abfragetypen zu beseitigen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-235">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="e93bf-236">Abfragetypen sind schlüssellose Entitätstypen, die grundsätzlich schreibgeschützt sind. Sie sollten allerdings nicht allein wegen des Schreibschutzes verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="e93bf-236">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="e93bf-237">Des Weiteren werden sie oft Sichten zugeordnet, was aber nur daran liegt, dass für Letztere häufig keine Schlüssel definiert werden.</span><span class="sxs-lookup"><span data-stu-id="e93bf-237">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="e93bf-238">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-238">**Mitigations**</span></span>

<span data-ttu-id="e93bf-239">Die folgenden Teile der API sind durch die Änderungen veraltet:</span><span class="sxs-lookup"><span data-stu-id="e93bf-239">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="e93bf-240">**`ModelBuilder.Query<>()`**: Rufen Sie stattdessen `ModelBuilder.Entity<>().HasNoKey()` auf, um einen schlüssellosen Entitätstyp festzulegen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-240">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="e93bf-241">Dieses Verhalten wird nach wie vor nicht konventionsgemäß festgelegt, um Fehlkonfigurationen zu vermeiden, wenn ein Primärschlüssel erwartet wird, jedoch nicht mit der Konvention übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-241">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="e93bf-242">**`DbQuery<>`**: Verwenden Sie stattdessen `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="e93bf-242">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="e93bf-243">**`DbContext.Query<>()`**: Verwenden Sie stattdessen `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="e93bf-243">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="e93bf-244">Die Konfigurations-API für Beziehungen abhängiger (owned) Typen wurde geändert</span><span class="sxs-lookup"><span data-stu-id="e93bf-244">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="e93bf-245">[Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="e93bf-245">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="e93bf-246">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-246">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="e93bf-247">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-247">**Old behavior**</span></span>

<span data-ttu-id="e93bf-248">Vor Version 3.0 wurde in EF Core die abhängige Beziehung direkt nach dem Aufruf von `OwnsOne` oder `OwnsMany` konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="e93bf-248">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="e93bf-249">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-249">**New behavior**</span></span>

<span data-ttu-id="e93bf-250">Ab Version 3.0 von EF Core ist eine Fluent-API verfügbar, mit der über `WithOwner()` eine Navigationseigenschaft für den Besitzer konfiguriert werden kann.</span><span class="sxs-lookup"><span data-stu-id="e93bf-250">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="e93bf-251">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="e93bf-251">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="e93bf-252">Die Konfiguration für die Beziehung zwischen Besitzertyp und abhängigem Typ sollte nun ähnlich wie bei der Konfiguration anderer Beziehungen nach `WithOwner()` verkettet werden.</span><span class="sxs-lookup"><span data-stu-id="e93bf-252">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="e93bf-253">Die Konfiguration für den abhängigen Typ wird jedoch weiterhin nach `OwnsOne()/OwnsMany()` verkettet.</span><span class="sxs-lookup"><span data-stu-id="e93bf-253">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="e93bf-254">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="e93bf-254">For example:</span></span>

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

<span data-ttu-id="e93bf-255">Wenn Sie zusätzlich `Entity()`, `HasOne()`, oder `Set()` für das Ziel eines abhängigen Typs aufrufen, wird keine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="e93bf-255">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="e93bf-256">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-256">**Why**</span></span>

<span data-ttu-id="e93bf-257">Diese Änderung wurde vorgenommen, um eine deutlichere Trennung zwischen der Konfiguration des abhängigen Typs und der _Beziehung zum abhängigen Typ_ zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-257">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="e93bf-258">Dadurch werden für Methoden wie `HasForeignKey` Mehrdeutigkeiten und Unklarheiten beseitigt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-258">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="e93bf-259">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-259">**Mitigations**</span></span>

<span data-ttu-id="e93bf-260">Passen Sie für die Beziehungen von abhängigen Typen die Konfiguration so an, dass die neue Fluent-API wie im obigen Beispiel verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="e93bf-260">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="e93bf-261">Abhängige Entitäten, die die Tabelle gemeinsam mit dem Prinzipal verwenden, sind jetzt optional</span><span class="sxs-lookup"><span data-stu-id="e93bf-261">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="e93bf-262">Issue #9005</span><span class="sxs-lookup"><span data-stu-id="e93bf-262">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="e93bf-263">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-263">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="e93bf-264">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-264">**Old behavior**</span></span>

<span data-ttu-id="e93bf-265">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="e93bf-265">Consider the following model:</span></span>
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
<span data-ttu-id="e93bf-266">Wenn `OrderDetails` im Besitz von `Order` ist oder explizit derselben Tabelle zugeordnet ist, war vor Version 3.0 in EF Core immer eine `OrderDetails`-Instanz beim Hinzufügen eines neuen `Order`-Objekts erforderlich.</span><span class="sxs-lookup"><span data-stu-id="e93bf-266">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="e93bf-267">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-267">**New behavior**</span></span>

<span data-ttu-id="e93bf-268">Ab Version 3.0 bietet EF Core die Möglichkeit, `Order` ohne `OrderDetails` hinzuzufügen, und alle `OrderDetails`-Eigenschaften, mit Ausnahme des primären Schlüssels, werden Spalten zugeordnet, die NULL-Werte zulassen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-268">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="e93bf-269">Bei Abfragen von EF Core wird `OrderDetails` auf `null` festgelegt, wenn eine der erforderlichen Eigenschaften keinen Wert aufweist oder keine erforderlichen Eigenschaften außer dem primären Schlüssel vorhanden und alle Eigenschaften `null` sind.</span><span class="sxs-lookup"><span data-stu-id="e93bf-269">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="e93bf-270">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-270">**Mitigations**</span></span>

<span data-ttu-id="e93bf-271">Wenn Ihr Modell von der gemeinsamen Nutzung einer Tabelle mit allen optionalen Spalten abhängig ist, aber die Navigation, die darauf zeigt, wahrscheinlich nicht `null` ist, sollte die Anwendung für die Handhabung von Fällen geändert werden, in denen die Navigation `null` ist.</span><span class="sxs-lookup"><span data-stu-id="e93bf-271">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="e93bf-272">Wenn das nicht möglich ist, sollte dem Entitätstyp eine erforderliche Eigenschaft hinzugefügt werden oder mindestens einer Eigenschaft sollte ein Wert ungleich `null` zugewiesen sein.</span><span class="sxs-lookup"><span data-stu-id="e93bf-272">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="e93bf-273">Alle Entitäten, die eine Tabelle mit einer Spalte für das Parallelitätstoken gemeinsam verwenden, müssen diese einer Eigenschaft zuordnen</span><span class="sxs-lookup"><span data-stu-id="e93bf-273">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="e93bf-274">Issue #14154</span><span class="sxs-lookup"><span data-stu-id="e93bf-274">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="e93bf-275">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-275">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="e93bf-276">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-276">**Old behavior**</span></span>

<span data-ttu-id="e93bf-277">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="e93bf-277">Consider the following model:</span></span>
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
<span data-ttu-id="e93bf-278">Wenn `OrderDetails` im Besitz von `Order` ist oder explizit derselben Tabelle zugeordnet ist, wird vor Version 3.0 in EF Core durch das alleinige Aktualisieren von `OrderDetails` der `Version`-Wert auf dem Client nicht aktualisiert, und das nächste Update schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="e93bf-278">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="e93bf-279">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-279">**New behavior**</span></span>

<span data-ttu-id="e93bf-280">Ab Version 3.0 gibt EF Core den neuen `Version`-Wert an `Order` weiter, wenn dies Besitzer von `OrderDetails` ist.</span><span class="sxs-lookup"><span data-stu-id="e93bf-280">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="e93bf-281">Andernfalls wird eine Ausnahme während der Modellvalidierung ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="e93bf-281">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="e93bf-282">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-282">**Why**</span></span>

<span data-ttu-id="e93bf-283">Diese Änderung wurde vorgenommen, um einen veralteten Wert für ein Parallelitätstoken zu vermeiden, wenn nur eine der Entitäten, die derselben Tabelle zugeordnet sind, aktualisiert wird.</span><span class="sxs-lookup"><span data-stu-id="e93bf-283">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="e93bf-284">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-284">**Mitigations**</span></span>

<span data-ttu-id="e93bf-285">Alle Entitäten, die die Tabelle gemeinsam nutzen, müssen eine Eigenschaft enthalten, die der Spalte für das Parallelitätstoken zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="e93bf-285">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="e93bf-286">Es ist möglich, eine solche im Schattenzustand zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="e93bf-286">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="e93bf-287">Geerbte Eigenschaften von nicht zugeordneten Typen sind nun einer einzelnen Spalte für alle abgeleiteten Typen zugeordnet</span><span class="sxs-lookup"><span data-stu-id="e93bf-287">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="e93bf-288">Issue #13998</span><span class="sxs-lookup"><span data-stu-id="e93bf-288">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="e93bf-289">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-289">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="e93bf-290">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-290">**Old behavior**</span></span>

<span data-ttu-id="e93bf-291">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="e93bf-291">Consider the following model:</span></span>
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

<span data-ttu-id="e93bf-292">Vor Version 3.0 wurde in EF Core die Eigenschaft `ShippingAddress` standardmäßig separaten Spalten für `BulkOrder` und `Order` zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="e93bf-292">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="e93bf-293">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-293">**New behavior**</span></span>

<span data-ttu-id="e93bf-294">Ab Version 3.0 erstellt EF Core nur eine Spalte für `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="e93bf-294">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="e93bf-295">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-295">**Why**</span></span>

<span data-ttu-id="e93bf-296">Der alte Verhalten war unerwartet.</span><span class="sxs-lookup"><span data-stu-id="e93bf-296">The old behavoir was unexpected.</span></span>

<span data-ttu-id="e93bf-297">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-297">**Mitigations**</span></span>

<span data-ttu-id="e93bf-298">Die Eigenschaft kann noch immer explizit einer separaten Spalte für die abgeleiteten Typen zugeordnet werden:</span><span class="sxs-lookup"><span data-stu-id="e93bf-298">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="e93bf-299">Die Konvention zur Fremdschlüsseleigenschaft entspricht nicht mehr dem Namen der Prinzipaleigenschaft</span><span class="sxs-lookup"><span data-stu-id="e93bf-299">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="e93bf-300">Issue #13274</span><span class="sxs-lookup"><span data-stu-id="e93bf-300">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="e93bf-301">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-301">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="e93bf-302">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-302">**Old behavior**</span></span>

<span data-ttu-id="e93bf-303">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="e93bf-303">Consider the following model:</span></span>
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
<span data-ttu-id="e93bf-304">Vor Version 3.0 wurde in EF Core gemäß der Konvention die `CustomerId`-Eigenschaft für den Fremdschlüssel verwendet.</span><span class="sxs-lookup"><span data-stu-id="e93bf-304">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="e93bf-305">Wenn `Order` jedoch ein abhängiger Typ ist, wäre `CustomerId` der Primärschlüssel, was normalerweise nicht der Erwartungshaltung entspricht.</span><span class="sxs-lookup"><span data-stu-id="e93bf-305">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="e93bf-306">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-306">**New behavior**</span></span>

<span data-ttu-id="e93bf-307">Ab Version 3.0 werden in EF Core konventionsgemäß keine Eigenschaften mehr für Fremdschlüssel verwendet, wenn diese denselben Namen wie die Prinzipaleigenschaft besitzen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-307">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="e93bf-308">Die Muster für den Namen des Prinzipaltyps, der mit dem Namen der Prinzipaleigenschaft verkettet wird, und für den Navigationsnamen, der mit dem Namen der Prinzipaleigenschaft verkettet wird, werden jedoch weiterhin abgeglichen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-308">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="e93bf-309">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="e93bf-309">For example:</span></span>

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

<span data-ttu-id="e93bf-310">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-310">**Why**</span></span>

<span data-ttu-id="e93bf-311">Diese Änderung wurde vorgenommen, um zu vermeiden, dass versehentlich eine Primärschlüsseleigenschaft für den abhängigen Typ definiert wird.</span><span class="sxs-lookup"><span data-stu-id="e93bf-311">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="e93bf-312">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-312">**Mitigations**</span></span>

<span data-ttu-id="e93bf-313">Wenn die Eigenschaft als Fremdschlüssel und daher Teil des Primärschlüssels vorgesehen war, müssen Sie diese explizit als solche festlegen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-313">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="e93bf-314">Die Datenbankverbindung wird jetzt geschlossen, wenn sie nicht mehr verwendet wird, bevor TransactionScope abgeschlossen wurde</span><span class="sxs-lookup"><span data-stu-id="e93bf-314">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="e93bf-315">Issue #14218</span><span class="sxs-lookup"><span data-stu-id="e93bf-315">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="e93bf-316">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-316">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="e93bf-317">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-317">**Old behavior**</span></span>

<span data-ttu-id="e93bf-318">Wenn vor Version 3.0 in EF Core der Kontext die Verbindung in einem `TransactionScope` öffnet, bleibt die Verbindung geöffnet, während der aktuelle `TransactionScope` aktiv ist.</span><span class="sxs-lookup"><span data-stu-id="e93bf-318">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="e93bf-319">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-319">**New behavior**</span></span>

<span data-ttu-id="e93bf-320">Ab Version 3.0 schließt EF Core die Verbindung, sobald sie nicht mehr verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="e93bf-320">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="e93bf-321">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-321">**Why**</span></span>

<span data-ttu-id="e93bf-322">Diese Änderung ermöglicht die Verwendung mehrerer Kontexte in demselben `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="e93bf-322">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="e93bf-323">Das neue Verhalten entspricht auch EF6.</span><span class="sxs-lookup"><span data-stu-id="e93bf-323">The new behavior alose matches EF6.</span></span>

<span data-ttu-id="e93bf-324">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-324">**Mitigations**</span></span>

<span data-ttu-id="e93bf-325">Wenn die Verbindung geöffnet bleiben muss, wird durch einen expliziten Aufruf von `OpenConnection()` sichergestellt, dass EF Core diese nicht vorzeitig schließt:</span><span class="sxs-lookup"><span data-stu-id="e93bf-325">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="e93bf-326">Für jede Eigenschaft wird separat ein ganzzahliger speicherinterner Schlüssel generiert</span><span class="sxs-lookup"><span data-stu-id="e93bf-326">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="e93bf-327">Issue #6872</span><span class="sxs-lookup"><span data-stu-id="e93bf-327">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="e93bf-328">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-328">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="e93bf-329">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-329">**Old behavior**</span></span>

<span data-ttu-id="e93bf-330">Vor Version 3.0 wurde in EF Core ein gemeinsam verwendeter Wertegenerator für alle speicherinternen ganzzahligen Schlüsseleigenschaften verwendet.</span><span class="sxs-lookup"><span data-stu-id="e93bf-330">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="e93bf-331">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-331">**New behavior**</span></span>

<span data-ttu-id="e93bf-332">Ab Version 3.0 von EF Core wird für jede ganzzahlige Schlüsseleigenschaft ein eigener Wertegenerator bei der Verwendung der In-Memory Database verwendet.</span><span class="sxs-lookup"><span data-stu-id="e93bf-332">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="e93bf-333">Wenn die Datenbank gelöscht wird, wird die Schlüsselgenerierung für alle Tabellen zurückgesetzt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-333">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="e93bf-334">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-334">**Why**</span></span>

<span data-ttu-id="e93bf-335">Diese Änderung wurde vorgenommen, um die speicherinterne Schlüsselgenerierung enger mit der Schlüsselgenerierung für echte Datenbanken abzustimmen. Außerdem sollen bei Verwendung der In-Memory Database Tests leichter voneinander isoliert werden können.</span><span class="sxs-lookup"><span data-stu-id="e93bf-335">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="e93bf-336">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-336">**Mitigations**</span></span>

<span data-ttu-id="e93bf-337">Wenn Anwendungen von bestimmten speicherinternen Schlüsselwerten abhängig sind, funktionieren Erstere durch die eingeführte Änderung möglicherweise nicht mehr.</span><span class="sxs-lookup"><span data-stu-id="e93bf-337">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="e93bf-338">Versuchen Sie, Abhängigkeiten von bestimmten Schlüsselwerten zu vermeiden, oder passen Sie die Anwendung an das neue Verhalten an.</span><span class="sxs-lookup"><span data-stu-id="e93bf-338">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="e93bf-339">Unterstützungsfelder werden standardmäßig verwendet</span><span class="sxs-lookup"><span data-stu-id="e93bf-339">Backing fields are used by default</span></span>

[<span data-ttu-id="e93bf-340">Issue #12430</span><span class="sxs-lookup"><span data-stu-id="e93bf-340">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="e93bf-341">Diese Änderung wurde in Vorschauversion 2 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-341">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="e93bf-342">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-342">**Old behavior**</span></span>

<span data-ttu-id="e93bf-343">Vor Version 3.0 wurde in EF Core mithilfe der Getter- und Settermethoden standardmäßig der Eigenschaftswert gelesen und geschrieben. Das war selbst dann der Fall, wenn das Unterstützungsfeld für eine Eigenschaft nicht bekannt war.</span><span class="sxs-lookup"><span data-stu-id="e93bf-343">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="e93bf-344">Eine Ausnahme war die Abfrageausführung, bei der das Unterstützungsfeld direkt festgelegt wurde, wenn es bekannt war.</span><span class="sxs-lookup"><span data-stu-id="e93bf-344">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="e93bf-345">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-345">**New behavior**</span></span>

<span data-ttu-id="e93bf-346">Ab Version 3.0 wird in EF Core die Eigenschaft mithilfe des Unterstützungsfelds gelesen und geschrieben, wenn dieses für eine Eigenschaft bekannt ist.</span><span class="sxs-lookup"><span data-stu-id="e93bf-346">Starting with EF Core 3.0, if the backing field for a property is known, then will always read and write that property using the backing field.</span></span>
<span data-ttu-id="e93bf-347">Dies kann dazu führen, dass eine Anwendung nicht mehr funktioniert, wenn sie auf zusätzliches Verhalten angewiesen ist, das im Code der Getter- und Settermethoden definiert ist.</span><span class="sxs-lookup"><span data-stu-id="e93bf-347">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="e93bf-348">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-348">**Why**</span></span>

<span data-ttu-id="e93bf-349">Diese Änderung wurde vorgenommen, um zu verhindern, dass in EF Core fälschlicherweise immer dann Geschäftslogik ausgelöst wird, wenn Datenbankvorgänge für Entitäten durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="e93bf-349">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="e93bf-350">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-350">**Mitigations**</span></span>

<span data-ttu-id="e93bf-351">Sie können das Verhalten vor Version 3.0 wiederherstellen, indem Sie über die modelBuilder-Fluent-API den Zugriffsmodus für die Eigenschaft konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="e93bf-351">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="e93bf-352">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="e93bf-352">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="e93bf-353">Eine Ausnahme wird ausgelöst, wenn mehrere kompatible Unterstützungsfelder gefunden werden</span><span class="sxs-lookup"><span data-stu-id="e93bf-353">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="e93bf-354">Issue #12523</span><span class="sxs-lookup"><span data-stu-id="e93bf-354">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="e93bf-355">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-355">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="e93bf-356">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-356">**Old behavior**</span></span>

<span data-ttu-id="e93bf-357">Vor Version 3.0 wurde in EF Core ein Unterstützungsfeld auf Grundlage einer Rangfolge ausgewählt, wenn die Regeln zum Suchen eines Eigenschaftsfelds auf mehrere Felder zutrafen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-357">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="e93bf-358">Dabei konnte es vorkommen, dass in mehrdeutigen Fällen das falsche Feld verwendet wurde.</span><span class="sxs-lookup"><span data-stu-id="e93bf-358">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="e93bf-359">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-359">**New behavior**</span></span>

<span data-ttu-id="e93bf-360">Ab Version 3.0 wird in EF Core eine Ausnahme ausgelöst, wenn sich mehrere Felder auf dieselbe Eigenschaft beziehen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-360">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="e93bf-361">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-361">**Why**</span></span>

<span data-ttu-id="e93bf-362">Diese Änderung wurde vorgenommen, um zu vermeiden, dass automatisch ein falsches Feld verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="e93bf-362">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="e93bf-363">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-363">**Mitigations**</span></span>

<span data-ttu-id="e93bf-364">Für Eigenschaften mit mehrdeutigen Unterstützungsfeldern muss das zu verwendende Feld explizit festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="e93bf-364">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="e93bf-365">Mit der Fluent-API ist dies beispielsweise wie folgt möglich:</span><span class="sxs-lookup"><span data-stu-id="e93bf-365">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="e93bf-366">„Nur-Feld“-Eigenschaftsnamen sollten dem Feldnamen entsprechen</span><span class="sxs-lookup"><span data-stu-id="e93bf-366">Field-only property names should match the field name</span></span>

<span data-ttu-id="e93bf-367">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-367">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="e93bf-368">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-368">**Old behavior**</span></span>

<span data-ttu-id="e93bf-369">Vor Version 3.0 konnte in EF Core eine Eigenschaft durch einen Zeichenfolgenwert angegeben werden, und wenn keine Eigenschaft mit diesem Namen für den CLR-Typ gefunden wurde, hat EF Core versucht, mithilfe von Übereinstimmungsregeln ein entsprechendes Feld zu finden.</span><span class="sxs-lookup"><span data-stu-id="e93bf-369">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convetion rules.</span></span>
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

<span data-ttu-id="e93bf-370">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-370">**New behavior**</span></span>

<span data-ttu-id="e93bf-371">Ab Version 3.0 muss in EF Core eine „Nur-Feld“-Eigenschaft mit dem Namen des Felds genau übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-371">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="e93bf-372">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-372">**Why**</span></span>

<span data-ttu-id="e93bf-373">Diese Änderung wurde vorgenommen, um zu vermeiden, dass das gleiche Feld für zwei Eigenschaften mit ähnlichem Namen verwendet wird. Durch die Änderung entsprechen nun auch die Übereinstimmungsregeln für „Nur-Feld“-Eigenschaften den Regeln für Eigenschaften, die CLR-Eigenschaften zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="e93bf-373">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="e93bf-374">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-374">**Mitigations**</span></span>

<span data-ttu-id="e93bf-375">„Nur-Feld“-Eigenschaften müssen so benannt werden wie das Feld, dem sie zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="e93bf-375">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="e93bf-376">In einer späteren Vorschauversion von EF Core 3.0 soll das explizite Konfigurieren eines Feldnamens, der sich vom Eigenschaftsnamen unterscheidet, erneut aktiviert werden:</span><span class="sxs-lookup"><span data-stu-id="e93bf-376">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="e93bf-377">Von AddDbContext/AddDbContextPool werden AddLogging und AddMemoryCache nicht mehr aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-377">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="e93bf-378">Issue #14756</span><span class="sxs-lookup"><span data-stu-id="e93bf-378">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="e93bf-379">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-379">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="e93bf-380">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-380">**Old behavior**</span></span>

<span data-ttu-id="e93bf-381">Vor EF Core 3.0 wurden beim Aufruf von `AddDbContext` oder `AddDbContextPool` durch Aufrufe von [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) und [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache) auch Protokollierungs- und Arbeitsspeichercaching-Dienste bei DI registriert.</span><span class="sxs-lookup"><span data-stu-id="e93bf-381">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="e93bf-382">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-382">**New behavior**</span></span>

<span data-ttu-id="e93bf-383">Ab EF Core 3.0 werden diese Dienste durch `AddDbContext` und `AddDbContextPool` nicht mehr bei Dependency Injection (DI) registriert.</span><span class="sxs-lookup"><span data-stu-id="e93bf-383">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="e93bf-384">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-384">**Why**</span></span>

<span data-ttu-id="e93bf-385">Für EF Core 3.0 ist es nicht erforderlich, dass diese Dienste im DI-Container der Anwendung enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="e93bf-385">EF Core 3.0 does not require that these services are in the application's DI cotainer.</span></span> <span data-ttu-id="e93bf-386">Wenn `ILoggerFactory` jedoch im DI-Container der Anwendung registriert ist, wird sie nach wie vor von EF Core verwendet.</span><span class="sxs-lookup"><span data-stu-id="e93bf-386">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="e93bf-387">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-387">**Mitigations**</span></span>

<span data-ttu-id="e93bf-388">Wenn Ihre Anwendung diese Dienste benötigt, registrieren Sie sie explizit mit [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) oder [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache) beim DI-Container.</span><span class="sxs-lookup"><span data-stu-id="e93bf-388">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="e93bf-389">DbContext.Entry erkennt nun mit DetectChanges lokale Änderungen</span><span class="sxs-lookup"><span data-stu-id="e93bf-389">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="e93bf-390">Issue #13552</span><span class="sxs-lookup"><span data-stu-id="e93bf-390">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="e93bf-391">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-391">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="e93bf-392">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-392">**Old behavior**</span></span>

<span data-ttu-id="e93bf-393">Vor Version 3.0 führte in EF Core ein Aufruf von `DbContext.Entry` dazu, dass Änderungen an allen überwachten Entitäten erkannt wurden.</span><span class="sxs-lookup"><span data-stu-id="e93bf-393">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="e93bf-394">Dadurch wurde sichergestellt, dass der Zustand in `EntityEntry` immer aktuell war.</span><span class="sxs-lookup"><span data-stu-id="e93bf-394">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="e93bf-395">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-395">**New behavior**</span></span>

<span data-ttu-id="e93bf-396">Ab Version 3.0 werden in EF Core Änderungen bei einem Aufruf von `DbContext.Entry` nur in der angegebenen Entität und in allen überwachten Prinzipalentitäten erkannt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-396">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="e93bf-397">Änderungen an anderer Stelle werden daher durch den Aufruf dieser Methode möglicherweise nicht erkannt, was sich auf den Anwendungszustand auswirken kann.</span><span class="sxs-lookup"><span data-stu-id="e93bf-397">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="e93bf-398">Beachten Sie, dass selbst die Erkennung lokaler Änderungen deaktiviert wird, wenn `ChangeTracker.AutoDetectChangesEnabled` auf `false` festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="e93bf-398">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="e93bf-399">Andere Methoden wie `ChangeTracker.Entries` und `SaveChanges`, die eine Änderungserkennung auslösen, führen nach wie vor zu einem vollständigen `DetectChanges`-Vorgang für alle überwachten Entitäten.</span><span class="sxs-lookup"><span data-stu-id="e93bf-399">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="e93bf-400">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-400">**Why**</span></span>

<span data-ttu-id="e93bf-401">Diese Änderung wurde vorgenommen, um die Leistung bei der standardmäßigen Verwendung von `context.Entry` zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="e93bf-401">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="e93bf-402">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-402">**Mitigations**</span></span>

<span data-ttu-id="e93bf-403">Rufen Sie `ChgangeTracker.DetectChanges()` explizit vor dem Aufruf von `Entry` auf, wenn das Verhalten vor Version 3.0 beibehalten werden soll.</span><span class="sxs-lookup"><span data-stu-id="e93bf-403">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="e93bf-404">Schlüssel aus Zeichenfolgen und Bytearrays werden standardmäßig nicht vom Client generiert</span><span class="sxs-lookup"><span data-stu-id="e93bf-404">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="e93bf-405">Issue #14617</span><span class="sxs-lookup"><span data-stu-id="e93bf-405">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="e93bf-406">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-406">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="e93bf-407">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-407">**Old behavior**</span></span>

<span data-ttu-id="e93bf-408">Vor Version 3.0 konnten in EF Core `string`- und `byte[]`-Schlüsseleigenschaften verwendet werden, ohne dass explizit ein anderer Wert als NULL festgelegt werden musste.</span><span class="sxs-lookup"><span data-stu-id="e93bf-408">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="e93bf-409">In diesem Fall wurde der Schlüsselwert auf dem Client als GUID generiert und für `byte[]` in Bytes serialisiert.</span><span class="sxs-lookup"><span data-stu-id="e93bf-409">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="e93bf-410">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-410">**New behavior**</span></span>

<span data-ttu-id="e93bf-411">Ab Version 3.0 wird in EF Core eine Ausnahme ausgelöst, wenn kein Schlüsselwert festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="e93bf-411">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="e93bf-412">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-412">**Why**</span></span>

<span data-ttu-id="e93bf-413">Diese Änderung wurde vorgenommen, da vom Client generierte `string`/`byte[]`-Werte in der Regel nicht sinnvoll sind. Das Standardverhalten führte außerdem zu kaum nachvollziehbaren generierten Schlüsselwerten.</span><span class="sxs-lookup"><span data-stu-id="e93bf-413">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="e93bf-414">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-414">**Mitigations**</span></span>

<span data-ttu-id="e93bf-415">Wenn Sie das Verhalten vor Version 3.0 nutzen möchten, müssen Sie explizit festlegen, dass für die Schlüsseleigenschaften generierte Werte verwendet sollen, wenn kein anderer Nicht-NULL-Wert festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="e93bf-415">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="e93bf-416">Dies ist beispielsweise mit der Fluent-API möglich:</span><span class="sxs-lookup"><span data-stu-id="e93bf-416">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="e93bf-417">Alternativ bieten sich auch Datenanmerkungen an:</span><span class="sxs-lookup"><span data-stu-id="e93bf-417">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="e93bf-418">ILoggerFactory ist nun ein bereichsbezogener Dienst</span><span class="sxs-lookup"><span data-stu-id="e93bf-418">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="e93bf-419">Issue #14698</span><span class="sxs-lookup"><span data-stu-id="e93bf-419">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="e93bf-420">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-420">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="e93bf-421">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-421">**Old behavior**</span></span>

<span data-ttu-id="e93bf-422">Vor Version 3.0 wurde `ILoggerFactory` in EF Core als Singletondienst registriert.</span><span class="sxs-lookup"><span data-stu-id="e93bf-422">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="e93bf-423">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-423">**New behavior**</span></span>

<span data-ttu-id="e93bf-424">Ab Version 3.0 wird in EF Core `ILoggerFactory` als bereichsbezogener Dienst registriert.</span><span class="sxs-lookup"><span data-stu-id="e93bf-424">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="e93bf-425">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-425">**Why**</span></span>

<span data-ttu-id="e93bf-426">Diese Änderung wurde vorgenommen, um einer `DbContext`-Instanz eine Protokollierung zuordnen zu können. Dadurch werden weitere Funktionen bereitgestellt. In einigen Fällen wird außerdem schädliches Verhalten wie etwa die schnelle Zunahme von internen Dienstanbietern beseitigt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-426">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="e93bf-427">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-427">**Mitigations**</span></span>

<span data-ttu-id="e93bf-428">Diese Änderung wirkt sich nur dann auf den Anwendungscode aus, wenn benutzerdefinierte Dienste auf dem internen Dienstanbieter von EF Core registriert und verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="e93bf-428">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="e93bf-429">Ein solches Szenario ist aber unüblich.</span><span class="sxs-lookup"><span data-stu-id="e93bf-429">This isn't common.</span></span>
<span data-ttu-id="e93bf-430">In einem derartigen Fall funktioniert fast alles wie vorher. Singletondienste, die von `ILoggerFactory` abhängig sind, müssen jedoch so angepasst werden, dass `ILoggerFactory` auf andere Weise abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="e93bf-430">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="e93bf-431">Erstellen Sie in diesen Situationen bitte im [GitHub-Issuetracker für EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) ein Issue, in dem Sie beschreiben, wie Sie `ILoggerFactory` verwenden. So können wir in Zukunft erneute Breaking Changes vermeiden.</span><span class="sxs-lookup"><span data-stu-id="e93bf-431">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="e93bf-432">IDbContextOptionsExtensionWithDebugInfo wurde mit IDbContextOptionsExtension zusammengeführt</span><span class="sxs-lookup"><span data-stu-id="e93bf-432">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="e93bf-433">Issue #13552</span><span class="sxs-lookup"><span data-stu-id="e93bf-433">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="e93bf-434">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-434">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="e93bf-435">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-435">**Old behavior**</span></span>

<span data-ttu-id="e93bf-436">`IDbContextOptionsExtensionWithDebugInfo` war eine zusätzliche optionale Schnittstelle, die von `IDbContextOptionsExtension` abgeleitet wurde, um während des 2.x-Releasezyklus Breaking Changes an der Schnittstelle zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="e93bf-436">`IDbContextOptionsExtensionWithDebugInfo` was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

<span data-ttu-id="e93bf-437">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-437">**New behavior**</span></span>

<span data-ttu-id="e93bf-438">Die Schnittstellen wurden in `IDbContextOptionsExtension` zusammengeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-438">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

<span data-ttu-id="e93bf-439">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-439">**Why**</span></span>

<span data-ttu-id="e93bf-440">Diese Änderung wurde vorgenommen, da es sich im Prinzip um eine Schnittstelle handelt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-440">This change was made because the interfaces are conceptually one.</span></span>

<span data-ttu-id="e93bf-441">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-441">**Mitigations**</span></span>

<span data-ttu-id="e93bf-442">Alle Implementierungen von `IDbContextOptionsExtension` müssen aktualisiert werden, damit der neue Member unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="e93bf-442">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="e93bf-443">Für Proxys mit Lazy Loading (trägem Laden) gilt nicht mehr die Annahme, dass Navigationseigenschaften vollständig geladen sind</span><span class="sxs-lookup"><span data-stu-id="e93bf-443">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="e93bf-444">Issue #12780</span><span class="sxs-lookup"><span data-stu-id="e93bf-444">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="e93bf-445">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-445">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="e93bf-446">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-446">**Old behavior**</span></span>

<span data-ttu-id="e93bf-447">Vor Version 3.0 gab es in EF Core nach der Freigabe eines `DbContext`-Objekts keine Möglichkeit, zu ermitteln, ob eine aus dem Kontext abgerufene Navigationseigenschaft für eine Entität vollständig geladen war.</span><span class="sxs-lookup"><span data-stu-id="e93bf-447">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="e93bf-448">Stattdessen galt für Proxys die Annahme, dass eine Verweisnavigation geladen ist, falls für diese ein anderer Wert als NULL vorliegt. Außerdem wurde davon ausgegangen, dass eine Sammlungsnavigation geladen ist, wenn sie nicht leer ist.</span><span class="sxs-lookup"><span data-stu-id="e93bf-448">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="e93bf-449">In diesen Fällen entsprach das Lazy Loading einer Nulloperation („No-Op“).</span><span class="sxs-lookup"><span data-stu-id="e93bf-449">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="e93bf-450">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-450">**New behavior**</span></span>

<span data-ttu-id="e93bf-451">Ab Version 3.0 überwachen in EF Core Proxys, ob eine Navigationseigenschaft geladen ist.</span><span class="sxs-lookup"><span data-stu-id="e93bf-451">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="e93bf-452">Wenn also auf eine Navigationseigenschaft zugegriffen werden soll, die nach der Freigabe des Kontexts geladen wird, ist dies immer eine No-Op-Operation. Das ist selbst dann der Fall, wenn die geladene Navigation leer oder NULL ist.</span><span class="sxs-lookup"><span data-stu-id="e93bf-452">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="e93bf-453">Wenn umgekehrt auf eine nicht geladene Navigationseigenschaft zugegriffen werden soll und der Kontext bereits freigegeben wurde, wird eine Ausnahme ausgelöst. Dieser Fall tritt auch dann ein, wenn die Navigationseigenschaft eine nicht leere Sammlung ist.</span><span class="sxs-lookup"><span data-stu-id="e93bf-453">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="e93bf-454">In dieser Situation wird also vom Anwendungscode versucht, zu einem ungültigen Zeitpunkt Lazy Loading durchzuführen. Die Anwendung sollte so angepasst werden, dass dieser Vorgang vermieden wird.</span><span class="sxs-lookup"><span data-stu-id="e93bf-454">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="e93bf-455">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-455">**Why**</span></span>

<span data-ttu-id="e93bf-456">Diese Änderung wurde vorgenommen, um das Verhalten beim Lazy Loading einer freigegebenen `DbContext`-Instanz konsistent und korrekt zu gestalten.</span><span class="sxs-lookup"><span data-stu-id="e93bf-456">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="e93bf-457">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-457">**Mitigations**</span></span>

<span data-ttu-id="e93bf-458">Aktualisieren Sie den Anwendungscode so, dass kein Lazy Loading durchgeführt wird, nachdem der Kontext freigegeben wurde. Alternativ können Sie auch, wie in der Ausnahmemeldung beschrieben wird, eine No-Op-Operation festlegen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-458">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="e93bf-459">Die Erstellung zu vieler interner Dienstanbieter führt standardmäßig zu einem Fehler</span><span class="sxs-lookup"><span data-stu-id="e93bf-459">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="e93bf-460">Issue #10236</span><span class="sxs-lookup"><span data-stu-id="e93bf-460">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="e93bf-461">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-461">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="e93bf-462">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-462">**Old behavior**</span></span>

<span data-ttu-id="e93bf-463">Vor Version 3.0 wurde in EF Core eine Warnung für eine Anwendung protokolliert, die unverhältnismäßig viele interne Dienstanbieter erstellte.</span><span class="sxs-lookup"><span data-stu-id="e93bf-463">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="e93bf-464">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-464">**New behavior**</span></span>

<span data-ttu-id="e93bf-465">Ab Version 3.0 wird diese Warnung in EF Core als Fehler betrachtet, und eine Ausnahme wird ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="e93bf-465">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="e93bf-466">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-466">**Why**</span></span>

<span data-ttu-id="e93bf-467">Diese Änderung wurde vorgenommen, damit auf den oben beschriebenen Fall explizit hingewiesen wird. So soll die Entwicklung von besserem Anwendungscode erleichtert werden.</span><span class="sxs-lookup"><span data-stu-id="e93bf-467">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="e93bf-468">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-468">**Mitigations**</span></span>

<span data-ttu-id="e93bf-469">Wenn dieser Fehler auftritt, müssen Sie die Grundursache ermitteln und die Erstellung vieler interner Dienstanbieter unterbinden.</span><span class="sxs-lookup"><span data-stu-id="e93bf-469">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="e93bf-470">Sie können den Fehler allerdings auch wieder in eine Warnung konvertierten (oder ihn ignorieren), indem Sie `DbContextOptionsBuilder` konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="e93bf-470">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="e93bf-471">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="e93bf-471">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="e93bf-472">Neues Verhalten für HasOne/HasMany bei Aufruf mit einer einzelnen Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="e93bf-472">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="e93bf-473">Issue #9171</span><span class="sxs-lookup"><span data-stu-id="e93bf-473">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="e93bf-474">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-474">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="e93bf-475">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-475">**Old behavior**</span></span>

<span data-ttu-id="e93bf-476">Vor EF Core 3.0 wurde Code, der `HasOne` oder `HasMany` mit einer einzelnen Zeichenfolge aufruft, auf irritierende Weise interpretiert.</span><span class="sxs-lookup"><span data-stu-id="e93bf-476">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpretted in a confusing way.</span></span>
<span data-ttu-id="e93bf-477">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="e93bf-477">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="e93bf-478">Der Code scheint `Samurai` mit einem anderen Entitätstyp über die `Entrance`-Navigationseigenschaft zu verknüpfen, die möglicherweise privat ist.</span><span class="sxs-lookup"><span data-stu-id="e93bf-478">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="e93bf-479">Tatsächlich versucht dieser Code, eine Beziehung zu einem Entitätstyp namens `Entrance` ohne Navigationseigenschaft herzustellen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-479">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="e93bf-480">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-480">**New behavior**</span></span>

<span data-ttu-id="e93bf-481">Ab EF Core 3.0 führt der obige Code jetzt die erwartete Aktion durch.</span><span class="sxs-lookup"><span data-stu-id="e93bf-481">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="e93bf-482">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-482">**Why**</span></span>

<span data-ttu-id="e93bf-483">Das alte Verhalten war sehr verwirrend, vor allem beim Lesen des Konfigurationscodes und bei der Suche nach Fehlern.</span><span class="sxs-lookup"><span data-stu-id="e93bf-483">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="e93bf-484">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-484">**Mitigations**</span></span>

<span data-ttu-id="e93bf-485">Dies führt nur zu Fehlern in Anwendungen, die Beziehungen explizit unter Verwendung von Zeichenfolgen für Typnamen und ohne explizite Angabe der Navigationseigenschaft konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="e93bf-485">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="e93bf-486">Dies ist nicht üblich.</span><span class="sxs-lookup"><span data-stu-id="e93bf-486">This is not common.</span></span>
<span data-ttu-id="e93bf-487">Das vorherige Verhalten kann durch explizite Übergabe von `null` für den Namen der Navigationseigenschaft erzielt werden.</span><span class="sxs-lookup"><span data-stu-id="e93bf-487">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="e93bf-488">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="e93bf-488">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="e93bf-489">Der Rückgabetyp für mehrere asynchrone Methoden wurde von Task in ValueTask geändert</span><span class="sxs-lookup"><span data-stu-id="e93bf-489">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="e93bf-490">Issue #15184</span><span class="sxs-lookup"><span data-stu-id="e93bf-490">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="e93bf-491">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-491">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="e93bf-492">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-492">**Old behavior**</span></span>

<span data-ttu-id="e93bf-493">Die folgenden asynchronen Methoden gaben zuvor `Task<T>` zurück:</span><span class="sxs-lookup"><span data-stu-id="e93bf-493">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="e93bf-494">`ValueGenerator.NextValueAsync()` (und abgeleitete Klassen)</span><span class="sxs-lookup"><span data-stu-id="e93bf-494">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="e93bf-495">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-495">**New behavior**</span></span>

<span data-ttu-id="e93bf-496">Die oben genannten Methoden geben nun `ValueTask<T>` über dieselbe `T` wie zuvor zurück.</span><span class="sxs-lookup"><span data-stu-id="e93bf-496">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="e93bf-497">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-497">**Why**</span></span>

<span data-ttu-id="e93bf-498">Durch diese Änderung verringert sich die Anzahl der Heapzuordnungen, die beim Aufrufen dieser Methoden entstehen, und dies führt zu einer Verbesserung der allgemeinen Leistung.</span><span class="sxs-lookup"><span data-stu-id="e93bf-498">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="e93bf-499">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-499">**Mitigations**</span></span>

<span data-ttu-id="e93bf-500">Anwendungen, die einfach die oben genannten APIs erwarten, müssen nur neu kompiliert werden – Quelländerungen sind nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="e93bf-500">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="e93bf-501">Eine komplexere Verwendung (z.B. Übergeben der zurückgegebenen `Task` an `Task.WhenAny()`) erfordern in der Regel, dass die zurückgegebene `ValueTask<T>` durch einen Aufruf von `AsTask()` in `Task<T>` konvertiert wird.</span><span class="sxs-lookup"><span data-stu-id="e93bf-501">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="e93bf-502">Beachten Sie, dass dadurch die mit dieser Änderung verbundene Verringerung der Zuordnungen aufgehoben wird.</span><span class="sxs-lookup"><span data-stu-id="e93bf-502">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="e93bf-503">Die Anmerkung Relational:TypeMapping heißt nun nur noch TypeMapping</span><span class="sxs-lookup"><span data-stu-id="e93bf-503">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="e93bf-504">Issue #9913</span><span class="sxs-lookup"><span data-stu-id="e93bf-504">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="e93bf-505">Diese Änderung wurde in Vorschauversion 2 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-505">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="e93bf-506">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-506">**Old behavior**</span></span>

<span data-ttu-id="e93bf-507">Der Anmerkungsname für Typzuordnungsanmerkungen war bisher Relational:TypeMapping.</span><span class="sxs-lookup"><span data-stu-id="e93bf-507">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="e93bf-508">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-508">**New behavior**</span></span>

<span data-ttu-id="e93bf-509">Der Anmerkungsname für Typzuordnungsanmerkungen lautet nun TypeMapping.</span><span class="sxs-lookup"><span data-stu-id="e93bf-509">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="e93bf-510">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-510">**Why**</span></span>

<span data-ttu-id="e93bf-511">Typzuordnungen werden nicht mehr ausschließlich für Anbieter relationaler Datenbanken verwendet.</span><span class="sxs-lookup"><span data-stu-id="e93bf-511">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="e93bf-512">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-512">**Mitigations**</span></span>

<span data-ttu-id="e93bf-513">Diese Änderung ist nur dann ein Breaking Change, wenn Anwendungen direkt über eine Anmerkung auf die Typzuordnung zugreifen. Dieses Szenario ist jedoch unüblich.</span><span class="sxs-lookup"><span data-stu-id="e93bf-513">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="e93bf-514">Verzichten Sie daher darauf, die Anmerkung direkt zu verwenden, und greifen Sie stattdessen über die Fluent-API auf Typzuordnungen zu, um das Problem zu beheben.</span><span class="sxs-lookup"><span data-stu-id="e93bf-514">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="e93bf-515">ToTable löst für einen abgeleiteten Typ eine Ausnahme aus</span><span class="sxs-lookup"><span data-stu-id="e93bf-515">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="e93bf-516">Issue #11811</span><span class="sxs-lookup"><span data-stu-id="e93bf-516">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="e93bf-517">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-517">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="e93bf-518">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-518">**Old behavior**</span></span>

<span data-ttu-id="e93bf-519">Vor Version 3.0 wurde in EF Core der Aufruf von `ToTable()` für einen abgeleiteten Typ ignoriert, da TPH die einzige Strategie zur Vererbungszuordnung war und diese in unzulässigen Fällen angewendet wurde.</span><span class="sxs-lookup"><span data-stu-id="e93bf-519">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="e93bf-520">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-520">**New behavior**</span></span>

<span data-ttu-id="e93bf-521">Ab Version 3.0 wird in EF Core eine Ausnahme ausgelöst, um später eine unerwartete Zuordnung zu vermeiden, wenn `ToTable()` für einen abgeleiteten Typ aufgerufen wird. Dieser Schritt dient auch als Vorbereitung für die TPT- und TPC-Unterstützung, die in einem künftigen Release folgen wird.</span><span class="sxs-lookup"><span data-stu-id="e93bf-521">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="e93bf-522">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-522">**Why**</span></span>

<span data-ttu-id="e93bf-523">Derzeit ist es nicht zulässig, einen abgeleiteten Typ einer anderen Tabelle zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-523">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="e93bf-524">Durch diese Änderung werden Breaking Changes vermieden, wenn der beschriebene Vorgang in Zukunft zulässig wird.</span><span class="sxs-lookup"><span data-stu-id="e93bf-524">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="e93bf-525">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-525">**Mitigations**</span></span>

<span data-ttu-id="e93bf-526">Entfernen Sie alle Zuordnungen von abgeleiteten Typen zu anderen Tabellen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-526">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="e93bf-527">ForSqlServerHasIndex wurde durch HasIndex ersetzt</span><span class="sxs-lookup"><span data-stu-id="e93bf-527">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="e93bf-528">Issue #12366</span><span class="sxs-lookup"><span data-stu-id="e93bf-528">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="e93bf-529">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-529">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="e93bf-530">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-530">**Old behavior**</span></span>

<span data-ttu-id="e93bf-531">Vor Version 3.0 konnten in EF Core mit `ForSqlServerHasIndex().ForSqlServerInclude()` Spalten konfiguriert werden, die mit `INCLUDE` verwendet wurden.</span><span class="sxs-lookup"><span data-stu-id="e93bf-531">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="e93bf-532">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-532">**New behavior**</span></span>

<span data-ttu-id="e93bf-533">Ab Version 3.0 wird in EF Core die Nutzung von `Include` für einen Index auf der relationalen Ebene unterstützt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-533">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="e93bf-534">Verwenden Sie `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="e93bf-534">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="e93bf-535">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-535">**Why**</span></span>

<span data-ttu-id="e93bf-536">Diese Änderung wurde vorgenommen, um die API für Indizes mit `Include` an einer zentralen Stelle für alle Datenbankanbieter zusammenzuführen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-536">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="e93bf-537">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-537">**Mitigations**</span></span>

<span data-ttu-id="e93bf-538">Verwenden Sie wie oben beschrieben die neue API.</span><span class="sxs-lookup"><span data-stu-id="e93bf-538">Use the new API, as shown above.</span></span>

## <a name="metadata-api-changes"></a><span data-ttu-id="e93bf-539">Metadaten-API-Änderungen</span><span class="sxs-lookup"><span data-stu-id="e93bf-539">Metadata API changes</span></span>

[<span data-ttu-id="e93bf-540">Issue #214</span><span class="sxs-lookup"><span data-stu-id="e93bf-540">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="e93bf-541">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-541">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="e93bf-542">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-542">**New behavior**</span></span>

<span data-ttu-id="e93bf-543">Die folgenden Eigenschaften wurden in Erweiterungsmethoden konvertiert:</span><span class="sxs-lookup"><span data-stu-id="e93bf-543">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="e93bf-544">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-544">**Why**</span></span>

<span data-ttu-id="e93bf-545">Mit dieser Änderung wird die Implementierung der oben genannten Schnittstellen vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="e93bf-545">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="e93bf-546">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-546">**Mitigations**</span></span>

<span data-ttu-id="e93bf-547">Verwenden Sie die neuen Erweiterungsmethoden.</span><span class="sxs-lookup"><span data-stu-id="e93bf-547">Use the new extension methods.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="e93bf-548">EF Core sendet keine PRAGMA-Anweisungen mehr, um Fremdschlüssel in SQLite zu erzwingen</span><span class="sxs-lookup"><span data-stu-id="e93bf-548">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="e93bf-549">Issue #12151</span><span class="sxs-lookup"><span data-stu-id="e93bf-549">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="e93bf-550">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-550">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="e93bf-551">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-551">**Old behavior**</span></span>

<span data-ttu-id="e93bf-552">Vor Version 3.0 wurden in EF Core `PRAGMA foreign_keys = 1`-Anweisungen gesendet, wenn eine Verbindung mit SQLite hergestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="e93bf-552">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="e93bf-553">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-553">**New behavior**</span></span>

<span data-ttu-id="e93bf-554">Ab Version 3.0 werden in EF Core keine `PRAGMA foreign_keys = 1`-Anweisungen mehr gesendet, wenn eine Verbindung mit SQLite hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="e93bf-554">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="e93bf-555">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-555">**Why**</span></span>

<span data-ttu-id="e93bf-556">Diese Änderung wurde vorgenommen, da von EF Core standardmäßig `SQLitePCLRaw.bundle_e_sqlite3` verwendet wird. Dadurch ist die Erzwingung von Fremdschlüsseln standardmäßig aktiviert und muss nicht jedes Mal explizit aktiviert werden, wenn eine Verbindung hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="e93bf-556">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="e93bf-557">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-557">**Mitigations**</span></span>

<span data-ttu-id="e93bf-558">Fremdschlüssel sind in SQLitePCLRaw.bundle_e_sqlite3 standardmäßig aktiviert. Dieses Bundle wird standardmäßig von EF Core verwendet.</span><span class="sxs-lookup"><span data-stu-id="e93bf-558">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="e93bf-559">In anderen Fällen können Fremdschlüssel durch die Angabe `Foreign Keys=True` in der Verbindungszeichenfolge aktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="e93bf-559">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="e93bf-560">SQLitePCLRaw.bundle_e_sqlite3 ist nun eine Abhängigkeit von Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="e93bf-560">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="e93bf-561">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-561">**Old behavior**</span></span>

<span data-ttu-id="e93bf-562">Vor Version 3.0 wurde von EF Core `SQLitePCLRaw.bundle_green` verwendet.</span><span class="sxs-lookup"><span data-stu-id="e93bf-562">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="e93bf-563">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-563">**New behavior**</span></span>

<span data-ttu-id="e93bf-564">Ab Version 3.0 wird von EF Core `SQLitePCLRaw.bundle_e_sqlite3` verwendet.</span><span class="sxs-lookup"><span data-stu-id="e93bf-564">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="e93bf-565">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-565">**Why**</span></span>

<span data-ttu-id="e93bf-566">Diese Änderung wurde vorgenommen, damit die unter iOS verwendete Version von SQLite sich konsistent zu Versionen auf anderen Plattformen verhält.</span><span class="sxs-lookup"><span data-stu-id="e93bf-566">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="e93bf-567">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-567">**Mitigations**</span></span>

<span data-ttu-id="e93bf-568">Konfigurieren Sie `Microsoft.Data.Sqlite` so, dass ein anderes `SQLitePCLRaw`-Bundle verwendet wird, um die native SQLite-Version unter iOS zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-568">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="e93bf-569">GUID-Werte werden jetzt als TEXT in SQLite gespeichert</span><span class="sxs-lookup"><span data-stu-id="e93bf-569">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="e93bf-570">Issue #15078</span><span class="sxs-lookup"><span data-stu-id="e93bf-570">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="e93bf-571">Diese Änderung wurde mit EF Core 3.0 Preview 4 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-571">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="e93bf-572">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-572">**Old behavior**</span></span>

<span data-ttu-id="e93bf-573">GUID-Werte wurden in der Vergangenheit in SQLite als BLOB-Werte gespeichert.</span><span class="sxs-lookup"><span data-stu-id="e93bf-573">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="e93bf-574">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-574">**New behavior**</span></span>

<span data-ttu-id="e93bf-575">GUID-Werte werden jetzt als TEXT gespeichert.</span><span class="sxs-lookup"><span data-stu-id="e93bf-575">Guid values are now sotred as TEXT.</span></span>

<span data-ttu-id="e93bf-576">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-576">**Why**</span></span>

<span data-ttu-id="e93bf-577">Das Binärformat der GUIDs ist nicht standardisiert.</span><span class="sxs-lookup"><span data-stu-id="e93bf-577">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="e93bf-578">Das Speichern der Werte als TEXT führt dazu, dass die Datenbank mit anderen Technologien eher kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="e93bf-578">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="e93bf-579">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-579">**Mitigations**</span></span>

<span data-ttu-id="e93bf-580">Sie können vorhandene Datenbanken zum neuen Format migrieren, indem Sie SQL-Code wie den folgenden ausführen:</span><span class="sxs-lookup"><span data-stu-id="e93bf-580">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="e93bf-581">Sie können in EF Core auch weiterhin das alte Verhalten verwenden, indem Sie einen Wertkonverter für diese Eigenschaften konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="e93bf-581">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="e93bf-582">Microsoft.Data.Sqlite kann weiterhin GUID-Werte aus BLOB- und TEXT-Spalten lesen. Da sich jedoch das Standardformat für Parameter und Konstanten geändert hat, müssen Sie wahrscheinlich Aktionen für die meisten GUID-Szenarios vornehmen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-582">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="e93bf-583">Char-Werte werden jetzt als TEXT in SQLite gespeichert</span><span class="sxs-lookup"><span data-stu-id="e93bf-583">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="e93bf-584">Issue #15020</span><span class="sxs-lookup"><span data-stu-id="e93bf-584">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="e93bf-585">Diese Änderung wurde mit EF Core 3.0 Preview 4 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-585">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="e93bf-586">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-586">**Old behavior**</span></span>

<span data-ttu-id="e93bf-587">Char-Werte wurden in der Vergangenheit in SQLite als INTEGER-Werte gespeichert.</span><span class="sxs-lookup"><span data-stu-id="e93bf-587">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="e93bf-588">Der Char-Wert *A* wurde z.B. als INTEGER-Wert 65 gespeichert.</span><span class="sxs-lookup"><span data-stu-id="e93bf-588">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="e93bf-589">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-589">**New behavior**</span></span>

<span data-ttu-id="e93bf-590">Char-Werte werden jetzt als TEXT gespeichert.</span><span class="sxs-lookup"><span data-stu-id="e93bf-590">Char values are now sotred as TEXT.</span></span>

<span data-ttu-id="e93bf-591">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-591">**Why**</span></span>

<span data-ttu-id="e93bf-592">Das Speichern der Werte als TEXT ist naheliegender und führt dazu, dass die Datenbank mit anderen Technologien eher kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="e93bf-592">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="e93bf-593">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-593">**Mitigations**</span></span>

<span data-ttu-id="e93bf-594">Sie können vorhandene Datenbanken zum neuen Format migrieren, indem Sie SQL-Code wie den folgenden ausführen:</span><span class="sxs-lookup"><span data-stu-id="e93bf-594">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="e93bf-595">Sie können in EF Core auch weiterhin das alte Verhalten verwenden, indem Sie einen Wertkonverter für diese Eigenschaften konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="e93bf-595">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="e93bf-596">Microsoft.Data.Sqlite kann auch weiterhin Zeichenwerte aus INTEGER- und TEXT-Spalten lesen, weshalb es sein kann, dass in einigen Szenarios keine weiteren Maßnahmen erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="e93bf-596">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="e93bf-597">Migrations-IDs werden jetzt mit dem Kalender der invarianten Kultur generiert</span><span class="sxs-lookup"><span data-stu-id="e93bf-597">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="e93bf-598">Issue #12978</span><span class="sxs-lookup"><span data-stu-id="e93bf-598">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="e93bf-599">Diese Änderung wurde mit EF Core 3.0 Preview 4 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-599">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="e93bf-600">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-600">**Old behavior**</span></span>

<span data-ttu-id="e93bf-601">Migrations-IDs wurden versehentlich mit dem Kalender der aktuellen Kultur generiert.</span><span class="sxs-lookup"><span data-stu-id="e93bf-601">Migration IDs were inadvertantly generated using the currret culture's calendar.</span></span>

<span data-ttu-id="e93bf-602">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-602">**New behavior**</span></span>

<span data-ttu-id="e93bf-603">Migrations-IDs werden jetzt immer mit dem Kalender der invarianten Kultur generiert (gregorianischer Kalender).</span><span class="sxs-lookup"><span data-stu-id="e93bf-603">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="e93bf-604">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-604">**Why**</span></span>

<span data-ttu-id="e93bf-605">Die Reihenfolge der Migrationen ist beim Aktualisieren einer Datenbank oder Auflösen von Mergekonflikten wesentlich.</span><span class="sxs-lookup"><span data-stu-id="e93bf-605">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="e93bf-606">Wenn der invariante Kalender verwendet wird, werden Probleme bei der Reihenfolge vermieden, die entstehen, wenn Teammitglieder unterschiedliche Systemkalender verwenden.</span><span class="sxs-lookup"><span data-stu-id="e93bf-606">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="e93bf-607">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-607">**Mitigations**</span></span>

<span data-ttu-id="e93bf-608">Diese Änderung betrifft jeden, der einen Kalender verwendet, der nicht gregorianisch ist und bei dem die Jahreszahl höher als die des gregorianischen Kalenders ist (wie z.B. in der buddhistischen Zeitrechnung).</span><span class="sxs-lookup"><span data-stu-id="e93bf-608">This change affects anyone using a non-Gregorian calender where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="e93bf-609">Vorhandene Migrations-IDs müssen aktualisiert werden, damit neue Migrationen in der Reihenfolge hinter vorhandenen Migrationen eingeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="e93bf-609">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="e93bf-610">Sie können die Migrations-ID im Migration-Attribut in der Designerdatei der Migration finden.</span><span class="sxs-lookup"><span data-stu-id="e93bf-610">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="e93bf-611">Die Tabelle mit dem Migrationsverlauf muss auch aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="e93bf-611">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="e93bf-612">LogQueryPossibleExceptionWithAggregateOperator wurde umbenannt</span><span class="sxs-lookup"><span data-stu-id="e93bf-612">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="e93bf-613">Issue #10985</span><span class="sxs-lookup"><span data-stu-id="e93bf-613">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="e93bf-614">Diese Änderung wurde mit EF Core 3.0 Preview 4 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-614">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="e93bf-615">**Änderung**</span><span class="sxs-lookup"><span data-stu-id="e93bf-615">**Change**</span></span>

<span data-ttu-id="e93bf-616">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` wurde umbenannt in `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="e93bf-616">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="e93bf-617">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-617">**Why**</span></span>

<span data-ttu-id="e93bf-618">Die Benennung dieses Warnereignisses wurde an allen anderen Warnereignissen ausgerichtet.</span><span class="sxs-lookup"><span data-stu-id="e93bf-618">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="e93bf-619">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-619">**Mitigations**</span></span>

<span data-ttu-id="e93bf-620">Verwenden Sie den neuen Namen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-620">Use the new name.</span></span> <span data-ttu-id="e93bf-621">(Beachten Sie, dass sich die Ereignis-ID-Nummer nicht geändert hat.)</span><span class="sxs-lookup"><span data-stu-id="e93bf-621">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="e93bf-622">Verdeutlichen der API für Einschränkungsnamen von Fremdschlüsseln</span><span class="sxs-lookup"><span data-stu-id="e93bf-622">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="e93bf-623">Issue #10730</span><span class="sxs-lookup"><span data-stu-id="e93bf-623">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="e93bf-624">Diese Änderung wurde mit EF Core 3.0 Preview 4 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e93bf-624">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="e93bf-625">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-625">**Old behavior**</span></span>

<span data-ttu-id="e93bf-626">Vor EF Core 3.0 wurden Einschränkungsnamen von Fremdschlüsseln einfach als „Namen“ bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="e93bf-626">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="e93bf-627">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="e93bf-627">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="e93bf-628">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="e93bf-628">**New behavior**</span></span>

<span data-ttu-id="e93bf-629">Ab EF Core 3.0 werden Einschränkungsnamen von Fremdschlüsseln nun als „Einschränkungsnamen“ bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="e93bf-629">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constaint name".</span></span> <span data-ttu-id="e93bf-630">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="e93bf-630">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="e93bf-631">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="e93bf-631">**Why**</span></span>

<span data-ttu-id="e93bf-632">Durch diese Änderung wird Konsistenz für Benennungen in diesem Bereich gewährleistet, und es wird verdeutlicht, dass es sich dabei um den Namen der Fremdschlüsseleinschränkung handelt und nicht um den Spalten- oder Eigenschaftennamen auf deren Grundlage der Fremdschlüssel definiert wird.</span><span class="sxs-lookup"><span data-stu-id="e93bf-632">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constaint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="e93bf-633">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="e93bf-633">**Mitigations**</span></span>

<span data-ttu-id="e93bf-634">Verwenden Sie den neuen Namen.</span><span class="sxs-lookup"><span data-stu-id="e93bf-634">Use the new name.</span></span>
