---
title: Asynchrone Abfragen und speichern – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
ms.openlocfilehash: 758f8bc3d14fc1f60f14ff14f4251aeed057c518
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994462"
---
# <a name="async-query-and-save"></a>Asynchrone Abfragen und speichern
> [!NOTE]
> **Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt. Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.

EF6-Unterstützung für asynchrone Abfrage, und speichern die [Async und await-Schlüsselwörtern](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) , die in .NET 4.5 eingeführt wurden. Während der Asynchronie nicht alle Anwendungen profitieren können, kann es verwendet werden, um die Client-Reaktionsfähigkeit und die Server-Skalierbarkeit zu verbessern, bei der Verarbeitung langer "," Netzwerk "oder" e/A Aufgaben.

## <a name="when-to-really-use-async"></a>Wann Asynchronität wirklich sinnvoll

Der Zweck dieser exemplarischen Vorgehensweise werden die Async-Konzepte in einer Weise vorgestellt, die es einfach macht, beobachten Sie den Unterschied zwischen der Ausführung des asynchronen und synchronen Programms. Diese exemplarische Vorgehensweise dient nicht zur Veranschaulichung einer der wichtigsten Szenarios, bei denen asynchronen Programmierung Vorteile bietet.

Asynchrone Programmierung ist in erster Linie auf freizugeben, des aktuellen verwalteten Threads (Thread ausgeführte .NET Code) eine andere Aktion ausführen, während er für einen Vorgang wartet, die keine Compute-Zeit erfordert ausgelegt, von einem verwalteten Thread. Z. B. während eine Abfrage die Datenbank-Engine verarbeitet liegt kein vom .NET Code durchgeführt werden.

In Client-Anwendungen (Windows Forms, WPF, usw.) kann der aktuelle Thread verwendet werden, um die Benutzeroberfläche reaktionsfähig halten, während der asynchrone Vorgang ausgeführt wird. In Server-Anwendungen (ASP.NET usw.) des Threads verwendet werden kann, um andere eingehende Anforderungen - verarbeiten kann dies die arbeitsspeicherbelegung reduzieren und/oder Erhöhung des Durchsatzes des Servers.

Verwenden von Async in den meisten Fällen hat keine spürbare Vorteile und kann auch nachteilig sein. Verwenden Sie Tests, profilerstellung und gesundem Menschenverstand zu tun, um die Auswirkungen von Async in Ihr jeweiliges Szenario zu messen, bevor Sie endgültig.

Hier sind einige weitere Ressourcen, die Informationen zu asynchronen:

