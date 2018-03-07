---
title: "Neue Features in EF Core 2.1 – EF Core"
author: divega
ms.author: divega
ms.date: 2/20/2018
ms.assetid: 585F90A3-4D5A-4DD1-92D8-5243B14E0FEC
ms.technology: entity-framework-core
uid: core/what-is-new/ef-core-2.1
ms.openlocfilehash: 1e5e9839bae1e5da4082d90c02d098bb3b2b43bd
ms.sourcegitcommit: 4b7d3d3e258b0d9cb778bb45a9f4a33c0792e38e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="new-features-in-ef-core-21"></a><span data-ttu-id="1e485-102">Neue Features in EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="1e485-102">New features in EF Core 2.1</span></span>
> [!NOTE]  
> <span data-ttu-id="1e485-103">Dieses Release befindet sich weiterhin in der Vorschauversion.</span><span class="sxs-lookup"><span data-stu-id="1e485-103">This release is still in preview.</span></span>

<span data-ttu-id="1e485-104">Neben zahlreichen geringfügigen Verbesserungen und über 100 Fehlerbehebungen weist EF Core 2.1 mehrere neue Features auf:</span><span class="sxs-lookup"><span data-stu-id="1e485-104">Besides numerous small improvements and more than a hundred product bug fixes, EF Core 2.1 includes several new features:</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="1e485-105">Lazy Loading</span><span class="sxs-lookup"><span data-stu-id="1e485-105">Lazy loading</span></span>
<span data-ttu-id="1e485-106">EF Core enthält nun die erforderlichen Bausteine für Benutzer, die Entitätsklassen zum bedarfsgesteuerten Laden ihrer Navigationseigenschaften erstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="1e485-106">EF Core now contains the necessary building blocks for anyone to author entity classes that can load their navigation properties on demand.</span></span> <span data-ttu-id="1e485-107">Wir haben zudem das neue Paket „Microsoft.EntityFrameworkCore.Proxies“ erstellt, das mithilfe dieser Bausteine Proxyklassen zum Lazy Loading auf Grundlage minimal geänderter Entitätsklassen (z.B. Klassen mit virtuellen Navigationseigenschaften) erzeugt.</span><span class="sxs-lookup"><span data-stu-id="1e485-107">We have also created a new package, Microsoft.EntityFrameworkCore.Proxies, that leverages those building blocks to produce lazy loading proxy classes based on minimally modified entity classes (e.g. classes with virtual navigation properties).</span></span>

