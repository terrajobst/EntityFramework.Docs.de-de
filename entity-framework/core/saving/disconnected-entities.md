---
title: "Getrennte Entitäten – EF Core"
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: 0b145217d40027c4b8e4746e9c5651652a28c9eb
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2018
---
# <a name="disconnected-entities"></a><span data-ttu-id="6d408-102">Getrennte Entitäten</span><span class="sxs-lookup"><span data-stu-id="6d408-102">Disconnected entities</span></span>

<span data-ttu-id="6d408-103">Eine Instanz von ' DbContext ' verfolgt automatisch Entitäten, die aus der Datenbank zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="6d408-103">A DbContext instance will automatically track entities returned from the database.</span></span> <span data-ttu-id="6d408-104">Änderungen an diesen Entitäten werden dann erkannt werden, wenn SaveChanges aufgerufen und die Datenbank aktualisiert werden, wenn erforderlich.</span><span class="sxs-lookup"><span data-stu-id="6d408-104">Changes made to these entities will then be detected when SaveChanges is called and the database will be updated as needed.</span></span> <span data-ttu-id="6d408-105">Finden Sie unter [grundlegende speichern](basic.md) und [verknüpften Daten](related-data.md) für Details.</span><span class="sxs-lookup"><span data-stu-id="6d408-105">See [Basic Save](basic.md) and [Related Data](related-data.md) for details.</span></span>

<span data-ttu-id="6d408-106">Allerdings sind manchmal Entitäten abgefragt werden mehrere Instanzen von Kontext verwenden, und klicken Sie dann gespeichert mit einer anderen Instanz.</span><span class="sxs-lookup"><span data-stu-id="6d408-106">However, sometimes entities are queried using one context instance and then saved using a different instance.</span></span> <span data-ttu-id="6d408-107">Dies geschieht häufig in "getrennt" Szenarien, z. B. eine Webanwendung, in denen die Entitäten werden abgefragt, an den Client gesendet, geändert, zurück an den Server in einer Anforderung gesendet und anschließend gespeichert.</span><span class="sxs-lookup"><span data-stu-id="6d408-107">This often happens in "disconnected" scenarios such as a web application where the entities are queried, sent to the client, modified, sent back to the server in a request, and then saved.</span></span> <span data-ttu-id="6d408-108">In diesem Fall der zweiten Kontext Instanz muss wissen, ob die Entitäten nicht vertraut sind (sollte nun eingefügt sein), oder vorhandene (sollte aktualisiert werden).</span><span class="sxs-lookup"><span data-stu-id="6d408-108">In this case, the second context instance needs to know whether the entities are new (should be inserted) or existing (should be updated).</span></span>

> [!TIP]  
> <span data-ttu-id="6d408-109">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="6d408-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) on GitHub.</span></span>

> [!TIP]
> <span data-ttu-id="6d408-110">EF Core kann nur eine Instanz des eine Entität mit einem angegebenen primären Schlüsselwert überwachen.</span><span class="sxs-lookup"><span data-stu-id="6d408-110">EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="6d408-111">Die beste Möglichkeit zur Vermeidung dieses wird, die ein Problem ist die Verwendung einen kurzlebigen Kontext für jede Unit of Work, dass der Kontext leer ist, startet verfügt über Entitäten angefügt, und speichert die Entitäten an, und klicken Sie dann den Kontext verworfen und verworfen.</span><span class="sxs-lookup"><span data-stu-id="6d408-111">The best way to avoid this being an issue is to use a short-lived context for each unit-of-work such that the context starts empty, has entities attached to it, saves those entities, and then the context is disposed and discarded.</span></span>

## <a name="identifying-new-entities"></a><span data-ttu-id="6d408-112">Identifizieren neue Entitäten</span><span class="sxs-lookup"><span data-stu-id="6d408-112">Identifying new entities</span></span>

### <a name="client-identifies-new-entities"></a><span data-ttu-id="6d408-113">Client identifiziert neue Entitäten</span><span class="sxs-lookup"><span data-stu-id="6d408-113">Client identifies new entities</span></span>

