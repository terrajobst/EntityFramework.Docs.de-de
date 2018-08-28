---
title: Beziehungen – EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: a53a862cc2443a1c4461aa287def100284635f26
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994941"
---
# <a name="relationships"></a><span data-ttu-id="a157c-102">Beziehungen</span><span class="sxs-lookup"><span data-stu-id="a157c-102">Relationships</span></span>

<span data-ttu-id="a157c-103">Eine Beziehung definiert, wie zwei Entitäten miteinander verknüpfen.</span><span class="sxs-lookup"><span data-stu-id="a157c-103">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="a157c-104">In einer relationalen Datenbank wird dies durch eine foreign Key-Einschränkung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="a157c-104">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="a157c-105">Die meisten Beispiele in diesem Artikel verwenden Sie eine 1: n Beziehung zur Veranschaulichung der Konzepte.</span><span class="sxs-lookup"><span data-stu-id="a157c-105">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="a157c-106">Weitere Beispiele für 1: 1- und m: n Beziehungen finden Sie unter den [andere Beziehung Muster](#other-relationship-patterns) Abschnitt am Ende des Artikels.</span><span class="sxs-lookup"><span data-stu-id="a157c-106">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="a157c-107">Begriffsdefinition</span><span class="sxs-lookup"><span data-stu-id="a157c-107">Definition of Terms</span></span>

<span data-ttu-id="a157c-108">Es wird eine Reihe von Begriffen verwendet, um Beziehungen in Datenbanken zu beschreiben</span><span class="sxs-lookup"><span data-stu-id="a157c-108">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="a157c-109">**Abhängige Entität:** Dies ist die Entität, die Fremdschlüsseleigenschaften enthält.</span><span class="sxs-lookup"><span data-stu-id="a157c-109">**Dependent entity:** This is the entity that contains the foreign key property(s).</span></span> <span data-ttu-id="a157c-110">Auch "Child" genannt.</span><span class="sxs-lookup"><span data-stu-id="a157c-110">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="a157c-111">**Prinzipalentität:** Dies ist die Entität, die bzw. der alternativen primären Schlüsseleigenschaften enthält.</span><span class="sxs-lookup"><span data-stu-id="a157c-111">**Principal entity:** This is the entity that contains the primary/alternate key property(s).</span></span> <span data-ttu-id="a157c-112">Manchmal bezeichnet als "Parent" der Beziehung.</span><span class="sxs-lookup"><span data-stu-id="a157c-112">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="a157c-113">**Fremdschlüssel:** Die Eigenschaft(en) in einer abhängigen Entität, welche die Inhalte der Primärschlüsseleigenschaft enthält, mit der die Entität zusammenhängt.</span><span class="sxs-lookup"><span data-stu-id="a157c-113">**Foreign key:** The property(s) in the dependent entity that is used to store the values of the principal key property that the entity is related to.</span></span>

* <span data-ttu-id="a157c-114">**Hauptschlüssel:** Die Eigenschaft(en), welche die Prinzipalentität eindeutig identifiziert.</span><span class="sxs-lookup"><span data-stu-id="a157c-114">**Principal key:** The property(s) that uniquely identifies the principal entity.</span></span> <span data-ttu-id="a157c-115">Dies kann ein Primär- oder Alternativschlüssel sein.</span><span class="sxs-lookup"><span data-stu-id="a157c-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="a157c-116">**Navigationseigenschaft:** eine Eigenschaft der primären und/oder abhängigen Entität, welche Verweise zu den verbundenen Entitäten enthält.</span><span class="sxs-lookup"><span data-stu-id="a157c-116">**Navigation property:** A property defined on the principal and/or dependent entity that contains a reference(s) to the related entity(s).</span></span>

  * <span data-ttu-id="a157c-117">**Auflistungsnavigationseigenschaft:** eine Navigationseigenschaft, die Verweise auf viele verknüpfte Entitäten enthält.</span><span class="sxs-lookup"><span data-stu-id="a157c-117">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="a157c-118">**Verweisnavigationseigenschaft:** eine Navigationseigenschaft, die einen Verweis auf eine einzelne verknüpfte Entität enthält.</span><span class="sxs-lookup"><span data-stu-id="a157c-118">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="a157c-119">**Inverse Navigationseigenschaft:** im Zusammenhang mit einer beliebigen anderen Navigationseigenschaft stellt diese Eigenschaft das anderen Ende der Beziehung dar.</span><span class="sxs-lookup"><span data-stu-id="a157c-119">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>

<span data-ttu-id="a157c-120">Das folgende Codebeispiel veranschaulicht eine 1: n Beziehung zwischen `Blog` und `Post`</span><span class="sxs-lookup"><span data-stu-id="a157c-120">The following code listing shows a one-to-many relationship between `Blog` and `Post`</span></span>

* <span data-ttu-id="a157c-121">`Post` die abhängige Entität</span><span class="sxs-lookup"><span data-stu-id="a157c-121">`Post` is the dependent entity</span></span>

* <span data-ttu-id="a157c-122">`Blog` ist die Prinzipalentität</span><span class="sxs-lookup"><span data-stu-id="a157c-122">`Blog` is the principal entity</span></span>

* <span data-ttu-id="a157c-123">`Post.BlogId` ist der Fremdschlüssel</span><span class="sxs-lookup"><span data-stu-id="a157c-123">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="a157c-124">`Blog.BlogId` der Prinzipalschlüssel (in diesem Fall ist es ein Primärschlüssel anstelle eines Alternativschlüssels)</span><span class="sxs-lookup"><span data-stu-id="a157c-124">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="a157c-125">`Post.Blog` ist eine Verweisnavigationseigenschaft</span><span class="sxs-lookup"><span data-stu-id="a157c-125">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="a157c-126">`Blog.Posts` ist eine Auflistungsnavigationseigenschaft</span><span class="sxs-lookup"><span data-stu-id="a157c-126">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="a157c-127">`Post.Blog`ist die inverse Navigationseigenschaft von `Blog.Posts`(und umgekehrt)</span><span class="sxs-lookup"><span data-stu-id="a157c-127">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a><span data-ttu-id="a157c-128">Konventionen</span><span class="sxs-lookup"><span data-stu-id="a157c-128">Conventions</span></span>

<span data-ttu-id="a157c-129">Gemäß der Konvention wird eine Beziehung erstellt werden, wenn eine Navigationseigenschaft, die ermittelt, die für einen Typ vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="a157c-129">By convention, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="a157c-130">Eine Eigenschaft gilt eine Navigationseigenschaft, wenn der Typ, auf die verwiesen, durch den aktuellen Anbieter nicht als ein skalarer Typ zugeordnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="a157c-130">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="a157c-131">Beziehungen, die gemäß der Konvention ermittelt werden, werden immer den Primärschlüssel der prinzipalentität Ziel.</span><span class="sxs-lookup"><span data-stu-id="a157c-131">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="a157c-132">Um einen alternativen Schlüssel als Ziel verwenden, muss zusätzliche Konfigurationsschritte mithilfe der Fluent-API ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="a157c-132">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="a157c-133">Vollständig definierten Beziehungen</span><span class="sxs-lookup"><span data-stu-id="a157c-133">Fully Defined Relationships</span></span>

<span data-ttu-id="a157c-134">Das bekannteste Muster für Beziehungen werden Navigationseigenschaften, die definiert, die an beiden Enden der Beziehung und eine Fremdschlüsseleigenschaft in der abhängigen Entität-Klasse definiert haben.</span><span class="sxs-lookup"><span data-stu-id="a157c-134">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="a157c-135">Wenn ein Paar von Navigationseigenschaften zwischen zwei Typen gefunden wird, werden sie als umgekehrte Navigationseigenschaften, die von der gleichen Beziehung konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="a157c-135">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="a157c-136">Wenn die abhängige Entität eine Eigenschaft namens enthält `<primary key property name>`, `<navigation property name><primary key property name>`, oder `<principal entity name><primary key property name>` als Fremdschlüssel so konfiguriert werden, wird.</span><span class="sxs-lookup"><span data-stu-id="a157c-136">If the dependent entity contains a property named `<primary key property name>`, `<navigation property name><primary key property name>`, or `<principal entity name><primary key property name>` then it will be configured as the foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> <span data-ttu-id="a157c-137">Treten mehrere Navigationseigenschaften, die zwischen zwei Arten definiert (d. h. mehrere unterschiedliche Paar von Navigationen, die auf den jeweils anderen verweisen), gemäß der Konvention werden dann keine Beziehungen erstellt werden, und Sie müssen manuell konfigurieren, um sich zu identifizieren wie die Navigationseigenschaften, kombinieren Sie.</span><span class="sxs-lookup"><span data-stu-id="a157c-137">If there are multiple navigation properties defined between two types (that is, more than one distinct pair of navigations that point to each other), then no relationships will be created by convention and you will need to manually configure them to identify how the navigation properties pair up.</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="a157c-138">Keine Foreign Key-Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="a157c-138">No Foreign Key Property</span></span>

<span data-ttu-id="a157c-139">Zwar wird empfohlen, eine Fremdschlüsseleigenschaft in der abhängigen Entität-Klasse definiert haben, ist es nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="a157c-139">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="a157c-140">Wenn keine foreign Key-Eigenschaft gefunden wird, wird eine Fremdschlüsseleigenschaft Volumeschattenkopie eingeführt werden, mit dem Namen `<navigation property name><principal key property name>` (finden Sie unter [Schatteneigenschaften](shadow-properties.md) Informationen).</span><span class="sxs-lookup"><span data-stu-id="a157c-140">If no foreign key property is found, a shadow foreign key property will be introduced with the name `<navigation property name><principal key property name>` (see [Shadow Properties](shadow-properties.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a><span data-ttu-id="a157c-141">Einzelne Navigationseigenschaft</span><span class="sxs-lookup"><span data-stu-id="a157c-141">Single Navigation Property</span></span>

<span data-ttu-id="a157c-142">Nur eine Navigationseigenschaft (keine umgekehrte Navigation und keine Fremdschlüsseleigenschaft) einschließlich ist ausreichend, um eine Beziehung, die gemäß der Konvention definiert haben.</span><span class="sxs-lookup"><span data-stu-id="a157c-142">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="a157c-143">Sie können auch die Möglichkeit, eine einzelne Navigationseigenschaft und eine Fremdschlüsseleigenschaft haben.</span><span class="sxs-lookup"><span data-stu-id="a157c-143">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a><span data-ttu-id="a157c-144">Kaskadierendes Delete</span><span class="sxs-lookup"><span data-stu-id="a157c-144">Cascade Delete</span></span>

<span data-ttu-id="a157c-145">Gemäß der Konvention Löschweitergabe festgelegt *Cascade* für erforderliche Beziehungen und *ClientSetNull* für optionale Beziehungen.</span><span class="sxs-lookup"><span data-stu-id="a157c-145">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="a157c-146">*CASCADE* bedeutet, dass abhängige Entitäten werden ebenfalls gelöscht.</span><span class="sxs-lookup"><span data-stu-id="a157c-146">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="a157c-147">*ClientSetNull* bedeutet, der abhängigen Entitäten, die nicht in den Speicher geladen bleibt unverändert und müssen manuell gelöscht oder aktualisiert, um auf eine gültige Prinzipal Entität verweisen.</span><span class="sxs-lookup"><span data-stu-id="a157c-147">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="a157c-148">Für Entitäten, die in den Arbeitsspeicher geladen werden, versucht EF Core, die die foreign Key-Eigenschaften auf null festgelegt.</span><span class="sxs-lookup"><span data-stu-id="a157c-148">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="a157c-149">Finden Sie unter den [erforderlichen und optionalen Beziehungen](#required-and-optional-relationships) Abschnitt für den Unterschied zwischen erforderlichen und optionalen Beziehungen.</span><span class="sxs-lookup"><span data-stu-id="a157c-149">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="a157c-150">Finden Sie unter [Löschweitergabe](../saving/cascade-delete.md) für Weitere Informationen zu den verschiedenen Verhalten und die Standardwerte, die gemäß der Konvention verwendet löschen.</span><span class="sxs-lookup"><span data-stu-id="a157c-150">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="a157c-151">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="a157c-151">Data Annotations</span></span>

<span data-ttu-id="a157c-152">Es gibt zwei datenanmerkungen, die verwendet werden können, so konfigurieren Sie Beziehungen, `[ForeignKey]` und `[InverseProperty]`.</span><span class="sxs-lookup"><span data-stu-id="a157c-152">There are two data annotations that can be used to configure relationships, `[ForeignKey]` and `[InverseProperty]`.</span></span>

### <a name="foreignkey"></a><span data-ttu-id="a157c-153">[ForeignKey]</span><span class="sxs-lookup"><span data-stu-id="a157c-153">[ForeignKey]</span></span>

<span data-ttu-id="a157c-154">Sie können die Datenanmerkungen verwenden, so konfigurieren Sie die Eigenschaft als die Fremdschlüsseleigenschaft für eine bestimmte Beziehung verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="a157c-154">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="a157c-155">Dies erfolgt normalerweise, wenn die Fremdschlüsseleigenschaft nicht gemäß der Konvention ermittelt wird.</span><span class="sxs-lookup"><span data-stu-id="a157c-155">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?name=Entities&highlight=17)]

> [!TIP]  
> <span data-ttu-id="a157c-156">Die `[ForeignKey]` Anmerkung in beiden Navigationseigenschaft in der Beziehung platziert werden kann.</span><span class="sxs-lookup"><span data-stu-id="a157c-156">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="a157c-157">Es muss nicht für die Navigationseigenschaft in der abhängigen Entität-Klasse zu wechseln.</span><span class="sxs-lookup"><span data-stu-id="a157c-157">It does not need to go on the navigation property in the dependent entity class.</span></span>

### <a name="inverseproperty"></a><span data-ttu-id="a157c-158">[InverseProperty]</span><span class="sxs-lookup"><span data-stu-id="a157c-158">[InverseProperty]</span></span>

<span data-ttu-id="a157c-159">Sie können die Datenanmerkungen verwenden, konfigurieren, wie Navigationseigenschaften für Entitäten abhängigen und der Prinzipaleigenschaft kombinieren Sie.</span><span class="sxs-lookup"><span data-stu-id="a157c-159">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="a157c-160">Dies erfolgt normalerweise, wenn mehr als ein Paar von Navigationseigenschaften zwischen zwei Entitätstypen vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="a157c-160">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?name=Entities&highlight=20,23)]

