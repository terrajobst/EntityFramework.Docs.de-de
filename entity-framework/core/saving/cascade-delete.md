---
title: "Löschweitergabe - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
ms.technology: entity-framework-core
uid: core/saving/cascade-delete
ms.openlocfilehash: 1ab9d114e27aac0bec972df631a426c8ce87a518
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="cascade-delete"></a>Kaskadierte Löschung

Kaskadierte Löschung wird häufig in der Terminologie von Datenbanken verwendet, um ein Merkmal beschreiben, die das Löschen einer Zeile automatisch verknüpfte Zeilen gelöscht auslösen können. Ein eng verwandtes Konzept gehörig von EF Core Delete Verhalten ist das automatische Löschen von einer untergeordneten Entität wird die Beziehung zu einem übergeordneten Element unterbrochen wurde – dies geht im Allgemeinen als "verwaiste löschen" bezeichnet.

EF Core implementiert mehrere unterschiedliche Delete Verhaltensweisen und ermöglicht die Konfiguration der Delete-Verhalten von einzelnen Beziehungen. EF Core implementiert auch Konventionen, die hilfreich, löschen Sie das Standardverhalten für jede Beziehung auf Basis der [Requiredness der Beziehung] automatisch zu konfigurieren (../modeling/relationships.md#required-and-optional-relationships).

## <a name="delete-behaviors"></a>Löschen von Verhalten
Löschen Verhalten wird definiert, der *deleteBehavior()* Enumerator geben, und übergeben werden kann, um die *OnDelete* fluent-API, um zu steuern, ob das Löschen einer Entität Prinzipal/im übergeordneten Element oder das Trennen von der Beziehung zu abhängige/untergeordneter Entitäten sollten einen Nebeneffekt auf abhängige/untergeordnete Entitäten enthalten.

Es gibt drei Aktionstypen, die EF ergreifen kann, wenn eine Entität Prinzipal/im übergeordneten Element gelöscht wird oder die Beziehung zum untergeordneten Element unterbrochen wird:
* Die untergeordneten/abhängigen können gelöscht werden
* Fremdschlüsselwerte des untergeordneten Standorts können festgelegt werden, auf Null
* Das untergeordnete Element unverändert.

> [!NOTE]  
> Das Löschverhalten konfiguriert in der EF-Core-Modell wird nur angewendet, wenn prinzipalentität wird mithilfe von EF Core gelöscht, und der abhängigen Entitäten (d. h. für überwachte Nachfolger) im Arbeitsspeicher geladen werden. Eine entsprechende überlappungsverhalten muss sein, dass Setup in der Datenbank, um sicherzustellen, dass Daten, die nicht vom Kontext nachverfolgt wird die erforderliche Aktion angewendet wurde. Wenn Sie EF Core verwenden, um die Datenbank zu erstellen, wird dieses Verhalten Cascade für Sie eingerichtet werden.

Für die zweite Aktion ist einen foreign Key-Wert auf null festlegen ungültig, wenn Fremdschlüssel nicht zulässig ist. (Ein NULL-Fremdschlüssel entspricht eine erforderliche Beziehung). In diesen Fällen verfolgt EF Core an, dass die Fremdschlüsseleigenschaft bis SaveChanges aufgerufen wird zu diesem Zeitpunkt eine Ausnahme ausgelöst wird, da die Änderung in der Datenbank dauerhaft gespeichert werden kann, als null markiert wurde. Dies ist vergleichbar mit dem eine einschränkungsverletzung aus der Datenbank abrufen.

Es gibt vier Verhaltensweisen, löschen, wie in den folgenden Tabellen aufgeführt. Für optionale Beziehungen (nullable Fremdschlüssel) es _ist_ möglich, einen null Fremdschlüsselwert speichern vortäuschen folgenden Auswirkungen:

| Verhaltensname               | Auswirkungen auf abhängige und untergeordneten Elementen im Arbeitsspeicher    | Auswirkungen auf abhängige/untergeordnete Datenbank  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| **Cascade**                 | Entitäten werden gelöscht.                   | Entitäten werden gelöscht.                   |
| **ClientSetNull** (Standard) | Fremdschlüsseleigenschaften festgelegt werden auf Null | Keiner                                   |
| **SetNull**                 | Fremdschlüsseleigenschaften festgelegt werden auf Null | Fremdschlüsseleigenschaften festgelegt werden auf Null |
| **Restrict**                | Keiner                                   | Keiner                                   |

Für die erforderlichen Beziehungen (null-Fremdschlüssel) ist es _nicht_ möglich, einen null Fremdschlüsselwert speichern vortäuschen folgenden Auswirkungen:

| Verhaltensname         | Auswirkungen auf abhängige und untergeordneten Elementen im Arbeitsspeicher | Auswirkungen auf abhängige/untergeordnete Datenbank |
|:----------------------|:------------------------------------|:--------------------------------------|
| **CASCADE** (Standard) | Entitäten werden gelöscht.                | Entitäten werden gelöscht.                  |
| **ClientSetNull**     | SaveChanges löst aus                  | Keiner                                  |
| **SetNull**           | SaveChanges löst aus                  | SaveChanges löst aus                    |
| **Restrict**          | Keiner                                | Keiner                                  |

In den obigen Tabellen *keine* kann zu einer Verletzung einer Einschränkung führen. Z. B. wenn eine Prinzipal/untergeordnete Entität wird gelöscht, jedoch keine Aktion ausgeführt wird, um den Fremdschlüssel einer abhängigen/untergeordneten zu ändern, löst klicken Sie dann die Datenbank wahrscheinlich auf SaveChanges aufgrund einer einschränkungsverletzung foreign.

Auf hoher Ebene:
* Wenn Sie Entitäten, die ohne ein übergeordnetes Element vorhanden sein können, und Sie EF, achten Sie darauf für die untergeordneten Elemente automatisch gelöscht werden soll, verwenden Sie *Cascade*.
  * Entitäten, die vorhanden sein können, ohne ein übergeordnetes Element in der Regel stellen für die erforderlichen Beziehungen nutzen *Cascade* ist die Standardeinstellung.
* Wenn Sie die Entitäten, die möglicherweise verfügen möglicherweise nicht über ein übergeordnetes Element haben, und Sie EF zu kümmern, des Fremdschlüssels für Sie erheblich werden soll, verwenden Sie *ClientSetNull*
  * Entitäten, die vorhanden sein können, ohne ein übergeordnetes Element in der Regel stellen Verwenden von optionalen Beziehungen für die *ClientSetNull* ist die Standardeinstellung.
  * Wenn Sie die Datenbank auch versuchen, die null-Werte zu Fremdschlüsseln untergeordneten auch weitergeben soll bei die untergeordneten Entität nicht geladen wird, verwenden Sie dann *SetNull*. Beachten Sie jedoch, dass die Datenbank muss dies unterstützen, und Konfigurieren der Datenbank wie folgt dazu, andere Einschränkungen führen kann, die in der Praxis eignet sich oft diese Option nicht. Deswegen *SetNull* ist nicht der Standard.
* Wenn Sie nicht möchten, dass EF Core jemals Löschen einer Entität automatisch oder automatisch, die den Fremdschlüssel null, dann verwenden *beschränken*. Beachten Sie, dass dies erfordert, dass Ihr Code untergeordneten Entitäten und deren Fremdschlüsselwerte manuell synchronisieren werden andernfalls Einschränkung Ausnahmen ausgelöst werden.

> [!NOTE]
> In EF Kern ausgeführt, im Gegensatz zu EF6 führen Sie kaskadierende Effekte nicht sofort, sondern stattdessen erfolgen nur, wenn SaveChanges aufgerufen wird.

> [!NOTE]  
> **Änderungen in EF Core 2.0:** In früheren Versionen *beschränken* würde optionale Fremdschlüsseleigenschaften in überwachten abhängigen Entitäten festgelegt werden, null und wurde die Standardeinstellung Verhalten für optionale Beziehungen zu löschen. In EF Core 2.0 die *ClientSetNull* wurde eingeführt, um dieses Verhalten darstellen, und stattdessen der Standardwert für optionale Beziehungen. Das Verhalten des *beschränken* wurde angepasst, um keine Nebeneffekte nie auf abhängige Entitäten haben.

## <a name="entity-deletion-examples"></a>Beispiele für das Löschen von Entitäten

Der folgende Code ist Teil einer [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) , heruntergeladen und ausgeführt werden können. Im Beispiel wird gezeigt, was geschieht für jede Löschverhalten für optionale und notwendige Beziehungen, wenn eine übergeordnete Entität gelöscht wird.

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

Wir führen Sie durch jede Variante zu verstehen, was geschieht.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior.Cascade mit erforderlichen oder optionalen Beziehung

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* Blog wird als gelöscht markiert.
* Beiträge bleiben anfänglich unverändert, da sich bis SaveChanges nicht überlappend ausgeführt werden
* SaveChanges sendet löschungen für sowohl Dependents/untergeordnete Elemente (Beiträge), und klicken Sie dann den Prinzipal/im übergeordneten Element (Blog)
* Nach dem Speichern, werden alle Entitäten getrennt, da sie jetzt aus der Datenbank gelöscht wurden

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull oder DeleteBehavior.SetNull mit erforderliche Beziehung

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* Blog wird als gelöscht markiert.
* Beiträge bleiben anfänglich unverändert, da sich bis SaveChanges nicht überlappend ausgeführt werden
* SaveChanges versucht Post-FK auf null festgelegt, aber dies schlägt fehl, da die '-fk ' nicht auf NULL festlegbar ist.

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull oder DeleteBehavior.SetNull mit optionale Beziehung

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* Blog wird als gelöscht markiert.
* Beiträge bleiben anfänglich unverändert, da sich bis SaveChanges nicht überlappend ausgeführt werden
* SaveChanges Versuche Legt die FK der abhängigen Elemente/untergeordnete Elemente (Beiträge) auf null fest, vor dem Löschen der Prinzipal/im übergeordneten Element (Blog)
* Nach dem Speichern der Prinzipal/im übergeordneten Element (Blog) gelöscht, jedoch werden weiterhin die abhängigen Elemente/untergeordneten Elemente (Beiträge) nachverfolgt.
* Die nachverfolgte Dependents/untergeordneten Elemente (Beiträge) haben jetzt FK Nullwerte und ihre Verweis auf die gelöschte Prinzipal/im übergeordneten Element (Blog) wurde entfernt

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior.Restrict mit erforderlichen oder optionalen Beziehung

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* Blog wird als gelöscht markiert.
* Beiträge bleiben anfänglich unverändert, da sich bis SaveChanges nicht überlappend ausgeführt werden
* Da *beschränken* teilt EF auf die '-fk ' nicht automatisch auf null festlegen, bleibt es ungleich Null und SaveChanges löst aus, ohne zu speichern

## <a name="delete-orphans-examples"></a>Beispiele für die verwaisten Objekte löschen

Der folgende Code ist Teil einer [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) , kann eine Ausführung heruntergeladen werden. Im Beispiel wird gezeigt, was geschieht, für jede Löschverhalten für optionale und notwendige Beziehungen, wenn die Beziehung zwischen einem übergeordneten/Prinzipal und seine untergeordneten Elemente/Dependents unterbrochen wird. In diesem Beispiel wird die Beziehung abgetrennt werden durch das Entfernen der abhängigen Elemente/untergeordnete Elemente (Beiträge) aus der auflistungsnavigationseigenschaft für den Prinzipal/im übergeordneten Element (Blog). Allerdings ist das Verhalten identisch, wenn der Verweis von abhängigen und untergeordneten Elementen auf Prinzipal/im übergeordneten Element stattdessen out gelöscht wird.

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

Wir führen Sie durch jede Variante zu verstehen, was geschieht.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior.Cascade mit erforderlichen oder optionalen Beziehung

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* Beiträge werden als geändert markiert, da das Trennen der Beziehung der FK als null markiert sein verursacht werden.
  * Ist der Fremdschlüssel keine NULL-Werte zulässt, klicken Sie dann ändert der tatsächliche Wert sich nicht, obwohl sie als null markiert ist
* SaveChanges sendet löschungen für abhängige Dateien/untergeordnete Elemente (Beiträge)
* Nach dem Speichern, werden die abhängigen Elemente/untergeordneten Elemente (Beiträge) getrennt, da sie jetzt aus der Datenbank gelöscht wurden

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull oder DeleteBehavior.SetNull mit erforderliche Beziehung

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* Beiträge werden als geändert markiert, da das Trennen der Beziehung der FK als null markiert sein verursacht werden.
  * Ist der Fremdschlüssel keine NULL-Werte zulässt, klicken Sie dann ändert der tatsächliche Wert sich nicht, obwohl sie als null markiert ist
* SaveChanges versucht Post-FK auf null festgelegt, aber dies schlägt fehl, da die '-fk ' nicht auf NULL festlegbar ist.

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull oder DeleteBehavior.SetNull mit optionale Beziehung

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* Beiträge werden als geändert markiert, da das Trennen der Beziehung der FK als null markiert sein verursacht werden.
  * Ist der Fremdschlüssel keine NULL-Werte zulässt, klicken Sie dann ändert der tatsächliche Wert sich nicht, obwohl sie als null markiert ist
* SaveChanges legt die FK der abhängigen Elemente/untergeordnete Elemente (Beiträge) auf null fest.
* Nach dem Speichern, die abhängigen Elemente/untergeordneten Elemente (Beiträge) jetzt Nullwerte FK haben und ihre Verweis auf die gelöschte Prinzipal/im übergeordneten Element (Blog) entfernt wurde

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior.Restrict mit erforderlichen oder optionalen Beziehung

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* Beiträge werden als geändert markiert, da das Trennen der Beziehung der FK als null markiert sein verursacht werden.
  * Ist der Fremdschlüssel keine NULL-Werte zulässt, klicken Sie dann ändert der tatsächliche Wert sich nicht, obwohl sie als null markiert ist
* Da *beschränken* teilt EF auf die '-fk ' nicht automatisch auf null festlegen, bleibt es ungleich Null und SaveChanges löst aus, ohne zu speichern

## <a name="cascading-to-untracked-entities"></a>Kaskadierende verfolgte Entitäten

Beim Aufruf *SaveChanges*, die Delete Cascade-Regeln gelten für alle Entitäten, die vom Kontext nachverfolgt werden. Dies ist die Situation, in der alle oben genannten Beispiele, ist SQL zum Löschen der Prinzipal/im übergeordneten Element (Blog) und alle abhängigen Elemente/untergeordneten Elemente (Beiträge) generiert wurde:

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Wenn nur der Prinzipalserver geladen – ist z. B. wenn eine Abfrage erfolgt für einen Blog ohne eine `Include(b => b.Posts)` auch Beiträge--enthalten dann SaveChanges generiert nur SQL aus, um den Prinzipal/im übergeordneten Element löschen:

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Die abhängigen Elemente/untergeordneten Elemente (Beiträge) wird nur gelöscht werden, wenn die Datenbank eine entsprechende überlappungsverhalten konfiguriert wurde. Wenn Sie EF verwenden, um die Datenbank zu erstellen, wird dieses Verhalten Cascade für Sie eingerichtet werden.
