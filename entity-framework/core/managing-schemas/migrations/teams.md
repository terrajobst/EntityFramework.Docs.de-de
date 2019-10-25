---
title: Migrationen in Team Umgebungen-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/teams
ms.openlocfilehash: e6a1b86761a201cbcae34cced7e64f11df37a420
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811984"
---
# <a name="migrations-in-team-environments"></a><span data-ttu-id="583b3-102">Migrationen in Teamumgebungen</span><span class="sxs-lookup"><span data-stu-id="583b3-102">Migrations in Team Environments</span></span>

<span data-ttu-id="583b3-103">Wenn Sie mit Migrationen in Team Umgebungen arbeiten, achten Sie besonders auf die Modell Momentaufnahme-Datei.</span><span class="sxs-lookup"><span data-stu-id="583b3-103">When working with Migrations in team environments, pay extra attention to the model snapshot file.</span></span> <span data-ttu-id="583b3-104">Diese Datei kann Ihnen mitteilen, ob die Migration Ihres Teamkollegen ordnungsgemäß mit Ihrem zusammengeführt wird oder ob Sie einen Konflikt auflösen müssen, indem Sie die Migration neu erstellen, bevor Sie Sie freigeben.</span><span class="sxs-lookup"><span data-stu-id="583b3-104">This file can tell you if your teammate's migration merges cleanly with yours or if you need to resolve a conflict by re-creating your migration before sharing it.</span></span>

## <a name="merging"></a><span data-ttu-id="583b3-105">Zusammenführen</span><span class="sxs-lookup"><span data-stu-id="583b3-105">Merging</span></span>

<span data-ttu-id="583b3-106">Wenn Sie Migrationen von ihren Teamkollegen zusammenführen, treten möglicherweise Konflikte in der Modell Momentaufnahme-Datei auf.</span><span class="sxs-lookup"><span data-stu-id="583b3-106">When you merge migrations from your teammates, you may get conflicts in your model snapshot file.</span></span> <span data-ttu-id="583b3-107">Wenn beide Änderungen nicht miteinander verbunden sind, ist die Zusammenführung trivial, und die beiden Migrationen können nebeneinander bestehen.</span><span class="sxs-lookup"><span data-stu-id="583b3-107">If both changes are unrelated, the merge is trivial and the two migrations can coexist.</span></span> <span data-ttu-id="583b3-108">Beispielsweise erhalten Sie möglicherweise einen Mergekonflikt in der Customer-Entitätstyp Konfiguration, der wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="583b3-108">For example, you may get a merge conflict in the customer entity type configuration that looks like this:</span></span>

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

<span data-ttu-id="583b3-109">Da diese beiden Eigenschaften im endgültigen Modell vorhanden sein müssen, müssen Sie die Zusammenführung durch Hinzufügen beider Eigenschaften vervollständigen.</span><span class="sxs-lookup"><span data-stu-id="583b3-109">Since both of these properties need to exist in the final model, complete the merge by adding both properties.</span></span> <span data-ttu-id="583b3-110">In vielen Fällen kann Ihr Versionskontrollsystem solche Änderungen automatisch zusammenführen.</span><span class="sxs-lookup"><span data-stu-id="583b3-110">In many cases, your version control system may automatically merge such changes for you.</span></span>

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

<span data-ttu-id="583b3-111">In diesen Fällen sind die Migration und die Migration Ihres Teamkollegen voneinander unabhängig.</span><span class="sxs-lookup"><span data-stu-id="583b3-111">In these cases, your migration and your teammate's migration are independent of each other.</span></span> <span data-ttu-id="583b3-112">Da eine der beiden Optionen zuerst angewendet werden kann, müssen Sie keine weiteren Änderungen an der Migration vornehmen, bevor Sie Sie für Ihr Team freigeben.</span><span class="sxs-lookup"><span data-stu-id="583b3-112">Since either of them could be applied first, you don't need to make any additional changes to your migration before sharing it with your team.</span></span>

## <a name="resolving-conflicts"></a><span data-ttu-id="583b3-113">Auflösen von Konflikten</span><span class="sxs-lookup"><span data-stu-id="583b3-113">Resolving conflicts</span></span>

<span data-ttu-id="583b3-114">Manchmal tritt beim Zusammenführen des Modell Momentaufnahme-Modells ein echter Konflikt auf.</span><span class="sxs-lookup"><span data-stu-id="583b3-114">Sometimes you encounter a true conflict when merging the model snapshot model.</span></span> <span data-ttu-id="583b3-115">Beispielsweise können Sie und Ihr Teamkollegen die gleiche Eigenschaft umbenennen.</span><span class="sxs-lookup"><span data-stu-id="583b3-115">For example, you and your teammate may each have renamed the same property.</span></span>

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

<span data-ttu-id="583b3-116">Wenn diese Art von Konflikt auftritt, beheben Sie diese, indem Sie die Migration neu erstellen.</span><span class="sxs-lookup"><span data-stu-id="583b3-116">If you encounter this kind of conflict, resolve it by re-creating your migration.</span></span> <span data-ttu-id="583b3-117">Führen Sie folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="583b3-117">Follow these steps:</span></span>

1. <span data-ttu-id="583b3-118">Abbrechen des Merge und Zurücksetzen auf das Arbeitsverzeichnis vor dem Merge</span><span class="sxs-lookup"><span data-stu-id="583b3-118">Abort the merge and rollback to your working directory before the merge</span></span>
2. <span data-ttu-id="583b3-119">Entfernen der Migration (behalten Sie jedoch Ihre Modelländerungen bei)</span><span class="sxs-lookup"><span data-stu-id="583b3-119">Remove your migration (but keep your model changes)</span></span>
3. <span data-ttu-id="583b3-120">Zusammenführen der Änderungen Ihres Teamkollegen in Ihrem Arbeitsverzeichnis</span><span class="sxs-lookup"><span data-stu-id="583b3-120">Merge your teammate's changes into your working directory</span></span>
4. <span data-ttu-id="583b3-121">Erneutes Hinzufügen der Migration</span><span class="sxs-lookup"><span data-stu-id="583b3-121">Re-add your migration</span></span>

<span data-ttu-id="583b3-122">Anschließend können die beiden Migrationen in der richtigen Reihenfolge angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="583b3-122">After doing this, the two migrations can be applied in the correct order.</span></span> <span data-ttu-id="583b3-123">Die Migration wird zuerst angewendet, und die Spalte wird in *Alias*umbenannt. Anschließend wird Sie von der Migration in *username*umbenannt.</span><span class="sxs-lookup"><span data-stu-id="583b3-123">Their migration is applied first, renaming the column to *Alias*, thereafter your migration renames it to *Username*.</span></span>

<span data-ttu-id="583b3-124">Ihre Migration kann für den Rest des Teams sicher freigegeben werden.</span><span class="sxs-lookup"><span data-stu-id="583b3-124">Your migration can safely be shared with the rest of the team.</span></span>
