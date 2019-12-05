---
title: Beziehungen-EF Core
description: Konfigurieren von Beziehungen zwischen Entitäts Typen bei Verwendung von Entity Framework Core
author: AndriySvyryd
ms.date: 11/21/2019
uid: core/modeling/relationships
ms.openlocfilehash: 452169c902d56fda0a65a5c2846a9b42c80640fd
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824764"
---
# <a name="relationships"></a>Beziehungen

Eine Beziehung definiert, wie zwei Entitäten miteinander in Beziehung stehen. In einer relationalen Datenbank wird dies durch eine FOREIGN KEY-Einschränkung repräsentiert.

> [!NOTE]  
> Die meisten Beispiele in diesem Artikel verwenden eine 1: n-Beziehung, um die Konzepte zu veranschaulichen. Beispiele für 1:1-und m:n-Beziehungen finden Sie im Abschnitt [andere Beziehungsmuster](#other-relationship-patterns) am Ende des Artikels.

## <a name="definition-of-terms"></a>Definition von Begriffen

Es wird eine Reihe von Begriffen verwendet, um Beziehungen in Datenbanken zu beschreiben

* **Abhängige Entität:** Dies ist die Entität, die die Fremdschlüssel Eigenschaften enthält. Auch "Child" genannt.

* **Prinzipal Entität:** Dies ist die Entität, die die Eigenschaften des primären/Alternativen Schlüssels enthält. Auch "Parent" genannt.

* **Fremdschlüssel:** Die Eigenschaften in der abhängigen Entität, die zum Speichern der Prinzipal Schlüsselwerte für die verknüpfte Entität verwendet werden.

* **Prinzipal Schlüssel:** Die Eigenschaften, die die Prinzipal Entität eindeutig identifizieren. Dies kann ein Primär- oder Alternativschlüssel sein.

* **Navigations Eigenschaft:** Eine für die Prinzipal-und/oder abhängige Entität definierte Eigenschaft, die auf die verknüpfte Entität verweist.

  * Auflistungs **Navigations Eigenschaft:** Eine Navigations Eigenschaft, die Verweise auf viele Verwandte Entitäten enthält.

  * **Verweis Navigations Eigenschaft:** Eine Navigations Eigenschaft, die einen Verweis auf eine einzelne verknüpfte Entität enthält.

  * **Inverse Navigationseigenschaft:** im Zusammenhang mit einer beliebigen anderen Navigationseigenschaft stellt diese Eigenschaft das anderen Ende der Beziehung dar.
  
* **Selbst verweisende Beziehung:** Eine Beziehung, in der die abhängigen und die Prinzipal Entitäts Typen identisch sind.

Der folgende Code zeigt eine 1: n-Beziehung zwischen `Blog` und `Post`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Entities)]

* `Post` ist die abhängige Entität.

* `Blog` ist die Prinzipalentität

* `Post.BlogId` ist der Fremdschlüssel.

* `Blog.BlogId` der Prinzipalschlüssel (in diesem Fall ist es ein Primärschlüssel anstelle eines Alternativschlüssels)

* `Post.Blog` ist eine Verweisnavigationseigenschaft

* `Blog.Posts` ist eine Auflistungsnavigationseigenschaft

* `Post.Blog`ist die inverse Navigationseigenschaft von `Blog.Posts`(und umgekehrt)

## <a name="conventions"></a>Konventionen

Standardmäßig wird eine Beziehung erstellt, wenn eine Navigations Eigenschaft für einen Typ erkannt wird. Eine Eigenschaft wird als Navigations Eigenschaft betrachtet, wenn der Typ, auf den Sie verweist, nicht vom aktuellen Datenbankanbieter als Skalartyp zugeordnet werden kann.

> [!NOTE]  
> Die von der Konvention ermittelten Beziehungen richten sich immer nach dem Primärschlüssel der Prinzipal Entität. Um einen alternativen Schlüssel als Ziel zu verwenden, muss mithilfe der flüssigen API eine zusätzliche Konfiguration durchgeführt werden.

### <a name="fully-defined-relationships"></a>Vollständig definierte Beziehungen

Das gängigste Muster für Beziehungen besteht darin, dass für beide Enden der Beziehung Navigations Eigenschaften und eine Fremdschlüssel Eigenschaft definiert sind, die in der abhängigen Entitäts Klasse definiert ist.

