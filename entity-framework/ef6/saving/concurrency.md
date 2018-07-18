---
title: 'Behandlung von Nebenläufigkeitskonflikten: EF 6'
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 2318e4d3-f561-4720-bbc3-921556806476
caps.latest.revision: 3
ms.openlocfilehash: b8608dbb4cadd60ff4ff4984583f8a9d843b3949
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2018
ms.locfileid: "39120784"
---
# <a name="handling-concurrency-conflicts"></a>Behandlung von Parallelitätskonflikten
Die optimistische Parallelität umfasst das optimistisch versuchen, Ihre Entität in der Datenbank in der Hoffnung zu speichern, die die Daten nicht, da die Entität geändert hat geladen wurde. Wenn es sich, dass stellte sich verfügt über die Daten geändert, und klicken Sie dann eine Ausnahme ausgelöst, und Sie den Konflikt beheben müssen, bevor Sie versuchen, erneut zu speichern. In diesem Thema wird beschrieben, wie solche Ausnahmen im Entity Framework behandelt wird. Die in diesem Thema dargestellten Techniken gelten jeweils für Modelle, die mit Code First und dem EF-Designer erstellt wurden.  

Dieser Beitrag ist nicht der geeignete Ort dafür eine umfassende Erläuterung der vollständigen Parallelität. In den folgenden Abschnitten wird davon ausgegangen Grundkenntnisse der Auflösung von Parallelität und zeigen anhand von Mustern für allgemeine Aufgaben.  

Viele dieser Muster verwenden, der in erläuterten Themen [arbeiten mit Eigenschaftswerten](~/ef6/saving/change-tracking/property-values.md).  

Auflösen von Parallelitätsproblemen, bei der Verwendung unabhängiger Zuordnungen (, denen der Fremdschlüssel nicht auf eine Eigenschaft in der Entität zugeordnet ist) ist sehr viel schwieriger als bei fremdschlüsselzuordnungen Verwendung. Aus diesem Grund, wenn Sie vorhaben, Concurrency-Lösung in Ihre Anwendung wird empfohlen, dass Sie immer über Fremdschlüssel in Ihren Entitäten zuordnen. Allen folgenden Beispielen wird davon ausgegangen, dass Sie fremdschlüsselzuordnungen verwenden.  

Von "SaveChanges" wird eine DbUpdateConcurrencyException ausgelöst, wenn eine Ausnahme für die vollständige Parallelität erkannt wird, bei dem Versuch, eine Entität zu speichern, die fremdschlüsselzuordnungen verwendet.  

## <a name="resolving-optimistic-concurrency-exceptions-with-reload-database-wins"></a>Beheben von Ausnahmen bzgl. vollständiger Parallelität mit neu laden (Datenbank-gewinnt)  

