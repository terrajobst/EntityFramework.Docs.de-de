---
title: Transaktionen - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
ms.technology: entity-framework-core
uid: core/saving/transactions
ms.openlocfilehash: 2dda7b7d58ae058fc2aa89fe16fbf46adc8c6bdc
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="using-transactions"></a>Verwenden von Transaktionen

Transaktionen können mehrere Datenbankvorgänge in eine atomare Weise verarbeitet werden. Wenn die Transaktion ein Commit ausgeführt wird, werden alle Vorgänge erfolgreich auf die Datenbank angewendet. Wenn die Transaktion ein Rollback ausgeführt wird, werden keiner der Vorgänge auf die Datenbank angewendet.

> [!TIP]  
> Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) finden Sie auf GitHub.

## <a name="default-transaction-behavior"></a>Standardverhalten für die Transaktion

Standardmäßig, wenn der Anbieter Transaktionen unterstützt alle Änderungen in einem einzigen Aufruf `SaveChanges()` in einer Transaktion angewendet werden. Wenn einer der Änderungen fehlschlägt, wird ein Rollback für die Transaktion, und keine der Änderungen auf die Datenbank angewendet werden. Dies bedeutet, dass `SaveChanges()` wird sichergestellt, dass entweder vollständig erfolgreich ausgeführt werden, oder lassen Sie die Datenbank, die unverändert bleiben sollen, wenn ein Fehler auftritt.

Für die meisten Anwendungen ist dieses Standardverhalten ausreichend. Transaktionen sollten nur manuell gesteuert werden, wenn die Anforderungen an Ihre Anwendung erforderlichen aufgefasst.

## <a name="controlling-transactions"></a>Steuern von Transaktionen

Sie können die `DbContext.Database` -API zum Starten der commit und Rollback-Transaktionen. Das folgende Beispiel zeigt zwei `SaveChanges()` Vorgänge und eine LINQ-Abfrage in einer einzelnen Transaktion ausgeführt wird.

Nicht alle Datenbankanbieter unterstützen Transaktionen. Einige Anbieter auslösen oder ohne-Op Wenn Transaktion APIs aufgerufen werden.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/ControllingTransaction/Sample.cs?highlight=3,17,18,19)] -->
``` csharp
        using (var context = new BloggingContext())
        {
            using (var transaction = context.Database.BeginTransaction())
            {
                try
                {
                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context.SaveChanges();

                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/visualstudio" });
                    context.SaveChanges();

                    var blogs = context.Blogs
                        .OrderBy(b => b.Url)
                        .ToList();

                    // Commit transaction if all commands succeed, transaction will auto-rollback
                    // when disposed if either commands fails
                    transaction.Commit();
                }
                catch (Exception)
                {
                    // TODO: Handle failure
                }
            }
        }
```

## <a name="cross-context-transaction-relational-databases-only"></a>Cross-Kontexttransaktion (nur bei relationalen Datenbanken)

Sie können auch eine Transaktion über mehrere Instanzen von Kontext freigeben. Diese Funktion ist nur verfügbar, wenn einen relationale Datenbank-Anbieter verwenden, da es erfordert, dass die Verwendung von `DbTransaction` und `DbConnection`, die speziell für relationale Datenbanken sind.

Um eine Transaktion gemeinsam nutzen, müssen die Kontexte verwenden beide eine `DbConnection` und ein `DbTransaction`.

### <a name="allow-connection-to-be-externally-provided"></a>Lassen Sie Verbindung mit extern bereitgestellt werden zu

Freigeben einer `DbConnection` erfordert die Möglichkeit, eine Verbindung in einem Kontext übergeben wird, wenn es zu erstellen.

Die einfachste Möglichkeit zum Zulassen `DbConnection` extern bereitgestellt werden, ist, Beenden der Verwendung der `DbContext.OnConfiguring` Methode so extern erstellen und konfigurieren den Kontext `DbContextOptions` und an den Kontext Konstruktor zu übergeben.

> [!TIP]  
> `DbContextOptionsBuilder` ist die API im verwendet haben `DbContext.OnConfiguring` um den Kontext zu konfigurieren, Sie sind nun damit extern erstellen mit `DbContextOptions`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Context&highlight=3,4,5)]

Eine Alternative besteht darin, die weitere Verwendung `DbContext.OnConfiguring`, akzeptieren jedoch eine `DbConnection` , gespeichert und dann im verwendet `DbContext.OnConfiguring`.

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

### <a name="share-connection-and-transaction"></a>Freigabe-Verbindung und der Transaktionsstatus

Sie können nun mehrere Kontext Instanzen erstellen, die die gleiche Verbindung verwenden. Verwenden Sie dann die `DbContext.Database.UseTransaction(DbTransaction)` -API, um beide Kontexte in derselben Transaktion eintragen.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?name=Transaction&highlight=1,2,3,7,16,23,24,25)]

## <a name="using-external-dbtransactions-relational-databases-only"></a>Verwenden von externen DbTransactions (nur bei relationalen Datenbanken)

Wenn Sie mehrere datenzugriffstechnologien Zugriff auf eine relationale Datenbank verwenden, empfiehlt es sich, eine Transaktion zwischen Operationen, die von diesen anderen Technologien gemeinsam nutzen.

Im folgenden Beispiel wird gezeigt, wie ein ADO.NET SqlClient-Vorgang und ein Entity Framework Core-Vorgang in derselben Transaktion ausgeführt.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?name=Transaction&highlight=4,10,21,26,27,28)]

## <a name="using-systemtransactions"></a>Using System.Transactions

> [!NOTE]  
> Dieses Feature ist neu in EF Core 2.1.

Es ist möglich, die Ambiente-Transaktionen verwenden, wenn Sie über einen größeren Bereich koordinieren müssen.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/AmbientTransaction/Sample.cs?name=Transaction&highlight=1,24,25,26)]

Es ist auch möglich, die in einer expliziten Transaktion eintragen.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Transactions/CommitableTransaction/Sample.cs?name=Transaction&highlight=1,13,26,27,28)]

### <a name="limitations-of-systemtransactions"></a>Einschränkungen von ' System.Transactions '  

1. EF Core basiert auf Datenbankanbietern zum Implementieren der Unterstützung für System.Transactions hinzu. Zwar Unterstützung für .NET Framework, die API nur wurde vor kurzem hinzugefügt, um .NET Core und unterstützen daher üblich, das zwischen ADO.NET-Anbietern ist ist nicht weit verbreitet ist. Wenn ein Anbieter keine Unterstützung für System.Transactions implementiert, ist es möglich, dass Aufrufe dieser APIs werden vollständig ignoriert. SqlClient für .NET Core unterstützt aus 2.1 oder höher. Für .NET Core 2.0 SqlClient löst eine Ausnahme des Sie versuchen, das Feature zu verwenden. 

   > [!IMPORTANT]  
   > Es wird empfohlen, dass Sie testen, ob die API ordnungsgemäß mit Ihrem Anbieter verhält, bevor Sie sich darauf beziehen, für die Verwaltung von Transaktionen. Es wird empfohlen, wenden Sie sich an den Maintainer des Datenbank-Anbieters an, wenn dies nicht der Fall. 

2. Ab Version 2.1, die System.Transactions-Implementierung in .NET Core bietet keine Unterstützung für verteilte Transaktionen, die aus diesem Grund können keine `TransactionScope` oder `CommitableTransaction`Transaktionen über mehrere Ressourcen-Manager koordiniert. 
