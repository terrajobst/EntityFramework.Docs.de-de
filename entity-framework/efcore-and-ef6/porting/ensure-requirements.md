---
title: 'Portieren von EF6 auf EF Core: Überprüfen von Anforderungen'
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3b66f3c-9d10-4974-a090-8ad093c9a53d
uid: efcore-and-ef6/porting/ensure-requirements
ms.openlocfilehash: 65bdc8bb9574d37db697aa47c8e8c480cefcb4f7
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949113"
---
# <a name="before-porting-from-ef6-to-ef-core-validate-your-applications-requirements"></a><span data-ttu-id="22d48-102">Vor dem Portieren von EF 6 nach EF Core: Überprüfen Sie die Anforderungen Ihrer Anwendung</span><span class="sxs-lookup"><span data-stu-id="22d48-102">Before porting from EF6 to EF Core: Validate your Application's Requirements</span></span>

<span data-ttu-id="22d48-103">Vor der Installation des portiervorgang ist es wichtig, um sicherzustellen, dass EF Core die Anforderungen für den Datenzugriff für Ihre Anwendung erfüllt.</span><span class="sxs-lookup"><span data-stu-id="22d48-103">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="22d48-104">Fehlende features</span><span class="sxs-lookup"><span data-stu-id="22d48-104">Missing features</span></span>

<span data-ttu-id="22d48-105">Stellen Sie sicher, dass EF Core verfügt über alle Funktionen, die Sie in Ihrer Anwendung verwenden müssen.</span><span class="sxs-lookup"><span data-stu-id="22d48-105">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="22d48-106">Finden Sie unter [Funktionsvergleich](../features.md) für einen detaillierten Vergleich wie der Features in EF Core im Vergleich zu EF6.</span><span class="sxs-lookup"><span data-stu-id="22d48-106">See [Feature Comparison](../features.md) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="22d48-107">Wenn alle erforderlichen Features fehlen, stellen Sie sicher, dass Sie den Mangel an diese Funktionen vor dem Portieren nach EF Core beheben können.</span><span class="sxs-lookup"><span data-stu-id="22d48-107">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="22d48-108">Verhaltensänderungen</span><span class="sxs-lookup"><span data-stu-id="22d48-108">Behavior changes</span></span>

