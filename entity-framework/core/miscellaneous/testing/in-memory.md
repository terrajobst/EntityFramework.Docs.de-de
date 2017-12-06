---
title: Testen mit InMemory - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: c5c48c575e9fd693d1f28d1a6d10eb83ebbc9d70
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2017
---
# <a name="testing-with-inmemory"></a><span data-ttu-id="1e280-102">Testen mit InMemory</span><span class="sxs-lookup"><span data-stu-id="1e280-102">Testing with InMemory</span></span>

<span data-ttu-id="1e280-103">Der Anbieter InMemory ist nützlich, wenn Sie Komponenten mithilfe von etwas, das Herstellen einer Verbindung mit der real-Datenbank, ohne den Aufwand für die tatsächliche Datenbankvorgänge entspricht in etwa testen möchten.</span><span class="sxs-lookup"><span data-stu-id="1e280-103">The InMemory provider is useful when you want to test components using something that approximates connecting to the real database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="1e280-104">Sie können anzeigen, dass dieser Artikel [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="1e280-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub.</span></span>

## <a name="inmemory-is-not-a-relational-database"></a><span data-ttu-id="1e280-105">InMemory ist nicht mit einer relationalen Datenbank</span><span class="sxs-lookup"><span data-stu-id="1e280-105">InMemory is not a relational database</span></span>

<span data-ttu-id="1e280-106">EF Datenbank kernanbieter keine relationalen Datenbanken werden.</span><span class="sxs-lookup"><span data-stu-id="1e280-106">EF Core database providers do not have to be relational databases.</span></span> <span data-ttu-id="1e280-107">InMemory dient als allgemeines Datenbank zum Testen und dient nicht zum Simulieren einer relationalen Datenbank.</span><span class="sxs-lookup"><span data-stu-id="1e280-107">InMemory is designed to be a general purpose database for testing, and is not designed to mimic a relational database.</span></span>

<span data-ttu-id="1e280-108">Beispiele hierfür sind:</span><span class="sxs-lookup"><span data-stu-id="1e280-108">Some examples of this include:</span></span>
* <span data-ttu-id="1e280-109">InMemory können Sie zum Speichern von Daten, die Einschränkungen der referenziellen Integrität in einer relationalen Datenbank verletzen würde.</span><span class="sxs-lookup"><span data-stu-id="1e280-109">InMemory will allow you to save data that would violate referential integrity constraints in a relational database.</span></span>

* <span data-ttu-id="1e280-110">Wenn Sie DefaultValueSql(string) für eine Eigenschaft im Modell verwenden, dies ist eine relationale Datenbank-API und hat keine Auswirkungen, bei der Ausführung für InMemory.</span><span class="sxs-lookup"><span data-stu-id="1e280-110">If you use DefaultValueSql(string) for a property in your model, this is a relational database API and will have no effect when running against InMemory.</span></span>

> [!TIP]  
> <span data-ttu-id="1e280-111">Für viele Zwecke testen werden diese Unterschiede spielt keine Rolle.</span><span class="sxs-lookup"><span data-stu-id="1e280-111">For many test purposes these differences will not matter.</span></span> <span data-ttu-id="1e280-112">Allerdings gegebenenfalls für die Tests anhand etwas mehr Verhalten hat wie ein "true" relationale Datenbank sollten Sie die Verwendung [SQLite InMemory-Modus](sqlite.md).</span><span class="sxs-lookup"><span data-stu-id="1e280-112">However, if you want to test against something that behaves more like a true relational database, then consider using [SQLite in-memory mode](sqlite.md).</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="1e280-113">Beispiel-Testszenario</span><span class="sxs-lookup"><span data-stu-id="1e280-113">Example testing scenario</span></span>

<span data-ttu-id="1e280-114">Betrachten Sie den folgenden Dienst, der Anwendungscode im Zusammenhang mit Blogs Eingriffe ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="1e280-114">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="1e280-115">Er verwendet intern eine `DbContext` , die eine Verbindung mit einer SQL Server-Datenbank her.</span><span class="sxs-lookup"><span data-stu-id="1e280-115">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="1e280-116">Wäre es nützlich, um diesem Kontext die Verbindung mit einer InMemory-Datenbank, damit wir effiziente Tests für diesen Dienst schreiben, ohne den Code ändern können, oder viele Vorgänge zum Erstellen eines Tests führen Austauschen des Kontexts double.</span><span class="sxs-lookup"><span data-stu-id="1e280-116">It would be useful to swap this context to connect to an InMemory database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="1e280-117">Bereiten Sie den Kontext</span><span class="sxs-lookup"><span data-stu-id="1e280-117">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="1e280-118">Vermeiden Sie die Konfiguration von zwei Datenbankanbieter</span><span class="sxs-lookup"><span data-stu-id="1e280-118">Avoid configuring two database providers</span></span>

<span data-ttu-id="1e280-119">In den Tests wirst du extern konfigurieren Sie den Kontext des InMemory-Anbieters verwenden.</span><span class="sxs-lookup"><span data-stu-id="1e280-119">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="1e280-120">Wenn Sie einen Datenbankanbieter durch Außerkraftsetzen konfigurieren `OnConfiguring` in den Kontext, dann müssen Sie fügen Sie bedingte Code zum sicherstellen, dass Sie nur den Datenbankanbieter konfigurieren, wenn Sie noch nicht konfiguriert wurde.</span><span class="sxs-lookup"><span data-stu-id="1e280-120">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> <span data-ttu-id="1e280-121">Bei Verwendung von ASP.NET Core sollte dann nicht mit diesem Code erforderlich, da der Datenbankanbieter bereits außerhalb des Kontexts (in "Startup.cs) konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="1e280-121">If you are using ASP.NET Core, then you should not need this code since your database provider is already configured outside of the context (in Startup.cs).</span></span>

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="1e280-122">Fügen Sie einen Konstruktor zu Testzwecken</span><span class="sxs-lookup"><span data-stu-id="1e280-122">Add a constructor for testing</span></span>

<span data-ttu-id="1e280-123">Die einfachste Methode zum Aktivieren von Tests mit einer anderen Datenbank so ändern Sie den Kontext, um einen Konstruktor verfügbar machen, das akzeptiert, wird eine `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="1e280-123">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="1e280-124">`DbContextOptions<TContext>`Gibt dem Kontext alle zugehörigen Einstellungen, z. B. welche Datenbank für die Verbindung.</span><span class="sxs-lookup"><span data-stu-id="1e280-124">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="1e280-125">Dies ist das gleiche Objekt, das erstellt wird, durch die OnConfiguring-Methode in Ihrem Kontext ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="1e280-125">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="1e280-126">Schreiben von tests</span><span class="sxs-lookup"><span data-stu-id="1e280-126">Writing tests</span></span>

<span data-ttu-id="1e280-127">Der Schlüssel für das Testen von mit diesem Anbieter ist die Fähigkeit, teilen Sie den Kontext zum Verwenden des InMemory-Anbieters und Steuern des Bereichs der Datenbank im Arbeitsspeicher.</span><span class="sxs-lookup"><span data-stu-id="1e280-127">The key to testing with this provider is the ability to tell the context to use the InMemory provider, and control the scope of the in-memory database.</span></span> <span data-ttu-id="1e280-128">Sie möchten in der Regel eine fehlerfreie Datenbank für jede Testmethode.</span><span class="sxs-lookup"><span data-stu-id="1e280-128">Typically you want a clean database for each test method.</span></span>

<span data-ttu-id="1e280-129">Hier ist ein Beispiel für eine Testklasse, die den InMemory-Datenbank verwendet.</span><span class="sxs-lookup"><span data-stu-id="1e280-129">Here is an example of a test class that uses the InMemory database.</span></span> <span data-ttu-id="1e280-130">Jede Testmethode gibt einen eindeutigen Datenbanknamen an, was bedeutet, dass jede Methode ihre eigene InMemory-Datenbank hat.</span><span class="sxs-lookup"><span data-stu-id="1e280-130">Each test method specifies a unique database name, meaning each method has its own InMemory database.</span></span>

>[!TIP]
> <span data-ttu-id="1e280-131">Verwenden der `.UseInMemoryDatabase()` Erweiterungsmethode, Verweis das NuGet-Paket `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="1e280-131">To use the `.UseInMemoryDatabase()` extension method, reference the Nuget package `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
