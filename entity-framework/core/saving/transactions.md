---
title: Transaktionen - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d3e6515b-8181-482c-a790-c4a6778748c1
ms.technology: entity-framework-core
uid: core/saving/transactions
ms.openlocfilehash: a2f890c0af1e83cbcc1d40d68540ff7132a9bafd
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="using-transactions"></a>Verwenden von Transaktionen

Transaktionen können mehrere Datenbankvorgänge in eine atomare Weise verarbeitet werden. Wenn die Transaktion ein Commit ausgeführt wird, werden alle Vorgänge erfolgreich auf die Datenbank angewendet. Wenn die Transaktion ein Rollback ausgeführt wird, werden keiner der Vorgänge auf die Datenbank angewendet.

> [!TIP]  
> Sie können anzeigen, dass dieser Artikel [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Transactions/) auf GitHub.

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
> `DbContextOptionsBuilder`ist die API im verwendet haben `DbContext.OnConfiguring` um den Kontext zu konfigurieren, Sie sind nun damit extern erstellen mit `DbContextOptions`.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?highlight=3,4,5)] -->
``` csharp
    public class BloggingContext : DbContext
    {
        public BloggingContext(DbContextOptions<BloggingContext> options)
            : base(options)
        { }

        public DbSet<Blog> Blogs { get; set; }
    }
```

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

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/SharingTransaction/Sample.cs?highlight=1,2,3,7,16,23,24,25)] -->
``` csharp
        var options = new DbContextOptionsBuilder<BloggingContext>()
            .UseSqlServer(new SqlConnection(connectionString))
            .Options;

        using (var context1 = new BloggingContext(options))
        {
            using (var transaction = context1.Database.BeginTransaction())
            {
                try
                {
                    context1.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context1.SaveChanges();

                    using (var context2 = new BloggingContext(options))
                    {
                        context2.Database.UseTransaction(transaction.GetDbTransaction());

                        var blogs = context2.Blogs
                            .OrderBy(b => b.Url)
                            .ToList();
                    }

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

## <a name="using-external-dbtransactions-relational-databases-only"></a>Verwenden von externen DbTransactions (nur bei relationalen Datenbanken)

Wenn Sie mehrere datenzugriffstechnologien Zugriff auf eine relationale Datenbank verwenden, empfiehlt es sich, eine Transaktion zwischen Operationen, die von diesen anderen Technologien gemeinsam nutzen.

Im folgenden Beispiel wird gezeigt, wie ein ADO.NET SqlClient-Vorgang und ein Entity Framework Core-Vorgang in derselben Transaktion ausgeführt.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Transactions/ExternalDbTransaction/Sample.cs?highlight=4,10,21,26,27,28)] -->
``` csharp
        var connection = new SqlConnection(connectionString);
        connection.Open();

        using (var transaction = connection.BeginTransaction())
        {
            try
            {
                // Run raw ADO.NET command in the transaction
                var command = connection.CreateCommand();
                command.Transaction = transaction;
                command.CommandText = "DELETE FROM dbo.Blogs";
                command.ExecuteNonQuery();

                // Run an EF Core command in the transaction
                var options = new DbContextOptionsBuilder<BloggingContext>()
                    .UseSqlServer(connection)
                    .Options;

                using (var context = new BloggingContext(options))
                {
                    context.Database.UseTransaction(transaction);
                    context.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/dotnet" });
                    context.SaveChanges();
                }

                // Commit transaction if all commands succeed, transaction will auto-rollback
                // when disposed if either commands fails
                transaction.Commit();
            }
            catch (System.Exception)
            {
                // TODO: Handle failure
            }
```
