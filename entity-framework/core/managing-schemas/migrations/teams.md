---
title: Migrationen in Teamumgebungen – EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: e8ff7f468d5ab6dbd6285f1abf9199e413288d10
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997694"
---
<a name="migrations-in-team-environments"></a><span data-ttu-id="4578c-102">Migrationen in Teamumgebungen</span><span class="sxs-lookup"><span data-stu-id="4578c-102">Migrations in Team Environments</span></span>
===============================
<span data-ttu-id="4578c-103">Bei der Arbeit mit Migrationen in teamumgebungen Achten Sie zusätzliche auf der Snapshot-Modelldatei.</span><span class="sxs-lookup"><span data-stu-id="4578c-103">When working with Migrations in team environments, pay extra attention to the model snapshot file.</span></span> <span data-ttu-id="4578c-104">Diese Datei können Sie erkennen, wenn Ihre Kollegin der Migration ordnungsgemäß mit dem eigenen zusammengeführt oder Sie einen Konflikt zu beheben, erstellen Sie Ihre Migration, bevor er freigegeben wird müssen.</span><span class="sxs-lookup"><span data-stu-id="4578c-104">This file can tell you if your teammate's migration merges cleanly with yours or if you need to resolve a conflict by re-creating your migration before sharing it.</span></span>

<a name="merging"></a><span data-ttu-id="4578c-105">Zusammenführen</span><span class="sxs-lookup"><span data-stu-id="4578c-105">Merging</span></span>
-------
<span data-ttu-id="4578c-106">Beim Zusammenführen von Migrationen von Ihren Teamkollegen erhalten Sie möglicherweise Konflikte in Ihrem Modell Datenbankmomentaufnahme-Datei.</span><span class="sxs-lookup"><span data-stu-id="4578c-106">When you merge migrations from your teammates, you may get conflicts in your model snapshot file.</span></span> <span data-ttu-id="4578c-107">Wenn beide Änderungen unabhängig sind, die Zusammenführung ist einfach, und zwei livemigrationen können gleichzeitig vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="4578c-107">If both changes are unrelated, the merge is trivial and the two migrations can coexist.</span></span> <span data-ttu-id="4578c-108">Beispielsweise erhalten Sie möglicherweise ein Zusammenführungskonflikt in der typenkonfiguration der Customer-Entität, die folgendermaßen aussieht:</span><span class="sxs-lookup"><span data-stu-id="4578c-108">For example, you may get a merge conflict in the customer entity type configuration that looks like this:</span></span>

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

<span data-ttu-id="4578c-109">Da diese beiden Eigenschaften im endgültigen Modell vorhanden sind müssen, führen Sie die Zusammenführung durch beide Eigenschaften hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="4578c-109">Since both of these properties need to exist in the final model, complete the merge by adding both properties.</span></span> <span data-ttu-id="4578c-110">In vielen Fällen kann Ihr Versionskontrollsystem solche Änderungen automatisch für Sie zusammenführen.</span><span class="sxs-lookup"><span data-stu-id="4578c-110">In many cases, your version control system may automatically merge such changes for you.</span></span>

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

<span data-ttu-id="4578c-111">In diesen Fällen sind die Migration und Ihre Kollegin der Migration unabhängig voneinander.</span><span class="sxs-lookup"><span data-stu-id="4578c-111">In these cases, your migration and your teammate's migration are independent of each other.</span></span> <span data-ttu-id="4578c-112">Da es sich bei einem der beiden zuerst angewendet werden kann, müssen Sie keine weiteren Änderungen vornehmen, um die Migration, bevor er mit Ihrem Team freigegeben wird.</span><span class="sxs-lookup"><span data-stu-id="4578c-112">Since either of them could be applied first, you don't need to make any additional changes to your migration before sharing it with your team.</span></span>

<a name="resolving-conflicts"></a><span data-ttu-id="4578c-113">Lösen von Konflikten</span><span class="sxs-lookup"><span data-stu-id="4578c-113">Resolving conflicts</span></span>
-------------------
<span data-ttu-id="4578c-114">Sie treten gelegentlich einen Konflikt mit "true", beim mergen der Modell-Momentaufnahme-Modell.</span><span class="sxs-lookup"><span data-stu-id="4578c-114">Sometimes you encounter a true conflict when merging the model snapshot model.</span></span> <span data-ttu-id="4578c-115">Angenommen, Sie und Ihre Kollegin möglicherweise jeweils die gleiche Eigenschaft umbenannt.</span><span class="sxs-lookup"><span data-stu-id="4578c-115">For example, you and your teammate may each have renamed the same property.</span></span>

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

<span data-ttu-id="4578c-116">Wenn Sie diese Art von Konflikt auftritt, müssen beheben Sie ihn, indem der Migration neu zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="4578c-116">If you encounter this kind of conflict, resolve it by re-creating your migration.</span></span> <span data-ttu-id="4578c-117">Führen Sie folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="4578c-117">Follow these steps:</span></span>

1. <span data-ttu-id="4578c-118">Abbrechen der zusammenführen und ein Rollback in Ihr Arbeitsverzeichnis vor der Zusammenführung</span><span class="sxs-lookup"><span data-stu-id="4578c-118">Abort the merge and rollback to your working directory before the merge</span></span>
2. <span data-ttu-id="4578c-119">Entfernen Sie die Migration (aber behalten Sie Ihr Modell ändert)</span><span class="sxs-lookup"><span data-stu-id="4578c-119">Remove your migration (but keep your model changes)</span></span>
3. <span data-ttu-id="4578c-120">Ihre Kollegin der Änderungen in Ihrem Arbeitsverzeichnis zusammenführen</span><span class="sxs-lookup"><span data-stu-id="4578c-120">Merge your teammate's changes into your working directory</span></span>
4. <span data-ttu-id="4578c-121">Fügen Sie Ihre Migration erneut hinzu.</span><span class="sxs-lookup"><span data-stu-id="4578c-121">Re-add your migration</span></span>

<span data-ttu-id="4578c-122">Nachdem Sie auf diese Weise können die zwei Migrationen in der richtigen Reihenfolge angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="4578c-122">After doing this, the two migrations can be applied in the correct order.</span></span> <span data-ttu-id="4578c-123">Ihre Migration wird zuerst angewendet, die Spalte umbenennen *Alias*, danach Ihre Migration benennt es um *Benutzername*.</span><span class="sxs-lookup"><span data-stu-id="4578c-123">Their migration is applied first, renaming the column to *Alias*, thereafter your migration renames it to *Username*.</span></span>

<span data-ttu-id="4578c-124">Die Migration kann mit dem Rest des Teams sicher freigegeben werden.</span><span class="sxs-lookup"><span data-stu-id="4578c-124">Your migration can safely be shared with the rest of the team.</span></span>