* Wenn zwischen zwei Typen ein paar Navigations Eigenschaften gefunden wird, werden diese als umgekehrte Navigations Eigenschaften derselben Beziehung konfiguriert.

* Wenn die abhängige Entität eine Eigenschaft mit einem Namen enthält, wird eines dieser Muster als Fremdschlüssel konfiguriert:
  * `<navigation property name><principal key property name>`
  * `<navigation property name>Id`
  * `<principal entity name><principal key property name>`
  * `<principal entity name>Id`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

In diesem Beispiel werden die markierten Eigenschaften zum Konfigurieren der Beziehung verwendet.

> [!NOTE]
> Wenn die Eigenschaft der Primärschlüssel oder ein Typ ist, der nicht mit dem Prinzipal Schlüssel kompatibel ist, wird Sie nicht als Fremdschlüssel konfiguriert.

> [!NOTE]
> Vor EF Core 3,0 wurde die Eigenschaft mit dem gleichen Namen wie die Prinzipal Schlüsseleigenschaft [ebenfalls als Fremdschlüssel abgeglichen](https://github.com/aspnet/EntityFrameworkCore/issues/13274) .

### <a name="no-foreign-key-property"></a>Keine Fremdschlüssel Eigenschaft

Es wird empfohlen, eine Fremdschlüssel Eigenschaft in der abhängigen Entitäts Klasse zu definieren, die jedoch nicht erforderlich ist. Wenn keine Fremdschlüssel Eigenschaft gefunden wird, wird eine [Schatten-Fremdschlüssel Eigenschaft](shadow-properties.md) mit dem Namen `<navigation property name><principal key property name>` oder `<principal entity name><principal key property name>`, wenn keine Navigation für den abhängigen Typ vorhanden ist, eingefügt.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

In diesem Beispiel wird der Schatten Fremdschlüssel `BlogId`, da der vorangestellt ist, dass der Navigations Name redundant ist.

> [!NOTE]
> Wenn bereits eine Eigenschaft mit demselben Namen vorhanden ist, wird dem Namen der Schatten Eigenschaft eine Zahl angehängt.

### <a name="single-navigation-property"></a>Einzelne Navigations Eigenschaft

Das Einschließen von nur einer Navigations Eigenschaft (keine umgekehrte Navigation und keine Fremdschlüssel Eigenschaft) reicht aus, um eine Beziehung gemäß der Konvention zu definieren. Sie können auch eine einzelne Navigations Eigenschaft und eine Fremdschlüssel Eigenschaft haben.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="limitations"></a>Einschränkungen

Wenn mehrere Navigations Eigenschaften zwischen zwei Typen definiert sind (d. h. mehr als nur ein Navigations Paar, das aufeinander zeigen), sind die Beziehungen, die von den Navigations Eigenschaften dargestellt werden, mehrdeutig. Sie müssen Sie manuell konfigurieren, um die Mehrdeutigkeit aufzulösen.

### <a name="cascade-delete"></a>Löschweitergabe

Gemäß der Konvention wird CASCADE DELETE für erforderliche Beziehungen und *clientsetnull* für optionale Beziehungen auf *Cascade* festgelegt. *Cascade* bedeutet, dass abhängige Entitäten ebenfalls gelöscht werden. *Clientsetnull* bedeutet, dass abhängige Entitäten, die nicht in den Arbeitsspeicher geladen werden, unverändert bleiben und manuell gelöscht oder aktualisiert werden müssen, um auf eine gültige Prinzipal Entität zu verweisen. Bei Entitäten, die in den Arbeitsspeicher geladen werden, wird EF Core versucht, die Fremdschlüssel Eigenschaften auf NULL festzulegen.

Den Unterschied zwischen erforderlichen und optionalen Beziehungen finden Sie im Abschnitt [erforderliche und optionale Beziehungen](#required-and-optional-relationships) .

Weitere Informationen zu den verschiedenen Lösch Verhalten und den von der Konvention verwendeten Standardwerten finden Sie unter [Cascade Delete](../saving/cascade-delete.md) .

## <a name="manual-configuration"></a>Manuelle Konfiguration

#### <a name="fluent-apitabfluent-api"></a>[Fließende API](#tab/fluent-api)

Um eine Beziehung in der fließenden API zu konfigurieren, müssen Sie zunächst die Navigations Eigenschaften ermitteln, die die Beziehung bilden. `HasOne` oder `HasMany` identifiziert die Navigations Eigenschaft für den Entitätstyp, für den Sie mit der Konfiguration beginnen. Anschließend verketten Sie einen aufzurufenden `WithOne` oder `WithMany`, um die umgekehrte Navigation zu ermitteln. `HasOne`/`WithOne` für die Eigenschaften der Verweis Navigation verwendet und `HasMany`/`WithMany` für Auflistungs Navigations Eigenschaften verwendet werden.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?highlight=14-16)]