<span data-ttu-id="6d408-114">Der einfachste Fall zu behandeln ist, wenn der Client dem Server informiert, ob die Entität neuen oder vorhandenen ist.</span><span class="sxs-lookup"><span data-stu-id="6d408-114">The simplest case to deal with is when the client informs the server whether the entity is new or existing.</span></span> <span data-ttu-id="6d408-115">Beispielsweise unterscheidet sich häufig die Anforderung an eine neue Entität einfügen aus der Anforderung zum Aktualisieren einer vorhandenen Entität.</span><span class="sxs-lookup"><span data-stu-id="6d408-115">For example, often the request to insert a new entity is different from the request to update an existing entity.</span></span>

<span data-ttu-id="6d408-116">Der übrige Teil dieses Abschnitts behandelt die Fälle, in denen es erforderlich, auf andere Weise zu ermitteln, ob zum Einfügen oder aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="6d408-116">The remainder of this section covers the cases where it necessary to determine in some other way whether to insert or update.</span></span>

### <a name="with-auto-generated-keys"></a><span data-ttu-id="6d408-117">Mit automatisch generierten Schlüsseln</span><span class="sxs-lookup"><span data-stu-id="6d408-117">With auto-generated keys</span></span>

<span data-ttu-id="6d408-118">Der Wert von einem automatisch generierten Schlüssel kann häufig verwendet werden, um zu bestimmen, ob eine Entität eingefügt oder aktualisiert werden muss.</span><span class="sxs-lookup"><span data-stu-id="6d408-118">The value of an automatically generated key can often be used to determine whether an entity needs to be inserted or updated.</span></span> <span data-ttu-id="6d408-119">Wenn der Schlüssel, nicht hat festgelegt (d. h. es immer noch den CLR-Standardwert von Null, 0 (null), usw.), wurde, wird die Entität neue muss und Einfügen benötigt.</span><span class="sxs-lookup"><span data-stu-id="6d408-119">If the key has not been set (i.e. it still has the CLR default value of null, zero, etc.), then the entity must be new and needs inserting.</span></span> <span data-ttu-id="6d408-120">Andererseits, wenn der Schlüssel-Wert festgelegt wurde, dann muss wurden bereits zuvor gespeichert und ist jetzt ein Update erforderlich.</span><span class="sxs-lookup"><span data-stu-id="6d408-120">On the other hand, if the key value has been set, then it must have already been previously saved and now needs updating.</span></span> <span data-ttu-id="6d408-121">Das heißt, wenn der Schlüssel einen Wert hat, klicken Sie dann Entität abgefragt wurde, an den Client gesendet und hat jetzt zurückkehren, um aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="6d408-121">In other words, if the key has a value, then entity was queried, sent to the client, and has now come back to be updated.</span></span>

<span data-ttu-id="6d408-122">Es ist einfach, eine Festlegung Schlüssel überprüfen, wenn der Entitätstyp bekannt ist:</span><span class="sxs-lookup"><span data-stu-id="6d408-122">It is easy to check for an unset key when the entity type is known:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

<span data-ttu-id="6d408-123">EF hat jedoch auch eine integrierten Möglichkeit hierzu für jede Entitätstyp und der Typ des Schlüssels:</span><span class="sxs-lookup"><span data-stu-id="6d408-123">However, EF also has a built-in way to do this for any entity type and key type:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> <span data-ttu-id="6d408-124">Schlüssel werden als Entitäten vom Kontext, nachverfolgt werden festgelegt, selbst wenn die Entität im Zustand ' Added ' befindet.</span><span class="sxs-lookup"><span data-stu-id="6d408-124">Keys are set as soon as entities are tracked by the context, even if the entity is in the Added state.</span></span> <span data-ttu-id="6d408-125">Dies ist hilfreich beim Durchlaufen von Entitäten und entscheiden Vorgehensweise mit einzelnen, z. B. bei Verwendung der TrackGraph-API in einem Diagramm.</span><span class="sxs-lookup"><span data-stu-id="6d408-125">This helps when traversing a graph of entities and deciding what to do with each, such as when using the TrackGraph API.</span></span> <span data-ttu-id="6d408-126">Der Schlüssel-Wert sollte nur verwendet werden, auf die Weise, die hier gezeigten _vor_ wird jeder Aufruf zum Nachverfolgen der Entität.</span><span class="sxs-lookup"><span data-stu-id="6d408-126">The key value should only be used in the way shown here _before_ any call is made to track the entity.</span></span>

