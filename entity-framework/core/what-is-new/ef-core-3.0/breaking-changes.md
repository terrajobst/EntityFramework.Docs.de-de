---
title: Breaking Changes in EF Core 3.0
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: faae0153e0f2bdd42d3b316582dfcab88d9ceb5b
ms.sourcegitcommit: ea1cdec0b982b922a59b9d9301d3ed2b94baca0f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452300"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="cd42b-102">Breaking Changes in EF Core 3.0 (aktuell in der Vorschauversion)</span><span class="sxs-lookup"><span data-stu-id="cd42b-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cd42b-103">Bitte beachten Sie, dass die Featuregruppen und Zeitpläne für künftige Releases jederzeit geändert werden können. Obwohl diese Seite bestmöglich aktualisiert wird, entspricht sie nicht immer den neuesten Plänen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="cd42b-104">Die folgenden API-Änderungen und Behavior Changes können dazu führen, dass Anwendungen, die für EF Core 2.2.x entwickelt wurden, beim Upgrade auf 3.0.0 nicht mehr funktionieren.</span><span class="sxs-lookup"><span data-stu-id="cd42b-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="cd42b-105">Änderungen, die sich voraussichtlich nur auf Datenbankanbieter auswirken, sind unter [Änderungen mit Auswirkungen auf den Anbieter](../../providers/provider-log.md) dokumentiert.</span><span class="sxs-lookup"><span data-stu-id="cd42b-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="cd42b-106">Werden Breaking Changes zwischen zwei Vorschauversionen von EF Core 3.0 in neuen Features eingeführt, werden diese hier nicht dokumentiert.</span><span class="sxs-lookup"><span data-stu-id="cd42b-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="cd42b-107">LINQ-Abfragen werden nicht mehr auf dem Client ausgewertet</span><span class="sxs-lookup"><span data-stu-id="cd42b-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="cd42b-108">[Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Siehe auch Issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="cd42b-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="cd42b-109">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-109">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd42b-110">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-110">**Old behavior**</span></span>

<span data-ttu-id="cd42b-111">Vor Version 3.0 wurden in EF Core automatisch Ausdrücke auf dem Client ausgewertet, wenn diese Teil einer Abfrage waren und nicht in SQL oder in einen Parameter konvertiert werden konnten.</span><span class="sxs-lookup"><span data-stu-id="cd42b-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="cd42b-112">Wenn potenziell aufwendige Ausdrücke auf dem Client ausgewertet werden sollten, wurde standardmäßig nur eine Warnung ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="cd42b-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="cd42b-113">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-113">**New behavior**</span></span>

<span data-ttu-id="cd42b-114">Ab Version 3.0 können in EF Core nur Ausdrücke der obersten Projektion (dem letzten `Select()`-Aufruf in der Abfrage) auf dem Client ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="cd42b-115">Wenn Ausdrücke in einem anderen Teil der Abfrage nicht in SQL oder in einen Parameter konvertiert werden können, wird eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="cd42b-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="cd42b-116">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-116">**Why**</span></span>

<span data-ttu-id="cd42b-117">Die automatische Auswertung von Abfragen auf Clients hat zur Folge, dass viele Abfragen auch dann ausgeführt werden, wenn wichtige Teile nicht übersetzt werden können.</span><span class="sxs-lookup"><span data-stu-id="cd42b-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="cd42b-118">Dieses Verhalten kann zu unerwartetem und potenziell schädlichem Verhalten führen, das möglicherweise erst in der Produktionsphase erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="cd42b-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="cd42b-119">Eine nicht übersetzbare Bedingung in einem `Where()`-Aufruf kann sich beispielsweise so auswirken, dass alle Zeilen einer Tabelle vom Datenbankserver übertragen werden und der Filter auf dem Client angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="cd42b-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="cd42b-120">Wenn die Tabelle in der Entwicklungsphase nur wenige Zeilen enthält, ist es gut möglich, dass diese Situation unerkannt bleibt. Wenn die Anwendung jedoch in die Produktionsumgebung überführt wird, enthält die Tabelle möglicherweise Millionen von Zeilen, was zu schwerwiegenden Konsequenzen führen kann.</span><span class="sxs-lookup"><span data-stu-id="cd42b-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="cd42b-121">Wie die Erfahrung in der Entwicklungsphase gezeigt hat, können auch Warnungen, die bei Auswertungen auf dem Client ausgelöst werden, dieses Problem nicht lösen, da sie sich leicht ignorieren lassen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="cd42b-122">Die automatische Auswertung auf Clients kann darüber hinaus zu Problemen führen, bei denen die Verbesserung der Abfrageübersetzung für bestimmte Ausdrücke zu unbeabsichtigten Breaking Changes zwischen Releases führt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="cd42b-123">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-123">**Mitigations**</span></span>

<span data-ttu-id="cd42b-124">Wenn sich eine Abfrage nicht vollständig übersetzen lässt, habe Sie zwei Möglichkeiten: Schreiben Sie sie entweder um, oder setzen Sie alternativ `AsEnumerable()`, `ToList()` oder ähnliche Methoden ein, um Daten wieder zurück an den Client zu übertragen, wo sie mit LINQ to Objects weiterverarbeitet werden können.</span><span class="sxs-lookup"><span data-stu-id="cd42b-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="cd42b-125">Entity Framework Core ist nicht mehr Teil des gemeinsam verwendeten ASP.NET Core-Frameworks</span><span class="sxs-lookup"><span data-stu-id="cd42b-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="cd42b-126">Issueankündigungen #325</span><span class="sxs-lookup"><span data-stu-id="cd42b-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="cd42b-127">Diese Änderung wurde in Vorschauversion 1 von ASP.NET Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-127">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="cd42b-128">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-128">**Old behavior**</span></span>

<span data-ttu-id="cd42b-129">Vor Version 3.0 von ASP.NET Core wurden EF Core und einige der zugehörigen Datenanbieter wie SQL Server eingeschlossen, wenn `Microsoft.AspNetCore.App` oder `Microsoft.AspNetCore.All` ein Paketverweis hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="cd42b-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="cd42b-130">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-130">**New behavior**</span></span>

<span data-ttu-id="cd42b-131">Ab Version 3.0 enthält das gemeinsam verwendete ASP.NET Core-Framework weder EF Core noch zugehörige Datenanbieter.</span><span class="sxs-lookup"><span data-stu-id="cd42b-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="cd42b-132">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-132">**Why**</span></span>

<span data-ttu-id="cd42b-133">Vor dieser Änderung konnte EF Core auf unterschiedliche Arten bezogen werden. Diese richteten sich danach, ob ASP.NET Core und SQL Server oder ein anderes Zielframework für die Anwendung vorgesehen war.</span><span class="sxs-lookup"><span data-stu-id="cd42b-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="cd42b-134">Bei einem Upgrade von ASP.NET Core wurde zudem ein Upgrade von EF Core und des SQL Server-Anbieters erzwungen, was nicht in allen Fällen gewünscht war.</span><span class="sxs-lookup"><span data-stu-id="cd42b-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="cd42b-135">Durch die eingeführte Änderung wird EF Core unter allen Anbietern, unterstützten .NET-Implementierungen und Anwendungstypen auf dieselbe Weise bezogen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="cd42b-136">Entwickler können nun außerdem genau festlegen, wann für EF Core und zugehörige Datenanbieter ein Upgrade durchgeführt werden soll.</span><span class="sxs-lookup"><span data-stu-id="cd42b-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="cd42b-137">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-137">**Mitigations**</span></span>

<span data-ttu-id="cd42b-138">Wenn Sie EF Core in einer Anwendung unter ASP.NET Core 3.0 oder in einer anderen unterstützten Anwendung verwenden möchten, müssen Sie dem EF Core-Datenbankanbieter, der für die Anwendung genutzt werden soll, explizit einen Paketverweis hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="cd42b-139">Das EF Core-Befehlszeilentool (dotnet ef) ist nicht länger Bestandteil des .NET Core SDK.</span><span class="sxs-lookup"><span data-stu-id="cd42b-139">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="cd42b-140">Issue #14016</span><span class="sxs-lookup"><span data-stu-id="cd42b-140">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="cd42b-141">Diese Änderung wurde in EF Core 3.0-preview 4 und der zugehörigen Version des .NET Core SDK eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-141">This change was introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="cd42b-142">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-142">**Old behavior**</span></span>

<span data-ttu-id="cd42b-143">Vor Version 3.0 war das `dotnet ef`-Tool im .NET Core SDK enthalten und konnte von der Befehlszeile aus ohne zusätzliche Schritte in einem beliebigen Projekt verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-143">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="cd42b-144">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-144">**New behavior**</span></span>

<span data-ttu-id="cd42b-145">Ab Version 3.0 ist das `dotnet ef`-Tool nicht mehr im .NET SDK enthalten und muss deshalb vor der Verwendung explizit als lokales oder globales Tool installiert werden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-145">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="cd42b-146">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-146">**Why**</span></span>

<span data-ttu-id="cd42b-147">Durch diese Änderung kann `dotnet ef` als reguläres .NET-CLI-Tool für NuGet verteilt und aktualisiert werden und entspricht damit dem üblichen Verfahren, dass EF Core 3.0 ebenfalls immer als NuGet-Paket verteilt wird.</span><span class="sxs-lookup"><span data-stu-id="cd42b-147">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="cd42b-148">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-148">**Mitigations**</span></span>

