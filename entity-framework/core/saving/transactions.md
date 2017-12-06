---
title: Transaktionen - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
ms.technology: entity-framework-core
uid: core/saving/transactions
ms.openlocfilehash: a2f890c0af1e83cbcc1d40d68540ff7132a9bafd
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="using-transactions"></a><span data-ttu-id="d9a55-102">Verwenden von Transaktionen</span><span class="sxs-lookup"><span data-stu-id="d9a55-102">Using Transactions</span></span>

<span data-ttu-id="d9a55-103">Transaktionen können mehrere Datenbankvorgänge in eine atomare Weise verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="d9a55-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="d9a55-104">Wenn die Transaktion ein Commit ausgeführt wird, werden alle Vorgänge erfolgreich auf die Datenbank angewendet.</span><span class="sxs-lookup"><span data-stu-id="d9a55-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="d9a55-105">Wenn die Transaktion ein Rollback ausgeführt wird, werden keiner der Vorgänge auf die Datenbank angewendet.</span><span class="sxs-lookup"><span data-stu-id="d9a55-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="d9a55-106">Sie können anzeigen, dass dieser Artikel [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="d9a55-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="d9a55-107">Standardverhalten für die Transaktion</span><span class="sxs-lookup"><span data-stu-id="d9a55-107">Default transaction behavior</span></span>

<span data-ttu-id="d9a55-108">Standardmäßig, wenn der Anbieter Transaktionen unterstützt alle Änderungen in einem einzigen Aufruf `SaveChanges()` in einer Transaktion angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="d9a55-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="d9a55-109">Wenn einer der Änderungen fehlschlägt, wird ein Rollback für die Transaktion, und keine der Änderungen auf die Datenbank angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="d9a55-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="d9a55-110">Dies bedeutet, dass `SaveChanges()` wird sichergestellt, dass entweder vollständig erfolgreich ausgeführt werden, oder lassen Sie die Datenbank, die unverändert bleiben sollen, wenn ein Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="d9a55-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="d9a55-111">Für die meisten Anwendungen ist dieses Standardverhalten ausreichend.</span><span class="sxs-lookup"><span data-stu-id="d9a55-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="d9a55-112">Transaktionen sollten nur manuell gesteuert werden, wenn die Anforderungen an Ihre Anwendung erforderlichen aufgefasst.</span><span class="sxs-lookup"><span data-stu-id="d9a55-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="d9a55-113">Steuern von Transaktionen</span><span class="sxs-lookup"><span data-stu-id="d9a55-113">Controlling transactions</span></span>

<span data-ttu-id="d9a55-114">Sie können die `DbContext.Database` -API zum Starten der commit und Rollback-Transaktionen.</span><span class="sxs-lookup"><span data-stu-id="d9a55-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="d9a55-115">Das folgende Beispiel zeigt zwei `SaveChanges()` Vorgänge und eine LINQ-Abfrage in einer einzelnen Transaktion ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="d9a55-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="d9a55-116">Nicht alle Datenbankanbieter unterstützen Transaktionen.</span><span class="sxs-lookup"><span data-stu-id="d9a55-116">Not all database providers support transactions.</span></span> <span data-ttu-id="d9a55-117">Einige Anbieter auslösen oder ohne-Op Wenn Transaktion APIs aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="d9a55-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/ControllingTransaction/Sample.cs?highlight=3,17,18,19)] -->
``` csharp
        using (var context = new BloggingContext())
        {
            using (var transaction = context.Database.BeginTransaction())
            {
                try
                {
                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context.SaveChanges();

                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/visualstudio" });
                    context.SaveChanges();

                    var blogs = context.Blogs
                        .OrderBy(b => b.Url)
                        .ToList();

                    // Commit transaction if all commands succeed, transaction will auto-rollback
                    // when disposed if either commands fails
                    transaction.Commit();
                }
                catch (Exception)
                {
                    // TODO: Handle failure
                }
            }
        }
```

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="d9a55-118">Cross-Kontexttransaktion (nur bei relationalen Datenbanken)</span><span class="sxs-lookup"><span data-stu-id="d9a55-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="d9a55-119">Sie können auch eine Transaktion über mehrere Instanzen von Kontext freigeben.</span><span class="sxs-lookup"><span data-stu-id="d9a55-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="d9a55-120">Diese Funktion ist nur verfügbar, wenn einen relationale Datenbank-Anbieter verwenden, da es erfordert, dass die Verwendung von `DbTransaction` und `DbConnection`, die speziell für relationale Datenbanken sind.</span><span class="sxs-lookup"><span data-stu-id="d9a55-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="d9a55-121">Um eine Transaktion gemeinsam nutzen, müssen die Kontexte verwenden beide eine `DbConnection` und ein `DbTransaction`.</span><span class="sxs-lookup"><span data-stu-id="d9a55-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="d9a55-122">Lassen Sie Verbindung mit extern bereitgestellt werden zu</span><span class="sxs-lookup"><span data-stu-id="d9a55-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="d9a55-123">Freigeben einer `DbConnection` erfordert die Möglichkeit, eine Verbindung in einem Kontext übergeben wird, wenn es zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d9a55-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="d9a55-124">Die einfachste Möglichkeit zum Zulassen `DbConnection` extern bereitgestellt werden, ist, Beenden der Verwendung der `DbContext.OnConfiguring` Methode so extern erstellen und konfigurieren den Kontext `DbContextOptions` und an den Kontext Konstruktor zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="d9a55-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="d9a55-125">`DbContextOptionsBuilder`ist die API im verwendet haben `DbContext.OnConfiguring` um den Kontext zu konfigurieren, Sie sind nun damit extern erstellen mit `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="d9a55-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?highlight=3,4,5)] -->
``` csharp
    public class BloggingContext : DbContext
    {
        public BloggingContext(DbContextOptions<BloggingContext> options)
            : base(options)
        { }

        public DbSet<Blog> Blogs { get; set; }
    }
