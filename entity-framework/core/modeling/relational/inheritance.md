---
title: Vererbung (relationale Datenbank)-EF Core
description: Konfigurieren der Vererbung von Entitäts Typen in einer relationalen Datenbank mithilfe von Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 30e25aa2968ceab03404baddb46e0ae59fc3ea6b
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824745"
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="b249b-103">Vererbung (relationale Datenbank)</span><span class="sxs-lookup"><span data-stu-id="b249b-103">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="b249b-104">Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="b249b-104">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="b249b-105">Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="b249b-105">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="b249b-106">Die Vererbung im EF-Modell wird verwendet, um zu steuern, wie die Vererbung in den Entitäts Klassen in der Datenbank dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="b249b-106">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="b249b-107">Derzeit wird nur das TPH-Muster (Table-per Hierarchy) in EF Core implementiert.</span><span class="sxs-lookup"><span data-stu-id="b249b-107">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="b249b-108">Andere gängige Muster wie Tabelle pro Typ (TPT) und Table-per-konkrete-Type (TPC) sind noch nicht verfügbar.</span><span class="sxs-lookup"><span data-stu-id="b249b-108">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="b249b-109">Konventionen</span><span class="sxs-lookup"><span data-stu-id="b249b-109">Conventions</span></span>

<span data-ttu-id="b249b-110">Standardmäßig wird die Vererbung mithilfe des TPH-Musters (Table-per Hierarchy) zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="b249b-110">By default, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="b249b-111">TPH verwendet eine einzelne Tabelle, um die Daten für alle Typen in der Hierarchie zu speichern.</span><span class="sxs-lookup"><span data-stu-id="b249b-111">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="b249b-112">Eine diskriminatorspalte wird verwendet, um den Typ zu identifizieren, den jede Zeile darstellt.</span><span class="sxs-lookup"><span data-stu-id="b249b-112">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="b249b-113">EF Core wird nur dann eine Vererbung eingerichtet, wenn mindestens zwei geerbte Typen explizit in das Modell eingeschlossen werden (Weitere Informationen finden Sie unter [Vererbung](../inheritance.md) ).</span><span class="sxs-lookup"><span data-stu-id="b249b-113">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="b249b-114">Im folgenden finden Sie ein Beispiel für ein einfaches Vererbungs Szenario und die in einer relationalen Datenbanktabelle gespeicherten Daten mithilfe des TPH-Musters.</span><span class="sxs-lookup"><span data-stu-id="b249b-114">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="b249b-115">Die *diskriminatorspalte* gibt an, welcher Typ von *Blog* in den einzelnen Zeilen gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="b249b-115">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs#Model)]

![Bild](_static/inheritance-tph-data.png)

>[!NOTE]
> <span data-ttu-id="b249b-117">Bei Verwendung der TPH-Zuordnung werden bei Bedarf automatisch NULL-Werte für Daten Bank Spalten festgelegt.</span><span class="sxs-lookup"><span data-stu-id="b249b-117">Database columns are automatically made nullable as necessary when using TPH mapping.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="b249b-118">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="b249b-118">Data Annotations</span></span>

<span data-ttu-id="b249b-119">Zum Konfigurieren der Vererbung können keine Daten Anmerkungen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="b249b-119">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="b249b-120">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="b249b-120">Fluent API</span></span>

<span data-ttu-id="b249b-121">Sie können die fließende API verwenden, um den Namen und den Typ der diskriminatorspalte und die Werte zu konfigurieren, die zum Identifizieren der einzelnen Typen in der Hierarchie verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="b249b-121">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs#Inheritance)]

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="b249b-122">Konfigurieren der diskriminatoreigenschaft</span><span class="sxs-lookup"><span data-stu-id="b249b-122">Configuring the discriminator property</span></span>

<span data-ttu-id="b249b-123">In den obigen Beispielen wird der Diskriminator als [Schatten Eigenschaft](xref:core/modeling/shadow-properties) für die Basis Entität der Hierarchie erstellt.</span><span class="sxs-lookup"><span data-stu-id="b249b-123">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="b249b-124">Da es sich um eine Eigenschaft im Modell handelt, kann Sie genau wie andere Eigenschaften konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="b249b-124">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="b249b-125">So legen Sie z. b. die maximale Länge fest, wenn der standardmäßige Diskriminator verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="b249b-125">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/DefaultDiscriminator.cs#DiscriminatorConfiguration)]

<span data-ttu-id="b249b-126">Der Diskriminator kann auch einer .net-Eigenschaft in der Entität zugeordnet und konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="b249b-126">The discriminator can also be mapped to a .NET property in your entity and configure it.</span></span> <span data-ttu-id="b249b-127">Beispiel:</span><span class="sxs-lookup"><span data-stu-id="b249b-127">For example:</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs#NonShadowDiscriminator)]

## <a name="shared-columns"></a><span data-ttu-id="b249b-128">Freigegebene Spalten</span><span class="sxs-lookup"><span data-stu-id="b249b-128">Shared columns</span></span>

<span data-ttu-id="b249b-129">Wenn zwei gleich geordnete Entitäts Typen über eine Eigenschaft mit demselben Namen verfügen, werden Sie standardmäßig zwei separaten Spalten zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="b249b-129">When two sibling entity types have a property with the same name they will be mapped to two separate columns by default.</span></span> <span data-ttu-id="b249b-130">Wenn Sie jedoch kompatibel sind, können Sie derselben Spalte zugeordnet werden:</span><span class="sxs-lookup"><span data-stu-id="b249b-130">But if they are compatible they can be mapped to the same column:</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs#SharedTPHColumns)]