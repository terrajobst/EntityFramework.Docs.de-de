---
title: "Löschweitergabe - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
ms.technology: entity-framework-core
uid: core/saving/cascade-delete
ms.openlocfilehash: a9481fe851cc264ab3eaecad052c2e683ae57a44
ms.sourcegitcommit: 5367516f063cb42804ec92c31cdf76322554f2b5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2017
---
# <a name="cascade-delete"></a><span data-ttu-id="65bdf-102">Kaskadierte Löschung</span><span class="sxs-lookup"><span data-stu-id="65bdf-102">Cascade Delete</span></span>

<span data-ttu-id="65bdf-103">Kaskadierte Löschung wird häufig in der Terminologie von Datenbanken verwendet, um ein Merkmal beschreiben, die das Löschen einer Zeile automatisch verknüpfte Zeilen gelöscht auslösen können.</span><span class="sxs-lookup"><span data-stu-id="65bdf-103">Cascade delete is commonly used in database terminology to describe a characteristic that allows the deletion of a row to automatically trigger the deletion of related rows.</span></span> <span data-ttu-id="65bdf-104">Ein eng verwandtes Konzept gehörig von EF Core Delete Verhalten ist das automatische Löschen von einer untergeordneten Entität wird die Beziehung zu einem übergeordneten Element unterbrochen wurde – dies geht im Allgemeinen als "verwaiste löschen" bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="65bdf-104">A closely related concept also covered by EF Core delete behaviors is the automatic deletion of a child entity when it's relationship to a parent has been severed--this i commonly known as "deleting orphans".</span></span>

