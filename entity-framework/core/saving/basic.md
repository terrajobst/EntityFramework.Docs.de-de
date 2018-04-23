---
title: Grundlegende Save - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: deead323301dc4a0ee0748b4536ddff4596b99e6
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="basic-save"></a><span data-ttu-id="1dc7e-102">Grundlegende speichern</span><span class="sxs-lookup"><span data-stu-id="1dc7e-102">Basic Save</span></span>

<span data-ttu-id="1dc7e-103">Informationen Sie zum Hinzufügen, ändern und Entfernen von Daten mit Ihrem Kontext und Entität Klassen.</span><span class="sxs-lookup"><span data-stu-id="1dc7e-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="1dc7e-104">Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) finden Sie auf GitHub.</span><span class="sxs-lookup"><span data-stu-id="1dc7e-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="1dc7e-105">Hinzufügen von Daten</span><span class="sxs-lookup"><span data-stu-id="1dc7e-105">Adding Data</span></span>

<span data-ttu-id="1dc7e-106">Verwenden der *DbSet.Add* Methode, um neue Instanzen der Entitätsklassen hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="1dc7e-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="1dc7e-107">Die Daten werden in der Datenbank eingefügt werden, wenn Sie aufrufen *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="1dc7e-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> <span data-ttu-id="1dc7e-108">Die Add-, Anfügen und Update-Methoden, die die gesamte auf das vollständige Diagramm von Entitäten Arbeit an sie übergeben, wie beschrieben in der [verknüpften Daten](related-data.md) Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="1dc7e-108">The Add, Attach, and Update methods all work on the full graph of entities passed to them, as described in the [Related Data](related-data.md) section.</span></span> <span data-ttu-id="1dc7e-109">Alternativ kann die EntityEntry.State-Eigenschaft zum Festlegen des Status von nur einer einzigen Entität verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="1dc7e-109">Alternately, the EntityEntry.State property can be used to set the state of just a single entity.</span></span> <span data-ttu-id="1dc7e-110">Beispielsweise `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="1dc7e-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="updating-data"></a><span data-ttu-id="1dc7e-111">Aktualisieren von Daten</span><span class="sxs-lookup"><span data-stu-id="1dc7e-111">Updating Data</span></span>

<span data-ttu-id="1dc7e-112">EF erkennt automatisch Änderungen an einer vorhandenen Entität, die vom Kontext nachverfolgt wird.</span><span class="sxs-lookup"><span data-stu-id="1dc7e-112">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="1dc7e-113">Dies schließt Entitäten, die Sie laden/aus der Datenbank Abfragen und Entitäten, die zuvor hinzugefügt und in der Datenbank gespeichert wurden.</span><span class="sxs-lookup"><span data-stu-id="1dc7e-113">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="1dc7e-114">Ändern Sie die Werte für Eigenschaften ein, und rufen dann *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="1dc7e-114">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="1dc7e-115">Löschen von Daten</span><span class="sxs-lookup"><span data-stu-id="1dc7e-115">Deleting Data</span></span>

<span data-ttu-id="1dc7e-116">Verwenden der *DbSet.Remove* Methode, um Instanzen der Entitätsklassen zu löschen.</span><span class="sxs-lookup"><span data-stu-id="1dc7e-116">Use the *DbSet.Remove* method to delete instances of you entity classes.</span></span>

<span data-ttu-id="1dc7e-117">Wenn die Entität bereits in der Datenbank vorhanden ist, wird sie während der gelöscht werden *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="1dc7e-117">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="1dc7e-118">Wenn die Entität nicht noch in der Datenbank gespeichert wurde (d. h. es wird nachverfolgt, wie hinzugefügt) und wird aus dem Kontext entfernt und nicht mehr eingefügt, wenn *SaveChanges* aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="1dc7e-118">If the entity has not yet been saved to the database (i.e. it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="1dc7e-119">Mehrere Vorgänge in einer einzelnen SaveChanges</span><span class="sxs-lookup"><span data-stu-id="1dc7e-119">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="1dc7e-120">Sie können mehrere Hinzufügen/Aktualisieren/Entfernen-Vorgänge in einem einzigen Aufruf kombinieren *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="1dc7e-120">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="1dc7e-121">Für die meisten Datenbankanbieter *SaveChanges* ist transaktional.</span><span class="sxs-lookup"><span data-stu-id="1dc7e-121">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="1dc7e-122">Dies bedeutet, dass alle Vorgänge, die entweder erfolgreich oder nicht und die Vorgänge werden nie Links teilweise angewendet.</span><span class="sxs-lookup"><span data-stu-id="1dc7e-122">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
