---
title: Transaktionen – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
uid: core/saving/transactions
ms.openlocfilehash: 7083a1228420416a1b60d9744ca2dad2339be53f
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993604"
---
# <a name="using-transactions"></a><span data-ttu-id="c3484-102">Verwenden von Transaktionen</span><span class="sxs-lookup"><span data-stu-id="c3484-102">Using Transactions</span></span>

<span data-ttu-id="c3484-103">Mit Transaktionen können mehrere Datenbankvorgänge einzeln verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="c3484-103">Transactions allow several database operations to be processed in an atomic manner.</span></span> <span data-ttu-id="c3484-104">Wenn die Transaktion committet wird, werden alle Vorgänge erfolgreich auf die Datenbank angewendet.</span><span class="sxs-lookup"><span data-stu-id="c3484-104">If the transaction is committed, all of the operations are successfully applied to the database.</span></span> <span data-ttu-id="c3484-105">Falls für die Transaktion ein Rollback ausgeführt wird, wird keiner der Vorgänge für die Datenbank übernommen.</span><span class="sxs-lookup"><span data-stu-id="c3484-105">If the transaction is rolled back, none of the operations are applied to the database.</span></span>

> [!TIP]  
> <span data-ttu-id="c3484-106">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="c3484-106">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) on GitHub.</span></span>

## <a name="default-transaction-behavior"></a><span data-ttu-id="c3484-107">Standardtransaktionsverhalten</span><span class="sxs-lookup"><span data-stu-id="c3484-107">Default transaction behavior</span></span>

<span data-ttu-id="c3484-108">Wenn der Datenbankanbieter Transaktionen unterstützt, werden standardmäßig alle Änderungen in einem einzigen Aufruf von `SaveChanges()` in einer Transaktion angewendet.</span><span class="sxs-lookup"><span data-stu-id="c3484-108">By default, if the database provider supports transactions, all changes in a single call to `SaveChanges()` are applied in a transaction.</span></span> <span data-ttu-id="c3484-109">Wenn bei einer der Änderungen ein Fehler auftritt, wird ein Rollback ausgeführt, und keine der Änderungen wird für die Datenbank übernommen.</span><span class="sxs-lookup"><span data-stu-id="c3484-109">If any of the changes fail, then the transaction is rolled back and none of the changes are applied to the database.</span></span> <span data-ttu-id="c3484-110">`SaveChanges()` wird also entweder vollständig ausgeführt oder überhaupt nicht, wenn ein Fehler auftritt. Die Datenbank bleibt in diesem Fall unverändert.</span><span class="sxs-lookup"><span data-stu-id="c3484-110">This means that `SaveChanges()` is guaranteed to either completely succeed, or leave the database unmodified if an error occurs.</span></span>

<span data-ttu-id="c3484-111">Bei den meisten Anwendungen ist dieses Standardverhalten ausreichend.</span><span class="sxs-lookup"><span data-stu-id="c3484-111">For most applications, this default behavior is sufficient.</span></span> <span data-ttu-id="c3484-112">Sie sollten Transaktionen nur manuell steuern, wenn die Anforderungen Ihrer Anwendung dies notwendig machen.</span><span class="sxs-lookup"><span data-stu-id="c3484-112">You should only manually control transactions if your application requirements deem it necessary.</span></span>

## <a name="controlling-transactions"></a><span data-ttu-id="c3484-113">Steuern von Transaktionen</span><span class="sxs-lookup"><span data-stu-id="c3484-113">Controlling transactions</span></span>

<span data-ttu-id="c3484-114">Mit der `DbContext.Database`-API können Sie Transaktionen beginnen, committen und ein Rollback dafür ausführen.</span><span class="sxs-lookup"><span data-stu-id="c3484-114">You can use the `DbContext.Database` API to begin, commit, and rollback transactions.</span></span> <span data-ttu-id="c3484-115">Im folgenden Beispiel werden zwei `SaveChanges()`-Vorgänge und eine LINQ-Abfrage dargestellt, die in einer einzelnen Transaktion ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="c3484-115">The following example shows two `SaveChanges()` operations and a LINQ query being executed in a single transaction.</span></span>

<span data-ttu-id="c3484-116">Nicht alle Datenbankanbieter unterstützen Transaktionen.</span><span class="sxs-lookup"><span data-stu-id="c3484-116">Not all database providers support transactions.</span></span> <span data-ttu-id="c3484-117">In diesem Fall wird ein Fehler ausgelöst oder kein Vorgang gestartet, wenn Transaktions-APIs aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="c3484-117">Some providers may throw or no-op when transaction APIs are called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ControllingTransaction/Sample.cs?name=Transaction&highlight=3,17,18,19)]

