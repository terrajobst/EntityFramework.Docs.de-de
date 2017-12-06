---
title: Verbindungsresilienz - EF Core
author: rowanmiller
ms.author: divega
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: aec69577cd4b19fdebedb33ed6fd8f2665b0a032
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2017
---
# <a name="connection-resiliency"></a><span data-ttu-id="5a897-102">Verbindungsstabilität</span><span class="sxs-lookup"><span data-stu-id="5a897-102">Connection Resiliency</span></span>

<span data-ttu-id="5a897-103">Verbindungsresilienz führt automatisch Wiederholungsversuche für fehlerhafte Datenbankbefehle.</span><span class="sxs-lookup"><span data-stu-id="5a897-103">Connection resiliency automatically retries failed database commands.</span></span> <span data-ttu-id="5a897-104">Die Funktion kann mit einer beliebigen Datenbank verwendet werden, durch Angabe einer "Ausführungsstrategie", die die Logik zum Erkennen von Fehlern, und wiederholen die Befehle kapselt.</span><span class="sxs-lookup"><span data-stu-id="5a897-104">The feature can be used with any database by supplying an "execution strategy", which encapsulates the logic necessary to detect failures and retry commands.</span></span> <span data-ttu-id="5a897-105">EF-Core-Anbieter können auf ihre Datenbank fehlerbedingungen und optimale wiederholungsrichtlinien zugeschnittene Ausführungsstrategien angeben.</span><span class="sxs-lookup"><span data-stu-id="5a897-105">EF Core providers can supply execution strategies tailored to their specific database failure conditions and optimal retry policies.</span></span>

<span data-ttu-id="5a897-106">Beispielsweise enthält die SQL Server-Anbieter eine Ausführungsstrategie, die speziell auf SQL Server (einschließlich SQL Azure) zugeschnitten ist.</span><span class="sxs-lookup"><span data-stu-id="5a897-106">As an example, the SQL Server provider includes an execution strategy that is specifically tailored to SQL Server (including SQL Azure).</span></span> <span data-ttu-id="5a897-107">Er erkennt die Ausnahmetypen, die wiederholt werden können und sensibler Standardwerte für die maximal Anzahl von Wiederholungen, Verzögerung zwischen Wiederholungen usw. enthält.</span><span class="sxs-lookup"><span data-stu-id="5a897-107">It is aware of the exception types that can be retried and has sensible defaults for maximum retries, delay between retries, etc.</span></span>

<span data-ttu-id="5a897-108">Eine Ausführungsstrategie wird angegeben, wenn Sie die Optionen für den Kontext zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="5a897-108">An execution strategy is specified when configuring the options for your context.</span></span> <span data-ttu-id="5a897-109">Dies ist in der Regel in der `OnConfiguring` Methode von den abgeleiteten Kontext oder `Startup.cs` für eine ASP.NET Core-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="5a897-109">This is typically in the `OnConfiguring` method of your derived context, or in `Startup.cs` for an ASP.NET Core application.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a><span data-ttu-id="5a897-110">Benutzerdefinierte Ausführungsstrategie</span><span class="sxs-lookup"><span data-stu-id="5a897-110">Custom execution strategy</span></span>

<span data-ttu-id="5a897-111">Es ist ein Mechanismus zum Registrieren einer benutzerdefinierten Ausführungsstrategie Ihrer Wahl, wenn Sie die Standardwerte ändern möchten.</span><span class="sxs-lookup"><span data-stu-id="5a897-111">There is a mechanism to register a custom execution strategy of your own if you wish to change any of the defaults.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a><span data-ttu-id="5a897-112">Ausführungsstrategien und Transaktionen</span><span class="sxs-lookup"><span data-stu-id="5a897-112">Execution strategies and transactions</span></span>