<span data-ttu-id="22d48-109">Dies ist eine nicht erschöpfende Liste einiger Änderungen im Verhalten zwischen EF6 und EF Core.</span><span class="sxs-lookup"><span data-stu-id="22d48-109">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="22d48-110">Es ist wichtig zu beachten Sie dies als den Port der Anwendung, wie sie die Art ändern können, die Ihre Anwendung verhält sich, die jedoch nicht angezeigt wie die Kompilierungsfehler nach dem Austausch zu EF Core.</span><span class="sxs-lookup"><span data-stu-id="22d48-110">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="22d48-111">DbSet.Add/Attach und Graph-Verhalten</span><span class="sxs-lookup"><span data-stu-id="22d48-111">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="22d48-112">In EF6 Aufrufen `DbSet.Add()` auf eine Entität in einer rekursiven Suche für alle Entitäten, die in die Navigationseigenschaften verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="22d48-112">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="22d48-113">Alle Entitäten, die gefunden werden und nicht bereits vom Kontext, nachverfolgt werden, sind auch markiert werden, wenn hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="22d48-113">Any entities that are found, and are not already tracked by the context, are also be marked as added.</span></span> <span data-ttu-id="22d48-114">`DbSet.Attach()` verhält sich identisch, außer der alle Entitäten markiert sind als nicht geändert.</span><span class="sxs-lookup"><span data-stu-id="22d48-114">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="22d48-115">**EF Core führt eine rekursive Suche nach ähnlichen, jedoch mit einigen etwas anderen Regeln.**</span><span class="sxs-lookup"><span data-stu-id="22d48-115">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="22d48-116">Die Stammentität ist immer im angeforderten Zustand (für hinzugefügt `DbSet.Add` und unverändert für `DbSet.Attach`).</span><span class="sxs-lookup"><span data-stu-id="22d48-116">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="22d48-117">**Für Entitäten, die während der rekursiven Suche von Navigationseigenschaften gefunden werden:**</span><span class="sxs-lookup"><span data-stu-id="22d48-117">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="22d48-118">**Wenn der Primärschlüssel der Entität speichern generierter ist**</span><span class="sxs-lookup"><span data-stu-id="22d48-118">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="22d48-119">Wenn der Primärschlüssel nicht auf einen Wert festgelegt ist, wird der Status zu hinzugefügten festgelegt.</span><span class="sxs-lookup"><span data-stu-id="22d48-119">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="22d48-120">Wert des Primärschlüssels gilt als "nicht festgelegt", wenn es der CLR-Standardwert für den Eigenschaftentyp zugewiesen ist (z. B. `0` für `int`, `null` für `string`usw..).</span><span class="sxs-lookup"><span data-stu-id="22d48-120">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (for example, `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="22d48-121">Wenn der primäre Schlüssel auf einen Wert festgelegt ist, wird der Status auf unchanged festgelegt.</span><span class="sxs-lookup"><span data-stu-id="22d48-121">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="22d48-122">Wenn der Primärschlüssel nicht generierten Datenbank ist, wird die Entität im den gleichen Zustand wie der Stamm gespeichert.</span><span class="sxs-lookup"><span data-stu-id="22d48-122">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="22d48-123">Code des ersten Datenbankinitialisierung</span><span class="sxs-lookup"><span data-stu-id="22d48-123">Code First database initialization</span></span>

<span data-ttu-id="22d48-124">**EF6 weist eine erhebliche Menge an Magic, die sie für das Auswählen von Verbindung mit der Datenbank, und Initialisieren der Datenbank ausführt. Zu diesen Regeln gehören:**</span><span class="sxs-lookup"><span data-stu-id="22d48-124">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="22d48-125">Wenn keine Konfiguration ausgeführt wird, wählt EF6 eine Datenbank auf SQL Express oder LocalDb.</span><span class="sxs-lookup"><span data-stu-id="22d48-125">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="22d48-126">Wenn eine Verbindungszeichenfolge mit dem gleichen Namen wie der Kontext in den Anwendungen ist `App/Web.config` -Datei diese Verbindung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="22d48-126">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="22d48-127">Wenn die Datenbank nicht vorhanden ist, wird es erstellt.</span><span class="sxs-lookup"><span data-stu-id="22d48-127">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="22d48-128">Wenn keine der Tabellen aus dem Modell in der Datenbank vorhanden sind, wird das Schema für das aktuelle Modell der Datenbank hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="22d48-128">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="22d48-129">Wenn Migrations aktiviert sind, werden sie verwendet, um die Datenbank zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="22d48-129">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="22d48-130">Wenn die Datenbank vorhanden ist, und das Schema von EF6 hatten zuvor erstellt haben, ist Klicken Sie dann das Schema für die Kompatibilität mit dem aktuellen Modell aktiviert.</span><span class="sxs-lookup"><span data-stu-id="22d48-130">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="22d48-131">Eine Ausnahme wird ausgelöst, wenn das Modell geändert wurde, da das Schema erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="22d48-131">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="22d48-132">**EF Core führt nicht zu, dass keines dieser Magic-Befehl.**</span><span class="sxs-lookup"><span data-stu-id="22d48-132">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="22d48-133">Die datenbankverbindung muss explizit im Code konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="22d48-133">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="22d48-134">Es wird keine Initialisierung ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="22d48-134">No initialization is performed.</span></span> <span data-ttu-id="22d48-135">Verwenden Sie `DbContext.Database.Migrate()` zum Anwenden von Migrationen (oder `DbContext.Database.EnsureCreated()` und `EnsureDeleted()` zu erstellen und die Datenbank löschen ohne Migrationen zu verwenden).</span><span class="sxs-lookup"><span data-stu-id="22d48-135">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="22d48-136">Erste Tabelle Benennungskonvention</span><span class="sxs-lookup"><span data-stu-id="22d48-136">Code First table naming convention</span></span>

<span data-ttu-id="22d48-137">EF6 wird der Name der Entitätsklasse über eine pluralisierungsdienst den Standardnamen für die Tabelle zu berechnen, dem die Entität zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="22d48-137">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="22d48-138">EF Core verwendet den Namen der `DbSet` -Eigenschaft, die die Entität für den abgeleiteten Kontext verfügbar gemacht wird.</span><span class="sxs-lookup"><span data-stu-id="22d48-138">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="22d48-139">Wenn die Entität nicht verfügt eine `DbSet` -Eigenschaft, und klicken Sie dann auf den Namen der Klasse wird verwendet.</span><span class="sxs-lookup"><span data-stu-id="22d48-139">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