<span data-ttu-id="1e485-108">Weitere Informationen zu diesem Thema finden Sie im [Abschnitt zum Lazy Loading](xref:core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="1e485-108">Read the [section on lazy loading](xref:core/querying/related-data#lazy-loading) for more information about this topic.</span></span>

## <a name="parameters-in-entity-constructors"></a><span data-ttu-id="1e485-109">Parameter in Entitätskonstruktoren</span><span class="sxs-lookup"><span data-stu-id="1e485-109">Parameters in entity constructors</span></span>
<span data-ttu-id="1e485-110">Als einer der erforderlichen Bausteine zum Lazy Loading haben wir die Erstellung von Entitäten ermöglicht, die Parameter in ihren Konstruktoren akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="1e485-110">As one of the required building blocks for lazy loading, we enabled the creation of entities that take parameters in their constructors.</span></span> <span data-ttu-id="1e485-111">Sie können mithilfe von Parametern Eigenschaftswerte, Delegaten zum Lazy Loading und Dienste einfügen.</span><span class="sxs-lookup"><span data-stu-id="1e485-111">You can use parameters to inject property values, lazy loading delegates, and services.</span></span>

<span data-ttu-id="1e485-112">Weitere Informationen zu diesem Thema finden Sie im [Abschnitt zum Entitätskonstruktor mit Parametern](xref:core/modeling/constructors).</span><span class="sxs-lookup"><span data-stu-id="1e485-112">Read the [section on entity constructor with parameters](xref:core/modeling/constructors) for more information about this topic.</span></span>

## <a name="value-conversions"></a><span data-ttu-id="1e485-113">Wertkonvertierungen</span><span class="sxs-lookup"><span data-stu-id="1e485-113">Value conversions</span></span>
<span data-ttu-id="1e485-114">Bislang konnte EF Core nur Eigenschaften von Typen zuordnen, die vom zugrunde liegenden Datenbankanbieter nativ unterstützt wurden.</span><span class="sxs-lookup"><span data-stu-id="1e485-114">Until now, EF Core could only map properties of types natively supported by the underlying database provider.</span></span> <span data-ttu-id="1e485-115">Werte wurden zwischen Spalten und Eigenschaften ohne jegliche Transformation hin und her kopiert.</span><span class="sxs-lookup"><span data-stu-id="1e485-115">Values were copied back and forth between columns and properties without any transformation.</span></span> <span data-ttu-id="1e485-116">Ab EF Core 2.1 können Wertkonvertierungen angewendet werden, um die aus Spalten abgerufenen Werte vor der Anwendung auf Eigenschaften zu transformieren (und umgekehrt).</span><span class="sxs-lookup"><span data-stu-id="1e485-116">Starting with EF Core 2.1, value conversions can be applied to transform the values obtained from columns before they are applied to properties, and vice versa.</span></span> <span data-ttu-id="1e485-117">Wir verfügen über eine Reihe von Konvertierungen, die der Konvention entsprechend bei Bedarf angewendet werden können, sowie über eine explizite Konfigurations-API, die die Registrierung von benutzerdefinierten Konvertierungen zwischen Spalten und Eigenschaften ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="1e485-117">We have a number of conversions that can be applied by convention as necessary, as well as an explicit configuration API that allows registering custom conversions between columns and properties.</span></span> <span data-ttu-id="1e485-118">Einige Anwendungsgebiete für dieses Feature sind z.B. Folgende:</span><span class="sxs-lookup"><span data-stu-id="1e485-118">Some of the application of this feature are:</span></span>

- <span data-ttu-id="1e485-119">Speichern von Enumerationen als Zeichenfolgen</span><span class="sxs-lookup"><span data-stu-id="1e485-119">Storing enums as strings</span></span>
- <span data-ttu-id="1e485-120">Zuordnen von ganzen Zahlen ohne Vorzeichen mit SQL Server</span><span class="sxs-lookup"><span data-stu-id="1e485-120">Mapping unsigned integers with SQL Server</span></span>
- <span data-ttu-id="1e485-121">Automatische Ver- und Entschlüsselung von Eigenschaftswerten</span><span class="sxs-lookup"><span data-stu-id="1e485-121">Automatic encryption and decryption of property values</span></span>

<span data-ttu-id="1e485-122">Weitere Informationen zu diesem Thema finden Sie im [Abschnitt zu Wertkonvertierungen](xref:core/modeling/value-conversions).</span><span class="sxs-lookup"><span data-stu-id="1e485-122">Read the [section on value conversions](xref:core/modeling/value-conversions) for more information about this topic.</span></span>  

## <a name="linq-groupby-translation"></a><span data-ttu-id="1e485-123">LINQ-GroupBy-Übersetzung</span><span class="sxs-lookup"><span data-stu-id="1e485-123">LINQ GroupBy translation</span></span>
<span data-ttu-id="1e485-124">Vor Version 2.1 wurde der GroupBy-LINQ-Operator in EF Core immer in der In-Memory Database ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="1e485-124">Before version 2.1, in EF Core the GroupBy LINQ operator was always be evaluated in memory.</span></span> <span data-ttu-id="1e485-125">Nun bieten wir in den meisten Fällen Unterstützung für dessen Übersetzung in die GROUP BY-SQL-Klausel.</span><span class="sxs-lookup"><span data-stu-id="1e485-125">We now support translating it to the SQL GROUP BY clause in most common cases.</span></span>

<span data-ttu-id="1e485-126">Dieses Beispiel zeigt eine Abfrage mit GroupBy zum Berechnen verschiedener Aggregatfunktionen:</span><span class="sxs-lookup"><span data-stu-id="1e485-126">This example shows a query with GroupBy used to compute various aggregate functions:</span></span>

``` csharp
var query = context.Orders
    .GroupBy(o => new { o.CustomerId, o.EmployeeId })
    .Select(g => new
        {
          g.Key.CustomerId,
          g.Key.EmployeeId,
          Sum = g.Sum(o => o.Amount),
          Min = g.Min(o => o.Amount),
          Max = g.Max(o => o.Amount),
          Avg = g.Average(o => Amount)
        });
```

<span data-ttu-id="1e485-127">Die entsprechende SQL-Übersetzung sieht wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="1e485-127">The corresponding SQL translation looks like this:</span></span>

``` SQL
SELECT [o].[CustomerId], [o].[EmployeeId],
    SUM([o].[Amount]), MIN([o].[Amount]), MAX([o].[Amount]), AVG([o].[Amount])
FROM [Orders] AS [o]
GROUP BY [o].[CustomerId], [o].[EmployeeId];
```

## <a name="data-seeding"></a><span data-ttu-id="1e485-128">Datenseeding</span><span class="sxs-lookup"><span data-stu-id="1e485-128">Data Seeding</span></span>
<span data-ttu-id="1e485-129">Mit dem neuen Release ist es möglich, die ursprünglichen Daten zum Auffüllen einer Datenbank anzugeben.</span><span class="sxs-lookup"><span data-stu-id="1e485-129">With the new release it will be possible to provide initial data to populate a database.</span></span> <span data-ttu-id="1e485-130">Im Gegensatz zu EF 6 wird das Datenseeding mit einem Entitätstyp als Teil der Modellkonfiguration verknüpft.</span><span class="sxs-lookup"><span data-stu-id="1e485-130">Unlike in EF6, seeding data is associated to an entity type as part of the model configuration.</span></span> <span data-ttu-id="1e485-131">EF Core-Migrationen können dann automatisch berechnen, welche Einfüge-, Aktualisierungs- oder Löschvorgänge beim Durchführen eines Upgrades für die Datenbank auf eine neue Modellversion angewendet werden müssen.</span><span class="sxs-lookup"><span data-stu-id="1e485-131">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="1e485-132">Beispielsweise können Sie damit Seeddaten für ein Post-Element in `OnModelCreating` konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="1e485-132">As an example, you can use this to configure seed data for a Post in `OnModelCreating`:</span></span>

``` csharp
modelBuilder.Entity<Post>().SeedData(new Post{ Id = 1, Text = "Hello World!" });
```

<span data-ttu-id="1e485-133">Weitere Informationen zu diesem Thema finden Sie im [Abschnitt zum Datenseeding](xref:core/modeling/data-seeding).</span><span class="sxs-lookup"><span data-stu-id="1e485-133">Read the [section on data seeding](xref:core/modeling/data-seeding) for more information about this topic.</span></span>  

## <a name="query-types"></a><span data-ttu-id="1e485-134">Abfragetypen</span><span class="sxs-lookup"><span data-stu-id="1e485-134">Query types</span></span>
<span data-ttu-id="1e485-135">Ein EF Core-Modell kann nun Abfragetypen einschließen.</span><span class="sxs-lookup"><span data-stu-id="1e485-135">An EF Core model can now include query types.</span></span> <span data-ttu-id="1e485-136">Im Gegensatz zu Entitätstypen werden für Abfragetypen keine Schlüssel definiert, und sie können nicht eingefügt, gelöscht oder aktualisiert werden (d.h., sie sind schreibgeschützt). Sie können jedoch direkt von Abfragen zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="1e485-136">Unlike entity types, query types do not have keys defined on them and cannot be inserted, deleted or updated (i.e. they are read-only), but they can be returned directly by queries.</span></span> <span data-ttu-id="1e485-137">Einige Verwendungsszenarien für Abfragetypen sind z.B. Folgende:</span><span class="sxs-lookup"><span data-stu-id="1e485-137">Some of the usage scenarios for query types are:</span></span>

- <span data-ttu-id="1e485-138">Zuordnen mit Ansichten ohne Primärschlüssel</span><span class="sxs-lookup"><span data-stu-id="1e485-138">Mapping to views without primary keys</span></span>
- <span data-ttu-id="1e485-139">Zuordnen mit Tabellen ohne Primärschlüssel</span><span class="sxs-lookup"><span data-stu-id="1e485-139">Mapping to tables without primary keys</span></span>
- <span data-ttu-id="1e485-140">Zuordnen mit im Modell definierten Abfragen</span><span class="sxs-lookup"><span data-stu-id="1e485-140">Mapping to queries defined in the model</span></span>
- <span data-ttu-id="1e485-141">Verwendung als Rückgabetyp für `FromSql()`-Abfragen</span><span class="sxs-lookup"><span data-stu-id="1e485-141">Serving as the return type for `FromSql()` queries</span></span>

<span data-ttu-id="1e485-142">Weitere Informationen zu diesem Thema finden Sie im [Abschnitt zu Abfragetypen](xref:core/modeling/query-types).</span><span class="sxs-lookup"><span data-stu-id="1e485-142">Read the [section on query types](xref:core/modeling/query-types) for more information about this topic.</span></span>

## <a name="include-for-derived-types"></a><span data-ttu-id="1e485-143">Einschließen für abgeleitete Typen</span><span class="sxs-lookup"><span data-stu-id="1e485-143">Include for derived types</span></span>
<span data-ttu-id="1e485-144">Nun ist es möglich, beim Schreiben von Ausdrücken für die `Include`-Methode nur für abgeleitete Typen definierte Navigationseigenschaften anzugeben.</span><span class="sxs-lookup"><span data-stu-id="1e485-144">It will be now possible to specify navigation properties only defined on derived types when writing expressions for the `Include` method.</span></span> <span data-ttu-id="1e485-145">Für die stark typisierte `Include`-Version unterstützen wir entweder die Verwendung einer expliziten Umwandlung oder des `as`-Operators.</span><span class="sxs-lookup"><span data-stu-id="1e485-145">For the strongly typed version of `Include`, we support using either an explicit cast or the `as` operator.</span></span> <span data-ttu-id="1e485-146">Nun bieten wir auch Unterstützung für den Verweis der für abgeleitete Typen definierten Namen der Navigationseigenschaften in der Zeichenfolgenversion `Include`:</span><span class="sxs-lookup"><span data-stu-id="1e485-146">We also now support referencing the names of navigation property defined on derived types in the string version of `Include`:</span></span>

``` csharp
var option1 = context.People.Include(p => ((Student)p).School);
var option2 = context.People.Include(p => (p as Student).School);
var option3 = context.People.Include("School");
```

<span data-ttu-id="1e485-147">Weitere Informationen zu diesem Thema finden Sie im [Abschnitt zum Einschließen mit abgeleiteten Typen](xref:core/querying/related-data#include-on-derived-types).</span><span class="sxs-lookup"><span data-stu-id="1e485-147">Read the [section on Include with derived types](xref:core/querying/related-data#include-on-derived-types) for more information about this topic.</span></span>

## <a name="systemtransactions-support"></a><span data-ttu-id="1e485-148">System.Transactions-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="1e485-148">System.Transactions support</span></span>
<span data-ttu-id="1e485-149">Wir haben die Möglichkeit zum Arbeiten mit System.Transactions-Features wie TransactionScope hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="1e485-149">We have added the ability to work with System.Transactions features such as TransactionScope.</span></span> <span data-ttu-id="1e485-150">Dieses funktioniert in .NET Framework und .NET Core bei der Verwendung von Datenbankanbietern, die Unterstützung hierfür bieten.</span><span class="sxs-lookup"><span data-stu-id="1e485-150">This will work on both .NET Framework and .NET Core when using database providers that support it.</span></span>

<span data-ttu-id="1e485-151">Weitere Informationen zu diesem Thema finden Sie im [Abschnitt zu System.Transactions](xref:core/saving/transactions#using-systemtransactions).</span><span class="sxs-lookup"><span data-stu-id="1e485-151">Read the [section on System.Transactions](xref:core/saving/transactions#using-systemtransactions) for more information about this topic.</span></span>

## <a name="better-column-ordering-in-initial-migration"></a><span data-ttu-id="1e485-152">Bessere Spaltensortierung bei der anfänglichen Migration</span><span class="sxs-lookup"><span data-stu-id="1e485-152">Better column ordering in initial migration</span></span>
<span data-ttu-id="1e485-153">Basierend auf dem Feedback von Kunden haben wir Migrationen dahingehend aktualisiert, dass Spalten für Tabellen zunächst in der gleichen Reihenfolge wie die Deklaration von Eigenschaften in Klassen generiert werden.</span><span class="sxs-lookup"><span data-stu-id="1e485-153">Based on customer feedback, we have updated migrations to initially generate columns for tables in the same order as properties are declared in classes.</span></span> <span data-ttu-id="1e485-154">Beachten Sie, dass EF Core nicht die Reihenfolge ändern kann, wenn nach der Erstellung der ersten Tabelle neue Member hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="1e485-154">Note that EF Core cannot change order when new members are added after the initial table creation.</span></span>

## <a name="optimization-of-correlated-subqueries"></a><span data-ttu-id="1e485-155">Optimierung von korrelierten Unterabfragen</span><span class="sxs-lookup"><span data-stu-id="1e485-155">Optimization of correlated subqueries</span></span>
<span data-ttu-id="1e485-156">Wir haben unsere Abfrageübersetzung dahingehend verbessert, dass die Ausführung von „N+1“-SQL-Abfragen in vielen gängigen Szenarien verhindert wird, in denen die Verwendung einer Navigationseigenschaft in der Projektion zum Verknüpfen von Daten aus der Stammabfrage mit Daten aus einer korrelierten Unterabfrage führt.</span><span class="sxs-lookup"><span data-stu-id="1e485-156">We have improved our query translation to avoid executing "N + 1" SQL queries in many common scenarios in which the usage of a navigation property in the projection leads to joining data from the root query with data from a correlated subquery.</span></span> <span data-ttu-id="1e485-157">Für die Optimierung ist das Puffern der Ergebnisse aus der Unterabfrage erforderlich. Zudem fordern wir Sie dazu auf, die Abfrage zum Auswählen des neuen Verhaltens zu ändern.</span><span class="sxs-lookup"><span data-stu-id="1e485-157">The optimization requires buffering the results form the subquery, and we require that you modify the query to opt-in the new behavior.</span></span>

<span data-ttu-id="1e485-158">Die folgende Abfrage wird z.B. normalerweise in eine Abfrage für Kunden plus N separate Abfragen für Aufträge übersetzt („N“ steht hierbei für die Anzahl der zurückgegebenen Kunden):</span><span class="sxs-lookup"><span data-stu-id="1e485-158">As an example, the following query normally gets translated into one query for Customers, plus N (where "N" is the number of customers returned) separate queries for Orders:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount));
```

<span data-ttu-id="1e485-159">Durch Einschließen von `ToList()` an der richtigen Stelle geben Sie an, dass eine Pufferung für die Aufträge, die die Optimierung ermöglichen, angemessen ist:</span><span class="sxs-lookup"><span data-stu-id="1e485-159">By including `ToList()` in the right place, you indicate that buffering is appropriate for the Orders, which enable the optimization:</span></span>

``` csharp
var query = context.Customers.Select(
    c => c.Orders.Where(o => o.Amount  > 100).Select(o => o.Amount).ToList());
```

<span data-ttu-id="1e485-160">Beachten Sie, dass diese Abfrage nur in zwei SQL-Abfragen übersetzt wird: eine für Kunden und die nächste für Aufträge.</span><span class="sxs-lookup"><span data-stu-id="1e485-160">Note that this query will be translated to only two SQL queries: One for Customers and the next one for Orders.</span></span>

### <a name="ownedattribute"></a><span data-ttu-id="1e485-161">OwnedAttribute</span><span class="sxs-lookup"><span data-stu-id="1e485-161">OwnedAttribute</span></span>

<span data-ttu-id="1e485-162">Nun können [Entitätstypen im Besitz](xref:core/modeling/owned-entities) konfiguriert werden, indem einfach der Typ mit `[Owned]` kommentiert und dann sichergestellt wird, dass die Besitzerentität zum Modell hinzugefügt wird:</span><span class="sxs-lookup"><span data-stu-id="1e485-162">It is now possible to configure [owned entity types](xref:core/modeling/owned-entities) by simply annotating the type with `[Owned]` and then making sure the owner entity is added to the model:</span></span>

``` csharp
[Owned]
public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}
```

## <a name="database-provider-compatibility"></a><span data-ttu-id="1e485-163">Kompatibilität mit Datenbankanbietern</span><span class="sxs-lookup"><span data-stu-id="1e485-163">Database provider compatibility</span></span>

<span data-ttu-id="1e485-164">EF Core 2.1 wurde zur Bereitstellung von Kompatibilität mit Datenbankanbietern für EF Core 2.0 entwickelt.</span><span class="sxs-lookup"><span data-stu-id="1e485-164">EF Core 2.1 was designed to be compatible with database providers created for EF Core 2.0.</span></span> <span data-ttu-id="1e485-165">Während einige der oben beschriebenen Features (z.B. Wertkonvertierungen) einen aktualisierten Anbieter erfordern, sind andere Feature (z.B. das Lazy Loading) mit den vorhandenen Anbietern kompatibel.</span><span class="sxs-lookup"><span data-stu-id="1e485-165">While some of the features described above (e.g. value conversions) require an updated provider others (e.g. lazy loading) will light up with existing providers.</span></span>

> [!TIP]
> <span data-ttu-id="1e485-166">Falls unerwarteterweise keine Kompatibilität besteht oder Probleme mit den neuen Features auftreten, oder falls Sie uns Ihre Feedback mitteilen möchten, melden Sie uns dies über [unsere Problemverfolgung](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span><span class="sxs-lookup"><span data-stu-id="1e485-166">If you find any unexpected incompatibility or any issue in the new features, or if you have feedback on them, please report it using [our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues/new).</span></span>
