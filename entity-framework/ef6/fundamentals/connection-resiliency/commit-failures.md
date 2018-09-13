---
title: Behandlung von Fehlern für den Commit Transaction - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
ms.openlocfilehash: 71d5649dd993bb95e24165a55d812c71a37f03f3
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489387"
---
# <a name="handling-transaction-commit-failures"></a><span data-ttu-id="eecd0-102">Behandeln Fehler bei commit</span><span class="sxs-lookup"><span data-stu-id="eecd0-102">Handling transaction commit failures</span></span>
> [!NOTE]
> <span data-ttu-id="eecd0-103">**EF6.1 oder höher, nur** -APIs, die Funktionen erläutert, die auf dieser Seite usw. in Entity Framework 6.1 eingeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="eecd0-103">**EF6.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="eecd0-104">Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.</span><span class="sxs-lookup"><span data-stu-id="eecd0-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="eecd0-105">Als Teil des 6.1 werden wir eine neue verbindungsstabilitätsfeature für EF eingeführt: die Möglichkeit zum Erkennen und automatisch wiederhergestellt, vorübergehenden verbindungsausfällen führen in der Bestätigung der Transaktion ein Commit ausgeführt haben.</span><span class="sxs-lookup"><span data-stu-id="eecd0-105">As part of 6.1 we are introducing a new connection resiliency feature for EF: the ability to detect and recover automatically when transient connection failures affect the acknowledgement of transaction commits.</span></span> <span data-ttu-id="eecd0-106">Die vollständigen Details des Szenarios sind am besten im Blogbeitrag beschrieben [SQL-Datenbankverbindungen und das Problem Idempotenz](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span><span class="sxs-lookup"><span data-stu-id="eecd0-106">The full details of the scenario are best described in the blog post [SQL Database Connectivity and the Idempotency Issue](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx).</span></span>  <span data-ttu-id="eecd0-107">Zusammenfassend lässt sich sagen ist das Szenario an, dass wenn eine Ausnahme, während ein Transaktionscommit ausgelöst wird es zwei mögliche Ursachen gibt:</span><span class="sxs-lookup"><span data-stu-id="eecd0-107">In summary, the scenario is that when an exception is raised during a transaction commit there are two possible causes:</span></span>  

1. <span data-ttu-id="eecd0-108">Fehler bei das Commit der Transaktion auf dem server</span><span class="sxs-lookup"><span data-stu-id="eecd0-108">The transaction commit failed on the server</span></span>
2. <span data-ttu-id="eecd0-109">Das Commit der Transaktion, die auf dem Server war erfolgreich, aber ein Verbindungsproblem verhindert die Benachrichtigung über den Client erreicht</span><span class="sxs-lookup"><span data-stu-id="eecd0-109">The transaction commit succeeded on the server but a connectivity issue prevented the success notification from reaching the client</span></span>  

<span data-ttu-id="eecd0-110">Wenn die erste Situation der Anwendung oder den Benutzer geschieht, können Sie den Vorgang wiederholen. Wenn jedoch die zweite Situation tritt auf, Wiederholungen sollten vermieden werden und die Anwendung kann automatisch wiederhergestellt.</span><span class="sxs-lookup"><span data-stu-id="eecd0-110">When the first situation happens the application or the user can retry the operation, but when the second situation occurs retries should be avoided and the application could recover automatically.</span></span> <span data-ttu-id="eecd0-111">Die Herausforderung besteht, die ohne die Fähigkeit zum erkennen, was die tatsächliche Ursache, die während des Commits, eine Ausnahme gemeldet wurde, die Anwendung nicht die richtige Vorgehensweise wählen kann.</span><span class="sxs-lookup"><span data-stu-id="eecd0-111">The challenge is that without the ability to detect what was the actual reason an exception was reported during commit, the application cannot choose the right course of action.</span></span> <span data-ttu-id="eecd0-112">Das neue Feature in EF 6.1 ermöglicht EF mit der Datenbank zu überprüfen, ob die Transaktion erfolgreich war, und führen Sie die richtige Vorgehensweise transparent.</span><span class="sxs-lookup"><span data-stu-id="eecd0-112">The new feature in EF 6.1 allows EF to double-check with the database if the transaction succeeded and take the right course of action transparently.</span></span>  