## <a name="fluent-api"></a><span data-ttu-id="a157c-161">Fluent-API</span><span class="sxs-lookup"><span data-stu-id="a157c-161">Fluent API</span></span>

<span data-ttu-id="a157c-162">Um eine Beziehung in der Fluent-API zu konfigurieren, starten Sie durch das Identifizieren der Navigationseigenschaften, die die Beziehung bilden.</span><span class="sxs-lookup"><span data-stu-id="a157c-162">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="a157c-163">`HasOne` oder `HasMany` identifiziert die Navigationseigenschaft für den Entitätstyp, der Sie die Konfiguration ab.</span><span class="sxs-lookup"><span data-stu-id="a157c-163">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="a157c-164">Sie verketten, klicken Sie dann einen Aufruf von `WithOne` oder `WithMany` die umgekehrte Navigation zu identifizieren.</span><span class="sxs-lookup"><span data-stu-id="a157c-164">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="a157c-165">`HasOne`/`WithOne` werden verwendet, für die verweisnavigationseigenschaften und `HasMany` / `WithMany` werden für Navigationseigenschaften für Auflistungen verwendet.</span><span class="sxs-lookup"><span data-stu-id="a157c-165">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?name=Model&highlight=8,9,10)]

### <a name="single-navigation-property"></a><span data-ttu-id="a157c-166">Einzelne Navigationseigenschaft</span><span class="sxs-lookup"><span data-stu-id="a157c-166">Single Navigation Property</span></span>

