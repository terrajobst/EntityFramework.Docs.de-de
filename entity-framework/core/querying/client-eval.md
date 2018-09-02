---
title: Clientauswertung im Vergleich zur Serverauswertung – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
uid: core/querying/client-eval
ms.openlocfilehash: 78f8d9576748a725634665f915def80b5a13820c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997876"
---
# <a name="client-vs-server-evaluation"></a>Clientauswertung im Vergleich zur Serverauswertung

Entity Framework Core unterstützt Teile der Abfrage, die auf dem Client ausgewertet werden, und Teile der Abfrage, die mithilfe von Push in die Datenbank übertragen werden. Der Datenbankanbieter bestimmt, welche Teile der Abfrage in der Datenbank ausgewertet werden.

> [!TIP]  
> Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) finden Sie auf GitHub.

## <a name="client-evaluation"></a>Clientauswertung

Im folgenden Beispiel werden URLs für Blogs, die von einer SQL Server-Datenbank zurückgegeben werden, mit einer Hilfsmethode standardisiert. Da der SQL Server-Anbieter keinen Einblick darin hat, wie diese Methode implementiert wird, kann sie nicht in SQL verschoben werden. Alle anderen Aspekte der Abfrage werden in der Datenbank ausgewertet. Die Rückgabe der `URL` über diese Methode erfolgt jedoch über den Client.

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

## <a name="disabling-client-evaluation"></a>Deaktivieren der Clientauswertung

Die Clientauswertung kann sehr hilfreich sein, in manchen Fällen kann sie jedoch auch zu einer schlechten Leistung führen. Betrachten Sie die folgende Abfrage, in der die Hilfsmethode jetzt in einem Filter verwendet wird. Da dieser Vorgang nicht in der Datenbank durchgeführt werden kann, werden sämtliche Daten mithilfe von Pull in den Speicher übertragen. Anschließend wird der Filter auf den Client angewendet. Abhängig vom Umfang der Daten und davon, wie viele dieser Daten herausgefiltert werden, könnte dies zu einer schlechten Leistung führen.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

Standardmäßig protokolliert EF Core bei der Durchführung einer Auswertung eine Warnung. Weitere Informationen zum Anzeigen von Protokollierungsausgaben finden Sie unter [Protokollierung](../miscellaneous/logging.md). Sie können das Verhalten bei einer Clientauswertung auslösen oder nichts unternehmen. Dies geschieht, wenn die Optionen für Ihren Kontext – normalerweise in `DbContext.OnConfiguring` oder `Startup.cs` – bei Verwendung von ASP.NET Core eingerichtet werden.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
