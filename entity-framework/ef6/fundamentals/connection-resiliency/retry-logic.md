---
title: Verbindungsresilienz und Wiederholungs Logik EF6
author: AndriySvyryd
ms.date: 11/20/2019
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
ms.openlocfilehash: 50e65bed32d0cfcf42746da0d632f9e990424b97
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2020
ms.locfileid: "79402108"
---
# <a name="connection-resiliency-and-retry-logic"></a><span data-ttu-id="c6016-102">Verbindungsresilienz und Wiederholungs Logik</span><span class="sxs-lookup"><span data-stu-id="c6016-102">Connection resiliency and retry logic</span></span>
> [!NOTE]
> <span data-ttu-id="c6016-103">**Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="c6016-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="c6016-104">Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.</span><span class="sxs-lookup"><span data-stu-id="c6016-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="c6016-105">Anwendungen, die eine Verbindung mit einem Datenbankserver herstellen, waren aufgrund von Back-End-Fehlern und Netzwerk Instabilität immer anfällig für Verbindungsunterbrechungen.</span><span class="sxs-lookup"><span data-stu-id="c6016-105">Applications connecting to a database server have always been vulnerable to connection breaks due to back-end failures and network instability.</span></span> <span data-ttu-id="c6016-106">In einer LAN-basierten Umgebung, die mit dedizierten Datenbankservern arbeitet, sind diese Fehler jedoch selten genug, dass die zusätzliche Logik zur Behandlung dieser Fehler nicht häufig erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="c6016-106">However, in a LAN based environment working against dedicated database servers these errors are rare enough that extra logic to handle those failures is not often required.</span></span> <span data-ttu-id="c6016-107">Mit dem Anstieg von cloudbasierten Datenbankservern wie Windows Azure SQL-Datenbank und Verbindungen über weniger zuverlässige Netzwerke kommt es nun häufiger vor, dass Verbindungsunterbrechungen auftreten.</span><span class="sxs-lookup"><span data-stu-id="c6016-107">With the rise of cloud based database servers such as Windows Azure SQL Database and connections over less reliable networks it is now more common for connection breaks to occur.</span></span> <span data-ttu-id="c6016-108">Dies liegt möglicherweise an der Abwehr von Techniken, die clouddatenbanken verwenden, um die Fairness des Diensts zu gewährleisten, z. b. die Verbindungs Drosselung oder eine Instabilität im Netzwerk, die vorübergehende Timeouts und andere vorübergehende Fehler verursacht.</span><span class="sxs-lookup"><span data-stu-id="c6016-108">This could be due to defensive techniques that cloud databases use to ensure fairness of service, such as connection throttling, or to instability in the network causing intermittent timeouts and other transient errors.</span></span>  

<span data-ttu-id="c6016-109">Verbindungsresilienz bezieht sich auf die Fähigkeit von EF, Befehle, die aufgrund dieser Verbindungsunterbrechungen fehlschlagen, automatisch zu wiederholen.</span><span class="sxs-lookup"><span data-stu-id="c6016-109">Connection Resiliency refers to the ability for EF to automatically retry any commands that fail due to these connection breaks.</span></span>  

## <a name="execution-strategies"></a><span data-ttu-id="c6016-110">Ausführungs Strategien</span><span class="sxs-lookup"><span data-stu-id="c6016-110">Execution Strategies</span></span>  

<span data-ttu-id="c6016-111">Die Verbindungs Wiederholung erfolgt durch eine Implementierung der idbexecutionstrategy-Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="c6016-111">Connection retry is taken care of by an implementation of the IDbExecutionStrategy interface.</span></span> <span data-ttu-id="c6016-112">Implementierungen der idbexecutionstrategy-Strategie sind dafür verantwortlich, einen Vorgang zu akzeptieren und, wenn eine Ausnahme auftritt, festzustellen, ob eine Wiederholung angemessen ist, und den Vorgang zu wiederholen, wenn dies der Fall ist.</span><span class="sxs-lookup"><span data-stu-id="c6016-112">Implementations of the IDbExecutionStrategy will be responsible for accepting an operation and, if an exception occurs, determining if a retry is appropriate and retrying if it is.</span></span> <span data-ttu-id="c6016-113">Es gibt vier Ausführungs Strategien, die mit EF ausgeliefert werden:</span><span class="sxs-lookup"><span data-stu-id="c6016-113">There are four execution strategies that ship with EF:</span></span>  

