---
title: Beziehungen-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 1e59ce9e19c12aa5564bc8467dcfcb3be8ee8996
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655674"
---
# <a name="relationships"></a>Beziehungen

Eine Beziehung definiert, wie zwei Entitäten miteinander in Beziehung stehen. In einer relationalen Datenbank wird dies durch eine FOREIGN KEY-Einschränkung repräsentiert.

> [!NOTE]  
> Die meisten Beispiele in diesem Artikel verwenden eine 1: n-Beziehung, um die Konzepte zu veranschaulichen. Beispiele für 1:1-und m:n-Beziehungen finden Sie im Abschnitt [andere Beziehungsmuster](#other-relationship-patterns) am Ende des Artikels.

## <a name="definition-of-terms"></a>Definition von Begriffen

Es gibt eine Reihe von Begriffen, die zum Beschreiben von Beziehungen verwendet werden.

* **Abhängige Entität:** Dies ist die Entität, die die Fremdschlüssel Eigenschaft (en) enthält. Wird manchmal auch als ' Child ' der Beziehung bezeichnet.

* **Prinzipal Entität:** Dies ist die Entität, die die primären/Alternativen Schlüsseleigenschaft (en) enthält. Wird manchmal auch als "übergeordnetes Element" der Beziehung bezeichnet.

* **Fremdschlüssel:** Die Eigenschaften in der abhängigen Entität, die verwendet werden, um die Werte der Prinzipal Schlüsseleigenschaft zu speichern, mit der die Entität verknüpft ist.

* **Prinzipal Schlüssel:** Die Eigenschaften, die die Prinzipal Entität eindeutig identifizieren. Hierbei kann es sich um den Primärschlüssel oder einen alternativen Schlüssel handeln.

* **Navigations Eigenschaft:** Eine Eigenschaft, die für die Prinzipal-und/oder abhängige Entität definiert ist, die einen Verweis auf die verknüpfte Entität (en) enthält.

  * Auflistungs **Navigations Eigenschaft:** Eine Navigations Eigenschaft, die Verweise auf viele Verwandte Entitäten enthält.

  * **Verweis Navigations Eigenschaft:** Eine Navigations Eigenschaft, die einen Verweis auf eine einzelne verknüpfte Entität enthält.

  * **Umgekehrte Navigations Eigenschaft:** Bei der Erörterung einer bestimmten Navigations Eigenschaft bezieht sich dieser Begriff auf die Navigations Eigenschaft am anderen Ende der Beziehung.

Das folgende Codebeispiel zeigt eine 1: n-Beziehung zwischen `Blog` und `Post`

* `Post` ist die abhängige Entität.

* `Blog` ist die Prinzipal Entität.

* `Post.BlogId` ist der Fremdschlüssel.

* `Blog.BlogId` ist der Prinzipal Schlüssel (in diesem Fall handelt es sich um einen Primärschlüssel anstelle eines alternativen Schlüssels).

* `Post.Blog` ist eine Verweis Navigations Eigenschaft.

* `Blog.Posts` ist eine Auflistungs Navigations Eigenschaft.

* `Post.Blog` ist die umgekehrte Navigations Eigenschaft `Blog.Posts` (und umgekehrt).

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Entities)]

## <a name="conventions"></a>Konventionen

Gemäß der Konvention wird eine Beziehung erstellt, wenn eine Navigations Eigenschaft für einen Typ erkannt wird. Eine Eigenschaft wird als Navigations Eigenschaft betrachtet, wenn der Typ, auf den Sie verweist, nicht vom aktuellen Datenbankanbieter als Skalartyp zugeordnet werden kann.

> [!NOTE]  
> Die von der Konvention ermittelten Beziehungen richten sich immer nach dem Primärschlüssel der Prinzipal Entität. Um einen alternativen Schlüssel als Ziel zu verwenden, muss mithilfe der flüssigen API eine zusätzliche Konfiguration durchgeführt werden.

### <a name="fully-defined-relationships"></a>Vollständig definierte Beziehungen

Das gängigste Muster für Beziehungen besteht darin, dass für beide Enden der Beziehung Navigations Eigenschaften und eine Fremdschlüssel Eigenschaft definiert sind, die in der abhängigen Entitäts Klasse definiert ist.

* Wenn zwischen zwei Typen ein paar Navigations Eigenschaften gefunden wird, werden diese als umgekehrte Navigations Eigenschaften derselben Beziehung konfiguriert.

* Wenn die abhängige Entität eine Eigenschaft mit dem Namen `<primary key property name>`, `<navigation property name><primary key property name>`oder `<principal entity name><primary key property name>` enthält, wird Sie als Fremdschlüssel konfiguriert.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> Wenn mehrere Navigations Eigenschaften zwischen zwei Typen definiert sind (d. h. mehr als ein unterschiedliches Navigations Paar, das aufeinander zeigt), werden keine Beziehungen gemäß der Konvention erstellt und müssen manuell konfiguriert werden, um zu ermitteln, wie das Das Paar der Navigations Eigenschaften

