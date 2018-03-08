---
title: Abfragetypen - EF Core
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: 19a371c65da33e8209cc1ab3423a67c34ddae61e
ms.sourcegitcommit: fc68321c211aca38f7b9dc3a75677c6ca1b2524b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2018
---
# <a name="query-types"></a><span data-ttu-id="b15b2-102">Abfragetypen</span><span class="sxs-lookup"><span data-stu-id="b15b2-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="b15b2-103">Dieses Feature ist neu in EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="b15b2-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="b15b2-104">Abfragetypen können nur-Lese Abfrage Ergebnistypen, die dem EF-Core-Modell hinzugefügt werden können.</span><span class="sxs-lookup"><span data-stu-id="b15b2-104">Query Types are read-only query result types that can be added to the EF Core model.</span></span> <span data-ttu-id="b15b2-105">Abfragetypen aktivieren die Ad-hoc-Abfragen (z. B. anonyme Typen), sondern sind flexibler, da sie die angegebene Zuordnungskonfiguration aufweisen können.</span><span class="sxs-lookup"><span data-stu-id="b15b2-105">Query Types enable ad-hoc querying (like anonymous types), but are more flexible because they can have mapping configuration specified.</span></span>

<span data-ttu-id="b15b2-106">Sie sind konzeptionell identisch mit Entitätstypen:</span><span class="sxs-lookup"><span data-stu-id="b15b2-106">They are conceptually similar to Entity Types in that:</span></span>

- <span data-ttu-id="b15b2-107">Sie sind POCO C#-Typen, die entweder im Modell hinzugefügt werden ```OnModelCreating``` mithilfe der ```ModelBuilder.Query``` -Methode, oder über eine DbContext ""-seteigenschaft (Abfrage eine solche Eigenschaft Datentypen als typisiert ist ```DbQuery<T>``` statt ```DbSet<T>```).</span><span class="sxs-lookup"><span data-stu-id="b15b2-107">They are POCO C# types that are added to the model, either in ```OnModelCreating``` using the ```ModelBuilder.Query``` method, or via a DbContext "set" property (for query types such a property is typed as ```DbQuery<T>``` rather that ```DbSet<T>```).</span></span>
- <span data-ttu-id="b15b2-108">Sie unterstützt viele der gleichen Mappingfunktionen als reguläre Entitätstypen.</span><span class="sxs-lookup"><span data-stu-id="b15b2-108">They support much of the same mapping capabilities as regular entity types.</span></span> <span data-ttu-id="b15b2-109">Z. B. Vererbungsmapping, Navigationen (siehe unten Limitiations) und auf den relationalen Speicher, die Möglichkeit, die Ziel-Datenbank-Schemaobjekten über konfigurieren ```ToTable```, ```HasColumn``` fluent-API-Methoden (oder datenanmerkungen).</span><span class="sxs-lookup"><span data-stu-id="b15b2-109">For example, inheritance mapping, navigations (see limitiations below) and, on relational stores, the ability to configure the target database schema objects via ```ToTable```, ```HasColumn``` fluent-api methods (or data annotations).</span></span>

<span data-ttu-id="b15b2-110">Abfragetypen unterscheiden sich von der Entität Typen in, das sie:</span><span class="sxs-lookup"><span data-stu-id="b15b2-110">Query Types are different from entity types in that they:</span></span>

- <span data-ttu-id="b15b2-111">Benötigen Sie keinen Schlüssel definiert werden.</span><span class="sxs-lookup"><span data-stu-id="b15b2-111">Do not require a key to be defined.</span></span>
- <span data-ttu-id="b15b2-112">Durch das System zur Änderungsnachverfolgung werden nie nachverfolgt werden.</span><span class="sxs-lookup"><span data-stu-id="b15b2-112">Are never tracked by the Change Tracker.</span></span>
- <span data-ttu-id="b15b2-113">Gemäß der Konvention werden nie ermittelt werden.</span><span class="sxs-lookup"><span data-stu-id="b15b2-113">Are never discovered by convention.</span></span>
- <span data-ttu-id="b15b2-114">Unterstützen nur eine Teilmenge der Navigation Mappingfunktionen – insbesondere, die sie möglicherweise nie dienen als das prinzipalende der Beziehung.</span><span class="sxs-lookup"><span data-stu-id="b15b2-114">Only support a subset of navigation mapping capabilities - Specifically, they may never act as the principal end of a relationship.</span></span>
- <span data-ttu-id="b15b2-115">Zugeordnet werden kann, um eine _Abfrage definieren_ -eine definierende Abfrage ist eine sekundäre Abfrage, die eine Datenquelle für den Abfragetyp dient.</span><span class="sxs-lookup"><span data-stu-id="b15b2-115">May be mapped to a _defining query_ - A Defining Query is a secondary query that acts a data source for a Query Type.</span></span>

