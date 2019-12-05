---
title: Typen von Keyless-Entitäten-EF Core
description: Konfigurieren von schlüssellosen Entitäts Typen mithilfe von Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 9/13/2019
uid: core/modeling/keyless-entity-types
ms.openlocfilehash: 129e24b154ba32583435aeb742dbf478350344e8
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824661"
---
# <a name="keyless-entity-types"></a><span data-ttu-id="4ba00-103">Schlüssellose Entitätstypen</span><span class="sxs-lookup"><span data-stu-id="4ba00-103">Keyless Entity Types</span></span>

> [!NOTE]
> <span data-ttu-id="4ba00-104">Diese Funktion wurde in EF Core 2,1 unter dem Namen der Abfrage Typen hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="4ba00-104">This feature was added in EF Core 2.1 under the name of query types.</span></span> <span data-ttu-id="4ba00-105">In EF Core 3,0 wurde das Konzept in schlüssellose Entitäts Typen umbenannt.</span><span class="sxs-lookup"><span data-stu-id="4ba00-105">In EF Core 3.0 the concept was renamed to keyless entity types.</span></span>

<span data-ttu-id="4ba00-106">Zusätzlich zu regulären Entitäts Typen kann ein EF Core Modell _Schlüssel-Entitäts Typen_enthalten, die verwendet werden können, um Datenbankabfragen für Daten auszuführen, die keine Schlüsselwerte enthalten.</span><span class="sxs-lookup"><span data-stu-id="4ba00-106">In addition to regular entity types, an EF Core model can contain _keyless entity types_, which can be used to carry out database queries against data that doesn't contain key values.</span></span>

## <a name="keyless-entity-types-characteristics"></a><span data-ttu-id="4ba00-107">Merkmale von Keyless-Entitäts Typen</span><span class="sxs-lookup"><span data-stu-id="4ba00-107">Keyless entity types characteristics</span></span>

<span data-ttu-id="4ba00-108">Schlüssellose Entitäts Typen unterstützen viele der gleichen Zuordnungs Funktionen wie reguläre Entitäts Typen wie Vererbungs Zuordnung und Navigations Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="4ba00-108">Keyless entity types support many of the same mapping capabilities as regular entity types, like inheritance mapping and navigation properties.</span></span> <span data-ttu-id="4ba00-109">Für relationale Speicher können sie die Ziel-Datenbank-Objekte und Spalten über die Methoden der fluent-API oder datenanmerkungen konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="4ba00-109">On relational stores, they can configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="4ba00-110">Sie unterscheiden sich jedoch von regulären Entitäts Typen darin, dass Sie:</span><span class="sxs-lookup"><span data-stu-id="4ba00-110">However, they are different from regular entity types in that they:</span></span>

- <span data-ttu-id="4ba00-111">Ein Schlüssel kann nicht definiert werden.</span><span class="sxs-lookup"><span data-stu-id="4ba00-111">Cannot have a key defined.</span></span>
- <span data-ttu-id="4ba00-112">Werden nie nach Änderungen im _dbcontext_ nachverfolgt und daher nie in der Datenbank eingefügt, aktualisiert oder gelöscht.</span><span class="sxs-lookup"><span data-stu-id="4ba00-112">Are never tracked for changes in the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="4ba00-113">Gemäß der Konvention werden nicht ermittelt werden.</span><span class="sxs-lookup"><span data-stu-id="4ba00-113">Are never discovered by convention.</span></span>
- <span data-ttu-id="4ba00-114">Unterstützt nur eine Teilmenge der Navigations Zuordnungsfunktionen, insbesondere:</span><span class="sxs-lookup"><span data-stu-id="4ba00-114">Only support a subset of navigation mapping capabilities, specifically:</span></span>
  - <span data-ttu-id="4ba00-115">Sie können nicht als das prinzipalende der Beziehung fungieren.</span><span class="sxs-lookup"><span data-stu-id="4ba00-115">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="4ba00-116">Sie haben möglicherweise keine Navigation zu besitzende Entitäten.</span><span class="sxs-lookup"><span data-stu-id="4ba00-116">They may not have navigations to owned entities</span></span>
  - <span data-ttu-id="4ba00-117">Sie können nur Verweis Navigations Eigenschaften enthalten, die auf reguläre Entitäten verweisen.</span><span class="sxs-lookup"><span data-stu-id="4ba00-117">They can only contain reference navigation properties pointing to regular entities.</span></span>
  - <span data-ttu-id="4ba00-118">Entitäten können keine Navigations Eigenschaften für Entitäts Typen ohne Schlüssel enthalten.</span><span class="sxs-lookup"><span data-stu-id="4ba00-118">Entities cannot contain navigation properties to keyless entity types.</span></span>
