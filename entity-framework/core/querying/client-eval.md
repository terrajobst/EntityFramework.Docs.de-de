---
title: Clientauswertung im Vergleich zur Serverauswertung – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
uid: core/querying/client-eval
ms.openlocfilehash: cb207d9e1b1004a4084dd6fc66712183b5bdd5dc
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921705"
---
# <a name="client-vs-server-evaluation"></a><span data-ttu-id="3cb91-102">Clientauswertung im Vergleich zur Serverauswertung</span><span class="sxs-lookup"><span data-stu-id="3cb91-102">Client vs. Server Evaluation</span></span>

<span data-ttu-id="3cb91-103">Entity Framework Core unterstützt Teile der Abfrage, die auf dem Client ausgewertet werden, und Teile der Abfrage, die mithilfe von Push in die Datenbank übertragen werden.</span><span class="sxs-lookup"><span data-stu-id="3cb91-103">Entity Framework Core supports parts of the query being evaluated on the client and parts of it being pushed to the database.</span></span> <span data-ttu-id="3cb91-104">Der Datenbankanbieter bestimmt, welche Teile der Abfrage in der Datenbank ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="3cb91-104">It is up to the database provider to determine which parts of the query will be evaluated in the database.</span></span>

> [!TIP]  
> <span data-ttu-id="3cb91-105">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="3cb91-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="client-evaluation"></a><span data-ttu-id="3cb91-106">Clientauswertung</span><span class="sxs-lookup"><span data-stu-id="3cb91-106">Client evaluation</span></span>

<span data-ttu-id="3cb91-107">Im folgenden Beispiel werden URLs für Blogs, die von einer SQL Server-Datenbank zurückgegeben werden, mit einer Hilfsmethode standardisiert.</span><span class="sxs-lookup"><span data-stu-id="3cb91-107">In the following example a helper method is used to standardize URLs for blogs that are returned from a SQL Server database.</span></span> <span data-ttu-id="3cb91-108">Da der SQL Server-Anbieter keinen Einblick darin hat, wie diese Methode implementiert wird, kann sie nicht in SQL verschoben werden.</span><span class="sxs-lookup"><span data-stu-id="3cb91-108">Because the SQL Server provider has no insight into how this method is implemented, it is not possible to translate it into SQL.</span></span> <span data-ttu-id="3cb91-109">Alle anderen Aspekte der Abfrage werden in der Datenbank ausgewertet. Die Rückgabe der `URL` über diese Methode erfolgt jedoch über den Client.</span><span class="sxs-lookup"><span data-stu-id="3cb91-109">All other aspects of the query are evaluated in the database, but passing the returned `URL` through this method is performed on the client.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs?highlight=6)] -->
``` csharp
var blogs = context.Blogs
    .OrderByDescending(blog => blog.Rating)
    .Select(blog => new
    {
        Id = blog.BlogId,
        Url = StandardizeUrl(blog.Url)
    })
    .ToList();
```

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs)] -->
``` csharp
public static string StandardizeUrl(string url)
{
    url = url.ToLower();

    if (!url.StartsWith("http://"))
    {
        url = string.Concat("http://", url);
    }

    return url;
}
```

## <a name="client-evaluation-performance-issues"></a><span data-ttu-id="3cb91-110">Leistungsprobleme bei der Clientauswertung</span><span class="sxs-lookup"><span data-stu-id="3cb91-110">Client evaluation performance issues</span></span>

<span data-ttu-id="3cb91-111">Die Clientauswertung kann sehr hilfreich sein, in manchen Fällen kann sie jedoch auch zu einer schlechten Leistung führen.</span><span class="sxs-lookup"><span data-stu-id="3cb91-111">While client evaluation can be very useful, in some instances it can result in poor performance.</span></span> <span data-ttu-id="3cb91-112">Betrachten Sie die folgende Abfrage, in der die Hilfsmethode jetzt in einem Filter verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="3cb91-112">Consider the following query, where the helper method is now used in a filter.</span></span> <span data-ttu-id="3cb91-113">Da dieser Vorgang nicht in der Datenbank durchgeführt werden kann, werden sämtliche Daten mithilfe von Pull in den Speicher übertragen. Anschließend wird der Filter auf den Client angewendet.</span><span class="sxs-lookup"><span data-stu-id="3cb91-113">Because this can't be performed in the database, all the data is pulled into memory and then the filter is applied on the client.</span></span> <span data-ttu-id="3cb91-114">Abhängig vom Umfang der Daten und davon, wie viele dieser Daten herausgefiltert werden, könnte dies zu einer schlechten Leistung führen.</span><span class="sxs-lookup"><span data-stu-id="3cb91-114">Depending on the amount of data, and how much of that data is filtered out, this could result in poor performance.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

## <a name="client-evaluation-logging"></a><span data-ttu-id="3cb91-115">Protokollieren der Clientauswertung</span><span class="sxs-lookup"><span data-stu-id="3cb91-115">Client evaluation logging</span></span>

<span data-ttu-id="3cb91-116">Standardmäßig protokolliert EF Core bei der Durchführung einer Auswertung eine Warnung.</span><span class="sxs-lookup"><span data-stu-id="3cb91-116">By default, EF Core will log a warning when client evaluation is performed.</span></span> <span data-ttu-id="3cb91-117">Weitere Informationen zum Anzeigen von Protokollierungsausgaben finden Sie unter [Protokollierung](../miscellaneous/logging.md).</span><span class="sxs-lookup"><span data-stu-id="3cb91-117">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> 

## <a name="optional-behavior-throw-an-exception-for-client-evaluation"></a><span data-ttu-id="3cb91-118">Optionales Verhalten: Auslösen einer Ausnahme für die Clientauswertung</span><span class="sxs-lookup"><span data-stu-id="3cb91-118">Optional behavior: throw an exception for client evaluation</span></span>

<span data-ttu-id="3cb91-119">Sie können das Verhalten bei einer Clientauswertung auslösen oder nichts unternehmen.</span><span class="sxs-lookup"><span data-stu-id="3cb91-119">You can change the behavior when client evaluation occurs to either throw or do nothing.</span></span> <span data-ttu-id="3cb91-120">Dies geschieht, wenn die Optionen für Ihren Kontext – normalerweise in `DbContext.OnConfiguring` oder `Startup.cs` – bei Verwendung von ASP.NET Core eingerichtet werden.</span><span class="sxs-lookup"><span data-stu-id="3cb91-120">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
