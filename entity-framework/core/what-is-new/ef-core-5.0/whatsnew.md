---
title: Neuerungen in EF Core 5.0
author: ajcvickers
ms.date: 03/15/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: 08a93555fd76d8a9f6d3011f59d9a34f76d0b22f
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136254"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="fb56a-102">Neuerungen in EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="fb56a-102">What's New in EF Core 5.0</span></span>

<span data-ttu-id="fb56a-103">EF Core 5.0 befindet sich derzeit in der Entwicklung.</span><span class="sxs-lookup"><span data-stu-id="fb56a-103">EF Core 5.0 is currently in development.</span></span>
<span data-ttu-id="fb56a-104">Diese Seite bietet eine Übersicht über interessante Änderungen in den einzelnen Vorschauversionen.</span><span class="sxs-lookup"><span data-stu-id="fb56a-104">This page will contain an overview of interesting changes introduced in each preview.</span></span>

<span data-ttu-id="fb56a-105">Auf dieser Seite wird der [Plan für EF Core 5.0](plan.md) nicht erneut aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="fb56a-105">This page does not duplicate the [plan for EF Core 5.0](plan.md).</span></span>
<span data-ttu-id="fb56a-106">Der Plan beschreibt die allgemeinen Themen für EF Core 5.0 einschließlich sämtlicher Features, die wir vor Auslieferung des finalen Releases integrieren möchten.</span><span class="sxs-lookup"><span data-stu-id="fb56a-106">The plan describes the overall themes for EF Core 5.0, including everything we are planning to include before shipping the final release.</span></span>

<span data-ttu-id="fb56a-107">Wir werden an dieser Stelle Links zur offiziellen Dokumentation einfügen, sobald diese veröffentlicht ist.</span><span class="sxs-lookup"><span data-stu-id="fb56a-107">We will add links from here to the official documentation as it is published.</span></span>

## <a name="preview-1"></a><span data-ttu-id="fb56a-108">Vorschauversion 1</span><span class="sxs-lookup"><span data-stu-id="fb56a-108">Preview 1</span></span>

### <a name="simple-logging"></a><span data-ttu-id="fb56a-109">Einfache Protokollierung</span><span class="sxs-lookup"><span data-stu-id="fb56a-109">Simple logging</span></span>

<span data-ttu-id="fb56a-110">Dieses Feature fügt eine Funktion hinzu, die `Database.Log` in EF6 ähnelt.</span><span class="sxs-lookup"><span data-stu-id="fb56a-110">This feature adds functionality similar to `Database.Log` in EF6.</span></span>
<span data-ttu-id="fb56a-111">Es bietet eine einfache Möglichkeit zum Abrufen von Protokollen aus EF Core, ohne ein externes Protokollierungsframework konfigurieren zu müssen.</span><span class="sxs-lookup"><span data-stu-id="fb56a-111">That is, it provides a simple way to get logs from EF Core without the need to configure any kind of external logging framework.</span></span>

