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
# <a name="client-vs-server-evaluation"></a>Client im Vergleich zu Server-Evaluierung

Entity Framework Core unterstützt Teile der Abfrage ausgewertet wird, auf dem Client und Teile davon per Push an die Datenbank übermittelt. Es liegt im Ermessen der Datenbankanbieter, um zu bestimmen, welche Teile der Abfrage in der Datenbank ausgewertet werden.

> [!TIP]  
> Sie können anzeigen, dass dieser Artikel [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) auf GitHub.

## <a name="client-evaluation"></a>Client-Evaluierung

Im folgenden Beispiel wird eine Hilfsmethode verwendet, um URLs für Blogs zu standardisieren, die aus einer SQL Server-Datenbank zurückgegeben werden. Da SQL Server-Anbieter keine Einblick hat, wie diese Methode implementiert wird, ist es nicht möglich, die in SQL zu übersetzen. Alle anderen Aspekte der Abfrage ausgewertet werden in der Datenbank, übergeben Sie jedoch das zurückgegebene `URL` über diese Methode auf dem Client ausgeführt wird.

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

## <a name="disabling-client-evaluation"></a>Deaktivieren des Client-Bewertung

Auswertung der Client kann sehr nützlich sein, kann in einigen Fällen dies zu schlechten Leistungen führen. Betrachten Sie die folgende Abfrage, wobei die Hilfsmethode jetzt in einem Filter verwendet wird. Da dies in der Datenbank ausgeführt werden kann, werden alle Daten in den Arbeitsspeicher, und klicken Sie dann den Filter gezogen werden auf dem Client angewendet. Abhängig vom Umfang der Daten, und wie viel dieser Daten herausgefiltert wird könnte dies zu schlechten Leistungen führen.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .Where(blog => StandardizeUrl(blog.Url).Contains("dotnet"))
    .ToList();
```

Standardmäßig protokolliert EF Core eine Warnung, wenn Client Auswertung ausgeführt wird. Finden Sie unter [Protokollierung](../miscellaneous/logging.md) für Weitere Informationen zur Anzeige von Protokollausgaben. Sie können das Verhalten beim Client Auswertung erfolgt, entweder auslösen oder Unternehmen Sie nichts ändern. Dies erfolgt, wenn Sie die Optionen für den Kontext - in der Regel in einrichten `DbContext.OnConfiguring`, oder im `Startup.cs` bei Verwendung von ASP.NET Core.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/ClientEval/ThrowOnClientEval/BloggingContext.cs?highlight=5)] -->
``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