<span data-ttu-id="cd42b-149">Um Migrationen zu verwalten oder ein Gerüst für `DbContext` zu erstellen, installieren Sie `dotnet-ef` mithilfe des Befehls `dotnet tool install`.</span><span class="sxs-lookup"><span data-stu-id="cd42b-149">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` using the `dotnet tool install` command.</span></span>
<span data-ttu-id="cd42b-150">Um beispielsweise eine Installation als globales Tool durchzuführen, können Sie diesen Befehl verwenden:</span><span class="sxs-lookup"><span data-stu-id="cd42b-150">For example, to install it as a global tool, you can type this command:</span></span>

  ``` console
  $ dotnet tool install --global dotnet-ef --version <exact-version>
  ```

<span data-ttu-id="cd42b-151">Eine Verwendung als lokales Tool ist ebenfalls möglich, wenn Sie die Abhängigkeiten eines Projekts wiederherstellen, das das Tool mithilfe einer [Toolmanifestdatei](https://github.com/dotnet/cli/issues/10288) als Toolabhängigkeit deklariert.</span><span class="sxs-lookup"><span data-stu-id="cd42b-151">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="cd42b-152">FromSql, ExecuteSql und ExecuteSqlAsync wurden umbenannt</span><span class="sxs-lookup"><span data-stu-id="cd42b-152">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="cd42b-153">Issue #10996</span><span class="sxs-lookup"><span data-stu-id="cd42b-153">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="cd42b-154">Diese Änderung wurde mit EF Core 3.0 Preview 4 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-154">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd42b-155">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-155">**Old behavior**</span></span>

<span data-ttu-id="cd42b-156">Vor Version 3.0 wurden in EF Core diese Methodennamen überladen, um entweder mit einer normalen Zeichenfolge oder einer Zeichenfolge, die in SQL und Parameter interpoliert werden sollte, zu arbeiten.</span><span class="sxs-lookup"><span data-stu-id="cd42b-156">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="cd42b-157">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-157">**New behavior**</span></span>

<span data-ttu-id="cd42b-158">Ab Version 3.0 verwenden Sie in EF Core `FromSqlRaw`, `ExecuteSqlRaw` und `ExecuteSqlRawAsync`, um eine parametrisierte Abfrage zu erstellen, bei der die Parameter einzeln aus der Abfragezeichenfolge übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-158">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="cd42b-159">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cd42b-159">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="cd42b-160">Verwenden Sie `FromSqlInterpolated`, `ExecuteSqlInterpolated` und `ExecuteSqlInterpolatedAsync`, um eine parametrisierte Abfrage zu erstellen, bei der die Parameter als Teil einer interpolierten Abfragezeichenfolge übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-160">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="cd42b-161">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cd42b-161">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="cd42b-162">Beachten Sie, dass die beiden oben genannten Abfragen dieselbe parametrisierte SQL mit denselben SQL-Parametern erzeugen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-162">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="cd42b-163">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-163">**Why**</span></span>

<span data-ttu-id="cd42b-164">Bei Methodenüberladungen wie dieser wird sehr leicht versehentlich die Methode für unformatierte Zeichenfolgen aufgerufen, wenn eigentlich beabsichtigt war, die Methode für interpolierte Zeichenfolgen aufzurufen (und umgekehrt).</span><span class="sxs-lookup"><span data-stu-id="cd42b-164">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="cd42b-165">Dadurch werden Abfragen möglicherweise nicht parametrisiert, obwohl dies der Fall sein sollte.</span><span class="sxs-lookup"><span data-stu-id="cd42b-165">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="cd42b-166">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-166">**Mitigations**</span></span>

<span data-ttu-id="cd42b-167">Verwenden Sie ab sofort die neuen Methodennamen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-167">Switch to use the new method names.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="cd42b-168">Die Ausführung der Abfrage wird auf der Debugebene protokolliert</span><span class="sxs-lookup"><span data-stu-id="cd42b-168">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="cd42b-169">Issue #14523</span><span class="sxs-lookup"><span data-stu-id="cd42b-169">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="cd42b-170">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-170">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="cd42b-171">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-171">**Old behavior**</span></span>

<span data-ttu-id="cd42b-172">Vor Version 3.0 wurde in EF Core die Ausführung von Abfragen und anderen Befehlen auf der `Info`-Ebene protokolliert.</span><span class="sxs-lookup"><span data-stu-id="cd42b-172">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

<span data-ttu-id="cd42b-173">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-173">**New behavior**</span></span>

<span data-ttu-id="cd42b-174">Ab Version 3.0 wird in EF Core die Ausführung von Befehlen und SQL-Abfragen auf der `Debug`-Ebene protokolliert.</span><span class="sxs-lookup"><span data-stu-id="cd42b-174">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

<span data-ttu-id="cd42b-175">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-175">**Why**</span></span>

<span data-ttu-id="cd42b-176">Diese Änderung wurde vorgenommen, damit auf der `Info`-Ebene nur die wichtigsten Daten erfasst werden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-176">This change was made to reduce the noise at the `Info` log level.</span></span>

<span data-ttu-id="cd42b-177">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-177">**Mitigations**</span></span>

<span data-ttu-id="cd42b-178">Dieses Protokollereignis wird durch `RelationalEventId.CommandExecuting` mit der Ereignis-ID 20100 definiert.</span><span class="sxs-lookup"><span data-stu-id="cd42b-178">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="cd42b-179">Um SQL erneut auf der Ebene `Info` zu protokollieren, konfigurieren Sie die Ebene explizit in `OnConfiguring` oder `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="cd42b-179">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="cd42b-180">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cd42b-180">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="cd42b-181">Temporäre Schlüsselwerte werden nicht mehr Entitätsinstanzen zugewiesen</span><span class="sxs-lookup"><span data-stu-id="cd42b-181">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="cd42b-182">Issue #12378</span><span class="sxs-lookup"><span data-stu-id="cd42b-182">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="cd42b-183">Diese Änderung wurde in Vorschauversion 2 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-183">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="cd42b-184">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-184">**Old behavior**</span></span>

<span data-ttu-id="cd42b-185">Vor Version 3.0 wurden in EF Core allen Schlüsseleigenschaften temporäre Werte zugewiesen. Später wurden den Eigenschaften dann die tatsächlichen Werte zugewiesen, die von der Datenbank generiert wurden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-185">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="cd42b-186">Die temporären Werte waren in der Regel große negative Zahlen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-186">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="cd42b-187">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-187">**New behavior**</span></span>

<span data-ttu-id="cd42b-188">Ab Version 3.0 wird in EF Core der temporäre Wert als Teil der Überwachungsinformationen der Entität gespeichert. Die Schlüsseleigenschaft selbst bleibt unverändert.</span><span class="sxs-lookup"><span data-stu-id="cd42b-188">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="cd42b-189">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-189">**Why**</span></span>

<span data-ttu-id="cd42b-190">Diese Änderung wurde vorgenommen, damit temporäre Schlüsselwerte nicht fälschlicherweise dauerhaft gespeichert werden, wenn eine Entität, die vorher von einer `DbContext`-Instanz überwacht wurde, in eine andere `DbContext`-Instanz verschoben wird.</span><span class="sxs-lookup"><span data-stu-id="cd42b-190">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="cd42b-191">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-191">**Mitigations**</span></span>

<span data-ttu-id="cd42b-192">Anwendungen, die Primärschlüsselwerte Fremdschlüsseln zuweisen, um Zuordnungen zwischen Entitäten zu erstellen, können vom alten Verhalten abhängig sein, wenn Primärschlüssel vom Speicher generiert werden und zu Entitäten im `Added`-Zustand gehören.</span><span class="sxs-lookup"><span data-stu-id="cd42b-192">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="cd42b-193">Sie können dies wie folgt vermeiden:</span><span class="sxs-lookup"><span data-stu-id="cd42b-193">This can be avoided by:</span></span>
* <span data-ttu-id="cd42b-194">Verwenden Sie keine vom Speicher generierten Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="cd42b-194">Not using store-generated keys.</span></span>
* <span data-ttu-id="cd42b-195">Legen Sie zum Erstellen von Zuordnungen keine Fremdschlüsselwerte, sondern Navigationseigenschaften fest.</span><span class="sxs-lookup"><span data-stu-id="cd42b-195">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="cd42b-196">Rufen Sie die tatsächlichen temporären Schlüsselwerte aus den Überwachungsinformationen der Entität ab.</span><span class="sxs-lookup"><span data-stu-id="cd42b-196">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="cd42b-197">`context.Entry(blog).Property(e => e.Id).CurrentValue` gibt beispielsweise den temporären Wert zurück, obwohl `blog.Id` noch nicht festgelegt wurde.</span><span class="sxs-lookup"><span data-stu-id="cd42b-197">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="cd42b-198">DetectChanges berücksichtigt vom Speicher generierte Schlüsselwerte</span><span class="sxs-lookup"><span data-stu-id="cd42b-198">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="cd42b-199">Issue #14616</span><span class="sxs-lookup"><span data-stu-id="cd42b-199">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="cd42b-200">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-200">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="cd42b-201">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-201">**Old behavior**</span></span>

<span data-ttu-id="cd42b-202">Vor Version 3.0 wurde in EF Core eine nicht überwachte Entität, die von `DetectChanges` gefunden wurde, im `Added`-Zustand überwacht und als neue Zeile eingefügt, sobald `SaveChanges` aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="cd42b-202">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="cd42b-203">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-203">**New behavior**</span></span>

<span data-ttu-id="cd42b-204">Ab Version 3.0 wird in EF Core die Entität im `Modified`-Zustand überwacht, wenn für diese generierte Schlüsselwerte verwendet werden und ein Schlüsselwert festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="cd42b-204">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="cd42b-205">Es wird also davon ausgegangen, dass eine Zeile für die Entität vorhanden ist. Wenn `SaveChanges` aufgerufen wird, wird die Zeile außerdem aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="cd42b-205">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="cd42b-206">Falls kein Schlüsselwert festgelegt ist oder der Entitätstyp keine generierten Schlüssel verwendet, wird die neue Entität wie in früheren Version weiterhin im `Added`-Zustand überwacht.</span><span class="sxs-lookup"><span data-stu-id="cd42b-206">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="cd42b-207">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-207">**Why**</span></span>

<span data-ttu-id="cd42b-208">Diese Änderung wurde vorgenommen, um bei der Verwendung von speichergenerierten Schlüsseln die Arbeit mit nicht verbundenen Entitätsgraphen einfacher und konsistenter zu gestalten.</span><span class="sxs-lookup"><span data-stu-id="cd42b-208">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="cd42b-209">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-209">**Mitigations**</span></span>

<span data-ttu-id="cd42b-210">Diese Änderung kann dazu führen, dass eine Anwendung nicht mehr funktioniert. Dieser Fall tritt ein, wenn für Entitätstypen generierte Schlüssel vorgesehen sind, dann jedoch explizit Schlüsselwerte für neue Instanzen festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-210">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="cd42b-211">Sie können dieses Problem vermeiden, indem Sie explizit festlegen, dass für Schlüsseleigenschaften keine generierten Werte verwendet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-211">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="cd42b-212">Dies ist beispielsweise mit der Fluent-API möglich:</span><span class="sxs-lookup"><span data-stu-id="cd42b-212">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="cd42b-213">Alternativ bieten sich auch Datenanmerkungen an:</span><span class="sxs-lookup"><span data-stu-id="cd42b-213">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="cd42b-214">Kaskadierende Deletes werden standardmäßig sofort ausgeführt</span><span class="sxs-lookup"><span data-stu-id="cd42b-214">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="cd42b-215">Issue #10114</span><span class="sxs-lookup"><span data-stu-id="cd42b-215">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="cd42b-216">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-216">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="cd42b-217">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-217">**Old behavior**</span></span>

<span data-ttu-id="cd42b-218">Vor Version 3.0 wurden in EF Core kaskadierenden Aktionen (Löschen von abhängigen Entitäten nach dem Löschen eines erforderlichen Prinzipals oder nach dem Aufheben einer Beziehung zum erforderlichen Prinzipal) erst ausgeführt, wenn SaveChanges aufgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="cd42b-218">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="cd42b-219">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-219">**New behavior**</span></span>

<span data-ttu-id="cd42b-220">Ab Version 3.0 werden in EF Core kaskadierende Aktionen angewendet, sobald die auslösende Bedingung erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="cd42b-220">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="cd42b-221">Wenn beispielsweise `context.Remove()` zum Löschen einer Prinzipalentität aufgerufen wird, führt das dazu, dass auch alle zugehörigen erforderlichen Entitäten, die überwacht werden, sofort auf `Deleted` festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-221">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="cd42b-222">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-222">**Why**</span></span>

