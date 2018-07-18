---
title: Arbeiten mit Transaktionen – EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 0d0f1824-d781-4cb3-8fda-b7eaefced1cd
caps.latest.revision: 3
ms.openlocfilehash: 4238c88cc149458ed11b96a0bf9aaed9aac40b2d
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "39121484"
---
# <a name="working-with-transactions"></a><span data-ttu-id="edd7d-102">Arbeiten mit Transaktionen</span><span class="sxs-lookup"><span data-stu-id="edd7d-102">Working with Transactions</span></span>
> [!NOTE]
> <span data-ttu-id="edd7d-103">**Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="edd7d-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="edd7d-104">Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.</span><span class="sxs-lookup"><span data-stu-id="edd7d-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="edd7d-105">In diesem Dokument wird beschrieben, mithilfe von Transaktionen in EF 6, einschließlich der Verbesserungen, die wir seit EF5 zum erleichtern der Arbeit mit Transaktionen hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="edd7d-105">This document will describe using transactions in EF6 including the enhancements we have added since EF5 to make working with transactions easy.</span></span>  

## <a name="what-ef-does-by-default"></a><span data-ttu-id="edd7d-106">Funktionsweise von EF standardmäßig</span><span class="sxs-lookup"><span data-stu-id="edd7d-106">What EF does by default</span></span>  

<span data-ttu-id="edd7d-107">In allen Versionen von Entity Framework, bei jedem Ausführen **SaveChanges()** einfügen, aktualisieren und löschen Sie das Framework für die Datenbank werden dieser Vorgang in einer Transaktion umschließen.</span><span class="sxs-lookup"><span data-stu-id="edd7d-107">In all versions of Entity Framework, whenever you execute **SaveChanges()** to insert, update or delete on the database the framework will wrap that operation in a transaction.</span></span> <span data-ttu-id="edd7d-108">Diese Transaktion nur so lange dauert, zum Ausführen des Vorgangs, und klicken Sie dann schließt.</span><span class="sxs-lookup"><span data-stu-id="edd7d-108">This transaction lasts only long enough to execute the operation and then completes.</span></span> <span data-ttu-id="edd7d-109">Wenn Sie einen anderen solcher Vorgang ausführen, wird eine neue Transaktion gestartet.</span><span class="sxs-lookup"><span data-stu-id="edd7d-109">When you execute another such operation a new transaction is started.</span></span>  

<span data-ttu-id="edd7d-110">Ab EF6 **Database.ExecuteSqlCommand()** in der Standardeinstellung wird umbrochen werden, den Befehl in einer Transaktion Falls noch nicht vorhanden war.</span><span class="sxs-lookup"><span data-stu-id="edd7d-110">Starting with EF6 **Database.ExecuteSqlCommand()** by default will wrap the command in a transaction if one was not already present.</span></span> <span data-ttu-id="edd7d-111">Es gibt Überladungen dieser Methode, mit denen Sie dieses Verhalten überschreiben, wenn Sie möchten.</span><span class="sxs-lookup"><span data-stu-id="edd7d-111">There are overloads of this method that allow you to override this behavior if you wish.</span></span> <span data-ttu-id="edd7d-112">In EF6-Ausführung von gespeicherten Prozeduren, die im Modell über APIs wie z. B. enthaltenen **ObjectContext.ExecuteFunction()** führt den gleichen (außer, dass das Standardverhalten im Moment nicht überschrieben werden kann).</span><span class="sxs-lookup"><span data-stu-id="edd7d-112">Also in EF6 execution of stored procedures included in the model through APIs such as **ObjectContext.ExecuteFunction()** does the same (except that the default behavior cannot at the moment be overridden).</span></span>  

<span data-ttu-id="edd7d-113">In beiden Fällen wird die Isolationsstufe der Transaktion, beliebige Isolationsstufe der Datenbankanbieter die Standardeinstellung berücksichtigt.</span><span class="sxs-lookup"><span data-stu-id="edd7d-113">In either case, the isolation level of the transaction is whatever isolation level the database provider considers its default setting.</span></span> <span data-ttu-id="edd7d-114">Standardmäßig ist z. B. auf SQL Server diese READ COMMITTED.</span><span class="sxs-lookup"><span data-stu-id="edd7d-114">By default, for instance, on SQL Server this is READ COMMITTED.</span></span>  

