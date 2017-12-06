---
title: Client im Vergleich zu Evaluierung von Server - Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
ms.technology: entity-framework-core
uid: core/querying/client-eval
ms.openlocfilehash: e1852b780041e9e92fb4d25129175346e3a601a3
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="client-vs-server-evaluation"></a><span data-ttu-id="4d980-102">Client im Vergleich zu Server-Evaluierung</span><span class="sxs-lookup"><span data-stu-id="4d980-102">Client vs. Server Evaluation</span></span>

<span data-ttu-id="4d980-103">Entity Framework Core unterstützt Teile der Abfrage ausgewertet wird, auf dem Client und Teile davon per Push an die Datenbank übermittelt.</span><span class="sxs-lookup"><span data-stu-id="4d980-103">Entity Framework Core supports parts of the query being evaluated on the client and parts of it being pushed to the database.</span></span> <span data-ttu-id="4d980-104">Es liegt im Ermessen der Datenbankanbieter, um zu bestimmen, welche Teile der Abfrage in der Datenbank ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="4d980-104">It is up to the database provider to determine which parts of the query will be evaluated in the database.</span></span>

> [!TIP]  
> <span data-ttu-id="4d980-105">Sie können anzeigen, dass dieser Artikel [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="4d980-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="client-evaluation"></a><span data-ttu-id="4d980-106">Client-Evaluierung</span><span class="sxs-lookup"><span data-stu-id="4d980-106">Client evaluation</span></span>

<span data-ttu-id="4d980-107">Im folgenden Beispiel wird eine Hilfsmethode verwendet, um URLs für Blogs zu standardisieren, die aus einer SQL Server-Datenbank zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="4d980-107">In the following example a helper method is used to standardize URLs for blogs that are returned from a SQL Server database.</span></span> <span data-ttu-id="4d980-108">Da SQL Server-Anbieter keine Einblick hat, wie diese Methode implementiert wird, ist es nicht möglich, die in SQL zu übersetzen.</span><span class="sxs-lookup"><span data-stu-id="4d980-108">Because the SQL Server provider has no insight into how this method is implemented, it is not possible to translate it into SQL.</span></span> <span data-ttu-id="4d980-109">Alle anderen Aspekte der Abfrage ausgewertet werden in der Datenbank, übergeben Sie jedoch das zurückgegebene `URL` über diese Methode auf dem Client ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="4d980-109">All other aspects of the query are evaluated in the database, but passing the returned `URL` through this method is performed on the client.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs?highlight=6)] -->
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

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
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

## <a name="disabling-client-evaluation"></a><span data-ttu-id="4d980-110">Deaktivieren des Client-Bewertung</span><span class="sxs-lookup"><span data-stu-id="4d980-110">Disabling client evaluation</span></span>

<span data-ttu-id="4d980-111">Auswertung der Client kann sehr nützlich sein, kann in einigen Fällen dies zu schlechten Leistungen führen.</span><span class="sxs-lookup"><span data-stu-id="4d980-111">While client evaluation can be very useful, in some instances it can result in poor performance.</span></span> <span data-ttu-id="4d980-112">Betrachten Sie die folgende Abfrage, wobei die Hilfsmethode jetzt in einem Filter verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="4d980-112">Consider the following query, where the helper method is now used in a filter.</span></span> <span data-ttu-id="4d980-113">Da dies in der Datenbank ausgeführt werden kann, werden alle Daten in den Arbeitsspeicher, und klicken Sie dann den Filter gezogen werden auf dem Client angewendet.</span><span class="sxs-lookup"><span data-stu-id="4d980-113">Because this can't be performed in the database, all the data is pulled into memory and then the filter is applied on the client.</span></span> <span data-ttu-id="4d980-114">Abhängig vom Umfang der Daten, und wie viel dieser Daten herausgefiltert wird könnte dies zu schlechten Leistungen führen.</span><span class="sxs-lookup"><span data-stu-id="4d980-114">Depending on the amount of data, and how much of that data is filtered out, this could result in poor performance.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

<span data-ttu-id="4d980-115">Standardmäßig protokolliert EF Core eine Warnung, wenn Client Auswertung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="4d980-115">By default, EF Core will log a warning when client evaluation is performed.</span></span> <span data-ttu-id="4d980-116">Finden Sie unter [Protokollierung](../miscellaneous/logging.md) für Weitere Informationen zur Anzeige von Protokollausgaben.</span><span class="sxs-lookup"><span data-stu-id="4d980-116">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="4d980-117">Sie können das Verhalten beim Client Auswertung erfolgt, entweder auslösen oder Unternehmen Sie nichts ändern.</span><span class="sxs-lookup"><span data-stu-id="4d980-117">You can change the behavior when client evaluation occurs to either throw or do nothing.</span></span> <span data-ttu-id="4d980-118">Dies erfolgt, wenn Sie die Optionen für den Kontext - in der Regel in einrichten `DbContext.OnConfiguring`, oder im `Startup.cs` bei Verwendung von ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4d980-118">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
