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
# <a name="raw-sql-queries"></a><span data-ttu-id="770e4-102">Unformatierte SQL-Abfragen</span><span class="sxs-lookup"><span data-stu-id="770e4-102">Raw SQL Queries</span></span>

<span data-ttu-id="770e4-103">Mit Entity Framework Core können Sie bei der Arbeit mit einer relationalen Datenbank die Struktur unformatierter SQL-Abfragen maximieren.</span><span class="sxs-lookup"><span data-stu-id="770e4-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="770e4-104">Dies kann nützlich sein, wenn die durchzuführende Abfrage nicht mit LINQ formuliert werden kann oder wenn die Verwendung einer LINQ-Abfrage zu einer ineffizienten SQL-Abfrage führt.</span><span class="sxs-lookup"><span data-stu-id="770e4-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in an inefficient SQL query.</span></span> <span data-ttu-id="770e4-105">Unformatierte SQL-Abfragen können reguläre Entitätstypen oder [schlüssellose Entitätstypen](xref:core/modeling/keyless-entity-types) zurückgeben, die Teil des Modells sind.</span><span class="sxs-lookup"><span data-stu-id="770e4-105">Raw SQL queries can return regular entity types or [keyless entity types](xref:core/modeling/keyless-entity-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="770e4-106">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/Querying/RawSQL/Sample.cs) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="770e4-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/Querying/RawSQL/Sample.cs) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="770e4-107">Grundlegende unformatierte SQL-Abfragen</span><span class="sxs-lookup"><span data-stu-id="770e4-107">Basic raw SQL queries</span></span>

<span data-ttu-id="770e4-108">Sie können die `FromSqlRaw`-Erweiterungsmethode verwenden, um eine LINQ-Abfrage basierend auf einer unformatierten SQL-Abfrage zu starten.</span><span class="sxs-lookup"><span data-stu-id="770e4-108">You can use the `FromSqlRaw` extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="770e4-109">Unformatierte SQL-Abfragen können für die Ausführung einer gespeicherten Prozedur verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="770e4-109">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="770e4-110">Übergeben von Parametern</span><span class="sxs-lookup"><span data-stu-id="770e4-110">Passing parameters</span></span>

> [!WARNING]
> <span data-ttu-id="770e4-111">**Für unformatierte SQL-Abfragen immer Parametrisierung verwenden**</span><span class="sxs-lookup"><span data-stu-id="770e4-111">**Always use parameterization for raw SQL queries**</span></span>
>
> <span data-ttu-id="770e4-112">Wenn Sie von Benutzern bereitgestellte Werte in eine unformatierte SQL-Abfrage einführen, muss darauf geachtet werden, dass Angriffe durch Einschleusung von SQL-Befehlen vermieden werden.</span><span class="sxs-lookup"><span data-stu-id="770e4-112">When introducing any user-provided values into a raw SQL query, care must be taken to avoid SQL injection attacks.</span></span> <span data-ttu-id="770e4-113">Überprüfen Sie nicht nur, ob solche Werte keine ungültigen Zeichen enthalten, sondern verwenden Sie auch immer die Parametrisierung, die die Werte getrennt vom SQL-Text sendet.</span><span class="sxs-lookup"><span data-stu-id="770e4-113">In addition to validating that such values don't contain invalid characters, always use parameterization which sends the values separate from the SQL text.</span></span>
>
> <span data-ttu-id="770e4-114">Übergeben Sie insbesondere niemals eine verkettete oder interpolierte Zeichenfolge (`$""`) mit nicht validierten und von Benutzern bereitgestellten Werten in `FromSqlRaw` oder `ExecuteSqlRaw`.</span><span class="sxs-lookup"><span data-stu-id="770e4-114">In particular, never pass a concatenated or interpolated string (`$""`) with unvalidated user-provided values into `FromSqlRaw` or `ExecuteSqlRaw`.</span></span> <span data-ttu-id="770e4-115">Dank der `FromSqlInterpolated`- und `ExecuteSqlInterpolated`-Methode kann die Zeichenfolgeninterpolationssyntax so eingesetzt werden, dass sie vor Angriffen durch Einschleusung von SQL-Befehlen schützt.</span><span class="sxs-lookup"><span data-stu-id="770e4-115">The `FromSqlInterpolated` and `ExecuteSqlInterpolated` methods allow using string interpolation syntax in a way that protects against SQL injection attacks.</span></span>

