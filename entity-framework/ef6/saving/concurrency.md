---
title: 'Behandlung von Nebenläufigkeitskonflikten: EF 6'
author: divega
ms.date: 10/23/2016
ms.assetid: 2318e4d3-f561-4720-bbc3-921556806476
ms.openlocfilehash: 81ae186201fdfac331b1d4e7836b222545fe78b5
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489153"
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="a13fe-102">Behandlung von Parallelitätskonflikten</span><span class="sxs-lookup"><span data-stu-id="a13fe-102">Handling Concurrency Conflicts</span></span>
<span data-ttu-id="a13fe-103">Die optimistische Parallelität umfasst das optimistisch versuchen, Ihre Entität in der Datenbank in der Hoffnung zu speichern, die die Daten nicht, da die Entität geändert hat geladen wurde.</span><span class="sxs-lookup"><span data-stu-id="a13fe-103">Optimistic concurrency involves optimistically attempting to save your entity to the database in the hope that the data there has not changed since the entity was loaded.</span></span> <span data-ttu-id="a13fe-104">Wenn es sich, dass stellte sich verfügt über die Daten geändert, und klicken Sie dann eine Ausnahme ausgelöst, und Sie den Konflikt beheben müssen, bevor Sie versuchen, erneut zu speichern.</span><span class="sxs-lookup"><span data-stu-id="a13fe-104">If it turns out that the data has changed then an exception is thrown and you must resolve the conflict before attempting to save again.</span></span> <span data-ttu-id="a13fe-105">In diesem Thema wird beschrieben, wie solche Ausnahmen im Entity Framework behandelt wird.</span><span class="sxs-lookup"><span data-stu-id="a13fe-105">This topic covers how to handle such exceptions in Entity Framework.</span></span> <span data-ttu-id="a13fe-106">Die in diesem Thema dargestellten Techniken gelten jeweils für Modelle, die mit Code First und dem EF-Designer erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="a13fe-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="a13fe-107">Dieser Beitrag ist nicht der geeignete Ort dafür eine umfassende Erläuterung der vollständigen Parallelität.</span><span class="sxs-lookup"><span data-stu-id="a13fe-107">This post is not the appropriate place for a full discussion of optimistic concurrency.</span></span> <span data-ttu-id="a13fe-108">In den folgenden Abschnitten wird davon ausgegangen Grundkenntnisse der Auflösung von Parallelität und zeigen anhand von Mustern für allgemeine Aufgaben.</span><span class="sxs-lookup"><span data-stu-id="a13fe-108">The sections below assume some knowledge of concurrency resolution and show patterns for common tasks.</span></span>  

<span data-ttu-id="a13fe-109">Viele dieser Muster verwenden, der in erläuterten Themen [arbeiten mit Eigenschaftswerten](~/ef6/saving/change-tracking/property-values.md).</span><span class="sxs-lookup"><span data-stu-id="a13fe-109">Many of these patterns make use of the topics discussed in [Working with Property Values](~/ef6/saving/change-tracking/property-values.md).</span></span>  

<span data-ttu-id="a13fe-110">Auflösen von Parallelitätsproblemen, bei der Verwendung unabhängiger Zuordnungen (, denen der Fremdschlüssel nicht auf eine Eigenschaft in der Entität zugeordnet ist) ist sehr viel schwieriger als bei fremdschlüsselzuordnungen Verwendung.</span><span class="sxs-lookup"><span data-stu-id="a13fe-110">Resolving concurrency issues when you are using independent associations (where the foreign key is not mapped to a property in your entity) is much more difficult than when you are using foreign key associations.</span></span> <span data-ttu-id="a13fe-111">Aus diesem Grund, wenn Sie vorhaben, Concurrency-Lösung in Ihre Anwendung wird empfohlen, dass Sie immer über Fremdschlüssel in Ihren Entitäten zuordnen.</span><span class="sxs-lookup"><span data-stu-id="a13fe-111">Therefore if you are going to do concurrency resolution in your application it is advised that you always map foreign keys into your entities.</span></span> <span data-ttu-id="a13fe-112">Allen folgenden Beispielen wird davon ausgegangen, dass Sie fremdschlüsselzuordnungen verwenden.</span><span class="sxs-lookup"><span data-stu-id="a13fe-112">All the examples below assume that you are using foreign key associations.</span></span>  

