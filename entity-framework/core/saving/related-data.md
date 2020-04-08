---
title: 'Speichern verwandter Daten: EF Core'
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
uid: core/saving/related-data
ms.openlocfilehash: 86d32b6172ee21c12a15e9ed4bb0142afc99c8bd
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413612"
---
# <a name="saving-related-data"></a>Speichern relevanter Daten

Zusätzlich zu isolierten Entitäten können Sie auch die Beziehungen verwenden, die in Ihrem Modell definiert sind.

> [!TIP]  
> Das in diesem Artikel verwendete [Beispiel](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/RelatedData/) finden Sie auf GitHub.

## <a name="adding-a-graph-of-new-entities"></a>Hinzufügen eines Diagramms neuer Entitäten

Wenn Sie mehrere neue verknüpfte Entitäten erstellen, und Sie eine dem Kontext hinzufügen, werden die anderen ebenfalls hinzugefügt.

Im folgenden Beispiel werden der Blog und drei relevante Beiträge in die Datenbank eingefügt. Die Beiträge werden gefunden und hinzugefügt, da sie über die Navigationseigenschaft `Blog.Posts` erreichbar sind.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> Verwenden Sie die Eigenschaft EntityEntry.State, um den Zustand einer einzelnen Entität festzulegen. Beispiel: `context.Entry(blog).State = EntityState.Modified`.

## <a name="adding-a-related-entity"></a>Hinzufügen einer verknüpften Entität

Wenn Sie auf eine neue Entität aus der Navigationseigenschaft einer Entität verweisen, die bereits über den Kontext nachverfolgt wird, wird die Entität erkannt und in die Datenbank eingefügt.

Im folgenden Beispiel wird die Entität `post` eingefügt, da sie der `Posts`-Eigenschaft der `blog`-Entität hinzugefügt wird, die aus der Datenbank abgerufen wurde.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a>Ändern von Beziehungen

Wenn Sie die Navigationseigenschaft einer Entität ändern, werden die entsprechenden Änderungen an der Fremdschlüsselspalte in der Datenbank vorgenommen.

Im folgenden Beispiel wird die Entität `post` aktualisiert, sodass sie zur neuen `blog`-Entität gehört, da ihre `Blog`-Navigationseigenschaft so festgelegt ist, dass sie auf `blog` zeigt. Beachten Sie, dass `blog` ebenfalls in die Datenbank eingefügt wird, da es eine neue Entität ist, die von der Navigationseigenschaft einer Entität referenziert wird, die bereits über den Kontext (`post`) nachverfolgt wird.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a>Entfernen von Beziehungen

Sie können eine Beziehung entfernen, indem Sie eine Verweisnavigation auf `null` festlegen oder die verknüpfte Entität aus einer Auflistungsnavigation entfernen.

Das Entfernen einer Beziehung kann Nebeneffekte gemäß dem in der Beziehung konfigurierten Verhalten des kaskadierenden Deletes für die abhängige Entität haben.

Das Verhalten eines kaskadierenden Deletes wird standardmäßig für erforderliche Beziehungen konfiguriert, und die untergeordnete bzw. abhängige Entität wird aus der Datenbank gelöscht. Das kaskadierende Delete wird für optionale Beziehungen nicht standardmäßig konfiguriert, aber die Fremdschlüsseleigenschaft wird auf NULL festgelegt.

Informationen darüber, wie die Erforderlichkeit von Beziehungen konfiguriert wird, finden Sie unter [Required and Optional Relationships (Erforderliche und optionale Beziehungen)](../modeling/relationships.md#required-and-optional-relationships).

Ausführliche Informationen zur Funktionsweise des kaskadierenden Deletes, wie es explizit konfiguriert werden kann und wie es gemäß der Konvention ausgewählt wird, finden Sie unter [Cascade Delete (Kaskadierendes Delete)](cascade-delete.md).

Im folgenden Beispiel wird das kaskadierende Delete der Beziehung zwischen `Blog` und `Post` konfiguriert, sodass die Entität `post` aus der Datenbank gelöscht wird.

[!code-csharp[Main](../../../samples/core/Saving/RelatedData/Sample.cs#RemovingRelationships)]
