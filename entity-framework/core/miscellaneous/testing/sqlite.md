---
title: Testen mit SQLite - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: bc9d6768a90ce17160c4126d2a68fddaa30d63de
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996867"
---
# <a name="testing-with-sqlite"></a><span data-ttu-id="a23ce-102">Testen mit SQLite</span><span class="sxs-lookup"><span data-stu-id="a23ce-102">Testing with SQLite</span></span>

<span data-ttu-id="a23ce-103">SQLite ist einen in-Memory-Modus, der Ihnen ermöglicht, SQLite verwenden, um Tests für eine relationale Datenbank, ohne den Aufwand für die tatsächliche Datenbankvorgänge zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="a23ce-103">SQLite has an in-memory mode that allows you to use SQLite to write tests against a relational database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="a23ce-104">Sie finden dieses Artikels [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) auf GitHub</span><span class="sxs-lookup"><span data-stu-id="a23ce-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="a23ce-105">Beispiel-Szenario</span><span class="sxs-lookup"><span data-stu-id="a23ce-105">Example testing scenario</span></span>

<span data-ttu-id="a23ce-106">Betrachten Sie den folgenden Dienst, mit dem Anwendungscode zum Durchführen bestimmter Vorgänge im Zusammenhang mit Blogs zu können.</span><span class="sxs-lookup"><span data-stu-id="a23ce-106">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="a23ce-107">Intern verwendet eine `DbContext` , die eine Verbindung mit SQL Server-Datenbank her.</span><span class="sxs-lookup"><span data-stu-id="a23ce-107">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="a23ce-108">Es wäre nützlich, um diesem Kontext für die Verbindung eine SQLite-Datenbank im Arbeitsspeicher, damit wir können effiziente Tests für diesen Dienst schreiben, ohne den Code ändern zu müssen, oder einen Großteil der Arbeit beim Erstellen eines Tests Austauschen des Kontexts double.</span><span class="sxs-lookup"><span data-stu-id="a23ce-108">It would be useful to swap this context to connect to an in-memory SQLite database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="a23ce-109">Bereiten Sie den Kontext</span><span class="sxs-lookup"><span data-stu-id="a23ce-109">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="a23ce-110">Vermeiden Sie die Konfiguration von zwei Datenbankanbieter</span><span class="sxs-lookup"><span data-stu-id="a23ce-110">Avoid configuring two database providers</span></span>

<span data-ttu-id="a23ce-111">In den Tests wird den Kontext zum Verwenden des InMemory-Anbieters extern konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a23ce-111">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="a23ce-112">Wenn Sie durch das Überschreiben ein Datenbankanbieters konfigurieren `OnConfiguring` im Kontext Ihrer müssen Sie fügen der bedingten Code, um sicherzustellen, dass Sie nur den Datenbankanbieter konfigurieren, wenn eine nicht bereits konfiguriert wurde.</span><span class="sxs-lookup"><span data-stu-id="a23ce-112">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

> [!TIP]  
> <span data-ttu-id="a23ce-113">Wenn Sie ASP.NET Core verwenden, sollte dann nicht dieser Code erforderlich, da Ihr Datenbankanbieter außerhalb des Kontexts (in "Startup.cs") konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="a23ce-113">If you are using ASP.NET Core, then you should not need this code since your database provider is configured outside of the context (in Startup.cs).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="a23ce-114">Fügen Sie einen Konstruktor für das Testen</span><span class="sxs-lookup"><span data-stu-id="a23ce-114">Add a constructor for testing</span></span>

<span data-ttu-id="a23ce-115">Die einfachste Möglichkeit zum Aktivieren von Tests mit einer anderen Datenbank ist so ändern Sie den Kontext, um einen Konstruktor verfügbar machen, die akzeptiert eine `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="a23ce-115">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="a23ce-116">`DbContextOptions<TContext>` Gibt dem Kontext alle zugehörigen Einstellungen, z. B. welche Datenbank für die Verbindung.</span><span class="sxs-lookup"><span data-stu-id="a23ce-116">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="a23ce-117">Dies ist das gleiche Objekt, das durch Ausführen der OnConfiguring-Methode im Kontext Ihrer basiert.</span><span class="sxs-lookup"><span data-stu-id="a23ce-117">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="a23ce-118">Schreiben von tests</span><span class="sxs-lookup"><span data-stu-id="a23ce-118">Writing tests</span></span>

<span data-ttu-id="a23ce-119">Die Taste, um Tests mit dieser Anbieter ist die Möglichkeit, teilen Sie den Kontext, SQLite, und Steuern des Gültigkeitsbereichs der in-Memory-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="a23ce-119">The key to testing with this provider is the ability to tell the context to use SQLite, and control the scope of the in-memory database.</span></span> <span data-ttu-id="a23ce-120">Der Bereich der Datenbank wird gesteuert, durch Öffnen und schließen die Verbindung.</span><span class="sxs-lookup"><span data-stu-id="a23ce-120">The scope of the database is controlled by opening and closing the connection.</span></span> <span data-ttu-id="a23ce-121">Die Datenbank ist für die Dauer beschränkt, die die Verbindung geöffnet ist.</span><span class="sxs-lookup"><span data-stu-id="a23ce-121">The database is scoped to the duration that the connection is open.</span></span> <span data-ttu-id="a23ce-122">Möchten Sie in der Regel eine fehlerfreie Datenbank, für jede Testmethode.</span><span class="sxs-lookup"><span data-stu-id="a23ce-122">Typically you want a clean database for each test method.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
