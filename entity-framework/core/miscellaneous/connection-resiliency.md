---
title: Verbindungsresilienz – EF Core
author: rowanmiller
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: d5101d0622ddc2c90ddded16b9ec6cc4eb814c36
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283835"
---
# <a name="connection-resiliency"></a><span data-ttu-id="c7cf3-102">Verbindungsresilienz</span><span class="sxs-lookup"><span data-stu-id="c7cf3-102">Connection Resiliency</span></span>

<span data-ttu-id="c7cf3-103">Verbindungsresilienz versucht automatisch fehlerhaften Datenbankbefehle auszuführen.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-103">Connection resiliency automatically retries failed database commands.</span></span> <span data-ttu-id="c7cf3-104">Die Funktion kann mit einer beliebigen Datenbank verwendet werden, um durch Angabe einer "Ausführungsstrategie", die erforderliche Logik zum Erkennen von Fehlern aus, und wiederholen die Befehle kapselt.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-104">The feature can be used with any database by supplying an "execution strategy", which encapsulates the logic necessary to detect failures and retry commands.</span></span> <span data-ttu-id="c7cf3-105">EF Core-Anbieter können Ausführungsstrategien maßgeschneidert für ihre Datenbank fehlerbedingungen und optimale wiederholungsrichtlinien angeben.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-105">EF Core providers can supply execution strategies tailored to their specific database failure conditions and optimal retry policies.</span></span>

<span data-ttu-id="c7cf3-106">Beispielsweise enthält den SQL Server-Anbieter eine Ausführungsstrategie, die speziell auf SQL Server (einschließlich SQL Azure) zugeschnitten ist.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-106">As an example, the SQL Server provider includes an execution strategy that is specifically tailored to SQL Server (including SQL Azure).</span></span> <span data-ttu-id="c7cf3-107">Es erkennt die Ausnahmetypen, die wiederholt werden können und verfügt über sinnvolle Standardwerte für maximale Wiederholungsversuche, Verzögerung zwischen Wiederholungen usw.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-107">It is aware of the exception types that can be retried and has sensible defaults for maximum retries, delay between retries, etc.</span></span>

<span data-ttu-id="c7cf3-108">Eine Ausführungsstrategie für die wird angegeben, wenn Sie die Optionen für den Kontext zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-108">An execution strategy is specified when configuring the options for your context.</span></span> <span data-ttu-id="c7cf3-109">Dies ist in der Regel in der `OnConfiguring` Methode, die von Ihrem abgeleiteten Kontext oder `Startup.cs` für eine ASP.NET Core-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-109">This is typically in the `OnConfiguring` method of your derived context, or in `Startup.cs` for an ASP.NET Core application.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a><span data-ttu-id="c7cf3-110">Benutzerdefinierte Ausführungsstrategie</span><span class="sxs-lookup"><span data-stu-id="c7cf3-110">Custom execution strategy</span></span>

<span data-ttu-id="c7cf3-111">Es gibt ein Mechanismus zum Registrieren einer benutzerdefinierten Ausführungsstrategie selbst, wenn Sie die Standardwerte ändern möchten.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-111">There is a mechanism to register a custom execution strategy of your own if you wish to change any of the defaults.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a><span data-ttu-id="c7cf3-112">Ausführungsstrategien und Transaktionen</span><span class="sxs-lookup"><span data-stu-id="c7cf3-112">Execution strategies and transactions</span></span>

<span data-ttu-id="c7cf3-113">Eine Ausführungsstrategie, die automatisch bei Fehlern versucht werden soll, muss in der Lage, jeden Vorgang in einen wiederholungsversuchblock wiederzugeben, die nicht.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-113">An execution strategy that automatically retries on failures needs to be able to play back each operation in a retry block that fails.</span></span> <span data-ttu-id="c7cf3-114">Wenn Wiederholungen aktiviert sind, wird jeder Vorgang, die Sie über EF Core ausführen individuell wiederholbaren Vorgang.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-114">When retries are enabled, each operation you perform via EF Core becomes its own retriable operation.</span></span> <span data-ttu-id="c7cf3-115">Das heißt, jede Abfrage und jeder Aufruf von `SaveChanges()` wird als eine Einheit wiederholt, wenn ein vorübergehender Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-115">That is, each query and each call to `SaveChanges()` will be retried as a unit if a transient failure occurs.</span></span>

<span data-ttu-id="c7cf3-116">Aber wenn Ihr Code initiiert eine Transaktion mit `BeginTransaction()` definieren Sie Ihre eigene Gruppe von Vorgängen, die als Einheit behandelt werden müssen, und alles innerhalb der Transaktion wiedergegeben werden soll ein Fehler auftreten muss.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-116">However, if your code initiates a transaction using `BeginTransaction()` you are defining your own group of operations that need to be treated as a unit, and everything inside the transaction would need to be played back shall a failure occur.</span></span> <span data-ttu-id="c7cf3-117">Sie erhalten eine Ausnahme wie folgt, wenn Sie versuchen, dies zu tun, wenn Sie eine Ausführungsstrategie verwenden:</span><span class="sxs-lookup"><span data-stu-id="c7cf3-117">You will receive an exception like the following if you attempt to do this when using an execution strategy:</span></span>

