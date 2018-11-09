---
title: Erstellen und Löschen von APIs – EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/10/2017
ms.openlocfilehash: 336f6fd655603a2474a58dfef377e121d9b04c3a
ms.sourcegitcommit: a088421ecac4f5dc5213208170490181ae2f5f0f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285638"
---
# <a name="create-and-drop-apis"></a><span data-ttu-id="cf43b-102">Erstellen und Löschen von APIs</span><span class="sxs-lookup"><span data-stu-id="cf43b-102">Create and Drop APIs</span></span>

<span data-ttu-id="cf43b-103">Die Methoden EnsureCreated und EnsureDeleted bieten eine einfache Alternative zur [Migrationen](migrations/index.md) für die Verwaltung des Datenbankschemas.</span><span class="sxs-lookup"><span data-stu-id="cf43b-103">The EnsureCreated and EnsureDeleted methods provide a lightweight alternative to [Migrations](migrations/index.md) for managing the database schema.</span></span> <span data-ttu-id="cf43b-104">Dies ist in Szenarien nützlich, wenn die Daten vorübergehend ist und gelöscht werden, können Wenn das Schema ändert.</span><span class="sxs-lookup"><span data-stu-id="cf43b-104">This is useful in scenarios when the data is transient and can be dropped when the schema changes.</span></span> <span data-ttu-id="cf43b-105">Beispielsweise während der Erstellung von Prototypen, Tests oder für den lokalen Caches.</span><span class="sxs-lookup"><span data-stu-id="cf43b-105">For example during prototyping, in tests, or for local caches.</span></span>

<span data-ttu-id="cf43b-106">Einige Anbieter (insbesondere nicht relationale diejenigen) unterstützen keine Migrationen.</span><span class="sxs-lookup"><span data-stu-id="cf43b-106">Some providers (especially non-relational ones) don't support Migrations.</span></span> <span data-ttu-id="cf43b-107">Für diese ist EnsureCreated oft die einfachste Möglichkeit zum Initialisieren des Datenbankschemas.</span><span class="sxs-lookup"><span data-stu-id="cf43b-107">For these, EnsureCreated is often the easiest way to initialize the database schema.</span></span>

> [!WARNING]
> <span data-ttu-id="cf43b-108">EnsureCreated und Migrationen funktionieren nicht gut zusammen.</span><span class="sxs-lookup"><span data-stu-id="cf43b-108">EnsureCreated and Migrations don't work well together.</span></span> <span data-ttu-id="cf43b-109">Wenn Sie Migrationen verwenden, verwenden Sie keine EnsureCreated um das Schema zu initialisieren.</span><span class="sxs-lookup"><span data-stu-id="cf43b-109">If you're using Migrations, don't use EnsureCreated to initialize the schema.</span></span>

<span data-ttu-id="cf43b-110">Übergang von EnsureCreated zu Migrationen ist nicht nahtlos.</span><span class="sxs-lookup"><span data-stu-id="cf43b-110">Transitioning from EnsureCreated to Migrations is not a seamless experience.</span></span> <span data-ttu-id="cf43b-111">Die Simpelest Möglichkeit hierzu ist die Datenbank löschen und neu erstellen mithilfe von Migrationen.</span><span class="sxs-lookup"><span data-stu-id="cf43b-111">The simpelest way to achieve this is to drop the database and re-create it using Migrations.</span></span> <span data-ttu-id="cf43b-112">Wenn sich abzeichnet, Migrationen in Zukunft verwenden, empfiehlt es sich, nur mit Migrationen zu beginnen, anstatt EnsureCreated.</span><span class="sxs-lookup"><span data-stu-id="cf43b-112">If you anticipate using Migrations in the future, it's best to just start with Migrations instead of using EnsureCreated.</span></span>

## <a name="ensuredeleted"></a><span data-ttu-id="cf43b-113">EnsureDeleted</span><span class="sxs-lookup"><span data-stu-id="cf43b-113">EnsureDeleted</span></span>

<span data-ttu-id="cf43b-114">Die Ensuredeleted--Methode wird die Datenbank löschen, wenn es vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="cf43b-114">The EnsureDeleted method will drop the database if it exists.</span></span> <span data-ttu-id="cf43b-115">Wenn Sie nicht über die entsprechenden Berechtigungen verfügen, wird eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="cf43b-115">If you don't have the appropiate permissions, an exception is thrown.</span></span>

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a><span data-ttu-id="cf43b-116">EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="cf43b-116">EnsureCreated</span></span>

<span data-ttu-id="cf43b-117">EnsureCreated wird die Datenbank erstellt, wenn er nicht vorhanden, und initialisieren das Datenbankschema.</span><span class="sxs-lookup"><span data-stu-id="cf43b-117">EnsureCreated will create the database if it doesn't exist and initialize the database schema.</span></span> <span data-ttu-id="cf43b-118">Wenn Tabellen vorhanden sind wird nicht durch das (einschließlich der Tabellen für ein anderes DbContext-Klasse) Schema initialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="cf43b-118">If any tables exist (including tables for another DbContext class), the schema won't be initialized.</span></span>

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> <span data-ttu-id="cf43b-119">Asynchrone Versionen dieser Methoden sind auch verfügbar.</span><span class="sxs-lookup"><span data-stu-id="cf43b-119">Async versions of these methods are also available.</span></span>

## <a name="sql-script"></a><span data-ttu-id="cf43b-120">SQL-Skript</span><span class="sxs-lookup"><span data-stu-id="cf43b-120">SQL Script</span></span>

<span data-ttu-id="cf43b-121">Rufen Sie die SQL von EnsureCreated verwendet, können Sie die GenerateCreateScript-Methode verwenden.</span><span class="sxs-lookup"><span data-stu-id="cf43b-121">To get the SQL used by EnsureCreated, you can use the GenerateCreateScript method.</span></span>

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a><span data-ttu-id="cf43b-122">Mehrere "DbContext"-Klassen</span><span class="sxs-lookup"><span data-stu-id="cf43b-122">Multiple DbContext classes</span></span>

<span data-ttu-id="cf43b-123">EnsureCreated funktioniert nur, wenn keine Tabellen in der Datenbank vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="cf43b-123">EnsureCreated only works when no tables are present in the database.</span></span> <span data-ttu-id="cf43b-124">Bei Bedarf können Sie Ihre eigene Überprüfung, um festzustellen, ob das Schema initialisiert werden muss, schreiben und verwenden den zugrunde liegenden IRelationalDatabaseCreator-Dienst, um das Schema zu initialisieren.</span><span class="sxs-lookup"><span data-stu-id="cf43b-124">If needed, you can write your own check to see if the schema needs to be initialized, and use the underlying IRelationalDatabaseCreator service to initialize the schema.</span></span>

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