<span data-ttu-id="a157c-167">Wenn Sie nur eine Navigationseigenschaft stehen die parameterlose Überladung von `WithOne` und `WithMany`.</span><span class="sxs-lookup"><span data-stu-id="a157c-167">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="a157c-168">Dies bedeutet, dass im Prinzip ein Verweis oder eine Auflistung am anderen Ende der Beziehung vorhanden ist, aber es keine Navigationseigenschaft, die in der Entitätsklasse enthalten gibt.</span><span class="sxs-lookup"><span data-stu-id="a157c-168">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?name=Model&highlight=10)]

### <a name="foreign-key"></a><span data-ttu-id="a157c-169">Fremdschlüssel</span><span class="sxs-lookup"><span data-stu-id="a157c-169">Foreign Key</span></span>

<span data-ttu-id="a157c-170">Sie können die Fluent-API verwenden, so konfigurieren Sie die Eigenschaft als die Fremdschlüsseleigenschaft für eine bestimmte Beziehung verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="a157c-170">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?name=Model&highlight=11)]

<span data-ttu-id="a157c-171">Das folgende Codebeispiel veranschaulicht einen zusammengesetzten Fremdschlüssel zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a157c-171">The following code listing shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?name=Model&highlight=13)]

<span data-ttu-id="a157c-172">Können Sie die zeichenfolgenüberladung von `HasForeignKey(...)` so konfigurieren Sie eine schatteneigenschaft als Fremdschlüssel (finden Sie unter [Schatteneigenschaften](shadow-properties.md) Informationen).</span><span class="sxs-lookup"><span data-stu-id="a157c-172">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="a157c-173">Es wird empfohlen, das Modell explizit die Shadow-Eigenschaft hinzugefügt werden, bevor es als Fremdschlüssel verwendet wird (siehe unten).</span><span class="sxs-lookup"><span data-stu-id="a157c-173">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a><span data-ttu-id="a157c-174">Dienstprinzipalschlüssel</span><span class="sxs-lookup"><span data-stu-id="a157c-174">Principal Key</span></span>

