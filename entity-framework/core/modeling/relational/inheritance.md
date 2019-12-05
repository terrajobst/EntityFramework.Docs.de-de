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
# <a name="inheritance-relational-database"></a>Vererbung (relationale Datenbank)

> [!NOTE]  
> Die Konfiguration in diesem Abschnitt gilt allgemein für relationale Datenbanken. Die hier gezeigten Erweiterungsmethoden werden verfügbar, wenn Sie einen relationalen Datenbankanbieter installieren (aufgrund des gemeinsam genutzten Pakets *Microsoft.EntityFrameworkCore.Relational*).

Die Vererbung im EF-Modell wird verwendet, um zu steuern, wie die Vererbung in den Entitäts Klassen in der Datenbank dargestellt wird.

> [!NOTE]  
> Derzeit wird nur das TPH-Muster (Table-per Hierarchy) in EF Core implementiert. Andere gängige Muster wie Tabelle pro Typ (TPT) und Table-per-konkrete-Type (TPC) sind noch nicht verfügbar.

## <a name="conventions"></a>Konventionen

Standardmäßig wird die Vererbung mithilfe des TPH-Musters (Table-per Hierarchy) zugeordnet. TPH verwendet eine einzelne Tabelle, um die Daten für alle Typen in der Hierarchie zu speichern. Eine diskriminatorspalte wird verwendet, um den Typ zu identifizieren, den jede Zeile darstellt.

EF Core wird nur dann eine Vererbung eingerichtet, wenn mindestens zwei geerbte Typen explizit in das Modell eingeschlossen werden (Weitere Informationen finden Sie unter [Vererbung](../inheritance.md) ).

Im folgenden finden Sie ein Beispiel für ein einfaches Vererbungs Szenario und die in einer relationalen Datenbanktabelle gespeicherten Daten mithilfe des TPH-Musters. Die *diskriminatorspalte* gibt an, welcher Typ von *Blog* in den einzelnen Zeilen gespeichert wird.

[!code-csharp[Main](../../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs#Model)]

![Bild](_static/inheritance-tph-data.png)

>[!NOTE]
> Bei Verwendung der TPH-Zuordnung werden bei Bedarf automatisch NULL-Werte für Daten Bank Spalten festgelegt.

## <a name="data-annotations"></a>Datenanmerkungen

Zum Konfigurieren der Vererbung können keine Daten Anmerkungen verwendet werden.

## <a name="fluent-api"></a>Fluent-API

Sie können die fließende API verwenden, um den Namen und den Typ der diskriminatorspalte und die Werte zu konfigurieren, die zum Identifizieren der einzelnen Typen in der Hierarchie verwendet werden.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs#Inheritance)]

## <a name="configuring-the-discriminator-property"></a>Konfigurieren der diskriminatoreigenschaft

In den obigen Beispielen wird der Diskriminator als [Schatten Eigenschaft](xref:core/modeling/shadow-properties) für die Basis Entität der Hierarchie erstellt. Da es sich um eine Eigenschaft im Modell handelt, kann Sie genau wie andere Eigenschaften konfiguriert werden. So legen Sie z. b. die maximale Länge fest, wenn der standardmäßige Diskriminator verwendet wird:

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/DefaultDiscriminator.cs#DiscriminatorConfiguration)]

Der Diskriminator kann auch einer .net-Eigenschaft in der Entität zugeordnet und konfiguriert werden. Beispiel:

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs#NonShadowDiscriminator)]

## <a name="shared-columns"></a>Freigegebene Spalten

Wenn zwei gleich geordnete Entitäts Typen über eine Eigenschaft mit demselben Namen verfügen, werden Sie standardmäßig zwei separaten Spalten zugeordnet. Wenn Sie jedoch kompatibel sind, können Sie derselben Spalte zugeordnet werden:

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs#SharedTPHColumns)]