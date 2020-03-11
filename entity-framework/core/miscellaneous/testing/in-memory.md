---
title: Testen mit inMemory-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 18641677098c20d9172136b07868dcb647d189c6
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414026"
---
# <a name="testing-with-inmemory"></a><span data-ttu-id="be034-102">Testen mit InMemory</span><span class="sxs-lookup"><span data-stu-id="be034-102">Testing with InMemory</span></span>

<span data-ttu-id="be034-103">Der inMemory-Anbieter ist nützlich, wenn Sie Komponenten mit etwas, das eine Verbindung mit der realen Datenbank herstellt, testen möchten, ohne den Aufwand tatsächlicher Daten Bank Vorgänge zu erhöhen.</span><span class="sxs-lookup"><span data-stu-id="be034-103">The InMemory provider is useful when you want to test components using something that approximates connecting to the real database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="be034-104">Das in diesem Artikel verwendete [Beispiel](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="be034-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub.</span></span>

## <a name="inmemory-is-not-a-relational-database"></a><span data-ttu-id="be034-105">InMemory ist keine relationale Datenbank.</span><span class="sxs-lookup"><span data-stu-id="be034-105">InMemory is not a relational database</span></span>

<span data-ttu-id="be034-106">EF Core Datenbankanbieter müssen keine relationalen Datenbanken sein.</span><span class="sxs-lookup"><span data-stu-id="be034-106">EF Core database providers do not have to be relational databases.</span></span> <span data-ttu-id="be034-107">InMemory ist als allgemeine Datenbank für Tests konzipiert und ist nicht für die imitiert einer relationalen Datenbank konzipiert.</span><span class="sxs-lookup"><span data-stu-id="be034-107">InMemory is designed to be a general purpose database for testing, and is not designed to mimic a relational database.</span></span>

<span data-ttu-id="be034-108">Beispiele hierfür sind:</span><span class="sxs-lookup"><span data-stu-id="be034-108">Some examples of this include:</span></span>

* <span data-ttu-id="be034-109">InMemory ermöglicht Ihnen das Speichern von Daten, die gegen Einschränkungen der referenziellen Integrität in einer relationalen Datenbank verstoßen.</span><span class="sxs-lookup"><span data-stu-id="be034-109">InMemory will allow you to save data that would violate referential integrity constraints in a relational database.</span></span>
* <span data-ttu-id="be034-110">Wenn Sie defaultvaluesql (String) für eine Eigenschaft im Modell verwenden, handelt es sich hierbei um eine relationale Datenbank-API, die bei der Ausführung für inMemory keine Auswirkung hat.</span><span class="sxs-lookup"><span data-stu-id="be034-110">If you use DefaultValueSql(string) for a property in your model, this is a relational database API and will have no effect when running against InMemory.</span></span>
* <span data-ttu-id="be034-111">Parallelität [über Timestamp/Row-Version](xref:core/modeling/concurrency#timestamprowversion) (`[Timestamp]` oder `IsRowVersion`) wird nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="be034-111">[Concurrency via Timestamp/row version](xref:core/modeling/concurrency#timestamprowversion) (`[Timestamp]` or `IsRowVersion`) is not supported.</span></span> <span data-ttu-id="be034-112">[Dbupdateconcurrency cyexception](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) wird nicht ausgelöst, wenn ein Update mit einem alten Parallelitäts Token ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="be034-112">No [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) will be thrown if an update is done using an old concurrency token.</span></span>

> [!TIP]  
> <span data-ttu-id="be034-113">Für viele Testzwecke sind diese Unterschiede nicht von Bedeutung.</span><span class="sxs-lookup"><span data-stu-id="be034-113">For many test purposes these differences will not matter.</span></span> <span data-ttu-id="be034-114">Wenn Sie jedoch eine Prüfung auf etwas durchsetzen möchten, das sich wie eine echte relationale Datenbank verhält, sollten Sie den [sqlite-in-Memory-Modus](sqlite.md)verwenden.</span><span class="sxs-lookup"><span data-stu-id="be034-114">However, if you want to test against something that behaves more like a true relational database, then consider using [SQLite in-memory mode](sqlite.md).</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="be034-115">Beispiel für ein Testszenario</span><span class="sxs-lookup"><span data-stu-id="be034-115">Example testing scenario</span></span>

<span data-ttu-id="be034-116">Beachten Sie den folgenden Dienst, der es Anwendungscode ermöglicht, einige mit Blogs verbundene Vorgänge auszuführen.</span><span class="sxs-lookup"><span data-stu-id="be034-116">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="be034-117">Intern wird eine `DbContext` verwendet, die eine Verbindung mit einer SQL Server Datenbank herstellt.</span><span class="sxs-lookup"><span data-stu-id="be034-117">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="be034-118">Es wäre hilfreich, diesen Kontext auszutauschen, um eine Verbindung mit einer inMemory-Datenbank herzustellen, damit wir effiziente Tests für diesen Dienst schreiben können, ohne den Code ändern zu müssen, oder viel Arbeit erledigen müssen, um einen Test Double-Kontext zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="be034-118">It would be useful to swap this context to connect to an InMemory database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="be034-119">Vorbereiten Ihres Kontexts</span><span class="sxs-lookup"><span data-stu-id="be034-119">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="be034-120">Vermeiden der Konfiguration von zwei Datenbankanbietern</span><span class="sxs-lookup"><span data-stu-id="be034-120">Avoid configuring two database providers</span></span>

<span data-ttu-id="be034-121">In den Tests werden Sie den Kontext extern so konfigurieren, dass der inMemory-Anbieter verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="be034-121">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="be034-122">Wenn Sie einen Datenbankanbieter konfigurieren, indem Sie `OnConfiguring` in ihrem Kontext überschreiben, müssen Sie einigen bedingten Code hinzufügen, um sicherzustellen, dass Sie den Datenbankanbieter nur dann konfigurieren, wenn noch keiner konfiguriert wurde.</span><span class="sxs-lookup"><span data-stu-id="be034-122">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> <span data-ttu-id="be034-123">Wenn Sie ASP.net Core verwenden, sollte dieser Code nicht benötigt werden, da der Datenbankanbieter bereits außerhalb des Kontexts (in Startup.cs) konfiguriert wurde.</span><span class="sxs-lookup"><span data-stu-id="be034-123">If you are using ASP.NET Core, then you should not need this code since your database provider is already configured outside of the context (in Startup.cs).</span></span>

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="be034-124">Hinzufügen eines Konstruktors zum Testen</span><span class="sxs-lookup"><span data-stu-id="be034-124">Add a constructor for testing</span></span>

<span data-ttu-id="be034-125">Die einfachste Möglichkeit, Tests für eine andere Datenbank zu aktivieren, besteht darin, den Kontext so zu ändern, dass er einen Konstruktor verfügbar macht, der eine `DbContextOptions<TContext>`akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="be034-125">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="be034-126">`DbContextOptions<TContext>` weist den Kontext alle seine Einstellungen zu, z. b. die Datenbank, mit der eine Verbindung hergestellt werden soll.</span><span class="sxs-lookup"><span data-stu-id="be034-126">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="be034-127">Dabei handelt es sich um das gleiche Objekt, das erstellt wird, indem die on-Konfigurations Methode in ihrem Kontext ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="be034-127">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="be034-128">Schreiben von Tests</span><span class="sxs-lookup"><span data-stu-id="be034-128">Writing tests</span></span>

<span data-ttu-id="be034-129">Der Schlüssel zum Testen mit diesem Anbieter ist die Möglichkeit, den Kontext anzuweisen, den inMemory-Anbieter zu verwenden, und den Bereich des in-Memory Database zu steuern.</span><span class="sxs-lookup"><span data-stu-id="be034-129">The key to testing with this provider is the ability to tell the context to use the InMemory provider, and control the scope of the in-memory database.</span></span> <span data-ttu-id="be034-130">In der Regel benötigen Sie eine saubere Datenbank für jede Testmethode.</span><span class="sxs-lookup"><span data-stu-id="be034-130">Typically you want a clean database for each test method.</span></span>

<span data-ttu-id="be034-131">Im folgenden finden Sie ein Beispiel für eine Testklasse, die die inMemory-Datenbank verwendet.</span><span class="sxs-lookup"><span data-stu-id="be034-131">Here is an example of a test class that uses the InMemory database.</span></span> <span data-ttu-id="be034-132">Jede Testmethode gibt einen eindeutigen Datenbanknamen an, was bedeutet, dass jede Methode über eine eigene inMemory-Datenbank verfügt.</span><span class="sxs-lookup"><span data-stu-id="be034-132">Each test method specifies a unique database name, meaning each method has its own InMemory database.</span></span>

>[!TIP]
> <span data-ttu-id="be034-133">Wenn Sie die `.UseInMemoryDatabase()`-Erweiterungsmethode verwenden möchten, verweisen Sie auf das nuget-Paket [Microsoft. entityframeworkcore. inMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="be034-133">To use the `.UseInMemoryDatabase()` extension method, reference the NuGet package [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
