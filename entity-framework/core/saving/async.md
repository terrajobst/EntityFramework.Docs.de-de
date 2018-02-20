---
title: Asynchrones Speichern - EF Core
author: rowanmiller
ms.author: divega
ms.date: 01/24/2017
ms.assetid: b64a606e-ecd9-4807-829a-b6ec05ade33f
ms.technology: entity-framework-core
uid: core/saving/async
ms.openlocfilehash: 640e5f131b0e9ef4e5028d1dcaf80f3e5bbd9d9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="asynchronous-saving"></a><span data-ttu-id="43d01-102">Asynchrones Speichern</span><span class="sxs-lookup"><span data-stu-id="43d01-102">Asynchronous Saving</span></span>

<span data-ttu-id="43d01-103">Asynchrones Speichern vermeidet einen Thread blockiert, während die Änderungen in die Datenbank geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="43d01-103">Asynchronous saving avoids blocking a thread while the changes are written to the database.</span></span> <span data-ttu-id="43d01-104">Dies kann nützlich, um zu vermeiden, fixieren die Benutzeroberfläche einer Anwendung dick-Client sein.</span><span class="sxs-lookup"><span data-stu-id="43d01-104">This can be useful to avoid freezing the UI of a thick-client application.</span></span> <span data-ttu-id="43d01-105">Asynchrone Vorgänge können auch in einer Web-Anwendung Durchsatz zu erhöhen, in dem der Thread freigegeben werden kann, um andere Anforderungen zu verarbeiten, während die Datenbankvorgang abgeschlossen wird.</span><span class="sxs-lookup"><span data-stu-id="43d01-105">Asynchronous operations can also increase throughput in a web application, where the thread can be freed up to service other requests while the database operation completes.</span></span> <span data-ttu-id="43d01-106">Weitere Informationen finden Sie unter [asynchrone Programmierung in c#](https://docs.microsoft.com/dotnet/csharp/async).</span><span class="sxs-lookup"><span data-stu-id="43d01-106">For more information, see [Asynchronous Programming in C#](https://docs.microsoft.com/dotnet/csharp/async).</span></span>

> [!WARNING]  
> <span data-ttu-id="43d01-107">EF Core unterstützt nicht mehrere parallele Vorgänge, die auf dieselbe Kontextinstanz ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="43d01-107">EF Core does not support multiple parallel operations being run on the same context instance.</span></span> <span data-ttu-id="43d01-108">Sie sollten immer vor dem nächsten Vorgang Abschluss eines Vorgangs zu warten.</span><span class="sxs-lookup"><span data-stu-id="43d01-108">You should always wait for an operation to complete before beginning the next operation.</span></span> <span data-ttu-id="43d01-109">Dies erfolgt in der Regel mithilfe der `await` Schlüsselwort bei jedem asynchronen Vorgang.</span><span class="sxs-lookup"><span data-stu-id="43d01-109">This is typically done by using the `await` keyword on each asynchronous operation.</span></span>

<span data-ttu-id="43d01-110">Entity Framework Core bietet `DbContext.SaveChangesAsync()` als Alternative zur asynchronen `DbContext.SaveChanges()`.</span><span class="sxs-lookup"><span data-stu-id="43d01-110">Entity Framework Core provides `DbContext.SaveChangesAsync()` as an asynchronous alternative to `DbContext.SaveChanges()`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Async/Sample.cs#Sample)]