<span data-ttu-id="edd7d-115">Entitätsframework wird Abfragen in einer Transaktion nicht umbrochen werden.</span><span class="sxs-lookup"><span data-stu-id="edd7d-115">Entity Framework does not wrap queries in a transaction.</span></span>  

<span data-ttu-id="edd7d-116">Diese Standard-Funktion eignet sich für eine Vielzahl von Benutzern und daher besteht keine Notwendigkeit, sich in EF6 nichts tun; Schreiben Sie Code einfach, wie Sie immer so gemacht hat.</span><span class="sxs-lookup"><span data-stu-id="edd7d-116">This default functionality is suitable for a lot of users and if so there is no need to do anything different in EF6; just write the code as you always did.</span></span>  

<span data-ttu-id="edd7d-117">Jedoch einige Benutzer benötigen mehr Kontrolle über ihre Transaktionen – Dies wird in den folgenden Abschnitten behandelt.</span><span class="sxs-lookup"><span data-stu-id="edd7d-117">However some users require greater control over their transactions – this is covered in the following sections.</span></span>  

## <a name="how-the-apis-work"></a><span data-ttu-id="edd7d-118">Funktionsweise der APIs</span><span class="sxs-lookup"><span data-stu-id="edd7d-118">How the APIs work</span></span>  

<span data-ttu-id="edd7d-119">Vor EF6 Entity Framework tiefsten Abgründe Öffnen der datenbankverbindung selbst (es wurde eine Ausnahme ausgelöst, wenn sie eine Verbindung übergeben wurde, die bereits geöffnet ist).</span><span class="sxs-lookup"><span data-stu-id="edd7d-119">Prior to EF6 Entity Framework insisted on opening the database connection itself (it threw an exception if it was passed a connection that was already open).</span></span> <span data-ttu-id="edd7d-120">Da nur eine Transaktion auf eine offene Verbindung gestartet werden kann, dies bedeutete, dass die einzige Möglichkeit, die ein Benutzer kann mehrere Vorgänge in einer Transaktion Umschließen mit wurde ein [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) oder verwenden Sie die  **ObjectContext.Connection** -Eigenschaft und Start Aufrufen **Open()** und **BeginTransaction()** direkt auf das zurückgegebene **EntityConnection** -Objekt.</span><span class="sxs-lookup"><span data-stu-id="edd7d-120">Since a transaction can only be started on an open connection, this meant that the only way a user could wrap several operations into one transaction was either to use a [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) or use the **ObjectContext.Connection** property and start calling **Open()** and **BeginTransaction()** directly on the returned **EntityConnection** object.</span></span> <span data-ttu-id="edd7d-121">Darüber hinaus würde eine API-Aufrufe, die die Datenbank erreichbar fehlschlagen, wenn Sie eine Transaktion für die Verbindung mit der zugrunde liegenden Datenbank selbst gestartet haben.</span><span class="sxs-lookup"><span data-stu-id="edd7d-121">In addition, API calls which contacted the database would fail if you had started a transaction on the underlying database connection on your own.</span></span>  

> [!NOTE]
> <span data-ttu-id="edd7d-122">Die Einschränkung nur geschlossene Verbindungen zu akzeptieren, wurde in Entity Framework 6 entfernt.</span><span class="sxs-lookup"><span data-stu-id="edd7d-122">The limitation of only accepting closed connections was removed in Entity Framework 6.</span></span> <span data-ttu-id="edd7d-123">Weitere Informationen finden Sie unter [Verbindungsverwaltung](~/ef6/fundamentals/connection-management.md).</span><span class="sxs-lookup"><span data-stu-id="edd7d-123">For details, see [Connection Management](~/ef6/fundamentals/connection-management.md).</span></span>  

<span data-ttu-id="edd7d-124">Ab EF6 das Framework jetzt bietet:</span><span class="sxs-lookup"><span data-stu-id="edd7d-124">Starting with EF6 the framework now provides:</span></span>  