### <a name="with-other-keys"></a><span data-ttu-id="6d408-127">Mit anderen Schlüsseln</span><span class="sxs-lookup"><span data-stu-id="6d408-127">With other keys</span></span>

<span data-ttu-id="6d408-128">Ein anderen Mechanismus ist erforderlich, um neue Entitäten zu identifizieren, wenn die Schlüsselwerte nicht automatisch generiert werden.</span><span class="sxs-lookup"><span data-stu-id="6d408-128">Some other mechanism is needed to identify new entities when key values are not generated automatically.</span></span> <span data-ttu-id="6d408-129">Es gibt zwei allgemeine Vorgehensweisen beim dies:</span><span class="sxs-lookup"><span data-stu-id="6d408-129">There are two general approaches to this:</span></span>
 * <span data-ttu-id="6d408-130">Abfrage für die Entität</span><span class="sxs-lookup"><span data-stu-id="6d408-130">Query for the entity</span></span>
 * <span data-ttu-id="6d408-131">Übergeben Sie ein Flag vom client</span><span class="sxs-lookup"><span data-stu-id="6d408-131">Pass a flag from the client</span></span>

<span data-ttu-id="6d408-132">Für die Entität eine Abfrage nur verwenden Sie die Find-Methode:</span><span class="sxs-lookup"><span data-stu-id="6d408-132">To query for the entity, just use the Find method:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

<span data-ttu-id="6d408-133">Es ist nicht Gegenstand dieses Dokuments, um den vollständigen Code für die Übergabe eines Flags von einem Client anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="6d408-133">It is beyond the scope of this document to show the full code for passing a flag from a client.</span></span> <span data-ttu-id="6d408-134">In einer Web-app bedeutet dies normalerweise, die verschiedene Anforderungen für unterschiedliche Aktionen, oder ein Zustand, der in der Anforderung übergeben, und extrahieren es im Controller.</span><span class="sxs-lookup"><span data-stu-id="6d408-134">In a web app, it usually means making different requests for different actions, or passing some state in the request then extracting it in the controller.</span></span>

## <a name="saving-single-entities"></a><span data-ttu-id="6d408-135">Speichern die einzelne Entitäten</span><span class="sxs-lookup"><span data-stu-id="6d408-135">Saving single entities</span></span>

<span data-ttu-id="6d408-136">Wenn es bekannt ist, unabhängig davon, ob eine INSERT- oder Update ist erforderlich, und klicken Sie dann hinzufügen oder aktualisieren ordnungsgemäß verwendet werden kann:</span><span class="sxs-lookup"><span data-stu-id="6d408-136">If it is known whether or not an insert or update is needed, then either Add or Update can be used appropriately:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

<span data-ttu-id="6d408-137">Jedoch, wenn die Entität automatisch generierten Schlüsselwerte verwendet wird, kann dann die Update-Methode für beide Fälle verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="6d408-137">However, if the entity uses auto-generated key values, then the Update method can be used for both cases:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

<span data-ttu-id="6d408-138">Die Update-Methode markiert normalerweise die Entität für das Update nicht einfügen.</span><span class="sxs-lookup"><span data-stu-id="6d408-138">The Update method normally marks the entity for update, not insert.</span></span> <span data-ttu-id="6d408-139">Wenn die Entität einen automatisch generierten Schlüssel hat, und kein Schlüsselwert festgelegt wurde, und klicken Sie dann die Entität für stattdessen automatisch markiert ist allerdings fügen Sie zu können ein.</span><span class="sxs-lookup"><span data-stu-id="6d408-139">However, if the entity has a auto-generated key, and no key value has been set, then the entity is instead automatically marked for insert.</span></span>

> [!TIP]  
> <span data-ttu-id="6d408-140">Dieses Verhalten wurde in der EF Core 2.0 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="6d408-140">This behavior was introduced in EF Core 2.0.</span></span> <span data-ttu-id="6d408-141">Bei früheren Versionen ist es immer erforderlich, um explizit anzugeben, hinzufügen oder aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="6d408-141">For earlier releases it is always necessary to explicitly choose either Add or Update.</span></span>

