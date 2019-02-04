---
title: Datenbankverbindungsresilienz und Wiederholungslogik Verbindungslogik - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
ms.openlocfilehash: 7d6aa870cc32a2b344457fbb04525a7c2c8d1c61
ms.sourcegitcommit: 159c2e9afed7745e7512730ffffaf154bcf2ff4a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/03/2019
ms.locfileid: "55668764"
---
# <a name="connection-resiliency-and-retry-logic"></a><span data-ttu-id="52e81-102">Verbindungslogik datenbankverbindungsresilienz und Wiederholungslogik</span><span class="sxs-lookup"><span data-stu-id="52e81-102">Connection resiliency and retry logic</span></span>
> [!NOTE]
> <span data-ttu-id="52e81-103">**Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="52e81-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="52e81-104">Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.</span><span class="sxs-lookup"><span data-stu-id="52e81-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="52e81-105">Anwendungen, die mit einem Datenbankserver verbinden wurden immer anfällig für Unterbrechungen der Verbindung aufgrund von Back-End-Fehlern und netzwerkinstabilität.</span><span class="sxs-lookup"><span data-stu-id="52e81-105">Applications connecting to a database server have always been vulnerable to connection breaks due to back-end failures and network instability.</span></span> <span data-ttu-id="52e81-106">Allerdings sind diese Fehler in einer LAN-basierte Umgebung arbeiten mit dedizierten Datenbankserver selten genug, dass zusätzliche Logik zur Behandlung dieser Fehler häufig nicht erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="52e81-106">However, in a LAN based environment working against dedicated database servers these errors are rare enough that extra logic to handle those failures is not often required.</span></span> <span data-ttu-id="52e81-107">Mit dem Aufstieg des Cloud-basierter Datenbankserver, z. B. Windows Azure SQL-Datenbank und Verbindungen über weniger zuverlässige Netzwerke ist es nun häufiger bei Unterbrechungen der Verbindung auftreten.</span><span class="sxs-lookup"><span data-stu-id="52e81-107">With the rise of cloud based database servers such as Windows Azure SQL Database and connections over less reliable networks it is now more common for connection breaks to occur.</span></span> <span data-ttu-id="52e81-108">Dies kann aufgrund von Verteidigungsmaßnahmen sein, die cloud-Datenbanken verwenden, um sicherzustellen, dass die Ausgewogenheit des Diensts, z. B. verbindungsdrosselung oder Instabilität im Netzwerk verursachen vorübergehende Timeouts und andere vorübergehende Fehler.</span><span class="sxs-lookup"><span data-stu-id="52e81-108">This could be due to defensive techniques that cloud databases use to ensure fairness of service, such as connection throttling, or to instability in the network causing intermittent timeouts and other transient errors.</span></span>  

<span data-ttu-id="52e81-109">Verbindungsresilienz bezieht sich auf die Möglichkeit, dass EF automatisch alle Befehle wiederholt, die fehl, weil diese Verbindung unterbrochen.</span><span class="sxs-lookup"><span data-stu-id="52e81-109">Connection Resiliency refers to the ability for EF to automatically retry any commands that fail due to these connection breaks.</span></span>  

## <a name="execution-strategies"></a><span data-ttu-id="52e81-110">Ausführungsstrategien</span><span class="sxs-lookup"><span data-stu-id="52e81-110">Execution Strategies</span></span>  

<span data-ttu-id="52e81-111">Herstellen einer erneuten Verbindung wird durch eine Implementierung der Schnittstelle IDbExecutionStrategy übernommen.</span><span class="sxs-lookup"><span data-stu-id="52e81-111">Connection retry is taken care of by an implementation of the IDbExecutionStrategy interface.</span></span> <span data-ttu-id="52e81-112">Implementierungen der IDbExecutionStrategy werden verantwortlich für das Akzeptieren eines Vorgangs und, wenn eine Ausnahme auftritt, bestimmen, ob eine Wiederholung geeignet ist, und wiederholen, wenn es sich handelt.</span><span class="sxs-lookup"><span data-stu-id="52e81-112">Implementations of the IDbExecutionStrategy will be responsible for accepting an operation and, if an exception occurs, determining if a retry is appropriate and retrying if it is.</span></span> <span data-ttu-id="52e81-113">Es gibt vier Ausführungsstrategien aus dem Lieferumfang von EF:</span><span class="sxs-lookup"><span data-stu-id="52e81-113">There are four execution strategies that ship with EF:</span></span>  

