---
title: Arbeiten mit Transaktionen – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0d0f1824-d781-4cb3-8fda-b7eaefced1cd
ms.openlocfilehash: 96cfff4cca59ab27dd68f50d0260e90902e33a92
ms.sourcegitcommit: eefcab31142f61a7aaeac03ea90dcd39f158b8b8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2019
ms.locfileid: "64873234"
---
# <a name="working-with-transactions"></a>Arbeiten mit Transaktionen
> [!NOTE]
> **Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt. Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.  

In diesem Dokument wird beschrieben, mithilfe von Transaktionen in EF 6, einschließlich der Verbesserungen, die wir seit EF5 zum erleichtern der Arbeit mit Transaktionen hinzugefügt haben.  

## <a name="what-ef-does-by-default"></a>Funktionsweise von EF standardmäßig  

In allen Versionen von Entity Framework, bei jedem Ausführen **SaveChanges()** einfügen, aktualisieren und löschen Sie das Framework für die Datenbank werden dieser Vorgang in einer Transaktion umschließen. Diese Transaktion nur so lange dauert, zum Ausführen des Vorgangs, und klicken Sie dann schließt. Wenn Sie einen anderen solcher Vorgang ausführen, wird eine neue Transaktion gestartet.  

Ab EF6 **Database.ExecuteSqlCommand()** in der Standardeinstellung wird umbrochen werden, den Befehl in einer Transaktion Falls noch nicht vorhanden war. Es gibt Überladungen dieser Methode, mit denen Sie dieses Verhalten überschreiben, wenn Sie möchten. In EF6-Ausführung von gespeicherten Prozeduren, die im Modell über APIs wie z. B. enthaltenen **ObjectContext.ExecuteFunction()** führt den gleichen (außer, dass das Standardverhalten im Moment nicht überschrieben werden kann).  

In beiden Fällen wird die Isolationsstufe der Transaktion, beliebige Isolationsstufe der Datenbankanbieter die Standardeinstellung berücksichtigt. Standardmäßig ist z. B. auf SQL Server diese READ COMMITTED.  

Entitätsframework wird Abfragen in einer Transaktion nicht umbrochen werden.  

Diese Standard-Funktion eignet sich für eine Vielzahl von Benutzern und daher besteht keine Notwendigkeit, sich in EF6 nichts tun; Schreiben Sie Code einfach, wie Sie immer so gemacht hat.  

Jedoch einige Benutzer benötigen mehr Kontrolle über ihre Transaktionen – Dies wird in den folgenden Abschnitten behandelt.  

## <a name="how-the-apis-work"></a>Funktionsweise der APIs  

Vor EF6 Entity Framework tiefsten Abgründe Öffnen der datenbankverbindung selbst (es wurde eine Ausnahme ausgelöst, wenn sie eine Verbindung übergeben wurde, die bereits geöffnet ist). Da nur eine Transaktion auf eine offene Verbindung gestartet werden kann, dies bedeutete, dass die einzige Möglichkeit, die ein Benutzer kann mehrere Vorgänge in einer Transaktion Umschließen mit wurde ein [TransactionScope](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) oder verwenden Sie die  **ObjectContext.Connection** -Eigenschaft und Start Aufrufen **Open()** und **BeginTransaction()** direkt auf das zurückgegebene **EntityConnection** -Objekt. Darüber hinaus würde eine API-Aufrufe, die die Datenbank erreichbar fehlschlagen, wenn Sie eine Transaktion für die Verbindung mit der zugrunde liegenden Datenbank selbst gestartet haben.  

> [!NOTE]
> Die Einschränkung nur geschlossene Verbindungen zu akzeptieren, wurde in Entity Framework 6 entfernt. Weitere Informationen finden Sie unter [Verbindungsverwaltung](~/ef6/fundamentals/connection-management.md).  

Ab EF6 das Framework jetzt bietet:  

1. **Database.BeginTransaction()** : Eine einfachere Methode für einen Benutzer zum Starten und Abschließen von Transaktionen selbst innerhalb einer vorhandenen "DbContext": zulassen, dass mehrere Vorgänge innerhalb einer Transaktion kombiniert werden und daher entweder alle die ein Commit oder alle der Rollback als eine. Sie können auch den Benutzer leichter auf die Isolationsstufe der Transaktion angeben.  
2. **Database.UseTransaction()** : ermöglicht die Verwendung von "DbContext", um eine Transaktion zu verwenden, die außerhalb von Entity Framework gestartet wurde.  

