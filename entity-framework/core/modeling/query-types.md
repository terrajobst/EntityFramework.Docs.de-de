---
title: Abfragetypen - EF Core
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: 4e02f106e086d243b23a60c02838f32555be210e
ms.sourcegitcommit: 26f33758c47399ae933f22fec8e1d19fa7d2c0b7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/19/2018
---
# <a name="query-types"></a><span data-ttu-id="46456-102">Abfragetypen</span><span class="sxs-lookup"><span data-stu-id="46456-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="46456-103">Dieses Feature ist neu in EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="46456-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="46456-104">Zusätzlich zu den Entitätstypen, kann ein Modell EF Core enthalten _Abfragen Typen_, die zum Ausführen von Datenbankabfragen für Daten, die Entitätstypen zugeordnet wird nicht verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="46456-104">In addition to entity types, an EF Core model can contain _query types_, which can be used to carry out database queries against data that isn't mapped to entity types.</span></span>

<span data-ttu-id="46456-105">Abfragetypen haben viele ähnlichkeiten mit Entitätstypen:</span><span class="sxs-lookup"><span data-stu-id="46456-105">Query types have many similarities with entity types:</span></span>

- <span data-ttu-id="46456-106">Sie können auch hinzugefügt werden, das Modell entweder in `OnModelCreating`, oder über eine "set"-Eigenschaft auf eine abgeleitete _DbContext_.</span><span class="sxs-lookup"><span data-stu-id="46456-106">They can also be added to the model either in `OnModelCreating`, or via a "set" property on a derived _DbContext_.</span></span>
- <span data-ttu-id="46456-107">Sie unterstützen viele derselben Mappingfunktionen, wie Vererbung, Zuordnen von Navigationseigenschaften (siehe unten Einschränkungen) und auf den relationalen Speicher, die Möglichkeit, die Ziel-Datenbankobjekte und Spalten über die fluent-API-Methoden oder datenanmerkungen konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="46456-107">They support many of the same mapping capabilities, like inheritance mapping, navigation properties (see limitations below) and, on relational stores, the ability to configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="46456-108">Jedoch Typen verschieden von Entität in, das sie:</span><span class="sxs-lookup"><span data-stu-id="46456-108">However they are different from entity types in that they:</span></span>

