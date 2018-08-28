---
title: Verbindungsverwaltung - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: ecaa5a27-b19e-4bf9-8142-a3fb00642270
ms.openlocfilehash: dc405e1876edc850ae6e4ce4649da52635d316ae
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997904"
---
# <a name="connection-management"></a><span data-ttu-id="9e67d-102">Verbindungsverwaltung</span><span class="sxs-lookup"><span data-stu-id="9e67d-102">Connection management</span></span>
<span data-ttu-id="9e67d-103">Diese Seite beschreibt das Verhalten von Entity Framework im Hinblick auf die Verbindungen an den Kontext und die Funktionalität der übergeben der **Database.Connection.Open()** API.</span><span class="sxs-lookup"><span data-stu-id="9e67d-103">This page describes the behavior of Entity Framework with regard to passing connections to the context and the functionality of the **Database.Connection.Open()** API.</span></span>  

## <a name="passing-connections-to-the-context"></a><span data-ttu-id="9e67d-104">Übergeben von Verbindungen in den Kontext</span><span class="sxs-lookup"><span data-stu-id="9e67d-104">Passing Connections to the Context</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="9e67d-105">Verhalten für EF5 und frühere Versionen</span><span class="sxs-lookup"><span data-stu-id="9e67d-105">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="9e67d-106">Es gibt zwei Konstruktoren, die Verbindungen zu akzeptieren:</span><span class="sxs-lookup"><span data-stu-id="9e67d-106">There are two constructors which accept connections:</span></span>  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

<span data-ttu-id="9e67d-107">Es ist möglich, die diese verwenden, jedoch müssen Sie einige Einschränkungen zu umgehen:</span><span class="sxs-lookup"><span data-stu-id="9e67d-107">It is possible to use these but you have to work around a couple of limitations:</span></span>  

1. <span data-ttu-id="9e67d-108">Wenn Sie eine geöffnete Verbindung an einen der beiden dann das erste Mal übergeben, das Framework versucht, die sie verwenden, die ausgelöst wird, eine "InvalidOperationException" nicht möglich, besagt, dass eine bereits offene Verbindung erneut öffnen.</span><span class="sxs-lookup"><span data-stu-id="9e67d-108">If you pass an open connection to either of these then the first time the framework attempts to use it an InvalidOperationException is thrown saying it cannot re-open an already open connection.</span></span>  
2. <span data-ttu-id="9e67d-109">Das Flag contextownsconnection den Wert ist interpretiert, und zwar unabhängig davon, ob die zugrunde liegende speicherverbindung verworfen werden soll, wenn der Kontext verworfen wird.</span><span class="sxs-lookup"><span data-stu-id="9e67d-109">The contextOwnsConnection flag is interpreted to mean whether or not the underlying store connection should be disposed when the context is disposed.</span></span> <span data-ttu-id="9e67d-110">Jedoch unabhängig von dieser Einstellung werden die Store-Verbindung wird immer geschlossen, wenn der Kontext verworfen wird.</span><span class="sxs-lookup"><span data-stu-id="9e67d-110">But, regardless of that setting, the store connection is always closed when the context is disposed.</span></span> <span data-ttu-id="9e67d-111">Also wenn Sie mehr als eine "DbContext" mit der gleichen Verbindung haben jeweils Kontext freigegeben wird zuerst wird die Verbindung schließen (auf ähnliche Weise, wenn Sie eine vorhandene ADO.NET-Verbindung mit einem "DbContext" kombiniert haben, wird "DbContext" stets zu schließen, wenn er verworfen wird) .</span><span class="sxs-lookup"><span data-stu-id="9e67d-111">So if you have more than one DbContext with the same connection whichever context is disposed first will close the connection (similarly if you have mixed an existing ADO.NET connection with a DbContext, DbContext will always close the connection when it is disposed).</span></span>  

<span data-ttu-id="9e67d-112">Es ist möglich, über die erste Einschränkung umgehen, indem Sie eine geschlossene Verbindung übergeben und nur Code, die es geöffnet werden, nachdem alle Kontexte erstellt wurden:</span><span class="sxs-lookup"><span data-stu-id="9e67d-112">It is possible to work around the first limitation above by passing a closed connection and only executing code that would open it once all contexts have been created:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Common;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;
using System.Linq;