### <a name="combining-several-operations-into-one-transaction-within-the-same-context"></a>Kombinieren mehrere Vorgänge in einer Transaktion innerhalb desselben Kontexts  

**Database.BeginTransaction()** verfügt über zwei Außerkraftsetzungen – mit der eine explizite annimmt [IsolationLevel](https://msdn.microsoft.com/library/system.data.isolationlevel.aspx) und eine, die keine Argumente erwartet und verwendet die standardmäßige IsolationLevel aus der zugrunde liegenden Datenbankanbieter. Zurückgeben der beiden Außerkraftsetzungen eine **DbContextTransaction** Objekt bietet **Commit()** und **Rollback()** Methoden, die Commit- und Rollback führen Sie auf den zugrunde liegenden Speicher die Transaktion.  

Die **DbContextTransaction** freigegeben werden, nachdem sie ein Commit oder Rollback wurde dient. Eine einfache Möglichkeit, dies zu erreichen ist die **using(...) {...}** Die Syntax für automatisch aufrufen **Dispose()** Wenn der verwendete block abgeschlossen ist:  

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
> Das Starten einer Transaktion erfordert, dass die zugrunde liegende speicherverbindung geöffnet ist. Daher ist das Database.BeginTransaction() aufrufen wird die Verbindung geöffnet, wenn es nicht bereits geöffnet ist. Wenn DbContextTransaction die Verbindung geöffnet. Klicken Sie dann schließt es er beim Dispose() aufrufen.  

### <a name="passing-an-existing-transaction-to-the-context"></a>Übergeben eine vorhandene Transaktion an den Kontext  

Manchmal möchten Sie eine Transaktion die noch umfassenderen im Gültigkeitsbereich befindet und enthält die Vorgänge, für dieselbe Datenbank jedoch außerhalb von EF vollständig. Zu diesem Zweck öffnen Sie die Verbindung und starten Sie die Transaktion selbst und dann mitteilen EF (a), verwenden Sie die Verbindung mit der Datenbank bereits geöffnet, und (b) um die vorhandene Transaktion für diese Verbindung verwenden.  

Zu diesem Zweck müssen Sie definieren und verwenden Sie einen Konstruktor für Ihre Kontextklasse, die von einem der Konstruktoren "DbContext" erbt, die i) einen vorhandenen Verbindungsparameter und Ii) die contextownsconnection den Wert booleschen in Anspruch nehmen.  

> [!NOTE]
> Auf "false" bei Aufruf in diesem Szenario muss das Flag contextownsconnection den Wert festgelegt werden. Dies ist wichtig, da Entity Framework darüber informiert, dass die Verbindung nicht schließen soll, wenn es mehr benötigt wird (z. B. finden Sie in Zeile 4 unten):  

``` csharp
using (var conn = new SqlConnection("..."))
{
    conn.Open();
    using (var context = new BloggingContext(conn, contextOwnsConnection: false))
    {
    }
}
```  

Darüber hinaus müssen Sie starten Sie die Transaktion selbst (einschließlich die IsolationLevel-Eigenschaft, wenn Sie die Standardeinstellung vermeiden möchten) und lassen Sie Entity Framework, wird eine vorhandene Transaktion bereits für die Verbindung gestartet (siehe Zeile 33 unten).  

Anschließend können Sie Datenbankvorgänge ausführen, entweder direkt auf die SqlConnection selbst oder auf "DbContext". Alle derartigen Vorgänge sind innerhalb einer Transaktion ausgeführt. Sie nutzen die Verantwortung für durch Commit oder Rollback der Transaktion und zum Aufrufen von Dispose() darauf sowie zum Schließen und Freigeben der Verbindung mit der Datenbank. Zum Beispiel:  

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

### <a name="clearing-up-the-transaction"></a>Deaktivieren Sie das Transaktionsprotokoll

Sie können null an Database.UseTransaction() zum Löschen des Entity Framework-Kenntnisse für die aktuelle Transaktion übergeben. Entitätsframework wird weder Commit oder Rollback der vorhandenen Transaktion, wenn Sie dies tun, also mit Umsicht verwenden und nur dann, wenn Sie sicher sind, dies ist, was Sie tun möchten.  

### <a name="errors-in-usetransaction"></a>Fehler in UseTransaction

