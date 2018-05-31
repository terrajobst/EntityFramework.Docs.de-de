---
title: Unformatierte SQL-Abfragen – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
ms.technology: entity-framework-core
uid: core/querying/raw-sql
ms.openlocfilehash: 29b7e20e875bf791a88a92636c1df4bc4e31656b
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2018
ms.locfileid: "34163212"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="21c2b-102">Unformatierte SQL-Abfragen</span><span class="sxs-lookup"><span data-stu-id="21c2b-102">Raw SQL Queries</span></span>

<span data-ttu-id="21c2b-103">Mit Entity Framework Core können Sie bei der Arbeit mit einer relationalen Datenbank die Struktur unformatierter SQL-Abfragen maximieren.</span><span class="sxs-lookup"><span data-stu-id="21c2b-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="21c2b-104">Dies kann nützlich sein, wenn die durchzuführende Abfrage nicht mit LINQ formuliert werden kann oder wenn die Verwendung einer LINQ-Abfrage dazu führt, dass eine ineffiziente SQL-Abfrage an die Datenbank gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="21c2b-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL being sent to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="21c2b-105">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="21c2b-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="limitations"></a><span data-ttu-id="21c2b-106">Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="21c2b-106">Limitations</span></span>

<span data-ttu-id="21c2b-107">Bei der Verwendung unformatierter SQL-Abfragen sind einige wenige Einschränkungen zu beachten:</span><span class="sxs-lookup"><span data-stu-id="21c2b-107">There are a few limitations to be aware of when using raw SQL queries:</span></span>
* <span data-ttu-id="21c2b-108">SQL-Abfragen können nur für die Rückgabe von Entitätstypen verwendet werden, die Teil Ihres Modells sind.</span><span class="sxs-lookup"><span data-stu-id="21c2b-108">SQL queries can only be used to return entity types that are part of your model.</span></span> <span data-ttu-id="21c2b-109">In unserem Backlog gibt es eine Erweiterung zum [Aktivieren der Rückgabe von Ad-hoc-Typen aus unformatierten SQL-Abfragen](https://github.com/aspnet/EntityFramework/issues/1862).</span><span class="sxs-lookup"><span data-stu-id="21c2b-109">There is an enhancement on our backlog to [enable returning ad-hoc types from raw SQL queries](https://github.com/aspnet/EntityFramework/issues/1862).</span></span>

* <span data-ttu-id="21c2b-110">Die SQL-Abfrage muss Daten für sämtliche Eigenschaften des Entitäts- oder Abfragetyps zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="21c2b-110">The SQL query must return data for all properties of the entity or query type.</span></span>

* <span data-ttu-id="21c2b-111">Die Spaltennamen im Resultset müssen mit den Spaltennamen übereinstimmen, denen Eigenschaften zugewiesen sind.</span><span class="sxs-lookup"><span data-stu-id="21c2b-111">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="21c2b-112">Beachten Sie, dass sich dies von EF 6 unterscheidet. Dort wurde die Zuordnung von Eigenschaften/Spalten bei unformatierten SQL-Abfragen ignoriert, und Spaltennamen in einem Resultset mussten mit den Eigenschaftsnamen übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="21c2b-112">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="21c2b-113">Die SQL-Abfrage darf keine zugehörigen Daten enthalten.</span><span class="sxs-lookup"><span data-stu-id="21c2b-113">The SQL query cannot contain related data.</span></span> <span data-ttu-id="21c2b-114">In vielen Fällen können Sie die Abfrage jedoch mit dem Operator `Include` zusammensetzen, damit zugehörige Daten zurückgegeben werden (siehe [Einschließen zugehöriger Daten](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="21c2b-114">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

* <span data-ttu-id="21c2b-115">An diese Methode übergebene `SELECT`-Anweisungen sollten im Allgemeinen zusammensetzbar sein: Wenn EF Core weitere Abfrageoperatoren auf dem Server auswerten muss (z.B. nach `FromSql` angewendete LINQ-Operatoren verschieben muss), wird die bereitgestellte SQL-Abfrage wie eine Unterabfrage behandelt.</span><span class="sxs-lookup"><span data-stu-id="21c2b-115">`SELECT` statements passed to this method should generally be composable: If EF Core needs to evaluate additional query operators on the server (e.g. to translate LINQ operators applied after `FromSql`), the supplied SQL will be treated as a subquery.</span></span> <span data-ttu-id="21c2b-116">Das heißt, dass die übergebene SQL-Abfrage keine Zeichen oder Optionen enthalten sollte, die in einer Unterabfrage ungültig sind, wie z.B.:</span><span class="sxs-lookup"><span data-stu-id="21c2b-116">This means that the SQL passed should not contain any characters or options that are not valid on a subquery, such as:</span></span>
  * <span data-ttu-id="21c2b-117">ein nachfolgendes Semikolon</span><span class="sxs-lookup"><span data-stu-id="21c2b-117">a trailing semicolon</span></span>
  * <span data-ttu-id="21c2b-118">Auf SQL Server ein nachfolgender Hinweis auf Abfrageebene-Hinweis, z.B. `OPTION (HASH JOIN)`</span><span class="sxs-lookup"><span data-stu-id="21c2b-118">On SQL Server, a trailing query-level hint, e.g. `OPTION (HASH JOIN)`</span></span>
  * <span data-ttu-id="21c2b-119">Auf SQL Server eine `ORDER BY`-Klausel, die nicht durch `TOP 100 PERCENT` in der `SELECT`-Klausel ergänzt wird</span><span class="sxs-lookup"><span data-stu-id="21c2b-119">On SQL Server, an `ORDER BY` clause that is not accompanied of `TOP 100 PERCENT` in the `SELECT` clause</span></span>

* <span data-ttu-id="21c2b-120">Andere SQL-Anweisungen als `SELECT` werden automatisch als nicht zusammensetzbar erkannt.</span><span class="sxs-lookup"><span data-stu-id="21c2b-120">SQL statements other than `SELECT` are recognized automatically as non-composable.</span></span> <span data-ttu-id="21c2b-121">Die vollständigen Ergebnisse gespeicherter Prozeduren werden daher immer an den Client zurückgegeben, und sämtliche LINQ-Operatoren, die nach `FromSql` angewendet werden, werden speicherintern ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="21c2b-121">As a consequence, the full results of stored procedures are always returned to the client and any LINQ operators applied after `FromSql` are evaluated in-memory.</span></span> 

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="21c2b-122">Grundlegende unformatierte SQL-Abfragen</span><span class="sxs-lookup"><span data-stu-id="21c2b-122">Basic raw SQL queries</span></span>

<span data-ttu-id="21c2b-123">Sie können die *FromSql*-Erweiterungsmethode verwenden, um eine LINQ-Abfrage basierend auf einer unformatierten SQL-Abfrage zu starten.</span><span class="sxs-lookup"><span data-stu-id="21c2b-123">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="21c2b-124">Unformatierte SQL-Abfragen können für die Ausführung einer gespeicherten Prozedur verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="21c2b-124">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="21c2b-125">Übergeben von Parametern</span><span class="sxs-lookup"><span data-stu-id="21c2b-125">Passing parameters</span></span>

<span data-ttu-id="21c2b-126">Wie bei jeder API, die von SQL akzeptiert wird, ist es wichtig, sämtliche Benutzereingaben zum Schutz vor Angriffen durch die Einschleusung von SQL-Befehlen zu parametrisieren.</span><span class="sxs-lookup"><span data-stu-id="21c2b-126">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="21c2b-127">Sie können in der SQL-Abfragezeichenfolge Platzhalter für Parameter einschließen und anschließend Parameterwerte als zusätzliche Argumente bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="21c2b-127">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="21c2b-128">Sämtliche von Ihnen bereitgestellte Parameterwerte werden automatisch in einen `DbParameter` konvertiert.</span><span class="sxs-lookup"><span data-stu-id="21c2b-128">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="21c2b-129">Im folgenden Beispiel wird ein einzelner Parameter an eine gespeicherte Prozedur übergeben.</span><span class="sxs-lookup"><span data-stu-id="21c2b-129">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="21c2b-130">Obwohl dies `String.Format`-Syntax ähnelt, ist der bereitgestellte Wert in einen Parameter eingeschlossen. Der Name des generierten Parameters wird dort eingefügt, wo der Platzhalter `{0}` angegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="21c2b-130">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="21c2b-131">Dies ist die gleiche Abfrage, jedoch mit einer Zeichenfolgeninterpolationssyntax, die ab EF Core 2.0 unterstützt wird:</span><span class="sxs-lookup"><span data-stu-id="21c2b-131">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="21c2b-132">Sie können auch einen DbParameter erstellen und diesen als Parameterwert bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="21c2b-132">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="21c2b-133">Dadurch können Sie benannte Parameter in der SQL-Abfragezeichenfolge verwenden.</span><span class="sxs-lookup"><span data-stu-id="21c2b-133">This allows you to use named parameters in the SQL query string</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="21c2b-134">Zusammensetzen mit LINQ</span><span class="sxs-lookup"><span data-stu-id="21c2b-134">Composing with LINQ</span></span>

<span data-ttu-id="21c2b-135">Wenn die SQL-Abfrage in der Datenbank zusammengesetzt werden kann, können Sie die erste unformatierte SQL-Abfrage mit LINQ-Operatoren zusammensetzen.</span><span class="sxs-lookup"><span data-stu-id="21c2b-135">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="21c2b-136">Zusammensetzbare SQL-Abfragen beginnen mit dem Schlüsselwort `SELECT`.</span><span class="sxs-lookup"><span data-stu-id="21c2b-136">SQL queries that can be composed on being with the `SELECT` keyword.</span></span>

<span data-ttu-id="21c2b-137">Im folgenden Beispiel wird eine unformatierte SQL-Abfrage verwendet, die eine Auswahl aus einer Tabellenwertfunktion trifft und diese anschließend mit LINQ zur Durchführung einer Filterung und Sortierung zusammensetzt.</span><span class="sxs-lookup"><span data-stu-id="21c2b-137">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a><span data-ttu-id="21c2b-138">Einschließen zugehöriger Daten</span><span class="sxs-lookup"><span data-stu-id="21c2b-138">Including related data</span></span>

<span data-ttu-id="21c2b-139">Das Zusammensetzen mit LINQ-Operatoren dient zum Einschließen zugehöriger Daten in der Abfrage.</span><span class="sxs-lookup"><span data-stu-id="21c2b-139">Composing with LINQ operators can be used to include related data in the query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> <span data-ttu-id="21c2b-140">**Für unformatierte SQL-Abfragen immer Parametrisierung verwenden:** APIs, die eine unformatierte SQL-Zeichenfolge wie `FromSql` und `ExecuteSqlCommand` akzeptieren, lassen zu, dass Werte problemlos als Parameter übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="21c2b-140">**Always use parameterization for raw SQL queries:** APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="21c2b-141">Zusätzlich zum Überprüfen der Benutzereingabe sollten Sie die Parametrisierung für sämtliche Werte verwenden, die in unformatierten SQL-Abfragen/-Befehlen enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="21c2b-141">In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="21c2b-142">Wenn Sie mit der Zeichenfolgenverkettung auf dynamische Weise Teile der Abfragezeichenfolge erstellen, müssen Sie zum Schutz vor Angriffen durch die Einschleusung von SQL-Befehlen jede Eingabe überprüfen.</span><span class="sxs-lookup"><span data-stu-id="21c2b-142">If you are using string concatenation to dynamically build any part of the query string then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