<span data-ttu-id="a157c-175">Wenn Sie den Fremdschlüssel auf eine Eigenschaft als Primärschlüssel verweisen möchten, können Sie die Fluent-API, so konfigurieren Sie die wichtigsten Prinzipaleigenschaft für die Beziehung.</span><span class="sxs-lookup"><span data-stu-id="a157c-175">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="a157c-176">Die Eigenschaft, die Sie als Prinzipal Schlüssel wird automatisch konfigurieren, werden Setup als alternativer Schlüssel (finden Sie unter [Alternativschlüssel](alternate-keys.md) Informationen).</span><span class="sxs-lookup"><span data-stu-id="a157c-176">The property that you configure as the principal key will automatically be setup as an alternate key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/PrincipalKey.cs?highlight=11)] -->
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

<span data-ttu-id="a157c-177">Das folgende Codebeispiel zeigt einen zusammengesetzten Schlüssel für Prinzipal zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a157c-177">The following code listing shows how to configure a composite principal key.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CompositePrincipalKey.cs?highlight=11)] -->
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
> <span data-ttu-id="a157c-178">Die Reihenfolge, in der Sie wichtige prinzipaleigenschaften angeben, muss der Reihenfolge entsprechen, in der sie für den Fremdschlüssel angegeben sind.</span><span class="sxs-lookup"><span data-stu-id="a157c-178">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="a157c-179">Erforderliche und optionale Beziehungen</span><span class="sxs-lookup"><span data-stu-id="a157c-179">Required and Optional Relationships</span></span>