Sehen Sie eine Ausnahme von Database.UseTransaction(), wenn Sie eine Transaktion übergeben wenn:  
- Entitätsframework verfügt bereits über eine vorhandene Transaktion  
- Entitätsframework wird in ein TransactionScope-Objekt bereits ausgeführt.  
- Das Verbindungsobjekt in der übergebenen Transaktion ist null. D. h. die Transaktion ist nicht mit einer Verbindung verknüpft – in der Regel ist dies ein Zeichen dafür, die diese Transaktion bereits abgeschlossen wurde  
- Das Verbindungsobjekt in der übergebenen Transaktion stimmt nicht mit Entity Framework Verbindung überein.  

## <a name="using-transactions-with-other-features"></a>Verwenden von Transaktionen mit anderen Funktionen  

In diesem Abschnitt wird erläutert, wie die oben genannten Transaktionen interagieren:  

- Verbindungsresilienz  
- Asynchrone Methoden  
- TransactionScope-Transaktionen  

### <a name="connection-resiliency"></a>Verbindungsresilienz  

Das neue Connection Resiliency-Feature funktioniert nicht mit vom Benutzer initiierte Transaktionen. Weitere Informationen finden Sie unter [Retrying Execution Strategies](~/ef6/fundamentals/connection-resiliency/retry-logic.md#user-initiated-transactions-are-not-supported).  

### <a name="asynchronous-programming"></a>Asynchrone Programmierung  

Der in den vorherigen Abschnitten beschriebenen Ansatz benötigt keine weiteren Optionen oder Einstellungen zum Arbeiten mit der [asynchronen Abfragen und speichern Sie die Methoden](~/ef6/fundamentals/async.md
). Aber bedenken, dass je nachdem, welche Aufgaben Sie in die asynchronen Methoden ausführen, lang andauernde Transaktionen – dadurch wiederum können Deadlocks oder zu blockieren Dadurch können, die sich negativ auf die Leistung der gesamten Anwendung ist.  

### <a name="transactionscope-transactions"></a>TransactionScope-Transaktionen  

Vor EF6 wurde die empfohlene Methode für die Bereitstellung von größerer Bereich Transaktionen eine TransactionScope-Objekt verwendet wird:  

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

SqlConnection und Entity Framework würde, sowohl die TransactionScope-Ambiente-Transaktion verwenden, und daher miteinander sein ein Commit ausgeführt.  

Ab .NET 4.5.1 TransactionScope aktualisiert wurde, um auch bei asynchronen Methoden über die Verwendung von arbeiten die [TransactionScopeAsyncFlowOption](https://msdn.microsoft.com/library/system.transactions.transactionscopeasyncflowoption.aspx) Enumeration:  

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

Es gibt noch einige Einschränkungen, die der TransactionScope-Ansatz:  

- Erfordert, dass .NET 4.5.1 oder höher, um mit asynchronen Methoden arbeiten.  
- Es kann nicht in der Cloud-Szenarien verwendet werden, es sei denn, Sie Sie sicher sind, dass nur eine Verbindung (Cloud-Szenarien unterstützen keine verteilte Transaktionen).  
- Er kann nicht mit dem Database.UseTransaction()-Ansatz, der den vorherigen Abschnitten kombiniert werden.  
- Es werden Ausnahmen auslösen, wenn Sie beim Ausgeben von DDL und verteilte Transaktionen über den MS DTC-Dienst nicht aktiviert haben.  

Vorteile der TransactionScope-Ansatz:  

- Es wird automatisch eine lokale Transaktion zu einer verteilten Transaktion aktualisiert, wenn Sie eine Verbindung mit einer Datenbank mit einer Verbindung mit einer anderen Datenbank innerhalb derselben Transaktion zu kombinieren oder stellen mehr als eine Verbindung mit einer bestimmten Datenbank (Hinweis: Sie benötigen der MSDTC-Dienst so konfiguriert, dass um verteilte Transaktionen für diese Option, um arbeiten zu ermöglichen).  
- Die Einfachheit der Programmierung. Wenn gewünscht, dass die Transaktion Ambiente und Karten mit implizit im Hintergrund ist, anstatt explizit unter der Sie steuern kann dann der TransactionScope-Ansatz für Sie besser geeignet.  

Zusammenfassend, mit dem neuen Database.BeginTransaction() und Database.UseTransaction()-APIs, die oben genannten ist nicht mehr der TransactionScope-Ansatz für die meisten Benutzer erforderlich sind. Wenn Sie weiterhin TransactionScope-Objekt verwenden werden Sie über die oben genannten Einschränkungen. Es wird empfohlen, mit dem Ansatz beschrieben, die in den vorherigen Abschnitten stattdessen nach Möglichkeit.  
