---
title: Beziehungen – EF-Designer - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 402fe960-754b-470f-976b-e5de3e9986b5
ms.openlocfilehash: d429c39dafbf183caabdc85748c188deb8dd6f66
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490652"
---
# <a name="relationships---ef-designer"></a>Beziehungen – EF-Designer
> [!NOTE]
> Diese Seite enthält Informationen zum Einrichten der Beziehungen in Ihrem Modell, mit dem EF Designer. Allgemeine Informationen zu Beziehungen in Entity Framework und das Zugreifen auf und Bearbeiten von Daten mithilfe von Beziehungen, finden Sie unter [Beziehungen und Navigationseigenschaften](~/ef6/fundamentals/relationships.md).

Zuordnungen definieren Beziehungen zwischen Entitätstypen in einem Modell. In diesem Thema veranschaulicht, wie Zuordnungen mit dem Entity Framework Designer (EF-Designer) zuordnen. Die folgende Abbildung zeigt die wichtigsten Windows, die bei der Arbeit mit dem EF Designer verwendet werden.

![EF-Designer](~/ef6/media/efdesigner.png)

> [!NOTE]
> Wenn Sie das konzeptionelle Modell erstellen, können Warnungen zu nicht zugeordneten Entitäten und Zuordnungen in der Fehlerliste angezeigt. Sie können diese Warnungen ignorieren, da nach der Auswahl, um die Datenbank aus dem Modell zu generieren, die Fehler verschwinden werden.

## <a name="associations-overview"></a>Übersicht über die Zuordnungen

Wenn Sie Ihr Modell mit dem EF Designer entworfen haben, stellt eine EDMX-Datei das Modell dar. In der EDMX-Datei ein **Zuordnung** -Element definiert eine Beziehung zwischen zwei Entitätstypen. Eine Zuordnung muss die Entitätstypen, die in der Beziehung enthalten sind, und die mögliche Anzahl von Entitätstypen an den Enden der Beziehung angeben, die auch als Multiplizität bezeichnet wird. Die Multiplizität eines zuordnungsendes kann einen Wert von eins (1), NULL oder eins (0.. 1) oder viele haben (\*). Diese Informationen sind in zwei untergeordneten angegeben **End** Elemente.

Zur Laufzeit können auf Entitätstypinstanzen an einem Ende einer Zuordnung über Navigationseigenschaften oder Fremdschlüssel zugegriffen werden, (Wenn Sie auswählen, Fremdschlüssel in Ihren Entitäten verfügbar zu machen). Wurden Fremdschlüssel verfügbar gemacht, die die Beziehung zwischen den Entitäten Verwaltung erfolgt über eine **ReferentialConstraint** Element (ein untergeordnetes Element von der **Zuordnung** Element). Es wird empfohlen, dass Sie immer für die Beziehung zwischen Fremdschlüssel in Ihren Entitäten verfügbar machen.

> [!NOTE]
> In der m: n (\*:\*) die Entitäten können keine Fremdschlüssel hinzugefügt. In einem \*:\* Beziehung, die Informationen zur Zuordnung mit der ein unabhängiges Objekt verwaltet wird.

Informationen zu CSDL-Elemente (**ReferentialConstraint**, **Zuordnung**usw.) finden Sie unter den [CSDL-Spezifikation](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).

## <a name="create-and-delete-associations"></a>Erstellen und Löschen von Zuordnungen

Erstellen eine Zuordnung mit den EF Designer-Updates des Inhalts der EDMX-Datei. Nach dem Erstellen der Zuordnung müssen Sie die Zuordnungen für die Zuordnung (weiter unten in diesem Thema beschrieben) erstellen.

> [!NOTE]
> In diesem Abschnitt wird davon ausgegangen, dass Sie die Entitäten, die Sie eine Zuordnung zwischen dem Modell erstellen möchten, bereits hinzugefügt.

### <a name="to-create-an-association"></a>So erstellen Sie eine Zuordnung

1.  Mit der rechten Maustaste in eines leeren Bereichs der Entwurfsoberfläche, zeigen Sie auf **Add New**, und wählen Sie **Zuordnung...** .
2.  Geben Sie die Einstellungen für die Zuordnung in der **Zuordnung hinzufügen** Dialogfeld.

    ![Zuordnung hinzufügen](~/ef6/media/addassociation.png)

    > [!NOTE]
    > Sie können auch keine Navigationseigenschaften oder Fremdschlüsseleigenschaften für die Entitäten an den Enden der Zuordnung löschen hinzufügen der ** Navigationseigenschaft ** und ** Fremdschlüsseleigenschaften zum Hinzufügen der &lt;Entitätstypname&gt; Entität ** Kontrollkästchen. Wird nur eine Navigationseigenschaft hinzugefügt, kann die Zuordnung nur in einer Richtung traversiert werden. Falls Sie keine Navigationseigenschaften hinzufügen, müssen Sie Fremdschlüsseleigenschaften hinzufügen, um auf Entitäten an den Enden der Zuordnung zuzugreifen.
    
3.  Klicken Sie auf **OK**.

### <a name="to-delete-an-association"></a>So löschen Sie eine Zuordnung

So löschen Sie eine Zuordnung führen Sie einen der folgenden:

-   Mit der rechten Maustaste in der Zuordnung auf die Entwurfsoberfläche, und wählen die EF-Designer **löschen**.

- OR-

-   Wählen Sie eine oder mehrere Zuordnungen aus, und drücken Sie die ENTF-Taste.

## <a name="include-foreign-key-properties-in-your-entities-referential-constraints"></a>Einschließen von Fremdschlüsseleigenschaften in Ihren Entitäten (referenzielle Einschränkungen)