## <a name="using-the-feature"></a><span data-ttu-id="eecd0-113">Mithilfe der Funktion</span><span class="sxs-lookup"><span data-stu-id="eecd0-113">Using the feature</span></span>  

<span data-ttu-id="eecd0-114">Damit können das Feature müssen Sie einen Aufruf von einschließen [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) im Konstruktor der Ihre **"dbconfiguration"**.</span><span class="sxs-lookup"><span data-stu-id="eecd0-114">In order to enable the feature you need include a call to [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) in the constructor of your **DbConfiguration**.</span></span> <span data-ttu-id="eecd0-115">Wenn Sie keine Erfahrung mit sind **"dbconfiguration"**, finden Sie unter [Code-basierte Konfiguration](~/ef6/fundamentals/configuring/code-based.md).</span><span class="sxs-lookup"><span data-stu-id="eecd0-115">If you are unfamiliar with **DbConfiguration**, see [Code Based Configuration](~/ef6/fundamentals/configuring/code-based.md).</span></span> <span data-ttu-id="eecd0-116">Dieses Feature kann in Kombination mit der automatischen Wiederholungsversuchen, die wir in EF6 eingeführt verwendet werden, die in der Situation dabei helfen, in dem die Transaktion tatsächlich beim commit auf dem Server aufgrund eines vorübergehenden Fehlers Fehler:</span><span class="sxs-lookup"><span data-stu-id="eecd0-116">This feature can be used in combination with the automatic retries we introduced in EF6, which help in the situation in which the transaction actually failed to commit on the server due to a transient failure:</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