<span data-ttu-id="6d408-142">Wenn die Entität wird nicht automatisch generierten Schlüssel verwenden, die Anwendung muss entscheiden Sie, ob die Entität eingefügt oder aktualisiert werden: Z. B.:</span><span class="sxs-lookup"><span data-stu-id="6d408-142">If the entity is not using auto-generated keys, then the application must decide whether the entity should be inserted or updated: For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

<span data-ttu-id="6d408-143">Bei den folgenden Schritten werden:</span><span class="sxs-lookup"><span data-stu-id="6d408-143">The steps here are:</span></span>
* <span data-ttu-id="6d408-144">Wenn suchen gibt null ist, und klicken Sie dann die Datenbank im Blog mit dieser ID bereits enthält, damit wir rufen hinzufügen markieren Sie es zum Einfügen.</span><span class="sxs-lookup"><span data-stu-id="6d408-144">If Find returns null, then the database doesn't already contain the blog with this ID, so we call Add mark it for insertion.</span></span>
* <span data-ttu-id="6d408-145">Wenn suchen eine Entität zurückgibt, klicken sie in der Datenbank vorhanden ist und der Kontext verfolgt nun der vorhandenen Entität</span><span class="sxs-lookup"><span data-stu-id="6d408-145">If Find returns an entity, then it exists in the database and the context is now tracking the existing entity</span></span>
  * <span data-ttu-id="6d408-146">Klicken Sie dann verwenden wir SetValues, um die Werte für alle Eigenschaften für diese Entität, mit denen festgelegt wird, die vom Client stammen.</span><span class="sxs-lookup"><span data-stu-id="6d408-146">We then use SetValues to set the values for all properties on this entity to those that came from the client.</span></span>
  * <span data-ttu-id="6d408-147">Der Aufruf SetValues werden in der Entität aktualisiert werden, nach Bedarf gekennzeichnet.</span><span class="sxs-lookup"><span data-stu-id="6d408-147">The SetValues call will mark the entity to be updated as needed.</span></span>

> [!TIP]  
> <span data-ttu-id="6d408-148">SetValues kennzeichnet nur, wie die Eigenschaften geändert werden, die über unterschiedliche Werte in die nachverfolgte Entität verfügen.</span><span class="sxs-lookup"><span data-stu-id="6d408-148">SetValues will only mark as modified the properties that have different values to those in the tracked entity.</span></span> <span data-ttu-id="6d408-149">Dies bedeutet, dass das Update gesendet werden, wird nur die Spalten, die tatsächlich geändert wurden aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="6d408-149">This means that when the update is sent, only those columns that have actually changed will be updated.</span></span> <span data-ttu-id="6d408-150">(Und wenn nichts geändert wurde, wird kein Update auf allen gesendet,.)</span><span class="sxs-lookup"><span data-stu-id="6d408-150">(And if nothing has changed, then no update will be sent at all.)</span></span>

## <a name="working-with-graphs"></a><span data-ttu-id="6d408-151">Arbeiten mit Diagrammen</span><span class="sxs-lookup"><span data-stu-id="6d408-151">Working with graphs</span></span>

### <a name="identity-resolution"></a><span data-ttu-id="6d408-152">Identitätsauflösung</span><span class="sxs-lookup"><span data-stu-id="6d408-152">Identity resolution</span></span>