<span data-ttu-id="5a897-113">Eine Ausführungsstrategie, die automatisch auf Fehler versucht werden soll, muss in der Lage, jeden Vorgang in einen wiederholungsversuchblock wiederzugeben, die nicht.</span><span class="sxs-lookup"><span data-stu-id="5a897-113">An execution strategy that automatically retries on failures needs to be able to play back each operation in an retry block that fails.</span></span> <span data-ttu-id="5a897-114">Wenn Wiederholungen aktiviert sind, wird jeder Vorgang, die Sie über EF Core ausführen eigener wiederholbar Vorgang, d. h. jede Abfrage und jeder Aufruf von `SaveChanges()` wird als eine Einheit wiederholt, wenn ein vorübergehender Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="5a897-114">When retries are enabled, each operation you perform via EF Core becomes its own retriable operation, i.e. each query and each call to `SaveChanges()` will be retried as a unit if a transient failure occurs.</span></span>

<span data-ttu-id="5a897-115">Jedoch wenn Codes initiiert eine Transaktion mit `BeginTransaction()` definieren Sie eine eigene Gruppe von Vorgängen, die als Einheit behandelt werden müssen, d. h. müssen alles innerhalb der Transaktion wiedergegeben werden soll ein Fehler auftreten.</span><span class="sxs-lookup"><span data-stu-id="5a897-115">However, if your code initiates a transaction using `BeginTransaction()` you are defining your own group of operations that need to be treated as a unit, i.e. everything inside the transaction would need to be played back shall a failure occur.</span></span> <span data-ttu-id="5a897-116">Sie erhalten eine der folgenden vergleichbare Ausnahme, wenn Sie versuchen, dieses Verfahren, wenn eine Ausführungsstrategie verwenden.</span><span class="sxs-lookup"><span data-stu-id="5a897-116">You will receive an exception like the following if you attempt to do this when using an execution strategy.</span></span>

> <span data-ttu-id="5a897-117">InvalidOperationException: Die konfigurierte Ausführungsstrategie 'SqlServerRetryingExecutionStrategy' unterstützt keine vom Benutzer initiierten Transaktionen.</span><span class="sxs-lookup"><span data-stu-id="5a897-117">InvalidOperationException: The configured execution strategy 'SqlServerRetryingExecutionStrategy' does not support user initiated transactions.</span></span> <span data-ttu-id="5a897-118">Verwenden Sie die Ausführungsstrategie "DbContext.Database.CreateExecutionStrategy()" zurückgegebene, um alle Vorgänge in der Transaktion als Einheit mit möglichem auszuführen.</span><span class="sxs-lookup"><span data-stu-id="5a897-118">Use the execution strategy returned by 'DbContext.Database.CreateExecutionStrategy()' to execute all the operations in the transaction as a retriable unit.</span></span>

<span data-ttu-id="5a897-119">Die Lösung besteht darin, manuell aufrufen Ausführungsstrategie mit einen Delegaten, die alle Elemente darstellt, die ausgeführt werden muss.</span><span class="sxs-lookup"><span data-stu-id="5a897-119">The solution is to manually invoke the execution strategy with a delegate representing everything that needs to be executed.</span></span> <span data-ttu-id="5a897-120">Um ein vorübergehender Fehler auftritt, wird die Ausführungsstrategie den Delegaten erneut aufrufen.</span><span class="sxs-lookup"><span data-stu-id="5a897-120">If a transient failure occurs, the execution strategy will invoke the delegate again.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a><span data-ttu-id="5a897-121">Fehler beim Commit der Transaktion und das Problem Idempotenz</span><span class="sxs-lookup"><span data-stu-id="5a897-121">Transaction commit failure and the idempotency issue</span></span>

<span data-ttu-id="5a897-122">Im Allgemeinen wird, wenn ein Verbindungsfehler vorhanden ist die aktuelle Transaktion ein Rollback ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="5a897-122">In general, when there is a connection failure the current transaction is rolled back.</span></span> <span data-ttu-id="5a897-123">Jedoch wenn die Verbindung getrennt wird, während die Transaktion wird wird ein Commit der resultierende Status der Transaktion ist unbekannt.</span><span class="sxs-lookup"><span data-stu-id="5a897-123">However, if the connection is dropped while the transaction is being committed the resulting state of the transaction is unknown.</span></span> <span data-ttu-id="5a897-124">Lesen Sie diese [Blogbeitrag](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) Weitere Details.</span><span class="sxs-lookup"><span data-stu-id="5a897-124">See this [blog post](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) for more details.</span></span>