- <span data-ttu-id="4ba00-119">Muss mit `.HasNoKey()` Methoden Aufrufes konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="4ba00-119">Need to be configured with `.HasNoKey()` method call.</span></span>
- <span data-ttu-id="4ba00-120">Kann einer _definierenden Abfrage_zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="4ba00-120">May be mapped to a _defining query_.</span></span> <span data-ttu-id="4ba00-121">Eine definierende Abfrage ist eine im Modell deklarierte Abfrage, die als Datenquelle für einen schlüssellosen Entitätstyp fungiert.</span><span class="sxs-lookup"><span data-stu-id="4ba00-121">A defining query is a query declared in the model that acts as a data source for a keyless entity type.</span></span>

## <a name="usage-scenarios"></a><span data-ttu-id="4ba00-122">Verwendungsszenarien</span><span class="sxs-lookup"><span data-stu-id="4ba00-122">Usage scenarios</span></span>

<span data-ttu-id="4ba00-123">Einige der wichtigsten Verwendungs Szenarien für Typen von schlüssellosen Entitäten sind:</span><span class="sxs-lookup"><span data-stu-id="4ba00-123">Some of the main usage scenarios for keyless entity types are:</span></span>

- <span data-ttu-id="4ba00-124">Fungieren als Rückgabetyp für unformatierte [SQL-Abfragen](xref:core/querying/raw-sql).</span><span class="sxs-lookup"><span data-stu-id="4ba00-124">Serving as the return type for [raw SQL queries](xref:core/querying/raw-sql).</span></span>
- <span data-ttu-id="4ba00-125">Zuordnung zu Daten Bank Sichten, die keinen Primärschlüssel enthalten.</span><span class="sxs-lookup"><span data-stu-id="4ba00-125">Mapping to database views that do not contain a primary key.</span></span>
- <span data-ttu-id="4ba00-126">Zuordnung zu Tabellen, die nicht über einen definierten Primärschlüssel verfügen.</span><span class="sxs-lookup"><span data-stu-id="4ba00-126">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="4ba00-127">Zuordnen zu Abfragen, die im Modell definiert.</span><span class="sxs-lookup"><span data-stu-id="4ba00-127">Mapping to queries defined in the model.</span></span>

## <a name="mapping-to-database-objects"></a><span data-ttu-id="4ba00-128">Zuordnen von Datenbankobjekten</span><span class="sxs-lookup"><span data-stu-id="4ba00-128">Mapping to database objects</span></span>

<span data-ttu-id="4ba00-129">Die Zuordnung eines Entitäts Typs mit einer anderen Tastatur zu einem Datenbankobjekt wird mithilfe der `ToTable` oder `ToView` flüssigen API erreicht.</span><span class="sxs-lookup"><span data-stu-id="4ba00-129">Mapping a keyless entity type to a database object is achieved using the `ToTable` or `ToView` fluent API.</span></span> <span data-ttu-id="4ba00-130">Aus der Perspektive von EF Core, das Datenbankobjekt, das in dieser Methode wird ein _Ansicht_, was bedeutet, dass es als eine nur-Lese Query-Datenquelle behandelt wird kann nicht Ziel von Updates, insert oder delete-Operationen.</span><span class="sxs-lookup"><span data-stu-id="4ba00-130">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="4ba00-131">Dies bedeutet jedoch nicht, dass das Datenbankobjekt tatsächlich eine Daten Bank Sicht sein muss.</span><span class="sxs-lookup"><span data-stu-id="4ba00-131">However, this does not mean that the database object is actually required to be a database view.</span></span> <span data-ttu-id="4ba00-132">Alternativ kann es sich um eine Datenbanktabelle handeln, die als schreibgeschützt behandelt wird.</span><span class="sxs-lookup"><span data-stu-id="4ba00-132">It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="4ba00-133">Im Gegensatz dazu geht EF Core bei regulären Entitäts Typen davon aus, dass ein in der `ToTable`-Methode festgelegtes Datenbankobjekt als _Tabelle_behandelt werden kann. Dies bedeutet, dass es als Abfrage Quelle verwendet werden kann, aber auch für Update-, DELETE-und INSERT-Vorgänge vorgesehen ist.</span><span class="sxs-lookup"><span data-stu-id="4ba00-133">Conversely, for regular entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="4ba00-134">In der Tat können Sie angeben, den Namen einer Datenbanksicht in `ToTable` und alles sollte gut funktionieren, solange die Ansicht für die für die Datenbank aktualisierbar sein, konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="4ba00-134">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