namespace ConnectionManagementExamples
{
    class ConnectionManagementExampleEF5
    {         
        public static void TwoDbContextsOneConnection()
        {
            using (var context1 = new BloggingContext())
            {
                var conn =
                    ((EntityConnection)  
                        ((IObjectContextAdapter)context1).ObjectContext.Connection)  
                            .StoreConnection;

                using (var context2 = new BloggingContext(conn, contextOwnsConnection: false))
                {
                    context2.Database.ExecuteSqlCommand(
                        @"UPDATE Blogs SET Rating = 5" +
                        " WHERE Name LIKE '%Entity Framework%'");

                    var query = context1.Posts.Where(p => p.Blog.Rating > 5);
                    foreach (var post in query)
                    {
                        post.Title += "[Cool Blog]";
                    }
                    context1.SaveChanges();
                }
            }
        }
    }
}
```  

<span data-ttu-id="9e67d-113">Die zweite Einschränkung bedeutet lediglich, dass Sie davon absehen, Ihr "DbContext" Objekte verwerfen, bis Sie bereit sind, für die Verbindung geschlossen werden müssen.</span><span class="sxs-lookup"><span data-stu-id="9e67d-113">The second limitation just means you need to refrain from disposing any of your DbContext objects until you are ready for the connection to be closed.</span></span>  

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="9e67d-114">In EF6 und in zukünftigen Versionen</span><span class="sxs-lookup"><span data-stu-id="9e67d-114">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="9e67d-115">In EF6 und zukünftige Versionen "DbContext" verfügt über die gleichen zwei Konstruktoren, jedoch ist nicht mehr erforderlich, dass die Verbindung an den Konstruktor übergebene geschlossen werden, bei der sie empfangen werden.</span><span class="sxs-lookup"><span data-stu-id="9e67d-115">In EF6 and future versions the DbContext has the same two constructors but no longer requires that the connection passed to the constructor be closed when it is received.</span></span> <span data-ttu-id="9e67d-116">Damit dies nun möglich ist:</span><span class="sxs-lookup"><span data-stu-id="9e67d-116">So this is now possible:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.SqlClient;
using System.Linq;
using System.Transactions;

namespace ConnectionManagementExamples
{
    class ConnectionManagementExample
    {
        public static void PassingAnOpenConnection()
        {
            using (var conn = new SqlConnection("{connectionString}"))
            {
                conn.Open();

                var sqlCommand = new SqlCommand();
                sqlCommand.Connection = conn;
                sqlCommand.CommandText =
                    @"UPDATE Blogs SET Rating = 5" +
                     " WHERE Name LIKE '%Entity Framework%'";
                sqlCommand.ExecuteNonQuery();

                using (var context = new BloggingContext(conn, contextOwnsConnection: false))
                {
                    var query = context.Posts.Where(p => p.Blog.Rating > 5);
                    foreach (var post in query)
                    {
                        post.Title += "[Cool Blog]";
                    }
                    context.SaveChanges();
                }

                var sqlCommand2 = new SqlCommand();
                sqlCommand2.Connection = conn;
                sqlCommand2.CommandText =
                    @"UPDATE Blogs SET Rating = 7" +
                     " WHERE Name LIKE '%Entity Framework Rocks%'";
                sqlCommand2.ExecuteNonQuery();
            }
        }
    }
}
```  

<span data-ttu-id="9e67d-117">Das Flag contextownsconnection den Wert steuert auch jetzt, und zwar unabhängig davon, ob die Verbindung sowohl geschlossen und freigegeben werden, wenn DbContext gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="9e67d-117">Also the contextOwnsConnection flag now controls whether or not the connection is both closed and disposed when the DbContext is disposed.</span></span> <span data-ttu-id="9e67d-118">Damit im obigen Beispiel die Verbindung nicht geschlossen wird, wenn der Kontext verworfen (Zeile 32) aus, wie es in früheren Versionen von EF gewesen wäre, sondern wenn die Verbindung selbst gelöscht wird (line 40).</span><span class="sxs-lookup"><span data-stu-id="9e67d-118">So in the above example the connection is not closed when the context is disposed (line 32) as it would have been in previous versions of EF, but rather when the connection itself is disposed (line 40).</span></span>  

<span data-ttu-id="9e67d-119">Es ist möglich, dass DbContext, um die Kontrolle über die Verbindung (nur Set contextownsconnection den Wert "true", oder verwenden Sie eine der anderen Konstruktoren) Wenn Sie dies wünschen.</span><span class="sxs-lookup"><span data-stu-id="9e67d-119">Of course it is still possible for the DbContext to take control of the connection (just set contextOwnsConnection to true or use one of the other constructors) if you so wish.</span></span>  

> [!NOTE]
> <span data-ttu-id="9e67d-120">Es gibt einige weiteren Überlegungen bei der Verwendung von Transaktionen mit diesem neuen Modell.</span><span class="sxs-lookup"><span data-stu-id="9e67d-120">There are some additional considerations when using transactions with this new model.</span></span> <span data-ttu-id="9e67d-121">Weitere Informationen finden Sie [arbeiten mit Transaktionen](~/ef6/saving/transactions.md).</span><span class="sxs-lookup"><span data-stu-id="9e67d-121">For details see [Working with Transactions](~/ef6/saving/transactions.md).</span></span>  

## <a name="databaseconnectionopen"></a><span data-ttu-id="9e67d-122">Database.Connection.Open()</span><span class="sxs-lookup"><span data-stu-id="9e67d-122">Database.Connection.Open()</span></span>  

### <a name="behavior-for-ef5-and-earlier-versions"></a><span data-ttu-id="9e67d-123">Verhalten für EF5 und frühere Versionen</span><span class="sxs-lookup"><span data-stu-id="9e67d-123">Behavior for EF5 and earlier versions</span></span>  