<span data-ttu-id="a157c-180">Sie können die Fluent-API verwenden, konfigurieren Sie, ob die Beziehung erforderlich oder optional ist.</span><span class="sxs-lookup"><span data-stu-id="a157c-180">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="a157c-181">Letzten Endes steuert, ob die Fremdschlüsseleigenschaft erforderlich oder optional ist.</span><span class="sxs-lookup"><span data-stu-id="a157c-181">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="a157c-182">Dies ist besonders hilfreich, wenn Sie einen Fremdschlüssel für die Schattenkopien Zustand verwenden.</span><span class="sxs-lookup"><span data-stu-id="a157c-182">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="a157c-183">Wenn Sie eine Fremdschlüsseleigenschaft in Ihrer Entitätsklasse verfügen, und klicken Sie dann die Requiredness der Beziehung bestimmt wird, je nachdem, ob die Fremdschlüsseleigenschaft erforderlich oder optional ist (finden Sie unter [erforderliche und optionale Eigenschaften](required-optional.md) Weitere (Informationen).</span><span class="sxs-lookup"><span data-stu-id="a157c-183">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

<!-- [!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/Required.cs?highlight=11)] -->
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

### <a name="cascade-delete"></a><span data-ttu-id="a157c-184">Kaskadierendes Delete</span><span class="sxs-lookup"><span data-stu-id="a157c-184">Cascade Delete</span></span>

