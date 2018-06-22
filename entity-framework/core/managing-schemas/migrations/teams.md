---
title: Migrationen in Team Umgebungen - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 40cbc1c1bb0273bf733fadb884bffadcceeb162b
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2017
ms.locfileid: "26053800"
---
<a name="migrations-in-team-environments"></a><span data-ttu-id="94a72-102">Migrationen in Team Umgebungen</span><span class="sxs-lookup"><span data-stu-id="94a72-102">Migrations in Team Environments</span></span>
===============================
<span data-ttu-id="94a72-103">Beim Arbeiten im Team-Umgebungen mit Migrationen auch Sie besondere Sorgfalt walten, um die Snapshot-Modelldatei.</span><span class="sxs-lookup"><span data-stu-id="94a72-103">When working with Migrations in team environments, pay extra attention to the model snapshot file.</span></span> <span data-ttu-id="94a72-104">Diese Datei können Sie feststellen, ob Ihres Kollegen Migration ordnungsgemäß mit Ihren Instanzen-der zusammengeführt werden soll, wenn Sie einen Konflikt zu beheben, durch die Migration vor der Freigabe erneut erstellen müssen.</span><span class="sxs-lookup"><span data-stu-id="94a72-104">This file can tell you if your teammate's migration merges cleanly with yours of if you need to resolve a conflict by re-creating your migration before sharing it.</span></span>

<a name="merging"></a><span data-ttu-id="94a72-105">Zusammenführen</span><span class="sxs-lookup"><span data-stu-id="94a72-105">Merging</span></span>
-------
<span data-ttu-id="94a72-106">Beim Zusammenführen von Migrationen von Ihre Teamkollegen darüber erhalten Sie möglicherweise Konflikte in Ihrem Modell Datenbankmomentaufnahme-Datei.</span><span class="sxs-lookup"><span data-stu-id="94a72-106">When you merge migrations from your teammates, you may get conflicts in your model snapshot file.</span></span> <span data-ttu-id="94a72-107">Wenn beide Änderungen nicht verbunden sind, die Zusammenführung ist trivial, und zwei Migrationen können parallel ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="94a72-107">If both changes are unrelated, the merge is trivial and the two migrations can coexist.</span></span> <span data-ttu-id="94a72-108">Beispielsweise erhalten Sie möglicherweise einen Merge-Konflikt in der typenkonfiguration der Customer-Entität, die wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="94a72-108">For example, you may get a merge conflict in the customer entity type configuration that looks like this:</span></span>

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

<span data-ttu-id="94a72-109">Da beide Eigenschaften im endgültigen Modell vorhanden sind müssen, führen Sie die Zusammenführung durch beide Eigenschaften hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="94a72-109">Since both of these properties need to exist in the final model, complete the merge by adding both properties.</span></span> <span data-ttu-id="94a72-110">In vielen Fällen kann das Versionskontrollsystem solche Änderungen automatisch für Sie zusammengeführt.</span><span class="sxs-lookup"><span data-stu-id="94a72-110">In many cases, your version control system may automatically merge such changes for you.</span></span>

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

<span data-ttu-id="94a72-111">In diesen Fällen sind die Migration und Ihres Kollegen Migration unabhängig voneinander.</span><span class="sxs-lookup"><span data-stu-id="94a72-111">In these cases, your migration and your teammate's migration are independent of each other.</span></span> <span data-ttu-id="94a72-112">Da diese zuerst angewendet werden konnten, müssen Sie weitere Änderungen vorzunehmen, um die Migration, bevor er mit Ihrem Team freigegeben wird.</span><span class="sxs-lookup"><span data-stu-id="94a72-112">Since either of them could be applied first, you don't need to make any additional changes to your migration before sharing it with your team.</span></span>

<a name="resolving-conflicts"></a><span data-ttu-id="94a72-113">Lösen von Konflikten</span><span class="sxs-lookup"><span data-stu-id="94a72-113">Resolving conflicts</span></span>
-------------------
<span data-ttu-id="94a72-114">In einigen Fällen auftreten ein Konflikts "true", beim Zusammenführen der Modell-Momentaufnahme-Modell.</span><span class="sxs-lookup"><span data-stu-id="94a72-114">Sometimes you encounter a true conflict when merging the model snapshot model.</span></span> <span data-ttu-id="94a72-115">Angenommen, Sie und Ihre Kollegin möglicherweise jeweils die gleiche Eigenschaft umbenannt.</span><span class="sxs-lookup"><span data-stu-id="94a72-115">For example, you and your teammate may each have renamed the same property.</span></span>

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

<span data-ttu-id="94a72-116">Wenn Sie diese Art von Konflikt auftritt, lösen Sie ihn nach der Migration neu zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="94a72-116">If you encounter this kind of conflict, resolve it by re-creating your migration.</span></span> <span data-ttu-id="94a72-117">Führen Sie folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="94a72-117">Follow these steps:</span></span>

1. <span data-ttu-id="94a72-118">Die Merge- und Ausführen eines Rollbacks zu Ihrem Arbeitsverzeichnis vor der Zusammenführung Abbrechen</span><span class="sxs-lookup"><span data-stu-id="94a72-118">Abort the merge and rollback to your working directory before the merge</span></span>
2. <span data-ttu-id="94a72-119">Entfernen der Migrations (behalten Sie jedoch Ihre modelländerungen)</span><span class="sxs-lookup"><span data-stu-id="94a72-119">Remove your migration (but keep your model changes)</span></span>
3. <span data-ttu-id="94a72-120">Zusammenführen von Änderungen Ihres Kollegen in Ihrem Arbeitsverzeichnis</span><span class="sxs-lookup"><span data-stu-id="94a72-120">Merge your teammate's changes into your working directory</span></span>
4. <span data-ttu-id="94a72-121">Fügen Sie die Migration erneut hinzu.</span><span class="sxs-lookup"><span data-stu-id="94a72-121">Re-add your migration</span></span>

<span data-ttu-id="94a72-122">Nachdem Sie auf diese Weise können die zwei Migrationen in der richtigen Reihenfolge angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="94a72-122">After doing this, the two migrations can be applied in the correct order.</span></span> <span data-ttu-id="94a72-123">Ihre Migration wird zuerst angewendet, die Spalte umbenennen *Alias*, danach Ihre Migration benennt es um *Benutzername*.</span><span class="sxs-lookup"><span data-stu-id="94a72-123">Their migration is applied first, renaming the column to *Alias*, thereafter your migration renames it to *Username*.</span></span>

<span data-ttu-id="94a72-124">Die Migration kann problemlos mit dem Rest des Teams freigegeben werden.</span><span class="sxs-lookup"><span data-stu-id="94a72-124">Your migration can safely be shared with the rest of the team.</span></span>
