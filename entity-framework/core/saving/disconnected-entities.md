---
title: "Getrennte Entitäten – EF Core"
author: ajcvickers
ms.author: avickers
ms.date: 10/27/2016
ms.assetid: 2533b195-d357-4056-b0e0-8698971bc3b0
ms.technology: entity-framework-core
uid: core/saving/disconnected-entities
ms.openlocfilehash: 0ea02876b9594d54c971a7b70fcf7ce591e56ba0
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/22/2017
---
# <a name="disconnected-entities"></a>Getrennte Entitäten

Eine Instanz von ' DbContext ' verfolgt automatisch Entitäten, die aus der Datenbank zurückgegeben. Änderungen an diesen Entitäten werden dann erkannt werden, wenn SaveChanges aufgerufen und die Datenbank aktualisiert werden, wenn erforderlich. Finden Sie unter [grundlegende speichern](basic.md) und [verknüpften Daten](related-data.md) für Details.

Allerdings sind manchmal Entitäten abgefragt werden mehrere Instanzen von Kontext verwenden, und klicken Sie dann gespeichert mit einer anderen Instanz. Dies geschieht häufig in "getrennt" Szenarien, z. B. eine Webanwendung, in denen die Entitäten werden abgefragt, an den Client gesendet, geändert, zurück an den Server in einer Anforderung gesendet und anschließend gespeichert. In diesem Fall der zweiten Kontext Instanz muss wissen, ob die Entitäten nicht vertraut sind (sollte nun eingefügt sein), oder vorhandene (sollte aktualisiert werden).

> [!TIP]  
> Das in diesem Artikel verwendete [Beispiel](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Disconnected/) finden Sie auf GitHub.

## <a name="identifying-new-entities"></a>Identifizieren neue Entitäten

### <a name="client-identifies-new-entities"></a>Client identifiziert neue Entitäten

Der einfachste Fall zu behandeln ist, wenn der Client dem Server informiert, ob die Entität neuen oder vorhandenen ist. Beispielsweise unterscheidet sich häufig die Anforderung an eine neue Entität einfügen aus der Anforderung zum Aktualisieren einer vorhandenen Entität.

Der übrige Teil dieses Abschnitts behandelt die Fälle, in denen es erforderlich, auf andere Weise zu ermitteln, ob zum Einfügen oder aktualisieren.

### <a name="with-auto-generated-keys"></a>Mit automatisch generierten Schlüsseln

Der Wert von einem automatisch generierten Schlüssel kann häufig verwendet werden, um zu bestimmen, ob eine Entität eingefügt oder aktualisiert werden muss. Wenn der Schlüssel, nicht hat festgelegt (d. h. es immer noch den CLR-Standardwert von Null, 0 (null), usw.), wurde, wird die Entität neue muss und Einfügen benötigt. Andererseits, wenn der Schlüssel-Wert festgelegt wurde, dann muss wurden bereits zuvor gespeichert und ist jetzt ein Update erforderlich. Das heißt, wenn der Schlüssel einen Wert hat, klicken Sie dann Entität abgefragt wurde, an den Client gesendet und hat jetzt zurückkehren, um aktualisiert werden.

Es ist einfach, eine Festlegung Schlüssel überprüfen, wenn der Entitätstyp bekannt ist:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewSimple)]

EF hat jedoch auch eine integrierten Möglichkeit hierzu für jede Entitätstyp und der Typ des Schlüssels:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewGeneral)]

> [!TIP]  
> Schlüssel werden als Entitäten vom Kontext, nachverfolgt werden festgelegt, selbst wenn die Entität im Zustand ' Added ' befindet. Dies ist hilfreich beim Durchlaufen von Entitäten und entscheiden Vorgehensweise mit einzelnen, z. B. bei Verwendung der TrackGraph-API in einem Diagramm. Der Schlüssel-Wert sollte nur verwendet werden, auf die Weise, die hier gezeigten _vor_ wird jeder Aufruf zum Nachverfolgen der Entität.

### <a name="with-other-keys"></a>Mit anderen Schlüsseln

Ein anderen Mechanismus ist erforderlich, um neue Entitäten zu identifizieren, wenn die Schlüsselwerte nicht automatisch generiert werden. Es gibt zwei allgemeine Vorgehensweisen beim dies:
 * Abfrage für die Entität
 * Übergeben Sie ein Flag vom client

Für die Entität eine Abfrage nur verwenden Sie die Find-Methode:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#IsItNewQuery)]

Es ist nicht Gegenstand dieses Dokuments, um den vollständigen Code für die Übergabe eines Flags von einem Client anzuzeigen. In einer Web-app bedeutet dies normalerweise, die verschiedene Anforderungen für unterschiedliche Aktionen, oder ein Zustand, der in der Anforderung übergeben, und extrahieren es im Controller.

## <a name="saving-single-entities"></a>Speichern die einzelne Entitäten

Wenn es bekannt ist, unabhängig davon, ob eine INSERT- oder Update ist erforderlich, und klicken Sie dann hinzufügen oder aktualisieren ordnungsgemäß verwendet werden kann:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertAndUpdateSingleEntity)]

Jedoch, wenn die Entität automatisch generierten Schlüsselwerte verwendet wird, kann dann die Update-Methode für beide Fälle verwendet werden:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntity)]

