---
title: 'Asynchrone Abfragen: EF Core'
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
ms.technology: entity-framework-core
uid: core/querying/async
ms.openlocfilehash: 6554f04d0edfe0ca2ee72ebed8b878a1997a9500
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052680"
---
# <a name="asynchronous-queries"></a><span data-ttu-id="55e52-102">Asynchrone Abfragen</span><span class="sxs-lookup"><span data-stu-id="55e52-102">Asynchronous Queries</span></span>

<span data-ttu-id="55e52-103">Asynchrone Abfragen vermeiden, dass ein Thread blockiert wird, während die Abfrage in der Datenbank ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="55e52-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="55e52-104">Dies hilft dabei, das Abstürzen der Benutzeroberfläche einer Thick-Client-Anwendung zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="55e52-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="55e52-105">Asynchrone Vorgänge können außerdem den Durchsatz in Webanwendungen erhöhen, in denen der Thread für andere Anforderungen freigegeben werden kann, während der Datenbankvorgang abgeschlossen wird.</span><span class="sxs-lookup"><span data-stu-id="55e52-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="55e52-106">Weitere Informationen finden Sie unter [Asynchrone Programmierung in C#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="55e52-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="55e52-107">EF Core unterstützt nicht die Ausführung mehrerer paralleler Vorgänge, die auf derselben Kontextinstanz ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="55e52-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="55e52-108">Sie sollten immer auf den Abschluss eines Vorgangs warten, bevor Sie den nächsten starten.</span><span class="sxs-lookup"><span data-stu-id="55e52-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="55e52-109">In der Regel erfolgt dies für alle asynchronen Vorgänge durch das Schlüsselwort `await`.</span><span class="sxs-lookup"><span data-stu-id="55e52-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="55e52-110">Entity Framework Core stellt eine Reihe von asynchronen Erweiterungsmethoden bereit, die alternativ zu den LINQ-Methoden verwendet werden können, die die Ausführung einer Abfrage und die Rückgabe von Ergebnissen auslösen.</span><span class="sxs-lookup"><span data-stu-id="55e52-110">Entity Framework Core provides a set of asynchronous extension methods that can be used as an alternative to the LINQ methods that cause a query to be executed and results returned.</span></span> <span data-ttu-id="55e52-111">Beispiele dafür sind `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()` usw. Es gibt nicht asynchrone Versionen von LINQ-Operatoren, z.B. `Where(...)`, `OrderBy(...)` usw., da diese Methoden nur die LINQ-Ausdrucksbaumstruktur erstellen und nicht die Abfrage in der Datenbank auslösen.</span><span class="sxs-lookup"><span data-stu-id="55e52-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`, etc. There are not async versions of LINQ operators such as `Where(...)`, `OrderBy(...)`, etc. because these methods only build up the LINQ expression tree and do not cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="55e52-112">Die asynchronen Erweiterungsmethoden von EF Core werden im Namespace `Microsoft.EntityFrameworkCore` definiert.</span><span class="sxs-lookup"><span data-stu-id="55e52-112">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="55e52-113">Dieser Namespace muss importiert werden, damit die Methoden verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="55e52-113">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/Async/Sample.cs#Sample)]
