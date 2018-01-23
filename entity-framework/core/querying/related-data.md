---
title: "Laden von verknüpften Daten - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: ec69bb128890a1e0b72fe77014f37747585bb5a5
ms.sourcegitcommit: 3b21a7fdeddc7b3c70d9b7777b72bef61f59216c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2018
---
# <a name="loading-related-data"></a><span data-ttu-id="3ca9e-102">Laden von verknüpften Daten</span><span class="sxs-lookup"><span data-stu-id="3ca9e-102">Loading Related Data</span></span>

<span data-ttu-id="3ca9e-103">Entity Framework Core können Sie die Navigationseigenschaften zum Laden verknüpfter Entitäten im Modell zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="3ca9e-104">Es gibt drei allgemeine O/RM-Muster, die zum Laden von verknüpften Daten verwendet.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="3ca9e-105">**Unverzüglichem Laden** bedeutet, dass die verknüpften Daten aus der Datenbank als Teil der ursprünglichen Abfrage geladen werden.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="3ca9e-106">**Explizites Laden** bedeutet, dass die verwandten Daten explizit aus der Datenbank zu einem späteren Zeitpunkt geladen werden.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="3ca9e-107">**Verzögertes Laden** bedeutet, dass die verwandten Daten transparent aus der Datenbank geladen werden, wenn die Navigationseigenschaft zugegriffen wird.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span> <span data-ttu-id="3ca9e-108">Verzögertes Laden ist noch nicht mit EF Core möglich.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-108">Lazy loading is not yet possible with EF Core.</span></span>

> [!TIP]  
> <span data-ttu-id="3ca9e-109">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="3ca9e-110">Eager Loading</span><span class="sxs-lookup"><span data-stu-id="3ca9e-110">Eager loading</span></span>

<span data-ttu-id="3ca9e-111">Sie können die `Include` Methode, um verwandte Daten angeben, die in den Ergebnissen enthalten sein.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-111">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="3ca9e-112">Im folgenden Beispiel müssen die Blogs, die in den Ergebnissen zurückgegeben werden ihre `Posts` Eigenschaft, die mit den zugehörigen Beiträgen aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-112">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="3ca9e-113">Entity Framework Core wird automatisch ein Fixup-Navigationseigenschaften für alle anderen Entitäten, die zuvor in die Kontextinstanz geladen wurden.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-113">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="3ca9e-114">Damit auch, wenn Sie die Daten für eine Navigationseigenschaft explizit nicht einschließen, die Eigenschaft kann noch immer aufgefüllt werden, wenn einige oder alle verknüpften Entitäten wurden zuvor geladen.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-114">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="3ca9e-115">Sie können aufeinander bezogene Daten in mehreren Beziehungen in einer einzelnen Abfrage einschließen.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-115">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="3ca9e-116">Einschließlich mehrere Ebenen</span><span class="sxs-lookup"><span data-stu-id="3ca9e-116">Including multiple levels</span></span>

<span data-ttu-id="3ca9e-117">Drilldown kann über Beziehungen zu mehrere Stufen der verknüpften Daten mithilfe der `ThenInclude` Methode.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-117">You can drill down thru relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="3ca9e-118">Im folgende Beispiel lädt alle Blogs, Verwandte Beiträge und der Autor jedes Post.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-118">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> <span data-ttu-id="3ca9e-119">Aktuelle Versionen von Visual Studio bieten eine falsche Code Abschluss Optionen und können dazu führen, dass das richtige Ausdrücke mit Syntaxfehler gekennzeichnet wird, bei Verwendung der `ThenInclude` Methode auf, nachdem eine auflistungsnavigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-119">Current versions of Visual Studio offer incorrect code completion options and can cause correct expressions to be flagged with syntax errors when using the `ThenInclude` method after a collection navigation property.</span></span> <span data-ttu-id="3ca9e-120">Dies ist ein Symptom für ein IntelliSense-Fehler auf https://github.com/dotnet/roslyn/issues/8237 nachverfolgt.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-120">This is a symptom of an IntelliSense bug tracked at https://github.com/dotnet/roslyn/issues/8237.</span></span> <span data-ttu-id="3ca9e-121">Sie können ruhig, um diese unbegründete Syntaxfehler zu ignorieren, solange der Code korrekt ist und erfolgreich kompiliert werden.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-121">It is safe to ignore these spurious syntax errors as long as the code is correct and can be compiled successfully.</span></span> 

<span data-ttu-id="3ca9e-122">Sie können mehrere Aufrufe verkettet `ThenInclude` zu fortfahren, einschließlich der weiteren Ebenen verknüpfter Daten.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-122">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="3ca9e-123">Sie können diese Option, um verwandte Daten aus mehreren Ebenen und mehrere Stämme in derselben Abfrage umfassen alle kombinieren.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-123">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="3ca9e-124">Möglicherweise sollen mehrere verknüpfte Entitäten für eine der Entitäten, die gerade enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-124">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="3ca9e-125">Beispielsweise beim Abfragen von `Blog`e, Sie enthalten `Posts` , und klicken Sie dann beide einschließen möchten die `Author` und `Tags` von der `Posts`.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-125">For example, when querying `Blog`s, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="3ca9e-126">Zu diesem Zweck müssen Sie angeben, angefangen beim Stamm Pfad enthalten.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-126">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="3ca9e-127">Beispielsweise `Blog -> Posts -> Author` und `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-127">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="3ca9e-128">Dies bedeutet nicht, dass erhalten Sie redundante Joins, in den meisten Fällen, die EF zu konsolidieren, werden, die Joins beim Generieren von SQL.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-128">This does not mean you will get redundant joins, in most cases EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="ignored-includes"></a><span data-ttu-id="3ca9e-129">Ignoriert enthält</span><span class="sxs-lookup"><span data-stu-id="3ca9e-129">Ignored includes</span></span>