<span data-ttu-id="cd42b-223">Diese Änderung wurde vorgenommen, um die Arbeit mit Datenbindungen und Überwachungsszenarios zu vereinfachen, in denen bekannt sein muss, welche Entitäten _vor_ dem Aufruf von `SaveChanges` gelöscht werden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-223">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="cd42b-224">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-224">**Mitigations**</span></span>

<span data-ttu-id="cd42b-225">Sie können das vorherige Verhalten wiederherstellen, indem Sie für `context.ChangedTracker` die entsprechenden Einstellungen vornehmen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-225">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="cd42b-226">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cd42b-226">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="cd42b-227">DeleteBehavior.Restrict verfügt über eine übersichtlichere Semantik</span><span class="sxs-lookup"><span data-stu-id="cd42b-227">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="cd42b-228">Issue #12661</span><span class="sxs-lookup"><span data-stu-id="cd42b-228">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="cd42b-229">Diese Änderung wird in Vorschauversion 5 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-229">This change will be introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="cd42b-230">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-230">**Old behavior**</span></span>

<span data-ttu-id="cd42b-231">Vor Version 3.0 wurden mit `DeleteBehavior.Restrict` Fremdschlüssel in der Datenbank mit `Restrict`-Semantik erstellt, es wurden aber auch unvorhergesehen interne Fixups geändert.</span><span class="sxs-lookup"><span data-stu-id="cd42b-231">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="cd42b-232">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-232">**New behavior**</span></span>

<span data-ttu-id="cd42b-233">Ab Version 3.0 wird mit `DeleteBehavior.Restrict` sichergestellt, dass Fremdschlüssel mit `Restrict`-Semantik erstellt werden – d. h. ohne Überlappungen; ausgenommen bei Einschränkungsverletzungen – und ohne Auswirkungen auf EF-interne Fixups.</span><span class="sxs-lookup"><span data-stu-id="cd42b-233">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="cd42b-234">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-234">**Why**</span></span>

<span data-ttu-id="cd42b-235">Diese Änderung wurde vorgenommen, um die Benutzerfreundlichkeit bei intuitiver Verwendung von `DeleteBehavior` ohne unerwartete Nebeneffekte zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="cd42b-235">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="cd42b-236">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-236">**Mitigations**</span></span>

<span data-ttu-id="cd42b-237">Sie können das vorherige Verhalten wiederherstellen, indem Sie `DeleteBehavior.ClientNoAction` verwenden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-237">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="cd42b-238">Abfragetypen werden mit Entitätstypen zusammengeführt</span><span class="sxs-lookup"><span data-stu-id="cd42b-238">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="cd42b-239">Issue #14194</span><span class="sxs-lookup"><span data-stu-id="cd42b-239">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="cd42b-240">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-240">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="cd42b-241">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-241">**Old behavior**</span></span>

<span data-ttu-id="cd42b-242">Vor Version 3.0 wurden in EF Core [Abfragetypen](xref:core/modeling/query-types) dazu verwendet, Daten abzufragen, in denen kein Primärschlüssel auf strukturierte Weise festgelegt war.</span><span class="sxs-lookup"><span data-stu-id="cd42b-242">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="cd42b-243">Abfragetypen wurden also für die Zuordnung von Entitätstypen ohne Schlüssel verwendet (in der Regel in einer Sicht, in einigen Fällen aber auch in einer Tabelle), während reguläre Entitätstypen für einen verfügbaren Schlüssel verwendet wurden (in der Regel in einer Tabelle, in einigen Fällen aber auch in einer Sicht).</span><span class="sxs-lookup"><span data-stu-id="cd42b-243">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="cd42b-244">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-244">**New behavior**</span></span>

<span data-ttu-id="cd42b-245">Aus einem Abfragetyp wird nun einfach ein Entitätstyp ohne Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="cd42b-245">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="cd42b-246">Entitätstypen ohne Schlüssel besitzen die gleiche Funktionalität wie Abfragetypen in früheren Versionen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-246">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="cd42b-247">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-247">**Why**</span></span>

<span data-ttu-id="cd42b-248">Diese Änderung wurde vorgenommen, um Unklarheiten bei der Verwendung von Abfragetypen zu beseitigen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-248">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="cd42b-249">Abfragetypen sind schlüssellose Entitätstypen, die grundsätzlich schreibgeschützt sind. Sie sollten allerdings nicht allein wegen des Schreibschutzes verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-249">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="cd42b-250">Des Weiteren werden sie oft Sichten zugeordnet, was aber nur daran liegt, dass für Letztere häufig keine Schlüssel definiert werden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-250">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="cd42b-251">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-251">**Mitigations**</span></span>

