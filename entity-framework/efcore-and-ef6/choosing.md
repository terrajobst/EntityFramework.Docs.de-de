---
title: EF Core und EF6 – Auswahl der geeigneten Version
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 17c81e0d6c384c06e45f0cf4f338d4ba402788e1
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949139"
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a>EF Core und EF6: Auswahl der geeigneten Version

Die folgenden Informationen helfen Ihnen bei der Auswahl zwischen Entity Framework Core und Entity Framework 6.

## <a name="guidance-for-new-applications"></a>Leitfaden für neue Anwendungen

Wenn Ihre Anwendung keine Features benötigt, die noch nicht in EF Core implementiert wurden, können Sie EF Core und alle zugehörigen Funktionen für neue Anwendungen nutzen.

EF6 erfordert .NET Framework 4.0 oder höher und wird nur unter Windows unterstützt (d.h., EF6 kann weder unter .NET Core noch unter anderen Betriebssystemen ausgeführt werden). Solange diese Einschränkungen kein Problem darstellen und die Anwendung keine neuen Features erfordert, die noch nicht für EF6 verfügbar sind, ist EF6 trotzdem eine gute Wahl für neue Anwendungen.

Überprüfen Sie anhand des [Featurevergleichs](features.md), ob EF Core eine geeignete Wahl für Ihre Anwendung ist.

## <a name="guidance-for-existing-ef6-applications"></a>Leitfaden für vorhandene EF6-Anwendungen

Aufgrund der wesentlichen Änderungen in EF Core wird nicht empfohlen, eine EF6-Anwendung auf EF Core umzustellen – es sei denn, es gibt einen zwingenden Grund für eine solche Änderung. Wenn Sie zu EF Core wechseln möchten, um neue Features zu nutzen, sollten Sie sich vor der Umstellung über die Einschränkungen im Klaren sein. Überprüfen Sie anhand des [Featurevergleichs](features.md), ob EF Core eine geeignete Wahl für Ihre Anwendung ist.

**Sie sollten den Wechsel von EF6 zu EF Core weniger als Upgrade, sondern als eine Portierung betrachten.** Weitere Informationen finden Sie unter [Portieren von EF6 auf EF Core](porting/index.md).
