---
title: Seeding von Daten – EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/02/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 8f28dfea12461572ade8fbf3910ebd216dafb389
ms.sourcegitcommit: fa863883f1193d2118c2f9cee90808baa5e3e73e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52857428"
---
# <a name="data-seeding"></a><span data-ttu-id="8f422-102">Data Seeding</span><span class="sxs-lookup"><span data-stu-id="8f422-102">Data Seeding</span></span>

<span data-ttu-id="8f422-103">Seeding von Daten ist der Prozess für das Auffüllen einer Datenbank mit einem anfänglichen Satz von Daten.</span><span class="sxs-lookup"><span data-stu-id="8f422-103">Data seeding is the process of populating a database with an initial set of data.</span></span>

<span data-ttu-id="8f422-104">Es gibt verschiedene Möglichkeiten, die dies in EF Core erreicht werden kann:</span><span class="sxs-lookup"><span data-stu-id="8f422-104">There are several ways this can be accomplished in EF Core:</span></span>
* <span data-ttu-id="8f422-105">Seed-Daten des Modells</span><span class="sxs-lookup"><span data-stu-id="8f422-105">Model seed data</span></span>
* <span data-ttu-id="8f422-106">Manuelle Migration Anpassung</span><span class="sxs-lookup"><span data-stu-id="8f422-106">Manual migration customization</span></span>
* <span data-ttu-id="8f422-107">Benutzerdefinierte Initialisierungslogik</span><span class="sxs-lookup"><span data-stu-id="8f422-107">Custom initialization logic</span></span>

## <a name="model-seed-data"></a><span data-ttu-id="8f422-108">Seed-Daten des Modells</span><span class="sxs-lookup"><span data-stu-id="8f422-108">Model seed data</span></span>

> [!NOTE]
> <span data-ttu-id="8f422-109">Dieses Feature ist neu in EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="8f422-109">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="8f422-110">Im Gegensatz zu kann in EF6 in EF Core seeding von Daten mit einem Entitätstyp als Teil der Modellkonfiguration verknüpft sein.</span><span class="sxs-lookup"><span data-stu-id="8f422-110">Unlike in EF6, in EF Core, seeding data can be associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="8f422-111">EF Core [Migrationen](xref:core/managing-schemas/migrations/index) bestimmen dann automatisch, welche INSERT-, UPDATE- oder DELETE-Operationen beim Upgrade der Datenbank auf eine neue Version des Datenmodells angewendet werden müssen.</span><span class="sxs-lookup"><span data-stu-id="8f422-111">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

> [!NOTE]
> <span data-ttu-id="8f422-112">Migrationen berücksichtigt nur modelländerungen beim bestimmen, welcher Vorgang ausgeführt werden soll, um die Seed-Daten in den gewünschten Zustand zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="8f422-112">Migrations only considers model changes when determining what operation should be performed to get the seed data into the desired state.</span></span> <span data-ttu-id="8f422-113">Daher können Änderungen an den Daten, die außerhalb von Migrationen durchgeführt getrennt werden, oder führen zu einem Fehler.</span><span class="sxs-lookup"><span data-stu-id="8f422-113">Thus any changes to the data performed outside of migrations might be lost or cause an error.</span></span>