> <span data-ttu-id="c7cf3-118">"InvalidOperationException": Die konfigurierte Ausführungsstrategie "SqlServerRetryingExecutionStrategy" unterstützt keine vom Benutzer initiierte Transaktionen.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-118">InvalidOperationException: The configured execution strategy 'SqlServerRetryingExecutionStrategy' does not support user initiated transactions.</span></span> <span data-ttu-id="c7cf3-119">Verwenden Sie die Ausführungsstrategie, die von „DbContext.Database.CreateExecutionStrategy()“ zurückgegeben wird, um alle Vorgänge in der Transaktion als wiederholbare Einheit auszuführen.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-119">Use the execution strategy returned by 'DbContext.Database.CreateExecutionStrategy()' to execute all the operations in the transaction as a retriable unit.</span></span>

<span data-ttu-id="c7cf3-120">Die Lösung besteht darin, manuell die Ausführungsstrategie mit einem Delegaten, der alle Komponenten darstellt aufrufen, die ausgeführt werden muss.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-120">The solution is to manually invoke the execution strategy with a delegate representing everything that needs to be executed.</span></span> <span data-ttu-id="c7cf3-121">Die Ausführungsstrategie ruft den Delegaten erneut auf, wenn ein vorübergehender Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-121">If a transient failure occurs, the execution strategy will invoke the delegate again.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a><span data-ttu-id="c7cf3-122">Fehler beim Commit der Transaktion und das Problem Idempotenz</span><span class="sxs-lookup"><span data-stu-id="c7cf3-122">Transaction commit failure and the idempotency issue</span></span>

<span data-ttu-id="c7cf3-123">Im Allgemeinen wird bei ein Verbindungsfehler die aktuelle Transaktion ein Rollback ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-123">In general, when there is a connection failure the current transaction is rolled back.</span></span> <span data-ttu-id="c7cf3-124">Allerdings, wenn die Verbindung getrennt wird, während die Transaktion wird ein Commit der resultierende Status der Transaktion ist unbekannt.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-124">However, if the connection is dropped while the transaction is being committed the resulting state of the transaction is unknown.</span></span> <span data-ttu-id="c7cf3-125">Finden Sie in diesem [Blogbeitrag](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) Weitere Details.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-125">See this [blog post](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) for more details.</span></span>

<span data-ttu-id="c7cf3-126">In der Standardeinstellung die Ausführungsstrategie wiederholt den Vorgang wurde ein Rollback für die Transaktion, wobei jedoch ist dies nicht der Fall dies zu einer Ausnahme führt, wenn der neue Datenbankstatus nicht kompatibel ist oder zu führen **datenbeschädigung** Wenn die Vorgang beruht nicht auf einem bestimmten Status, z. B. wenn eine neue Zeile mit automatisch generierter Schlüsselwerte einfügen.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-126">By default, the execution strategy will retry the operation as if the transaction was rolled back, but if it's not the case this will result in an exception if the new database state is incompatible or could lead to **data corruption** if the operation does not rely on a particular state, for example when inserting a new row with auto-generated key values.</span></span>

<span data-ttu-id="c7cf3-127">Es gibt mehrere Möglichkeiten, diese verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-127">There are several ways to deal with this.</span></span>

### <a name="option-1---do-almost-nothing"></a><span data-ttu-id="c7cf3-128">Option 1: nothing (fast)</span><span class="sxs-lookup"><span data-stu-id="c7cf3-128">Option 1 - Do (almost) nothing</span></span>

<span data-ttu-id="c7cf3-129">Die Wahrscheinlichkeit für einen Verbindungsfehler während des Transaktionscommits ist gering, sodass es für die Anwendung nur fehlschlägt, wenn diese Bedingung, tatsächlich eintritt gültig sein kann.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-129">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>

<span data-ttu-id="c7cf3-130">Allerdings müssen Sie zu vermeiden, vom Speicher generierte Schlüssel verwenden, um sicherzustellen, dass eine Ausnahme ausgelöst wird, anstatt eine Zeile mit doppelte hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-130">However, you need to avoid using store-generated keys in order to ensure that an exception is thrown instead of adding a duplicate row.</span></span> <span data-ttu-id="c7cf3-131">Sollten Sie einen Client generierter GUID-Wert oder einen für die clientseitige-wertgenerator.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-131">Consider using a client-generated GUID value or a client-side value generator.</span></span>

### <a name="option-2---rebuild-application-state"></a><span data-ttu-id="c7cf3-132">Option 2 – Rebuild-Anwendungsstatus</span><span class="sxs-lookup"><span data-stu-id="c7cf3-132">Option 2 - Rebuild application state</span></span>

