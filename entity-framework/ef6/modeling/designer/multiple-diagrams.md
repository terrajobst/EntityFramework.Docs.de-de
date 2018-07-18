---
title: Mehrere Diagramme pro Modell – EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: b95db5c8-de8d-43bd-9ccc-5df6a5e25e1b
caps.latest.revision: 3
ms.openlocfilehash: 3a022b3e44ecd4b6b62224cb6494c794397a9739
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121345"
---
# <a name="multiple-diagrams-per-model"></a>Mehrere Diagramme pro Modell
> [!NOTE]
> **EF5 oder höher, nur** -APIs, die Funktionen erläutert, die auf dieser Seite usw. in Entity Framework 5 eingeführt wurden. Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.

Die Seite und dieses Video zeigt, wie ein Modell in mehreren Diagrammen, die mit dem Entity Framework Designer (EF-Designer) zu unterteilen. Möglicherweise möchten dieses Feature zu verwenden, wenn das Modell ist zu groß zum Anzeigen oder bearbeiten.

In früheren Versionen von EF Designer können Sie nur ein Diagramm pro die EDMX-Datei verwenden. Ab Visual Studio 2012, können die EF-Designer Sie die EDMX-Datei in mehreren Diagrammen aufgeteilt.

## <a name="watch-the-video"></a>Video ansehen
Dieses Video zeigt, wie ein Modell in mehreren Diagrammen, die mit dem Entity Framework Designer (EF-Designer) geteilt. Möglicherweise möchten dieses Feature zu verwenden, wenn das Modell ist zu groß zum Anzeigen oder bearbeiten.

**Präsentiert von**: Julia Kornich