<span data-ttu-id="a157c-185">Sie können die Fluent-API verwenden, das Cascade Löschverhalten für eine bestimmte Beziehung explizit zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a157c-185">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="a157c-186">Finden Sie unter [Löschweitergabe](../saving/cascade-delete.md) im Abschnitt Speichern von Daten für eine detaillierte Erläuterung der einzelnen Optionen.</span><span class="sxs-lookup"><span data-stu-id="a157c-186">See [Cascade Delete](../saving/cascade-delete.md) on the Saving Data section for a detailed discussion of each option.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CascadeDelete.cs?highlight=11)] -->
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

## <a name="other-relationship-patterns"></a><span data-ttu-id="a157c-187">Andere Beziehung-Muster</span><span class="sxs-lookup"><span data-stu-id="a157c-187">Other Relationship Patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="a157c-188">1: 1</span><span class="sxs-lookup"><span data-stu-id="a157c-188">One-to-one</span></span>

<span data-ttu-id="a157c-189">1: 1-Beziehungen haben einer verweisnavigationseigenschaft auf beiden Seiten.</span><span class="sxs-lookup"><span data-stu-id="a157c-189">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="a157c-190">Folgen sie die gleichen Konventionen wie 1: n Beziehungen, aber ein eindeutiger Index wird eingeführt, auf die Fremdschlüsseleigenschaft, um sicherzustellen, dass nur eine abhängige, der jedem Prinzipal verknüpft ist.</span><span class="sxs-lookup"><span data-stu-id="a157c-190">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/Relationships/OneToOne.cs?highlight=6,15,16)] -->
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
> <span data-ttu-id="a157c-191">EF wählt eine der Entitäten, die basierend auf der Fähigkeit zum Erkennen von Fremdschlüsseleigenschaften abhängig sein.</span><span class="sxs-lookup"><span data-stu-id="a157c-191">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="a157c-192">Wenn die falsche Entität als die abhängigen ausgewählt wird, können Sie die Fluent-API, um dies zu korrigieren.</span><span class="sxs-lookup"><span data-stu-id="a157c-192">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="a157c-193">Wenn Sie die Beziehung mit der Fluent-API konfigurieren möchten, verwenden Sie die `HasOne` und `WithOne` Methoden.</span><span class="sxs-lookup"><span data-stu-id="a157c-193">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="a157c-194">Wenn die foreign Key konfigurieren, Sie den abhängige Entitätstyp - angeben müssen, beachten Sie den generischen Parameter bereitgestellt, um `HasForeignKey` in der folgenden Liste.</span><span class="sxs-lookup"><span data-stu-id="a157c-194">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="a157c-195">In einer 1: n Beziehung wird deutlich, dass die Entität mit dem die Navigation Verweis des abhängigen und der Auftrag mit dem die Auflistung der Prinzipal ist.</span><span class="sxs-lookup"><span data-stu-id="a157c-195">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="a157c-196">Dies ist jedoch nicht in einer 1: 1-Beziehung – daher so müssen sie explizit zu definieren.</span><span class="sxs-lookup"><span data-stu-id="a157c-196">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/OneToOne.cs?highlight=11)] -->
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

### <a name="many-to-many"></a><span data-ttu-id="a157c-197">M: n</span><span class="sxs-lookup"><span data-stu-id="a157c-197">Many-to-many</span></span>

<span data-ttu-id="a157c-198">M: n Beziehungen ohne eine Entitätsklasse zur Darstellung der Jointabelle werden noch nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="a157c-198">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="a157c-199">Allerdings können Sie eine m: n Beziehung darstellen, durch Einschließen einer Entitätsklasse für die Jointabelle und die Zuordnung zwei separate 1: n Beziehungen.</span><span class="sxs-lookup"><span data-stu-id="a157c-199">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<PostTag>()
            .HasKey(t => new { t.PostId, t.TagId });

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