Die Reload-Methode kann verwendet werden, um die aktuellen Werte der Entität mit den Werten in der Datenbank zu überschreiben. Die Entität in der Regel erhält anschließend zurück an den Benutzer in irgendeiner Form, und sie müssen ihre Änderungen erneut vornehmen, und speichern Sie erneut versuchen. Zum Beispiel:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;

        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Update the values of the entity that failed to save from the store
            ex.Entries.Single().Reload();
        }

    } while (saveFailed);
}
```  

Eine gute Möglichkeit zum Simulieren einer Parallelitätsausnahme ist, legen Sie einen Haltepunkt im Aufruf von "SaveChanges", und ändern Sie eine Entität, die mit einem anderen Tool wie SQL Management Studio in der Datenbank gespeichert wird. Sie können auch eine Zeile vor "SaveChanges" zum Aktualisieren der Datenbank, die direkt mit SqlCommand einfügen. Zum Beispiel:  

``` csharp
context.Database.SqlCommand(
    "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
```  

Die Einträge-Methode für DbUpdateConcurrencyException gibt zurück, die "dbentityentry"-Instanzen für die Entitäten, die Fehler beim Aktualisieren. (Diese Eigenschaft gibt zurzeit immer einen einzelnen Wert für Parallelitätsprobleme auftreten. Sie können mehrere Werte für die allgemeinen Ausnahmen zurückgegeben.) Eine Alternative für einige Situationen ist möglicherweise Einträge für alle Entitäten zu erhalten, die aus der Datenbank erneut geladen werden müssen, und für jedes dieser Aufruf erneut zu laden.  

## <a name="resolving-optimistic-concurrency-exceptions-as-client-wins"></a>Beheben von Ausnahmen bzgl. vollständiger Parallelität als Client gewinnt  

Das obige Beispiel, das erneute Laden verwendet, wird auch als Datenbank Wins bezeichnet, oder Speicher gewinnt, da die Werte in der Entität von Werten aus der Datenbank überschrieben werden. Manchmal möchten Sie möglicherweise das Gegenteil machen, und überschreiben die Werte in der Datenbank mit den aktuellen Werten in der Entität. Dies wird auch als Client gewinnt bezeichnet und kann von den aktuellen Datenbankwerten abrufen und Festlegen von ihnen als die ursprünglichen Werte für die Entität ausgeführt werden. (Finden Sie unter [arbeiten mit Eigenschaftswerten](~/ef6/saving/change-tracking/property-values.md) Informationen zu aktuellen und ursprünglichen Werte.) Zum Beispiel:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Update original values from the database
            var entry = ex.Entries.Single();
            entry.OriginalValues.SetValues(entry.GetDatabaseValues());
        }

    } while (saveFailed);
}
```  

## <a name="custom-resolution-of-optimistic-concurrency-exceptions"></a>Benutzerdefinierte Auflösung der Ausnahmen bzgl. vollständiger Parallelität  

In einigen Fällen empfiehlt es sich um den aktuellen Werten in der Datenbank mit den aktuellen Werten in der Entität zu kombinieren. Dies ist in der Regel einige benutzerdefinierte Logik oder Benutzerinteraktion erforderlich. Beispielsweise können Sie ein Formular für dem Benutzer, die die aktuellen Werte, die Werte in der Datenbank darstellen, und eine Standardsammlung von aufgelösten Werte. Der Benutzer klicken Sie dann die aufgelösten Werte nach Bedarf bearbeiten, und es wäre diese aufgelösten Werte, die in der Datenbank gespeichert. Dies kann erfolgen mithilfe der Objekte DbPropertyValues CurrentValues und GetDatabaseValues auf der Entität Eintrag zurückgegeben. Zum Beispiel:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Get the current entity values and the values in the database
            var entry = ex.Entries.Single();
            var currentValues = entry.CurrentValues;
            var databaseValues = entry.GetDatabaseValues();

            // Choose an initial set of resolved values. In this case we
            // make the default be the values currently in the database.
            var resolvedValues = databaseValues.Clone();

            // Have the user choose what the resolved values should be
            HaveUserResolveConcurrency(currentValues, databaseValues, resolvedValues);

            // Update the original values with the database values and
            // the current values with whatever the user choose.
            entry.OriginalValues.SetValues(databaseValues);
            entry.CurrentValues.SetValues(resolvedValues);
        }
    } while (saveFailed);
}

public void HaveUserResolveConcurrency(DbPropertyValues currentValues,
                                       DbPropertyValues databaseValues,
                                       DbPropertyValues resolvedValues)
{
    // Show the current, database, and resolved values to the user and have
    // them edit the resolved values to get the correct resolution.
}
```  

## <a name="custom-resolution-of-optimistic-concurrency-exceptions-using-objects"></a>Benutzerdefinierte Auflösung der Ausnahmen bzgl. vollständiger Parallelität, die mithilfe von Objekten  

Der obige Code verwendet die DbPropertyValues-Instanzen für die Weitergabe der aktuellen Datenbank und aufgelösten Werte. Manchmal kann es einfacher, verwenden Instanzen des Entitätstyps für diesen sein. Dies kann erfolgen mithilfe der ToObject und SetValues von DbPropertyValues. Zum Beispiel:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Get the current entity values and the values in the database
            // as instances of the entity type
            var entry = ex.Entries.Single();
            var databaseValues = entry.GetDatabaseValues();
            var databaseValuesAsBlog = (Blog)databaseValues.ToObject();

            // Choose an initial set of resolved values. In this case we
            // make the default be the values currently in the database.
            var resolvedValuesAsBlog = (Blog)databaseValues.ToObject();

            // Have the user choose what the resolved values should be
            HaveUserResolveConcurrency((Blog)entry.Entity,
                                       databaseValuesAsBlog,
                                       resolvedValuesAsBlog);

            // Update the original values with the database values and
            // the current values with whatever the user choose.
            entry.OriginalValues.SetValues(databaseValues);
            entry.CurrentValues.SetValues(resolvedValuesAsBlog);
        }

    } while (saveFailed);
}

public void HaveUserResolveConcurrency(Blog entity,
                                       Blog databaseValues,
                                       Blog resolvedValues)
{
    // Show the current, database, and resolved values to the user and have
    // them update the resolved values to get the correct resolution.
}
```  
