---
title: Neue Features in EF Core 3.0
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: 528733d6eec33de2c9538541a6ed5be704b9d433
ms.sourcegitcommit: d01fc19aa42ca34c3bebccbc96ee26d06fcecaa2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/16/2019
ms.locfileid: "71005558"
---
# <a name="new-features-included-in-ef-core-30"></a><span data-ttu-id="c1488-102">Neue Features in EF Core 3.0</span><span class="sxs-lookup"><span data-stu-id="c1488-102">New features included in EF Core 3.0</span></span>

<span data-ttu-id="c1488-103">Die folgende Liste enthält die wichtigsten neuen Features, die für EF Core 3.0 geplant sind.</span><span class="sxs-lookup"><span data-stu-id="c1488-103">The following list includes the major new features planned for EF Core 3.0.</span></span>

<span data-ttu-id="c1488-104">EF Core 3.0 ist eine Hauptversion und enthält auch zahlreiche [Breaking Changes](xref:core/what-is-new/ef-core-3.0/breaking-changes), bei denen es sich um Verbesserungen der API handelt, die einen negativen Einfluss auf vorhandene Anwendungen haben können.</span><span class="sxs-lookup"><span data-stu-id="c1488-104">EF Core 3.0 is a major release and also contains numerous [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes), which are API improvements that may have negative impact on existing applications.</span></span>  

## <a name="linq-improvements"></a><span data-ttu-id="c1488-105">LINQ-Verbesserungen</span><span class="sxs-lookup"><span data-stu-id="c1488-105">LINQ improvements</span></span> 

<span data-ttu-id="c1488-106">LINQ ermöglicht es Ihnen, Datenbankabfragen zu schreiben, ohne die Sprache Ihrer Wahl zu verlassen. Rich-Type-Informationen werden genutzt, um IntelliSense und Typprüfung zur Kompilierungszeit zu bieten.</span><span class="sxs-lookup"><span data-stu-id="c1488-106">LINQ enables you to write database queries without leaving your language of choice, taking advantage of rich type information to offer IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="c1488-107">LINQ ermöglicht es Ihnen darüber hinaus, eine unbegrenzte Anzahl komplizierter Abfragen zu schreiben, die beliebige Ausdrücke enthalten (Methodenaufrufe oder Operationen).</span><span class="sxs-lookup"><span data-stu-id="c1488-107">But LINQ also enables you to write an unlimited number of complicated queries containing arbitrary expressions (method calls or operations).</span></span>
<span data-ttu-id="c1488-108">Die Verarbeitung all dieser Kombinationen hat immer eine erhebliche Herausforderung für LINQ-Anbieter dargestellt.</span><span class="sxs-lookup"><span data-stu-id="c1488-108">Handling all those combinations has always been a significant challenge for LINQ providers.</span></span>
<span data-ttu-id="c1488-109">In EF Core 3.0 haben wir unsere LINQ-Implementierung neu geschrieben, um die Übersetzung weiterer Ausdrücke in SQL zu ermöglichen, um in mehr Fällen effiziente Abfragen zu generieren, um zu verhindern, dass ineffiziente Abfragen nicht erkannt werden, und um uns die schrittweise Einführung neuer Abfragefunktionen und Leistungsoptimierungen ohne Bruch mit vorhandenen Anwendungen und Datenanbietern zu erleichtern.</span><span class="sxs-lookup"><span data-stu-id="c1488-109">In EF Core 3.0, we've rewritten our LINQ implementation to enable translating more expressions into SQL, to generate efficient queries in more cases, to prevent inefficient queries from going undetected, and to make it easier for us to gradually introduce new query capabilities and performance improvementswithout breaking existing applications and data providers.</span></span>

### <a name="client-evaluation"></a><span data-ttu-id="c1488-110">Clientauswertung</span><span class="sxs-lookup"><span data-stu-id="c1488-110">Client evaluation</span></span>

<span data-ttu-id="c1488-111">Die wichtigste Entwurfsänderung in EF Core 3.0 hat mit der Weise zu tun, wie LINQ Ausdrücke behandelt, die es nicht in SQL oder Parameter übersetzen kann:</span><span class="sxs-lookup"><span data-stu-id="c1488-111">The main design change in EF Core 3.0 has to do with how it handles LINQ expressions that it cannot translate to SQL or parameters:</span></span>

