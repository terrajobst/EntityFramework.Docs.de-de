---
title: Unformatierte SQL-Abfragen – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: 343162596780e6146b57f73a38221701009cd855
ms.sourcegitcommit: 85d17524d8e022f933cde7fc848313f57dfd3eb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/06/2019
ms.locfileid: "55760508"
---
# <a name="raw-sql-queries"></a>Unformatierte SQL-Abfragen

Mit Entity Framework Core können Sie bei der Arbeit mit einer relationalen Datenbank die Struktur unformatierter SQL-Abfragen maximieren. Dies kann nützlich sein, wenn die durchzuführende Abfrage nicht mit LINQ formuliert werden kann oder wenn die Verwendung einer LINQ-Abfrage zu ineffizienten SQL-Abfragen führt. Unformatierte SQL-Abfragen können Entitätstypen oder seit EF Core 2.1 [Abfragetypen](xref:core/modeling/query-types) zurückgeben, die Teil des Modells sind.

> [!TIP]  
> Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) finden Sie auf GitHub.

## <a name="basic-raw-sql-queries"></a>Grundlegende unformatierte SQL-Abfragen

Sie können die *FromSql*-Erweiterungsmethode verwenden, um eine LINQ-Abfrage basierend auf einer unformatierten SQL-Abfrage zu starten.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

Unformatierte SQL-Abfragen können für die Ausführung einer gespeicherten Prozedur verwendet werden.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>Übergeben von Parametern

Wie bei jeder API, die von SQL akzeptiert wird, ist es wichtig, sämtliche Benutzereingaben zum Schutz vor Angriffen durch die Einschleusung von SQL-Befehlen zu parametrisieren. Sie können in der SQL-Abfragezeichenfolge Platzhalter für Parameter einschließen und anschließend Parameterwerte als zusätzliche Argumente bereitstellen. Sämtliche von Ihnen bereitgestellte Parameterwerte werden automatisch in einen `DbParameter` konvertiert.

Im folgenden Beispiel wird ein einzelner Parameter an eine gespeicherte Prozedur übergeben. Obwohl dies `String.Format`-Syntax ähnelt, ist der bereitgestellte Wert in einen Parameter eingeschlossen. Der Name des generierten Parameters wird dort eingefügt, wo der Platzhalter `{0}` angegeben wurde.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

Dies ist die gleiche Abfrage, jedoch mit einer Zeichenfolgeninterpolationssyntax, die ab EF Core 2.0 unterstützt wird:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

Sie können auch einen DbParameter erstellen und diesen als Parameterwert bereitstellen. Dadurch können Sie benannte Parameter in der SQL-Abfragezeichenfolge verwenden.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>Zusammensetzen mit LINQ

Wenn die SQL-Abfrage in der Datenbank zusammengesetzt werden kann, können Sie die erste unformatierte SQL-Abfrage mit LINQ-Operatoren zusammensetzen. Zusammensetzbare SQL-Abfragen beginnen mit dem Schlüsselwort `SELECT`.

Im folgenden Beispiel wird eine unformatierte SQL-Abfrage verwendet, die eine Auswahl aus einer Tabellenwertfunktion trifft und diese anschließend mit LINQ zur Durchführung einer Filterung und Sortierung zusammensetzt.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

## <a name="change-tracking"></a>Änderungsnachverfolgung

Abfragen, die die Methode `FromSql()` verwenden, befolgen genau dieselben Änderungsnachverfolgungsregeln wie jede andere LINQ-Abfrage in EF Core. Wenn beispielsweise die Abfrage Entitätstypen projiziert, werden die Ergebnisse standardmäßig nachverfolgt.  

Im folgenden Beispiel wird eine unformatierte SQL-Abfrage verwendet, die aus einer Tabellenwertfunktion (TVF) auswählt und anschließend die Änderungsnachverfolgung mit dem Aufruf auf „.AsNoTracking()“ deaktiviert:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a>Einschließen zugehöriger Daten

Die `Include()`-Methode kann verwendet werden, um verwandte Daten einzuschließen, genauso wie bei jeder anderen LINQ-Abfrage:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

## <a name="limitations"></a>Einschränkungen

Bei der Verwendung unformatierter SQL-Abfragen sind einige wenige Einschränkungen zu beachten:

* Die SQL-Abfrage muss Daten für sämtliche Eigenschaften des Entitäts- oder Abfragetyps zurückgeben.

* Die Spaltennamen im Resultset müssen mit den Spaltennamen übereinstimmen, denen Eigenschaften zugewiesen sind. Beachten Sie, dass sich dies von EF 6 unterscheidet. Dort wurde die Zuordnung von Eigenschaften/Spalten bei unformatierten SQL-Abfragen ignoriert, und Spaltennamen in einem Resultset mussten mit den Eigenschaftsnamen übereinstimmen.

* Die SQL-Abfrage darf keine zugehörigen Daten enthalten. In vielen Fällen können Sie die Abfrage jedoch mit dem Operator `Include` zusammensetzen, damit zugehörige Daten zurückgegeben werden (siehe [Einschließen zugehöriger Daten](#including-related-data)).

* An diese Methode übergebene `SELECT`-Anweisungen müssen in der Regel zusammensetzbar sein: Wenn EF Core weitere Abfrageoperatoren auf dem Server auswerten muss (z.B. um nach `FromSql` angewandte LINQ-Operatoren zu verschieben), wird die bereitgestellte SQL-Abfrage wie eine Unterabfrage behandelt. Das heißt, dass die übergebene SQL-Abfrage keine Zeichen oder Optionen enthalten sollte, die in einer Unterabfrage ungültig sind, wie z.B.:
  * ein nachfolgendes Semikolon
  * Auf SQL Server ein nachfolgender Hinweis auf Abfrageebene, z.B. `OPTION (HASH JOIN)`
  * Auf SQL Server eine `ORDER BY`-Klausel, die nicht durch `TOP 100 PERCENT` in der `SELECT`-Klausel ergänzt wird

* Andere SQL-Anweisungen als `SELECT` werden automatisch als nicht zusammensetzbar erkannt. Die vollständigen Ergebnisse gespeicherter Prozeduren werden daher immer an den Client zurückgegeben, und sämtliche LINQ-Operatoren, die nach `FromSql` angewendet werden, werden speicherintern ausgewertet.

> [!WARNING]  
> **Für unformatierte SQL-Abfragen immer Parametrisierung verwenden:** APIs, die eine unformatierte SQL-Zeichenfolge wie `FromSql` und `ExecuteSqlCommand` akzeptieren, lassen zu, dass Werte problemlos als Parameter übergeben werden. Zusätzlich zum Überprüfen der Benutzereingabe sollten Sie die Parametrisierung für sämtliche Werte verwenden, die in unformatierten SQL-Abfragen/-Befehlen enthalten sind. Wenn Sie mit der Zeichenfolgenverkettung auf dynamische Weise Teile der Abfragezeichenfolge erstellen, müssen Sie zum Schutz vor Angriffen durch die Einschleusung von SQL-Befehlen jede Eingabe überprüfen.