1. <span data-ttu-id="edd7d-125">**Database.BeginTransaction()** : eine einfachere Methode für einen Benutzer zum Starten und Abschließen von Transaktionen selbst innerhalb einer vorhandenen "DbContext": zulassen, dass mehrere Vorgänge innerhalb einer Transaktion kombiniert werden und daher entweder alle ein Commit oder alle ein Rollback als eine.</span><span class="sxs-lookup"><span data-stu-id="edd7d-125">**Database.BeginTransaction()** : An easier method for a user to start and complete transactions themselves within an existing DbContext – allowing several operations to be combined within the same transaction and hence either all committed or all rolled back as one.</span></span> <span data-ttu-id="edd7d-126">Sie können auch den Benutzer leichter auf die Isolationsstufe der Transaktion angeben.</span><span class="sxs-lookup"><span data-stu-id="edd7d-126">It also allows the user to more easily specify the isolation level for the transaction.</span></span>  
2. <span data-ttu-id="edd7d-127">**Database.UseTransaction()** : ermöglicht die Verwendung von "DbContext", um eine Transaktion zu verwenden, die außerhalb von Entity Framework gestartet wurde.</span><span class="sxs-lookup"><span data-stu-id="edd7d-127">**Database.UseTransaction()** : which allows the DbContext to use a transaction which was started outside of the Entity Framework.</span></span>  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a><span data-ttu-id="edd7d-128">Kombinieren mehrere Vorgänge in einer Transaktion innerhalb desselben Kontexts</span><span class="sxs-lookup"><span data-stu-id="edd7d-128">Combining several operations into one transaction within the same context</span></span>  

<span data-ttu-id="edd7d-129">**Database.BeginTransaction()** verfügt über zwei Außerkraftsetzungen – mit der eine explizite annimmt [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) und eine, die keine Argumente erwartet und verwendet die standardmäßige IsolationLevel aus der zugrunde liegenden Datenbankanbieter.</span><span class="sxs-lookup"><span data-stu-id="edd7d-129">**Database.BeginTransaction()** has two overrides – one which takes an explicit [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) and one which takes no arguments and uses the default IsolationLevel from the underlying database provider.</span></span> <span data-ttu-id="edd7d-130">Zurückgeben der beiden Außerkraftsetzungen eine **DbContextTransaction** Objekt bietet **Commit()** und **Rollback()** Methoden, die Commit- und Rollback führen Sie auf den zugrunde liegenden Speicher die Transaktion.</span><span class="sxs-lookup"><span data-stu-id="edd7d-130">Both overrides return a **DbContextTransaction** object which provides **Commit()** and **Rollback()** methods which perform commit and rollback on the underlying store transaction.</span></span>  

<span data-ttu-id="edd7d-131">Die **DbContextTransaction** freigegeben werden, nachdem sie ein Commit oder Rollback wurde dient.</span><span class="sxs-lookup"><span data-stu-id="edd7d-131">The **DbContextTransaction** is meant to be disposed once it has been committed or rolled back.</span></span> <span data-ttu-id="edd7d-132">Eine einfache Möglichkeit, dies zu erreichen ist die **using(...) {...}**</span><span class="sxs-lookup"><span data-stu-id="edd7d-132">One easy way to accomplish this is the **using(…) {…}**</span></span> <span data-ttu-id="edd7d-133">Die Syntax für automatisch aufrufen **Dispose()** Wenn der verwendete block abgeschlossen ist:</span><span class="sxs-lookup"><span data-stu-id="edd7d-133">syntax which will automatically call **Dispose()** when the using block completes:</span></span>  

``` csharp
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        static void StartOwnTransactionWithinContext()
        {
            using (var context = new BloggingContext())
            {
                using (var dbContextTransaction = context.Database.BeginTransaction())
                {
                    try
                    {
                        context.Database.ExecuteSqlCommand(
                            @"UPDATE Blogs SET Rating = 5" +
                                " WHERE Name LIKE '%Entity Framework%'"
                            );

                        var query = context.Posts.Where(p => p.Blog.Rating >= 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }

                        context.SaveChanges();

                        dbContextTransaction.Commit();
                    }
                    catch (Exception)
                    {
                        dbContextTransaction.Rollback();
                    }
                }
            }
        }
    }
}
```  

