---
title: "Speichern im Zusammenhang, Daten per Push – EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
ms.technology: entity-framework-core
uid: core/saving/related-data
ms.openlocfilehash: 078879163002cb66e0f0f439415789963181ec15
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="saving-related-data"></a><span data-ttu-id="a5969-102">Speichern verwandter Daten</span><span class="sxs-lookup"><span data-stu-id="a5969-102">Saving Related Data</span></span>

<span data-ttu-id="a5969-103">Sie können auch vornehmen, zusätzlich zu den isolierten Entitäten verwenden, der im Modell definierten Beziehungen.</span><span class="sxs-lookup"><span data-stu-id="a5969-103">In addition to isolated entities, you can also make use of the relationships defined in your model.</span></span>

> [!TIP]  
> <span data-ttu-id="a5969-104">Sie können anzeigen, dass dieser Artikel [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="a5969-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) on GitHub.</span></span>

## <a name="adding-a-graph-of-new-entities"></a><span data-ttu-id="a5969-105">Hinzufügen von neuen Entitäten in einem Diagramm</span><span class="sxs-lookup"><span data-stu-id="a5969-105">Adding a graph of new entities</span></span>

<span data-ttu-id="a5969-106">Wenn Sie mehrere neue verknüpfte Entitäten erstellen, verursacht Hinzufügen von mindestens einem von ihnen an den Kontext zu hinzugefügt werden andere.</span><span class="sxs-lookup"><span data-stu-id="a5969-106">If you create several new related entities, adding one of them to the context will cause the others to be added too.</span></span>

<span data-ttu-id="a5969-107">Im folgenden Beispiel werden im Blog und drei verwandten Beiträge alle in der Datenbank eingefügt.</span><span class="sxs-lookup"><span data-stu-id="a5969-107">In the following example, the blog and three related posts are all inserted into the database.</span></span> <span data-ttu-id="a5969-108">Die Beiträge gefunden und hinzugefügt werden, da es über erreichbar sind, werden die `Blog.Posts` Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="a5969-108">The posts are found and added, because they are reachable via the `Blog.Posts` navigation property.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

## <a name="adding-a-related-entity"></a><span data-ttu-id="a5969-109">Hinzufügen einer verknüpften Entität</span><span class="sxs-lookup"><span data-stu-id="a5969-109">Adding a related entity</span></span>

<span data-ttu-id="a5969-110">Wenn Sie eine neue Entität aus der Navigationseigenschaft einer Entität, die bereits vom Kontext nachverfolgt wird verweisen, wird die Entität ermittelt und in die Datenbank eingefügt.</span><span class="sxs-lookup"><span data-stu-id="a5969-110">If you reference a new entity from the navigation property of an entity that is already tracked by the context, the entity will be discovered and inserted into the database.</span></span>

<span data-ttu-id="a5969-111">Im folgenden Beispiel die `post` Entität eingefügt werden, weil es hinzugefügt wird die `Posts` Eigenschaft von der `blog` Entität, die sich aus der Datenbank abgerufen wurde.</span><span class="sxs-lookup"><span data-stu-id="a5969-111">In the following example, the `post` entity is inserted because it is added to the `Posts` property of the `blog` entity which was fetched from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a><span data-ttu-id="a5969-112">Ändern von Beziehungen</span><span class="sxs-lookup"><span data-stu-id="a5969-112">Changing relationships</span></span>

<span data-ttu-id="a5969-113">Wenn Sie die Navigationseigenschaft einer Entität ändern, wird die entsprechende Fremdschlüsselspalte in der Datenbank geändert werden.</span><span class="sxs-lookup"><span data-stu-id="a5969-113">If you change the navigation property of an entity, the corresponding changes will be made to the foreign key column in the database.</span></span>

<span data-ttu-id="a5969-114">Im folgenden Beispiel die `post` Entität wird aktualisiert, um mit dem neuen gehören `blog` Entität da seine `Blog` Navigationseigenschaft auf festgelegt ist `blog`.</span><span class="sxs-lookup"><span data-stu-id="a5969-114">In the following example, the `post` entity is updated to belong to the new `blog` entity because its `Blog` navigation property is set to point to `blog`.</span></span> <span data-ttu-id="a5969-115">Beachten Sie, dass `blog` wird auch in die Datenbank eingefügt werden, da es sich um eine neue Entität handelt, die von der Navigationseigenschaft einer Entität verwiesen wird, die bereits vom Kontext nachverfolgt wird (`post`).</span><span class="sxs-lookup"><span data-stu-id="a5969-115">Note that `blog` will also be inserted into the database because it is a new entity that is referenced by the navigation property of an entity that is already tracked by the context (`post`).</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a><span data-ttu-id="a5969-116">Entfernen von Beziehungen</span><span class="sxs-lookup"><span data-stu-id="a5969-116">Removing relationships</span></span>

<span data-ttu-id="a5969-117">Sie können eine Beziehung entfernen, indem Sie eine Referenz Navigation zu `null`, oder entfernen Sie die verknüpfte Entität aus einer Auflistung Navigation.</span><span class="sxs-lookup"><span data-stu-id="a5969-117">You can remove a relationship by setting a reference navigation to `null`, or removing the related entity from a collection navigation.</span></span>

<span data-ttu-id="a5969-118">Entfernen einer Beziehung kann Nebeneffekte haben, für die abhängige Entität, gemäß der Cascade gelöscht Verhalten in der Beziehung konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="a5969-118">Removing a relationship can have side effects on the dependent entity, according to the cascade delete behavior configured in the relationship.</span></span>

<span data-ttu-id="a5969-119">Wird standardmäßig für die erforderlichen Beziehungen eine Cascade Delete-Verhalten konfiguriert ist, und die untergeordneten/abhängige Entität aus der Datenbank gelöscht werden.</span><span class="sxs-lookup"><span data-stu-id="a5969-119">By default, for required relationships, a cascade delete behavior is configured and the child/dependent entity will be deleted from the database.</span></span> <span data-ttu-id="a5969-120">Für optionale Beziehungen kaskadierendes Delete ist nicht standardmäßig konfiguriert werden, aber die foreign Key-Eigenschaft festgelegt werden auf Null.</span><span class="sxs-lookup"><span data-stu-id="a5969-120">For optional relationships, cascade delete is not configured by default, but the foreign key property will be set to null.</span></span>

<span data-ttu-id="a5969-121">Finden Sie unter [erforderlichen und optionalen Beziehungen](../modeling/relationships.md#required-and-optional-relationships) zu erfahren, wie die Requiredness von Beziehungen konfiguriert werden kann.</span><span class="sxs-lookup"><span data-stu-id="a5969-121">See [Required and Optional Relationships](../modeling/relationships.md#required-and-optional-relationships) to learn about how the requiredness of relationships can be configured.</span></span>

<span data-ttu-id="a5969-122">Finden Sie unter [Löschweitergabe](cascade-delete.md) für Weitere Informationen zu den wie Löschweitergabe Verhalten arbeiten, wie diese explizit konfiguriert werden können und wie sie gemäß der Konvention ausgewählt werden.</span><span class="sxs-lookup"><span data-stu-id="a5969-122">See [Cascade Delete](cascade-delete.md) for more details on how cascade delete behaviors work, how they can be configured explicitly and  how they are selected by convention.</span></span>

<span data-ttu-id="a5969-123">Im folgenden Beispiel wird eine kaskadierte Löschung auf der Beziehung zwischen konfiguriert `Blog` und `Post`, sodass die `post` Entität aus der Datenbank gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="a5969-123">In the following example, a cascade delete is configured on the relationship between `Blog` and `Post`, so the `post` entity is deleted from the database.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
