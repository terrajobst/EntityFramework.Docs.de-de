---
title: "Asynchrone Abfragen – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
ms.technology: entity-framework-core
uid: core/querying/async
ms.openlocfilehash: 6554f04d0edfe0ca2ee72ebed8b878a1997a9500
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="asynchronous-queries"></a><span data-ttu-id="c9f29-102">Asynchrone Abfragen</span><span class="sxs-lookup"><span data-stu-id="c9f29-102">Asynchronous Queries</span></span>

<span data-ttu-id="c9f29-103">Asynchrone Abfragen vermeiden, einen Thread zu blockieren, während die Abfrage in der Datenbank ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="c9f29-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="c9f29-104">Dies kann nützlich, um zu vermeiden, fixieren die Benutzeroberfläche einer Anwendung dick-Client sein.</span><span class="sxs-lookup"><span data-stu-id="c9f29-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="c9f29-105">Asynchrone Vorgänge können auch in einer Web-Anwendung Durchsatz zu erhöhen, in dem der Thread freigegeben werden kann, um andere Anforderungen zu verarbeiten, während die Datenbankvorgang abgeschlossen wird.</span><span class="sxs-lookup"><span data-stu-id="c9f29-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="c9f29-106">Weitere Informationen finden Sie unter [asynchrone Programmierung in c#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="c9f29-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="c9f29-107">EF Core unterstützt nicht mehrere parallele Vorgänge, die auf dieselbe Kontextinstanz ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="c9f29-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="c9f29-108">Sie sollten immer vor dem nächsten Vorgang Abschluss eines Vorgangs zu warten.</span><span class="sxs-lookup"><span data-stu-id="c9f29-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="c9f29-109">Dies erfolgt in der Regel mithilfe der `await` Schlüsselwort bei jedem asynchronen Vorgang.</span><span class="sxs-lookup"><span data-stu-id="c9f29-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="c9f29-110">Entity Framework Core bietet eine Reihe von asynchronen Erweiterungsmethoden, die verwendet werden können, als Alternative zu den LINQ-Methoden, die dazu führen, dass eine Abfrage ausgeführt werden und die Ergebnisse zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="c9f29-110">Entity Framework Core provides a set of asynchronous extension methods that can be used as an alternative to the LINQ methods that cause a query to be executed and results returned.</span></span> <span data-ttu-id="c9f29-111">Beispiele hierfür sind `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`usw. Es sind nicht asynchronen Versionen der LINQ-Operatoren wie z. B. `Where(...)`, `OrderBy(...)`usw., da diese Methoden nur die LINQ-Ausdrucksbaumstruktur zu erstellen und führen nicht dazu, dass die Abfrage in der Datenbank ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="c9f29-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. There are not async versions of LINQ operators such as `Where(...)`, `OrderBy(...)`, etc. because these methods only build up the LINQ expression tree and do not cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="c9f29-112">Die EF Core Async-Erweiterungsmethoden werden definiert, der `Microsoft.EntityFrameworkCore` Namespace.</span><span class="sxs-lookup"><span data-stu-id="c9f29-112">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="c9f29-113">Dieser Namespace muss für die Methoden zur Verfügung stehen, importiert werden.</span><span class="sxs-lookup"><span data-stu-id="c9f29-113">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