<span data-ttu-id="6d408-153">Wie oben bereits erwähnt, können EF Core nur eine Instanz des eine Entität mit einem angegebenen primären Schlüsselwert überwachen.</span><span class="sxs-lookup"><span data-stu-id="6d408-153">As noted above, EF Core can only track one instance of any entity with a given primary key value.</span></span> <span data-ttu-id="6d408-154">Beim Arbeiten mit Diagrammen sollte das Diagramm im Idealfall erstellt werden, dass diese invariante wird beibehalten, und der Kontext für nur eine Unit of Work verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="6d408-154">When working with graphs the graph should ideally be created such that this invariant is maintained, and the context should be used for only one unit-of-work.</span></span> <span data-ttu-id="6d408-155">Wenn das Diagramm Duplikate enthält, wird zum Diagramm zu verarbeiten, sendet sie an EF konsolidieren mehrere Instanzen in einem sein.</span><span class="sxs-lookup"><span data-stu-id="6d408-155">If the graph does contain duplicates, then it will be necessary to process the graph before sending it to EF to consolidate multiple instances into one.</span></span> <span data-ttu-id="6d408-156">Dies kann nicht triviale sein, in dem Instanzen in Konflikt stehenden Werte und Beziehungen, verfügen, damit die Konsolidierung von Duplikaten in Ihrer anwendungspipeline Auflösung des Konflikts zwischen vermeiden so bald wie möglich ausgeführt werden soll.</span><span class="sxs-lookup"><span data-stu-id="6d408-156">This may not be trivial where instances have conflicting values and relationships, so consolidating duplicates should be done as soon as possible in your application pipeline to avoid conflict resolution.</span></span>

### <a name="all-newall-existing-entities"></a><span data-ttu-id="6d408-157">Alle neuen Eigenschaft bzw. alle vorhandenen Entitäten</span><span class="sxs-lookup"><span data-stu-id="6d408-157">All new/all existing entities</span></span>

<span data-ttu-id="6d408-158">Ein Beispiel für das Arbeiten mit Diagrammen wird einfügen oder aktualisieren ein Blogs zusammen mit ihrer Auflistung von zugehörigen Beiträge.</span><span class="sxs-lookup"><span data-stu-id="6d408-158">An example of working with graphs is inserting or updating a blog together with its collection of associated posts.</span></span> <span data-ttu-id="6d408-159">Wenn alle Entitäten im Diagramm eingefügt werden soll, oder alle aktualisiert werden soll, ist der Prozess identisch für einzelne Entitäten wie oben beschrieben aus.</span><span class="sxs-lookup"><span data-stu-id="6d408-159">If all the entities in the graph should be inserted, or all should be updated, then the process is the same as described above for single entities.</span></span> <span data-ttu-id="6d408-160">Angenommen, ein Diagramm der Blogs und Beiträge, die wie folgt erstellt:</span><span class="sxs-lookup"><span data-stu-id="6d408-160">For example, a graph of blogs and posts created like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

<span data-ttu-id="6d408-161">können wie folgt eingefügt werden:</span><span class="sxs-lookup"><span data-stu-id="6d408-161">can be inserted like this:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

<span data-ttu-id="6d408-162">Der Aufruf von Add werden im Blog und alle Beiträge einzufügenden gekennzeichnet.</span><span class="sxs-lookup"><span data-stu-id="6d408-162">The call to Add will mark the blog and all the posts to be inserted.</span></span>

<span data-ttu-id="6d408-163">Ebenso, wenn alle Entitäten in einem Diagramm aktualisiert werden müssen, kann dann Update verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="6d408-163">Likewise, if all the entities in a graph need to be updated, then Update can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

<span data-ttu-id="6d408-164">Im Blog und alle ihre Beiträge werden gekennzeichnet werden, um aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="6d408-164">The blog and all its posts will be marked to be updated.</span></span>

### <a name="mix-of-new-and-existing-entities"></a><span data-ttu-id="6d408-165">Mischung neue und vorhandene Entitäten</span><span class="sxs-lookup"><span data-stu-id="6d408-165">Mix of new and existing entities</span></span>

<span data-ttu-id="6d408-166">Mit automatisch generierten Schlüssel kann Update erneut für einfügungen und Updates, verwendet werden, selbst wenn das Diagramm enthält eine Mischung aus Entitäten, die eingefügt werden müssen, auch solche, die aktualisiert werden müssen:</span><span class="sxs-lookup"><span data-stu-id="6d408-166">With auto-generated keys, Update can again be used for both inserts and updates, even if the graph contains a mix of entities that require inserting and those that require updating:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

<span data-ttu-id="6d408-167">Update kennzeichnet jede Entität im Diagramm, Blog oder Post zum Einfügen, wenn sie einen Satz Schlüsselwert keinen während alle anderen Entitäten für Update markiert sind.</span><span class="sxs-lookup"><span data-stu-id="6d408-167">Update will mark any entity in the graph, blog or post, for insertion if it does not have a key value set, while all other entities are marked for update.</span></span>