<span data-ttu-id="c1488-112">In den ersten Versionen ermittelte EF Core einfach, welche Anteile einer Abfrage in SQL übersetzt werden konnten, und führte den Rest der Abfrage auf dem Client aus.</span><span class="sxs-lookup"><span data-stu-id="c1488-112">In the first few versions, EF Core simply figured out what portions of a query could be translated to SQL, and executed the rest of the query on the client.</span></span>
<span data-ttu-id="c1488-113">Diese Art von clientseitiger Ausführung kann in einigen Situationen wünschenswert sein, aber in vielen anderen Fällen kann sie zu ineffizienten Abfragen führen.</span><span class="sxs-lookup"><span data-stu-id="c1488-113">This type of client-side execution can be desirable in some situations, but in many other cases it can result in inefficient queries.</span></span>
<span data-ttu-id="c1488-114">Wenn beispielsweise EF Core 2.2 ein Prädikat in einem `Where()`-Aufruf nicht übersetzen konnte, führte es eine SQL-Anweisung ohne Filter aus, las alle Zeilen aus der Datenbank und filterte sie dann im Arbeitsspeicher.</span><span class="sxs-lookup"><span data-stu-id="c1488-114">For example, if EF Core 2.2 couldn't translate a predicate in a `Where()` call, it executed a SQL statement without a filter, read all all the rows from the database, and then filtered them in-memory.</span></span>
<span data-ttu-id="c1488-115">Das kann akzeptabel sein, wenn die Datenbank eine kleine Anzahl Zeilen enthält, kann aber auch zu erheblichen Leistungsproblemen oder sogar dem Ausfall der Anwendung führen, wenn eine Datenbank eine große Anzahl Zeilen enthält.</span><span class="sxs-lookup"><span data-stu-id="c1488-115">That may be acceptable if the database contains a small number of rows, but can result in significant performance issues or even application failure if the database contains a large number or rows.</span></span>
<span data-ttu-id="c1488-116">In EF Core 3.0 haben wir die Clientauswertung auf die Projektion der obersten Ebene (den letzten Aufruf von `Select()`) eingeschränkt.</span><span class="sxs-lookup"><span data-stu-id="c1488-116">In EF Core 3.0 we have restricted client evaluation to only happen on the top-level projection (the last call to `Select()`).</span></span>
<span data-ttu-id="c1488-117">Wenn EF Core 3.0 an irgendeiner anderen Stelle in der Abfrage Ausdrücke erkennt, die nicht übersetzt werden können, gibt es eine Ausnahme zur Laufzeit aus.</span><span class="sxs-lookup"><span data-stu-id="c1488-117">When EF Core 3.0 detects expressions that cannot be translated anywhere else in the query, it throws a runtime exception.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="c1488-118">Cosmos DB-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="c1488-118">Cosmos DB support</span></span> 

<span data-ttu-id="c1488-119">Der Cosmos DB-Anbieter für EF Core ermöglicht es Entwicklern, die mit dem EF-Programmiermodell vertraut sind, Azure Cosmos DB problemlos als Anwendungsdatenbank-Ziel einzusetzen.</span><span class="sxs-lookup"><span data-stu-id="c1488-119">The Cosmos DB provider for EF Core enables developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span>
<span data-ttu-id="c1488-120">Ziel ist es, einige der Vorteile von Cosmos DB – beispielsweise die globale Verteilung, Always On-Verfügbarkeit, elastische Skalierbarkeit und geringe Latenz – noch besser für .NET-Entwickler zugänglich zu machen.</span><span class="sxs-lookup"><span data-stu-id="c1488-120">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span>
<span data-ttu-id="c1488-121">Der Anbieter wird die meisten EF Core-Features unterstützen, darunter die automatische Änderungsnachverfolgung, LINQ und Wertekonvertierungen anhand der SQL-API in Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="c1488-121">The provider will enable most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>

## <a name="c-80-support"></a><span data-ttu-id="c1488-122">C# 8.0-Unterstützung</span><span class="sxs-lookup"><span data-stu-id="c1488-122">C# 8.0 support</span></span>

