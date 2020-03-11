---
title: Testen mit SQLite-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: f7f847d8c766c0d4d7577ea6760ee72a17f84933
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414632"
---
# <a name="testing-with-sqlite"></a><span data-ttu-id="58ceb-102">Testen mit SQLite</span><span class="sxs-lookup"><span data-stu-id="58ceb-102">Testing with SQLite</span></span>

<span data-ttu-id="58ceb-103">SQLite verfügt über einen in-Memory-Modus, der Ihnen die Verwendung von SQLite zum Schreiben von Tests für eine relationale Datenbank ohne den Aufwand tatsächlicher Daten Bank Vorgänge ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="58ceb-103">SQLite has an in-memory mode that allows you to use SQLite to write tests against a relational database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="58ceb-104">Sie können das [Beispiel](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) dieses Artikels auf GitHub anzeigen.</span><span class="sxs-lookup"><span data-stu-id="58ceb-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="58ceb-105">Beispiel für ein Testszenario</span><span class="sxs-lookup"><span data-stu-id="58ceb-105">Example testing scenario</span></span>

<span data-ttu-id="58ceb-106">Beachten Sie den folgenden Dienst, der es Anwendungscode ermöglicht, einige mit Blogs verbundene Vorgänge auszuführen.</span><span class="sxs-lookup"><span data-stu-id="58ceb-106">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="58ceb-107">Intern wird eine `DbContext` verwendet, die eine Verbindung mit einer SQL Server Datenbank herstellt.</span><span class="sxs-lookup"><span data-stu-id="58ceb-107">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="58ceb-108">Es wäre hilfreich, diesen Kontext auszutauschen, um eine Verbindung mit einer in-Memory-SQLite-Datenbank herzustellen, damit wir effiziente Tests für diesen Dienst schreiben können, ohne den Code ändern zu müssen, oder viel Arbeit erledigen, um einen doppelten Test des Kontexts zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="58ceb-108">It would be useful to swap this context to connect to an in-memory SQLite database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="58ceb-109">Vorbereiten Ihres Kontexts</span><span class="sxs-lookup"><span data-stu-id="58ceb-109">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="58ceb-110">Vermeiden der Konfiguration von zwei Datenbankanbietern</span><span class="sxs-lookup"><span data-stu-id="58ceb-110">Avoid configuring two database providers</span></span>

<span data-ttu-id="58ceb-111">In den Tests werden Sie den Kontext extern so konfigurieren, dass der inMemory-Anbieter verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="58ceb-111">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="58ceb-112">Wenn Sie einen Datenbankanbieter konfigurieren, indem Sie `OnConfiguring` in ihrem Kontext überschreiben, müssen Sie einigen bedingten Code hinzufügen, um sicherzustellen, dass Sie den Datenbankanbieter nur dann konfigurieren, wenn noch keiner konfiguriert wurde.</span><span class="sxs-lookup"><span data-stu-id="58ceb-112">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

> [!TIP]  
> <span data-ttu-id="58ceb-113">Wenn Sie ASP.net Core verwenden, sollte dieser Code nicht benötigt werden, da der Datenbankanbieter außerhalb des Kontexts konfiguriert ist (in Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="58ceb-113">If you are using ASP.NET Core, then you should not need this code since your database provider is configured outside of the context (in Startup.cs).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="58ceb-114">Hinzufügen eines Konstruktors zum Testen</span><span class="sxs-lookup"><span data-stu-id="58ceb-114">Add a constructor for testing</span></span>

<span data-ttu-id="58ceb-115">Die einfachste Möglichkeit, Tests für eine andere Datenbank zu aktivieren, besteht darin, den Kontext so zu ändern, dass er einen Konstruktor verfügbar macht, der eine `DbContextOptions<TContext>`akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="58ceb-115">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="58ceb-116">`DbContextOptions<TContext>` weist den Kontext alle seine Einstellungen zu, z. b. die Datenbank, mit der eine Verbindung hergestellt werden soll.</span><span class="sxs-lookup"><span data-stu-id="58ceb-116">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="58ceb-117">Dabei handelt es sich um das gleiche Objekt, das erstellt wird, indem die on-Konfigurations Methode in ihrem Kontext ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="58ceb-117">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="58ceb-118">Schreiben von Tests</span><span class="sxs-lookup"><span data-stu-id="58ceb-118">Writing tests</span></span>

<span data-ttu-id="58ceb-119">Der Schlüssel zum Testen mit diesem Anbieter ist die Möglichkeit, den Kontext anzuweisen, SQLite zu verwenden, und den Bereich des in-Memory Database zu steuern.</span><span class="sxs-lookup"><span data-stu-id="58ceb-119">The key to testing with this provider is the ability to tell the context to use SQLite, and control the scope of the in-memory database.</span></span> <span data-ttu-id="58ceb-120">Der Bereich der Datenbank wird gesteuert, indem die Verbindung geöffnet und geschlossen wird.</span><span class="sxs-lookup"><span data-stu-id="58ceb-120">The scope of the database is controlled by opening and closing the connection.</span></span> <span data-ttu-id="58ceb-121">Die Datenbank bezieht sich auf die Dauer, in der die Verbindung geöffnet ist.</span><span class="sxs-lookup"><span data-stu-id="58ceb-121">The database is scoped to the duration that the connection is open.</span></span> <span data-ttu-id="58ceb-122">In der Regel benötigen Sie eine saubere Datenbank für jede Testmethode.</span><span class="sxs-lookup"><span data-stu-id="58ceb-122">Typically you want a clean database for each test method.</span></span>

>[!TIP]
> <span data-ttu-id="58ceb-123">Wenn Sie `SqliteConnection()` und die `.UseSqlite()`-Erweiterungsmethode verwenden möchten, verweisen Sie auf das nuget-Paket [Microsoft. entityframeworkcore. sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="58ceb-123">To use `SqliteConnection()` and the `.UseSqlite()` extension method, reference the NuGet package [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