1. <span data-ttu-id="c6016-114">**Defaultexecutionstrategy**: Diese Ausführungs Strategie führt keinen Wiederholungsversuch für Vorgänge aus. Dies ist die Standardeinstellung für andere Datenbanken als SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c6016-114">**DefaultExecutionStrategy**: this execution strategy does not retry any operations, it is the default for databases other than sql server.</span></span>  
2. <span data-ttu-id="c6016-115">**Defaultionqlexecutionstrategy**: Dies ist eine interne Ausführungs Strategie, die standardmäßig verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="c6016-115">**DefaultSqlExecutionStrategy**: this is an internal execution strategy that is used by default.</span></span> <span data-ttu-id="c6016-116">Diese Strategie führt nicht zu einem erneuten Versuch, sondern schließt alle Ausnahmen ein, die vorübergehend sein könnten, um Benutzer darüber zu informieren, dass Sie möglicherweise die verbindungsresilienz aktivieren möchten.</span><span class="sxs-lookup"><span data-stu-id="c6016-116">This strategy does not retry at all, however, it will wrap any exceptions that could be transient to inform users that they might want to enable connection resiliency.</span></span>  
3. <span data-ttu-id="c6016-117">**Dbexecutionstrategy**: Diese Klasse eignet sich als Basisklasse für andere Ausführungs Strategien, einschließlich ihrer eigenen benutzerdefinierten.</span><span class="sxs-lookup"><span data-stu-id="c6016-117">**DbExecutionStrategy**: this class is suitable as a base class for other execution strategies, including your own custom ones.</span></span> <span data-ttu-id="c6016-118">Es implementiert eine exponentielle Wiederholungs Richtlinie, bei der der anfängliche Wiederholungsversuch mit einer Verzögerung von 0 (null) erfolgt, und die Verzögerung erhöht sich exponentiell, bis die maximale Anzahl der Wiederholungs Versuche übertrifft.</span><span class="sxs-lookup"><span data-stu-id="c6016-118">It implements an exponential retry policy, where the initial retry happens with zero delay and the delay increases exponentially until the maximum retry count is hit.</span></span> <span data-ttu-id="c6016-119">Diese Klasse verfügt über eine abstrakte Methode "schuldretryon", die in abgeleiteten Ausführungs Strategien implementiert werden kann, um zu steuern, welche Ausnahmen wiederholt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="c6016-119">This class has an abstract ShouldRetryOn method that can be implemented in derived execution strategies to control which exceptions should be retried.</span></span>  
4. <span data-ttu-id="c6016-120">**Sqlazureexecutionstrategy**: Diese Ausführungs Strategie erbt von dbexecutionstrategy und führt einen Wiederholungsversuch für Ausnahmen aus, die bekanntermaßen bei der Arbeit mit Azure SQL-Datenbank vorübergehend sind.</span><span class="sxs-lookup"><span data-stu-id="c6016-120">**SqlAzureExecutionStrategy**: this execution strategy inherits from DbExecutionStrategy and will retry on exceptions that are known to be possibly transient when working with Azure SQL Database.</span></span>

> [!NOTE]
> <span data-ttu-id="c6016-121">Die Ausführungs Strategien 2 und 4 sind im SQL Server-Anbieter enthalten, der im Lieferumfang von EF enthalten ist, das sich in der EntityFramework. SqlServer-Assembly befindet und für die Arbeit mit SQL Server konzipiert ist.</span><span class="sxs-lookup"><span data-stu-id="c6016-121">Execution strategies 2 and 4 are included in the Sql Server provider that ships with EF, which is in the EntityFramework.SqlServer assembly and are designed to work with SQL Server.</span></span>  

## <a name="enabling-an-execution-strategy"></a><span data-ttu-id="c6016-122">Aktivieren einer Ausführungs Strategie</span><span class="sxs-lookup"><span data-stu-id="c6016-122">Enabling an Execution Strategy</span></span>  

