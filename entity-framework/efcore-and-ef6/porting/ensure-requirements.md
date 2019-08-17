---
title: Portieren von EF6 auf EF Core-Validate-Anforderungen
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 07caa39e8a56db71e493331ea9f0286309abc52a
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565346"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a><span data-ttu-id="6c42c-102">Vor dem Portieren von EF6 auf EF Core: Überprüfen der Anforderungen Ihrer Anwendung</span><span class="sxs-lookup"><span data-stu-id="6c42c-102">Before porting from EF6 to EF Core: Validate your Application's Requirements</span></span>

<span data-ttu-id="6c42c-103">Bevor Sie den Portierungs Prozess starten, müssen Sie überprüfen, ob EF Core den Datenzugriffs Anforderungen für Ihre Anwendung entspricht.</span><span class="sxs-lookup"><span data-stu-id="6c42c-103">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="6c42c-104">Fehlende Features</span><span class="sxs-lookup"><span data-stu-id="6c42c-104">Missing features</span></span>

<span data-ttu-id="6c42c-105">Stellen Sie sicher, dass EF Core über alle Funktionen verfügt, die Sie in Ihrer Anwendung verwenden müssen.</span><span class="sxs-lookup"><span data-stu-id="6c42c-105">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="6c42c-106">Unter [Funktions Vergleich](../features.md) finden Sie einen ausführlichen Vergleich, wie die in EF Core festgelegte Funktion mit EF6 verglichen wird.</span><span class="sxs-lookup"><span data-stu-id="6c42c-106">See [Feature Comparison](../features.md) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="6c42c-107">Wenn erforderliche Features fehlen, stellen Sie sicher, dass das Fehlen dieser Features vor dem Portieren auf EF Core kompensiert werden kann.</span><span class="sxs-lookup"><span data-stu-id="6c42c-107">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="6c42c-108">Verhaltensänderungen</span><span class="sxs-lookup"><span data-stu-id="6c42c-108">Behavior changes</span></span>