1. <span data-ttu-id="52e81-114">**DefaultExecutionStrategy**: Diese Ausführungsstrategie wird nicht erneut versucht, alle Vorgänge ist dies die Standardeinstellung für die Datenbanken als SqlServer.</span><span class="sxs-lookup"><span data-stu-id="52e81-114">**DefaultExecutionStrategy**: this execution strategy does not retry any operations, it is the default for databases other than sql server.</span></span>  
2. <span data-ttu-id="52e81-115">**DefaultSqlExecutionStrategy**: Dies ist eine interne Ausführung-Strategie, die standardmäßig verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="52e81-115">**DefaultSqlExecutionStrategy**: this is an internal execution strategy that is used by default.</span></span> <span data-ttu-id="52e81-116">Diese Strategie wird nicht erneut versucht, alle, bricht jedoch alle Ausnahmen, die möglicherweise vorübergehende, um Benutzer zu informieren, die diese resilienz von Verbindungen aktivieren möchten.</span><span class="sxs-lookup"><span data-stu-id="52e81-116">This strategy does not retry at all, however, it will wrap any exceptions that could be transient to inform users that they might want to enable connection resiliency.</span></span>  
3. <span data-ttu-id="52e81-117">**DbExecutionStrategy**: Diese Klasse als Basisklasse für andere Ausführungsstrategien, einschließlich Ihrer eigenen benutzerdefinierten geeignet ist.</span><span class="sxs-lookup"><span data-stu-id="52e81-117">**DbExecutionStrategy**: this class is suitable as a base class for other execution strategies, including your own custom ones.</span></span> <span data-ttu-id="52e81-118">Sie implementiert eine exponentielle wiederholungsrichtlinie, in dem der erste Wiederholungsversuch mit 0 (null) Verzögerung und die Verzögerung steigt exponentiell erfolgt bis die maximale Anzahl von Wiederholungsversuchen erreicht ist.</span><span class="sxs-lookup"><span data-stu-id="52e81-118">It implements an exponential retry policy, where the initial retry happens with zero delay and the delay increases exponentially until the maximum retry count is hit.</span></span> <span data-ttu-id="52e81-119">Diese Klasse verfügt über eine abstrakte ShouldRetryOn-Methode, die implementiert werden kann, in der abgeleiteten Ausführungsstrategien steuern, welche Ausnahmen wiederholt werden soll.</span><span class="sxs-lookup"><span data-stu-id="52e81-119">This class has an abstract ShouldRetryOn method that can be implemented in derived execution strategies to control which exceptions should be retried.</span></span>  
4. <span data-ttu-id="52e81-120">**"Sqlazureexecutionstrategy"**: Diese Ausführungsstrategie von DbExecutionStrategy erbt, und wiederholt auf Ausnahmen, die bekannt ist, dass beim Arbeiten mit Azure SQL-Datenbank möglicherweise vorübergehend sein.</span><span class="sxs-lookup"><span data-stu-id="52e81-120">**SqlAzureExecutionStrategy**: this execution strategy inherits from DbExecutionStrategy and will retry on exceptions that are known to be possibly transient when working with Azure SQL Database.</span></span>

> [!NOTE]
> <span data-ttu-id="52e81-121">Ausführungsstrategien 2 und 4 in der Sql Server-Anbieter, der Lieferumfang von EF, handelt es sich in der Assembly EntityFramework.SqlServer enthalten sind, und dienen zum Arbeiten mit SQL Server.</span><span class="sxs-lookup"><span data-stu-id="52e81-121">Execution strategies 2 and 4 are included in the Sql Server provider that ships with EF, which is in the EntityFramework.SqlServer assembly and are designed to work with SQL Server.</span></span>  