<span data-ttu-id="770e4-116">Im folgenden Beispiel wird ein einzelner Parameter an eine gespeicherte Prozedur übergeben, indem ein Parameterplatzhalter in die SQL-Abfragezeichenfolge eingeschlossen und ein zusätzliches Argument bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="770e4-116">The following example passes a single parameter to a stored procedure by including a parameter placeholder in the SQL query string and providing an additional argument.</span></span> <span data-ttu-id="770e4-117">Obwohl dies `String.Format`-Syntax ähnelt, ist der bereitgestellte Wert in einen `DbParameter` eingeschlossen. Der Name des generierten Parameters wird dort eingefügt, wo der `{0}`-Platzhalter angegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="770e4-117">While this may look like `String.Format` syntax, the supplied value is wrapped in a `DbParameter` and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="770e4-118">Als Alternative zu `FromSqlRaw` können sie `FromSqlInterpolated` verwenden, um eine sichere Verwendung der Zeichenfolgeninterpolation zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="770e4-118">As an alternative to `FromSqlRaw`, you can use `FromSqlInterpolated` which allows the safe use of string interpolation.</span></span> <span data-ttu-id="770e4-119">Wie im vorherigen Beispiel wird der Wert in einen `DbParameter` konvertiert und ist daher nicht anfällig für die Einschleusung von SQL-Befehlen:</span><span class="sxs-lookup"><span data-stu-id="770e4-119">As with the previous example, the value is converted to a `DbParameter` and is therefore not vulnerable to SQL injection:</span></span>

