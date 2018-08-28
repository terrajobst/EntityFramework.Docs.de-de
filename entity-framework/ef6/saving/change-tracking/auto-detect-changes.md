---
title: Automatisches Erkennen von Änderungen – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: a8d1488d-9a54-4623-a76b-e81329ff2756
ms.openlocfilehash: bca33e12674c47cc7e047e85b11746c8e39246b4
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998098"
---
# <a name="automatic-detect-changes"></a><span data-ttu-id="7e606-102">Automatisches Erkennen von Änderungen</span><span class="sxs-lookup"><span data-stu-id="7e606-102">Automatic detect changes</span></span>
<span data-ttu-id="7e606-103">Bei Verwendung von POCO-Entitäten, die meisten erfolgt die Bestimmung der wie eine Entität geändert wurde (und aus diesem Grund müssen die Updates an die Datenbank gesendet werden), durch den Algorithmus für die Änderungen zu erkennen.</span><span class="sxs-lookup"><span data-stu-id="7e606-103">When using most POCO entities the determination of how an entity has changed (and therefore which updates need to be sent to the database) is handled by the Detect Changes algorithm.</span></span> <span data-ttu-id="7e606-104">Erkennen von Änderungen funktioniert durch das Erkennen von den Unterschieden zwischen der aktuellen Eigenschaftswerte der Entität und die ursprünglichen Eigenschaftswerte, die in einer Momentaufnahme gespeichert werden, wenn die Entität abgefragt oder angefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="7e606-104">Detect Changes works by detecting the differences between the current property values of the entity and the original property values that are stored in a snapshot when the entity was queried or attached.</span></span> <span data-ttu-id="7e606-105">Die in diesem Thema dargestellten Techniken gelten jeweils für Modelle, die mit Code First und dem EF-Designer erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="7e606-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="7e606-106">Standardmäßig führt Entity Framework erkennt Änderungen automatisch, wenn die folgenden Methoden aufgerufen werden:</span><span class="sxs-lookup"><span data-stu-id="7e606-106">By default, Entity Framework performs Detect Changes automatically when the following methods are called:</span></span>  

- <span data-ttu-id="7e606-107">DbSet.Find</span><span class="sxs-lookup"><span data-stu-id="7e606-107">DbSet.Find</span></span>  
- <span data-ttu-id="7e606-108">DbSet.Local</span><span class="sxs-lookup"><span data-stu-id="7e606-108">DbSet.Local</span></span>  
- <span data-ttu-id="7e606-109">"DbSet.Add"</span><span class="sxs-lookup"><span data-stu-id="7e606-109">DbSet.Add</span></span>  
- <span data-ttu-id="7e606-110">"DbSet.AddRange"</span><span class="sxs-lookup"><span data-stu-id="7e606-110">DbSet.AddRange</span></span>
- <span data-ttu-id="7e606-111">DbSet.Remove</span><span class="sxs-lookup"><span data-stu-id="7e606-111">DbSet.Remove</span></span>  
- <span data-ttu-id="7e606-112">DbSet.RemoveRange</span><span class="sxs-lookup"><span data-stu-id="7e606-112">DbSet.RemoveRange</span></span>
- <span data-ttu-id="7e606-113">DbSet.Attach</span><span class="sxs-lookup"><span data-stu-id="7e606-113">DbSet.Attach</span></span>  
- <span data-ttu-id="7e606-114">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="7e606-114">DbContext.SaveChanges</span></span>  
- <span data-ttu-id="7e606-115">DbContext.GetValidationErrors</span><span class="sxs-lookup"><span data-stu-id="7e606-115">DbContext.GetValidationErrors</span></span>  
- <span data-ttu-id="7e606-116">DbContext.Entry</span><span class="sxs-lookup"><span data-stu-id="7e606-116">DbContext.Entry</span></span>  
- <span data-ttu-id="7e606-117">DbChangeTracker.Entries</span><span class="sxs-lookup"><span data-stu-id="7e606-117">DbChangeTracker.Entries</span></span>  

## <a name="disabling-automatic-detection-of-changes"></a><span data-ttu-id="7e606-118">Deaktivieren die automatischen Erkennung von Änderungen</span><span class="sxs-lookup"><span data-stu-id="7e606-118">Disabling automatic detection of changes</span></span>  

<span data-ttu-id="7e606-119">Wenn Sie eine Vielzahl von Entities im Kontext Ihrer verfolgen und Sie eine dieser Methoden oft in einer Schleife aufrufen, können Sie erhebliche Leistungssteigerungen durch Deaktivieren der Erkennung von Änderungen für die Dauer der Schleife abrufen.</span><span class="sxs-lookup"><span data-stu-id="7e606-119">If you are tracking a lot of entities in your context and you call one of these methods many times in a loop, then you may get significant performance improvements by turning off detection of changes for the duration of the loop.</span></span> <span data-ttu-id="7e606-120">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="7e606-120">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    try
    {
        context.Configuration.AutoDetectChangesEnabled = false;

        // Make many calls in a loop
        foreach (var blog in aLotOfBlogs)
        {
            context.Blogs.Add(blog);
        }
    }
    finally
    {
        context.Configuration.AutoDetectChangesEnabled = true;
    }
}
```  

<span data-ttu-id="7e606-121">Vergessen Sie nicht die Erkennung von Änderungen nach der Schleife erneut zu aktivieren – wir haben einen Try/finally verwendet, um sicherzustellen, ist immer wieder aktiviert, auch wenn Code in der Schleife eine Ausnahme auslöst.</span><span class="sxs-lookup"><span data-stu-id="7e606-121">Don’t forget to re-enable detection of changes after the loop — We've used a try/finally to ensure it is always re-enabled even if code in the loop throws an exception.</span></span>  

<span data-ttu-id="7e606-122">Eine Alternative zu deaktivieren und erneut aktiviert ist, die automatischen Erkennung von Änderungen auf alle Uhrzeiten und entweder Aufrufkontext deaktiviert zu lassen. ChangeTracker.DetectChanges explizit, oder verwenden ändern Änderungsnachverfolgungsproxys sorgfältig.</span><span class="sxs-lookup"><span data-stu-id="7e606-122">An alternative to disabling and re-enabling is to leave automatic detection of changes turned off at all times and either call context.ChangeTracker.DetectChanges explicitly or use change tracking proxies diligently.</span></span> <span data-ttu-id="7e606-123">Beide Optionen sind erweiterte und kann es zu schwer erkennbaren Fehlern in Ihrer Anwendung verwenden sie daher mit Vorsicht.</span><span class="sxs-lookup"><span data-stu-id="7e606-123">Both of these options are advanced and can easily introduce subtle bugs into your application so use them with care.</span></span>  

<span data-ttu-id="7e606-124">Bei Bedarf hinzufügen oder Entfernen von zahlreichen Objekten aus einem Kontext, erwägen Sie die Verwendung von "DbSet.AddRange" und DbSet.RemoveRange an.</span><span class="sxs-lookup"><span data-stu-id="7e606-124">If you need to add or remove many objects from a context, consider using DbSet.AddRange and DbSet.RemoveRange.</span></span> <span data-ttu-id="7e606-125">Diese Methoden erkennen Änderungen automatisch nur einmal, wenn die Add- oder Remove-Vorgänge abgeschlossen sind.</span><span class="sxs-lookup"><span data-stu-id="7e606-125">This methods automatically detect changes only once after the add or remove operations are completed.</span></span> 
