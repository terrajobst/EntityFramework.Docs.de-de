---
title: Abfragetypen – EF Core
author: anpete
ms.date: 02/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/query-types
ms.openlocfilehash: 6f0f860c6a4e619e13d55e6207234a8b5261ee09
ms.sourcegitcommit: d1230e34673b8323a227ab37958dfa77f3684728
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2019
ms.locfileid: "68330793"
---
# <a name="query-types"></a><span data-ttu-id="09221-102">Abfragetypen</span><span class="sxs-lookup"><span data-stu-id="09221-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="09221-103">Dieses Feature ist neu in EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="09221-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="09221-104">Zusätzlich zu den Entitätstypen, kann ein EF Core-Modell enthalten _Abfragetypen_, die zum Datenbankabfragen für Daten ausführen, auf denen die Entitätstypen zugeordnet wird nicht verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="09221-104">In addition to entity types, an EF Core model can contain _query types_, which can be used to carry out database queries against data that isn't mapped to entity types.</span></span>

## <a name="compare-query-types-to-entity-types"></a><span data-ttu-id="09221-105">Vergleichen von Abfragetypen, die Entitätstypen</span><span class="sxs-lookup"><span data-stu-id="09221-105">Compare query types to entity types</span></span>

<span data-ttu-id="09221-106">Abfragetypen sind wie Entitätstypen in, das sie:</span><span class="sxs-lookup"><span data-stu-id="09221-106">Query types are like entity types in that they:</span></span>

- <span data-ttu-id="09221-107">Können mit dem Modell hinzugefügt werden, entweder in `OnModelCreating` oder über eine "set"-Eigenschaft in einer abgeleiteten _"DbContext"_ .</span><span class="sxs-lookup"><span data-stu-id="09221-107">Can be added to the model either in `OnModelCreating` or via a "set" property on a derived _DbContext_.</span></span>
- <span data-ttu-id="09221-108">Unterstützt viele der gleichen Zuordnungsfunktionen, wie die Vererbungseigenschaften Zuordnung und die Navigation.</span><span class="sxs-lookup"><span data-stu-id="09221-108">Support many of the same mapping capabilities, like inheritance mapping and navigation properties.</span></span> <span data-ttu-id="09221-109">Für relationale Speicher können sie die Ziel-Datenbank-Objekte und Spalten über die Methoden der fluent-API oder datenanmerkungen konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="09221-109">On relational stores, they can configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="09221-110">Sie unterscheiden sich jedoch von Entität Typen in, das sie:</span><span class="sxs-lookup"><span data-stu-id="09221-110">However, they are different from entity types in that they:</span></span>

- <span data-ttu-id="09221-111">Erfordern Sie keinen Schlüssel definiert werden.</span><span class="sxs-lookup"><span data-stu-id="09221-111">Do not require a key to be defined.</span></span>
- <span data-ttu-id="09221-112">Werden nie damit Änderungen nachverfolgt, auf die _"DbContext"_ und daher nie eingefügt, aktualisiert oder gelöscht werden in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="09221-112">Are never tracked for changes on the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="09221-113">Gemäß der Konvention werden nicht ermittelt werden.</span><span class="sxs-lookup"><span data-stu-id="09221-113">Are never discovered by convention.</span></span>
- <span data-ttu-id="09221-114">Unterstützen Sie nur eine Teilmenge der Funktionen für die Zuordnung von Navigation - insbesondere:</span><span class="sxs-lookup"><span data-stu-id="09221-114">Only support a subset of navigation mapping capabilities - Specifically:</span></span>
  - <span data-ttu-id="09221-115">Sie können nicht als das prinzipalende der Beziehung fungieren.</span><span class="sxs-lookup"><span data-stu-id="09221-115">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="09221-116">Sie können nur auf Entitäten verweisen verweisnavigationseigenschaften enthalten.</span><span class="sxs-lookup"><span data-stu-id="09221-116">They can only contain reference navigation properties pointing to entities.</span></span>
  - <span data-ttu-id="09221-117">Entitäten können keine Navigationseigenschaften für Abfragetypen enthalten.</span><span class="sxs-lookup"><span data-stu-id="09221-117">Entities cannot contain navigation properties to query types.</span></span>