#### <a name="data-annotationstabdata-annotations"></a>[Daten Anmerkungen](#tab/data-annotations)

Mit den Daten Anmerkungen können Sie konfigurieren, wie Navigations Eigenschaften für die abhängigen und Prinzipal Entitäten gekoppelt werden. Dies erfolgt in der Regel, wenn zwischen zwei Entitäts Typen mehr als ein paar Navigations Eigenschaften vorhanden ist.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?highlight=33,36)]

> [!NOTE]
> Sie können für die Eigenschaften der abhängigen Entität nur [Required] verwenden, um die Eignung der Beziehung zu beeinflussen. [Erforderlich] die Navigation von der Prinzipal Entität wird normalerweise ignoriert, kann jedoch dazu führen, dass die Entität zu einer abhängigen Entität wird.

> [!NOTE]
> Die Daten Anmerkungen `[ForeignKey]` und `[InverseProperty]` sind im `System.ComponentModel.DataAnnotations.Schema`-Namespace verfügbar. `[Required]` ist im `System.ComponentModel.DataAnnotations`-Namespace verfügbar.

---

### <a name="single-navigation-property"></a>Einzelne Navigations Eigenschaft

Wenn Sie nur über eine Navigations Eigenschaft verfügen, sind `WithOne` und `WithMany`parameterlose über Ladungen vorhanden. Dies gibt an, dass ein Verweis oder eine Auflistung am anderen Ende der Beziehung konzeptionell ist, aber in der Entitäts Klasse ist keine Navigations Eigenschaft enthalten.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a>Fremdschlüssel

#### <a name="fluent-apitabfluent-api"></a>[Fließende API](#tab/fluent-api)

Sie können die fließende API verwenden, um zu konfigurieren, welche Eigenschaft als Fremdschlüssel Eigenschaft für eine bestimmte Beziehung verwendet werden soll.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?highlight=17)]

#### <a name="data-annotationstabdata-annotations"></a>[Daten Anmerkungen](#tab/data-annotations)

Mit den Daten Anmerkungen können Sie konfigurieren, welche Eigenschaft als Fremdschlüssel Eigenschaft für eine bestimmte Beziehung verwendet werden soll. Dies erfolgt in der Regel, wenn die Fremdschlüssel Eigenschaft nicht gemäß der Konvention ermittelt wird.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> Die `[ForeignKey]`-Anmerkung kann in einer der beiden Navigations Eigenschaften in der Beziehung platziert werden. Die Navigations Eigenschaft in der abhängigen Entitäts Klasse muss nicht verwendet werden.

> [!NOTE]
> Die mit `[ForeignKey]` für eine Navigations Eigenschaft angegebene Eigenschaft muss nicht den abhängigen Typ enthalten. In diesem Fall wird der angegebene Name verwendet, um einen Schatten Fremdschlüssel zu erstellen.

---

Der folgende Code zeigt, wie ein zusammengesetzter Fremdschlüssel konfiguriert wird.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?highlight=20)]

Sie können die Zeichen folgen Überladung von `HasForeignKey(...)` verwenden, um eine Schatten Eigenschaft als Fremdschlüssel zu konfigurieren (Weitere Informationen finden Sie unter [Schatten Eigenschaften](shadow-properties.md) ). Es wird empfohlen, die Schatten Eigenschaft explizit dem Modell hinzuzufügen, bevor Sie als Fremdschlüssel verwendet wird (wie unten gezeigt).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a>Ohne Navigations Eigenschaft

Sie müssen nicht unbedingt eine Navigations Eigenschaft bereitstellen. Sie können auf einer Seite der Beziehung einfach einen Fremdschlüssel bereitstellen.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a>Prinzipal Schlüssel