## <a name="enabling-an-execution-strategy"></a><span data-ttu-id="52e81-122">Aktivieren eine Ausführungsstrategie</span><span class="sxs-lookup"><span data-stu-id="52e81-122">Enabling an Execution Strategy</span></span>  

<span data-ttu-id="52e81-123">Die einfachste Möglichkeit zum Teilen von EF verwenden, eine Ausführungsstrategie für die wird mit der SetExecutionStrategy-Methode, der die ["dbconfiguration"](~/ef6/fundamentals/configuring/code-based.md) Klasse:</span><span class="sxs-lookup"><span data-stu-id="52e81-123">The easiest way to tell EF to use an execution strategy is with the SetExecutionStrategy method of the [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) class:</span></span>  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

<span data-ttu-id="52e81-124">Dieser Code weist EF die "sqlazureexecutionstrategy" beim Verbinden mit SQL Server zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="52e81-124">This code tells EF to use the SqlAzureExecutionStrategy when connecting to SQL Server.</span></span>  

## <a name="configuring-the-execution-strategy"></a><span data-ttu-id="52e81-125">Konfigurieren die Ausführungsstrategie</span><span class="sxs-lookup"><span data-stu-id="52e81-125">Configuring the Execution Strategy</span></span>  

<span data-ttu-id="52e81-126">Der Konstruktor der "sqlazureexecutionstrategy" kann zwei Parameter: "maxretrycount" und MaxDelay akzeptieren.</span><span class="sxs-lookup"><span data-stu-id="52e81-126">The constructor of SqlAzureExecutionStrategy can accept two parameters, MaxRetryCount and MaxDelay.</span></span> <span data-ttu-id="52e81-127">MaxRetry-Anzahl ist die maximale Anzahl von Wiederholungen, die die Strategie wiederholt wird.</span><span class="sxs-lookup"><span data-stu-id="52e81-127">MaxRetry count is the maximum number of times that the strategy will retry.</span></span> <span data-ttu-id="52e81-128">Die MaxDelay ist ein TimeSpan-Objekt, das die maximale Verzögerung zwischen Wiederholungen, die die Ausführungsstrategie verwenden darstellt.</span><span class="sxs-lookup"><span data-stu-id="52e81-128">The MaxDelay is a TimeSpan representing the maximum delay between retries that the execution strategy will use.</span></span>  

<span data-ttu-id="52e81-129">Um die maximale Anzahl von Wiederholungen auf 1 und die maximale Verzögerung beträgt 30 Sekunden festlegen sollen Execue Folgendes:</span><span class="sxs-lookup"><span data-stu-id="52e81-129">To set the maximum number of retries to 1 and the maximum delay to 30 seconds you would execue the following:</span></span>  

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

<span data-ttu-id="52e81-130">Die "sqlazureexecutionstrategy" Vorgang wird wiederholt, sofort beim ersten ein vorübergehender Fehler auftritt, aber mehr zwischen den einzelnen Wiederholungsversuchen, bis die maximale Anzahl Wiederholungslimit verzögert überschritten wird, oder die Gesamtzeit, die maximale Verzögerung erreicht.</span><span class="sxs-lookup"><span data-stu-id="52e81-130">The SqlAzureExecutionStrategy will retry instantly the first time a transient failure occurs, but will delay longer between each retry until either the max retry limit is exceeded or the total time hits the max delay.</span></span>  

<span data-ttu-id="52e81-131">Die Ausführungsstrategien werden nur eine begrenzte Anzahl von Ausnahmen, die in der Regel Tansient wiederholen, müssen Sie weiterhin, behandeln andere Fehler sowie das Abfangen der Ausnahme RetryLimitExceeded für den Fall, in denen ein Fehler ist nicht vorübergehend oder dauert zu lange, Auflösen sich selbst.</span><span class="sxs-lookup"><span data-stu-id="52e81-131">The execution strategies will only retry a limited number of exceptions that are usually tansient, you will still need to handle other errors as well as catching the RetryLimitExceeded exception for the case where an error is not transient or takes too long to resolve itself.</span></span>  