<span data-ttu-id="cd42b-252">Die folgenden Teile der API sind durch die Änderungen veraltet:</span><span class="sxs-lookup"><span data-stu-id="cd42b-252">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="cd42b-253">**`ModelBuilder.Query<>()`** : Rufen Sie stattdessen `ModelBuilder.Entity<>().HasNoKey()` auf, um einen schlüssellosen Entitätstyp festzulegen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-253">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="cd42b-254">Dieses Verhalten wird nach wie vor nicht konventionsgemäß festgelegt, um Fehlkonfigurationen zu vermeiden, wenn ein Primärschlüssel erwartet wird, jedoch nicht mit der Konvention übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-254">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="cd42b-255">**`DbQuery<>`** : Verwenden Sie stattdessen `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="cd42b-255">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="cd42b-256">**`DbContext.Query<>()`** : Verwenden Sie stattdessen `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="cd42b-256">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="cd42b-257">Die Konfigurations-API für Beziehungen abhängiger (owned) Typen wurde geändert</span><span class="sxs-lookup"><span data-stu-id="cd42b-257">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="cd42b-258">[Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="cd42b-258">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="cd42b-259">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-259">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="cd42b-260">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-260">**Old behavior**</span></span>

<span data-ttu-id="cd42b-261">Vor Version 3.0 wurde in EF Core die abhängige Beziehung direkt nach dem Aufruf von `OwnsOne` oder `OwnsMany` konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="cd42b-261">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="cd42b-262">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-262">**New behavior**</span></span>

<span data-ttu-id="cd42b-263">Ab Version 3.0 von EF Core ist eine Fluent-API verfügbar, mit der über `WithOwner()` eine Navigationseigenschaft für den Besitzer konfiguriert werden kann.</span><span class="sxs-lookup"><span data-stu-id="cd42b-263">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="cd42b-264">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cd42b-264">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="cd42b-265">Die Konfiguration für die Beziehung zwischen Besitzertyp und abhängigem Typ sollte nun ähnlich wie bei der Konfiguration anderer Beziehungen nach `WithOwner()` verkettet werden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-265">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="cd42b-266">Die Konfiguration für den abhängigen Typ wird jedoch weiterhin nach `OwnsOne()/OwnsMany()` verkettet.</span><span class="sxs-lookup"><span data-stu-id="cd42b-266">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="cd42b-267">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cd42b-267">For example:</span></span>

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

<span data-ttu-id="cd42b-268">Wenn Sie zusätzlich `Entity()`, `HasOne()`, oder `Set()` für das Ziel eines abhängigen Typs aufrufen, wird keine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="cd42b-268">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="cd42b-269">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-269">**Why**</span></span>

<span data-ttu-id="cd42b-270">Diese Änderung wurde vorgenommen, um eine deutlichere Trennung zwischen der Konfiguration des abhängigen Typs und der _Beziehung zum abhängigen Typ_ zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-270">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="cd42b-271">Dadurch werden für Methoden wie `HasForeignKey` Mehrdeutigkeiten und Unklarheiten beseitigt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-271">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="cd42b-272">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-272">**Mitigations**</span></span>

<span data-ttu-id="cd42b-273">Passen Sie für die Beziehungen von abhängigen Typen die Konfiguration so an, dass die neue Fluent-API wie im obigen Beispiel verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="cd42b-273">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="cd42b-274">Abhängige Entitäten, die die Tabelle gemeinsam mit dem Prinzipal verwenden, sind jetzt optional</span><span class="sxs-lookup"><span data-stu-id="cd42b-274">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="cd42b-275">Issue #9005</span><span class="sxs-lookup"><span data-stu-id="cd42b-275">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="cd42b-276">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-276">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd42b-277">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-277">**Old behavior**</span></span>

<span data-ttu-id="cd42b-278">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="cd42b-278">Consider the following model:</span></span>
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
<span data-ttu-id="cd42b-279">Wenn `OrderDetails` im Besitz von `Order` ist oder explizit derselben Tabelle zugeordnet ist, war vor Version 3.0 in EF Core immer eine `OrderDetails`-Instanz beim Hinzufügen eines neuen `Order`-Objekts erforderlich.</span><span class="sxs-lookup"><span data-stu-id="cd42b-279">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="cd42b-280">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-280">**New behavior**</span></span>

<span data-ttu-id="cd42b-281">Ab Version 3.0 bietet EF Core die Möglichkeit, `Order` ohne `OrderDetails` hinzuzufügen, und alle `OrderDetails`-Eigenschaften, mit Ausnahme des primären Schlüssels, werden Spalten zugeordnet, die NULL-Werte zulassen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-281">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="cd42b-282">Bei Abfragen von EF Core wird `OrderDetails` auf `null` festgelegt, wenn eine der erforderlichen Eigenschaften keinen Wert aufweist oder keine erforderlichen Eigenschaften außer dem primären Schlüssel vorhanden und alle Eigenschaften `null` sind.</span><span class="sxs-lookup"><span data-stu-id="cd42b-282">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="cd42b-283">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-283">**Mitigations**</span></span>

<span data-ttu-id="cd42b-284">Wenn Ihr Modell von der gemeinsamen Nutzung einer Tabelle mit allen optionalen Spalten abhängig ist, aber die Navigation, die darauf zeigt, wahrscheinlich nicht `null` ist, sollte die Anwendung für die Handhabung von Fällen geändert werden, in denen die Navigation `null` ist.</span><span class="sxs-lookup"><span data-stu-id="cd42b-284">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="cd42b-285">Wenn das nicht möglich ist, sollte dem Entitätstyp eine erforderliche Eigenschaft hinzugefügt werden oder mindestens einer Eigenschaft sollte ein Wert ungleich `null` zugewiesen sein.</span><span class="sxs-lookup"><span data-stu-id="cd42b-285">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="cd42b-286">Alle Entitäten, die eine Tabelle mit einer Spalte für das Parallelitätstoken gemeinsam verwenden, müssen diese einer Eigenschaft zuordnen</span><span class="sxs-lookup"><span data-stu-id="cd42b-286">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="cd42b-287">Issue #14154</span><span class="sxs-lookup"><span data-stu-id="cd42b-287">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="cd42b-288">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-288">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd42b-289">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-289">**Old behavior**</span></span>

<span data-ttu-id="cd42b-290">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="cd42b-290">Consider the following model:</span></span>
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
<span data-ttu-id="cd42b-291">Wenn `OrderDetails` im Besitz von `Order` ist oder explizit derselben Tabelle zugeordnet ist, wird vor Version 3.0 in EF Core durch das alleinige Aktualisieren von `OrderDetails` der `Version`-Wert auf dem Client nicht aktualisiert, und das nächste Update schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="cd42b-291">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="cd42b-292">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-292">**New behavior**</span></span>

<span data-ttu-id="cd42b-293">Ab Version 3.0 gibt EF Core den neuen `Version`-Wert an `Order` weiter, wenn dies Besitzer von `OrderDetails` ist.</span><span class="sxs-lookup"><span data-stu-id="cd42b-293">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="cd42b-294">Andernfalls wird eine Ausnahme während der Modellvalidierung ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="cd42b-294">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="cd42b-295">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-295">**Why**</span></span>

<span data-ttu-id="cd42b-296">Diese Änderung wurde vorgenommen, um einen veralteten Wert für ein Parallelitätstoken zu vermeiden, wenn nur eine der Entitäten, die derselben Tabelle zugeordnet sind, aktualisiert wird.</span><span class="sxs-lookup"><span data-stu-id="cd42b-296">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="cd42b-297">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-297">**Mitigations**</span></span>

<span data-ttu-id="cd42b-298">Alle Entitäten, die die Tabelle gemeinsam nutzen, müssen eine Eigenschaft enthalten, die der Spalte für das Parallelitätstoken zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="cd42b-298">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="cd42b-299">Es ist möglich, eine solche im Schattenzustand zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="cd42b-299">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="cd42b-300">Geerbte Eigenschaften von nicht zugeordneten Typen sind nun einer einzelnen Spalte für alle abgeleiteten Typen zugeordnet</span><span class="sxs-lookup"><span data-stu-id="cd42b-300">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="cd42b-301">Issue #13998</span><span class="sxs-lookup"><span data-stu-id="cd42b-301">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="cd42b-302">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-302">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd42b-303">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-303">**Old behavior**</span></span>

<span data-ttu-id="cd42b-304">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="cd42b-304">Consider the following model:</span></span>
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

<span data-ttu-id="cd42b-305">Vor Version 3.0 wurde in EF Core die Eigenschaft `ShippingAddress` standardmäßig separaten Spalten für `BulkOrder` und `Order` zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="cd42b-305">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="cd42b-306">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-306">**New behavior**</span></span>

<span data-ttu-id="cd42b-307">Ab Version 3.0 erstellt EF Core nur eine Spalte für `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="cd42b-307">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="cd42b-308">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-308">**Why**</span></span>

<span data-ttu-id="cd42b-309">Der alte Verhalten war unerwartet.</span><span class="sxs-lookup"><span data-stu-id="cd42b-309">The old behavoir was unexpected.</span></span>

<span data-ttu-id="cd42b-310">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-310">**Mitigations**</span></span>

<span data-ttu-id="cd42b-311">Die Eigenschaft kann noch immer explizit einer separaten Spalte für die abgeleiteten Typen zugeordnet werden:</span><span class="sxs-lookup"><span data-stu-id="cd42b-311">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="cd42b-312">Die Konvention zur Fremdschlüsseleigenschaft entspricht nicht mehr dem Namen der Prinzipaleigenschaft</span><span class="sxs-lookup"><span data-stu-id="cd42b-312">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="cd42b-313">Issue #13274</span><span class="sxs-lookup"><span data-stu-id="cd42b-313">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="cd42b-314">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-314">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="cd42b-315">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-315">**Old behavior**</span></span>

<span data-ttu-id="cd42b-316">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="cd42b-316">Consider the following model:</span></span>
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
<span data-ttu-id="cd42b-317">Vor Version 3.0 wurde in EF Core gemäß der Konvention die `CustomerId`-Eigenschaft für den Fremdschlüssel verwendet.</span><span class="sxs-lookup"><span data-stu-id="cd42b-317">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="cd42b-318">Wenn `Order` jedoch ein abhängiger Typ ist, wäre `CustomerId` der Primärschlüssel, was normalerweise nicht der Erwartungshaltung entspricht.</span><span class="sxs-lookup"><span data-stu-id="cd42b-318">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="cd42b-319">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-319">**New behavior**</span></span>

<span data-ttu-id="cd42b-320">Ab Version 3.0 werden in EF Core konventionsgemäß keine Eigenschaften mehr für Fremdschlüssel verwendet, wenn diese denselben Namen wie die Prinzipaleigenschaft besitzen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-320">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="cd42b-321">Die Muster für den Namen des Prinzipaltyps, der mit dem Namen der Prinzipaleigenschaft verkettet wird, und für den Navigationsnamen, der mit dem Namen der Prinzipaleigenschaft verkettet wird, werden jedoch weiterhin abgeglichen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-321">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="cd42b-322">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cd42b-322">For example:</span></span>

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

<span data-ttu-id="cd42b-323">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-323">**Why**</span></span>

<span data-ttu-id="cd42b-324">Diese Änderung wurde vorgenommen, um zu vermeiden, dass versehentlich eine Primärschlüsseleigenschaft für den abhängigen Typ definiert wird.</span><span class="sxs-lookup"><span data-stu-id="cd42b-324">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="cd42b-325">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-325">**Mitigations**</span></span>

<span data-ttu-id="cd42b-326">Wenn die Eigenschaft als Fremdschlüssel und daher Teil des Primärschlüssels vorgesehen war, müssen Sie diese explizit als solche festlegen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-326">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="cd42b-327">Die Datenbankverbindung wird jetzt geschlossen, wenn sie nicht mehr verwendet wird, bevor TransactionScope abgeschlossen wurde</span><span class="sxs-lookup"><span data-stu-id="cd42b-327">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="cd42b-328">Issue #14218</span><span class="sxs-lookup"><span data-stu-id="cd42b-328">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="cd42b-329">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-329">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd42b-330">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-330">**Old behavior**</span></span>

<span data-ttu-id="cd42b-331">Wenn vor Version 3.0 in EF Core der Kontext die Verbindung in einem `TransactionScope` öffnet, bleibt die Verbindung geöffnet, während der aktuelle `TransactionScope` aktiv ist.</span><span class="sxs-lookup"><span data-stu-id="cd42b-331">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="cd42b-332">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-332">**New behavior**</span></span>

<span data-ttu-id="cd42b-333">Ab Version 3.0 schließt EF Core die Verbindung, sobald sie nicht mehr verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="cd42b-333">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="cd42b-334">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-334">**Why**</span></span>

<span data-ttu-id="cd42b-335">Diese Änderung ermöglicht die Verwendung mehrerer Kontexte in demselben `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="cd42b-335">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="cd42b-336">Das neue Verhalten entspricht außerdem EF6.</span><span class="sxs-lookup"><span data-stu-id="cd42b-336">The new behavior also matches EF6.</span></span>

<span data-ttu-id="cd42b-337">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-337">**Mitigations**</span></span>

<span data-ttu-id="cd42b-338">Wenn die Verbindung geöffnet bleiben muss, wird durch einen expliziten Aufruf von `OpenConnection()` sichergestellt, dass EF Core diese nicht vorzeitig schließt:</span><span class="sxs-lookup"><span data-stu-id="cd42b-338">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="cd42b-339">Für jede Eigenschaft wird separat ein ganzzahliger speicherinterner Schlüssel generiert</span><span class="sxs-lookup"><span data-stu-id="cd42b-339">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="cd42b-340">Issue #6872</span><span class="sxs-lookup"><span data-stu-id="cd42b-340">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="cd42b-341">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-341">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd42b-342">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-342">**Old behavior**</span></span>

<span data-ttu-id="cd42b-343">Vor Version 3.0 wurde in EF Core ein gemeinsam verwendeter Wertegenerator für alle speicherinternen ganzzahligen Schlüsseleigenschaften verwendet.</span><span class="sxs-lookup"><span data-stu-id="cd42b-343">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="cd42b-344">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-344">**New behavior**</span></span>

<span data-ttu-id="cd42b-345">Ab Version 3.0 von EF Core wird für jede ganzzahlige Schlüsseleigenschaft ein eigener Wertegenerator bei der Verwendung der In-Memory Database verwendet.</span><span class="sxs-lookup"><span data-stu-id="cd42b-345">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="cd42b-346">Wenn die Datenbank gelöscht wird, wird die Schlüsselgenerierung für alle Tabellen zurückgesetzt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-346">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="cd42b-347">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-347">**Why**</span></span>

<span data-ttu-id="cd42b-348">Diese Änderung wurde vorgenommen, um die speicherinterne Schlüsselgenerierung enger mit der Schlüsselgenerierung für echte Datenbanken abzustimmen. Außerdem sollen bei Verwendung der In-Memory Database Tests leichter voneinander isoliert werden können.</span><span class="sxs-lookup"><span data-stu-id="cd42b-348">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="cd42b-349">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-349">**Mitigations**</span></span>

<span data-ttu-id="cd42b-350">Wenn Anwendungen von bestimmten speicherinternen Schlüsselwerten abhängig sind, funktionieren Erstere durch die eingeführte Änderung möglicherweise nicht mehr.</span><span class="sxs-lookup"><span data-stu-id="cd42b-350">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="cd42b-351">Versuchen Sie, Abhängigkeiten von bestimmten Schlüsselwerten zu vermeiden, oder passen Sie die Anwendung an das neue Verhalten an.</span><span class="sxs-lookup"><span data-stu-id="cd42b-351">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="cd42b-352">Unterstützungsfelder werden standardmäßig verwendet</span><span class="sxs-lookup"><span data-stu-id="cd42b-352">Backing fields are used by default</span></span>

[<span data-ttu-id="cd42b-353">Issue #12430</span><span class="sxs-lookup"><span data-stu-id="cd42b-353">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="cd42b-354">Diese Änderung wurde in Vorschauversion 2 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-354">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="cd42b-355">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-355">**Old behavior**</span></span>

<span data-ttu-id="cd42b-356">Vor Version 3.0 wurde in EF Core mithilfe der Getter- und Settermethoden standardmäßig der Eigenschaftswert gelesen und geschrieben. Das war selbst dann der Fall, wenn das Unterstützungsfeld für eine Eigenschaft nicht bekannt war.</span><span class="sxs-lookup"><span data-stu-id="cd42b-356">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="cd42b-357">Eine Ausnahme war die Abfrageausführung, bei der das Unterstützungsfeld direkt festgelegt wurde, wenn es bekannt war.</span><span class="sxs-lookup"><span data-stu-id="cd42b-357">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="cd42b-358">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-358">**New behavior**</span></span>

<span data-ttu-id="cd42b-359">Ab Version 3.0 wird in EF Core die Eigenschaft mithilfe des Unterstützungsfelds gelesen und geschrieben, wenn dieses für eine Eigenschaft bekannt ist.</span><span class="sxs-lookup"><span data-stu-id="cd42b-359">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="cd42b-360">Dies kann dazu führen, dass eine Anwendung nicht mehr funktioniert, wenn sie auf zusätzliches Verhalten angewiesen ist, das im Code der Getter- und Settermethoden definiert ist.</span><span class="sxs-lookup"><span data-stu-id="cd42b-360">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="cd42b-361">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-361">**Why**</span></span>

<span data-ttu-id="cd42b-362">Diese Änderung wurde vorgenommen, um zu verhindern, dass in EF Core fälschlicherweise immer dann Geschäftslogik ausgelöst wird, wenn Datenbankvorgänge für Entitäten durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-362">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="cd42b-363">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-363">**Mitigations**</span></span>

<span data-ttu-id="cd42b-364">Sie können das Verhalten vor Version 3.0 wiederherstellen, indem Sie über die modelBuilder-Fluent-API den Zugriffsmodus für die Eigenschaft konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="cd42b-364">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="cd42b-365">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cd42b-365">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="cd42b-366">Eine Ausnahme wird ausgelöst, wenn mehrere kompatible Unterstützungsfelder gefunden werden</span><span class="sxs-lookup"><span data-stu-id="cd42b-366">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="cd42b-367">Issue #12523</span><span class="sxs-lookup"><span data-stu-id="cd42b-367">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="cd42b-368">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-368">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd42b-369">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-369">**Old behavior**</span></span>

<span data-ttu-id="cd42b-370">Vor Version 3.0 wurde in EF Core ein Unterstützungsfeld auf Grundlage einer Rangfolge ausgewählt, wenn die Regeln zum Suchen eines Eigenschaftsfelds auf mehrere Felder zutrafen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-370">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="cd42b-371">Dabei konnte es vorkommen, dass in mehrdeutigen Fällen das falsche Feld verwendet wurde.</span><span class="sxs-lookup"><span data-stu-id="cd42b-371">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="cd42b-372">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-372">**New behavior**</span></span>

<span data-ttu-id="cd42b-373">Ab Version 3.0 wird in EF Core eine Ausnahme ausgelöst, wenn sich mehrere Felder auf dieselbe Eigenschaft beziehen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-373">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="cd42b-374">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-374">**Why**</span></span>

<span data-ttu-id="cd42b-375">Diese Änderung wurde vorgenommen, um zu vermeiden, dass automatisch ein falsches Feld verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="cd42b-375">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="cd42b-376">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-376">**Mitigations**</span></span>

<span data-ttu-id="cd42b-377">Für Eigenschaften mit mehrdeutigen Unterstützungsfeldern muss das zu verwendende Feld explizit festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-377">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="cd42b-378">Mit der Fluent-API ist dies beispielsweise wie folgt möglich:</span><span class="sxs-lookup"><span data-stu-id="cd42b-378">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="cd42b-379">„Nur-Feld“-Eigenschaftsnamen sollten dem Feldnamen entsprechen</span><span class="sxs-lookup"><span data-stu-id="cd42b-379">Field-only property names should match the field name</span></span>

<span data-ttu-id="cd42b-380">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-380">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd42b-381">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-381">**Old behavior**</span></span>

<span data-ttu-id="cd42b-382">Vor Version 3.0 konnte in EF Core eine Eigenschaft durch einen Zeichenfolgenwert angegeben werden, und wenn keine Eigenschaft mit diesem Namen im CLR-Typ gefunden wurde, versuchte EF Core, mithilfe von Konventionsregeln ein übereinstimmendes Feld zu finden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-382">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="cd42b-383">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-383">**New behavior**</span></span>

<span data-ttu-id="cd42b-384">Ab Version 3.0 muss in EF Core eine „Nur-Feld“-Eigenschaft mit dem Namen des Felds genau übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-384">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="cd42b-385">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-385">**Why**</span></span>

<span data-ttu-id="cd42b-386">Diese Änderung wurde vorgenommen, um zu vermeiden, dass das gleiche Feld für zwei Eigenschaften mit ähnlichem Namen verwendet wird. Durch die Änderung entsprechen nun auch die Übereinstimmungsregeln für „Nur-Feld“-Eigenschaften den Regeln für Eigenschaften, die CLR-Eigenschaften zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="cd42b-386">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="cd42b-387">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-387">**Mitigations**</span></span>

<span data-ttu-id="cd42b-388">„Nur-Feld“-Eigenschaften müssen so benannt werden wie das Feld, dem sie zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="cd42b-388">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="cd42b-389">In einer späteren Vorschauversion von EF Core 3.0 soll das explizite Konfigurieren eines Feldnamens, der sich vom Eigenschaftsnamen unterscheidet, erneut aktiviert werden:</span><span class="sxs-lookup"><span data-stu-id="cd42b-389">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="cd42b-390">Von AddDbContext/AddDbContextPool werden AddLogging und AddMemoryCache nicht mehr aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-390">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="cd42b-391">Issue #14756</span><span class="sxs-lookup"><span data-stu-id="cd42b-391">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="cd42b-392">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-392">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd42b-393">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-393">**Old behavior**</span></span>

<span data-ttu-id="cd42b-394">Vor EF Core 3.0 wurden beim Aufruf von `AddDbContext` oder `AddDbContextPool` durch Aufrufe von [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) und [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache) auch Protokollierungs- und Arbeitsspeichercaching-Dienste bei DI registriert.</span><span class="sxs-lookup"><span data-stu-id="cd42b-394">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="cd42b-395">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-395">**New behavior**</span></span>

<span data-ttu-id="cd42b-396">Ab EF Core 3.0 werden diese Dienste durch `AddDbContext` und `AddDbContextPool` nicht mehr bei Dependency Injection (DI) registriert.</span><span class="sxs-lookup"><span data-stu-id="cd42b-396">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="cd42b-397">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-397">**Why**</span></span>

<span data-ttu-id="cd42b-398">Für EF Core 3.0 ist es nicht erforderlich, dass diese Dienste im DI-Container der Anwendung enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="cd42b-398">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="cd42b-399">Wenn `ILoggerFactory` jedoch im DI-Container der Anwendung registriert ist, wird sie nach wie vor von EF Core verwendet.</span><span class="sxs-lookup"><span data-stu-id="cd42b-399">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="cd42b-400">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-400">**Mitigations**</span></span>

<span data-ttu-id="cd42b-401">Wenn Ihre Anwendung diese Dienste benötigt, registrieren Sie sie explizit mit [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) oder [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache) beim DI-Container.</span><span class="sxs-lookup"><span data-stu-id="cd42b-401">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="cd42b-402">DbContext.Entry erkennt nun mit DetectChanges lokale Änderungen</span><span class="sxs-lookup"><span data-stu-id="cd42b-402">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="cd42b-403">Issue #13552</span><span class="sxs-lookup"><span data-stu-id="cd42b-403">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="cd42b-404">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-404">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="cd42b-405">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-405">**Old behavior**</span></span>

<span data-ttu-id="cd42b-406">Vor Version 3.0 führte in EF Core ein Aufruf von `DbContext.Entry` dazu, dass Änderungen an allen überwachten Entitäten erkannt wurden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-406">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="cd42b-407">Dadurch wurde sichergestellt, dass der Zustand in `EntityEntry` immer aktuell war.</span><span class="sxs-lookup"><span data-stu-id="cd42b-407">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="cd42b-408">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-408">**New behavior**</span></span>

<span data-ttu-id="cd42b-409">Ab Version 3.0 werden in EF Core Änderungen bei einem Aufruf von `DbContext.Entry` nur in der angegebenen Entität und in allen überwachten Prinzipalentitäten erkannt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-409">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="cd42b-410">Änderungen an anderer Stelle werden daher durch den Aufruf dieser Methode möglicherweise nicht erkannt, was sich auf den Anwendungszustand auswirken kann.</span><span class="sxs-lookup"><span data-stu-id="cd42b-410">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="cd42b-411">Beachten Sie, dass selbst die Erkennung lokaler Änderungen deaktiviert wird, wenn `ChangeTracker.AutoDetectChangesEnabled` auf `false` festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="cd42b-411">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="cd42b-412">Andere Methoden wie `ChangeTracker.Entries` und `SaveChanges`, die eine Änderungserkennung auslösen, führen nach wie vor zu einem vollständigen `DetectChanges`-Vorgang für alle überwachten Entitäten.</span><span class="sxs-lookup"><span data-stu-id="cd42b-412">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="cd42b-413">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-413">**Why**</span></span>

<span data-ttu-id="cd42b-414">Diese Änderung wurde vorgenommen, um die Leistung bei der standardmäßigen Verwendung von `context.Entry` zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="cd42b-414">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="cd42b-415">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-415">**Mitigations**</span></span>

<span data-ttu-id="cd42b-416">Rufen Sie `ChgangeTracker.DetectChanges()` explizit vor dem Aufruf von `Entry` auf, wenn das Verhalten vor Version 3.0 beibehalten werden soll.</span><span class="sxs-lookup"><span data-stu-id="cd42b-416">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="cd42b-417">Schlüssel aus Zeichenfolgen und Bytearrays werden standardmäßig nicht vom Client generiert</span><span class="sxs-lookup"><span data-stu-id="cd42b-417">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="cd42b-418">Issue #14617</span><span class="sxs-lookup"><span data-stu-id="cd42b-418">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="cd42b-419">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-419">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd42b-420">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-420">**Old behavior**</span></span>

<span data-ttu-id="cd42b-421">Vor Version 3.0 konnten in EF Core `string`- und `byte[]`-Schlüsseleigenschaften verwendet werden, ohne dass explizit ein anderer Wert als NULL festgelegt werden musste.</span><span class="sxs-lookup"><span data-stu-id="cd42b-421">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="cd42b-422">In diesem Fall wurde der Schlüsselwert auf dem Client als GUID generiert und für `byte[]` in Bytes serialisiert.</span><span class="sxs-lookup"><span data-stu-id="cd42b-422">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="cd42b-423">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-423">**New behavior**</span></span>

<span data-ttu-id="cd42b-424">Ab Version 3.0 wird in EF Core eine Ausnahme ausgelöst, wenn kein Schlüsselwert festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="cd42b-424">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="cd42b-425">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-425">**Why**</span></span>

<span data-ttu-id="cd42b-426">Diese Änderung wurde vorgenommen, da vom Client generierte `string`/`byte[]`-Werte in der Regel nicht sinnvoll sind. Das Standardverhalten führte außerdem zu kaum nachvollziehbaren generierten Schlüsselwerten.</span><span class="sxs-lookup"><span data-stu-id="cd42b-426">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="cd42b-427">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-427">**Mitigations**</span></span>

<span data-ttu-id="cd42b-428">Wenn Sie das Verhalten vor Version 3.0 nutzen möchten, müssen Sie explizit festlegen, dass für die Schlüsseleigenschaften generierte Werte verwendet sollen, wenn kein anderer Nicht-NULL-Wert festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="cd42b-428">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="cd42b-429">Dies ist beispielsweise mit der Fluent-API möglich:</span><span class="sxs-lookup"><span data-stu-id="cd42b-429">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="cd42b-430">Alternativ bieten sich auch Datenanmerkungen an:</span><span class="sxs-lookup"><span data-stu-id="cd42b-430">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="cd42b-431">ILoggerFactory ist nun ein bereichsbezogener Dienst</span><span class="sxs-lookup"><span data-stu-id="cd42b-431">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="cd42b-432">Issue #14698</span><span class="sxs-lookup"><span data-stu-id="cd42b-432">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="cd42b-433">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-433">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="cd42b-434">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-434">**Old behavior**</span></span>

<span data-ttu-id="cd42b-435">Vor Version 3.0 wurde `ILoggerFactory` in EF Core als Singletondienst registriert.</span><span class="sxs-lookup"><span data-stu-id="cd42b-435">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="cd42b-436">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-436">**New behavior**</span></span>

<span data-ttu-id="cd42b-437">Ab Version 3.0 wird in EF Core `ILoggerFactory` als bereichsbezogener Dienst registriert.</span><span class="sxs-lookup"><span data-stu-id="cd42b-437">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="cd42b-438">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-438">**Why**</span></span>

<span data-ttu-id="cd42b-439">Diese Änderung wurde vorgenommen, um einer `DbContext`-Instanz eine Protokollierung zuordnen zu können. Dadurch werden weitere Funktionen bereitgestellt. In einigen Fällen wird außerdem schädliches Verhalten wie etwa die schnelle Zunahme von internen Dienstanbietern beseitigt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-439">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="cd42b-440">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-440">**Mitigations**</span></span>

<span data-ttu-id="cd42b-441">Diese Änderung wirkt sich nur dann auf den Anwendungscode aus, wenn benutzerdefinierte Dienste auf dem internen Dienstanbieter von EF Core registriert und verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-441">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="cd42b-442">Ein solches Szenario ist aber unüblich.</span><span class="sxs-lookup"><span data-stu-id="cd42b-442">This isn't common.</span></span>
<span data-ttu-id="cd42b-443">In einem derartigen Fall funktioniert fast alles wie vorher. Singletondienste, die von `ILoggerFactory` abhängig sind, müssen jedoch so angepasst werden, dass `ILoggerFactory` auf andere Weise abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="cd42b-443">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="cd42b-444">Erstellen Sie in diesen Situationen bitte im [GitHub-Issuetracker für EF Core](https://github.com/aspnet/EntityFrameworkCore/issues) ein Issue, in dem Sie beschreiben, wie Sie `ILoggerFactory` verwenden. So können wir in Zukunft erneute Breaking Changes vermeiden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-444">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="cd42b-445">IDbContextOptionsExtensionWithDebugInfo wurde mit IDbContextOptionsExtension zusammengeführt</span><span class="sxs-lookup"><span data-stu-id="cd42b-445">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="cd42b-446">Issue #13552</span><span class="sxs-lookup"><span data-stu-id="cd42b-446">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="cd42b-447">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-447">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="cd42b-448">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-448">**Old behavior**</span></span>

<span data-ttu-id="cd42b-449">`IDbContextOptionsExtensionWithDebugInfo` war eine zusätzliche optionale Schnittstelle, die von `IDbContextOptionsExtension` abgeleitet wurde, um während des 2.x-Releasezyklus Breaking Changes an der Schnittstelle zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-449">`IDbContextOptionsExtensionWithDebugInfo` was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

<span data-ttu-id="cd42b-450">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-450">**New behavior**</span></span>

<span data-ttu-id="cd42b-451">Die Schnittstellen wurden in `IDbContextOptionsExtension` zusammengeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-451">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

<span data-ttu-id="cd42b-452">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-452">**Why**</span></span>

<span data-ttu-id="cd42b-453">Diese Änderung wurde vorgenommen, da es sich im Prinzip um eine Schnittstelle handelt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-453">This change was made because the interfaces are conceptually one.</span></span>

<span data-ttu-id="cd42b-454">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-454">**Mitigations**</span></span>

<span data-ttu-id="cd42b-455">Alle Implementierungen von `IDbContextOptionsExtension` müssen aktualisiert werden, damit der neue Member unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="cd42b-455">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="cd42b-456">Für Proxys mit Lazy Loading (trägem Laden) gilt nicht mehr die Annahme, dass Navigationseigenschaften vollständig geladen sind</span><span class="sxs-lookup"><span data-stu-id="cd42b-456">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="cd42b-457">Issue #12780</span><span class="sxs-lookup"><span data-stu-id="cd42b-457">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="cd42b-458">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-458">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd42b-459">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-459">**Old behavior**</span></span>

<span data-ttu-id="cd42b-460">Vor Version 3.0 gab es in EF Core nach der Freigabe eines `DbContext`-Objekts keine Möglichkeit, zu ermitteln, ob eine aus dem Kontext abgerufene Navigationseigenschaft für eine Entität vollständig geladen war.</span><span class="sxs-lookup"><span data-stu-id="cd42b-460">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="cd42b-461">Stattdessen galt für Proxys die Annahme, dass eine Verweisnavigation geladen ist, falls für diese ein anderer Wert als NULL vorliegt. Außerdem wurde davon ausgegangen, dass eine Sammlungsnavigation geladen ist, wenn sie nicht leer ist.</span><span class="sxs-lookup"><span data-stu-id="cd42b-461">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="cd42b-462">In diesen Fällen entsprach das Lazy Loading einer Nulloperation („No-Op“).</span><span class="sxs-lookup"><span data-stu-id="cd42b-462">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="cd42b-463">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-463">**New behavior**</span></span>

<span data-ttu-id="cd42b-464">Ab Version 3.0 überwachen in EF Core Proxys, ob eine Navigationseigenschaft geladen ist.</span><span class="sxs-lookup"><span data-stu-id="cd42b-464">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="cd42b-465">Wenn also auf eine Navigationseigenschaft zugegriffen werden soll, die nach der Freigabe des Kontexts geladen wird, ist dies immer eine No-Op-Operation. Das ist selbst dann der Fall, wenn die geladene Navigation leer oder NULL ist.</span><span class="sxs-lookup"><span data-stu-id="cd42b-465">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="cd42b-466">Wenn umgekehrt auf eine nicht geladene Navigationseigenschaft zugegriffen werden soll und der Kontext bereits freigegeben wurde, wird eine Ausnahme ausgelöst. Dieser Fall tritt auch dann ein, wenn die Navigationseigenschaft eine nicht leere Sammlung ist.</span><span class="sxs-lookup"><span data-stu-id="cd42b-466">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="cd42b-467">In dieser Situation wird also vom Anwendungscode versucht, zu einem ungültigen Zeitpunkt Lazy Loading durchzuführen. Die Anwendung sollte so angepasst werden, dass dieser Vorgang vermieden wird.</span><span class="sxs-lookup"><span data-stu-id="cd42b-467">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="cd42b-468">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-468">**Why**</span></span>

<span data-ttu-id="cd42b-469">Diese Änderung wurde vorgenommen, um das Verhalten beim Lazy Loading einer freigegebenen `DbContext`-Instanz konsistent und korrekt zu gestalten.</span><span class="sxs-lookup"><span data-stu-id="cd42b-469">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="cd42b-470">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-470">**Mitigations**</span></span>

<span data-ttu-id="cd42b-471">Aktualisieren Sie den Anwendungscode so, dass kein Lazy Loading durchgeführt wird, nachdem der Kontext freigegeben wurde. Alternativ können Sie auch, wie in der Ausnahmemeldung beschrieben wird, eine No-Op-Operation festlegen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-471">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="cd42b-472">Die Erstellung zu vieler interner Dienstanbieter führt standardmäßig zu einem Fehler</span><span class="sxs-lookup"><span data-stu-id="cd42b-472">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="cd42b-473">Issue #10236</span><span class="sxs-lookup"><span data-stu-id="cd42b-473">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="cd42b-474">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-474">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="cd42b-475">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-475">**Old behavior**</span></span>

<span data-ttu-id="cd42b-476">Vor Version 3.0 wurde in EF Core eine Warnung für eine Anwendung protokolliert, die unverhältnismäßig viele interne Dienstanbieter erstellte.</span><span class="sxs-lookup"><span data-stu-id="cd42b-476">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="cd42b-477">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-477">**New behavior**</span></span>

<span data-ttu-id="cd42b-478">Ab Version 3.0 wird diese Warnung in EF Core als Fehler betrachtet, und eine Ausnahme wird ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="cd42b-478">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="cd42b-479">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-479">**Why**</span></span>

<span data-ttu-id="cd42b-480">Diese Änderung wurde vorgenommen, damit auf den oben beschriebenen Fall explizit hingewiesen wird. So soll die Entwicklung von besserem Anwendungscode erleichtert werden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-480">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="cd42b-481">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-481">**Mitigations**</span></span>

<span data-ttu-id="cd42b-482">Wenn dieser Fehler auftritt, müssen Sie die Grundursache ermitteln und die Erstellung vieler interner Dienstanbieter unterbinden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-482">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="cd42b-483">Sie können den Fehler allerdings auch wieder in eine Warnung konvertierten (oder ihn ignorieren), indem Sie `DbContextOptionsBuilder` konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="cd42b-483">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="cd42b-484">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cd42b-484">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="cd42b-485">Neues Verhalten für HasOne/HasMany bei Aufruf mit einer einzelnen Zeichenfolge</span><span class="sxs-lookup"><span data-stu-id="cd42b-485">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="cd42b-486">Issue #9171</span><span class="sxs-lookup"><span data-stu-id="cd42b-486">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="cd42b-487">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-487">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd42b-488">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-488">**Old behavior**</span></span>

<span data-ttu-id="cd42b-489">Vor EF Core 3.0 wurde Code, der `HasOne` oder `HasMany` mit einer einzelnen Zeichenfolge aufruft, auf irritierende Weise interpretiert.</span><span class="sxs-lookup"><span data-stu-id="cd42b-489">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="cd42b-490">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cd42b-490">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="cd42b-491">Der Code scheint `Samurai` mit einem anderen Entitätstyp über die `Entrance`-Navigationseigenschaft zu verknüpfen, die möglicherweise privat ist.</span><span class="sxs-lookup"><span data-stu-id="cd42b-491">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="cd42b-492">Tatsächlich versucht dieser Code, eine Beziehung zu einem Entitätstyp namens `Entrance` ohne Navigationseigenschaft herzustellen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-492">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="cd42b-493">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-493">**New behavior**</span></span>

<span data-ttu-id="cd42b-494">Ab EF Core 3.0 führt der obige Code jetzt die erwartete Aktion durch.</span><span class="sxs-lookup"><span data-stu-id="cd42b-494">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="cd42b-495">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-495">**Why**</span></span>

<span data-ttu-id="cd42b-496">Das alte Verhalten war sehr verwirrend, vor allem beim Lesen des Konfigurationscodes und bei der Suche nach Fehlern.</span><span class="sxs-lookup"><span data-stu-id="cd42b-496">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="cd42b-497">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-497">**Mitigations**</span></span>

<span data-ttu-id="cd42b-498">Dies führt nur zu Fehlern in Anwendungen, die Beziehungen explizit unter Verwendung von Zeichenfolgen für Typnamen und ohne explizite Angabe der Navigationseigenschaft konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="cd42b-498">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="cd42b-499">Dies ist nicht üblich.</span><span class="sxs-lookup"><span data-stu-id="cd42b-499">This is not common.</span></span>
<span data-ttu-id="cd42b-500">Das vorherige Verhalten kann durch explizite Übergabe von `null` für den Namen der Navigationseigenschaft erzielt werden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-500">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="cd42b-501">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cd42b-501">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="cd42b-502">Der Rückgabetyp für mehrere asynchrone Methoden wurde von Task in ValueTask geändert</span><span class="sxs-lookup"><span data-stu-id="cd42b-502">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="cd42b-503">Issue #15184</span><span class="sxs-lookup"><span data-stu-id="cd42b-503">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="cd42b-504">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-504">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd42b-505">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-505">**Old behavior**</span></span>

<span data-ttu-id="cd42b-506">Die folgenden asynchronen Methoden gaben zuvor `Task<T>` zurück:</span><span class="sxs-lookup"><span data-stu-id="cd42b-506">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="cd42b-507">`ValueGenerator.NextValueAsync()` (und abgeleitete Klassen)</span><span class="sxs-lookup"><span data-stu-id="cd42b-507">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="cd42b-508">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-508">**New behavior**</span></span>

<span data-ttu-id="cd42b-509">Die oben genannten Methoden geben nun `ValueTask<T>` über dieselbe `T` wie zuvor zurück.</span><span class="sxs-lookup"><span data-stu-id="cd42b-509">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="cd42b-510">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-510">**Why**</span></span>

<span data-ttu-id="cd42b-511">Durch diese Änderung verringert sich die Anzahl der Heapzuordnungen, die beim Aufrufen dieser Methoden entstehen, und dies führt zu einer Verbesserung der allgemeinen Leistung.</span><span class="sxs-lookup"><span data-stu-id="cd42b-511">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="cd42b-512">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-512">**Mitigations**</span></span>

<span data-ttu-id="cd42b-513">Anwendungen, die einfach die oben genannten APIs erwarten, müssen nur neu kompiliert werden – Quelländerungen sind nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="cd42b-513">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="cd42b-514">Eine komplexere Verwendung (z.B. Übergeben der zurückgegebenen `Task` an `Task.WhenAny()`) erfordern in der Regel, dass die zurückgegebene `ValueTask<T>` durch einen Aufruf von `AsTask()` in `Task<T>` konvertiert wird.</span><span class="sxs-lookup"><span data-stu-id="cd42b-514">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="cd42b-515">Beachten Sie, dass dadurch die mit dieser Änderung verbundene Verringerung der Zuordnungen aufgehoben wird.</span><span class="sxs-lookup"><span data-stu-id="cd42b-515">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="cd42b-516">Die Anmerkung Relational:TypeMapping heißt nun nur noch TypeMapping</span><span class="sxs-lookup"><span data-stu-id="cd42b-516">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="cd42b-517">Issue #9913</span><span class="sxs-lookup"><span data-stu-id="cd42b-517">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="cd42b-518">Diese Änderung wurde in Vorschauversion 2 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-518">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="cd42b-519">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-519">**Old behavior**</span></span>

<span data-ttu-id="cd42b-520">Der Anmerkungsname für Typzuordnungsanmerkungen war bisher Relational:TypeMapping.</span><span class="sxs-lookup"><span data-stu-id="cd42b-520">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="cd42b-521">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-521">**New behavior**</span></span>

<span data-ttu-id="cd42b-522">Der Anmerkungsname für Typzuordnungsanmerkungen lautet nun TypeMapping.</span><span class="sxs-lookup"><span data-stu-id="cd42b-522">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="cd42b-523">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-523">**Why**</span></span>

<span data-ttu-id="cd42b-524">Typzuordnungen werden nicht mehr ausschließlich für Anbieter relationaler Datenbanken verwendet.</span><span class="sxs-lookup"><span data-stu-id="cd42b-524">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="cd42b-525">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-525">**Mitigations**</span></span>

<span data-ttu-id="cd42b-526">Diese Änderung ist nur dann ein Breaking Change, wenn Anwendungen direkt über eine Anmerkung auf die Typzuordnung zugreifen. Dieses Szenario ist jedoch unüblich.</span><span class="sxs-lookup"><span data-stu-id="cd42b-526">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="cd42b-527">Verzichten Sie daher darauf, die Anmerkung direkt zu verwenden, und greifen Sie stattdessen über die Fluent-API auf Typzuordnungen zu, um das Problem zu beheben.</span><span class="sxs-lookup"><span data-stu-id="cd42b-527">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="cd42b-528">ToTable löst für einen abgeleiteten Typ eine Ausnahme aus</span><span class="sxs-lookup"><span data-stu-id="cd42b-528">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="cd42b-529">Issue #11811</span><span class="sxs-lookup"><span data-stu-id="cd42b-529">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="cd42b-530">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-530">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="cd42b-531">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-531">**Old behavior**</span></span>

<span data-ttu-id="cd42b-532">Vor Version 3.0 wurde in EF Core der Aufruf von `ToTable()` für einen abgeleiteten Typ ignoriert, da TPH die einzige Strategie zur Vererbungszuordnung war und diese in unzulässigen Fällen angewendet wurde.</span><span class="sxs-lookup"><span data-stu-id="cd42b-532">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="cd42b-533">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-533">**New behavior**</span></span>

<span data-ttu-id="cd42b-534">Ab Version 3.0 wird in EF Core eine Ausnahme ausgelöst, um später eine unerwartete Zuordnung zu vermeiden, wenn `ToTable()` für einen abgeleiteten Typ aufgerufen wird. Dieser Schritt dient auch als Vorbereitung für die TPT- und TPC-Unterstützung, die in einem künftigen Release folgen wird.</span><span class="sxs-lookup"><span data-stu-id="cd42b-534">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="cd42b-535">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-535">**Why**</span></span>

<span data-ttu-id="cd42b-536">Derzeit ist es nicht zulässig, einen abgeleiteten Typ einer anderen Tabelle zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-536">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="cd42b-537">Durch diese Änderung werden Breaking Changes vermieden, wenn der beschriebene Vorgang in Zukunft zulässig wird.</span><span class="sxs-lookup"><span data-stu-id="cd42b-537">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="cd42b-538">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-538">**Mitigations**</span></span>

<span data-ttu-id="cd42b-539">Entfernen Sie alle Zuordnungen von abgeleiteten Typen zu anderen Tabellen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-539">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="cd42b-540">ForSqlServerHasIndex wurde durch HasIndex ersetzt</span><span class="sxs-lookup"><span data-stu-id="cd42b-540">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="cd42b-541">Issue #12366</span><span class="sxs-lookup"><span data-stu-id="cd42b-541">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="cd42b-542">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-542">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="cd42b-543">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-543">**Old behavior**</span></span>

<span data-ttu-id="cd42b-544">Vor Version 3.0 konnten in EF Core mit `ForSqlServerHasIndex().ForSqlServerInclude()` Spalten konfiguriert werden, die mit `INCLUDE` verwendet wurden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-544">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="cd42b-545">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-545">**New behavior**</span></span>

<span data-ttu-id="cd42b-546">Ab Version 3.0 wird in EF Core die Nutzung von `Include` für einen Index auf der relationalen Ebene unterstützt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-546">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="cd42b-547">Verwenden Sie `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="cd42b-547">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="cd42b-548">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-548">**Why**</span></span>

<span data-ttu-id="cd42b-549">Diese Änderung wurde vorgenommen, um die API für Indizes mit `Include` an einer zentralen Stelle für alle Datenbankanbieter zusammenzuführen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-549">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="cd42b-550">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-550">**Mitigations**</span></span>

<span data-ttu-id="cd42b-551">Verwenden Sie wie oben beschrieben die neue API.</span><span class="sxs-lookup"><span data-stu-id="cd42b-551">Use the new API, as shown above.</span></span>

## <a name="metadata-api-changes"></a><span data-ttu-id="cd42b-552">Metadaten-API-Änderungen</span><span class="sxs-lookup"><span data-stu-id="cd42b-552">Metadata API changes</span></span>

[<span data-ttu-id="cd42b-553">Issue #214</span><span class="sxs-lookup"><span data-stu-id="cd42b-553">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="cd42b-554">Diese Änderung wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-554">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd42b-555">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-555">**New behavior**</span></span>

<span data-ttu-id="cd42b-556">Die folgenden Eigenschaften wurden in Erweiterungsmethoden konvertiert:</span><span class="sxs-lookup"><span data-stu-id="cd42b-556">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="cd42b-557">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-557">**Why**</span></span>

<span data-ttu-id="cd42b-558">Mit dieser Änderung wird die Implementierung der oben genannten Schnittstellen vereinfacht.</span><span class="sxs-lookup"><span data-stu-id="cd42b-558">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="cd42b-559">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-559">**Mitigations**</span></span>

<span data-ttu-id="cd42b-560">Verwenden Sie die neuen Erweiterungsmethoden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-560">Use the new extension methods.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="cd42b-561">EF Core sendet keine PRAGMA-Anweisungen mehr, um Fremdschlüssel in SQLite zu erzwingen</span><span class="sxs-lookup"><span data-stu-id="cd42b-561">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="cd42b-562">Issue #12151</span><span class="sxs-lookup"><span data-stu-id="cd42b-562">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="cd42b-563">Diese Änderung wurde in Vorschauversion 3 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-563">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="cd42b-564">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-564">**Old behavior**</span></span>

<span data-ttu-id="cd42b-565">Vor Version 3.0 wurden in EF Core `PRAGMA foreign_keys = 1`-Anweisungen gesendet, wenn eine Verbindung mit SQLite hergestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="cd42b-565">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="cd42b-566">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-566">**New behavior**</span></span>

<span data-ttu-id="cd42b-567">Ab Version 3.0 werden in EF Core keine `PRAGMA foreign_keys = 1`-Anweisungen mehr gesendet, wenn eine Verbindung mit SQLite hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="cd42b-567">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="cd42b-568">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-568">**Why**</span></span>

<span data-ttu-id="cd42b-569">Diese Änderung wurde vorgenommen, da von EF Core standardmäßig `SQLitePCLRaw.bundle_e_sqlite3` verwendet wird. Dadurch ist die Erzwingung von Fremdschlüsseln standardmäßig aktiviert und muss nicht jedes Mal explizit aktiviert werden, wenn eine Verbindung hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="cd42b-569">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="cd42b-570">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-570">**Mitigations**</span></span>

<span data-ttu-id="cd42b-571">Fremdschlüssel sind in SQLitePCLRaw.bundle_e_sqlite3 standardmäßig aktiviert. Dieses Bundle wird standardmäßig von EF Core verwendet.</span><span class="sxs-lookup"><span data-stu-id="cd42b-571">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="cd42b-572">In anderen Fällen können Fremdschlüssel durch die Angabe `Foreign Keys=True` in der Verbindungszeichenfolge aktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-572">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="cd42b-573">SQLitePCLRaw.bundle_e_sqlite3 ist nun eine Abhängigkeit von Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="cd42b-573">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="cd42b-574">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-574">**Old behavior**</span></span>

<span data-ttu-id="cd42b-575">Vor Version 3.0 wurde von EF Core `SQLitePCLRaw.bundle_green` verwendet.</span><span class="sxs-lookup"><span data-stu-id="cd42b-575">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="cd42b-576">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-576">**New behavior**</span></span>

<span data-ttu-id="cd42b-577">Ab Version 3.0 wird von EF Core `SQLitePCLRaw.bundle_e_sqlite3` verwendet.</span><span class="sxs-lookup"><span data-stu-id="cd42b-577">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="cd42b-578">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-578">**Why**</span></span>

<span data-ttu-id="cd42b-579">Diese Änderung wurde vorgenommen, damit die unter iOS verwendete Version von SQLite sich konsistent zu Versionen auf anderen Plattformen verhält.</span><span class="sxs-lookup"><span data-stu-id="cd42b-579">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="cd42b-580">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-580">**Mitigations**</span></span>

<span data-ttu-id="cd42b-581">Konfigurieren Sie `Microsoft.Data.Sqlite` so, dass ein anderes `SQLitePCLRaw`-Bundle verwendet wird, um die native SQLite-Version unter iOS zu nutzen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-581">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="cd42b-582">GUID-Werte werden jetzt als TEXT in SQLite gespeichert</span><span class="sxs-lookup"><span data-stu-id="cd42b-582">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="cd42b-583">Issue #15078</span><span class="sxs-lookup"><span data-stu-id="cd42b-583">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="cd42b-584">Diese Änderung wurde mit EF Core 3.0 Preview 4 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-584">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd42b-585">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-585">**Old behavior**</span></span>

<span data-ttu-id="cd42b-586">GUID-Werte wurden in der Vergangenheit in SQLite als BLOB-Werte gespeichert.</span><span class="sxs-lookup"><span data-stu-id="cd42b-586">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="cd42b-587">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-587">**New behavior**</span></span>

<span data-ttu-id="cd42b-588">GUID-Werte werden jetzt als TEXT gespeichert.</span><span class="sxs-lookup"><span data-stu-id="cd42b-588">Guid values are now sotred as TEXT.</span></span>

<span data-ttu-id="cd42b-589">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-589">**Why**</span></span>

<span data-ttu-id="cd42b-590">Das Binärformat der GUIDs ist nicht standardisiert.</span><span class="sxs-lookup"><span data-stu-id="cd42b-590">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="cd42b-591">Das Speichern der Werte als TEXT führt dazu, dass die Datenbank mit anderen Technologien eher kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="cd42b-591">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="cd42b-592">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-592">**Mitigations**</span></span>

<span data-ttu-id="cd42b-593">Sie können vorhandene Datenbanken zum neuen Format migrieren, indem Sie SQL-Code wie den folgenden ausführen:</span><span class="sxs-lookup"><span data-stu-id="cd42b-593">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="cd42b-594">Sie können in EF Core auch weiterhin das alte Verhalten verwenden, indem Sie einen Wertkonverter für diese Eigenschaften konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="cd42b-594">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="cd42b-595">Microsoft.Data.Sqlite kann weiterhin GUID-Werte aus BLOB- und TEXT-Spalten lesen. Da sich jedoch das Standardformat für Parameter und Konstanten geändert hat, müssen Sie wahrscheinlich Aktionen für die meisten GUID-Szenarios vornehmen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-595">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="cd42b-596">Char-Werte werden jetzt als TEXT in SQLite gespeichert</span><span class="sxs-lookup"><span data-stu-id="cd42b-596">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="cd42b-597">Issue #15020</span><span class="sxs-lookup"><span data-stu-id="cd42b-597">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="cd42b-598">Diese Änderung wurde mit EF Core 3.0 Preview 4 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-598">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd42b-599">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-599">**Old behavior**</span></span>

<span data-ttu-id="cd42b-600">Char-Werte wurden in der Vergangenheit in SQLite als INTEGER-Werte gespeichert.</span><span class="sxs-lookup"><span data-stu-id="cd42b-600">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="cd42b-601">Der Char-Wert *A* wurde z.B. als INTEGER-Wert 65 gespeichert.</span><span class="sxs-lookup"><span data-stu-id="cd42b-601">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="cd42b-602">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-602">**New behavior**</span></span>

<span data-ttu-id="cd42b-603">Char-Werte werden jetzt als TEXT gespeichert.</span><span class="sxs-lookup"><span data-stu-id="cd42b-603">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="cd42b-604">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-604">**Why**</span></span>

<span data-ttu-id="cd42b-605">Das Speichern der Werte als TEXT ist naheliegender und führt dazu, dass die Datenbank mit anderen Technologien eher kompatibel ist.</span><span class="sxs-lookup"><span data-stu-id="cd42b-605">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="cd42b-606">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-606">**Mitigations**</span></span>

<span data-ttu-id="cd42b-607">Sie können vorhandene Datenbanken zum neuen Format migrieren, indem Sie SQL-Code wie den folgenden ausführen:</span><span class="sxs-lookup"><span data-stu-id="cd42b-607">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="cd42b-608">Sie können in EF Core auch weiterhin das alte Verhalten verwenden, indem Sie einen Wertkonverter für diese Eigenschaften konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="cd42b-608">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="cd42b-609">Microsoft.Data.Sqlite kann auch weiterhin Zeichenwerte aus INTEGER- und TEXT-Spalten lesen, weshalb es sein kann, dass in einigen Szenarios keine weiteren Maßnahmen erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="cd42b-609">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="cd42b-610">Migrations-IDs werden jetzt mit dem Kalender der invarianten Kultur generiert</span><span class="sxs-lookup"><span data-stu-id="cd42b-610">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="cd42b-611">Issue #12978</span><span class="sxs-lookup"><span data-stu-id="cd42b-611">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="cd42b-612">Diese Änderung wurde mit EF Core 3.0 Preview 4 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-612">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd42b-613">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-613">**Old behavior**</span></span>

<span data-ttu-id="cd42b-614">Migrations-IDs wurden versehentlich mit dem Kalender der aktuellen Kultur generiert.</span><span class="sxs-lookup"><span data-stu-id="cd42b-614">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="cd42b-615">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-615">**New behavior**</span></span>

<span data-ttu-id="cd42b-616">Migrations-IDs werden jetzt immer mit dem Kalender der invarianten Kultur generiert (gregorianischer Kalender).</span><span class="sxs-lookup"><span data-stu-id="cd42b-616">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="cd42b-617">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-617">**Why**</span></span>

<span data-ttu-id="cd42b-618">Die Reihenfolge der Migrationen ist beim Aktualisieren einer Datenbank oder Auflösen von Mergekonflikten wesentlich.</span><span class="sxs-lookup"><span data-stu-id="cd42b-618">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="cd42b-619">Wenn der invariante Kalender verwendet wird, werden Probleme bei der Reihenfolge vermieden, die entstehen, wenn Teammitglieder unterschiedliche Systemkalender verwenden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-619">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="cd42b-620">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-620">**Mitigations**</span></span>

<span data-ttu-id="cd42b-621">Diese Änderung betrifft jeden Benutzer, der einen Kalender verwendet, der nicht gregorianisch ist und bei dem die Jahreszahl höher als die des gregorianischen Kalenders ist (wie z.B. in der buddhistischen Zeitrechnung).</span><span class="sxs-lookup"><span data-stu-id="cd42b-621">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="cd42b-622">Vorhandene Migrations-IDs müssen aktualisiert werden, damit neue Migrationen in der Reihenfolge hinter vorhandenen Migrationen eingeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-622">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="cd42b-623">Sie können die Migrations-ID im Migration-Attribut in der Designerdatei der Migration finden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-623">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="cd42b-624">Die Tabelle mit dem Migrationsverlauf muss auch aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="cd42b-624">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="cd42b-625">LogQueryPossibleExceptionWithAggregateOperator wurde umbenannt</span><span class="sxs-lookup"><span data-stu-id="cd42b-625">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="cd42b-626">Issue #10985</span><span class="sxs-lookup"><span data-stu-id="cd42b-626">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="cd42b-627">Diese Änderung wurde mit EF Core 3.0 Preview 4 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-627">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd42b-628">**Änderung**</span><span class="sxs-lookup"><span data-stu-id="cd42b-628">**Change**</span></span>

<span data-ttu-id="cd42b-629">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` wurde umbenannt in `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="cd42b-629">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="cd42b-630">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-630">**Why**</span></span>

<span data-ttu-id="cd42b-631">Die Benennung dieses Warnereignisses wurde an allen anderen Warnereignissen ausgerichtet.</span><span class="sxs-lookup"><span data-stu-id="cd42b-631">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="cd42b-632">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-632">**Mitigations**</span></span>

<span data-ttu-id="cd42b-633">Verwenden Sie den neuen Namen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-633">Use the new name.</span></span> <span data-ttu-id="cd42b-634">(Beachten Sie, dass sich die Ereignis-ID-Nummer nicht geändert hat.)</span><span class="sxs-lookup"><span data-stu-id="cd42b-634">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="cd42b-635">Verdeutlichen der API für Einschränkungsnamen von Fremdschlüsseln</span><span class="sxs-lookup"><span data-stu-id="cd42b-635">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="cd42b-636">Issue #10730</span><span class="sxs-lookup"><span data-stu-id="cd42b-636">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="cd42b-637">Diese Änderung wurde mit EF Core 3.0 Preview 4 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="cd42b-637">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="cd42b-638">**Altes Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-638">**Old behavior**</span></span>

<span data-ttu-id="cd42b-639">Vor EF Core 3.0 wurden Einschränkungsnamen von Fremdschlüsseln einfach als „Namen“ bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="cd42b-639">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="cd42b-640">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cd42b-640">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="cd42b-641">**Neues Verhalten**</span><span class="sxs-lookup"><span data-stu-id="cd42b-641">**New behavior**</span></span>

<span data-ttu-id="cd42b-642">Ab EF Core 3.0 werden Einschränkungsnamen von Fremdschlüsseln nun als „Einschränkungsnamen“ bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="cd42b-642">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="cd42b-643">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cd42b-643">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="cd42b-644">**Hintergründe**</span><span class="sxs-lookup"><span data-stu-id="cd42b-644">**Why**</span></span>

<span data-ttu-id="cd42b-645">Durch diese Änderung wird Konsistenz für Benennungen in diesem Bereich gewährleistet, und es wird verdeutlicht, dass es sich dabei um den Namen der Fremdschlüsseleinschränkung handelt und nicht um den Spalten- oder Eigenschaftennamen, auf deren Grundlage der Fremdschlüssel definiert ist.</span><span class="sxs-lookup"><span data-stu-id="cd42b-645">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="cd42b-646">**Vorbeugende Maßnahmen**</span><span class="sxs-lookup"><span data-stu-id="cd42b-646">**Mitigations**</span></span>

<span data-ttu-id="cd42b-647">Verwenden Sie den neuen Namen.</span><span class="sxs-lookup"><span data-stu-id="cd42b-647">Use the new name.</span></span>
