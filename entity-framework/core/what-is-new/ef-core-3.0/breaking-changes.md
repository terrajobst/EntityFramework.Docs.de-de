---
title: Breaking Changes in EF Core 3.0
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 7cc0bd3946be2e63d9fb46a023bf6abe750ae0e3
ms.sourcegitcommit: e90d6cfa3e96f10b8b5275430759a66a0c714ed1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2019
ms.locfileid: "68286495"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="21d26-102">Breaking Changes in EF Core 3.0 (aktuell in der Vorschauversion)</span><span class="sxs-lookup"><span data-stu-id="21d26-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="21d26-103">Bitte beachten Sie, dass die Featuregruppen und Zeitpläne für künftige Releases jederzeit geändert werden können. Obwohl diese Seite bestmöglich aktualisiert wird, entspricht sie nicht immer den neuesten Plänen.</span><span class="sxs-lookup"><span data-stu-id="21d26-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="21d26-104">Die folgenden API-Änderungen und Behavior Changes können dazu führen, dass Anwendungen, die für EF Core 2.2.x entwickelt wurden, beim Upgrade auf 3.0.0 nicht mehr funktionieren.</span><span class="sxs-lookup"><span data-stu-id="21d26-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="21d26-105">Änderungen, die sich voraussichtlich nur auf Datenbankanbieter auswirken, sind unter [Änderungen mit Auswirkungen auf den Anbieter](../../providers/provider-log.md) dokumentiert.</span><span class="sxs-lookup"><span data-stu-id="21d26-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="21d26-106">Werden Breaking Changes zwischen zwei Vorschauversionen von EF Core 3.0 in neuen Features eingeführt, werden diese hier nicht dokumentiert.</span><span class="sxs-lookup"><span data-stu-id="21d26-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="21d26-107">LINQ-Abfragen werden nicht mehr auf dem Client ausgewertet</span><span class="sxs-lookup"><span data-stu-id="21d26-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="21d26-108">[Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Siehe auch Issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="21d26-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="21d26-109">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-109">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="21d26-110">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-110">**Old behavior**</span></span>

<span data-ttu-id="21d26-111">Vor Version 3.0 wurden in EF Core automatisch Ausdrücke auf dem Client ausgewertet, wenn diese Teil einer Abfrage waren und nicht in SQL oder in einen Parameter konvertiert werden konnten.</span><span class="sxs-lookup"><span data-stu-id="21d26-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="21d26-112">Wenn potenziell aufwendige Ausdrücke auf dem Client ausgewertet werden sollten, wurde standardmäßig nur eine Warnung ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="21d26-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="21d26-113">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-113">**New behavior**</span></span>

<span data-ttu-id="21d26-114">Ab Version 3.0 können in EF Core nur Ausdrücke der obersten Projektion (dem letzten `Select()`-Aufruf in der Abfrage) auf dem Client ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="21d26-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="21d26-115">Wenn Ausdrücke in einem anderen Teil der Abfrage nicht in SQL oder in einen Parameter konvertiert werden können, wird eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="21d26-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="21d26-116">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-116">**Why**</span></span>

<span data-ttu-id="21d26-117">Die automatische Auswertung von Abfragen auf Clients hat zur Folge, dass viele Abfragen auch dann ausgeführt werden, wenn wichtige Teile nicht übersetzt werden können.</span><span class="sxs-lookup"><span data-stu-id="21d26-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="21d26-118">Dieses Verhalten kann zu unerwartetem und potenziell schädlichem Verhalten führen, das möglicherweise erst in der Produktionsphase erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="21d26-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="21d26-119">Eine nicht übersetzbare Bedingung in einem `Where()`-Aufruf kann sich beispielsweise so auswirken, dass alle Zeilen einer Tabelle vom Datenbankserver übertragen werden und der Filter auf dem Client angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="21d26-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="21d26-120">Wenn die Tabelle in der Entwicklungsphase nur wenige Zeilen enthält, ist es gut möglich, dass diese Situation unerkannt bleibt. Wenn die Anwendung jedoch in die Produktionsumgebung überführt wird, enthält die Tabelle möglicherweise Millionen von Zeilen, was zu schwerwiegenden Konsequenzen führen kann.</span><span class="sxs-lookup"><span data-stu-id="21d26-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="21d26-121">Wie die Erfahrung in der Entwicklungsphase gezeigt hat, können auch Warnungen, die bei Auswertungen auf dem Client ausgelöst werden, dieses Problem nicht lösen, da sie sich leicht ignorieren lassen.</span><span class="sxs-lookup"><span data-stu-id="21d26-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="21d26-122">Die automatische Auswertung auf Clients kann darüber hinaus zu Problemen führen, bei denen die Verbesserung der Abfrageübersetzung für bestimmte Ausdrücke zu unbeabsichtigten Breaking Changes zwischen Releases führt.</span><span class="sxs-lookup"><span data-stu-id="21d26-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="21d26-123">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-123">**Mitigations**</span></span>

<span data-ttu-id="21d26-124">Wenn sich eine Abfrage nicht vollständig übersetzen lässt, habe Sie zwei Möglichkeiten: Schreiben Sie sie entweder um, oder setzen Sie alternativ `AsEnumerable()`, `ToList()` oder ähnliche Methoden ein, um Daten wieder zurück an den Client zu übertragen, wo sie mit LINQ to Objects weiterverarbeitet werden können.</span><span class="sxs-lookup"><span data-stu-id="21d26-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="21d26-125">Entity Framework Core ist nicht mehr Teil des gemeinsam verwendeten ASP.NET Core-Frameworks</span><span class="sxs-lookup"><span data-stu-id="21d26-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="21d26-126">Issueankündigungen #325</span><span class="sxs-lookup"><span data-stu-id="21d26-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="21d26-127">Diese Änderung wird in Vorschauversion 1 von ASP.NET Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-127">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="21d26-128">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-128">**Old behavior**</span></span>

<span data-ttu-id="21d26-129">Vor Version 3.0 von ASP.NET Core wurden EF Core und einige der zugehörigen Datenanbieter wie SQL Server eingeschlossen, wenn `Microsoft.AspNetCore.App` oder `Microsoft.AspNetCore.All` ein Paketverweis hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="21d26-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="21d26-130">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-130">**New behavior**</span></span>

<span data-ttu-id="21d26-131">Ab Version 3.0 enthält das gemeinsam verwendete ASP.NET Core-Framework weder EF Core noch zugehörige Datenanbieter.</span><span class="sxs-lookup"><span data-stu-id="21d26-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="21d26-132">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-132">**Why**</span></span>

<span data-ttu-id="21d26-133">Vor dieser Änderung konnte EF Core auf unterschiedliche Arten bezogen werden. Diese richteten sich danach, ob ASP.NET Core und SQL Server oder ein anderes Zielframework für die Anwendung vorgesehen war.</span><span class="sxs-lookup"><span data-stu-id="21d26-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="21d26-134">Bei einem Upgrade von ASP.NET Core wurde zudem ein Upgrade von EF Core und des SQL Server-Anbieters erzwungen, was nicht in allen Fällen gewünscht war.</span><span class="sxs-lookup"><span data-stu-id="21d26-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="21d26-135">Durch die eingeführte Änderung wird EF Core unter allen Anbietern, unterstützten .NET-Implementierungen und Anwendungstypen auf dieselbe Weise bezogen.</span><span class="sxs-lookup"><span data-stu-id="21d26-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="21d26-136">Entwickler können nun außerdem genau festlegen, wann für EF Core und zugehörige Datenanbieter ein Upgrade durchgeführt werden soll.</span><span class="sxs-lookup"><span data-stu-id="21d26-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="21d26-137">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-137">**Mitigations**</span></span>

<span data-ttu-id="21d26-138">Wenn Sie EF Core in einer Anwendung unter ASP.NET Core 3.0 oder in einer anderen unterstützten Anwendung verwenden möchten, müssen Sie dem EF Core-Datenbankanbieter, der für die Anwendung genutzt werden soll, explizit einen Paketverweis hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="21d26-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="21d26-139">Das EF Core-Befehlszeilentool (dotnet ef) ist nicht länger Bestandteil des .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="21d26-139">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="21d26-140">Issue #14016</span><span class="sxs-lookup"><span data-stu-id="21d26-140">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="21d26-141">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 und der zugehörigen Version des .NET Core SDK eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-141">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="21d26-142">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-142">**Old behavior**</span></span>

<span data-ttu-id="21d26-143">Vor Version 3.0 war das `dotnet ef`-Tool im .NET Core SDK enthalten und konnte von der Befehlszeile aus ohne zusätzliche Schritte in einem beliebigen Projekt verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="21d26-143">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="21d26-144">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-144">**New behavior**</span></span>

<span data-ttu-id="21d26-145">Ab Version 3.0 ist das `dotnet ef`-Tool nicht mehr im .NET SDK enthalten und muss deshalb vor der Verwendung explizit als lokales oder globales Tool installiert werden.</span><span class="sxs-lookup"><span data-stu-id="21d26-145">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="21d26-146">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-146">**Why**</span></span>

<span data-ttu-id="21d26-147">Durch diese Änderung kann `dotnet ef` als reguläres .NET-CLI-Tool für NuGet verteilt und aktualisiert werden und entspricht damit dem üblichen Verfahren, dass EF Core 3.0 ebenfalls immer als NuGet-Paket verteilt wird.</span><span class="sxs-lookup"><span data-stu-id="21d26-147">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="21d26-148">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-148">**Mitigations**</span></span>

<span data-ttu-id="21d26-149">Um Migrationen zu verwalten oder ein Gerüst für `DbContext` zu erstellen, installieren Sie `dotnet-ef` als globales Tool:</span><span class="sxs-lookup"><span data-stu-id="21d26-149">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` as a global tool:</span></span>

  ``` console
    $ dotnet tool install --global dotnet-ef --version 3.0.0-*
  ```

<span data-ttu-id="21d26-150">Eine Verwendung als lokales Tool ist ebenfalls möglich, wenn Sie die Abhängigkeiten eines Projekts wiederherstellen, das das Tool mithilfe einer [Toolmanifestdatei](https://github.com/dotnet/cli/issues/10288) als Toolabhängigkeit deklariert.</span><span class="sxs-lookup"><span data-stu-id="21d26-150">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="21d26-151">FromSql, ExecuteSql und ExecuteSqlAsync wurden umbenannt</span><span class="sxs-lookup"><span data-stu-id="21d26-151">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="21d26-152">Issue #10996</span><span class="sxs-lookup"><span data-stu-id="21d26-152">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="21d26-153">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-153">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="21d26-154">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-154">**Old behavior**</span></span>

<span data-ttu-id="21d26-155">Vor Version 3.0 wurden in EF Core diese Methodennamen überladen, um entweder mit einer normalen Zeichenfolge oder einer Zeichenfolge, die in SQL und Parameter interpoliert werden sollte, zu arbeiten.</span><span class="sxs-lookup"><span data-stu-id="21d26-155">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="21d26-156">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-156">**New behavior**</span></span>

<span data-ttu-id="21d26-157">Ab Version 3.0 verwenden Sie in EF Core `FromSqlRaw`, `ExecuteSqlRaw` und `ExecuteSqlRawAsync`, um eine parametrisierte Abfrage zu erstellen, bei der die Parameter einzeln aus der Abfragezeichenfolge übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="21d26-157">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="21d26-158">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="21d26-158">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="21d26-159">Verwenden Sie `FromSqlInterpolated`, `ExecuteSqlInterpolated` und `ExecuteSqlInterpolatedAsync`, um eine parametrisierte Abfrage zu erstellen, bei der die Parameter als Teil einer interpolierten Abfragezeichenfolge übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="21d26-159">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="21d26-160">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="21d26-160">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="21d26-161">Beachten Sie, dass die beiden oben genannten Abfragen dieselbe parametrisierte SQL mit denselben SQL-Parametern erzeugen.</span><span class="sxs-lookup"><span data-stu-id="21d26-161">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="21d26-162">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-162">**Why**</span></span>

<span data-ttu-id="21d26-163">Bei Methodenüberladungen wie dieser wird sehr leicht versehentlich die Methode für unformatierte Zeichenfolgen aufgerufen, wenn eigentlich beabsichtigt war, die Methode für interpolierte Zeichenfolgen aufzurufen (und umgekehrt).</span><span class="sxs-lookup"><span data-stu-id="21d26-163">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="21d26-164">Dadurch werden Abfragen möglicherweise nicht parametrisiert, obwohl dies der Fall sein sollte.</span><span class="sxs-lookup"><span data-stu-id="21d26-164">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="21d26-165">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-165">**Mitigations**</span></span>

<span data-ttu-id="21d26-166">Verwenden Sie ab sofort die neuen Methodennamen.</span><span class="sxs-lookup"><span data-stu-id="21d26-166">Switch to use the new method names.</span></span>

## <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="21d26-167">FromSql-Methoden können nur für die Stammelemente der Abfrage angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="21d26-167">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="21d26-168">Issue #15704</span><span class="sxs-lookup"><span data-stu-id="21d26-168">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="21d26-169">Diese Änderung wird in Vorschauversion 6 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-169">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="21d26-170">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-170">**Old behavior**</span></span>

<span data-ttu-id="21d26-171">Vor EF Core 3.0 konnte die `FromSql`-Methode an einer beliebigen Stelle in der Abfrage angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="21d26-171">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="21d26-172">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-172">**New behavior**</span></span>

<span data-ttu-id="21d26-173">Ab EF Core 3.0 können die neuen Methoden `FromSqlRaw` und `FromSqlInterpolated` (durch die `FromSql` ersetzt wird) nur für Stammelemente von Abfragen angegeben werden, d.h. direkt für das `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="21d26-173">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="21d26-174">Der Versuch, sie an anderer Stelle anzugeben, führt zu einem Kompilierungsfehler.</span><span class="sxs-lookup"><span data-stu-id="21d26-174">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="21d26-175">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-175">**Why**</span></span>

<span data-ttu-id="21d26-176">Die Angabe von `FromSql` an einer anderen Stelle als für ein `DbSet` erbrachte keine zusätzliche Bedeutung und keinen Mehrwert, sondern konnte in bestimmten Szenarien zu Mehrdeutigkeiten führen.</span><span class="sxs-lookup"><span data-stu-id="21d26-176">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="21d26-177">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-177">**Mitigations**</span></span>

<span data-ttu-id="21d26-178">`FromSql` Aufrufe sollten so verschoben, dass sie direkt für das zugehörige `DbSet` gelten.</span><span class="sxs-lookup"><span data-stu-id="21d26-178">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

## <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="21d26-179">~~Die Abfrageausführung wird auf Debugebene protokolliert~~ – zurückgesetzt</span><span class="sxs-lookup"><span data-stu-id="21d26-179">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="21d26-180">Issue #14523</span><span class="sxs-lookup"><span data-stu-id="21d26-180">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="21d26-181">Diese Änderung wird in Vorschauversion 7 von EF Core 3.0 zurückgesetzt.</span><span class="sxs-lookup"><span data-stu-id="21d26-181">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="21d26-182">Wir haben diese Änderung zurückgesetzt, weil die neue Konfiguration in EF Core 3.0 die Angabe des Protokolliergrads für jedes Ereignis durch die Anwendung ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="21d26-182">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="21d26-183">Um z.B. die Protokollierung von SQL zu `Debug` zu ändern, konfigurieren Sie den Grad explizit in `OnConfiguring` oder `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="21d26-183">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="21d26-184">Temporäre Schlüsselwerte werden nicht mehr Entitätsinstanzen zugewiesen</span><span class="sxs-lookup"><span data-stu-id="21d26-184">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="21d26-185">Issue #12378</span><span class="sxs-lookup"><span data-stu-id="21d26-185">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="21d26-186">Diese Änderung wird in Vorschauversion 2 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-186">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="21d26-187">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-187">**Old behavior**</span></span>

<span data-ttu-id="21d26-188">Vor Version 3.0 wurden in EF Core allen Schlüsseleigenschaften temporäre Werte zugewiesen. Später wurden den Eigenschaften dann die tatsächlichen Werte zugewiesen, die von der Datenbank generiert wurden.</span><span class="sxs-lookup"><span data-stu-id="21d26-188">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="21d26-189">Die temporären Werte waren in der Regel große negative Zahlen.</span><span class="sxs-lookup"><span data-stu-id="21d26-189">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="21d26-190">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-190">**New behavior**</span></span>

<span data-ttu-id="21d26-191">Ab Version 3.0 wird in EF Core der temporäre Wert als Teil der Überwachungsinformationen der Entität gespeichert. Die Schlüsseleigenschaft selbst bleibt unverändert.</span><span class="sxs-lookup"><span data-stu-id="21d26-191">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="21d26-192">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-192">**Why**</span></span>

<span data-ttu-id="21d26-193">Diese Änderung wurde vorgenommen, damit temporäre Schlüsselwerte nicht fälschlicherweise dauerhaft gespeichert werden, wenn eine Entität, die vorher von einer `DbContext`-Instanz überwacht wurde, in eine andere `DbContext`-Instanz verschoben wird.</span><span class="sxs-lookup"><span data-stu-id="21d26-193">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="21d26-194">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-194">**Mitigations**</span></span>

<span data-ttu-id="21d26-195">Anwendungen, die Primärschlüsselwerte Fremdschlüsseln zuweisen, um Zuordnungen zwischen Entitäten zu erstellen, können vom alten Verhalten abhängig sein, wenn Primärschlüssel vom Speicher generiert werden und zu Entitäten im `Added`-Zustand gehören.</span><span class="sxs-lookup"><span data-stu-id="21d26-195">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="21d26-196">Sie können dies wie folgt vermeiden:</span><span class="sxs-lookup"><span data-stu-id="21d26-196">This can be avoided by:</span></span>
* <span data-ttu-id="21d26-197">Verwenden Sie keine vom Speicher generierten Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="21d26-197">Not using store-generated keys.</span></span>
* <span data-ttu-id="21d26-198">Legen Sie zum Erstellen von Zuordnungen keine Fremdschlüsselwerte, sondern Navigationseigenschaften fest.</span><span class="sxs-lookup"><span data-stu-id="21d26-198">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="21d26-199">Rufen Sie die tatsächlichen temporären Schlüsselwerte aus den Überwachungsinformationen der Entität ab.</span><span class="sxs-lookup"><span data-stu-id="21d26-199">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="21d26-200">`context.Entry(blog).Property(e => e.Id).CurrentValue` gibt beispielsweise den temporären Wert zurück, obwohl `blog.Id` noch nicht festgelegt wurde.</span><span class="sxs-lookup"><span data-stu-id="21d26-200">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="21d26-201">DetectChanges berücksichtigt vom Speicher generierte Schlüsselwerte</span><span class="sxs-lookup"><span data-stu-id="21d26-201">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="21d26-202">Issue #14616</span><span class="sxs-lookup"><span data-stu-id="21d26-202">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="21d26-203">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-203">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="21d26-204">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-204">**Old behavior**</span></span>

<span data-ttu-id="21d26-205">Vor Version 3.0 wurde in EF Core eine nicht überwachte Entität, die von `DetectChanges` gefunden wurde, im `Added`-Zustand überwacht und als neue Zeile eingefügt, sobald `SaveChanges` aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="21d26-205">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="21d26-206">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-206">**New behavior**</span></span>

<span data-ttu-id="21d26-207">Ab Version 3.0 wird in EF Core die Entität im `Modified`-Zustand überwacht, wenn für diese generierte Schlüsselwerte verwendet werden und ein Schlüsselwert festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="21d26-207">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="21d26-208">Es wird also davon ausgegangen, dass eine Zeile für die Entität vorhanden ist. Wenn `SaveChanges` aufgerufen wird, wird die Zeile außerdem aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="21d26-208">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="21d26-209">Falls kein Schlüsselwert festgelegt ist oder der Entitätstyp keine generierten Schlüssel verwendet, wird die neue Entität wie in früheren Version weiterhin im `Added`-Zustand überwacht.</span><span class="sxs-lookup"><span data-stu-id="21d26-209">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="21d26-210">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-210">**Why**</span></span>

<span data-ttu-id="21d26-211">Diese Änderung wurde vorgenommen, um bei der Verwendung von speichergenerierten Schlüsseln die Arbeit mit nicht verbundenen Entitätsgraphen einfacher und konsistenter zu gestalten.</span><span class="sxs-lookup"><span data-stu-id="21d26-211">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="21d26-212">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-212">**Mitigations**</span></span>

<span data-ttu-id="21d26-213">Diese Änderung kann dazu führen, dass eine Anwendung nicht mehr funktioniert. Dieser Fall tritt ein, wenn für Entitätstypen generierte Schlüssel vorgesehen sind, dann jedoch explizit Schlüsselwerte für neue Instanzen festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="21d26-213">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="21d26-214">Sie können dieses Problem vermeiden, indem Sie explizit festlegen, dass für Schlüsseleigenschaften keine generierten Werte verwendet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="21d26-214">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="21d26-215">Dies ist beispielsweise mit der Fluent-API möglich:</span><span class="sxs-lookup"><span data-stu-id="21d26-215">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="21d26-216">Alternativ bieten sich auch Datenanmerkungen an:</span><span class="sxs-lookup"><span data-stu-id="21d26-216">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="21d26-217">Kaskadierende Deletes werden standardmäßig sofort ausgeführt</span><span class="sxs-lookup"><span data-stu-id="21d26-217">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="21d26-218">Issue #10114</span><span class="sxs-lookup"><span data-stu-id="21d26-218">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="21d26-219">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-219">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="21d26-220">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-220">**Old behavior**</span></span>

<span data-ttu-id="21d26-221">Vor Version 3.0 wurden in EF Core kaskadierenden Aktionen (Löschen von abhängigen Entitäten nach dem Löschen eines erforderlichen Prinzipals oder nach dem Aufheben einer Beziehung zum erforderlichen Prinzipal) erst ausgeführt, wenn SaveChanges aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="21d26-221">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="21d26-222">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-222">**New behavior**</span></span>

<span data-ttu-id="21d26-223">Ab Version 3.0 werden in EF Core kaskadierende Aktionen angewendet, sobald die auslösende Bedingung erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="21d26-223">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="21d26-224">Wenn beispielsweise `context.Remove()` zum Löschen einer Prinzipalentität aufgerufen wird, führt das dazu, dass auch alle zugehörigen erforderlichen Entitäten, die überwacht werden, sofort auf `Deleted` festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="21d26-224">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="21d26-225">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-225">**Why**</span></span>

<span data-ttu-id="21d26-226">Diese Änderung wurde vorgenommen, um die Arbeit mit Datenbindungen und Überwachungsszenarios zu vereinfachen, in denen bekannt sein muss, welche Entitäten _vor_ dem Aufruf von `SaveChanges` gelöscht werden.</span><span class="sxs-lookup"><span data-stu-id="21d26-226">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="21d26-227">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-227">**Mitigations**</span></span>

<span data-ttu-id="21d26-228">Sie können das vorherige Verhalten wiederherstellen, indem Sie für `context.ChangedTracker` die entsprechenden Einstellungen vornehmen.</span><span class="sxs-lookup"><span data-stu-id="21d26-228">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="21d26-229">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="21d26-229">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="21d26-230">DeleteBehavior.Restrict verfügt über eine übersichtlichere Semantik</span><span class="sxs-lookup"><span data-stu-id="21d26-230">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="21d26-231">Issue #12661</span><span class="sxs-lookup"><span data-stu-id="21d26-231">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="21d26-232">Diese Änderung wird in Vorschauversion 5 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-232">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="21d26-233">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-233">**Old behavior**</span></span>

<span data-ttu-id="21d26-234">Vor Version 3.0 wurden mit `DeleteBehavior.Restrict` Fremdschlüssel in der Datenbank mit `Restrict`-Semantik erstellt, es wurden aber auch unvorhergesehen interne Fixups geändert.</span><span class="sxs-lookup"><span data-stu-id="21d26-234">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="21d26-235">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-235">**New behavior**</span></span>

<span data-ttu-id="21d26-236">Ab Version 3.0 wird mit `DeleteBehavior.Restrict` sichergestellt, dass Fremdschlüssel mit `Restrict`-Semantik erstellt werden – d. h. ohne Überlappungen; ausgenommen bei Einschränkungsverletzungen – und ohne Auswirkungen auf EF-interne Fixups.</span><span class="sxs-lookup"><span data-stu-id="21d26-236">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="21d26-237">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-237">**Why**</span></span>

<span data-ttu-id="21d26-238">Diese Änderung wurde vorgenommen, um die Benutzerfreundlichkeit bei intuitiver Verwendung von `DeleteBehavior` ohne unerwartete Nebeneffekte zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="21d26-238">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="21d26-239">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-239">**Mitigations**</span></span>

<span data-ttu-id="21d26-240">Sie können das vorherige Verhalten wiederherstellen, indem Sie `DeleteBehavior.ClientNoAction` verwenden.</span><span class="sxs-lookup"><span data-stu-id="21d26-240">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="21d26-241">Abfragetypen werden mit Entitätstypen zusammengeführt</span><span class="sxs-lookup"><span data-stu-id="21d26-241">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="21d26-242">Issue #14194</span><span class="sxs-lookup"><span data-stu-id="21d26-242">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="21d26-243">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-243">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="21d26-244">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-244">**Old behavior**</span></span>

<span data-ttu-id="21d26-245">Vor Version 3.0 wurden in EF Core [Abfragetypen](xref:core/modeling/query-types) dazu verwendet, Daten abzufragen, in denen kein Primärschlüssel auf strukturierte Weise festgelegt war.</span><span class="sxs-lookup"><span data-stu-id="21d26-245">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="21d26-246">Abfragetypen wurden also für die Zuordnung von Entitätstypen ohne Schlüssel verwendet (in der Regel in einer Sicht, in einigen Fällen aber auch in einer Tabelle), während reguläre Entitätstypen für einen verfügbaren Schlüssel verwendet wurden (in der Regel in einer Tabelle, in einigen Fällen aber auch in einer Sicht).</span><span class="sxs-lookup"><span data-stu-id="21d26-246">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="21d26-247">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-247">**New behavior**</span></span>

<span data-ttu-id="21d26-248">Aus einem Abfragetyp wird nun einfach ein Entitätstyp ohne Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="21d26-248">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="21d26-249">Entitätstypen ohne Schlüssel besitzen die gleiche Funktionalität wie Abfragetypen in früheren Versionen.</span><span class="sxs-lookup"><span data-stu-id="21d26-249">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="21d26-250">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-250">**Why**</span></span>

<span data-ttu-id="21d26-251">Diese Änderung wurde vorgenommen, um Unklarheiten bei der Verwendung von Abfragetypen zu beseitigen.</span><span class="sxs-lookup"><span data-stu-id="21d26-251">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="21d26-252">Abfragetypen sind schlüssellose Entitätstypen, die grundsätzlich schreibgeschützt sind. Sie sollten allerdings nicht allein wegen des Schreibschutzes verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="21d26-252">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="21d26-253">Des Weiteren werden sie oft Sichten zugeordnet, was aber nur daran liegt, dass für Letztere häufig keine Schlüssel definiert werden.</span><span class="sxs-lookup"><span data-stu-id="21d26-253">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="21d26-254">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-254">**Mitigations**</span></span>

<span data-ttu-id="21d26-255">Die folgenden Teile der API sind durch die Änderungen veraltet:</span><span class="sxs-lookup"><span data-stu-id="21d26-255">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="21d26-256">**`ModelBuilder.Query<>()`** : Rufen Sie stattdessen `ModelBuilder.Entity<>().HasNoKey()` auf, um einen schlüssellosen Entitätstyp festzulegen.</span><span class="sxs-lookup"><span data-stu-id="21d26-256">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="21d26-257">Dieses Verhalten wird nach wie vor nicht konventionsgemäß festgelegt, um Fehlkonfigurationen zu vermeiden, wenn ein Primärschlüssel erwartet wird, jedoch nicht mit der Konvention übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="21d26-257">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="21d26-258">**`DbQuery<>`** : Verwenden Sie stattdessen `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="21d26-258">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="21d26-259">**`DbContext.Query<>()`** : Verwenden Sie stattdessen `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="21d26-259">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="21d26-260">Die Konfigurations-API für Beziehungen abhängiger (owned) Typen wurde geändert</span><span class="sxs-lookup"><span data-stu-id="21d26-260">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="21d26-261">[Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="21d26-261">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="21d26-262">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-262">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="21d26-263">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-263">**Old behavior**</span></span>

<span data-ttu-id="21d26-264">Vor Version 3.0 wurde in EF Core die abhängige Beziehung direkt nach dem Aufruf von `OwnsOne` oder `OwnsMany` konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="21d26-264">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="21d26-265">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-265">**New behavior**</span></span>

<span data-ttu-id="21d26-266">Ab Version 3.0 von EF Core ist eine Fluent-API verfügbar, mit der über `WithOwner()` eine Navigationseigenschaft für den Besitzer konfiguriert werden kann.</span><span class="sxs-lookup"><span data-stu-id="21d26-266">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="21d26-267">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="21d26-267">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="21d26-268">Die Konfiguration für die Beziehung zwischen Besitzertyp und abhängigem Typ sollte nun ähnlich wie bei der Konfiguration anderer Beziehungen nach `WithOwner()` verkettet werden.</span><span class="sxs-lookup"><span data-stu-id="21d26-268">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="21d26-269">Die Konfiguration für den abhängigen Typ wird jedoch weiterhin nach `OwnsOne()/OwnsMany()` verkettet.</span><span class="sxs-lookup"><span data-stu-id="21d26-269">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="21d26-270">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="21d26-270">For example:</span></span>

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

<span data-ttu-id="21d26-271">Wenn Sie zusätzlich `Entity()`, `HasOne()`, oder `Set()` für das Ziel eines abhängigen Typs aufrufen, wird keine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="21d26-271">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="21d26-272">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-272">**Why**</span></span>

<span data-ttu-id="21d26-273">Diese Änderung wurde vorgenommen, um eine deutlichere Trennung zwischen der Konfiguration des abhängigen Typs und der _Beziehung zum abhängigen Typ_ zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="21d26-273">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="21d26-274">Dadurch werden für Methoden wie `HasForeignKey` Mehrdeutigkeiten und Unklarheiten beseitigt.</span><span class="sxs-lookup"><span data-stu-id="21d26-274">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="21d26-275">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-275">**Mitigations**</span></span>

<span data-ttu-id="21d26-276">Passen Sie für die Beziehungen von abhängigen Typen die Konfiguration so an, dass die neue Fluent-API wie im obigen Beispiel verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="21d26-276">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="21d26-277">Abhängige Entitäten, die die Tabelle gemeinsam mit dem Prinzipal verwenden, sind jetzt optional</span><span class="sxs-lookup"><span data-stu-id="21d26-277">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="21d26-278">Issue #9005</span><span class="sxs-lookup"><span data-stu-id="21d26-278">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="21d26-279">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-279">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="21d26-280">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-280">**Old behavior**</span></span>

<span data-ttu-id="21d26-281">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="21d26-281">Consider the following model:</span></span>
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
<span data-ttu-id="21d26-282">Wenn `OrderDetails` im Besitz von `Order` ist oder explizit derselben Tabelle zugeordnet ist, war vor Version 3.0 in EF Core immer eine `OrderDetails`-Instanz beim Hinzufügen eines neuen `Order`-Objekts erforderlich.</span><span class="sxs-lookup"><span data-stu-id="21d26-282">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="21d26-283">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-283">**New behavior**</span></span>

<span data-ttu-id="21d26-284">Ab Version 3.0 bietet EF Core die Möglichkeit, `Order` ohne `OrderDetails` hinzuzufügen, und alle `OrderDetails`-Eigenschaften, mit Ausnahme des primären Schlüssels, werden Spalten zugeordnet, die NULL-Werte zulassen.</span><span class="sxs-lookup"><span data-stu-id="21d26-284">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="21d26-285">Bei Abfragen von EF Core wird `OrderDetails` auf `null` festgelegt, wenn eine der erforderlichen Eigenschaften keinen Wert aufweist oder keine erforderlichen Eigenschaften außer dem primären Schlüssel vorhanden und alle Eigenschaften `null` sind.</span><span class="sxs-lookup"><span data-stu-id="21d26-285">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="21d26-286">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-286">**Mitigations**</span></span>

<span data-ttu-id="21d26-287">Wenn Ihr Modell von der gemeinsamen Nutzung einer Tabelle mit allen optionalen Spalten abhängig ist, aber die Navigation, die darauf zeigt, wahrscheinlich nicht `null` ist, sollte die Anwendung für die Handhabung von Fällen geändert werden, in denen die Navigation `null` ist.</span><span class="sxs-lookup"><span data-stu-id="21d26-287">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="21d26-288">Wenn das nicht möglich ist, sollte dem Entitätstyp eine erforderliche Eigenschaft hinzugefügt werden oder mindestens einer Eigenschaft sollte ein Wert ungleich `null` zugewiesen sein.</span><span class="sxs-lookup"><span data-stu-id="21d26-288">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="21d26-289">Alle Entitäten, die eine Tabelle mit einer Spalte für das Parallelitätstoken gemeinsam verwenden, müssen diese einer Eigenschaft zuordnen</span><span class="sxs-lookup"><span data-stu-id="21d26-289">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="21d26-290">Issue #14154</span><span class="sxs-lookup"><span data-stu-id="21d26-290">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="21d26-291">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-291">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="21d26-292">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-292">**Old behavior**</span></span>

<span data-ttu-id="21d26-293">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="21d26-293">Consider the following model:</span></span>
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
<span data-ttu-id="21d26-294">Wenn `OrderDetails` im Besitz von `Order` ist oder explizit derselben Tabelle zugeordnet ist, wird vor Version 3.0 in EF Core durch das alleinige Aktualisieren von `OrderDetails` der `Version`-Wert auf dem Client nicht aktualisiert, und das nächste Update schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="21d26-294">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="21d26-295">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-295">**New behavior**</span></span>

<span data-ttu-id="21d26-296">Ab Version 3.0 gibt EF Core den neuen `Version`-Wert an `Order` weiter, wenn dies Besitzer von `OrderDetails` ist.</span><span class="sxs-lookup"><span data-stu-id="21d26-296">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="21d26-297">Andernfalls wird eine Ausnahme während der Modellvalidierung ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="21d26-297">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="21d26-298">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-298">**Why**</span></span>

<span data-ttu-id="21d26-299">Diese Änderung wurde vorgenommen, um einen veralteten Wert für ein Parallelitätstoken zu vermeiden, wenn nur eine der Entitäten, die derselben Tabelle zugeordnet sind, aktualisiert wird.</span><span class="sxs-lookup"><span data-stu-id="21d26-299">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="21d26-300">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-300">**Mitigations**</span></span>

<span data-ttu-id="21d26-301">Alle Entitäten, die die Tabelle gemeinsam nutzen, müssen eine Eigenschaft enthalten, die der Spalte für das Parallelitätstoken zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="21d26-301">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="21d26-302">Es ist möglich, eine solche im Schattenzustand zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="21d26-302">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="21d26-303">Geerbte Eigenschaften von nicht zugeordneten Typen sind nun einer einzelnen Spalte für alle abgeleiteten Typen zugeordnet</span><span class="sxs-lookup"><span data-stu-id="21d26-303">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="21d26-304">Issue #13998</span><span class="sxs-lookup"><span data-stu-id="21d26-304">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="21d26-305">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-305">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="21d26-306">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-306">**Old behavior**</span></span>

<span data-ttu-id="21d26-307">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="21d26-307">Consider the following model:</span></span>
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

<span data-ttu-id="21d26-308">Vor Version 3.0 wurde in EF Core die Eigenschaft `ShippingAddress` standardmäßig separaten Spalten für `BulkOrder` und `Order` zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="21d26-308">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="21d26-309">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-309">**New behavior**</span></span>

<span data-ttu-id="21d26-310">Ab Version 3.0 erstellt EF Core nur eine Spalte für `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="21d26-310">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="21d26-311">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-311">**Why**</span></span>

<span data-ttu-id="21d26-312">Der alte Verhalten war unerwartet.</span><span class="sxs-lookup"><span data-stu-id="21d26-312">The old behavoir was unexpected.</span></span>

<span data-ttu-id="21d26-313">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-313">**Mitigations**</span></span>

<span data-ttu-id="21d26-314">Die Eigenschaft kann noch immer explizit einer separaten Spalte für die abgeleiteten Typen zugeordnet werden:</span><span class="sxs-lookup"><span data-stu-id="21d26-314">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="21d26-315">Die Konvention zur Fremdschlüsseleigenschaft entspricht nicht mehr dem Namen der Prinzipaleigenschaft</span><span class="sxs-lookup"><span data-stu-id="21d26-315">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="21d26-316">Issue #13274</span><span class="sxs-lookup"><span data-stu-id="21d26-316">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="21d26-317">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-317">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="21d26-318">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-318">**Old behavior**</span></span>

<span data-ttu-id="21d26-319">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="21d26-319">Consider the following model:</span></span>
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
<span data-ttu-id="21d26-320">Vor Version 3.0 wurde in EF Core gemäß der Konvention die `CustomerId`-Eigenschaft für den Fremdschlüssel verwendet.</span><span class="sxs-lookup"><span data-stu-id="21d26-320">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="21d26-321">Wenn `Order` jedoch ein abhängiger Typ ist, wäre `CustomerId` der Primärschlüssel, was normalerweise nicht der Erwartungshaltung entspricht.</span><span class="sxs-lookup"><span data-stu-id="21d26-321">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="21d26-322">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-322">**New behavior**</span></span>

<span data-ttu-id="21d26-323">Ab Version 3.0 werden in EF Core konventionsgemäß keine Eigenschaften mehr für Fremdschlüssel verwendet, wenn diese denselben Namen wie die Prinzipaleigenschaft besitzen.</span><span class="sxs-lookup"><span data-stu-id="21d26-323">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="21d26-324">Die Muster für den Namen des Prinzipaltyps, der mit dem Namen der Prinzipaleigenschaft verkettet wird, und für den Navigationsnamen, der mit dem Namen der Prinzipaleigenschaft verkettet wird, werden jedoch weiterhin abgeglichen.</span><span class="sxs-lookup"><span data-stu-id="21d26-324">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="21d26-325">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="21d26-325">For example:</span></span>

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

<span data-ttu-id="21d26-326">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-326">**Why**</span></span>

<span data-ttu-id="21d26-327">Diese Änderung wurde vorgenommen, um zu vermeiden, dass versehentlich eine Primärschlüsseleigenschaft für den abhängigen Typ definiert wird.</span><span class="sxs-lookup"><span data-stu-id="21d26-327">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="21d26-328">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-328">**Mitigations**</span></span>

<span data-ttu-id="21d26-329">Wenn die Eigenschaft als Fremdschlüssel und daher Teil des Primärschlüssels vorgesehen war, müssen Sie diese explizit als solche festlegen.</span><span class="sxs-lookup"><span data-stu-id="21d26-329">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="21d26-330">Die Datenbankverbindung wird jetzt geschlossen, wenn sie nicht mehr verwendet wird, bevor TransactionScope abgeschlossen wurde</span><span class="sxs-lookup"><span data-stu-id="21d26-330">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="21d26-331">Issue #14218</span><span class="sxs-lookup"><span data-stu-id="21d26-331">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="21d26-332">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-332">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="21d26-333">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-333">**Old behavior**</span></span>

<span data-ttu-id="21d26-334">Wenn vor Version 3.0 in EF Core der Kontext die Verbindung in einem `TransactionScope` öffnet, bleibt die Verbindung geöffnet, während der aktuelle `TransactionScope` aktiv ist.</span><span class="sxs-lookup"><span data-stu-id="21d26-334">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="21d26-335">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-335">**New behavior**</span></span>

<span data-ttu-id="21d26-336">Ab Version 3.0 schließt EF Core die Verbindung, sobald sie nicht mehr verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="21d26-336">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="21d26-337">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-337">**Why**</span></span>

<span data-ttu-id="21d26-338">Diese Änderung ermöglicht die Verwendung mehrerer Kontexte in demselben `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="21d26-338">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="21d26-339">Das neue Verhalten entspricht außerdem EF6.</span><span class="sxs-lookup"><span data-stu-id="21d26-339">The new behavior also matches EF6.</span></span>

<span data-ttu-id="21d26-340">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-340">**Mitigations**</span></span>

<span data-ttu-id="21d26-341">Wenn die Verbindung geöffnet bleiben muss, wird durch einen expliziten Aufruf von `OpenConnection()` sichergestellt, dass EF Core diese nicht vorzeitig schließt:</span><span class="sxs-lookup"><span data-stu-id="21d26-341">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="21d26-342">Für jede Eigenschaft wird separat ein ganzzahliger speicherinterner Schlüssel generiert</span><span class="sxs-lookup"><span data-stu-id="21d26-342">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="21d26-343">Issue #6872</span><span class="sxs-lookup"><span data-stu-id="21d26-343">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="21d26-344">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-344">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="21d26-345">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-345">**Old behavior**</span></span>

<span data-ttu-id="21d26-346">Vor Version 3.0 wurde in EF Core ein gemeinsam verwendeter Wertegenerator für alle speicherinternen ganzzahligen Schlüsseleigenschaften verwendet.</span><span class="sxs-lookup"><span data-stu-id="21d26-346">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="21d26-347">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-347">**New behavior**</span></span>

<span data-ttu-id="21d26-348">Ab Version 3.0 von EF Core wird für jede ganzzahlige Schlüsseleigenschaft ein eigener Wertegenerator bei der Verwendung der In-Memory Database verwendet.</span><span class="sxs-lookup"><span data-stu-id="21d26-348">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="21d26-349">Wenn die Datenbank gelöscht wird, wird die Schlüsselgenerierung für alle Tabellen zurückgesetzt.</span><span class="sxs-lookup"><span data-stu-id="21d26-349">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="21d26-350">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-350">**Why**</span></span>

<span data-ttu-id="21d26-351">Diese Änderung wurde vorgenommen, um die speicherinterne Schlüsselgenerierung enger mit der Schlüsselgenerierung für echte Datenbanken abzustimmen. Außerdem sollen bei Verwendung der In-Memory Database Tests leichter voneinander isoliert werden können.</span><span class="sxs-lookup"><span data-stu-id="21d26-351">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="21d26-352">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-352">**Mitigations**</span></span>

<span data-ttu-id="21d26-353">Wenn Anwendungen von bestimmten speicherinternen Schlüsselwerten abhängig sind, funktionieren Erstere durch die eingeführte Änderung möglicherweise nicht mehr.</span><span class="sxs-lookup"><span data-stu-id="21d26-353">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="21d26-354">Versuchen Sie, Abhängigkeiten von bestimmten Schlüsselwerten zu vermeiden, oder passen Sie die Anwendung an das neue Verhalten an.</span><span class="sxs-lookup"><span data-stu-id="21d26-354">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="21d26-355">Unterstützungsfelder werden standardmäßig verwendet</span><span class="sxs-lookup"><span data-stu-id="21d26-355">Backing fields are used by default</span></span>

[<span data-ttu-id="21d26-356">Issue #12430</span><span class="sxs-lookup"><span data-stu-id="21d26-356">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="21d26-357">Diese Änderung wird in Vorschauversion 2 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-357">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="21d26-358">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-358">**Old behavior**</span></span>

<span data-ttu-id="21d26-359">Vor Version 3.0 wurde in EF Core mithilfe der Getter- und Settermethoden standardmäßig der Eigenschaftswert gelesen und geschrieben. Das war selbst dann der Fall, wenn das Unterstützungsfeld für eine Eigenschaft nicht bekannt war.</span><span class="sxs-lookup"><span data-stu-id="21d26-359">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="21d26-360">Eine Ausnahme war die Abfrageausführung, bei der das Unterstützungsfeld direkt festgelegt wurde, wenn es bekannt war.</span><span class="sxs-lookup"><span data-stu-id="21d26-360">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="21d26-361">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-361">**New behavior**</span></span>

<span data-ttu-id="21d26-362">Ab Version 3.0 wird in EF Core die Eigenschaft mithilfe des Unterstützungsfelds gelesen und geschrieben, wenn dieses für eine Eigenschaft bekannt ist.</span><span class="sxs-lookup"><span data-stu-id="21d26-362">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="21d26-363">Dies kann dazu führen, dass eine Anwendung nicht mehr funktioniert, wenn sie auf zusätzliches Verhalten angewiesen ist, das im Code der Getter- und Settermethoden definiert ist.</span><span class="sxs-lookup"><span data-stu-id="21d26-363">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="21d26-364">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-364">**Why**</span></span>

<span data-ttu-id="21d26-365">Diese Änderung wurde vorgenommen, um zu verhindern, dass in EF Core fälschlicherweise immer dann Geschäftslogik ausgelöst wird, wenn Datenbankvorgänge für Entitäten durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="21d26-365">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="21d26-366">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-366">**Mitigations**</span></span>

<span data-ttu-id="21d26-367">Sie können das Verhalten von vor Version 3.0 wiederherstellen, indem Sie in `ModelBuilder` den Zugriffsmodus für die Eigenschaft konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="21d26-367">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="21d26-368">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="21d26-368">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="21d26-369">Eine Ausnahme wird ausgelöst, wenn mehrere kompatible Unterstützungsfelder gefunden werden</span><span class="sxs-lookup"><span data-stu-id="21d26-369">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="21d26-370">Issue #12523</span><span class="sxs-lookup"><span data-stu-id="21d26-370">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="21d26-371">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-371">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="21d26-372">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-372">**Old behavior**</span></span>

<span data-ttu-id="21d26-373">Vor Version 3.0 wurde in EF Core ein Unterstützungsfeld auf Grundlage einer Rangfolge ausgewählt, wenn die Regeln zum Suchen eines Eigenschaftsfelds auf mehrere Felder zutrafen.</span><span class="sxs-lookup"><span data-stu-id="21d26-373">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="21d26-374">Dabei konnte es vorkommen, dass in mehrdeutigen Fällen das falsche Feld verwendet wurde.</span><span class="sxs-lookup"><span data-stu-id="21d26-374">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="21d26-375">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-375">**New behavior**</span></span>

<span data-ttu-id="21d26-376">Ab Version 3.0 wird in EF Core eine Ausnahme ausgelöst, wenn sich mehrere Felder auf dieselbe Eigenschaft beziehen.</span><span class="sxs-lookup"><span data-stu-id="21d26-376">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="21d26-377">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-377">**Why**</span></span>

<span data-ttu-id="21d26-378">Diese Änderung wurde vorgenommen, um zu vermeiden, dass automatisch ein falsches Feld verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="21d26-378">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="21d26-379">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-379">**Mitigations**</span></span>

<span data-ttu-id="21d26-380">Für Eigenschaften mit mehrdeutigen Unterstützungsfeldern muss das zu verwendende Feld explizit festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="21d26-380">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="21d26-381">Mit der Fluent-API ist dies beispielsweise wie folgt möglich:</span><span class="sxs-lookup"><span data-stu-id="21d26-381">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="21d26-382">„Nur-Feld“-Eigenschaftsnamen sollten dem Feldnamen entsprechen</span><span class="sxs-lookup"><span data-stu-id="21d26-382">Field-only property names should match the field name</span></span>

<span data-ttu-id="21d26-383">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-383">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="21d26-384">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-384">**Old behavior**</span></span>

<span data-ttu-id="21d26-385">Vor Version 3.0 konnte in EF Core eine Eigenschaft durch einen Zeichenfolgenwert angegeben werden, und wenn keine Eigenschaft mit diesem Namen im CLR-Typ gefunden wurde, versuchte EF Core, mithilfe von Konventionsregeln ein übereinstimmendes Feld zu finden.</span><span class="sxs-lookup"><span data-stu-id="21d26-385">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="21d26-386">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-386">**New behavior**</span></span>

<span data-ttu-id="21d26-387">Ab Version 3.0 muss in EF Core eine „Nur-Feld“-Eigenschaft mit dem Namen des Felds genau übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="21d26-387">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="21d26-388">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-388">**Why**</span></span>

<span data-ttu-id="21d26-389">Diese Änderung wurde vorgenommen, um zu vermeiden, dass das gleiche Feld für zwei Eigenschaften mit ähnlichem Namen verwendet wird. Durch die Änderung entsprechen nun auch die Übereinstimmungsregeln für „Nur-Feld“-Eigenschaften den Regeln für Eigenschaften, die CLR-Eigenschaften zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="21d26-389">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="21d26-390">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-390">**Mitigations**</span></span>

<span data-ttu-id="21d26-391">„Nur-Feld“-Eigenschaften müssen so benannt werden wie das Feld, dem sie zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="21d26-391">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="21d26-392">In einer späteren Vorschauversion von EF Core 3.0 soll das explizite Konfigurieren eines Feldnamens, der sich vom Eigenschaftsnamen unterscheidet, erneut aktiviert werden:</span><span class="sxs-lookup"><span data-stu-id="21d26-392">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="21d26-393">Von AddDbContext/AddDbContextPool werden AddLogging und AddMemoryCache nicht mehr aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="21d26-393">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="21d26-394">Issue #14756</span><span class="sxs-lookup"><span data-stu-id="21d26-394">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="21d26-395">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-395">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="21d26-396">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-396">**Old behavior**</span></span>

<span data-ttu-id="21d26-397">Vor EF Core 3.0 wurden beim Aufruf von `AddDbContext` oder `AddDbContextPool` durch Aufrufe von [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) und [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache) auch Protokollierungs- und Arbeitsspeichercaching-Dienste bei DI registriert.</span><span class="sxs-lookup"><span data-stu-id="21d26-397">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="21d26-398">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-398">**New behavior**</span></span>

<span data-ttu-id="21d26-399">Ab EF Core 3.0 werden diese Dienste durch `AddDbContext` und `AddDbContextPool` nicht mehr bei Dependency Injection (DI) registriert.</span><span class="sxs-lookup"><span data-stu-id="21d26-399">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="21d26-400">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-400">**Why**</span></span>

<span data-ttu-id="21d26-401">Für EF Core 3.0 ist es nicht erforderlich, dass diese Dienste im DI-Container der Anwendung enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="21d26-401">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="21d26-402">Wenn `ILoggerFactory` jedoch im DI-Container der Anwendung registriert ist, wird sie nach wie vor von EF Core verwendet.</span><span class="sxs-lookup"><span data-stu-id="21d26-402">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="21d26-403">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-403">**Mitigations**</span></span>

<span data-ttu-id="21d26-404">Wenn Ihre Anwendung diese Dienste benötigt, registrieren Sie sie explizit mit [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) oder [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache) beim DI-Container.</span><span class="sxs-lookup"><span data-stu-id="21d26-404">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="21d26-405">DbContext.Entry erkennt nun mit DetectChanges lokale Änderungen</span><span class="sxs-lookup"><span data-stu-id="21d26-405">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="21d26-406">Issue #13552</span><span class="sxs-lookup"><span data-stu-id="21d26-406">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="21d26-407">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-407">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="21d26-408">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-408">**Old behavior**</span></span>

<span data-ttu-id="21d26-409">Vor Version 3.0 führte in EF Core ein Aufruf von `DbContext.Entry` dazu, dass Änderungen an allen überwachten Entitäten erkannt wurden.</span><span class="sxs-lookup"><span data-stu-id="21d26-409">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="21d26-410">Dadurch wurde sichergestellt, dass der Zustand in `EntityEntry` immer aktuell war.</span><span class="sxs-lookup"><span data-stu-id="21d26-410">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="21d26-411">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-411">**New behavior**</span></span>

<span data-ttu-id="21d26-412">Ab Version 3.0 werden in EF Core Änderungen bei einem Aufruf von `DbContext.Entry` nur in der angegebenen Entität und in allen überwachten Prinzipalentitäten erkannt.</span><span class="sxs-lookup"><span data-stu-id="21d26-412">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="21d26-413">Änderungen an anderer Stelle werden daher durch den Aufruf dieser Methode möglicherweise nicht erkannt, was sich auf den Anwendungszustand auswirken kann.</span><span class="sxs-lookup"><span data-stu-id="21d26-413">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="21d26-414">Beachten Sie, dass selbst die Erkennung lokaler Änderungen deaktiviert wird, wenn `ChangeTracker.AutoDetectChangesEnabled` auf `false` festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="21d26-414">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="21d26-415">Andere Methoden wie `ChangeTracker.Entries` und `SaveChanges`, die eine Änderungserkennung auslösen, führen nach wie vor zu einem vollständigen `DetectChanges`-Vorgang für alle überwachten Entitäten.</span><span class="sxs-lookup"><span data-stu-id="21d26-415">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="21d26-416">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-416">**Why**</span></span>

<span data-ttu-id="21d26-417">Diese Änderung wurde vorgenommen, um die Leistung bei der standardmäßigen Verwendung von `context.Entry` zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="21d26-417">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="21d26-418">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-418">**Mitigations**</span></span>

<span data-ttu-id="21d26-419">Rufen Sie `ChgangeTracker.DetectChanges()` explizit vor dem Aufruf von `Entry` auf, wenn das Verhalten vor Version 3.0 beibehalten werden soll.</span><span class="sxs-lookup"><span data-stu-id="21d26-419">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="21d26-420">Schlüssel aus Zeichenfolgen und Bytearrays werden standardmäßig nicht vom Client generiert</span><span class="sxs-lookup"><span data-stu-id="21d26-420">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="21d26-421">Issue #14617</span><span class="sxs-lookup"><span data-stu-id="21d26-421">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="21d26-422">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-422">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="21d26-423">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-423">**Old behavior**</span></span>

<span data-ttu-id="21d26-424">Vor Version 3.0 konnten in EF Core `string`- und `byte[]`-Schlüsseleigenschaften verwendet werden, ohne dass explizit ein anderer Wert als NULL festgelegt werden musste.</span><span class="sxs-lookup"><span data-stu-id="21d26-424">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="21d26-425">In diesem Fall wurde der Schlüsselwert auf dem Client als GUID generiert und für `byte[]` in Bytes serialisiert.</span><span class="sxs-lookup"><span data-stu-id="21d26-425">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="21d26-426">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-426">**New behavior**</span></span>

<span data-ttu-id="21d26-427">Ab Version 3.0 wird in EF Core eine Ausnahme ausgelöst, wenn kein Schlüsselwert festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="21d26-427">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="21d26-428">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-428">**Why**</span></span>

<span data-ttu-id="21d26-429">Diese Änderung wurde vorgenommen, da vom Client generierte `string`/`byte[]`-Werte in der Regel nicht sinnvoll sind. Das Standardverhalten führte außerdem zu kaum nachvollziehbaren generierten Schlüsselwerten.</span><span class="sxs-lookup"><span data-stu-id="21d26-429">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="21d26-430">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-430">**Mitigations**</span></span>

<span data-ttu-id="21d26-431">Wenn Sie das Verhalten vor Version 3.0 nutzen möchten, müssen Sie explizit festlegen, dass für die Schlüsseleigenschaften generierte Werte verwendet sollen, wenn kein anderer Nicht-NULL-Wert festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="21d26-431">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="21d26-432">Dies ist beispielsweise mit der Fluent-API möglich:</span><span class="sxs-lookup"><span data-stu-id="21d26-432">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="21d26-433">Alternativ bieten sich auch Datenanmerkungen an:</span><span class="sxs-lookup"><span data-stu-id="21d26-433">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="21d26-434">ILoggerFactory ist nun ein bereichsbezogener Dienst</span><span class="sxs-lookup"><span data-stu-id="21d26-434">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="21d26-435">Issue #14698</span><span class="sxs-lookup"><span data-stu-id="21d26-435">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="21d26-436">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-436">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="21d26-437">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-437">**Old behavior**</span></span>

<span data-ttu-id="21d26-438">Vor Version 3.0 wurde `ILoggerFactory` in EF Core als Singletondienst registriert.</span><span class="sxs-lookup"><span data-stu-id="21d26-438">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="21d26-439">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-439">**New behavior**</span></span>

<span data-ttu-id="21d26-440">Ab Version 3.0 wird in EF Core `ILoggerFactory` als bereichsbezogener Dienst registriert.</span><span class="sxs-lookup"><span data-stu-id="21d26-440">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="21d26-441">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-441">**Why**</span></span>

<span data-ttu-id="21d26-442">Diese Änderung wurde vorgenommen, um einer `DbContext`-Instanz eine Protokollierung zuordnen zu können. Dadurch werden weitere Funktionen bereitgestellt. In einigen Fällen wird außerdem schädliches Verhalten wie etwa die schnelle Zunahme von internen Dienstanbietern beseitigt.</span><span class="sxs-lookup"><span data-stu-id="21d26-442">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="21d26-443">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-443">**Mitigations**</span></span>

<span data-ttu-id="21d26-444">Diese Änderung wirkt sich nur dann auf den Anwendungscode aus, wenn benutzerdefinierte Dienste auf dem internen Dienstanbieter von EF Core registriert und verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="21d26-444">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="21d26-445">Ein solches Szenario ist aber unüblich.</span><span class="sxs-lookup"><span data-stu-id="21d26-445">This isn't common.</span></span>
<span data-ttu-id="21d26-446">In einem derartigen Fall funktioniert fast alles wie vorher. Singletondienste, die von `ILoggerFactory` abhängig sind, müssen jedoch so angepasst werden, dass `ILoggerFactory` auf andere Weise abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="21d26-446">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="21d26-447">Erstellen Sie in diesen Situationen bitte im [GitHub-Issuetracker für EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) ein Issue, in dem Sie beschreiben, wie Sie `ILoggerFactory` verwenden. So können wir in Zukunft erneute Breaking Changes vermeiden.</span><span class="sxs-lookup"><span data-stu-id="21d26-447">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="21d26-448">Für Proxys mit Lazy Loading (trägem Laden) gilt nicht mehr die Annahme, dass Navigationseigenschaften vollständig geladen sind</span><span class="sxs-lookup"><span data-stu-id="21d26-448">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="21d26-449">Issue #12780</span><span class="sxs-lookup"><span data-stu-id="21d26-449">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="21d26-450">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-450">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="21d26-451">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-451">**Old behavior**</span></span>

<span data-ttu-id="21d26-452">Vor Version 3.0 gab es in EF Core nach der Freigabe eines `DbContext`-Objekts keine Möglichkeit, zu ermitteln, ob eine aus dem Kontext abgerufene Navigationseigenschaft für eine Entität vollständig geladen war.</span><span class="sxs-lookup"><span data-stu-id="21d26-452">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="21d26-453">Stattdessen galt für Proxys die Annahme, dass eine Verweisnavigation geladen ist, falls für diese ein anderer Wert als NULL vorliegt. Außerdem wurde davon ausgegangen, dass eine Sammlungsnavigation geladen ist, wenn sie nicht leer ist.</span><span class="sxs-lookup"><span data-stu-id="21d26-453">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="21d26-454">In diesen Fällen entsprach das Lazy Loading einer Nulloperation („No-Op“).</span><span class="sxs-lookup"><span data-stu-id="21d26-454">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="21d26-455">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-455">**New behavior**</span></span>

<span data-ttu-id="21d26-456">Ab Version 3.0 überwachen in EF Core Proxys, ob eine Navigationseigenschaft geladen ist.</span><span class="sxs-lookup"><span data-stu-id="21d26-456">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="21d26-457">Wenn also auf eine Navigationseigenschaft zugegriffen werden soll, die nach der Freigabe des Kontexts geladen wird, ist dies immer eine No-Op-Operation. Das ist selbst dann der Fall, wenn die geladene Navigation leer oder NULL ist.</span><span class="sxs-lookup"><span data-stu-id="21d26-457">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="21d26-458">Wenn umgekehrt auf eine nicht geladene Navigationseigenschaft zugegriffen werden soll und der Kontext bereits freigegeben wurde, wird eine Ausnahme ausgelöst. Dieser Fall tritt auch dann ein, wenn die Navigationseigenschaft eine nicht leere Sammlung ist.</span><span class="sxs-lookup"><span data-stu-id="21d26-458">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="21d26-459">In dieser Situation wird also vom Anwendungscode versucht, zu einem ungültigen Zeitpunkt Lazy Loading durchzuführen. Die Anwendung sollte so angepasst werden, dass dieser Vorgang vermieden wird.</span><span class="sxs-lookup"><span data-stu-id="21d26-459">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="21d26-460">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-460">**Why**</span></span>

<span data-ttu-id="21d26-461">Diese Änderung wurde vorgenommen, um das Verhalten beim Lazy Loading einer freigegebenen `DbContext`-Instanz konsistent und korrekt zu gestalten.</span><span class="sxs-lookup"><span data-stu-id="21d26-461">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="21d26-462">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-462">**Mitigations**</span></span>

<span data-ttu-id="21d26-463">Aktualisieren Sie den Anwendungscode so, dass kein Lazy Loading durchgeführt wird, nachdem der Kontext freigegeben wurde. Alternativ können Sie auch, wie in der Ausnahmemeldung beschrieben wird, eine No-Op-Operation festlegen.</span><span class="sxs-lookup"><span data-stu-id="21d26-463">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="21d26-464">Die Erstellung zu vieler interner Dienstanbieter führt standardmäßig zu einem Fehler</span><span class="sxs-lookup"><span data-stu-id="21d26-464">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="21d26-465">Issue #10236</span><span class="sxs-lookup"><span data-stu-id="21d26-465">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="21d26-466">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-466">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="21d26-467">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-467">**Old behavior**</span></span>

<span data-ttu-id="21d26-468">Vor Version 3.0 wurde in EF Core eine Warnung für eine Anwendung protokolliert, die unverhältnismäßig viele interne Dienstanbieter erstellte.</span><span class="sxs-lookup"><span data-stu-id="21d26-468">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="21d26-469">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-469">**New behavior**</span></span>

<span data-ttu-id="21d26-470">Ab Version 3.0 wird diese Warnung in EF Core als Fehler betrachtet, und eine Ausnahme wird ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="21d26-470">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="21d26-471">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-471">**Why**</span></span>

<span data-ttu-id="21d26-472">Diese Änderung wurde vorgenommen, damit auf den oben beschriebenen Fall explizit hingewiesen wird. So soll die Entwicklung von besserem Anwendungscode erleichtert werden.</span><span class="sxs-lookup"><span data-stu-id="21d26-472">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="21d26-473">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-473">**Mitigations**</span></span>

<span data-ttu-id="21d26-474">Wenn dieser Fehler auftritt, müssen Sie die Grundursache ermitteln und die Erstellung vieler interner Dienstanbieter unterbinden.</span><span class="sxs-lookup"><span data-stu-id="21d26-474">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="21d26-475">Sie können den Fehler allerdings auch wieder in eine Warnung konvertierten (oder ihn ignorieren), indem Sie `DbContextOptionsBuilder` konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="21d26-475">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="21d26-476">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="21d26-476">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="21d26-477">Neues Verhalten für HasOne/HasMany bei Aufruf mit einer einzelnen Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="21d26-477">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="21d26-478">Issue #9171</span><span class="sxs-lookup"><span data-stu-id="21d26-478">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="21d26-479">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-479">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="21d26-480">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-480">**Old behavior**</span></span>

<span data-ttu-id="21d26-481">Vor EF Core 3.0 wurde Code, der `HasOne` oder `HasMany` mit einer einzelnen Zeichenfolge aufruft, auf irritierende Weise interpretiert.</span><span class="sxs-lookup"><span data-stu-id="21d26-481">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="21d26-482">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="21d26-482">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="21d26-483">Der Code scheint `Samurai` mit einem anderen Entitätstyp über die `Entrance`-Navigationseigenschaft zu verknüpfen, die möglicherweise privat ist.</span><span class="sxs-lookup"><span data-stu-id="21d26-483">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="21d26-484">Tatsächlich versucht dieser Code, eine Beziehung zu einem Entitätstyp namens `Entrance` ohne Navigationseigenschaft herzustellen.</span><span class="sxs-lookup"><span data-stu-id="21d26-484">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="21d26-485">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-485">**New behavior**</span></span>

<span data-ttu-id="21d26-486">Ab EF Core 3.0 führt der obige Code jetzt die erwartete Aktion durch.</span><span class="sxs-lookup"><span data-stu-id="21d26-486">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="21d26-487">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-487">**Why**</span></span>

<span data-ttu-id="21d26-488">Das alte Verhalten war sehr verwirrend, vor allem beim Lesen des Konfigurationscodes und bei der Suche nach Fehlern.</span><span class="sxs-lookup"><span data-stu-id="21d26-488">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="21d26-489">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-489">**Mitigations**</span></span>

<span data-ttu-id="21d26-490">Dies führt nur zu Fehlern in Anwendungen, die Beziehungen explizit unter Verwendung von Zeichenfolgen für Typnamen und ohne explizite Angabe der Navigationseigenschaft konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="21d26-490">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="21d26-491">Dies ist nicht üblich.</span><span class="sxs-lookup"><span data-stu-id="21d26-491">This is not common.</span></span>
<span data-ttu-id="21d26-492">Das vorherige Verhalten kann durch explizite Übergabe von `null` für den Namen der Navigationseigenschaft erzielt werden.</span><span class="sxs-lookup"><span data-stu-id="21d26-492">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="21d26-493">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="21d26-493">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="21d26-494">Der Rückgabetyp für mehrere asynchrone Methoden wurde von Task in ValueTask geändert</span><span class="sxs-lookup"><span data-stu-id="21d26-494">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="21d26-495">Issue #15184</span><span class="sxs-lookup"><span data-stu-id="21d26-495">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="21d26-496">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-496">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="21d26-497">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-497">**Old behavior**</span></span>

<span data-ttu-id="21d26-498">Die folgenden asynchronen Methoden gaben zuvor `Task<T>` zurück:</span><span class="sxs-lookup"><span data-stu-id="21d26-498">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="21d26-499">`ValueGenerator.NextValueAsync()` (und abgeleitete Klassen)</span><span class="sxs-lookup"><span data-stu-id="21d26-499">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="21d26-500">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-500">**New behavior**</span></span>

<span data-ttu-id="21d26-501">Die oben genannten Methoden geben nun `ValueTask<T>` über dieselbe `T` wie zuvor zurück.</span><span class="sxs-lookup"><span data-stu-id="21d26-501">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="21d26-502">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-502">**Why**</span></span>

<span data-ttu-id="21d26-503">Durch diese Änderung verringert sich die Anzahl der Heapzuordnungen, die beim Aufrufen dieser Methoden entstehen, und dies führt zu einer Verbesserung der allgemeinen Leistung.</span><span class="sxs-lookup"><span data-stu-id="21d26-503">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="21d26-504">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-504">**Mitigations**</span></span>

<span data-ttu-id="21d26-505">Anwendungen, die einfach die oben genannten APIs erwarten, müssen nur neu kompiliert werden – Quelländerungen sind nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="21d26-505">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="21d26-506">Eine komplexere Verwendung (z.B. Übergeben der zurückgegebenen `Task` an `Task.WhenAny()`) erfordern in der Regel, dass die zurückgegebene `ValueTask<T>` durch einen Aufruf von `AsTask()` in `Task<T>` konvertiert wird.</span><span class="sxs-lookup"><span data-stu-id="21d26-506">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="21d26-507">Beachten Sie, dass dadurch die mit dieser Änderung verbundene Verringerung der Zuordnungen aufgehoben wird.</span><span class="sxs-lookup"><span data-stu-id="21d26-507">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="21d26-508">Die Anmerkung Relational:TypeMapping heißt nun nur noch TypeMapping</span><span class="sxs-lookup"><span data-stu-id="21d26-508">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="21d26-509">Issue #9913</span><span class="sxs-lookup"><span data-stu-id="21d26-509">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="21d26-510">Diese Änderung wird in Vorschauversion 2 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-510">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="21d26-511">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-511">**Old behavior**</span></span>

<span data-ttu-id="21d26-512">Der Anmerkungsname für Typzuordnungsanmerkungen war bisher Relational:TypeMapping.</span><span class="sxs-lookup"><span data-stu-id="21d26-512">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="21d26-513">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-513">**New behavior**</span></span>

<span data-ttu-id="21d26-514">Der Anmerkungsname für Typzuordnungsanmerkungen lautet nun TypeMapping.</span><span class="sxs-lookup"><span data-stu-id="21d26-514">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="21d26-515">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-515">**Why**</span></span>

<span data-ttu-id="21d26-516">Typzuordnungen werden nicht mehr ausschließlich für Anbieter relationaler Datenbanken verwendet.</span><span class="sxs-lookup"><span data-stu-id="21d26-516">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="21d26-517">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-517">**Mitigations**</span></span>

<span data-ttu-id="21d26-518">Diese Änderung ist nur dann ein Breaking Change, wenn Anwendungen direkt über eine Anmerkung auf die Typzuordnung zugreifen. Dieses Szenario ist jedoch unüblich.</span><span class="sxs-lookup"><span data-stu-id="21d26-518">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="21d26-519">Verzichten Sie daher darauf, die Anmerkung direkt zu verwenden, und greifen Sie stattdessen über die Fluent-API auf Typzuordnungen zu, um das Problem zu beheben.</span><span class="sxs-lookup"><span data-stu-id="21d26-519">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="21d26-520">ToTable löst für einen abgeleiteten Typ eine Ausnahme aus</span><span class="sxs-lookup"><span data-stu-id="21d26-520">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="21d26-521">Issue #11811</span><span class="sxs-lookup"><span data-stu-id="21d26-521">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="21d26-522">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-522">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="21d26-523">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-523">**Old behavior**</span></span>

<span data-ttu-id="21d26-524">Vor Version 3.0 wurde in EF Core der Aufruf von `ToTable()` für einen abgeleiteten Typ ignoriert, da TPH die einzige Strategie zur Vererbungszuordnung war und diese in unzulässigen Fällen angewendet wurde.</span><span class="sxs-lookup"><span data-stu-id="21d26-524">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="21d26-525">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-525">**New behavior**</span></span>

<span data-ttu-id="21d26-526">Ab Version 3.0 wird in EF Core eine Ausnahme ausgelöst, um später eine unerwartete Zuordnung zu vermeiden, wenn `ToTable()` für einen abgeleiteten Typ aufgerufen wird. Dieser Schritt dient auch als Vorbereitung für die TPT- und TPC-Unterstützung, die in einem künftigen Release folgen wird.</span><span class="sxs-lookup"><span data-stu-id="21d26-526">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="21d26-527">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-527">**Why**</span></span>

<span data-ttu-id="21d26-528">Derzeit ist es nicht zulässig, einen abgeleiteten Typ einer anderen Tabelle zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="21d26-528">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="21d26-529">Durch diese Änderung werden Breaking Changes vermieden, wenn der beschriebene Vorgang in Zukunft zulässig wird.</span><span class="sxs-lookup"><span data-stu-id="21d26-529">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="21d26-530">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-530">**Mitigations**</span></span>

<span data-ttu-id="21d26-531">Entfernen Sie alle Zuordnungen von abgeleiteten Typen zu anderen Tabellen.</span><span class="sxs-lookup"><span data-stu-id="21d26-531">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="21d26-532">ForSqlServerHasIndex wurde durch HasIndex ersetzt</span><span class="sxs-lookup"><span data-stu-id="21d26-532">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="21d26-533">Issue #12366</span><span class="sxs-lookup"><span data-stu-id="21d26-533">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="21d26-534">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-534">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="21d26-535">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-535">**Old behavior**</span></span>

<span data-ttu-id="21d26-536">Vor Version 3.0 konnten in EF Core mit `ForSqlServerHasIndex().ForSqlServerInclude()` Spalten konfiguriert werden, die mit `INCLUDE` verwendet wurden.</span><span class="sxs-lookup"><span data-stu-id="21d26-536">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="21d26-537">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-537">**New behavior**</span></span>

<span data-ttu-id="21d26-538">Ab Version 3.0 wird in EF Core die Nutzung von `Include` für einen Index auf der relationalen Ebene unterstützt.</span><span class="sxs-lookup"><span data-stu-id="21d26-538">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="21d26-539">Verwenden Sie `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="21d26-539">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="21d26-540">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-540">**Why**</span></span>

<span data-ttu-id="21d26-541">Diese Änderung wurde vorgenommen, um die API für Indizes mit `Include` an einer zentralen Stelle für alle Datenbankanbieter zusammenzuführen.</span><span class="sxs-lookup"><span data-stu-id="21d26-541">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="21d26-542">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-542">**Mitigations**</span></span>

<span data-ttu-id="21d26-543">Verwenden Sie wie oben beschrieben die neue API.</span><span class="sxs-lookup"><span data-stu-id="21d26-543">Use the new API, as shown above.</span></span>

## <a name="metadata-api-changes"></a><span data-ttu-id="21d26-544">Metadaten-API-Änderungen</span><span class="sxs-lookup"><span data-stu-id="21d26-544">Metadata API changes</span></span>

[<span data-ttu-id="21d26-545">Issue #214</span><span class="sxs-lookup"><span data-stu-id="21d26-545">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="21d26-546">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-546">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="21d26-547">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-547">**New behavior**</span></span>

<span data-ttu-id="21d26-548">Die folgenden Eigenschaften wurden in Erweiterungsmethoden konvertiert:</span><span class="sxs-lookup"><span data-stu-id="21d26-548">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="21d26-549">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-549">**Why**</span></span>

<span data-ttu-id="21d26-550">Mit dieser Änderung wird die Implementierung der oben genannten Schnittstellen vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="21d26-550">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="21d26-551">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-551">**Mitigations**</span></span>

<span data-ttu-id="21d26-552">Verwenden Sie die neuen Erweiterungsmethoden.</span><span class="sxs-lookup"><span data-stu-id="21d26-552">Use the new extension methods.</span></span>

## <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="21d26-553">Anbieterspezifische Metadaten-API-Änderungen</span><span class="sxs-lookup"><span data-stu-id="21d26-553">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="21d26-554">Issue #214</span><span class="sxs-lookup"><span data-stu-id="21d26-554">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="21d26-555">Diese Änderung wird in Vorschauversion 6 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-555">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="21d26-556">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-556">**New behavior**</span></span>

<span data-ttu-id="21d26-557">Die anbieterspezifischen Erweiterungsmethoden werden vereinfacht:</span><span class="sxs-lookup"><span data-stu-id="21d26-557">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.GetSqlServerIsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.ForSqlServerUseIdentityColumn()`

<span data-ttu-id="21d26-558">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-558">**Why**</span></span>

<span data-ttu-id="21d26-559">Mit dieser Änderung wird die Implementierung der oben genannten Erweiterungsmethoden vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="21d26-559">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="21d26-560">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-560">**Mitigations**</span></span>

<span data-ttu-id="21d26-561">Verwenden Sie die neuen Erweiterungsmethoden.</span><span class="sxs-lookup"><span data-stu-id="21d26-561">Use the new extension methods.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="21d26-562">EF Core sendet keine PRAGMA-Anweisungen mehr, um Fremdschlüssel in SQLite zu erzwingen</span><span class="sxs-lookup"><span data-stu-id="21d26-562">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="21d26-563">Issue #12151</span><span class="sxs-lookup"><span data-stu-id="21d26-563">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="21d26-564">Diese Änderung wird in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-564">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="21d26-565">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-565">**Old behavior**</span></span>

<span data-ttu-id="21d26-566">Vor Version 3.0 wurden in EF Core `PRAGMA foreign_keys = 1`-Anweisungen gesendet, wenn eine Verbindung mit SQLite hergestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="21d26-566">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="21d26-567">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-567">**New behavior**</span></span>

<span data-ttu-id="21d26-568">Ab Version 3.0 werden in EF Core keine `PRAGMA foreign_keys = 1`-Anweisungen mehr gesendet, wenn eine Verbindung mit SQLite hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="21d26-568">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="21d26-569">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-569">**Why**</span></span>

<span data-ttu-id="21d26-570">Diese Änderung wurde vorgenommen, da von EF Core standardmäßig `SQLitePCLRaw.bundle_e_sqlite3` verwendet wird. Dadurch ist die Erzwingung von Fremdschlüsseln standardmäßig aktiviert und muss nicht jedes Mal explizit aktiviert werden, wenn eine Verbindung hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="21d26-570">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="21d26-571">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-571">**Mitigations**</span></span>

<span data-ttu-id="21d26-572">Fremdschlüssel sind in SQLitePCLRaw.bundle_e_sqlite3 standardmäßig aktiviert. Dieses Bundle wird standardmäßig von EF Core verwendet.</span><span class="sxs-lookup"><span data-stu-id="21d26-572">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="21d26-573">In anderen Fällen können Fremdschlüssel durch die Angabe `Foreign Keys=True` in der Verbindungszeichenfolge aktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="21d26-573">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="21d26-574">SQLitePCLRaw.bundle_e_sqlite3 ist nun eine Abhängigkeit von Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="21d26-574">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="21d26-575">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-575">**Old behavior**</span></span>

<span data-ttu-id="21d26-576">Vor Version 3.0 wurde von EF Core `SQLitePCLRaw.bundle_green` verwendet.</span><span class="sxs-lookup"><span data-stu-id="21d26-576">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="21d26-577">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-577">**New behavior**</span></span>

<span data-ttu-id="21d26-578">Ab Version 3.0 wird von EF Core `SQLitePCLRaw.bundle_e_sqlite3` verwendet.</span><span class="sxs-lookup"><span data-stu-id="21d26-578">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="21d26-579">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-579">**Why**</span></span>

<span data-ttu-id="21d26-580">Diese Änderung wurde vorgenommen, damit die unter iOS verwendete Version von SQLite sich konsistent zu Versionen auf anderen Plattformen verhält.</span><span class="sxs-lookup"><span data-stu-id="21d26-580">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="21d26-581">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-581">**Mitigations**</span></span>

<span data-ttu-id="21d26-582">Konfigurieren Sie `Microsoft.Data.Sqlite` so, dass ein anderes `SQLitePCLRaw`-Bundle verwendet wird, um die native SQLite-Version unter iOS zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="21d26-582">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="21d26-583">GUID-Werte werden jetzt als TEXT in SQLite gespeichert</span><span class="sxs-lookup"><span data-stu-id="21d26-583">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="21d26-584">Issue #15078</span><span class="sxs-lookup"><span data-stu-id="21d26-584">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="21d26-585">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-585">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="21d26-586">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-586">**Old behavior**</span></span>

<span data-ttu-id="21d26-587">GUID-Werte wurden in der Vergangenheit in SQLite als BLOB-Werte gespeichert.</span><span class="sxs-lookup"><span data-stu-id="21d26-587">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="21d26-588">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-588">**New behavior**</span></span>

<span data-ttu-id="21d26-589">GUID-Werte werden jetzt als TEXT gespeichert.</span><span class="sxs-lookup"><span data-stu-id="21d26-589">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="21d26-590">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-590">**Why**</span></span>

<span data-ttu-id="21d26-591">Das Binärformat der GUIDs ist nicht standardisiert.</span><span class="sxs-lookup"><span data-stu-id="21d26-591">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="21d26-592">Das Speichern der Werte als TEXT führt dazu, dass die Datenbank mit anderen Technologien eher kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="21d26-592">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="21d26-593">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-593">**Mitigations**</span></span>

<span data-ttu-id="21d26-594">Sie können vorhandene Datenbanken zum neuen Format migrieren, indem Sie SQL-Code wie den folgenden ausführen:</span><span class="sxs-lookup"><span data-stu-id="21d26-594">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="21d26-595">Sie können in EF Core auch weiterhin das alte Verhalten verwenden, indem Sie einen Wertkonverter für diese Eigenschaften konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="21d26-595">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="21d26-596">Microsoft.Data.Sqlite kann weiterhin GUID-Werte aus BLOB- und TEXT-Spalten lesen. Da sich jedoch das Standardformat für Parameter und Konstanten geändert hat, müssen Sie wahrscheinlich Aktionen für die meisten GUID-Szenarios vornehmen.</span><span class="sxs-lookup"><span data-stu-id="21d26-596">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="21d26-597">Char-Werte werden jetzt als TEXT in SQLite gespeichert</span><span class="sxs-lookup"><span data-stu-id="21d26-597">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="21d26-598">Issue #15020</span><span class="sxs-lookup"><span data-stu-id="21d26-598">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="21d26-599">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-599">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="21d26-600">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-600">**Old behavior**</span></span>

<span data-ttu-id="21d26-601">Char-Werte wurden in der Vergangenheit in SQLite als INTEGER-Werte gespeichert.</span><span class="sxs-lookup"><span data-stu-id="21d26-601">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="21d26-602">Der Char-Wert *A* wurde z.B. als INTEGER-Wert 65 gespeichert.</span><span class="sxs-lookup"><span data-stu-id="21d26-602">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="21d26-603">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-603">**New behavior**</span></span>

<span data-ttu-id="21d26-604">Char-Werte werden jetzt als TEXT gespeichert.</span><span class="sxs-lookup"><span data-stu-id="21d26-604">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="21d26-605">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-605">**Why**</span></span>

<span data-ttu-id="21d26-606">Das Speichern der Werte als TEXT ist naheliegender und führt dazu, dass die Datenbank mit anderen Technologien eher kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="21d26-606">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="21d26-607">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-607">**Mitigations**</span></span>

<span data-ttu-id="21d26-608">Sie können vorhandene Datenbanken zum neuen Format migrieren, indem Sie SQL-Code wie den folgenden ausführen:</span><span class="sxs-lookup"><span data-stu-id="21d26-608">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="21d26-609">Sie können in EF Core auch weiterhin das alte Verhalten verwenden, indem Sie einen Wertkonverter für diese Eigenschaften konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="21d26-609">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="21d26-610">Microsoft.Data.Sqlite kann auch weiterhin Zeichenwerte aus INTEGER- und TEXT-Spalten lesen, weshalb es sein kann, dass in einigen Szenarios keine weiteren Maßnahmen erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="21d26-610">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="21d26-611">Migrations-IDs werden jetzt mit dem Kalender der invarianten Kultur generiert</span><span class="sxs-lookup"><span data-stu-id="21d26-611">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="21d26-612">Issue #12978</span><span class="sxs-lookup"><span data-stu-id="21d26-612">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="21d26-613">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-613">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="21d26-614">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-614">**Old behavior**</span></span>

<span data-ttu-id="21d26-615">Migrations-IDs wurden versehentlich mit dem Kalender der aktuellen Kultur generiert.</span><span class="sxs-lookup"><span data-stu-id="21d26-615">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="21d26-616">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-616">**New behavior**</span></span>

<span data-ttu-id="21d26-617">Migrations-IDs werden jetzt immer mit dem Kalender der invarianten Kultur generiert (gregorianischer Kalender).</span><span class="sxs-lookup"><span data-stu-id="21d26-617">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="21d26-618">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-618">**Why**</span></span>

<span data-ttu-id="21d26-619">Die Reihenfolge der Migrationen ist beim Aktualisieren einer Datenbank oder Auflösen von Mergekonflikten wesentlich.</span><span class="sxs-lookup"><span data-stu-id="21d26-619">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="21d26-620">Wenn der invariante Kalender verwendet wird, werden Probleme bei der Reihenfolge vermieden, die entstehen, wenn Teammitglieder unterschiedliche Systemkalender verwenden.</span><span class="sxs-lookup"><span data-stu-id="21d26-620">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="21d26-621">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-621">**Mitigations**</span></span>

<span data-ttu-id="21d26-622">Diese Änderung betrifft jeden Benutzer, der einen Kalender verwendet, der nicht gregorianisch ist und bei dem die Jahreszahl höher als die des gregorianischen Kalenders ist (wie z.B. in der buddhistischen Zeitrechnung).</span><span class="sxs-lookup"><span data-stu-id="21d26-622">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="21d26-623">Vorhandene Migrations-IDs müssen aktualisiert werden, damit neue Migrationen in der Reihenfolge hinter vorhandenen Migrationen eingeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="21d26-623">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="21d26-624">Sie können die Migrations-ID im Migration-Attribut in der Designerdatei der Migration finden.</span><span class="sxs-lookup"><span data-stu-id="21d26-624">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="21d26-625">Die Tabelle mit dem Migrationsverlauf muss auch aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="21d26-625">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="userownumberforpaging-has-been-removed"></a><span data-ttu-id="21d26-626">UseRowNumberForPaging wurde entfernt.</span><span class="sxs-lookup"><span data-stu-id="21d26-626">UseRowNumberForPaging has been removed</span></span>

[<span data-ttu-id="21d26-627">Issue #16400</span><span class="sxs-lookup"><span data-stu-id="21d26-627">Tracking Issue #16400</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16400)

<span data-ttu-id="21d26-628">Diese Änderung wird in Vorschauversion 6 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-628">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="21d26-629">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-629">**Old behavior**</span></span>

<span data-ttu-id="21d26-630">Vor EF Core 3.0 konnte `UseRowNumberForPaging` zum Generieren von SQL-Code für die Paginierung verwendet werden, der mit SQL Server 2008 kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="21d26-630">Before EF Core 3.0, `UseRowNumberForPaging` could be used to generate SQL for paging that is compatible with SQL Server 2008.</span></span>

<span data-ttu-id="21d26-631">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-631">**New behavior**</span></span>

<span data-ttu-id="21d26-632">Ab EF Core 3.0 generiert EF nur noch SQL-Code für die Paginierung, die mit höheren SQL Server-Versionen kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="21d26-632">Starting with EF Core 3.0, EF will only generate SQL for paging that is only compatible with later SQL Server versions.</span></span> 

<span data-ttu-id="21d26-633">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-633">**Why**</span></span>

<span data-ttu-id="21d26-634">Wir führen diese Änderung ein, weil [SQL Server 2008 kein unterstütztes Produkt mehr ist](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) und weil die Aktualisierung dieses Features auf die in EF Core 3.0 vorgenommenen Änderungen an Abfragen ein wichtiger Schritt ist.</span><span class="sxs-lookup"><span data-stu-id="21d26-634">We are making this change because [SQL Server 2008 is no longer a supported product](https://blogs.msdn.microsoft.com/sqlreleaseservices/end-of-mainstream-support-for-sql-server-2008-and-sql-server-2008-r2/) and updating this feature to work with the query changes made in EF Core 3.0 is significant work.</span></span>

<span data-ttu-id="21d26-635">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-635">**Mitigations**</span></span>

<span data-ttu-id="21d26-636">Es empfiehlt sich, eine Update auf eine neuere Version von SQL Server durchzuführen oder einen höheren Kompatibilitätsgrad zu verwenden, damit der generierte SQL-Code unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="21d26-636">We recommend updating to a newer version of SQL Server, or using a higher compatibility level, so that the generated SQL is supported.</span></span> <span data-ttu-id="21d26-637">Wenn dies bei Ihnen nicht möglich ist, fügen Sie einen [Kommentar zum Issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) ein, und geben Sie alle relevanten Informationen an.</span><span class="sxs-lookup"><span data-stu-id="21d26-637">That being said, if you are unable to do this, then please [comment on the tracking issue](https://github.com/aspnet/EntityFrameworkCore/issues/16400) with details.</span></span> <span data-ttu-id="21d26-638">Je nach Feedback der Benutzer wird diese Entscheidung möglicherweise noch einmal überprüft.</span><span class="sxs-lookup"><span data-stu-id="21d26-638">We may revisit this decision based on feedback.</span></span>

## <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="21d26-639">Erweiterungsinformationen und Metadaten aus IDbContextOptionsExtension entfernt</span><span class="sxs-lookup"><span data-stu-id="21d26-639">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="21d26-640">Issue #16119</span><span class="sxs-lookup"><span data-stu-id="21d26-640">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="21d26-641">Diese Änderung wird in Vorschauversion 7 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-641">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="21d26-642">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-642">**Old behavior**</span></span>

<span data-ttu-id="21d26-643">`IDbContextOptionsExtension` enthielt Methoden zum Bereitstellen von Metadaten zur Erweiterung.</span><span class="sxs-lookup"><span data-stu-id="21d26-643">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="21d26-644">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-644">**New behavior**</span></span>

<span data-ttu-id="21d26-645">Diese Methoden wurden in eine neue abstrakte `DbContextOptionsExtensionInfo`-Basisklasse verschoben, die von einer neuen `IDbContextOptionsExtension.Info`-Eigenschaft zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="21d26-645">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="21d26-646">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-646">**Why**</span></span>

<span data-ttu-id="21d26-647">In den Releases zwischen 2.0 und 3.0 mussten wir diese Methoden mehrmals ergänzen oder ändern.</span><span class="sxs-lookup"><span data-stu-id="21d26-647">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="21d26-648">Durch die Bereitstellung dieser Methoden in einer neuen abstrakten Basisklasse lassen sich solche Änderungen einfacher einführen, ohne dass es zu Konflikten mit vorhandenen Erweiterungen kommt.</span><span class="sxs-lookup"><span data-stu-id="21d26-648">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="21d26-649">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-649">**Mitigations**</span></span>

<span data-ttu-id="21d26-650">Erweiterungen wurden aktualisiert und verwenden das neue Muster.</span><span class="sxs-lookup"><span data-stu-id="21d26-650">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="21d26-651">Beispiele finden sich in den vielen Implementierungen von `IDbContextOptionsExtension` für verschiedene Arten von Erweiterungen im EF Core-Quellcode.</span><span class="sxs-lookup"><span data-stu-id="21d26-651">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="21d26-652">LogQueryPossibleExceptionWithAggregateOperator wurde umbenannt</span><span class="sxs-lookup"><span data-stu-id="21d26-652">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="21d26-653">Issue #10985</span><span class="sxs-lookup"><span data-stu-id="21d26-653">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="21d26-654">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-654">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="21d26-655">**Änderung**</span><span class="sxs-lookup"><span data-stu-id="21d26-655">**Change**</span></span>

<span data-ttu-id="21d26-656">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` wurde umbenannt in `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="21d26-656">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="21d26-657">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-657">**Why**</span></span>

<span data-ttu-id="21d26-658">Die Benennung dieses Warnereignisses wurde an allen anderen Warnereignissen ausgerichtet.</span><span class="sxs-lookup"><span data-stu-id="21d26-658">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="21d26-659">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-659">**Mitigations**</span></span>

<span data-ttu-id="21d26-660">Verwenden Sie den neuen Namen.</span><span class="sxs-lookup"><span data-stu-id="21d26-660">Use the new name.</span></span> <span data-ttu-id="21d26-661">(Beachten Sie, dass sich die Ereignis-ID-Nummer nicht geändert hat.)</span><span class="sxs-lookup"><span data-stu-id="21d26-661">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="21d26-662">Verdeutlichen der API für Einschränkungsnamen von Fremdschlüsseln</span><span class="sxs-lookup"><span data-stu-id="21d26-662">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="21d26-663">Issue #10730</span><span class="sxs-lookup"><span data-stu-id="21d26-663">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="21d26-664">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-664">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="21d26-665">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-665">**Old behavior**</span></span>

<span data-ttu-id="21d26-666">Vor EF Core 3.0 wurden Einschränkungsnamen von Fremdschlüsseln einfach als „Namen“ bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="21d26-666">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="21d26-667">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="21d26-667">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="21d26-668">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-668">**New behavior**</span></span>

<span data-ttu-id="21d26-669">Ab EF Core 3.0 werden Einschränkungsnamen von Fremdschlüsseln nun als „Einschränkungsnamen“ bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="21d26-669">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="21d26-670">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="21d26-670">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="21d26-671">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-671">**Why**</span></span>

<span data-ttu-id="21d26-672">Durch diese Änderung wird Konsistenz für Benennungen in diesem Bereich gewährleistet, und es wird verdeutlicht, dass es sich dabei um den Namen der Fremdschlüsseleinschränkung handelt und nicht um den Spalten- oder Eigenschaftennamen, auf deren Grundlage der Fremdschlüssel definiert ist.</span><span class="sxs-lookup"><span data-stu-id="21d26-672">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="21d26-673">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-673">**Mitigations**</span></span>

<span data-ttu-id="21d26-674">Verwenden Sie den neuen Namen.</span><span class="sxs-lookup"><span data-stu-id="21d26-674">Use the new name.</span></span>

## <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="21d26-675">„IRelationalDatabaseCreator.HasTables/HasTablesAsync“ ist jetzt öffentlich</span><span class="sxs-lookup"><span data-stu-id="21d26-675">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="21d26-676">Issue #15997</span><span class="sxs-lookup"><span data-stu-id="21d26-676">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="21d26-677">Diese Änderung wird in Vorschauversion 7 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-677">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="21d26-678">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-678">**Old behavior**</span></span>

<span data-ttu-id="21d26-679">Vor EF Core 3.0 waren diese Methoden geschützt.</span><span class="sxs-lookup"><span data-stu-id="21d26-679">Before EF Core 3.0, these methods were protected.</span></span>

<span data-ttu-id="21d26-680">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-680">**New behavior**</span></span>

<span data-ttu-id="21d26-681">Ab EF Core 3.0 sind diese Methoden öffentlich.</span><span class="sxs-lookup"><span data-stu-id="21d26-681">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="21d26-682">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-682">**Why**</span></span>

<span data-ttu-id="21d26-683">Diese Methoden werden von EF verwendet, um zu ermitteln, ob eine Datenbank erstellt wurde, aber leer ist.</span><span class="sxs-lookup"><span data-stu-id="21d26-683">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="21d26-684">Dies kann auch außerhalb von EF nützlich sein, um zu ermitteln, ob Migrationen angewendet werden sollen oder nicht.</span><span class="sxs-lookup"><span data-stu-id="21d26-684">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="21d26-685">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-685">**Mitigations**</span></span>

<span data-ttu-id="21d26-686">Der Zugriff auf Überschreibungen wurde geändert.</span><span class="sxs-lookup"><span data-stu-id="21d26-686">Change the accessibility of any overrides.</span></span>

## <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="21d26-687">Microsoft.EntityFrameworkCore.Design ist jetzt ein DevelopmentDependency-Paket</span><span class="sxs-lookup"><span data-stu-id="21d26-687">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="21d26-688">Issue #11506</span><span class="sxs-lookup"><span data-stu-id="21d26-688">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="21d26-689">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-689">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="21d26-690">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-690">**Old behavior**</span></span>

<span data-ttu-id="21d26-691">Vor EF Core 3.0 war Microsoft.EntityFrameworkCore.Design ein reguläres NuGet-Paket, auf dessen Assembly von Projekten verwiesen werden kann, die davon abhängig sind.</span><span class="sxs-lookup"><span data-stu-id="21d26-691">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="21d26-692">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-692">**New behavior**</span></span>

<span data-ttu-id="21d26-693">Ab EF Core 3.0 ist es ein DevelopmentDependency-Paket.</span><span class="sxs-lookup"><span data-stu-id="21d26-693">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="21d26-694">Das bedeutet, dass die Abhängigkeit nicht transitiv auf andere Projekte übertragen wird und Sie nicht mehr standardmäßig auf die Assembly verweisen können.</span><span class="sxs-lookup"><span data-stu-id="21d26-694">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="21d26-695">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-695">**Why**</span></span>

<span data-ttu-id="21d26-696">Dieses Paket ist nur für die Verwendung zur Entwurfszeit konzipiert.</span><span class="sxs-lookup"><span data-stu-id="21d26-696">This package is only intended to be used at design time.</span></span> <span data-ttu-id="21d26-697">Bereitgestellte Anwendungen sollten nicht darauf verweisen.</span><span class="sxs-lookup"><span data-stu-id="21d26-697">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="21d26-698">Die Kennzeichnung des Pakets als DevelopmentDependency unterstreicht diese Empfehlung.</span><span class="sxs-lookup"><span data-stu-id="21d26-698">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="21d26-699">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-699">**Mitigations**</span></span>

<span data-ttu-id="21d26-700">Wenn Sie auf dieses Paket verweisen müssen, um das Verhalten von EF Core zur Entwurfszeit zu überschreiben, können Sie die Metadaten des PackageReference-Elements in Ihrem Projekt aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="21d26-700">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="21d26-701">Wenn transitiv über via Microsoft.EntityFrameworkCore.Tools auf das Paket verwiesen wird, müssen Sie dem Paket eine explizite PackageReference hinzufügen, um seine Metadaten zu ändern.</span><span class="sxs-lookup"><span data-stu-id="21d26-701">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

## <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="21d26-702">SQLitePCL.raw zu Version 2.0.0 aktualisiert</span><span class="sxs-lookup"><span data-stu-id="21d26-702">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="21d26-703">Issue #14824</span><span class="sxs-lookup"><span data-stu-id="21d26-703">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="21d26-704">Diese Änderung wird in Vorschauversion 7 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-704">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="21d26-705">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-705">**Old behavior**</span></span>

<span data-ttu-id="21d26-706">Microsoft.EntityFrameworkCore.Sqlite war früher von Version 1.1.12 von SQLitePCL.raw abhängig.</span><span class="sxs-lookup"><span data-stu-id="21d26-706">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="21d26-707">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-707">**New behavior**</span></span>

<span data-ttu-id="21d26-708">Wir haben unser Paket aktualisiert, so dass es jetzt von Version 2.0.0 abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="21d26-708">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="21d26-709">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-709">**Why**</span></span>

<span data-ttu-id="21d26-710">Version 2.0.0 von SQLitePCL.raw zielt auf .NET Standard 2.0 ab.</span><span class="sxs-lookup"><span data-stu-id="21d26-710">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="21d26-711">Zuvor war .NET Standard 1.1 das Ziel, hierfür war für ein umfassender Funktionsabschluss von transitiven Paketen erforderlich.</span><span class="sxs-lookup"><span data-stu-id="21d26-711">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="21d26-712">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-712">**Mitigations**</span></span>

<span data-ttu-id="21d26-713">SQLitePCL.raw, Version 2.0.0, enthält einige Breaking Changes.</span><span class="sxs-lookup"><span data-stu-id="21d26-713">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="21d26-714">Weitere Informationen finden Sie in den [Versionshinweisen](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md).</span><span class="sxs-lookup"><span data-stu-id="21d26-714">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>


## <a name="nettopologysuite-updated-to-version-200"></a><span data-ttu-id="21d26-715">NetTopologySuite auf Version 2.0.0 aktualisiert</span><span class="sxs-lookup"><span data-stu-id="21d26-715">NetTopologySuite updated to version 2.0.0</span></span>

[<span data-ttu-id="21d26-716">Issue #14825</span><span class="sxs-lookup"><span data-stu-id="21d26-716">Tracking Issue #14825</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14825)

<span data-ttu-id="21d26-717">Diese Änderung wird in Vorschauversion 7 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="21d26-717">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="21d26-718">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-718">**Old behavior**</span></span>

<span data-ttu-id="21d26-719">Die Pakete mit räumlichen Daten waren bisher von Version 1.15.1 der NetTopologySuite abhängig.</span><span class="sxs-lookup"><span data-stu-id="21d26-719">The spatial packages previously depended on version 1.15.1 of NetTopologySuite.</span></span>

<span data-ttu-id="21d26-720">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="21d26-720">**New behavior**</span></span>

<span data-ttu-id="21d26-721">Wir haben unser Paket aktualisiert, so dass es jetzt von Version 2.0.0 abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="21d26-721">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="21d26-722">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="21d26-722">**Why**</span></span>

<span data-ttu-id="21d26-723">Version 2.0.0 der NetTopologySuite behebt verschiedene Verwendungsprobleme, die von EF Core-Benutzern gemeldet wurden.</span><span class="sxs-lookup"><span data-stu-id="21d26-723">Version 2.0.0 of NetTopologySuite aims to address several usability issues encountered by EF Core users.</span></span>

<span data-ttu-id="21d26-724">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="21d26-724">**Mitigations**</span></span>

<span data-ttu-id="21d26-725">NetTopologySuite 2.0.0 umfasst einige Breaking Changes.</span><span class="sxs-lookup"><span data-stu-id="21d26-725">NetTopologySuite version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="21d26-726">Weitere Informationen finden Sie in den [Versionshinweisen](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001).</span><span class="sxs-lookup"><span data-stu-id="21d26-726">See the [release notes](https://www.nuget.org/packages/NetTopologySuite/2.0.0-pre001) for details.</span></span>