<span data-ttu-id="65bdf-105">EF Core implementiert mehrere unterschiedliche Delete Verhaltensweisen und ermöglicht die Konfiguration der Delete-Verhalten von einzelnen Beziehungen.</span><span class="sxs-lookup"><span data-stu-id="65bdf-105">EF Core implements several different delete behaviors and allows for the configuration of the delete behaviors of individual relationships.</span></span> <span data-ttu-id="65bdf-106">EF Core implementiert auch Konventionen, die hilfreich, löschen Sie das Standardverhalten für jede Beziehung auf Basis der [Requiredness der Beziehung](.. /Modeling/Relationships.MD#Required-and-optional-Relationships) automatisch zu konfigurieren .</span><span class="sxs-lookup"><span data-stu-id="65bdf-106">EF Core also implements conventions that automatically configure useful default delete behaviors for each relationship based on the [requiredness of the relationship] (../modeling/relationships.md#required-and-optional-relationships).</span></span>

## <a name="delete-behaviors"></a><span data-ttu-id="65bdf-107">Löschen von Verhalten</span><span class="sxs-lookup"><span data-stu-id="65bdf-107">Delete behaviors</span></span>
<span data-ttu-id="65bdf-108">Löschen Verhalten wird definiert, der *deleteBehavior()* Enumerator geben, und übergeben werden kann, um die *OnDelete* fluent-API, um zu steuern, ob das Löschen einer Entität Prinzipal/im übergeordneten Element oder das Trennen von der Beziehung zu abhängige/untergeordneter Entitäten sollten einen Nebeneffekt auf abhängige/untergeordnete Entitäten enthalten.</span><span class="sxs-lookup"><span data-stu-id="65bdf-108">Delete behaviors are defined in the *DeleteBehavior* enumerator type and can be passed to the *OnDelete* fluent API to control whether the deletion of a principal/parent entity or the severing of the relationship to dependent/child entities should have a side effect on the dependent/child entities.</span></span>

<span data-ttu-id="65bdf-109">Es gibt drei Aktionstypen, die EF ergreifen kann, wenn eine Entität Prinzipal/im übergeordneten Element gelöscht wird oder die Beziehung zum untergeordneten Element unterbrochen wird:</span><span class="sxs-lookup"><span data-stu-id="65bdf-109">There are three actions EF can take when a principal/parent entity is deleted or the relationship to the child is severed:</span></span>
* <span data-ttu-id="65bdf-110">Die untergeordneten/abhängigen können gelöscht werden</span><span class="sxs-lookup"><span data-stu-id="65bdf-110">The child/dependent can be deleted</span></span>
* <span data-ttu-id="65bdf-111">Fremdschlüsselwerte des untergeordneten Standorts können festgelegt werden, auf Null</span><span class="sxs-lookup"><span data-stu-id="65bdf-111">The child's foreign key values can be set to null</span></span>
* <span data-ttu-id="65bdf-112">Das untergeordnete Element unverändert.</span><span class="sxs-lookup"><span data-stu-id="65bdf-112">The child remains unchanged</span></span>

> [!NOTE]  
> <span data-ttu-id="65bdf-113">Das Löschverhalten konfiguriert in der EF-Core-Modell wird nur angewendet, wenn prinzipalentität wird mithilfe von EF Core gelöscht, und der abhängigen Entitäten (d. h. für überwachte Nachfolger) im Arbeitsspeicher geladen werden.</span><span class="sxs-lookup"><span data-stu-id="65bdf-113">The delete behavior configured in the EF Core model is only applied when the principal entity is deleted using EF Core and the dependent entities are loaded in memory (i.e. for tracked dependents).</span></span> <span data-ttu-id="65bdf-114">Eine entsprechende überlappungsverhalten muss sein, dass Setup in der Datenbank, um sicherzustellen, dass Daten, die nicht vom Kontext nachverfolgt wird die erforderliche Aktion angewendet wurde.</span><span class="sxs-lookup"><span data-stu-id="65bdf-114">A corresponding cascade behavior needs to be setup in the database to ensure data that is not being tracked by the context has the necessary action applied.</span></span> <span data-ttu-id="65bdf-115">Wenn Sie EF Core verwenden, um die Datenbank zu erstellen, wird dieses Verhalten Cascade für Sie eingerichtet werden.</span><span class="sxs-lookup"><span data-stu-id="65bdf-115">If you use EF Core to create the database, this cascade behavior will be setup for you.</span></span>

<span data-ttu-id="65bdf-116">Für die zweite Aktion ist einen foreign Key-Wert auf null festlegen ungültig, wenn Fremdschlüssel nicht zulässig ist.</span><span class="sxs-lookup"><span data-stu-id="65bdf-116">For the second action above, setting a foreign key value to null is not valid if foreign key is not nullable.</span></span> <span data-ttu-id="65bdf-117">(Ein NULL-Fremdschlüssel entspricht eine erforderliche Beziehung). In diesen Fällen verfolgt EF Core an, dass die Fremdschlüsseleigenschaft bis SaveChanges aufgerufen wird zu diesem Zeitpunkt eine Ausnahme ausgelöst wird, da die Änderung in der Datenbank dauerhaft gespeichert werden kann, als null markiert wurde.</span><span class="sxs-lookup"><span data-stu-id="65bdf-117">(A non-nullable foreign key is equivalent to a required relationship.) In these cases, EF Core tracks that the foreign key property has been marked as null until SaveChanges is called, at which time an exception is thrown because the change cannot be persisted to the database.</span></span> <span data-ttu-id="65bdf-118">Dies ist vergleichbar mit dem eine einschränkungsverletzung aus der Datenbank abrufen.</span><span class="sxs-lookup"><span data-stu-id="65bdf-118">This is similar to getting a constraint violation from the database.</span></span>

<span data-ttu-id="65bdf-119">Es gibt vier Verhaltensweisen, löschen, wie in den folgenden Tabellen aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="65bdf-119">There are four delete behaviors, as listed in the tables below.</span></span> <span data-ttu-id="65bdf-120">Für optionale Beziehungen (nullable Fremdschlüssel) es _ist_ möglich, einen null Fremdschlüsselwert speichern vortäuschen folgenden Auswirkungen:</span><span class="sxs-lookup"><span data-stu-id="65bdf-120">For optional relationships (nullable foreign key) it _is_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="65bdf-121">Verhaltensname</span><span class="sxs-lookup"><span data-stu-id="65bdf-121">Behavior Name</span></span> | <span data-ttu-id="65bdf-122">Auswirkungen auf abhängige und untergeordneten Elementen im Arbeitsspeicher</span><span class="sxs-lookup"><span data-stu-id="65bdf-122">Effect on dependent/child in memory</span></span> | <span data-ttu-id="65bdf-123">Auswirkungen auf abhängige/untergeordnete Datenbank</span><span class="sxs-lookup"><span data-stu-id="65bdf-123">Effect on dependent/child in database</span></span>
|-|-|-
| <span data-ttu-id="65bdf-124">**CASCADE**</span><span class="sxs-lookup"><span data-stu-id="65bdf-124">**Cascade**</span></span> | <span data-ttu-id="65bdf-125">Entitäten werden gelöscht.</span><span class="sxs-lookup"><span data-stu-id="65bdf-125">Entities are deleted</span></span> | <span data-ttu-id="65bdf-126">Entitäten werden gelöscht.</span><span class="sxs-lookup"><span data-stu-id="65bdf-126">Entities are deleted</span></span>
| <span data-ttu-id="65bdf-127">**ClientSetNull** (Standard)</span><span class="sxs-lookup"><span data-stu-id="65bdf-127">**ClientSetNull** (Default)</span></span> | <span data-ttu-id="65bdf-128">Fremdschlüsseleigenschaften festgelegt werden auf Null</span><span class="sxs-lookup"><span data-stu-id="65bdf-128">Foreign key properties are set to null</span></span> | <span data-ttu-id="65bdf-129">Keine</span><span class="sxs-lookup"><span data-stu-id="65bdf-129">None</span></span>
| <span data-ttu-id="65bdf-130">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="65bdf-130">**SetNull**</span></span> | <span data-ttu-id="65bdf-131">Fremdschlüsseleigenschaften festgelegt werden auf Null</span><span class="sxs-lookup"><span data-stu-id="65bdf-131">Foreign key properties are set to null</span></span> | <span data-ttu-id="65bdf-132">Fremdschlüsseleigenschaften festgelegt werden auf Null</span><span class="sxs-lookup"><span data-stu-id="65bdf-132">Foreign key properties are set to null</span></span>
| <span data-ttu-id="65bdf-133">**Einschränken**</span><span class="sxs-lookup"><span data-stu-id="65bdf-133">**Restrict**</span></span> | <span data-ttu-id="65bdf-134">Keine</span><span class="sxs-lookup"><span data-stu-id="65bdf-134">None</span></span> | <span data-ttu-id="65bdf-135">Keine</span><span class="sxs-lookup"><span data-stu-id="65bdf-135">None</span></span>

<span data-ttu-id="65bdf-136">Für die erforderlichen Beziehungen (null-Fremdschlüssel) ist es _nicht_ möglich, einen null Fremdschlüsselwert speichern vortäuschen folgenden Auswirkungen:</span><span class="sxs-lookup"><span data-stu-id="65bdf-136">For required relationships (non-nullable foreign key) it is _not_ possible to save a null foreign key value, which results in the following effects:</span></span>

| <span data-ttu-id="65bdf-137">Verhaltensname</span><span class="sxs-lookup"><span data-stu-id="65bdf-137">Behavior Name</span></span> | <span data-ttu-id="65bdf-138">Auswirkungen auf abhängige und untergeordneten Elementen im Arbeitsspeicher</span><span class="sxs-lookup"><span data-stu-id="65bdf-138">Effect on dependent/child in memory</span></span> | <span data-ttu-id="65bdf-139">Auswirkungen auf abhängige/untergeordnete Datenbank</span><span class="sxs-lookup"><span data-stu-id="65bdf-139">Effect on dependent/child in database</span></span>
|-|-|-
| <span data-ttu-id="65bdf-140">**CASCADE** (Standard)</span><span class="sxs-lookup"><span data-stu-id="65bdf-140">**Cascade** (Default)</span></span> | <span data-ttu-id="65bdf-141">Entitäten werden gelöscht.</span><span class="sxs-lookup"><span data-stu-id="65bdf-141">Entities are deleted</span></span> | <span data-ttu-id="65bdf-142">Entitäten werden gelöscht.</span><span class="sxs-lookup"><span data-stu-id="65bdf-142">Entities are deleted</span></span>
| <span data-ttu-id="65bdf-143">**ClientSetNull**</span><span class="sxs-lookup"><span data-stu-id="65bdf-143">**ClientSetNull**</span></span> | <span data-ttu-id="65bdf-144">SaveChanges löst aus</span><span class="sxs-lookup"><span data-stu-id="65bdf-144">SaveChanges throws</span></span> | <span data-ttu-id="65bdf-145">Keine</span><span class="sxs-lookup"><span data-stu-id="65bdf-145">None</span></span>
| <span data-ttu-id="65bdf-146">**SetNull**</span><span class="sxs-lookup"><span data-stu-id="65bdf-146">**SetNull**</span></span> | <span data-ttu-id="65bdf-147">SaveChanges löst aus</span><span class="sxs-lookup"><span data-stu-id="65bdf-147">SaveChanges throws</span></span> | <span data-ttu-id="65bdf-148">SaveChanges löst aus</span><span class="sxs-lookup"><span data-stu-id="65bdf-148">SaveChanges throws</span></span>
| <span data-ttu-id="65bdf-149">**Einschränken**</span><span class="sxs-lookup"><span data-stu-id="65bdf-149">**Restrict**</span></span> | <span data-ttu-id="65bdf-150">Keine</span><span class="sxs-lookup"><span data-stu-id="65bdf-150">None</span></span> | <span data-ttu-id="65bdf-151">Keine</span><span class="sxs-lookup"><span data-stu-id="65bdf-151">None</span></span>

<span data-ttu-id="65bdf-152">In den obigen Tabellen *keine* kann zu einer Verletzung einer Einschränkung führen.</span><span class="sxs-lookup"><span data-stu-id="65bdf-152">In the tables above, *None* can result in a constraint violation.</span></span> <span data-ttu-id="65bdf-153">Z. B. wenn eine Prinzipal/untergeordnete Entität wird gelöscht, jedoch keine Aktion ausgeführt wird, um den Fremdschlüssel einer abhängigen/untergeordneten zu ändern, löst klicken Sie dann die Datenbank wahrscheinlich auf SaveChanges aufgrund einer einschränkungsverletzung foreign.</span><span class="sxs-lookup"><span data-stu-id="65bdf-153">For example, if a principal/child entity is deleted but no action is taken to change the foreign key of a dependent/child, then the database will likely throw on SaveChanges due to a foreign constraint violation.</span></span>

<span data-ttu-id="65bdf-154">Auf hoher Ebene:</span><span class="sxs-lookup"><span data-stu-id="65bdf-154">At a high level:</span></span>
* <span data-ttu-id="65bdf-155">Wenn Sie Entitäten, die ohne ein übergeordnetes Element vorhanden sein können, und Sie EF, achten Sie darauf für die untergeordneten Elemente automatisch gelöscht werden soll, verwenden Sie *Cascade*.</span><span class="sxs-lookup"><span data-stu-id="65bdf-155">If you have entities that cannot exist without a parent, and you want EF to take care for deleting the children automatically, then use *Cascade*.</span></span>
  * <span data-ttu-id="65bdf-156">Entitäten, die vorhanden sein können, ohne ein übergeordnetes Element in der Regel stellen für die erforderlichen Beziehungen nutzen *Cascade* ist die Standardeinstellung.</span><span class="sxs-lookup"><span data-stu-id="65bdf-156">Entities that cannot exist without a parent usually make use of required relationships, for which *Cascade* is the default.</span></span>
* <span data-ttu-id="65bdf-157">Wenn Sie die Entitäten, die möglicherweise verfügen möglicherweise nicht über ein übergeordnetes Element haben, und Sie EF zu kümmern, des Fremdschlüssels für Sie erheblich werden soll, verwenden Sie *ClientSetNull*</span><span class="sxs-lookup"><span data-stu-id="65bdf-157">If you have entities that may or may not have a parent, and you want EF to take care of nulling out the foreign key for you, then use *ClientSetNull*</span></span>
  * <span data-ttu-id="65bdf-158">Entitäten, die vorhanden sein können, ohne ein übergeordnetes Element in der Regel stellen Verwenden von optionalen Beziehungen für die *ClientSetNull* ist die Standardeinstellung.</span><span class="sxs-lookup"><span data-stu-id="65bdf-158">Entities that can exist without a parent usually make use of optional relationships, for which *ClientSetNull* is the default.</span></span>
  * <span data-ttu-id="65bdf-159">Wenn Sie die Datenbank auch versuchen, die null-Werte zu Fremdschlüsseln untergeordneten auch weitergeben soll bei die untergeordneten Entität nicht geladen wird, verwenden Sie dann *SetNull*.</span><span class="sxs-lookup"><span data-stu-id="65bdf-159">If you want the database to also try to propagate null values to child foreign keys even when the child entity is not loaded, then use *SetNull*.</span></span> <span data-ttu-id="65bdf-160">Beachten Sie jedoch, dass die Datenbank muss dies unterstützen, und Konfigurieren der Datenbank wie folgt dazu, andere Einschränkungen führen kann, die in der Praxis eignet sich oft diese Option nicht.</span><span class="sxs-lookup"><span data-stu-id="65bdf-160">However, note that the database must support this, and configuring the database like this can result in other restrictions, which in practice often makes this option impractical.</span></span> <span data-ttu-id="65bdf-161">Deswegen *SetNull* ist nicht der Standard.</span><span class="sxs-lookup"><span data-stu-id="65bdf-161">This is why *SetNull* is not the default.</span></span>
* <span data-ttu-id="65bdf-162">Wenn Sie nicht möchten, dass EF Core jemals Löschen einer Entität automatisch oder automatisch, die den Fremdschlüssel null, dann verwenden *beschränken*.</span><span class="sxs-lookup"><span data-stu-id="65bdf-162">If you don't want EF Core to ever delete an entity automatically or null out the foreign key automatically, then use *Restrict*.</span></span> <span data-ttu-id="65bdf-163">Beachten Sie, dass dies erfordert, dass Ihr Code untergeordneten Entitäten und deren Fremdschlüsselwerte manuell synchronisieren werden andernfalls Einschränkung Ausnahmen ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="65bdf-163">Note that this requires that your code keep child entities and their foreign key values in sync manually otherwise constraint exceptions will be thrown.</span></span>

> [!NOTE]
> <span data-ttu-id="65bdf-164">In EF Kern ausgeführt, im Gegensatz zu EF6 führen Sie kaskadierende Effekte nicht sofort, sondern stattdessen erfolgen nur, wenn SaveChanges aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="65bdf-164">In EF Core, unlike EF6, cascading effects do not happen immediately, but instead only when SaveChanges is called.</span></span>

> [!NOTE]  
> <span data-ttu-id="65bdf-165">**Änderungen in EF Core 2.0:** In früheren Versionen *beschränken* würde optionale Fremdschlüsseleigenschaften in überwachten abhängigen Entitäten festgelegt werden, null und wurde die Standardeinstellung Verhalten für optionale Beziehungen zu löschen.</span><span class="sxs-lookup"><span data-stu-id="65bdf-165">**Changes in EF Core 2.0:** In previous releases, *Restrict* would cause optional foreign key properties in tracked dependent entities to be set to null, and was the default delete behavior for optional relationships.</span></span> <span data-ttu-id="65bdf-166">In EF Core 2.0 die *ClientSetNull* wurde eingeführt, um dieses Verhalten darstellen, und stattdessen der Standardwert für optionale Beziehungen.</span><span class="sxs-lookup"><span data-stu-id="65bdf-166">In EF Core 2.0, the *ClientSetNull* was introduced to represent that behavior and became the default for optional relationships.</span></span> <span data-ttu-id="65bdf-167">Das Verhalten des *beschränken* wurde angepasst, um keine Nebeneffekte nie auf abhängige Entitäten haben.</span><span class="sxs-lookup"><span data-stu-id="65bdf-167">The behavior of *Restrict* was adjusted to never have any side effects on dependent entities.</span></span>

## <a name="entity-deletion-examples"></a><span data-ttu-id="65bdf-168">Beispiele für das Löschen von Entitäten</span><span class="sxs-lookup"><span data-stu-id="65bdf-168">Entity deletion examples</span></span>

<span data-ttu-id="65bdf-169">Der folgende Code ist Teil einer [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) , kann eine Ausführung heruntergeladen werden.</span><span class="sxs-lookup"><span data-stu-id="65bdf-169">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded an run.</span></span> <span data-ttu-id="65bdf-170">Im Beispiel wird gezeigt, was geschieht für jede Löschverhalten für optionale und notwendige Beziehungen, wenn eine übergeordnete Entität gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="65bdf-170">The sample shows what happens for each delete behavior for both optional and required relationships when a parent entity is deleted.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

<span data-ttu-id="65bdf-171">Wir führen Sie durch jede Variante zu verstehen, was geschieht.</span><span class="sxs-lookup"><span data-stu-id="65bdf-171">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="65bdf-172">DeleteBehavior.Cascade mit erforderlichen oder optionalen Beziehung</span><span class="sxs-lookup"><span data-stu-id="65bdf-172">DeleteBehavior.Cascade with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="65bdf-173">Blog wird als gelöscht markiert.</span><span class="sxs-lookup"><span data-stu-id="65bdf-173">Blog is marked as Deleted</span></span>
* <span data-ttu-id="65bdf-174">Beiträge bleiben anfänglich unverändert, da sich bis SaveChanges nicht überlappend ausgeführt werden</span><span class="sxs-lookup"><span data-stu-id="65bdf-174">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="65bdf-175">SaveChanges sendet löschungen für sowohl Dependents/untergeordnete Elemente (Beiträge), und klicken Sie dann den Prinzipal/im übergeordneten Element (Blog)</span><span class="sxs-lookup"><span data-stu-id="65bdf-175">SaveChanges sends deletes for both dependents/children (posts) and then the principal/parent (blog)</span></span>
* <span data-ttu-id="65bdf-176">Nach dem Speichern, werden alle Entitäten getrennt, da sie jetzt aus der Datenbank gelöscht wurden</span><span class="sxs-lookup"><span data-stu-id="65bdf-176">After saving, all entities are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="65bdf-177">DeleteBehavior.ClientSetNull oder DeleteBehavior.SetNull mit erforderliche Beziehung</span><span class="sxs-lookup"><span data-stu-id="65bdf-177">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="65bdf-178">Blog wird als gelöscht markiert.</span><span class="sxs-lookup"><span data-stu-id="65bdf-178">Blog is marked as Deleted</span></span>
* <span data-ttu-id="65bdf-179">Beiträge bleiben anfänglich unverändert, da sich bis SaveChanges nicht überlappend ausgeführt werden</span><span class="sxs-lookup"><span data-stu-id="65bdf-179">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="65bdf-180">SaveChanges versucht Post-FK auf null festgelegt, aber dies schlägt fehl, da die '-fk ' nicht auf NULL festlegbar ist.</span><span class="sxs-lookup"><span data-stu-id="65bdf-180">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="65bdf-181">DeleteBehavior.ClientSetNull oder DeleteBehavior.SetNull mit optionale Beziehung</span><span class="sxs-lookup"><span data-stu-id="65bdf-181">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="65bdf-182">Blog wird als gelöscht markiert.</span><span class="sxs-lookup"><span data-stu-id="65bdf-182">Blog is marked as Deleted</span></span>
* <span data-ttu-id="65bdf-183">Beiträge bleiben anfänglich unverändert, da sich bis SaveChanges nicht überlappend ausgeführt werden</span><span class="sxs-lookup"><span data-stu-id="65bdf-183">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="65bdf-184">SaveChanges Versuche Legt die FK der abhängigen Elemente/untergeordnete Elemente (Beiträge) auf null fest, vor dem Löschen der Prinzipal/im übergeordneten Element (Blog)</span><span class="sxs-lookup"><span data-stu-id="65bdf-184">SaveChanges attempts sets the FK of both dependents/children (posts) to null before deleting the principal/parent (blog)</span></span>
* <span data-ttu-id="65bdf-185">Nach dem Speichern der Prinzipal/im übergeordneten Element (Blog) gelöscht, jedoch werden weiterhin die abhängigen Elemente/untergeordneten Elemente (Beiträge) nachverfolgt.</span><span class="sxs-lookup"><span data-stu-id="65bdf-185">After saving, the principal/parent (blog) is deleted, but the dependents/children (posts) are still tracked</span></span>
* <span data-ttu-id="65bdf-186">Die nachverfolgte Dependents/untergeordneten Elemente (Beiträge) haben jetzt FK Nullwerte und ihre Verweis auf die gelöschte Prinzipal/im übergeordneten Element (Blog) wurde entfernt</span><span class="sxs-lookup"><span data-stu-id="65bdf-186">The tracked dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="65bdf-187">DeleteBehavior.Restrict mit erforderlichen oder optionalen Beziehung</span><span class="sxs-lookup"><span data-stu-id="65bdf-187">DeleteBehavior.Restrict with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="65bdf-188">Blog wird als gelöscht markiert.</span><span class="sxs-lookup"><span data-stu-id="65bdf-188">Blog is marked as Deleted</span></span>
* <span data-ttu-id="65bdf-189">Beiträge bleiben anfänglich unverändert, da sich bis SaveChanges nicht überlappend ausgeführt werden</span><span class="sxs-lookup"><span data-stu-id="65bdf-189">Posts initially remain Unchanged since cascades do not happen until SaveChanges</span></span>
* <span data-ttu-id="65bdf-190">Da *beschränken* teilt EF auf die '-fk ' nicht automatisch auf null festlegen, bleibt es ungleich Null und SaveChanges löst aus, ohne zu speichern</span><span class="sxs-lookup"><span data-stu-id="65bdf-190">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="delete-orphans-examples"></a><span data-ttu-id="65bdf-191">Beispiele für die verwaisten Objekte löschen</span><span class="sxs-lookup"><span data-stu-id="65bdf-191">Delete orphans examples</span></span>

<span data-ttu-id="65bdf-192">Der folgende Code ist Teil einer [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) , kann eine Ausführung heruntergeladen werden.</span><span class="sxs-lookup"><span data-stu-id="65bdf-192">The code below is part of a [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) that can be downloaded an run.</span></span> <span data-ttu-id="65bdf-193">Im Beispiel wird gezeigt, was geschieht, für jede Löschverhalten für optionale und notwendige Beziehungen, wenn die Beziehung zwischen einem übergeordneten/Prinzipal und seine untergeordneten Elemente/Dependents unterbrochen wird.</span><span class="sxs-lookup"><span data-stu-id="65bdf-193">The sample shows what happens for each delete behavior for both optional and required relationships when the relationship between a parent/principal and its children/dependents is severed.</span></span> <span data-ttu-id="65bdf-194">In diesem Beispiel wird die Beziehung abgetrennt werden durch das Entfernen der abhängigen Elemente/untergeordnete Elemente (Beiträge) aus der auflistungsnavigationseigenschaft für den Prinzipal/im übergeordneten Element (Blog).</span><span class="sxs-lookup"><span data-stu-id="65bdf-194">In this example, the relationship is severed by removing the dependents/children (posts) from the collection navigation property on the principal/parent (blog).</span></span> <span data-ttu-id="65bdf-195">Allerdings ist das Verhalten identisch, wenn der Verweis von abhängigen und untergeordneten Elementen auf Prinzipal/im übergeordneten Element stattdessen out gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="65bdf-195">However, the behavior is the same if the reference from dependent/child to principal/parent is instead nulled out.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

<span data-ttu-id="65bdf-196">Wir führen Sie durch jede Variante zu verstehen, was geschieht.</span><span class="sxs-lookup"><span data-stu-id="65bdf-196">Let's walk through each variation to understand what is happening.</span></span>

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a><span data-ttu-id="65bdf-197">DeleteBehavior.Cascade mit erforderlichen oder optionalen Beziehung</span><span class="sxs-lookup"><span data-stu-id="65bdf-197">DeleteBehavior.Cascade with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* <span data-ttu-id="65bdf-198">Beiträge werden als geändert markiert, da das Trennen der Beziehung der FK als null markiert sein verursacht werden.</span><span class="sxs-lookup"><span data-stu-id="65bdf-198">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="65bdf-199">Ist der Fremdschlüssel keine NULL-Werte zulässt, klicken Sie dann ändert der tatsächliche Wert sich nicht, obwohl sie als null markiert ist</span><span class="sxs-lookup"><span data-stu-id="65bdf-199">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="65bdf-200">SaveChanges sendet löschungen für abhängige Dateien/untergeordnete Elemente (Beiträge)</span><span class="sxs-lookup"><span data-stu-id="65bdf-200">SaveChanges sends deletes for dependents/children (posts)</span></span>
* <span data-ttu-id="65bdf-201">Nach dem Speichern, werden die abhängigen Elemente/untergeordneten Elemente (Beiträge) getrennt, da sie jetzt aus der Datenbank gelöscht wurden</span><span class="sxs-lookup"><span data-stu-id="65bdf-201">After saving, the dependents/children (posts) are detached since they have now been deleted from the database</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a><span data-ttu-id="65bdf-202">DeleteBehavior.ClientSetNull oder DeleteBehavior.SetNull mit erforderliche Beziehung</span><span class="sxs-lookup"><span data-stu-id="65bdf-202">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with required relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* <span data-ttu-id="65bdf-203">Beiträge werden als geändert markiert, da das Trennen der Beziehung der FK als null markiert sein verursacht werden.</span><span class="sxs-lookup"><span data-stu-id="65bdf-203">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="65bdf-204">Ist der Fremdschlüssel keine NULL-Werte zulässt, klicken Sie dann ändert der tatsächliche Wert sich nicht, obwohl sie als null markiert ist</span><span class="sxs-lookup"><span data-stu-id="65bdf-204">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="65bdf-205">SaveChanges versucht Post-FK auf null festgelegt, aber dies schlägt fehl, da die '-fk ' nicht auf NULL festlegbar ist.</span><span class="sxs-lookup"><span data-stu-id="65bdf-205">SaveChanges attempts to set the post FK to null, but this fails because the FK is not nullable</span></span>

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a><span data-ttu-id="65bdf-206">DeleteBehavior.ClientSetNull oder DeleteBehavior.SetNull mit optionale Beziehung</span><span class="sxs-lookup"><span data-stu-id="65bdf-206">DeleteBehavior.ClientSetNull or DeleteBehavior.SetNull with optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* <span data-ttu-id="65bdf-207">Beiträge werden als geändert markiert, da das Trennen der Beziehung der FK als null markiert sein verursacht werden.</span><span class="sxs-lookup"><span data-stu-id="65bdf-207">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="65bdf-208">Ist der Fremdschlüssel keine NULL-Werte zulässt, klicken Sie dann ändert der tatsächliche Wert sich nicht, obwohl sie als null markiert ist</span><span class="sxs-lookup"><span data-stu-id="65bdf-208">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="65bdf-209">SaveChanges legt die FK der abhängigen Elemente/untergeordnete Elemente (Beiträge) auf null fest.</span><span class="sxs-lookup"><span data-stu-id="65bdf-209">SaveChanges sets the FK of both dependents/children (posts) to null</span></span>
* <span data-ttu-id="65bdf-210">Nach dem Speichern, die abhängigen Elemente/untergeordneten Elemente (Beiträge) jetzt Nullwerte FK haben und ihre Verweis auf die gelöschte Prinzipal/im übergeordneten Element (Blog) entfernt wurde</span><span class="sxs-lookup"><span data-stu-id="65bdf-210">After saving, the dependents/children (posts) now have null FK values and their reference to the deleted principal/parent (blog) has been removed</span></span>

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a><span data-ttu-id="65bdf-211">DeleteBehavior.Restrict mit erforderlichen oder optionalen Beziehung</span><span class="sxs-lookup"><span data-stu-id="65bdf-211">DeleteBehavior.Restrict with required or optional relationship</span></span>

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* <span data-ttu-id="65bdf-212">Beiträge werden als geändert markiert, da das Trennen der Beziehung der FK als null markiert sein verursacht werden.</span><span class="sxs-lookup"><span data-stu-id="65bdf-212">Posts are marked as Modified because severing the relationship caused the FK to be marked as null</span></span>
  * <span data-ttu-id="65bdf-213">Ist der Fremdschlüssel keine NULL-Werte zulässt, klicken Sie dann ändert der tatsächliche Wert sich nicht, obwohl sie als null markiert ist</span><span class="sxs-lookup"><span data-stu-id="65bdf-213">If the FK is not nullable, then the actual value will not change even though it is marked as null</span></span>
* <span data-ttu-id="65bdf-214">Da *beschränken* teilt EF auf die '-fk ' nicht automatisch auf null festlegen, bleibt es ungleich Null und SaveChanges löst aus, ohne zu speichern</span><span class="sxs-lookup"><span data-stu-id="65bdf-214">Since *Restrict* tells EF to not automatically set the FK to null, it remains non-null and SaveChanges throws without saving</span></span>

## <a name="cascading-to-untracked-entities"></a><span data-ttu-id="65bdf-215">Kaskadierende verfolgte Entitäten</span><span class="sxs-lookup"><span data-stu-id="65bdf-215">Cascading to untracked entities</span></span>

<span data-ttu-id="65bdf-216">Beim Aufruf *SaveChanges*, die Delete Cascade-Regeln gelten für alle Entitäten, die vom Kontext nachverfolgt werden.</span><span class="sxs-lookup"><span data-stu-id="65bdf-216">When you call *SaveChanges*, the cascade delete rules will be applied to any entities that are being tracked by the context.</span></span> <span data-ttu-id="65bdf-217">Dies ist die Situation, in der alle oben genannten Beispiele, ist SQL zum Löschen der Prinzipal/im übergeordneten Element (Blog) und alle abhängigen Elemente/untergeordneten Elemente (Beiträge) generiert wurde:</span><span class="sxs-lookup"><span data-stu-id="65bdf-217">This is the situation in all the examples shown above, which is why SQL was generated to delete both the principal/parent (blog) and all the dependents/children (posts):</span></span>

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="65bdf-218">Wenn nur der Prinzipalserver geladen – ist z. B. wenn eine Abfrage erfolgt für einen Blog ohne eine `Include(b => b.Posts)` auch Beiträge--enthalten dann SaveChanges generiert nur SQL aus, um den Prinzipal/im übergeordneten Element löschen:</span><span class="sxs-lookup"><span data-stu-id="65bdf-218">If only the principal is loaded--for example, when a query is made for a blog without an `Include(b => b.Posts)` to also include posts--then SaveChanges will only generate SQL to delete the principal/parent:</span></span>

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

<span data-ttu-id="65bdf-219">Die abhängigen Elemente/untergeordneten Elemente (Beiträge) wird nur gelöscht werden, wenn die Datenbank eine entsprechende überlappungsverhalten konfiguriert wurde.</span><span class="sxs-lookup"><span data-stu-id="65bdf-219">The dependents/children (posts) will only be deleted if the database has a corresponding cascade behavior configured.</span></span> <span data-ttu-id="65bdf-220">Wenn Sie EF verwenden, um die Datenbank zu erstellen, wird dieses Verhalten Cascade für Sie eingerichtet werden.</span><span class="sxs-lookup"><span data-stu-id="65bdf-220">If you use EF to create the database, this cascade behavior will be setup for you.</span></span>
