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
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a>EF Core und EF6: Auswahl der geeigneten Version

Die folgenden Informationen helfen Ihnen bei der Auswahl zwischen Entity Framework Core und Entity Framework 6.

## <a name="guidance-for-new-applications"></a>Leitfaden für neue Anwendungen

Da es sich bei EF Core um ein neues Produkt handelt und noch immer einige wichtige O/RM-Features fehlen, stellt EF6 für viele Anwendungen weiterhin die beste Wahl dar.

**EF Core wird für diese Art von Anwendungen empfohlen:**

* Neue Anwendungen, die keine der Features benötigen, die noch nicht in EF Core implementiert sind. Überprüfen Sie anhand des [Featurevergleichs](features.md), ob EF Core eine geeignete Wahl für Ihre Anwendung ist.

* Anwendungen, die für .NET Core konzipiert sind, beispielsweise UWP- (Universal Windows Platform) und ASP.NET Core-Anwendungen. Diese Anwendungen können EF6 nicht verwenden, da für sie das .NET Framework (d.h. .NET Framework 4.5) benötigt wird.

## <a name="guidance-for-existing-ef6-applications"></a>Leitfaden für vorhandene EF6-Anwendungen

Aufgrund der wesentlichen Änderungen in EF Core wird nicht empfohlen, eine EF6-Anwendung auf EF Core umzustellen – es sei denn, es gibt einen zwingenden Grund für eine solche Änderung. Wenn Sie zu EF Core wechseln möchten, um neue Features zu nutzen, sollten Sie sich vor der Umstellung über die Einschränkungen im Klaren sein. Überprüfen Sie anhand des [Featurevergleichs](features.md), ob EF Core eine geeignete Wahl für Ihre Anwendung ist.

**Sie sollten den Wechsel von EF6 zu EF Core weniger als Upgrade, sondern als eine Portierung betrachten.** Weitere Informationen finden Sie unter [Portieren von EF6 auf EF Core](porting/index.md).
