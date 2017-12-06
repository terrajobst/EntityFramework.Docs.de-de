---
title: "Portieren von EF6 auf EF-Core - Anforderungen zu überprüfen"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 2f45039e63546d266ec6ce0bfa66ef7e9fb3d7e7
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a><span data-ttu-id="82297-102">Vor dem Portieren von EF6 auf EF Core: Überprüfen Sie den Anforderungen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="82297-102">Before porting from EF6 to EF Core: Validate your Application's Requirements</span></span>

<span data-ttu-id="82297-103">Bevor Sie mit der Portierung von beginnen ist es wichtig zu überprüfen, ob EF Core Anforderungen der Daten für Ihre Anwendung entspricht.</span><span class="sxs-lookup"><span data-stu-id="82297-103">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="82297-104">Fehlende Funktionen</span><span class="sxs-lookup"><span data-stu-id="82297-104">Missing features</span></span>

<span data-ttu-id="82297-105">Stellen Sie sicher, dass EF Core alle Funktionen verfügt, die Sie in Ihrer Anwendung verwenden müssen.</span><span class="sxs-lookup"><span data-stu-id="82297-105">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="82297-106">Finden Sie unter [Funktionsvergleich](../features.md) für einen ausführlichen Vergleich wie die Funktionen von EF-Core mit EF6 verglichen.</span><span class="sxs-lookup"><span data-stu-id="82297-106">See [Feature Comparison](../features.md) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="82297-107">Wenn alle erforderlichen Funktionen fehlen, stellen Sie sicher, dass für das Fehlen dieser Funktionen vor dem Portieren auf EF Core kompensiert werden können.</span><span class="sxs-lookup"><span data-stu-id="82297-107">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="82297-108">Verändertes Programmverhalten</span><span class="sxs-lookup"><span data-stu-id="82297-108">Behavior changes</span></span>

