---
title: Beziehungen-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 1e9c62bec47263ef452c7ac425a0bb446f9371d8
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197648"
---
# <a name="relationships"></a><span data-ttu-id="e7b4f-102">Beziehungen</span><span class="sxs-lookup"><span data-stu-id="e7b4f-102">Relationships</span></span>

<span data-ttu-id="e7b4f-103">Eine Beziehung definiert, wie zwei Entitäten miteinander in Beziehung stehen.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-103">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="e7b4f-104">In einer relationalen Datenbank wird dies durch eine FOREIGN KEY-Einschränkung repräsentiert.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-104">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="e7b4f-105">Die meisten Beispiele in diesem Artikel verwenden eine 1: n-Beziehung, um die Konzepte zu veranschaulichen.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-105">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="e7b4f-106">Beispiele für 1:1-und m:n-Beziehungen finden Sie im Abschnitt [andere Beziehungsmuster](#other-relationship-patterns) am Ende des Artikels.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-106">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="e7b4f-107">Definition von Begriffen</span><span class="sxs-lookup"><span data-stu-id="e7b4f-107">Definition of Terms</span></span>

<span data-ttu-id="e7b4f-108">Es wird eine Reihe von Begriffen verwendet, um Beziehungen in Datenbanken zu beschreiben</span><span class="sxs-lookup"><span data-stu-id="e7b4f-108">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="e7b4f-109">**Abhängige Entität:** Dies ist die Entität, die die Fremdschlüssel Eigenschaft (en) enthält.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-109">**Dependent entity:** This is the entity that contains the foreign key property(s).</span></span> <span data-ttu-id="e7b4f-110">Auch "Child" genannt.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-110">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="e7b4f-111">**Prinzipal Entität:** Dies ist die Entität, die die primären/Alternativen Schlüsseleigenschaft (en) enthält.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-111">**Principal entity:** This is the entity that contains the primary/alternate key property(s).</span></span> <span data-ttu-id="e7b4f-112">Auch "Parent" genannt.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-112">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="e7b4f-113">**Fremdschlüssel:** Die Eigenschaften in der abhängigen Entität, die verwendet werden, um die Werte der Prinzipal Schlüsseleigenschaft zu speichern, mit der die Entität verknüpft ist.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-113">**Foreign key:** The property(s) in the dependent entity that is used to store the values of the principal key property that the entity is related to.</span></span>

* <span data-ttu-id="e7b4f-114">**Prinzipal Schlüssel:** Die Eigenschaften, die die Prinzipal Entität eindeutig identifizieren.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-114">**Principal key:** The property(s) that uniquely identifies the principal entity.</span></span> <span data-ttu-id="e7b4f-115">Dies kann ein Primär- oder Alternativschlüssel sein.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="e7b4f-116">**Navigations Eigenschaft:** Eine Eigenschaft, die für die Prinzipal-und/oder abhängige Entität definiert ist, die einen Verweis auf die verknüpfte Entität (en) enthält.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-116">**Navigation property:** A property defined on the principal and/or dependent entity that contains a reference(s) to the related entity(s).</span></span>

  * <span data-ttu-id="e7b4f-117">**Auflistungs Navigations Eigenschaft:** Eine Navigations Eigenschaft, die Verweise auf viele Verwandte Entitäten enthält.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-117">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="e7b4f-118">**Verweis Navigations Eigenschaft:** Eine Navigations Eigenschaft, die einen Verweis auf eine einzelne verknüpfte Entität enthält.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-118">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="e7b4f-119">**Umgekehrte Navigations Eigenschaft:** Bei der Erörterung einer bestimmten Navigations Eigenschaft bezieht sich dieser Begriff auf die Navigations Eigenschaft am anderen Ende der Beziehung.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-119">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>

<span data-ttu-id="e7b4f-120">Das folgende Codebeispiel zeigt eine 1: n-Beziehung zwischen `Blog` und.`Post`</span><span class="sxs-lookup"><span data-stu-id="e7b4f-120">The following code listing shows a one-to-many relationship between `Blog` and `Post`</span></span>