- <span data-ttu-id="09221-118">Adressiert werden, auf die _ModelBuilder_ mithilfe der `Query` Methode anstelle der `Entity` Methode.</span><span class="sxs-lookup"><span data-stu-id="09221-118">Are addressed on the _ModelBuilder_ using the `Query` method rather than the `Entity` method.</span></span>
- <span data-ttu-id="09221-119">Zugeordnet sind, auf die _"DbContext"_ über Eigenschaften vom Typ `DbQuery<T>` statt `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="09221-119">Are mapped on the _DbContext_ through properties of type `DbQuery<T>` rather than `DbSet<T>`</span></span>
- <span data-ttu-id="09221-120">Datenbankobjekte mit zugeordnet sind die `ToView` Methode statt `ToTable`.</span><span class="sxs-lookup"><span data-stu-id="09221-120">Are mapped to database objects using the `ToView` method, rather than `ToTable`.</span></span>
- <span data-ttu-id="09221-121">Zugeordnet werden kann eine _Abfrage definiert_ : Definieren der Abfrage ist eine sekundäre Abfrage, die im Modell, die eine Datenquelle für den Abfragetyp dient deklariert.</span><span class="sxs-lookup"><span data-stu-id="09221-121">May be mapped to a _defining query_ - A defining query is a secondary query declared in the model that acts a data source for a query type.</span></span>

## <a name="usage-scenarios"></a><span data-ttu-id="09221-122">Verwendungsszenarien</span><span class="sxs-lookup"><span data-stu-id="09221-122">Usage scenarios</span></span>

<span data-ttu-id="09221-123">Einige der wichtigsten Verwendungsszenarien für Abfragetypen sind:</span><span class="sxs-lookup"><span data-stu-id="09221-123">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="09221-124">Dient als Rückgabetyp für ad-hoc- `FromSql()` Abfragen.</span><span class="sxs-lookup"><span data-stu-id="09221-124">Serving as the return type for ad hoc `FromSql()` queries.</span></span>
- <span data-ttu-id="09221-125">Zuordnung zu Datenbankansichten.</span><span class="sxs-lookup"><span data-stu-id="09221-125">Mapping to database views.</span></span>
- <span data-ttu-id="09221-126">Zuordnung zu Tabellen, die nicht über einen definierten Primärschlüssel verfügen.</span><span class="sxs-lookup"><span data-stu-id="09221-126">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="09221-127">Zuordnen zu Abfragen, die im Modell definiert.</span><span class="sxs-lookup"><span data-stu-id="09221-127">Mapping to queries defined in the model.</span></span>

## <a name="mapping-to-database-objects"></a><span data-ttu-id="09221-128">Zuordnen von Datenbankobjekten</span><span class="sxs-lookup"><span data-stu-id="09221-128">Mapping to database objects</span></span>

<span data-ttu-id="09221-129">Zuordnen der Abfragetyp auf ein Datenbankobjekt wird erreicht, indem die `ToView` fluent-API.</span><span class="sxs-lookup"><span data-stu-id="09221-129">Mapping a query type to a database object is achieved using the `ToView` fluent API.</span></span> <span data-ttu-id="09221-130">Aus der Perspektive von EF Core, das Datenbankobjekt, das in dieser Methode wird ein _Ansicht_, was bedeutet, dass es als eine nur-Lese Query-Datenquelle behandelt wird kann nicht Ziel von Updates, insert oder delete-Operationen.</span><span class="sxs-lookup"><span data-stu-id="09221-130">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="09221-131">Dies bedeutet jedoch nicht, dass das Datenbankobjekt tatsächlich erforderlich ist ist, eine Datenbanksicht sein - es kann auch eine Datenbanktabelle, die als schreibgeschützt behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="09221-131">However, this does not mean that the database object is actually required to be a database view - It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="09221-132">Im Gegensatz dazu für Entitätstypen, EF Core geht davon aus, dass ein Datenbankobjekt in angegeben die `ToTable` Methode behandelt werden kann, als eine _Tabelle_, was bedeutet, dass er kann als ein Query-Datenquelle verwendet werden, sondern auch das Ziel aktualisieren, löschen und einfügen Vorgänge.</span><span class="sxs-lookup"><span data-stu-id="09221-132">Conversely, for entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="09221-133">In der Tat können Sie angeben, den Namen einer Datenbanksicht in `ToTable` und alles sollte gut funktionieren, solange die Ansicht für die für die Datenbank aktualisierbar sein, konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="09221-133">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

## <a name="example"></a><span data-ttu-id="09221-134">Beispiel</span><span class="sxs-lookup"><span data-stu-id="09221-134">Example</span></span>

<span data-ttu-id="09221-135">Das folgende Beispiel zeigt, wie Sie den Abfragetyp zu verwenden, um eine Datenbankansicht abzufragen.</span><span class="sxs-lookup"><span data-stu-id="09221-135">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="09221-136">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryTypes) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="09221-136">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="09221-137">Zuerst definieren wir ein einfaches Blog und Post-Modell:</span><span class="sxs-lookup"><span data-stu-id="09221-137">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="09221-138">Als Nächstes definieren wir eine einfache Datenbank-Ansicht, die wir Abfragen die Anzahl an Beiträgen, die jeden Blog zugeordnet werden kann:</span><span class="sxs-lookup"><span data-stu-id="09221-138">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#View)]

<span data-ttu-id="09221-139">Als Nächstes definieren wir eine Klasse, um das Ergebnis aus der Datenbankansicht enthalten soll:</span><span class="sxs-lookup"><span data-stu-id="09221-139">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="09221-140">Als Nächstes konfigurieren wir den Abfragetyp in _"onmodelcreating"_ mithilfe der `modelBuilder.Query<T>` API.</span><span class="sxs-lookup"><span data-stu-id="09221-140">Next, we configure the query type in _OnModelCreating_ using the `modelBuilder.Query<T>` API.</span></span>
<span data-ttu-id="09221-141">Verwenden wir standard fluent-Konfigurations-APIs, um die Zuordnung für den Abfragetyp zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="09221-141">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="09221-142">Als Nächstes konfigurieren wir den `DbContext` so, dass `DbQuery<T>`er folgendes einschließt:[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#DbQuery)]</span><span class="sxs-lookup"><span data-stu-id="09221-142">Next, we configure the `DbContext` to include the `DbQuery<T>`: [!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#DbQuery)]</span></span>

<span data-ttu-id="09221-143">Schließlich können wir die Datenbanksicht auf die übliche Weise Abfragen:</span><span class="sxs-lookup"><span data-stu-id="09221-143">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../samples/core/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="09221-144">Beachten Sie, dass wir auch eine Ebene Abfrageeigenschaft (DbQuery) fungieren als Stamm für Abfragen gegen diese Art von Kontext definiert haben.</span><span class="sxs-lookup"><span data-stu-id="09221-144">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
