---
title: Vererbung-EF Core
description: Konfigurieren der Vererbung von Entitäts Typen mit Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 10/27/2016
uid: core/modeling/inheritance
ms.openlocfilehash: 507854e3acc0347adee612e516b3e2e0b10f55cf
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502166"
---
# <a name="inheritance"></a>Vererbung

EF kann eine .net-Typhierarchie einer Datenbank zuordnen. Dies ermöglicht es Ihnen, Ihre .NET-Entitäten wie gewohnt mit Basis-und abgeleiteten Typen in Code zu schreiben und EF nahtlos das geeignete Datenbankschema zu erstellen, Abfragen auszugeben usw. Die tatsächlichen Details, wie eine Typhierarchie zugeordnet wird, sind Anbieter abhängig. Diese Seite beschreibt die Vererbungs Unterstützung im Kontext einer relationalen Datenbank.

Zurzeit unterstützt EF Core nur das TPH-Muster (Table-per Hierarchy). TPH verwendet eine einzelne Tabelle zum Speichern der Daten für alle Typen in der Hierarchie, und eine diskriminatorspalte wird verwendet, um den Typ zu identifizieren, den jede Zeile darstellt.

> [!NOTE]
> Die Tabelle pro Typ (TPT) und "Table-per-Concrete-Type" (TPC), die von EF6 unterstützt werden, werden von EF Core noch nicht unterstützt. TPT ist ein wichtiges Feature, das für EF Core 5,0 geplant ist.

## <a name="entity-type-hierarchy-mapping"></a>Entitätstyp-Hierarchie Zuordnung

Gemäß der Konvention wird von EF nur die Vererbung eingerichtet, wenn mindestens zwei geerbte Typen explizit im Modell enthalten sind. EF scannt nicht automatisch nach Basis Typen oder abgeleiteten Typen, die ansonsten nicht im Modell enthalten sind.

Sie können Typen in das Modell einschließen, indem Sie ein dbset für jeden Typ in der Vererbungs Hierarchie verfügbar machen:

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?name=InheritanceDbSets&highlight=3-4)]

Dieses Modell wird dem folgenden Datenbankschema zugeordnet (Beachten Sie die implizit erstellte *diskriminatorspalte* , die identifiziert, welche Art von *Blog* in den einzelnen Zeilen gespeichert ist):

![Bild](_static/inheritance-tph-data.png)

>[!NOTE]
> Bei Verwendung der TPH-Zuordnung werden bei Bedarf automatisch NULL-Werte für Daten Bank Spalten festgelegt. Beispielsweise kann die Spalte *rssurl* auf NULL festgelegt werden, da reguläre *Blog* Instanzen nicht über diese Eigenschaft verfügen.

Wenn Sie kein dbset für eine oder mehrere Entitäten in der Hierarchie verfügbar machen möchten, können Sie auch die fließende API verwenden, um sicherzustellen, dass Sie im Modell enthalten sind.

> [!TIP]
> Wenn Sie sich nicht auf Konventionen verlassen, können Sie den Basistyp explizit mithilfe `HasBaseType`angeben. Sie können auch `.HasBaseType((Type)null)` verwenden, um einen Entitätstyp aus der Hierarchie zu entfernen.

## <a name="discriminator-configuration"></a>Diskriminatorkonfiguration

Sie können den Namen und den Typ der diskriminatorspalte und die Werte, die zum Identifizieren der einzelnen Typen in der Hierarchie verwendet werden, konfigurieren:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorConfiguration.cs?name=DiscriminatorConfiguration&highlight=4-6)]

In den obigen Beispielen wurde der Diskriminator von EF implizit als [Schatten Eigenschaft](xref:core/modeling/shadow-properties) für die Basis Entität der Hierarchie hinzugefügt. Diese Eigenschaft kann wie jede andere konfiguriert werden:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorPropertyConfiguration.cs?name=DiscriminatorPropertyConfiguration&highlight=4-5)]

Schließlich kann der Diskriminator auch einer regulären .net-Eigenschaft in der Entität zugeordnet werden:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs?name=NonShadowDiscriminator&highlight=4)]

## <a name="shared-columns"></a>Freigegebene Spalten

Wenn zwei gleich geordnete Entitäts Typen in der Hierarchie über eine Eigenschaft mit demselben Namen verfügen, werden Sie standardmäßig zwei separaten Spalten zugeordnet. Wenn Ihr Typ jedoch identisch ist, können Sie derselben Daten Bank Spalte zugeordnet werden:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs?name=SharedTPHColumns&highlight=9,13)]