<span data-ttu-id="6c42c-109">Dies ist eine nicht vollständige Liste einiger Verhaltensänderungen zwischen EF6 und EF Core.</span><span class="sxs-lookup"><span data-stu-id="6c42c-109">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="6c42c-110">Beachten Sie, dass Sie diese Vorgehensweise als Port für Ihre Anwendung behalten müssen, da Sie sich möglicherweise auf die Art und Weise verhält, in der sich Ihre Anwendung verhält, aber nicht als Kompilierungsfehler angezeigt wird, nachdem der Austausch auf EF Core</span><span class="sxs-lookup"><span data-stu-id="6c42c-110">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="6c42c-111">Dbset. Add/Attach und Graph-Verhalten</span><span class="sxs-lookup"><span data-stu-id="6c42c-111">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="6c42c-112">In EF6 ergibt das `DbSet.Add()` Aufrufen von für eine Entität eine rekursive Suche nach allen Entitäten, auf die in ihren Navigations Eigenschaften verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="6c42c-112">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="6c42c-113">Alle Entitäten, die gefunden wurden und nicht bereits vom Kontext nachverfolgt wurden, werden ebenfalls als hinzugefügt gekennzeichnet.</span><span class="sxs-lookup"><span data-stu-id="6c42c-113">Any entities that are found, and are not already tracked by the context, are also marked as added.</span></span> <span data-ttu-id="6c42c-114">`DbSet.Attach()`verhält sich identisch, mit dem Unterschied, dass alle Entitäten als unverändert markiert sind.</span><span class="sxs-lookup"><span data-stu-id="6c42c-114">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="6c42c-115">**EF Core führt eine ähnliche rekursive Suche durch, aber mit etwas anderen Regeln.**</span><span class="sxs-lookup"><span data-stu-id="6c42c-115">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="6c42c-116">Die Stamm Entität befindet sich immer im angeforderten Zustand (für `DbSet.Add` hinzugefügt und `DbSet.Attach`unverändert).</span><span class="sxs-lookup"><span data-stu-id="6c42c-116">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="6c42c-117">**Für Entitäten, die während der rekursiven Suche von Navigations Eigenschaften gefunden werden:**</span><span class="sxs-lookup"><span data-stu-id="6c42c-117">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="6c42c-118">**Wenn der Primärschlüssel der Entität Speicher generiert ist**</span><span class="sxs-lookup"><span data-stu-id="6c42c-118">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="6c42c-119">Wenn der Primärschlüssel nicht auf einen Wert festgelegt ist, wird der Zustand auf Added festgelegt.</span><span class="sxs-lookup"><span data-stu-id="6c42c-119">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="6c42c-120">Der Primärschlüssel Wert wird als "nicht festgelegt" betrachtet, wenn ihm der CLR-Standardwert für den Eigenschaftentyp `0` zugewiesen wird `null` ( `string`z. b. für `int`, für usw.).</span><span class="sxs-lookup"><span data-stu-id="6c42c-120">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (for example, `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="6c42c-121">Wenn der Primärschlüssel auf einen Wert festgelegt ist, wird der Status unverändert festgelegt.</span><span class="sxs-lookup"><span data-stu-id="6c42c-121">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="6c42c-122">Wenn der Primärschlüssel nicht von der Datenbank generiert wird, wird die Entität in denselben Zustand wie der Stamm eingefügt.</span><span class="sxs-lookup"><span data-stu-id="6c42c-122">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="6c42c-123">Code First Daten Bank Initialisierung</span><span class="sxs-lookup"><span data-stu-id="6c42c-123">Code First database initialization</span></span>

<span data-ttu-id="6c42c-124">**EF6 hat eine beträchtliche Menge an Magie, die Sie beim Auswählen der Datenbankverbindung und beim Initialisieren der Datenbank durchführt. Zu diesen Regeln gehören:**</span><span class="sxs-lookup"><span data-stu-id="6c42c-124">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="6c42c-125">Wenn keine Konfiguration durchgeführt wird, wählt EF6 eine Datenbank in SQL Express oder localdb aus.</span><span class="sxs-lookup"><span data-stu-id="6c42c-125">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="6c42c-126">Wenn sich eine Verbindungs Zeichenfolge mit dem gleichen Namen wie der Kontext in `App/Web.config` der Anwendungsdatei befindet, wird diese Verbindung verwendet.</span><span class="sxs-lookup"><span data-stu-id="6c42c-126">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="6c42c-127">Wenn die Datenbank nicht vorhanden ist, wird Sie erstellt.</span><span class="sxs-lookup"><span data-stu-id="6c42c-127">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="6c42c-128">Wenn keine der Tabellen aus dem Modell in der Datenbank vorhanden ist, wird das Schema für das aktuelle Modell der Datenbank hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="6c42c-128">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="6c42c-129">Wenn Migrationen aktiviert sind, werden Sie verwendet, um die Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="6c42c-129">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="6c42c-130">Wenn die Datenbank vorhanden ist und das Schema bereits von EF6 erstellt wurde, wird das Schema auf Kompatibilität mit dem aktuellen Modell geprüft.</span><span class="sxs-lookup"><span data-stu-id="6c42c-130">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="6c42c-131">Eine Ausnahme wird ausgelöst, wenn sich das Modell seit der Erstellung des Schemas geändert hat.</span><span class="sxs-lookup"><span data-stu-id="6c42c-131">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="6c42c-132">**EF Core führt diese Magie nicht aus.**</span><span class="sxs-lookup"><span data-stu-id="6c42c-132">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="6c42c-133">Die Datenbankverbindung muss explizit im Code konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="6c42c-133">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="6c42c-134">Es wird keine Initialisierung ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="6c42c-134">No initialization is performed.</span></span> <span data-ttu-id="6c42c-135">Sie müssen verwenden `DbContext.Database.Migrate()` , um Migrationen anzuwenden `DbContext.Database.EnsureCreated()` ( `EnsureDeleted()` oder und um die Datenbank ohne Verwendung von Migrationen zu erstellen bzw. zu löschen).</span><span class="sxs-lookup"><span data-stu-id="6c42c-135">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="6c42c-136">Benennungs Konvention für Code First Tabelle</span><span class="sxs-lookup"><span data-stu-id="6c42c-136">Code First table naming convention</span></span>

<span data-ttu-id="6c42c-137">EF6 führt den Entitäts Klassennamen über einen pluralisierungs Dienst aus, um den Standard Tabellennamen zu berechnen, dem die Entität zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="6c42c-137">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="6c42c-138">EF Core verwendet den Namen `DbSet` der Eigenschaft, in der die Entität für den abgeleiteten Kontext verfügbar gemacht wird.</span><span class="sxs-lookup"><span data-stu-id="6c42c-138">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="6c42c-139">Wenn die Entität nicht über eine `DbSet` -Eigenschaft verfügt, wird der Klassenname verwendet.</span><span class="sxs-lookup"><span data-stu-id="6c42c-139">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