<span data-ttu-id="5a897-125">Wird standardmäßig die Ausführungsstrategie wiederholt den Vorgang als ob Rollback der Transaktion, ist dies nicht der Fall Dies führt jedoch eine Ausnahme ausgelöst, wenn der neuen Datenbankstatus nicht kompatibel ist oder zur führen **datenbeschädigung** Wenn die Vorgang beruht nicht auf einem bestimmten Status, z. B. Wenn Sie eine neue Zeile mit automatisch generierten Schlüsselwerte einfügen.</span><span class="sxs-lookup"><span data-stu-id="5a897-125">By default, the execution strategy will retry the operation as if the transaction was rolled back, but if it's not the case this will result in an exception if the new database state is incompatible or could lead to **data corruption** if the operation does not rely on a particular state, for example when inserting a new row with auto-generated key values.</span></span>

<span data-ttu-id="5a897-126">Es gibt mehrere Möglichkeiten, zu diesem Zweck verfügt.</span><span class="sxs-lookup"><span data-stu-id="5a897-126">There are several ways to deal with this.</span></span>

### <a name="option-1---do-almost-nothing"></a><span data-ttu-id="5a897-127">Option 1 – nothing (fast)</span><span class="sxs-lookup"><span data-stu-id="5a897-127">Option 1 - Do (almost) nothing</span></span>

<span data-ttu-id="5a897-128">Die Wahrscheinlichkeit für einen Verbindungsfehler während des Transaktionscommits ist gering, sodass sie möglicherweise für Ihre Anwendung nur fehlschlägt, wenn tatsächlich diese Bedingung tritt ein akzeptabel.</span><span class="sxs-lookup"><span data-stu-id="5a897-128">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>

<span data-ttu-id="5a897-129">Allerdings müssen Sie zur Vermeidung des Speicher generierte Schlüssel, um sicherzustellen, dass eine Ausnahme ausgelöst wird, anstatt eine doppelte Zeile hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="5a897-129">However, you need to avoid using store-generated keys in order to ensure that an exception is thrown instead of adding a duplicate row.</span></span> <span data-ttu-id="5a897-130">Erwägen Sie eine Client-generierte GUID-Wert oder einen für die clientseitige-wertgenerator.</span><span class="sxs-lookup"><span data-stu-id="5a897-130">Consider using a client-generated GUID value or a client-side value generator.</span></span>

### <a name="option-2---rebuild-application-state"></a><span data-ttu-id="5a897-131">Option 2: Rebuild-Anwendungsstatus</span><span class="sxs-lookup"><span data-stu-id="5a897-131">Option 2 - Rebuild application state</span></span>