## <a name="cross-context-transaction-relational-databases-only"></a><span data-ttu-id="c3484-118">Kontextübergreifende Transaktionen (nur relationale Datenbanken)</span><span class="sxs-lookup"><span data-stu-id="c3484-118">Cross-context transaction (relational databases only)</span></span>

<span data-ttu-id="c3484-119">Sie können Transaktionen auch für mehrere Kontextinstanzen freigeben.</span><span class="sxs-lookup"><span data-stu-id="c3484-119">You can also share a transaction across multiple context instances.</span></span> <span data-ttu-id="c3484-120">Diese Funktion ist nur bei Anbietern von relationalen Datenbanken verfügbar, da `DbTransaction` und `DbConnection` verwendet werden müssen, die charakteristisch für relationale Datenbanken sind.</span><span class="sxs-lookup"><span data-stu-id="c3484-120">This functionality is only available when using a relational database provider because it requires the use of `DbTransaction` and `DbConnection`, which are specific to relational databases.</span></span>

<span data-ttu-id="c3484-121">Um eine Transaktion freigeben zu können, müssen die Kontexte `DbConnection` und `DbTransaction` verwenden.</span><span class="sxs-lookup"><span data-stu-id="c3484-121">To share a transaction, the contexts must share both a `DbConnection` and a `DbTransaction`.</span></span>

### <a name="allow-connection-to-be-externally-provided"></a><span data-ttu-id="c3484-122">Zulassen von extern bereitgestellten Verbindungen</span><span class="sxs-lookup"><span data-stu-id="c3484-122">Allow connection to be externally provided</span></span>

<span data-ttu-id="c3484-123">Um `DbConnection` freigeben zu können, muss eine Verbindung an einen Kontext übergeben werden können, während dieser erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="c3484-123">Sharing a `DbConnection` requires the ability to pass a connection into a context when constructing it.</span></span>

<span data-ttu-id="c3484-124">Die einfachste Möglichkeit, eine externe Bereitstellung von `DbConnection` zuzulassen, ist, den Kontext nicht mehr mit der `DbContext.OnConfiguring`-Methode zu konfigurieren, sondern extern `DbContextOptions` zu erstellen und es an den Kontextkonstruktor zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="c3484-124">The easiest way to allow `DbConnection` to be externally provided, is to stop using the `DbContext.OnConfiguring` method to configure the context and externally create `DbContextOptions` and pass them to the context constructor.</span></span>

> [!TIP]  
> <span data-ttu-id="c3484-125">`DbContextOptionsBuilder` ist die API, die Sie in `DbContext.OnConfiguring` zum Konfigurieren des Kontexts verwendet haben. Jetzt werden Sie es extern verwenden und `DbContextOptions` erstellen.</span><span class="sxs-lookup"><span data-stu-id="c3484-125">`DbContextOptionsBuilder` is the API you used in `DbContext.OnConfiguring` to configure the context, you are now going to use it externally to create `DbContextOptions`.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

<span data-ttu-id="c3484-126">Alternativ können Sie weiterhin `DbContext.OnConfiguring` verwenden und eine `DbConnection` akzeptieren, die gespeichert und dann in `DbContext.OnConfiguring` verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="c3484-126">An alternative is to keep using `DbContext.OnConfiguring`, but accept a `DbConnection` that is saved and then used in `DbContext.OnConfiguring`.</span></span>

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

### <a name="share-connection-and-transaction"></a><span data-ttu-id="c3484-127">Freigeben der Verbindung und der Transaktion</span><span class="sxs-lookup"><span data-stu-id="c3484-127">Share connection and transaction</span></span>

<span data-ttu-id="c3484-128">Sie können nun mehrere Kontextinstanzen erstellen, die die gleiche Verbindung verwenden.</span><span class="sxs-lookup"><span data-stu-id="c3484-128">You can now create multiple context instances that share the same connection.</span></span> <span data-ttu-id="c3484-129">Anschließend tragen Sie mit der `DbContext.Database.UseTransaction(DbTransaction)`-API beide Kontexte in derselben Transaktion ein.</span><span class="sxs-lookup"><span data-stu-id="c3484-129">Then use the `DbContext.Database.UseTransaction(DbTransaction)` API to enlist both contexts in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a><span data-ttu-id="c3484-130">Verwenden von externen DbTransactions (nur relationale Datenbanken)</span><span class="sxs-lookup"><span data-stu-id="c3484-130">Using external DbTransactions (relational databases only)</span></span>

<span data-ttu-id="c3484-131">Wenn Sie mit verschiedenen Datenzugriffstechnologien auf eine relationale Datenbank zugreifen, sollten Sie eine gemeinsame Transaktion für die Vorgänge einrichten, die von diesen verschiedenen Technologien ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="c3484-131">If you are using multiple data access technologies to access a relational database, you may want to share a transaction between operations performed by these different technologies.</span></span>

