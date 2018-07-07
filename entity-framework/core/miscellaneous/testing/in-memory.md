---
title: Testen mit InMemory - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: f814c8955e155688bb5e8d34b9c9f6d24dcc6601
ms.sourcegitcommit: fd50ac53b93a03825dcbb42ed2e7ca95ca858d5f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/07/2018
ms.locfileid: "37900261"
---
# <a name="testing-with-inmemory"></a><span data-ttu-id="95c98-102">Testen mit InMemory</span><span class="sxs-lookup"><span data-stu-id="95c98-102">Testing with InMemory</span></span>

<span data-ttu-id="95c98-103">Der InMemory-Anbieter ist nützlich, wenn Sie Komponenten verwenden, indem Sie die Verbindung zur echten Datenbank, ohne den Aufwand für die tatsächliche Datenbankvorgänge Tools, testen möchten.</span><span class="sxs-lookup"><span data-stu-id="95c98-103">The InMemory provider is useful when you want to test components using something that approximates connecting to the real database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="95c98-104">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="95c98-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub.</span></span>

## <a name="inmemory-is-not-a-relational-database"></a><span data-ttu-id="95c98-105">InMemory ist nicht mit einer relationalen Datenbank</span><span class="sxs-lookup"><span data-stu-id="95c98-105">InMemory is not a relational database</span></span>

<span data-ttu-id="95c98-106">EF Core-Datenbankanbieter keine relationalen Datenbanken sein.</span><span class="sxs-lookup"><span data-stu-id="95c98-106">EF Core database providers do not have to be relational databases.</span></span> <span data-ttu-id="95c98-107">InMemory fungiert als ein allgemeiner-Datenbank zu Testzwecken und dient nicht zum imitieren von einer relationalen Datenbank.</span><span class="sxs-lookup"><span data-stu-id="95c98-107">InMemory is designed to be a general purpose database for testing, and is not designed to mimic a relational database.</span></span>

<span data-ttu-id="95c98-108">Beispiele hierfür sind:</span><span class="sxs-lookup"><span data-stu-id="95c98-108">Some examples of this include:</span></span>

