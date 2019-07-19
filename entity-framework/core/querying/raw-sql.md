---
title: Unformatierte SQL-Abfragen – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: 91592ea9f7c73f10446993282c1874c852000871
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306548"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="b3fe5-102">Unformatierte SQL-Abfragen</span><span class="sxs-lookup"><span data-stu-id="b3fe5-102">Raw SQL Queries</span></span>

<span data-ttu-id="b3fe5-103">Mit Entity Framework Core können Sie bei der Arbeit mit einer relationalen Datenbank die Struktur unformatierter SQL-Abfragen maximieren.</span><span class="sxs-lookup"><span data-stu-id="b3fe5-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="b3fe5-104">Dies kann nützlich sein, wenn die durchzuführende Abfrage nicht mit LINQ formuliert werden kann oder wenn die Verwendung einer LINQ-Abfrage zu ineffizienten SQL-Abfragen führt.</span><span class="sxs-lookup"><span data-stu-id="b3fe5-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL queries.</span></span> <span data-ttu-id="b3fe5-105">Unformatierte SQL-Abfragen können Entitätstypen oder seit EF Core 2.1 [Abfragetypen](xref:core/modeling/query-types) zurückgeben, die Teil des Modells sind.</span><span class="sxs-lookup"><span data-stu-id="b3fe5-105">Raw SQL queries can return entity types or, starting with EF Core 2.1, [query types](xref:core/modeling/query-types) that are part of your model.</span></span>

> [!TIP]  
> <span data-ttu-id="b3fe5-106">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="b3fe5-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="b3fe5-107">Grundlegende unformatierte SQL-Abfragen</span><span class="sxs-lookup"><span data-stu-id="b3fe5-107">Basic raw SQL queries</span></span>

<span data-ttu-id="b3fe5-108">Sie können die *FromSql*-Erweiterungsmethode verwenden, um eine LINQ-Abfrage basierend auf einer unformatierten SQL-Abfrage zu starten.</span><span class="sxs-lookup"><span data-stu-id="b3fe5-108">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="b3fe5-109">Unformatierte SQL-Abfragen können für die Ausführung einer gespeicherten Prozedur verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="b3fe5-109">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="b3fe5-110">Übergeben von Parametern</span><span class="sxs-lookup"><span data-stu-id="b3fe5-110">Passing parameters</span></span>

<span data-ttu-id="b3fe5-111">Wie bei jeder API, die von SQL akzeptiert wird, ist es wichtig, sämtliche Benutzereingaben zum Schutz vor Angriffen durch die Einschleusung von SQL-Befehlen zu parametrisieren.</span><span class="sxs-lookup"><span data-stu-id="b3fe5-111">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="b3fe5-112">Sie können in der SQL-Abfragezeichenfolge Platzhalter für Parameter einschließen und anschließend Parameterwerte als zusätzliche Argumente bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="b3fe5-112">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="b3fe5-113">Sämtliche von Ihnen bereitgestellte Parameterwerte werden automatisch in einen `DbParameter` konvertiert.</span><span class="sxs-lookup"><span data-stu-id="b3fe5-113">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="b3fe5-114">Im folgenden Beispiel wird ein einzelner Parameter an eine gespeicherte Prozedur übergeben.</span><span class="sxs-lookup"><span data-stu-id="b3fe5-114">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="b3fe5-115">Obwohl dies `String.Format`-Syntax ähnelt, ist der bereitgestellte Wert in einen Parameter eingeschlossen. Der Name des generierten Parameters wird dort eingefügt, wo der Platzhalter `{0}` angegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="b3fe5-115">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="b3fe5-116">Dies ist die gleiche Abfrage, jedoch mit einer Zeichenfolgeninterpolationssyntax, die ab EF Core 2.0 unterstützt wird:</span><span class="sxs-lookup"><span data-stu-id="b3fe5-116">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="b3fe5-117">Sie können auch einen DbParameter erstellen und diesen als Parameterwert bereitstellen:</span><span class="sxs-lookup"><span data-stu-id="b3fe5-117">You can also construct a DbParameter and supply it as a parameter value:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

<span data-ttu-id="b3fe5-118">Dadurch können Sie benannte Parameter in der SQL-Abfragezeichenfolge verwenden. Dies ist nützlich, wenn eine gespeicherte Prozedur optionale Parameter aufweist:</span><span class="sxs-lookup"><span data-stu-id="b3fe5-118">This allows you to use named parameters in the SQL query string, which is useful when a stored procedure has optional parameters:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="b3fe5-119">Zusammensetzen mit LINQ</span><span class="sxs-lookup"><span data-stu-id="b3fe5-119">Composing with LINQ</span></span>