<span data-ttu-id="9e67d-124">In EF5 und früheren Versionen ein Fehler vorhanden ist, dass die **ObjectContext.Connection.State** wurde nicht entsprechend den tatsächlichen Zustand der zugrunde liegenden speicherverbindung aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="9e67d-124">In EF5 and earlier versions there is a bug such that the **ObjectContext.Connection.State** was not updated to reflect the true state of the underlying store connection.</span></span> <span data-ttu-id="9e67d-125">Z. B. Wenn Sie den folgenden Code ausführen Sie zurückgegeben werden können, den Status **geschlossen** ist, obwohl in der Tat ist die zugrunde liegende speicherverbindung **öffnen**.</span><span class="sxs-lookup"><span data-stu-id="9e67d-125">For example, if you executed the following code you can be returned the status **Closed** even though in fact the underlying store connection is **Open**.</span></span>  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

<span data-ttu-id="9e67d-126">Wenn Sie die Verbindung mit der Datenbank durch Aufrufen von Database.Connection.Open() öffnen sie werden öffnen erst beim nächsten einer Abfrage ausführen oder nichts anderes erfordert eine datenbankverbindung (z. B. SaveChanges()) jedoch nach, die zugrunde liegende zu speichern. Verbindung wird geschlossen.</span><span class="sxs-lookup"><span data-stu-id="9e67d-126">Separately, if you open the database connection by calling Database.Connection.Open() it will be open until the next time you execute a query or call anything which requires a database connection (for example, SaveChanges()) but after that the underlying store connection will be closed.</span></span> <span data-ttu-id="9e67d-127">Der Kontext wird anschließend erneut zu öffnen und erneut schließt die Verbindung jedes Mal, wenn eine andere Datenbank erforderlich ist:</span><span class="sxs-lookup"><span data-stu-id="9e67d-127">The context will then re-open and re-close the connection any time another database operation is required:</span></span>  

``` csharp
using System;
using System.Data;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;

namespace ConnectionManagementExamples
{
    public class DatabaseOpenConnectionBehaviorEF5
    {
        public static void DatabaseOpenConnectionBehavior()
        {
            using (var context = new BloggingContext())
            {
                // At this point the underlying store connection is closed

                context.Database.Connection.Open();

                // Now the underlying store connection is open  
                // (though ObjectContext.Connection.State will report closed)

                var blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);

                // The underlying store connection is still open  

                context.SaveChanges();

                // After SaveChanges() the underlying store connection is closed  
                // Each SaveChanges() / query etc now opens and immediately closes
                // the underlying store connection

                blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();
            }
        }
    }
}
```  

### <a name="behavior-in-ef6-and-future-versions"></a><span data-ttu-id="9e67d-128">In EF6 und in zukünftigen Versionen</span><span class="sxs-lookup"><span data-stu-id="9e67d-128">Behavior in EF6 and future versions</span></span>  

<span data-ttu-id="9e67d-129">Für EF6 und zukünftige Versionen haben wir der Vorgehensweise, die ergriffen, wenn der aufrufende Code zum Öffnen der Verbindung durch den aufrufenden Kontext auswählt. Database.Connection.Open() dann ist es verfügt über einen guten Grund dafür ein, und das Framework der Annahme, dass er möchte die Kontrolle über das Öffnen und Schließen der Verbindung und wird die Verbindung nicht mehr automatisch beendet.</span><span class="sxs-lookup"><span data-stu-id="9e67d-129">For EF6 and future versions we have taken the approach that if the calling code chooses to open the connection by calling context.Database.Connection.Open() then it has a good reason for doing so and the framework will assume that it wants control over opening and closing of the connection and will no longer close the connection automatically.</span></span>  

> [!NOTE]
> <span data-ttu-id="9e67d-130">Dies kann potenziell zur Verbindungen führen die geöffnet sind, für lange Zeit verwenden Sie daher mit Vorsicht.</span><span class="sxs-lookup"><span data-stu-id="9e67d-130">This can potentially lead to connections which are open for a long time so use with care.</span></span>  

<span data-ttu-id="9e67d-131">Wir können den Code auch aktualisiert, sodass ObjectContext.Connection.State jetzt verfolgt den Status der zugrunde liegenden Verbindung ordnungsgemäß des.</span><span class="sxs-lookup"><span data-stu-id="9e67d-131">We also updated the code so that ObjectContext.Connection.State now keeps track of the state of the underlying connection correctly.</span></span>  

``` csharp
using System;
using System.Data;
using System.Data.Entity;
using System.Data.Entity.Core.EntityClient;
using System.Data.Entity.Infrastructure;

namespace ConnectionManagementExamples
{
    internal class DatabaseOpenConnectionBehaviorEF6
    {
        public static void DatabaseOpenConnectionBehavior()
        {
            using (var context = new BloggingContext())
            {
                // At this point the underlying store connection is closed

                context.Database.Connection.Open();

                // Now the underlying store connection is open and the
                // ObjectContext.Connection.State correctly reports open too

                var blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();

                // The underlying store connection remains open for the next operation  

                blog = new Blog { /* Blog’s properties */ };
                context.Blogs.Add(blog);
                context.SaveChanges();

                // The underlying store connection is still open

           } // The context is disposed – so now the underlying store connection is closed
        }
    }
}
```  