<span data-ttu-id="3ca9e-130">Wenn Sie die Abfrage ändern, sodass sie nicht mehr Instanzen des Entitätstyps, die zurückgibt mit der Abfrage wurde gestartet, werden die Operatoren Include ignoriert.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-130">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="3ca9e-131">Im folgenden Beispiel werden die Include-Operatoren basierend auf den `Blog`, aber dann die `Select` Operator wird verwendet, so ändern Sie die Abfrage aus, um einen anonymen Typ zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-131">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="3ca9e-132">Die Include-Operatoren haben in diesem Fall keine Auswirkungen.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-132">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="3ca9e-133">EF Core protokolliert standardmäßig eine Warnung bei enthalten Operatoren werden ignoriert.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-133">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="3ca9e-134">Finden Sie unter [Protokollierung](../miscellaneous/logging.md) für Weitere Informationen zur Anzeige von Protokollausgaben.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-134">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="3ca9e-135">Sie können das Verhalten ändern, wenn ein Operator Include ignoriert wird, entweder auslösen oder Unternehmen Sie nichts.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-135">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="3ca9e-136">Dies erfolgt, wenn Sie die Optionen für den Kontext - in der Regel in einrichten `DbContext.OnConfiguring`, oder im `Startup.cs` bei Verwendung von ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-136">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="3ca9e-137">Explizites Laden</span><span class="sxs-lookup"><span data-stu-id="3ca9e-137">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="3ca9e-138">Diese Funktion wurde in der EF Core 1.1 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-138">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="3ca9e-139">Sie können explizit laden, eine Navigationseigenschaft über die `DbContext.Entry(...)` API.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-139">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="3ca9e-140">Sie können auch explizit eine Navigationseigenschaft laden, durch Ausführen einer separaten Abfrage, die die verknüpften Entitäten zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-140">You can also explicitly load a navigation property by executing a seperate query that returns the related entities.</span></span> <span data-ttu-id="3ca9e-141">Wenn die änderungsnachverfolgung aktiviert ist, beim Laden einer Entität EF Core automatisch legen Sie die Navigationseigenschaften der neu geladenen workflowprozessinstanz zum Verweisen auf alle Entitäten, die bereits geladen, und legen Sie die Navigationseigenschaften der bereits geladene Entitäten zum Verweisen auf die Entität neu geladen.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-141">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="3ca9e-142">Abfragen von verknüpften Entitäten</span><span class="sxs-lookup"><span data-stu-id="3ca9e-142">Querying related entities</span></span>

<span data-ttu-id="3ca9e-143">Außerdem erhalten Sie eine LINQ-Abfrage, die den Inhalt einer Navigationseigenschaft darstellt.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-143">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="3ca9e-144">Dadurch können Sie Aktionen wie die aggregate-Operator über die verknüpften Entitäten ausführen, ohne sie in den Arbeitsspeicher laden ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-144">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="3ca9e-145">Sie können auch filtern, welche verknüpften Entitäten in den Arbeitsspeicher geladen werden.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-145">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="3ca9e-146">Lazy Loading</span><span class="sxs-lookup"><span data-stu-id="3ca9e-146">Lazy loading</span></span>

<span data-ttu-id="3ca9e-147">Verzögertes Laden wird noch von EF Core nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-147">Lazy loading is not yet supported by EF Core.</span></span> <span data-ttu-id="3ca9e-148">Sehen Sie die [verzögertes Laden Element auf unseren nachholbedarf](https://github.com/aspnet/EntityFramework/issues/3797) dieses Feature zu verfolgen.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-148">You can view the [lazy loading item on our backlog](https://github.com/aspnet/EntityFramework/issues/3797) to track this feature.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="3ca9e-149">Daten und Serialisierung</span><span class="sxs-lookup"><span data-stu-id="3ca9e-149">Related data and serialization</span></span>

<span data-ttu-id="3ca9e-150">Da EF Core wird automatisch ein Fixup-Navigationseigenschaften, Sie Zyklen in Ihre Objektdiagramm ergeben können.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-150">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="3ca9e-151">Z. B. beim Laden eines Blogs und bezieht sich Beiträge in einem Blog Objekt führt, die eine Auflistung von Beiträgen verweist.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-151">For example, Loading a blog and it's related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="3ca9e-152">Jedes dieser Beiträge weist einen Verweis zurück auf den Blog.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-152">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="3ca9e-153">Einige serialisierungsframeworks lassen sich nicht auf solche Zyklen aus.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-153">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="3ca9e-154">Json.NET wird z. B. die folgende Ausnahme ausgelöst, ist ein Zyklus auftritt.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-154">For example, Json.NET will throw the following exception if a cycle is encoutered.</span></span>

> <span data-ttu-id="3ca9e-155">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-155">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="3ca9e-156">Wenn Sie ASP.NET Core verwenden, können Sie Json.NET um Zyklen zu ignorieren, die im Objektdiagramm gefunden konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-156">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="3ca9e-157">Dies erfolgt in der `ConfigureServices(...)` Methode in `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="3ca9e-157">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```