<span data-ttu-id="b3fe5-120">Wenn die SQL-Abfrage in der Datenbank zusammengesetzt werden kann, können Sie die erste unformatierte SQL-Abfrage mit LINQ-Operatoren zusammensetzen.</span><span class="sxs-lookup"><span data-stu-id="b3fe5-120">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="b3fe5-121">Zusammensetzbare SQL-Abfragen beginnen mit dem Schlüsselwort `SELECT`.</span><span class="sxs-lookup"><span data-stu-id="b3fe5-121">SQL queries that can be composed on begin with the `SELECT` keyword.</span></span>

<span data-ttu-id="b3fe5-122">Im folgenden Beispiel wird eine unformatierte SQL-Abfrage verwendet, die eine Auswahl aus einer Tabellenwertfunktion trifft und diese anschließend mit LINQ zur Durchführung einer Filterung und Sortierung zusammensetzt.</span><span class="sxs-lookup"><span data-stu-id="b3fe5-122">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

## <a name="change-tracking"></a><span data-ttu-id="b3fe5-123">Änderungsnachverfolgung</span><span class="sxs-lookup"><span data-stu-id="b3fe5-123">Change Tracking</span></span>

<span data-ttu-id="b3fe5-124">Abfragen, die die Methode `FromSql()` verwenden, befolgen genau dieselben Änderungsnachverfolgungsregeln wie jede andere LINQ-Abfrage in EF Core.</span><span class="sxs-lookup"><span data-stu-id="b3fe5-124">Queries that use the `FromSql()` follow the exact same change tracking rules as any other LINQ query in EF Core.</span></span> <span data-ttu-id="b3fe5-125">Wenn beispielsweise die Abfrage Entitätstypen projiziert, werden die Ergebnisse standardmäßig nachverfolgt.</span><span class="sxs-lookup"><span data-stu-id="b3fe5-125">For example, if the query projects entity types, the results will be tracked by default.</span></span>  

<span data-ttu-id="b3fe5-126">Im folgenden Beispiel wird eine unformatierte SQL-Abfrage verwendet, die aus einer Tabellenwertfunktion (TVF) auswählt und anschließend die Änderungsnachverfolgung mit dem Aufruf auf „.AsNoTracking()“ deaktiviert:</span><span class="sxs-lookup"><span data-stu-id="b3fe5-126">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF), then disables change tracking with the call to .AsNoTracking():</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a><span data-ttu-id="b3fe5-127">Einschließen zugehöriger Daten</span><span class="sxs-lookup"><span data-stu-id="b3fe5-127">Including related data</span></span>

<span data-ttu-id="b3fe5-128">Die `Include()`-Methode kann verwendet werden, um verwandte Daten einzuschließen, genauso wie bei jeder anderen LINQ-Abfrage:</span><span class="sxs-lookup"><span data-stu-id="b3fe5-128">The `Include()` method can be used to include related data, just like with any other LINQ query:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

## <a name="limitations"></a><span data-ttu-id="b3fe5-129">Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="b3fe5-129">Limitations</span></span>

<span data-ttu-id="b3fe5-130">Bei der Verwendung unformatierter SQL-Abfragen sind einige wenige Einschränkungen zu beachten:</span><span class="sxs-lookup"><span data-stu-id="b3fe5-130">There are a few limitations to be aware of when using raw SQL queries:</span></span>

* <span data-ttu-id="b3fe5-131">Die SQL-Abfrage muss Daten für sämtliche Eigenschaften des Entitäts- oder Abfragetyps zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="b3fe5-131">The SQL query must return data for all properties of the entity or query type.</span></span>