- <span data-ttu-id="46456-109">Benötigen Sie keinen Schlüssel definiert werden.</span><span class="sxs-lookup"><span data-stu-id="46456-109">Do not require a key to be defined.</span></span>
- <span data-ttu-id="46456-110">Nie für Änderungen nachverfolgt werden, auf die _DbContext_ und daher nie eingefügt, aktualisiert oder gelöscht werden in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="46456-110">Are never tracked for changes on the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="46456-111">Gemäß der Konvention werden nie ermittelt werden.</span><span class="sxs-lookup"><span data-stu-id="46456-111">Are never discovered by convention.</span></span>
- <span data-ttu-id="46456-112">Unterstützen nur eine Teilmenge der Navigation Mappingfunktionen – insbesondere, die sie möglicherweise nie dienen als das prinzipalende der Beziehung.</span><span class="sxs-lookup"><span data-stu-id="46456-112">Only support a subset of navigation mapping capabilities - Specifically, they may never act as the principal end of a relationship.</span></span>
- <span data-ttu-id="46456-113">Adressiert sind, auf die _ModelBuilder_ mithilfe der `Query` Methode statt über das `Entity` Methode.</span><span class="sxs-lookup"><span data-stu-id="46456-113">Are addressed on the _ModelBuilder_ using the `Query` method rather than the `Entity` method.</span></span>
- <span data-ttu-id="46456-114">Zugeordnet sind, auf die _DbContext_ über Eigenschaften des Typs `DbQuery<T>` statt `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="46456-114">Are mapped on the _DbContext_ through properties of type `DbQuery<T>` rather than `DbSet<T>`</span></span>
- <span data-ttu-id="46456-115">Zugeordnet sind, auf der Datenbankobjekte, die mit der `ToView` -Methode, anstatt `ToTable`.</span><span class="sxs-lookup"><span data-stu-id="46456-115">Are mapped to database objects using the `ToView` method, rather than `ToTable`.</span></span>
- <span data-ttu-id="46456-116">Zugeordnet werden kann ein _Abfrage definieren_ – eine Abfrage definieren einer sekundären Abfrage, die im Modell, die eine Datenquelle für den Abfragetyp fungiert deklariert ist.</span><span class="sxs-lookup"><span data-stu-id="46456-116">May be mapped to a _defining query_ - A defining query is a secondary query declared in the model that acts a data source for a query type.</span></span>

<span data-ttu-id="46456-117">Einige der wichtigsten Verwendungsszenarien für Abfragetypen sind:</span><span class="sxs-lookup"><span data-stu-id="46456-117">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="46456-118">Dient als Rückgabetyp für ad-hoc- `FromSql()` Abfragen.</span><span class="sxs-lookup"><span data-stu-id="46456-118">Serving as the return type for ad hoc `FromSql()` queries.</span></span>
- <span data-ttu-id="46456-119">Zuordnung zu Datenbanksichten.</span><span class="sxs-lookup"><span data-stu-id="46456-119">Mapping to database views.</span></span>
- <span data-ttu-id="46456-120">Zuordnung zu Tabellen, die nicht über einen definierten Primärschlüssel verfügen.</span><span class="sxs-lookup"><span data-stu-id="46456-120">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="46456-121">Zuordnen zu Abfragen, die im Modell definiert.</span><span class="sxs-lookup"><span data-stu-id="46456-121">Mapping to queries defined in the model.</span></span>

> [!TIP]
> <span data-ttu-id="46456-122">Zuordnungstyp eine Abfrage mit einem Datenbankobjekt erfolgt mithilfe der `ToView` fluent-API.</span><span class="sxs-lookup"><span data-stu-id="46456-122">Mapping a query type to a database object is achieved using the `ToView` fluent API.</span></span> <span data-ttu-id="46456-123">Aus der Perspektive des Kerns EF wird in dieser Methode angegebene Datenbankobjekt ist eine _Ansicht_, dies bedeutet, dass es als Abfragequelle für nur-Lese behandelt kann nicht Ziel von Updates, insert oder delete-Operationen.</span><span class="sxs-lookup"><span data-stu-id="46456-123">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="46456-124">Dies bedeutet jedoch nicht, dass das Datenbankobjekt tatsächlich erforderlich ist, damit eine Datenbanksicht werden - es kann auch eine Datenbanktabelle, die als schreibgeschützt behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="46456-124">However, this does not mean that the database object is actually required to be a database view - It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="46456-125">Umgekehrt für Entitätstypen, EF Core setzt voraus, dass ein Datenbankobjekt in angegeben die `ToTable` Methode behandelt werden kann, als ein _Tabelle_, was bedeutet, dass es als Abfragequelle für verwendet werden können, aber auch das Ziel aktualisieren, löschen und einfügen DDL-Vorgänge.</span><span class="sxs-lookup"><span data-stu-id="46456-125">Conversely, for entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="46456-126">Sie können in der Tat Geben Sie den Namen einer Datenbanksicht in `ToTable` und alles sollte einwandfrei funktionieren, solange die Ansicht für die Datenbank aktualisierbar konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="46456-126">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

## <a name="example"></a><span data-ttu-id="46456-127">Beispiel</span><span class="sxs-lookup"><span data-stu-id="46456-127">Example</span></span>

<span data-ttu-id="46456-128">Im folgende Beispiel wird gezeigt, wie Abfragetyp zum Abfragen einer Datenbankansicht verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="46456-128">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="46456-129">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="46456-129">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="46456-130">Zuerst definieren wir ein einfache Blog und Post-Modell:</span><span class="sxs-lookup"><span data-stu-id="46456-130">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="46456-131">Als Nächstes definieren wir eine einfache Datenbank-Sicht, die uns, Fragen Sie die Anzahl der Beiträge, die jeden Blog zugeordnet werden kann:</span><span class="sxs-lookup"><span data-stu-id="46456-131">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

<span data-ttu-id="46456-132">Als Nächstes definieren wir eine Klasse, um das Ergebnis aus der Datenbankansicht aufnehmen:</span><span class="sxs-lookup"><span data-stu-id="46456-132">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="46456-133">Als Nächstes konfigurieren wir den Abfragetyp in _OnModelCreating_ mithilfe der `modelBuilder.Query<T>` API.</span><span class="sxs-lookup"><span data-stu-id="46456-133">Next, we configure the query type in _OnModelCreating_ using the `modelBuilder.Query<T>` API.</span></span>
<span data-ttu-id="46456-134">Wir verwenden standard fluent-Konfigurations-APIs, um die Zuordnung für den Abfragetyp zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="46456-134">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="46456-135">Schließlich können wir die Datenbanksicht auf die übliche Weise Abfragen:</span><span class="sxs-lookup"><span data-stu-id="46456-135">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="46456-136">Beachten Sie, dass wir auch eine Ebene Abfrage Kontexteigenschaft (DbQuery) fungieren als Stamm für Abfragen auf diesen Typ definiert haben.</span><span class="sxs-lookup"><span data-stu-id="46456-136">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
