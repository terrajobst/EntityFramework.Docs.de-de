---
title: Arbeiten mit Status von Entitäten – EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: acb27f46-3f3a-4179-874a-d6bea5d7120c
caps.latest.revision: 3
ms.openlocfilehash: e27d70dd7a6a63c2b225e9cf4fe7ac8cc280d4ca
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2018
ms.locfileid: "39120808"
---
# <a name="working-with-entity-states"></a><span data-ttu-id="f6cc8-102">Arbeiten mit Status von Entitäten</span><span class="sxs-lookup"><span data-stu-id="f6cc8-102">Working with entity states</span></span>
<span data-ttu-id="f6cc8-103">In diesem Thema wird behandelt, das Hinzufügen und Anfügen von Entitäten an einen Kontext und wie Entity Framework diese während SaveChanges verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-103">This topic will cover how to add and attach entities to a context and how Entity Framework processes these during SaveChanges.</span></span>
<span data-ttu-id="f6cc8-104">Entitätsframework übernimmt Überwachung, die der Zustand der Entitäten, während sie mit einem Kontext verbunden sind, aber in nicht verbundenen oder N-Tier-Szenarien Sie EF wissen, welcher Zustand Ihrer Entitäten können auf sein soll.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-104">Entity Framework takes care of tracking the state of entities while they are connected to a context, but in disconnected or N-Tier scenarios you can let EF know what state your entities should be in.</span></span>
<span data-ttu-id="f6cc8-105">Die in diesem Thema dargestellten Techniken gelten jeweils für Modelle, die mit Code First und dem EF-Designer erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="entity-states-and-savechanges"></a><span data-ttu-id="f6cc8-106">Status von Entitäten und "SaveChanges"</span><span class="sxs-lookup"><span data-stu-id="f6cc8-106">Entity states and SaveChanges</span></span>

<span data-ttu-id="f6cc8-107">Eine Entität kann einen der fünf Status sein, wie durch die "EntityState"-Enumeration definiert.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-107">An entity can be in one of five states as defined by the EntityState enumeration.</span></span> <span data-ttu-id="f6cc8-108">Folgende Status sind möglich:</span><span class="sxs-lookup"><span data-stu-id="f6cc8-108">These states are:</span></span>  

- <span data-ttu-id="f6cc8-109">Hinzugefügt: die Entität vom Kontext nachverfolgt wird, aber noch nicht in der Datenbank vorhanden</span><span class="sxs-lookup"><span data-stu-id="f6cc8-109">Added: the entity is being tracked by the context but does not yet exist in the database</span></span>  
- <span data-ttu-id="f6cc8-110">Unchanged: die Entität vom Kontext nachverfolgt wird, und in der Datenbank vorhanden ist und die Eigenschaftswerte nicht aus den Werten in der Datenbank geändert haben</span><span class="sxs-lookup"><span data-stu-id="f6cc8-110">Unchanged: the entity is being tracked by the context and exists in the database, and its property values have not changed from the values in the database</span></span>  
- <span data-ttu-id="f6cc8-111">Geänderte: die Entität vom Kontext nachverfolgt wird, und in der Datenbank vorhanden ist, und einige oder alle der Eigenschaftswerte geändert wurden</span><span class="sxs-lookup"><span data-stu-id="f6cc8-111">Modified: the entity is being tracked by the context and exists in the database, and some or all of its property values have been modified</span></span>  
- <span data-ttu-id="f6cc8-112">Gelöschte: die Entität vom Kontext nachverfolgt wird, und in der Datenbank vorhanden, aber wurde zum Löschen aus der Datenbank das nächste Mal, das aufgerufen wird, "SaveChanges" markiert</span><span class="sxs-lookup"><span data-stu-id="f6cc8-112">Deleted: the entity is being tracked by the context and exists in the database, but has been marked for deletion from the database the next time SaveChanges is called</span></span>  
- <span data-ttu-id="f6cc8-113">Getrennt: die Entität wird nicht vom Kontext nachverfolgt wird</span><span class="sxs-lookup"><span data-stu-id="f6cc8-113">Detached: the entity is not being tracked by the context</span></span>  

<span data-ttu-id="f6cc8-114">"SaveChanges" führt verschiedene Dinge für Entitäten in unterschiedlichen Zuständen:</span><span class="sxs-lookup"><span data-stu-id="f6cc8-114">SaveChanges does different things for entities in different states:</span></span>  