<span data-ttu-id="c1488-123">EF Core 3.0 nutzt einige der neuen Funktionen in C# 8.0:</span><span class="sxs-lookup"><span data-stu-id="c1488-123">EF Core 3.0 takes advantage of some of the new features in C# 8.0:</span></span>

### <a name="asynchronous-streams"></a><span data-ttu-id="c1488-124">Asynchrone Streams</span><span class="sxs-lookup"><span data-stu-id="c1488-124">Asynchronous streams</span></span>

<span data-ttu-id="c1488-125">Asynchrone Abfrageergebnisse werden jetzt mithilfe der neuen `IAsyncEnumerable<T>`-Standardschnittelle verfügbar gemacht und können mithilfe von `await foreach` konsumiert werden.</span><span class="sxs-lookup"><span data-stu-id="c1488-125">Asynchronous query results are now exposed using the new standard `IAsyncEnumerable<T>` interface and can be consumed using `await foreach`.</span></span>

``` csharp
var orders = 
  from o in context.Orders
  where o.Status == OrderStatus.Pending
  select o;

await foreach(var o in orders)
{
  Proccess(o);
} 
```

### <a name="nullable-reference-types"></a><span data-ttu-id="c1488-126">Nullwerte zulassende Verweistypen</span><span class="sxs-lookup"><span data-stu-id="c1488-126">Nullable reference types</span></span> 

<span data-ttu-id="c1488-127">Wenn diese neue Funktion in Ihrem Code aktiviert ist, kann EF Core Überlegungen zur NULL-Zulässigkeit für Eigenschaften von Verweistypen (entweder primitiver Typen wie Zeichenfolgen oder Navigationseigenschaften) anstellen, um die NULL-Zulässigkeit von Spalten und Beziehungen in der Datenbank zu entscheiden.</span><span class="sxs-lookup"><span data-stu-id="c1488-127">When this new feature is enabled in your code, EF Core can reason about the nullability of properties of refrence types (either of primitive types like string or navigation properties) to decide the nullability of columns and relationships in the database.</span></span>

## <a name="interception"></a><span data-ttu-id="c1488-128">Interception</span><span class="sxs-lookup"><span data-stu-id="c1488-128">Interception</span></span>

<span data-ttu-id="c1488-129">Die neue Abfang-API in EF Core 3.0 ermöglicht das programmgesteuerte Beobachten und Ändern des Ergebnisses von Datenbankvorgängen auf unteren Ebenen, die im Rahmen des normalen Betriebs von EF Core auftreten, z. B. das Öffnen von Verbindungen, das Einleiten von Transaktionen und das Ausführen von Befehlen.</span><span class="sxs-lookup"><span data-stu-id="c1488-129">The new interception API in EF Core 3.0 allows programatically observing and modifying the outcome of low-level database operations that occur as part of the normal operation of EF Core, such as opening connections, initating transactions, and executing commands.</span></span> 

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="c1488-130">Reverse Engineering (Zurückentwicklung) von Datenbanksichten</span><span class="sxs-lookup"><span data-stu-id="c1488-130">Reverse engineering of database views</span></span>

<span data-ttu-id="c1488-131">Entitätstypen ohne Schlüssel (bisher als [Abfragetypen](xref:core/modeling/query-types) bezeichnet) stellen Daten dar, die aus der Datenbank gelesen, aber nicht aktualisiert werden können.</span><span class="sxs-lookup"><span data-stu-id="c1488-131">Entity types without keys (previously known as [query types](xref:core/modeling/query-types)) represent data that can be read from the database, but cannot be updated.</span></span>
<span data-ttu-id="c1488-132">Dank dieser Eigenschaft eignen sich diese Typen in den meisten Szenarien hervorragend für die Zuordnung von Datenbanksichten. Für das Reverse Engineering von Datenbanksichten haben wir daher die Erstellung von Entitätstypen ohne Schlüssel automatisiert.</span><span class="sxs-lookup"><span data-stu-id="c1488-132">This characteristic makes them an excellent fit for mapping database views in most scenarios, so we automated the creation of entity types without keys when reverse engineering database views.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="c1488-133">Abhängige Entitäten, die die Tabelle gemeinsam mit dem Prinzipal verwenden, sind jetzt optional</span><span class="sxs-lookup"><span data-stu-id="c1488-133">Dependent entities sharing the table with the principal are now optional</span></span>