1. <span data-ttu-id="c7cf3-133">Verwerfen der aktuellen `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-133">Discard the current `DbContext`.</span></span>
2. <span data-ttu-id="c7cf3-134">Erstellen Sie ein neues `DbContext` und Wiederherstellen des Zustands Ihrer Anwendung aus der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-134">Create a new `DbContext` and restore the state of your application from the database.</span></span>
3. <span data-ttu-id="c7cf3-135">Informieren Sie den Benutzer, dass der letzte Vorgang nicht erfolgreich abgeschlossen wurde haben kann.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-135">Inform the user that the last operation might not have been completed successfully.</span></span>

### <a name="option-3---add-state-verification"></a><span data-ttu-id="c7cf3-136">Option 3 - Status-Überprüfung hinzufügen</span><span class="sxs-lookup"><span data-stu-id="c7cf3-136">Option 3 - Add state verification</span></span>

<span data-ttu-id="c7cf3-137">Für den Großteil der Vorgänge, die den Zustand der Datenbank zu ändern, ist es möglich, Code hinzuzufügen, die überprüft, ob es erfolgreich war.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-137">For most of the operations that change the database state it is possible to add code that checks whether it succeeded.</span></span> <span data-ttu-id="c7cf3-138">EF bietet eine Erweiterungsmethode zur - Vereinfachung `IExecutionStrategy.ExecuteInTransaction`.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-138">EF provides an extension method to make this easier - `IExecutionStrategy.ExecuteInTransaction`.</span></span>

<span data-ttu-id="c7cf3-139">Diese Methode beginnt und führt einen Commit für eine Transaktion und akzeptiert auch eine Funktion in der `verifySucceeded` Parameter, der aufgerufen wird, wenn ein vorübergehender Fehler, während das Commit der Transaktion auftritt.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-139">This method begins and commits a transaction and also accepts a function in the `verifySucceeded` parameter that is invoked when a transient error occurs during the transaction commit.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> <span data-ttu-id="c7cf3-140">Hier `SaveChanges` wird aufgerufen, mit `acceptAllChangesOnSuccess` festgelegt `false` um zu vermeiden, Ändern des Status der `Blog` Entität `Unchanged` Wenn `SaveChanges` erfolgreich ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-140">Here `SaveChanges` is invoked with `acceptAllChangesOnSuccess` set to `false` to avoid changing the state of the `Blog` entity to `Unchanged` if `SaveChanges` succeeds.</span></span> <span data-ttu-id="c7cf3-141">Dadurch können um den gleichen Vorgang zu wiederholen, wenn der Commit schlägt fehl, und die Transaktion ein Rollback.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-141">This allows to retry the same operation if the commit fails and the transaction is rolled back.</span></span>

### <a name="option-4---manually-track-the-transaction"></a><span data-ttu-id="c7cf3-142">Option 4: die Transaktion manuell nachverfolgen</span><span class="sxs-lookup"><span data-stu-id="c7cf3-142">Option 4 - Manually track the transaction</span></span>

<span data-ttu-id="c7cf3-143">Wenn Sie müssen Speicher generierte Schlüssel oder eine generische Möglichkeit Commit Fehler zu beheben, die nicht von den ausgeführten Vorgang abhängig ist konnte jeder Transaktion eine ID zugewiesen werden, die überprüft wird, wenn der Commit schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-143">If you need to use store-generated keys or need a generic way of handling commit failures that doesn't depend on the operation performed each transaction could be assigned an ID that is checked when the commit fails.</span></span>

1. <span data-ttu-id="c7cf3-144">Fügen Sie eine Tabelle mit der Datenbank verwendet, um den Status der Transaktionen nachzuverfolgen.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-144">Add a table to the database used to track the status of the transactions.</span></span>
2. <span data-ttu-id="c7cf3-145">Eine Zeile in der Tabelle am Anfang jeder Transaktion eingefügt.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-145">Insert a row into the table at the beginning of each transaction.</span></span>
3. <span data-ttu-id="c7cf3-146">Wenn die Verbindung während des Commits fehlschlägt, überprüfen Sie das Vorhandensein der entsprechenden Zeile in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-146">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>
4. <span data-ttu-id="c7cf3-147">Wenn der Commit erfolgreich ausgeführt wird, löschen Sie die entsprechende Zeile aus, um die Vergrößerung der Tabelle zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-147">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> <span data-ttu-id="c7cf3-148">Stellen Sie sicher, dass der Kontext für die Überprüfung verwendet, eine Ausführungsstrategie definiert wurde, wie die Verbindung vermutlich erneut bei der Überprüfung fehlschlägt ist, wenn während des Transaktionscommits aufgetreten ist.</span><span class="sxs-lookup"><span data-stu-id="c7cf3-148">Make sure that the context used for the verification has an execution strategy defined as the connection is likely to fail again during verification if it failed during transaction commit.</span></span>