Wenn Sie möchten, dass der Fremdschlüssel auf eine andere Eigenschaft als den Primärschlüssel verweist, können Sie mit der fließend-API die Prinzipal Schlüsseleigenschaft für die Beziehung konfigurieren. Die Eigenschaft, die Sie als Prinzipal Schlüssel konfigurieren, wird automatisch als [alternativer Schlüssel](alternate-keys.md)eingerichtet.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?name=PrincipalKey&highlight=11)]

Der folgende Code zeigt, wie ein zusammengesetzter Prinzipal Schlüssel konfiguriert wird.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?name=Composite&highlight=11)]

> [!WARNING]  
> Die Reihenfolge, in der Sie die Prinzipal Schlüsseleigenschaften angeben, muss mit der Reihenfolge identisch sein, in der Sie für den Fremdschlüssel angegeben werden.

### <a name="required-and-optional-relationships"></a>Erforderliche und optionale Beziehungen

Mit der fließend-API können Sie konfigurieren, ob die Beziehung erforderlich oder optional ist. Letztendlich steuert dies, ob die Fremdschlüssel Eigenschaft erforderlich oder optional ist. Dies ist besonders nützlich, wenn Sie einen Schatten Zustands-Fremdschlüssel verwenden. Wenn Sie über eine Fremdschlüssel Eigenschaft in der Entitäts Klasse verfügen, wird die Eignung der Beziehung abhängig davon bestimmt, ob die Fremdschlüssel Eigenschaft erforderlich oder optional ist (Weitere Informationen finden Sie unter [erforderliche und optionale Eigenschaften](required-optional.md) ).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?name=Required&highlight=11)]

> [!NOTE]
> Durch das Aufrufen von `IsRequired(false)` wird die Fremdschlüssel Eigenschaft auch dann optional, wenn Sie nicht anderweitig konfiguriert ist.

### <a name="cascade-delete"></a>Löschweitergabe

Sie können die fließende API verwenden, um das kaskadierte Lösch Verhalten für eine bestimmte Beziehung explizit zu konfigurieren.

Eine ausführliche Erläuterung der einzelnen Optionen finden Sie unter [Cascade Delete](../saving/cascade-delete.md) .

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?name=CascadeDelete&highlight=11)]

## <a name="other-relationship-patterns"></a>Andere Beziehungsmuster

### <a name="one-to-one"></a>1:1

Eine zu 1 Beziehung hat eine Verweis Navigations Eigenschaft auf beiden Seiten. Sie folgen denselben Konventionen wie 1: n-Beziehungen, aber es wird ein eindeutiger Index für die Fremdschlüssel Eigenschaft eingeführt, um sicherzustellen, dass nur ein abhängiger mit den einzelnen Prinzipalen verknüpft ist.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneToOne.cs?name=Property&highlight=6,15,16)]

> [!NOTE]  
> EF wählt eine der Entitäten aus, die abhängig von der Fähigkeit, eine Fremdschlüssel Eigenschaft zu erkennen, abhängig ist. Wenn die falsche Entität als abhängig ausgewählt wird, können Sie diese mithilfe der flüssigen API korrigieren.

Beim Konfigurieren der Beziehung mit der flüssigen API verwenden Sie die Methoden `HasOne` und `WithOne`.

Beim Konfigurieren des fremd Schlüssels müssen Sie den abhängigen Entitätstyp angeben. Beachten Sie den generischen Parameter, der für `HasForeignKey` in der folgenden Auflistung bereitgestellt wird. In einer 1: n-Beziehung ist klar, dass die Entität mit der Verweis Navigation die abhängige ist und die mit der Auflistung der Prinzipal ist. Dies ist jedoch in einer eins-zu-eins-Beziehung nicht der Fall, daher muss Sie explizit definiert werden.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?name=OneToOne&highlight=11)]

### <a name="many-to-many"></a>m:n

M:n-Beziehungen ohne Entitäts Klasse zur Darstellung der jointabelle werden noch nicht unterstützt. Sie können jedoch eine m:n-Beziehung darstellen, indem Sie eine Entitäts Klasse für die jointabelle einschließen und zwei separate 1: n-Beziehungen Mapping.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?name=ManyToMany&highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)]