* <span data-ttu-id="e7b4f-121">`Post`ist die abhängige Entität</span><span class="sxs-lookup"><span data-stu-id="e7b4f-121">`Post` is the dependent entity</span></span>

* <span data-ttu-id="e7b4f-122">`Blog` ist die Prinzipalentität</span><span class="sxs-lookup"><span data-stu-id="e7b4f-122">`Blog` is the principal entity</span></span>

* <span data-ttu-id="e7b4f-123">`Post.BlogId`ist der Fremdschlüssel</span><span class="sxs-lookup"><span data-stu-id="e7b4f-123">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="e7b4f-124">`Blog.BlogId` der Prinzipalschlüssel (in diesem Fall ist es ein Primärschlüssel anstelle eines Alternativschlüssels)</span><span class="sxs-lookup"><span data-stu-id="e7b4f-124">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="e7b4f-125">`Post.Blog` ist eine Verweisnavigationseigenschaft</span><span class="sxs-lookup"><span data-stu-id="e7b4f-125">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="e7b4f-126">`Blog.Posts` ist eine Auflistungsnavigationseigenschaft</span><span class="sxs-lookup"><span data-stu-id="e7b4f-126">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="e7b4f-127">`Post.Blog`ist die inverse Navigationseigenschaft von `Blog.Posts`(und umgekehrt)</span><span class="sxs-lookup"><span data-stu-id="e7b4f-127">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Entities)]

## <a name="conventions"></a><span data-ttu-id="e7b4f-128">Konventionen</span><span class="sxs-lookup"><span data-stu-id="e7b4f-128">Conventions</span></span>

<span data-ttu-id="e7b4f-129">Gemäß der Konvention wird eine Beziehung erstellt, wenn eine Navigations Eigenschaft für einen Typ erkannt wird.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-129">By convention, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="e7b4f-130">Eine Eigenschaft wird als Navigations Eigenschaft betrachtet, wenn der Typ, auf den Sie verweist, nicht vom aktuellen Datenbankanbieter als Skalartyp zugeordnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-130">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="e7b4f-131">Die von der Konvention ermittelten Beziehungen richten sich immer nach dem Primärschlüssel der Prinzipal Entität.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-131">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="e7b4f-132">Um einen alternativen Schlüssel als Ziel zu verwenden, muss mithilfe der flüssigen API eine zusätzliche Konfiguration durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-132">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="e7b4f-133">Vollständig definierte Beziehungen</span><span class="sxs-lookup"><span data-stu-id="e7b4f-133">Fully Defined Relationships</span></span>

<span data-ttu-id="e7b4f-134">Das gängigste Muster für Beziehungen besteht darin, dass für beide Enden der Beziehung Navigations Eigenschaften und eine Fremdschlüssel Eigenschaft definiert sind, die in der abhängigen Entitäts Klasse definiert ist.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-134">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="e7b4f-135">Wenn zwischen zwei Typen ein paar Navigations Eigenschaften gefunden wird, werden diese als umgekehrte Navigations Eigenschaften derselben Beziehung konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-135">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="e7b4f-136">Wenn die abhängige Entität eine Eigenschaft mit `<primary key property name>`dem `<navigation property name><primary key property name>`Namen, `<principal entity name><primary key property name>` oder enthält, wird Sie als Fremdschlüssel konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-136">If the dependent entity contains a property named `<primary key property name>`, `<navigation property name><primary key property name>`, or `<principal entity name><primary key property name>` then it will be configured as the foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> <span data-ttu-id="e7b4f-137">Wenn mehrere Navigations Eigenschaften zwischen zwei Typen definiert sind (d. h. mehr als ein unterschiedliches Navigations Paar, das aufeinander zeigt), werden keine Beziehungen gemäß der Konvention erstellt und müssen manuell konfiguriert werden, um zu ermitteln, wie das Das Paar der Navigations Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="e7b4f-137">If there are multiple navigation properties defined between two types (that is, more than one distinct pair of navigations that point to each other), then no relationships will be created by convention and you will need to manually configure them to identify how the navigation properties pair up.</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="e7b4f-138">Keine Fremdschlüssel Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="e7b4f-138">No Foreign Key Property</span></span>