<span data-ttu-id="b15b2-116">Einige der wichtigsten Verwendungsszenarien für Abfragetypen sind:</span><span class="sxs-lookup"><span data-stu-id="b15b2-116">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="b15b2-117">Zuordnung zu Datenbanksichten.</span><span class="sxs-lookup"><span data-stu-id="b15b2-117">Mapping to database views.</span></span>
- <span data-ttu-id="b15b2-118">Zuordnung zu Tabellen, die nicht über einen definierten Primärschlüssel verfügen.</span><span class="sxs-lookup"><span data-stu-id="b15b2-118">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="b15b2-119">Dient als Rückgabetyp für ad-hoc- ```FromSql()``` Abfragen.</span><span class="sxs-lookup"><span data-stu-id="b15b2-119">Serving as the return type for ad hoc ```FromSql()``` queries.</span></span>
- <span data-ttu-id="b15b2-120">Zuordnen zu Abfragen, die im Modell definiert.</span><span class="sxs-lookup"><span data-stu-id="b15b2-120">Mapping to queries defined in the model.</span></span>

> [!TIP]
> <span data-ttu-id="b15b2-121">Zuordnen von Abfragetyp zu einer Datenbankansicht erfolgt mithilfe der ```ToTable``` fluent-API.</span><span class="sxs-lookup"><span data-stu-id="b15b2-121">Mapping a query type to a database view is achieved using the ```ToTable``` fluent API.</span></span>

## <a name="example"></a><span data-ttu-id="b15b2-122">Beispiel</span><span class="sxs-lookup"><span data-stu-id="b15b2-122">Example</span></span>

<span data-ttu-id="b15b2-123">Im folgende Beispiel wird gezeigt, wie Abfragetyp zum Abfragen einer Datenbankansicht verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="b15b2-123">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="b15b2-124">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="b15b2-124">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="b15b2-125">Zuerst definieren wir ein einfache Blog und Post-Modell:</span><span class="sxs-lookup"><span data-stu-id="b15b2-125">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="b15b2-126">Als Nächstes definieren wir eine einfache Datenbank-Sicht, die uns, Fragen Sie die Anzahl der Beiträge, die jeden Blog zugeordnet werden kann:</span><span class="sxs-lookup"><span data-stu-id="b15b2-126">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

<span data-ttu-id="b15b2-127">Als Nächstes definieren wir eine Klasse, um das Ergebnis aus der Datenbankansicht aufnehmen:</span><span class="sxs-lookup"><span data-stu-id="b15b2-127">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="b15b2-128">Als Nächstes konfigurieren wir den Abfragetyp in _OnModelCreating_ mithilfe der ```modelBuilder.Query<T>``` API.</span><span class="sxs-lookup"><span data-stu-id="b15b2-128">Next, we configure the query type in _OnModelCreating_ using the ```modelBuilder.Query<T>``` API.</span></span>
<span data-ttu-id="b15b2-129">Wir verwenden standard fluent-Konfigurations-APIs, um die Zuordnung für den Abfragetyp zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="b15b2-129">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="b15b2-130">Schließlich können wir die Datenbanksicht auf die übliche Weise Abfragen:</span><span class="sxs-lookup"><span data-stu-id="b15b2-130">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="b15b2-131">Beachten Sie, dass wir auch eine Ebene Abfrage Kontexteigenschaft (DbQuery) fungieren als Stamm für Abfragen auf diesen Typ definiert haben.</span><span class="sxs-lookup"><span data-stu-id="b15b2-131">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
