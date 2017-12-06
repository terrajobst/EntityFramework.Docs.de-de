---
title: RAW-SQL-Abfragen - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
ms.technology: entity-framework-core
uid: core/querying/raw-sql
ms.openlocfilehash: ddf3a841800684688d50dcf9323f4d83c851222f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="raw-sql-queries"></a>RAW-SQL-Abfragen

Entity Framework Core können Sie SQL-Abfragen, die unformatierten Dropdown, bei der Arbeit mit einer relationalen Datenbank. Dies kann nützlich sein, wenn die Abfrage, die Sie ausführen möchten mit LINQ ausgedrückt werden kann, oder wenn eine LINQ-Abfrage mit keine ineffizienten SQL, die an die Datenbank dazu führte, ist.

> [!TIP]  
> Sie können anzeigen, dass dieser Artikel [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) auf GitHub.

## <a name="limitations"></a>Einschränkungen

Es gibt einige Einschränkungen bei Verwendung von SQL-Abfragen, die unformatierte geachtet werden:
* SQL-Abfragen können nur auf Entitätstypen zurückgeben, die Teil des Modells verwendet werden. Es ist eine Erweiterung auf unseren nachholbedarf zu [aktivieren, die Ad-hoc-Typen von unformatierten SQL-Abfragen zurückgeben](https://github.com/aspnet/EntityFramework/issues/1862).

* Die SQL-Abfrage muss es sich um Daten für alle Eigenschaften des Entitätstyps zurückgeben.

* Die Spaltennamen im Resultset müssen die Spaltennamen entsprechen, denen Eigenschaften zugeordnet werden. Beachten Sie, dass dieser vom EF6 unterscheidet, in denen Eigenschaft/spaltenzuordnung für unformatierte SQL-Abfragen wurde ignoriert, und Resultsetspalte mussten Namen der Eigenschaft entsprechen.

* Die SQL-Abfrage kann nicht verknüpfte Daten enthalten. Allerdings in vielen Fällen machen Sie auf die Abfrage mithilfe der `Include` Operator, um verwandte Daten zurückzugeben (finden Sie unter [einschließlich der zugehörige Daten](#including-related-data)).

## <a name="basic-raw-sql-queries"></a>Grundlegende unformatierten SQL-Abfragen

Sie können die *FromSql* Erweiterungsmethode, um eine LINQ-Abfrage basierend auf einer unformatierten SQL-Abfrage zu beginnen.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

RAW-SQL-Abfragen können zum Ausführen einer gespeicherten Prozedur verwendet werden.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>Übergeben von Parametern

Wie bei jeder API, die SQL akzeptiert, ist es wichtig, die zum Schutz vor SQL Injection-Angriff Eingabe durch den Benutzer zu parametrisieren. Sie können Platzhalter für Parameter in der SQL-Abfragezeichenfolge enthalten, und geben Sie dann Parameterwerte als zusätzliche Argumente. Alle Parameterwerte, die Sie angeben, werden automatisch konvertiert werden, um eine `DbParameter`.

Das folgende Beispiel übergibt einen einzelnen Parameter an eine gespeicherte Prozedur. Während dies aussehen könnte z. B. `String.Format` -Syntax wird der angegebene Wert in umschlossen Parameter und den generierten Parameternamen eingefügt Where der `{0}` Platzhalter angegeben wurde.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

Dies ist die gleiche Abfrage jedoch mit einer Zeichenfolge Interpolation Syntax in EF Core 2.0 und höher unterstützt wird:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

Sie können auch ein DbParameter-Objekt zu erstellen und als Parameterwert angeben. Dadurch können Sie benannte Parameter in der SQL-Abfragezeichenfolge verwendet werden.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>Verfassen von LINQ

Wenn die SQL-Abfrage auf die Datenbank geschrieben werden kann, können Sie zusätzlich zu den anfänglichen raw SQL-Abfrage, die mithilfe von LINQ-Operatoren zusammenstellen. SQL-Abfragen, die zusammengesetzt werden können, auf die mit dem `SELECT` Schlüsselwort.

Im folgenden Beispiel wird eine unformatierte SQL-Abfrage, die aus einem Tabellenwertparameter-Funktion (TVF) Wählt aus, und klicken Sie dann verfasst darauf verwenden LINQ zum Ausführen von Filtern und sortieren.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a>Einschließlich der zugehörige Daten

Verfassen von LINQ-Operatoren kann verwendet werden, um verwandte Daten in der Abfrage enthalten.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> **Verwenden Sie immer die Parametrisierung für unformatierte SQL-Abfragen:** APIs, die eine unformatierte SQL akzeptieren eine Zeichenfolge wie z. B. `FromSql` und `ExecuteSqlCommand` ermöglicht, dass Werte einfach als Parameter übergeben werden. Zusätzlich zum Validieren von Benutzereingaben, verwenden Sie immer Parametrisierung für eine beliebige Werte, die in einem unformatierten SQL-Abfrage/Befehl verwendet. Wenn Sie einen beliebigen Teil der Abfragezeichenfolge dynamisch zu erstellen, dann sind Sie verantwortlich für das Validieren von Eingaben zum Schutz vor SQL Injection-Angriffen Verketten von Zeichenfolgen verwenden.