<span data-ttu-id="c6016-123">Die einfachste Möglichkeit, EF die Verwendung einer Ausführungs Strategie mitzuteilen, ist die Methode "" der Methode "" der Klasse " [dbconfiguration](~/ef6/fundamentals/configuring/code-based.md) ":</span><span class="sxs-lookup"><span data-stu-id="c6016-123">The easiest way to tell EF to use an execution strategy is with the SetExecutionStrategy method of the [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) class:</span></span>  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

<span data-ttu-id="c6016-124">Dieser Code weist EF an, beim Herstellen einer Verbindung mit SQL Server sqlazureexecutionstrategy zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="c6016-124">This code tells EF to use the SqlAzureExecutionStrategy when connecting to SQL Server.</span></span>  

## <a name="configuring-the-execution-strategy"></a><span data-ttu-id="c6016-125">Konfigurieren der Ausführungs Strategie</span><span class="sxs-lookup"><span data-stu-id="c6016-125">Configuring the Execution Strategy</span></span>  

<span data-ttu-id="c6016-126">Der Konstruktor von sqlazureexecutionstrategy kann zwei Parameter akzeptieren: "maxRetryCount" und "MaxDelay".</span><span class="sxs-lookup"><span data-stu-id="c6016-126">The constructor of SqlAzureExecutionStrategy can accept two parameters, MaxRetryCount and MaxDelay.</span></span> <span data-ttu-id="c6016-127">Die Anzahl der maxretry-Werte ist die maximale Anzahl von Wiederholungs versuchen für die Strategie.</span><span class="sxs-lookup"><span data-stu-id="c6016-127">MaxRetry count is the maximum number of times that the strategy will retry.</span></span> <span data-ttu-id="c6016-128">MaxDelay ist ein TimeSpan-Wert, der die maximale Verzögerung zwischen Wiederholungs versuchen darstellt, die von der Ausführungs Strategie verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="c6016-128">The MaxDelay is a TimeSpan representing the maximum delay between retries that the execution strategy will use.</span></span>  