<span data-ttu-id="c3484-132">Im folgenden Beispiel wird gezeigt, wie ein ADO.NET SqlClient-Vorgang und ein Entity Framework Core-Vorgang in derselben Transaktion ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="c3484-132">The following example, shows how to perform an ADO.NET SqlClient operation and an Entity Framework Core operation in the same transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a><span data-ttu-id="c3484-133">Verwenden von System.Transactions</span><span class="sxs-lookup"><span data-stu-id="c3484-133">Using System.Transactions</span></span>

> [!NOTE]  
> <span data-ttu-id="c3484-134">Dieses Feature ist neu in EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="c3484-134">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="c3484-135">Es ist möglich, Ambient-Transaktionen zu verwenden, wenn Sie einen größeren Bereich koordinieren müssen.</span><span class="sxs-lookup"><span data-stu-id="c3484-135">It is possible to use ambient transactions if you need to coordinate across a larger scope.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,2,3,26,27,28)]

<span data-ttu-id="c3484-136">Sie können auch eine Eintragung in einer expliziten Transaktion vornehmen.</span><span class="sxs-lookup"><span data-stu-id="c3484-136">It is also possible to enlist in an explicit transaction.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,15,28,29,30)]

### <a name="limitations-of-systemtransactions"></a><span data-ttu-id="c3484-137">Einschränkungen von System.Transactions</span><span class="sxs-lookup"><span data-stu-id="c3484-137">Limitations of System.Transactions</span></span>  

1. <span data-ttu-id="c3484-138">In EF Core müssen die Datenbankanbieter die Unterstützung für System.Transactions implementieren.</span><span class="sxs-lookup"><span data-stu-id="c3484-138">EF Core relies on database providers to implement support for System.Transactions.</span></span> <span data-ttu-id="c3484-139">Unterstützung ist unter ADO.NET-Anbietern für .NET Framework zwar recht üblich, die API wurde .NET Core jedoch erst vor Kurzem hinzugefügt. Daher ist die Unterstützung noch nicht weit verbreitet.</span><span class="sxs-lookup"><span data-stu-id="c3484-139">Although support is quite common among ADO.NET providers for .NET Framework, the API has only been recently added to .NET Core and hence support is not as widespread.</span></span> <span data-ttu-id="c3484-140">Wenn ein Anbieter keine Unterstützung für System.Transactions implementiert, ist es möglich, dass Aufrufe dieser APIs vollständig ignoriert werden.</span><span class="sxs-lookup"><span data-stu-id="c3484-140">If a provider does not implement support for System.Transactions, it is possible that calls to these APIs will be completely ignored.</span></span> <span data-ttu-id="c3484-141">SqlClient für .NET Core bietet Unterstützung ab Version 2.1.</span><span class="sxs-lookup"><span data-stu-id="c3484-141">SqlClient for .NET Core does support it from 2.1 onwards.</span></span> <span data-ttu-id="c3484-142">In SqlClient für .NET Core 2.0 wird eine Ausnahme ausgelöst, wenn Sie dieses Feature ausführen.</span><span class="sxs-lookup"><span data-stu-id="c3484-142">SqlClient for .NET Core 2.0 will throw an exception of you attempt to use the feature.</span></span> 

   > [!IMPORTANT]  
   > <span data-ttu-id="c3484-143">Daher sollten Sie testen, ob die API ordnungsgemäß mit Ihrem Anbieter funktioniert, bevor Sie sie für die Verwaltung von Transaktionen einsetzen.</span><span class="sxs-lookup"><span data-stu-id="c3484-143">It is recommended that you test that the API behaves correctly with your provider before you rely on it for managing transactions.</span></span> <span data-ttu-id="c3484-144">Sollte die API nicht funktionieren, wenden Sie sich bitte an den Maintainer des Datenbankanbieters.</span><span class="sxs-lookup"><span data-stu-id="c3484-144">You are encouraged to contact the maintainer of the database provider if it does not.</span></span> 

2. <span data-ttu-id="c3484-145">Ab Version 2.1 enthält die System.Transactions-Implementierung in .NET Core keine Unterstützung für verteilte Transaktionen. Aus diesem Grund können Sie weder mit `TransactionScope` noch mit `CommittableTransaction` Transaktionen in mehreren Ressourcen-Managern koordinieren.</span><span class="sxs-lookup"><span data-stu-id="c3484-145">As of version 2.1, the System.Transactions implementation in .NET Core does not include support for distributed transactions, therefore you cannot use `TransactionScope` or `CommittableTransaction` to coordinate transactions across multiple resource managers.</span></span> 