<span data-ttu-id="52e81-132">Es gibt einige bekannte Einschränkungen auf, wenn eine Wiederholung Ausführungsstrategie verwenden:</span><span class="sxs-lookup"><span data-stu-id="52e81-132">There are some known of limitations when using a retrying execution strategy:</span></span>  

## <a name="streaming-queries-are-not-supported"></a><span data-ttu-id="52e81-133">Streamingabfragen werden nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="52e81-133">Streaming queries are not supported</span></span>  

<span data-ttu-id="52e81-134">Standardmäßig werden EF 6 und höher Abfrageergebnisse, anstatt sie streaming Puffern.</span><span class="sxs-lookup"><span data-stu-id="52e81-134">By default, EF6 and later version will buffer query results rather than streaming them.</span></span> <span data-ttu-id="52e81-135">Wenn Sie möchten Ergebnisse gestreamt, Sie können die AsStreaming-Methode um eine LINQ to Entities-Abfrage, um streaming zu ändern.</span><span class="sxs-lookup"><span data-stu-id="52e81-135">If you want to have results streamed you can use the AsStreaming method to change a LINQ to Entities query to streaming.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

<span data-ttu-id="52e81-136">Streaming wird nicht unterstützt, wenn eine Wiederholung Ausführungsstrategie registriert wird.</span><span class="sxs-lookup"><span data-stu-id="52e81-136">Streaming is not supported when a retrying execution strategy is registered.</span></span> <span data-ttu-id="52e81-137">Diese Einschränkung ist vorhanden, da die Verbindung Teil Weg durch die zurückgegebenen Ergebnisse löschen.</span><span class="sxs-lookup"><span data-stu-id="52e81-137">This limitation exists because the connection could drop part way through the results being returned.</span></span> <span data-ttu-id="52e81-138">In diesem Fall EF benötigt die gesamte Abfrage erneut ausführen, aber keine zuverlässige Möglichkeit, zu wissen, welche Ergebnisse zurückgegeben wurden (die Daten möglicherweise wurden geändert, da die ursprüngliche Abfrage gesendet wurde, Ergebnisse in einer anderen Reihenfolge zurückkehren können, Ergebnisse möglicherweise keinen eindeutigen Bezeichner usw..).</span><span class="sxs-lookup"><span data-stu-id="52e81-138">When this occurs, EF needs to re-run the entire query but has no reliable way of knowing which results have already been returned (data may have changed since the initial query was sent, results may come back in a different order, results may not have a unique identifier, etc.).</span></span>  

## <a name="user-initiated-transactions-are-not-supported"></a><span data-ttu-id="52e81-139">Vom Benutzer initiierte Transaktionen werden nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="52e81-139">User initiated transactions are not supported</span></span>  

<span data-ttu-id="52e81-140">Wenn Sie eine Ausführungsstrategie, die zu Wiederholungen konfiguriert haben, gibt es einige Einschränkungen für die Verwendung von Transaktionen.</span><span class="sxs-lookup"><span data-stu-id="52e81-140">When you have configured an execution strategy that results in retries, there are some limitations around the use of transactions.</span></span>  

<span data-ttu-id="52e81-141">Standardmäßig wird EF datenbankaktualisierungen innerhalb einer Transaktion ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="52e81-141">By default, EF will perform any database updates within a transaction.</span></span> <span data-ttu-id="52e81-142">Sie müssen nichts tun, um dies zu ermöglichen, EF immer führt dies automatisch aus.</span><span class="sxs-lookup"><span data-stu-id="52e81-142">You don’t need to do anything to enable this, EF always does this automatically.</span></span>  