<span data-ttu-id="e7b4f-139">Es wird empfohlen, eine Fremdschlüssel Eigenschaft in der abhängigen Entitäts Klasse zu definieren, die jedoch nicht erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-139">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="e7b4f-140">Wenn keine Fremdschlüssel Eigenschaft gefunden wird, wird eine Schatten-Fremdschlüssel Eigenschaft mit dem Namen `<navigation property name><principal key property name>` eingeführt (Weitere Informationen finden Sie unter [Schatten Eigenschaften](shadow-properties.md) ).</span><span class="sxs-lookup"><span data-stu-id="e7b4f-140">If no foreign key property is found, a shadow foreign key property will be introduced with the name `<navigation property name><principal key property name>` (see [Shadow Properties](shadow-properties.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a><span data-ttu-id="e7b4f-141">Einzelne Navigations Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="e7b4f-141">Single Navigation Property</span></span>

<span data-ttu-id="e7b4f-142">Das Einschließen von nur einer Navigations Eigenschaft (keine umgekehrte Navigation und keine Fremdschlüssel Eigenschaft) reicht aus, um eine Beziehung gemäß der Konvention zu definieren.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-142">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="e7b4f-143">Sie können auch eine einzelne Navigations Eigenschaft und eine Fremdschlüssel Eigenschaft haben.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-143">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a><span data-ttu-id="e7b4f-144">Kaskadierendes Delete</span><span class="sxs-lookup"><span data-stu-id="e7b4f-144">Cascade Delete</span></span>