<span data-ttu-id="c6016-129">Führen Sie die folgenden Schritte aus, um die maximale Anzahl von Wiederholungen auf 1 und die maximale Verzögerung auf 30 Sekunden festzulegen:</span><span class="sxs-lookup"><span data-stu-id="c6016-129">To set the maximum number of retries to 1 and the maximum delay to 30 seconds you would execute the following:</span></span>  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy(
            "System.Data.SqlClient",
            () => new SqlAzureExecutionStrategy(1, TimeSpan.FromSeconds(30)));
    }
}
```  

<span data-ttu-id="c6016-130">Sqlazureexecutionstrategy wird beim ersten Auftreten eines vorübergehenden Fehlers sofort wiederholt, verzögert sich jedoch zwischen den einzelnen Wiederholungen, bis entweder die maximale Wiederholungs Grenze überschritten wird oder die Gesamtzeit die maximale Verzögerung erreicht.</span><span class="sxs-lookup"><span data-stu-id="c6016-130">The SqlAzureExecutionStrategy will retry instantly the first time a transient failure occurs, but will delay longer between each retry until either the max retry limit is exceeded or the total time hits the max delay.</span></span>  

<span data-ttu-id="c6016-131">Bei den Ausführungs Strategien wird nur eine begrenzte Anzahl von Ausnahmen wiederholt, die normalerweise vorübergehend sind. Sie müssen weiterhin andere Fehler behandeln und die Ausnahme "retrylimitexceging" abfangen, wenn ein Fehler nicht vorübergehend ist oder zu lange für die Auflösung benötigt wird. etabliert.</span><span class="sxs-lookup"><span data-stu-id="c6016-131">The execution strategies will only retry a limited number of exceptions that are usually transient, you will still need to handle other errors as well as catching the RetryLimitExceeded exception for the case where an error is not transient or takes too long to resolve itself.</span></span>  

<span data-ttu-id="c6016-132">Bei der Verwendung einer Wiederholungs Ausführungs Strategie gibt es einige bekannte Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="c6016-132">There are some known of limitations when using a retrying execution strategy:</span></span>  

## <a name="streaming-queries-are-not-supported"></a><span data-ttu-id="c6016-133">Streaming-Abfragen werden nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="c6016-133">Streaming queries are not supported</span></span>  

<span data-ttu-id="c6016-134">Standardmäßig puffert EF6 und höhere Versionen Abfrageergebnisse, anstatt Sie zu streamen.</span><span class="sxs-lookup"><span data-stu-id="c6016-134">By default, EF6 and later version will buffer query results rather than streaming them.</span></span> <span data-ttu-id="c6016-135">Wenn die Ergebnisse gestreamt werden sollen, können Sie die asstreaming-Methode verwenden, um eine LINQ to Entities Abfrage in das Streaming zu ändern.</span><span class="sxs-lookup"><span data-stu-id="c6016-135">If you want to have results streamed you can use the AsStreaming method to change a LINQ to Entities query to streaming.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

<span data-ttu-id="c6016-136">Streaming wird nicht unterstützt, wenn eine Wiederholungs Ausführungs Strategie registriert wird.</span><span class="sxs-lookup"><span data-stu-id="c6016-136">Streaming is not supported when a retrying execution strategy is registered.</span></span> <span data-ttu-id="c6016-137">Diese Einschränkung ist vorhanden, da die Verbindung die Ergebnisse, die zurückgegeben werden, auf dem Weg ablegen könnte.</span><span class="sxs-lookup"><span data-stu-id="c6016-137">This limitation exists because the connection could drop part way through the results being returned.</span></span> <span data-ttu-id="c6016-138">In diesem Fall muss EF die gesamte Abfrage erneut ausführen, kann jedoch nicht zuverlässig festzustellen, welche Ergebnisse bereits zurückgegeben wurden (die Daten haben sich möglicherweise seit dem Senden der ersten Abfrage geändert, Ergebnisse können in einer anderen Reihenfolge zurückgegeben werden, und die Ergebnisse haben möglicherweise keinen eindeutigen Bezeichner. , usw.).</span><span class="sxs-lookup"><span data-stu-id="c6016-138">When this occurs, EF needs to re-run the entire query but has no reliable way of knowing which results have already been returned (data may have changed since the initial query was sent, results may come back in a different order, results may not have a unique identifier, etc.).</span></span>  

## <a name="user-initiated-transactions-are-not-supported"></a><span data-ttu-id="c6016-139">Vom Benutzer initiierte Transaktionen werden nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="c6016-139">User initiated transactions are not supported</span></span>  

<span data-ttu-id="c6016-140">Wenn Sie eine Ausführungs Strategie konfiguriert haben, die zu Wiederholungen führt, gibt es einige Einschränkungen bei der Verwendung von Transaktionen.</span><span class="sxs-lookup"><span data-stu-id="c6016-140">When you have configured an execution strategy that results in retries, there are some limitations around the use of transactions.</span></span>  

<span data-ttu-id="c6016-141">Standardmäßig führt EF innerhalb einer Transaktion alle Datenbankupdates aus.</span><span class="sxs-lookup"><span data-stu-id="c6016-141">By default, EF will perform any database updates within a transaction.</span></span> <span data-ttu-id="c6016-142">Sie müssen nichts tun, um dies zu ermöglichen. EF führt dies immer automatisch durch.</span><span class="sxs-lookup"><span data-stu-id="c6016-142">You don’t need to do anything to enable this, EF always does this automatically.</span></span>  

<span data-ttu-id="c6016-143">Im folgenden Code wird z. b. "SaveChanges" automatisch innerhalb einer Transaktion ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="c6016-143">For example, in the following code SaveChanges is automatically performed within a transaction.</span></span> <span data-ttu-id="c6016-144">Wenn SaveChanges nach dem Einfügen einer der neuen Standorte fehlschlagen würde, wird für die Transaktion ein Rollback ausgeführt, und es werden keine Änderungen auf die Datenbank angewendet.</span><span class="sxs-lookup"><span data-stu-id="c6016-144">If SaveChanges were to fail after inserting one of the new Site’s then the transaction would be rolled back and no changes applied to the database.</span></span> <span data-ttu-id="c6016-145">Außerdem bleibt der Kontext in einem Zustand, in dem SaveChanges erneut aufgerufen werden kann, um die Änderungen zu übernehmen.</span><span class="sxs-lookup"><span data-stu-id="c6016-145">The context is also left in a state that allows SaveChanges to be called again to retry applying the changes.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

<span data-ttu-id="c6016-146">Wenn keine Wiederholungs Ausführungs Strategie verwendet wird, können Sie mehrere Vorgänge in einer einzelnen Transaktion umschließen.</span><span class="sxs-lookup"><span data-stu-id="c6016-146">When not using a retrying execution strategy you can wrap multiple operations in a single transaction.</span></span> <span data-ttu-id="c6016-147">Der folgende Code umschließt z. b. zwei SaveChanges-Aufrufe in einer einzelnen Transaktion.</span><span class="sxs-lookup"><span data-stu-id="c6016-147">For example, the following code wraps two SaveChanges calls in a single transaction.</span></span> <span data-ttu-id="c6016-148">Wenn ein Teil eines der beiden Vorgänge fehlschlägt, wird keine der Änderungen angewendet.</span><span class="sxs-lookup"><span data-stu-id="c6016-148">If any part of either operation fails then none of the changes are applied.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Site { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }
}
```  

