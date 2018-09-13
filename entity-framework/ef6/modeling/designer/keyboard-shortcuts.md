---
title: Entity Framework Designer-Tastenkombinationen - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 3c76cdd5-17c5-4c54-a6a5-cf21b974636b
ms.openlocfilehash: c75eafcca0863faa1ad64202e98b61832827377c
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490245"
---
# <a name="entity-framework-designer-keyboard-shortcuts"></a>Entity Framework Designer-Tastenkombinationen
Diese Seite enthält eine Liste der Tastatur zahlreiche programmierbare, die in verschiedenen Bildschirme in den die Entity Framework-Tools für Visual Studio verfügbar sind.

## <a name="adonet-entity-data-model-wizard"></a>ADO.NET Entity Data Model-Assistenten

### <a name="step-one-choose-model-contents"></a>Schritt 1: Auswählen von Modellinhalten

![Assistent 1](~/ef6/media/wizardone.png)

| Verknüpfung  | Aktion                                                     | Hinweise                                               |
|:----------|:-----------------------------------------------------------|:----------------------------------------------------|
| **ALT + n** | Zum nächsten Bildschirm zu verschieben                                        | Für die gesamte Auswahl der Modellinhalt nicht verfügbar. |
| **ALT + f** | Beenden Sie Assistenten                                              | Für die gesamte Auswahl der Modellinhalt nicht verfügbar. |
| **ALT + w** | Schaltet den Fokus "sollte den wie das Modell enthalten?" im Bereich. |                                                     |

### <a name="step-two-choose-your-connection"></a>Schritt 2: Wählen Sie die Verbindung

![Assistent 2](~/ef6/media/wizardtwo.png)

| Verknüpfung  | Aktion                                                     | Hinweise                                                   |
|:----------|:-----------------------------------------------------------|:--------------------------------------------------------|
| **ALT + n** | Zum nächsten Bildschirm zu verschieben                                        |                                                         |
| **ALT + p** | Zum vorherigen Bildschirm verschieben                                    |                                                         |
| **ALT + w** | Schaltet den Fokus "sollte den wie das Modell enthalten?" im Bereich. |                                                         |
| **Alt + c** | Öffnen Sie das Fenster "Connection Properties"                    | Ermöglicht die Definition einer neuen Datenbank-Verbindung. |
| **ALT + e** | Vertrauliche Daten aus der Verbindungszeichenfolge ausschließen.          |                                                         |
| **ALT + i** | Einbeziehung vertraulicher Daten in der Verbindungszeichenfolge            |                                                         |
| **ALT + s** | Schalten Sie die Option "Speichern von Verbindungseinstellungen in" App.config "" |                                                         |

### <a name="step-three-choose-your-version"></a>Schritt 3: Wählen Sie Ihre Version

![Assistenten drei](~/ef6/media/wizardthree.png)

| Verknüpfung  | Aktion                                             | Hinweise                                                                                 |
|:----------|:---------------------------------------------------|:--------------------------------------------------------------------------------------|
| **ALT + n** | Zum nächsten Bildschirm zu verschieben                                |                                                                                       |
| **ALT + p** | Zum vorherigen Bildschirm verschieben                            |                                                                                       |
| **ALT + w** | Wechseln Sie den Fokus zur Auswahl der Entity Framework-version | Ermöglicht die Angabe einer anderen Version von Entity Framework für die Verwendung im Projekt. |

### <a name="step-four-choose-your-database-objects-and-settings"></a>Schritt 4: Wählen Sie Ihre Datenbankobjekte und Einstellungen

![Assistenten vier](~/ef6/media/wizardfour.png)

| Verknüpfung  | Aktion                                                                                    | Hinweise                                                               |
|:----------|:------------------------------------------------------------------------------------------|:--------------------------------------------------------------------|
| **ALT + f** | Beenden Sie Assistenten                                                                             |                                                                     |
| **ALT + p** | Zum vorherigen Bildschirm verschieben                                                                   |                                                                     |
| **ALT + w** | Schaltet den Fokus Bereichs "Auswahl" der Datenbank-Objekt                                            | Angeben von Datenbankobjekten, um reverse Engineering ermöglicht.    |
| **ALT + s** | Umschalten der "in den Singular oder Plural setzen generierte Objektnamen" Option                       |                                                                     |
| **ALT + k** | Schalten Sie die Option "Include foreign Key-Spalten im Modell."                              | Für die gesamte Auswahl der Modellinhalt nicht verfügbar.                 |
| **ALT + i** | Schalten Sie die Option "Import ausgewählte gespeicherte Prozeduren und Funktionen in Entity Model" | Für die gesamte Auswahl der Modellinhalt nicht verfügbar.                 |
| **ALT + m** | Ändert den Fokus auf das Textfeld "Model-Namespace"                                        | Für die gesamte Auswahl der Modellinhalt nicht verfügbar.                 |
| **LEERTASTE** | Auswahl für Element ein-/ausblenden                                                               | Wenn das Element untergeordnete Elemente verfügt, werden alle untergeordneten Elemente sowie ein-und ausgeschaltet werden |
| **Links**  | Die untergeordnete Struktur reduzieren                                                                       |                                                                     |
| **Rechts** | Erweitern Sie die untergeordnete Struktur                                                                         |                                                                     |
| **Nach-oben**    | Navigieren Sie zum vorherigen Element in Struktur                                                      |                                                                     |
| **Nach unten**  | Navigieren Sie zum nächsten Element in Struktur                                                          |                                                                     |

