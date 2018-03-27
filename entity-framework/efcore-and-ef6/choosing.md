---
title: EF Core und EF6 – Auswahl der geeigneten Version
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: f0a632902384a65ea3cddf752ad262c7a2e89e2e
ms.sourcegitcommit: 2ef0a4a90b01edd22b9206f8729b8de459ef8cab
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2018
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a><span data-ttu-id="0977a-102">EF Core und EF6: Auswahl der geeigneten Version</span><span class="sxs-lookup"><span data-stu-id="0977a-102">EF Core and EF6: Which One Is Right for You</span></span>

<span data-ttu-id="0977a-103">Die folgenden Informationen helfen Ihnen bei der Auswahl zwischen Entity Framework Core und Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="0977a-103">The following information will help you choose between Entity Framework Core and Entity Framework 6.</span></span>

## <a name="guidance-for-new-applications"></a><span data-ttu-id="0977a-104">Leitfaden für neue Anwendungen</span><span class="sxs-lookup"><span data-stu-id="0977a-104">Guidance for new applications</span></span>

<span data-ttu-id="0977a-105">Wenn Ihre Anwendung keine Features benötigt, die noch nicht in EF Core implementiert wurden, können Sie EF Core und alle zugehörigen Funktionen für neue Anwendungen nutzen.</span><span class="sxs-lookup"><span data-stu-id="0977a-105">Consider using EF Core for new applications if you want to take advantage of the all the capabilities of EF Core and your application does not require any features that are not yet implemented in EF Core.</span></span>

<span data-ttu-id="0977a-106">EF6 erfordert .NET Framework 4.0 oder höher und wird nur unter Windows unterstützt, d.h., es kann weder unter .NET Core noch unter anderen Betriebssystemen ausgeführt werden. Solange diese Einschränkungen kein Problem darstellen und die Anwendung keine neuen Features erfordert, die noch nicht für EF6 verfügbar sind, ist EF6 trotzdem eine gute Wahl für neue Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="0977a-106">EF6 requires .NET Framework 4.0 (or a later version) and is only supported on Windows (i.e. it does not run on .NET Core and is not supported in other operating systems), but it is still a viable choice for new applications as long those constraints are acceptable and the application does not require new features in EF Core that are not available to EF6.</span></span>

<span data-ttu-id="0977a-107">Überprüfen Sie anhand des [Featurevergleichs](features.md), ob EF Core eine geeignete Wahl für Ihre Anwendung ist.</span><span class="sxs-lookup"><span data-stu-id="0977a-107">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

## <a name="guidance-for-existing-ef6-applications"></a><span data-ttu-id="0977a-108">Leitfaden für vorhandene EF6-Anwendungen</span><span class="sxs-lookup"><span data-stu-id="0977a-108">Guidance for existing EF6 applications</span></span>

<span data-ttu-id="0977a-109">Aufgrund der wesentlichen Änderungen in EF Core wird nicht empfohlen, eine EF6-Anwendung auf EF Core umzustellen – es sei denn, es gibt einen zwingenden Grund für eine solche Änderung.</span><span class="sxs-lookup"><span data-stu-id="0977a-109">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span> <span data-ttu-id="0977a-110">Wenn Sie zu EF Core wechseln möchten, um neue Features zu nutzen, sollten Sie sich vor der Umstellung über die Einschränkungen im Klaren sein.</span><span class="sxs-lookup"><span data-stu-id="0977a-110">If you want to move to EF Core to make use of new features, then make sure you are aware of its limitations before you start.</span></span> <span data-ttu-id="0977a-111">Überprüfen Sie anhand des [Featurevergleichs](features.md), ob EF Core eine geeignete Wahl für Ihre Anwendung ist.</span><span class="sxs-lookup"><span data-stu-id="0977a-111">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

<span data-ttu-id="0977a-112">**Sie sollten den Wechsel von EF6 zu EF Core weniger als Upgrade, sondern als eine Portierung betrachten.**</span><span class="sxs-lookup"><span data-stu-id="0977a-112">**You should view the move from EF6 to EF Core as a port rather than an upgrade.**</span></span> <span data-ttu-id="0977a-113">Weitere Informationen finden Sie unter [Portieren von EF6 auf EF Core](porting/index.md).</span><span class="sxs-lookup"><span data-stu-id="0977a-113">See [Porting from EF6 to EF Core](porting/index.md) for more information.</span></span>
