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
# <a name="raw-sql-queries"></a><span data-ttu-id="8df7e-102">RAW-SQL-Abfragen</span><span class="sxs-lookup"><span data-stu-id="8df7e-102">Raw SQL Queries</span></span>

<span data-ttu-id="8df7e-103">Entity Framework Core können Sie SQL-Abfragen, die unformatierten Dropdown, bei der Arbeit mit einer relationalen Datenbank.</span><span class="sxs-lookup"><span data-stu-id="8df7e-103">Entity Framework Core allows you to drop down to raw SQL queries when working with a relational database.</span></span> <span data-ttu-id="8df7e-104">Dies kann nützlich sein, wenn die Abfrage, die Sie ausführen möchten mit LINQ ausgedrückt werden kann, oder wenn eine LINQ-Abfrage mit keine ineffizienten SQL, die an die Datenbank dazu führte, ist.</span><span class="sxs-lookup"><span data-stu-id="8df7e-104">This can be useful if the query you want to perform can't be expressed using LINQ, or if using a LINQ query is resulting in inefficient SQL being sent to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="8df7e-105">Sie können anzeigen, dass dieser Artikel [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="8df7e-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="limitations"></a><span data-ttu-id="8df7e-106">Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="8df7e-106">Limitations</span></span>

<span data-ttu-id="8df7e-107">Es gibt einige Einschränkungen bei Verwendung von SQL-Abfragen, die unformatierte geachtet werden:</span><span class="sxs-lookup"><span data-stu-id="8df7e-107">There are a couple of limitations to be aware of when using raw SQL queries:</span></span>
* <span data-ttu-id="8df7e-108">SQL-Abfragen können nur auf Entitätstypen zurückgeben, die Teil des Modells verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="8df7e-108">SQL queries can only be used to return entity types that are part of your model.</span></span> <span data-ttu-id="8df7e-109">Es ist eine Erweiterung auf unseren nachholbedarf zu [aktivieren, die Ad-hoc-Typen von unformatierten SQL-Abfragen zurückgeben](https://github.com/aspnet/EntityFramework/issues/1862).</span><span class="sxs-lookup"><span data-stu-id="8df7e-109">There is an enhancement on our backlog to [enable returning ad-hoc types from raw SQL queries](https://github.com/aspnet/EntityFramework/issues/1862).</span></span>

* <span data-ttu-id="8df7e-110">Die SQL-Abfrage muss es sich um Daten für alle Eigenschaften des Entitätstyps zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="8df7e-110">The SQL query must return data for all properties of the entity type.</span></span>

* <span data-ttu-id="8df7e-111">Die Spaltennamen im Resultset müssen die Spaltennamen entsprechen, denen Eigenschaften zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="8df7e-111">The column names in the result set must match the column names that properties are mapped to.</span></span> <span data-ttu-id="8df7e-112">Beachten Sie, dass dieser vom EF6 unterscheidet, in denen Eigenschaft/spaltenzuordnung für unformatierte SQL-Abfragen wurde ignoriert, und Resultsetspalte mussten Namen der Eigenschaft entsprechen.</span><span class="sxs-lookup"><span data-stu-id="8df7e-112">Note this is different from EF6 where property/column mapping was ignored for raw SQL queries and result set column names had to match the property names.</span></span>

* <span data-ttu-id="8df7e-113">Die SQL-Abfrage kann nicht verknüpfte Daten enthalten.</span><span class="sxs-lookup"><span data-stu-id="8df7e-113">The SQL query cannot contain related data.</span></span> <span data-ttu-id="8df7e-114">Allerdings in vielen Fällen machen Sie auf die Abfrage mithilfe der `Include` Operator, um verwandte Daten zurückzugeben (finden Sie unter [einschließlich der zugehörige Daten](#including-related-data)).</span><span class="sxs-lookup"><span data-stu-id="8df7e-114">However, in many cases you can compose on top of the query using the `Include` operator to return related data (see [Including related data](#including-related-data)).</span></span>

## <a name="basic-raw-sql-queries"></a><span data-ttu-id="8df7e-115">Grundlegende unformatierten SQL-Abfragen</span><span class="sxs-lookup"><span data-stu-id="8df7e-115">Basic raw SQL queries</span></span>

<span data-ttu-id="8df7e-116">Sie können die *FromSql* Erweiterungsmethode, um eine LINQ-Abfrage basierend auf einer unformatierten SQL-Abfrage zu beginnen.</span><span class="sxs-lookup"><span data-stu-id="8df7e-116">You can use the *FromSql* extension method to begin a LINQ query based on a raw SQL query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

<span data-ttu-id="8df7e-117">RAW-SQL-Abfragen können zum Ausführen einer gespeicherten Prozedur verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="8df7e-117">Raw SQL queries can be used to execute a stored procedure.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a><span data-ttu-id="8df7e-118">Übergeben von Parametern</span><span class="sxs-lookup"><span data-stu-id="8df7e-118">Passing parameters</span></span>

<span data-ttu-id="8df7e-119">Wie bei jeder API, die SQL akzeptiert, ist es wichtig, die zum Schutz vor SQL Injection-Angriff Eingabe durch den Benutzer zu parametrisieren.</span><span class="sxs-lookup"><span data-stu-id="8df7e-119">As with any API that accepts SQL, it is important to parameterize any user input to protect against a SQL injection attack.</span></span> <span data-ttu-id="8df7e-120">Sie können Platzhalter für Parameter in der SQL-Abfragezeichenfolge enthalten, und geben Sie dann Parameterwerte als zusätzliche Argumente.</span><span class="sxs-lookup"><span data-stu-id="8df7e-120">You can include parameter placeholders in the SQL query string and then supply parameter values as additional arguments.</span></span> <span data-ttu-id="8df7e-121">Alle Parameterwerte, die Sie angeben, werden automatisch konvertiert werden, um eine `DbParameter`.</span><span class="sxs-lookup"><span data-stu-id="8df7e-121">Any parameter values you supply will automatically be converted to a `DbParameter`.</span></span>

<span data-ttu-id="8df7e-122">Das folgende Beispiel übergibt einen einzelnen Parameter an eine gespeicherte Prozedur.</span><span class="sxs-lookup"><span data-stu-id="8df7e-122">The following example passes a single parameter to a stored procedure.</span></span> <span data-ttu-id="8df7e-123">Während dies aussehen könnte z. B. `String.Format` -Syntax wird der angegebene Wert in umschlossen Parameter und den generierten Parameternamen eingefügt Where der `{0}` Platzhalter angegeben wurde.</span><span class="sxs-lookup"><span data-stu-id="8df7e-123">While this may look like `String.Format` syntax, the supplied value is wrapped in a parameter and the generated parameter name inserted where the `{0}` placeholder was specified.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

<span data-ttu-id="8df7e-124">Dies ist die gleiche Abfrage jedoch mit einer Zeichenfolge Interpolation Syntax in EF Core 2.0 und höher unterstützt wird:</span><span class="sxs-lookup"><span data-stu-id="8df7e-124">This is the same query but using string interpolation syntax, which is supported in EF Core 2.0 and above:</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

<span data-ttu-id="8df7e-125">Sie können auch ein DbParameter-Objekt zu erstellen und als Parameterwert angeben.</span><span class="sxs-lookup"><span data-stu-id="8df7e-125">You can also construct a DbParameter and supply it as a parameter value.</span></span> <span data-ttu-id="8df7e-126">Dadurch können Sie benannte Parameter in der SQL-Abfragezeichenfolge verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="8df7e-126">This allows you to use named parameters in the SQL query string</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a><span data-ttu-id="8df7e-127">Verfassen von LINQ</span><span class="sxs-lookup"><span data-stu-id="8df7e-127">Composing with LINQ</span></span>

<span data-ttu-id="8df7e-128">Wenn die SQL-Abfrage auf die Datenbank geschrieben werden kann, können Sie zusätzlich zu den anfänglichen raw SQL-Abfrage, die mithilfe von LINQ-Operatoren zusammenstellen.</span><span class="sxs-lookup"><span data-stu-id="8df7e-128">If the SQL query can be composed on in the database, then you can compose on top of the initial raw SQL query using LINQ operators.</span></span> <span data-ttu-id="8df7e-129">SQL-Abfragen, die zusammengesetzt werden können, auf die mit dem `SELECT` Schlüsselwort.</span><span class="sxs-lookup"><span data-stu-id="8df7e-129">SQL queries that can be composed on being with the `SELECT` keyword.</span></span>

<span data-ttu-id="8df7e-130">Im folgenden Beispiel wird eine unformatierte SQL-Abfrage, die aus einem Tabellenwertparameter-Funktion (TVF) Wählt aus, und klicken Sie dann verfasst darauf verwenden LINQ zum Ausführen von Filtern und sortieren.</span><span class="sxs-lookup"><span data-stu-id="8df7e-130">The following example uses a raw SQL query that selects from a Table-Valued Function (TVF) and then composes on it using LINQ to perform filtering and sorting.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a><span data-ttu-id="8df7e-131">Einschließlich der zugehörige Daten</span><span class="sxs-lookup"><span data-stu-id="8df7e-131">Including related data</span></span>

<span data-ttu-id="8df7e-132">Verfassen von LINQ-Operatoren kann verwendet werden, um verwandte Daten in der Abfrage enthalten.</span><span class="sxs-lookup"><span data-stu-id="8df7e-132">Composing with LINQ operators can be used to include related data in the query.</span></span>

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> <span data-ttu-id="8df7e-133">**Verwenden Sie immer die Parametrisierung für unformatierte SQL-Abfragen:** APIs, die eine unformatierte SQL akzeptieren eine Zeichenfolge wie z. B. `FromSql` und `ExecuteSqlCommand` ermöglicht, dass Werte einfach als Parameter übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="8df7e-133">**Always use parameterization for raw SQL queries:** APIs that accept a raw SQL string such as `FromSql` and `ExecuteSqlCommand` allow values to be easily passed as parameters.</span></span> <span data-ttu-id="8df7e-134">Zusätzlich zum Validieren von Benutzereingaben, verwenden Sie immer Parametrisierung für eine beliebige Werte, die in einem unformatierten SQL-Abfrage/Befehl verwendet.</span><span class="sxs-lookup"><span data-stu-id="8df7e-134">In addition to validating user input, always use parameterization for any values used in a raw SQL query/command.</span></span> <span data-ttu-id="8df7e-135">Wenn Sie einen beliebigen Teil der Abfragezeichenfolge dynamisch zu erstellen, dann sind Sie verantwortlich für das Validieren von Eingaben zum Schutz vor SQL Injection-Angriffen Verketten von Zeichenfolgen verwenden.</span><span class="sxs-lookup"><span data-stu-id="8df7e-135">If you are using string concatenation to dynamically build any part of the query string then you are responsible for validating any input to protect against SQL injection attacks.</span></span>
