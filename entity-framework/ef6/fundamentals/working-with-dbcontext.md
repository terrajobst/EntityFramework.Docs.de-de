---
title: Arbeiten mit "DbContext" - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
ms.openlocfilehash: f95f503c4e40e65503d5af0c1b686d0055728bfe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998145"
---
# <a name="working-with-dbcontext"></a><span data-ttu-id="bc01f-102">Arbeiten mit "DbContext"</span><span class="sxs-lookup"><span data-stu-id="bc01f-102">Working with DbContext</span></span>

<span data-ttu-id="bc01f-103">Um Entity Framework zum Abfragen, einfügen, aktualisieren und Löschen von Daten mithilfe von .NET-Objekten verwenden, müssen Sie zuerst [erstellen Sie ein Modell](~/ef6/modeling/index.md) der zugeordnet werden, die Entitäten und Beziehungen, die in Ihrem Modell, Tabellen in einer Datenbank definiert sind.</span><span class="sxs-lookup"><span data-stu-id="bc01f-103">In order to use Entity Framework to query, insert, update, and delete data using .NET objects, you first need to [Create a Model](~/ef6/modeling/index.md) which maps the entities and relationships that are defined in your model to tables in a database.</span></span>

<span data-ttu-id="bc01f-104">Sobald Sie ein Modell verfügen, ist die primäre Klasse, die Ihre Anwendung interagiert mit `System.Data.Entity.DbContext` (häufig als der Context-Klasse bezeichnet).</span><span class="sxs-lookup"><span data-stu-id="bc01f-104">Once you have a model, the primary class your application interacts with is `System.Data.Entity.DbContext` (often referred to as the context class).</span></span> <span data-ttu-id="bc01f-105">Sie können einen "DbContext", die ein Modell zugeordnet sind:</span><span class="sxs-lookup"><span data-stu-id="bc01f-105">You can use a DbContext associated to a model to:</span></span>
- <span data-ttu-id="bc01f-106">Schreiben und Ausführen von Abfragen</span><span class="sxs-lookup"><span data-stu-id="bc01f-106">Write and execute queries</span></span>   
- <span data-ttu-id="bc01f-107">Abfrageergebnisse als Entitätsobjekte zu materialisieren.</span><span class="sxs-lookup"><span data-stu-id="bc01f-107">Materialize query results as entity objects</span></span>
- <span data-ttu-id="bc01f-108">Nachverfolgen von Änderungen, die mit diesen Objekten vorgenommen werden</span><span class="sxs-lookup"><span data-stu-id="bc01f-108">Track changes that are made to those objects</span></span>
- <span data-ttu-id="bc01f-109">Speichern von objektänderungen an die Datenbank</span><span class="sxs-lookup"><span data-stu-id="bc01f-109">Persist object changes back on the database</span></span>
- <span data-ttu-id="bc01f-110">Binden von Objekten im Arbeitsspeicher, an der UI-Steuerelemente</span><span class="sxs-lookup"><span data-stu-id="bc01f-110">Bind objects in memory to UI controls</span></span>

<span data-ttu-id="bc01f-111">Diese Seite bietet hilfreiche Informationen zur Verwaltung der Context-Klasse.</span><span class="sxs-lookup"><span data-stu-id="bc01f-111">This page gives some guidance on how to manage the context class.</span></span>  

## <a name="defining-a-dbcontext-derived-class"></a><span data-ttu-id="bc01f-112">Definieren einer Klasse "DbContext" abgeleitet</span><span class="sxs-lookup"><span data-stu-id="bc01f-112">Defining a DbContext derived class</span></span>  

<span data-ttu-id="bc01f-113">Die empfohlene Methode zum Arbeiten mit Kontext ist eine Klasse definiert, die von "DbContext" abgeleitet und stellt "DbSet"-Eigenschaften, die Auflistungen der angegebenen Entitäten im Kontext darstellen.</span><span class="sxs-lookup"><span data-stu-id="bc01f-113">The recommended way to work with context is to define a class that derives from DbContext and exposes DbSet properties that represent collections of the specified entities in the context.</span></span> <span data-ttu-id="bc01f-114">Wenn Sie mit dem EF Designer arbeiten, wird der Kontext für Sie generiert.</span><span class="sxs-lookup"><span data-stu-id="bc01f-114">If you are working with the EF Designer, the context will be generated for you.</span></span> <span data-ttu-id="bc01f-115">Wenn Sie mit Code First arbeiten, werden Sie in der Regel den Kontext selbst schreiben.</span><span class="sxs-lookup"><span data-stu-id="bc01f-115">If you are working with Code First, you will typically write the context yourself.</span></span>  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