<span data-ttu-id="e7b4f-145">Gemäß der Konvention wird CASCADE DELETE für erforderliche Beziehungen und *clientsetnull* für optionale Beziehungen auf *Cascade* festgelegt.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-145">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="e7b4f-146">*Cascade* bedeutet, dass abhängige Entitäten ebenfalls gelöscht werden.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-146">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="e7b4f-147">*Clientsetnull* bedeutet, dass abhängige Entitäten, die nicht in den Arbeitsspeicher geladen werden, unverändert bleiben und manuell gelöscht oder aktualisiert werden müssen, um auf eine gültige Prinzipal Entität zu verweisen.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-147">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="e7b4f-148">Bei Entitäten, die in den Arbeitsspeicher geladen werden, wird EF Core versucht, die Fremdschlüssel Eigenschaften auf NULL festzulegen.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-148">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="e7b4f-149">Den Unterschied zwischen erforderlichen und optionalen Beziehungen finden Sie im Abschnitt [erforderliche und optionale Beziehungen](#required-and-optional-relationships) .</span><span class="sxs-lookup"><span data-stu-id="e7b4f-149">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="e7b4f-150">Weitere Informationen zu den verschiedenen Lösch Verhalten und den von der Konvention verwendeten Standardwerten finden Sie unter [Cascade Delete](../saving/cascade-delete.md) .</span><span class="sxs-lookup"><span data-stu-id="e7b4f-150">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="e7b4f-151">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="e7b4f-151">Data Annotations</span></span>

<span data-ttu-id="e7b4f-152">Es gibt zwei Daten Anmerkungen, die zum Konfigurieren von Beziehungen verwendet werden können `[ForeignKey]` , `[InverseProperty]`und.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-152">There are two data annotations that can be used to configure relationships, `[ForeignKey]` and `[InverseProperty]`.</span></span> <span data-ttu-id="e7b4f-153">Diese sind im `System.ComponentModel.DataAnnotations.Schema` -Namespace verfügbar.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-153">These are available in the `System.ComponentModel.DataAnnotations.Schema` namespace.</span></span>

### <a name="foreignkey"></a><span data-ttu-id="e7b4f-154">ForeignKey</span><span class="sxs-lookup"><span data-stu-id="e7b4f-154">[ForeignKey]</span></span>

<span data-ttu-id="e7b4f-155">Mit den Daten Anmerkungen können Sie konfigurieren, welche Eigenschaft als Fremdschlüssel Eigenschaft für eine bestimmte Beziehung verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-155">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="e7b4f-156">Dies erfolgt in der Regel, wenn die Fremdschlüssel Eigenschaft nicht gemäß der Konvention ermittelt wird.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-156">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> <span data-ttu-id="e7b4f-157">Die `[ForeignKey]` -Anmerkung kann für jede Navigations Eigenschaft in der Beziehung platziert werden.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-157">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="e7b4f-158">Die Navigations Eigenschaft in der abhängigen Entitäts Klasse muss nicht verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-158">It does not need to go on the navigation property in the dependent entity class.</span></span>

### <a name="inverseproperty"></a><span data-ttu-id="e7b4f-159">[Invergenproperty]</span><span class="sxs-lookup"><span data-stu-id="e7b4f-159">[InverseProperty]</span></span>

<span data-ttu-id="e7b4f-160">Mit den Daten Anmerkungen können Sie konfigurieren, wie Navigations Eigenschaften für die abhängigen und Prinzipal Entitäten gekoppelt werden.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-160">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="e7b4f-161">Dies erfolgt in der Regel, wenn zwischen zwei Entitäts Typen mehr als ein paar Navigations Eigenschaften vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-161">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a><span data-ttu-id="e7b4f-162">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="e7b4f-162">Fluent API</span></span>

<span data-ttu-id="e7b4f-163">Um eine Beziehung in der fließenden API zu konfigurieren, müssen Sie zunächst die Navigations Eigenschaften ermitteln, die die Beziehung bilden.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-163">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="e7b4f-164">`HasOne`oder `HasMany` identifiziert die Navigations Eigenschaft für den Entitätstyp, für den Sie mit der Konfiguration beginnen.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-164">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="e7b4f-165">Anschließend verketten Sie einen aufrufen `WithOne` von `WithMany` oder, um die umgekehrte Navigation zu ermitteln.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-165">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="e7b4f-166">`HasOne`/`WithOne`werden für Verweis Navigations Eigenschaften verwendet und `HasMany` / `WithMany` für Auflistungs Navigations Eigenschaften verwendet.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-166">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a><span data-ttu-id="e7b4f-167">Einzelne Navigations Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="e7b4f-167">Single Navigation Property</span></span>

<span data-ttu-id="e7b4f-168">Wenn Sie nur über eine Navigations Eigenschaft verfügen, gibt es Parameter lose über Ladungen von `WithOne` und `WithMany`.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-168">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="e7b4f-169">Dies gibt an, dass ein Verweis oder eine Auflistung am anderen Ende der Beziehung konzeptionell ist, aber in der Entitäts Klasse ist keine Navigations Eigenschaft enthalten.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-169">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a><span data-ttu-id="e7b4f-170">Fremdschlüssel</span><span class="sxs-lookup"><span data-stu-id="e7b4f-170">Foreign Key</span></span>

<span data-ttu-id="e7b4f-171">Sie können die fließende API verwenden, um zu konfigurieren, welche Eigenschaft als Fremdschlüssel Eigenschaft für eine bestimmte Beziehung verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-171">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?highlight=17)]

<span data-ttu-id="e7b4f-172">Das folgende Codebeispiel zeigt, wie ein zusammengesetzter Fremdschlüssel konfiguriert wird.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-172">The following code listing shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?highlight=20)]

<span data-ttu-id="e7b4f-173">Sie können die Zeichen folgen Überladung `HasForeignKey(...)` von verwenden, um eine Schatten Eigenschaft als Fremdschlüssel zu konfigurieren (Weitere Informationen finden Sie unter [Schatten Eigenschaften](shadow-properties.md) ).</span><span class="sxs-lookup"><span data-stu-id="e7b4f-173">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="e7b4f-174">Es wird empfohlen, die Schatten Eigenschaft explizit dem Modell hinzuzufügen, bevor Sie als Fremdschlüssel verwendet wird (wie unten gezeigt).</span><span class="sxs-lookup"><span data-stu-id="e7b4f-174">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a><span data-ttu-id="e7b4f-175">Ohne Navigations Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="e7b4f-175">Without Navigation Property</span></span>

<span data-ttu-id="e7b4f-176">Sie müssen nicht unbedingt eine Navigations Eigenschaft bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-176">You don't necessarily need to provide a navigation property.</span></span> <span data-ttu-id="e7b4f-177">Sie können auf einer Seite der Beziehung einfach einen Fremdschlüssel bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-177">You can simply provide a Foreign Key on one side of the relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a><span data-ttu-id="e7b4f-178">Prinzipal Schlüssel</span><span class="sxs-lookup"><span data-stu-id="e7b4f-178">Principal Key</span></span>

<span data-ttu-id="e7b4f-179">Wenn Sie möchten, dass der Fremdschlüssel auf eine andere Eigenschaft als den Primärschlüssel verweist, können Sie mit der fließend-API die Prinzipal Schlüsseleigenschaft für die Beziehung konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-179">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="e7b4f-180">Die Eigenschaft, die Sie als Prinzipal Schlüssel konfigurieren, wird automatisch als alternativer Schlüssel eingerichtet (Weitere Informationen finden Sie in den [Alternativen Schlüsseln](alternate-keys.md) ).</span><span class="sxs-lookup"><span data-stu-id="e7b4f-180">The property that you configure as the principal key will automatically be setup as an alternate key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => s.CarLicensePlate)
            .HasPrincipalKey(c => c.LicensePlate);
    }
}