Die Update-Methode markiert normalerweise die Entität für das Update nicht einfügen. Wenn die Entität einen automatisch generierten Schlüssel hat, und kein Schlüsselwert festgelegt wurde, und klicken Sie dann die Entität für stattdessen automatisch markiert ist allerdings fügen Sie zu können ein.

> [!TIP]  
> Dieses Verhalten wurde in der EF Core 2.0 eingeführt. Bei früheren Versionen ist es immer erforderlich, um explizit anzugeben, hinzufügen oder aktualisieren.

Wenn die Entität wird nicht automatisch generierten Schlüssel verwenden, die Anwendung muss entscheiden Sie, ob die Entität eingefügt oder aktualisiert werden: Z. B.:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateSingleEntityWithFind)]

Bei den folgenden Schritten werden:
* Wenn suchen gibt null ist, und klicken Sie dann die Datenbank im Blog mit dieser ID bereits enthält, damit wir rufen hinzufügen markieren Sie es zum Einfügen.
* Wenn suchen eine Entität zurückgibt, klicken sie in der Datenbank vorhanden ist und der Kontext verfolgt nun der vorhandenen Entität
  * Klicken Sie dann verwenden wir SetValues, um die Werte für alle Eigenschaften für diese Entität, mit denen festgelegt wird, die vom Client stammen.
  * Der Aufruf SetValues werden in der Entität aktualisiert werden, nach Bedarf gekennzeichnet.

> [!TIP]  
> SetValues kennzeichnet nur, wie die Eigenschaften geändert werden, die über unterschiedliche Werte in die nachverfolgte Entität verfügen. Dies bedeutet, dass das Update gesendet werden, wird nur die Spalten, die tatsächlich geändert wurden aktualisiert. (Und wenn nichts geändert wurde, wird kein Update auf allen gesendet,.)

## <a name="working-with-graphs"></a>Arbeiten mit Diagrammen

### <a name="all-newall-existing-entities"></a>Alle neuen Eigenschaft bzw. alle vorhandenen Entitäten

Ein Beispiel für das Arbeiten mit Diagrammen wird einfügen oder aktualisieren ein Blogs zusammen mit ihrer Auflistung von zugehörigen Beiträge. Wenn alle Entitäten im Diagramm eingefügt werden soll, oder alle aktualisiert werden soll, ist der Prozess identisch für einzelne Entitäten wie oben beschrieben aus. Angenommen, ein Diagramm der Blogs und Beiträge, die wie folgt erstellt:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#CreateBlogAndPosts)]

können wie folgt eingefügt werden:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertGraph)]

Der Aufruf von Add werden im Blog und alle Beiträge einzufügenden gekennzeichnet.

Ebenso, wenn alle Entitäten in einem Diagramm aktualisiert werden müssen, kann dann Update verwendet werden:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#UpdateGraph)]

Im Blog und alle ihre Beiträge werden gekennzeichnet werden, um aktualisiert werden.

### <a name="mix-of-new-and-existing-entities"></a>Mischung neue und vorhandene Entitäten

Mit automatisch generierten Schlüssel kann Update erneut für einfügungen und Updates, verwendet werden, selbst wenn das Diagramm enthält eine Mischung aus Entitäten, die eingefügt werden müssen, auch solche, die aktualisiert werden müssen:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraph)]

Update kennzeichnet jede Entität im Diagramm, Blog oder Post zum Einfügen, wenn sie einen Satz Schlüsselwert keinen während alle anderen Entitäten für Update markiert sind.

Wie vor, bei der automatisch generierten Schlüssel nicht mit einer Abfrage und einige Verarbeitungsschritte verwendet werden können:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertOrUpdateGraphWithFind)]

## <a name="handling-deletes"></a>Behandlung von Löschvorgängen

Delete aufspüren zu kann, da behandeln das Fehlen einer Entität bedeutet häufig, dass sie gelöscht werden soll. Eine Möglichkeit für den Umgang mit Dies ist die Verwendung von "vorläufige Löschvorgänge", so, dass die Entität markiert ist, als gelöscht, statt Sie tatsächlich gelöscht werden. Löscht wird dann der Updates identisch. Vorläufige Löschvorgänge können implementiert werden, sich mit [Abfragen Filter](xref:core/querying/filters).

Für "true" löscht wird ein allgemeines Muster um eine Erweiterung des Abfragemusters für eine durchführen, was im Wesentlichen ein Diagramm Diff. Zum Beispiel:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#InsertUpdateOrDeleteGraphWithFind)]

## <a name="trackgraph"></a>TrackGraph

Intern, Add-, Anfügen und Update verwenden Graph-Durchlauf mit einem Feststellung für jede Entität, gibt an, ob er als hinzugefügt (insert), "geändert" (zu aktualisieren), markiert werden soll Unchanged (keine), oder gelöschte (zu löschen). Dieser Mechanismus wird über die TrackGraph-API verfügbar gemacht. Nehmen wir beispielsweise an, sendet der Client wieder Entitäten in einem Diagramm wird einige Kennzeichen für jede Entität, der angibt, wie sie behandelt werden sollen. TrackGraph kann dann verwendet werden, um dieses Flag zu verarbeiten:

[!code-csharp[Main](../../../samples/core/Saving/Saving/Disconnected/Sample.cs#TrackGraph)]

Die Flags werden nur als Teil der Entität aus Gründen der Einfachheit der im Beispiel angezeigt. In der Regel würden die Flags Teil einer DTO oder einem anderen Status in der Anforderung enthalten sein.