> [!NOTE]
> <span data-ttu-id="edd7d-134">Das Starten einer Transaktion erfordert, dass die zugrunde liegende speicherverbindung geöffnet ist.</span><span class="sxs-lookup"><span data-stu-id="edd7d-134">Beginning a transaction requires that the underlying store connection is open.</span></span> <span data-ttu-id="edd7d-135">Daher ist das Database.BeginTransaction() aufrufen wird die Verbindung geöffnet, wenn es nicht bereits geöffnet ist.</span><span class="sxs-lookup"><span data-stu-id="edd7d-135">So calling Database.BeginTransaction() will open the connection  if it is not already opened.</span></span> <span data-ttu-id="edd7d-136">Wenn DbContextTransaction die Verbindung geöffnet. Klicken Sie dann schließt es er beim Dispose() aufrufen.</span><span class="sxs-lookup"><span data-stu-id="edd7d-136">If DbContextTransaction opened the connection then it will close it when Dispose() is called.</span></span>  

### <a name="passing-an-existing-transaction-to-the-context"></a><span data-ttu-id="edd7d-137">Übergeben eine vorhandene Transaktion an den Kontext</span><span class="sxs-lookup"><span data-stu-id="edd7d-137">Passing an existing transaction to the context</span></span>  

<span data-ttu-id="edd7d-138">Manchmal möchten Sie eine Transaktion die noch umfassenderen im Gültigkeitsbereich befindet und enthält die Vorgänge, für dieselbe Datenbank jedoch außerhalb von EF vollständig.</span><span class="sxs-lookup"><span data-stu-id="edd7d-138">Sometimes you would like a transaction which is even broader in scope and which includes operations on the same database but outside of EF completely.</span></span> <span data-ttu-id="edd7d-139">Zu diesem Zweck öffnen Sie die Verbindung und starten Sie die Transaktion selbst und dann mitteilen EF (a), verwenden Sie die Verbindung mit der Datenbank bereits geöffnet, und (b) um die vorhandene Transaktion für diese Verbindung verwenden.</span><span class="sxs-lookup"><span data-stu-id="edd7d-139">To accomplish this you must open the connection and start the transaction yourself and then tell EF a) to use the already-opened database connection, and b) to use the existing transaction on that connection.</span></span>  

<span data-ttu-id="edd7d-140">Zu diesem Zweck müssen Sie definieren und verwenden Sie einen Konstruktor für Ihre Kontextklasse, die von einem der Konstruktoren "DbContext" erbt, die i) einen vorhandenen Verbindungsparameter und Ii) die contextownsconnection den Wert booleschen in Anspruch nehmen.</span><span class="sxs-lookup"><span data-stu-id="edd7d-140">To do this you must define and use a constructor on your context class which inherits from one of the DbContext constructors which take i) an existing connection parameter and ii) the contextOwnsConnection boolean.</span></span>  

> [!NOTE]
> <span data-ttu-id="edd7d-141">Auf "false" bei Aufruf in diesem Szenario muss das Flag contextownsconnection den Wert festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="edd7d-141">The contextOwnsConnection flag must be set to false when called in this scenario.</span></span> <span data-ttu-id="edd7d-142">Dies ist wichtig, da Entity Framework darüber informiert, dass die Verbindung nicht schließen soll, wenn es mehr benötigt wird (z. B. finden Sie in Zeile 4 unten):</span><span class="sxs-lookup"><span data-stu-id="edd7d-142">This is important as it informs Entity Framework that it should not close the connection when it is done with it (for example, see line 4 below):</span></span>  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