<span data-ttu-id="6d408-168">Wie vor, bei der automatisch generierten Schlüssel nicht mit einer Abfrage und einige Verarbeitungsschritte verwendet werden können:</span><span class="sxs-lookup"><span data-stu-id="6d408-168">As before, when not using auto-generated keys, a query and some processing can be used:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a><span data-ttu-id="6d408-169">Behandlung von Löschvorgängen</span><span class="sxs-lookup"><span data-stu-id="6d408-169">Handling deletes</span></span>

<span data-ttu-id="6d408-170">Delete aufspüren zu kann, da behandeln das Fehlen einer Entität bedeutet häufig, dass sie gelöscht werden soll.</span><span class="sxs-lookup"><span data-stu-id="6d408-170">Delete can be tricky to handle since often the absence of an entity means that it should be deleted.</span></span> <span data-ttu-id="6d408-171">Eine Möglichkeit für den Umgang mit Dies ist die Verwendung von "vorläufige Löschvorgänge", so, dass die Entität markiert ist, als gelöscht, statt Sie tatsächlich gelöscht werden.</span><span class="sxs-lookup"><span data-stu-id="6d408-171">One way to deal with this is to use "soft deletes" such that the entity is marked as deleted rather than actually being deleted.</span></span> <span data-ttu-id="6d408-172">Löscht wird dann der Updates identisch.</span><span class="sxs-lookup"><span data-stu-id="6d408-172">Deletes then becomes the same as updates.</span></span> <span data-ttu-id="6d408-173">Vorläufige Löschvorgänge können implementiert werden, sich mit [Abfragen Filter](xref:core/querying/filters).</span><span class="sxs-lookup"><span data-stu-id="6d408-173">Soft deletes can be implemented in using [query filters](xref:core/querying/filters).</span></span>

<span data-ttu-id="6d408-174">Für "true" löscht wird ein allgemeines Muster um eine Erweiterung des Abfragemusters für eine durchführen, was im Wesentlichen ein Diagramm Diff. Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="6d408-174">For true deletes, a common pattern is to use an extension of the query pattern to perform what is essentially a graph diff. For example:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a><span data-ttu-id="6d408-175">TrackGraph</span><span class="sxs-lookup"><span data-stu-id="6d408-175">TrackGraph</span></span>

<span data-ttu-id="6d408-176">Intern, Add-, Anfügen und Update verwenden Graph-Durchlauf mit einem Feststellung für jede Entität, gibt an, ob er als hinzugefügt (insert), "geändert" (zu aktualisieren), markiert werden soll Unchanged (keine), oder gelöschte (zu löschen).</span><span class="sxs-lookup"><span data-stu-id="6d408-176">Internally, Add, Attach, and Update use graph-traversal with a determination made for each entity as to whether it should be marked as Added (to insert), Modified (to update), Unchanged (do nothing), or Deleted (to delete).</span></span> <span data-ttu-id="6d408-177">Dieser Mechanismus wird über die TrackGraph-API verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="6d408-177">This mechanism is exposed via the TrackGraph API.</span></span> <span data-ttu-id="6d408-178">Nehmen wir beispielsweise an, sendet der Client wieder Entitäten in einem Diagramm wird einige Kennzeichen für jede Entität, der angibt, wie sie behandelt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="6d408-178">For example, let's assume that when the client sends back a graph of entities it sets some flag on each entity indicating how it should be handled.</span></span> <span data-ttu-id="6d408-179">TrackGraph kann dann verwendet werden, um dieses Flag zu verarbeiten:</span><span class="sxs-lookup"><span data-stu-id="6d408-179">TrackGraph can then be used to process this flag:</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

<span data-ttu-id="6d408-180">Die Flags werden nur als Teil der Entität aus Gründen der Einfachheit der im Beispiel angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6d408-180">The flags are only shown as part of the entity for simplicity of the example.</span></span> <span data-ttu-id="6d408-181">In der Regel würden die Flags Teil einer DTO oder einem anderen Status in der Anforderung enthalten sein.</span><span class="sxs-lookup"><span data-stu-id="6d408-181">Typically the flags would be part of a DTO or some other state included in the request.</span></span>