- <span data-ttu-id="f6cc8-115">Unveränderte Entitäten werden nicht von "SaveChanges" berührt werden.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-115">Unchanged entities are not touched by SaveChanges.</span></span> <span data-ttu-id="f6cc8-116">Updates werden nicht in der Datenbank für Entitäten in den Status "Unchanged" gesendet.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-116">Updates are not sent to the database for entities in the Unchanged state.</span></span>  
- <span data-ttu-id="f6cc8-117">Hinzugefügt von Entitäten, die in die Datenbank eingefügt werden, und klicken Sie dann werden nicht verändert, wenn "SaveChanges" zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-117">Added entities are inserted into the database and then become Unchanged when SaveChanges returns.</span></span>  
- <span data-ttu-id="f6cc8-118">Geänderte Entitäten werden in der Datenbank aktualisiert und dann werden nicht verändert, wenn "SaveChanges" zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-118">Modified entities are updated in the database and then become Unchanged when SaveChanges returns.</span></span>  
- <span data-ttu-id="f6cc8-119">Gelöschte Entitäten aus der Datenbank gelöscht werden, und klicken Sie dann aus dem Kontext getrennt sind.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-119">Deleted entities are deleted from the database and are then detached from the context.</span></span>  

<span data-ttu-id="f6cc8-120">Die folgenden Beispiele zeigen die Möglichkeiten, die in denen der Zustand einer Entität oder ein Diagramm für die Entität geändert werden kann.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-120">The following examples show ways in which the state of an entity or an entity graph can be changed.</span></span>  

## <a name="adding-a-new-entity-to-the-context"></a><span data-ttu-id="f6cc8-121">Hinzufügen einer neuen Entität in den Kontext</span><span class="sxs-lookup"><span data-stu-id="f6cc8-121">Adding a new entity to the context</span></span>  

<span data-ttu-id="f6cc8-122">Eine neue Entität kann in den Kontext hinzugefügt werden, indem Sie die Add-Methode für "DbSet" aufrufen.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-122">A new entity can be added to the context by calling the Add method on DbSet.</span></span>
<span data-ttu-id="f6cc8-123">Dies versetzt die Entität auf, in dem Status "Added", Sie dies bedeutet, dass es in der Datenbank das nächste Mal eingefügt wird, das "SaveChanges" aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-123">This puts the entity into the Added state, meaning that it will be inserted into the database the next time that SaveChanges is called.</span></span>
<span data-ttu-id="f6cc8-124">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f6cc8-124">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Blogs.Add(blog);
    context.SaveChanges();
}
```  

<span data-ttu-id="f6cc8-125">Eine weitere Möglichkeit zum Hinzufügen einer neuen Entität in den Kontext ist, dessen Zustand Added zu ändern.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-125">Another way to add a new entity to the context is to change its state to Added.</span></span> <span data-ttu-id="f6cc8-126">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f6cc8-126">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Entry(blog).State = EntityState.Added;
    context.SaveChanges();
}
```  