<span data-ttu-id="edd7d-143">Darüber hinaus müssen Sie starten Sie die Transaktion selbst (einschließlich die IsolationLevel-Eigenschaft, wenn Sie die Standardeinstellung vermeiden möchten) und lassen Sie Entity Framework, wird eine vorhandene Transaktion bereits für die Verbindung gestartet (siehe Zeile 33 unten).</span><span class="sxs-lookup"><span data-stu-id="edd7d-143">Furthermore, you must start the transaction yourself (including the IsolationLevel if you want to avoid the default setting) and let Entity Framework know that there is an existing transaction already started on the connection (see line 33 below).</span></span>  

<span data-ttu-id="edd7d-144">Anschließend können Sie Datenbankvorgänge ausführen, entweder direkt auf die SqlConnection selbst oder auf "DbContext".</span><span class="sxs-lookup"><span data-stu-id="edd7d-144">Then you are free to execute database operations either directly on the SqlConnection itself, or on the DbContext.</span></span> <span data-ttu-id="edd7d-145">Alle derartigen Vorgänge sind innerhalb einer Transaktion ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="edd7d-145">All such operations are executed within one transaction.</span></span> <span data-ttu-id="edd7d-146">Sie nutzen die Verantwortung für durch Commit oder Rollback der Transaktion und zum Aufrufen von Dispose() darauf sowie zum Schließen und Freigeben der Verbindung mit der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="edd7d-146">You take responsibility for committing or rolling back the transaction and for calling Dispose() on it, as well as for closing and disposing the database connection.</span></span> <span data-ttu-id="edd7d-147">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="edd7d-147">For example:</span></span>  

``` csharp
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
sing System.Transactions;

namespace TransactionsExamples
{
     class TransactionsExample
     {
        static void UsingExternalTransaction()
        {
            using (var conn = new SqlConnection("..."))
            {
               conn.Open();

               using (var sqlTxn = conn.BeginTransaction(System.Data.IsolationLevel.Snapshot))
               {
                   try
                   {
                       var sqlCommand = new SqlCommand();
                       sqlCommand.Connection = conn;
                       sqlCommand.Transaction = sqlTxn;
                       sqlCommand.CommandText =
                           @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                       sqlCommand.ExecuteNonQuery();

                       using (var context =  
                         new BloggingContext(conn, contextOwnsConnection: false))
                        {
                            context.Database.UseTransaction(sqlTxn);

                            var query =  context.Posts.Where(p => p.Blog.Rating >= 5);
                            foreach (var post in query)
                            {
                                post.Title += "[Cool Blog]";
                            }
                           context.SaveChanges();
                        }

                        sqlTxn.Commit();
                    }
                    catch (Exception)
                    {
                        sqlTxn.Rollback();
                    }
                }
            }
        }
    }
}
```  

### <a name="clearing-up-the-transaction"></a><span data-ttu-id="edd7d-148">Deaktivieren Sie das Transaktionsprotokoll</span><span class="sxs-lookup"><span data-stu-id="edd7d-148">Clearing up the transaction</span></span>

<span data-ttu-id="edd7d-149">Sie können null an Database.UseTransaction() zum Löschen des Entity Framework-Kenntnisse für die aktuelle Transaktion übergeben.</span><span class="sxs-lookup"><span data-stu-id="edd7d-149">You can pass null to Database.UseTransaction() to clear Entity Framework’s knowledge of the current transaction.</span></span> <span data-ttu-id="edd7d-150">Entitätsframework wird weder Commit oder Rollback der vorhandenen Transaktion, wenn Sie dies tun, also mit Umsicht verwenden und nur dann, wenn Sie sicher sind, dies ist, was Sie tun möchten.</span><span class="sxs-lookup"><span data-stu-id="edd7d-150">Entity Framework will neither commit nor rollback the existing transaction when you do this, so use with care and only if you’re sure this is what you want to do.</span></span>  

### <a name="errors-in-usetransaction"></a><span data-ttu-id="edd7d-151">Fehler in UseTransaction</span><span class="sxs-lookup"><span data-stu-id="edd7d-151">Errors in UseTransaction</span></span>

