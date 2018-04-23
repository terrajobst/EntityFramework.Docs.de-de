---
title: Speichern im Zusammenhang, Daten per Push – EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 07b6680f-ffcf-412c-9857-f997486b386c
ms.technology: entity-framework-core
uid: core/saving/related-data
ms.openlocfilehash: b0ed25267c85e82db18d8a89693b6040db7e4b34
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
---
# <a name="saving-related-data"></a>Speichern verwandter Daten

Sie können auch vornehmen, zusätzlich zu den isolierten Entitäten verwenden, der im Modell definierten Beziehungen.

> [!TIP]  
> Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/RelatedData/) finden Sie auf GitHub.

## <a name="adding-a-graph-of-new-entities"></a>Hinzufügen von neuen Entitäten in einem Diagramm

Wenn Sie mehrere neue verknüpfte Entitäten erstellen, verursacht Hinzufügen von mindestens einem von ihnen an den Kontext zu hinzugefügt werden andere.

Im folgenden Beispiel werden im Blog und drei verwandten Beiträge alle in der Datenbank eingefügt. Die Beiträge gefunden und hinzugefügt werden, da es über erreichbar sind, werden die `Blog.Posts` Navigationseigenschaft.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingGraphOfEntities)]

> [!TIP]  
> Verwenden Sie die EntityEntry.State-Eigenschaft zum Festlegen des Status von nur einer einzelnen Entität. Beispielsweise `context.Entry(blog).State = EntityState.Modified`.

## <a name="adding-a-related-entity"></a>Hinzufügen einer verknüpften Entität

Wenn Sie eine neue Entität aus der Navigationseigenschaft einer Entität, die bereits vom Kontext nachverfolgt wird verweisen, wird die Entität ermittelt und in die Datenbank eingefügt.

Im folgenden Beispiel die `post` Entität eingefügt werden, weil es hinzugefügt wird die `Posts` Eigenschaft von der `blog` Entität, die sich aus der Datenbank abgerufen wurde.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#AddingRelatedEntity)]

## <a name="changing-relationships"></a>Ändern von Beziehungen

Wenn Sie die Navigationseigenschaft einer Entität ändern, wird die entsprechende Fremdschlüsselspalte in der Datenbank geändert werden.

Im folgenden Beispiel die `post` Entität wird aktualisiert, um mit dem neuen gehören `blog` Entität da seine `Blog` Navigationseigenschaft auf festgelegt ist `blog`. Beachten Sie, dass `blog` wird auch in die Datenbank eingefügt werden, da es sich um eine neue Entität handelt, die von der Navigationseigenschaft einer Entität verwiesen wird, die bereits vom Kontext nachverfolgt wird (`post`).

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#ChangingRelationships)]

## <a name="removing-relationships"></a>Entfernen von Beziehungen

Sie können eine Beziehung entfernen, indem Sie eine Referenz Navigation zu `null`, oder entfernen Sie die verknüpfte Entität aus einer Auflistung Navigation.

Entfernen einer Beziehung kann Nebeneffekte haben, für die abhängige Entität, gemäß der Cascade gelöscht Verhalten in der Beziehung konfiguriert werden.

Wird standardmäßig für die erforderlichen Beziehungen eine Cascade Delete-Verhalten konfiguriert ist, und die untergeordneten/abhängige Entität aus der Datenbank gelöscht werden. Für optionale Beziehungen kaskadierendes Delete ist nicht standardmäßig konfiguriert werden, aber die foreign Key-Eigenschaft festgelegt werden auf Null.

Finden Sie unter [erforderlichen und optionalen Beziehungen](../modeling/relationships.md#required-and-optional-relationships) zu erfahren, wie die Requiredness von Beziehungen konfiguriert werden kann.

Finden Sie unter [Löschweitergabe](cascade-delete.md) für Weitere Informationen zu den wie Löschweitergabe Verhalten arbeiten, wie diese explizit konfiguriert werden können und wie sie gemäß der Konvention ausgewählt werden.

Im folgenden Beispiel wird eine kaskadierte Löschung auf der Beziehung zwischen konfiguriert `Blog` und `Post`, sodass die `post` Entität aus der Datenbank gelöscht wird.

[!code-csharp[Main](../../../samples/core/Saving/Saving/RelatedData/Sample.cs#RemovingRelationships)]