### <a name="no-foreign-key-property"></a>Keine Fremdschlüssel Eigenschaft

Es wird empfohlen, eine Fremdschlüssel Eigenschaft in der abhängigen Entitäts Klasse zu definieren, die jedoch nicht erforderlich ist. Wenn keine Fremdschlüssel Eigenschaft gefunden wird, wird eine Schatten-Fremdschlüssel Eigenschaft mit dem Namen `<navigation property name><principal key property name>` eingeführt (Weitere Informationen finden Sie unter [Schatten Eigenschaften](shadow-properties.md) ).

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a>Einzelne Navigations Eigenschaft

Das Einschließen von nur einer Navigations Eigenschaft (keine umgekehrte Navigation und keine Fremdschlüssel Eigenschaft) reicht aus, um eine Beziehung gemäß der Konvention zu definieren. Sie können auch eine einzelne Navigations Eigenschaft und eine Fremdschlüssel Eigenschaft haben.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a>Kaskadierendes Delete

Gemäß der Konvention wird CASCADE DELETE für erforderliche Beziehungen und *clientsetnull* für optionale Beziehungen auf *Cascade* festgelegt. *Cascade* bedeutet, dass abhängige Entitäten ebenfalls gelöscht werden. *Clientsetnull* bedeutet, dass abhängige Entitäten, die nicht in den Arbeitsspeicher geladen werden, unverändert bleiben und manuell gelöscht oder aktualisiert werden müssen, um auf eine gültige Prinzipal Entität zu verweisen. Bei Entitäten, die in den Arbeitsspeicher geladen werden, wird EF Core versucht, die Fremdschlüssel Eigenschaften auf NULL festzulegen.

Den Unterschied zwischen erforderlichen und optionalen Beziehungen finden Sie im Abschnitt [erforderliche und optionale Beziehungen](#required-and-optional-relationships) .

Weitere Informationen zu den verschiedenen Lösch Verhalten und den von der Konvention verwendeten Standardwerten finden Sie unter [Cascade Delete](../saving/cascade-delete.md) .

## <a name="data-annotations"></a>Datenanmerkungen

Es gibt zwei Daten Anmerkungen, die zum Konfigurieren von Beziehungen, `[ForeignKey]` und `[InverseProperty]`verwendet werden können. Diese sind im `System.ComponentModel.DataAnnotations.Schema`-Namespace verfügbar.

### <a name="foreignkey"></a>ForeignKey

Mit den Daten Anmerkungen können Sie konfigurieren, welche Eigenschaft als Fremdschlüssel Eigenschaft für eine bestimmte Beziehung verwendet werden soll. Dies erfolgt in der Regel, wenn die Fremdschlüssel Eigenschaft nicht gemäß der Konvention ermittelt wird.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> Die `[ForeignKey]`-Anmerkung kann in einer der beiden Navigations Eigenschaften in der Beziehung platziert werden. Die Navigations Eigenschaft in der abhängigen Entitäts Klasse muss nicht verwendet werden.

### <a name="inverseproperty"></a>[Invergenproperty]

Mit den Daten Anmerkungen können Sie konfigurieren, wie Navigations Eigenschaften für die abhängigen und Prinzipal Entitäten gekoppelt werden. Dies erfolgt in der Regel, wenn zwischen zwei Entitäts Typen mehr als ein paar Navigations Eigenschaften vorhanden ist.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a>Fluent-API

Um eine Beziehung in der fließenden API zu konfigurieren, müssen Sie zunächst die Navigations Eigenschaften ermitteln, die die Beziehung bilden. `HasOne` oder `HasMany` identifiziert die Navigations Eigenschaft für den Entitätstyp, für den Sie mit der Konfiguration beginnen. Anschließend verketten Sie einen aufzurufenden `WithOne` oder `WithMany`, um die umgekehrte Navigation zu ermitteln. `HasOne`/`WithOne` für die Eigenschaften der Verweis Navigation verwendet und `HasMany`/`WithMany` für Auflistungs Navigations Eigenschaften verwendet werden.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a>Einzelne Navigations Eigenschaft

Wenn Sie nur über eine Navigations Eigenschaft verfügen, sind `WithOne` und `WithMany`parameterlose über Ladungen vorhanden. Dies gibt an, dass ein Verweis oder eine Auflistung am anderen Ende der Beziehung konzeptionell ist, aber in der Entitäts Klasse ist keine Navigations Eigenschaft enthalten.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a>Fremdschlüssel

Sie können die fließende API verwenden, um zu konfigurieren, welche Eigenschaft als Fremdschlüssel Eigenschaft für eine bestimmte Beziehung verwendet werden soll.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?highlight=17)]

Das folgende Codebeispiel zeigt, wie ein zusammengesetzter Fremdschlüssel konfiguriert wird.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?highlight=20)]

