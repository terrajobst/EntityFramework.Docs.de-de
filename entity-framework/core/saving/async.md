---
title: 'Asynchroner Speichervorgang: EF Core'
author: rowanmiller
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
uid: core/saving/async
ms.openlocfilehash: 0823b86f0579dd3e42f6bd2aebfb433d3cbe00ab
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413690"
---
# <a name="asynchronous-saving"></a><span data-ttu-id="ae850-102">Asynchroner Speichervorgang</span><span class="sxs-lookup"><span data-stu-id="ae850-102">Asynchronous Saving</span></span>

<span data-ttu-id="ae850-103">Mit dem asynchronen Speichervorgang wird vermieden, dass ein Thread blockiert wird, während die Änderungen in die Datenbank geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="ae850-103">Asynchronous saving avoids blocking a thread while the changes are written to the database.</span></span> <span data-ttu-id="ae850-104">Dies hilft dabei, das Abstürzen der Benutzeroberfläche einer Thick-Client-Anwendung zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="ae850-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="ae850-105">Asynchrone Vorgänge können außerdem den Durchsatz in Webanwendungen erhöhen, in denen der Thread für andere Anforderungen freigegeben werden kann, während der Datenbankvorgang abgeschlossen wird.</span><span class="sxs-lookup"><span data-stu-id="ae850-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="ae850-106">Weitere Informationen finden Sie unter [Asynchrone Programmierung in C#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="ae850-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="ae850-107">EF Core unterstützt nicht die Ausführung mehrerer paralleler Vorgänge, die auf derselben Kontextinstanz ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="ae850-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="ae850-108">Sie sollten immer auf den Abschluss eines Vorgangs warten, bevor Sie den nächsten starten.</span><span class="sxs-lookup"><span data-stu-id="ae850-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="ae850-109">In der Regel erfolgt dies für alle asynchronen Vorgänge durch das Schlüsselwort `await`.</span><span class="sxs-lookup"><span data-stu-id="ae850-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="ae850-110">Entity Framework Core stellt `DbContext.SaveChangesAsync()` als asynchrone Alternative zu `DbContext.SaveChanges()` bereit.</span><span class="sxs-lookup"><span data-stu-id="ae850-110">Entity Framework Core provides `DbContext.SaveChangesAsync()` as an asynchronous alternative to `DbContext.SaveChanges()`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Async/Sample.cs#Sample)]
