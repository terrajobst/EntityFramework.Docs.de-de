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
# <a name="client-vs-server-evaluation"></a>Clientauswertung im Vergleich zur Serverauswertung

Entity Framework Core unterstützt Teile der Abfrage, die auf dem Client ausgewertet werden, und Teile der Abfrage, die mithilfe von Push in die Datenbank übertragen werden. Der Datenbankanbieter bestimmt, welche Teile der Abfrage in der Datenbank ausgewertet werden.

> [!TIP]  
> Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) finden Sie auf GitHub.

## <a name="client-evaluation"></a>Clientauswertung

Im folgenden Beispiel werden URLs für Blogs, die von einer SQL Server-Datenbank zurückgegeben werden, mit einer Hilfsmethode standardisiert. Da der SQL Server-Anbieter keinen Einblick darin hat, wie diese Methode implementiert wird, kann sie nicht in SQL verschoben werden. Alle anderen Aspekte der Abfrage werden in der Datenbank ausgewertet. Die Rückgabe der `URL` über diese Methode erfolgt jedoch über den Client.

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

## <a name="client-evaluation-performance-issues"></a>Leistungsprobleme bei der Clientauswertung

Die Clientauswertung kann sehr hilfreich sein, in manchen Fällen kann sie jedoch auch zu einer schlechten Leistung führen. Betrachten Sie die folgende Abfrage, in der die Hilfsmethode jetzt in einem Filter verwendet wird. Da dieser Vorgang nicht in der Datenbank durchgeführt werden kann, werden sämtliche Daten mithilfe von Pull in den Speicher übertragen. Anschließend wird der Filter auf den Client angewendet. Abhängig vom Umfang der Daten und davon, wie viele dieser Daten herausgefiltert werden, könnte dies zu einer schlechten Leistung führen.

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

## <a name="client-evaluation-logging"></a>Protokollieren der Clientauswertung

Standardmäßig protokolliert EF Core bei der Durchführung einer Auswertung eine Warnung. Weitere Informationen zum Anzeigen von Protokollierungsausgaben finden Sie unter [Protokollierung](../miscellaneous/logging.md). 

## <a name="optional-behavior-throw-an-exception-for-client-evaluation"></a>Optionales Verhalten: Auslösen einer Ausnahme für die Clientauswertung

Sie können das Verhalten bei einer Clientauswertung auslösen oder nichts unternehmen. Dies geschieht, wenn die Optionen für Ihren Kontext – normalerweise in `DbContext.OnConfiguring` oder `Startup.cs` – bei Verwendung von ASP.NET Core eingerichtet werden.

<!-- [!code-csharp[Main](samples/core/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