Sie können die Zeichen folgen Überladung von `HasForeignKey(...)` verwenden, um eine Schatten Eigenschaft als Fremdschlüssel zu konfigurieren (Weitere Informationen finden Sie unter [Schatten Eigenschaften](shadow-properties.md) ). Es wird empfohlen, die Schatten Eigenschaft explizit dem Modell hinzuzufügen, bevor Sie als Fremdschlüssel verwendet wird (wie unten gezeigt).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a>Ohne Navigations Eigenschaft

Sie müssen nicht unbedingt eine Navigations Eigenschaft bereitstellen. Sie können auf einer Seite der Beziehung einfach einen Fremdschlüssel bereitstellen.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a>Prinzipal Schlüssel

Wenn Sie möchten, dass der Fremdschlüssel auf eine andere Eigenschaft als den Primärschlüssel verweist, können Sie mit der fließend-API die Prinzipal Schlüsseleigenschaft für die Beziehung konfigurieren. Die Eigenschaft, die Sie als Prinzipal Schlüssel konfigurieren, wird automatisch als alternativer Schlüssel eingerichtet (Weitere Informationen finden Sie in den [Alternativen Schlüsseln](alternate-keys.md) ).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?name=PrincipalKey&highlight=11)]

Im folgenden Codebeispiel wird gezeigt, wie ein zusammengesetzter Prinzipal Schlüssel konfiguriert wird.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?name=Composite&highlight=11)]

> [!WARNING]  
> Die Reihenfolge, in der Sie die Prinzipal Schlüsseleigenschaften angeben, muss mit der Reihenfolge identisch sein, in der Sie für den Fremdschlüssel angegeben werden.

### <a name="required-and-optional-relationships"></a>Erforderliche und optionale Beziehungen

Mit der fließend-API können Sie konfigurieren, ob die Beziehung erforderlich oder optional ist. Letztendlich steuert dies, ob die Fremdschlüssel Eigenschaft erforderlich oder optional ist. Dies ist besonders nützlich, wenn Sie einen Schatten Zustands-Fremdschlüssel verwenden. Wenn Sie über eine Fremdschlüssel Eigenschaft in der Entitäts Klasse verfügen, wird die Eignung der Beziehung abhängig davon bestimmt, ob die Fremdschlüssel Eigenschaft erforderlich oder optional ist (Weitere Informationen finden Sie unter [erforderliche und optionale Eigenschaften](required-optional.md) ).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?name=Required&highlight=11)]

### <a name="cascade-delete"></a>Kaskadierendes Delete

Sie können die fließende API verwenden, um das kaskadierte Lösch Verhalten für eine bestimmte Beziehung explizit zu konfigurieren.

Eine ausführliche Erläuterung der einzelnen Optionen finden Sie im Abschnitt " [Löschen](../saving/cascade-delete.md) von Daten" im Abschnitt "Speichern von Daten".

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?name=CascadeDelete&highlight=11)]

## <a name="other-relationship-patterns"></a>Andere Beziehungsmuster

### <a name="one-to-one"></a>Eins zu eins

Eine zu 1 Beziehung hat eine Verweis Navigations Eigenschaft auf beiden Seiten. Sie folgen denselben Konventionen wie 1: n-Beziehungen, aber es wird ein eindeutiger Index für die Fremdschlüssel Eigenschaft eingeführt, um sicherzustellen, dass nur ein abhängiger mit den einzelnen Prinzipalen verknüpft ist.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneToOne.cs?name=Property&highlight=6,15,16)]

> [!NOTE]  
> EF wählt eine der Entitäten aus, die abhängig von der Fähigkeit, eine Fremdschlüssel Eigenschaft zu erkennen, abhängig ist. Wenn die falsche Entität als abhängig ausgewählt wird, können Sie diese mithilfe der flüssigen API korrigieren.

Beim Konfigurieren der Beziehung mit der flüssigen API verwenden Sie die Methoden `HasOne` und `WithOne`.

Beim Konfigurieren des fremd Schlüssels müssen Sie den abhängigen Entitätstyp angeben. Beachten Sie den generischen Parameter, der für `HasForeignKey` in der folgenden Auflistung bereitgestellt wird. In einer 1: n-Beziehung ist klar, dass die Entität mit der Verweis Navigation die abhängige ist und die mit der Auflistung der Prinzipal ist. Dies ist jedoch in einer eins-zu-eins-Beziehung nicht der Fall, daher muss Sie explizit definiert werden.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?name=OneToOne&highlight=11)]

### <a name="many-to-many"></a>M:n-Zahl

M:n-Beziehungen ohne Entitäts Klasse zur Darstellung der jointabelle werden noch nicht unterstützt. Sie können jedoch eine m:n-Beziehung darstellen, indem Sie eine Entitäts Klasse für die jointabelle einschließen und zwei separate 1: n-Beziehungen Mapping.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?name=ManyToMany&highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)]
