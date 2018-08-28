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
# <a name="connection-management"></a>Verbindungsverwaltung
Diese Seite beschreibt das Verhalten von Entity Framework im Hinblick auf die Verbindungen an den Kontext und die Funktionalität der übergeben der **Database.Connection.Open()** API.  

## <a name="passing-connections-to-the-context"></a>Übergeben von Verbindungen in den Kontext  

### <a name="behavior-for-ef5-and-earlier-versions"></a>Verhalten für EF5 und frühere Versionen  

Es gibt zwei Konstruktoren, die Verbindungen zu akzeptieren:  

``` csharp
public DbContext(DbConnection existingConnection, bool contextOwnsConnection)
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection)
```  

Es ist möglich, die diese verwenden, jedoch müssen Sie einige Einschränkungen zu umgehen:  

1. Wenn Sie eine geöffnete Verbindung an einen der beiden dann das erste Mal übergeben, das Framework versucht, die sie verwenden, die ausgelöst wird, eine "InvalidOperationException" nicht möglich, besagt, dass eine bereits offene Verbindung erneut öffnen.  
2. Das Flag contextownsconnection den Wert ist interpretiert, und zwar unabhängig davon, ob die zugrunde liegende speicherverbindung verworfen werden soll, wenn der Kontext verworfen wird. Jedoch unabhängig von dieser Einstellung werden die Store-Verbindung wird immer geschlossen, wenn der Kontext verworfen wird. Also wenn Sie mehr als eine "DbContext" mit der gleichen Verbindung haben jeweils Kontext freigegeben wird zuerst wird die Verbindung schließen (auf ähnliche Weise, wenn Sie eine vorhandene ADO.NET-Verbindung mit einem "DbContext" kombiniert haben, wird "DbContext" stets zu schließen, wenn er verworfen wird) .  

Es ist möglich, über die erste Einschränkung umgehen, indem Sie eine geschlossene Verbindung übergeben und nur Code, die es geöffnet werden, nachdem alle Kontexte erstellt wurden:  

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

Die zweite Einschränkung bedeutet lediglich, dass Sie davon absehen, Ihr "DbContext" Objekte verwerfen, bis Sie bereit sind, für die Verbindung geschlossen werden müssen.  

### <a name="behavior-in-ef6-and-future-versions"></a>In EF6 und in zukünftigen Versionen  

In EF6 und zukünftige Versionen "DbContext" verfügt über die gleichen zwei Konstruktoren, jedoch ist nicht mehr erforderlich, dass die Verbindung an den Konstruktor übergebene geschlossen werden, bei der sie empfangen werden. Damit dies nun möglich ist:  

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

Das Flag contextownsconnection den Wert steuert auch jetzt, und zwar unabhängig davon, ob die Verbindung sowohl geschlossen und freigegeben werden, wenn DbContext gelöscht wird. Damit im obigen Beispiel die Verbindung nicht geschlossen wird, wenn der Kontext verworfen (Zeile 32) aus, wie es in früheren Versionen von EF gewesen wäre, sondern wenn die Verbindung selbst gelöscht wird (line 40).  

Es ist möglich, dass DbContext, um die Kontrolle über die Verbindung (nur Set contextownsconnection den Wert "true", oder verwenden Sie eine der anderen Konstruktoren) Wenn Sie dies wünschen.  

> [!NOTE]
> Es gibt einige weiteren Überlegungen bei der Verwendung von Transaktionen mit diesem neuen Modell. Weitere Informationen finden Sie [arbeiten mit Transaktionen](~/ef6/saving/transactions.md).  

## <a name="databaseconnectionopen"></a>Database.Connection.Open()  

### <a name="behavior-for-ef5-and-earlier-versions"></a>Verhalten für EF5 und frühere Versionen  

In EF5 und früheren Versionen ein Fehler vorhanden ist, dass die **ObjectContext.Connection.State** wurde nicht entsprechend den tatsächlichen Zustand der zugrunde liegenden speicherverbindung aktualisiert. Z. B. Wenn Sie den folgenden Code ausführen Sie zurückgegeben werden können, den Status **geschlossen** ist, obwohl in der Tat ist die zugrunde liegende speicherverbindung **öffnen**.  

``` csharp
((IObjectContextAdapter)context).ObjectContext.Connection.State
```  

Wenn Sie die Verbindung mit der Datenbank durch Aufrufen von Database.Connection.Open() öffnen sie werden öffnen erst beim nächsten einer Abfrage ausführen oder nichts anderes erfordert eine datenbankverbindung (z. B. SaveChanges()) jedoch nach, die zugrunde liegende zu speichern. Verbindung wird geschlossen. Der Kontext wird anschließend erneut zu öffnen und erneut schließt die Verbindung jedes Mal, wenn eine andere Datenbank erforderlich ist:  

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

### <a name="behavior-in-ef6-and-future-versions"></a>In EF6 und in zukünftigen Versionen  

Für EF6 und zukünftige Versionen haben wir der Vorgehensweise, die ergriffen, wenn der aufrufende Code zum Öffnen der Verbindung durch den aufrufenden Kontext auswählt. Database.Connection.Open() dann ist es verfügt über einen guten Grund dafür ein, und das Framework der Annahme, dass er möchte die Kontrolle über das Öffnen und Schließen der Verbindung und wird die Verbindung nicht mehr automatisch beendet.  

> [!NOTE]
> Dies kann potenziell zur Verbindungen führen die geöffnet sind, für lange Zeit verwenden Sie daher mit Vorsicht.  

Wir können den Code auch aktualisiert, sodass ObjectContext.Connection.State jetzt verfolgt den Status der zugrunde liegenden Verbindung ordnungsgemäß des.  

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
