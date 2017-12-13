---
title: "EF Core und EF6 – Auswahl der geeigneten Version"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 9a113e0965fa75a03510199fb75165f6e9be0bbd
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/27/2017
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a><span data-ttu-id="bd372-102">EF Core und EF6: Auswahl der geeigneten Version</span><span class="sxs-lookup"><span data-stu-id="bd372-102">EF Core and EF6: Which One Is Right for You</span></span>

<span data-ttu-id="bd372-103">Die folgenden Informationen helfen Ihnen bei der Auswahl zwischen Entity Framework Core und Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="bd372-103">The following information will help you choose between Entity Framework Core and Entity Framework 6.</span></span>

## <a name="guidance-for-new-applications"></a><span data-ttu-id="bd372-104">Leitfaden für neue Anwendungen</span><span class="sxs-lookup"><span data-stu-id="bd372-104">Guidance for new applications</span></span>

<span data-ttu-id="bd372-105">Da es sich bei EF Core um ein neues Produkt handelt und noch immer einige wichtige O/RM-Features fehlen, stellt EF6 für viele Anwendungen weiterhin die beste Wahl dar.</span><span class="sxs-lookup"><span data-stu-id="bd372-105">Because EF Core is a new product, and still lacks some critical O/RM features, EF6 will still be the most suitable choice for many applications.</span></span>

<span data-ttu-id="bd372-106">**EF Core wird für diese Art von Anwendungen empfohlen:**</span><span class="sxs-lookup"><span data-stu-id="bd372-106">**These are the types of applications we would recommend using EF Core for:**</span></span>

* <span data-ttu-id="bd372-107">Neue Anwendungen, die keine der Features benötigen, die noch nicht in EF Core implementiert sind.</span><span class="sxs-lookup"><span data-stu-id="bd372-107">New applications that do not require features that are not yet implemented in EF Core.</span></span> <span data-ttu-id="bd372-108">Überprüfen Sie anhand des [Featurevergleichs](features.md), ob EF Core eine geeignete Wahl für Ihre Anwendung ist.</span><span class="sxs-lookup"><span data-stu-id="bd372-108">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

* <span data-ttu-id="bd372-109">Anwendungen, die für .NET Core konzipiert sind, beispielsweise UWP- (Universal Windows Platform) und ASP.NET Core-Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="bd372-109">Applications that target .NET Core, such as Universal Windows Platform (UWP) and ASP.NET Core applications.</span></span> <span data-ttu-id="bd372-110">Diese Anwendungen können EF6 nicht verwenden, da für sie das .NET Framework (d.h. .NET Framework 4.5) benötigt wird.</span><span class="sxs-lookup"><span data-stu-id="bd372-110">These applications can not use EF6 as it requires the .NET Framework (i.e. .NET Framework 4.5).</span></span>

## <a name="guidance-for-existing-ef6-applications"></a><span data-ttu-id="bd372-111">Leitfaden für vorhandene EF6-Anwendungen</span><span class="sxs-lookup"><span data-stu-id="bd372-111">Guidance for existing EF6 applications</span></span>

<span data-ttu-id="bd372-112">Aufgrund der wesentlichen Änderungen in EF Core wird nicht empfohlen, eine EF6-Anwendung auf EF Core umzustellen – es sei denn, es gibt einen zwingenden Grund für eine solche Änderung.</span><span class="sxs-lookup"><span data-stu-id="bd372-112">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span> <span data-ttu-id="bd372-113">Wenn Sie zu EF Core wechseln möchten, um neue Features zu nutzen, sollten Sie sich vor der Umstellung über die Einschränkungen im Klaren sein.</span><span class="sxs-lookup"><span data-stu-id="bd372-113">If you want to move to EF Core to make use of new features, then make sure you are aware of its limitations before you start.</span></span> <span data-ttu-id="bd372-114">Überprüfen Sie anhand des [Featurevergleichs](features.md), ob EF Core eine geeignete Wahl für Ihre Anwendung ist.</span><span class="sxs-lookup"><span data-stu-id="bd372-114">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

<span data-ttu-id="bd372-115">**Sie sollten den Wechsel von EF6 zu EF Core weniger als Upgrade, sondern als eine Portierung betrachten.**</span><span class="sxs-lookup"><span data-stu-id="bd372-115">**You should view the move from EF6 to EF Core as a port rather than an upgrade.**</span></span> <span data-ttu-id="bd372-116">Weitere Informationen finden Sie unter [Portieren von EF6 auf EF Core](porting/index.md).</span><span class="sxs-lookup"><span data-stu-id="bd372-116">See [Porting from EF6 to EF Core](porting/index.md) for more information.</span></span>