<span data-ttu-id="52e81-143">Beispielsweise wird im folgenden Code "SaveChanges" automatisch innerhalb einer Transaktion ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="52e81-143">For example, in the following code SaveChanges is automatically performed within a transaction.</span></span> <span data-ttu-id="52e81-144">Wenn "SaveChanges" schlagen fehl, nachdem eines der neuen Website einfügen, und klicken Sie dann die Transaktion ein Rollback ausgeführt werden sollen und keine Änderungen auf die Datenbank angewendet wurden.</span><span class="sxs-lookup"><span data-stu-id="52e81-144">If SaveChanges were to fail after inserting one of the new Site’s then the transaction would be rolled back and no changes applied to the database.</span></span> <span data-ttu-id="52e81-145">Der Kontext bleibt auch in einem Zustand, die "SaveChanges", um wiederholen, Anwenden der Änderungen erneut aufgerufen werden können.</span><span class="sxs-lookup"><span data-stu-id="52e81-145">The context is also left in a state that allows SaveChanges to be called again to retry applying the changes.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

<span data-ttu-id="52e81-146">Wenn Sie keine Wiederholung Ausführungsstrategie verwenden, können Sie mehrere Vorgänge in einer einzelnen Transaktion umschließen.</span><span class="sxs-lookup"><span data-stu-id="52e81-146">When not using a retrying execution strategy you can wrap multiple operations in a single transaction.</span></span> <span data-ttu-id="52e81-147">Beispielsweise umschließt der folgende Code zwei "SaveChanges" aufrufen, in einer einzelnen Transaktion.</span><span class="sxs-lookup"><span data-stu-id="52e81-147">For example, the following code wraps two SaveChanges calls in a single transaction.</span></span> <span data-ttu-id="52e81-148">Wenn keine der Änderungen klicken Sie dann einen beliebigen Teil entweder Vorgang fehlschlägt, werden angewendet.</span><span class="sxs-lookup"><span data-stu-id="52e81-148">If any part of either operation fails then none of the changes are applied.</span></span>  

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

<span data-ttu-id="52e81-149">Dies wird nicht unterstützt, wenn eine Wiederholung Ausführungsstrategie verwenden, da EF über aller vorherigen Vorgänge und die Vorgehensweise beim Wiederholen Sie diese nicht.</span><span class="sxs-lookup"><span data-stu-id="52e81-149">This is not supported when using a retrying execution strategy because EF isn’t aware of any previous operations and how to retry them.</span></span> <span data-ttu-id="52e81-150">Beispielsweise, wenn die zweite "SaveChanges" EF nicht mehr danach nicht hat die erforderliche Informationen zum Wiederholen des ersten Aufrufs von "SaveChanges".</span><span class="sxs-lookup"><span data-stu-id="52e81-150">For example, if the second SaveChanges failed then EF no longer has the required information to retry the first SaveChanges call.</span></span>  

### <a name="workaround-suspend-execution-strategy"></a><span data-ttu-id="52e81-151">Problemumgehung: Ausführungsstrategie anhalten</span><span class="sxs-lookup"><span data-stu-id="52e81-151">Workaround: Suspend Execution Strategy</span></span>  

<span data-ttu-id="52e81-152">Eine mögliche Lösung besteht, die wiederholt Ausführungsstrategie für den Codeabschnitt anzuhalten, die ein Benutzer verwenden, muss die Transaktion initiiert.</span><span class="sxs-lookup"><span data-stu-id="52e81-152">One possible workaround is to suspend the retrying execution strategy for the piece of code that needs to use a user initiated transaction.</span></span> <span data-ttu-id="52e81-153">Die einfachste Möglichkeit hierzu ist ein SuspendExecutionStrategy-Flag, um Ihren Code basierte Configuration-Klasse und ändern die Ausführung Strategie Lambda, um die Standardeinstellung (nicht retying) Ausführungsstrategie zurückzugeben, wenn das Flag festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="52e81-153">The easiest way to do this is to add a SuspendExecutionStrategy flag to your code based configuration class and change the execution strategy lambda to return the default (non-retying) execution strategy when the flag is set.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;
using System.Runtime.Remoting.Messaging;