<span data-ttu-id="a13fe-113">Von "SaveChanges" wird eine DbUpdateConcurrencyException ausgelöst, wenn eine Ausnahme für die vollständige Parallelität erkannt wird, bei dem Versuch, eine Entität zu speichern, die fremdschlüsselzuordnungen verwendet.</span><span class="sxs-lookup"><span data-stu-id="a13fe-113">A DbUpdateConcurrencyException is thrown by SaveChanges when an optimistic concurrency exception is detected while attempting to save an entity that uses foreign key associations.</span></span>  

## <a name="resolving-optimistic-concurrency-exceptions-with-reload-database-wins"></a><span data-ttu-id="a13fe-114">Beheben von Ausnahmen bzgl. vollständiger Parallelität mit neu laden (Datenbank-gewinnt)</span><span class="sxs-lookup"><span data-stu-id="a13fe-114">Resolving optimistic concurrency exceptions with Reload (database wins)</span></span>  

<span data-ttu-id="a13fe-115">Die Reload-Methode kann verwendet werden, um die aktuellen Werte der Entität mit den Werten in der Datenbank zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="a13fe-115">The Reload method can be used to overwrite the current values of the entity with the values now in the database.</span></span> <span data-ttu-id="a13fe-116">Die Entität in der Regel erhält anschließend zurück an den Benutzer in irgendeiner Form, und sie müssen ihre Änderungen erneut vornehmen, und speichern Sie erneut versuchen.</span><span class="sxs-lookup"><span data-stu-id="a13fe-116">The entity is then typically given back to the user in some form and they must try to make their changes again and re-save.</span></span> <span data-ttu-id="a13fe-117">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a13fe-117">For example:</span></span>  

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

<span data-ttu-id="a13fe-118">Eine gute Möglichkeit zum Simulieren einer Parallelitätsausnahme ist, legen Sie einen Haltepunkt im Aufruf von "SaveChanges", und ändern Sie eine Entität, die mit einem anderen Tool wie SQL Management Studio in der Datenbank gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="a13fe-118">A good way to simulate a concurrency exception is to set a breakpoint on the SaveChanges call and then modify an entity that is being saved in the database using another tool such as SQL Management Studio.</span></span> <span data-ttu-id="a13fe-119">Sie können auch eine Zeile vor "SaveChanges" zum Aktualisieren der Datenbank, die direkt mit SqlCommand einfügen.</span><span class="sxs-lookup"><span data-stu-id="a13fe-119">You could also insert a line before SaveChanges to update the database directly using SqlCommand.</span></span> <span data-ttu-id="a13fe-120">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a13fe-120">For example:</span></span>  

``` csharp
context.Database.SqlCommand(
    "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
```  

<span data-ttu-id="a13fe-121">Die Einträge-Methode für DbUpdateConcurrencyException gibt zurück, die "dbentityentry"-Instanzen für die Entitäten, die Fehler beim Aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="a13fe-121">The Entries method on DbUpdateConcurrencyException returns the DbEntityEntry instances for the entities that failed to update.</span></span> <span data-ttu-id="a13fe-122">(Diese Eigenschaft gibt zurzeit immer einen einzelnen Wert für Parallelitätsprobleme auftreten.</span><span class="sxs-lookup"><span data-stu-id="a13fe-122">(This property currently always returns a single value for concurrency issues.</span></span> <span data-ttu-id="a13fe-123">Sie können mehrere Werte für die allgemeinen Ausnahmen zurückgegeben.) Eine Alternative für einige Situationen ist möglicherweise Einträge für alle Entitäten zu erhalten, die aus der Datenbank erneut geladen werden müssen, und für jedes dieser Aufruf erneut zu laden.</span><span class="sxs-lookup"><span data-stu-id="a13fe-123">It may return multiple values for general update exceptions.) An alternative for some situations might be to get entries for all entities that may need to be reloaded from the database and call reload for each of these.</span></span>  

## <a name="resolving-optimistic-concurrency-exceptions-as-client-wins"></a><span data-ttu-id="a13fe-124">Beheben von Ausnahmen bzgl. vollständiger Parallelität als Client gewinnt</span><span class="sxs-lookup"><span data-stu-id="a13fe-124">Resolving optimistic concurrency exceptions as client wins</span></span>  