-   [Übersicht über die Brandon Bray Async/await in .NET 4.5](http://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   [Asynchrone Programmierung](https://msdn.microsoft.com/library/hh191443.aspx) Seiten in der MSDN-Bibliothek
-   [Wie Sie erstellen ASP.NET Web-Anwendungen mit Async](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (enthält eine Demo der erhöhten Durchsatz)

## <a name="create-the-model"></a>Erstellen des Modells

Wir verwenden die [Code First-Workflow](~/ef6/modeling/code-first/workflows/new-database.md) zum Erstellen des Modells und generieren die Datenbank, jedoch auch die asynchrone Funktionalität mit allen EF-Modellen, einschließlich der mit dem EF Designer erstellt wird.

-   Erstellen einer Konsolenanwendung und nennen sie **AsyncDemo**
-   Hinzufügen des EntityFramework NuGet-Pakets
    -   Im Projektmappen-Explorer mit der Maustaste auf die **AsyncDemo** Projekt
    -   Wählen Sie **NuGet-Pakete verwalten...**
    -   Wählen Sie in das Dialogfeld "NuGet-Pakete verwalten" die **Online** Registerkarte, und wählen Sie die **EntityFramework** Paket
    -   Klicken Sie auf **installieren**
-   Hinzufügen einer **"Model.cs"** Klasse durch die folgende Implementierung

``` csharp
    using System.Collections.Generic;
    using System.Data.Entity;

    namespace AsyncDemo
    {
        public class BloggingContext : DbContext
        {
            public DbSet<Blog> Blogs { get; set; }
            public DbSet<Post> Posts { get; set; }
        }

        public class Blog
        {
            public int BlogId { get; set; }
            public string Name { get; set; }

            public virtual List<Post> Posts { get; set; }
        }

        public class Post
        {
            public int PostId { get; set; }
            public string Title { get; set; }
            public string Content { get; set; }

            public int BlogId { get; set; }
            public virtual Blog Blog { get; set; }
        }
    }
```

 

## <a name="create-a-synchronous-program"></a>Erstellen Sie eine synchrone Anwendung

Nun, da wir ein EF-Modell verfügen, einen Code zu schreiben, der er verwendet, um einige Daten zugreift.

-   Ersetzen Sie den Inhalt der **"Program.cs"** durch den folgenden Code

``` csharp
    using System;
    using System.Linq;

    namespace AsyncDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                PerformDatabaseOperations();

                Console.WriteLine();
                Console.WriteLine("Quote of the day");
                Console.WriteLine(" Don't worry about the world coming to an end today... ");
                Console.WriteLine(" It's already tomorrow in Australia.");

                Console.WriteLine();
                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            public static void PerformDatabaseOperations()
            {
                using (var db = new BloggingContext())
                {
                    // Create a new blog and save it
                    db.Blogs.Add(new Blog
                    {
                        Name = "Test Blog #" + (db.Blogs.Count() + 1)
                    });
                    db.SaveChanges();

                    // Query for all blogs ordered by name
                    var blogs = (from b in db.Blogs
                                orderby b.Name
                                select b).ToList();

                    // Write all blogs out to Console
                    Console.WriteLine();
                    Console.WriteLine("All blogs:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" " + blog.Name);
                    }
                }
            }
        }
    }
```

Dieser Code Ruft die **PerformDatabaseOperations** Methode hierdurch eine neue **Blog** mit der Datenbank und dann Ruft alle **Blogs** aus der Datenbank und gibt sie an der **Konsole**. Anschließend schreibt das Programm ein Zitat des Tages die **Konsole**.

Da der Code Syncronous ist, können wir den folgende Ausführungsablauf beobachten, wenn das Programm ausgeführt wird:

1.  **"SaveChanges"** beginnt mit der neuen push **Blog** in der Datenbank
2.  **"SaveChanges"** abgeschlossen ist
3.  Abfrage für alle **Blogs** an die Datenbank gesendet wird
4.  Abfrage zurückgibt und die Ergebnisse werden geschrieben, um **Konsole**
5.  Zitat des Tages geschrieben **Konsole**

![SyncOutput](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a>Asynchronen

Nun, wir unser Programm verfügbar sind und ausgeführt haben, können wir beginnen, machen verwenden das neue Async und await-Schlüsselwörtern. Wir haben die folgenden Änderungen an "Program.cs" vorgenommen.

1.  Zeile 2: Die mit-Anweisung für die **System.Data.Entity** Namespace erhalten wir Zugriff auf die EF-Async-Erweiterungsmethoden.
2.  Zeile 4: Die mit-Anweisung für die **System.Threading.Tasks** Namespace kann wir verwenden die **Aufgabe** Typ.
3.  Zeile 12 und 18: wir als Task, der überwacht den Status der Erfassung **PerformSomeDatabaseOperations** (Zeile 12), und klicken Sie dann blockiert die Ausführung des Programms für diese Aufgabe auf vollständige einmal alle Aufgaben für die Anwendung (Zeile 18) erfolgt.
4.  Zeile 25: Wir haben Update **PerformSomeDatabaseOperations** als markiert werden **Async** und Zurückgeben einer **Aufgabe**.
5.  Zeile 35: Wir nun die asynchrone Version von "SaveChanges" aufrufen und dessen Abschluss warten.
6.  Zeile 42: Wir nun die asynchrone Version von "ToList" aufrufen und Warten auf das Ergebnis.

Eine umfassende Liste der verfügbaren Erweiterungsmethoden im Namespace System.Data.Entity finden Sie in der QueryableExtensions-Klasse. *Sie auch müssen hinzufügen "Verwenden von System.Data.Entity" mit Anweisungen.*

``` csharp
    using System;
    using System.Data.Entity;
    using System.Linq;
    using System.Threading.Tasks;

    namespace AsyncDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                var task = PerformDatabaseOperations();

                Console.WriteLine("Quote of the day");
                Console.WriteLine(" Don't worry about the world coming to an end today... ");
                Console.WriteLine(" It's already tomorrow in Australia.");

                task.Wait();

                Console.WriteLine();
                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            public static async Task PerformDatabaseOperations()
            {
                using (var db = new BloggingContext())
                {
                    // Create a new blog and save it
                    db.Blogs.Add(new Blog
                    {
                        Name = "Test Blog #" + (db.Blogs.Count() + 1)
                    });
                    Console.WriteLine("Calling SaveChanges.");
                    await db.SaveChangesAsync();
                    Console.WriteLine("SaveChanges completed.");

                    // Query for all blogs ordered by name
                    Console.WriteLine("Executing query.");
                    var blogs = await (from b in db.Blogs
                                orderby b.Name
                                select b).ToListAsync();

                    // Write all blogs out to Console
                    Console.WriteLine("Query completed with following results:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" - " + blog.Name);
                    }
                }
            }
        }
    }
```

Nun, da der Code den asynchronen ist, können wir einen anderen Ausführungsfluss beobachten, wenn das Programm ausgeführt wird:

1.  **"SaveChanges"** beginnt mit der neuen push **Blog** in der Datenbank *nach dem Senden des Befehls in der Datenbank nicht mehr compute-Zeit ist erforderlich, für den aktuellen verwalteten Thread. Die **PerformDatabaseOperations** Methodenrückgabe (auch wenn die Ausführung noch nicht abgeschlossen) und Programmablauf in der Main-Methode wird fortgesetzt.*
2.  **Zitat des Tages in die Konsole geschrieben**
    *da keine weiteren Aufgaben für die Sie in der Main-Methode ist, wird der verwaltete Thread blockiert, die Wartezeit aufrufen, bis der Datenbankvorgang abgeschlossen ist. Sobald der Vorgang abgeschlossen ist, die restlichen unsere **PerformDatabaseOperations** * ausgeführt wird.
3.  **"SaveChanges"** abgeschlossen ist
4.  Abfrage für alle **Blogs** wird gesendet, um die Datenbank *in diesem Fall der verwaltete Thread eine andere Aktion ausführen, während die Abfrage in der Datenbank verarbeitet werden kann. Da alle anderen Ausführung abgeschlossen ist, wird der Thread gerade jedoch auf den Aufruf warten angehalten.*
5.  Abfrage zurückgibt und die Ergebnisse werden geschrieben, um **Konsole**

![AsyncOutput](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a>Die Schlussfolgerung

Wir haben jetzt gesehen, wie einfach es ist, Erstellen von asynchronen Methoden von EF verwenden. Obwohl die Vorteile der asynchronen sehr deutlich, dass mit einer einfachen Konsolen-app möglicherweise nicht, können diese Strategien in Situationen angewendet werden, in dem lang andauernden oder netzwerkgebunden Aktivitäten möglicherweise andernfalls die Anwendung blockieren oder dazu führen, dass eine große Anzahl von threads Erhöhen Sie den Speicherbedarf.