> [!NOTE]
> <span data-ttu-id="770e4-120">Vor Version 3.0 waren `FromSqlRaw` und `FromSqlInterpolated` zwei Überladungen namens `FromSql`.</span><span class="sxs-lookup"><span data-stu-id="770e4-120">Prior to version 3.0, `FromSqlRaw` and `FromSqlInterpolated` were two overloads named `FromSql`.</span></span> <span data-ttu-id="770e4-121">Weitere Informationen finden Sie im Abschnitt [Frühere Versionen](#previous-versions).</span><span class="sxs-lookup"><span data-stu-id="770e4-121">See the [previous versions section](#previous-versions) for more details.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlInterpolated($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="770e4-122">Sie können auch einen DbParameter erstellen und diesen als Parameterwert bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="770e4-122">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="770e4-123">Da anstelle eines Zeichenfolgeplatzhalters ein regulärer SQL-Parameterplatzhalter verwendet wird, kann `FromSqlRaw` sicher verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="770e4-123">Since a regular SQL parameter placeholder is used, rather than a string placeholder, `FromSqlRaw` can be safely used:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

<span data-ttu-id="770e4-124">Dadurch können Sie benannte Parameter in der SQL-Abfragezeichenfolge verwenden. Dies ist nützlich, wenn eine gespeicherte Prozedur optionale Parameter aufweist:</span><span class="sxs-lookup"><span data-stu-id="770e4-124">This allows you to use named parameters in the SQL query string, which is useful when a stored procedure has optional parameters:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="770e4-125">Zusammensetzen mit LINQ</span><span class="sxs-lookup"><span data-stu-id="770e4-125">Composing with LINQ</span></span>

<span data-ttu-id="770e4-126">Wenn die SQL-Abfrage in der Datenbank zusammengesetzt werden kann, können Sie die erste unformatierte SQL-Abfrage mit LINQ-Operatoren zusammensetzen.</span><span class="sxs-lookup"><span data-stu-id="770e4-126">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="770e4-127">Zusammensetzbare SQL-Abfragen beginnen mit dem Schlüsselwort `SELECT`.</span><span class="sxs-lookup"><span data-stu-id="770e4-127">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span>

<span data-ttu-id="770e4-128">Im folgenden Beispiel wird eine unformatierte SQL-Abfrage verwendet, die eine Auswahl aus einer Tabellenwertfunktion trifft und diese anschließend mit LINQ zur Durchführung einer Filterung und Sortierung zusammensetzt.</span><span class="sxs-lookup"><span data-stu-id="770e4-128">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

<span data-ttu-id="770e4-129">Dadurch wird die folgende SQL-Abfrage erzeugt:</span><span class="sxs-lookup"><span data-stu-id="770e4-129">This will produce the following SQL query:</span></span>

``` sql
SELECT [b].[Id], [b].[Name], [b].[Rating]
        FROM (
            SELECT * FROM dbo.SearchBlogs(@p0)
        ) AS b
        WHERE b."Rating" > 3
        ORDER BY b."Rating" DESC
```

## <a name="change-tracking"></a><span data-ttu-id="770e4-130">Änderungsnachverfolgung</span><span class="sxs-lookup"><span data-stu-id="770e4-130">Change Tracking</span></span>

<span data-ttu-id="770e4-131">Abfragen, die die `FromSql`-Methode verwenden, befolgen genau dieselben Änderungsnachverfolgungsregeln wie jede andere LINQ-Abfrage in EF Core.</span><span class="sxs-lookup"><span data-stu-id="770e4-131">Queries that use the `FromSql` methods follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="770e4-132">Wenn beispielsweise die Abfrage Entitätstypen projiziert, werden die Ergebnisse standardmäßig nachverfolgt.</span><span class="sxs-lookup"><span data-stu-id="770e4-132">For example, if the query projects entity types, the results will be tracked by default.</span></span>

<span data-ttu-id="770e4-133">Im folgenden Beispiel wird eine unformatierte SQL-Abfrage verwendet, die aus einer Tabellenwertfunktion (TVF) auswählt und anschließend die Änderungsnachverfolgung mit dem Aufruf auf `AsNoTracking` deaktiviert:</span><span class="sxs-lookup"><span data-stu-id="770e4-133">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with the call to `AsNoTracking`:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a><span data-ttu-id="770e4-134">Einschließen zugehöriger Daten</span><span class="sxs-lookup"><span data-stu-id="770e4-134">Including related data</span></span>

<span data-ttu-id="770e4-135">Die `Include`-Methode kann verwendet werden, um verwandte Daten einzuschließen, genauso wie bei jeder anderen LINQ-Abfrage:</span><span class="sxs-lookup"><span data-stu-id="770e4-135">The `Include` method can be used to include related data, just like with any other LINQ query:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

<span data-ttu-id="770e4-136">Beachten Sie, dass erforderlich ist, dass Ihre unformatierte SQL-Abfrage zusammensetzbar ist. Sie funktioniert insbesondere bei gespeicherten Prozeduraufrufen nicht.</span><span class="sxs-lookup"><span data-stu-id="770e4-136">Note that this requires your raw SQL query to be composable; it will notably not work with stored procedure calls.</span></span> <span data-ttu-id="770e4-137">Weitere Informationen finden Sie bei der Zusammensetzbarkeit unter [Einschränkungen](#limitations).</span><span class="sxs-lookup"><span data-stu-id="770e4-137">See notes on composability under [Limitations](#limitations)).</span></span>

## <a name="limitations"></a><span data-ttu-id="770e4-138">Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="770e4-138">Limitations</span></span>

<span data-ttu-id="770e4-139">Bei der Verwendung unformatierter SQL-Abfragen sind einige wenige Einschränkungen zu beachten:</span><span class="sxs-lookup"><span data-stu-id="770e4-139">There are a few limitations to be aware of when using raw SQL queries:</span></span>

* <span data-ttu-id="770e4-140">Die SQL-Abfrage muss Daten für sämtliche Eigenschaften des Entitätstyps zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="770e4-140">The SQL query must return data for all properties of the entity type.</span></span>

* <span data-ttu-id="770e4-141">Die Spaltennamen im Resultset müssen mit den Spaltennamen übereinstimmen, denen Eigenschaften zugewiesen sind.</span><span class="sxs-lookup"><span data-stu-id="770e4-141">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="770e4-142">Beachten Sie, dass sich dies von EF 6 unterscheidet. Dort wurde die Zuordnung von Eigenschaften/Spalten bei unformatierten SQL-Abfragen ignoriert, und Spaltennamen in einem Resultset mussten mit den Eigenschaftsnamen übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="770e4-142">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="770e4-143">Die SQL-Abfrage darf keine zugehörigen Daten enthalten.</span><span class="sxs-lookup"><span data-stu-id="770e4-143">The SQL query cannot contain related data.</span></span> <span data-ttu-id="770e4-144">In vielen Fällen können Sie die Abfrage jedoch mit dem Operator `Include` zusammensetzen, damit zugehörige Daten zurückgegeben werden (siehe [Einschließen zugehöriger Daten](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="770e4-144">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="770e4-145">An diese Methode übergebene `SELECT`-Anweisungen müssen in der Regel zusammensetzbar sein: Wenn EF Core zusätzliche Abfrageoperatoren auf dem Server auswerten muss (z. B. um nach den `FromSql`-Methoden angewandte LINQ-Operatoren zu verschieben), wird die bereitgestellte SQL-Abfrage wie eine Unterabfrage behandelt.</span><span class="sxs-lookup"><span data-stu-id="770e4-145">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (for example, to translate LINQ operators applied after `FromSql` methods), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="770e4-146">Das heißt, dass die übergebene SQL-Abfrage keine Zeichen oder Optionen enthalten sollte, die in einer Unterabfrage ungültig sind, wie z.B.:</span><span class="sxs-lookup"><span data-stu-id="770e4-146">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="770e4-147">Ein nachfolgendes Semikolon</span><span class="sxs-lookup"><span data-stu-id="770e4-147">A trailing semicolon</span></span>
  * <span data-ttu-id="770e4-148">Auf SQL Server ein nachfolgender Hinweis auf Abfrageebene, z.B. `OPTION (HASH JOIN)`</span><span class="sxs-lookup"><span data-stu-id="770e4-148">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
  * <span data-ttu-id="770e4-149">In SQL Server eine `ORDER BY`-Klausel, die nicht durch `OFFSET 0` ODER `TOP 100 PERCENT` in der `SELECT`-Klausel ergänzt wird</span><span class="sxs-lookup"><span data-stu-id="770e4-149">On SQL Server, an `ORDER BY` clause that is not accompanied of `OFFSET 0` OR `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="770e4-150">Beachten Sie, dass SQL Server das Zusammensetzen über gespeicherte Prozeduraufrufe nicht zulässt, sodass jeder Versuch, zusätzliche Abfrageoperatoren auf einen solchen Aufruf anzuwenden, zu ungültiger SQL führt.</span><span class="sxs-lookup"><span data-stu-id="770e4-150">Note that SQL Server does not allow composing over stored procedure calls, so any attempt to apply additional query operators to such a call will result in invalid SQL.</span></span> <span data-ttu-id="770e4-151">Abfrageoperatoren können nach `AsEnumerable()` für die Clientauswertung eingeführt werden.</span><span class="sxs-lookup"><span data-stu-id="770e4-151">Query operators may be introduced after `AsEnumerable()` for client evaluation.</span></span>

## <a name="previous-versions"></a><span data-ttu-id="770e4-152">Frühere Versionen</span><span class="sxs-lookup"><span data-stu-id="770e4-152">Previous versions</span></span>

<span data-ttu-id="770e4-153">In EF Core Version 2.2 und früher gab es zwei Überladungen namens `FromSql`, die sich wie die neueren `FromSqlRaw` und `FromSqlInterpolated` verhielten.</span><span class="sxs-lookup"><span data-stu-id="770e4-153">EF Core version 2.2 and earlier had two overloads named `FromSql` which behaved in the same way as the newer `FromSqlRaw` and `FromSqlInterpolated`.</span></span> <span data-ttu-id="770e4-154">Dadurch wurde sehr leicht versehentlich die Methode für unformatierte Zeichenfolgen aufgerufen, wenn eigentlich beabsichtigt war, die Methode für interpolierte Zeichenfolgen aufzurufen (und umgekehrt).</span><span class="sxs-lookup"><span data-stu-id="770e4-154">This made it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span> <span data-ttu-id="770e4-155">Dadurch werden Abfragen möglicherweise nicht parametrisiert, obwohl dies der Fall sein sollte.</span><span class="sxs-lookup"><span data-stu-id="770e4-155">This could result in queries not being parameterized when they should have been.</span></span>