<span data-ttu-id="bc01f-116">Nachdem Sie einen Kontext haben, würden Sie für Abfragen, hinzufügen (mithilfe von `Add` oder `Attach` Methoden) oder zu entfernen (mit `Remove`) Entitäten im Zusammenhang mit diesen Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="bc01f-116">Once you have a context, you would query for, add (using `Add` or `Attach` methods ) or remove (using `Remove`) entities in the context through these properties.</span></span> <span data-ttu-id="bc01f-117">Zugreifen auf eine `DbSet` Eigenschaft auf ein Context-Objekt darstellt, eine Startabfrage, die alle Entitäten des angegebenen Typs zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="bc01f-117">Accessing a `DbSet` property on a context object represent a starting query that returns all entities of the specified type.</span></span> <span data-ttu-id="bc01f-118">Beachten Sie, dass nur Zugriff auf eine Eigenschaft der Abfrage nicht ausgeführt werden soll.</span><span class="sxs-lookup"><span data-stu-id="bc01f-118">Note that just accessing a property will not execute the query.</span></span> <span data-ttu-id="bc01f-119">Abfrage wird ausgeführt, wenn:</span><span class="sxs-lookup"><span data-stu-id="bc01f-119">A query is executed when:</span></span>  

- <span data-ttu-id="bc01f-120">Sie wird durch eine `foreach` (C#)-Anweisung oder eine `For Each` (Visual Basic)-Anweisung aufgezählt.</span><span class="sxs-lookup"><span data-stu-id="bc01f-120">It is enumerated by a `foreach` (C#) or `For Each` (Visual Basic) statement.</span></span>  
- <span data-ttu-id="bc01f-121">Es aufgezählt wird durch einen Auflistungsvorgang z. B. `ToArray`, `ToDictionary`, oder `ToList`.</span><span class="sxs-lookup"><span data-stu-id="bc01f-121">It is enumerated by a collection operation such as `ToArray`, `ToDictionary`, or `ToList`.</span></span>  
- <span data-ttu-id="bc01f-122">LINQ-Operatoren, z. B. `First` oder `Any` im äußersten Teil der Abfrage angegeben sind.</span><span class="sxs-lookup"><span data-stu-id="bc01f-122">LINQ operators such as `First` or `Any` are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="bc01f-123">Eine der folgenden Methoden aufgerufen werden: die `Load` Erweiterungsmethode `DbEntityEntry.Reload`, `Database.ExecuteSqlCommand`, und `DbSet<T>.Find`, wenn eine Entität mit dem angegebenen Schlüssel nicht bereits geladen, im Kontext gefunden wird.</span><span class="sxs-lookup"><span data-stu-id="bc01f-123">One of the following methods are called: the `Load` extension method, `DbEntityEntry.Reload`,  `Database.ExecuteSqlCommand`, and `DbSet<T>.Find`, if an entity with the specified key is not found already loaded in the context.</span></span>  

## <a name="lifetime"></a><span data-ttu-id="bc01f-124">Lebensdauer</span><span class="sxs-lookup"><span data-stu-id="bc01f-124">Lifetime</span></span>  

<span data-ttu-id="bc01f-125">Die Lebensdauer des Kontexts beginnt, wenn die Instanz erstellt wird, und endet, wenn die Instanz freigegeben oder Garbage Collection durchgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="bc01f-125">The lifetime of the context begins when the instance is created and ends when the instance is either disposed or garbage-collected.</span></span> <span data-ttu-id="bc01f-126">Verwendung **mit** sollten Sie alle Ressourcen, die vom Kontext gesteuerten am Ende des Blocks freigegeben werden.</span><span class="sxs-lookup"><span data-stu-id="bc01f-126">Use **using** if you want all the resources that the context controls to be disposed at the end of the block.</span></span> <span data-ttu-id="bc01f-127">Bei Verwendung von **mit**, erstellt der Compiler automatisch einen Try/finally-Block und ruft dispose im der **schließlich** Block.</span><span class="sxs-lookup"><span data-stu-id="bc01f-127">When you use **using**, the compiler automatically creates a try/finally block and calls dispose in the **finally** block.</span></span>  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

<span data-ttu-id="bc01f-128">Hier sind einige allgemeinen Richtlinien, bei der Entscheidung über die Lebensdauer des Kontexts:</span><span class="sxs-lookup"><span data-stu-id="bc01f-128">Here are some general guidelines when deciding on the lifetime of the context:</span></span>  

- <span data-ttu-id="bc01f-129">Verwenden Sie bei der Arbeit mit Webanwendungen eine Context-Instanz pro Anforderung.</span><span class="sxs-lookup"><span data-stu-id="bc01f-129">When working with Web applications, use a context instance per request.</span></span>  
- <span data-ttu-id="bc01f-130">Verwenden Sie bei der Arbeit mit Windows Presentation Foundation (WPF) oder Windows Forms eine Kontextinstanz pro Formular.</span><span class="sxs-lookup"><span data-stu-id="bc01f-130">When working with Windows Presentation Foundation (WPF) or Windows Forms, use a context instance per form.</span></span> <span data-ttu-id="bc01f-131">Dadurch können Sie die änderungsnachverfolgung zurückgreifen, Kontext bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="bc01f-131">This lets you use change-tracking functionality that context provides.</span></span>  
- <span data-ttu-id="bc01f-132">Wenn die Context-Instanz, die von einem Dependency Injection-Container erstellt wird, ist es in der Regel die Verantwortung für den Container, den Kontext zu verwerfen.</span><span class="sxs-lookup"><span data-stu-id="bc01f-132">If the context instance is created by a dependency injection container, it is usually the responsibility of the container to dispose the context.</span></span>
- <span data-ttu-id="bc01f-133">Wenn der Kontext im Anwendungscode erstellt wird, müssen Sie Kontext freigeben, wenn es nicht mehr benötigt wird.</span><span class="sxs-lookup"><span data-stu-id="bc01f-133">If the context is created in application code, remember to dispose of the context when it is no longer required.</span></span>  
- <span data-ttu-id="bc01f-134">Beim Arbeiten mit lang andauernden Kontext Folgendes:</span><span class="sxs-lookup"><span data-stu-id="bc01f-134">When working with long-running context consider the following:</span></span>  
    - <span data-ttu-id="bc01f-135">Wenn Sie weitere Objekte und ihre Verweise in den Arbeitsspeicher laden, kann die arbeitsspeichernutzung der den Kontext schnell erhöhen.</span><span class="sxs-lookup"><span data-stu-id="bc01f-135">As you load more objects and their references into memory, the memory consumption of the context may increase rapidly.</span></span> <span data-ttu-id="bc01f-136">Dies kann zu Leistungseinbußen führen.</span><span class="sxs-lookup"><span data-stu-id="bc01f-136">This may cause performance issues.</span></span>  
    - <span data-ttu-id="bc01f-137">Der Kontext ist nicht threadsicher, sollte daher nicht über mehrere Threads gleichzeitig Arbeit für sie freigegeben werden.</span><span class="sxs-lookup"><span data-stu-id="bc01f-137">The context is not thread-safe, therefore it should not be shared across multiple threads doing work on it concurrently.</span></span>
    - <span data-ttu-id="bc01f-138">Wenn eine Ausnahme bewirkt, den Kontext dass, der nicht wiederhergestellt werden, kann die gesamte Anwendung beendet.</span><span class="sxs-lookup"><span data-stu-id="bc01f-138">If an exception causes the context to be in an unrecoverable state, the whole application may terminate.</span></span>  
    - <span data-ttu-id="bc01f-139">Je größer der zeitliche Abstand zwischen der Abfrage der Daten und ihrer Aktualisierung, desto höher ist die Wahrscheinlichkeit, dass Parallelitätsprobleme auftreten.</span><span class="sxs-lookup"><span data-stu-id="bc01f-139">The chances of running into concurrency-related issues increase as the gap between the time when the data is queried and updated grows.</span></span>  

## <a name="connections"></a><span data-ttu-id="bc01f-140">Verbindungen</span><span class="sxs-lookup"><span data-stu-id="bc01f-140">Connections</span></span>  

<span data-ttu-id="bc01f-141">Standardmäßig wird der Kontext Verbindungen mit der Datenbank verwaltet.</span><span class="sxs-lookup"><span data-stu-id="bc01f-141">By default, the context manages connections to the database.</span></span> <span data-ttu-id="bc01f-142">Der Kontext wird geöffnet und schließt Verbindungen nach Bedarf.</span><span class="sxs-lookup"><span data-stu-id="bc01f-142">The context opens and closes connections as needed.</span></span> <span data-ttu-id="bc01f-143">Beispielsweise wird der Kontext öffnet eine Verbindung zum Ausführen einer Abfrage und schließt dann die Verbindung aus, wenn alle Resultsets verarbeitet wurden.</span><span class="sxs-lookup"><span data-stu-id="bc01f-143">For example, the context opens a connection to execute a query, and then closes the connection when all the result sets have been processed.</span></span>  

<span data-ttu-id="bc01f-144">In bestimmten Fällen ist es erforderlich, den Zeitpunkt für das Öffnen und Schließen der Verbindung genauer zu steuern.</span><span class="sxs-lookup"><span data-stu-id="bc01f-144">There are cases when you want to have more control over when the connection opens and closes.</span></span> <span data-ttu-id="bc01f-145">Beispielsweise wird bei der Arbeit mit SQL Server Compact oft empfohlen, eine separate offene Verbindung zur Datenbank für die Lebensdauer der Anwendung zur Verbesserung der Leistung zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="bc01f-145">For example, when working with SQL Server Compact, it is often recommended to maintain a separate open connection to the database for the lifetime of the application to improve performance.</span></span> <span data-ttu-id="bc01f-146">Mit der `Connection`-Eigenschaft können Sie diesen Vorgang manuell steuern.</span><span class="sxs-lookup"><span data-stu-id="bc01f-146">You can manage this process manually by using the `Connection` property.</span></span>  