## <a name="ef-designer-surface"></a>EF-Designer-Oberfläche

![Designeroberfläche](~/ef6/media/designersurface.png)

| Verknüpfung                                                                                | Aktion                      | Hinweise                                                                                                                                                                                                                               |
|:----------------------------------------------------------------------------------------|:----------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Geben Sie Speicherplatz /**                                                                         | Auswahl ein-/ausblenden            | Schaltet die Auswahl für das Objekt den Fokus besitzt.                                                                                                                                                                                         |
| **ESC**                                                                                 | Auswahl aufheben            | Bricht die aktuelle Auswahl ab.                                                                                                                                                                                                      |
| **STRG + A**                                                                            | Alles auswählen                  | Wählt alle Formen auf der Entwurfsoberfläche angezeigt.                                                                                                                                                                                       |
| **Pfeil nach oben**                                                                            | Nach oben                     | Die ausgewählten Entität in einer Rasterseite Inkrement verschoben werden. <br/> Wenn Sie sich in eine Liste verschiebt zum vorherigen gleichgeordneten.                                                                                                                            |
| **Pfeil nach unten**                                                                          | Nach unten                   | Die ausgewählten Entität nach unten Schrittweite von einer Rasterseite verschoben. <br/> Wenn Sie sich in eine Liste verschiebt zum nächsten gleichgeordneten.                                                                                                                              |
| **NACH-LINKS-TASTE**                                                                          | Nach links                   | Die ausgewählten Entität linken einer Rasterseite Inkrement verschoben werden. <br/> Wenn Sie sich in eine Liste verschiebt zum vorherigen gleichgeordneten.                                                                                                                          |
| **NACH-RECHTS-TASTE**                                                                         | Nach rechts verschieben                  | Die ausgewählten Entität nach rechts ein Raster Inkrement verschoben werden. <br/> Wenn Sie sich in eine Liste verschiebt zum nächsten gleichgeordneten.                                                                                                                             |
| **UMSCHALT + nach-links**                                                                  | Größe Form nach links             | Reduziert die Breite der ausgewählten Entität von einer Rasterseite Inkrement an.                                                                                                                                                                     |
| **UMSCHALT + nach-rechts**                                                                 | Richtige Größe-Form            | Erhöht die Breite der ausgewählten Entität von einer Rasterseite Inkrement an.                                                                                                                                                                   |
| **Home**                                                                                | Erste Peer                  | Verschiebt den Fokus und Auswahl auf das erste Objekt auf der Entwurfsoberfläche auf der gleichen Ebene.                                                                                                                                         |
| **ENDE**                                                                                 | Letzte Peer                   | Verschiebt den Fokus und Auswahl, um das letzte Objekt auf der Entwurfsoberfläche auf der gleichen Ebene.                                                                                                                                          |
| **STRG + POS1**                                                                         | Erste Peer (Fokus)          | Identisch mit dem ersten Peer, aber anstatt Fokus und Auswahl der Fokus verschoben.                                                                                                                                                          |
| **STRG + Ende**                                                                          | Letzte Peer (Fokus)           | Identisch mit dem letzten per Peering verknüpfen, aber Sie müssen nicht mehr verschoben, Fokus und Auswahl den Fokus.                                                                                                                                                           |
| **TAB**                                                                                 | Nächsten Peer                   | Verschiebt den Fokus und Auswahl auf das nächste Objekt auf der Entwurfsoberfläche auf der gleichen Ebene.                                                                                                                                          |
| **UMSCHALT+TAB**                                                                           | Vorherige Peer               | Verschiebt den Fokus und Auswahl auf das vorherige Objekt auf der Entwurfsoberfläche auf der gleichen Ebene.                                                                                                                                      |
| **ALT-Taste + Strg + Tab**                                                                        | Nächsten Peer (Fokus)           | Identisch mit dem nächsten peer, aber anstatt Fokus und Auswahl den Fokus.                                                                                                                                                           |
| **ALT-Taste + Strg + Umschalt + Tab**                                                                  | Vorherige Peer (Fokus)       | Identisch mit der vorherigen Peer, aber anstatt Fokus und Auswahl der Fokus verschoben.                                                                                                                                                       |
| **&lt;**                                                                                | Ascend                      | Wechselt zum nächsten Objekt auf der Entwurfsoberfläche ein auf der Ebene in der Hierarchie höher. Wenn es keine Formen über dieser Form in der Hierarchie sind (d. h. das Objekt direkt auf der Entwurfsoberfläche platziert ist), das Diagramm ausgewählt ist. |
| **&gt;**                                                                                | Verzweigen                     | Wechselt zum nächsten enthaltene Objekt auf der Entwurfsoberfläche ein auf der Ebene unter diesem Thema in der Hierarchie. Wenn keine darin enthaltenen Objekte sind, ist dies keine Aktion aus.                                                                              |
| **STRG + &lt;**                                                                         | Ascend (Fokus)              | Identisch mit ascend Befehl, doch das Verschieben des Fokus ohne Auswahl.                                                                                                                                                                          |
| **STRG + &gt;**                                                                         | Verzweigen (Fokus)             | Identisch mit Befehl, doch das Verschieben des Fokus ohne Auswahl abgeleitet werden.                                                                                                                                                                         |
| **Umschalt + Ende**                                                                         | Führen Sie die in verbundenen         | Verschiebt von einer Entität in einer Entität, die diese Entität verbunden ist.                                                                                                                                                               |
| **ENTF**                                                                                 | Löschen                      | Löschen Sie ein Objekt oder den Connector aus dem Diagramm.                                                                                                                                                                                     |
| **Ins**                                                                                 | Insert                      | Fügt eine neue Eigenschaft zu einer Entität an, wenn entweder der Depot-Header "Skalareigenschaften" oder eine Eigenschaft selbst ausgewählt ist.                                                                                                           |
| **Bild-auf**                                                                               | Diagramm der führen Sie einen Bildlauf nach oben           | Führt einen Bildlauf durch die Entwurfsoberfläche, in Schritten von 75 % der die Höhe der aktuell sichtbaren Entwurfsoberfläche gleich.                                                                                                                    |
| **Bild-ab**                                                                             | Diagramm der führen Sie einen Bildlauf nach unten         | Führt einen Bildlauf die Entwurfsoberfläche nach unten ein.                                                                                                                                                                                                    |
| **Umschalt + Bild-ab**                                                                     | Führen Sie einen Bildlauf Diagramm rechts        | Führt einen Bildlauf die Entwurfsoberfläche nach rechts.                                                                                                                                                                                            |
| **Umschalt + Bild-auf**                                                                       | Führen Sie einen Bildlauf Diagramm links         | Führt einen Bildlauf die Entwurfsoberfläche auf der linken Seite.                                                                                                                                                                                             |
| **F2**                                                                                  | Geben Sie den Bearbeitungsmodus             | Standardmäßige Tastenkombination für einen Text-Steuerelement in den Bearbeitungsmodus wechselt.                                                                                                                                                               |
| **UMSCHALT + F10**                                                                         | Anzeigen des Kontextmenüs       | Standardmäßige Tastenkombination für das Kontextmenü eines ausgewählten Elements anzeigen.                                                                                                                                                          |
| **STRG + UMSCHALTTASTE + Maus Linksklick**  <br/> **STRG + UMSCHALT + Mausrad vorwärts**  | Semantischer Zoom In            | Der Bereich der Diagrammsicht anzuzeigen, unter dem Mauszeiger auf vergrößert.                                                                                                                                                                 |
| **STRG + UMSCHALTTASTE + Maus mit der rechten Maustaste** <br/> **STRG + UMSCHALT + Mausrad rückwärts** | Der semantische Zoom           | Verkleinert die aus dem Bereich der in der Diagrammansicht unter dem Mauszeiger auf. Es Zentriert das Diagramm erneut, wenn Sie sich zu weit für das aktuelle Diagramm Center verkleinern.                                                                          |
| **STRG + UMSCHALTTASTE + '+'** <br/> **STRG + Mausrad vorwärts**                        | Vergrößern                     | Vergrößert den Mittelpunkt der in der Diagrammansicht.                                                                                                                                                                                         |
| **STRG + UMSCHALTTASTE + '-'** <br/> **STRG + Mausrad rückwärts**                       | Verkleinern                    | Verkleinert die aus dem Bereich der Diagrammansicht auf den geklickt wurde. Es Zentriert das Diagramm erneut, wenn Sie sich zu weit für das aktuelle Diagramm Center verkleinern.                                                                                            |
| **STRG + UMSCHALTTASTE + zeichnen ein Rechteck mit der linken Maustaste unten**                  | Zoombereich                   | Vergrößert die Bewältigung des Bereichs, den Sie ausgewählt haben. Wenn Sie die STRG + UMSCHALT gedrückt halten, sehen Sie sich, dass der Cursor in einem Lupensymbol dargestellt Änderungen, Sie definieren den Bereich zu vergrößern können.                        |
| **Kontextmenü-Taste + bin "**                                                              | Öffnen Sie die Mappingdetails (Fenster) | Öffnet das Fenster "Mappingdetails", um die Zuordnungen für die ausgewählte Entität bearbeiten                                                                                                                                                               |

## <a name="mapping-details-window"></a>Mappingdetails (Fenster)

![Zuordnung werden Verknüpfungen](~/ef6/media/mappingdetailsshortcuts.png)

| Verknüpfung                  | Aktion         | Hinweise                                                                                                                                 |
|:--------------------------|:---------------|:--------------------------------------------------------------------------------------------------------------------------------------|
| **TAB**                   | Wechseln Sie den Kontext | Wechselt zwischen den Bereich des Hauptfensters und in der Symbolleiste auf der linken Seite                                                                     |
| **Pfeiltasten**            | Navigation     | Verschieben Sie nach oben oder unten Zeilen oder rechts und Links, über die Spalten im Bereich Hauptfensters. Verschieben Sie zwischen den Schaltflächen auf der Symbolleiste auf der linken Seite. |
| **EINGABETASTE** <br/> **LEERTASTE** | Auswählen         | Eine Schaltfläche auswählt, auf der Symbolleiste auf der linken Seite.                                                                                          |
| **ALT + nach-unten-Taste**      | Öffnen der Liste      | Löschen Sie eine Liste, wenn eine Zelle ausgewählt wird, die über eine Dropdownliste verfügt.                                                                     |
| **EINGABETASTE**                 | Liste auswählen    | Wählt ein Element in einer Dropdownliste.                                                                                               |
| **ESC**                   | Liste schließen     | Schließt eine Dropdownliste an.                                                                                                              |

## <a name="visual-studio-navigation"></a>Visual Studio-Navigation

Entitätsframework stellt außerdem eine Reihe von Aktionen, die benutzerdefinierte Tastenkombinationen zugeordnet haben kann (keine Verknüpfungen werden standardmäßig zugeordnet). Um diese benutzerdefinierte Tastenkombinationen erstellen möchten, klicken Sie auf das Menü "Extras", und klicken Sie auf Optionen.  Wählen Sie unter Umgebung Tastatur ein.  Weisen Sie den Bildlauf nach unten in der Liste in der Mitte, bis Sie können den gewünschten Befehl auswählen, geben Sie die Tastenkombination in das Textfeld "Tastenkombination drücken", und klicken Sie auf. Die möglichen Tastenkombinationen sind wie folgt:

| Verknüpfung                                                                                       |
|:-----------------------------------------------------------------------------------------------|
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Add.ComplexProperty.ComplexTypes**        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddCodeGenerationItem**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddFunctionImport**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.AddEnumType**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Association**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ComplexProperty**                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ComplexType**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Entity**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.FunctionImport**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.Inheritance**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.NavigationProperty**               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNew.ScalarProperty**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddNewDiagram**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.AddtoDiagram**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Close**                                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Collapse**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ConverttoEnum**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.CollapseAll**                     |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.ExpandAll**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.ExportasImage**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Diagram.LayoutDiagram**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Edit**                                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.EntityKey**                               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Expand**                                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.FunctionImportMapping**                   |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.GenerateDatabasefromModel**               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.GoToDefinition**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Grid.ShowGrid**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Grid.SnaptoGrid**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.IncludeRelated**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MappingDetails**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ModelBrowser**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveDiagramstoSeparateFile**              |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Down**                     |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Down5**                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.ToBottom**                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.ToTop**                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Up**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MoveProperties.Up5**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.MovetonewDiagram**                        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Open**                                    |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Refactor.MovetoNewComplexType**           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Refactor.Rename**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.RemovefromDiagram**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Rename**                                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ScalarPropertyFormat.DisplayName**        |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ScalarPropertyFormat.DisplayNameandType** |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.BaseType**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Entity**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Property**                         |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Select.Subtype**                          |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.SelectAll**                               |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.SelectAssociation**                       |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ShowinDiagram**                           |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.ShowinModelBrowser**                      |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.StoredProcedureMapping**                  |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.TableMapping**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.UpdateModelfromDatabase**                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Validate**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.10**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.100**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.125**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.150**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.200**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.25**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.300**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.33**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.400**                                |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.50**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.66**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.75**                                 |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.Custom**                             |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomIn**                             |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomOut**                            |
| **OtherContextMenus.MicrosoftDataEntityDesignContext.Zoom.ZoomtoFit**                          |
| **View.EntityDataModelBrowser**                                                                |
| **View.EntityDataModelMappingDetails**                                                         |