<span data-ttu-id="82297-109">Dies ist eine nicht erschöpfende Liste einiger Änderungen im Verhalten zwischen EF6 und EF Core.</span><span class="sxs-lookup"><span data-stu-id="82297-109">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="82297-110">Es ist wichtig zu beachten Sie den Port die Anwendung halten sich ändernder wie können die Anwendung verhält sich aber wird nicht angezeigt werden, als Kompilierungsfehler nach dem Austausch und EF Core.</span><span class="sxs-lookup"><span data-stu-id="82297-110">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="82297-111">DbSet.Add/Attach und Graph-Verhalten</span><span class="sxs-lookup"><span data-stu-id="82297-111">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="82297-112">In EF6 Aufrufen von `DbSet.Add()` für eine Entität führt in eine rekursive Suche für alle Entitäten, die in die Navigationseigenschaften verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="82297-112">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="82297-113">Alle benutzerentitäten, die gefunden werden, und nicht bereits vom Kontext, nachverfolgt werden auch als hinzugefügt markiert werden.</span><span class="sxs-lookup"><span data-stu-id="82297-113">Any entities that are found, and are not already tracked by the context, are also be marked as added.</span></span> <span data-ttu-id="82297-114">`DbSet.Attach()`verhält sich, mit der Ausnahme alle Entitäten gekennzeichnet sind als unverändert.</span><span class="sxs-lookup"><span data-stu-id="82297-114">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="82297-115">**EF Core führt eine rekursive Suche in ähnlichen, jedoch mit einigen etwas anderen Regeln.**</span><span class="sxs-lookup"><span data-stu-id="82297-115">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="82297-116">Die Stammentität befindet sich immer im angeforderten Status (für hinzugefügt `DbSet.Add` und unverändert `DbSet.Attach`).</span><span class="sxs-lookup"><span data-stu-id="82297-116">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="82297-117">**Für Entitäten, die während der rekursiven Suche von Navigationseigenschaften gefunden werden:**</span><span class="sxs-lookup"><span data-stu-id="82297-117">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="82297-118">**Wenn der Primärschlüssel der Entität speichern generierter ist**</span><span class="sxs-lookup"><span data-stu-id="82297-118">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="82297-119">Wenn der Primärschlüssel nicht auf einen Wert festgelegt ist, wird der Status zu hinzugefügte festgelegt.</span><span class="sxs-lookup"><span data-stu-id="82297-119">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="82297-120">Der Primärschlüsselwert gilt als "nicht konfiguriert", wenn es der CLR-Standardwert für den Eigenschaftentyp zugewiesen ist (d. h. `0` für `int`, `null` für `string`usw..).</span><span class="sxs-lookup"><span data-stu-id="82297-120">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (i.e. `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="82297-121">Wenn der Primärschlüssel auf einen Wert festgelegt ist, wird der Status auf unverändert festgelegt.</span><span class="sxs-lookup"><span data-stu-id="82297-121">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="82297-122">Wenn der Primärschlüssel nicht datenbankgeneriert ist, wird die Entität im gleichen Zustand wie der Stamm gespeichert.</span><span class="sxs-lookup"><span data-stu-id="82297-122">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="82297-123">Initialisierung der ersten Datenbank Code</span><span class="sxs-lookup"><span data-stu-id="82297-123">Code First database initialization</span></span>

<span data-ttu-id="82297-124">**EF6 verfügt über eine beträchtliche Menge an Magic, die ausgeführt wird, um die Verbindung mit der Datenbank auswählen und zum Initialisieren der Datenbank. Diese Regeln gehören:**</span><span class="sxs-lookup"><span data-stu-id="82297-124">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="82297-125">Wenn keine Konfiguration ausgeführt wird, wählt EF6 eine Datenbank auf SQL Express oder LocalDb.</span><span class="sxs-lookup"><span data-stu-id="82297-125">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="82297-126">Wenn eine Verbindungszeichenfolge mit dem gleichen Namen wie der Kontext in den Clientanwendungen ist `App/Web.config` -Datei wird diese Verbindung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="82297-126">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="82297-127">Wenn die Datenbank nicht vorhanden ist, wird er erstellt.</span><span class="sxs-lookup"><span data-stu-id="82297-127">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="82297-128">Wenn keine der Tabellen aus dem Modell in der Datenbank vorhanden sind, wird das Schema für das aktuelle Modell mit der Datenbank hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="82297-128">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="82297-129">Wenn es sich bei Migrationen aktiviert sind, werden sie verwendet, zum Erstellen der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="82297-129">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="82297-130">Wenn die Datenbank vorhanden ist und EF6 hatte zuvor das Schema erstellt, wird das Schema für die Kompatibilität mit dem aktuellen Modell überprüft.</span><span class="sxs-lookup"><span data-stu-id="82297-130">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="82297-131">Eine Ausnahme wird ausgelöst, wenn das Modell geändert hat, seit der Erstellung des Schemas.</span><span class="sxs-lookup"><span data-stu-id="82297-131">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="82297-132">**EF Core führt keine dieser Magic.**</span><span class="sxs-lookup"><span data-stu-id="82297-132">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="82297-133">Verbindung mit der Datenbank muss explizit in Code konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="82297-133">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="82297-134">Keine Initialisierung ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="82297-134">No initialization is performed.</span></span> <span data-ttu-id="82297-135">Verwenden Sie `DbContext.Database.Migrate()` anzuwendende Migrationen (oder `DbContext.Database.EnsureCreated()` und `EnsureDeleted()` zu erstellen und die Datenbank löschen ohne Verwendung von Migrationen).</span><span class="sxs-lookup"><span data-stu-id="82297-135">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="82297-136">Erste Tabelle Namenskonvention</span><span class="sxs-lookup"><span data-stu-id="82297-136">Code First table naming convention</span></span>

<span data-ttu-id="82297-137">EF6, wird der Klassenname für die Entität über ein Pluralisierung-Dienst, um den Standardnamen für die Tabelle zu berechnen, dem die Entität zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="82297-137">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="82297-138">EF Core verwendet den Namen des der `DbSet` -Eigenschaft, die die Entität für den abgeleiteten Kontext verfügbar gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="82297-138">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="82297-139">Wenn die Entität keine `DbSet` -Eigenschaft, und klicken Sie dann auf den Klassennamen verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="82297-139">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