1. <span data-ttu-id="5a897-132">Verwerfen den aktuellen `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="5a897-132">Discard the current `DbContext`.</span></span>
2. <span data-ttu-id="5a897-133">Erstellen Sie ein neues `DbContext` und Wiederherstellen des Zustands der Anwendung aus der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="5a897-133">Create a new `DbContext` and restore the state of your application from the database.</span></span>
3. <span data-ttu-id="5a897-134">Dem Benutzer zu informieren, dass der letzte Vorgang nicht erfolgreich abgeschlossen wurden wurden möglicherweise.</span><span class="sxs-lookup"><span data-stu-id="5a897-134">Inform the user that the last operation might not have been completed successfully.</span></span>

### <a name="option-3---add-state-verification"></a><span data-ttu-id="5a897-135">Option 3 - Status-Überprüfung hinzufügen</span><span class="sxs-lookup"><span data-stu-id="5a897-135">Option 3 - Add state verification</span></span>

<span data-ttu-id="5a897-136">Für die meisten Vorgänge, die den Status einer Datenbank ändern, ist es möglich, Code hinzufügen, der überprüft, ob es erfolgreich ausgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="5a897-136">For most of the operations that change the database state it is possible to add code that checks whether it succeeded.</span></span> <span data-ttu-id="5a897-137">EF bietet eine Erweiterungsmethode zur - Vereinfachung `IExecutionStrategy.ExecuteInTransaction`.</span><span class="sxs-lookup"><span data-stu-id="5a897-137">EF provides an extension method to make this easier - `IExecutionStrategy.ExecuteInTransaction`.</span></span>

<span data-ttu-id="5a897-138">Diese Methode startet einen Commit für eine Transaktion und akzeptiert auch eine Funktion in der `verifySucceeded` Parameter, der aufgerufen wird, wenn ein vorübergehender Fehler beim Commit Transaktion auftritt.</span><span class="sxs-lookup"><span data-stu-id="5a897-138">This method begins and commits a transaction and also accepts a function in the `verifySucceeded` parameter that is invoked when a transient error occurs during the transaction commit.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> <span data-ttu-id="5a897-139">Hier `SaveChanges` mit aufgerufen wird `acceptAllChangesOnSuccess` festgelegt `false` zu vermeiden, Ändern des Status der `Blog` Entität `Unchanged` Wenn `SaveChanges` erfolgreich ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="5a897-139">Here `SaveChanges` is invoked with `acceptAllChangesOnSuccess` set to `false` to avoid changing the state of the `Blog` entity to `Unchanged` if `SaveChanges` succeeds.</span></span> <span data-ttu-id="5a897-140">Dies ermöglicht um den gleichen Vorgang zu wiederholen, wenn das Commit schlägt fehl, und die Transaktion ein Rollback.</span><span class="sxs-lookup"><span data-stu-id="5a897-140">This allows to retry the same operation if the commit fails and the transaction is rolled back.</span></span>

### <a name="option-4---manually-track-the-transaction"></a><span data-ttu-id="5a897-141">Option 4: die Transaktion manuell nachverfolgen</span><span class="sxs-lookup"><span data-stu-id="5a897-141">Option 4 - Manually track the transaction</span></span>

<span data-ttu-id="5a897-142">Wenn Sie müssen Speicher generierte Schlüssel verwenden, oder benötigen eine generische Methode der Behandlung von commitfehlern, die nicht auf den ausgeführten Vorgang abhängig konnte jede Transaktion eine ID zugewiesen, die überprüft wird, wenn dies fehlschlägt.</span><span class="sxs-lookup"><span data-stu-id="5a897-142">If you need to use store-generated keys or need a generic way of handling commit failures that doesn't depend on the operation performed each transaction could be assigned an ID that is checked when the commit fails.</span></span>

1. <span data-ttu-id="5a897-143">Fügen Sie eine Tabelle mit der Datenbank verwendet, um den Status der Transaktionen nachzuverfolgen.</span><span class="sxs-lookup"><span data-stu-id="5a897-143">Add a table to the database used to track the status of the transactions.</span></span>
2. <span data-ttu-id="5a897-144">In der Tabelle am Anfang jeder Transaktion eine Zeile eingefügt werden.</span><span class="sxs-lookup"><span data-stu-id="5a897-144">Insert a row into the table at the beginning of each transaction.</span></span>
3. <span data-ttu-id="5a897-145">Wenn die Verbindung während der Commit-fehlschlägt, überprüfen Sie das Vorhandensein der entsprechenden Zeile in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="5a897-145">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>
4. <span data-ttu-id="5a897-146">Wenn das Commit erfolgreich ausgeführt wurde, löschen Sie die entsprechende Zeile aus, um die Vergrößerung der Tabelle zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="5a897-146">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> <span data-ttu-id="5a897-147">Stellen Sie sicher, dass der Kontext für die Überprüfung verwendet eine Ausführungsstrategie definiert, wie die Verbindung wahrscheinlich erneut bei der Überprüfung fehl ist, wenn während des Transaktionscommits aufgetreten ist.</span><span class="sxs-lookup"><span data-stu-id="5a897-147">Make sure that the context used for the verification has an execution strategy defined as the connection is likely to fail again during verification if it failed during transaction commit.</span></span>
