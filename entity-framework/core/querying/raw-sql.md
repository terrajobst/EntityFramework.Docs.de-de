---
title: Unformatierte SQL-Abfragen – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: d8f52edfdf4bd7776ab8d81185c867cbfd7bcf44
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813597"
---
# <a name="raw-sql-queries"></a>Unformatierte SQL-Abfragen

Mit Entity Framework Core können Sie bei der Arbeit mit einer relationalen Datenbank die Struktur unformatierter SQL-Abfragen maximieren. Dies kann nützlich sein, wenn die durchzuführende Abfrage nicht mit LINQ formuliert werden kann oder wenn die Verwendung einer LINQ-Abfrage zu einer ineffizienten SQL-Abfrage führt. Unformatierte SQL-Abfragen können reguläre Entitätstypen oder [schlüssellose Entitätstypen](xref:core/modeling/keyless-entity-types) zurückgeben, die Teil des Modells sind.

> [!TIP]  
> Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/Querying/RawSQL/Sample.cs) finden Sie auf GitHub.

## <a name="basic-raw-sql-queries"></a>Grundlegende unformatierte SQL-Abfragen

Sie können die `FromSqlRaw`-Erweiterungsmethode verwenden, um eine LINQ-Abfrage basierend auf einer unformatierten SQL-Abfrage zu starten.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("SELECT * FROM dbo.Blogs")
    .ToList();
```

Unformatierte SQL-Abfragen können für die Ausführung einer gespeicherten Prozedur verwendet werden.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>Übergeben von Parametern

> [!WARNING]
> **Für unformatierte SQL-Abfragen immer Parametrisierung verwenden**
>
> Wenn Sie von Benutzern bereitgestellte Werte in eine unformatierte SQL-Abfrage einführen, muss darauf geachtet werden, dass Angriffe durch Einschleusung von SQL-Befehlen vermieden werden. Überprüfen Sie nicht nur, ob solche Werte keine ungültigen Zeichen enthalten, sondern verwenden Sie auch immer die Parametrisierung, die die Werte getrennt vom SQL-Text sendet.
>
> Übergeben Sie insbesondere niemals eine verkettete oder interpolierte Zeichenfolge (`$""`) mit nicht validierten und von Benutzern bereitgestellten Werten in `FromSqlRaw` oder `ExecuteSqlRaw`. Dank der `FromSqlInterpolated`- und `ExecuteSqlInterpolated`-Methode kann die Zeichenfolgeninterpolationssyntax so eingesetzt werden, dass sie vor Angriffen durch Einschleusung von SQL-Befehlen schützt.

Im folgenden Beispiel wird ein einzelner Parameter an eine gespeicherte Prozedur übergeben, indem ein Parameterplatzhalter in die SQL-Abfragezeichenfolge eingeschlossen und ein zusätzliches Argument bereitgestellt wird. Obwohl dies `String.Format`-Syntax ähnelt, ist der bereitgestellte Wert in einen `DbParameter` eingeschlossen. Der Name des generierten Parameters wird dort eingefügt, wo der `{0}`-Platzhalter angegeben wurde.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

Als Alternative zu `FromSqlRaw` können sie `FromSqlInterpolated` verwenden, um eine sichere Verwendung der Zeichenfolgeninterpolation zu ermöglichen. Wie im vorherigen Beispiel wird der Wert in einen `DbParameter` konvertiert und ist daher nicht anfällig für die Einschleusung von SQL-Befehlen:

> [!NOTE]
> Vor Version 3.0 waren `FromSqlRaw` und `FromSqlInterpolated` zwei Überladungen namens `FromSql`. Weitere Informationen finden Sie im Abschnitt [Frühere Versionen](#previous-versions).

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlInterpolated($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

Sie können auch einen DbParameter erstellen und diesen als Parameterwert bereitstellen. Da anstelle eines Zeichenfolgeplatzhalters ein regulärer SQL-Parameterplatzhalter verwendet wird, kann `FromSqlRaw` sicher verwendet werden:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

Dadurch können Sie benannte Parameter in der SQL-Abfragezeichenfolge verwenden. Dies ist nützlich, wenn eine gespeicherte Prozedur optionale Parameter aufweist:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>Zusammensetzen mit LINQ

Wenn die SQL-Abfrage in der Datenbank zusammengesetzt werden kann, können Sie die erste unformatierte SQL-Abfrage mit LINQ-Operatoren zusammensetzen. Zusammensetzbare SQL-Abfragen beginnen mit dem Schlüsselwort `SELECT`.

Im folgenden Beispiel wird eine unformatierte SQL-Abfrage verwendet, die eine Auswahl aus einer Tabellenwertfunktion trifft und diese anschließend mit LINQ zur Durchführung einer Filterung und Sortierung zusammensetzt.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

Dadurch wird die folgende SQL-Abfrage erzeugt:

``` sql
SELECT [b].[Id], [b].[Name], [b].[Rating]
        FROM (
            SELECT * FROM dbo.SearchBlogs(@p0)
        ) AS b
        WHERE b."Rating" > 3
        ORDER BY b."Rating" DESC
