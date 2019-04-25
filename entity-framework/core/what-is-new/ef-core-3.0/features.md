---
title: Neue Features in EF Core 3.0
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: 7501a806271c9734e85e31845f260f2d512da077
ms.sourcegitcommit: 5280dcac4423acad8b440143433459b18886115b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "58867956"
---
# <a name="new-features-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="d7618-102">Neue Features in EF Core 3.0 (aktuell in der Vorschauversion)</span><span class="sxs-lookup"><span data-stu-id="d7618-102">New features included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d7618-103">Bitte beachten Sie, dass die Featuregruppen und Zeitpläne für künftige Releases jederzeit geändert werden können. Obwohl diese Seite bestmöglich aktualisiert wird, entspricht sie nicht immer den neuesten Plänen.</span><span class="sxs-lookup"><span data-stu-id="d7618-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="d7618-104">Die folgende Liste enthält die wichtigsten neuen Features, die für EF Core 3.0 geplant sind.</span><span class="sxs-lookup"><span data-stu-id="d7618-104">The following list includes the major new features planned for EF Core 3.0.</span></span>
<span data-ttu-id="d7618-105">Die meisten davon sind aktuell nicht in der Vorschauversion enthalten und werden im Verlauf der RTM-Vorbereitung zur Verfügung gestellt.</span><span class="sxs-lookup"><span data-stu-id="d7618-105">Most of these features are not included in the current preview, but will become available as we make progress towards RTM.</span></span>

<span data-ttu-id="d7618-106">Der Grund dafür ist, dass zu Releasebeginn zuerst die geplanten [Breaking Changes](xref:core/what-is-new/ef-core-3.0/breaking-changes) implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="d7618-106">The reason is that at the beginning of the release we are focusing on implementing planned [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes).</span></span>
<span data-ttu-id="d7618-107">Allein durch viele dieser Änderungen wird EF Core bereits deutlich verbessert.</span><span class="sxs-lookup"><span data-stu-id="d7618-107">Many of these breaking changes are improvements to EF Core on their own.</span></span>
<span data-ttu-id="d7618-108">Für weitere Optimierungen werden zusätzliche Breaking Changes erforderlich sein.</span><span class="sxs-lookup"><span data-stu-id="d7618-108">Many others are required to unblock further improvements.</span></span> 