Es wird empfohlen, dass Sie immer für die Beziehung zwischen Fremdschlüssel in Ihren Entitäten verfügbar machen. Entitätsframework verwendet eine referenzielle Einschränkung um zu erkennen, dass eine Eigenschaft als Fremdschlüssel für eine Beziehung fungiert.

Wenn Sie aktiviert das ***Fremdschlüsseleigenschaften zum Hinzufügen der &lt;Entitätstypname&gt; Entität*** Kontrollkästchen beim Erstellen einer Beziehung, wurde diese referenzielle Einschränkung für die Sie hinzugefügt.

Bei Verwendung der EF-Designer zum Hinzufügen oder Bearbeiten einer referenziellen Einschränkung, die EF-Designer fügt hinzu oder ändert einen **ReferentialConstraint** Element im CSDL-Inhalt der EDMX-Datei.

-   Doppelklicken Sie auf die Zuordnung, die Sie bearbeiten möchten.
    Die **referenzielle Einschränkung** Dialogfeld wird angezeigt.
-   Von der **Principal** Dropdown-Liste, wählen Sie die prinzipalentität in der referenziellen Einschränkung.
    Schlüsseleigenschaften der Entität werden hinzugefügt, um die **Dienstprinzipalschlüssel** Liste im Dialogfeld.
-   Von der **abhängige** Dropdown-Liste, wählen Sie die abhängige Entität in der referenziellen Einschränkung.
-   Wählen Sie für jeden prinzipalschlüssel, die von einem abhängigen Schlüssel einen entsprechenden abhängigen Schlüssel aus den Dropdown-Listen in die **abhängiger Schlüssel** Spalte.

    ![REF-Einschränkung](~/ef6/media/refconstraint.png)

-   Klicken Sie auf **OK**.

## <a name="create-and-edit-association-mappings"></a>Erstellen und Bearbeiten von Zuordnungsmappings

Sie können angeben, wie die Datenbank in eine Zuordnung zugeordnet wird die **Mappingdetails** Fenster des EF-Designers.

> [!NOTE]
> Sie können nur Informationen für die Zuordnungen zuordnen, die keine referenzielle Einschränkung angegeben haben. Wenn eine referenzielle Einschränkung angegeben wird dann eine Fremdschlüsseleigenschaft ist in der Entität enthalten, und können Sie die Mapping-Details für die Entität aus, um zu steuern, welche Spalte der Fremdschlüssel zugeordnet.

### <a name="create-an-association-mapping"></a>Erstellen Sie ein Zuordnungsmapping

-   Mit der rechten Maustaste einer Zuordnung in die Entwurfsoberfläche, und wählen **Tabellenzuordnung**.
    Dies das Zuordnungsmapping in die **Mappingdetails** Fenster.
-   Klicken Sie auf **Hinzufügen einer Tabelle oder Sicht**.
    Eine Dropdownliste wird angezeigt, die die Tabellen im Speichermodell enthält.
-   Wählen Sie die Tabelle aus, der die Zuordnung zugeordnet wird.
    Die **Mappingdetails** Fenster zeigt beide Enden der Zuordnung und die Schlüsseleigenschaften des Entitätstyps an jedem **End**.
-   Klicken Sie für jede Schlüsseleigenschaft auf das **Spalte** ein, und wählen Sie die Spalte, der die Eigenschaft zugeordnet wird.

    ![Zuordnungsdetails 4](~/ef6/media/mappingdetails4.png)

### <a name="edit-an-association-mapping"></a>Bearbeiten Sie ein Zuordnungsmapping

-   Mit der rechten Maustaste einer Zuordnung in die Entwurfsoberfläche, und wählen **Tabellenzuordnung**.
    Dies das Zuordnungsmapping in die **Mappingdetails** Fenster.
-   Klicken Sie auf **ordnet &lt;Tabellenname&gt;**.
    Eine Dropdownliste wird angezeigt, die die Tabellen im Speichermodell enthält.
-   Wählen Sie die Tabelle aus, der die Zuordnung zugeordnet wird.
    Die **Mappingdetails** Fenster werden beide Enden der Zuordnung und die Schlüsseleigenschaften des Entitätstyps an jedem Ende angezeigt.
-   Klicken Sie für jede Schlüsseleigenschaft auf das **Spalte** ein, und wählen Sie die Spalte, der die Eigenschaft zugeordnet wird.

## <a name="edit-and-delete-navigation-properties"></a>Bearbeiten und Löschen von Navigationseigenschaften

Navigationseigenschaften sind Verknüpfungseigenschaften, mit denen die Entitäten an den Enden einer Zuordnung in einem Modell zu suchen. Navigationseigenschaften können beim Erstellen einer Zuordnung zwischen zwei Entitätstypen erstellt werden.

#### <a name="to-edit-navigation-properties"></a>Bearbeiten von Navigationseigenschaften

-   Wählen Sie eine Navigationseigenschaft auf der EF Designer-Oberfläche.
    Informationen über die Navigationseigenschaft wird angezeigt, in der Visual Studio **Eigenschaften** Fenster.
-   Ändern der Einstellungen der Eigenschaften in der **Eigenschaften** Fenster.

#### <a name="to-delete-navigation-properties"></a>Löschen von Navigationseigenschaften

-   Wenn Fremdschlüssel nicht für Entitätstypen im konzeptionellen Modell verfügbar gemacht werden, kann das Löschen einer Navigationseigenschaft dazu führen, dass die entsprechende Zuordnung nur in einer Richtung oder gar nicht traversiert werden kann.
-   Mit der rechten Maustaste einer Navigationseigenschaft auf der Entwurfsoberfläche, und wählen den EF Designer **löschen**.