namespace Demo
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            this.SetExecutionStrategy("System.Data.SqlClient", () => SuspendExecutionStrategy
              ? (IDbExecutionStrategy)new DefaultExecutionStrategy()
              : new SqlAzureExecutionStrategy());
        }

        public static bool SuspendExecutionStrategy
        {
            get
            {
                return (bool?)CallContext.LogicalGetData("SuspendExecutionStrategy") ?? false;
            }
            set
            {
                CallContext.LogicalSetData("SuspendExecutionStrategy", value);
            }
        }
    }
}
```  

<span data-ttu-id="52e81-154">Beachten Sie, dass wir CallContext verwenden, um den Flagwert zu speichern.</span><span class="sxs-lookup"><span data-stu-id="52e81-154">Note that we are using CallContext to store the flag value.</span></span> <span data-ttu-id="52e81-155">Dies bietet ähnliche Funktionen wie der threadlokale Speicher ist jedoch problemlos mit asynchronem Code – einschließlich Async-Abfrage verwenden, und speichern Sie mit Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="52e81-155">This provides similar functionality to thread local storage but is safe to use with asynchronous code - including async query and save with Entity Framework.</span></span>  

<span data-ttu-id="52e81-156">Wir können jetzt die Ausführungsstrategie für den Codeabschnitt anhalten, die eine vom Benutzer initiierten Transaktion verwendet.</span><span class="sxs-lookup"><span data-stu-id="52e81-156">We can now suspend the execution strategy for the section of code that uses a user initiated transaction.</span></span>  

``` csharp
using (var db = new BloggingContext())
{
    MyConfiguration.SuspendExecutionStrategy = true;

    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }

    MyConfiguration.SuspendExecutionStrategy = false;
}
```  

### <a name="workaround-manually-call-execution-strategy"></a><span data-ttu-id="52e81-157">Problemumgehung: Rufen Sie die Ausführungsstrategie manuell</span><span class="sxs-lookup"><span data-stu-id="52e81-157">Workaround: Manually Call Execution Strategy</span></span>  

<span data-ttu-id="52e81-158">Eine weitere Möglichkeit ist die Ausführungsstrategie verwenden manuell, und geben sie den gesamten Satz von Logik ausgeführt werden, damit sie alles, was wiederholt werden kann, wenn ein Vorgang fehlschlägt.</span><span class="sxs-lookup"><span data-stu-id="52e81-158">Another option is to manually use the execution strategy and give it the entire set of logic to be run, so that it can retry everything if one of the operations fails.</span></span> <span data-ttu-id="52e81-159">Wir benötigen die Ausführungsstrategie - mithilfe der Technik anhalten oberhalb - angezeigt wird, sodass alle Kontexte, in der wiederholbare Codeblock verwendet nicht versuchen, wiederholen Sie dann.</span><span class="sxs-lookup"><span data-stu-id="52e81-159">We still need to suspend the execution strategy - using the technique shown above - so that any contexts used inside the retryable code block do not attempt to retry.</span></span>  

<span data-ttu-id="52e81-160">Beachten Sie, dass Kontexte erstellt werden soll, in den Codeblock wiederholt werden.</span><span class="sxs-lookup"><span data-stu-id="52e81-160">Note that any contexts should be constructed within the code block to be retried.</span></span> <span data-ttu-id="52e81-161">Dadurch wird sichergestellt, dass wir mit einem sauberen Status für jeden Wiederholungsversuch gestartet werden.</span><span class="sxs-lookup"><span data-stu-id="52e81-161">This ensures that we are starting with a clean state for each retry.</span></span>  

``` csharp
var executionStrategy = new SqlAzureExecutionStrategy();

MyConfiguration.SuspendExecutionStrategy = true;

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

MyConfiguration.SuspendExecutionStrategy = false;
```  