<span data-ttu-id="d7618-109">Eine vollständige Liste der aktuellen Fehlerbehebungen und Verbesserungen finden Sie [mit der entsprechenden Abfrage in unserem Issuetracker](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="d7618-109">For a complete list of bug fixes and enhancements underway, you can see [this query in our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).</span></span>

## <a name="linq-improvements"></a><span data-ttu-id="d7618-110">LINQ-Verbesserungen</span><span class="sxs-lookup"><span data-stu-id="d7618-110">LINQ improvements</span></span> 

[<span data-ttu-id="d7618-111">Issue #12795</span><span class="sxs-lookup"><span data-stu-id="d7618-111">Tracking Issue #12795</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

<span data-ttu-id="d7618-112">Dieses Feature befindet sich noch in der Entwicklungsphase und ist nicht in der aktuellen Vorschauversion enthalten.</span><span class="sxs-lookup"><span data-stu-id="d7618-112">Work on this feature has started but it isn't included in the current preview.</span></span>

<span data-ttu-id="d7618-113">LINQ ermöglicht es Ihnen, Datenbankabfragen zu schreiben, ohne die Sprache Ihrer Wahl zu verlassen. Gleichzeitig werden Rich-Type-Informationen verwendet, um von IntelliSense und der Typprüfung zur Kompilierungszeit zu profitieren.</span><span class="sxs-lookup"><span data-stu-id="d7618-113">LINQ enables you to write database queries without leaving your language of choice, taking advantage of rich type information to get IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="d7618-114">Aber mit LINQ können Sie auch eine unbegrenzte Anzahl komplizierter Abfragen schreiben, und dies war schon immer eine große Herausforderung für LINQ-Anbieter.</span><span class="sxs-lookup"><span data-stu-id="d7618-114">But LINQ also enables you to write an unlimited number of complicated queries, and that has always been a huge challenge for LINQ providers.</span></span>
<span data-ttu-id="d7618-115">In den ersten Versionen von EF Core wurde dies teilweise gelöst, indem wir herausgefunden haben, welche Teile einer Abfrage in SQL übersetzt werden konnten, und dann den Rest der Abfrage im Speicher des Clients ausführen ließen.</span><span class="sxs-lookup"><span data-stu-id="d7618-115">In the first few versions of EF Core, we solved that in part by figuring out what portions of a query could be translated to SQL, and then by allowing the rest of the query to execute in memory on the client.</span></span>
<span data-ttu-id="d7618-116">Diese clientseitige Ausführung kann in einigen Situationen wünschenswert sein, aber in vielen anderen Fällen kann sie zu ineffizienten Abfragen führen, die möglicherweise erst nach der Bereitstellung einer Anwendung in der Produktion identifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="d7618-116">This client-side execution can be desirable in some situations, but in many other cases it can result in inefficient queries that may not be identified until an application is deployed to production.</span></span>
<span data-ttu-id="d7618-117">In EF Core 3.0 planen wir, tiefgreifende Änderungen an der Funktionsweise unserer LINQ-Implementierung und der Art ihrer Überprüfung vorzunehmen.</span><span class="sxs-lookup"><span data-stu-id="d7618-117">In EF Core 3.0, we're planning to make profound changes to how our LINQ implementation works, and how we test it.</span></span>
<span data-ttu-id="d7618-118">Die Ziele bestehen darin, die Implementierung robuster zu machen (z.B., um Breaking Changes in Patchreleases zu vermeiden), mehr Ausdrücke korrekt in SQL übersetzen zu können, in mehr Fällen effiziente Abfragen zu erzeugen und zu verhindern, dass ineffiziente Abfragen unentdeckt bleiben.</span><span class="sxs-lookup"><span data-stu-id="d7618-118">The goals are to make it more robust (for example, to avoid breaking queries in patch releases), to enable translating more expressions correctly into SQL, to generate efficient queries in more cases, and to prevent inefficient queries from going undetected.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="d7618-119">Cosmos DB-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="d7618-119">Cosmos DB support</span></span> 

[<span data-ttu-id="d7618-120">Issue #8443</span><span class="sxs-lookup"><span data-stu-id="d7618-120">Tracking Issue #8443</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/8443)

<span data-ttu-id="d7618-121">Dieses Feature ist in der aktuellen Vorschauversion enthalten. Die Arbeit daran ist aber noch nicht abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="d7618-121">This feature is included in the current preview, but isn't complete yet.</span></span> 

<span data-ttu-id="d7618-122">Wir arbeiten an einem Cosmos DB-Anbieter für EF Core, damit Entwickler, die mit dem EF-Programmiermodell vertraut sind, Azure Cosmos DB problemlos als Anwendungsdatenbank einsetzen können.</span><span class="sxs-lookup"><span data-stu-id="d7618-122">We're working on a Cosmos DB provider for EF Core, to enable developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span>
<span data-ttu-id="d7618-123">Ziel ist es, einige der Vorteile von Cosmos DB – beispielsweise die globale Verteilung, Always On-Verfügbarkeit, elastische Skalierbarkeit und geringe Latenz – noch besser für .NET-Entwickler zugänglich zu machen.</span><span class="sxs-lookup"><span data-stu-id="d7618-123">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span>
<span data-ttu-id="d7618-124">Der Anbieter wird die meisten EF Core-Features unterstützen, darunter die automatische Änderungsnachverfolgung, LINQ und Wertekonvertierungen anhand der SQL-API in Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="d7618-124">The provider will enable most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>
<span data-ttu-id="d7618-125">Diese Bemühungen begannen vor EF Core 2.2, und [wir haben einige Vorschauversionen des Anbieters zur Verfügung gestellt](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).</span><span class="sxs-lookup"><span data-stu-id="d7618-125">We started this effort before EF Core 2.2, and [we have made some preview versions of the provider available](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).</span></span>
<span data-ttu-id="d7618-126">Der neue Plan sieht vor, den Anbieter neben EF Core 3.0 weiterzuentwickeln.</span><span class="sxs-lookup"><span data-stu-id="d7618-126">The new plan is to continue developing the provider alongside EF Core 3.0.</span></span> 

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="d7618-127">Abhängige Entitäten, die die Tabelle gemeinsam mit dem Prinzipal verwenden, sind jetzt optional</span><span class="sxs-lookup"><span data-stu-id="d7618-127">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="d7618-128">Issue #9005</span><span class="sxs-lookup"><span data-stu-id="d7618-128">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="d7618-129">Dieses Feature wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="d7618-129">This feature will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d7618-130">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="d7618-130">Consider the following model:</span></span>
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

<span data-ttu-id="d7618-131">Ab EF Core 3.0 gibt es die Möglichkeit, wenn `OrderDetails` im Besitz von `Order` ist oder explizit derselben Tabelle zugeordnet ist, dass `Order` ohne `OrderDetails` hinzugefügt werden kann, und alle `OrderDetails`-Eigenschaften, mit Ausnahme des primären Schlüssels, werden Spalten zugeordnet, die NULL-Werte zulassen.</span><span class="sxs-lookup"><span data-stu-id="d7618-131">Starting with EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table it will be possible to add an `Order` without an `OrderDetails` and all of the `OrderDetails` properties except the primary key will be mapped to nullable columns.</span></span>
<span data-ttu-id="d7618-132">Bei Abfragen von EF Core wird `OrderDetails` auf `null` festgelegt, wenn eine der erforderlichen Eigenschaften keinen Wert aufweist oder keine erforderlichen Eigenschaften außer dem primären Schlüssel vorhanden und alle Eigenschaften `null` sind.</span><span class="sxs-lookup"><span data-stu-id="d7618-132">When querying EF Core will set `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

## <a name="c-80-support"></a><span data-ttu-id="d7618-133">C# 8.0-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="d7618-133">C# 8.0 support</span></span>

<span data-ttu-id="d7618-134">[Issue #12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[Issue #10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)</span><span class="sxs-lookup"><span data-stu-id="d7618-134">[Tracking Issue #12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[Tracking Issue #10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)</span></span>

<span data-ttu-id="d7618-135">Dieses Feature befindet sich noch in der Entwicklungsphase und ist nicht in der aktuellen Vorschauversion enthalten.</span><span class="sxs-lookup"><span data-stu-id="d7618-135">Work on this feature has started but it isn't included in the current preview.</span></span>

<span data-ttu-id="d7618-136">Wir möchten, dass unsere Kunden bei der Verwendung von EF Core einige der [neuen Features in C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) nutzen können, z.B. asynchrone Streams (einschließlich `await foreach`) und Nullable-Verweistypen.</span><span class="sxs-lookup"><span data-stu-id="d7618-136">We want our customers to take advantage of some of the [new features coming in C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) like async streams (including `await foreach`) and nullable reference types while using EF Core.</span></span>

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="d7618-137">Reverse Engineering (Zurückentwicklung) von Datenbanksichten</span><span class="sxs-lookup"><span data-stu-id="d7618-137">Reverse engineering of database views</span></span>

[<span data-ttu-id="d7618-138">Issue #1679</span><span class="sxs-lookup"><span data-stu-id="d7618-138">Tracking Issue #1679</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/1679)

<span data-ttu-id="d7618-139">Dieses Feature ist nicht in der aktuellen Vorschauversion enthalten.</span><span class="sxs-lookup"><span data-stu-id="d7618-139">This feature isn't included in the current preview.</span></span>

<span data-ttu-id="d7618-140">[Abfragetypen](xref:core/modeling/query-types) wurden in EF Core 2.1 eingeführt und werden in EF Core 3.0 als Entitätstypen ohne Schlüssel betrachtet. Sie stellen Daten dar, die zwar aus einer Datenbank gelesen, aber nicht aktualisiert werden können.</span><span class="sxs-lookup"><span data-stu-id="d7618-140">[Query types](xref:core/modeling/query-types), introduced in EF Core 2.1 and considered entity types without keys in EF Core 3.0, represent data that can be read from the database, but cannot be updated.</span></span>
<span data-ttu-id="d7618-141">Dank dieser Eigenschaft eignen sich diese Typen in den meisten Szenarios hervorragend für Datenbanksichten. Für das Reverse Engineering von Datenbanksichten ist daher geplant, dass die Erstellung von Entitätstypen ohne Schlüssel künftig automatisiert wird.</span><span class="sxs-lookup"><span data-stu-id="d7618-141">This characteristic makes them an excellent fit for database views in most scenarios, so we plan to automate the creation of entity types without keys when reverse engineering database views.</span></span>

## <a name="property-bag-entities"></a><span data-ttu-id="d7618-142">Entitäten mit Eigenschaftenbehältern</span><span class="sxs-lookup"><span data-stu-id="d7618-142">Property bag entities</span></span>

<span data-ttu-id="d7618-143">[Issue #13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) und [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)</span><span class="sxs-lookup"><span data-stu-id="d7618-143">[Tracking Issue #13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) and [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)</span></span>

<span data-ttu-id="d7618-144">Dieses Feature befindet sich noch in der Entwicklungsphase und ist nicht in der aktuellen Vorschauversion enthalten.</span><span class="sxs-lookup"><span data-stu-id="d7618-144">Work on this feature has started but it isn't included in the current preview.</span></span> 

<span data-ttu-id="d7618-145">Bei diesem Feature geht es darum, Entitäten zu aktivieren, die Daten in indizierten Eigenschaften anstelle von regulären Eigenschaften speichern, und auch darum, Instanzen derselben .NET-Klasse (potenziell etwas so Einfaches wie `Dictionary<string, object>`) verwenden zu können, um verschiedene Entitätstypen im selben EF Core-Modell darzustellen.</span><span class="sxs-lookup"><span data-stu-id="d7618-145">This feature is about enabling entities that store data in indexed properties instead of regular properties, and also about being able to use instances of the same .NET class (potentially something as simple as a `Dictionary<string, object>`) to represent different entity types in the same EF Core model.</span></span>
<span data-ttu-id="d7618-146">Dieses Feature ist ein Ausgangspunkt für die Unterstützung von m:n-Beziehungen ohne eine Joinentität ([Issue #1368](https://github.com/aspnet/EntityFrameworkCore/issues/1368)) – eine der gefragtesten Verbesserungen für EF Core.</span><span class="sxs-lookup"><span data-stu-id="d7618-146">This feature is a stepping stone to support many-to-many relationships without a join entity ([issue #1368](https://github.com/aspnet/EntityFrameworkCore/issues/1368)), which is one of the most requested improvements for EF Core.</span></span>

## <a name="ef-63-on-net-core"></a><span data-ttu-id="d7618-147">EF 6.3 unter .NET Core</span><span class="sxs-lookup"><span data-stu-id="d7618-147">EF 6.3 on .NET Core</span></span>

[<span data-ttu-id="d7618-148">Issue EF6#271</span><span class="sxs-lookup"><span data-stu-id="d7618-148">Tracking Issue EF6#271</span></span>](https://github.com/aspnet/EntityFramework6/issues/271)

<span data-ttu-id="d7618-149">Dieses Feature befindet sich noch in der Entwicklungsphase und ist nicht in der aktuellen Vorschauversion enthalten.</span><span class="sxs-lookup"><span data-stu-id="d7618-149">Work on this feature has started but it isn't included in the current preview.</span></span> 

<span data-ttu-id="d7618-150">Uns ist bewusst, dass viele bestehende Anwendungen frühere Versionen von EF verwenden und dass die Portierung auf EF Core nur zur Nutzung von .NET Core unter Umständen mit einem erheblichen Aufwand verbunden ist.</span><span class="sxs-lookup"><span data-stu-id="d7618-150">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can sometimes require a significant effort.</span></span>
<span data-ttu-id="d7618-151">Aus diesem Grund werden wir die nächste Version von EF 6 zur Ausführung auf .NET Core 3.0 anpassen.</span><span class="sxs-lookup"><span data-stu-id="d7618-151">For that reason, we will be adapting the next version of EF 6 to run on .NET Core 3.0.</span></span>
<span data-ttu-id="d7618-152">Dies soll die Portierung vorhandener Anwendungen mit minimalen Änderungen erleichtern.</span><span class="sxs-lookup"><span data-stu-id="d7618-152">We are doing this to facilitate porting existing applications with minimal changes.</span></span>
<span data-ttu-id="d7618-153">Es wird jedoch auch einige Einschränkungen geben.</span><span class="sxs-lookup"><span data-stu-id="d7618-153">There are going to be some limitations.</span></span> <span data-ttu-id="d7618-154">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d7618-154">For example:</span></span>
- <span data-ttu-id="d7618-155">Neue Anbieter werden unter .NET Core nicht nur mit den bereits unterstützten SQL Server-Datenbanken, sondern auch mit anderen arbeiten müssen.</span><span class="sxs-lookup"><span data-stu-id="d7618-155">It will require new providers to work with other databases besides the included SQL Server support on .NET Core</span></span>
- <span data-ttu-id="d7618-156">Die Unterstützung räumlicher Daten für SQL Server wird nicht aktiviert.</span><span class="sxs-lookup"><span data-stu-id="d7618-156">Spatial support with SQL Server won't be enabled</span></span>

<span data-ttu-id="d7618-157">Beachten Sie außerdem, dass aktuell für EF 6 keine weiteren Features geplant sind.</span><span class="sxs-lookup"><span data-stu-id="d7618-157">Note also that there are no new features planned for EF 6 at this point.</span></span>
