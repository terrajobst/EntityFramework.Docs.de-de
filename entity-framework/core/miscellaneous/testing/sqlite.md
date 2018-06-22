---
title: Testen mit SQLite – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: 8fcb4b1a37da6f52219ebe3c672160627fe28fab
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052700"
---
# <a name="testing-with-sqlite"></a><span data-ttu-id="64c35-102">Testen mit SQLite</span><span class="sxs-lookup"><span data-stu-id="64c35-102">Testing with SQLite</span></span>

<span data-ttu-id="64c35-103">SQLite ist einen in-Memory-Modus, der Ihnen ermöglicht, SQLite verwenden, um Tests für eine relationale Datenbank, ohne den Aufwand für die tatsächliche Datenbankvorgänge zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="64c35-103">SQLite has an in-memory mode that allows you to use SQLite to write tests against a relational database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="64c35-104">Sie können anzeigen, dass dieser Artikel [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) auf GitHub</span><span class="sxs-lookup"><span data-stu-id="64c35-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="64c35-105">Beispiel-Testszenario</span><span class="sxs-lookup"><span data-stu-id="64c35-105">Example testing scenario</span></span>

<span data-ttu-id="64c35-106">Betrachten Sie den folgenden Dienst, der Anwendungscode im Zusammenhang mit Blogs Eingriffe ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="64c35-106">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="64c35-107">Er verwendet intern eine `DbContext` , die eine Verbindung mit einer SQL Server-Datenbank her.</span><span class="sxs-lookup"><span data-stu-id="64c35-107">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="64c35-108">Es wäre hilfreich zum Austauschen von diesem Kontext für die Verbindung mit einer in-Memory-SQLite-Datenbank, damit wir effiziente Tests für diesen Dienst schreiben, ohne den Code ändern, oder führen Sie einen Großteil der Arbeit, die zum Erstellen eines Tests können doppelte des Kontexts.</span><span class="sxs-lookup"><span data-stu-id="64c35-108">It would be useful to swap this context to connect to an in-memory SQLite database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="64c35-109">Bereiten Sie den Kontext</span><span class="sxs-lookup"><span data-stu-id="64c35-109">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="64c35-110">Vermeiden Sie die Konfiguration von zwei Datenbankanbieter</span><span class="sxs-lookup"><span data-stu-id="64c35-110">Avoid configuring two database providers</span></span>

<span data-ttu-id="64c35-111">In den Tests wirst du extern konfigurieren Sie den Kontext des InMemory-Anbieters verwenden.</span><span class="sxs-lookup"><span data-stu-id="64c35-111">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="64c35-112">Wenn Sie einen Datenbankanbieter durch Außerkraftsetzen konfigurieren `OnConfiguring` in den Kontext, dann müssen Sie fügen Sie bedingte Code zum sicherstellen, dass Sie nur den Datenbankanbieter konfigurieren, wenn Sie noch nicht konfiguriert wurde.</span><span class="sxs-lookup"><span data-stu-id="64c35-112">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

> [!TIP]  
> <span data-ttu-id="64c35-113">Bei Verwendung von ASP.NET Core sollte dann nicht mit diesem Code erforderlich, da der Datenbankanbieter außerhalb des Kontexts (in "Startup.cs) konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="64c35-113">If you are using ASP.NET Core, then you should not need this code since your database provider is configured outside of the context (in Startup.cs).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="64c35-114">Fügen Sie einen Konstruktor zu Testzwecken</span><span class="sxs-lookup"><span data-stu-id="64c35-114">Add a constructor for testing</span></span>

<span data-ttu-id="64c35-115">Die einfachste Methode zum Aktivieren von Tests mit einer anderen Datenbank so ändern Sie den Kontext, um einen Konstruktor verfügbar machen, das akzeptiert, wird eine `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="64c35-115">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="64c35-116">`DbContextOptions<TContext>`Gibt dem Kontext alle zugehörigen Einstellungen, z. B. welche Datenbank für die Verbindung.</span><span class="sxs-lookup"><span data-stu-id="64c35-116">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="64c35-117">Dies ist das gleiche Objekt, das erstellt wird, durch die OnConfiguring-Methode in Ihrem Kontext ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="64c35-117">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="64c35-118">Schreiben von tests</span><span class="sxs-lookup"><span data-stu-id="64c35-118">Writing tests</span></span>

<span data-ttu-id="64c35-119">Der Schlüssel für das Testen von mit diesem Anbieter ist die Fähigkeit, teilen Sie den Kontext SQLite verwenden und Steuern des Gültigkeitsbereichs der Datenbank im Arbeitsspeicher.</span><span class="sxs-lookup"><span data-stu-id="64c35-119">The key to testing with this provider is the ability to tell the context to use SQLite, and control the scope of the in-memory database.</span></span> <span data-ttu-id="64c35-120">Der Bereich der Datenbank wird gesteuert, durch Öffnen und schließen die Verbindung.</span><span class="sxs-lookup"><span data-stu-id="64c35-120">The scope of the database is controlled by opening and closing the connection.</span></span> <span data-ttu-id="64c35-121">Die Datenbank ist auf die Dauer beschränkt, die die Verbindung geöffnet ist.</span><span class="sxs-lookup"><span data-stu-id="64c35-121">The database is scoped to the duration that the connection is open.</span></span> <span data-ttu-id="64c35-122">Sie möchten in der Regel eine fehlerfreie Datenbank für jede Testmethode.</span><span class="sxs-lookup"><span data-stu-id="64c35-122">Typically you want a clean database for each test method.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