<span data-ttu-id="fb56a-112">Eine vorläufige Dokumentation ist im [Wöchentlichen Status von EF vom 5. Dezember 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863) (in englischer Sprache) enthalten.</span><span class="sxs-lookup"><span data-stu-id="fb56a-112">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="fb56a-113">Zusätzliche Dokumentation finden Sie im Issue [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span><span class="sxs-lookup"><span data-stu-id="fb56a-113">Additional documentation is tracked by issue [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span></span>

### <a name="simple-way-to-get-generated-sql"></a><span data-ttu-id="fb56a-114">Einfache Möglichkeit zum Abrufen von generiertem SQL-Code</span><span class="sxs-lookup"><span data-stu-id="fb56a-114">Simple way to get generated SQL</span></span>

<span data-ttu-id="fb56a-115">In EF Core 5.0 wird die Erweiterungsmethode `ToQueryString` eingeführt, die die von EF Core beim Ausführen einer LINQ-Abfrage generierte SQL-Abfrage zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="fb56a-115">EF Core 5.0 introduces the `ToQueryString` extension method which will return the SQL that EF Core will generate when executing a LINQ query.</span></span>

<span data-ttu-id="fb56a-116">Eine vorläufige Dokumentation ist im [Wöchentlichen Status von EF vom 9. Januar 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246) (in englischer Sprache) enthalten.</span><span class="sxs-lookup"><span data-stu-id="fb56a-116">Preliminary documentation is included in the [EF weekly status for January 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span></span>

<span data-ttu-id="fb56a-117">Zusätzliche Dokumentation finden Sie im Issue [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span><span class="sxs-lookup"><span data-stu-id="fb56a-117">Additional documentation is tracked by issue [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span></span>

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a><span data-ttu-id="fb56a-118">Verwenden eines C#-Attributs zum Angeben des Fehlens eines Schlüssels einer Entität</span><span class="sxs-lookup"><span data-stu-id="fb56a-118">Use a C# attribute to indicate that an entity has no key</span></span>

<span data-ttu-id="fb56a-119">Ein Entitätstyp kann nun mithilfe des neuen Attributs `KeylessAttribute` als schlüssellos konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="fb56a-119">An entity type can now be configured as having no key using the new `KeylessAttribute`.</span></span>
<span data-ttu-id="fb56a-120">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fb56a-120">For example:</span></span>

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

<span data-ttu-id="fb56a-121">Die Dokumentation finden Sie im Issue [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span><span class="sxs-lookup"><span data-stu-id="fb56a-121">Documentation is tracked by issue [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span></span>

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a><span data-ttu-id="fb56a-122">Ändern von Verbindung oder Verbindungszeichenfolge in initialisiertem DbContext möglich</span><span class="sxs-lookup"><span data-stu-id="fb56a-122">Connection or connection string can be changed on initialized DbContext</span></span>

<span data-ttu-id="fb56a-123">Es ist jetzt einfacher, eine DbContext-Instanz ohne Verbindung oder Verbindungszeichenfolge zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="fb56a-123">It is now easier to create a DbContext instance without any connection or connection string.</span></span>
<span data-ttu-id="fb56a-124">Die Verbindung oder Verbindungszeichenfolge kann jetzt auch in der Kontextinstanz verändert werden.</span><span class="sxs-lookup"><span data-stu-id="fb56a-124">Also, the connection or connection string can now be mutated on the context instance.</span></span>
<span data-ttu-id="fb56a-125">So kann ein und dieselbe Kontextinstanz dynamisch eine Verbindung mit verschiedenen Datenbanken herstellen.</span><span class="sxs-lookup"><span data-stu-id="fb56a-125">This allows the same context instance to dynamically connect to different databases.</span></span>

<span data-ttu-id="fb56a-126">Die Dokumentation finden Sie im Issue [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span><span class="sxs-lookup"><span data-stu-id="fb56a-126">Documentation is tracked by issue [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span></span>

### <a name="change-tracking-proxies"></a><span data-ttu-id="fb56a-127">Proxys zur Änderungsnachverfolgung</span><span class="sxs-lookup"><span data-stu-id="fb56a-127">Change-tracking proxies</span></span>

<span data-ttu-id="fb56a-128">EF Core kann jetzt Runtimeproxys generieren, die automatisch [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) und [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1) implementieren.</span><span class="sxs-lookup"><span data-stu-id="fb56a-128">EF Core can now generate runtime proxies that automatically implement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) and [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span></span>
<span data-ttu-id="fb56a-129">Diese melden dann Wertänderungen bei Entitätseigenschaften direkt an EF Core, sodass die Notwendigkeit einer Überprüfung auf Änderungen entfällt.</span><span class="sxs-lookup"><span data-stu-id="fb56a-129">These then report value changes on entity properties directly to EF Core, avoiding the need to scan for changes.</span></span>
<span data-ttu-id="fb56a-130">Für Proxys gelten jedoch bestimmte Einschränkungen, daher eignen sie sich nicht in jedem Fall.</span><span class="sxs-lookup"><span data-stu-id="fb56a-130">However, proxies come with their own set of limitations, so they are not for everyone.</span></span>

<span data-ttu-id="fb56a-131">Die Dokumentation finden Sie im Issue [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span><span class="sxs-lookup"><span data-stu-id="fb56a-131">Documentation is tracked by issue [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span></span>

### <a name="enhanced-debug-views"></a><span data-ttu-id="fb56a-132">Erweiterte Debugansichten</span><span class="sxs-lookup"><span data-stu-id="fb56a-132">Enhanced debug views</span></span>

<span data-ttu-id="fb56a-133">Debugansichten sind eine einfache Möglichkeit, beim Debuggen von Problemen interne Vorgänge in EF Core zu untersuchen.</span><span class="sxs-lookup"><span data-stu-id="fb56a-133">Debug views are an easy way to look at the internals of EF Core when debugging issues.</span></span>
<span data-ttu-id="fb56a-134">Ein Debugansicht für das Modell wurde bereits vor einiger Zeit implementiert.</span><span class="sxs-lookup"><span data-stu-id="fb56a-134">A debug view for the Model was implemented some time ago.</span></span>
<span data-ttu-id="fb56a-135">Für EF Core 5.0 haben wir die Modellansicht vereinfacht und im Zustands-Manager eine neue Debugansicht für nachverfolgte Entitäten hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="fb56a-135">For EF Core 5.0, we have made the model view easier to read and added a new debug view for tracked entities in the state manager.</span></span>

<span data-ttu-id="fb56a-136">Eine vorläufige Dokumentation ist im [Wöchentlichen Status von EF vom 12. Dezember 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206) (in englischer Sprache) enthalten.</span><span class="sxs-lookup"><span data-stu-id="fb56a-136">Preliminary documentation is included in the [EF weekly status for December 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span></span>

<span data-ttu-id="fb56a-137">Zusätzliche Dokumentation finden Sie im Issue [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span><span class="sxs-lookup"><span data-stu-id="fb56a-137">Additional documentation is tracked by issue [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span></span>

### <a name="improved-handling-of-database-null-semantics"></a><span data-ttu-id="fb56a-138">Verbesserte Verarbeitung von NULL-Semantik in der Datenbank</span><span class="sxs-lookup"><span data-stu-id="fb56a-138">Improved handling of database null semantics</span></span>

<span data-ttu-id="fb56a-139">Relationale Datenbanken behandeln NULL in der Regel als unbekannten Wert und daher als ungleich jedem anderen NULL-Wert.</span><span class="sxs-lookup"><span data-stu-id="fb56a-139">Relational databases typically treat NULL as an unknown value and therefore not equal to any other NULL.</span></span>
<span data-ttu-id="fb56a-140">C# dagegen behandelt NULL als definierten Wert, der gleich jedem anderen NULL-Wert sein kann.</span><span class="sxs-lookup"><span data-stu-id="fb56a-140">C#, on the other hand, treats null as a defined value which compares equal to any other null.</span></span>
<span data-ttu-id="fb56a-141">EF Core übersetzt Abfragen standardmäßig so, dass sie die NULL-Semantik von C# verwenden.</span><span class="sxs-lookup"><span data-stu-id="fb56a-141">EF Core by default translates queries so that they use C# null semantics.</span></span>
<span data-ttu-id="fb56a-142">In EF Core 5.0 wurde die Effizienz dieser Übersetzungen erheblich verbessert.</span><span class="sxs-lookup"><span data-stu-id="fb56a-142">EF Core 5.0 greatly improves the efficiency of these translations.</span></span>

<span data-ttu-id="fb56a-143">Die Dokumentation finden Sie im Issue [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span><span class="sxs-lookup"><span data-stu-id="fb56a-143">Documentation is tracked by issue [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span></span>

### <a name="indexer-properties"></a><span data-ttu-id="fb56a-144">Indexereigenschaften</span><span class="sxs-lookup"><span data-stu-id="fb56a-144">Indexer properties</span></span>

<span data-ttu-id="fb56a-145">EF Core 5.0 unterstützt die Zuordnung von C#-Indexereigenschaften.</span><span class="sxs-lookup"><span data-stu-id="fb56a-145">EF Core 5.0 supports mapping of C# indexer properties.</span></span>
<span data-ttu-id="fb56a-146">So können Entitäten als Eigenschaftenbehälter fungieren, wobei Spalten benannten Eigenschaften im Behälter zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="fb56a-146">This allows entities to act as property bags where columns are mapped to named properties in the bag.</span></span>

<span data-ttu-id="fb56a-147">Die Dokumentation finden Sie im Issue [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span><span class="sxs-lookup"><span data-stu-id="fb56a-147">Documentation is tracked by issue [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span></span>

### <a name="generation-of-check-constraints-for-enum-mappings"></a><span data-ttu-id="fb56a-148">Generieren von CHECK-Einschränkungen für Enumerationszuordnungen</span><span class="sxs-lookup"><span data-stu-id="fb56a-148">Generation of check constraints for enum mappings</span></span>

<span data-ttu-id="fb56a-149">EF Core 5.0-Migrationen können jetzt CHECK-Einschränkungen aus Zuordnungen der enum-Eigenschaft generieren.</span><span class="sxs-lookup"><span data-stu-id="fb56a-149">EF Core 5.0 Migrations can now generate CHECK constraints for enum property mappings.</span></span>
<span data-ttu-id="fb56a-150">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fb56a-150">For example:</span></span>

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN('Useful', 'Useless', 'Unknown'))
```

<span data-ttu-id="fb56a-151">Die Dokumentation finden Sie im Issue [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span><span class="sxs-lookup"><span data-stu-id="fb56a-151">Documentation is tracked by issue [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span></span>

### <a name="isrelational"></a><span data-ttu-id="fb56a-152">IsRelational</span><span class="sxs-lookup"><span data-stu-id="fb56a-152">IsRelational</span></span>

<span data-ttu-id="fb56a-153">Zusätzlich zu den existierenden `IsSqlServer`-, `IsSqlite`- und `IsInMemory`-Methoden wurde eine neue `IsRelational`-Methode hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="fb56a-153">A new `IsRelational` method has been added in addition to the existing `IsSqlServer`, `IsSqlite`, and `IsInMemory`.</span></span>
<span data-ttu-id="fb56a-154">Diese kann verwendet werden, um zu testen, ob DbContext einen Anbieter einer relationalen Datenbank verwendet.</span><span class="sxs-lookup"><span data-stu-id="fb56a-154">This can be used to test if the DbContext is using any relational database provider.</span></span>
<span data-ttu-id="fb56a-155">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fb56a-155">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

<span data-ttu-id="fb56a-156">Die Dokumentation finden Sie im Issue [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span><span class="sxs-lookup"><span data-stu-id="fb56a-156">Documentation is tracked by issue [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span></span>

### <a name="cosmos-optimistic-concurrency-with-etags"></a><span data-ttu-id="fb56a-157">Optimistische Nebenläufigkeit in Cosmos mit Entitätstags (ETags)</span><span class="sxs-lookup"><span data-stu-id="fb56a-157">Cosmos optimistic concurrency with ETags</span></span>

<span data-ttu-id="fb56a-158">Der Azure Cosmos DB-Datenbankanbieter unterstützt nun optimistische Nebenläufigkeit mithilfe von ETags.</span><span class="sxs-lookup"><span data-stu-id="fb56a-158">The Azure Cosmos DB database provider now supports optimistic concurrency using ETags.</span></span>
<span data-ttu-id="fb56a-159">Verwenden Sie das Modellerstellungstool in OnModelCreating, um ein ETag zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="fb56a-159">Use the model builder in OnModelCreating to configure an ETag:</span></span>

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

<span data-ttu-id="fb56a-160">SaveChanges löst dann eine `DbUpdateConcurrencyException`-Ausnahme für einen Nebenläufigkeitskonflikt aus, die [verarbeitet werden kann](https://docs.microsoft.com/ef/core/saving/concurrency), z. B. um Wiederholungsversuche zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="fb56a-160">SaveChanges will then throw an `DbUpdateConcurrencyException` on a concurrency conflict, which [can be handled](https://docs.microsoft.com/ef/core/saving/concurrency) to implement retries, etc.</span></span>


<span data-ttu-id="fb56a-161">Die Dokumentation finden Sie im Issue [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span><span class="sxs-lookup"><span data-stu-id="fb56a-161">Documentation is tracked by issue [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span></span>

### <a name="query-translations-for-more-datetime-constructs"></a><span data-ttu-id="fb56a-162">Abfrageübersetzungen für weitere DateTime-Konstrukte</span><span class="sxs-lookup"><span data-stu-id="fb56a-162">Query translations for more DateTime constructs</span></span>

<span data-ttu-id="fb56a-163">Abfragen mit neuer DateTime-Konstruktion werden jetzt übersetzt.</span><span class="sxs-lookup"><span data-stu-id="fb56a-163">Queries containing new DateTime construction are now translated.</span></span>

<span data-ttu-id="fb56a-164">Außerdem werden nun die folgenden SQL Server-Funktionen zugeordnet:</span><span class="sxs-lookup"><span data-stu-id="fb56a-164">In addition, the following SQL Server functions are now mapped:</span></span>
* <span data-ttu-id="fb56a-165">DateDiffWeek</span><span class="sxs-lookup"><span data-stu-id="fb56a-165">DateDiffWeek</span></span>
* <span data-ttu-id="fb56a-166">DateFromParts</span><span class="sxs-lookup"><span data-stu-id="fb56a-166">DateFromParts</span></span>

<span data-ttu-id="fb56a-167">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fb56a-167">For example:</span></span>

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

<span data-ttu-id="fb56a-168">Die Dokumentation finden Sie im Issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="fb56a-168">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translations-for-more-byte-array-constructs"></a><span data-ttu-id="fb56a-169">Abfrageübersetzungen für weitere Bytearraykonstrukte</span><span class="sxs-lookup"><span data-stu-id="fb56a-169">Query translations for more byte array constructs</span></span>

<span data-ttu-id="fb56a-170">Abfragen, die Contains, Length, SequenceEqual usw. in byte[]-Eigenschaften verwenden, werden jetzt in SQL übersetzt.</span><span class="sxs-lookup"><span data-stu-id="fb56a-170">Queries using Contains, Length, SequenceEqual, etc. on byte[] properties are now translated to SQL.</span></span>

<span data-ttu-id="fb56a-171">Eine vorläufige Dokumentation ist im [Wöchentlichen Status von EF vom 5. Dezember 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863) (in englischer Sprache) enthalten.</span><span class="sxs-lookup"><span data-stu-id="fb56a-171">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="fb56a-172">Zusätzliche Dokumentation finden Sie im Issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="fb56a-172">Additional documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-reverse"></a><span data-ttu-id="fb56a-173">Abfrageübersetzung für Reverse</span><span class="sxs-lookup"><span data-stu-id="fb56a-173">Query translation for Reverse</span></span>

<span data-ttu-id="fb56a-174">Abfragen, die `Reverse` verwenden, werden jetzt übersetzt.</span><span class="sxs-lookup"><span data-stu-id="fb56a-174">Queries using `Reverse` are now translated.</span></span>
<span data-ttu-id="fb56a-175">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fb56a-175">For example:</span></span>

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

<span data-ttu-id="fb56a-176">Die Dokumentation finden Sie im Issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="fb56a-176">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-bitwise-operators"></a><span data-ttu-id="fb56a-177">Abfrageübersetzung für bitweise Operatoren</span><span class="sxs-lookup"><span data-stu-id="fb56a-177">Query translation for bitwise operators</span></span>

<span data-ttu-id="fb56a-178">Abfragen, die bitweise Operatoren verwenden, werden jetzt in mehr Fällen übersetzt. Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fb56a-178">Queries using bitwise operators are now translated in more cases For example:</span></span>

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

<span data-ttu-id="fb56a-179">Die Dokumentation finden Sie im Issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="fb56a-179">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-strings-on-cosmos"></a><span data-ttu-id="fb56a-180">Abfrageübersetzung für Zeichenfolgen in Cosmos</span><span class="sxs-lookup"><span data-stu-id="fb56a-180">Query translation for strings on Cosmos</span></span>

<span data-ttu-id="fb56a-181">Abfragen, die die Zeichenfolgenmethoden Contains, StartsWith und EndsWith verwenden, werden jetzt übersetzt, wenn der Azure Cosmos DB-Anbieter verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="fb56a-181">Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="fb56a-182">Die Dokumentation finden Sie im Issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="fb56a-182">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>