<span data-ttu-id="a13fe-125">Das obige Beispiel, das erneute Laden verwendet, wird auch als Datenbank Wins bezeichnet, oder Speicher gewinnt, da die Werte in der Entität von Werten aus der Datenbank überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="a13fe-125">The example above that uses Reload is sometimes called database wins or store wins because the values in the entity are overwritten by values from the database.</span></span> <span data-ttu-id="a13fe-126">Manchmal möchten Sie möglicherweise das Gegenteil machen, und überschreiben die Werte in der Datenbank mit den aktuellen Werten in der Entität.</span><span class="sxs-lookup"><span data-stu-id="a13fe-126">Sometimes you may wish to do the opposite and overwrite the values in the database with the values currently in the entity.</span></span> <span data-ttu-id="a13fe-127">Dies wird auch als Client gewinnt bezeichnet und kann von den aktuellen Datenbankwerten abrufen und Festlegen von ihnen als die ursprünglichen Werte für die Entität ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="a13fe-127">This is sometimes called client wins and can be done by getting the current database values and setting them as the original values for the entity.</span></span> <span data-ttu-id="a13fe-128">(Finden Sie unter [arbeiten mit Eigenschaftswerten](~/ef6/saving/change-tracking/property-values.md) Informationen zu aktuellen und ursprünglichen Werte.) Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a13fe-128">(See [Working with Property Values](~/ef6/saving/change-tracking/property-values.md) for information on current and original values.) For example:</span></span>  

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

## <a name="custom-resolution-of-optimistic-concurrency-exceptions"></a><span data-ttu-id="a13fe-129">Benutzerdefinierte Auflösung der Ausnahmen bzgl. vollständiger Parallelität</span><span class="sxs-lookup"><span data-stu-id="a13fe-129">Custom resolution of optimistic concurrency exceptions</span></span>  

<span data-ttu-id="a13fe-130">In einigen Fällen empfiehlt es sich um den aktuellen Werten in der Datenbank mit den aktuellen Werten in der Entität zu kombinieren.</span><span class="sxs-lookup"><span data-stu-id="a13fe-130">Sometimes you may want to combine the values currently in the database with the values currently in the entity.</span></span> <span data-ttu-id="a13fe-131">Dies ist in der Regel einige benutzerdefinierte Logik oder Benutzerinteraktion erforderlich.</span><span class="sxs-lookup"><span data-stu-id="a13fe-131">This usually requires some custom logic or user interaction.</span></span> <span data-ttu-id="a13fe-132">Beispielsweise können Sie ein Formular für dem Benutzer, die die aktuellen Werte, die Werte in der Datenbank darstellen, und eine Standardsammlung von aufgelösten Werte.</span><span class="sxs-lookup"><span data-stu-id="a13fe-132">For example, you might present a form to the user containing the current values, the values in the database, and a default set of resolved values.</span></span> <span data-ttu-id="a13fe-133">Der Benutzer klicken Sie dann die aufgelösten Werte nach Bedarf bearbeiten, und es wäre diese aufgelösten Werte, die in der Datenbank gespeichert.</span><span class="sxs-lookup"><span data-stu-id="a13fe-133">The user would then edit the resolved values as necessary and it would be these resolved values that get saved to the database.</span></span> <span data-ttu-id="a13fe-134">Dies kann erfolgen mithilfe der Objekte DbPropertyValues CurrentValues und GetDatabaseValues auf der Entität Eintrag zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="a13fe-134">This can be done using the DbPropertyValues objects returned from CurrentValues and GetDatabaseValues on the entity’s entry.</span></span> <span data-ttu-id="a13fe-135">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a13fe-135">For example:</span></span>  

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

## <a name="custom-resolution-of-optimistic-concurrency-exceptions-using-objects"></a><span data-ttu-id="a13fe-136">Benutzerdefinierte Auflösung der Ausnahmen bzgl. vollständiger Parallelität, die mithilfe von Objekten</span><span class="sxs-lookup"><span data-stu-id="a13fe-136">Custom resolution of optimistic concurrency exceptions using objects</span></span>  

<span data-ttu-id="a13fe-137">Der obige Code verwendet die DbPropertyValues-Instanzen für die Weitergabe der aktuellen Datenbank und aufgelösten Werte.</span><span class="sxs-lookup"><span data-stu-id="a13fe-137">The code above uses DbPropertyValues instances for passing around current, database, and resolved values.</span></span> <span data-ttu-id="a13fe-138">Manchmal kann es einfacher, verwenden Instanzen des Entitätstyps für diesen sein.</span><span class="sxs-lookup"><span data-stu-id="a13fe-138">Sometimes it may be easier to use instances of your entity type for this.</span></span> <span data-ttu-id="a13fe-139">Dies kann erfolgen mithilfe der ToObject und SetValues von DbPropertyValues.</span><span class="sxs-lookup"><span data-stu-id="a13fe-139">This can be done using the ToObject and SetValues methods of DbPropertyValues.</span></span> <span data-ttu-id="a13fe-140">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="a13fe-140">For example:</span></span>  

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
