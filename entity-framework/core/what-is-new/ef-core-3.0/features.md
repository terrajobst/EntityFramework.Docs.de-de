---
title: Neue Features in EF Core 3.0
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: d61fa884f4669daa220ffc96ae59dd63518e6d5a
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921673"
---
# <a name="new-features-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="753e4-102">Neue Features in EF Core 3.0 (aktuell in der Vorschauversion)</span><span class="sxs-lookup"><span data-stu-id="753e4-102">New features included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="753e4-103">Bitte beachten Sie, dass die Featuregruppen und Zeitpläne für künftige Releases jederzeit geändert werden können. Obwohl diese Seite bestmöglich aktualisiert wird, entspricht sie nicht immer den neuesten Plänen.</span><span class="sxs-lookup"><span data-stu-id="753e4-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="753e4-104">Die folgende Liste enthält die wichtigsten neuen Features, die für EF Core 3.0 geplant sind.</span><span class="sxs-lookup"><span data-stu-id="753e4-104">The following list includes the major new features planned for EF Core 3.0.</span></span>
<span data-ttu-id="753e4-105">Die meisten davon sind aktuell nicht in der Vorschauversion enthalten und werden im Verlauf der RTM-Vorbereitung zur Verfügung gestellt.</span><span class="sxs-lookup"><span data-stu-id="753e4-105">Most of these features are not included in the current preview, but will become available as we make progress towards RTM.</span></span>

<span data-ttu-id="753e4-106">Der Grund dafür ist, dass zu Releasebeginn zuerst die geplanten [Breaking Changes](xref:core/what-is-new/ef-core-3.0/breaking-changes) implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="753e4-106">The reason is that at the beginning of the release we are focusing on implementing planned [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes).</span></span>
<span data-ttu-id="753e4-107">Allein durch viele dieser Änderungen wird EF Core bereits deutlich verbessert.</span><span class="sxs-lookup"><span data-stu-id="753e4-107">Many of these breaking changes are improvements to EF Core on their own.</span></span>
<span data-ttu-id="753e4-108">Für weitere Optimierungen werden zusätzliche Breaking Changes erforderlich sein.</span><span class="sxs-lookup"><span data-stu-id="753e4-108">Many others are required to unblock further improvements.</span></span> 

