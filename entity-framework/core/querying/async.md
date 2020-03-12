---
title: 'Asynchrone Abfragen: EF Core'
author: smitpatel
ms.date: 10/03/2019
ms.assetid: b6429b14-cba0-4af4-878f-b829777c89cb
uid: core/querying/async
ms.openlocfilehash: ce26db32a616dac5edac2a8451014ae63cbfc12d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413780"
---
# <a name="asynchronous-queries"></a><span data-ttu-id="be16b-102">Asynchrone Abfragen</span><span class="sxs-lookup"><span data-stu-id="be16b-102">Asynchronous Queries</span></span>

<span data-ttu-id="be16b-103">Asynchrone Abfragen vermeiden, dass ein Thread blockiert wird, während die Abfrage in der Datenbank ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="be16b-103">Asynchronous queries avoid blocking a thread while the query is executed in the database.</span></span> <span data-ttu-id="be16b-104">Asynchrone Abfragen sind wichtig, um die Reaktionsfähigkeit der Benutzeroberfläche in Thick-Clientanwendungen zu gewährleisten.</span><span class="sxs-lookup"><span data-stu-id="be16b-104">Async queries are important for keeping a responsive UI in thick-client applications.</span></span> <span data-ttu-id="be16b-105">Sie können auch den Durchsatz von Webanwendungen erhöhen, indem sie den Thread für die Behandlung anderer Anforderungen in Webanwendungen freigegeben.</span><span class="sxs-lookup"><span data-stu-id="be16b-105">They can also increase throughput in web applications where they free up the thread to service other requests in web applications.</span></span> <span data-ttu-id="be16b-106">Weitere Informationen finden Sie unter [Asynchrone Programmierung in C#](/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="be16b-106">For more information, see [Asynchronous Programming in C#](/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="be16b-107">EF Core unterstützt nicht die Ausführung mehrerer paralleler Vorgänge, die auf derselben Kontextinstanz ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="be16b-107">EF Core doesn't support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="be16b-108">Sie sollten immer auf den Abschluss eines Vorgangs warten, bevor Sie den nächsten starten.</span><span class="sxs-lookup"><span data-stu-id="be16b-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="be16b-109">In der Regel erfolgt dies für alle asynchronen Vorgänge durch das Schlüsselwort `await`.</span><span class="sxs-lookup"><span data-stu-id="be16b-109">This is typically done by using the `await` keyword on each async operation.</span></span>

<span data-ttu-id="be16b-110">Entity Framework Core stellt asynchrone Erweiterungsmethoden bereit, die den LINQ-Methoden ähneln, die eine Abfrage ausführen und Ergebnisse zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="be16b-110">Entity Framework Core provides a set of async extension methods similar to the LINQ methods, which execute a query and return results.</span></span> <span data-ttu-id="be16b-111">Beispiele dafür sind. `ToListAsync()`, `ToArrayAsync()` und `SingleAsync()`.</span><span class="sxs-lookup"><span data-stu-id="be16b-111">Examples include `ToListAsync()`, `ToArrayAsync()`, `SingleAsync()`.</span></span> <span data-ttu-id="be16b-112">Für einige LINQ-Operatoren, z. B. `Where(...)` oder `OrderBy(...)`, gibt es keine asynchronen Versionen, da diese Methoden nur die LINQ-Ausdrucksbaumstruktur erstellen und nicht die Ausführung der Abfrage in der Datenbank auslösen.</span><span class="sxs-lookup"><span data-stu-id="be16b-112">There are no async versions of some LINQ operators such as `Where(...)` or `OrderBy(...)` because these methods only build up the LINQ expression tree and don't cause the query to be executed in the database.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="be16b-113">Die asynchronen Erweiterungsmethoden von EF Core werden im Namespace `Microsoft.EntityFrameworkCore` definiert.</span><span class="sxs-lookup"><span data-stu-id="be16b-113">The EF Core async extension methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="be16b-114">Dieser Namespace muss importiert werden, damit die Methoden verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="be16b-114">This namespace must be imported for the methods to be available.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Async/Sample.cs#ToListAsync)]