<span data-ttu-id="edd7d-152">Sehen Sie eine Ausnahme von Database.UseTransaction(), wenn Sie eine Transaktion übergeben wenn:</span><span class="sxs-lookup"><span data-stu-id="edd7d-152">You will see an exception from Database.UseTransaction() if you pass a transaction when:</span></span>  
- <span data-ttu-id="edd7d-153">Entitätsframework verfügt bereits über eine vorhandene Transaktion</span><span class="sxs-lookup"><span data-stu-id="edd7d-153">Entity Framework already has an existing transaction</span></span>  
- <span data-ttu-id="edd7d-154">Entitätsframework wird in ein TransactionScope-Objekt bereits ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="edd7d-154">Entity Framework is already operating within a TransactionScope</span></span>  
- <span data-ttu-id="edd7d-155">Das Verbindungsobjekt in der übergebenen Transaktion ist null.</span><span class="sxs-lookup"><span data-stu-id="edd7d-155">The connection object in the transaction passed is null.</span></span> <span data-ttu-id="edd7d-156">D. h. die Transaktion ist nicht mit einer Verbindung verknüpft – in der Regel ist dies ein Zeichen dafür, die diese Transaktion bereits abgeschlossen wurde</span><span class="sxs-lookup"><span data-stu-id="edd7d-156">That is, the transaction is not associated with a connection – usually this is a sign that that transaction has already completed</span></span>  
- <span data-ttu-id="edd7d-157">Das Verbindungsobjekt in der übergebenen Transaktion stimmt nicht mit Entity Framework Verbindung überein.</span><span class="sxs-lookup"><span data-stu-id="edd7d-157">The connection object in the transaction passed does not match the Entity Framework’s connection.</span></span>  

## <a name="using-transactions-with-other-features"></a><span data-ttu-id="edd7d-158">Verwenden von Transaktionen mit anderen Funktionen</span><span class="sxs-lookup"><span data-stu-id="edd7d-158">Using transactions with other features</span></span>  

<span data-ttu-id="edd7d-159">In diesem Abschnitt wird erläutert, wie die oben genannten Transaktionen interagieren:</span><span class="sxs-lookup"><span data-stu-id="edd7d-159">This section details how the above transactions interact with:</span></span>  

- <span data-ttu-id="edd7d-160">Verbindungsresilienz</span><span class="sxs-lookup"><span data-stu-id="edd7d-160">Connection resiliency</span></span>  
- <span data-ttu-id="edd7d-161">Asynchrone Methoden</span><span class="sxs-lookup"><span data-stu-id="edd7d-161">Asynchronous methods</span></span>  
- <span data-ttu-id="edd7d-162">TransactionScope-Transaktionen</span><span class="sxs-lookup"><span data-stu-id="edd7d-162">TransactionScope transactions</span></span>  

### <a name="connection-resiliency"></a><span data-ttu-id="edd7d-163">Verbindungsresilienz</span><span class="sxs-lookup"><span data-stu-id="edd7d-163">Connection Resiliency</span></span>  