```

<span data-ttu-id="d9a55-126">Eine Alternative besteht darin, die weitere Verwendung `DbContext.OnConfiguring`, akzeptieren jedoch eine `DbConnection` , gespeichert und dann im verwendet `DbContext.OnConfiguring`.</span><span class="sxs-lookup"><span data-stu-id="d9a55-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    private DbConnection _connection;

    public BloggingContext(DbConnection connection)
    {
      _connection = connection;
    }

    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer(_connection);
    }
}
```

### <a name="share-connection-and-transaction"></a><span data-ttu-id="d9a55-127">Freigabe-Verbindung und der Transaktionsstatus</span><span class="sxs-lookup"><span data-stu-id="d9a55-127">Share connection and transaction</span></span>

<span data-ttu-id="d9a55-128">Sie können nun mehrere Kontext Instanzen erstellen, die die gleiche Verbindung verwenden.</span><span class="sxs-lookup"><span data-stu-id="d9a55-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="d9a55-129">Verwenden Sie dann die `DbContext.Database.UseTransaction(DbTransaction)` -API, um beide Kontexte in derselben Transaktion eintragen.</span><span class="sxs-lookup"><span data-stu-id="d9a55-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?highlight=1,2,3,7,16,23,24,25)] -->
``` csharp
        var options = new DbContextOptionsBuilder<BloggingContext>()
            .UseSqlServer(new SqlConnection(connectionString))
            .Options;

        using (var context1 = new BloggingContext(options))
        {
            using (var transaction = context1.Database.BeginTransaction())
            {
                try
                {
                    context1.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context1.SaveChanges();

                    using (var context2 = new BloggingContext(options))
                    {
                        context2.Database.UseTransaction(transaction.GetDbTransaction());

                        var blogs = context2.Blogs
                            .OrderBy(b => b.Url)
                            .ToList();
                    }

                    // Commit transaction if all commands succeed, transaction will auto-rollback
                    // when disposed if either commands fails
                    transaction.Commit();
                }
                catch (Exception)
                {
                    // TODO: Handle failure
                }
            }
        }
```

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="d9a55-130">Verwenden von externen DbTransactions (nur bei relationalen Datenbanken)</span><span class="sxs-lookup"><span data-stu-id="d9a55-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="d9a55-131">Wenn Sie mehrere datenzugriffstechnologien Zugriff auf eine relationale Datenbank verwenden, empfiehlt es sich, eine Transaktion zwischen Operationen, die von diesen anderen Technologien gemeinsam nutzen.</span><span class="sxs-lookup"><span data-stu-id="d9a55-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="d9a55-132">Im folgenden Beispiel wird gezeigt, wie ein ADO.NET SqlClient-Vorgang und ein Entity Framework Core-Vorgang in derselben Transaktion ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="d9a55-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?highlight=4,10,21,26,27,28)] -->
``` csharp
        var connection = new SqlConnection(connectionString);
        connection.Open();

        using (var transaction = connection.BeginTransaction())
        {
            try
            {
                // Run raw ADO.NET command in the transaction
                var command = connection.CreateCommand();
                command.Transaction = transaction;
                command.CommandText = "DELETE FROM dbo.Blogs";
                command.ExecuteNonQuery();

                // Run an EF Core command in the transaction
                var options = new DbContextOptionsBuilder<BloggingContext>()
                    .UseSqlServer(connection)
                    .Options;

                using (var context = new BloggingContext(options))
                {
                    context.Database.UseTransaction(transaction);
                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context.SaveChanges();
                }

                // Commit transaction if all commands succeed, transaction will auto-rollback
                // when disposed if either commands fails
                transaction.Commit();
            }
            catch (System.Exception)
            {
                // TODO: Handle failure
            }
```
