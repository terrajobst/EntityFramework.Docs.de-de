---
title: Arbeiten mit dbcontext-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
ms.openlocfilehash: d961ffd8bed7f5b2f82dcfa30fc0241b7437be50
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413882"
---
# <a name="working-with-dbcontext"></a><span data-ttu-id="7ecf6-102">Arbeiten mit DbContext</span><span class="sxs-lookup"><span data-stu-id="7ecf6-102">Working with DbContext</span></span>

<span data-ttu-id="7ecf6-103">Um Entity Framework zum Abfragen, einfügen, aktualisieren und Löschen von Daten mithilfe von .NET-Objekten verwenden zu können, müssen Sie zunächst [ein Modell erstellen](~/ef6/modeling/index.md) , das die im Modell definierten Entitäten und Beziehungen den Tabellen in einer Datenbank zuordnet.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-103">In order to use Entity Framework to query, insert, update, and delete data using .NET objects, you first need to [Create a Model](~/ef6/modeling/index.md) which maps the entities and relationships that are defined in your model to tables in a database.</span></span>

<span data-ttu-id="7ecf6-104">Sobald Sie über ein Modell verfügen, wird die primäre Klasse, mit der Ihre Anwendung interagiert, `System.Data.Entity.DbContext` (häufig als Kontext Klasse bezeichnet).</span><span class="sxs-lookup"><span data-stu-id="7ecf6-104">Once you have a model, the primary class your application interacts with is `System.Data.Entity.DbContext` (often referred to as the context class).</span></span> <span data-ttu-id="7ecf6-105">Sie können einen mit einem Modell verknüpften dbcontext für Folgendes verwenden:</span><span class="sxs-lookup"><span data-stu-id="7ecf6-105">You can use a DbContext associated to a model to:</span></span>
- <span data-ttu-id="7ecf6-106">Schreiben und Ausführen von Abfragen</span><span class="sxs-lookup"><span data-stu-id="7ecf6-106">Write and execute queries</span></span>   
- <span data-ttu-id="7ecf6-107">Materialisieren von Abfrage Ergebnissen als Entitäts Objekte</span><span class="sxs-lookup"><span data-stu-id="7ecf6-107">Materialize query results as entity objects</span></span>
- <span data-ttu-id="7ecf6-108">An diesen Objekten vorgenommene Änderungen nachverfolgen</span><span class="sxs-lookup"><span data-stu-id="7ecf6-108">Track changes that are made to those objects</span></span>
- <span data-ttu-id="7ecf6-109">Objektänderungen in der Datenbank beibehalten</span><span class="sxs-lookup"><span data-stu-id="7ecf6-109">Persist object changes back on the database</span></span>
- <span data-ttu-id="7ecf6-110">Binden von Objekten im Arbeitsspeicher an UI-Steuerelemente</span><span class="sxs-lookup"><span data-stu-id="7ecf6-110">Bind objects in memory to UI controls</span></span>

<span data-ttu-id="7ecf6-111">Diese Seite enthält eine Anleitung zum Verwalten der Kontext Klasse.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-111">This page gives some guidance on how to manage the context class.</span></span>  

## <a name="defining-a-dbcontext-derived-class"></a><span data-ttu-id="7ecf6-112">Definieren einer von dbcontext abgeleiteten Klasse</span><span class="sxs-lookup"><span data-stu-id="7ecf6-112">Defining a DbContext derived class</span></span>  

<span data-ttu-id="7ecf6-113">Die empfohlene Vorgehensweise, um mit Kontext zu arbeiten, besteht darin, eine Klasse zu definieren, die von dbcontext abgeleitet ist und dbset-Eigenschaften verfügbar macht, die Sammlungen der angegebenen Entitäten im Kontext darstellen.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-113">The recommended way to work with context is to define a class that derives from DbContext and exposes DbSet properties that represent collections of the specified entities in the context.</span></span> <span data-ttu-id="7ecf6-114">Wenn Sie mit dem EF-Designer arbeiten, wird der Kontext für Sie generiert.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-114">If you are working with the EF Designer, the context will be generated for you.</span></span> <span data-ttu-id="7ecf6-115">Wenn Sie mit Code First arbeiten, schreiben Sie den Kontext in der Regel selbst.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-115">If you are working with Code First, you will typically write the context yourself.</span></span>  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