<span data-ttu-id="c1488-134">Ab EF Core 3.0 gibt es die Möglichkeit, wenn `OrderDetails` im Besitz von `Order` ist oder explizit derselben Tabelle zugeordnet ist, dass `Order` ohne `OrderDetails` hinzugefügt werden kann, und alle `OrderDetails`-Eigenschaften, mit Ausnahme des primären Schlüssels, werden Spalten zugeordnet, die NULL-Werte zulassen.</span><span class="sxs-lookup"><span data-stu-id="c1488-134">Starting with EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table, it will be possible to add an `Order` without an `OrderDetails` and all of the `OrderDetails` properties, except the primary key will be mapped to nullable columns.</span></span>

<span data-ttu-id="c1488-135">Bei Abfragen von EF Core wird `OrderDetails` auf `null` festgelegt, wenn eine der erforderlichen Eigenschaften keinen Wert aufweist oder keine erforderlichen Eigenschaften außer dem primären Schlüssel vorhanden und alle Eigenschaften `null` sind.</span><span class="sxs-lookup"><span data-stu-id="c1488-135">When querying, EF Core will set `OrderDetails` to `null` if any of its required properties doesn't have a value, or if it has no required properties besides the primary key and all properties are `null`.</span></span>

``` csharp
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

## <a name="ef-63-on-net-core"></a><span data-ttu-id="c1488-136">EF 6.3 unter .NET Core</span><span class="sxs-lookup"><span data-stu-id="c1488-136">EF 6.3 on .NET Core</span></span>

<span data-ttu-id="c1488-137">Uns ist bewusst, dass viele bestehende Anwendungen frühere Versionen von EF verwenden und dass die Portierung auf EF Core nur zur Nutzung von .NET Core unter Umständen mit einem erheblichen Aufwand verbunden ist.</span><span class="sxs-lookup"><span data-stu-id="c1488-137">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can sometimes require a significant effort.</span></span>
<span data-ttu-id="c1488-138">Aus diesem Grund haben wir für die neueste Version von EF 6 die Ausführung unter .NET Core 3.0 ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="c1488-138">For that reason, we have enabled the newewst version of EF 6 to run on .NET Core 3.0.</span></span>
<span data-ttu-id="c1488-139">Es gibt eine Reihe von Beschränkungen, darunter:</span><span class="sxs-lookup"><span data-stu-id="c1488-139">There are some limitations, for example:</span></span>
- <span data-ttu-id="c1488-140">Neue Anbieter sind für die Arbeit mit .NET Core erforderlich</span><span class="sxs-lookup"><span data-stu-id="c1488-140">New providers are required to work on .NET Core</span></span>
- <span data-ttu-id="c1488-141">Die Unterstützung räumlicher Daten für SQL Server wird nicht aktiviert.</span><span class="sxs-lookup"><span data-stu-id="c1488-141">Spatial support with SQL Server won't be enabled</span></span>

## <a name="postponed-features"></a><span data-ttu-id="c1488-142">Verschobene Features</span><span class="sxs-lookup"><span data-stu-id="c1488-142">Postponed features</span></span>

<span data-ttu-id="c1488-143">Einige Features, die ursprünglich für EF Core 3.0 geplant waren, wurden auf zukünftige Versionen verschoben:</span><span class="sxs-lookup"><span data-stu-id="c1488-143">Some features originally planned for EF Core 3.0 were postponed to future releases:</span></span> 

- <span data-ttu-id="c1488-144">Die Fähigkeit, Teile eines Modells in Migrationen zu ignorieren, wie in [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725) nachverfolgt.</span><span class="sxs-lookup"><span data-stu-id="c1488-144">Ability to ingore parts of a model in migrations, tracked by [#2725](https://github.com/aspnet/EntityFrameworkCore/issues/2725).</span></span>
- <span data-ttu-id="c1488-145">Entitäten von Eigenschaftenbehältern, die in zwei separaten Problemen nachverfolgt werden: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) über Entitäten von gemeinsam genutzten Typen und [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) über Unterstützung der Zuordnung von indizierten Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="c1488-145">Property bag entities, tracked by two separate issues: [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914) about shared-type entities and [#13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) about indexed property mapping support.</span></span>