<span data-ttu-id="753e4-109">Eine vollständige Liste der aktuellen Fehlerbehebungen und Verbesserungen finden Sie [mit der entsprechenden Abfrage in unserem Issuetracker](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="753e4-109">For a complete list of bug fixes and enhancements underway, you can see [this query in our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).</span></span>

## <a name="linq-improvements"></a><span data-ttu-id="753e4-110">LINQ-Verbesserungen</span><span class="sxs-lookup"><span data-stu-id="753e4-110">LINQ improvements</span></span> 

[<span data-ttu-id="753e4-111">Issue #12795</span><span class="sxs-lookup"><span data-stu-id="753e4-111">Tracking Issue #12795</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

<span data-ttu-id="753e4-112">Dieses Feature befindet sich noch in der Entwicklungsphase und ist nicht in der aktuellen Vorschauversion enthalten.</span><span class="sxs-lookup"><span data-stu-id="753e4-112">Work on this feature has started but it isn't included in the current preview.</span></span>

<span data-ttu-id="753e4-113">LINQ ermöglicht es Ihnen, Datenbankabfragen zu schreiben, ohne die Sprache Ihrer Wahl zu verlassen. Gleichzeitig werden Rich-Type-Informationen verwendet, um von IntelliSense und der Typprüfung zur Kompilierungszeit zu profitieren.</span><span class="sxs-lookup"><span data-stu-id="753e4-113">LINQ enables you to write database queries without leaving your language of choice, taking advantage of rich type information to get IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="753e4-114">Aber mit LINQ können Sie auch eine unbegrenzte Anzahl komplizierter Abfragen schreiben, und dies war schon immer eine große Herausforderung für LINQ-Anbieter.</span><span class="sxs-lookup"><span data-stu-id="753e4-114">But LINQ also enables you to write an unlimited number of complicated queries, and that has always been a huge challenge for LINQ providers.</span></span>
<span data-ttu-id="753e4-115">In den ersten Versionen von EF Core wurde dies teilweise gelöst, indem wir herausgefunden haben, welche Teile einer Abfrage in SQL übersetzt werden konnten, und dann den Rest der Abfrage im Speicher des Clients ausführen ließen.</span><span class="sxs-lookup"><span data-stu-id="753e4-115">In the first few versions of EF Core, we solved that in part by figuring out what portions of a query could be translated to SQL, and then by allowing the rest of the query to execute in memory on the client.</span></span>
<span data-ttu-id="753e4-116">Diese clientseitige Ausführung kann in einigen Situationen wünschenswert sein, aber in vielen anderen Fällen kann sie zu ineffizienten Abfragen führen, die möglicherweise erst nach der Bereitstellung einer Anwendung in der Produktion identifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="753e4-116">This client-side execution can be desirable in some situations, but in many other cases it can result in inefficient queries that may not be identified until an application is deployed to production.</span></span>
<span data-ttu-id="753e4-117">In EF Core 3.0 planen wir, tiefgreifende Änderungen an der Funktionsweise unserer LINQ-Implementierung und der Art ihrer Überprüfung vorzunehmen.</span><span class="sxs-lookup"><span data-stu-id="753e4-117">In EF Core 3.0, we're planning to make profound changes to how our LINQ implementation works, and how we test it.</span></span>
<span data-ttu-id="753e4-118">Die Ziele bestehen darin, die Implementierung robuster zu machen (z.B., um Breaking Changes in Patchreleases zu vermeiden), mehr Ausdrücke korrekt in SQL übersetzen zu können, in mehr Fällen effiziente Abfragen zu erzeugen und zu verhindern, dass ineffiziente Abfragen unentdeckt bleiben.</span><span class="sxs-lookup"><span data-stu-id="753e4-118">The goals are to make it more robust (for example, to avoid breaking queries in patch releases), to enable translating more expressions correctly into SQL, to generate efficient queries in more cases, and to prevent inefficient queries from going undetected.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="753e4-119">Cosmos DB-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="753e4-119">Cosmos DB support</span></span> 

[<span data-ttu-id="753e4-120">Issue #8443</span><span class="sxs-lookup"><span data-stu-id="753e4-120">Tracking Issue #8443</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/8443)

<span data-ttu-id="753e4-121">Dieses Feature ist in der aktuellen Vorschauversion enthalten. Die Arbeit daran ist aber noch nicht abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="753e4-121">This feature is included in the current preview, but isn't complete yet.</span></span> 

<span data-ttu-id="753e4-122">Wir arbeiten an einem Cosmos DB-Anbieter für EF Core, damit Entwickler, die mit dem EF-Programmiermodell vertraut sind, Azure Cosmos DB problemlos als Anwendungsdatenbank einsetzen können.</span><span class="sxs-lookup"><span data-stu-id="753e4-122">We're working on a Cosmos DB provider for EF Core, to enable developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span>
<span data-ttu-id="753e4-123">Ziel ist es, einige der Vorteile von Cosmos DB – beispielsweise die globale Verteilung, Always On-Verfügbarkeit, elastische Skalierbarkeit und geringe Latenz – noch besser für .NET-Entwickler zugänglich zu machen.</span><span class="sxs-lookup"><span data-stu-id="753e4-123">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span>
<span data-ttu-id="753e4-124">Der Anbieter wird die meisten EF Core-Features unterstützen, darunter die automatische Änderungsnachverfolgung, LINQ und Wertekonvertierungen anhand der SQL-API in Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="753e4-124">The provider will enable most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>
<span data-ttu-id="753e4-125">Diese Bemühungen begannen vor EF Core 2.2, und [wir haben einige Vorschauversionen des Anbieters zur Verfügung gestellt](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).</span><span class="sxs-lookup"><span data-stu-id="753e4-125">We started this effort before EF Core 2.2, and [we have made some preview versions of the provider available](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).</span></span>
<span data-ttu-id="753e4-126">Der neue Plan sieht vor, den Anbieter neben EF Core 3.0 weiterzuentwickeln.</span><span class="sxs-lookup"><span data-stu-id="753e4-126">The new plan is to continue developing the provider alongside EF Core 3.0.</span></span> 

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="753e4-127">Abhängige Entitäten, die die Tabelle gemeinsam mit dem Prinzipal verwenden, sind jetzt optional</span><span class="sxs-lookup"><span data-stu-id="753e4-127">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="753e4-128">Issue #9005</span><span class="sxs-lookup"><span data-stu-id="753e4-128">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="753e4-129">Dieses Feature wird in Vorschauversion 4 von EF Core 3.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="753e4-129">This feature will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="753e4-130">Sehen Sie sich das folgende Modell an:</span><span class="sxs-lookup"><span data-stu-id="753e4-130">Consider the following model:</span></span>
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

[Owned]
public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```

<span data-ttu-id="753e4-131">Ab EF Core 3.0 gibt es die Möglichkeit, wenn `OrderDetails` im Besitz von `Order` ist oder explizit derselben Tabelle zugeordnet ist, dass `Order` ohne `OrderDetails` hinzugefügt werden kann, und alle `OrderDetails`-Eigenschaften, mit Ausnahme des primären Schlüssels, werden Spalten zugeordnet, die NULL-Werte zulassen.</span><span class="sxs-lookup"><span data-stu-id="753e4-131">Starting with EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table, it will be possible to add an `Order` without an `OrderDetails` and all of the `OrderDetails` properties, except the primary key will be mapped to nullable columns.</span></span>

<span data-ttu-id="753e4-132">Bei Abfragen von EF Core wird `OrderDetails` auf `null` festgelegt, wenn eine der erforderlichen Eigenschaften keinen Wert aufweist oder keine erforderlichen Eigenschaften außer dem primären Schlüssel vorhanden und alle Eigenschaften `null` sind.</span><span class="sxs-lookup"><span data-stu-id="753e4-132">When querying, EF Core will set `OrderDetails` to `null` if any of its required properties doesn't have a value, or if it has no required properties besides the primary key and all properties are `null`.</span></span>

## <a name="c-80-support"></a><span data-ttu-id="753e4-133">C# 8.0-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="753e4-133">C# 8.0 support</span></span>

<span data-ttu-id="753e4-134">[Issue #12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[Issue #10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)</span><span class="sxs-lookup"><span data-stu-id="753e4-134">[Tracking Issue #12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[Tracking Issue #10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)</span></span>

<span data-ttu-id="753e4-135">Dieses Feature befindet sich noch in der Entwicklungsphase und ist nicht in der aktuellen Vorschauversion enthalten.</span><span class="sxs-lookup"><span data-stu-id="753e4-135">Work on this feature has started but it isn't included in the current preview.</span></span>

<span data-ttu-id="753e4-136">Wir möchten, dass unsere Kunden bei der Verwendung von EF Core einige der [neuen Features in C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) nutzen können, z.B. asynchrone Streams (einschließlich `await foreach`) und Nullable-Verweistypen.</span><span class="sxs-lookup"><span data-stu-id="753e4-136">We want our customers to take advantage of some of the [new features coming in C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) like async streams (including `await foreach`) and nullable reference types while using EF Core.</span></span>

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="753e4-137">Reverse Engineering (Zurückentwicklung) von Datenbanksichten</span><span class="sxs-lookup"><span data-stu-id="753e4-137">Reverse engineering of database views</span></span>

[<span data-ttu-id="753e4-138">Issue #1679</span><span class="sxs-lookup"><span data-stu-id="753e4-138">Tracking Issue #1679</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/1679)

<span data-ttu-id="753e4-139">Dieses Feature ist nicht in der aktuellen Vorschauversion enthalten.</span><span class="sxs-lookup"><span data-stu-id="753e4-139">This feature isn't included in the current preview.</span></span>

<span data-ttu-id="753e4-140">[Abfragetypen](xref:core/modeling/query-types) wurden in EF Core 2.1 eingeführt und werden in EF Core 3.0 als Entitätstypen ohne Schlüssel betrachtet. Sie stellen Daten dar, die zwar aus einer Datenbank gelesen, aber nicht aktualisiert werden können.</span><span class="sxs-lookup"><span data-stu-id="753e4-140">[Query types](xref:core/modeling/query-types), introduced in EF Core 2.1 and considered entity types without keys in EF Core 3.0, represent data that can be read from the database, but cannot be updated.</span></span>
<span data-ttu-id="753e4-141">Dank dieser Eigenschaft eignen sich diese Typen in den meisten Szenarios hervorragend für Datenbanksichten. Für das Reverse Engineering von Datenbanksichten ist daher geplant, dass die Erstellung von Entitätstypen ohne Schlüssel künftig automatisiert wird.</span><span class="sxs-lookup"><span data-stu-id="753e4-141">This characteristic makes them an excellent fit for database views in most scenarios, so we plan to automate the creation of entity types without keys when reverse engineering database views.</span></span>

## <a name="ef-63-on-net-core"></a><span data-ttu-id="753e4-142">EF 6.3 unter .NET Core</span><span class="sxs-lookup"><span data-stu-id="753e4-142">EF 6.3 on .NET Core</span></span>

[<span data-ttu-id="753e4-143">Issue EF6#271</span><span class="sxs-lookup"><span data-stu-id="753e4-143">Tracking Issue EF6#271</span></span>](https://github.com/aspnet/EntityFramework6/issues/271)

<span data-ttu-id="753e4-144">Dieses Feature befindet sich noch in der Entwicklungsphase und ist nicht in der aktuellen Vorschauversion enthalten.</span><span class="sxs-lookup"><span data-stu-id="753e4-144">Work on this feature has started but it isn't included in the current preview.</span></span> 

<span data-ttu-id="753e4-145">Uns ist bewusst, dass viele bestehende Anwendungen frühere Versionen von EF verwenden und dass die Portierung auf EF Core nur zur Nutzung von .NET Core unter Umständen mit einem erheblichen Aufwand verbunden ist.</span><span class="sxs-lookup"><span data-stu-id="753e4-145">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can sometimes require a significant effort.</span></span>
<span data-ttu-id="753e4-146">Aus diesem Grund werden wir die nächste Version von EF 6 zur Ausführung auf .NET Core 3.0 anpassen.</span><span class="sxs-lookup"><span data-stu-id="753e4-146">For that reason, we will be adapting the next version of EF 6 to run on .NET Core 3.0.</span></span>
<span data-ttu-id="753e4-147">Dies soll die Portierung vorhandener Anwendungen mit minimalen Änderungen erleichtern.</span><span class="sxs-lookup"><span data-stu-id="753e4-147">We are doing this to facilitate porting existing applications with minimal changes.</span></span>
<span data-ttu-id="753e4-148">Es wird jedoch auch einige Einschränkungen geben.</span><span class="sxs-lookup"><span data-stu-id="753e4-148">There are going to be some limitations.</span></span> <span data-ttu-id="753e4-149">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="753e4-149">For example:</span></span>
- <span data-ttu-id="753e4-150">Neue Anbieter werden unter .NET Core nicht nur mit den bereits unterstützten SQL Server-Datenbanken, sondern auch mit anderen arbeiten müssen.</span><span class="sxs-lookup"><span data-stu-id="753e4-150">It will require new providers to work with other databases besides the included SQL Server support on .NET Core</span></span>
- <span data-ttu-id="753e4-151">Die Unterstützung räumlicher Daten für SQL Server wird nicht aktiviert.</span><span class="sxs-lookup"><span data-stu-id="753e4-151">Spatial support with SQL Server won't be enabled</span></span>

<span data-ttu-id="753e4-152">Beachten Sie außerdem, dass aktuell für EF 6 keine weiteren Features geplant sind.</span><span class="sxs-lookup"><span data-stu-id="753e4-152">Note also that there are no new features planned for EF 6 at this point.</span></span>