<span data-ttu-id="edd7d-164">Das neue Connection Resiliency-Feature funktioniert nicht mit vom Benutzer initiierte Transaktionen.</span><span class="sxs-lookup"><span data-stu-id="edd7d-164">The new Connection Resiliency feature does not work with user-initiated transactions.</span></span> <span data-ttu-id="edd7d-165">Weitere Informationen finden Sie unter [Limitations with Retrying Execution Strategies](~/ef6/fundamentals/connection-resiliency/retry-logic.md#limitations).</span><span class="sxs-lookup"><span data-stu-id="edd7d-165">For details, see [Limitations with Retrying Execution Strategies](~/ef6/fundamentals/connection-resiliency/retry-logic.md#limitations).</span></span>  

### <a name="asynchronous-programming"></a><span data-ttu-id="edd7d-166">Asynchrone Programmierung</span><span class="sxs-lookup"><span data-stu-id="edd7d-166">Asynchronous Programming</span></span>  

<span data-ttu-id="edd7d-167">Der in den vorherigen Abschnitten beschriebenen Ansatz benötigt keine weiteren Optionen oder Einstellungen zum Arbeiten mit der [asynchronen Abfragen und speichern Sie die Methoden](~/ef6/fundamentals/async.md
).</span><span class="sxs-lookup"><span data-stu-id="edd7d-167">The approach outlined in the previous sections needs no further options or settings to work with the [asynchronous query and save methods](~/ef6/fundamentals/async.md
).</span></span> <span data-ttu-id="edd7d-168">Aber bedenken, dass je nachdem, welche Aufgaben Sie in die asynchronen Methoden ausführen, lang andauernde Transaktionen – dadurch wiederum können Deadlocks oder zu blockieren Dadurch können, die sich negativ auf die Leistung der gesamten Anwendung ist.</span><span class="sxs-lookup"><span data-stu-id="edd7d-168">But be aware that, depending on what you do within the asynchronous methods, this may result in long-running transactions – which can in turn cause deadlocks or blocking which is bad for the performance of the overall application.</span></span>  

### <a name="transactionscope-transactions"></a><span data-ttu-id="edd7d-169">TransactionScope-Transaktionen</span><span class="sxs-lookup"><span data-stu-id="edd7d-169">TransactionScope Transactions</span></span>  

<span data-ttu-id="edd7d-170">Vor EF6 wurde die empfohlene Methode für die Bereitstellung von größerer Bereich Transaktionen eine TransactionScope-Objekt verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="edd7d-170">Prior to EF6 the recommended way of providing larger scope transactions was to use a TransactionScope object:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        static void UsingTransactionScope()
        {
            using (var scope = new TransactionScope(TransactionScopeOption.Required))
            {
                using (var conn = new SqlConnection("..."))
                {
                    conn.Open();

                    var sqlCommand = new SqlCommand();
                    sqlCommand.Connection = conn;
                    sqlCommand.CommandText =
                        @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                    sqlCommand.ExecuteNonQuery();

                    using (var context =
                        new BloggingContext(conn, contextOwnsConnection: false))
                    {
                        var query = context.Posts.Where(p => p.Blog.Rating > 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }
                        context.SaveChanges();
                    }
                }

                scope.Complete();
            }
        }
    }
}
```  

<span data-ttu-id="edd7d-171">SqlConnection und Entity Framework würde, sowohl die TransactionScope-Ambiente-Transaktion verwenden, und daher miteinander sein ein Commit ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="edd7d-171">The SqlConnection and Entity Framework would both use the ambient TransactionScope transaction and hence be committed together.</span></span>  

<span data-ttu-id="edd7d-172">Ab .NET 4.5.1 TransactionScope aktualisiert wurde, um auch bei asynchronen Methoden über die Verwendung von arbeiten die [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) Enumeration:</span><span class="sxs-lookup"><span data-stu-id="edd7d-172">Starting with .NET 4.5.1 TransactionScope has been updated to also work with asynchronous methods via the use of the [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) enumeration:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace TransactionsExamples
{
    class TransactionsExample
    {
        public static void AsyncTransactionScope()
        {
            using (var scope = new TransactionScope(TransactionScopeAsyncFlowOption.Enabled))
            {
                using (var conn = new SqlConnection("..."))
                {
                    await conn.OpenAsync();

                    var sqlCommand = new SqlCommand();
                    sqlCommand.Connection = conn;
                    sqlCommand.CommandText =
                        @"UPDATE Blogs SET Rating = 5" +
                            " WHERE Name LIKE '%Entity Framework%'";
                    await sqlCommand.ExecuteNonQueryAsync();

                    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
                    {
                        var query = context.Posts.Where(p => p.Blog.Rating > 5);
                        foreach (var post in query)
                        {
                            post.Title += "[Cool Blog]";
                        }

                        await context.SaveChangesAsync();
                    }
                }
            }
        }
    }
}
```  

<span data-ttu-id="edd7d-173">Es gibt noch einige Einschränkungen, die der TransactionScope-Ansatz:</span><span class="sxs-lookup"><span data-stu-id="edd7d-173">There are still some limitations to the TransactionScope approach:</span></span>  