> [!NOTE]
> <span data-ttu-id="4ba00-135">`ToView` geht davon aus, dass das Objekt bereits in der Datenbank vorhanden ist, und wird nicht durch Migrationen erstellt.</span><span class="sxs-lookup"><span data-stu-id="4ba00-135">`ToView` assumes that the object already exists in the database and it won't be created by migrations.</span></span>

## <a name="example"></a><span data-ttu-id="4ba00-136">Beispiel</span><span class="sxs-lookup"><span data-stu-id="4ba00-136">Example</span></span>

<span data-ttu-id="4ba00-137">Das folgende Beispiel zeigt, wie Sie schlüssellose Entitäts Typen verwenden, um eine Daten Bank Sicht abzufragen.</span><span class="sxs-lookup"><span data-stu-id="4ba00-137">The following example shows how to use keyless entity types to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="4ba00-138">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="4ba00-138">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) on GitHub.</span></span>

<span data-ttu-id="4ba00-139">Zuerst definieren wir ein einfaches Blog und Post-Modell:</span><span class="sxs-lookup"><span data-stu-id="4ba00-139">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Entities)]

<span data-ttu-id="4ba00-140">Als Nächstes definieren wir eine einfache Datenbank-Ansicht, die wir Abfragen die Anzahl an Beiträgen, die jeden Blog zugeordnet werden kann:</span><span class="sxs-lookup"><span data-stu-id="4ba00-140">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#View)]

<span data-ttu-id="4ba00-141">Als Nächstes definieren wir eine Klasse, um das Ergebnis aus der Datenbankansicht enthalten soll:</span><span class="sxs-lookup"><span data-stu-id="4ba00-141">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#KeylessEntityType)]

<span data-ttu-id="4ba00-142">Als Nächstes konfigurieren wir den Entitätstyp "Schlüssel" in " _onmodelcreating_ " mithilfe der `HasNoKey`-API.</span><span class="sxs-lookup"><span data-stu-id="4ba00-142">Next, we configure the keyless entity type in _OnModelCreating_ using the `HasNoKey` API.</span></span>
<span data-ttu-id="4ba00-143">Wir verwenden die fließende Konfigurations-API, um die Zuordnung für den Entitätstyp "Schlüssel" zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="4ba00-143">We use fluent configuration API to configure the mapping for the keyless entity type:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Configuration)]

<span data-ttu-id="4ba00-144">Als Nächstes konfigurieren wir die `DbContext`, um die `DbSet<T>`einzuschließen:</span><span class="sxs-lookup"><span data-stu-id="4ba00-144">Next, we configure the `DbContext` to include the `DbSet<T>`:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#DbSet)]

<span data-ttu-id="4ba00-145">Schließlich können wir die Datenbanksicht auf die übliche Weise Abfragen:</span><span class="sxs-lookup"><span data-stu-id="4ba00-145">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="4ba00-146">Beachten Sie, dass wir auch eine Kontext Stufen-Abfrage Eigenschaft (dbset) definiert haben, die als Stamm für Abfragen dieses Typs fungiert.</span><span class="sxs-lookup"><span data-stu-id="4ba00-146">Note we have also defined a context level query property (DbSet) to act as a root for queries against this type.</span></span>