public class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

<span data-ttu-id="e7b4f-181">Im folgenden Codebeispiel wird gezeigt, wie ein zusammengesetzter Prinzipal Schlüssel konfiguriert wird.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-181">The following code listing shows how to configure a composite principal key.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => new { s.CarState, s.CarLicensePlate })
            .HasPrincipalKey(c => new { c.State, c.LicensePlate });
    }
}

public class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarState { get; set; }
    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

> [!WARNING]  
> <span data-ttu-id="e7b4f-182">Die Reihenfolge, in der Sie die Prinzipal Schlüsseleigenschaften angeben, muss mit der Reihenfolge identisch sein, in der Sie für den Fremdschlüssel angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-182">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="e7b4f-183">Erforderliche und optionale Beziehungen</span><span class="sxs-lookup"><span data-stu-id="e7b4f-183">Required and Optional Relationships</span></span>

<span data-ttu-id="e7b4f-184">Mit der fließend-API können Sie konfigurieren, ob die Beziehung erforderlich oder optional ist.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-184">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="e7b4f-185">Letztendlich steuert dies, ob die Fremdschlüssel Eigenschaft erforderlich oder optional ist.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-185">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="e7b4f-186">Dies ist besonders nützlich, wenn Sie einen Schatten Zustands-Fremdschlüssel verwenden.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-186">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="e7b4f-187">Wenn Sie über eine Fremdschlüssel Eigenschaft in der Entitäts Klasse verfügen, wird die Eignung der Beziehung abhängig davon bestimmt, ob die Fremdschlüssel Eigenschaft erforderlich oder optional ist (Weitere Informationen finden Sie unter [erforderliche und optionale Eigenschaften](required-optional.md) ).</span><span class="sxs-lookup"><span data-stu-id="e7b4f-187">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

