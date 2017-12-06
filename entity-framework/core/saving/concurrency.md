---
title: "Behandeln von Parallelität - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bce0539d-b0cd-457d-be71-f7ca16f3baea
ms.technology: entity-framework-core
uid: core/saving/concurrency
ms.openlocfilehash: bbd3e154c1b27b16c7d8f8fbf9ed51df0849795c
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="handling-concurrency"></a>Behandeln von Parallelität

Wenn eine Eigenschaft als ein parallelitätstoken konfiguriert ist prüft EF dann, dass kein anderer Benutzer beim Speichern der Änderungen an diesen Datensatz der Wert in der Datenbank nicht geändert wurde.

> [!TIP]  
> Sie können anzeigen, dass dieser Artikel [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) auf GitHub.

## <a name="how-concurrency-handling-works-in-ef-core"></a>Funktionsweise der Parallelitätsbehandlung in EF Core

Eine ausführliche Beschreibung der Funktionsweise der Parallelitätsbehandlung in Entity Framework Core, finden Sie unter [Parallelitätstoken](../modeling/concurrency.md).

## <a name="resolving-concurrency-conflicts"></a>Auflösen von Parallelitätskonflikten

Auflösen von einem Parallelitätskonflikt umfasst mithilfe eines Algorithmus, um die ausstehenden Änderungen aus dem aktuellen Benutzer mit den Änderungen in der Datenbank zusammenzuführen. Die genaue Vorgehensweise richten sich nach der Anwendung, aber eine gängige Methode ist die Werte für den Benutzer anzeigen und entscheiden, die richtigen Werte in der Datenbank gespeichert werden.

**Es gibt drei Gruppen von Werten verfügbar, in denen einen Parallelitätskonflikt gelöst wird.**

* **Aktuelle Werte** sind die Werte, die die Anwendung versucht hat, in die Datenbank geschrieben.

* **Ursprüngliche Werte** sind die Werte, die ursprünglich aus der Datenbank abgerufen wurden, bevor Änderungen vorgenommen wurden.

* **Datenbank-Werte** sind die Werte, die derzeit in der Datenbank gespeichert.

Behandeln Sie einen Parallelitätskonflikt Abfangen einer `DbUpdateConcurrencyException` während `SaveChanges()`, verwenden `DbUpdateConcurrencyException.Entries` einen neuen Satz von Änderungen für die betroffenen Entitäten vorbereiten, und wiederholen dann die `SaveChanges()` Vorgang.

Im folgenden Beispiel `Person.FirstName` und `Person.LastName` Setup als parallelitätstoken sind. Es ist ein `// TODO:` Kommentar in den Speicherort, in denen schließen Sie anwendungsspezifische Logik zum Auswählen des Wert in der Datenbank gespeichert werden sollen.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Concurrency/Sample.cs?highlight=53,54)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using System;
using System.ComponentModel.DataAnnotations;
using System.Linq;

namespace EFSaving.Concurrency
{
    public class Sample
    {
        public static void Run()
        {
            // Ensure database is created and has a person in it
            using (var context = new PersonContext())
            {
                context.Database.EnsureDeleted();
                context.Database.EnsureCreated();

                context.People.Add(new Person { FirstName = "John", LastName = "Doe" });
                context.SaveChanges();
            }

            using (var context = new PersonContext())
            {
                // Fetch a person from database and change phone number
                var person = context.People.Single(p => p.PersonId == 1);
                person.PhoneNumber = "555-555-5555";

                // Change the persons name in the database (will cause a concurrency conflict)
                context.Database.ExecuteSqlCommand("UPDATE dbo.People SET FirstName = 'Jane' WHERE PersonId = 1");

                try
                {
                    // Attempt to save changes to the database
                    context.SaveChanges();
                }
                catch (DbUpdateConcurrencyException ex)
                {
                    foreach (var entry in ex.Entries)
                    {
                        if (entry.Entity is Person)
                        {
                            // Using a NoTracking query means we get the entity but it is not tracked by the context
                            // and will not be merged with existing entities in the context.
                            var databaseEntity = context.People.AsNoTracking().Single(p => p.PersonId == ((Person)entry.Entity).PersonId);
                            var databaseEntry = context.Entry(databaseEntity);

                            foreach (var property in entry.Metadata.GetProperties())
                            {
                                var proposedValue = entry.Property(property.Name).CurrentValue;
                                var originalValue = entry.Property(property.Name).OriginalValue;
                                var databaseValue = databaseEntry.Property(property.Name).CurrentValue;

                                // TODO: Logic to decide which value should be written to database
                                // entry.Property(property.Name).CurrentValue = <value to be saved>;

                                // Update original values to
                                entry.Property(property.Name).OriginalValue = databaseEntry.Property(property.Name).CurrentValue;
                            }
                        }
                        else
                        {
                            throw new NotSupportedException("Don't know how to handle concurrency conflicts for " + entry.Metadata.Name);
                        }
                    }

                    // Retry the save operation
                    context.SaveChanges();
                }
            }
        }

        public class PersonContext : DbContext
        {
            public DbSet<Person> People { get; set; }

            protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
            {
                optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFSaving.Concurrency;Trusted_Connection=True;");
            }
        }

        public class Person
        {
            public int PersonId { get; set; }

            [ConcurrencyCheck]
            public string FirstName { get; set; }

            [ConcurrencyCheck]
            public string LastName { get; set; }

            public string PhoneNumber { get; set; }
        }

    }
}
```