<span data-ttu-id="7ecf6-116">Wenn Sie über einen Kontext verfügen, können Sie über diese Eigenschaften Abfragen, hinzufügen (mit `Add` oder `Attach` Methoden) oder (mithilfe von `Remove`) Entitäten im Kontext entfernen.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-116">Once you have a context, you would query for, add (using `Add` or `Attach` methods ) or remove (using `Remove`) entities in the context through these properties.</span></span> <span data-ttu-id="7ecf6-117">Der Zugriff auf eine `DbSet`-Eigenschaft für ein Kontext Objekt stellt eine Start Abfrage dar, die alle Entitäten des angegebenen Typs zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-117">Accessing a `DbSet` property on a context object represent a starting query that returns all entities of the specified type.</span></span> <span data-ttu-id="7ecf6-118">Beachten Sie, dass beim Zugreifen auf eine Eigenschaft die Abfrage nicht ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-118">Note that just accessing a property will not execute the query.</span></span> <span data-ttu-id="7ecf6-119">Eine Abfrage wird ausgeführt, wenn Folgendes gilt:</span><span class="sxs-lookup"><span data-stu-id="7ecf6-119">A query is executed when:</span></span>  

- <span data-ttu-id="7ecf6-120">Sie wird durch eine `foreach` (C#)-Anweisung oder eine `For Each` (Visual Basic)-Anweisung aufgezählt.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-120">It is enumerated by a `foreach` (C#) or `For Each` (Visual Basic) statement.</span></span>  
- <span data-ttu-id="7ecf6-121">Sie wird durch einen Auflistungs Vorgang, z. b. `ToArray`, `ToDictionary`oder `ToList`, aufgezählt.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-121">It is enumerated by a collection operation such as `ToArray`, `ToDictionary`, or `ToList`.</span></span>  
- <span data-ttu-id="7ecf6-122">LINQ-Operatoren, wie z. b. `First` oder `Any`, werden im äußersten Teil der Abfrage angegeben.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-122">LINQ operators such as `First` or `Any` are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="7ecf6-123">Eine der folgenden Methoden wird aufgerufen: die `Load`-Erweiterungsmethode, `DbEntityEntry.Reload`, `Database.ExecuteSqlCommand`und `DbSet<T>.Find`, wenn eine Entität mit dem angegebenen Schlüssel nicht gefunden wurde, die bereits im Kontext geladen wurde.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-123">One of the following methods are called: the `Load` extension method, `DbEntityEntry.Reload`,  `Database.ExecuteSqlCommand`, and `DbSet<T>.Find`, if an entity with the specified key is not found already loaded in the context.</span></span>  

## <a name="lifetime"></a><span data-ttu-id="7ecf6-124">Gültigkeitsdauer</span><span class="sxs-lookup"><span data-stu-id="7ecf6-124">Lifetime</span></span>  

<span data-ttu-id="7ecf6-125">Die Lebensdauer des Kontexts beginnt, wenn die Instanz erstellt wird, und endet, wenn die Instanz entweder verworfen oder Garbage Collection durchgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-125">The lifetime of the context begins when the instance is created and ends when the instance is either disposed or garbage-collected.</span></span> <span data-ttu-id="7ecf6-126">Verwenden Sie **die Verwendung von, wenn alle** Ressourcen, die die Kontext Steuerelemente haben, am Ende des Blocks verworfen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-126">Use **using** if you want all the resources that the context controls to be disposed at the end of the block.</span></span> <span data-ttu-id="7ecf6-127">Wenn Sie **mit**verwenden, erstellt der Compiler automatisch einen try/after-Block und ruft **die Freigabe im letzten Block auf** .</span><span class="sxs-lookup"><span data-stu-id="7ecf6-127">When you use **using**, the compiler automatically creates a try/finally block and calls dispose in the **finally** block.</span></span>  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

<span data-ttu-id="7ecf6-128">Im folgenden finden Sie einige allgemeine Richtlinien für die Entscheidung über die Lebensdauer des Kontexts:</span><span class="sxs-lookup"><span data-stu-id="7ecf6-128">Here are some general guidelines when deciding on the lifetime of the context:</span></span>  

- <span data-ttu-id="7ecf6-129">Verwenden Sie bei der Arbeit mit Webanwendungen eine Kontext Instanz pro Anforderung.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-129">When working with Web applications, use a context instance per request.</span></span>  
- <span data-ttu-id="7ecf6-130">Verwenden Sie beim Arbeiten mit Windows Presentation Foundation (WPF) oder Windows Forms eine Kontext Instanz pro Formular.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-130">When working with Windows Presentation Foundation (WPF) or Windows Forms, use a context instance per form.</span></span> <span data-ttu-id="7ecf6-131">Auf diese Weise können Sie Funktionen zur Änderungs Nachverfolgung verwenden, die der Kontext bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-131">This lets you use change-tracking functionality that context provides.</span></span>  
- <span data-ttu-id="7ecf6-132">Wenn die Kontext Instanz von einem Container für die Abhängigkeitsinjektion erstellt wird, liegt es in der Regel in der Verantwortung des Containers, den Kontext zu verwerfen.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-132">If the context instance is created by a dependency injection container, it is usually the responsibility of the container to dispose the context.</span></span>
- <span data-ttu-id="7ecf6-133">Wenn der Kontext im Anwendungscode erstellt wird, sollten Sie den Kontext verwerfen, wenn er nicht mehr benötigt wird.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-133">If the context is created in application code, remember to dispose of the context when it is no longer required.</span></span>  
- <span data-ttu-id="7ecf6-134">Beim Arbeiten mit einem Kontext mit langer Ausführungszeit sollten Sie Folgendes beachten:</span><span class="sxs-lookup"><span data-stu-id="7ecf6-134">When working with long-running context consider the following:</span></span>  
    - <span data-ttu-id="7ecf6-135">Wenn Sie weitere Objekte und ihre Verweise in den Arbeitsspeicher laden, kann sich die Arbeitsspeicher Nutzung des Kontexts schnell erhöhen.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-135">As you load more objects and their references into memory, the memory consumption of the context may increase rapidly.</span></span> <span data-ttu-id="7ecf6-136">Dies kann zu Leistungseinbußen führen.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-136">This may cause performance issues.</span></span>  
    - <span data-ttu-id="7ecf6-137">Der Kontext ist nicht Thread sicher und sollte daher nicht für mehrere Threads freigegeben werden, die gleichzeitig daran arbeiten.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-137">The context is not thread-safe, therefore it should not be shared across multiple threads doing work on it concurrently.</span></span>
    - <span data-ttu-id="7ecf6-138">Wenn eine Ausnahme bewirkt, dass sich der Kontext in einem nicht wiederherstellbaren Zustand befindet, kann die gesamte Anwendung beendet werden.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-138">If an exception causes the context to be in an unrecoverable state, the whole application may terminate.</span></span>  
    - <span data-ttu-id="7ecf6-139">Je größer der zeitliche Abstand zwischen der Abfrage der Daten und ihrer Aktualisierung, desto höher ist die Wahrscheinlichkeit, dass Parallelitätsprobleme auftreten.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-139">The chances of running into concurrency-related issues increase as the gap between the time when the data is queried and updated grows.</span></span>  

## <a name="connections"></a><span data-ttu-id="7ecf6-140">Verbindungen</span><span class="sxs-lookup"><span data-stu-id="7ecf6-140">Connections</span></span>  

<span data-ttu-id="7ecf6-141">Standardmäßig verwaltet der Kontext Verbindungen mit der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-141">By default, the context manages connections to the database.</span></span> <span data-ttu-id="7ecf6-142">Der Kontext wird geöffnet und schließt die Verbindungen nach Bedarf.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-142">The context opens and closes connections as needed.</span></span> <span data-ttu-id="7ecf6-143">Der Kontext öffnet z. b. eine Verbindung, um eine Abfrage auszuführen, und schließt dann die Verbindung, wenn alle Resultsets verarbeitet wurden.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-143">For example, the context opens a connection to execute a query, and then closes the connection when all the result sets have been processed.</span></span>  

<span data-ttu-id="7ecf6-144">In bestimmten Fällen ist es erforderlich, den Zeitpunkt für das Öffnen und Schließen der Verbindung genauer zu steuern.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-144">There are cases when you want to have more control over when the connection opens and closes.</span></span> <span data-ttu-id="7ecf6-145">Wenn Sie z. b. mit SQL Server Compact arbeiten, wird häufig empfohlen, für die Lebensdauer der Anwendung eine separate offene Verbindung mit der Datenbank beizubehalten, um die Leistung zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-145">For example, when working with SQL Server Compact, it is often recommended to maintain a separate open connection to the database for the lifetime of the application to improve performance.</span></span> <span data-ttu-id="7ecf6-146">Mit der `Connection`-Eigenschaft können Sie diesen Vorgang manuell steuern.</span><span class="sxs-lookup"><span data-stu-id="7ecf6-146">You can manage this process manually by using the `Connection` property.</span></span>  