```

## <a name="change-tracking"></a>Änderungsnachverfolgung

Abfragen, die die `FromSql`-Methode verwenden, befolgen genau dieselben Änderungsnachverfolgungsregeln wie jede andere LINQ-Abfrage in EF Core. Wenn beispielsweise die Abfrage Entitätstypen projiziert, werden die Ergebnisse standardmäßig nachverfolgt.

Im folgenden Beispiel wird eine unformatierte SQL-Abfrage verwendet, die aus einer Tabellenwertfunktion (TVF) auswählt und anschließend die Änderungsnachverfolgung mit dem Aufruf auf `AsNoTracking` deaktiviert:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a>Einschließen zugehöriger Daten

Die `Include`-Methode kann verwendet werden, um verwandte Daten einzuschließen, genauso wie bei jeder anderen LINQ-Abfrage:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

Beachten Sie, dass erforderlich ist, dass Ihre unformatierte SQL-Abfrage zusammensetzbar ist. Sie funktioniert insbesondere bei gespeicherten Prozeduraufrufen nicht. Weitere Informationen finden Sie bei der Zusammensetzbarkeit unter [Einschränkungen](#limitations).

## <a name="limitations"></a>Einschränkungen

Bei der Verwendung unformatierter SQL-Abfragen sind einige wenige Einschränkungen zu beachten:

* Die SQL-Abfrage muss Daten für sämtliche Eigenschaften des Entitätstyps zurückgeben.

* Die Spaltennamen im Resultset müssen mit den Spaltennamen übereinstimmen, denen Eigenschaften zugewiesen sind. Beachten Sie, dass sich dies von EF 6 unterscheidet. Dort wurde die Zuordnung von Eigenschaften/Spalten bei unformatierten SQL-Abfragen ignoriert, und Spaltennamen in einem Resultset mussten mit den Eigenschaftsnamen übereinstimmen.

* Die SQL-Abfrage darf keine zugehörigen Daten enthalten. In vielen Fällen können Sie die Abfrage jedoch mit dem Operator `Include` zusammensetzen, damit zugehörige Daten zurückgegeben werden (siehe [Einschließen zugehöriger Daten](#including-related-data)).

* An diese Methode übergebene `SELECT`-Anweisungen müssen in der Regel zusammensetzbar sein: Wenn EF Core zusätzliche Abfrageoperatoren auf dem Server auswerten muss (z. B. um nach den `FromSql`-Methoden angewandte LINQ-Operatoren zu verschieben), wird die bereitgestellte SQL-Abfrage wie eine Unterabfrage behandelt. Das heißt, dass die übergebene SQL-Abfrage keine Zeichen oder Optionen enthalten sollte, die in einer Unterabfrage ungültig sind, wie z.B.:
  * Ein nachfolgendes Semikolon
  * Auf SQL Server ein nachfolgender Hinweis auf Abfrageebene, z.B. `OPTION (HASH JOIN)`
  * In SQL Server eine `ORDER BY`-Klausel, die nicht durch `OFFSET 0` ODER `TOP 100 PERCENT` in der `SELECT`-Klausel ergänzt wird

* Beachten Sie, dass SQL Server das Zusammensetzen über gespeicherte Prozeduraufrufe nicht zulässt, sodass jeder Versuch, zusätzliche Abfrageoperatoren auf einen solchen Aufruf anzuwenden, zu ungültiger SQL führt. Abfrageoperatoren können nach `AsEnumerable()` für die Clientauswertung eingeführt werden.

## <a name="previous-versions"></a>Frühere Versionen

In EF Core Version 2.2 und früher gab es zwei Überladungen namens `FromSql`, die sich wie die neueren `FromSqlRaw` und `FromSqlInterpolated` verhielten. Dadurch wurde sehr leicht versehentlich die Methode für unformatierte Zeichenfolgen aufgerufen, wenn eigentlich beabsichtigt war, die Methode für interpolierte Zeichenfolgen aufzurufen (und umgekehrt). Dadurch werden Abfragen möglicherweise nicht parametrisiert, obwohl dies der Fall sein sollte.