- <span data-ttu-id="edd7d-174">Erfordert, dass .NET 4.5.1 oder höher, um mit asynchronen Methoden arbeiten.</span><span class="sxs-lookup"><span data-stu-id="edd7d-174">Requires .NET 4.5.1 or greater to work with asynchronous methods.</span></span>  
- <span data-ttu-id="edd7d-175">Es kann nicht in der Cloud-Szenarien verwendet werden, es sei denn, Sie Sie sicher sind, dass nur eine Verbindung (Cloud-Szenarien unterstützen keine verteilte Transaktionen).</span><span class="sxs-lookup"><span data-stu-id="edd7d-175">It cannot be used in cloud scenarios unless you are sure you have one and only one connection (cloud scenarios do not support distributed transactions).</span></span>  
- <span data-ttu-id="edd7d-176">Er kann nicht mit dem Database.UseTransaction()-Ansatz, der den vorherigen Abschnitten kombiniert werden.</span><span class="sxs-lookup"><span data-stu-id="edd7d-176">It cannot be combined with the Database.UseTransaction() approach of the previous sections.</span></span>  
- <span data-ttu-id="edd7d-177">Es werden Ausnahmen auslösen, wenn Sie beim Ausgeben von DDL und verteilte Transaktionen über den MS DTC-Dienst nicht aktiviert haben.</span><span class="sxs-lookup"><span data-stu-id="edd7d-177">It will throw exceptions if you issue any DDL and have not enabled distributed transactions through the MSDTC Service.</span></span>  

<span data-ttu-id="edd7d-178">Vorteile der TransactionScope-Ansatz:</span><span class="sxs-lookup"><span data-stu-id="edd7d-178">Advantages of the TransactionScope approach:</span></span>  

- <span data-ttu-id="edd7d-179">Es wird automatisch eine lokale Transaktion zu einer verteilten Transaktion aktualisiert, wenn Sie eine Verbindung mit einer Datenbank mit einer Verbindung mit einer anderen Datenbank innerhalb derselben Transaktion zu kombinieren oder stellen mehr als eine Verbindung mit einer bestimmten Datenbank (Hinweis: Sie benötigen der MSDTC-Dienst so konfiguriert, dass um verteilte Transaktionen für diese Option, um arbeiten zu ermöglichen).</span><span class="sxs-lookup"><span data-stu-id="edd7d-179">It will automatically upgrade a local transaction to a distributed transaction if you make more than one connection to a given database or combine a connection to one database with a connection to a different database within the same transaction (note: you must have the MSDTC service configured to allow distributed transactions for this to work).</span></span>  
- <span data-ttu-id="edd7d-180">Die Einfachheit der Programmierung.</span><span class="sxs-lookup"><span data-stu-id="edd7d-180">Ease of coding.</span></span> <span data-ttu-id="edd7d-181">Wenn gewünscht, dass die Transaktion Ambiente und Karten mit implizit im Hintergrund ist, anstatt explizit unter der Sie steuern kann dann der TransactionScope-Ansatz für Sie besser geeignet.</span><span class="sxs-lookup"><span data-stu-id="edd7d-181">If you prefer the transaction to be ambient and dealt with implicitly in the background rather than explicitly under you control then the TransactionScope approach may suit you better.</span></span>  

<span data-ttu-id="edd7d-182">Zusammenfassend, mit dem neuen Database.BeginTransaction() und Database.UseTransaction()-APIs, die oben genannten ist nicht mehr der TransactionScope-Ansatz für die meisten Benutzer erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="edd7d-182">In summary, with the new Database.BeginTransaction() and Database.UseTransaction() APIs above, the TransactionScope approach is no longer necessary for most users.</span></span> <span data-ttu-id="edd7d-183">Wenn Sie weiterhin TransactionScope-Objekt verwenden werden Sie über die oben genannten Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="edd7d-183">If you do continue to use TransactionScope then be aware of the above limitations.</span></span> <span data-ttu-id="edd7d-184">Es wird empfohlen, mit dem Ansatz beschrieben, die in den vorherigen Abschnitten stattdessen nach Möglichkeit.</span><span class="sxs-lookup"><span data-stu-id="edd7d-184">We recommend using the approach outlined in the previous sections instead where possible.</span></span>  