<span data-ttu-id="8f422-114">Dadurch wird beispielsweise startwertdaten für konfiguriert eine `Blog` in `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="8f422-114">As an example, this will configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[BlogSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="8f422-115">Um Entitäten hinzuzufügen, die eine Beziehung die Fremdschlüsselwerte müssen angegeben werden:</span><span class="sxs-lookup"><span data-stu-id="8f422-115">To add entities that have a relationship the foreign key values need to be specified:</span></span>

[!code-csharp[PostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="8f422-116">Wenn der Entitätstyp alle Eigenschaften, die Shadow-Status, die eine anonyme Klasse verwendet werden kann aufweist, um die Werte bereitzustellen:</span><span class="sxs-lookup"><span data-stu-id="8f422-116">If the entity type has any properties in shadow state an anonymous class can be used to provide the values:</span></span>

[!code-csharp[AnonymousPostSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=AnonymousPostSeed)]

<span data-ttu-id="8f422-117">Der eigenen Entität, die Typen, die auf ähnliche Weise ausgeführt werden können:</span><span class="sxs-lookup"><span data-stu-id="8f422-117">Owned entity types can be seeded in a similar fashion:</span></span>

[!code-csharp[OwnedTypeSeed](../../../samples/core/Modeling/DataSeeding/DataSeedingContext.cs?name=OwnedTypeSeed)]

<span data-ttu-id="8f422-118">Finden Sie unter den [vollständigen Beispielprojekt](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) für weiteren Kontext.</span><span class="sxs-lookup"><span data-stu-id="8f422-118">See the [full sample project](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/DataSeeding) for more context.</span></span>

<span data-ttu-id="8f422-119">Nachdem die Daten, dem Modell hinzugefügt wurde [Migrationen](xref:core/managing-schemas/migrations/index) sollte verwendet werden, um die Änderungen zu übernehmen.</span><span class="sxs-lookup"><span data-stu-id="8f422-119">Once the data has been added to the model, [migrations](xref:core/managing-schemas/migrations/index) should be used to apply the changes.</span></span>

> [!TIP]
> <span data-ttu-id="8f422-120">Wenn Sie zum Anwenden von Migrationen als Teil einer automatisierten Bereitstellung benötigen können Sie [erstellen Sie ein SQL­Skript](xref:core/managing-schemas/migrations/index#generate-sql-scripts) können, die vor der Ausführung eine Vorschau angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="8f422-120">If you need to apply migrations as part of an automated deployment you can [create a SQL script](xref:core/managing-schemas/migrations/index#generate-sql-scripts) that can be previewed before execution.</span></span>

<span data-ttu-id="8f422-121">Alternativ können Sie `context.Database.EnsureCreated()` zum Erstellen einer neuen Datenbank, die die Seed-Daten, z. B. für eine Testdatenbank oder bei Verwendung von in-Memory-Anbieter oder eine nicht-Relation-Datenbank enthält.</span><span class="sxs-lookup"><span data-stu-id="8f422-121">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider or any non-relation database.</span></span> <span data-ttu-id="8f422-122">Beachten Sie, dass bei die Datenbank bereits vorhanden ist, `EnsureCreated()` wird weder die Schema noch Seed-Daten in der Datenbank aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="8f422-122">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor seed data in the database.</span></span> <span data-ttu-id="8f422-123">Sie sollten nicht für relationale Datenbanken aufrufen `EnsureCreated()` , wenn Sie Migrationen verwenden möchten.</span><span class="sxs-lookup"><span data-stu-id="8f422-123">For relational databases you shouldn't call `EnsureCreated()` if you plan to use Migrations.</span></span>

<span data-ttu-id="8f422-124">Diese Art von Seed-Daten wird durch Migrationen verwaltet, und das Skript zum Aktualisieren der Daten, die bereits in der Datenbank ist ohne Verbindung zur Datenbank generiert werden muss.</span><span class="sxs-lookup"><span data-stu-id="8f422-124">This type of seed data is managed by migrations and the script to update the data that's already in the database needs to be generated without connecting to the database.</span></span> <span data-ttu-id="8f422-125">Diese erzwingt einige Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="8f422-125">This imposes some restrictions:</span></span>
* <span data-ttu-id="8f422-126">Wert des Primärschlüssels muss angegeben werden, auch wenn es in der Regel von der Datenbank generiert wird.</span><span class="sxs-lookup"><span data-stu-id="8f422-126">The primary key value needs to be specified even if it's usually generated by the database.</span></span> <span data-ttu-id="8f422-127">Er wird zum Erkennen von datenänderungen zwischen Migrationen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="8f422-127">It will be used to detect data changes between migrations.</span></span>
* <span data-ttu-id="8f422-128">Zuvor werden per Seeding hinzugefügte Daten entfernt werden, wenn der Primärschlüssel in keiner Weise geändert wird.</span><span class="sxs-lookup"><span data-stu-id="8f422-128">Previously seeded data will be removed if the primary key is changed in any way.</span></span>

<span data-ttu-id="8f422-129">Aus diesem Grund eignet sich diese Funktion am besten für statische Daten, die ist nicht dazu gedacht, die außerhalb von Migrationen zu ändern, und hängt nicht alles in der Datenbank, z. B. Postleitzahlen.</span><span class="sxs-lookup"><span data-stu-id="8f422-129">Therefore this feature is most useful for static data that's not expected to change outside of migrations and does not depend on anything else in the database, for example ZIP codes.</span></span>

<span data-ttu-id="8f422-130">Wenn Ihr Szenario eine der folgenden enthält wird empfohlen, die angepasste Initialisierung von Logik, die im vorherigen Abschnitt beschrieben zu verwenden:</span><span class="sxs-lookup"><span data-stu-id="8f422-130">If your scenario includes any of the following it is recommended to use custom initialization logic described in the last section:</span></span>
* <span data-ttu-id="8f422-131">Temporäre Daten für Tests</span><span class="sxs-lookup"><span data-stu-id="8f422-131">Temporary data for testing</span></span>
* <span data-ttu-id="8f422-132">Daten, die abhängig von Datenbankstatus</span><span class="sxs-lookup"><span data-stu-id="8f422-132">Data that depends on database state</span></span>
* <span data-ttu-id="8f422-133">Daten, die Schlüsselwerte generiert werden, von der Datenbank, einschließlich der Entitäten, die alternativen Schlüssel als Identität verwendet.</span><span class="sxs-lookup"><span data-stu-id="8f422-133">Data that needs key values to be generated by the database, including entities that use alternate keys as the identity</span></span>
* <span data-ttu-id="8f422-134">Daten, die eine benutzerdefinierte Transformation erfordert, (, erfolgt nicht durch [Wert Konvertierungen](xref:core/modeling/value-conversions)), z. B. eine Kennwort-hashing</span><span class="sxs-lookup"><span data-stu-id="8f422-134">Data that requires custom transformation (that is not handled by [value conversions](xref:core/modeling/value-conversions)), such as some password hashing</span></span>
* <span data-ttu-id="8f422-135">Daten, die Aufrufe an externe API, z.B. ASP.NET Core Identity-Rollen und Benutzer erstellen erfordern</span><span class="sxs-lookup"><span data-stu-id="8f422-135">Data that requires calls to external API, such as ASP.NET Core Identity roles and users creation</span></span>

## <a name="manual-migration-customization"></a><span data-ttu-id="8f422-136">Manuelle Migration Anpassung</span><span class="sxs-lookup"><span data-stu-id="8f422-136">Manual migration customization</span></span>

<span data-ttu-id="8f422-137">Wenn eine Migration die Änderungen an den Daten, die mit angegebenen hinzugefügt wird `HasData` werden in Aufrufe an transformiert `InsertData()`, `UpdateData()`, und `DeleteData()`.</span><span class="sxs-lookup"><span data-stu-id="8f422-137">When a migration is added the changes to the data specified with `HasData` are transformed to calls to `InsertData()`, `UpdateData()`, and `DeleteData()`.</span></span> <span data-ttu-id="8f422-138">Eine Möglichkeit, arbeiten umgehungslösungen zu einigen der Einschränkungen der `HasData` besteht darin, diese Aufrufe manuell hinzufügen oder [benutzerdefinierte Vorgänge](xref:core/managing-schemas/migrations/operations) zur Migration stattdessen.</span><span class="sxs-lookup"><span data-stu-id="8f422-138">One way of working around some of the limitations of `HasData` is to manually add these calls or [custom operations](xref:core/managing-schemas/migrations/operations) to the migration instead.</span></span>

[!code-csharp[CustomInsert](../../../samples/core/Modeling/DataSeeding/Migrations/20181102235626_Initial.cs?name=CustomInsert)]

## <a name="custom-initialization-logic"></a><span data-ttu-id="8f422-139">Benutzerdefinierte Initialisierungslogik</span><span class="sxs-lookup"><span data-stu-id="8f422-139">Custom initialization logic</span></span>

<span data-ttu-id="8f422-140">Eine einfache und leistungsstarke Möglichkeit zum seeding von Daten ausführen ist die Verwendung [ `DbContext.SaveChanges()` ](xref:core/saving/index) vor der hauptanwendung Logik ihrer Ausführung beginnt.</span><span class="sxs-lookup"><span data-stu-id="8f422-140">A straightforward and powerful way to perform data seeding is to use [`DbContext.SaveChanges()`](xref:core/saving/index) before the main application logic begins execution.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataSeeding/Program.cs?name=CustomSeeding)]

> [!WARNING]
> <span data-ttu-id="8f422-141">Der seedingcode muss nicht Teil der normal-app-Ausführung, wie dies von Parallelitätsproblemen führen kann, wenn mehrere Instanzen ausgeführt werden und müsste auch die app keine Berechtigung zum Ändern des Datenbankschemas.</span><span class="sxs-lookup"><span data-stu-id="8f422-141">The seeding code should not be part of the normal app execution as this can cause concurrency issues when multiple instances are running and would also require the app having permission to modify the database schema.</span></span>

<span data-ttu-id="8f422-142">Je nach den Einschränkungen der Bereitstellung kann der Initialisierungscode auf unterschiedliche Weise ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="8f422-142">Depending on the constraints of your deployment the initialization code can be executed in different ways:</span></span>
* <span data-ttu-id="8f422-143">Lokales Ausführen der app für die Initialisierung</span><span class="sxs-lookup"><span data-stu-id="8f422-143">Running the initialization app locally</span></span>
* <span data-ttu-id="8f422-144">Die Initialisierung-app mit der Haupt-app, die Initialisierungsroutine aufrufen, und deaktivieren, oder entfernen die Initialisierung-app bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="8f422-144">Deploying the initialization app with the main app, invoking the initialization routine and disabling or removing the initialization app.</span></span>

<span data-ttu-id="8f422-145">Dies kann in der Regel mit automatisiert werden [Veröffentlichungsprofile](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/visual-studio-publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="8f422-145">This can usually be automated by using [publish profiles](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/visual-studio-publish-profiles).</span></span>