<span data-ttu-id="f6cc8-127">Schließlich können Sie eine neue Entität in den Kontext hinzufügen, indem Sie binden es in einer anderen Entität, die bereits nachverfolgt wird.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-127">Finally, you can add a new entity to the context by hooking it up to another entity that is already being tracked.</span></span>
<span data-ttu-id="f6cc8-128">Dies kann durch die auflistungsnavigationseigenschaft einer anderen Entität die neue Entität hinzugefügt oder durch Festlegen einer Verweiseigenschaft für die Navigation von einer anderen Entität, zeigen Sie auf die neue Entität sein.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-128">This could be by adding the new entity to the collection navigation property of another entity or by setting a reference navigation property of another entity to point to the new entity.</span></span> <span data-ttu-id="f6cc8-129">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f6cc8-129">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Add a new User by setting a reference from a tracked Blog
    var blog = context.Blogs.Find(1);
    blog.Owner = new User { UserName = "johndoe1987" };

    // Add a new Post by adding to the collection of a tracked Blog
    var blog = context.Blogs.Find(2);
    blog.Posts.Add(new Post { Name = "How to Add Entities" });

    context.SaveChanges();
}
```  

<span data-ttu-id="f6cc8-130">Beachten Sie, die für alle diese Beispiele aus, wenn die Entität, die hinzuzufügenden Verweise auf andere Entitäten verfügt, die nicht noch klicken Sie dann diese neuen Entitäten nachverfolgt wird auch zum Kontext hinzugefügt werden und wird eingefügt werden in die Datenbank das nächste Mal, das "SaveChanges" aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-130">Note that for all of these examples if the entity being added has references to other entities that are not yet tracked then these new entities will also be added to the context and will be inserted into the database the next time that SaveChanges is called.</span></span>  

## <a name="attaching-an-existing-entity-to-the-context"></a><span data-ttu-id="f6cc8-131">Eine vorhandene Entität anzufügen an den Kontext.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-131">Attaching an existing entity to the context</span></span>  

<span data-ttu-id="f6cc8-132">Wenn man eine Entität, die Sie bereits wissen, in der Datenbank, aber die nicht derzeit vom Kontext nachverfolgt wird und Sie, den Kontext erkennen können, der die Entität, die mithilfe der Attach-Methode für "DbSet" Nachverfolgen vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-132">If you have an entity that you know already exists in the database but which is not currently being tracked by the context then you can tell the context to track the entity using the Attach method on DbSet.</span></span> <span data-ttu-id="f6cc8-133">Die Entität werden im Zustand Unchanged im Kontext.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-133">The entity will be in the Unchanged state in the context.</span></span> <span data-ttu-id="f6cc8-134">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f6cc8-134">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="f6cc8-135">Beachten Sie, dass keine Änderungen in der Datenbank vorgenommen werden, werden wenn SaveChanges aufgerufen wird, ohne andere Änderungen an den angefügten Entität.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-135">Note that no changes will be made to the database if SaveChanges is called without doing any other manipulation of the attached entity.</span></span> <span data-ttu-id="f6cc8-136">Dies ist, da die Entität im Zustand Unchanged ist.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-136">This is because the entity is in the Unchanged state.</span></span>  

<span data-ttu-id="f6cc8-137">Eine weitere Möglichkeit zum Anfügen einer vorhandenen Entität an den Kontext ist so ändern Sie den Zustand auf Unchanged.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-137">Another way to attach an existing entity to the context is to change its state to Unchanged.</span></span> <span data-ttu-id="f6cc8-138">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f6cc8-138">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="f6cc8-139">Beachten Sie, dass für diesen beiden Beispielen verfügt die Entität angefügt werden Verweise auf andere Entitäten, die noch nicht verfolgt werden dann diese neuen Entitäten wird auch an der der Kontext, in dem Status "Unchanged".</span><span class="sxs-lookup"><span data-stu-id="f6cc8-139">Note that for both of these examples if the entity being attached has references to other entities that are not yet tracked then these new entities will also attached to the context in the Unchanged state.</span></span>  

## <a name="attaching-an-existing-but-modified-entity-to-the-context"></a><span data-ttu-id="f6cc8-140">Anfügen einer vorhandenen geänderte Entität an den Kontext</span><span class="sxs-lookup"><span data-stu-id="f6cc8-140">Attaching an existing but modified entity to the context</span></span>  

<span data-ttu-id="f6cc8-141">Wenn man ist eine Entität, die bereits vorhanden, in der Datenbank jedoch, welche Änderungen möglicherweise vorgenommen wurden und Sie, dass den Kontext feststellen können, fügen Sie die Entität aus, und legen seinen Status auf "geändert".</span><span class="sxs-lookup"><span data-stu-id="f6cc8-141">If you have an entity that you know already exists in the database but to which changes may have been made then you can tell the context to attach the entity and set its state to Modified.</span></span>
<span data-ttu-id="f6cc8-142">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f6cc8-142">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Modified;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="f6cc8-143">Wenn Sie den Status in "geändert" ändern, alle Eigenschaften der Entität als geändert markiert, und alle Eigenschaftswerte werden an die Datenbank gesendet werden, wenn SaveChanges aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-143">When you change the state to Modified all the properties of the entity will be marked as modified and all the property values will be sent to the database when SaveChanges is called.</span></span>  

<span data-ttu-id="f6cc8-144">Beachten Sie, dass wenn die Entität, die angefügt werden Verweise auf andere Entitäten verfügt, die noch nicht nachverfolgt werden, klicken Sie dann diese neuen Entitäten wird an der der Kontext, in dem Status "Unchanged" – sie werden nicht automatisch durchgeführt "geändert".</span><span class="sxs-lookup"><span data-stu-id="f6cc8-144">Note that if the entity being attached has references to other entities that are not yet tracked, then these new entities will attached to the context in the Unchanged state—they will not automatically be made Modified.</span></span>
<span data-ttu-id="f6cc8-145">Wenn Sie über mehrere Entitäten verfügen, die müssen "geändert" markiert werden, sollten Sie den Status für jede dieser Entitäten einzeln festlegen.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-145">If you have multiple entities that need to be marked Modified you should set the state for each of these entities individually.</span></span>  

## <a name="changing-the-state-of-a-tracked-entity"></a><span data-ttu-id="f6cc8-146">Ändern des Status einer verfolgten Entität</span><span class="sxs-lookup"><span data-stu-id="f6cc8-146">Changing the state of a tracked entity</span></span>  

<span data-ttu-id="f6cc8-147">Sie können den Status einer Entität ändern, die bereits nachverfolgt wird durch Festlegen der State-Eigenschaft auf den Eintrag.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-147">You can change the state of an entity that is already being tracked by setting the State property on its entry.</span></span> <span data-ttu-id="f6cc8-148">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f6cc8-148">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="f6cc8-149">Beachten Sie, dass aufrufen hinzufügen oder Anfügen einer Entität, das bereits nachverfolgt wird auch verwendet werden kann, den Entitätszustand ändern.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-149">Note that calling Add or Attach for an entity that is already tracked can also be used to change the entity state.</span></span> <span data-ttu-id="f6cc8-150">Beispielsweise wird durch Aufrufen von Anfügen für eine Entität, die derzeit im Zustand Added wird seinen Zustand auf Unchanged geändert.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-150">For example, calling Attach for an entity that is currently in the Added state will change its state to Unchanged.</span></span>  

## <a name="insert-or-update-pattern"></a><span data-ttu-id="f6cc8-151">Einfügen oder Aktualisieren der Muster</span><span class="sxs-lookup"><span data-stu-id="f6cc8-151">Insert or update pattern</span></span>  

<span data-ttu-id="f6cc8-152">Ein häufiges Muster für einige Anwendungen ist eine Entität als neue (wodurch das Einfügen einer Datenbank) hinzufügen oder Anfügen einer Entität wie vorhandene und markieren Sie ihn als geändert (was in einem Datenbankupdate) je nach dem Wert des Primärschlüssels.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-152">A common pattern for some applications is to either Add an entity as new (resulting in a database insert) or Attach an entity as existing and mark it as modified (resulting in a database update) depending on the value of the primary key.</span></span>
<span data-ttu-id="f6cc8-153">Beispielsweise ist bei Verwendung der Datenbank generierte Primärschlüsseln ganzer Zahlen als es üblich, eine Entität mit einem NULL-Schlüssel als neu und einer Entität mit einem NULL-Schlüssel wie vorhandene behandelt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-153">For example, when using database generated integer primary keys it is common to treat an entity with a zero key as new and an entity with a non-zero key as existing.</span></span>
<span data-ttu-id="f6cc8-154">Dieses Muster kann erreicht werden, durch Festlegen den Entitätszustand basierend auf dem Wert des Primärschlüssels zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-154">This pattern can be achieved by setting the entity state based on a check of the primary key value.</span></span> <span data-ttu-id="f6cc8-155">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f6cc8-155">For example:</span></span>  

``` csharp
public void InsertOrUpdate(Blog blog)
{
    using (var context = new BloggingContext())
    {
        context.Entry(blog).State = blog.BlogId == 0 ?
                                   EntityState.Added :
                                   EntityState.Modified;

        context.SaveChanges();
    }
}
```  

<span data-ttu-id="f6cc8-156">Beachten Sie, dass wenn Sie den Status in "geändert" ändern, werden alle Eigenschaften der Entität als geändert markiert, und alle Eigenschaftswerte werden an die Datenbank gesendet werden, wenn SaveChanges aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="f6cc8-156">Note that when you change the state to Modified all the properties of the entity will be marked as modified and all the property values will be sent to the database when SaveChanges is called.</span></span>  