**Video**: [WMV](http://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.wmv) | [MP4](http://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-mp4video-multiplediagrams.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/C/2/5C2B52AB-5532-426F-B078-1E253341B5FA/HDI-ITPro-MSDN-winvideo-multiplediagrams.zip)

## <a name="ef-designer-overview"></a>Übersicht über EF-Designer

Wenn Sie ein Modell mit dem EF Designer Assistenten für Entity Data Model erstellen, wird eine EDMX-Datei erstellt und der Projektmappe hinzugefügt. Diese Datei definiert die Form von Entitäten und wie sie mit der Datenbank zugeordnet.

Die EF-Designer besteht aus den folgenden Komponenten:

-   Eine visuelle Entwurfsoberfläche zum Bearbeiten des Modells. Entitäten und Zuordnungen können erstellt, geändert oder gelöscht werden.
-   Ein **Modellbrowser** Fenster, das Strukturansichten des Modells bereitstellt.  Die Entitäten und ihre Zuordnungen befinden sich unter dem *\[ModelName\]* Ordner. Die Datenbanktabellen und-Einschränkungen befinden sich unter dem  *\[ModelName\]*. Store-Ordner.
-   Ein **Mappingdetails** Fenster zum Anzeigen und bearbeiten Zuordnungen. Entitätstypen oder Zuordnungen können Datenbanktabellen, Spalten und gespeicherten Prozeduren zugeordnet werden. 

Das visuelle Design Surface Fenster wird automatisch geöffnet, nach Abschluss des Assistenten für Entity Data Model. Wenn Sie den Browser nicht sichtbar ist, mit der rechten Maustaste die Hauptentwurfsoberfläche und wählen **Modellbrowser**.

Der folgende Screenshot zeigt eine EDMX-Datei, die in der EF-Designer geöffnet. Der Screenshot zeigt die visuelle Entwurfsoberfläche (auf der linken Seite) und die **Modellbrowser** Fenster (rechts).

![EFDesigner2](~/ef6/media/efdesigner2.png)

Klicken Sie auf STRG + Z, um einen Vorgang im EF Designer rückgängig zu machen.

## <a name="working-with-diagrams"></a>Verwenden von Diagrammen

Die EF-Designer erstellt standardmäßig ein Diagramm Diagram1 aufgerufen. Wenn Sie ein Diagramm mit einer großen Anzahl von Entitäten und Zuordnungen haben, werden Sie am häufigsten wie diese logisch aufteilen möchten. Ab Visual Studio 2012, können Sie das konzeptionelle Modell in mehreren Diagrammen anzeigen.   

Wenn Sie neue Diagramme hinzufügen, werden sie unter dem Ordner "Diagramme" im Modellbrowser-Fenster angezeigt. So benennen Sie ein Diagramm um: Wählen Sie das Diagramm in das Fenster "Modellbrowser", klicken Sie einmal auf den Namen und geben Sie den neuen Namen.  Sie können auch mit der rechten Maustaste des Diagrammnamen ein, und wählen Sie **umbenennen**.

Der Diagrammname wird neben dem Namen der EDMX-Datei, in Visual Studio-Editor angezeigt. Z. B. Model1.edmx\[Diagram1\].

![DiagramName](~/ef6/media/diagramname.png)

Der Inhalt für Diagramme (Form und Farbe der Entitäten und Zuordnungen) befindet sich in der. edmx.diagram-Datei. Um diese Datei anzuzeigen, wählen Sie die Projektmappen-Explorer aus, und erweitern Sie die EDMX-Datei. 

![DiagramFiles](~/ef6/media/diagramfiles.png)

Sie sollten nicht bearbeiten, die. edmx.diagram Datei manuell, den Inhalt dieser Datei möglicherweise von dem EF Designer überschrieben.
 
## <a name="splitting-entities-and-associations-into-a-new-diagram"></a>Aufteilen von Entitäten und Zuordnungen in ein neues Diagramm

Sie können Entitäten, auf das vorhandene Diagramm (halten Sie die UMSCHALTTASTE mehrere Entitäten auswählen) auswählen. Klicken Sie auf der rechten Maustaste, und wählen Sie **neuen Diagramm verschieben**. Das neue Diagramm erstellt, und die ausgewählten Entitäten und ihre Zuordnungen werden in das Diagramm verschoben.

Alternativ können Sie mit der rechten Maustaste in des Ordners "Diagramme" im Modellbrowser und wählen Sie **neues Diagramm hinzufügen.** Sie können dann Drag & drop Entitäten unter dem Ordner Entitätstypen im Modell-Browser auf die Entwurfsoberfläche.

Sie können auch Ausschneiden oder kopieren Entitäten (mit STRG + X oder STRG + C-Schlüssel) aus einem Diagramm und fügen Sie (mit STRG + V-Schlüssel), auf dem anderen. Wenn das Diagramm, das Sie eine Entität bereits beim Einfügen in, eine Entität mit demselben Namen enthält, wird eine neue Entität erstellt und dem Modell hinzugefügt werden.  Zum Beispiel: Diagram2 enthält die Entität "Department". Anschließend fügen Sie eine andere Abteilung auf Diagram2. Die Entität Department1 erstellt und dem konzeptionellen Modell hinzugefügt.   

Um verknüpfte Entitäten in einem Diagramm einzuschließen, Rick auf die Entität und wählen Sie **enthalten verwandte**. Dies veranlasst eine Kopie der verknüpften Entitäten und Zuordnungen im angegebenen Diagramm.

## <a name="changing-the-color-of-entities"></a>Ändern der Farbe von Entitäten

Zusätzlich zu einem Modell in mehreren Diagrammen aufteilen, können Sie auch die Farben der Entitäten ändern.

Um die Farbe zu ändern, wählen Sie eine Entität (oder mehrere Entitäten) auf der Entwurfsoberfläche aus. Klicken Sie dann, klicken Sie auf der rechten Maustaste, und wählen Sie **Eigenschaften**. Wählen Sie im Fenster Eigenschaften die **Füllfarbe** Eigenschaft. Geben Sie die Farbe, die entweder ein gültiger Farbname (z. B. Rot) oder einen gültigen RGB (z. B. 255, 128, 128). 

![Farbe](~/ef6/media/color.png)

## <a name="summary"></a>Zusammenfassung

In diesem Thema erläutert, wie ein Modell in mehreren Diagrammen geteilt und wie Sie eine andere Farbe für eine Entität mit dem Entity Framework-Designer angeben. 