* <span data-ttu-id="b3fe5-132">Die Spaltennamen im Resultset müssen mit den Spaltennamen übereinstimmen, denen Eigenschaften zugewiesen sind.</span><span class="sxs-lookup"><span data-stu-id="b3fe5-132">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="b3fe5-133">Beachten Sie, dass sich dies von EF 6 unterscheidet. Dort wurde die Zuordnung von Eigenschaften/Spalten bei unformatierten SQL-Abfragen ignoriert, und Spaltennamen in einem Resultset mussten mit den Eigenschaftsnamen übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="b3fe5-133">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="b3fe5-134">Die SQL-Abfrage darf keine zugehörigen Daten enthalten.</span><span class="sxs-lookup"><span data-stu-id="b3fe5-134">The SQL query cannot contain related data.</span></span> <span data-ttu-id="b3fe5-135">In vielen Fällen können Sie die Abfrage jedoch mit dem Operator `Include` zusammensetzen, damit zugehörige Daten zurückgegeben werden (siehe [Einschließen zugehöriger Daten](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="b3fe5-135">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="b3fe5-136">An diese Methode übergebene `SELECT`-Anweisungen müssen in der Regel zusammensetzbar sein: Wenn EF Core weitere Abfrageoperatoren auf dem Server auswerten muss (z.B. um nach `FromSql` angewandte LINQ-Operatoren zu verschieben), wird die bereitgestellte SQL-Abfrage wie eine Unterabfrage behandelt.</span><span class="sxs-lookup"><span data-stu-id="b3fe5-136">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (for example, to translate LINQ operators applied after `FromSql`), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="b3fe5-137">Das heißt, dass die übergebene SQL-Abfrage keine Zeichen oder Optionen enthalten sollte, die in einer Unterabfrage ungültig sind, wie z.B.:</span><span class="sxs-lookup"><span data-stu-id="b3fe5-137">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="b3fe5-138">ein nachfolgendes Semikolon</span><span class="sxs-lookup"><span data-stu-id="b3fe5-138">a trailing semicolon</span></span>
  * <span data-ttu-id="b3fe5-139">Auf SQL Server ein nachfolgender Hinweis auf Abfrageebene, z.B. `OPTION (HASH JOIN)`</span><span class="sxs-lookup"><span data-stu-id="b3fe5-139">On SQL Server, a trailing query-level hint (for example, `OPTION (HASH JOIN)`)</span></span>
  * <span data-ttu-id="b3fe5-140">In SQL Server eine `ORDER BY`-Klausel, die nicht durch `OFFSET 0` ODER `TOP 100 PERCENT` in der `SELECT`-Klausel ergänzt wird</span><span class="sxs-lookup"><span data-stu-id="b3fe5-140">On SQL Server, an `ORDER BY` clause that is not accompanied of `OFFSET 0` OR `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="b3fe5-141">Andere SQL-Anweisungen als `SELECT` werden automatisch als nicht zusammensetzbar erkannt.</span><span class="sxs-lookup"><span data-stu-id="b3fe5-141">SQL statements other than `SELECT` are recognized automatically as non-composable.</span></span> <span data-ttu-id="b3fe5-142">Die vollständigen Ergebnisse gespeicherter Prozeduren werden daher immer an den Client zurückgegeben, und sämtliche LINQ-Operatoren, die nach `FromSql` angewendet werden, werden speicherintern ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="b3fe5-142">As a consequence, the full results of stored procedures are always returned to the client and any LINQ operators applied after `FromSql` are evaluated in-memory.</span></span>

> [!WARNING]  
> <span data-ttu-id="b3fe5-143">**Für unformatierte SQL-Abfragen immer Parametrisierung verwenden:** Zusätzlich zum Überprüfen der Benutzereingabe sollten Sie die Parametrisierung für sämtliche Werte verwenden, die in unformatierten SQL-Abfragen/-Befehlen enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="b3fe5-143">**Always use parameterization for raw SQL queries:** In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="b3fe5-144">APIs, die eine unformatierte SQL-Zeichenfolge wie `FromSql` und `ExecuteSqlCommand` akzeptieren, lassen zu, dass Werte problemlos als Parameter übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="b3fe5-144">APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="b3fe5-145">Für Überladungen von `FromSql` und `ExecuteSqlCommand`, die FormattableString akzeptieren, kann auch die Syntax zur Zeichenfolgeninterpolation so verwendet werden, dass Angriffe durch Einschleusung von SQL-Befehlen verhindert werden.</span><span class="sxs-lookup"><span data-stu-id="b3fe5-145">Overloads of `FromSql` and `ExecuteSqlCommand` that accept FormattableString also allow using string interpolation syntax in a way that helps protect against SQL injection attacks.</span></span> 
> 
> <span data-ttu-id="b3fe5-146">Wenn Sie Zeichenfolgen verketten oder interpolieren, um einen Teil der Abfragezeichenfolge dynamisch zu erstellen, müssen Sie Eingaben so überprüfen, dass Angriffe durch Einschleusung von SQL-Befehlen nicht möglich sind. Dieselbe Überprüfung ist notwendig, wenn Benutzereingaben an Anweisungen oder gespeicherte Prozeduren übergeben werden, die diese Eingaben als dynamisches SQL ausführen können.</span><span class="sxs-lookup"><span data-stu-id="b3fe5-146">If you are using string concatenation or interpolation to dynamically build any part of the query string, or passing user input to statements or stored procedures that can execute those inputs as dynamic SQL, then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