public class MyConfiguration : DbConfiguration  
{
  public MyConfiguration()  
  {  
    SetTransactionHandler(SqlProviderServices.ProviderInvariantName, () => new CommitFailureHandler());  
    SetExecutionStrategy(SqlProviderServices.ProviderInvariantName, () => new SqlAzureExecutionStrategy());  
  }  
}
```  

## <a name="how-transactions-are-tracked"></a><span data-ttu-id="eecd0-117">Wie werden Transaktionen nachverfolgt.</span><span class="sxs-lookup"><span data-stu-id="eecd0-117">How transactions are tracked</span></span>  

<span data-ttu-id="eecd0-118">Wenn das Feature aktiviert ist, fügen EF eine neue Tabelle für die Datenbank namens **__Transactions**.</span><span class="sxs-lookup"><span data-stu-id="eecd0-118">When the feature is enabled, EF will automatically add a new table to the database called **__Transactions**.</span></span> <span data-ttu-id="eecd0-119">Eine neue Zeile wird in dieser Tabelle eingefügt, jedes Mal, wenn eine Transaktion von EF erstellt und diese Zeile wird auf Vorhandensein geprüft, ob es sich bei einem Transaktionsfehler während des Commits.</span><span class="sxs-lookup"><span data-stu-id="eecd0-119">A new row is inserted in this table every time a transaction is created by EF and that row is checked for existence if a transaction failure occurs during commit.</span></span>  

<span data-ttu-id="eecd0-120">Obwohl EF ist bemüht sich, löscht Zeilen aus der Tabelle aus, wenn sie nicht mehr benötigt werden, kann die Tabelle vergrößert, wenn die Anwendung beendet wird, vorzeitig und aus diesem Grund müssen Sie möglicherweise die Tabelle manuell in einigen Fällen zu löschen.</span><span class="sxs-lookup"><span data-stu-id="eecd0-120">Although EF will do a best effort to prune rows from the table when they aren’t needed anymore, the table can grow if the application exits prematurely and for that reason you may need to purge the table manually in some cases.</span></span>  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a><span data-ttu-id="eecd0-121">Gewusst wie: Behandeln von commitfehlern mit früheren Versionen</span><span class="sxs-lookup"><span data-stu-id="eecd0-121">How to handle commit failures with previous Versions</span></span>

<span data-ttu-id="eecd0-122">Vor dem EF 6.1 war nicht Mechanismus zum Commit-Fehler in der EF-Produkt zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="eecd0-122">Before EF 6.1 there was not mechanism to handle commit failures in the EF product.</span></span> <span data-ttu-id="eecd0-123">Es gibt mehrere Möglichkeiten, die in dieser Situation, die mit früheren Versionen von EF6 angewendet werden kann:</span><span class="sxs-lookup"><span data-stu-id="eecd0-123">There are several ways to dealing with this situation that can be applied to previous versions of EF6:</span></span>  

* <span data-ttu-id="eecd0-124">Option 1: keine Aktion durchführen</span><span class="sxs-lookup"><span data-stu-id="eecd0-124">Option 1 - Do nothing</span></span>  

  <span data-ttu-id="eecd0-125">Die Wahrscheinlichkeit für einen Verbindungsfehler während des Transaktionscommits ist gering, sodass es für die Anwendung nur fehlschlägt, wenn diese Bedingung, tatsächlich eintritt gültig sein kann.</span><span class="sxs-lookup"><span data-stu-id="eecd0-125">The likelihood of a connection failure during transaction commit is low so it may be acceptable for your application to just fail if this condition actually occurs.</span></span>  

* <span data-ttu-id="eecd0-126">Option 2: Verwenden Sie die Datenbank, um Status zurückzusetzen</span><span class="sxs-lookup"><span data-stu-id="eecd0-126">Option 2 - Use the database to reset state</span></span>  

  1. <span data-ttu-id="eecd0-127">Verwerfen der aktuellen "DbContext"</span><span class="sxs-lookup"><span data-stu-id="eecd0-127">Discard the current DbContext</span></span>  
  2. <span data-ttu-id="eecd0-128">Erstellen Sie einen neuen "DbContext" und Wiederherstellen Sie des Zustands Ihrer Anwendung aus der Datenbank</span><span class="sxs-lookup"><span data-stu-id="eecd0-128">Create a new DbContext and restore the state of your application from the database</span></span>  
  3. <span data-ttu-id="eecd0-129">Informieren Sie den Benutzer, dass der letzte Vorgang nicht erfolgreich abgeschlossen wurde haben kann</span><span class="sxs-lookup"><span data-stu-id="eecd0-129">Inform the user that the last operation might not have been completed successfully</span></span>  

* <span data-ttu-id="eecd0-130">Option 3: die Transaktion manuell nachverfolgen</span><span class="sxs-lookup"><span data-stu-id="eecd0-130">Option 3 - Manually track the transaction</span></span>  

  1. <span data-ttu-id="eecd0-131">Fügen Sie eine Tabelle nicht nachverfolgt, mit der Datenbank verwendet, um den Status der Transaktionen nachzuverfolgen.</span><span class="sxs-lookup"><span data-stu-id="eecd0-131">Add a non-tracked table to the database used to track the status of the transactions.</span></span>  
  2. <span data-ttu-id="eecd0-132">Eine Zeile in der Tabelle am Anfang jeder Transaktion eingefügt.</span><span class="sxs-lookup"><span data-stu-id="eecd0-132">Insert a row into the table at the beginning of each transaction.</span></span>  
  3. <span data-ttu-id="eecd0-133">Wenn die Verbindung während des Commits fehlschlägt, überprüfen Sie das Vorhandensein der entsprechenden Zeile in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="eecd0-133">If the connection fails during the commit, check for the presence of the corresponding row in the database.</span></span>  
     - <span data-ttu-id="eecd0-134">Wenn die Zeile vorhanden ist, weiterhin normal ausgeführt, da die Transaktion erfolgreich durchgeführt wurde</span><span class="sxs-lookup"><span data-stu-id="eecd0-134">If the row is present, continue normally, as the transaction was committed successfully</span></span>  
     - <span data-ttu-id="eecd0-135">Wenn die Zeile nicht vorhanden ist, verwenden Sie eine Ausführungsstrategie, um den aktuellen Vorgang wiederholen.</span><span class="sxs-lookup"><span data-stu-id="eecd0-135">If the row is absent, use an execution strategy to retry the current operation.</span></span>  
  4. <span data-ttu-id="eecd0-136">Wenn der Commit erfolgreich ausgeführt wird, löschen Sie die entsprechende Zeile aus, um die Vergrößerung der Tabelle zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="eecd0-136">If the commit is successful, delete the corresponding row to avoid the growth of the table.</span></span>  

<span data-ttu-id="eecd0-137">[In diesem Blogbeitrag](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) enthält Beispielcode, um dies zu erreichen unter SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="eecd0-137">[This blog post](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) contains sample code for accomplishing this on SQL Azure.</span></span>  