<span data-ttu-id="c6016-149">Dies wird nicht unterstützt, wenn eine Wiederholungs Ausführungs Strategie verwendet wird, da EF keine früheren Vorgänge kennt und die Wiederholungs Versuche ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="c6016-149">This is not supported when using a retrying execution strategy because EF isn’t aware of any previous operations and how to retry them.</span></span> <span data-ttu-id="c6016-150">Wenn beispielsweise die zweite SaveChanges fehlgeschlagen ist, verfügt EF nicht mehr über die erforderlichen Informationen, um den ersten SaveChanges-Befehl zu wiederholen.</span><span class="sxs-lookup"><span data-stu-id="c6016-150">For example, if the second SaveChanges failed then EF no longer has the required information to retry the first SaveChanges call.</span></span>  

### <a name="solution-manually-call-execution-strategy"></a><span data-ttu-id="c6016-151">Lösung: Manuelles Abrufen der Ausführungs Strategie</span><span class="sxs-lookup"><span data-stu-id="c6016-151">Solution: Manually Call Execution Strategy</span></span>  

<span data-ttu-id="c6016-152">Die Lösung besteht darin, die Ausführungs Strategie manuell zu verwenden und ihr den gesamten Logik Satz zuzuweisen, der ausgeführt werden soll, damit Sie alles wiederholen kann, wenn einer der Vorgänge fehlschlägt.</span><span class="sxs-lookup"><span data-stu-id="c6016-152">The solution is to manually use the execution strategy and give it the entire set of logic to be run, so that it can retry everything if one of the operations fails.</span></span> <span data-ttu-id="c6016-153">Wenn eine von dbexecutionstrategy abgeleitete Ausführungs Strategie ausgeführt wird, wird die in SaveChanges verwendete implizite Ausführungs Strategie angehalten.</span><span class="sxs-lookup"><span data-stu-id="c6016-153">When an execution strategy derived from DbExecutionStrategy is running it will suspend the implicit execution strategy used in SaveChanges.</span></span>  

<span data-ttu-id="c6016-154">Beachten Sie, dass alle Kontexte innerhalb des Codeblocks erstellt werden müssen, um erneut versucht werden zu können.</span><span class="sxs-lookup"><span data-stu-id="c6016-154">Note that any contexts should be constructed within the code block to be retried.</span></span> <span data-ttu-id="c6016-155">Dadurch wird sichergestellt, dass für jeden Wiederholungsversuch ein sauberer Zustand gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="c6016-155">This ensures that we are starting with a clean state for each retry.</span></span>  

``` csharp
var executionStrategy = new SqlAzureExecutionStrategy();

executionStrategy.Execute(
    () =>
    {
        using (var db = new BloggingContext())
        {
            using (var trn = db.Database.BeginTransaction())
            {
                db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                db.SaveChanges();

                db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
                db.SaveChanges();

                trn.Commit();
            }
        }
    });
```  