<!-- [!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}
```

### <a name="cascade-delete"></a><span data-ttu-id="e7b4f-188">Kaskadierendes Delete</span><span class="sxs-lookup"><span data-stu-id="e7b4f-188">Cascade Delete</span></span>

<span data-ttu-id="e7b4f-189">Sie können die fließende API verwenden, um das kaskadierte Lösch Verhalten für eine bestimmte Beziehung explizit zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-189">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="e7b4f-190">Eine ausführliche Erläuterung der einzelnen Optionen finden Sie im Abschnitt " [Löschen](../saving/cascade-delete.md) von Daten" im Abschnitt "Speichern von Daten".</span><span class="sxs-lookup"><span data-stu-id="e7b4f-190">See [Cascade Delete](../saving/cascade-delete.md) on the Saving Data section for a detailed discussion of each option.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .OnDelete(DeleteBehavior.Cascade);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int? BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="other-relationship-patterns"></a><span data-ttu-id="e7b4f-191">Andere Beziehungsmuster</span><span class="sxs-lookup"><span data-stu-id="e7b4f-191">Other Relationship Patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="e7b4f-192">Eins zu eins</span><span class="sxs-lookup"><span data-stu-id="e7b4f-192">One-to-one</span></span>

<span data-ttu-id="e7b4f-193">Eine zu 1 Beziehung hat eine Verweis Navigations Eigenschaft auf beiden Seiten.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-193">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="e7b4f-194">Sie folgen denselben Konventionen wie 1: n-Beziehungen, aber es wird ein eindeutiger Index für die Fremdschlüssel Eigenschaft eingeführt, um sicherzustellen, dass nur ein abhängiger mit den einzelnen Prinzipalen verknüpft ist.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-194">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Relationships/OneToOne.cs?highlight=6,15,16)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

> [!NOTE]  
> <span data-ttu-id="e7b4f-195">EF wählt eine der Entitäten aus, die abhängig von der Fähigkeit, eine Fremdschlüssel Eigenschaft zu erkennen, abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-195">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="e7b4f-196">Wenn die falsche Entität als abhängig ausgewählt wird, können Sie diese mithilfe der flüssigen API korrigieren.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-196">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="e7b4f-197">Beim Konfigurieren der Beziehung mit der flüssigen API verwenden Sie die- `HasOne` Methode `WithOne` und die-Methode.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-197">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="e7b4f-198">Wenn Sie den Fremdschlüssel konfigurieren, müssen Sie den abhängigen Entitätstyp angeben. Beachten Sie den generischen Parameter, `HasForeignKey` der in der nachfolgenden Auflistung bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-198">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="e7b4f-199">In einer 1: n-Beziehung ist klar, dass die Entität mit der Verweis Navigation die abhängige ist und die mit der Auflistung der Prinzipal ist.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-199">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="e7b4f-200">Dies ist jedoch in einer eins-zu-eins-Beziehung nicht der Fall, daher muss Sie explizit definiert werden.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-200">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<BlogImage> BlogImages { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasOne(p => p.BlogImage)
            .WithOne(i => i.Blog)
            .HasForeignKey<BlogImage>(b => b.BlogForeignKey);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogForeignKey { get; set; }
    public Blog Blog { get; set; }
}
```

### <a name="many-to-many"></a><span data-ttu-id="e7b4f-201">M:n-Zahl</span><span class="sxs-lookup"><span data-stu-id="e7b4f-201">Many-to-many</span></span>

<span data-ttu-id="e7b4f-202">M:n-Beziehungen ohne Entitäts Klasse zur Darstellung der jointabelle werden noch nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-202">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="e7b4f-203">Sie können jedoch eine m:n-Beziehung darstellen, indem Sie eine Entitäts Klasse für die jointabelle einschließen und zwei separate 1: n-Beziehungen Mapping.</span><span class="sxs-lookup"><span data-stu-id="e7b4f-203">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<PostTag>()
            .HasKey(pt => new { pt.PostId, pt.TagId });

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Post)
            .WithMany(p => p.PostTags)
            .HasForeignKey(pt => pt.PostId);

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Tag)
            .WithMany(t => t.PostTags)
            .HasForeignKey(pt => pt.TagId);
    }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class Tag
{
    public string TagId { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class PostTag
{
    public int PostId { get; set; }
    public Post Post { get; set; }

    public string TagId { get; set; }
    public Tag Tag { get; set; }
}
```
