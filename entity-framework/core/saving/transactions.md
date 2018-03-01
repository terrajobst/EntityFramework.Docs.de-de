---
title: Transaktionen - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
ms.technology: entity-framework-core
uid: core/saving/transactions
ms.openlocfilehash: 2dda7b7d58ae058fc2aa89fe16fbf46adc8c6bdc
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="using-transactions"></a><span data-ttu-id="34ac5-102">Verwenden von Transaktionen</span><span class="sxs-lookup"><span data-stu-id="34ac5-102">Using Transactions</span></span>

<span data-ttu-id="34ac5-103">Transaktionen können mehrere Datenbankvorgänge in eine atomare Weise verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="34ac5-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="34ac5-104">Wenn die Transaktion ein Commit ausgeführt wird, werden alle Vorgänge erfolgreich auf die Datenbank angewendet.</span><span class="sxs-lookup"><span data-stu-id="34ac5-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="34ac5-105">Wenn die Transaktion ein Rollback ausgeführt wird, werden keiner der Vorgänge auf die Datenbank angewendet.</span><span class="sxs-lookup"><span data-stu-id="34ac5-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="34ac5-106">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="34ac5-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="34ac5-107">Standardverhalten für die Transaktion</span><span class="sxs-lookup"><span data-stu-id="34ac5-107">Default transaction behavior</span></span>

<span data-ttu-id="34ac5-108">Standardmäßig, wenn der Anbieter Transaktionen unterstützt alle Änderungen in einem einzigen Aufruf `SaveChanges()` in einer Transaktion angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="34ac5-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="34ac5-109">Wenn einer der Änderungen fehlschlägt, wird ein Rollback für die Transaktion, und keine der Änderungen auf die Datenbank angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="34ac5-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="34ac5-110">Dies bedeutet, dass `SaveChanges()` wird sichergestellt, dass entweder vollständig erfolgreich ausgeführt werden, oder lassen Sie die Datenbank, die unverändert bleiben sollen, wenn ein Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="34ac5-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="34ac5-111">Für die meisten Anwendungen ist dieses Standardverhalten ausreichend.</span><span class="sxs-lookup"><span data-stu-id="34ac5-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="34ac5-112">Transaktionen sollten nur manuell gesteuert werden, wenn die Anforderungen an Ihre Anwendung erforderlichen aufgefasst.</span><span class="sxs-lookup"><span data-stu-id="34ac5-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="34ac5-113">Steuern von Transaktionen</span><span class="sxs-lookup"><span data-stu-id="34ac5-113">Controlling transactions</span></span>

<span data-ttu-id="34ac5-114">Sie können die `DbContext.Database` -API zum Starten der commit und Rollback-Transaktionen.</span><span class="sxs-lookup"><span data-stu-id="34ac5-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="34ac5-115">Das folgende Beispiel zeigt zwei `SaveChanges()` Vorgänge und eine LINQ-Abfrage in einer einzelnen Transaktion ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="34ac5-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="34ac5-116">Nicht alle Datenbankanbieter unterstützen Transaktionen.</span><span class="sxs-lookup"><span data-stu-id="34ac5-116">Not all database providers support transactions.</span></span> <span data-ttu-id="34ac5-117">Einige Anbieter auslösen oder ohne-Op Wenn Transaktion APIs aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="34ac5-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

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

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="34ac5-118">Cross-Kontexttransaktion (nur bei relationalen Datenbanken)</span><span class="sxs-lookup"><span data-stu-id="34ac5-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="34ac5-119">Sie können auch eine Transaktion über mehrere Instanzen von Kontext freigeben.</span><span class="sxs-lookup"><span data-stu-id="34ac5-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="34ac5-120">Diese Funktion ist nur verfügbar, wenn einen relationale Datenbank-Anbieter verwenden, da es erfordert, dass die Verwendung von `DbTransaction` und `DbConnection`, die speziell für relationale Datenbanken sind.</span><span class="sxs-lookup"><span data-stu-id="34ac5-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="34ac5-121">Um eine Transaktion gemeinsam nutzen, müssen die Kontexte verwenden beide eine `DbConnection` und ein `DbTransaction`.</span><span class="sxs-lookup"><span data-stu-id="34ac5-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="34ac5-122">Lassen Sie Verbindung mit extern bereitgestellt werden zu</span><span class="sxs-lookup"><span data-stu-id="34ac5-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="34ac5-123">Freigeben einer `DbConnection` erfordert die Möglichkeit, eine Verbindung in einem Kontext übergeben wird, wenn es zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="34ac5-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="34ac5-124">Die einfachste Möglichkeit zum Zulassen `DbConnection` extern bereitgestellt werden, ist, Beenden der Verwendung der `DbContext.OnConfiguring` Methode so extern erstellen und konfigurieren den Kontext `DbContextOptions` und an den Kontext Konstruktor zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="34ac5-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="34ac5-125">`DbContextOptionsBuilder` ist die API im verwendet haben `DbContext.OnConfiguring` um den Kontext zu konfigurieren, Sie sind nun damit extern erstellen mit `DbContextOptions`.</span><span class="sxs-lookup"><span data-stu-id="34ac5-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

<span data-ttu-id="34ac5-126">Eine Alternative besteht darin, die weitere Verwendung `DbContext.OnConfiguring`, akzeptieren jedoch eine `DbConnection` , gespeichert und dann im verwendet `DbContext.OnConfiguring`.</span><span class="sxs-lookup"><span data-stu-id="34ac5-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

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