* <span data-ttu-id="95c98-109">InMemory können Sie zum Speichern von Daten, die Einschränkungen der referenziellen Integrität in einer relationalen Datenbank verletzen würde.</span><span class="sxs-lookup"><span data-stu-id="95c98-109">InMemory will allow you to save data that would violate referential integrity constraints in a relational database.</span></span>
* <span data-ttu-id="95c98-110">Wenn Sie DefaultValueSql(string) für eine Eigenschaft im Modell verwenden, dies ist eine relationale Datenbank-API und hat keine Auswirkungen, bei der Ausführung für InMemory.</span><span class="sxs-lookup"><span data-stu-id="95c98-110">If you use DefaultValueSql(string) for a property in your model, this is a relational database API and will have no effect when running against InMemory.</span></span>
* <span data-ttu-id="95c98-111">[Parallelität über Zeitstempel/Zeilenversion](xref:core/modeling/concurrency#timestamprow-version) (`[Timestamp]` oder `IsRowVersion`) wird nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="95c98-111">[Concurrency via Timestamp/row version](xref:core/modeling/concurrency#timestamprow-version) (`[Timestamp]` or `IsRowVersion`) is not supported.</span></span> <span data-ttu-id="95c98-112">Keine [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) wird ausgelöst, wenn ein Update erfolgt mit einem alten Concurrency-Token.</span><span class="sxs-lookup"><span data-stu-id="95c98-112">No [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) will be thrown if an update is done using an old concurrency token.</span></span>

> [!TIP]  
> <span data-ttu-id="95c98-113">Für viele Zwecke testen werden diese Unterschiede nicht wichtig.</span><span class="sxs-lookup"><span data-stu-id="95c98-113">For many test purposes these differences will not matter.</span></span> <span data-ttu-id="95c98-114">Wenn Sie mit dem Sie etwas zu testen, die mehr wie eine "true" relationale Datenbank verhält sich möchten, klicken Sie dann sollten Sie jedoch [SQLite-in-Memory-Modus](sqlite.md).</span><span class="sxs-lookup"><span data-stu-id="95c98-114">However, if you want to test against something that behaves more like a true relational database, then consider using [SQLite in-memory mode](sqlite.md).</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="95c98-115">Beispiel-Szenario</span><span class="sxs-lookup"><span data-stu-id="95c98-115">Example testing scenario</span></span>

<span data-ttu-id="95c98-116">Betrachten Sie den folgenden Dienst, mit dem Anwendungscode zum Durchführen bestimmter Vorgänge im Zusammenhang mit Blogs zu können.</span><span class="sxs-lookup"><span data-stu-id="95c98-116">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="95c98-117">Intern verwendet eine `DbContext` , die eine Verbindung mit SQL Server-Datenbank her.</span><span class="sxs-lookup"><span data-stu-id="95c98-117">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="95c98-118">Es wäre nützlich, um diesem Kontext für die Verbindung eine InMemory-Datenbank, damit wir können effiziente Tests für diesen Dienst schreiben, ohne den Code ändern zu müssen, oder einen Großteil der Arbeit beim Erstellen eines Tests Austauschen des Kontexts double.</span><span class="sxs-lookup"><span data-stu-id="95c98-118">It would be useful to swap this context to connect to an InMemory database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="95c98-119">Bereiten Sie den Kontext</span><span class="sxs-lookup"><span data-stu-id="95c98-119">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="95c98-120">Vermeiden Sie die Konfiguration von zwei Datenbankanbieter</span><span class="sxs-lookup"><span data-stu-id="95c98-120">Avoid configuring two database providers</span></span>

<span data-ttu-id="95c98-121">In den Tests wird den Kontext zum Verwenden des InMemory-Anbieters extern konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="95c98-121">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="95c98-122">Wenn Sie durch das Überschreiben ein Datenbankanbieters konfigurieren `OnConfiguring` im Kontext Ihrer müssen Sie fügen der bedingten Code, um sicherzustellen, dass Sie nur den Datenbankanbieter konfigurieren, wenn eine nicht bereits konfiguriert wurde.</span><span class="sxs-lookup"><span data-stu-id="95c98-122">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> <span data-ttu-id="95c98-123">Wenn Sie ASP.NET Core verwenden, sollten dann nicht dieser Code erforderlich, da Ihr Datenbankanbieter außerhalb des Kontexts (in "Startup.cs") bereits konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="95c98-123">If you are using ASP.NET Core, then you should not need this code since your database provider is already configured outside of the context (in Startup.cs).</span></span>

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="95c98-124">Fügen Sie einen Konstruktor für das Testen</span><span class="sxs-lookup"><span data-stu-id="95c98-124">Add a constructor for testing</span></span>

<span data-ttu-id="95c98-125">Die einfachste Möglichkeit zum Aktivieren von Tests mit einer anderen Datenbank ist so ändern Sie den Kontext, um einen Konstruktor verfügbar machen, die akzeptiert eine `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="95c98-125">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="95c98-126">`DbContextOptions<TContext>` Gibt dem Kontext alle zugehörigen Einstellungen, z. B. welche Datenbank für die Verbindung.</span><span class="sxs-lookup"><span data-stu-id="95c98-126">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="95c98-127">Dies ist das gleiche Objekt, das durch Ausführen der OnConfiguring-Methode im Kontext Ihrer basiert.</span><span class="sxs-lookup"><span data-stu-id="95c98-127">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="95c98-128">Schreiben von tests</span><span class="sxs-lookup"><span data-stu-id="95c98-128">Writing tests</span></span>

<span data-ttu-id="95c98-129">Die Taste, um Tests mit dieser Anbieter ist die Möglichkeit, teilen Sie den Kontext den InMemory-Anbieter, und Steuern des Gültigkeitsbereichs der in-Memory-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="95c98-129">The key to testing with this provider is the ability to tell the context to use the InMemory provider, and control the scope of the in-memory database.</span></span> <span data-ttu-id="95c98-130">Möchten Sie in der Regel eine fehlerfreie Datenbank, für jede Testmethode.</span><span class="sxs-lookup"><span data-stu-id="95c98-130">Typically you want a clean database for each test method.</span></span>

<span data-ttu-id="95c98-131">Hier ist ein Beispiel für eine Testklasse, die der InMemory-Datenbank verwendet.</span><span class="sxs-lookup"><span data-stu-id="95c98-131">Here is an example of a test class that uses the InMemory database.</span></span> <span data-ttu-id="95c98-132">Jede Testmethode gibt einen eindeutigen Datenbanknamen an, was bedeutet, dass jede Methode ihre eigene InMemory-Datenbank verfügt.</span><span class="sxs-lookup"><span data-stu-id="95c98-132">Each test method specifies a unique database name, meaning each method has its own InMemory database.</span></span>

>[!TIP]
> <span data-ttu-id="95c98-133">Verwenden der `.UseInMemoryDatabase()` Erweiterungsmethode Verweis das NuGet-Paket `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="95c98-133">To use the `.UseInMemoryDatabase()` extension method, reference the NuGet package `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
