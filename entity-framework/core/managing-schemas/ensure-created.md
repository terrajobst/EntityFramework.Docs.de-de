---
title: 'Erstellen und Löschen von APIs: EF Core'
author: bricelam
ms.author: bricelam
ms.date: 11/07/2018
uid: core/managing-schemas/ensure-created
ms.openlocfilehash: 32ac6cd043df73cd041780ec4c8805675adc5ab1
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811786"
---
# <a name="create-and-drop-apis"></a><span data-ttu-id="456bd-102">Erstellen und Löschen von APIs</span><span class="sxs-lookup"><span data-stu-id="456bd-102">Create and Drop APIs</span></span>

<span data-ttu-id="456bd-103">Die ensuneu-und ensuredeleted-Methoden stellen eine leichte Alternative zu [Migrationen](migrations/index.md) zum Verwalten des Datenbankschemas dar.</span><span class="sxs-lookup"><span data-stu-id="456bd-103">The EnsureCreated and EnsureDeleted methods provide a lightweight alternative to [Migrations](migrations/index.md) for managing the database schema.</span></span> <span data-ttu-id="456bd-104">Diese Methoden sind in Szenarios nützlich, in denen die Daten vorübergehend sind und gelöscht werden können, wenn sich das Schema ändert.</span><span class="sxs-lookup"><span data-stu-id="456bd-104">These methods are useful in scenarios when the data is transient and can be dropped when the schema changes.</span></span> <span data-ttu-id="456bd-105">Beispielsweise während der Prototyperstellung in Tests oder für lokale Caches.</span><span class="sxs-lookup"><span data-stu-id="456bd-105">For example during prototyping, in tests, or for local caches.</span></span>

<span data-ttu-id="456bd-106">Einige Anbieter (insbesondere nicht relationale) unterstützen keine Migrationen.</span><span class="sxs-lookup"><span data-stu-id="456bd-106">Some providers (especially non-relational ones) don't support Migrations.</span></span> <span data-ttu-id="456bd-107">Für diese Anbieter ist "ensuneu" oft die einfachste Möglichkeit, das Datenbankschema zu initialisieren.</span><span class="sxs-lookup"><span data-stu-id="456bd-107">For these providers, EnsureCreated is often the easiest way to initialize the database schema.</span></span>

> [!WARNING]
> <span data-ttu-id="456bd-108">Ensuneu erstellte und Migrationen funktionieren nicht gut zusammen.</span><span class="sxs-lookup"><span data-stu-id="456bd-108">EnsureCreated and Migrations don't work well together.</span></span> <span data-ttu-id="456bd-109">Wenn Sie Migrationen verwenden, verwenden Sie "ensurecreated" nicht, um das Schema zu initialisieren.</span><span class="sxs-lookup"><span data-stu-id="456bd-109">If you're using Migrations, don't use EnsureCreated to initialize the schema.</span></span>

<span data-ttu-id="456bd-110">Der Übergang von ensuneu in Migrationen ist nicht nahtlos.</span><span class="sxs-lookup"><span data-stu-id="456bd-110">Transitioning from EnsureCreated to Migrations is not a seamless experience.</span></span> <span data-ttu-id="456bd-111">Die einfachste Möglichkeit hierzu besteht darin, die Datenbank zu löschen und Sie mithilfe von Migrationen neu zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="456bd-111">The simplest way to do it is to drop the database and re-create it using Migrations.</span></span> <span data-ttu-id="456bd-112">Wenn Sie die Verwendung von Migrationen in der Zukunft erwarten, empfiehlt es sich, einfach mit Migrationen zu beginnen, anstatt "ensuneu" zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="456bd-112">If you anticipate using migrations in the future, it's best to just start with Migrations instead of using EnsureCreated.</span></span>

## <a name="ensuredeleted"></a><span data-ttu-id="456bd-113">Ensuredeleted</span><span class="sxs-lookup"><span data-stu-id="456bd-113">EnsureDeleted</span></span>

<span data-ttu-id="456bd-114">Die ensuredeleted-Methode löscht die Datenbank, wenn Sie vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="456bd-114">The EnsureDeleted method will drop the database if it exists.</span></span> <span data-ttu-id="456bd-115">Wenn Sie nicht über die entsprechenden Berechtigungen verfügen, wird eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="456bd-115">If you don't have the appropriate permissions, an exception is thrown.</span></span>

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a><span data-ttu-id="456bd-116">Ensuneu erstellt</span><span class="sxs-lookup"><span data-stu-id="456bd-116">EnsureCreated</span></span>

<span data-ttu-id="456bd-117">Ensuneu erstellt erstellt die Datenbank, wenn Sie nicht vorhanden ist, und initialisiert das Datenbankschema.</span><span class="sxs-lookup"><span data-stu-id="456bd-117">EnsureCreated will create the database if it doesn't exist and initialize the database schema.</span></span> <span data-ttu-id="456bd-118">Wenn Tabellen vorhanden sind (einschließlich Tabellen für eine andere dbcontext-Klasse), wird das Schema nicht initialisiert.</span><span class="sxs-lookup"><span data-stu-id="456bd-118">If any tables exist (including tables for another DbContext class), the schema won't be initialized.</span></span>

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> <span data-ttu-id="456bd-119">Asynchrone Versionen dieser Methoden sind ebenfalls verfügbar.</span><span class="sxs-lookup"><span data-stu-id="456bd-119">Async versions of these methods are also available.</span></span>

## <a name="sql-script"></a><span data-ttu-id="456bd-120">SQL-Skript</span><span class="sxs-lookup"><span data-stu-id="456bd-120">SQL Script</span></span>

<span data-ttu-id="456bd-121">Um das von ensuneu verwendete SQL zu erhalten, können Sie die generatecreatescript-Methode verwenden.</span><span class="sxs-lookup"><span data-stu-id="456bd-121">To get the SQL used by EnsureCreated, you can use the GenerateCreateScript method.</span></span>

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a><span data-ttu-id="456bd-122">Mehrere dbcontext-Klassen</span><span class="sxs-lookup"><span data-stu-id="456bd-122">Multiple DbContext classes</span></span>

<span data-ttu-id="456bd-123">Ensuneu erstellt funktioniert nur, wenn keine Tabellen in der Datenbank vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="456bd-123">EnsureCreated only works when no tables are present in the database.</span></span> <span data-ttu-id="456bd-124">Bei Bedarf können Sie eine eigene Überprüfung schreiben, um festzustellen, ob das Schema initialisiert werden muss, und den zugrunde liegenden irelationaldatabasecreator-Dienst verwenden, um das Schema zu initialisieren.</span><span class="sxs-lookup"><span data-stu-id="456bd-124">If needed, you can write your own check to see if the schema needs to be initialized, and use the underlying IRelationalDatabaseCreator service to initialize the schema.</span></span>

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