### <a name="share-connection-and-transaction"></a><span data-ttu-id="34ac5-127">Freigabe-Verbindung und der Transaktionsstatus</span><span class="sxs-lookup"><span data-stu-id="34ac5-127">Share connection and transaction</span></span>

<span data-ttu-id="34ac5-128">Sie können nun mehrere Kontext Instanzen erstellen, die die gleiche Verbindung verwenden.</span><span class="sxs-lookup"><span data-stu-id="34ac5-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="34ac5-129">Verwenden Sie dann die `DbContext.Database.UseTransaction(DbTransaction)` -API, um beide Kontexte in derselben Transaktion eintragen.</span><span class="sxs-lookup"><span data-stu-id="34ac5-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="34ac5-130">Verwenden von externen DbTransactions (nur bei relationalen Datenbanken)</span><span class="sxs-lookup"><span data-stu-id="34ac5-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="34ac5-131">Wenn Sie mehrere datenzugriffstechnologien Zugriff auf eine relationale Datenbank verwenden, empfiehlt es sich, eine Transaktion zwischen Operationen, die von diesen anderen Technologien gemeinsam nutzen.</span><span class="sxs-lookup"><span data-stu-id="34ac5-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="34ac5-132">Im folgenden Beispiel wird gezeigt, wie ein ADO.NET SqlClient-Vorgang und ein Entity Framework Core-Vorgang in derselben Transaktion ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="34ac5-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a><span data-ttu-id="34ac5-133">Using System.Transactions</span><span class="sxs-lookup"><span data-stu-id="34ac5-133">Using System.Transactions</span></span>

> [!NOTE]  
> <span data-ttu-id="34ac5-134">Dieses Feature ist neu in EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="34ac5-134">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="34ac5-135">Es ist möglich, die Ambiente-Transaktionen verwenden, wenn Sie über einen größeren Bereich koordinieren müssen.</span><span class="sxs-lookup"><span data-stu-id="34ac5-135">It is possible to use ambient transactions if you need to coordinate across a larger scope.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,24,25,26)]

<span data-ttu-id="34ac5-136">Es ist auch möglich, die in einer expliziten Transaktion eintragen.</span><span class="sxs-lookup"><span data-stu-id="34ac5-136">It is also possible to enlist in an explicit transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,13,26,27,28)]

### <a name="limitations-of-systemtransactions"></a><span data-ttu-id="34ac5-137">Einschränkungen von ' System.Transactions '</span><span class="sxs-lookup"><span data-stu-id="34ac5-137">Limitations of System.Transactions</span></span>  

1. <span data-ttu-id="34ac5-138">EF Core basiert auf Datenbankanbietern zum Implementieren der Unterstützung für System.Transactions hinzu.</span><span class="sxs-lookup"><span data-stu-id="34ac5-138">EF Core relies on database providers to implement support for System.Transactions.</span></span> <span data-ttu-id="34ac5-139">Zwar Unterstützung für .NET Framework, die API nur wurde vor kurzem hinzugefügt, um .NET Core und unterstützen daher üblich, das zwischen ADO.NET-Anbietern ist ist nicht weit verbreitet ist.</span><span class="sxs-lookup"><span data-stu-id="34ac5-139">Although support is quite common among ADO.NET providers for .NET Framework, the API has only been recently added to .NET Core and hence support is not be as widespread.</span></span> <span data-ttu-id="34ac5-140">Wenn ein Anbieter keine Unterstützung für System.Transactions implementiert, ist es möglich, dass Aufrufe dieser APIs werden vollständig ignoriert.</span><span class="sxs-lookup"><span data-stu-id="34ac5-140">If a provider does not implement support for System.Transactions, it is possible that calls to these APIs will be completely ignored.</span></span> <span data-ttu-id="34ac5-141">SqlClient für .NET Core unterstützt aus 2.1 oder höher.</span><span class="sxs-lookup"><span data-stu-id="34ac5-141">SqlClient for .NET Core does support it from 2.1 onwards.</span></span> <span data-ttu-id="34ac5-142">Für .NET Core 2.0 SqlClient löst eine Ausnahme des Sie versuchen, das Feature zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="34ac5-142">SqlClient for .NET Core 2.0 will throw an exception of you attempt to use the feature.</span></span> 

   > [!IMPORTANT]  
   > <span data-ttu-id="34ac5-143">Es wird empfohlen, dass Sie testen, ob die API ordnungsgemäß mit Ihrem Anbieter verhält, bevor Sie sich darauf beziehen, für die Verwaltung von Transaktionen.</span><span class="sxs-lookup"><span data-stu-id="34ac5-143">It is recommended that you test that the API behaves correctly with your provider before you rely on it for managing transactions.</span></span> <span data-ttu-id="34ac5-144">Es wird empfohlen, wenden Sie sich an den Maintainer des Datenbank-Anbieters an, wenn dies nicht der Fall.</span><span class="sxs-lookup"><span data-stu-id="34ac5-144">You are encouraged to contact the maintainer of the database provider if it does not.</span></span> 

2. <span data-ttu-id="34ac5-145">Ab Version 2.1, die System.Transactions-Implementierung in .NET Core bietet keine Unterstützung für verteilte Transaktionen, die aus diesem Grund können keine `TransactionScope` oder `CommitableTransaction`Transaktionen über mehrere Ressourcen-Manager koordiniert.</span><span class="sxs-lookup"><span data-stu-id="34ac5-145">As of version 2.1, the System.Transactions implementation in .NET Core does not include support for distributed transactions, therefore you cannot use `TransactionScope` or `CommitableTransaction`to coordinate transactions across multiple resource managers.</span></span> 
